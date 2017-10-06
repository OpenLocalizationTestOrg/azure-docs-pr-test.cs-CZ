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
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a>Scénář: Aktivovat aplikaci logiky s funkcemi Azure a Azure Service Bus

Pokud budete potřebovat toodeploy dlouhodobé naslouchacího procesu nebo úlohy, můžete použít Azure Functions toocreate aktivační událost pro aplikace logiky. Můžete například vytvořit funkci, která naslouchá na frontu a okamžitě fire aplikace logiky jako aktivační událost nabízené.

## <a name="build-hello-logic-app"></a>Vytvoření aplikace logiky hello
V tomto příkladu máte funkci systémem pro každou aplikaci logiky, který potřebuje toobe aktivaci. Nejprve vytvořte aplikaci logiky, který má aktivační procedury pro požadavek HTTP. vždy, když je přijatá zpráva fronty, Funkce Hello volá tohoto koncového bodu.  

1. Vytvoření aplikace logiky.
2. Vyberte hello **ruční – když je obdržena požadavek HTTP** aktivační události.
   Volitelně můžete zadat toouse schématu JSON s uvítací zprávu fronty pomocí některého nástroje, například [jsonschema.net](http://jsonschema.net). Vložte hello schématu hello aktivační události. Schémata pomoct pochopit hello tvar vlastnosti data a toku hello snadněji prostřednictvím hello pracovního postupu návrháře hello.
2. Přidáte žádné další kroky, které mají toooccur po přijetí zprávy fronty. Například odešlete e-mailu prostřednictvím Office 365.  
3. Uložte hello logiku toogenerate hello zpětného volání adresu URL aplikace pro aplikaci logiky toothis hello aktivační události. Hello URL se objeví na kartě hello aktivační události.

![Hello zpětného volání adresy URL se zobrazí na kartě aktivační událost hello][1]

## <a name="build-hello-function"></a>Vytvořte funkci hello
Dále musíte vytvořit funkci, která funguje jako hello aktivační události a naslouchá toohello fronty.

1. V hello [portálu Azure Functions](https://functions.azure.com/signin), vyberte **novou funkci**a potom vyberte hello **ServiceBusQueueTrigger - C#** šablony.
   
    ![Azure Functions na portálu][2]
2. Konfigurace hello připojení toohello fronty Service Bus, která používá hello Azure Service Bus SDK `OnMessageReceive()` naslouchací proces.
3. Základní funkce toocall hello logiku aplikace koncový bod (vytvořenou dříve) zapsat pomocí zprávu fronty hello jako trigger. Tady je příklad úplné funkce. Příklad Hello používá `application/json` obsahu typ zprávy, ale můžete změnit podle potřeby tohoto typu.
   
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

tootest, přidat zprávu fronty pomocí nástroje podobného [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer). V tématu aplikace logiky hello fire ihned po hello funkce přijme zprávu hello.

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
