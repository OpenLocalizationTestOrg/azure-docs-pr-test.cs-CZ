---
title: "Vlastní kód pro Azure Logic Apps s Azure Functions | Microsoft Docs"
description: "Vytvoření a spuštění vlastní kód pro Azure Logic Apps s Azure Functions"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 18442c87b049200fac5ed41cc7034ba7a848b8d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a><span data-ttu-id="3cfbf-103">Přidejte a spuštění vlastní kód pro logic apps prostřednictvím Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3cfbf-103">Add and run custom code for logic apps through Azure Functions</span></span>

<span data-ttu-id="3cfbf-104">Spustit vlastní fragmenty kódu C# nebo node.js v aplikace logiky, můžete vytvořit vlastní funkce prostřednictvím Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-104">To run custom snippets of C# or node.js in logic apps, you can create custom functions through Azure Functions.</span></span> 
<span data-ttu-id="3cfbf-105">[Azure Functions](../azure-functions/functions-overview.md) nabízí práci bez serveru v Microsoft Azure a jsou užitečné pro provádění těchto úkolů:</span><span class="sxs-lookup"><span data-stu-id="3cfbf-105">[Azure Functions](../azure-functions/functions-overview.md) offers server-free computing in Microsoft Azure and are useful for performing these tasks:</span></span>

* <span data-ttu-id="3cfbf-106">Pokročilé formátování nebo výpočetní polí v aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="3cfbf-106">Advanced formatting or compute of fields in logic apps</span></span>
* <span data-ttu-id="3cfbf-107">Provádění výpočtů v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-107">Perform calculations in a workflow.</span></span>
* <span data-ttu-id="3cfbf-108">Rozšířit funkce aplikace logiky s funkcemi, které jsou podporovány v C# nebo node.js</span><span class="sxs-lookup"><span data-stu-id="3cfbf-108">Extend the logic app functionality with functions that are supported in C# or node.js</span></span>

## <a name="create-custom-functions-for-your-logic-apps"></a><span data-ttu-id="3cfbf-109">Vytvoření vlastní funkce pro logic apps</span><span class="sxs-lookup"><span data-stu-id="3cfbf-109">Create custom functions for your logic apps</span></span>

<span data-ttu-id="3cfbf-110">Doporučujeme vytvořit funkce na portálu Azure Functions z **obecný Webhook - uzlu** nebo **obecný Webhook - C#** šablony.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-110">We recommend that you create a function in the Azure Functions portal, from the **Generic Webhook - Node** or **Generic Webhook - C#** templates.</span></span> <span data-ttu-id="3cfbf-111">Výsledek vytvoří automaticky vyplněna šablonu, která přijímá `application/json` z aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-111">The result creates an auto-populated a template that accepts `application/json` from a logic app.</span></span> <span data-ttu-id="3cfbf-112">Funkce, které můžete vytvořit z těchto šablon se automaticky zjišťují a zobrazí v návrháři aplikace logiky v rámci **Azure Functions v mojí oblasti.**</span><span class="sxs-lookup"><span data-stu-id="3cfbf-112">Functions that you create from these templates are automatically detected and appear in the Logic App Designer under **Azure Functions in my region.**</span></span>

<span data-ttu-id="3cfbf-113">Na portálu Azure na **integrací** v podokně funkce, vaše šablona by měla ukazují, že **režimu** nastavena na **Webhooku** a **Webhooku typ** je nastaven na **obecné JSON**.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-113">In the Azure portal, on the **Integrate** pane for your function, your template should show that **Mode** set to **Webhook** and **Webhook type** is set to **Generic JSON**.</span></span> 

<span data-ttu-id="3cfbf-114">Funkce Webhooku žádost o přijmout a předejte ji do metody prostřednictvím `data` proměnné.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-114">Webhook functions accept a request and pass it into the method via a `data` variable.</span></span> <span data-ttu-id="3cfbf-115">Máte přístup k vlastnosti vaše datové části pomocí zápisu s tečkou jako `data.function-name`.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-115">You can access the properties of your payload by using dot notation like `data.function-name`.</span></span> <span data-ttu-id="3cfbf-116">Například jednoduché funkce JavaScript, která převede hodnotu data a času na datum řetězec vypadá jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3cfbf-116">For example, a simple JavaScript function that converts a DateTime value into a date string looks like the following example:</span></span>

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a><span data-ttu-id="3cfbf-117">Azure Functions volání z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="3cfbf-117">Call Azure Functions from logic apps</span></span>

<span data-ttu-id="3cfbf-118">Seznam kontejnery v rámci vašeho předplatného a vyberte funkci, kterou chcete volat v návrháři aplikace logiky, klikněte na tlačítko **akce** nabídce a vyberte z **Azure Functions v mojí oblasti**.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-118">To list the containers in your subscription and select the function that you want to call, in Logic App Designer, click the **Actions** menu, and select from **Azure Functions in my Region**.</span></span>

