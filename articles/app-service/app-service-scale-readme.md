---
title: "Aplikační služba Azure: Škálování aplikací služby App Service"
description: "Přečtěte si hello in a výpisů nastavení velikosti aplikace ve službě App Service."
keywords: "app service, azure app service, škálování, škálovatelné, plán služby App Service, náklady služby App Service"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: f403c971-4450-432b-8cea-3eeb426c0147
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2016
ms.author: byvinyal
ms.openlocfilehash: d8cd12f73915a916a75d46da2f751a40d567b189
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a><span data-ttu-id="1b2b2-104">Aplikační služba Azure: Škálování aplikací služby App Service</span><span class="sxs-lookup"><span data-stu-id="1b2b2-104">Azure App Service: Scaling App Service Applications</span></span>
<span data-ttu-id="1b2b2-105">Aplikace hostované v Azure App Service můžete [dosáhnout masivním měřítku](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span><span class="sxs-lookup"><span data-stu-id="1b2b2-105">Applications hosted in Azure App Service can [achieve massive scale](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span></span>
<span data-ttu-id="1b2b2-106">Škálování aplikace je však složitý problém, který nemá řešení "velikosti, která vyhoví všechny".</span><span class="sxs-lookup"><span data-stu-id="1b2b2-106">However, scaling an application is a complex problem that does not have a "one size fits all" solution.</span></span> <span data-ttu-id="1b2b2-107">toocorrectly škálování aplikace jsou 3 klíčové oblasti, které přispějí úspěch tooyour aplikace:</span><span class="sxs-lookup"><span data-stu-id="1b2b2-107">toocorrectly scale your application there are 3 key areas that will contribute tooyour applications success:</span></span>

1. <span data-ttu-id="1b2b2-108">Princip architektuře aplikace a její slabá místa.</span><span class="sxs-lookup"><span data-stu-id="1b2b2-108">Understanding your application architecture and its weaknesses.</span></span>
   * <span data-ttu-id="1b2b2-109">Je vaše aplikace Stateful?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-109">Is your Application Stateful?</span></span> <span data-ttu-id="1b2b2-110">Bezstavové?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-110">Stateless?</span></span>
   * <span data-ttu-id="1b2b2-111">Co jsou všechny součásti hello aplikace?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-111">What are all hello components of your application?</span></span>
     * <span data-ttu-id="1b2b2-112">Kde jsou hello kritická místa v aplikaci hello?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-112">Where are hello bottlenecks in hello application?</span></span>
   * <span data-ttu-id="1b2b2-113">Když je zatížení použité tooyour aplikace, co by došlo k přerušení první?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-113">When load is applied tooyour app, what will break first?</span></span>
2. <span data-ttu-id="1b2b2-114">Principy hello očekává požadavky na zatížení a výkon.</span><span class="sxs-lookup"><span data-stu-id="1b2b2-114">Understanding hello expected load and performance requirements.</span></span>
   * <span data-ttu-id="1b2b2-115">Potřebuje aplikace hello tooserve tisíc uživatelů? nebo jeden milión?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-115">Does hello application need tooserve one thousand users? or one million?</span></span>
   * <span data-ttu-id="1b2b2-116">Vrátí se provoz z jednoho geografické umístění nebo globálně?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-116">Will traffic come from a single geographic location or globally?</span></span>
   * <span data-ttu-id="1b2b2-117">Existují sezónních odchylek? provoz vrcholů?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-117">Are there seasonal variations? traffic peaks?</span></span>
   * <span data-ttu-id="1b2b2-118">Jak rychle by měl odpovídat hello aplikace?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-118">How fast should hello app respond?</span></span> <span data-ttu-id="1b2b2-119">1 sekunda?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-119">1 second?</span></span> <span data-ttu-id="1b2b2-120">1 milisekundu?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-120">1 millisecond?</span></span>
3. <span data-ttu-id="1b2b2-121">Pochopení a správně platformu využívají hello hostování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b2b2-121">Understanding and correctly leverage hello platform hosting your app.</span></span>
   * <span data-ttu-id="1b2b2-122">Co funkce měli I využít tooachieve Moje cíle škálování?</span><span class="sxs-lookup"><span data-stu-id="1b2b2-122">What features should I leverage tooachieve my scale goals?</span></span>

<span data-ttu-id="1b2b2-123">Tato část vám pomůže porozumět všechny hello faktory a navrhnout strategii, která využívá výhod hello nezbytné služby App Service funkce tooachieve cílů škálovatelnost nápovědy.</span><span class="sxs-lookup"><span data-stu-id="1b2b2-123">This section will help you understand all hello factors and help you devise a strategy that takes advantage of hello necessary App Service features tooachieve your scalability goals.</span></span>

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

