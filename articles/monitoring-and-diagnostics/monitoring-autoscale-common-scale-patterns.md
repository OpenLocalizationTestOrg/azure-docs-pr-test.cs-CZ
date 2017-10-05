---
title: "Přehled běžných vzorů škálování | Microsoft Docs"
description: "Další některých běžných vzorů na automatické škálování prostředku v Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: fce51546e041c8989d813c3935e058c52b38ba77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-common-autoscale-patterns"></a><span data-ttu-id="f22be-103">Přehled běžných vzorů škálování</span><span class="sxs-lookup"><span data-stu-id="f22be-103">Overview of common autoscale patterns</span></span>
<span data-ttu-id="f22be-104">Tento článek popisuje některé z běžných vzorů škálování prostředku v Azure.</span><span class="sxs-lookup"><span data-stu-id="f22be-104">This article describes some of the common patterns to scale your resource in Azure.</span></span>

<span data-ttu-id="f22be-105">Azure monitorování automatického škálování se vztahuje pouze na virtuální počítač škálování sady (VMSS), cloudové služby, plány služby app a prostředí app service.</span><span class="sxs-lookup"><span data-stu-id="f22be-105">Azure Monitor auto scale applies only to Virtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="f22be-106">Umožňuje Začínáme</span><span class="sxs-lookup"><span data-stu-id="f22be-106">Lets get started</span></span>

<span data-ttu-id="f22be-107">Tento článek předpokládá, že jste obeznámeni s automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="f22be-107">This article assumes that you are familiar with auto scale.</span></span> <span data-ttu-id="f22be-108">Můžete [Začněte zde škálování prostředku][1].</span><span class="sxs-lookup"><span data-stu-id="f22be-108">You can [get started here to scale your resource][1].</span></span> <span data-ttu-id="f22be-109">Toto jsou některé obecné vzory škálování.</span><span class="sxs-lookup"><span data-stu-id="f22be-109">The following are some of the common scale patterns.</span></span>

## <a name="scale-based-on-cpu"></a><span data-ttu-id="f22be-110">Škálování podle využití procesoru</span><span class="sxs-lookup"><span data-stu-id="f22be-110">Scale based on CPU</span></span>

<span data-ttu-id="f22be-111">Máte webovou aplikaci (/ VMSS/cloudové služby role) a</span><span class="sxs-lookup"><span data-stu-id="f22be-111">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="f22be-112">Chcete Škálováním na více systémů nebo měřítka ve na základě využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="f22be-112">You want to scale out/scale in based on CPU.</span></span>
- <span data-ttu-id="f22be-113">Kromě toho chcete zajistit, že je minimální počet instancí.</span><span class="sxs-lookup"><span data-stu-id="f22be-113">Additionally, you want to ensure there is a minimum number of instances.</span></span> 
- <span data-ttu-id="f22be-114">Navíc chcete zajistit nastavit maximální limit pro počet instancí, které je možné škálovat na.</span><span class="sxs-lookup"><span data-stu-id="f22be-114">Also, you want to ensure that you set a maximum limit to the number of instances you can scale to.</span></span>

![Škálování podle využití procesoru][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a><span data-ttu-id="f22be-116">Škálování jinak o víkendech vs dny v týdnu</span><span class="sxs-lookup"><span data-stu-id="f22be-116">Scale differently on weekdays vs weekends</span></span>

<span data-ttu-id="f22be-117">Máte webovou aplikaci (/ VMSS/cloudové služby role) a</span><span class="sxs-lookup"><span data-stu-id="f22be-117">You have a web app (/VMSS/cloud service role) and</span></span>

- <span data-ttu-id="f22be-118">Chcete 3 instance ve výchozím nastavení (ve všední dny)</span><span class="sxs-lookup"><span data-stu-id="f22be-118">You want 3 instances by default (on weekdays)</span></span>
- <span data-ttu-id="f22be-119">O víkendech nepředpokládáte přenosy a proto chcete škálovat dolů 1 instance o víkendech.</span><span class="sxs-lookup"><span data-stu-id="f22be-119">You don't expect traffic on weekends and hence you want to scale down to 1 instance on weekends.</span></span>

![Škálování jinak o víkendech vs dny v týdnu][3]

## <a name="scale-differently-during-holidays"></a><span data-ttu-id="f22be-121">Jinak škálování během svátků</span><span class="sxs-lookup"><span data-stu-id="f22be-121">Scale differently during holidays</span></span>

<span data-ttu-id="f22be-122">Máte webovou aplikaci (/ VMSS/cloudové služby role) a</span><span class="sxs-lookup"><span data-stu-id="f22be-122">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="f22be-123">Chcete škálovat nahoru/dolů podle využití procesoru ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="f22be-123">You want to scale up/down based on CPU usage by default</span></span>
- <span data-ttu-id="f22be-124">Však během sváteční sezóny (nebo konkrétní dny, které jsou důležité pro vaši firmu) chcete přepsat výchozí hodnoty a mít k dispozici větší kapacitu.</span><span class="sxs-lookup"><span data-stu-id="f22be-124">However, during holiday season (or specific days that are important for your business) you want to override the defaults and have more capacity at your disposal.</span></span>

![Jinak na svátků škálování][4]

## <a name="scale-based-on-custom-metric"></a><span data-ttu-id="f22be-126">Škálování podle vlastní metriku</span><span class="sxs-lookup"><span data-stu-id="f22be-126">Scale based on custom metric</span></span>

<span data-ttu-id="f22be-127">Máte front-end webové a vrstvu rozhraní API, který komunikuje s back-end.</span><span class="sxs-lookup"><span data-stu-id="f22be-127">You have a web front end and a API tier that communicates with the backend.</span></span> 

- <span data-ttu-id="f22be-128">Chcete-li škálovat vrstvu rozhraní API na základě vlastních událostí v části front end (Příklad: Chcete škálovat procesu odbavení na základě počtu položek v nákupní košík)</span><span class="sxs-lookup"><span data-stu-id="f22be-128">You want to scale the API tier based on custom events in the front end (example: You want to scale your checkout process based on the number of items in the shopping cart)</span></span>

![Škálování podle vlastní metriku][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png