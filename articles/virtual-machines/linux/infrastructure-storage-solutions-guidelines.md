---
title: "aaaStorage řešení pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro nasazení řešení úložiště ve službách infrastruktury Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3c368f07-189b-4520-bbe2-f4080253bbf2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d270c4786d7b55b18b011aa345063b6816a80876
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-infrastructure-guidelines-for-linux-vms"></a>Pokyny infrastruktury úložiště Azure pro virtuální počítače s Linuxem

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na rozumět potřebám úložiště a aspekty návrhu pro dosažení optimálního virtuální počítač (VM) výkonu.

## <a name="implementation-guidelines-for-storage"></a>Postup implementace pro úložiště
Rozhodnutí:

* Budete toouse disky Azure spravované nebo nespravované disky?
* Potřebujete toouse Standard nebo Premium úložiště pro úlohy?
* Potřebujete disky toocreate proložení disku větší než 4TB?
* Potřebujete pro úlohy proložení tooachieve optimální vstupně-výstupní výkon disku?
* Jaká sada účtů úložiště potřebujete toohost IT zatížení nebo infrastrukturu?

Úlohy:

* Zkontrolujte požadavky na vstupně-výstupních operací aplikace hello nasazujete a plánování hello odpovídající počet a typ úložiště účtů.
* Vytvořte sadu hello účty úložiště používající zásady vytváření názvů. Můžete použít portál Azure CLI nebo hello hello.

## <a name="storage"></a>Úložiště
Azure Storage je klíčovou součástí nasazení a správa virtuálních počítačů (VM) a aplikací. Úložiště Azure poskytuje služby pro ukládání dat souboru, nestrukturovaných dat a zprávy, který je taky součástí hello infrastruktury pro podporu virtuálních počítačů.

[Disky systému Azure spravované](../../storage/storage-managed-disks-overview.md) zpracovává úložiště pro vás pozadí hello. S nespravované disky můžete vytvořit účty úložiště toohold hello disky (soubory VHD) pro virtuální počítače Azure. Při rozšiřování prostředků, musíte se ujistit, že kterou jste vytvořili další účty úložiště, takže nepřekračují hello limit IOPS pro úložiště s žádným disky. S spravované disky zpracování úložiště, můžete se už není omezené podle hello limity účtu úložiště (například 20 000 IOPS / účet). Také již máte toocopy účtů úložiště toomultiple vlastních bitových kopií (soubory VHD). Můžete spravovat v centrálním umístění – jeden účet úložiště podle oblasti Azure – a použít je toocreate stovky virtuálních počítačů v předplatném. Doporučujeme že používat spravované disky pro nová nasazení.

K dispozici pro podporu virtuálních počítačů existují dva typy účtů úložiště:

* Udělte účty úložiště Standard storage přístup tooblob úložiště (používá se pro ukládání virtuálních počítačů Azure disky), tabulky úložiště, fronty úložiště a úložiště souborů.
* [Storage úrovně Premium](../../storage/storage-premium-storage.md) účty poskytovat podporu disků vysoce výkonné, nízká latence pro zatížení s intenzivním vstupně-výstupních operací, jako je například horizontálně dělené MongoDB clusteru. Storage úrovně Premium aktuálně podporuje jen disky virtuálního počítače Azure.

Azure vytvoří virtuální počítače s diskem operačního systému, dočasným diskovým a nula nebo více disků volitelnými daty. Hello disk operačního systému a datové disky jsou objekty BLOB stránky Azure, zatímco dočasným diskovým hello uložených místně na uzlu hello tam, kde počítač hello platný. Vezměte v potaz při návrhu aplikace tooonly použít toto dočasný disku pro dočasnou data jako hello, může použít k migraci virtuálních počítačů mezi hostiteli během údržby. Všechna data uložená na disku dočasné hello bude ztracena.

