---
title: "aaaMigrate z AWS a jiné platformy tooManaged disky v Azure | Microsoft Docs"
description: "Vytvořit virtuální počítače v Azure pomocí virtuální pevné disky odeslán z ostatních cloudů jako AWS nebo jiných virtualizačních platforem a využijte výhod Azure spravované disků."
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
ms.date: 02/07/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 66c3912397ab905aafb3910e13ac711befb8f502
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-amazon-web-services-aws-and-other-platforms-toomanaged-disks-in-azure"></a>Migrace z Amazon Web Services (AWS) a jiné platformy tooManaged disky v Azure

Můžete nahrát soubory virtuálního pevného disku z AWS nebo místní virtualizace řešení tooAzure toocreate virtuálních počítačů, které využít výhod spravované disky. Disky systému Azure spravované odebere hello nutné toomanaging účtů úložiště pro virtuální počítače Azure IaaS. Můžete mít tooonly zadat typ hello (Standard nebo Premium) a velikost disku je nutné, a bude Azure vytvářet a spravovat hello disku pro vás. 

Můžete nahrát zobecněný a specializované virtuálních pevných disků. 
- **Zobecněný virtuální pevný disk** -zaznamenala všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep. 
- **Specializuje virtuálního pevného disku** -udržuje hello uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače. 

> [!IMPORTANT]
> Před nahráním žádné tooAzure virtuálního pevného disku, postupujte podle [připravit tooAzure tooupload Windows VHD nebo VHDX](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
>
>


| Scénář                                                                                                                         | Dokumentace                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Máte existující instance AWS EC2, který může jako toomigrate tooAzure spravované disky                                     | [Přesunout virtuální počítač z tooAzure Amazon Web Services (AWS)](aws-to-azure.md)                           |
| Máte z virtuálního počítače a jiné virtualizační platformy, které chcete toouse toouse jako toocreate image více virtuálních počítačů Azure. | [Nahrát zobecněný virtuální pevný disk a použít ho toocreate nové virtuální počítače v Azure](upload-generalized-managed.md) |
| Musíte jednoznačně přizpůsobené virtuálního počítače, které chcete toorecreate v Azure.                                                      | [Nahrát specializované tooAzure virtuální pevný disk a vytvořte nový virtuální počítač](create-vm-specialized.md)         |


## <a name="overview-of-managed-disks"></a>Přehled spravovaných disků

Disky systému Azure spravované usnadňuje správu virtuálních počítačů odebráním hello nutné toomanage úložiště účtů. Spravovat disky také výhody z vyšší spolehlivost virtuálních počítačů ve skupině dostupnosti. Zajišťuje, že hello disky pro různé virtuální počítače ve skupině dostupnosti bude dostatečně izolované od každé další tooavoid jediný bod selhání. Automaticky umístí disky pro různé virtuální počítače do skupiny dostupnosti v různých jednotek škálování úložiště (razítka) omezující hello dopad chyby škálování jednotky jednoho úložiště se nezdařila z důvodu z důvodu selhání toohardware a softwaru. V závislosti na vašich potřebách můžete vybrat ze dvou typů možností úložiště: 
 
- [Pro prémiové disky spravované](../../storage/common/storage-premium-storage.md) jsou plnou stavu jednotky SSD (Solid-State Drive) na základě úložiště úložná média, který dodává highperformance, podporu disků s nízkou latencí pro virtuální počítače běžící I náročnými úlohy. Můžete využít výhod hello rychlosti a výkonu z těchto disků pomocí migrace tooPremium spravované disky.  

- [Standardní disky spravované](../../storage/common/storage-standard-storage.md) používat jednotku pevného disku (HDD) na základě paměťových médií a jsou nejvhodnější pro vývoj/testování a další úlohy nepravidelným přístupu, které jsou méně citlivou variabilita tooperformance.  

## <a name="plan-for-hello-migration-toomanaged-disks"></a>Plánování migrace hello tooManaged disky

Tato část vám pomůže toomake hello nejlepší rozhodnutí o typy virtuálního počítače a disku.


### <a name="location"></a>Umístění

Vyberte umístění, kde Azure spravované disky jsou dostupné. Při migraci disků spravovaných tooPremium, ujistěte se také, že úložiště Premium je k dispozici v hello oblasti, kde jsou plánování toomigrate k. V tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services) aktuální informace o dostupných umístění.

