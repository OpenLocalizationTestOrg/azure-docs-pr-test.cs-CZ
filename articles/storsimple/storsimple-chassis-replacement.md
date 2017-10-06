---
title: "Skříň aaaReplace na zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak tooremove a nahradit text hello skříň pro StorSimple primární skříň nebo EBOD skříň."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 537659ed-4c46-49c1-b1e4-186262f2542d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: f8576d63520a6f7d3267180d2a68d4fc38fd48fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-chassis-on-your-storsimple-device"></a><span data-ttu-id="b645d-103">Nahraďte hello skříň zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="b645d-103">Replace hello chassis on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="b645d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b645d-104">Overview</span></span>
<span data-ttu-id="b645d-105">Tento kurz vysvětluje, jak tooremove a nahraďte skříň v zařízení řady StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="b645d-105">This tutorial explains how tooremove and replace a chassis in a StorSimple 8000 series device.</span></span> <span data-ttu-id="b645d-106">Hello StorSimple 8100 modelu je zařízení jeden skříň (jednom šasi), zatímco hello 8600 je zařízení duální skříň (dvě chassis).</span><span class="sxs-lookup"><span data-stu-id="b645d-106">hello StorSimple 8100 model is a single enclosure device (one chassis), whereas hello 8600 is a dual enclosure device (two chassis).</span></span> <span data-ttu-id="b645d-107">Pro 8600 model jsou potenciálně dvě skříň, které by mohlo selhat v zařízení hello: hello skříň pro primární skříň hello nebo hello skříň pro hello EBOD skříň.</span><span class="sxs-lookup"><span data-stu-id="b645d-107">For an 8600 model, there are potentially two chassis that could fail in hello device: hello chassis for hello primary enclosure or hello chassis for hello EBOD enclosure.</span></span>

<span data-ttu-id="b645d-108">V obou případech skříň hello nahrazení, který je dodáván společností Microsoft je prázdný.</span><span class="sxs-lookup"><span data-stu-id="b645d-108">In either case, hello replacement chassis that is shipped by Microsoft is empty.</span></span> <span data-ttu-id="b645d-109">Žádné napájení a chlazení moduly (PCMs), moduly řadič diskové jednotky SSD (Solid-State Drive), pevných disků (HDD) nebo EBOD moduly budou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="b645d-109">No Power and Cooling Modules (PCMs), controller modules, solid state disk drives (SSDs), hard disk drives (HDDs), or EBOD modules will be included.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b645d-110">Před odebírání a nahrazování hello skříň, zkontrolujte informace o zabezpečení hello v [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="b645d-110">Before removing and replacing hello chassis, review hello safety information in [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="remove-hello-chassis"></a><span data-ttu-id="b645d-111">Odebrat hello skříň</span><span class="sxs-lookup"><span data-stu-id="b645d-111">Remove hello chassis</span></span>
<span data-ttu-id="b645d-112">Proveďte následující kroky tooremove hello skříň zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="b645d-112">Perform hello following steps tooremove hello chassis on your StorSimple device.</span></span>

