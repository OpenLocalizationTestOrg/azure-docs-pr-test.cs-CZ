---
title: "aaaCustom kód pro Azure Logic Apps s Azure Functions | Microsoft Docs"
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
ms.openlocfilehash: 18b3821ed7e434feb568b9b96e9a5a2189dba3bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a><span data-ttu-id="ba40c-103">Přidejte a spuštění vlastní kód pro logic apps prostřednictvím Azure Functions</span><span class="sxs-lookup"><span data-stu-id="ba40c-103">Add and run custom code for logic apps through Azure Functions</span></span>

<span data-ttu-id="ba40c-104">toorun vlastní fragmenty kódu C# nebo node.js v aplikace logiky, můžete vytvořit vlastní funkce prostřednictvím Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="ba40c-104">toorun custom snippets of C# or node.js in logic apps, you can create custom functions through Azure Functions.</span></span> 
<span data-ttu-id="ba40c-105">[Azure Functions](../azure-functions/functions-overview.md) nabízí práci bez serveru v Microsoft Azure a jsou užitečné pro provádění těchto úkolů:</span><span class="sxs-lookup"><span data-stu-id="ba40c-105">[Azure Functions](../azure-functions/functions-overview.md) offers server-free computing in Microsoft Azure and are useful for performing these tasks:</span></span>

* <span data-ttu-id="ba40c-106">Pokročilé formátování nebo výpočetní polí v aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="ba40c-106">Advanced formatting or compute of fields in logic apps</span></span>
* <span data-ttu-id="ba40c-107">Provádění výpočtů v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="ba40c-107">Perform calculations in a workflow.</span></span>
* <span data-ttu-id="ba40c-108">Rozšířit funkce aplikace logiky hello s funkcemi, které jsou podporovány v C# nebo node.js</span><span class="sxs-lookup"><span data-stu-id="ba40c-108">Extend hello logic app functionality with functions that are supported in C# or node.js</span></span>

## <a name="create-custom-functions-for-your-logic-apps"></a><span data-ttu-id="ba40c-109">Vytvoření vlastní funkce pro logic apps</span><span class="sxs-lookup"><span data-stu-id="ba40c-109">Create custom functions for your logic apps</span></span>

<span data-ttu-id="ba40c-110">Doporučujeme vytvořit funkci Azure Functions portálu hello z hello **obecný Webhook - uzlu** nebo **obecný Webhook - C#** šablony.</span><span class="sxs-lookup"><span data-stu-id="ba40c-110">We recommend that you create a function in hello Azure Functions portal, from hello **Generic Webhook - Node** or **Generic Webhook - C#** templates.</span></span> <span data-ttu-id="ba40c-111">výsledek Hello vytvoří automaticky vyplněna šablonu, která přijímá `application/json` z aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ba40c-111">hello result creates an auto-populated a template that accepts `application/json` from a logic app.</span></span> <span data-ttu-id="ba40c-112">Funkce, které můžete vytvořit z těchto šablon se automaticky zjišťují a zobrazují v hello návrhář aplikace logiky v rámci **Azure Functions v mojí oblasti.**</span><span class="sxs-lookup"><span data-stu-id="ba40c-112">Functions that you create from these templates are automatically detected and appear in hello Logic App Designer under **Azure Functions in my region.**</span></span>

<span data-ttu-id="ba40c-113">V portálu Azure, na hello hello **integrací** v podokně funkce, vaše šablona by měla ukazují, že **režimu** nastavit příliš**Webhooku** a **Webhooku typ** je nastaven příliš**obecné JSON**.</span><span class="sxs-lookup"><span data-stu-id="ba40c-113">In hello Azure portal, on hello **Integrate** pane for your function, your template should show that **Mode** set too**Webhook** and **Webhook type** is set too**Generic JSON**.</span></span> 

<span data-ttu-id="ba40c-114">Funkce Webhooku žádost o přijmout a předejte ji do metody hello přes `data` proměnné.</span><span class="sxs-lookup"><span data-stu-id="ba40c-114">Webhook functions accept a request and pass it into hello method via a `data` variable.</span></span> <span data-ttu-id="ba40c-115">Máte přístup k vlastnosti hello vaše datové části pomocí zápisu s tečkou jako `data.function-name`.</span><span class="sxs-lookup"><span data-stu-id="ba40c-115">You can access hello properties of your payload by using dot notation like `data.function-name`.</span></span> <span data-ttu-id="ba40c-116">Například jednoduché funkce JavaScript, která převede hodnotu data a času na řetězec datum vypadá hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="ba40c-116">For example, a simple JavaScript function that converts a DateTime value into a date string looks like hello following example:</span></span>

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a><span data-ttu-id="ba40c-117">Azure Functions volání z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="ba40c-117">Call Azure Functions from logic apps</span></span>

<span data-ttu-id="ba40c-118">kontejnery hello toolist v vaše předplatné a vyberte hello funkce, které chcete toocall v návrháři aplikace logiky, klikněte na tlačítko hello **akce** nabídce a vyberte z **Azure Functions v mojí oblasti**.</span><span class="sxs-lookup"><span data-stu-id="ba40c-118">toolist hello containers in your subscription and select hello function that you want toocall, in Logic App Designer, click hello **Actions** menu, and select from **Azure Functions in my Region**.</span></span>

