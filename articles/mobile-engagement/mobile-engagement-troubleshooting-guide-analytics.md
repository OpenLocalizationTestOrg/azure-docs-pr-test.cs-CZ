---
title: "Průvodce - Analytics odstraňováním potíží s Azure Mobile Engagement."
description: "Řešení potíží s analýzy, monitorování, segmentaci a řídicí panel v Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04a7020a-ad74-4491-be69-0bd574890029
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e30c9ac0a8421ffcf4fc3e2548cfd7ac49701900
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a><span data-ttu-id="41e9f-103">Průvodce řešením potíží pro problémy analýzy, monitorování, segmentaci a řídicí panel</span><span class="sxs-lookup"><span data-stu-id="41e9f-103">Troubleshooting guide for Analytics, Monitoring, Segmentation, and Dashboard issues</span></span>
<span data-ttu-id="41e9f-104">Následují informace o možných problémech se můžete setkat s jak Azure Mobile Engagement shromažďuje informace o aplikacích, zařízení a uživatelů.</span><span class="sxs-lookup"><span data-stu-id="41e9f-104">The following are possible issues you may encounter with how Azure Mobile Engagement gathers information about your applications, devices, and users.</span></span>

## <a name="missingdelayed-information"></a><span data-ttu-id="41e9f-105">Chybějící nebo opožděná informace</span><span class="sxs-lookup"><span data-stu-id="41e9f-105">Missing/Delayed information</span></span>
### <a name="issue"></a><span data-ttu-id="41e9f-106">Problém</span><span class="sxs-lookup"><span data-stu-id="41e9f-106">Issue</span></span>
* <span data-ttu-id="41e9f-107">V uvedených v analýzy, segmentace nebo řídicí panel je zpožděno informace.</span><span class="sxs-lookup"><span data-stu-id="41e9f-107">Information is delayed in appearing in Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="41e9f-108">Chybí informace z monitorování.</span><span class="sxs-lookup"><span data-stu-id="41e9f-108">Information is missing from Monitoring.</span></span>
* <span data-ttu-id="41e9f-109">Chybí informace z analýzy, segmentace nebo řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="41e9f-109">Information is missing from Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="41e9f-110">Nedosáhli limitů segmentace.</span><span class="sxs-lookup"><span data-stu-id="41e9f-110">Hitting segmentation limits.</span></span>

### <a name="causes"></a><span data-ttu-id="41e9f-111">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="41e9f-111">Causes</span></span>
* <span data-ttu-id="41e9f-112">Můžete použít rozhraní API analýzy, monitorování API a rozhraní API segmenty zobrazíte, pokud žádná data jste chybí uživatelské rozhraní je viditelné prostřednictvím rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="41e9f-112">You can use the Analytics API, Monitor API, and Segments API to see if any data you are missing from the UI is visible through the APIs.</span></span>
* <span data-ttu-id="41e9f-113">Pokud Azure Mobile Engagement SDK není správně integrovaná do aplikace pak nebudete moci zobrazit informace v analýzy, segmentace, monitorování nebo řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="41e9f-113">If the Azure Mobile Engagement SDK is not correctly integrated into your app then you won't be able to see information in the Analytics, Segmentation, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="41e9f-114">Segmenty nelze změnit po jejich vytvoření, segmenty může být pouze "klonovaný" (zkopírovat) nebo "zničen" (odstranit).</span><span class="sxs-lookup"><span data-stu-id="41e9f-114">Segments can't be changed once they are created, segments can only be "cloned" (copied) or "destroyed" (deleted).</span></span> <span data-ttu-id="41e9f-115">Segmenty může obsahovat jenom 10 kritéria.</span><span class="sxs-lookup"><span data-stu-id="41e9f-115">Segments can only contain 10 criteria.</span></span>
* <span data-ttu-id="41e9f-116">Nejlepší způsob, jak otestovat chybějící informace z monitorování je nastavit testovací zařízení, odinstalujte a znovu nainstalovat aplikaci na testovací zařízení.</span><span class="sxs-lookup"><span data-stu-id="41e9f-116">The best way to test missing information from monitoring is to setup a test device, uninstall and/or reinstall the app on the test device.</span></span>
* <span data-ttu-id="41e9f-117">Informace se aktualizují každých 24 hodin pro analýzy, segmentace nebo řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="41e9f-117">Information is refreshed every 24 hours for Analytics, Segmentation, or Dashboards.</span></span>
* <span data-ttu-id="41e9f-118">Informace v nové segmenty, se nemusí zobrazit do 24 hodin po jejich vytvoření i v případě, že předchozí informace podle segmentu.</span><span class="sxs-lookup"><span data-stu-id="41e9f-118">Information in new segments may not be displayed until 24 hours after they are created even if the segment is based on previous information.</span></span>
* <span data-ttu-id="41e9f-119">Filtrování dat analýzy v uživatelském rozhraní se zobrazí všechny příklady tohoto typu bez ohledu na verzi vaší aplikace (např.) "Dojde k chybě" filtrovat podle názvu se zobrazí z verze 1 a 2 verzi aplikace).</span><span class="sxs-lookup"><span data-stu-id="41e9f-119">Filtering your analytics data in the UI will show all examples of this type regardless of the version of your app (e.g. "Crashes" filtered by name will show from version 1 and version 2 of your app).</span></span>
* <span data-ttu-id="41e9f-120">Časové období pro analýzy je založena na datum z nastavení zařízení uživatelů, takže může uživatele, jehož telefon má datum nesprávné sady zobrazí v nesprávný časové období.</span><span class="sxs-lookup"><span data-stu-id="41e9f-120">The time period for Analytics is based on the date from the users' device settings, so a user whose phone has the date incorrectly set could show up in the wrong time period.</span></span>
* <span data-ttu-id="41e9f-121">Žádné na straně serveru, který data se protokolují, když použijete tlačítko pro "test" nabízených oznámení, data se protokolují pouze pro skutečné nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="41e9f-121">No server side data is logged when you use the button to "test" pushes, data is only logged for real push campaigns.</span></span>

