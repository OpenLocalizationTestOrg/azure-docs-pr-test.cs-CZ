---
title: Telemetrie Application Insights ve Visual Studio CodeLens | Dokumentace Microsoftu
description: "Přistupujte rychle k požadavkům Application Insights a telemetrii výjimek pomocí CodeLens v sadě Visual Studio."
services: application-insights
documentationcenter: .net
author: numberbycolors
manager: carmonm
ms.assetid: 93559e44-23cb-4b9d-8425-60f7f0d0a82c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: 20933afab55043ccc5ce908c04c6e4a7a28e3538
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-telemetry-in-visual-studio-codelens"></a><span data-ttu-id="0f67b-103">Telemetrie Application Insights ve Visual Studio CodeLens</span><span class="sxs-lookup"><span data-stu-id="0f67b-103">Application Insights telemetry in Visual Studio CodeLens</span></span>
<span data-ttu-id="0f67b-104">Metody v kódu vaší webové aplikace mohou být opatřeny poznámkami s telemetrií o výjimkách za běhu a časech odezvy na požadavky.</span><span class="sxs-lookup"><span data-stu-id="0f67b-104">Methods in the code of your web app can be annotated with telemetry about run-time exceptions and request response times.</span></span> <span data-ttu-id="0f67b-105">Pokud ve své aplikaci nainstalujete [Azure Application Insights](app-insights-overview.md), telemetrie se zobrazí ve Visual Studio [CodeLens](https://msdn.microsoft.com/library/dn269218.aspx) s poznámkami v horní části každé funkce, kde jste zvyklí vídat užitečné informace, jako například počet míst, ze kterých se na funkci odkazuje, nebo jméno poslední osoby, která ji upravila.</span><span class="sxs-lookup"><span data-stu-id="0f67b-105">If you install [Azure Application Insights](app-insights-overview.md) in your application, the telemetry appears in Visual Studio [CodeLens](https://msdn.microsoft.com/library/dn269218.aspx) - the notes at the top of each function where you're used to seeing useful information such as the number of places the function is referenced or the last person who edited it.</span></span>

![CodeLens](./media/app-insights-visual-studio-codelens/codelens-overview.png)

> [!NOTE]
> <span data-ttu-id="0f67b-107">Application Insights v CodeLens je k dispozici v sadě Visual Studio 2015 Update 3 a novější nebo s nejnovější verzí [rozšíření Developer Analytics Tools](https://visualstudiogallery.msdn.microsoft.com/82367b81-3f97-4de1-bbf1-eaf52ddc635a).</span><span class="sxs-lookup"><span data-stu-id="0f67b-107">Application Insights in CodeLens is available in Visual Studio 2015 Update 3 and later, or with the latest version of [Developer Analytics Tools extension](https://visualstudiogallery.msdn.microsoft.com/82367b81-3f97-4de1-bbf1-eaf52ddc635a).</span></span> <span data-ttu-id="0f67b-108">CodeLens je k dispozici v edicích Enterprise a Professional sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0f67b-108">CodeLens is available in the Enterprise and Professional editions of Visual Studio.</span></span>
> 
> 

## <a name="where-to-find-application-insights-data"></a><span data-ttu-id="0f67b-109">Kde najít data Application Insights</span><span class="sxs-lookup"><span data-stu-id="0f67b-109">Where to find Application Insights data</span></span>
<span data-ttu-id="0f67b-110">V indikátorech CodeLens veřejných metod žádostí vaší webové aplikace vyhledejte telemetrii Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0f67b-110">Look for Application Insights telemetry in the CodeLens indicators of the public request methods of your web application.</span></span> <span data-ttu-id="0f67b-111">Indikátory CodeLens jsou uvedené nad metodami a dalšími deklaracemi v kódu C# a Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="0f67b-111">CodeLens indicators are shown above method and other declarations in C# and Visual Basic code.</span></span> <span data-ttu-id="0f67b-112">Pokud jsou pro metodu k dispozici data Application Insights, zobrazí se indikátory požadavků a výjimek, například „100 požadavků, 1 % neúspěšných“ nebo „10 výjimek“.</span><span class="sxs-lookup"><span data-stu-id="0f67b-112">If Application Insights data is available for a method, you'll see indicators for requests and exceptions such as "100 requests, 1% failed" or "10 exceptions."</span></span> <span data-ttu-id="0f67b-113">Další podrobnosti zobrazíte kliknutím na indikátor CodeLens.</span><span class="sxs-lookup"><span data-stu-id="0f67b-113">Click a CodeLens indicator for more details.</span></span> 

> [!TIP]
> <span data-ttu-id="0f67b-114">Načtení indikátorů požadavků a výjimek Application Insights může trvat o několik sekund déle než načtení dalších indikátorů CodeLens.</span><span class="sxs-lookup"><span data-stu-id="0f67b-114">Application Insights request and exception indicators may take a few extra seconds to load after other CodeLens indicators appear.</span></span>
> 
> 

## <a name="exceptions-in-codelens"></a><span data-ttu-id="0f67b-115">Výjimky v CodeLens</span><span class="sxs-lookup"><span data-stu-id="0f67b-115">Exceptions in CodeLens</span></span>
![Bude doplněno](./media/app-insights-visual-studio-codelens/codelens-exceptions.png)

<span data-ttu-id="0f67b-117">Indikátor výjimek CodeLens ukazuje počet výskytů výjimek pro 15 nejčastěji se vyskytujících výjimek ve vaší aplikaci v posledních 24 hodinách během zpracování požadavku, který metoda obsluhuje.</span><span class="sxs-lookup"><span data-stu-id="0f67b-117">The exception CodeLens indicator shows the number of exceptions that have occurred in the past 24 hours from the 15 most frequently occurring exceptions in your application during that period, while processing the request served by the method.</span></span>

<span data-ttu-id="0f67b-118">Chcete-li zobrazit podrobnosti, klikněte na indikátor výjimek CodeLens:</span><span class="sxs-lookup"><span data-stu-id="0f67b-118">To see more details, click the exceptions CodeLens indicator:</span></span>

* <span data-ttu-id="0f67b-119">Procentuální změna počtu výjimek z posledních 24 hodin vzhledem k předchozím 24 hodinám.</span><span class="sxs-lookup"><span data-stu-id="0f67b-119">The percentage change in number of exceptions from the most recent 24 hours relative to the prior 24 hours</span></span>
* <span data-ttu-id="0f67b-120">Chcete-li přejít do kódu k funkci, která vyvolává výjimku, zvolte **Přejít ke kódu**.</span><span class="sxs-lookup"><span data-stu-id="0f67b-120">Choose **Go to code** to navigate to the source code for the function throwing the exception</span></span>
* <span data-ttu-id="0f67b-121">Chcete-li se dotázat na všechny instance této výjimky, ke kterým došlo v posledních 24 hodinách, zvolte **Vyhledat**.</span><span class="sxs-lookup"><span data-stu-id="0f67b-121">Choose **Search** to query all instances of this exception that have occurred in the past 24 hours</span></span>
* <span data-ttu-id="0f67b-122">Chcete-li zobrazit vizualizaci trendu výskytu této výjimky v posledních 24 hodinách, zvolte **Trend**.</span><span class="sxs-lookup"><span data-stu-id="0f67b-122">Choose **Trend** to view a trend visualization for occurrences of this exception in the past 24 hours</span></span>
* <span data-ttu-id="0f67b-123">Chcete-li se dotázat na všechny výjimky, ke kterým došlo v posledních 24 hodinách, zvolte **Zobrazit všechny výjimky v této aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="0f67b-123">Choose **View all exceptions in this app** to query all exceptions that have occurred in the past 24 hours</span></span>
* <span data-ttu-id="0f67b-124">Chcete-li zobrazit vizualizaci trendu výskytu všech výjimek v posledních 24 hodinách, zvolte **Prozkoumat trendy výjimek**.</span><span class="sxs-lookup"><span data-stu-id="0f67b-124">Choose **Explore exception trends** to view a trend visualization for all exceptions that have occurred in the past 24 hours.</span></span> 

> [!TIP]
> <span data-ttu-id="0f67b-125">Pokud v CodeLens uvidíte „0 výjimek“, ale víte, že by tam měly být výjimky, zkontrolujte, že jste v CodeLens vybrali správný prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0f67b-125">If you see "0 exceptions" in CodeLens but you know there should be exceptions, check to make sure the right Application Insights resource is selected in CodeLens.</span></span> <span data-ttu-id="0f67b-126">Chcete-li vybrat jiný prostředek, klikněte pravým tlačítkem na svůj projekt v Průzkumníku řešení a zvolte **Application Insights > Zvolit zdroj telemetrie**.</span><span class="sxs-lookup"><span data-stu-id="0f67b-126">To select another resource, right-click on your project in the Solution Explorer and choose **Application Insights > Choose Telemetry Source**.</span></span> <span data-ttu-id="0f67b-127">CodeLens se zobrazuje pouze pro 15 nejčastěji se vyskytujících výjimek ve vaší aplikaci v posledních 24 hodinách, takže pokud nějaká výjimka není mezi 15 nejčastějšími, zobrazí se „0 výjimek“.</span><span class="sxs-lookup"><span data-stu-id="0f67b-127">CodeLens is only shown for the 15 most frequently occurring exceptions in your application in the past 24 hours, so if an exception is the 16th most frequently or less, you'll see "0 exceptions."</span></span> <span data-ttu-id="0f67b-128">Výjimky v zobrazeních ASP.NET se nemusí zobrazit v metodách kontroleru, které tato zobrazení vygenerovaly.</span><span class="sxs-lookup"><span data-stu-id="0f67b-128">Exceptions from ASP.NET views may not appear on the controller methods that generated those views.</span></span>
> 
> [!TIP]
> <span data-ttu-id="0f67b-129">Pokud v CodeLens uvidíte „?</span><span class="sxs-lookup"><span data-stu-id="0f67b-129">If you see "?</span></span> <span data-ttu-id="0f67b-130">výjimek“, je nutné přidružit účet Azure k sadě Visual Studio nebo je možné, že vypršely přihlašovací údaje vašeho účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="0f67b-130">exceptions" in CodeLens, you need to associate your Azure account with Visual Studio or your Azure account credential may have expired.</span></span> <span data-ttu-id="0f67b-131">V obou případech klikněte na „?</span><span class="sxs-lookup"><span data-stu-id="0f67b-131">In either case, click "?</span></span> <span data-ttu-id="0f67b-132">výjimek“, zvolte **Přidat účet...** a zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0f67b-132">exceptions" and choose **Add an account...** to enter your credentials.</span></span>
> 
> 

## <a name="requests-in-codelens"></a><span data-ttu-id="0f67b-133">Požadavky v CodeLens</span><span class="sxs-lookup"><span data-stu-id="0f67b-133">Requests in CodeLens</span></span>
![Bude doplněno](./media/app-insights-visual-studio-codelens/codelens-requests.png)

<span data-ttu-id="0f67b-135">Indikátor požadavků CodeLens ukazuje počet požadavků HTTP, které metoda obsloužila během posledních 24 hodin, a procentuální podíl neúspěšných požadavků.</span><span class="sxs-lookup"><span data-stu-id="0f67b-135">The request CodeLens indicator shows the number of HTTP requests that been serviced by a method in the past 24 hours, plus the percentage of those requests that failed.</span></span>

<span data-ttu-id="0f67b-136">Chcete-li zobrazit podrobnosti, klikněte na indikátor požadavků CodeLens:</span><span class="sxs-lookup"><span data-stu-id="0f67b-136">To see more details, click the requests CodeLens indicator:</span></span>

* <span data-ttu-id="0f67b-137">Absolutní a procentuální změny počtu požadavků, neúspěšných požadavků a průměrná doba odezvy během posledních 24 hodin ve srovnání s předchozími 24 hodinami.</span><span class="sxs-lookup"><span data-stu-id="0f67b-137">The absolute and percentage changes in number of requests, failed requests, and average response times over the past 24 hours compared to the prior 24 hours</span></span>
* <span data-ttu-id="0f67b-138">Spolehlivost metody, vypočítaná jako procento úspěšných požadavků v posledních 24 hodinách.</span><span class="sxs-lookup"><span data-stu-id="0f67b-138">The reliability of the method, calculated as the percentage of requests that did not fail in the past 24 hours</span></span>
* <span data-ttu-id="0f67b-139">Chcete-li se dotázat na všechny požadavky nebo pouze na neúspěšné požadavky, ke kterým došlo v posledních 24 hodinách, zvolte **Vyhledat** (neúspěšné) požadavky.</span><span class="sxs-lookup"><span data-stu-id="0f67b-139">Choose **Search** for requests or failed requests to query all the (failed) requests that occurred in the past 24 hours</span></span>
* <span data-ttu-id="0f67b-140">Chcete-li zobrazit vizualizaci trendu požadavků, neúspěšných požadavků nebo průměrné doby odezvy v posledních 24 hodinách, zvolte **Trend**.</span><span class="sxs-lookup"><span data-stu-id="0f67b-140">Choose **Trend** to view a trend visualization for requests, failed requests, or average response times in the past 24 hours.</span></span>
* <span data-ttu-id="0f67b-141">Chcete-li změnit, který prostředek je zdrojem dat pro CodeLens, v levém horním rohu zobrazení podrobností CodeLens zvolte název prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0f67b-141">Choose the name of the Application Insights resource in the upper left corner of the CodeLens details view to change which resource is the source for CodeLens data.</span></span>

## <span data-ttu-id="0f67b-142"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f67b-142"><a name="next"></a>Next steps</span></span>
|  |  |
| --- | --- |
| <span data-ttu-id="0f67b-143">**[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md)**</span><span class="sxs-lookup"><span data-stu-id="0f67b-143">**[Working with Application Insights in Visual Studio](app-insights-visual-studio.md)**</span></span><br/><span data-ttu-id="0f67b-144">Hledejte telemetrii, zobrazujte data v CodeLens a konfigurujte Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0f67b-144">Search telemetry, see data in CodeLens, and configure Application Insights.</span></span> <span data-ttu-id="0f67b-145">Vše v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0f67b-145">All within Visual Studio.</span></span> |![Klikněte pravým tlačítkem myši na projekt a vyberte Application Insights, Vyhledávání](./media/app-insights-visual-studio-codelens/34.png) |
| <span data-ttu-id="0f67b-147">**[Přidání dalších dat](app-insights-asp-net-more.md)**</span><span class="sxs-lookup"><span data-stu-id="0f67b-147">**[Add more data](app-insights-asp-net-more.md)**</span></span><br/><span data-ttu-id="0f67b-148">Sledování využití, dostupnosti, závislostí, výjimek.</span><span class="sxs-lookup"><span data-stu-id="0f67b-148">Monitor usage, availability, dependencies, exceptions.</span></span> <span data-ttu-id="0f67b-149">Integrujte trasování z rozhraní protokolování.</span><span class="sxs-lookup"><span data-stu-id="0f67b-149">Integrate traces from logging frameworks.</span></span> <span data-ttu-id="0f67b-150">Zapisuje vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="0f67b-150">Write custom telemetry.</span></span> |![Visual Studio](./media/app-insights-visual-studio-codelens/64.png) |
| <span data-ttu-id="0f67b-152">**[Práce s portálem Application Insights](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="0f67b-152">**[Working with the Application Insights portal](app-insights-dashboards.md)**</span></span><br/><span data-ttu-id="0f67b-153">Řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a export telemetrie.</span><span class="sxs-lookup"><span data-stu-id="0f67b-153">Dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and telemetry export.</span></span> |![Visual Studio](./media/app-insights-visual-studio-codelens/62.png) |

