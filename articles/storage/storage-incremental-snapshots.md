---
title: "Použít snímky přírůstkové zálohování a obnovení nespravované disků virtuálního počítače Azure | Microsoft Docs"
description: "Vytvořte vlastní řešení pro zálohování a obnovení disky virtuálního počítače Azure pomocí přírůstkové snímků."
services: storage
documentationcenter: na
author: aungoo-msft
manager: tadb
editor: tysonn
ms.assetid: 3524b987-bd65-4e35-83e7-fbc2136643e5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: aungoo
ms.openlocfilehash: 7b08ce207b2a3cc2dd3d3559765def6af42a844a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky
## <a name="overview"></a>Přehled
Úložiště Azure poskytuje schopnost pořizovat snímky objektů BLOB. Snímky zaznamenání stavu objektů blob v tomto bodě v čase. V tomto článku jsme se popisují scénáře o tom, jak je možné uchovávat zálohy disků virtuálních počítačů pomocí snímků. Můžete použít tuto metodu, když jste zvolit nepoužívání Azure zálohování a obnovení služby a chcete vytvořit vlastní strategii zálohování pro disky virtuálního počítače.

Disky virtuálních počítačů Azure jsou uloženy jako objekty BLOB stránky ve službě Azure Storage. Vzhledem k tomu, že jsme mluvíme o strategie zálohování pro disky virtuálního počítače v tomto článku, jsme se odkazující na snímky v souvislosti s objekty BLOB stránky. Další informace o snímků, najdete v tématu [vytvoření snímku objektu Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Co je snímek?
Objekt blob snímek je jen pro čtení verze objektu blob zachycenou v bodě v čase. Po vytvoření snímku, ho můžete číst, kopírovat, nebo odstranit, ale nedojde ke změně. Snímky poskytují způsob, jak zálohovat objekt blob, jak se objevuje v časovém okamžiku. Dokud REST verze 2015-04-05 bylo možnost Kopírovat úplné snímky. S ostatními verze 2015-07-08 a vyšší, můžete také můžete zkopírovat přírůstkové snímky.

## <a name="full-snapshot-copy"></a>Kopírování úplné snímku
Snímky je možné zkopírovat do jiného účtu úložiště jako objekt blob zachovat zálohy základní objekt blob. Můžete také zkopírovat snímek přes jeho základní objekt blob, které je třeba obnovení objektu blob na starší verzi. Po zkopírování snímek z jednoho účtu úložiště do druhého bude zabírat na stejném místě jako objekt blob základní stránky. Kopírování celou snímky z jednoho účtu úložiště do druhého proto bude pomalé a bude také využívat velké množství místa v cílový účet úložiště.

> [!NOTE]
> Pokud základní objekt blob zkopírujete na jiný cíl, nejsou spolu se ji zkopírovat snímky objektu blob. Podobně přepíšete základní objekt blob se kopie, neovlivní snímky přidružené základní objekt blob a zůstanou beze změn v části název základního objektu blob.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Zálohovat disky pomocí snímků
Jako strategie zálohování pro disky virtuálního počítače, můžete využít pravidelné snímky objektu blob disk nebo stránce a kopírování je do jiného úložiště účtu pomocí nástroje jako [kopírovat objekt Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) operaci nebo [AzCopy](storage-use-azcopy.md). Snímek můžete zkopírovat na cílový objekt blob stránky s jiným názvem. Výsledný objekt blob stránky cílové je objekt blob stránky lze zapisovat a není snímek. Později v tomto článku jsme se popisují kroky pro zálohování disky virtuálního počítače pomocí snímků.

### <a name="restore-disks-using-snapshots"></a>Obnovení disků pomocí snímků
Pokud nastane čas k obnovení na disku na předchozí stabilní verzi zachytit v jednom z snímků zálohy, můžete zkopírovat snímek přes základní stránky objektu blob. Po snímku je propagována do základní stránky objektu blob, zůstanou snímku, ale dojde k přepsání zdrojem kopie, kterou může být číst a zapisovat. Později v tomto článku jsme se popisují postup obnovit předchozí verze disku z jeho snímků.

### <a name="implementing-full-snapshot-copy"></a>Implementace kopie úplné snímku
Pomocí těchto kroků můžete implementovat kopii úplné snímku

* Nejprve vytvořte snímek pomocí základní objekt blob [snímku Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) operaci.
* Zkopírujte snímek pomocí účet cílového úložiště [kopírovat objekt Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx).
* Opakujte tento postup k udržování záložní kopie vaše základní objekt blob.

## <a name="incremental-snapshot-copy"></a>Kopírování přírůstkový snímek
Nová funkce v [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) rozhraní API poskytuje mnohem lepší způsob, jak zálohovat snímky objekty BLOB stránky nebo disků. Rozhraní API vrátí seznam změn mezi základní objekt blob a snímky. Tím se snižuje množství prostoru úložiště použít na účet záloh. Rozhraní API podporuje objekty BLOB stránky na Storage úrovně Premium, jakož i úložiště Standard Storage. Pomocí tohoto rozhraní API, teď můžete sestavit rychlejší a efektivní řešení zálohování pro virtuální počítače Azure. Bude k dispozici REST verze 2015-07-08 a vyšší.

Přírůstkové kopie snímků umožňuje kopírovat z jednoho účtu úložiště do druhého rozdíl mezi,

* Základní objekt blob a jeho snímek nebo
* Všechny dva snímky základní objekt blob

Za předpokladu, že jsou splněny následující podmínky,

* Objekt blob byl vytvořen ledna-1-2016 nebo novější.
* Objekt blob nebyla přepsána [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) nebo [kopírovat objekt Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) mezi dvěma snímky.

**Poznámka:**: Tato funkce je dostupná pro Premium a Standard objekty BLOB stránky Azure.

Pokud máte vlastní strategii zálohování, který používá snímky kopírování snímky z jednoho účtu úložiště do druhého může být velmi pomalé a spotřebovává velké množství prostoru úložiště. Namísto kopírování celý snímek na účet úložiště záloh, můžete napsat rozdíl mezi po sobě jdoucích snímků na objekt blob stránky zálohování. Tímto způsobem je podstatně snížit čas pro kopírování a místa pro uložení zálohy.

### <a name="implementing-incremental-snapshot-copy"></a>Implementace přírůstkový snímek kopie
Pomocí těchto kroků můžete implementovat přírůstkový snímek kopie

* Pořízení snímku základní objekt blob pomocí [snímku Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx).
* Kopírování snímku do cílového úložiště pro zálohy účtu pomocí [kopírovat objekt Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). Bude jím objektů blob zálohy stránky. Pořízení snímku tento objekt blob stránky zálohování a uložení v zálohování účtu.
* Vytvořte nový snímek objektu blob základní pomocí snímku objektů Blob.
* Získat rozdíl mezi prvním a druhém snímky základní objekt blob pomocí [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx). Použít nový parametr **prevsnapshot** zadejte hodnotu data a času chcete získat rozdíl oproti snímku. Pokud tento parametr, REST odpověď bude obsahovat pouze stránky, které byly změněny mezi cíl snímku a předchozího snímku, včetně zrušte stránky.
* Použití [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) použití těchto změn na objekt blob stránky zálohování.
* Nakonec pořízení snímku objektů blob zálohy stránky a uloží jej v účtu úložiště záloh.

V další části jsme se podrobněji popisují jak můžete spravovat zálohy disků pomocí přírůstkové kopie snímků

## <a name="scenario"></a>Scénář
V této části jsme se popisují scénáře, který zahrnuje vlastní strategii zálohování pro disky virtuálního počítače pomocí snímků.

Zvažte DS-series virtuální počítač Azure s připojený disk P30 úložiště premium. P30 disk označuje *mypremiumdisk* je uložený v účtu úložiště premium názvem *mypremiumaccount*. Účet standardního úložiště s názvem *mybackupstdaccount* se použije pro uložení zálohy *mypremiumdisk*. Rádi bychom se zachovat snímek *mypremiumdisk* každých 12 hodin.

Další informace o vytváření účtu úložiště a disky, najdete v tématu [účty Azure storage](storage-create-storage-account.md).

Další informace o zálohování virtuálních počítačů Azure, najdete v tématu [zálohování virtuálního počítače Azure plánování](../backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Postup Udržovat zálohy disku pomocí přírůstkové snímků
Níže uvedené kroky bude trvat snímky *mypremiumdisk* a udržovat záloh v *mybackupstdaccount*. Zálohování bude objekt blob standardní stránky názvem *mybackupstdpageblob*. Objekt blob stránky zálohování bude vždy odrážet stejného stavu jako poslední snímek *mypremiumdisk*.

1. Nejprve vytvořte objekt blob zálohování stránky pro disk úložiště premium. K tomuto účelu pořízení snímku *mypremiumdisk* názvem *mypremiumdisk_ss1*.
2. Zkopírujte tento snímek do mybackupstdaccount jako objekt blob stránky názvem *mybackupstdpageblob*.
3. Pořízení snímku *mybackupstdpageblob* názvem *mybackupstdpageblob_ss1*pomocí [snímku Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) a uloží jej v *mybackupstdaccount*.
4. V daném intervalu zálohování vytvořit snímek jiného *mypremiumdisk*, například *mypremiumdisk_ss2*a ukládá je v *mypremiumaccount*.
5. Získat přírůstkové změny mezi dvěma snímky *mypremiumdisk_ss2* a *mypremiumdisk_ss1*pomocí [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) na *mypremiumdisk_ss2* s **prevsnapshot** parametr nastaven na časové razítko *mypremiumdisk_ss1*. Zapište tyto přírůstkové změny do objektů blob zálohy stránky *mybackupstdpageblob* v *mybackupstdaccount*. Pokud jsou v přírůstkové změny odstraněné rozsahy, je potřeba smazat z objektů blob zálohy stránky. Použití [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) k zápisu do objektů blob zálohy stránky přírůstkové změny.
6. Pořízení snímku objektů blob zálohy stránky *mybackupstdpageblob*, volané *mybackupstdpageblob_ss2*. Odstranit předchozí snímek *mypremiumdisk_ss1* z prémiový účet úložiště.
7. Opakujte kroky 4 až 6 každých intervalu zálohování. Tímto způsobem můžete udržovat zálohy *mypremiumdisk* v standardní účet úložiště.

![Zálohování pomocí přírůstkové snímků disku](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Postup obnovení disku ze snímků
Níže uvedené kroky obnoví premium disku *mypremiumdisk* na dřívějším snímkem z účtu úložiště pro zálohy *mybackupstdaccount*.

1. Identifikujte bod v čase, který chcete obnovit premium disk. Řekněme, který je snímek *mybackupstdpageblob_ss2*, který je uložený v účtu úložiště pro zálohy *mybackupstdaccount*.
2. V mybackupstdaccount, zvýšení úrovně snímku *mybackupstdpageblob_ss2* jako nový objekt blob zálohování základní stránky *mybackupstdpageblobrestored*.
3. Pořízení snímku tento objekt blob stránky obnovené zálohy, nazývá *mybackupstdpageblobrestored_ss1*.
4. Kopírovat objekt blob obnovené stránky *mybackupstdpageblobrestored* z *mybackupstdaccount* k *mypremiumaccount* jako nový disk premium *mypremiumdiskrestored*.
5. Pořízení snímku *mypremiumdiskrestored*, volané *mypremiumdiskrestored_ss1* pro provedení budoucí přírůstkové zálohování.
6. Bodu řady DS virtuální počítač obnovený disk *mypremiumdiskrestored* a odpojit starý *mypremiumdisk* z virtuálního počítače.
7. Zahájit proces zálohování popsané v předchozí části pro obnovené disk *mypremiumdiskrestored*pomocí *mybackupstdpageblobrestored* jako objekt blob zálohování stránky.

![Obnovení disku ze snímků](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Další kroky
Další informace o vytváření snímků objektu blob a plánování infrastruktury zálohování virtuálních počítačů přes odkazy níže.

* [Vytvoření snímku objektu Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx)
* [Plánování vaší infrastruktury zálohování virtuálních počítačů](../backup/backup-azure-vms-introduction.md)

