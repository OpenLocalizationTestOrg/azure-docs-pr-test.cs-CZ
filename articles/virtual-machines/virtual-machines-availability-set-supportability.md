---
title: "aaaSupportability přidávání stávající sadu dostupnosti virtuálních počítačů Azure tooan | Microsoft Docs"
description: "Možnosti podpory přidávání virtuálních počítačích Azure tooan stávající sadu dostupnosti."
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/15/2017
ms.author: delhan
ms.openlocfilehash: dc2bd86b916f1d1a0a0d4c9e870df829434c96b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="supportability-of-adding-azure-vms-tooan-existing-availability-set"></a><span data-ttu-id="7ac79-103">Možnosti podpory přidávání virtuálních počítačích Azure tooan stávající sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="7ac79-103">Supportability of adding Azure VMs tooan existing availability set</span></span>

<span data-ttu-id="7ac79-104">Někdy můžete setkat s omezení při přidání nové virtuální počítače (VM) tooan stávající sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="7ac79-104">You may occasionally encounter limitations when you add new virtual machines (VMs) tooan existing availability set.</span></span> <span data-ttu-id="7ac79-105">Následující Hello grafu podrobnosti hello které řady virtuálních počítačů je možné kombinovat ve stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="7ac79-105">hello following chart details which VM series you can mix in hello same availability set.</span></span>

<span data-ttu-id="7ac79-106">Tady je hello sahat matice toomix různé typy virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="7ac79-106">Here is hello supportability matrix toomix different types of VMs:</span></span>

<span data-ttu-id="7ac79-107">Ná & sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="7ac79-107">Series & Availability Set</span></span>|<span data-ttu-id="7ac79-108">Druhý virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="7ac79-108">Second VM</span></span>|<span data-ttu-id="7ac79-109">A</span><span class="sxs-lookup"><span data-stu-id="7ac79-109">A</span></span>|<span data-ttu-id="7ac79-110">Av2</span><span class="sxs-lookup"><span data-stu-id="7ac79-110">Av2</span></span>|<span data-ttu-id="7ac79-111">D</span><span class="sxs-lookup"><span data-stu-id="7ac79-111">D</span></span>|<span data-ttu-id="7ac79-112">Dv2</span><span class="sxs-lookup"><span data-stu-id="7ac79-112">Dv2</span></span>|<span data-ttu-id="7ac79-113">Dv3</span><span class="sxs-lookup"><span data-stu-id="7ac79-113">Dv3</span></span>|
|---|---|---|---|---|---|---|
|<span data-ttu-id="7ac79-114">První virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="7ac79-114">First VM</span></span>|||||||
|<span data-ttu-id="7ac79-115">A</span><span class="sxs-lookup"><span data-stu-id="7ac79-115">A</span></span>||<span data-ttu-id="7ac79-116">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-116">OK</span></span>|<span data-ttu-id="7ac79-117">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-117">OK</span></span>|<span data-ttu-id="7ac79-118">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-118">OK</span></span>|<span data-ttu-id="7ac79-119">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-119">OK</span></span>|<span data-ttu-id="7ac79-120">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-120">OK</span></span>|
|<span data-ttu-id="7ac79-121">Av2</span><span class="sxs-lookup"><span data-stu-id="7ac79-121">Av2</span></span>||<span data-ttu-id="7ac79-122">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-122">OK</span></span>|<span data-ttu-id="7ac79-123">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-123">OK</span></span>|<span data-ttu-id="7ac79-124">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-124">OK</span></span>|<span data-ttu-id="7ac79-125">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-125">OK</span></span>|<span data-ttu-id="7ac79-126">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-126">OK</span></span>|
|<span data-ttu-id="7ac79-127">D</span><span class="sxs-lookup"><span data-stu-id="7ac79-127">D</span></span>||<span data-ttu-id="7ac79-128">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-128">OK</span></span>|<span data-ttu-id="7ac79-129">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-129">OK</span></span>|<span data-ttu-id="7ac79-130">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-130">OK</span></span>|<span data-ttu-id="7ac79-131">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-131">OK</span></span>|<span data-ttu-id="7ac79-132">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-132">OK</span></span>|
|<span data-ttu-id="7ac79-133">Dv2</span><span class="sxs-lookup"><span data-stu-id="7ac79-133">Dv2</span></span>||<span data-ttu-id="7ac79-134">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-134">OK</span></span>|<span data-ttu-id="7ac79-135">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-135">OK</span></span>|<span data-ttu-id="7ac79-136">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-136">OK</span></span>|<span data-ttu-id="7ac79-137">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-137">OK</span></span>|<span data-ttu-id="7ac79-138">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-138">OK</span></span>|
|<span data-ttu-id="7ac79-139">Dv3</span><span class="sxs-lookup"><span data-stu-id="7ac79-139">Dv3</span></span>||<span data-ttu-id="7ac79-140">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-140">OK</span></span>|<span data-ttu-id="7ac79-141">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-141">OK</span></span>|<span data-ttu-id="7ac79-142">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-142">OK</span></span>|<span data-ttu-id="7ac79-143">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-143">OK</span></span>|<span data-ttu-id="7ac79-144">OK</span><span class="sxs-lookup"><span data-stu-id="7ac79-144">OK</span></span>|

<span data-ttu-id="7ac79-145">Všechny ostatní řady nelze v hello stejné dostupnosti nastavit, protože vyžadují konkrétní hardware.</span><span class="sxs-lookup"><span data-stu-id="7ac79-145">All other series could not be in hello same availability set because they require a specific hardware.</span></span>