<span data-ttu-id="3cfbf-119">Po výběru funkce, budete vyzváni k zadání datová část vstupního objektu.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-119">After you select the function, you are asked to specify an input payload object.</span></span> <span data-ttu-id="3cfbf-120">Tento objekt je zpráva, že aplikace logiky odešle do funkce a musí být objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-120">This object is the message that the logic app sends to the function and must be a JSON object.</span></span> <span data-ttu-id="3cfbf-121">Například, pokud chcete předat **poslední změny** data ze služby Salesforce aktivační událost, datové části funkce může vypadat jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="3cfbf-121">For example, if you want to pass in the **Last Modified** date from a Salesforce trigger, the function payload might look like this example:</span></span>

![Datum poslední změny][1]

## <a name="trigger-logic-apps-from-a-function"></a><span data-ttu-id="3cfbf-123">Aplikace logiky aktivační události z funkce</span><span class="sxs-lookup"><span data-stu-id="3cfbf-123">Trigger logic apps from a function</span></span>

<span data-ttu-id="3cfbf-124">Můžete aktivovat aplikaci logiky z uvnitř funkce.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-124">You can trigger a logic app from inside a function.</span></span> <span data-ttu-id="3cfbf-125">V tématu [Logic apps jako koncové body s](logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="3cfbf-125">See [Logic apps as callable endpoints](logic-apps-http-endpoint.md).</span></span> <span data-ttu-id="3cfbf-126">Vytvoření aplikace logiky, který má aktivační události ruční a potom z uvnitř vaší funkce generování HTTP POST na ruční aktivaci, adresa URL s odebranou datovou částí, který chcete odeslat do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-126">Create a logic app that has a manual trigger, then from inside your function, generate an HTTP POST to the manual trigger URL with the payload that you want sent to the logic app.</span></span>

### <a name="create-a-function-from-logic-app-designer"></a><span data-ttu-id="3cfbf-127">Vytvoření funkce z návrháře aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="3cfbf-127">Create a function from Logic App Designer</span></span>

<span data-ttu-id="3cfbf-128">Můžete také vytvořit funkci webhooku node.js z návrháře.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-128">You can also create a node.js webhook function from the designer.</span></span> <span data-ttu-id="3cfbf-129">Nejdřív vyberte **Azure Functions v mojí oblasti** a potom vyberte kontejner pro funkce.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-129">First, select **Azure Functions in my Region,** and then choose a container for your function.</span></span> <span data-ttu-id="3cfbf-130">Pokud ještě nemáte kontejner, budete muset vytvořit z [portálu Azure Functions](https://functions.azure.com/signin).</span><span class="sxs-lookup"><span data-stu-id="3cfbf-130">If you don't yet have a container, you need to create one from the [Azure Functions portal](https://functions.azure.com/signin).</span></span> <span data-ttu-id="3cfbf-131">Potom vyberte **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-131">Then select **Create New**.</span></span>  

<span data-ttu-id="3cfbf-132">Chcete-li vygenerovat šablony založené na datech, které chcete vypočítat, zadejte objektu context, který chcete předat do funkce.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-132">To generate a template based on the data that you want to compute, specify the context object that you plan to pass into a function.</span></span> <span data-ttu-id="3cfbf-133">Tento objekt musí být objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-133">This object must be a JSON object.</span></span> <span data-ttu-id="3cfbf-134">Například pokud předáte obsah souboru z akce FTP, datové části kontextu vypadá v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="3cfbf-134">For example, if you pass in the file content from an FTP action, the context payload looks like this example:</span></span>

![Datová část kontextu][2]

> [!NOTE]
> <span data-ttu-id="3cfbf-136">Protože tento objekt nebyl přetypovat jako řetězec, obsah se přidá přímo do datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-136">Because this object wasn't cast as a string, the content is added directly to the JSON payload.</span></span> <span data-ttu-id="3cfbf-137">Však dojde k chybě, pokud objekt není token JSON (to znamená, že řetězec nebo JSON objekt nebo pole).</span><span class="sxs-lookup"><span data-stu-id="3cfbf-137">However, an error occurs if the object is not a JSON token (that is, a string or a JSON object/array).</span></span> <span data-ttu-id="3cfbf-138">Převést objekt jako řetězec, přidáte uvozovky, jak je znázorněno v první obrázek v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-138">To cast the object as a string, add quotes as shown in the first illustration in this article.</span></span>
> 

<span data-ttu-id="3cfbf-139">Návrhář potom vytvoří šablonu funkce, můžete vytvořit vložený.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-139">The designer then generates a function template that you can create inline.</span></span> <span data-ttu-id="3cfbf-140">Proměnné jsou předem vytvořené podle kontextu, kterou chcete použít k předání do funkce.</span><span class="sxs-lookup"><span data-stu-id="3cfbf-140">Variables are pre-created based on the context that you plan to pass into the function.</span></span>

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
