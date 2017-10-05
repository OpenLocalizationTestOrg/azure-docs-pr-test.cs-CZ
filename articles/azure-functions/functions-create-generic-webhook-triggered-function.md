---
title: "Vytvoří funkci, v Azure aktivovány obecný webhook | Microsoft Docs"
description: "Použití Azure Functions vytvořit funkci bez serveru, který je vyvolán webhooku v Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: f283f8d79c5ae5fb6a72c84c9e9edb7bb8de4a83
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a><span data-ttu-id="8de7a-103">Vytvoření funkce aktivovány obecný webhook</span><span class="sxs-lookup"><span data-stu-id="8de7a-103">Create a function triggered by a generic webhook</span></span>

<span data-ttu-id="8de7a-104">Služba Azure Functions umožňuje spuštění kódu v prostředí bez serveru, aniž byste nejdřív museli vytvořit virtuální počítač nebo publikovat webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8de7a-104">Azure Functions lets you execute your code in a serverless environment without having to first create a VM or publish a web application.</span></span> <span data-ttu-id="8de7a-105">Můžete například nakonfigurovat funkce, která se aktivovat výstrahu aktivováno monitorování Azure.</span><span class="sxs-lookup"><span data-stu-id="8de7a-105">For example, you can configure a function to be triggered by an alert raised by Azure Monitor.</span></span> <span data-ttu-id="8de7a-106">Toto téma ukazuje, jak provádět kód C#, pokud skupina prostředků se přidá do vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="8de7a-106">This topic shows you how to execute C# code when a resource group is added to your subscription.</span></span>   

