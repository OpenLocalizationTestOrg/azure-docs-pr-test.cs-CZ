---
title: "Scénář – aplikace logiky aktivační událost s funkcemi Azure a Azure Service Bus | Microsoft Docs"
description: "Vytvořit funkci pro aktivaci aplikace logiky pomocí funkce Azure a Azure Service Bus"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/23/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 088f10bc32dd492f82f0a10a7e5829e76f588758
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a><span data-ttu-id="7dc4f-103">Scénář: Aktivovat aplikaci logiky s funkcemi Azure a Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="7dc4f-103">Scenario: Trigger a logic app with Azure Functions and Azure Service Bus</span></span>

<span data-ttu-id="7dc4f-104">Azure Functions můžete vytvořit aktivační událost pro aplikace logiky, když potřebujete nasadit dlouho běžící naslouchacího procesu nebo úlohy.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-104">You can use Azure Functions to create a trigger for a logic app when you need to deploy a long-running listener or task.</span></span> <span data-ttu-id="7dc4f-105">Můžete například vytvořit funkci, která naslouchá na frontu a okamžitě fire aplikace logiky jako aktivační událost nabízené.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-105">For example, you can create a function that listens in on a queue and then immediately fire a logic app as a push trigger.</span></span>

## <a name="build-the-logic-app"></a><span data-ttu-id="7dc4f-106">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="7dc4f-106">Build the logic app</span></span>
<span data-ttu-id="7dc4f-107">V tomto příkladu máte funkci pro každou aplikaci logiky, která musí být aktivována systémem.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-107">In this example, you have a function running for each logic app that needs to be triggered.</span></span> <span data-ttu-id="7dc4f-108">Nejprve vytvořte aplikaci logiky, který má aktivační procedury pro požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-108">First, create a logic app that has an HTTP request trigger.</span></span> <span data-ttu-id="7dc4f-109">Vždy, když je přijatá zpráva fronty, zavolá funkci tohoto koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-109">The function calls that endpoint whenever a queue message is received.</span></span>  

1. <span data-ttu-id="7dc4f-110">Vytvoření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-110">Create a logic app.</span></span>
2. <span data-ttu-id="7dc4f-111">Vyberte **ruční – když je obdržena požadavek HTTP** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-111">Select the **Manual - When an HTTP Request is Received** trigger.</span></span>
   <span data-ttu-id="7dc4f-112">Volitelně můžete zadat schématu JSON pro použití s zprávy ve frontě pomocí některého nástroje, například [jsonschema.net](http://jsonschema.net).</span><span class="sxs-lookup"><span data-stu-id="7dc4f-112">Optionally, you can specify a JSON schema to use with the queue message by using a tool like [jsonschema.net](http://jsonschema.net).</span></span> <span data-ttu-id="7dc4f-113">Vložte schéma aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-113">Paste the schema in the trigger.</span></span> <span data-ttu-id="7dc4f-114">Schémata pomoct pochopit obrazec vlastnosti data a toku snadněji prostřednictvím pracovního postupu návrháře.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-114">Schemas help the designer understand the shape of the data and flow properties more easily through the workflow.</span></span>
2. <span data-ttu-id="7dc4f-115">Přidáte další kroky, které mají být provedeny po přijetí zprávy fronty.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-115">Add any additional steps that you want to occur after a queue message is received.</span></span> <span data-ttu-id="7dc4f-116">Například odešlete e-mailu prostřednictvím Office 365.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-116">For example, send an email via Office 365.</span></span>  
3. <span data-ttu-id="7dc4f-117">Uložte aplikaci logiky ke generování adresy URL zpětného volání pro aktivační událost pro tuto aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-117">Save the logic app to generate the callback URL for the trigger to this logic app.</span></span> <span data-ttu-id="7dc4f-118">Adresa URL se zobrazí na kartě aktivační události.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-118">The URL appears on the trigger card.</span></span>

![Zpětné volání adresy URL se zobrazí na kartě aktivační události][1]

## <a name="build-the-function"></a><span data-ttu-id="7dc4f-120">Vytvoření funkce</span><span class="sxs-lookup"><span data-stu-id="7dc4f-120">Build the function</span></span>
<span data-ttu-id="7dc4f-121">Dále musíte vytvořit funkci, která funguje jako aktivační události a naslouchá do fronty.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-121">Next, you must create a function that acts as the trigger and listens to the queue.</span></span>

1. <span data-ttu-id="7dc4f-122">V [portálu Azure Functions](https://functions.azure.com/signin), vyberte **novou funkci**a pak vyberte **ServiceBusQueueTrigger - C#** šablony.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-122">In the [Azure Functions portal](https://functions.azure.com/signin), select **New Function**, and then select the **ServiceBusQueueTrigger - C#** template.</span></span>
   
    ![Azure Functions na portálu][2]
2. <span data-ttu-id="7dc4f-124">Konfigurace připojení do fronty Service Bus, která používá Azure Service Bus SDK `OnMessageReceive()` naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-124">Configure the connection to the Service Bus queue, which uses the Azure Service Bus SDK `OnMessageReceive()` listener.</span></span>
3. <span data-ttu-id="7dc4f-125">Write – základní funkce k volání koncový bod aplikace logiky (vytvořenou dříve) pomocí zprávy ve frontě aktivační události.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-125">Write a basic function to call the logic app endpoint (created earlier) by using the queue message as a trigger.</span></span> <span data-ttu-id="7dc4f-126">Tady je příklad úplné funkce.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-126">Here's a full example of a function.</span></span> <span data-ttu-id="7dc4f-127">V příkladu se používá `application/json` obsahu typ zprávy, ale můžete změnit podle potřeby tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-127">The example uses an `application/json` message content type, but you can change this type as necessary.</span></span>
   
   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
   
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

<span data-ttu-id="7dc4f-128">Chcete-li otestovat, přidejte zprávu fronty pomocí některého nástroje, například [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span><span class="sxs-lookup"><span data-stu-id="7dc4f-128">To test, add a queue message via a tool like [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span></span> <span data-ttu-id="7dc4f-129">V tématu aplikace logiky fire ihned po funkce obdrží zprávu.</span><span class="sxs-lookup"><span data-stu-id="7dc4f-129">See the logic app fire immediately after the function receives the message.</span></span>

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
