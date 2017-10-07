---
title: "aaaAvailability nastaví pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro nasazení sad dostupnosti ve službách infrastruktury Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 24f1d91c-8cc0-4251-bb67-ac4c4c37e8cd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 78e4da5c8fd9eb186cafacb46454444b73a9439c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-availability-sets-guidelines-for-linux-vms"></a>Azure dostupnosti nastavuje pravidla pro virtuální počítače s Linuxem

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek zaměřuje na pochopení hello požadované kroky plánování pro sady dostupnosti tooensure aplikace zůstávají dostupné během plánované i neplánované události.

## <a name="implementation-guidelines-for-availability-sets"></a>Postup implementace pro skupiny dostupnosti
Rozhodnutí:

* Kolik skupiny dostupnosti je nutný pro hello různých rolí a úrovně ve vaší infrastruktuře aplikace?

Úlohy:

* Definujte hello počet virtuálních počítačů v každé aplikační vrstvě, které vyžadujete.
* Určete, zda je nutné tooadjust hello počet selhání nebo aktualizace toobe domén používá pro vaši aplikaci.
* Definovat skupiny dostupnosti hello vyžaduje vaše zásady vytváření názvů a co virtuální počítače jsou umístěny v nich. Virtuální počítač se může nacházet pouze v sadě dostupnosti jeden. 

## <a name="availability-sets"></a>Skupiny dostupnosti
V Azure můžete virtuální počítače (VM) umístit do logické seskupení tooa názvem skupiny dostupnosti. Když vytvoříte virtuální počítače v rámci skupiny dostupnosti, hello platformy Azure rozděluje hello umístění těchto virtuálních počítačů na hello základní infrastruktury. Mělo by být toohello události plánované údržby platformy Azure nebo základní hardware nebo selhání infrastruktury, hello pomocí sad dostupnosti zajistí, že aspoň jeden virtuální počítač zůstane spuštěný.

Jako osvědčený postup nesmí nacházet aplikace na jeden virtuální počítač. Skupinu dostupnosti, která obsahuje jeden virtuální počítač není získáte ochranu z plánovaná nebo neplánovaná události v rámci hello platformy Azure. Hello [smlouva Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) vyžaduje dva nebo více virtuálních počítačů v rámci dostupnost sady tooallow hello distribuci virtuálních počítačů mezi hello základní infrastruktury. Pokud používáte [Azure Premium Storage](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), platí Smlouva Azure SLA hello tooa jeden virtuální počítač.

Základní infrastruktura Hello v Azure je rozděleno v clusterech toomultiple hardwaru. Každý hardwaru clusteru může podporovat velikosti rozsah virtuálních počítačů. Skupina dostupnosti může být hostovaný pouze na jednom hardwaru clusteru v libovolném bodě v čase. Velikosti hello rozsah virtuálních počítačů, které může existovat v sadě dostupnosti jeden tedy omezené toohello rozsah virtuálních počítačů velikosti podporované hello hardwaru clusteru. Hello hardwaru clusteru pro sadu dostupnosti hello je vybraná při hello první virtuální počítač v sadě dostupnosti hello je nasazen nebo při spuštění hello první virtuální počítač do nějaké skupiny dostupnosti, kde se všechny virtuální počítače jsou aktuálně ve stavu Zastaveno navrácena hello. Hello následující rozhraní příkazového řádku příkaz mohou mít k dispozici pro skupinu dostupnosti velikost používané toodetermine hello rozsah virtuálních počítačů: "velikosti-az virtuálních počítačů seznamu – umístění \<řetězec\>"

Každý cluster hardwaru je rozděleno v toomultiple aktualizace domén a domén selhání. Tyto domény jsou definovány, jaké hostitelé sdílení běžné cyklu aktualizace nebo podobné fyzické infrastruktuře sdílené složky například energie a sítě. Azure automaticky distribuuje virtuální počítače v rámci dostupnosti v rámci domény toomaintain dostupnost a odolnost proti chybám nastavit. V závislosti na velikosti hello aplikace a hello počet virtuálních počítačů v rámci skupiny dostupnosti, můžete upravit počet hello domén chcete toouse. Další informace o [Správa dostupnosti a použití aktualizací a chyby domén](manage-availability.md).

Při navrhování infrastruktury aplikace, naplánování toouse vrstev aplikace hello. Skupiny virtuálních počítačů, které slouží hello stejný účel tooavailability sad, jako je například sadu dostupnosti pro vaše klientské virtuální počítače se systémem nginx nebo Apache. Vytvořte samostatnou sadu dostupnosti pro virtuální počítače back-end s MongoDB nebo MySQL. cílem Hello je tooensure, že jednotlivé komponenty aplikace je chráněn skupinu dostupnosti a alespoň jednou se instance vždy zůstane spuštěný.

Nástroje pro vyrovnávání zatížení před každou toowork vrstvy aplikace společně se skupina dostupnosti, můžete použít a ujistěte se, že provoz může být vždy směrované tooa spuštěna instance. Bez nástroj pro vyrovnávání zatížení virtuálních počítačů může dál běžet v rámci události plánovaných a neplánovaných údržby, ale koncový uživatel nemusí být možné tooresolve je pokud hello primárního virtuálního počítače není k dispozici.

Návrh aplikace pro zajištění vysoké dostupnosti ve vrstvě úložiště. Hello osvědčeným postupem je příliš[používat spravované disky pro virtuální počítače ve skupině dostupnosti](manage-availability.md#use-managed-disks-for-vms-in-an-availability-set). Pokud aktuálně používáte nespravované disky, důrazně doporučujeme příliš[převést virtuální počítače ve skupině dostupnosti disky spravované toouse](convert-unmanaged-to-managed-disks.md#convert-vms-in-an-availability-set).

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

