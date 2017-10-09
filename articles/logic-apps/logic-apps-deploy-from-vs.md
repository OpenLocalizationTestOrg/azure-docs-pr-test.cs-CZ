---
title: "aaaCreate, vytvářet a nasazovat aplikace logiky v sadě Visual Studio – Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 5154cb05f9a48e9f0f2381a6953947217f7bb114
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a><span data-ttu-id="46be2-103">Návrh, vytvoření a nasazení Azure Logic Apps v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46be2-103">Design, build, and deploy Azure Logic Apps in Visual Studio</span></span>

<span data-ttu-id="46be2-104">I když hello [portál Azure](https://portal.azure.com/) nabízí skvělý způsob pro vás toocreate a spravovat Azure Logic Apps, Visual Studio můžete použít pro navrhování, sestavování a nasazení aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="46be2-104">Although hello [Azure portal](https://portal.azure.com/) offers a great way for you toocreate and manage Azure Logic Apps, you can use Visual Studio for designing, building, and deploying your logic apps.</span></span> <span data-ttu-id="46be2-105">Visual Studio poskytuje bohaté nástroje, například hello logiku aplikace Návrhář vám aplikace logiky toocreate, nakonfigurujte šablony nasazení a automatizace a nasaďte tooany prostředí.</span><span class="sxs-lookup"><span data-stu-id="46be2-105">Visual Studio provides rich tools like hello Logic App Designer for you toocreate logic apps, configure deployment and automation templates, and deploy tooany environment.</span></span> 

<span data-ttu-id="46be2-106">Další tooget začít s Azure Logic Apps, [jak toocreate svou první aplikaci logiky v hello portál Azure](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="46be2-106">tooget started with Azure Logic Apps, learn [how toocreate your first logic app in hello Azure portal](logic-apps-create-a-logic-app.md).</span></span>

## <a name="installation-steps"></a><span data-ttu-id="46be2-107">Postup instalace</span><span class="sxs-lookup"><span data-stu-id="46be2-107">Installation steps</span></span>

<span data-ttu-id="46be2-108">tooinstall a konfigurace nástrojů Visual Studio pro Azure Logic Apps, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="46be2-108">tooinstall and configure Visual Studio tools for Azure Logic Apps, follow these steps.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="46be2-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="46be2-109">Prerequisites</span></span>

* <span data-ttu-id="46be2-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) nebo Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="46be2-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) or Visual Studio 2015</span></span>
* <span data-ttu-id="46be2-111">[Nejnovější sadu Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo vyšší)</span><span class="sxs-lookup"><span data-stu-id="46be2-111">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="46be2-112">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="46be2-112">Azure PowerShell</span></span>](https://github.com/Azure/azure-powershell#installation)
* <span data-ttu-id="46be2-113">Přístup k toohello webové při použití vložených Návrhář hello</span><span class="sxs-lookup"><span data-stu-id="46be2-113">Access toohello web when using hello embedded designer</span></span>

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a><span data-ttu-id="46be2-114">Instalace nástrojů Visual Studio pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="46be2-114">Install Visual Studio tools for Azure Logic Apps</span></span>

<span data-ttu-id="46be2-115">Po instalaci hello požadavky:</span><span class="sxs-lookup"><span data-stu-id="46be2-115">After you install hello prerequisites:</span></span>

1. <span data-ttu-id="46be2-116">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46be2-116">Open Visual Studio.</span></span> <span data-ttu-id="46be2-117">Na hello **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="46be2-117">On hello **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="46be2-118">Rozbalte hello **Online** kategorie, můžete vyhledat online.</span><span class="sxs-lookup"><span data-stu-id="46be2-118">Expand hello **Online** category so you can search online.</span></span>
3. <span data-ttu-id="46be2-119">Procházet nebo Hledat **Logic Apps** vyhledejte **nástroje aplikace logiky Azure pro sadu Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="46be2-119">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="46be2-120">toodownload a příponu hello instalace, klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="46be2-120">toodownload and install hello extension, click **Download**.</span></span>
5. <span data-ttu-id="46be2-121">Po instalaci, restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46be2-121">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="46be2-122">Můžete také stáhnout [nástroje aplikace logiky Azure pro Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) a hello [nástroje aplikace logiky Azure pro sadu Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) přímo z hello Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="46be2-122">You can also download [Azure Logic Apps Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) and hello [Azure Logic Apps Tools for Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) directly from hello Visual Studio Marketplace.</span></span>

<span data-ttu-id="46be2-123">Po dokončení instalace, můžete vytvořit projekt skupiny prostředků Azure hello pomocí návrháře aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="46be2-123">After you finish installation, you can use hello Azure Resource Group project with Logic App Designer.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="46be2-124">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="46be2-124">Create your project</span></span>

1. <span data-ttu-id="46be2-125">Na hello **soubor** nabídky, přejděte příliš**nový**a vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="46be2-125">On hello **File** menu, go too**New**, and select **Project**.</span></span> <span data-ttu-id="46be2-126">Nebo tooadd vašeho projektu tooan existující řešení, přejděte příliš**přidat**a vyberte **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="46be2-126">Or tooadd your project tooan existing solution, go too**Add**, and select **New Project**.</span></span>

    ![Nabídka Soubor](./media/logic-apps-deploy-from-vs/filemenu.png)

2. <span data-ttu-id="46be2-128">V hello **nový projekt** okně Najít **cloudu**a vyberte **skupiny prostředků Azure**.</span><span class="sxs-lookup"><span data-stu-id="46be2-128">In hello **New Project** window, find **Cloud**, and select **Azure Resource Group**.</span></span> <span data-ttu-id="46be2-129">Název projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="46be2-129">Name your project, and click **OK**.</span></span>

    ![Přidat nový projekt](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. <span data-ttu-id="46be2-131">Vyberte hello **aplikace logiky** šablony, která pro vás toouse vytvoří šablonu nasazení prázdné logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="46be2-131">Select hello **Logic App** template, which creates a blank logic app deployment template for you toouse.</span></span> <span data-ttu-id="46be2-132">Vyberte šablonu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="46be2-132">After you select your template, click **OK**.</span></span>

    ![Vyberte šablonu aplikace logiky](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    <span data-ttu-id="46be2-134">Přidali jste nyní řešení tooyour projektu aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="46be2-134">You've now added your logic app project tooyour solution.</span></span> 
    <span data-ttu-id="46be2-135">V Průzkumníku řešení hello by se zobrazit váš soubor nasazení.</span><span class="sxs-lookup"><span data-stu-id="46be2-135">In hello Solution Explorer, your deployment file should appear.</span></span>

    ![Soubor nasazení](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a><span data-ttu-id="46be2-137">Vytvoření aplikace logiky pomocí návrháře aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="46be2-137">Create your logic app with Logic App Designer</span></span>

<span data-ttu-id="46be2-138">Pokud máte projekt skupiny prostředků Azure, který obsahuje aplikace logiky, můžete otevřít hello návrhář aplikace na základě logiky v sadě Visual Studio toocreate pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="46be2-138">When you have an Azure Resource Group project that contains a logic app, you can open hello Logic App Designer in Visual Studio toocreate your workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="46be2-139">Návrhář Hello vyžaduje připojení k Internetu příliš dotaz konektory pro dostupné vlastnosti a data.</span><span class="sxs-lookup"><span data-stu-id="46be2-139">hello designer requires an internet connection too query connectors for available properties and data.</span></span> <span data-ttu-id="46be2-140">Například pokud použijete konektor hello Dynamics CRM Online, Návrhář hello dotazy k dispozici vlastní tooshow CRM instance a výchozí vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="46be2-140">For example, if you use hello Dynamics CRM Online connector, hello designer queries your CRM instance tooshow available custom and default properties.</span></span>

1. <span data-ttu-id="46be2-141">Klikněte pravým tlačítkem na vaše `<template>.json` soubor a vyberte **otevřete pomocí návrháře aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="46be2-141">Right-click your `<template>.json` file, and select **Open with Logic App Designer**.</span></span> <span data-ttu-id="46be2-142">(`Ctrl+L`)</span><span class="sxs-lookup"><span data-stu-id="46be2-142">(`Ctrl+L`)</span></span>

2. <span data-ttu-id="46be2-143">Zvolte si předplatné, skupinu prostředků a umístění pro šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="46be2-143">Choose your Azure subscription, resource group, and location for your deployment template.</span></span>

    > [!NOTE]
    > <span data-ttu-id="46be2-144">Návrh aplikace logiky vytvoří připojení k rozhraní API prostředky tento dotaz na vlastnosti při návrhu.</span><span class="sxs-lookup"><span data-stu-id="46be2-144">Designing a logic app creates API Connection resources that query for properties during design.</span></span> <span data-ttu-id="46be2-145">Visual Studio použije vaše toocreate vybraného prostředku skupiny tato připojení v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="46be2-145">Visual Studio uses your selected resource group toocreate those connections during design time.</span></span> <span data-ttu-id="46be2-146">tooview nebo změnit všech připojení k rozhraní API, přejděte toohello portál Azure a vyhledejte **rozhraní API připojení**.</span><span class="sxs-lookup"><span data-stu-id="46be2-146">tooview or change any API Connections, go toohello Azure portal, and browse for **API Connections**.</span></span>

    ![Výběr předplatného.](./media/logic-apps-deploy-from-vs/designer_picker.png)

    <span data-ttu-id="46be2-148">Návrhář Hello používá hello definici v hello `<template>.json` soubor pro vykreslování.</span><span class="sxs-lookup"><span data-stu-id="46be2-148">hello designer uses hello definition in hello `<template>.json` file for rendering.</span></span>

4. <span data-ttu-id="46be2-149">Vytváření a návrhu aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="46be2-149">Create and design your logic app.</span></span> <span data-ttu-id="46be2-150">Šablona nasazení aktualizována s vašimi změnami.</span><span class="sxs-lookup"><span data-stu-id="46be2-150">Your deployment template is updated with your changes.</span></span>

    ![Návrhář aplikace logiky v sadě Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

<span data-ttu-id="46be2-152">Visual Studio. přidá `Microsoft.Web/connections` prostředky příliš váš zdrojový soubor pro všechna připojení aplikace logiky musí toofunction.</span><span class="sxs-lookup"><span data-stu-id="46be2-152">Visual Studio adds `Microsoft.Web/connections` resources too your resource file for any connections your logic app needs toofunction.</span></span> <span data-ttu-id="46be2-153">Tyto vlastnosti připojení lze nastavit při nasazení a spravovat poté, co nasadíte v **rozhraní API připojení** v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="46be2-153">These connection properties can be set when you deploy, and managed after you deploy in **API Connections** in hello Azure portal.</span></span>

### <a name="switch-toojson-code-view"></a><span data-ttu-id="46be2-154">Zobrazení kódu tooJSON přepínače</span><span class="sxs-lookup"><span data-stu-id="46be2-154">Switch tooJSON code view</span></span>

<span data-ttu-id="46be2-155">tooshow hello reprezentace JSON pro svou aplikaci logiky, vyberte hello **zobrazení kódu** karta v dolní části hello hello návrháře.</span><span class="sxs-lookup"><span data-stu-id="46be2-155">tooshow hello JSON representation for your logic app, select hello **Code View** tab at hello bottom of hello designer.</span></span>

<span data-ttu-id="46be2-156">tooswitch zpět toohello úplné prostředků JSON, klikněte pravým tlačítkem na hello `<template>.json` soubor a vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="46be2-156">tooswitch back toohello full resource JSON, right-click hello `<template>.json` file, and select **Open**.</span></span>

### <a name="add-references-for-dependent-resources-toovisual-studio-deployment-templates"></a><span data-ttu-id="46be2-157">Přidání odkazů pro závislé prostředky tooVisual Studio nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="46be2-157">Add references for dependent resources tooVisual Studio deployment templates</span></span>

<span data-ttu-id="46be2-158">Pokud chcete prostředky závislé tooreference aplikace logiky, můžete použít [funkce šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) v šabloně nasazení aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="46be2-158">When you want your logic app tooreference dependent resources, you can use [Azure Resource Manager template functions](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) in your logic app deployment template.</span></span> <span data-ttu-id="46be2-159">Například můžete vaše logiku aplikace tooreference účet funkce Azure nebo integrace, které chcete toodeploy společně se svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="46be2-159">For example, you might want your logic app tooreference an Azure Function or integration account that you want toodeploy alongside your logic app.</span></span> <span data-ttu-id="46be2-160">Postupujte podle těchto pokynů o tom, jak toouse parametry v šabloně nasazení, který hello návrhář aplikace na základě logiky vykreslí správně.</span><span class="sxs-lookup"><span data-stu-id="46be2-160">Follow these guidelines about how toouse parameters in your deployment template so that hello Logic App Designer renders correctly.</span></span> 

<span data-ttu-id="46be2-161">Používáte logiku aplikace parametry v těchto druhů triggery a akce:</span><span class="sxs-lookup"><span data-stu-id="46be2-161">You can use logic app parameters in these kinds of triggers and actions:</span></span>

*   <span data-ttu-id="46be2-162">Podřízené pracovní postup</span><span class="sxs-lookup"><span data-stu-id="46be2-162">Child workflow</span></span>
*   <span data-ttu-id="46be2-163">Funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="46be2-163">Function app</span></span>
*   <span data-ttu-id="46be2-164">APIM volání</span><span class="sxs-lookup"><span data-stu-id="46be2-164">APIM call</span></span>
*   <span data-ttu-id="46be2-165">Adresa URL rozhraní API připojení modulu runtime</span><span class="sxs-lookup"><span data-stu-id="46be2-165">API connection runtime URL</span></span>
*   <span data-ttu-id="46be2-166">Cesta připojení rozhraní API</span><span class="sxs-lookup"><span data-stu-id="46be2-166">API connection path</span></span>

<span data-ttu-id="46be2-167">A můžete použít šablonu funkce jako je například parametry, proměnné, resourceId, concat atd. Můžete zde je ukázka, jak můžete nahradit ID prostředku hello funkce Azure:</span><span class="sxs-lookup"><span data-stu-id="46be2-167">And you can use template functions such as parameters, variables, resourceId, concat, etc. For example, here's how you can replace hello Azure Function resource ID:</span></span>

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

<span data-ttu-id="46be2-168">A kde byste použili parametry:</span><span class="sxs-lookup"><span data-stu-id="46be2-168">And where you would use parameters:</span></span>

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
<span data-ttu-id="46be2-169">Další příklad můžete parametrizovat hello Service Bus odeslat zprávu operace:</span><span class="sxs-lookup"><span data-stu-id="46be2-169">As another example you can parameterize hello Service Bus send message operation:</span></span>

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
> <span data-ttu-id="46be2-170">host.runtimeUrl je volitelné a lze odebrat ze šablony, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="46be2-170">host.runtimeUrl is optional and can be removed from your template if present.</span></span>
> 


> [!NOTE] 
> <span data-ttu-id="46be2-171">Pro hello návrhář aplikace na základě logiky toowork při použití parametrů, je nutné zadat výchozí hodnoty, například:</span><span class="sxs-lookup"><span data-stu-id="46be2-171">For hello Logic App Designer toowork when you use parameters, you must provide default values, for example:</span></span>
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

### <a name="save-your-logic-app"></a><span data-ttu-id="46be2-172">Uložení aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="46be2-172">Save your logic app</span></span>

<span data-ttu-id="46be2-173">toosave svou aplikaci logiky na kdykoli, přejděte příliš**soubor** > **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="46be2-173">toosave your logic app at anytime, go too**File** > **Save**.</span></span> <span data-ttu-id="46be2-174">(`Ctrl+S`)</span><span class="sxs-lookup"><span data-stu-id="46be2-174">(`Ctrl+S`)</span></span> 

<span data-ttu-id="46be2-175">Pokud svou aplikaci logiky všechny chyby při ukládání aplikace, se objeví v sadě Visual Studio hello **výstupy** okno.</span><span class="sxs-lookup"><span data-stu-id="46be2-175">If your logic app has any errors when you save your app, they appear in hello Visual Studio **Outputs** window.</span></span>

## <a name="deploy-your-logic-app-from-visual-studio"></a><span data-ttu-id="46be2-176">Nasazení aplikace logiky ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46be2-176">Deploy your logic app from Visual Studio</span></span>

<span data-ttu-id="46be2-177">Po dokončení konfigurace aplikace, můžete nasadit přímo ze sady Visual Studio v několika krocích.</span><span class="sxs-lookup"><span data-stu-id="46be2-177">After configuring your app, you can deploy directly from Visual Studio in just a couple steps.</span></span> 

1. <span data-ttu-id="46be2-178">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a přejděte příliš**nasadit** > **nové nasazení...**</span><span class="sxs-lookup"><span data-stu-id="46be2-178">In Solution Explorer, right-click your project, and go too**Deploy** > **New Deployment...**</span></span>

    ![Nové nasazení](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. <span data-ttu-id="46be2-180">Když se zobrazí výzva, přihlaste se tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="46be2-180">When you're prompted, sign in tooyour Azure subscription.</span></span> 

3. <span data-ttu-id="46be2-181">Nyní je třeba vybrat hello podrobnosti pro skupinu prostředků hello místo toodeploy svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="46be2-181">Now you must select hello details for hello resource group where you want toodeploy your logic app.</span></span> <span data-ttu-id="46be2-182">Až budete hotoví, vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="46be2-182">When you're done, select **Deploy**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="46be2-183">Ujistěte se, že jste vybrali hello správnou šablonu a soubor parametrů pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="46be2-183">Make sure that you select hello correct template and parameters file for hello resource group.</span></span> <span data-ttu-id="46be2-184">Například pokud chcete toodeploy tooa provozním prostředí, vyberte soubor parametrů produkční hello.</span><span class="sxs-lookup"><span data-stu-id="46be2-184">For example, if you want toodeploy tooa production environment, choose hello production parameters file.</span></span>

    ![Nasazení tooresource skupiny](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    <span data-ttu-id="46be2-186">Stav nasazení Hello se zobrazí v hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="46be2-186">hello deployment status appears in hello **Output** window.</span></span> 
    <span data-ttu-id="46be2-187">Můžete mít tooselect **Azure zřizování** v hello **zobrazit výstup z** seznamu.</span><span class="sxs-lookup"><span data-stu-id="46be2-187">You might have tooselect **Azure Provisioning** in hello **Show output from** list.</span></span>

    ![Výstup stavu nasazení](./media/logic-apps-deploy-from-vs/output.png)

<span data-ttu-id="46be2-189">V budoucích hello můžete upravit aplikaci logiky ve správě zdrojového kódu a použít nové verze sady Visual Studio toodeploy.</span><span class="sxs-lookup"><span data-stu-id="46be2-189">In hello future, you can edit your logic app in source control, and use Visual Studio toodeploy new versions.</span></span>

> [!NOTE]
> <span data-ttu-id="46be2-190">Pokud změníte hello definice v hello portál Azure přímo, tyto změny se přepíší při nasazení ze sady Visual Studio příště.</span><span class="sxs-lookup"><span data-stu-id="46be2-190">If you change hello definition in hello Azure portal directly, those changes are overwritten when you deploy from Visual Studio next time.</span></span> 

## <a name="add-your-logic-app-tooan-existing-resource-group-project"></a><span data-ttu-id="46be2-191">Přidejte logiku aplikace tooan existující skupinu prostředků projektu</span><span class="sxs-lookup"><span data-stu-id="46be2-191">Add your logic app tooan existing Resource Group project</span></span>

<span data-ttu-id="46be2-192">Pokud máte existující projekt skupiny prostředků, můžete přidat projektu logiku aplikace toothat v okně osnovy JSON hello.</span><span class="sxs-lookup"><span data-stu-id="46be2-192">If you have an existing Resource Group project, you can add your logic app toothat project in hello JSON Outline window.</span></span> <span data-ttu-id="46be2-193">Můžete také přidat další aplikace logiky spolu s hello aplikace, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="46be2-193">You can also add another logic app alongside hello app you previously created.</span></span>

1. <span data-ttu-id="46be2-194">Otevřete hello `<template>.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="46be2-194">Open hello `<template>.json` file.</span></span>

2. <span data-ttu-id="46be2-195">tooopen hello okno osnovy JSON, přejděte příliš**zobrazení** > **ostatní okna** > **osnovy JSON**.</span><span class="sxs-lookup"><span data-stu-id="46be2-195">tooopen hello JSON Outline window, go too**View** > **Other Windows** > **JSON Outline**.</span></span>

3. <span data-ttu-id="46be2-196">Klikněte na tlačítko tooadd soubor prostředků toohello šablony **přidat prostředek** hello horní části okna osnovy JSON hello.</span><span class="sxs-lookup"><span data-stu-id="46be2-196">tooadd a resource toohello template file, click **Add Resource** at hello top of hello JSON Outline window.</span></span> <span data-ttu-id="46be2-197">Nebo v okně hello osnovou JSON, klikněte pravým tlačítkem na **prostředky**a vyberte **přidat nový prostředek**.</span><span class="sxs-lookup"><span data-stu-id="46be2-197">Or in hello JSON Outline window, right-click **resources**, and select **Add New Resource**.</span></span>

    ![Osnova JSON – okno](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. <span data-ttu-id="46be2-199">V hello **přidat prostředek** dialogové okno, vyhledejte a vyberte **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="46be2-199">In hello **Add Resource** dialog box, find and select **Logic App**.</span></span> <span data-ttu-id="46be2-200">Název aplikace logiky a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="46be2-200">Name your logic app, and choose **Add**.</span></span>

    ![Přidání prostředku](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a><span data-ttu-id="46be2-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="46be2-202">Next Steps</span></span>

* [<span data-ttu-id="46be2-203">Správa aplikací logiky v Průzkumníku cloudu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46be2-203">Manage logic apps with Visual Studio Cloud Explorer</span></span>](logic-apps-manage-from-vs.md)
* <span data-ttu-id="46be2-204">[Zobrazení běžných příkladů a scénářů](logic-apps-examples-and-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="46be2-204">[View common examples and scenarios](logic-apps-examples-and-scenarios.md)</span></span>
* [<span data-ttu-id="46be2-205">Zjistěte, jak tooautomate obchodní procesy službou Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="46be2-205">Learn how tooautomate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="46be2-206">Zjistěte, jak toointegrate vaše systémy službou Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="46be2-206">Learn how toointegrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
