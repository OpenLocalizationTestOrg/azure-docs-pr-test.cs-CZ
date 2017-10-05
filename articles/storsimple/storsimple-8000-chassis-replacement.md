---
title: "Nahraďte skříň na zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak odeberete a nahradíte skříň pro StorSimple primární skříň nebo EBOD skříň."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 073fcf0064f1d1482f4683d733f00cf918ff2f38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a><span data-ttu-id="23eb3-103">Nahraďte skříň zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="23eb3-103">Replace the chassis on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="23eb3-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="23eb3-104">Overview</span></span>
<span data-ttu-id="23eb3-105">Tento kurz vysvětluje, jak odeberete a nahradíte skříň v zařízení řady StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="23eb3-105">This tutorial explains how to remove and replace a chassis in a StorSimple 8000 series device.</span></span> <span data-ttu-id="23eb3-106">Model StorSimple 8100 je zařízení jeden skříň (jednom šasi), zatímco 8600 je zařízení duální skříň (dvě chassis).</span><span class="sxs-lookup"><span data-stu-id="23eb3-106">The StorSimple 8100 model is a single enclosure device (one chassis), whereas the 8600 is a dual enclosure device (two chassis).</span></span> <span data-ttu-id="23eb3-107">Pro 8600 model jsou potenciálně dvě skříň, které by mohlo selhat v zařízení: Skříň pro primární skříň nebo skříň pro EBOD skříň.</span><span class="sxs-lookup"><span data-stu-id="23eb3-107">For an 8600 model, there are potentially two chassis that could fail in the device: the chassis for the primary enclosure or the chassis for the EBOD enclosure.</span></span>

<span data-ttu-id="23eb3-108">V obou případech skříň nahrazení, který je dodáván společností Microsoft je prázdný.</span><span class="sxs-lookup"><span data-stu-id="23eb3-108">In either case, the replacement chassis that is shipped by Microsoft is empty.</span></span> <span data-ttu-id="23eb3-109">Žádné napájení a chlazení moduly (PCMs), moduly řadič diskové jednotky SSD (Solid-State Drive), pevných disků (HDD) nebo EBOD moduly budou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="23eb3-109">No Power and Cooling Modules (PCMs), controller modules, solid state disk drives (SSDs), hard disk drives (HDDs), or EBOD modules will be included.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23eb3-110">Před odebráním a nahraďte skříň, informace zabezpečení v [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="23eb3-110">Before removing and replacing the chassis, review the safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="remove-the-chassis"></a><span data-ttu-id="23eb3-111">Odebrat skříň.</span><span class="sxs-lookup"><span data-stu-id="23eb3-111">Remove the chassis</span></span>
<span data-ttu-id="23eb3-112">Proveďte následující kroky k odebrání skříň zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="23eb3-112">Perform the following steps to remove the chassis on your StorSimple device.</span></span>

