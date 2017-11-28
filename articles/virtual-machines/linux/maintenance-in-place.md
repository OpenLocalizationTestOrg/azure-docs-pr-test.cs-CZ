---
title: "aaaVM zachování údržby pro virtuální počítače Windows v Azure | Microsoft Docs"
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
ms.openlocfilehash: 544a2dcca52bb3ac51d341bceaf4ba3e7c71fd82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vm-preserving-maintenance-in-place-vm-migration"></a><span data-ttu-id="f00c3-103">Virtuální počítač zachování údržby (místní počítač migrace)</span><span class="sxs-lookup"><span data-stu-id="f00c3-103">VM preserving maintenance (In-place VM migration)</span></span>

<span data-ttu-id="f00c3-104">Většina hello aktualizací používat žádné dopad toohosted virtuálních počítačů, jsou případy, kdy toocomponents aktualizace nebo služby výsledkem minimální narušení toorunning virtuálních počítačů (bez úplné restartování hello virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="f00c3-104">While hello majority of updates have no impact toohosted VMs, there are cases where updates toocomponents or services result in minimal interference toorunning VMs (without a full reboot of hello virtual machine).</span></span>

<span data-ttu-id="f00c3-105">Tyto aktualizace jsou provést pomocí technologie, která umožňuje migrace za provozu na místě, označované taky jako "zachování paměti update".</span><span class="sxs-lookup"><span data-stu-id="f00c3-105">These updates are accomplished with technology that enables in-place live migration, also called "memory-preserving update".</span></span> <span data-ttu-id="f00c3-106">Při aktualizaci hello hostitele, hello je umístěn virtuální počítač do stavu "pozastavení", zachování hello paměti RAM, během hello hostitelské prostředí (například příslušný operační systém) hello potřebné aktualizace a opravy.</span><span class="sxs-lookup"><span data-stu-id="f00c3-106">When updating hello host, hello virtual machine is placed into a “paused” state, preserving hello memory in RAM, while hello hosting environment (e.g. the underlying operating system) applies hello necessary updates and patches.</span></span>
<span data-ttu-id="f00c3-107">Hello virtuálního počítače je pak pokračuje v rámci 30 sekund byla pozastavena.</span><span class="sxs-lookup"><span data-stu-id="f00c3-107">hello virtual machine is then resumed within 30 seconds of being paused.</span></span>
<span data-ttu-id="f00c3-108">Po obnovení, je automaticky synchronizovat hodiny hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f00c3-108">After resuming, hello clock of hello virtual machine is automatically synchronized.</span></span>

<span data-ttu-id="f00c3-109">Ne všechny aktualizace můžete nasadit pomocí tento mechanismus, ale dané období krátké pozastavení nasazení aktualizací v tomto způsobem výrazně snižuje dopad toovirtual počítače.</span><span class="sxs-lookup"><span data-stu-id="f00c3-109">Not all updates can be deployed by using this mechanism, but given the short pause period, deploying updates in this way greatly reduces impact toovirtual machines.</span></span>

<span data-ttu-id="f00c3-110">Aktualizace s více instancemi (virtuálních počítačů v nastavení dostupnosti) jsou použité jednu aktualizační doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="f00c3-110">Multi-instance updates (VMs in an availability set) are applied one update domain at a time.</span></span>

<span data-ttu-id="f00c3-111">Některé aplikace může být ovlivněno tyto aktualizace více než jiné.</span><span class="sxs-lookup"><span data-stu-id="f00c3-111">Some applications may be impacted by these updates more than others.</span></span> <span data-ttu-id="f00c3-112">Aplikace, které provádějí zpracování událostí v reálném čase, streamování médií nebo překódování nebo vysokou propustnost sítě scénáře, nemusí být například navrženou tootolerate 30 sekundové pauzy.</span><span class="sxs-lookup"><span data-stu-id="f00c3-112">Applications that perform real-time event processing, media streaming or transcoding, or high throughput networking scenarios, for example, may not be designed tootolerate a 30 second pause.</span></span> <span data-ttu-id="f00c3-113">Aplikace běžící na virtuálním počítači můžete další informace o nadcházejících aktualizace pomocí volání hello [naplánované události](../virtual-machines-scheduled-events.md) API hello [Azure Metadata služby](../virtual-machines-instancemetadataservice-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f00c3-113">Applications running in a virtual machine can learn about upcoming updates by calling hello [Scheduled Events](../virtual-machines-scheduled-events.md) API of hello [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).</span></span>
