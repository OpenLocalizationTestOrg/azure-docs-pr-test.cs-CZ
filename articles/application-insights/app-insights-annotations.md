---
title: "aaaRelease poznámky pro službu Application Insights | Microsoft Docs"
description: "Přidání nasazení nebo vytvořit značek tooyour metriky explorer grafů ve službě Application Insights."
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
ms.openlocfilehash: e802d22701cb69e96fd1a6b469ea67454195f77a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="annotations-on-metric-charts-in-application-insights"></a><span data-ttu-id="0eb2a-103">Poznámky u metriky grafů ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="0eb2a-103">Annotations on metric charts in Application Insights</span></span>
<span data-ttu-id="0eb2a-104">Poznámky na [Průzkumníku metrik](app-insights-metrics-explorer.md) grafy zobrazují, kde jste nasadili nového sestavení nebo jinou významnou událost.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-104">Annotations on [Metrics Explorer](app-insights-metrics-explorer.md) charts show where you deployed a new build, or other significant event.</span></span> <span data-ttu-id="0eb2a-105">Provádění ho snadno toosee zda změny měla vliv na výkon vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-105">They make it easy toosee whether your changes had any effect on your application's performance.</span></span> <span data-ttu-id="0eb2a-106">Můžete být automaticky vytvoří pomocí hello [Visual Studio Team Services sestavení systému](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs).</span><span class="sxs-lookup"><span data-stu-id="0eb2a-106">They can be automatically created by hello [Visual Studio Team Services build system](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs).</span></span> <span data-ttu-id="0eb2a-107">Můžete také vytvořit poznámky tooflag všechny události, které chcete pomocí [jejich vytváření z prostředí PowerShell](#create-annotations-from-powershell).</span><span class="sxs-lookup"><span data-stu-id="0eb2a-107">You can also create annotations tooflag any event you like by [creating them from PowerShell](#create-annotations-from-powershell).</span></span>

