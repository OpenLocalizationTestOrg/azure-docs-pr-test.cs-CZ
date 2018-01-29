---
title: "Výběr mezi službami Flow, Logic Apps, Functions a WebJobs | Dokumentace Microsoftu"
description: "Porovnejte cloudové integrační služby od Microsoftu a rozhodněte se, jaké služby byste měli použít."
services: functions,app-service\logic
documentationcenter: na
author: ggailey777
manager: wpickett
tags: 
keywords: microsoft flow, flow, logic apps, azure functions, functions, azure webjobs, webjobs, event processing, dynamic compute, serverless architecture
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7ffe44828735a5687008ebc5a7d8d9f017f49daa
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Výběr mezi službami Flow, Logic Apps, Functions a WebJobs
Tento článek porovnává následující služby v cloudu Microsoftu, které dokáží řešit problémy s integrací a automatizovat obchodní procesy:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure App Service WebJobs](../app-service/web-sites-create-web-jobs.md)

Všechny tyto služby jsou užitečné při spojování různorodých systémů. Všechny můžou definovat vstup, akce, podmínky a výstup. Každou z nich můžete spouštět podle plánu nebo triggerem. Každá služba však zahrnuje jedinečné výhody a jejich porovnání není otázkou, která ze služeb je nejlepší, ale spíše, která služba je nejvhodnější pro danou situaci. Nejlepší způsob, jak rychle sestavit škálovatelné řešení integrace se všemi funkcemi, je často kombinace těchto služeb.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Flow vs. Logic Apps
Služby Microsoft Flow a Azure Logic Apps můžeme probrat společně, protože obě jsou integrační služby *založené na konfiguraci*. Usnadňují sestavování procesů a pracovních postupů a integrují se s různými aplikacemi SaaS a podnikovými aplikacemi. 

* Služba Flow je postavená na službě Logic Apps.
* Obě služby obsahují stejného návrháře pracovních postupů.
* [Konektory](../connectors/apis-list.md) fungující v jedné službě můžou fungovat i v té druhé.

Flow umožňuje každému administrativnímu pracovníkovi provádět jednoduché integrace (například proces schvalování v knihovně dokumentů služby SharePoint) bez nutnosti kontaktovat vývojáře nebo IT oddělení. Logic Apps na druhé straně může povolovat pokročilé integrace (například procesy B2B), kde se vyžadují postupy zabezpečení a DevOps na podnikové úrovni. U obchodních pracovních postupů je typické, že se jejich složitost v průběhu času zvyšuje. Proto můžete nejprve začít s tokem a pak ho podle potřeby převést na aplikaci logiky.

Následující tabulka vám pomůže určit, jestli je pro danou integraci nejlepší Flow nebo Logic Apps.