### <a name="vm-sizes"></a>Velikost virtuálních počítačů

Při migraci disků spravovaných tooPremium, máte tooupdate hello velikost virtuálního počítače tooPremium hello podporující velikost úložiště k dispozici v hello oblasti, kde je umístěn virtuální počítač. Zkontrolujte hello velikosti virtuálních počítačů, které jsou podporující Storage úrovně Premium. Specifikace velikosti virtuálního počítače Azure Hello jsou uvedeny v [velikosti virtuálních počítačů](sizes.md).
Zkontrolujte hello výkonové charakteristiky virtuálních počítačů, které pracovat s Storage úrovně Premium a zvolte hello nejvhodnější velikost virtuálního počítače, který nejlépe vyhovuje vaší zatížení. Ujistěte se, zda je dostatečnou šířku pásma k dispozici na disku provozu hello toodrive virtuálních počítačů.

### <a name="disk-sizes"></a>Velikost disků

**Premium spravované disky**

Existují tři typy Premium spravované disky, které lze použít s vaší virtuálních počítačů a každá z nich má konkrétní IOPs a propustnost omezení. Při výběru hello Premium typ disku pro virtuální počítač podle potřeb hello vaší aplikace z hlediska kapacity, výkon, škálovatelnost a načte ve špičce, zvažte tyto limity.

| Disky typu Premium  | P10               | P20               | P30               |
|---------------------|-------------------|-------------------|-------------------|
| Velikost disku           | 128 GB            | 512 GB            | 1024 GB (1 TB)    |
| Vstupně-výstupní operace za sekundu / disk       | 500               | 2300              | 5000              |
| Propustnost / disk | 100 MB za sekundu | 150 MB za sekundu | 200 MB za sekundu |

**Standardní disky spravované**

Existuje pět typů standardní spravované disků, které lze použít s virtuálního počítače. Každý z nich mají různé kapacity ale mít stejné IOPS a omezení propustnosti. Vyberte typ hello standardní spravovaných disků na základě potřeb kapacity hello vaší aplikace.

| Disk typu Standard  | S4               | S6               | S10              | S20              | S30              |
|---------------------|------------------|------------------|------------------|------------------|------------------|
| Velikost disku           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   |
| Vstupně-výstupní operace za sekundu / disk       | 500              | 500              | 500              | 500              | 500              |
| Propustnost / disk | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu |

### <a name="disk-caching-policy"></a>Zásady ukládání do mezipaměti na disku 

**Premium spravované disky**

Ve výchozím nastavení, je disk ukládání do mezipaměti zásad *jen pro čtení* pro všechny hello Premium datových disků, a *pro čtení a zápis* pro disk operačního systému Premium hello připojené toohello virtuálních počítačů. Toto nastavení konfigurace se doporučuje tooachieve hello optimální výkon pro aplikace IOs. Těžký zápisu nebo pouze pro zápis datové disky (například soubory protokolu serveru SQL Server) zakažte ukládání do mezipaměti disku, takže můžete dosáhnout lepší výkon aplikace.

### <a name="pricing"></a>Ceny

Zkontrolujte hello [ceny pro spravované disky](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Ceny disků spravované Premium je stejný jako hello nespravované prémiové disky. Ale ceny pro standardní disky spravované se liší od standardní nespravované disky.


## <a name="next-steps"></a>Další kroky

- Před nahráním žádné tooAzure virtuálního pevného disku, postupujte podle [připravit tooAzure tooupload Windows VHD nebo VHDX](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
