---
title: "Vytvoření nového prostředku Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 5f8814ee943424c1c278ab3732129d4459f83819
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-insights-resource"></a><span data-ttu-id="e727a-103">Vytvořte prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="e727a-103">Create an Application Insights resource</span></span>
<span data-ttu-id="e727a-104">Azure Application Insights zobrazí data o vaší aplikaci v Microsoft Azure *prostředků*.</span><span class="sxs-lookup"><span data-stu-id="e727a-104">Azure Application Insights displays data about your application in a Microsoft Azure *resource*.</span></span> <span data-ttu-id="e727a-105">Vytvoření nového prostředku, proto je součástí [nastavení Application Insights pro monitorování nové aplikace][start].</span><span class="sxs-lookup"><span data-stu-id="e727a-105">Creating a new resource is therefore part of [setting up Application Insights to monitor a new application][start].</span></span> <span data-ttu-id="e727a-106">V mnoha případech vytvoření prostředku můžete provést automaticky rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="e727a-106">In many cases, creating a resource can be done automatically by the IDE.</span></span> <span data-ttu-id="e727a-107">Ale v některých případech můžete vytvořit prostředek ručně – například mít samostatné prostředky pro vývoj a produkční sestavení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e727a-107">But in some cases, you create a resource manually - for example, to have separate resources for development and production builds of your application.</span></span>

<span data-ttu-id="e727a-108">Po vytvoření prostředku, můžete získat svůj klíč instrumentace a použít ke konfiguraci SDK v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e727a-108">After you have created the resource, you get its instrumentation key and use that to configure the SDK in the application.</span></span> <span data-ttu-id="e727a-109">Klíč prostředku propojí telemetrie do prostředku.</span><span class="sxs-lookup"><span data-stu-id="e727a-109">The resource key links the telemetry to the resource.</span></span>