#### <a name="tooremove-a-chassis"></a><span data-ttu-id="b645d-113">tooremove skříň</span><span class="sxs-lookup"><span data-stu-id="b645d-113">tooremove a chassis</span></span>
1. <span data-ttu-id="b645d-114">Zajistěte, aby toto zařízení StorSimple hello je vypnout a odpojit od všech zdrojů energie hello.</span><span class="sxs-lookup"><span data-stu-id="b645d-114">Make sure that hello StorSimple device is shut down and disconnected from all hello power sources.</span></span>
2. <span data-ttu-id="b645d-115">Odeberte všechny síťové hello a SAS kabely, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b645d-115">Remove all hello network and SAS cables, if applicable.</span></span>
3. <span data-ttu-id="b645d-116">Odeberte hello jednotku ze hello rack.</span><span class="sxs-lookup"><span data-stu-id="b645d-116">Remove hello unit from hello rack.</span></span>
4. <span data-ttu-id="b645d-117">Odeberte všechny jednotky hello a poznamenejte si hello sloty, ze kterých budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="b645d-117">Remove each of hello drives and note hello slots from which they are removed.</span></span> <span data-ttu-id="b645d-118">Další informace najdete v tématu [odebrat hello disku](storsimple-disk-drive-replacement.md#remove-the-disk-drive).</span><span class="sxs-lookup"><span data-stu-id="b645d-118">For more information, see [Remove hello disk drive](storsimple-disk-drive-replacement.md#remove-the-disk-drive).</span></span>
5. <span data-ttu-id="b645d-119">Na hello EBOD skříň (Pokud se jedná hello skříň, do které se nezdařilo) odeberte hello EBOD řadiče moduly.</span><span class="sxs-lookup"><span data-stu-id="b645d-119">On hello EBOD enclosure (if this is hello chassis that failed), remove hello EBOD controller modules.</span></span> <span data-ttu-id="b645d-120">Další informace najdete v tématu [odebrat řadič EBOD](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller).</span><span class="sxs-lookup"><span data-stu-id="b645d-120">For more information, see [Remove an EBOD controller](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller).</span></span> 
   
    <span data-ttu-id="b645d-121">Na hello primární skříň (Pokud se jedná hello skříň, do které se nezdařilo), odeberte hello řadiče a poznamenejte si hello sloty, ze kterých budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="b645d-121">On hello primary enclosure (if this is hello chassis that failed), remove hello controllers and note hello slots from which they are removed.</span></span> <span data-ttu-id="b645d-122">Další informace najdete v tématu [odebrání řadiče](storsimple-controller-replacement.md#remove-a-controller).</span><span class="sxs-lookup"><span data-stu-id="b645d-122">For more information, see [Remove a controller](storsimple-controller-replacement.md#remove-a-controller).</span></span>

## <a name="install-hello-chassis"></a><span data-ttu-id="b645d-123">Nainstalujte hello skříň</span><span class="sxs-lookup"><span data-stu-id="b645d-123">Install hello chassis</span></span>
<span data-ttu-id="b645d-124">Proveďte následující kroky tooinstall hello skříň zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="b645d-124">Perform hello following steps tooinstall hello chassis on your StorSimple device.</span></span>

#### <a name="tooinstall-a-chassis"></a><span data-ttu-id="b645d-125">tooinstall skříň</span><span class="sxs-lookup"><span data-stu-id="b645d-125">tooinstall a chassis</span></span>
1. <span data-ttu-id="b645d-126">Připojte hello skříň v racku hello.</span><span class="sxs-lookup"><span data-stu-id="b645d-126">Mount hello chassis in hello rack.</span></span> <span data-ttu-id="b645d-127">Další informace najdete v tématu [Rack připojení vaší 8100 StorSimple zařízení](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) nebo [zařízení 8600 vaší StorSimple Rack připojení](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="b645d-127">For more information, see [Rack-mount your StorSimple 8100 device](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) or [Rack-mount your StorSimple 8600 device](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span></span>
2. <span data-ttu-id="b645d-128">Po hello skříň namontovat do racku hello, nainstalujte v hello stejné umisťuje, zda byly dříve nainstalovány v hello řadiče moduly.</span><span class="sxs-lookup"><span data-stu-id="b645d-128">After hello chassis is mounted in hello rack, install hello controller modules in hello same positions that they were previously installed in.</span></span>
3. <span data-ttu-id="b645d-129">Instalace hello jednotek v hello stejné umisťuje a přihrádek, zda byly dříve nainstalovány v.</span><span class="sxs-lookup"><span data-stu-id="b645d-129">Install hello drives in hello same positions and slots that they were previously installed in.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b645d-130">Doporučujeme nejprve nainstalovat hello SSD v hello sloty a pak nainstalujte HDD hello.</span><span class="sxs-lookup"><span data-stu-id="b645d-130">We recommend that you install hello SSDs in hello slots first, and then install hello HDDs.</span></span>
   > 
   > 
4. <span data-ttu-id="b645d-131">S hello zařízení namontováno v racku hello a hello komponenty nainstalované, připojit vaše zařízení toohello odpovídající power zdroje a zapnout hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="b645d-131">With hello device mounted in hello rack and hello components installed, connect your device toohello appropriate power sources, and turn on hello device.</span></span> <span data-ttu-id="b645d-132">Podrobnosti najdete v tématu [zapojení kabeláže zařízení StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) nebo [zapojení kabeláže zařízení StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="b645d-132">For details, see [Cable your StorSimple 8100 device](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) or [Cable your StorSimple 8600 device](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b645d-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b645d-133">Next steps</span></span>
<span data-ttu-id="b645d-134">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="b645d-134">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

