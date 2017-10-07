---
title: "aaaAzure Mobile Engagement Průvodce odstraňováním potíží - Analytics"
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
ms.openlocfilehash: 69c6ff8f5c8540f8ba8b85b9ffec55acc59329fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a><span data-ttu-id="a0f5a-103">Průvodce řešením potíží pro problémy analýzy, monitorování, segmentaci a řídicí panel</span><span class="sxs-lookup"><span data-stu-id="a0f5a-103">Troubleshooting guide for Analytics, Monitoring, Segmentation, and Dashboard issues</span></span>
<span data-ttu-id="a0f5a-104">Hello následují možných problémů se můžete setkat s jak Azure Mobile Engagement shromažďuje informace o aplikacích, zařízení a uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-104">hello following are possible issues you may encounter with how Azure Mobile Engagement gathers information about your applications, devices, and users.</span></span>

## <a name="missingdelayed-information"></a><span data-ttu-id="a0f5a-105">Chybějící nebo opožděná informace</span><span class="sxs-lookup"><span data-stu-id="a0f5a-105">Missing/Delayed information</span></span>
### <a name="issue"></a><span data-ttu-id="a0f5a-106">Problém</span><span class="sxs-lookup"><span data-stu-id="a0f5a-106">Issue</span></span>
* <span data-ttu-id="a0f5a-107">V uvedených v analýzy, segmentace nebo řídicí panel je zpožděno informace.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-107">Information is delayed in appearing in Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="a0f5a-108">Chybí informace z monitorování.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-108">Information is missing from Monitoring.</span></span>
* <span data-ttu-id="a0f5a-109">Chybí informace z analýzy, segmentace nebo řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-109">Information is missing from Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="a0f5a-110">Nedosáhli limitů segmentace.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-110">Hitting segmentation limits.</span></span>

### <a name="causes"></a><span data-ttu-id="a0f5a-111">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="a0f5a-111">Causes</span></span>
* <span data-ttu-id="a0f5a-112">Můžete použít hello Analytics rozhraní API, rozhraní API monitorování, a rozhraní API segmenty toosee Pokud žádná data jste chybí hello uživatelského rozhraní je viditelná prostřednictvím hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-112">You can use hello Analytics API, Monitor API, and Segments API toosee if any data you are missing from hello UI is visible through hello APIs.</span></span>
* <span data-ttu-id="a0f5a-113">Pokud hello Azure Mobile Engagement SDK není správně integrovaná do aplikace nebudete moct toosee informace v hello Analytics segmentace, monitorování nebo řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-113">If hello Azure Mobile Engagement SDK is not correctly integrated into your app then you won't be able toosee information in hello Analytics, Segmentation, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="a0f5a-114">Segmenty nelze změnit po jejich vytvoření, segmenty může být pouze "klonovaný" (zkopírovat) nebo "zničen" (odstranit).</span><span class="sxs-lookup"><span data-stu-id="a0f5a-114">Segments can't be changed once they are created, segments can only be "cloned" (copied) or "destroyed" (deleted).</span></span> <span data-ttu-id="a0f5a-115">Segmenty může obsahovat jenom 10 kritéria.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-115">Segments can only contain 10 criteria.</span></span>
* <span data-ttu-id="a0f5a-116">Hello nejlepší způsob, jak tootest chybějící informace z monitorování je toosetup testovací zařízení, odinstalujte a znovu nainstalujte aplikaci hello na hello testovací zařízení.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-116">hello best way tootest missing information from monitoring is toosetup a test device, uninstall and/or reinstall hello app on hello test device.</span></span>
* <span data-ttu-id="a0f5a-117">Informace se aktualizují každých 24 hodin pro analýzy, segmentace nebo řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-117">Information is refreshed every 24 hours for Analytics, Segmentation, or Dashboards.</span></span>
* <span data-ttu-id="a0f5a-118">Informace v nové segmenty, se nemusí zobrazit do 24 hodin po jejich vytvoření i v případě hello segment je založena na předchozí informace.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-118">Information in new segments may not be displayed until 24 hours after they are created even if hello segment is based on previous information.</span></span>
* <span data-ttu-id="a0f5a-119">Filtrování dat analýzy v hello uživatelského rozhraní se zobrazí všechny příklady tohoto typu bez ohledu na verzi hello vaší aplikace (např.) "Dojde k chybě" filtrovat podle názvu se zobrazí z verze 1 a 2 verzi aplikace).</span><span class="sxs-lookup"><span data-stu-id="a0f5a-119">Filtering your analytics data in hello UI will show all examples of this type regardless of hello version of your app (e.g. "Crashes" filtered by name will show from version 1 and version 2 of your app).</span></span>
* <span data-ttu-id="a0f5a-120">Hello časové období pro analýzy je založena na hello datum z hello uživatelských nastavení zařízení, takže uživatel, jehož telefon je správně nastaveno datum hello může zobrazí v hello nesprávný časové období.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-120">hello time period for Analytics is based on hello date from hello users' device settings, so a user whose phone has hello date incorrectly set could show up in hello wrong time period.</span></span>
* <span data-ttu-id="a0f5a-121">Žádné na straně serveru, který data se protokolují, když použijete tlačítko hello příliš "test" nabízených oznámení, data se protokolují pouze pro skutečné nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-121">No server side data is logged when you use hello button too"test" pushes, data is only logged for real push campaigns.</span></span>