Odolnost a vysoká dostupnost zajišťuje hello tooensure prostředí základní Azure Storage, aby vaše data i nadále chráněn proti neplánované údržby nebo k selhání hardwaru. Při navrhování prostředí Azure Storage, můžete zvolit tooreplicate úložiště virtuálního počítače:

* místně v rámci dané datové centrum Azure
* v datových centrech Azure v rámci dané oblasti
* v datových centrech Azure v různých oblastech.

Můžete si přečíst [Další informace o možnostech replikace hello pro zajištění vysoké dostupnosti](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Disky operačního systému a datové disky mají maximální velikost 4 TB. Toosurpass Manager logické svazek (LVM) můžete použít tento limit podle sdružování společně datové disky toopresent logické svazky větší než 1023GB tooyour virtuálních počítačů.

Existují některá omezení škálovatelnosti při návrhu nasazení Azure Storage – Další informace najdete v tématu [limitů předplatného a služby Microsoft Azure, kvóty a omezení](../../azure-subscription-service-limits.md#storage-limits). Viz také [úložiště Azure škálovatelnosti a cílech výkonnosti](../../storage/storage-scalability-targets.md).

Pro uložení aplikace můžete uložit nestrukturovaných dat, jako jsou dokumenty, obrázky, zálohování, konfigurační data, protokoly, atd. Pomocí úložiště objektů blob. Namísto aplikace zápis toohello tooa připojen virtuální disk virtuálního počítače může aplikace hello přímo zapisovat tooAzure úložiště objektů blob. Úložiště objektů BLOB také poskytuje možnost hello [za provozu a cool vrstvy úložiště](../../storage/storage-blob-storage-tiers.md) v závislosti na dostupnosti požadavky a náklady na omezení.

## <a name="striped-disks"></a>Prokládané disky
Kromě toho, abyste toocreate disky větší než 1023 GB v velký počet instancí, pomocí proložení pro datové disky zlepšuje výkon tím, že několik objektů BLOB tooback hello úložiště pro jeden svazek. S proložení, hello vstupně-výstupní operace vyžaduje toowrite a čtení dat z jednoho logického disku pokračuje paralelně.

Systému Azure vynucuje omezení pro hello počet datových disků a k dispozici, v závislosti na velikosti virtuálního počítače hello šířku pásma. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md)

Pokud používáte disk proložení pro Azure datových disků, vezměte v úvahu hello následující pokyny:

* Připojte hello maximální datových disků povolená pro hello velikost virtuálního počítače.
* Použijte LVM.
* Nepoužívejte možnosti ukládání do mezipaměti Azure datový disk (ukládání do mezipaměti zásad = None).

Další informace najdete v tématu [konfigurace LVM na virtuální počítač s Linuxem](configure-lvm.md).

## <a name="multiple-storage-accounts"></a>Více účtů úložiště
Tato část se nevztahuje příliš[Azure spravované disky](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), protože nelze vytvořit samostatné úložiště účtů. 

Když navrhujete prostředí Azure Storage pro nespravovaná disky, můžete použít více účtů úložiště jako hello počet virtuálních počítačů můžete nasadit zvyšuje. Tento přístup pomáhá distribuovat out hello vstupně-výstupní operace přes hello základní Azure Storage infrastruktury toomaintain optimálního výkonu pro virtuální počítače a aplikace. Při navrhování hello aplikace, které nasazujete, zvažte hello vstupně-výstupních operací požadavky, které má každý virtuální počítač a vyrovnávat na těchto virtuálních počítačů mezi různými účty úložiště Azure. Zkuste tooavoid seskupení všech hello vysoké vstupně-výstupních operací náročných v účtech úložiště toojust jeden nebo dva virtuální počítače.

Další informace o funkcích hello vstupně-výstupních operací hello různých možností úložiště Azure a některé doporučujeme maximální možné limity, najdete v části [úložiště Azure škálovatelnosti a cílech výkonnosti](../../storage/storage-scalability-targets.md).

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