![Příklad poznámky s viditelné korelace s doba odezvy serveru](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a><span data-ttu-id="0eb2a-109">Poznámky k verzi s služby VSTS sestavení</span><span class="sxs-lookup"><span data-stu-id="0eb2a-109">Release annotations with VSTS build</span></span>

<span data-ttu-id="0eb2a-110">Poznámky k verzi jsou funkce hello cloudového sestavování a verzí služby Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-110">Release annotations are a feature of hello cloud-based build and release service of Visual Studio Team Services.</span></span> 

### <a name="install-hello-annotations-extension-one-time"></a><span data-ttu-id="0eb2a-111">Nainstalujte rozšíření poznámky hello (jednou)</span><span class="sxs-lookup"><span data-stu-id="0eb2a-111">Install hello Annotations extension (one time)</span></span>
<span data-ttu-id="0eb2a-112">toobe možné toocreate verze poznámky, budete potřebovat tooinstall jeden z hello mnoho rozšíření Team služeb k dispozici v hello Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-112">toobe able toocreate release annotations, you'll need tooinstall one of hello many Team Service extensions available in hello Visual Studio Marketplace.</span></span>

1. <span data-ttu-id="0eb2a-113">Přihlaste se tooyour [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projektu.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-113">Sign in tooyour [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) project.</span></span>
2. <span data-ttu-id="0eb2a-114">V sadě Visual Studio Marketplace [získat hello poznámky k verzi rozšíření](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)a přidejte ji tooyour účtu Team Services.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-114">In Visual Studio Marketplace, [get hello Release Annotations extension](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), and add it tooyour Team Services account.</span></span>

![Top na pravé straně Team Services webové stránky, otevřete Marketplace.](./media/app-insights-annotations/10.png)

<span data-ttu-id="0eb2a-117">Potřebujete jenom toodo tomto jednou pro váš účet Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-117">You only need toodo this once for your Visual Studio Team Services account.</span></span> <span data-ttu-id="0eb2a-118">Poznámky k verzi se teď dá nakonfigurovat pro každý projekt ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-118">Release annotations can now be configured for any project in your account.</span></span> 

### <a name="configure-release-annotations"></a><span data-ttu-id="0eb2a-119">Konfigurace verze poznámek</span><span class="sxs-lookup"><span data-stu-id="0eb2a-119">Configure release annotations</span></span>

<span data-ttu-id="0eb2a-120">Ke každé šabloně verze služby VSTS musíte klíč tooget samostatné rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-120">You need tooget a separate API key for each VSTS release template.</span></span>

1. <span data-ttu-id="0eb2a-121">Přihlaste se toohello [portálu Microsoft Azure](https://portal.azure.com) a otevřete prostředek Application Insights hello, který monitoruje vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-121">Sign in toohello [Microsoft Azure Portal](https://portal.azure.com) and open hello Application Insights resource that monitors your application.</span></span> <span data-ttu-id="0eb2a-122">(Nebo [vytvořit nový](app-insights-overview.md), pokud jste tak ještě neučinili.)</span><span class="sxs-lookup"><span data-stu-id="0eb2a-122">(Or [create one now](app-insights-overview.md), if you haven't done so yet.)</span></span>
2. <span data-ttu-id="0eb2a-123">Otevřete **přístup pomocí rozhraní API**, **Application Insights Id**.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-123">Open **API Access**,  **Application Insights Id**.</span></span>
   
    ![Na stránce portal.azure.com otevřete prostředek Application Insights a zvolte nastavení.](./media/app-insights-annotations/20.png)

4. <span data-ttu-id="0eb2a-127">V samostatném okně prohlížeče otevřete (nebo vytvořte) hello verzi šablony, která spravuje vaše nasazení z Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-127">In a separate browser window, open (or create) hello release template that manages your deployments from Visual Studio Team Services.</span></span> 
   
    <span data-ttu-id="0eb2a-128">Přidat úlohu a vyberte úlohu hello Application Insights verze poznámky z nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-128">Add a task, and select hello Application Insights Release Annotation task from hello menu.</span></span>
   
    <span data-ttu-id="0eb2a-129">Vložení hello **Id aplikace** který jste zkopírovali ze hello okno přístup pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-129">Paste hello **Application Id** that you copied from hello API Access blade.</span></span>
   
    ![Ve Visual Studio Team Services otevřete verzi, vyberte verzi definice a zvolte Upravit.](./media/app-insights-annotations/30.png)
4. <span data-ttu-id="0eb2a-133">Sada hello **APIKey** proměnné pole tooa `$(ApiKey)`.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-133">Set hello **APIKey** field tooa variable `$(ApiKey)`.</span></span>

5. <span data-ttu-id="0eb2a-134">Zpět v hello okno Azure vytvořte nový klíč rozhraní API a provést jeho kopii.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-134">Back in hello Azure window, create a new API Key and take a copy of it.</span></span>
   
    ![V okně přístup pomocí rozhraní API v Azure okno hello hello klikněte na tlačítko vytvořit klíč rozhraní API.](./media/app-insights-annotations/40.png)

6. <span data-ttu-id="0eb2a-138">Otevřete kartu Konfigurace hello hello verze šablony.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-138">Open hello Configuration tab of hello release template.</span></span>
   
    <span data-ttu-id="0eb2a-139">Vytvoření proměnné definice pro `ApiKey`.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-139">Create a variable definition for `ApiKey`.</span></span>
   
    <span data-ttu-id="0eb2a-140">Vložte klíče toohello rozhraní API ApiKey definicí proměnné.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-140">Paste your API key toohello ApiKey variable definition.</span></span>
   
    ![V okně hello Team Services vyberte na kartě Konfigurace hello a klikněte na tlačítko Přidat proměnnou.](./media/app-insights-annotations/50.png)
7. <span data-ttu-id="0eb2a-143">Nakonec **Uložit** hello verze definice.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-143">Finally, **Save** hello release definition.</span></span>


## <a name="view-annotations"></a><span data-ttu-id="0eb2a-144">Zobrazení poznámky</span><span class="sxs-lookup"><span data-stu-id="0eb2a-144">View annotations</span></span>
<span data-ttu-id="0eb2a-145">Nyní vždy, když používáte hello verze šablony toodeploy novou verzi, poznámky odešle tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-145">Now, whenever you use hello release template toodeploy a new release, an annotation will be sent tooApplication Insights.</span></span> <span data-ttu-id="0eb2a-146">Hello poznámky se zobrazí v grafech v Průzkumníku metrik.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-146">hello annotations will appear on charts in Metrics Explorer.</span></span>

<span data-ttu-id="0eb2a-147">Klikněte na žádné poznámky značky tooopen podrobnosti o hello verze, včetně žadatele, zdroj ovládacího prvku větve, definice vydání, prostředí a další.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-147">Click on any annotation marker tooopen details about hello release, including requestor, source control branch, release definition, environment, and more.</span></span>

![Klikněte na možnost všechny značky poznámky verze.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a><span data-ttu-id="0eb2a-149">Vytvořit vlastní poznámky z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0eb2a-149">Create custom annotations from PowerShell</span></span>
<span data-ttu-id="0eb2a-150">Můžete také vytvořit poznámky z jakýkoli proces, který chcete (bez použití VS Team System).</span><span class="sxs-lookup"><span data-stu-id="0eb2a-150">You can also create annotations from any process you like (without using VS Team System).</span></span> 


1. <span data-ttu-id="0eb2a-151">Vytvořit místní kopii hello [skript prostředí Powershell z Githubu](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).</span><span class="sxs-lookup"><span data-stu-id="0eb2a-151">Make a local copy of hello [Powershell script from GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).</span></span>

2. <span data-ttu-id="0eb2a-152">Získejte hello ID aplikace a vytvořte klíč rozhraní API z hello okno přístup pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-152">Get hello Application ID and create an API key from hello API Access blade.</span></span>

3. <span data-ttu-id="0eb2a-153">Volání skriptu hello takto:</span><span class="sxs-lookup"><span data-stu-id="0eb2a-153">Call hello script like this:</span></span>

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

<span data-ttu-id="0eb2a-154">Je snadno toomodify hello skriptu, například toocreate poznámky pro posledních hello.</span><span class="sxs-lookup"><span data-stu-id="0eb2a-154">It's easy toomodify hello script, for example toocreate annotations for hello past.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0eb2a-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0eb2a-155">Next steps</span></span>

* [<span data-ttu-id="0eb2a-156">Vytváření pracovních položek</span><span class="sxs-lookup"><span data-stu-id="0eb2a-156">Create work items</span></span>](app-insights-diagnostic-search.md#create-work-item)
* [<span data-ttu-id="0eb2a-157">Automatizace v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0eb2a-157">Automation with PowerShell</span></span>](app-insights-powershell.md)
