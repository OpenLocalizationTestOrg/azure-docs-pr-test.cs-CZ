---
title: "Postup škálování Azure časové řady Přehled prostředí | Microsoft Docs"
description: "Tento kurz se zaměřuje na postup škálování Azure časové řady Přehled prostředí"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: 8f6c66ea2173c98179ec899d6626c2ab6f7ec4b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-scale-your-time-series-insights-environment"></a><span data-ttu-id="fccf7-103">Postup škálování prostředí Statistika časové řady</span><span class="sxs-lookup"><span data-stu-id="fccf7-103">How to scale your Time Series Insights environment</span></span>

<span data-ttu-id="fccf7-104">Tento kurz se zaměřuje na postup škálování prostředí Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="fccf7-104">This tutorial covers how to scale your Time Series Insights environment.</span></span>

> [!NOTE]
> <span data-ttu-id="fccf7-105">Rozšiřování škálování využívajících typů sku není povoleno.</span><span class="sxs-lookup"><span data-stu-id="fccf7-105">Scale up across sku types is not allowed.</span></span> <span data-ttu-id="fccf7-106">Prostředí s S1 Sku nelze převést do prostředí S2.</span><span class="sxs-lookup"><span data-stu-id="fccf7-106">An environment with a S1 Sku cannot be converted into an S2 environment.</span></span>

## <a name="s1-sku-ingress-rates-and-capacities"></a><span data-ttu-id="fccf7-107">Příjem příchozích dat sazby S1 SKU a kapacity</span><span class="sxs-lookup"><span data-stu-id="fccf7-107">S1 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="fccf7-108">S1 Kapacita SKU</span><span class="sxs-lookup"><span data-stu-id="fccf7-108">S1 SKU Capacity</span></span> | <span data-ttu-id="fccf7-109">Příchozí míra</span><span class="sxs-lookup"><span data-stu-id="fccf7-109">Ingress Rate</span></span> | <span data-ttu-id="fccf7-110">Maximální kapacita úložiště</span><span class="sxs-lookup"><span data-stu-id="fccf7-110">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="fccf7-111">1</span><span class="sxs-lookup"><span data-stu-id="fccf7-111">1</span></span> | <span data-ttu-id="fccf7-112">1 GB (1 milion událostí)</span><span class="sxs-lookup"><span data-stu-id="fccf7-112">1 GB (1 million events)</span></span> | <span data-ttu-id="fccf7-113">(30 milionů událostí) 30 GB za měsíc</span><span class="sxs-lookup"><span data-stu-id="fccf7-113">30 GB (30 million events) per month</span></span> |
| <span data-ttu-id="fccf7-114">10</span><span class="sxs-lookup"><span data-stu-id="fccf7-114">10</span></span> | <span data-ttu-id="fccf7-115">10 GB (10 milionů událostí)</span><span class="sxs-lookup"><span data-stu-id="fccf7-115">10 GB (10 million events)</span></span> | <span data-ttu-id="fccf7-116">300 GB za měsíc (300 milión událostí)</span><span class="sxs-lookup"><span data-stu-id="fccf7-116">300 GB (300 million events) per month</span></span> |

## <a name="s2-sku-ingress-rates-and-capacities"></a><span data-ttu-id="fccf7-117">S2 SKU příjem příchozích dat rychlostí a kapacity</span><span class="sxs-lookup"><span data-stu-id="fccf7-117">S2 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="fccf7-118">S2 Kapacita SKU</span><span class="sxs-lookup"><span data-stu-id="fccf7-118">S2 SKU Capacity</span></span> | <span data-ttu-id="fccf7-119">Příchozí míra</span><span class="sxs-lookup"><span data-stu-id="fccf7-119">Ingress Rate</span></span> | <span data-ttu-id="fccf7-120">Maximální kapacita úložiště</span><span class="sxs-lookup"><span data-stu-id="fccf7-120">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="fccf7-121">1</span><span class="sxs-lookup"><span data-stu-id="fccf7-121">1</span></span> | <span data-ttu-id="fccf7-122">10 GB (10 milionů událostí)</span><span class="sxs-lookup"><span data-stu-id="fccf7-122">10 GB (10 million events)</span></span> | <span data-ttu-id="fccf7-123">300 GB za měsíc (300 milión událostí)</span><span class="sxs-lookup"><span data-stu-id="fccf7-123">300 GB (300 million events) per month</span></span> |
| <span data-ttu-id="fccf7-124">10</span><span class="sxs-lookup"><span data-stu-id="fccf7-124">10</span></span> | <span data-ttu-id="fccf7-125">100 GB (100 miliónů událostí)</span><span class="sxs-lookup"><span data-stu-id="fccf7-125">100 GB (100 million events)</span></span> | <span data-ttu-id="fccf7-126">3 TB (3 miliardy událostí) za měsíc</span><span class="sxs-lookup"><span data-stu-id="fccf7-126">3 TB (3 billion events) per month</span></span> |

<span data-ttu-id="fccf7-127">Kapacity lineárně, takže S1 sku s kapacitou 2 podporuje 2 GB (2 miliony) událostí za den příchozího rychlost a 60 GB (60 milión událostí) za měsíc.</span><span class="sxs-lookup"><span data-stu-id="fccf7-127">Capacities scale linearly, so a S1 sku with capacity 2 supports 2 GB (2 million) events per day ingress rate and 60 GB (60 million events) per month.</span></span>

## <a name="changing-the-capacity-of-your-environment"></a><span data-ttu-id="fccf7-128">Změna kapacitu vašeho prostředí</span><span class="sxs-lookup"><span data-stu-id="fccf7-128">Changing the capacity of your environment</span></span>

1. <span data-ttu-id="fccf7-129">Na portálu Azure vyberte prostředí, jehož kapacitu, kterou chcete změnit.</span><span class="sxs-lookup"><span data-stu-id="fccf7-129">In the Azure portal, select the environment whose capacity you want to change.</span></span>
1. <span data-ttu-id="fccf7-130">V části nastavení klikněte na tlačítko Konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="fccf7-130">Under Settings, click Configure.</span></span>
1. <span data-ttu-id="fccf7-131">Posuvníkem kapacity vyberte kapacitu, který splňuje požadavky pro příjem příchozích dat sazby a kapacitu úložiště.</span><span class="sxs-lookup"><span data-stu-id="fccf7-131">Use the Capacity slider to select the capacity that meets the requirements for your ingress rates and storage capacity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fccf7-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fccf7-132">Next steps</span></span>

* <span data-ttu-id="fccf7-133">Ověřte, že nové kapacity dostatečná k zabránění, omezení šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="fccf7-133">Verify that the new capacity is sufficient to prevent throttling.</span></span> <span data-ttu-id="fccf7-134">Další podrobnosti najdete v tématu *prostředí může získávání omezeny* části [zde](time-series-insights-diagnose-and-solve-problems.md).</span><span class="sxs-lookup"><span data-stu-id="fccf7-134">For more details, see the *Your environment might be getting throttled* section [here](time-series-insights-diagnose-and-solve-problems.md).</span></span>