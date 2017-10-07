---
title: "aaaMigrate tooManaged disky virtuálních počítačů Azure | Microsoft Docs"
description: "Migrujte virtuální počítače Azure vytvořené pomocí nespravované disky v toouse účty úložiště spravované disky."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 29420f13c4ffd5b25726e0ef1aafe89347286a89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-vms-toomanaged-disks-in-azure"></a>Migrovat virtuální počítače Azure tooManaged disky v Azure

Disky systému Azure spravované usnadňuje správu úložiště odebráním hello nutné tooseparately spravovat účty úložiště.  Vaše stávající toobenefit tooManaged disky virtuálních počítačích Azure můžete také migrovat z vyšší spolehlivost virtuálních počítačů ve skupině dostupnosti. Zajišťuje, že hello disky pro různé virtuální počítače ve skupině dostupnosti bude dostatečně izolované od každé další tooavoid jediný bod selhání. Automaticky umístí disky pro různé virtuální počítače do skupiny dostupnosti v různých jednotek škálování úložiště (razítka) omezující hello dopad chyby škálování jednotky jednoho úložiště se nezdařila z důvodu z důvodu selhání toohardware a softwaru.
V závislosti na vašich potřebách můžete vybrat ze dvou typů možností úložiště:

- [Pro prémiové disky spravované](../../storage/common/storage-premium-storage.md) jsou plnou stavu jednotky SSD (Solid-State Drive) založené na úložná média, který dodává highperformance, podporu disků s nízkou latencí pro virtuální počítače běžící I náročnými úlohy. Můžete využít výhod hello rychlosti a výkonu z těchto disků pomocí migrace tooPremium spravované disky.

- [Standardní disky spravované](../../storage/common/storage-standard-storage.md) používat jednotku pevného disku (HDD) na základě paměťových médií a jsou nejvhodnější pro vývoj/testování a další úlohy nepravidelným přístupu, které jsou méně citlivou variabilita tooperformance.

Můžete migrovat tooManaged disky v těchto scénářích:

| Migrujte...                                            | Dokumentace k propojení                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Převést samostatný virtuální počítače a virtuální počítače discích toomanaged sadu dostupnosti   | [Převeďte disky toouse spravovat virtuální počítače](convert-unmanaged-to-managed-disks.md) |
| Jeden virtuální počítač z classic tooResource Manager na discích spravovaných     | [Migrovat jeden virtuální počítač](migrate-single-classic-to-resource-manager.md)  | 
| Všechny virtuální počítače hello ve virtuální síti ze classic tooResource Manager na discích spravovaných     | [Migrovat prostředky infrastruktury z classic tooResource Manager](migration-classic-resource-manager-ps.md) a potom [převést virtuální počítač z nespravovaných disky toomanaged disků](convert-unmanaged-to-managed-disks.md) | 






## <a name="plan-for-hello-conversion-toomanaged-disks"></a>Plánování pro převod hello tooManaged disky

Tato část vám pomůže toomake hello nejlepší rozhodnutí o typy virtuálního počítače a disku.


## <a name="location"></a>Umístění

Vyberte umístění, kde Azure spravované disky jsou dostupné. Je-li přesouváte tooPremium spravované disky, zkontrolujte také, že úložiště Premium je k dispozici v hello oblasti, kde jsou plánování toomove k. V tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services) aktuální informace o dostupných umístění.

## <a name="vm-sizes"></a>Velikost virtuálních počítačů

Při migraci disků spravovaných tooPremium, máte tooupdate hello velikost virtuálního počítače tooPremium hello podporující velikost úložiště k dispozici v hello oblasti, kde je umístěn virtuální počítač. Zkontrolujte hello velikosti virtuálních počítačů, které jsou podporující Storage úrovně Premium. Specifikace velikosti virtuálního počítače Azure Hello jsou uvedeny v [velikosti virtuálních počítačů](sizes.md).
Zkontrolujte hello výkonové charakteristiky virtuálních počítačů, které pracovat s Storage úrovně Premium a zvolte hello nejvhodnější velikost virtuálního počítače, který nejlépe vyhovuje vaší zatížení. Ujistěte se, zda je dostatečnou šířku pásma k dispozici na disku provozu hello toodrive virtuálních počítačů.

## <a name="disk-sizes"></a>Velikost disků

**Premium spravované disky**

Existují sedm typy premium spravované disky, které lze použít s vaší virtuálních počítačů a každá z nich má konkrétní IOPs a propustnost omezení. Vzít v úvahu tyto limity při výběru hello Premium typ disku pro virtuální počítač podle potřeb hello vaší aplikace z hlediska kapacity, výkon, škálovatelnost a načte ve špičce.

| Disky typu Premium  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Velikost disku           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2 048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Vstupně-výstupní operace za sekundu / disk       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Propustnost / disk | 25 MB za sekundu  | 50 MB za sekundu  | 100 MB za sekundu | 150 MB za sekundu | 200 MB za sekundu | 250 MB za sekundu | 250 MB za sekundu |

**Standardní disky spravované**

Existují sedm typy standardní spravovaných disků, které lze použít s virtuálního počítače. Každý z nich mají různé kapacity ale mít stejné IOPS a omezení propustnosti. Vyberte typ hello standardní spravovaných disků na základě potřeb kapacity hello vaší aplikace.

| Disk typu Standard  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Velikost disku           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2 048 GB (2TB)    | 4095 GB (4 TB)   | 
| Vstupně-výstupní operace za sekundu / disk       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Propustnost / disk | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 

## <a name="disk-caching-policy"></a>Zásady ukládání do mezipaměti na disku

**Premium spravované disky**

Ve výchozím nastavení, je disk ukládání do mezipaměti zásad *jen pro čtení* pro všechny hello Premium datových disků, a *pro čtení a zápis* pro disk operačního systému Premium hello připojené toohello virtuálních počítačů. Toto nastavení konfigurace se doporučuje tooachieve hello optimální výkon pro aplikace IOs. Těžký zápisu nebo pouze pro zápis datové disky (například soubory protokolu serveru SQL Server) zakažte ukládání do mezipaměti disku, takže můžete dosáhnout lepší výkon aplikace.

## <a name="pricing"></a>Ceny

Zkontrolujte hello [ceny pro spravované disky](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Ceny disků spravované Premium je stejný jako hello nespravované prémiové disky. Ale ceny pro standardní disky spravované se liší od standardní nespravované disky.



## <a name="next-steps"></a>Další kroky

- Další informace o [spravované disky](managed-disks-overview.md)
