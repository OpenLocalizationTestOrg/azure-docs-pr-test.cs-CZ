---
title: "aaaExploring HockeyApp dat ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: ed7cf99b48f5ec78d6b401bb954cfcd014b9d1f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a><span data-ttu-id="d8ed5-103">Zkoumat HockeyApp data ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="d8ed5-103">Exploring HockeyApp data in Application Insights</span></span>
<span data-ttu-id="d8ed5-104">[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) doporučuje hello platforma pro monitorování provozu desktop a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-104">[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) is hello recommended platform for monitoring live desktop and mobile apps.</span></span> <span data-ttu-id="d8ed5-105">Z HockeyApp můžete odeslat vlastní a sledování využití telemetrie toomonitor a pomoct s diagnostikou (v přidání data o chybách toogetting).</span><span class="sxs-lookup"><span data-stu-id="d8ed5-105">From HockeyApp, you can send custom and trace telemetry toomonitor usage and assist in diagnosis (in addition toogetting crash data).</span></span> <span data-ttu-id="d8ed5-106">Tento datový proud telemetrie lze dotazovat pomocí hello výkonné [Analytics](app-insights-analytics.md) funkce [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d8ed5-106">This stream of telemetry can be queried using hello powerful [Analytics](app-insights-analytics.md) feature of [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="d8ed5-107">Kromě toho můžete [exportovat vlastní hello a trasování telemetrie](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="d8ed5-107">In addition, you can [export hello custom and trace telemetry](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="d8ed5-108">tooenable tyto funkce, musíte nastavit mostu, který předává HockeyApp vlastních dat tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-108">tooenable these features, you set up a bridge that relays HockeyApp custom data tooApplication Insights.</span></span>

## <a name="hello-hockeyapp-bridge-app"></a><span data-ttu-id="d8ed5-109">aplikace Hello most HockeyApp</span><span class="sxs-lookup"><span data-stu-id="d8ed5-109">hello HockeyApp Bridge app</span></span>
<span data-ttu-id="d8ed5-110">Hello HockeyApp most aplikace je hello základní funkce, která umožňuje tooaccess HockeyApp vlastní a trasování telemetrii ve službě Application Insights prostřednictvím hello analýzy a průběžné Export funkcí.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-110">hello HockeyApp Bridge App is hello core feature that enables you tooaccess your HockeyApp custom and trace telemetry in Application Insights through hello Analytics and Continuous Export features.</span></span> <span data-ttu-id="d8ed5-111">Vlastní a trasování událostí shromážděných za HockeyApp po vytvoření hello hello HockeyApp most aplikace budou přístupné z těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-111">Custom and trace events collected by HockeyApp after hello creation of hello HockeyApp Bridge App will be accessible from these features.</span></span> <span data-ttu-id="d8ed5-112">Podívejme se, jak tooset si některé z těchto aplikací most.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-112">Let’s see how tooset up one of these Bridge Apps.</span></span>

<span data-ttu-id="d8ed5-113">V HockeyApp, otevřete nastavení účtu [rozhraní API tokenů](https://rink.hockeyapp.net/manage/auth_tokens).</span><span class="sxs-lookup"><span data-stu-id="d8ed5-113">In HockeyApp, open Account Settings, [API Tokens](https://rink.hockeyapp.net/manage/auth_tokens).</span></span> <span data-ttu-id="d8ed5-114">Vytvořit nový token nebo znovu použít existující šablonu.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-114">Either create a new token or reuse an existing one.</span></span> <span data-ttu-id="d8ed5-115">minimální oprávnění Hello vyžaduje, jsou "jen pro čtení".</span><span class="sxs-lookup"><span data-stu-id="d8ed5-115">hello minimum rights required are "read only".</span></span> <span data-ttu-id="d8ed5-116">Trvat kopii hello API tokenu.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-116">Take a copy of hello API token.</span></span>

![Získání tokenu rozhraní API HockeyApp](./media/app-insights-hockeyapp-bridge-app/01.png)

<span data-ttu-id="d8ed5-118">Otevřete hello portálu Microsoft Azure a [vytvořte prostředek Application Insights](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="d8ed5-118">Open hello Microsoft Azure portal and [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="d8ed5-119">Nastavte typ aplikace příliš "HockeyApp most aplikace":</span><span class="sxs-lookup"><span data-stu-id="d8ed5-119">Set Application Type too“HockeyApp bridge application”:</span></span>

![Nový prostředek Application Insights](./media/app-insights-hockeyapp-bridge-app/02.png)

<span data-ttu-id="d8ed5-121">Nepotřebujete tooset název – automaticky nastaví ho z názvu HockeyApp hello.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-121">You don't need tooset a name - this will automatically be set from hello HockeyApp name.</span></span>

<span data-ttu-id="d8ed5-122">Hello HockeyApp most pole se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-122">hello HockeyApp bridge fields appear.</span></span> 

![Zadejte pole most](./media/app-insights-hockeyapp-bridge-app/03.png)

<span data-ttu-id="d8ed5-124">Zadejte hello HockeyApp token, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-124">Enter hello HockeyApp token you noted earlier.</span></span> <span data-ttu-id="d8ed5-125">Tato akce vyplní hello "HockeyApp aplikace" rozevírací nabídku s vašimi aplikacemi HockeyApp.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-125">This action populates hello “HockeyApp Application” dropdown menu with all your HockeyApp applications.</span></span> <span data-ttu-id="d8ed5-126">Vyberte hello jeden chcete toouse a dokončení hello zbytek hello pole.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-126">Select hello one you want toouse, and complete hello remainder of hello fields.</span></span> 

<span data-ttu-id="d8ed5-127">Otevřete nový prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-127">Open hello new resource.</span></span> 

<span data-ttu-id="d8ed5-128">Všimněte si, že jeho zpracování chvíli trvá hello dat předávaných toostart.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-128">Note that hello data takes a while toostart flowing.</span></span>

![Prostředek Application Insights čekání dat](./media/app-insights-hockeyapp-bridge-app/04.png)

<span data-ttu-id="d8ed5-130">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="d8ed5-130">That’s it!</span></span> <span data-ttu-id="d8ed5-131">Vlastní a trasování data shromážděná ve vaší aplikaci instrumentovány HockeyApp od tohoto okamžiku je nyní také k dispozici tooyou v hello analýzy a průběžné exportovat funkce Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-131">Custom and trace data collected in your HockeyApp-instrumented app from this point forward is now also available tooyou in hello Analytics and Continuous Export features of Application Insights.</span></span>

<span data-ttu-id="d8ed5-132">Stručně pojďme si shrnout každý z těchto funkcí tooyou nyní k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-132">Let’s briefly review each of these features now available tooyou.</span></span>

## <a name="analytics"></a><span data-ttu-id="d8ed5-133">Analýza</span><span class="sxs-lookup"><span data-stu-id="d8ed5-133">Analytics</span></span>
<span data-ttu-id="d8ed5-134">Analýza je výkonný nástroj pro ad-hoc dotazování dat, což vám toodiagnose a analyzovat telemetrie a rychle zjistit hlavní příčiny a vzory.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-134">Analytics is a powerful tool for ad-hoc querying of your data, allowing you toodiagnose and analyze your telemetry and quickly discover root causes and patterns.</span></span>

![Analýza](./media/app-insights-hockeyapp-bridge-app/05.png)

* [<span data-ttu-id="d8ed5-136">Další informace o analýzy</span><span class="sxs-lookup"><span data-stu-id="d8ed5-136">Learn more about Analytics</span></span>](app-insights-analytics-tour.md)

## <a name="continuous-export"></a><span data-ttu-id="d8ed5-137">Průběžný export</span><span class="sxs-lookup"><span data-stu-id="d8ed5-137">Continuous export</span></span>
<span data-ttu-id="d8ed5-138">Průběžné Export vám umožní tooexport dat do kontejner úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-138">Continuous Export allows you tooexport your data into an Azure Blob Storage container.</span></span> <span data-ttu-id="d8ed5-139">To je užitečné, pokud potřebujete mít tookeep data po dobu delší než doba uchování hello aktuálně nabízená Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-139">This is very useful if you need tookeep your data for longer than hello retention period currently offered by Application Insights.</span></span> <span data-ttu-id="d8ed5-140">Můžete ponechat hello data v úložišti objektů blob, zpracování do databáze SQL nebo vaše upřednostňované řešení datového skladu.</span><span class="sxs-lookup"><span data-stu-id="d8ed5-140">You can keep hello data in blob storage, process it into a SQL Database, or your preferred data warehousing solution.</span></span>

[<span data-ttu-id="d8ed5-141">Další informace o průběžné Export</span><span class="sxs-lookup"><span data-stu-id="d8ed5-141">Learn more about Continuous Export</span></span>](app-insights-export-telemetry.md)

## <a name="next-steps"></a><span data-ttu-id="d8ed5-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8ed5-142">Next steps</span></span>
* [<span data-ttu-id="d8ed5-143">Použít Analytics tooyour data</span><span class="sxs-lookup"><span data-stu-id="d8ed5-143">Apply Analytics tooyour data</span></span>](app-insights-analytics-tour.md)

