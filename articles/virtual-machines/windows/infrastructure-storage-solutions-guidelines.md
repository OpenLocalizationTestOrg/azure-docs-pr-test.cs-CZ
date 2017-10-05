---
title: "Řešení úložiště pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro nasazení řešení úložiště ve službách infrastruktury Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ceb7d32-7a0d-4004-b701-c2bbcaff6b0b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ba0d66c5af771ebcb3ca8e6742e15a5506d8fd61
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a>Pokyny infrastruktury úložiště Azure pro virtuální počítače Windows

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na rozumět potřebám úložiště a aspekty návrhu pro dosažení optimálního virtuální počítač (VM) výkonu.

## <a name="implementation-guidelines-for-storage"></a>Postup implementace pro úložiště
Rozhodnutí:

* Chystáte se používat Azure disky spravované nebo nespravované disky?
* Je třeba použít Standard nebo Premium úložiště pro úlohy?
* Je potřeba prokládání disků k vytvoření disky větší než 4TB?
* Je potřeba prokládání disků k dosažení optimálního výkonu vstupně-výstupních operací pro vaše úlohy?
* Jaká sada účtů úložiště potřebujete k hostování zatížení IT nebo infrastrukturu?

Úlohy:

* Zkontrolujte požadavky na vstupně-výstupní operace aplikací, nasazení a naplánovat odpovídající počet a typ úložiště účtů.
* Vytvořte sadu účtů úložiště pomocí zásady vytváření názvů. Můžete použít Azure PowerShell nebo portálu.

## <a name="storage"></a>Úložiště
Azure Storage je klíčovou součástí nasazení a správa virtuálních počítačů (VM) a aplikací. Úložiště Azure poskytuje služby pro ukládání dat souboru, nestrukturovaných dat a zprávy, který je taky součástí infrastruktury podpora virtuálních počítačů.

[Disky systému Azure spravované](../../storage/storage-managed-disks-overview.md) zpracovává úložiště pro vás na pozadí. S nespravované disky vytvořte účty úložiště pro uložení disky (soubory VHD) pro virtuální počítače Azure. Při rozšiřování prostředků, musíte se ujistit, že kterou jste vytvořili další účty úložiště, takže nepřekračují limit IOPS pro úložiště s žádným z vaše disky. S spravované disky zpracování úložiště, můžete se už není omezené podle limity účtu úložiště (například 20 000 IOPS / účet). Také již budete muset zkopírovat vlastních bitových kopií (soubory VHD) k několika účtům úložiště. Můžete spravovat v centrálním umístění – jeden účet úložiště podle oblasti Azure – a pomocí nich vytvořte stovky virtuálních počítačů v předplatném. Doporučujeme že používat spravované disky pro nová nasazení.

K dispozici pro podporu virtuálních počítačů existují dva typy účtů úložiště:

* Udělte účty úložiště Standard storage přístup do úložiště objektů blob (používá se pro ukládání virtuálních počítačů Azure disky), úložiště table, úložiště queue a úložiště file.
* [Storage úrovně Premium](../../storage/storage-premium-storage.md) účty poskytovat podporu disků vysoce výkonné, nízká latence pro zatížení s intenzivním vstupně-výstupních operací, jako je například SQL servery v clusteru služby AlwaysOn. Storage úrovně Premium aktuálně podporuje jen disky virtuálního počítače Azure.

Azure vytvoří virtuální počítače s diskem operačního systému, dočasným diskovým a nula nebo více disků volitelnými daty. Disk operačního systému a datové disky jsou objekty BLOB stránky Azure, zatímco dočasným diskovým uložených místně na uzlu, kde je umístěn na počítač. Vezměte v potaz při návrhu aplikace použít pouze tento dočasný disk pro dočasnou data jako virtuální počítač může být migrovat mezi hostiteli během údržby. Všechna data uložená na dočasném disku bude ztracena.

Odolnost a vysoká dostupnost zajišťuje základní prostředí Azure Storage k zajištění, že vaše data i nadále chráněn proti neplánované údržby nebo k selhání hardwaru. Při navrhování prostředí Azure Storage, můžete replikovat úložiště virtuálního počítače:

* místně v rámci dané datové centrum Azure
* v datových centrech Azure v rámci dané oblasti
* v datových centrech v různých oblastech Azure

Můžete si přečíst [Další informace o možnostech replikace pro zajištění vysoké dostupnosti](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Disky operačního systému a datové disky mají maximální velikost 4 TB. Prostory úložiště v systému Windows Server 2012 nebo novějším můžete překročí toto omezení pomocí datových disků do virtuálního počítače prezentovat logické svazky větší než 4TB sdružování společně.

Existují některá omezení škálovatelnosti při návrhu nasazení Azure Storage – Další informace najdete v tématu [limitů předplatného a služby Microsoft Azure, kvóty a omezení](../../azure-subscription-service-limits.md#storage-limits). Viz také [úložiště Azure škálovatelnosti a cílech výkonnosti](../../storage/storage-scalability-targets.md).

Pro uložení aplikace můžete uložit nestrukturovaných dat, jako jsou dokumenty, obrázky, zálohování, konfigurační data, protokoly, atd. Pomocí úložiště objektů blob. Namísto aplikace zápis do virtuální disk připojený k virtuálnímu počítači může aplikace zapisovat přímo do Azure blob storage. Úložiště objektů BLOB také poskytuje možnost [za provozu a cool vrstvy úložiště](../../storage/storage-blob-storage-tiers.md) v závislosti na dostupnosti požadavky a náklady na omezení.

## <a name="striped-disks"></a>Prokládané disky
Kromě toho umožňuje vytvářet disky větší než 4TB v mnoha případech pomocí proložení pro datové disky zlepšuje výkon tím, že několik objektů blob pro zálohování úložiště pro jeden svazek. S proložení, vstupně-výstupních operací nutná k zápisu a čtení dat z jednoho logického disku pokračuje paralelně.

Systému Azure vynucuje omezení pro počet datových disků a šířku pásma, které jsou k dispozici, v závislosti na velikosti virtuálního počítače. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).

Pokud používáte disk proložení pro Azure datových disků, vezměte v úvahu následující pokyny:

* Připojení maximální datových disků pro velikost virtuálního počítače.
* Prostory úložiště.
* Nepoužívejte možnosti ukládání do mezipaměti Azure datový disk (ukládání do mezipaměti zásad = None).

Další informace najdete v tématu [prostory úložiště – návrh a výkon](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).

## <a name="multiple-storage-accounts"></a>Více účtů úložiště
Tato část se nevztahuje na [Azure spravované disky](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), protože nelze vytvořit samostatné úložiště účtů. 

Když navrhujete prostředí Azure Storage pro nespravovaná disky, můžete použít více účtů úložiště jako počet virtuálních počítačů můžete nasadit zvyšuje. Tento přístup pomáhá distribuovat na vstupy/výstupy mezi základní infrastrukturu Azure Storage tak, aby udržení optimálního výkonu pro virtuální počítače a aplikace. Při navrhování aplikace, které nasazujete, zvažte požadavky na vstupně-výstupních operací, které má každý virtuální počítač a vyrovnávat na těchto virtuálních počítačů mezi různými účty úložiště Azure. Pokuste se vyhnout, všechny vysoké vstupně-výstupních operací náročných na jen jeden nebo dva účty úložiště virtuálních počítačů v seskupení.

Další informace o možnostech vstupně-výstupních operací různé možnosti Azure Storage a některé doporučujeme maximální možné limity, najdete v části [úložiště Azure škálovatelnosti a cílech výkonnosti](../../storage/storage-scalability-targets.md).

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

