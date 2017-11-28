---
title: "aaaHow tooscale prostředí Azure časové řady Insights | Microsoft Docs"
description: "Tento kurz se zaměřuje na tom, jak tooscale prostředí Statistika Azure časové řady"
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
ms.openlocfilehash: 55eda388997589185bd34228762b95e182b228ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-your-time-series-insights-environment"></a><span data-ttu-id="7ea8e-103">Jak tooscale prostředí Statistika časové řady</span><span class="sxs-lookup"><span data-stu-id="7ea8e-103">How tooscale your Time Series Insights environment</span></span>

<span data-ttu-id="7ea8e-104">Tento kurz se zaměřuje na tom, jak tooscale prostředí Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="7ea8e-104">This tutorial covers how tooscale your Time Series Insights environment.</span></span>

> [!NOTE]
> <span data-ttu-id="7ea8e-105">Rozšiřování škálování využívajících typů sku není povoleno.</span><span class="sxs-lookup"><span data-stu-id="7ea8e-105">Scale up across sku types is not allowed.</span></span> <span data-ttu-id="7ea8e-106">Prostředí s S1 Sku nelze převést do prostředí S2.</span><span class="sxs-lookup"><span data-stu-id="7ea8e-106">An environment with a S1 Sku cannot be converted into an S2 environment.</span></span>

## <a name="s1-sku-ingress-rates-and-capacities"></a><span data-ttu-id="7ea8e-107">Příjem příchozích dat sazby S1 SKU a kapacity</span><span class="sxs-lookup"><span data-stu-id="7ea8e-107">S1 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="7ea8e-108">S1 Kapacita SKU</span><span class="sxs-lookup"><span data-stu-id="7ea8e-108">S1 SKU Capacity</span></span> | <span data-ttu-id="7ea8e-109">Příchozí míra</span><span class="sxs-lookup"><span data-stu-id="7ea8e-109">Ingress Rate</span></span> | <span data-ttu-id="7ea8e-110">Maximální kapacita úložiště</span><span class="sxs-lookup"><span data-stu-id="7ea8e-110">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="7ea8e-111">1</span><span class="sxs-lookup"><span data-stu-id="7ea8e-111">1</span></span> | <span data-ttu-id="7ea8e-112">1 GB (1 milion událostí)</span><span class="sxs-lookup"><span data-stu-id="7ea8e-112">1 GB (1 million events)</span></span> | <span data-ttu-id="7ea8e-113">(30 milionů událostí) 30 GB za měsíc</span><span class="sxs-lookup"><span data-stu-id="7ea8e-113">30 GB (30 million events) per month</span></span> |
| <span data-ttu-id="7ea8e-114">10</span><span class="sxs-lookup"><span data-stu-id="7ea8e-114">10</span></span> | <span data-ttu-id="7ea8e-115">10 GB (10 milionů událostí)</span><span class="sxs-lookup"><span data-stu-id="7ea8e-115">10 GB (10 million events)</span></span> | <span data-ttu-id="7ea8e-116">300 GB za měsíc (300 milión událostí)</span><span class="sxs-lookup"><span data-stu-id="7ea8e-116">300 GB (300 million events) per month</span></span> |

## <a name="s2-sku-ingress-rates-and-capacities"></a><span data-ttu-id="7ea8e-117">S2 SKU příjem příchozích dat rychlostí a kapacity</span><span class="sxs-lookup"><span data-stu-id="7ea8e-117">S2 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="7ea8e-118">S2 Kapacita SKU</span><span class="sxs-lookup"><span data-stu-id="7ea8e-118">S2 SKU Capacity</span></span> | <span data-ttu-id="7ea8e-119">Příchozí míra</span><span class="sxs-lookup"><span data-stu-id="7ea8e-119">Ingress Rate</span></span> | <span data-ttu-id="7ea8e-120">Maximální kapacita úložiště</span><span class="sxs-lookup"><span data-stu-id="7ea8e-120">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="7ea8e-121">1</span><span class="sxs-lookup"><span data-stu-id="7ea8e-121">1</span></span> | <span data-ttu-id="7ea8e-122">10 GB (10 milionů událostí)</span><span class="sxs-lookup"><span data-stu-id="7ea8e-122">10 GB (10 million events)</span></span> | <span data-ttu-id="7ea8e-123">300 GB za měsíc (300 milión událostí)</span><span class="sxs-lookup"><span data-stu-id="7ea8e-123">300 GB (300 million events) per month</span></span> |
| <span data-ttu-id="7ea8e-124">10</span><span class="sxs-lookup"><span data-stu-id="7ea8e-124">10</span></span> | <span data-ttu-id="7ea8e-125">100 GB (100 miliónů událostí)</span><span class="sxs-lookup"><span data-stu-id="7ea8e-125">100 GB (100 million events)</span></span> | <span data-ttu-id="7ea8e-126">3 TB (3 miliardy událostí) za měsíc</span><span class="sxs-lookup"><span data-stu-id="7ea8e-126">3 TB (3 billion events) per month</span></span> |

<span data-ttu-id="7ea8e-127">Kapacity lineárně, takže S1 sku s kapacitou 2 podporuje 2 GB (2 miliony) událostí za den příchozího rychlost a 60 GB (60 milión událostí) za měsíc.</span><span class="sxs-lookup"><span data-stu-id="7ea8e-127">Capacities scale linearly, so a S1 sku with capacity 2 supports 2 GB (2 million) events per day ingress rate and 60 GB (60 million events) per month.</span></span>

## <a name="changing-hello-capacity-of-your-environment"></a><span data-ttu-id="7ea8e-128">Změna hello kapacitu vašeho prostředí</span><span class="sxs-lookup"><span data-stu-id="7ea8e-128">Changing hello capacity of your environment</span></span>

1. <span data-ttu-id="7ea8e-129">V hello portálu Azure, vyberte text hello prostředí jejichž kapacity chcete toochange.</span><span class="sxs-lookup"><span data-stu-id="7ea8e-129">In hello Azure portal, select hello environment whose capacity you want toochange.</span></span>
1. <span data-ttu-id="7ea8e-130">V části nastavení klikněte na tlačítko Konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="7ea8e-130">Under Settings, click Configure.</span></span>
1. <span data-ttu-id="7ea8e-131">Použijte hello kapacity posuvníku tooselect hello kapacitu, který splňuje požadavky hello sazby příjem příchozích dat a kapacitu úložiště.</span><span class="sxs-lookup"><span data-stu-id="7ea8e-131">Use hello Capacity slider tooselect hello capacity that meets hello requirements for your ingress rates and storage capacity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ea8e-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ea8e-132">Next steps</span></span>

* <span data-ttu-id="7ea8e-133">Ověřte, zda je dostatek nové kapacity hello tooprevent omezení.</span><span class="sxs-lookup"><span data-stu-id="7ea8e-133">Verify that hello new capacity is sufficient tooprevent throttling.</span></span> <span data-ttu-id="7ea8e-134">Další podrobnosti najdete v tématu hello *prostředí může získávání omezeny* části [zde](time-series-insights-diagnose-and-solve-problems.md).</span><span class="sxs-lookup"><span data-stu-id="7ea8e-134">For more details, see hello *Your environment might be getting throttled* section [here](time-series-insights-diagnose-and-solve-problems.md).</span></span>
