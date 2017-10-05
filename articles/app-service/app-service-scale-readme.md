---
title: "Aplikační služba Azure: Škálování aplikací služby App Service"
description: "Zjistěte, výpisů a in nastavení velikosti aplikace ve službě App Service."
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
ms.openlocfilehash: 4ebaafe69fc1f91c7890611b14e8600df5c40cde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a><span data-ttu-id="baab8-104">Aplikační služba Azure: Škálování aplikací služby App Service</span><span class="sxs-lookup"><span data-stu-id="baab8-104">Azure App Service: Scaling App Service Applications</span></span>
<span data-ttu-id="baab8-105">Aplikace hostované v Azure App Service můžete [dosáhnout masivním měřítku](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span><span class="sxs-lookup"><span data-stu-id="baab8-105">Applications hosted in Azure App Service can [achieve massive scale](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).</span></span>
<span data-ttu-id="baab8-106">Škálování aplikace je však složitý problém, který nemá řešení "velikosti, která vyhoví všechny".</span><span class="sxs-lookup"><span data-stu-id="baab8-106">However, scaling an application is a complex problem that does not have a "one size fits all" solution.</span></span> <span data-ttu-id="baab8-107">Správně škálování aplikace existuje jsou 3 klíčové oblasti, budou tyto rovněž přispívat k úspěchu aplikace:</span><span class="sxs-lookup"><span data-stu-id="baab8-107">To correctly scale your application there are 3 key areas that will contribute to your applications success:</span></span>

1. <span data-ttu-id="baab8-108">Princip architektuře aplikace a její slabá místa.</span><span class="sxs-lookup"><span data-stu-id="baab8-108">Understanding your application architecture and its weaknesses.</span></span>
   * <span data-ttu-id="baab8-109">Je vaše aplikace Stateful?</span><span class="sxs-lookup"><span data-stu-id="baab8-109">Is your Application Stateful?</span></span> <span data-ttu-id="baab8-110">Bezstavové?</span><span class="sxs-lookup"><span data-stu-id="baab8-110">Stateless?</span></span>
   * <span data-ttu-id="baab8-111">Co jsou všechny součásti aplikace?</span><span class="sxs-lookup"><span data-stu-id="baab8-111">What are all the components of your application?</span></span>
     * <span data-ttu-id="baab8-112">Kde jsou kritická místa v aplikaci?</span><span class="sxs-lookup"><span data-stu-id="baab8-112">Where are the bottlenecks in the application?</span></span>
   * <span data-ttu-id="baab8-113">Při načítání do vaší aplikace, co by došlo k přerušení první?</span><span class="sxs-lookup"><span data-stu-id="baab8-113">When load is applied to your app, what will break first?</span></span>
2. <span data-ttu-id="baab8-114">Seznámení s požadavky na očekávané zátěže a výkonu.</span><span class="sxs-lookup"><span data-stu-id="baab8-114">Understanding the expected load and performance requirements.</span></span>
   * <span data-ttu-id="baab8-115">Aplikace musí poskytovat tisíc uživatelů? nebo jeden milión?</span><span class="sxs-lookup"><span data-stu-id="baab8-115">Does the application need to serve one thousand users? or one million?</span></span>
   * <span data-ttu-id="baab8-116">Vrátí se provoz z jednoho geografické umístění nebo globálně?</span><span class="sxs-lookup"><span data-stu-id="baab8-116">Will traffic come from a single geographic location or globally?</span></span>
   * <span data-ttu-id="baab8-117">Existují sezónních odchylek? provoz vrcholů?</span><span class="sxs-lookup"><span data-stu-id="baab8-117">Are there seasonal variations? traffic peaks?</span></span>
   * <span data-ttu-id="baab8-118">Jak rychle musí odpovídat aplikace?</span><span class="sxs-lookup"><span data-stu-id="baab8-118">How fast should the app respond?</span></span> <span data-ttu-id="baab8-119">1 sekunda?</span><span class="sxs-lookup"><span data-stu-id="baab8-119">1 second?</span></span> <span data-ttu-id="baab8-120">1 milisekundu?</span><span class="sxs-lookup"><span data-stu-id="baab8-120">1 millisecond?</span></span>
3. <span data-ttu-id="baab8-121">Princip fungování a způsob správně využít platformou hostování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="baab8-121">Understanding and correctly leverage the platform hosting your app.</span></span>
   * <span data-ttu-id="baab8-122">Jaké funkce by je měli využít k dosažení Moje cíle škálování?</span><span class="sxs-lookup"><span data-stu-id="baab8-122">What features should I leverage to achieve my scale goals?</span></span>

<span data-ttu-id="baab8-123">Tato část vám pomůže porozumět faktory a navrhnout strategii, která využívá potřebné funkce služby App Service, abyste dosáhli svých cílů škálovatelnost nápovědy.</span><span class="sxs-lookup"><span data-stu-id="baab8-123">This section will help you understand all the factors and help you devise a strategy that takes advantage of the necessary App Service features to achieve your scalability goals.</span></span>

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

