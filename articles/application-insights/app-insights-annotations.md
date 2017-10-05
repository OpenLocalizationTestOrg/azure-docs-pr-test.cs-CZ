---
title: "Verze poznámky pro službu Application Insights | Microsoft Docs"
description: "Přidání nasazení nebo vytvořit značek do grafů Průzkumníku metrik ve službě Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: f7eb2f3cba535eb64db5544c498289c9e895987a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="annotations-on-metric-charts-in-application-insights"></a><span data-ttu-id="a1a38-103">Poznámky u metriky grafů ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="a1a38-103">Annotations on metric charts in Application Insights</span></span>
<span data-ttu-id="a1a38-104">Poznámky na [Průzkumníku metrik](app-insights-metrics-explorer.md) grafy zobrazují, kde jste nasadili nového sestavení nebo jinou významnou událost.</span><span class="sxs-lookup"><span data-stu-id="a1a38-104">Annotations on [Metrics Explorer](app-insights-metrics-explorer.md) charts show where you deployed a new build, or other significant event.</span></span> <span data-ttu-id="a1a38-105">Ujistěte se snadno zjistit, zda změny měla vliv na výkon vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1a38-105">They make it easy to see whether your changes had any effect on your application's performance.</span></span> <span data-ttu-id="a1a38-106">Můžete být automaticky vytvoří pomocí [Visual Studio Team Services sestavení systému](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs).</span><span class="sxs-lookup"><span data-stu-id="a1a38-106">They can be automatically created by the [Visual Studio Team Services build system](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs).</span></span> <span data-ttu-id="a1a38-107">Můžete také vytvořit poznámky pro všechny události, které si přejete, a příznak [jejich vytváření z prostředí PowerShell](#create-annotations-from-powershell).</span><span class="sxs-lookup"><span data-stu-id="a1a38-107">You can also create annotations to flag any event you like by [creating them from PowerShell](#create-annotations-from-powershell).</span></span>