|  | Tok | Logic Apps |
| --- | --- | --- |
| Cílová skupina |Administrativní pracovníci, podnikoví uživatelé, správci služby SharePoint |Profesionální integrátoři a vývojáře, IT profesionálové |
| Scénáře |Samoobslužné |Pokročilé integrace |
| Návrhářský nástroj |V prohlížeči a mobilní aplikaci, pouze uživatelské rozhraní |V prohlížeči a sadě [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), k dispozici je [zobrazení kódu](../logic-apps/logic-apps-author-definitions.md) |
| Správa životního cyklu aplikací (ALM) |Návrh a testování v neprodukčním prostředí, přesun do produkčního prostředí až po ověření připravenosti. |DevOps: správa zdrojového kódu, testování, podpora, automatizace a spravovatelnost pomocí [správě prostředků Azure](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Prostředí pro správu |Správa zásad toku prostředí a prevence ztráty dat (DLP), sledování licencí [https://admin.flow.microsoft.com](https://admin.flow.microsoft.com) |Správa skupin prostředků, připojení, správy přístupu a protokolování [https://portal.azure.com](https://portal.azure.com) |
| Zabezpečení |Protokoly auditu zabezpečení a dodržování předpisů Office 365, prevence ztráty dat (DLP), [šifrování v klidovém stavu](https://wikipedia.org/wiki/Data_at_rest#Encryption) pro citlivá data atd. |Zajištění zabezpečení Azure: [zabezpečení Azure](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/), [protokoly auditu](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) a další. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Functions vs. Webové úlohy
Služby Azure Functions a Azure App Service WebJobs můžeme probrat společně, protože obě jsou integrační služby *založené na kódu* a navržené pro vývojáře. Umožňují spouštění skriptu nebo části kódu v reakci na různé události, jako jsou [nové objekty blob služby Storage](functions-bindings-storage.md) nebo [požadavky webhooku](functions-bindings-http-webhook.md). Tady jsou jejich podobnosti: 

* Obě jsou postavené na službě [Azure App Service](../app-service/app-service-web-overview.md) a využívají funkce, jako jsou [správa zdrojového kódu](../app-service/app-service-continuous-deployment.md), [ověřování](../app-service/app-service-authentication-overview.md) a [monitorování](../app-service/web-sites-monitor.md).
* Obě služby jsou zaměřené na vývojáře.
* Obě podporují standardní skriptovací a programovací jazyky.
* Obě zahrnují podporu NuGet a npm.

Služba Functions přirozeně navazuje na službu WebJobs v tom smyslu, že ze služby WebJobs přijímá to nejlepší a dále to vylepšuje. Mezi tato vylepšení patří: 

* Model aplikace [bez serveru](https://azure.microsoft.com/overview/serverless-computing/).
* Zjednodušený vývoj, testování a spouštění kódu přímo v prohlížeči.
* Integrace s dalšími službami Azure a službami třetích stran, jako jsou [webhooky GitHubu](https://developer.github.com/webhooks/creating/).
* Platby za použití bez nutnosti platit za [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Automatické [dynamické škálování](functions-scale.md).
* Stávající zákazníci služby App Service můžou nadále využívat plán služby App Service (za účelem využití nevyužitých prostředků).
* Integrace s Logic Apps.

Následující tabulka shrnuje rozdíly mezi službami Functions a WebJobs:

|  | Funkce | Webové úlohy |
| --- | --- | --- |
| Škálování |Škálování bez konfigurace |Škálování s plánem služby App Service |
| Ceny |Platby za použití nebo součástí plánu služby App Service |Součástí plánu služby App Service |
| Typ spouštění |Aktivované, plánované (triggerem časovače) |Aktivované, nepřetržité, plánované |
| Aktivační události |[Časovač](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-cosmosdb.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md), [Azure App Service Mobile Apps](functions-bindings-mobile-apps.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [fronty a objekty blob služby Azure Storage](functions-bindings-storage-blob.md), [fronty a témata služby Azure Service Bus](functions-bindings-service-bus.md) |[fronty a objekty blob služby Azure Storage](functions-bindings-storage-blob.md), [fronty a témata služby Azure Service Bus](functions-bindings-service-bus.md) |
| Vývoj v prohlížeči |Podporuje se |Nepodporuje se |
| C# |Podporuje se |Podporuje se |
| F# |Podporuje se |Nepodporuje se |
| JavaScript |Podporuje se |Podporuje se |
| Java |Preview | Nepodporuje se |
| Bash |Experimentální |Podporuje se |
| Skriptování v systému Windows (.cmd, .bat) |Experimentální |Podporuje se |
| PowerShell |Experimentální |Podporuje se |
| PHP |Experimentální |Podporuje se |
| Python |Experimentální |Podporuje se |
| TypeScript |Experimentální |Nepodporuje se |

Jestli použít Functions nebo WebJobs závisí hlavně na tom, k čemu už používáte službu App Service. Pokud máte aplikaci App Service, pro kterou chcete spouštět fragmenty kódu, a chcete je spravovat společně ve stejném prostředí DevOps, použijte službu WebJobs. V následujících scénářích použijte službu Functions.

* Chcete spouštět fragmenty kódu pro další služby Azure nebo služby třetích stran.
* Chcete spravovat integrační kód odděleně od aplikací App Service.
* Chcete volat fragmenty kódu z aplikace logiky. 

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Služby Flow, Logic Apps a Functions společně
Jak už jsme zmínili, to, která služba je pro vás nejvhodnější, závisí na vaší situaci. 

* Pro jednoduchou optimalizaci podniku použijte službu Flow.
* Pokud je váš scénář integrace příliš pokročilý pro službu Flow nebo pokud potřebujete možnosti DevOps, použijte službu Logic Apps.
* Pokud nějaký krok ve vašem scénáři integrace vyžaduje vysoce přizpůsobenou transformaci nebo specializovaný kód, napište funkci a aktivujte ji jako akci v aplikaci logiky.

Aplikaci logiky můžete volat v rámci toku. Můžete také volat funkci v aplikaci logiky a aplikaci logiky ve funkci. Integrace mezi službami Flow, Logic Apps a Functions se v průběhu času neustále zlepšuje. Můžete něco vytvořit v jedné službě a používat to v ostatních službách. Proto se vyplatí jakákoli investice do těchto tří technologií.

## <a name="next-steps"></a>Další kroky
Začněte s jednotlivými službami vytvořením prvního toku, aplikace logiky, aplikace funkcí nebo webové úlohy. Klikněte na některý z následujících odkazů:

* [Začínáme s Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Vytvoření aplikace logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Vytvoření první funkce Azure](functions-create-first-azure-function.md)
* [Nasazení WebJobs pomocí sady Visual Studio](../app-service/websites-dotnet-deploy-webjobs.md)

Případně si můžete prostudovat další informace o těchto integračních službách na následujících odkazech:

* [Leveraging Azure Functions & Azure App Service for integration scenarios by Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/) (Využití služeb Azure Functions a Azure App Service pro scénáře integrace, autor: Christopher Anderson)
* [Integrations Made Simple by Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/) (Zjednodušená integrace, autor: Charlese Lamanna)
* [Živý webcast o Logic Apps](http://aka.ms/logicappslive)
* [Nejčastější dotazy k Microsoft Flow](https://flow.microsoft.com/documentation/frequently-asked-questions/)