## <a name="cant-locate-items-in-ui"></a><span data-ttu-id="a0f5a-122">Nelze najít položky v uživatelském rozhraní</span><span class="sxs-lookup"><span data-stu-id="a0f5a-122">Can't locate items in UI</span></span>
### <a name="issue"></a><span data-ttu-id="a0f5a-123">Problém</span><span class="sxs-lookup"><span data-stu-id="a0f5a-123">Issue</span></span>
* <span data-ttu-id="a0f5a-124">Nelze vytvořit segmenty založené na určité součástí nebo informace o aplikaci vlastní značky kritéria.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-124">Can't create segments based on certain built in or custom app info tag criteria.</span></span>
* <span data-ttu-id="a0f5a-125">Nelze najít určité součástí nebo informace o aplikaci vlastní značky kritéria analýzy, monitorování nebo řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-125">Can't find certain built in or custom app info tag criteria in Analytics, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="a0f5a-126">Nemůže interpretovat data hello analýzy, monitorování, segmentace nebo řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-126">Can't interpret hello data in Analytics, Monitoring, Segmentation, or Dashboards.</span></span>

### <a name="causes"></a><span data-ttu-id="a0f5a-127">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="a0f5a-127">Causes</span></span>
* <span data-ttu-id="a0f5a-128">Některé položky součástí a informace o aplikaci značky jsou dostupné jen jako nabízené kritéria, ale nemusí být přidány tooa segmentu nebo viditelné z analýzy, monitorování nebo řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-128">Some built in items and app info tags are only available as push criteria but may not be added tooa segment or visible from Analytics, Monitoring, or Dashboard.</span></span> 
* <span data-ttu-id="a0f5a-129">Pro integrovaný položky a informace o aplikaci značky, které nelze přidat tooa segmentu, budete potřebovat toosetup seznamu cílových kritérií pro každý hello tooperform kampaň stejné funkce jako cílení na základě v segmentu.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-129">For built in items and app info tags that can't be added tooa segment, you will need toosetup list of targeting criteria in each campaign tooperform hello same function as targeting based on a segment.</span></span>
* <span data-ttu-id="a0f5a-130">V tématu hello kontextové nabídky v hello analýzy, monitorování, segmentaci a řídicí panely částech hello Azure Mobile Engagement uživatelského rozhraní pro další pomoc a jak tooinformation.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-130">See hello context menus in hello Analytics, Monitoring, Segmentation, and Dashboards sections of hello Azure Mobile Engagement UI for more help and how tooinformation.</span></span>

## <a name="crash-troubleshooting"></a><span data-ttu-id="a0f5a-131">Řešení potíží s havárií</span><span class="sxs-lookup"><span data-stu-id="a0f5a-131">Crash troubleshooting</span></span>
### <a name="issue"></a><span data-ttu-id="a0f5a-132">Problém</span><span class="sxs-lookup"><span data-stu-id="a0f5a-132">Issue</span></span>
* <span data-ttu-id="a0f5a-133">Aplikace spadne uvedených v analýzy, monitorování nebo řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-133">Application Crashes appearing in Analytics, Monitoring, or Dashboard.</span></span>

### <a name="causes"></a><span data-ttu-id="a0f5a-134">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="a0f5a-134">Causes</span></span>
* <span data-ttu-id="a0f5a-135">tootroubleshoot aplikace spadne zobrazená v analýzy, monitorování nebo řídicího panelu zkontrolujte, zda toocheck hello poznámky k verzi pro známé problémy s předchozími verzemi hello SDK.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-135">tootroubleshoot Application Crashes seen in Analytics, Monitoring, or Dashboard make sure toocheck hello release notes for known issues with previous versions of hello SDK.</span></span>
* <span data-ttu-id="a0f5a-136">řešení potíží s toofurther aplikace dojde k chybě provádění událost z testovací zařízení s instalaci aplikace a vyhledejte ID zařízení v části hello "monitorování – události" hello Azure Mobile Engagement uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-136">toofurther troubleshoot application crashes perform an event from a test device with your application installed and look up your device ID in hello “Monitor – Events” section of hello Azure Mobile Engagement UI.</span></span> <span data-ttu-id="a0f5a-137">Potom proveďte hello událost, která je příčinou toocrash vaší aplikace a vyhledejte další informace v hello "Havárií – monitorování" části hello Azure Mobile Engagement uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a0f5a-137">Then perform hello event that is causing your application toocrash and look up additional information in hello “Monitor – Crash” section of hello Azure Mobile Engagement UI.</span></span> 