## <a name="sign-up-to-microsoft-azure"></a><span data-ttu-id="e727a-110">Zaregistrujte si Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="e727a-110">Sign up to Microsoft Azure</span></span>
<span data-ttu-id="e727a-111">Pokud jste to ještě získali [Microsoft account, pořiďte si ho](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="e727a-111">If you haven't got a [Microsoft account, get one now](http://live.com).</span></span> <span data-ttu-id="e727a-112">(Pokud používáte služby jako Outlook.com, OneDrive, Windows Phone nebo XBox Live, už máte účet Microsoft.)</span><span class="sxs-lookup"><span data-stu-id="e727a-112">(If you use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, you already have a Microsoft account.)</span></span>

<span data-ttu-id="e727a-113">Potřebujete předplatné [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="e727a-113">You also need a subscription to [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="e727a-114">Pokud váš tým nebo společnost má předplatné Azure, vlastník můžete přidat můžete, pomocí účtu Windows Live ID.</span><span class="sxs-lookup"><span data-stu-id="e727a-114">If your team or organization has an Azure subscription, the owner can add you to it, using your Windows Live ID.</span></span> <span data-ttu-id="e727a-115">Se účtují poplatky se používá.</span><span class="sxs-lookup"><span data-stu-id="e727a-115">You're only charged for what you use.</span></span> <span data-ttu-id="e727a-116">Základní plán výchozí umožňuje určité množství experimentální použití zdarma.</span><span class="sxs-lookup"><span data-stu-id="e727a-116">The default basic plan allows for a certain amount of experimental use free of charge.</span></span>

<span data-ttu-id="e727a-117">Pokud máte k dispozici přístup do předplatného, přihlaste se k Application Insights na [http://portal.azure.com](https://portal.azure.com)a používat svůj Live ID k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e727a-117">When you've got access to a subscription, log in to Application Insights at [http://portal.azure.com](https://portal.azure.com), and use your Live ID to login.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="e727a-118">Vytvořte prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="e727a-118">Create an Application Insights resource</span></span>
<span data-ttu-id="e727a-119">V [portal.azure.com](https://portal.azure.com), přidejte prostředek Application Insights:</span><span class="sxs-lookup"><span data-stu-id="e727a-119">In the [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-create-new-resource/01-new.png)

* <span data-ttu-id="e727a-121">**Typ aplikace** ovlivňuje, co se zobrazí v okně Přehled a vlastnosti, které jsou k dispozici v [metriky explorer][metrics].</span><span class="sxs-lookup"><span data-stu-id="e727a-121">**Application type** affects what you see on the overview blade and the properties available in [metric explorer][metrics].</span></span> <span data-ttu-id="e727a-122">Pokud nevidíte vašeho typu aplikace, vyberte Obecné.</span><span class="sxs-lookup"><span data-stu-id="e727a-122">If you don't see your type of app, choose General.</span></span>
* <span data-ttu-id="e727a-123">**Předplatné** je váš účet platebních v Azure.</span><span class="sxs-lookup"><span data-stu-id="e727a-123">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="e727a-124">**Skupina prostředků** je užitečný pro správu vlastnosti jako řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="e727a-124">**Resource group** is a convenience for managing properties like access control.</span></span> <span data-ttu-id="e727a-125">Pokud jste již vytvořili ostatní prostředky služby Azure, je možné uvést tento nový prostředek do stejné skupiny.</span><span class="sxs-lookup"><span data-stu-id="e727a-125">If you have already created other Azure resources, you can choose to put this new resource in the same group.</span></span>
* <span data-ttu-id="e727a-126">**Umístění** je, kde společnost Microsoft uchovávat data.</span><span class="sxs-lookup"><span data-stu-id="e727a-126">**Location** is where we keep your data.</span></span>
* <span data-ttu-id="e727a-127">**Připnout na řídicí panel** vloží dlaždici rychlý přístup pro prostředek na Azure domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="e727a-127">**Pin to dashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> <span data-ttu-id="e727a-128">Nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="e727a-128">Recommended.</span></span>

<span data-ttu-id="e727a-129">Po vytvoření aplikace, otevře se nové okno.</span><span class="sxs-lookup"><span data-stu-id="e727a-129">When your app has been created, a new blade opens.</span></span> <span data-ttu-id="e727a-130">Toto okno je, kde uvidíte data o využití a výkonu o vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e727a-130">This blade is where you see performance and usage data about your app.</span></span> 

<span data-ttu-id="e727a-131">Chcete-li se vrátit k němu při příštím přihlášení do Azure, vyhledejte dlaždice úvodní vaší aplikace na Tabule start (domovskou obrazovku).</span><span class="sxs-lookup"><span data-stu-id="e727a-131">To get back to it next time you log in to Azure, look for your app's quick-start tile on the start board (home screen).</span></span> <span data-ttu-id="e727a-132">Nebo klikněte na tlačítko Procházet a najít.</span><span class="sxs-lookup"><span data-stu-id="e727a-132">Or click Browse to find it.</span></span>

## <a name="copy-the-instrumentation-key"></a><span data-ttu-id="e727a-133">Zkopírovat klíč instrumentace</span><span class="sxs-lookup"><span data-stu-id="e727a-133">Copy the instrumentation key</span></span>
<span data-ttu-id="e727a-134">Klíč instrumentace identifikuje prostředek, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e727a-134">The instrumentation key identifies the resource that you created.</span></span> <span data-ttu-id="e727a-135">Je nutné ji poskytnout k sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="e727a-135">You need it to give to the SDK.</span></span>

![Klikněte na tlačítko Essentials, klikněte na klíč instrumentace, CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a><span data-ttu-id="e727a-137">Instalace sady SDK v aplikaci</span><span class="sxs-lookup"><span data-stu-id="e727a-137">Install the SDK in your app</span></span>
<span data-ttu-id="e727a-138">Nainstalujte službu Application Insights SDK ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e727a-138">Install the Application Insights SDK in your app.</span></span> <span data-ttu-id="e727a-139">Tento krok výraznou závisí na typu aplikace.</span><span class="sxs-lookup"><span data-stu-id="e727a-139">This step depends heavily on the type of your application.</span></span> 

<span data-ttu-id="e727a-140">Klíč instrumentace použít ke konfiguraci [sady SDK, který nainstalujete v aplikaci][start].</span><span class="sxs-lookup"><span data-stu-id="e727a-140">Use the instrumentation key to configure [the SDK that you install in your application][start].</span></span>

<span data-ttu-id="e727a-141">Sada SDK zahrnuje standardní moduly, které odesílat telemetrická data bez nutnosti psaní jakéhokoli kódu.</span><span class="sxs-lookup"><span data-stu-id="e727a-141">The SDK includes standard modules that send telemetry without you having to write any code.</span></span> <span data-ttu-id="e727a-142">Sledování akcí uživatele nebo diagnostikovat problémy podrobněji, [použít rozhraní API] [ api] k odeslání vlastní telemetrie.</span><span class="sxs-lookup"><span data-stu-id="e727a-142">To track user actions or diagnose issues in more detail, [use the API][api] to send your own telemetry.</span></span>

## <span data-ttu-id="e727a-143"><a name="monitor"></a>Zobrazit data telemetrie</span><span class="sxs-lookup"><span data-stu-id="e727a-143"><a name="monitor"></a>See telemetry data</span></span>
<span data-ttu-id="e727a-144">Zavřete okno rychlý start se vraťte do okna vaší aplikací na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e727a-144">Close the quick start blade to return to your application blade in the Azure portal.</span></span>

<span data-ttu-id="e727a-145">Kliknutím na dlaždici hledání najdete v části [diagnostické vyhledávání][diagnostic], kde se zobrazí první události.</span><span class="sxs-lookup"><span data-stu-id="e727a-145">Click the Search tile to see [Diagnostic Search][diagnostic], where the first events appear.</span></span> 

<span data-ttu-id="e727a-146">Pokud očekáváte více dat, klikněte na tlačítko **aktualizovat** za několik sekund.</span><span class="sxs-lookup"><span data-stu-id="e727a-146">If you're expecting more data, click **Refresh** after a few seconds  .</span></span>

## <a name="creating-a-resource-automatically"></a><span data-ttu-id="e727a-147">Vytvoření prostředku automaticky</span><span class="sxs-lookup"><span data-stu-id="e727a-147">Creating a resource automatically</span></span>
<span data-ttu-id="e727a-148">Můžete napsat [skript prostředí PowerShell](app-insights-powershell.md) automaticky vytvoření prostředku.</span><span class="sxs-lookup"><span data-stu-id="e727a-148">You can write a [PowerShell script](app-insights-powershell.md) to create a resource automatically.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e727a-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e727a-149">Next steps</span></span>
* [<span data-ttu-id="e727a-150">Vytvoření řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="e727a-150">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="e727a-151">Diagnostické vyhledávání</span><span class="sxs-lookup"><span data-stu-id="e727a-151">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="e727a-152">Zkoumání metrik</span><span class="sxs-lookup"><span data-stu-id="e727a-152">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="e727a-153">Psaní analytických dotazů</span><span class="sxs-lookup"><span data-stu-id="e727a-153">Write Analytics queries</span></span>](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