#### <a name="to-remove-a-chassis"></a><span data-ttu-id="23eb3-113">Chcete-li odebrat skříň</span><span class="sxs-lookup"><span data-stu-id="23eb3-113">To remove a chassis</span></span>
1. <span data-ttu-id="23eb3-114">Ujistěte se, že je zařízení StorSimple vypnout a odpojit od všech zdrojů energie.</span><span class="sxs-lookup"><span data-stu-id="23eb3-114">Make sure that the StorSimple device is shut down and disconnected from all the power sources.</span></span>
2. <span data-ttu-id="23eb3-115">Odeberte všechny sítě a SAS kabely, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="23eb3-115">Remove all the network and SAS cables, if applicable.</span></span>
3. <span data-ttu-id="23eb3-116">Odeberte jednotku z racku.</span><span class="sxs-lookup"><span data-stu-id="23eb3-116">Remove the unit from the rack.</span></span>
4. <span data-ttu-id="23eb3-117">Odeberte všechny jednotky a poznamenejte si sloty, ze kterých budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="23eb3-117">Remove each of the drives and note the slots from which they are removed.</span></span> <span data-ttu-id="23eb3-118">Další informace najdete v tématu [odebrat z disku](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).</span><span class="sxs-lookup"><span data-stu-id="23eb3-118">For more information, see [Remove the disk drive](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).</span></span>
5. <span data-ttu-id="23eb3-119">Ve skříni EBOD (Pokud se jedná skříň, která se nezdařilo) odebrat moduly EBOD řadiče.</span><span class="sxs-lookup"><span data-stu-id="23eb3-119">On the EBOD enclosure (if this is the chassis that failed), remove the EBOD controller modules.</span></span> <span data-ttu-id="23eb3-120">Další informace najdete v tématu [odebrat řadič EBOD](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).</span><span class="sxs-lookup"><span data-stu-id="23eb3-120">For more information, see [Remove an EBOD controller](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).</span></span>
   
    <span data-ttu-id="23eb3-121">Na primární skříň (Pokud se jedná skříň, která se nezdařilo) odeberte řadiče a poznamenejte si sloty, ze kterých budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="23eb3-121">On the primary enclosure (if this is the chassis that failed), remove the controllers and note the slots from which they are removed.</span></span> <span data-ttu-id="23eb3-122">Další informace najdete v tématu [odebrání řadiče](storsimple-8000-controller-replacement.md#remove-a-controller).</span><span class="sxs-lookup"><span data-stu-id="23eb3-122">For more information, see [Remove a controller](storsimple-8000-controller-replacement.md#remove-a-controller).</span></span>

## <a name="install-the-chassis"></a><span data-ttu-id="23eb3-123">Nainstalujte skříň.</span><span class="sxs-lookup"><span data-stu-id="23eb3-123">Install the chassis</span></span>
<span data-ttu-id="23eb3-124">Proveďte následující kroky instalace skříň na zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="23eb3-124">Perform the following steps to install the chassis on your StorSimple device.</span></span>

#### <a name="to-install-a-chassis"></a><span data-ttu-id="23eb3-125">Chcete-li nainstalovat skříň</span><span class="sxs-lookup"><span data-stu-id="23eb3-125">To install a chassis</span></span>
1. <span data-ttu-id="23eb3-126">Připojte skříň v racku.</span><span class="sxs-lookup"><span data-stu-id="23eb3-126">Mount the chassis in the rack.</span></span> <span data-ttu-id="23eb3-127">Další informace najdete v tématu [Rack připojení vaší 8100 StorSimple zařízení](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) nebo [zařízení 8600 vaší StorSimple Rack připojení](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="23eb3-127">For more information, see [Rack-mount your StorSimple 8100 device](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) or [Rack-mount your StorSimple 8600 device](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span></span>
2. <span data-ttu-id="23eb3-128">Po skříň připojena v racku, nainstalujte na stejném místě, které byly dříve nainstalovány v moduly řadiče.</span><span class="sxs-lookup"><span data-stu-id="23eb3-128">After the chassis is mounted in the rack, install the controller modules in the same positions that they were previously installed in.</span></span>
3. <span data-ttu-id="23eb3-129">Nainstalujte na discích ve stejné pozic a sloty, které byly dříve nainstalovány v.</span><span class="sxs-lookup"><span data-stu-id="23eb3-129">Install the drives in the same positions and slots that they were previously installed in.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="23eb3-130">Doporučujeme nejprve nainstalovat SSD v přihrádkách a pak nainstalujte pevné disky.</span><span class="sxs-lookup"><span data-stu-id="23eb3-130">We recommend that you install the SSDs in the slots first, and then install the HDDs.</span></span>
  
4. <span data-ttu-id="23eb3-131">U zařízení připojené v racku a komponenty nainstalované připojení zařízení k příslušné power zdroje a zapněte zařízení.</span><span class="sxs-lookup"><span data-stu-id="23eb3-131">With the device mounted in the rack and the components installed, connect your device to the appropriate power sources, and turn on the device.</span></span> <span data-ttu-id="23eb3-132">Podrobnosti najdete v tématu [zapojení kabeláže zařízení StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) nebo [zapojení kabeláže zařízení StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="23eb3-132">For details, see [Cable your StorSimple 8100 device](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) or [Cable your StorSimple 8600 device](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span></span>

## <a name="next-steps"></a><span data-ttu-id="23eb3-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23eb3-133">Next steps</span></span>
<span data-ttu-id="23eb3-134">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="23eb3-134">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

