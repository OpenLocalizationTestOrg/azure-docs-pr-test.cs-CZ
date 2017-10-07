---
title: "aaaCreate nový prostředek Azure Application Insights | Microsoft Docs"
description: "Ručně nastavte Application Insights monitorování nové aplikace za provozu."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: bwren
ms.openlocfilehash: 3aba7045e1f8fe43d473f0be01dd52106ab992a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-insights-resource"></a><span data-ttu-id="0416e-103">Vytvořte prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="0416e-103">Create an Application Insights resource</span></span>
<span data-ttu-id="0416e-104">Azure Application Insights zobrazí data o vaší aplikaci v Microsoft Azure *prostředků*.</span><span class="sxs-lookup"><span data-stu-id="0416e-104">Azure Application Insights displays data about your application in a Microsoft Azure *resource*.</span></span> <span data-ttu-id="0416e-105">Vytvoření nového prostředku, proto je součástí [nastavení Application Insights toomonitor novou aplikaci][start].</span><span class="sxs-lookup"><span data-stu-id="0416e-105">Creating a new resource is therefore part of [setting up Application Insights toomonitor a new application][start].</span></span> <span data-ttu-id="0416e-106">V mnoha případech vytvoření prostředku můžete provést automaticky hello IDE.</span><span class="sxs-lookup"><span data-stu-id="0416e-106">In many cases, creating a resource can be done automatically by hello IDE.</span></span> <span data-ttu-id="0416e-107">Ale v některých případech můžete ručně vytvořit prostředek – například toohave samostatné prostředky pro vývoj a produkci sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="0416e-107">But in some cases, you create a resource manually - for example, toohave separate resources for development and production builds of your application.</span></span>

<span data-ttu-id="0416e-108">Po vytvoření hello prostředků, můžete získat svůj klíč instrumentace a používat tento hello tooconfigure SDK v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="0416e-108">After you have created hello resource, you get its instrumentation key and use that tooconfigure hello SDK in hello application.</span></span> <span data-ttu-id="0416e-109">klíče odkazy na zdroje Hello hello telemetrie toohello prostředků.</span><span class="sxs-lookup"><span data-stu-id="0416e-109">hello resource key links hello telemetry toohello resource.</span></span>

