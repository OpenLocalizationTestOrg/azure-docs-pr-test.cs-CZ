---
title: "aaaUse snímky přírůstkové zálohování a obnovení nespravované disků virtuálního počítače Azure | Microsoft Docs"
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
ms.openlocfilehash: 6d3e6d78e953f77a1028ac35dcde1ef046dbc3bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky
## <a name="overview"></a>Přehled
Úložiště Azure poskytuje možnost hello tootake snímky objektů BLOB. Snímky zaznamenat stav hello objektů blob v tomto bodě v čase. V tomto článku jsme se popisují scénáře o tom, jak je možné uchovávat zálohy disků virtuálních počítačů pomocí snímků. Můžete použít tuto metodu, když zvolíte není toouse Azure zálohování a obnovení služby a chcete toocreate vlastní strategii zálohování pro disky virtuálního počítače.

Disky virtuálních počítačů Azure jsou uloženy jako objekty BLOB stránky ve službě Azure Storage. Vzhledem k tomu, že jsme mluvíme o strategie zálohování pro disky virtuálního počítače v tomto článku, jsme se odkazující toosnapshots v kontextu hello objekty BLOB stránky. toolearn Další informace o snímky odkazovat příliš[vytvoření snímku objektu Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Co je snímek?
Objekt blob snímek je jen pro čtení verze objektu blob zachycenou v bodě v čase. Po vytvoření snímku, ho můžete číst, kopírovat, nebo odstranit, ale nedojde ke změně. Snímky zadejte tooback způsob, jak se objekt blob, jak se objevuje v časovém okamžiku. Dokud REST verze 2015-04-05 bylo hello možnost toocopy úplné snímky. S hello REST verze 2015-07-08 a vyšší, můžete také můžete zkopírovat přírůstkové snímky.

## <a name="full-snapshot-copy"></a>Kopírování úplné snímku
Snímky lze zkopírovat tooanother účtu úložiště jako objektů blob zálohy tookeep hello základní objekt blob. Můžete také zkopírovat snímek přes jeho základní objekt blob, které je třeba obnovení hello blob tooan starší verze. Po zkopírování snímek z jednoho tooanother účet úložiště bude zabírat hello stejné místo jako objekt blob hello základní stránky. Proto kopírování celou snímky z jednoho tooanother účet úložiště bude pomalé a spotřebuje také velké množství místa v hello cílový účet úložiště.

> [!NOTE]
> Pokud zkopírujete cílové tooanother hello základní objekt blob, hello snímky objektu hello blob nezkopírují spolu s ním. Podobně přepíšete základní objekt blob se kopie, neovlivní snímky přidružené hello základní objekt blob a zůstanou beze změn v části název základního objektu blob.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Zálohovat disky pomocí snímků
Jako strategie zálohování pro disky virtuálního počítače, můžete pořizovat pravidelné snímky hello disku nebo stránky objektu blob a zkopírujte je tooanother účtu úložiště pomocí nástroje, například [kopírovat objekt Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) operaci nebo [AzCopy](storage-use-azcopy.md). Můžete zkopírovat snímku tooa cílový objekt blob stránky s jiným názvem. Hello výsledný objekt blob stránky cílové je objekt blob stránky lze zapisovat a není snímek. Později v tomto článku jsme se popisují kroky tootake zálohy disků virtuálních počítačů pomocí snímků.

### <a name="restore-disks-using-snapshots"></a>Obnovení disků pomocí snímků
Pokud nastane čas toorestore disku tooa předchozí stabilní verzi zachytit v jednom z hello snímků zálohy, můžete zkopírovat snímek přes hello základní stránky objektu blob. Objekt blob základní stránky propagovaných toohello po hello snímku hello snímku zůstane, ale dojde k přepsání zdrojem kopie, kterou může být číst a zapisovat. Později v tomto článku jsme se popisují kroky toorestore předchozí verze disku z jeho snímků.

### <a name="implementing-full-snapshot-copy"></a>Implementace kopie úplné snímku
Můžete implementovat kopii úplné snímku provedením následujících hello

* Nejprve vytvořte snímek hello základní objekt blob pomocí hello [snímku Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) operaci.
* Potom kopie hello snímku tooa cílové úložiště účtu pomocí [kopírovat objekt Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx).
* Opakujte tento proces toomaintain záložní kopie vaše základní objekt blob.

## <a name="incremental-snapshot-copy"></a>Kopírování přírůstkový snímek
Hello nová funkce v [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) rozhraní API poskytuje mnohem lepší způsob tooback až hello snímky objekty BLOB stránky nebo disků. Hello rozhraní API vrátí hello seznam změn mezi hello základní objekt blob a hello snímky. Tím se snižuje množství hello na účet záloh hello využitého prostoru úložiště. Hello rozhraní API podporuje objekty BLOB stránky na Storage úrovně Premium, jakož i úložiště Standard Storage. Pomocí tohoto rozhraní API, teď můžete sestavit rychlejší a efektivní řešení zálohování pro virtuální počítače Azure. Bude k dispozici hello REST verze 2015-07-08 a vyšší.

Přírůstkové kopie snímků vám umožní toocopy z jednoho úložiště účet tooanother hello rozdíl mezi,

* Základní objekt blob a jeho snímek nebo
* Všechny dva snímky základní objekt blob hello

Pokud jsou splněny následující podmínky hello,

* Objekt blob Hello byl vytvořen ledna-1-2016 nebo novější.
* nebyl k přepsání objektů blob Hello [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) nebo [kopírovat objekt Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) mezi dvěma snímky.

**Poznámka:**: Tato funkce je dostupná pro Premium a Standard objekty BLOB stránky Azure.

Pokud máte vlastní strategii zálohování, který používá snímky kopírování hello snímky z jednoho tooanother účet úložiště může být velmi pomalé a spotřebuje velké množství prostoru úložiště. Namísto kopírování hello celý snímek tooa úložiště záloh účet, můžete napsat hello rozdíl mezi po sobě jdoucí snímky tooa zálohování stránky objektu blob. Tímto způsobem hello čas toocopy a místo toostore zálohy je podstatně snížit.

### <a name="implementing-incremental-snapshot-copy"></a>Implementace přírůstkový snímek kopie
Kopírování přírůstkový snímek můžete implementovat pomocí tohoto postupu následující hello

* Pořízení snímku hello základní objekt blob pomocí [snímku Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx).
* Kopírování hello snímku toohello cílového úložiště pro zálohy účtu pomocí [kopírovat objekt Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). To bude objekt blob hello zálohování stránky. Pořízení snímku tento objekt blob stránky zálohování a uložení v zálohování účtu.
* Vytvořte nový snímek objektu blob základní hello pomocí snímku objektů Blob.
* Získat hello rozdíl mezi prvním a druhém snímky základní objekt blob pomocí [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx). Použít nový parametr hello **prevsnapshot** toospecify hello hello snímek, který se má tooget hello rozdíl oproti hodnotu DateTime. Pokud tento parametr, hello REST odpověď bude obsahovat pouze hello stránky, které byly změněny mezi cíl snímku a předchozího snímku, včetně zrušte stránky.
* Použití [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) tooapply objekt blob se tyto změny toohello zálohování stránky.
* Nakonec pořízení snímku objektů blob zálohy stránky hello a uloží jej v účtu úložiště pro zálohy hello.

V další části hello jsme se podrobněji popisují jak můžete spravovat zálohy disků pomocí přírůstkové kopie snímků

## <a name="scenario"></a>Scénář
V této části jsme se popisují scénáře, který zahrnuje vlastní strategii zálohování pro disky virtuálního počítače pomocí snímků.

Zvažte DS-series virtuální počítač Azure s připojený disk P30 úložiště premium. Hello P30 disku s názvem *mypremiumdisk* je uložený v účtu úložiště premium názvem *mypremiumaccount*. Účet standardního úložiště s názvem *mybackupstdaccount* se použije pro uložení zálohy hello *mypremiumdisk*. Rádi bychom znali tookeep snímek z *mypremiumdisk* každých 12 hodin.

toolearn o vytvoření účtu úložiště a disky, najdete v příliš[účty Azure storage](storage-create-storage-account.md).

toolearn o zálohování virtuálních počítačů Azure, najdete v příliš[zálohování virtuálního počítače Azure plánování](../backup/backup-azure-vms-introduction.md).

## <a name="steps-toomaintain-backups-of-a-disk-using-incremental-snapshots"></a>Kroky toomaintain zálohy disku pomocí přírůstkové snímků
Hello provedení níže popsaných kroků bude trvat snímky *mypremiumdisk* a udržovat hello záloh v *mybackupstdaccount*. Hello zálohování bude objekt blob standardní stránky názvem *mybackupstdpageblob*. Hello objekt blob stránky zálohování bude vždy odrážet hello stejné stavu jako poslední snímek hello *mypremiumdisk*.

1. Nejprve vytvořte objekt blob hello zálohování stránky pro disk úložiště premium. toodo se proveďte snímek *mypremiumdisk* názvem *mypremiumdisk_ss1*.
2. Zkopírujte tento snímek toomybackupstdaccount jako objekt blob stránky názvem *mybackupstdpageblob*.
3. Pořízení snímku *mybackupstdpageblob* názvem *mybackupstdpageblob_ss1*pomocí [snímku Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) a uloží jej v *mybackupstdaccount*.
4. Během intervalu zálohování hello vytvořit snímek jiného *mypremiumdisk*, například *mypremiumdisk_ss2*a ukládá je v *mypremiumaccount*.
5. Získat hello přírůstkové změny mezi hello dva snímky *mypremiumdisk_ss2* a *mypremiumdisk_ss1*pomocí [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) na  *mypremiumdisk_ss2* s **prevsnapshot** parametr nastavit časové razítko toohello *mypremiumdisk_ss1*. Zápis objektů blob zálohy stránky tyto přírůstkové změny toohello *mybackupstdpageblob* v *mybackupstdaccount*. Pokud jsou v přírůstkové změny hello odstraněné rozsahy, je potřeba smazat z objektu blob zálohování stránku hello. Použití [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) toowrite objektů blob zálohy stránky toohello přírůstkové změny.
6. Pořízení snímku objektů blob zálohy stránky hello *mybackupstdpageblob*, volané *mybackupstdpageblob_ss2*. Odstranit předchozí snímek hello *mypremiumdisk_ss1* z prémiový účet úložiště.
7. Opakujte kroky 4 až 6 každých intervalu zálohování. Tímto způsobem můžete udržovat zálohy *mypremiumdisk* v standardní účet úložiště.

![Zálohování pomocí přírůstkové snímků disku](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-toorestore-a-disk-from-snapshots"></a>Kroky toorestore disk ze snímků
Hello níže uvedené kroky obnoví premium disku *mypremiumdisk* tooan dříve snímek z účtu úložiště pro zálohy hello *mybackupstdaccount*.

1. Identifikaci hello bodu v čase, které chcete toorestore hello premium disk. Řekněme, který je snímek *mybackupstdpageblob_ss2*, který je uložený v účtu úložiště pro zálohy hello *mybackupstdaccount*.
2. V mybackupstdaccount, zvýšení úrovně hello snímku *mybackupstdpageblob_ss2* jako nový blob zálohování základní stránku hello *mybackupstdpageblobrestored*.
3. Pořízení snímku tento objekt blob stránky obnovené zálohy, nazývá *mybackupstdpageblobrestored_ss1*.
4. Kopírovat objekt blob stránky hello obnovit *mybackupstdpageblobrestored* z *mybackupstdaccount* příliš*mypremiumaccount* jako nový disk premium hello  *mypremiumdiskrestored*.
5. Pořízení snímku *mypremiumdiskrestored*, volané *mypremiumdiskrestored_ss1* pro provedení budoucí přírůstkové zálohování.
6. Bodu řady hello DS disku toohello obnovit virtuální počítač *mypremiumdiskrestored* a odpojit hello staré *mypremiumdisk* z hello virtuálních počítačů.
7. Spusťte proces zálohování hello popsané v předchozí části pro disk hello obnovit *mypremiumdiskrestored*, pomocí hello *mybackupstdpageblobrestored* jako objekt blob hello zálohování stránky.

![Obnovení disku ze snímků](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Další kroky
Další informace o vytváření snímků objektu blob a plánování infrastruktury zálohování virtuálních počítačů pomocí hello odkazy níže.

* [Vytvoření snímku objektu Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx)
* [Plánování vaší infrastruktury zálohování virtuálních počítačů](../backup/backup-azure-vms-introduction.md)