![Příklad poznámky s viditelné korelace s doba odezvy serveru](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a><span data-ttu-id="a1a38-109">Poznámky k verzi s služby VSTS sestavení</span><span class="sxs-lookup"><span data-stu-id="a1a38-109">Release annotations with VSTS build</span></span>

<span data-ttu-id="a1a38-110">Poznámky k verzi jsou funkce cloudového sestavování a verzí služby Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a1a38-110">Release annotations are a feature of the cloud-based build and release service of Visual Studio Team Services.</span></span> 

### <a name="install-the-annotations-extension-one-time"></a><span data-ttu-id="a1a38-111">Nainstalujte rozšíření poznámky (jednou)</span><span class="sxs-lookup"><span data-stu-id="a1a38-111">Install the Annotations extension (one time)</span></span>
<span data-ttu-id="a1a38-112">Abyste mohli vytvořit poznámky k verzi, musíte nainstalovat některou z mnoha rozšíření Team služeb, která je k dispozici ve Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a1a38-112">To be able to create release annotations, you'll need to install one of the many Team Service extensions available in the Visual Studio Marketplace.</span></span>

1. <span data-ttu-id="a1a38-113">Přihlaste se k vaší [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projektu.</span><span class="sxs-lookup"><span data-stu-id="a1a38-113">Sign in to your [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) project.</span></span>
2. <span data-ttu-id="a1a38-114">V sadě Visual Studio Marketplace [získat rozšíření verze poznámek](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)a přidejte ji do vašeho účtu Team Services.</span><span class="sxs-lookup"><span data-stu-id="a1a38-114">In Visual Studio Marketplace, [get the Release Annotations extension](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), and add it to your Team Services account.</span></span>

![Top na pravé straně Team Services webové stránky, otevřete Marketplace.](./media/app-insights-annotations/10.png)

<span data-ttu-id="a1a38-117">Stačí k tomu jednou pro váš účet Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a1a38-117">You only need to do this once for your Visual Studio Team Services account.</span></span> <span data-ttu-id="a1a38-118">Poznámky k verzi se teď dá nakonfigurovat pro každý projekt ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="a1a38-118">Release annotations can now be configured for any project in your account.</span></span> 

### <a name="configure-release-annotations"></a><span data-ttu-id="a1a38-119">Konfigurace verze poznámek</span><span class="sxs-lookup"><span data-stu-id="a1a38-119">Configure release annotations</span></span>

<span data-ttu-id="a1a38-120">Je nutné získat samostatné klíč rozhraní API pro každou verzi šablony služby VSTS.</span><span class="sxs-lookup"><span data-stu-id="a1a38-120">You need to get a separate API key for each VSTS release template.</span></span>

1. <span data-ttu-id="a1a38-121">Přihlaste se k [portálu Microsoft Azure](https://portal.azure.com) a otevřete prostředek Application Insights, který monitoruje vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1a38-121">Sign in to the [Microsoft Azure Portal](https://portal.azure.com) and open the Application Insights resource that monitors your application.</span></span> <span data-ttu-id="a1a38-122">(Nebo [vytvořit nový](app-insights-overview.md), pokud jste tak ještě neučinili.)</span><span class="sxs-lookup"><span data-stu-id="a1a38-122">(Or [create one now](app-insights-overview.md), if you haven't done so yet.)</span></span>
2. <span data-ttu-id="a1a38-123">Otevřete **přístup pomocí rozhraní API**, **Application Insights Id**.</span><span class="sxs-lookup"><span data-stu-id="a1a38-123">Open **API Access**,  **Application Insights Id**.</span></span>
   
    ![Na stránce portal.azure.com otevřete prostředek Application Insights a zvolte nastavení.](./media/app-insights-annotations/20.png)

4. <span data-ttu-id="a1a38-127">V samostatném okně prohlížeče otevřete (nebo vytvořte) verzi šablony, která spravuje vaše nasazení z Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a1a38-127">In a separate browser window, open (or create) the release template that manages your deployments from Visual Studio Team Services.</span></span> 
   
    <span data-ttu-id="a1a38-128">Přidat úlohu a vyberte úlohu Application Insights verze poznámky z nabídky.</span><span class="sxs-lookup"><span data-stu-id="a1a38-128">Add a task, and select the Application Insights Release Annotation task from the menu.</span></span>
   
    <span data-ttu-id="a1a38-129">Vložení **Id aplikace** který jste zkopírovali v okně přístup pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a1a38-129">Paste the **Application Id** that you copied from the API Access blade.</span></span>
   
    ![Ve Visual Studio Team Services otevřete verzi, vyberte verzi definice a zvolte Upravit.](./media/app-insights-annotations/30.png)
4. <span data-ttu-id="a1a38-133">Nastavte **APIKey** pole k proměnné `$(ApiKey)`.</span><span class="sxs-lookup"><span data-stu-id="a1a38-133">Set the **APIKey** field to a variable `$(ApiKey)`.</span></span>

5. <span data-ttu-id="a1a38-134">Zpět v okně Azure vytvořte nový klíč rozhraní API a provést jeho kopii.</span><span class="sxs-lookup"><span data-stu-id="a1a38-134">Back in the Azure window, create a new API Key and take a copy of it.</span></span>
   
    ![V okně přístup pomocí rozhraní API v okně Azure klikněte na tlačítko vytvořit klíč rozhraní API.](./media/app-insights-annotations/40.png)

6. <span data-ttu-id="a1a38-138">Otevřete kartu Konfigurace šablony verze.</span><span class="sxs-lookup"><span data-stu-id="a1a38-138">Open the Configuration tab of the release template.</span></span>
   
    <span data-ttu-id="a1a38-139">Vytvoření proměnné definice pro `ApiKey`.</span><span class="sxs-lookup"><span data-stu-id="a1a38-139">Create a variable definition for `ApiKey`.</span></span>
   
    <span data-ttu-id="a1a38-140">Vložte klíč rozhraní API k definici ApiKey proměnné.</span><span class="sxs-lookup"><span data-stu-id="a1a38-140">Paste your API key to the ApiKey variable definition.</span></span>
   
    ![V okně Team Services vyberte na kartě Konfigurace a klikněte na tlačítko Přidat proměnnou.](./media/app-insights-annotations/50.png)
7. <span data-ttu-id="a1a38-143">Nakonec **Uložit** definici verze.</span><span class="sxs-lookup"><span data-stu-id="a1a38-143">Finally, **Save** the release definition.</span></span>


## <a name="view-annotations"></a><span data-ttu-id="a1a38-144">Zobrazení poznámky</span><span class="sxs-lookup"><span data-stu-id="a1a38-144">View annotations</span></span>
<span data-ttu-id="a1a38-145">Nyní vždy, když použijete šablonu verze nasadit novou verzi, poznámky odešle do služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a1a38-145">Now, whenever you use the release template to deploy a new release, an annotation will be sent to Application Insights.</span></span> <span data-ttu-id="a1a38-146">Poznámky se zobrazí v grafech v Průzkumníku metrik.</span><span class="sxs-lookup"><span data-stu-id="a1a38-146">The annotations will appear on charts in Metrics Explorer.</span></span>

<span data-ttu-id="a1a38-147">Klikněte na všechny značky poznámky otevřít podrobnosti o verze, včetně žadatele, zdroj ovládacího prvku větve, verze definice, prostředí a další.</span><span class="sxs-lookup"><span data-stu-id="a1a38-147">Click on any annotation marker to open details about the release, including requestor, source control branch, release definition, environment, and more.</span></span>

![Klikněte na možnost všechny značky poznámky verze.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a><span data-ttu-id="a1a38-149">Vytvořit vlastní poznámky z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a1a38-149">Create custom annotations from PowerShell</span></span>
<span data-ttu-id="a1a38-150">Můžete také vytvořit poznámky z jakýkoli proces, který chcete (bez použití VS Team System).</span><span class="sxs-lookup"><span data-stu-id="a1a38-150">You can also create annotations from any process you like (without using VS Team System).</span></span> 


1. <span data-ttu-id="a1a38-151">Vytvořit místní kopii [skript prostředí Powershell z Githubu](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).</span><span class="sxs-lookup"><span data-stu-id="a1a38-151">Make a local copy of the [Powershell script from GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).</span></span>

2. <span data-ttu-id="a1a38-152">Získání ID aplikace a vytvořte klíč rozhraní API v okně přístup pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a1a38-152">Get the Application ID and create an API key from the API Access blade.</span></span>

3. <span data-ttu-id="a1a38-153">Volání skriptu takto:</span><span class="sxs-lookup"><span data-stu-id="a1a38-153">Call the script like this:</span></span>

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

<span data-ttu-id="a1a38-154">Je snadné a upravte skript, například vytvořit poznámky za poslední.</span><span class="sxs-lookup"><span data-stu-id="a1a38-154">It's easy to modify the script, for example to create annotations for the past.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1a38-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a1a38-155">Next steps</span></span>

* [<span data-ttu-id="a1a38-156">Vytváření pracovních položek</span><span class="sxs-lookup"><span data-stu-id="a1a38-156">Create work items</span></span>](app-insights-diagnostic-search.md#create-work-item)
* [<span data-ttu-id="a1a38-157">Automatizace v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a1a38-157">Automation with PowerShell</span></span>](app-insights-powershell.md)