<span data-ttu-id="ba40c-119">Po výběru hello funkce, se zobrazí výzva toospecify datová část vstupního objektu.</span><span class="sxs-lookup"><span data-stu-id="ba40c-119">After you select hello function, you are asked toospecify an input payload object.</span></span> <span data-ttu-id="ba40c-120">Tento objekt je uvítací zprávu, která hello logiku aplikace odesílá toohello funkce a musí být objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="ba40c-120">This object is hello message that hello logic app sends toohello function and must be a JSON object.</span></span> <span data-ttu-id="ba40c-121">Například, pokud chcete, aby toopass v hello **poslední změny** data ze služby Salesforce aktivační událost, datové funkce hello může vypadat jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="ba40c-121">For example, if you want toopass in hello **Last Modified** date from a Salesforce trigger, hello function payload might look like this example:</span></span>

![Datum poslední změny][1]

## <a name="trigger-logic-apps-from-a-function"></a><span data-ttu-id="ba40c-123">Aplikace logiky aktivační události z funkce</span><span class="sxs-lookup"><span data-stu-id="ba40c-123">Trigger logic apps from a function</span></span>

<span data-ttu-id="ba40c-124">Můžete aktivovat aplikaci logiky z uvnitř funkce.</span><span class="sxs-lookup"><span data-stu-id="ba40c-124">You can trigger a logic app from inside a function.</span></span> <span data-ttu-id="ba40c-125">V tématu [Logic apps jako koncové body s](logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="ba40c-125">See [Logic apps as callable endpoints](logic-apps-http-endpoint.md).</span></span> <span data-ttu-id="ba40c-126">Vytvoření aplikace logiky, který má aktivační události ruční a potom z uvnitř vaší funkce generovat adresu URL ruční aktivační událost toohello HTTP POST s odebranou datovou částí hello, který chcete odeslat toohello aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ba40c-126">Create a logic app that has a manual trigger, then from inside your function, generate an HTTP POST toohello manual trigger URL with hello payload that you want sent toohello logic app.</span></span>

### <a name="create-a-function-from-logic-app-designer"></a><span data-ttu-id="ba40c-127">Vytvoření funkce z návrháře aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="ba40c-127">Create a function from Logic App Designer</span></span>

<span data-ttu-id="ba40c-128">Můžete také vytvořit funkci webhooku node.js z Návrháře hello.</span><span class="sxs-lookup"><span data-stu-id="ba40c-128">You can also create a node.js webhook function from hello designer.</span></span> <span data-ttu-id="ba40c-129">Nejdřív vyberte **Azure Functions v mojí oblasti** a potom vyberte kontejner pro funkce.</span><span class="sxs-lookup"><span data-stu-id="ba40c-129">First, select **Azure Functions in my Region,** and then choose a container for your function.</span></span> <span data-ttu-id="ba40c-130">Pokud ještě nemáte kontejner, je třeba toocreate jeden z hello [portálu Azure Functions](https://functions.azure.com/signin).</span><span class="sxs-lookup"><span data-stu-id="ba40c-130">If you don't yet have a container, you need toocreate one from hello [Azure Functions portal](https://functions.azure.com/signin).</span></span> <span data-ttu-id="ba40c-131">Potom vyberte **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="ba40c-131">Then select **Create New**.</span></span>  

<span data-ttu-id="ba40c-132">toogenerate šablonu na základě hello dat, které chcete toocompute, zadejte objekt kontextu hello plánování toopass do funkce.</span><span class="sxs-lookup"><span data-stu-id="ba40c-132">toogenerate a template based on hello data that you want toocompute, specify hello context object that you plan toopass into a function.</span></span> <span data-ttu-id="ba40c-133">Tento objekt musí být objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="ba40c-133">This object must be a JSON object.</span></span> <span data-ttu-id="ba40c-134">Například pokud předáte obsah souboru hello z akce FTP, datové části kontextu hello vypadá v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="ba40c-134">For example, if you pass in hello file content from an FTP action, hello context payload looks like this example:</span></span>

![Datová část kontextu][2]

> [!NOTE]
> <span data-ttu-id="ba40c-136">Protože tento objekt nebyl přetypovat jako řetězec, se přidá hello obsah přímo toohello datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="ba40c-136">Because this object wasn't cast as a string, hello content is added directly toohello JSON payload.</span></span> <span data-ttu-id="ba40c-137">Však dojde k chybě, pokud objekt hello není token JSON (to znamená, že řetězec nebo JSON objekt nebo pole).</span><span class="sxs-lookup"><span data-stu-id="ba40c-137">However, an error occurs if hello object is not a JSON token (that is, a string or a JSON object/array).</span></span> <span data-ttu-id="ba40c-138">toocast hello objekt jako řetězec, přidávat uvozovky, jak je znázorněno v hello první obrázek v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ba40c-138">toocast hello object as a string, add quotes as shown in hello first illustration in this article.</span></span>
> 

<span data-ttu-id="ba40c-139">Návrhář Hello potom vytvoří šablonu funkce, můžete vytvořit vložený.</span><span class="sxs-lookup"><span data-stu-id="ba40c-139">hello designer then generates a function template that you can create inline.</span></span> <span data-ttu-id="ba40c-140">Proměnné jsou předem vytvořené podle kontextu hello plánování toopass do funkce hello.</span><span class="sxs-lookup"><span data-stu-id="ba40c-140">Variables are pre-created based on hello context that you plan toopass into hello function.</span></span>

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
