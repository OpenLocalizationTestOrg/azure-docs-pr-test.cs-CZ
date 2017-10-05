---
title: "Vytvářet, vytvářet a nasazovat aplikace logiky v sadě Visual Studio – Azure Logic Apps | Microsoft Docs"
description: "Vytváření projektů sady Visual Studio, můžete navrhnout, vytvořit a nasadit Azure Logic Apps."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: e7f5cf483d22e4c60dedbe5176ceb0bc8b2b6e66
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a><span data-ttu-id="8bef9-103">Návrh, vytvoření a nasazení Azure Logic Apps v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8bef9-103">Design, build, and deploy Azure Logic Apps in Visual Studio</span></span>

<span data-ttu-id="8bef9-104">I když [portál Azure](https://portal.azure.com/) nabízí skvělý způsob, jak můžete vytvořit a spravovat Azure Logic Apps, můžete použít Visual Studio pro navrhování, sestavování a nasazení aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="8bef9-104">Although the [Azure portal](https://portal.azure.com/) offers a great way for you to create and manage Azure Logic Apps, you can use Visual Studio for designing, building, and deploying your logic apps.</span></span> <span data-ttu-id="8bef9-105">Visual Studio poskytuje bohaté nástroje, například návrháře logiku aplikace můžete vytvářet aplikace logiky, šablony nasazení a automatizace nakonfigurujte a nasaďte na jakémkoli prostředí.</span><span class="sxs-lookup"><span data-stu-id="8bef9-105">Visual Studio provides rich tools like the Logic App Designer for you to create logic apps, configure deployment and automation templates, and deploy to any environment.</span></span> 

<span data-ttu-id="8bef9-106">Chcete-li začít se službou Azure Logic Apps, zjistěte další [postup vytvoření první aplikace logiky na portálu Azure](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="8bef9-106">To get started with Azure Logic Apps, learn [how to create your first logic app in the Azure portal](logic-apps-create-a-logic-app.md).</span></span>

## <a name="installation-steps"></a><span data-ttu-id="8bef9-107">Postup instalace</span><span class="sxs-lookup"><span data-stu-id="8bef9-107">Installation steps</span></span>

<span data-ttu-id="8bef9-108">Instalace a konfigurace nástrojů Visual Studio pro Azure Logic Apps, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="8bef9-108">To install and configure Visual Studio tools for Azure Logic Apps, follow these steps.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="8bef9-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8bef9-109">Prerequisites</span></span>

* <span data-ttu-id="8bef9-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) nebo Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="8bef9-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) or Visual Studio 2015</span></span>
* <span data-ttu-id="8bef9-111">[Nejnovější sadu Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo vyšší)</span><span class="sxs-lookup"><span data-stu-id="8bef9-111">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="8bef9-112">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bef9-112">Azure PowerShell</span></span>](https://github.com/Azure/azure-powershell#installation)
* <span data-ttu-id="8bef9-113">Přístup k webovému při použití návrháře embedded</span><span class="sxs-lookup"><span data-stu-id="8bef9-113">Access to the web when using the embedded designer</span></span>

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a><span data-ttu-id="8bef9-114">Instalace nástrojů Visual Studio pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="8bef9-114">Install Visual Studio tools for Azure Logic Apps</span></span>

<span data-ttu-id="8bef9-115">Po instalaci požadavky:</span><span class="sxs-lookup"><span data-stu-id="8bef9-115">After you install the prerequisites:</span></span>

1. <span data-ttu-id="8bef9-116">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8bef9-116">Open Visual Studio.</span></span> <span data-ttu-id="8bef9-117">Na **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-117">On the **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="8bef9-118">Rozbalte **Online** kategorie, můžete vyhledat online.</span><span class="sxs-lookup"><span data-stu-id="8bef9-118">Expand the **Online** category so you can search online.</span></span>
3. <span data-ttu-id="8bef9-119">Procházet nebo Hledat **Logic Apps** vyhledejte **nástroje aplikace logiky Azure pro sadu Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-119">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="8bef9-120">Chcete-li stáhnout a nainstalovat rozšíření, klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-120">To download and install the extension, click **Download**.</span></span>
5. <span data-ttu-id="8bef9-121">Po instalaci, restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8bef9-121">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="8bef9-122">Můžete také stáhnout [nástroje aplikace logiky Azure pro Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) a [nástroje aplikace logiky Azure pro sadu Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) přímo z Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="8bef9-122">You can also download [Azure Logic Apps Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) and the [Azure Logic Apps Tools for Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) directly from the Visual Studio Marketplace.</span></span>

<span data-ttu-id="8bef9-123">Po dokončení instalace, můžete vytvořit projekt skupiny prostředků Azure pomocí návrháře aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="8bef9-123">After you finish installation, you can use the Azure Resource Group project with Logic App Designer.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="8bef9-124">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="8bef9-124">Create your project</span></span>

1. <span data-ttu-id="8bef9-125">Na **soubor** nabídky, přejděte na **nový**a vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-125">On the **File** menu, go to **New**, and select **Project**.</span></span> <span data-ttu-id="8bef9-126">Nebo pokud chcete přidat do existujícího řešení projektu, přejděte na **přidat**a vyberte **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-126">Or to add your project to an existing solution, go to **Add**, and select **New Project**.</span></span>

    ![Nabídka Soubor](./media/logic-apps-deploy-from-vs/filemenu.png)

2. <span data-ttu-id="8bef9-128">V **nový projekt** okně Najít **cloudu**a vyberte **skupiny prostředků Azure**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-128">In the **New Project** window, find **Cloud**, and select **Azure Resource Group**.</span></span> <span data-ttu-id="8bef9-129">Název projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-129">Name your project, and click **OK**.</span></span>

    ![Přidat nový projekt](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. <span data-ttu-id="8bef9-131">Vyberte **aplikace logiky** šablony, která vytvoří šablonu prázdné logiku aplikace nasazení budete moci použít.</span><span class="sxs-lookup"><span data-stu-id="8bef9-131">Select the **Logic App** template, which creates a blank logic app deployment template for you to use.</span></span> <span data-ttu-id="8bef9-132">Vyberte šablonu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-132">After you select your template, click **OK**.</span></span>

    ![Vyberte šablonu aplikace logiky](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    <span data-ttu-id="8bef9-134">Přidali jste nyní projektu aplikace logiky pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="8bef9-134">You've now added your logic app project to your solution.</span></span> 
    <span data-ttu-id="8bef9-135">V Průzkumníku řešení by se zobrazit váš soubor nasazení.</span><span class="sxs-lookup"><span data-stu-id="8bef9-135">In the Solution Explorer, your deployment file should appear.</span></span>

    ![Soubor nasazení](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a><span data-ttu-id="8bef9-137">Vytvoření aplikace logiky pomocí návrháře aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="8bef9-137">Create your logic app with Logic App Designer</span></span>

<span data-ttu-id="8bef9-138">Pokud máte projekt skupiny prostředků Azure, který obsahuje aplikace logiky, můžete otevřít návrhář aplikace logiky v sadě Visual Studio k vytvoření pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="8bef9-138">When you have an Azure Resource Group project that contains a logic app, you can open the Logic App Designer in Visual Studio to create your workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="8bef9-139">Návrháře vyžaduje připojení k Internetu dotazu konektory pro dostupné vlastnosti a data.</span><span class="sxs-lookup"><span data-stu-id="8bef9-139">The designer requires an internet connection to query connectors for available properties and data.</span></span> <span data-ttu-id="8bef9-140">Například pokud použijete konektor Dynamics CRM Online, návrháře dotazuje CRM instanci pro zobrazení k dispozici vlastní a výchozí vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8bef9-140">For example, if you use the Dynamics CRM Online connector, the designer queries your CRM instance to show available custom and default properties.</span></span>

1. <span data-ttu-id="8bef9-141">Klikněte pravým tlačítkem na vaše `<template>.json` soubor a vyberte **otevřete pomocí návrháře aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-141">Right-click your `<template>.json` file, and select **Open with Logic App Designer**.</span></span> <span data-ttu-id="8bef9-142">(`Ctrl+L`)</span><span class="sxs-lookup"><span data-stu-id="8bef9-142">(`Ctrl+L`)</span></span>

2. <span data-ttu-id="8bef9-143">Zvolte si předplatné, skupinu prostředků a umístění pro šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="8bef9-143">Choose your Azure subscription, resource group, and location for your deployment template.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bef9-144">Návrh aplikace logiky vytvoří připojení k rozhraní API prostředky tento dotaz na vlastnosti při návrhu.</span><span class="sxs-lookup"><span data-stu-id="8bef9-144">Designing a logic app creates API Connection resources that query for properties during design.</span></span> <span data-ttu-id="8bef9-145">Visual Studio používá k vytvoření tato připojení při návrhu vaší vybranou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="8bef9-145">Visual Studio uses your selected resource group to create those connections during design time.</span></span> <span data-ttu-id="8bef9-146">Zobrazení nebo změna všech připojení k rozhraní API, přejděte na portál Azure a vyhledejte **rozhraní API připojení**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-146">To view or change any API Connections, go to the Azure portal, and browse for **API Connections**.</span></span>

    ![Výběr předplatného.](./media/logic-apps-deploy-from-vs/designer_picker.png)

    <span data-ttu-id="8bef9-148">Používá definici v návrháři `<template>.json` soubor pro vykreslování.</span><span class="sxs-lookup"><span data-stu-id="8bef9-148">The designer uses the definition in the `<template>.json` file for rendering.</span></span>

4. <span data-ttu-id="8bef9-149">Vytváření a návrhu aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="8bef9-149">Create and design your logic app.</span></span> <span data-ttu-id="8bef9-150">Šablona nasazení aktualizována s vašimi změnami.</span><span class="sxs-lookup"><span data-stu-id="8bef9-150">Your deployment template is updated with your changes.</span></span>

    ![Návrhář aplikace logiky v sadě Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

<span data-ttu-id="8bef9-152">Visual Studio. přidá `Microsoft.Web/connections` prostředky do souboru prostředků pro všechna připojení aplikace logiky musí pracovat.</span><span class="sxs-lookup"><span data-stu-id="8bef9-152">Visual Studio adds `Microsoft.Web/connections` resources to your resource file for any connections your logic app needs to function.</span></span> <span data-ttu-id="8bef9-153">Tyto vlastnosti připojení lze nastavit při nasazení a spravovat poté, co nasadíte v **rozhraní API připojení** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8bef9-153">These connection properties can be set when you deploy, and managed after you deploy in **API Connections** in the Azure portal.</span></span>

### <a name="switch-to-json-code-view"></a><span data-ttu-id="8bef9-154">Umožňuje přepnout do zobrazení kódu JSON</span><span class="sxs-lookup"><span data-stu-id="8bef9-154">Switch to JSON code view</span></span>

<span data-ttu-id="8bef9-155">Chcete-li zobrazit reprezentaci JSON pro svou aplikaci logiky, vyberte **zobrazení kódu** karta v dolní části návrháře.</span><span class="sxs-lookup"><span data-stu-id="8bef9-155">To show the JSON representation for your logic app, select the **Code View** tab at the bottom of the designer.</span></span>

<span data-ttu-id="8bef9-156">Chcete-li přepnout zpět na úplné prostředků JSON, klikněte pravým tlačítkem `<template>.json` soubor a vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-156">To switch back to the full resource JSON, right-click the `<template>.json` file, and select **Open**.</span></span>

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a><span data-ttu-id="8bef9-157">Přidání odkazů pro závislé prostředky pro nasazení šablony sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8bef9-157">Add references for dependent resources to Visual Studio deployment templates</span></span>

<span data-ttu-id="8bef9-158">Pokud chcete svou aplikaci logiky odkazovat na závislé prostředky, můžete použít [funkce šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) v šabloně nasazení aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="8bef9-158">When you want your logic app to reference dependent resources, you can use [Azure Resource Manager template functions](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) in your logic app deployment template.</span></span> <span data-ttu-id="8bef9-159">Například můžete svou aplikaci logiky, chcete-li funkce Azure nebo integrace účtu, který chcete nasadit souběžně s svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="8bef9-159">For example, you might want your logic app to reference an Azure Function or integration account that you want to deploy alongside your logic app.</span></span> <span data-ttu-id="8bef9-160">Postupujte podle těchto pokynů o tom, jak používat parametry v šabloně nasazení tak, aby návrháře aplikace logiky vykreslí správně.</span><span class="sxs-lookup"><span data-stu-id="8bef9-160">Follow these guidelines about how to use parameters in your deployment template so that the Logic App Designer renders correctly.</span></span> 

<span data-ttu-id="8bef9-161">Používáte logiku aplikace parametry v těchto druhů triggery a akce:</span><span class="sxs-lookup"><span data-stu-id="8bef9-161">You can use logic app parameters in these kinds of triggers and actions:</span></span>

*   <span data-ttu-id="8bef9-162">Podřízené pracovní postup</span><span class="sxs-lookup"><span data-stu-id="8bef9-162">Child workflow</span></span>
*   <span data-ttu-id="8bef9-163">Funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="8bef9-163">Function app</span></span>
*   <span data-ttu-id="8bef9-164">APIM volání</span><span class="sxs-lookup"><span data-stu-id="8bef9-164">APIM call</span></span>
*   <span data-ttu-id="8bef9-165">Adresa URL rozhraní API připojení modulu runtime</span><span class="sxs-lookup"><span data-stu-id="8bef9-165">API connection runtime URL</span></span>
*   <span data-ttu-id="8bef9-166">Cesta připojení rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8bef9-166">API connection path</span></span>

<span data-ttu-id="8bef9-167">A můžete použít šablonu funkce jako je například parametry, proměnné, resourceId, concat atd. Můžete zde je ukázka, jak můžete nahradit ID prostředku Azure funkce:</span><span class="sxs-lookup"><span data-stu-id="8bef9-167">And you can use template functions such as parameters, variables, resourceId, concat, etc. For example, here's how you can replace the Azure Function resource ID:</span></span>

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

<span data-ttu-id="8bef9-168">A kde byste použili parametry:</span><span class="sxs-lookup"><span data-stu-id="8bef9-168">And where you would use parameters:</span></span>

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
<span data-ttu-id="8bef9-169">Další příklad můžete parametrizovat operaci odeslání zprávy služby Service Bus:</span><span class="sxs-lookup"><span data-stu-id="8bef9-169">As another example you can parameterize the Service Bus send message operation:</span></span>

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> <span data-ttu-id="8bef9-170">host.runtimeUrl je volitelné a lze odebrat ze šablony, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8bef9-170">host.runtimeUrl is optional and can be removed from your template if present.</span></span>
> 


> [!NOTE] 
> <span data-ttu-id="8bef9-171">Pro návrháře logiku aplikace pro práci při použití parametrů je nutné zadat výchozí hodnoty, například:</span><span class="sxs-lookup"><span data-stu-id="8bef9-171">For the Logic App Designer to work when you use parameters, you must provide default values, for example:</span></span>
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a><span data-ttu-id="8bef9-172">Uložení aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="8bef9-172">Save your logic app</span></span>

<span data-ttu-id="8bef9-173">Chcete-li uložit svou aplikaci logiky na kdykoli, přejděte na **soubor** > **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-173">To save your logic app at anytime, go to **File** > **Save**.</span></span> <span data-ttu-id="8bef9-174">(`Ctrl+S`)</span><span class="sxs-lookup"><span data-stu-id="8bef9-174">(`Ctrl+S`)</span></span> 

<span data-ttu-id="8bef9-175">Pokud svou aplikaci logiky má všechny chyby při ukládání aplikace, zobrazí se v sadě Visual Studio **výstupy** okno.</span><span class="sxs-lookup"><span data-stu-id="8bef9-175">If your logic app has any errors when you save your app, they appear in the Visual Studio **Outputs** window.</span></span>

## <a name="deploy-your-logic-app-from-visual-studio"></a><span data-ttu-id="8bef9-176">Nasazení aplikace logiky ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8bef9-176">Deploy your logic app from Visual Studio</span></span>

<span data-ttu-id="8bef9-177">Po dokončení konfigurace aplikace, můžete nasadit přímo ze sady Visual Studio v několika krocích.</span><span class="sxs-lookup"><span data-stu-id="8bef9-177">After configuring your app, you can deploy directly from Visual Studio in just a couple steps.</span></span> 

1. <span data-ttu-id="8bef9-178">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a přejděte na **nasadit** > **nové nasazení...**</span><span class="sxs-lookup"><span data-stu-id="8bef9-178">In Solution Explorer, right-click your project, and go to **Deploy** > **New Deployment...**</span></span>

    ![Nové nasazení](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. <span data-ttu-id="8bef9-180">Když se zobrazí výzva, přihlaste se k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="8bef9-180">When you're prompted, sign in to your Azure subscription.</span></span> 

3. <span data-ttu-id="8bef9-181">Nyní je třeba vybrat podrobnosti o skupině prostředků, ve které chcete nasadit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="8bef9-181">Now you must select the details for the resource group where you want to deploy your logic app.</span></span> <span data-ttu-id="8bef9-182">Až budete hotoví, vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-182">When you're done, select **Deploy**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bef9-183">Ujistěte se, že jste vybrali správný soubor šablony a parametry pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="8bef9-183">Make sure that you select the correct template and parameters file for the resource group.</span></span> <span data-ttu-id="8bef9-184">Například pokud chcete nasadit do produkčního prostředí, vyberte soubor produkční parametry.</span><span class="sxs-lookup"><span data-stu-id="8bef9-184">For example, if you want to deploy to a production environment, choose the production parameters file.</span></span>

    ![Nasazení do skupiny prostředků](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    <span data-ttu-id="8bef9-186">Stav nasazení se zobrazí v **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="8bef9-186">The deployment status appears in the **Output** window.</span></span> 
    <span data-ttu-id="8bef9-187">Možná budete muset vybrat **Azure zřizování** v **zobrazit výstup z** seznamu.</span><span class="sxs-lookup"><span data-stu-id="8bef9-187">You might have to select **Azure Provisioning** in the **Show output from** list.</span></span>

    ![Výstup stavu nasazení](./media/logic-apps-deploy-from-vs/output.png)

<span data-ttu-id="8bef9-189">V budoucnu můžete upravit aplikaci logiky ve správě zdrojového kódu a použít k nasazení nové verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8bef9-189">In the future, you can edit your logic app in source control, and use Visual Studio to deploy new versions.</span></span>

> [!NOTE]
> <span data-ttu-id="8bef9-190">Pokud změníte definici na portálu Azure přímo, tyto změny se přepíší při nasazení ze sady Visual Studio příště.</span><span class="sxs-lookup"><span data-stu-id="8bef9-190">If you change the definition in the Azure portal directly, those changes are overwritten when you deploy from Visual Studio next time.</span></span> 

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a><span data-ttu-id="8bef9-191">Přidat do existujícího projektu skupiny prostředků aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="8bef9-191">Add your logic app to an existing Resource Group project</span></span>

<span data-ttu-id="8bef9-192">Pokud máte stávající projekt skupiny prostředků, můžete přidat aplikaci logiky do tohoto projektu v okně osnovy JSON.</span><span class="sxs-lookup"><span data-stu-id="8bef9-192">If you have an existing Resource Group project, you can add your logic app to that project in the JSON Outline window.</span></span> <span data-ttu-id="8bef9-193">Můžete také přidat další aplikace logiky spolu s aplikaci, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8bef9-193">You can also add another logic app alongside the app you previously created.</span></span>

1. <span data-ttu-id="8bef9-194">Otevřete soubor `<template>.json`.</span><span class="sxs-lookup"><span data-stu-id="8bef9-194">Open the `<template>.json` file.</span></span>

2. <span data-ttu-id="8bef9-195">Chcete-li otevřít okno osnovou JSON, přejděte na **zobrazení** > **ostatní okna** > **osnovy JSON**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-195">To open the JSON Outline window, go to **View** > **Other Windows** > **JSON Outline**.</span></span>

3. <span data-ttu-id="8bef9-196">Chcete-li přidat prostředek k souboru šablony, klikněte na tlačítko **přidat prostředek** v horní části okna osnovy JSON.</span><span class="sxs-lookup"><span data-stu-id="8bef9-196">To add a resource to the template file, click **Add Resource** at the top of the JSON Outline window.</span></span> <span data-ttu-id="8bef9-197">Nebo v okně osnovou JSON, klikněte pravým tlačítkem na **prostředky**a vyberte **přidat nový prostředek**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-197">Or in the JSON Outline window, right-click **resources**, and select **Add New Resource**.</span></span>

    ![Osnova JSON – okno](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. <span data-ttu-id="8bef9-199">V **přidat prostředek** dialogové okno, vyhledejte a vyberte **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-199">In the **Add Resource** dialog box, find and select **Logic App**.</span></span> <span data-ttu-id="8bef9-200">Název aplikace logiky a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8bef9-200">Name your logic app, and choose **Add**.</span></span>

    ![Přidání prostředku](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a><span data-ttu-id="8bef9-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8bef9-202">Next Steps</span></span>

* [<span data-ttu-id="8bef9-203">Správa aplikací logiky v Průzkumníku cloudu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8bef9-203">Manage logic apps with Visual Studio Cloud Explorer</span></span>](logic-apps-manage-from-vs.md)
* <span data-ttu-id="8bef9-204">[Zobrazení běžných příkladů a scénářů](logic-apps-examples-and-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="8bef9-204">[View common examples and scenarios](logic-apps-examples-and-scenarios.md)</span></span>
* [<span data-ttu-id="8bef9-205">Informace o automatizaci obchodních procesů službou Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="8bef9-205">Learn how to automate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="8bef9-206">Zjistěte, jak integrovat systémy s Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="8bef9-206">Learn how to integrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
