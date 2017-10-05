---
title: "Zkoumat HockeyApp data ve službě Azure Application Insights | Microsoft Docs"
description: "Analýza využití a výkonu vaší aplikace Azure pomocí Application Insights."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: bwren
ms.openlocfilehash: 450ca10613d137393090578619f3766734d1d493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a><span data-ttu-id="d3427-103">Zkoumat HockeyApp data ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="d3427-103">Exploring HockeyApp data in Application Insights</span></span>
<span data-ttu-id="d3427-104">[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) je doporučené platforma pro monitorování provozu desktop a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3427-104">[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) is the recommended platform for monitoring live desktop and mobile apps.</span></span> <span data-ttu-id="d3427-105">Z HockeyApp můžete odeslat vlastní a trasování telemetrii ke sledování využití a pomoct s diagnostikou (kromě získávání data o chybách).</span><span class="sxs-lookup"><span data-stu-id="d3427-105">From HockeyApp, you can send custom and trace telemetry to monitor usage and assist in diagnosis (in addition to getting crash data).</span></span> <span data-ttu-id="d3427-106">Tento datový proud telemetrie lze dotazovat pomocí výkonného [Analytics](app-insights-analytics.md) funkce [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3427-106">This stream of telemetry can be queried using the powerful [Analytics](app-insights-analytics.md) feature of [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="d3427-107">Kromě toho můžete [exportovat vlastní a trasování telemetrie](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="d3427-107">In addition, you can [export the custom and trace telemetry](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="d3427-108">Chcete-li tyto funkce povolit, nastavte mostu, který předává HockeyApp vlastních dat do služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d3427-108">To enable these features, you set up a bridge that relays HockeyApp custom data to Application Insights.</span></span>

## <a name="the-hockeyapp-bridge-app"></a><span data-ttu-id="d3427-109">HockeyApp most aplikace</span><span class="sxs-lookup"><span data-stu-id="d3427-109">The HockeyApp Bridge app</span></span>
<span data-ttu-id="d3427-110">Tato aplikace most HockeyApp je základní funkce, která umožňuje přístup k vašim HockeyApp vlastní a trasování telemetrii ve službě Application Insights prostřednictvím analýzy a průběžné Export funkcí.</span><span class="sxs-lookup"><span data-stu-id="d3427-110">The HockeyApp Bridge App is the core feature that enables you to access your HockeyApp custom and trace telemetry in Application Insights through the Analytics and Continuous Export features.</span></span> <span data-ttu-id="d3427-111">Vlastní a trasování událostí shromážděných za HockeyApp po vytvoření aplikace most HockeyApp budou přístupné z těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="d3427-111">Custom and trace events collected by HockeyApp after the creation of the HockeyApp Bridge App will be accessible from these features.</span></span> <span data-ttu-id="d3427-112">Podíváme se, jak nastavit některé z těchto aplikací most.</span><span class="sxs-lookup"><span data-stu-id="d3427-112">Let’s see how to set up one of these Bridge Apps.</span></span>

<span data-ttu-id="d3427-113">V HockeyApp, otevřete nastavení účtu [rozhraní API tokenů](https://rink.hockeyapp.net/manage/auth_tokens).</span><span class="sxs-lookup"><span data-stu-id="d3427-113">In HockeyApp, open Account Settings, [API Tokens](https://rink.hockeyapp.net/manage/auth_tokens).</span></span> <span data-ttu-id="d3427-114">Vytvořit nový token nebo znovu použít existující šablonu.</span><span class="sxs-lookup"><span data-stu-id="d3427-114">Either create a new token or reuse an existing one.</span></span> <span data-ttu-id="d3427-115">Minimální práva potřebná jsou "jen pro čtení".</span><span class="sxs-lookup"><span data-stu-id="d3427-115">The minimum rights required are "read only".</span></span> <span data-ttu-id="d3427-116">Trvat kopii rozhraní API tokenu.</span><span class="sxs-lookup"><span data-stu-id="d3427-116">Take a copy of the API token.</span></span>

![Získání tokenu rozhraní API HockeyApp](./media/app-insights-hockeyapp-bridge-app/01.png)

<span data-ttu-id="d3427-118">Otevřete portál Microsoft Azure a [vytvořte prostředek Application Insights](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="d3427-118">Open the Microsoft Azure portal and [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="d3427-119">Typ aplikace nastavte na "HockeyApp most aplikace":</span><span class="sxs-lookup"><span data-stu-id="d3427-119">Set Application Type to “HockeyApp bridge application”:</span></span>

![Nový prostředek Application Insights](./media/app-insights-hockeyapp-bridge-app/02.png)

<span data-ttu-id="d3427-121">Nemusíte nastavení názvu – to bude možné z názvu HockeyApp automaticky nastavit.</span><span class="sxs-lookup"><span data-stu-id="d3427-121">You don't need to set a name - this will automatically be set from the HockeyApp name.</span></span>

<span data-ttu-id="d3427-122">Zobrazí se pole most HockeyApp.</span><span class="sxs-lookup"><span data-stu-id="d3427-122">The HockeyApp bridge fields appear.</span></span> 

![Zadejte pole most](./media/app-insights-hockeyapp-bridge-app/03.png)

<span data-ttu-id="d3427-124">Zadejte HockeyApp token, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="d3427-124">Enter the HockeyApp token you noted earlier.</span></span> <span data-ttu-id="d3427-125">Tato akce vyplní "HockeyApp aplikace" rozevírací nabídky s vašimi aplikacemi HockeyApp.</span><span class="sxs-lookup"><span data-stu-id="d3427-125">This action populates the “HockeyApp Application” dropdown menu with all your HockeyApp applications.</span></span> <span data-ttu-id="d3427-126">Vyberte ten, který chcete použít a dokončete polí.</span><span class="sxs-lookup"><span data-stu-id="d3427-126">Select the one you want to use, and complete the remainder of the fields.</span></span> 

<span data-ttu-id="d3427-127">Otevřete nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="d3427-127">Open the new resource.</span></span> 

<span data-ttu-id="d3427-128">Všimněte si, že data trvá dobu spuštění toku.</span><span class="sxs-lookup"><span data-stu-id="d3427-128">Note that the data takes a while to start flowing.</span></span>

![Prostředek Application Insights čekání dat](./media/app-insights-hockeyapp-bridge-app/04.png)

<span data-ttu-id="d3427-130">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="d3427-130">That’s it!</span></span> <span data-ttu-id="d3427-131">Vlastní a trasování data shromážděná ve vaší aplikaci instrumentovány HockeyApp od tohoto okamžiku je nyní také k dispozici v analýzy a průběžné Export funkce Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d3427-131">Custom and trace data collected in your HockeyApp-instrumented app from this point forward is now also available to you in the Analytics and Continuous Export features of Application Insights.</span></span>

<span data-ttu-id="d3427-132">Stručně pojďme si shrnout každý z těchto funkcí, které jsou nyní k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d3427-132">Let’s briefly review each of these features now available to you.</span></span>

## <a name="analytics"></a><span data-ttu-id="d3427-133">Analýza</span><span class="sxs-lookup"><span data-stu-id="d3427-133">Analytics</span></span>
<span data-ttu-id="d3427-134">Analytics je výkonný nástroj pro zadávání dotazů ad-hoc vašich dat, který vám umožní diagnostikovat a analyzovat telemetrie a rychle zjistit hlavní příčiny a vzory.</span><span class="sxs-lookup"><span data-stu-id="d3427-134">Analytics is a powerful tool for ad-hoc querying of your data, allowing you to diagnose and analyze your telemetry and quickly discover root causes and patterns.</span></span>

![Analýza](./media/app-insights-hockeyapp-bridge-app/05.png)

* [<span data-ttu-id="d3427-136">Další informace o analýzy</span><span class="sxs-lookup"><span data-stu-id="d3427-136">Learn more about Analytics</span></span>](app-insights-analytics-tour.md)

## <a name="continuous-export"></a><span data-ttu-id="d3427-137">Průběžný export</span><span class="sxs-lookup"><span data-stu-id="d3427-137">Continuous export</span></span>
<span data-ttu-id="d3427-138">Průběžné Export umožňuje exportovat data do kontejner úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="d3427-138">Continuous Export allows you to export your data into an Azure Blob Storage container.</span></span> <span data-ttu-id="d3427-139">To je užitečné, pokud je potřeba uchovávat data po dobu delší než doba uchování aktuálně nabízená Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d3427-139">This is very useful if you need to keep your data for longer than the retention period currently offered by Application Insights.</span></span> <span data-ttu-id="d3427-140">Můžete ponechat data v úložišti objektů blob, zpracování do databáze SQL nebo vaše upřednostňované řešení datového skladu.</span><span class="sxs-lookup"><span data-stu-id="d3427-140">You can keep the data in blob storage, process it into a SQL Database, or your preferred data warehousing solution.</span></span>

[<span data-ttu-id="d3427-141">Další informace o průběžné Export</span><span class="sxs-lookup"><span data-stu-id="d3427-141">Learn more about Continuous Export</span></span>](app-insights-export-telemetry.md)

## <a name="next-steps"></a><span data-ttu-id="d3427-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3427-142">Next steps</span></span>
* [<span data-ttu-id="d3427-143">Pro data aplikace Analytics</span><span class="sxs-lookup"><span data-stu-id="d3427-143">Apply Analytics to your data</span></span>](app-insights-analytics-tour.md)

