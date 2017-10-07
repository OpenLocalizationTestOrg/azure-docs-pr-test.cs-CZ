---
title: "aaaOverview běžných vzorů škálování | Microsoft Docs"
description: "Podívejte se, že některé z běžných vzorů tooauto hello škálování prostředku v Azure."
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
ms.openlocfilehash: fc5bd97852e0af01aa32940c99721ab8e21033ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-common-autoscale-patterns"></a><span data-ttu-id="376e2-103">Přehled běžných vzorů škálování</span><span class="sxs-lookup"><span data-stu-id="376e2-103">Overview of common autoscale patterns</span></span>
<span data-ttu-id="376e2-104">Tento článek popisuje některé běžné vzory tooscale hello prostředku v Azure.</span><span class="sxs-lookup"><span data-stu-id="376e2-104">This article describes some of hello common patterns tooscale your resource in Azure.</span></span>

<span data-ttu-id="376e2-105">Azure monitorování automatického škálování se vztahují pouze tooVirtual počítač škálování sady (VMSS), cloudové služby, plány služby app a prostředí app service.</span><span class="sxs-lookup"><span data-stu-id="376e2-105">Azure Monitor auto scale applies only tooVirtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="376e2-106">Umožňuje Začínáme</span><span class="sxs-lookup"><span data-stu-id="376e2-106">Lets get started</span></span>

<span data-ttu-id="376e2-107">Tento článek předpokládá, že jste obeznámeni s automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="376e2-107">This article assumes that you are familiar with auto scale.</span></span> <span data-ttu-id="376e2-108">Můžete [získat Začínáme sem tooscale prostředku][1].</span><span class="sxs-lookup"><span data-stu-id="376e2-108">You can [get started here tooscale your resource][1].</span></span> <span data-ttu-id="376e2-109">Hello Toto jsou některé z běžných vzorů škálování hello.</span><span class="sxs-lookup"><span data-stu-id="376e2-109">hello following are some of hello common scale patterns.</span></span>

## <a name="scale-based-on-cpu"></a><span data-ttu-id="376e2-110">Škálování podle využití procesoru</span><span class="sxs-lookup"><span data-stu-id="376e2-110">Scale based on CPU</span></span>

<span data-ttu-id="376e2-111">Máte webovou aplikaci (/ VMSS/cloudové služby role) a</span><span class="sxs-lookup"><span data-stu-id="376e2-111">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="376e2-112">Chcete-li tooscale na více systémů nebo měřítka v závislosti na procesor.</span><span class="sxs-lookup"><span data-stu-id="376e2-112">You want tooscale out/scale in based on CPU.</span></span>
- <span data-ttu-id="376e2-113">Kromě toho chcete tooensure je minimální počet instancí.</span><span class="sxs-lookup"><span data-stu-id="376e2-113">Additionally, you want tooensure there is a minimum number of instances.</span></span> 
- <span data-ttu-id="376e2-114">Navíc chcete tooensure, která můžete nastavit maximální limit toohello, počet instancí, které je možné škálovat na.</span><span class="sxs-lookup"><span data-stu-id="376e2-114">Also, you want tooensure that you set a maximum limit toohello number of instances you can scale to.</span></span>

![Škálování podle využití procesoru][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a><span data-ttu-id="376e2-116">Škálování jinak o víkendech vs dny v týdnu</span><span class="sxs-lookup"><span data-stu-id="376e2-116">Scale differently on weekdays vs weekends</span></span>

<span data-ttu-id="376e2-117">Máte webovou aplikaci (/ VMSS/cloudové služby role) a</span><span class="sxs-lookup"><span data-stu-id="376e2-117">You have a web app (/VMSS/cloud service role) and</span></span>

- <span data-ttu-id="376e2-118">Chcete 3 instance ve výchozím nastavení (ve všední dny)</span><span class="sxs-lookup"><span data-stu-id="376e2-118">You want 3 instances by default (on weekdays)</span></span>
- <span data-ttu-id="376e2-119">O víkendech nepředpokládáte přenosy a proto chcete tooscale dolů too1 instance o víkendech.</span><span class="sxs-lookup"><span data-stu-id="376e2-119">You don't expect traffic on weekends and hence you want tooscale down too1 instance on weekends.</span></span>

![Škálování jinak o víkendech vs dny v týdnu][3]

## <a name="scale-differently-during-holidays"></a><span data-ttu-id="376e2-121">Jinak škálování během svátků</span><span class="sxs-lookup"><span data-stu-id="376e2-121">Scale differently during holidays</span></span>

<span data-ttu-id="376e2-122">Máte webovou aplikaci (/ VMSS/cloudové služby role) a</span><span class="sxs-lookup"><span data-stu-id="376e2-122">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="376e2-123">Chcete-li tooscale nahoru/dolů podle využití procesoru ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="376e2-123">You want tooscale up/down based on CPU usage by default</span></span>
- <span data-ttu-id="376e2-124">Během sváteční sezóny (nebo konkrétní dny, které jsou důležité pro vaši firmu) však má výchozí hello toooverride a mít k dispozici větší kapacitu.</span><span class="sxs-lookup"><span data-stu-id="376e2-124">However, during holiday season (or specific days that are important for your business) you want toooverride hello defaults and have more capacity at your disposal.</span></span>

![Jinak na svátků škálování][4]

## <a name="scale-based-on-custom-metric"></a><span data-ttu-id="376e2-126">Škálování podle vlastní metriku</span><span class="sxs-lookup"><span data-stu-id="376e2-126">Scale based on custom metric</span></span>

<span data-ttu-id="376e2-127">Máte front-end webové a vrstvu rozhraní API, který komunikuje s back-end hello.</span><span class="sxs-lookup"><span data-stu-id="376e2-127">You have a web front end and a API tier that communicates with hello backend.</span></span> 

- <span data-ttu-id="376e2-128">Chcete tooscale hello rozhraní API vrstvy na základě vlastních událostí v hello front end (Příklad: Chcete tooscale procesu odbavení podle hello počet položek v hello nákupní košík)</span><span class="sxs-lookup"><span data-stu-id="376e2-128">You want tooscale hello API tier based on custom events in hello front end (example: You want tooscale your checkout process based on hello number of items in hello shopping cart)</span></span>

![Škálování podle vlastní metriku][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png