## <a name="sign-up-toomicrosoft-azure"></a><span data-ttu-id="0416e-110">Zaregistrujte si tooMicrosoft Azure</span><span class="sxs-lookup"><span data-stu-id="0416e-110">Sign up tooMicrosoft Azure</span></span>
<span data-ttu-id="0416e-111">Pokud jste to ještě získali [Microsoft account, pořiďte si ho](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="0416e-111">If you haven't got a [Microsoft account, get one now](http://live.com).</span></span> <span data-ttu-id="0416e-112">(Pokud používáte služby jako Outlook.com, OneDrive, Windows Phone nebo XBox Live, už máte účet Microsoft.)</span><span class="sxs-lookup"><span data-stu-id="0416e-112">(If you use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, you already have a Microsoft account.)</span></span>

<span data-ttu-id="0416e-113">Musíte taky předplatné příliš[Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="0416e-113">You also need a subscription too[Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="0416e-114">Pokud váš tým nebo společnost má předplatné Azure, vlastník hello vás může přidat tooit, pomocí účtu Windows Live ID.</span><span class="sxs-lookup"><span data-stu-id="0416e-114">If your team or organization has an Azure subscription, hello owner can add you tooit, using your Windows Live ID.</span></span> <span data-ttu-id="0416e-115">Se účtují poplatky se používá.</span><span class="sxs-lookup"><span data-stu-id="0416e-115">You're only charged for what you use.</span></span> <span data-ttu-id="0416e-116">Základní plán Hello výchozí umožňuje určité množství experimentální použití zdarma.</span><span class="sxs-lookup"><span data-stu-id="0416e-116">hello default basic plan allows for a certain amount of experimental use free of charge.</span></span>

<span data-ttu-id="0416e-117">Pokud máte k dispozici přístup tooa předplatného, přihlaste se tooApplication Insights na [http://portal.azure.com](https://portal.azure.com)a používat vaše toologin Live ID.</span><span class="sxs-lookup"><span data-stu-id="0416e-117">When you've got access tooa subscription, log in tooApplication Insights at [http://portal.azure.com](https://portal.azure.com), and use your Live ID toologin.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="0416e-118">Vytvořte prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="0416e-118">Create an Application Insights resource</span></span>
<span data-ttu-id="0416e-119">V hello [portal.azure.com](https://portal.azure.com), přidejte prostředek Application Insights:</span><span class="sxs-lookup"><span data-stu-id="0416e-119">In hello [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-create-new-resource/01-new.png)

* <span data-ttu-id="0416e-121">**Typ aplikace** ovlivňuje, co se zobrazí v okně Přehled hello a hello vlastnosti, které jsou k dispozici v [metriky explorer][metrics].</span><span class="sxs-lookup"><span data-stu-id="0416e-121">**Application type** affects what you see on hello overview blade and hello properties available in [metric explorer][metrics].</span></span> <span data-ttu-id="0416e-122">Pokud nevidíte vašeho typu aplikace, vyberte Obecné.</span><span class="sxs-lookup"><span data-stu-id="0416e-122">If you don't see your type of app, choose General.</span></span>
* <span data-ttu-id="0416e-123">**Předplatné** je váš účet platebních v Azure.</span><span class="sxs-lookup"><span data-stu-id="0416e-123">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="0416e-124">**Skupina prostředků** je užitečný pro správu vlastnosti jako řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="0416e-124">**Resource group** is a convenience for managing properties like access control.</span></span> <span data-ttu-id="0416e-125">Pokud jste již vytvořili ostatní prostředky služby Azure, můžete vybrat tooput tento nový prostředek v hello stejné skupiny.</span><span class="sxs-lookup"><span data-stu-id="0416e-125">If you have already created other Azure resources, you can choose tooput this new resource in hello same group.</span></span>
* <span data-ttu-id="0416e-126">**Umístění** je, kde společnost Microsoft uchovávat data.</span><span class="sxs-lookup"><span data-stu-id="0416e-126">**Location** is where we keep your data.</span></span>
* <span data-ttu-id="0416e-127">**PIN kód toodashboard** vloží dlaždici rychlý přístup pro prostředek na Azure domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="0416e-127">**Pin toodashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> <span data-ttu-id="0416e-128">Nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="0416e-128">Recommended.</span></span>

<span data-ttu-id="0416e-129">Po vytvoření aplikace, otevře se nové okno.</span><span class="sxs-lookup"><span data-stu-id="0416e-129">When your app has been created, a new blade opens.</span></span> <span data-ttu-id="0416e-130">Toto okno je, kde uvidíte data o využití a výkonu o vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0416e-130">This blade is where you see performance and usage data about your app.</span></span> 

<span data-ttu-id="0416e-131">tooget back tooit při příštím přihlášení tooAzure, podívejte se na dlaždici úvodní vaší aplikace na hello spustit Tabule (domovskou obrazovku).</span><span class="sxs-lookup"><span data-stu-id="0416e-131">tooget back tooit next time you log in tooAzure, look for your app's quick-start tile on hello start board (home screen).</span></span> <span data-ttu-id="0416e-132">Nebo klikněte na tlačítko Procházet toofind ho.</span><span class="sxs-lookup"><span data-stu-id="0416e-132">Or click Browse toofind it.</span></span>

## <a name="copy-hello-instrumentation-key"></a><span data-ttu-id="0416e-133">Zkopírovat klíč instrumentace hello</span><span class="sxs-lookup"><span data-stu-id="0416e-133">Copy hello instrumentation key</span></span>
<span data-ttu-id="0416e-134">klíč instrumentace Hello identifikuje hello prostředek, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0416e-134">hello instrumentation key identifies hello resource that you created.</span></span> <span data-ttu-id="0416e-135">Budete ho potřebovat toogive toohello SDK.</span><span class="sxs-lookup"><span data-stu-id="0416e-135">You need it toogive toohello SDK.</span></span>

![Klikněte na tlačítko Essentials, klikněte na tlačítko hello klíč instrumentace, CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-hello-sdk-in-your-app"></a><span data-ttu-id="0416e-137">Nainstalujte hello SDK v aplikaci</span><span class="sxs-lookup"><span data-stu-id="0416e-137">Install hello SDK in your app</span></span>
<span data-ttu-id="0416e-138">Nainstalujte hello Application Insights SDK ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0416e-138">Install hello Application Insights SDK in your app.</span></span> <span data-ttu-id="0416e-139">Tento krok výraznou závisí na typu hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0416e-139">This step depends heavily on hello type of your application.</span></span> 

<span data-ttu-id="0416e-140">Použití klíče tooconfigure hello instrumentace [hello SDK, který nainstalujete v aplikaci][start].</span><span class="sxs-lookup"><span data-stu-id="0416e-140">Use hello instrumentation key tooconfigure [hello SDK that you install in your application][start].</span></span>

<span data-ttu-id="0416e-141">Hello SDK zahrnuje standardní moduly, které odesílat telemetrická data bez nutnosti toowrite žádný kód.</span><span class="sxs-lookup"><span data-stu-id="0416e-141">hello SDK includes standard modules that send telemetry without you having toowrite any code.</span></span> <span data-ttu-id="0416e-142">akce uživatelů tootrack nebo diagnostikovat problémy podrobněji, [pomocí rozhraní API hello] [ api] toosend vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="0416e-142">tootrack user actions or diagnose issues in more detail, [use hello API][api] toosend your own telemetry.</span></span>

## <span data-ttu-id="0416e-143"><a name="monitor"></a>Zobrazit data telemetrie</span><span class="sxs-lookup"><span data-stu-id="0416e-143"><a name="monitor"></a>See telemetry data</span></span>
<span data-ttu-id="0416e-144">Zavřít hello rychlé spuštění okna okno tooreturn tooyour aplikací v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0416e-144">Close hello quick start blade tooreturn tooyour application blade in hello Azure portal.</span></span>

<span data-ttu-id="0416e-145">Klikněte na tlačítko hello Search dlaždice toosee [diagnostické vyhledávání][diagnostic], kde se zobrazí události první hello.</span><span class="sxs-lookup"><span data-stu-id="0416e-145">Click hello Search tile toosee [Diagnostic Search][diagnostic], where hello first events appear.</span></span> 

<span data-ttu-id="0416e-146">Pokud očekáváte více dat, klikněte na tlačítko **aktualizovat** za několik sekund.</span><span class="sxs-lookup"><span data-stu-id="0416e-146">If you're expecting more data, click **Refresh** after a few seconds  .</span></span>

## <a name="creating-a-resource-automatically"></a><span data-ttu-id="0416e-147">Vytvoření prostředku automaticky</span><span class="sxs-lookup"><span data-stu-id="0416e-147">Creating a resource automatically</span></span>
<span data-ttu-id="0416e-148">Můžete napsat [skript prostředí PowerShell](app-insights-powershell.md) toocreate prostředek automaticky.</span><span class="sxs-lookup"><span data-stu-id="0416e-148">You can write a [PowerShell script](app-insights-powershell.md) toocreate a resource automatically.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0416e-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0416e-149">Next steps</span></span>
* [<span data-ttu-id="0416e-150">Vytvoření řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="0416e-150">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="0416e-151">Diagnostické vyhledávání</span><span class="sxs-lookup"><span data-stu-id="0416e-151">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="0416e-152">Zkoumání metrik</span><span class="sxs-lookup"><span data-stu-id="0416e-152">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="0416e-153">Psaní analytických dotazů</span><span class="sxs-lookup"><span data-stu-id="0416e-153">Write Analytics queries</span></span>](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

