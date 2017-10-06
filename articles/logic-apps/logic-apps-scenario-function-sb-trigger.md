---
title: "aaaScenario – aplikace logiky aktivační událost s funkcemi Azure a Azure Service Bus | Microsoft Docs"
description: "Vytvoření aplikace logiky tootrigger funkce pomocí funkce Azure a Azure Service Bus"
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
ms.openlocfilehash: a7b78ebcfe492eee2e08ceeae6b9c5f8ed4717bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a><span data-ttu-id="60f3f-103">Scénář: Aktivovat aplikaci logiky s funkcemi Azure a Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="60f3f-103">Scenario: Trigger a logic app with Azure Functions and Azure Service Bus</span></span>

<span data-ttu-id="60f3f-104">Pokud budete potřebovat toodeploy dlouhodobé naslouchacího procesu nebo úlohy, můžete použít Azure Functions toocreate aktivační událost pro aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="60f3f-104">You can use Azure Functions toocreate a trigger for a logic app when you need toodeploy a long-running listener or task.</span></span> <span data-ttu-id="60f3f-105">Můžete například vytvořit funkci, která naslouchá na frontu a okamžitě fire aplikace logiky jako aktivační událost nabízené.</span><span class="sxs-lookup"><span data-stu-id="60f3f-105">For example, you can create a function that listens in on a queue and then immediately fire a logic app as a push trigger.</span></span>

## <a name="build-hello-logic-app"></a><span data-ttu-id="60f3f-106">Vytvoření aplikace logiky hello</span><span class="sxs-lookup"><span data-stu-id="60f3f-106">Build hello logic app</span></span>
<span data-ttu-id="60f3f-107">V tomto příkladu máte funkci systémem pro každou aplikaci logiky, který potřebuje toobe aktivaci.</span><span class="sxs-lookup"><span data-stu-id="60f3f-107">In this example, you have a function running for each logic app that needs toobe triggered.</span></span> <span data-ttu-id="60f3f-108">Nejprve vytvořte aplikaci logiky, který má aktivační procedury pro požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="60f3f-108">First, create a logic app that has an HTTP request trigger.</span></span> <span data-ttu-id="60f3f-109">vždy, když je přijatá zpráva fronty, Funkce Hello volá tohoto koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="60f3f-109">hello function calls that endpoint whenever a queue message is received.</span></span>  

1. <span data-ttu-id="60f3f-110">Vytvoření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="60f3f-110">Create a logic app.</span></span>
2. <span data-ttu-id="60f3f-111">Vyberte hello **ruční – když je obdržena požadavek HTTP** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="60f3f-111">Select hello **Manual - When an HTTP Request is Received** trigger.</span></span>
   <span data-ttu-id="60f3f-112">Volitelně můžete zadat toouse schématu JSON s uvítací zprávu fronty pomocí některého nástroje, například [jsonschema.net](http://jsonschema.net).</span><span class="sxs-lookup"><span data-stu-id="60f3f-112">Optionally, you can specify a JSON schema toouse with hello queue message by using a tool like [jsonschema.net](http://jsonschema.net).</span></span> <span data-ttu-id="60f3f-113">Vložte hello schématu hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="60f3f-113">Paste hello schema in hello trigger.</span></span> <span data-ttu-id="60f3f-114">Schémata pomoct pochopit hello tvar vlastnosti data a toku hello snadněji prostřednictvím hello pracovního postupu návrháře hello.</span><span class="sxs-lookup"><span data-stu-id="60f3f-114">Schemas help hello designer understand hello shape of hello data and flow properties more easily through hello workflow.</span></span>
2. <span data-ttu-id="60f3f-115">Přidáte žádné další kroky, které mají toooccur po přijetí zprávy fronty.</span><span class="sxs-lookup"><span data-stu-id="60f3f-115">Add any additional steps that you want toooccur after a queue message is received.</span></span> <span data-ttu-id="60f3f-116">Například odešlete e-mailu prostřednictvím Office 365.</span><span class="sxs-lookup"><span data-stu-id="60f3f-116">For example, send an email via Office 365.</span></span>  
3. <span data-ttu-id="60f3f-117">Uložte hello logiku toogenerate hello zpětného volání adresu URL aplikace pro aplikaci logiky toothis hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="60f3f-117">Save hello logic app toogenerate hello callback URL for hello trigger toothis logic app.</span></span> <span data-ttu-id="60f3f-118">Hello URL se objeví na kartě hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="60f3f-118">hello URL appears on hello trigger card.</span></span>

![Hello zpětného volání adresy URL se zobrazí na kartě aktivační událost hello][1]

## <a name="build-hello-function"></a><span data-ttu-id="60f3f-120">Vytvořte funkci hello</span><span class="sxs-lookup"><span data-stu-id="60f3f-120">Build hello function</span></span>
<span data-ttu-id="60f3f-121">Dále musíte vytvořit funkci, která funguje jako hello aktivační události a naslouchá toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="60f3f-121">Next, you must create a function that acts as hello trigger and listens toohello queue.</span></span>

1. <span data-ttu-id="60f3f-122">V hello [portálu Azure Functions](https://functions.azure.com/signin), vyberte **novou funkci**a potom vyberte hello **ServiceBusQueueTrigger - C#** šablony.</span><span class="sxs-lookup"><span data-stu-id="60f3f-122">In hello [Azure Functions portal](https://functions.azure.com/signin), select **New Function**, and then select hello **ServiceBusQueueTrigger - C#** template.</span></span>
   
    ![Azure Functions na portálu][2]
2. <span data-ttu-id="60f3f-124">Konfigurace hello připojení toohello fronty Service Bus, která používá hello Azure Service Bus SDK `OnMessageReceive()` naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="60f3f-124">Configure hello connection toohello Service Bus queue, which uses hello Azure Service Bus SDK `OnMessageReceive()` listener.</span></span>
3. <span data-ttu-id="60f3f-125">Základní funkce toocall hello logiku aplikace koncový bod (vytvořenou dříve) zapsat pomocí zprávu fronty hello jako trigger.</span><span class="sxs-lookup"><span data-stu-id="60f3f-125">Write a basic function toocall hello logic app endpoint (created earlier) by using hello queue message as a trigger.</span></span> <span data-ttu-id="60f3f-126">Tady je příklad úplné funkce.</span><span class="sxs-lookup"><span data-stu-id="60f3f-126">Here's a full example of a function.</span></span> <span data-ttu-id="60f3f-127">Příklad Hello používá `application/json` obsahu typ zprávy, ale můžete změnit podle potřeby tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="60f3f-127">hello example uses an `application/json` message content type, but you can change this type as necessary.</span></span>
   
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

<span data-ttu-id="60f3f-128">tootest, přidat zprávu fronty pomocí nástroje podobného [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span><span class="sxs-lookup"><span data-stu-id="60f3f-128">tootest, add a queue message via a tool like [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span></span> <span data-ttu-id="60f3f-129">V tématu aplikace logiky hello fire ihned po hello funkce přijme zprávu hello.</span><span class="sxs-lookup"><span data-stu-id="60f3f-129">See hello logic app fire immediately after hello function receives hello message.</span></span>

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
