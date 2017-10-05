---
title: "Sady dostupnosti pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro nasazení sad dostupnosti ve službách infrastruktury Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f9449f58-664b-4d5d-82f6-84c5d083047f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f94004fbd32dcc8a2394fcdfe2ac61519364f176
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-availability-sets-guidelines-for-windows-vms"></a>Azure dostupnosti nastavuje pravidla pro virtuální počítače Windows

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na pochopení požadované kroky plánování pro skupiny dostupnosti, aby vaše aplikace zůstanou přístupné během plánované i neplánované události.

## <a name="implementation-guidelines-for-availability-sets"></a>Postup implementace pro skupiny dostupnosti
Rozhodnutí:

* Kolik skupiny dostupnosti je nutný pro různé role a úrovně ve vaší infrastruktuře aplikace?

Úlohy:

* Zadejte počet virtuálních počítačů v jednotlivých vrstvách aplikace, kterou požadujete.
* Určete, pokud budete muset upravit počet selhání nebo aktualizovat domény, který se má použít pro vaši aplikaci.
* Definovat požadované dostupnosti sad, pomocí vaší zásady vytváření názvů a co virtuální počítače jsou umístěny v nich. Virtuální počítač se může nacházet pouze v sadě dostupnosti jeden.

## <a name="availability-sets"></a>Skupiny dostupnosti
V Azure můžete virtuální počítače (VM) umístit do logické seskupení názvem skupiny dostupnosti. Když vytvoříte virtuální počítače v rámci skupiny dostupnosti, rozděluje platformy Azure umístění těchto virtuálních počítačů na odpovídající infrastruktury. Mělo by být události plánované údržby na platformu Azure nebo základní hardware nebo selhání infrastruktury, použití skupiny dostupnosti zajistí, že aspoň jeden virtuální počítač zůstane spuštěný.

Jako osvědčený postup nesmí nacházet aplikace na jeden virtuální počítač. Skupinu dostupnosti, která obsahuje jeden virtuální počítač není získáte ochranu z plánovaná nebo neplánovaná události v rámci platformy Azure. [Smlouva Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) vyžaduje dva nebo více virtuálních počítačů v rámci dostupnosti nastavit, aby umožňovala na distribuci mezi základní infrastrukturu virtuálních počítačů. Pokud používáte [Azure Premium Storage](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), platí Smlouva Azure SLA pro jeden virtuální počítač.

Základní infrastrukturu v Azure je v rozdělen do více clusterů hardwaru. Každý hardwaru clusteru může podporovat velikosti rozsah virtuálních počítačů. Skupina dostupnosti může být hostovaný pouze na jednom hardwaru clusteru v libovolném bodě v čase. Proto je omezený na velikosti rozsah virtuálního počítače nepodporuje hardwaru clusteru velikosti rozsah virtuálních počítačů, které může existovat v sadě dostupnosti jeden. Hardwaru clusteru pro skupiny dostupnosti je vybraná při nasazení první virtuální počítač v sadě dostupnosti nebo při spuštění první virtuální počítač do nějaké skupiny dostupnosti, kde se všechny virtuální počítače jsou aktuálně ve stavu Zastaveno navrácena. Následující příkaz prostředí PowerShell slouží k určení velikosti rozsahu z virtuálních počítačů, která je k dispozici pro skupinu dostupnosti: "Get-AzureRmVMSize - ResourceGroupName \<řetězec\> - AvailabilitySetName \<řetězec\>"

Každý cluster hardwaru je rozděleno v několika aktualizace domén a domén selhání. Tyto domény jsou definovány, jaké hostitelé sdílení běžné cyklu aktualizace nebo podobné fyzické infrastruktuře sdílené složky například energie a sítě. Azure automaticky distribuuje virtuální počítače v rámci dostupnosti nastavit napříč doménami zachování dostupnosti a odolnosti proti chybám. V závislosti na velikosti vaší aplikace a počet virtuálních počítačů v rámci skupiny dostupnosti můžete upravit počet domén, které chcete použít. Další informace o [Správa dostupnosti a použití aktualizací a chyby domén](manage-availability.md).

Při navrhování infrastruktury aplikace, naplánujte vrstvy aplikace, které používáte. Nastaví skupiny virtuálních počítačů, které slouží ke stejnému účelu v na dostupnost, například sadu dostupnosti pro virtuální počítače front-endové služby IIS. Vytvořte samostatnou sadu dostupnosti pro virtuální počítače back-end systémem SQL Server. Cílem je zajistit, že jednotlivé komponenty aplikace je chráněn skupinu dostupnosti a alespoň jednou se instance vždy zůstane spuštěný.

Nástroje pro vyrovnávání zatížení, můžete použít před každou aplikační vrstvě fungovat společně se skupina dostupnosti a ujistěte se, že provoz může být vždy směrovány do spuštěné instance. Bez nástroj pro vyrovnávání zatížení virtuálních počítačů může dál běžet v rámci události plánovaných a neplánovaných údržby, ale nemusí být možné jejich řešení, pokud je primární virtuální počítač není k dispozici koncovým uživatelům.

Návrh aplikace pro zajištění vysoké dostupnosti ve vrstvě úložiště. Osvědčeným postupem je [používat spravované disky pro virtuální počítače ve skupině dostupnosti](manage-availability.md#use-managed-disks-for-vms-in-an-availability-set). Pokud aktuálně používáte nespravované disky, důrazně doporučujeme vám [převést virtuální počítače ve skupině dostupnosti použít spravované disky](convert-unmanaged-to-managed-disks.md#convert-vms-in-an-availability-set).

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]
