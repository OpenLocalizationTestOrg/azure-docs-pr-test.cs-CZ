---
title: "Virtuální počítač zachování údržby pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Místní migrace virtuálních počítačů pro zachování aktualizace paměti."
services: virtual-machines-windows
documentationcenter: 
author: 
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: 
ms.openlocfilehash: 09fc9021e8dfb910d1a81178434ca2e27c0bacf7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="vm-preserving-maintenance-in-place-vm-migration"></a><span data-ttu-id="eaba0-103">Virtuální počítač zachování údržby (místní počítač migrace)</span><span class="sxs-lookup"><span data-stu-id="eaba0-103">VM preserving maintenance (In-place VM migration)</span></span>

<span data-ttu-id="eaba0-104">Zatímco většina aktualizace nemají žádný vliv na hostované virtuální počítače, jsou případy, kdy aktualizace služeb nebo komponent způsobit minimální narušení k spuštěných virtuálních počítačů (bez úplné restartování virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="eaba0-104">While the majority of updates have no impact to hosted VMs, there are cases where updates to components or services result in minimal interference to running VMs (without a full reboot of the virtual machine).</span></span>

<span data-ttu-id="eaba0-105">Tyto aktualizace jsou provést pomocí technologie, která umožňuje migrace za provozu na místě, označované taky jako "zachování paměti update".</span><span class="sxs-lookup"><span data-stu-id="eaba0-105">These updates are accomplished with technology that enables in-place live migration, also called "memory-preserving update".</span></span> <span data-ttu-id="eaba0-106">Při aktualizaci hostitele, virtuální počítač je umístěn do stavu "pozastavení", zachování paměti v paměti RAM, zatímco hostitelské prostředí (například příslušný operační systém) se vztahuje potřebné aktualizace a opravy.</span><span class="sxs-lookup"><span data-stu-id="eaba0-106">When updating the host, the virtual machine is placed into a “paused” state, preserving the memory in RAM, while the hosting environment (e.g. the underlying operating system) applies the necessary updates and patches.</span></span>
<span data-ttu-id="eaba0-107">Virtuální počítač je potom pokračuje v rámci 30 sekund byla pozastavena.</span><span class="sxs-lookup"><span data-stu-id="eaba0-107">The virtual machine is then resumed within 30 seconds of being paused.</span></span>
<span data-ttu-id="eaba0-108">Po obnovení se hodiny virtuálního počítače automaticky synchronizují.</span><span class="sxs-lookup"><span data-stu-id="eaba0-108">After resuming, the clock of the virtual machine is automatically synchronized.</span></span>

<span data-ttu-id="eaba0-109">Ne všechny aktualizace je možné nasadit pomocí tohoto mechanismu, ale díky krátkému období pozastavení má tento způsob nasazení aktualizací výrazně nižší vliv na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="eaba0-109">Not all updates can be deployed by using this mechanism, but given the short pause period, deploying updates in this way greatly reduces impact to virtual machines.</span></span>

<span data-ttu-id="eaba0-110">Aktualizace s více instancemi (virtuálních počítačů v nastavení dostupnosti) jsou použité jednu aktualizační doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="eaba0-110">Multi-instance updates (VMs in an availability set) are applied one update domain at a time.</span></span>

<span data-ttu-id="eaba0-111">Některé aplikace může být ovlivněno tyto aktualizace více než jiné.</span><span class="sxs-lookup"><span data-stu-id="eaba0-111">Some applications may be impacted by these updates more than others.</span></span> <span data-ttu-id="eaba0-112">Aplikace, které provádějí zpracování událostí v reálném čase, streamování médií nebo překódování nebo vysokou propustnost sítě scénářů, například nemusí být navržena k tolerovat 30 sekundové pauzy.</span><span class="sxs-lookup"><span data-stu-id="eaba0-112">Applications that perform real-time event processing, media streaming or transcoding, or high throughput networking scenarios, for example, may not be designed to tolerate a 30 second pause.</span></span> <span data-ttu-id="eaba0-113">Aplikace běžící na virtuálním počítači můžete další informace o nadcházejících aktualizace voláním [naplánované události](../virtual-machines-scheduled-events.md) API [Metadata služby Azure](../virtual-machines-instancemetadataservice-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eaba0-113">Applications running in a virtual machine can learn about upcoming updates by calling the [Scheduled Events](../virtual-machines-scheduled-events.md) API of the [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).</span></span>