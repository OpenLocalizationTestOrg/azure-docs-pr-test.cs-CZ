---
title: "Nastavit možnosti přidávání virtuálních počítačích Azure do existující dostupnosti | Microsoft Docs"
description: "Možnosti podpory přidávání virtuálních počítačích Azure do stávající sadu dostupnosti."
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
ms.openlocfilehash: 3ce9b8a79108cb9e57df14bcb3354cc637193233
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a><span data-ttu-id="85b4b-103">Možnosti podpory přidávání virtuálních počítačích Azure do stávající sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="85b4b-103">Supportability of adding Azure VMs to an existing availability set</span></span>

<span data-ttu-id="85b4b-104">Někdy můžete setkat s omezení při přidání nových virtuálních počítačů (VM) do stávající sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="85b4b-104">You may occasionally encounter limitations when you add new virtual machines (VMs) to an existing availability set.</span></span> <span data-ttu-id="85b4b-105">Následující graf zobrazuje podrobnosti o které řady virtuálních počítačů je možné kombinovat ve stejné sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="85b4b-105">The following chart details which VM series you can mix in the same availability set.</span></span>

<span data-ttu-id="85b4b-106">Tady je podpoře matice do kombinovat různé typy virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="85b4b-106">Here is the supportability matrix to mix different types of VMs:</span></span>

<span data-ttu-id="85b4b-107">Ná & sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="85b4b-107">Series & Availability Set</span></span>|<span data-ttu-id="85b4b-108">Druhý virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="85b4b-108">Second VM</span></span>|<span data-ttu-id="85b4b-109">A</span><span class="sxs-lookup"><span data-stu-id="85b4b-109">A</span></span>|<span data-ttu-id="85b4b-110">Av2</span><span class="sxs-lookup"><span data-stu-id="85b4b-110">Av2</span></span>|<span data-ttu-id="85b4b-111">D</span><span class="sxs-lookup"><span data-stu-id="85b4b-111">D</span></span>|<span data-ttu-id="85b4b-112">Dv2</span><span class="sxs-lookup"><span data-stu-id="85b4b-112">Dv2</span></span>|<span data-ttu-id="85b4b-113">Dv3</span><span class="sxs-lookup"><span data-stu-id="85b4b-113">Dv3</span></span>|
|---|---|---|---|---|---|---|
|<span data-ttu-id="85b4b-114">První virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="85b4b-114">First VM</span></span>|||||||
|<span data-ttu-id="85b4b-115">A</span><span class="sxs-lookup"><span data-stu-id="85b4b-115">A</span></span>||<span data-ttu-id="85b4b-116">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-116">OK</span></span>|<span data-ttu-id="85b4b-117">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-117">OK</span></span>|<span data-ttu-id="85b4b-118">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-118">OK</span></span>|<span data-ttu-id="85b4b-119">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-119">OK</span></span>|<span data-ttu-id="85b4b-120">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-120">OK</span></span>|
|<span data-ttu-id="85b4b-121">Av2</span><span class="sxs-lookup"><span data-stu-id="85b4b-121">Av2</span></span>||<span data-ttu-id="85b4b-122">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-122">OK</span></span>|<span data-ttu-id="85b4b-123">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-123">OK</span></span>|<span data-ttu-id="85b4b-124">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-124">OK</span></span>|<span data-ttu-id="85b4b-125">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-125">OK</span></span>|<span data-ttu-id="85b4b-126">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-126">OK</span></span>|
|<span data-ttu-id="85b4b-127">D</span><span class="sxs-lookup"><span data-stu-id="85b4b-127">D</span></span>||<span data-ttu-id="85b4b-128">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-128">OK</span></span>|<span data-ttu-id="85b4b-129">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-129">OK</span></span>|<span data-ttu-id="85b4b-130">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-130">OK</span></span>|<span data-ttu-id="85b4b-131">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-131">OK</span></span>|<span data-ttu-id="85b4b-132">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-132">OK</span></span>|
|<span data-ttu-id="85b4b-133">Dv2</span><span class="sxs-lookup"><span data-stu-id="85b4b-133">Dv2</span></span>||<span data-ttu-id="85b4b-134">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-134">OK</span></span>|<span data-ttu-id="85b4b-135">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-135">OK</span></span>|<span data-ttu-id="85b4b-136">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-136">OK</span></span>|<span data-ttu-id="85b4b-137">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-137">OK</span></span>|<span data-ttu-id="85b4b-138">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-138">OK</span></span>|
|<span data-ttu-id="85b4b-139">Dv3</span><span class="sxs-lookup"><span data-stu-id="85b4b-139">Dv3</span></span>||<span data-ttu-id="85b4b-140">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-140">OK</span></span>|<span data-ttu-id="85b4b-141">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-141">OK</span></span>|<span data-ttu-id="85b4b-142">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-142">OK</span></span>|<span data-ttu-id="85b4b-143">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-143">OK</span></span>|<span data-ttu-id="85b4b-144">OK</span><span class="sxs-lookup"><span data-stu-id="85b4b-144">OK</span></span>|

<span data-ttu-id="85b4b-145">Všechny ostatní řady nemůže být ve stejné dostupnosti nastavit, protože vyžadují konkrétní hardware.</span><span class="sxs-lookup"><span data-stu-id="85b4b-145">All other series could not be in the same availability set because they require a specific hardware.</span></span>