![Obecný webhook aktivuje funkce na portálu Azure](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a><span data-ttu-id="8de7a-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8de7a-108">Prerequisites</span></span> 

<span data-ttu-id="8de7a-109">K provedení kroků v tomto kurzu je potřeba:</span><span class="sxs-lookup"><span data-stu-id="8de7a-109">To complete this tutorial:</span></span>

+ <span data-ttu-id="8de7a-110">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="8de7a-110">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="8de7a-111">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="8de7a-111">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

<span data-ttu-id="8de7a-112">Dál vytvoříte v nové aplikaci Function App funkci.</span><span class="sxs-lookup"><span data-stu-id="8de7a-112">Next, you create a function in the new function app.</span></span>

## <span data-ttu-id="8de7a-113"><a name="create-function"></a>Vytvoření obecný webhook aktivované funkce</span><span class="sxs-lookup"><span data-stu-id="8de7a-113"><a name="create-function"></a>Create a generic webhook triggered function</span></span>

1. <span data-ttu-id="8de7a-114">Rozbalte aplikaci Function App a klikněte na tlačítko **+** vedle položky **Funkce**.</span><span class="sxs-lookup"><span data-stu-id="8de7a-114">Expand your function app and click the **+** button next to **Functions**.</span></span> <span data-ttu-id="8de7a-115">Pokud tuto funkci je první z nich v aplikaci funkce, vyberte **vlastní funkce**.</span><span class="sxs-lookup"><span data-stu-id="8de7a-115">If this function is the first one in your function app, select **Custom function**.</span></span> <span data-ttu-id="8de7a-116">Zobrazí se kompletní sada šablon funkcí.</span><span class="sxs-lookup"><span data-stu-id="8de7a-116">This displays the complete set of function templates.</span></span>

    ![Stručný úvod do služby Functions na webu Azure Portal](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. <span data-ttu-id="8de7a-118">Vyberte **obecný WebHook - C#** šablony.</span><span class="sxs-lookup"><span data-stu-id="8de7a-118">Select the **Generic WebHook - C#** template.</span></span> <span data-ttu-id="8de7a-119">Zadejte název pro funkce C# a pak vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8de7a-119">Type a name for your C# function, then select **Create**.</span></span>

     ![Vytvoří funkci, obecný webhook aktivuje na portálu Azure](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. <span data-ttu-id="8de7a-121">V nové funkce, klikněte na **URL funkce <> / Get**, zkopírujte a uložte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8de7a-121">In your new function, click **</> Get function URL**, then copy and save the value.</span></span> <span data-ttu-id="8de7a-122">Tuto hodnotu použijete ke konfiguraci webhooku.</span><span class="sxs-lookup"><span data-stu-id="8de7a-122">You use this value to configure the webhook.</span></span> 

    ![Kontrola kódu funkce](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
<span data-ttu-id="8de7a-124">Dále vytvoříte koncový bod webhooku ve výstraze protokolu aktivit v Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="8de7a-124">Next, you create a webhook endpoint in an activity log alert in Azure Monitor.</span></span> 

## <a name="create-an-activity-log-alert"></a><span data-ttu-id="8de7a-125">Vytvořit výstrahu protokolu aktivit</span><span class="sxs-lookup"><span data-stu-id="8de7a-125">Create an activity log alert</span></span>

1. <span data-ttu-id="8de7a-126">Na portálu Azure přejděte do **monitorování** služby, vyberte **výstrahy**a klikněte na tlačítko **přidat aktivitu protokolu upozornění**.</span><span class="sxs-lookup"><span data-stu-id="8de7a-126">In the Azure portal, navigate to the **Monitor** service, select **Alerts**, and click **Add activity log alert**.</span></span>   

    ![Monitorování](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. <span data-ttu-id="8de7a-128">Použijte nastavení uvedená v tabulce:</span><span class="sxs-lookup"><span data-stu-id="8de7a-128">Use the settings as specified in the table:</span></span>

    ![Vytvořit výstrahu protokolu aktivit](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | <span data-ttu-id="8de7a-130">Nastavení</span><span class="sxs-lookup"><span data-stu-id="8de7a-130">Setting</span></span>      |  <span data-ttu-id="8de7a-131">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="8de7a-131">Suggested value</span></span>   | <span data-ttu-id="8de7a-132">Popis</span><span class="sxs-lookup"><span data-stu-id="8de7a-132">Description</span></span>                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | <span data-ttu-id="8de7a-133">**Název výstrahy protokolu aktivit**</span><span class="sxs-lookup"><span data-stu-id="8de7a-133">**Activity log alert name**</span></span> | <span data-ttu-id="8de7a-134">prostředek skupiny vytvořit – upozornění</span><span class="sxs-lookup"><span data-stu-id="8de7a-134">resource-group-create-alert</span></span> | <span data-ttu-id="8de7a-135">Název protokolu výstraha aktivity.</span><span class="sxs-lookup"><span data-stu-id="8de7a-135">Name of the activity log alert.</span></span> |
    | <span data-ttu-id="8de7a-136">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="8de7a-136">**Subscription**</span></span> | <span data-ttu-id="8de7a-137">Vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="8de7a-137">Your subscription</span></span> | <span data-ttu-id="8de7a-138">Odběr, který používáte pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8de7a-138">The subscription you are using for this tutorial.</span></span> | 
    |  <span data-ttu-id="8de7a-139">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="8de7a-139">**Resource Group**</span></span> | <span data-ttu-id="8de7a-140">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="8de7a-140">myResourceGroup</span></span> | <span data-ttu-id="8de7a-141">Výstrahy prostředky jsou nasazeny do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="8de7a-141">The resource group that the alert resources are deployed to.</span></span> <span data-ttu-id="8de7a-142">Pomocí stejné skupině prostředků jako aplikace funkce usnadňuje odstraňování až po dokončení kurzu.</span><span class="sxs-lookup"><span data-stu-id="8de7a-142">Using the same resource group as your function app makes it easier to clean up after you complete the tutorial.</span></span> |
    | <span data-ttu-id="8de7a-143">**Kategorie události**</span><span class="sxs-lookup"><span data-stu-id="8de7a-143">**Event category**</span></span> | <span data-ttu-id="8de7a-144">Pro správu</span><span class="sxs-lookup"><span data-stu-id="8de7a-144">Administrative</span></span> | <span data-ttu-id="8de7a-145">Tato kategorie zahrnuje změny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="8de7a-145">This category includes changes made to Azure resources.</span></span>  |
    | <span data-ttu-id="8de7a-146">**Typ prostředku**</span><span class="sxs-lookup"><span data-stu-id="8de7a-146">**Resource type**</span></span> | <span data-ttu-id="8de7a-147">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="8de7a-147">Resource groups</span></span> | <span data-ttu-id="8de7a-148">Filtrování výstrah pro aktivity skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="8de7a-148">Filters alerts to resource group activities.</span></span> |
    | <span data-ttu-id="8de7a-149">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="8de7a-149">**Resource Group**</span></span><br/><span data-ttu-id="8de7a-150">a **prostředků**</span><span class="sxs-lookup"><span data-stu-id="8de7a-150">and **Resource**</span></span> | <span data-ttu-id="8de7a-151">Všechny</span><span class="sxs-lookup"><span data-stu-id="8de7a-151">All</span></span> | <span data-ttu-id="8de7a-152">Monitorujte všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="8de7a-152">Monitor all resources.</span></span> |
    | <span data-ttu-id="8de7a-153">**Název operace**</span><span class="sxs-lookup"><span data-stu-id="8de7a-153">**Operation name**</span></span> | <span data-ttu-id="8de7a-154">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="8de7a-154">Create Resource Group</span></span> | <span data-ttu-id="8de7a-155">Filtrování výstrah pro vytvoření operací.</span><span class="sxs-lookup"><span data-stu-id="8de7a-155">Filters alerts to create operations.</span></span> |
    | <span data-ttu-id="8de7a-156">**Úroveň**</span><span class="sxs-lookup"><span data-stu-id="8de7a-156">**Level**</span></span> | <span data-ttu-id="8de7a-157">Informační</span><span class="sxs-lookup"><span data-stu-id="8de7a-157">Informational</span></span> | <span data-ttu-id="8de7a-158">Zahrnout informační úrovně výstrahy.</span><span class="sxs-lookup"><span data-stu-id="8de7a-158">Include informational level alerts.</span></span> | 
    | <span data-ttu-id="8de7a-159">**Stav**</span><span class="sxs-lookup"><span data-stu-id="8de7a-159">**Status**</span></span> | <span data-ttu-id="8de7a-160">Úspěch</span><span class="sxs-lookup"><span data-stu-id="8de7a-160">Succeeded</span></span> | <span data-ttu-id="8de7a-161">Filtrování výstrah na akce, které byly úspěšně dokončeny.</span><span class="sxs-lookup"><span data-stu-id="8de7a-161">Filters alerts to actions that have completed successfully.</span></span> |
    | <span data-ttu-id="8de7a-162">**Akce skupiny**</span><span class="sxs-lookup"><span data-stu-id="8de7a-162">**Action group**</span></span> | <span data-ttu-id="8de7a-163">Nový</span><span class="sxs-lookup"><span data-stu-id="8de7a-163">New</span></span> | <span data-ttu-id="8de7a-164">Vytvořte novou skupinu akce, který definuje akce trvá, když je vydána výstraha.</span><span class="sxs-lookup"><span data-stu-id="8de7a-164">Create a new action group, which defines the action takes when an alert is raised.</span></span> |
    | <span data-ttu-id="8de7a-165">**Název skupiny akce**</span><span class="sxs-lookup"><span data-stu-id="8de7a-165">**Action group name**</span></span> | <span data-ttu-id="8de7a-166">Funkce webhooku</span><span class="sxs-lookup"><span data-stu-id="8de7a-166">function-webhook</span></span> | <span data-ttu-id="8de7a-167">Název pro identifikaci skupiny akce.</span><span class="sxs-lookup"><span data-stu-id="8de7a-167">A name to identify the action group.</span></span>  | 
    | <span data-ttu-id="8de7a-168">**Krátký název**</span><span class="sxs-lookup"><span data-stu-id="8de7a-168">**Short name**</span></span> | <span data-ttu-id="8de7a-169">funcwebhook</span><span class="sxs-lookup"><span data-stu-id="8de7a-169">funcwebhook</span></span> | <span data-ttu-id="8de7a-170">Krátký název pro skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="8de7a-170">A short name for the action group.</span></span> |  

3. <span data-ttu-id="8de7a-171">V **akce**, přidat akci pomocí nastavení uvedeného v tabulce:</span><span class="sxs-lookup"><span data-stu-id="8de7a-171">In **Actions**, add an action using the settings as specified in the table:</span></span> 

    ![Přidat skupinu akce](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | <span data-ttu-id="8de7a-173">Nastavení</span><span class="sxs-lookup"><span data-stu-id="8de7a-173">Setting</span></span>      |  <span data-ttu-id="8de7a-174">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="8de7a-174">Suggested value</span></span>   | <span data-ttu-id="8de7a-175">Popis</span><span class="sxs-lookup"><span data-stu-id="8de7a-175">Description</span></span>                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | <span data-ttu-id="8de7a-176">**Název**</span><span class="sxs-lookup"><span data-stu-id="8de7a-176">**Name**</span></span> | <span data-ttu-id="8de7a-177">CallFunctionWebhook</span><span class="sxs-lookup"><span data-stu-id="8de7a-177">CallFunctionWebhook</span></span> | <span data-ttu-id="8de7a-178">Název akce.</span><span class="sxs-lookup"><span data-stu-id="8de7a-178">A name for the action.</span></span> |
    | <span data-ttu-id="8de7a-179">**Typ akce**</span><span class="sxs-lookup"><span data-stu-id="8de7a-179">**Action type**</span></span> | <span data-ttu-id="8de7a-180">Webhook</span><span class="sxs-lookup"><span data-stu-id="8de7a-180">Webhook</span></span> | <span data-ttu-id="8de7a-181">Odpověď na výstrahy je, že adresa URL Webhooku se nazývá.</span><span class="sxs-lookup"><span data-stu-id="8de7a-181">The response to the alert is that a Webhook URL is called.</span></span> |
    | <span data-ttu-id="8de7a-182">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="8de7a-182">**Details**</span></span> | <span data-ttu-id="8de7a-183">URL – funkce</span><span class="sxs-lookup"><span data-stu-id="8de7a-183">Function URL</span></span> | <span data-ttu-id="8de7a-184">Vložte adresu URL webhooku funkce, který jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="8de7a-184">Paste in the webhook URL of the function that you copied earlier.</span></span> |<span data-ttu-id="8de7a-185">v</span><span class="sxs-lookup"><span data-stu-id="8de7a-185">v</span></span>

4. <span data-ttu-id="8de7a-186">Klikněte na tlačítko **OK** vytvořit skupinu výstrahy a akce.</span><span class="sxs-lookup"><span data-stu-id="8de7a-186">Click **OK** to create the alert and action group.</span></span>  

<span data-ttu-id="8de7a-187">Webhook se nyní nazývá při vytvoření skupiny prostředků v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="8de7a-187">The webhook is now called when a resource group is created in your subscription.</span></span> <span data-ttu-id="8de7a-188">Potom aktualizujte kód ve vaší funkci pro zpracování dat protokolu JSON v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="8de7a-188">Next, you update the code in your function to handle the JSON log data in the body of the request.</span></span>   

## <a name="update-the-function-code"></a><span data-ttu-id="8de7a-189">Aktualizace kódu funkce</span><span class="sxs-lookup"><span data-stu-id="8de7a-189">Update the function code</span></span>

1. <span data-ttu-id="8de7a-190">Přejděte zpět do aplikace pro funkce na portálu a rozšířit funkce.</span><span class="sxs-lookup"><span data-stu-id="8de7a-190">Navigate back to your function app in the portal, and expand your function.</span></span> 

2. <span data-ttu-id="8de7a-191">Nahraďte kód skriptu jazyka C# ve funkci portálu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="8de7a-191">Replace the C# script code in the function in the portal with the following code:</span></span>

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get the activityLog object from the JSON in the message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if the resource in the activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about the created resource group to the streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

<span data-ttu-id="8de7a-192">Nyní můžete otestovat funkci tak, že vytvoříte novou skupinu prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="8de7a-192">Now you can test the function by creating a new resource group in your subscription.</span></span>

## <a name="test-the-function"></a><span data-ttu-id="8de7a-193">Testování funkce</span><span class="sxs-lookup"><span data-stu-id="8de7a-193">Test the function</span></span>

1. <span data-ttu-id="8de7a-194">Klikněte na ikonu skupiny prostředků v levé části portálu Azure, vyberte **+ přidat**, zadejte **název skupiny prostředků**a vyberte **vytvořit** k vytvoření skupiny prostředků prázdný.</span><span class="sxs-lookup"><span data-stu-id="8de7a-194">Click the resource group icon in the left of the Azure portal, select **+ Add**, type a **Resource group name**, and select **Create** to create an empty resource group.</span></span>
    
    ![Vytvořte skupinu prostředků.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. <span data-ttu-id="8de7a-196">Vraťte se zpátky a funkce a rozbalte **protokoly** okno.</span><span class="sxs-lookup"><span data-stu-id="8de7a-196">Go back to your function and expand the **Logs** window.</span></span> <span data-ttu-id="8de7a-197">Po vytvoření skupiny prostředků webhooku se aktivuje výstraha protokolu aktivity a provádí funkce.</span><span class="sxs-lookup"><span data-stu-id="8de7a-197">After the resource group is created, the activity log alert triggers the webhook and the function executes.</span></span> <span data-ttu-id="8de7a-198">Zobrazí název nové skupiny prostředků zapisují do protokolů.</span><span class="sxs-lookup"><span data-stu-id="8de7a-198">You see the name of the new resource group written to the logs.</span></span>  

    ![Přidáte nastavení aplikace testu.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. <span data-ttu-id="8de7a-200">(Volitelné) Přejděte zpět a odstraňte skupinu prostředků, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8de7a-200">(Optional) Go back and delete the resource group that you created.</span></span> <span data-ttu-id="8de7a-201">Všimněte si, že tato aktivita nebude aktivovat funkce.</span><span class="sxs-lookup"><span data-stu-id="8de7a-201">Note that this activity doesn't trigger the function.</span></span> <span data-ttu-id="8de7a-202">Důvodem je, že odstranění operations filtrují výstraha.</span><span class="sxs-lookup"><span data-stu-id="8de7a-202">This is because delete operations are filtered out by the alert.</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="8de7a-203">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="8de7a-203">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="8de7a-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8de7a-204">Next steps</span></span>

<span data-ttu-id="8de7a-205">Funkce, která se spouští při přijetí požadavku z obecný webhook jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8de7a-205">You have created a function that runs when a request is received from a generic webhook.</span></span> 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="8de7a-206">Další informace o aktivačních událostech webhooků najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="8de7a-206">For more information about webhook triggers, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span> <span data-ttu-id="8de7a-207">Další informace o vývoji funkce v jazyce C#, najdete v části [Azure funkcí jazyka C# skript referenční informace pro vývojáře](functions-reference-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="8de7a-207">To learn more about developing functions in C#, see [Azure Functions C# script developer reference](functions-reference-csharp.md).</span></span>