## <a name="cant-locate-items-in-ui"></a><span data-ttu-id="41e9f-122">Nelze najít položky v uživatelském rozhraní</span><span class="sxs-lookup"><span data-stu-id="41e9f-122">Can't locate items in UI</span></span>
### <a name="issue"></a><span data-ttu-id="41e9f-123">Problém</span><span class="sxs-lookup"><span data-stu-id="41e9f-123">Issue</span></span>
* <span data-ttu-id="41e9f-124">Nelze vytvořit segmenty založené na určité součástí nebo informace o aplikaci vlastní značky kritéria.</span><span class="sxs-lookup"><span data-stu-id="41e9f-124">Can't create segments based on certain built in or custom app info tag criteria.</span></span>
* <span data-ttu-id="41e9f-125">Nelze najít určité součástí nebo informace o aplikaci vlastní značky kritéria analýzy, monitorování nebo řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="41e9f-125">Can't find certain built in or custom app info tag criteria in Analytics, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="41e9f-126">Nemůže interpretovat data v analýzy, monitorování, segmentace nebo řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="41e9f-126">Can't interpret the data in Analytics, Monitoring, Segmentation, or Dashboards.</span></span>

### <a name="causes"></a><span data-ttu-id="41e9f-127">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="41e9f-127">Causes</span></span>
* <span data-ttu-id="41e9f-128">Některé položky součástí a značky informace o aplikaci jsou dostupné jen jako nabízené kritéria, ale nemusí být přidány do segment nebo viditelné z analýzy, monitorování nebo řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="41e9f-128">Some built in items and app info tags are only available as push criteria but may not be added to a segment or visible from Analytics, Monitoring, or Dashboard.</span></span> 
* <span data-ttu-id="41e9f-129">Pro integrované položky a značky informace o aplikaci, které nelze přidat do segmentu bude muset instalace seznamu cílových kritérií pro každou kampaň provádí stejnou funkci jako cílení na základě v segmentu.</span><span class="sxs-lookup"><span data-stu-id="41e9f-129">For built in items and app info tags that can't be added to a segment, you will need to setup list of targeting criteria in each campaign to perform the same function as targeting based on a segment.</span></span>
* <span data-ttu-id="41e9f-130">Kontextové nabídky v částech analýzy, monitorování, segmentaci a řídicí panely Azure Mobile Engagement uživatelského rozhraní pro další pomoc v tématu a jak informace.</span><span class="sxs-lookup"><span data-stu-id="41e9f-130">See the context menus in the Analytics, Monitoring, Segmentation, and Dashboards sections of the Azure Mobile Engagement UI for more help and how to information.</span></span>

## <a name="crash-troubleshooting"></a><span data-ttu-id="41e9f-131">Řešení potíží s havárií</span><span class="sxs-lookup"><span data-stu-id="41e9f-131">Crash troubleshooting</span></span>
### <a name="issue"></a><span data-ttu-id="41e9f-132">Problém</span><span class="sxs-lookup"><span data-stu-id="41e9f-132">Issue</span></span>
* <span data-ttu-id="41e9f-133">Aplikace spadne uvedených v analýzy, monitorování nebo řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="41e9f-133">Application Crashes appearing in Analytics, Monitoring, or Dashboard.</span></span>

### <a name="causes"></a><span data-ttu-id="41e9f-134">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="41e9f-134">Causes</span></span>
* <span data-ttu-id="41e9f-135">Chcete-li vyřešit aplikace spadne zobrazená v analýzy, monitorování nebo řídicí panel nezapomeňte zaškrtnout známé problémy s předchozími verzemi sady SDK v poznámkách k verzi.</span><span class="sxs-lookup"><span data-stu-id="41e9f-135">To troubleshoot Application Crashes seen in Analytics, Monitoring, or Dashboard make sure to check the release notes for known issues with previous versions of the SDK.</span></span>
* <span data-ttu-id="41e9f-136">Chcete-li dále řešit havárie aplikací proveďte událost z testovací zařízení s instalaci aplikace a vyhledávání ID zařízení v části "Monitorování událostí –" rozhraní Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="41e9f-136">To further troubleshoot application crashes perform an event from a test device with your application installed and look up your device ID in the “Monitor – Events” section of the Azure Mobile Engagement UI.</span></span> <span data-ttu-id="41e9f-137">Pak proveďte událost, která je příčinou aplikace k chybě a vyhledejte další informace v části "Havárií – monitorování" rozhraní Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="41e9f-137">Then perform the event that is causing your application to crash and look up additional information in the “Monitor – Crash” section of the Azure Mobile Engagement UI.</span></span> 

