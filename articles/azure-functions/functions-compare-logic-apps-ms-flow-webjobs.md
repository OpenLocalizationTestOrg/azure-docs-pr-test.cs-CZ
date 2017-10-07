---
title: "aaaChoose mezi toku, Logic Apps, funkce a webové úlohy | Microsoft Docs"
description: "Porovnání a kontrast hello cloudu integračních služeb od společnosti Microsoft a rozhodnout, které služeb, měli byste použít."
services: functions,app-service\logic
documentationcenter: na
author: cephalin
manager: wpickett
tags: 
keywords: "Microsoft toku, tok, aplikace logiky, azure funkce, funkce, azure webjobs webové úlohy, událostí zpracování, dynamické výpočetní architektura bez serveru"
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 6becc1e389698e517924b18295dac4375542d524
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Výběr mezi službami Flow, Logic Apps, Functions a WebJobs
Tento článek obsahuje porovnání a hello následujících služeb v cloudu Microsoft hello, což může všechny vyřešit problémy integraci a automatizaci obchodních procesů se liší od:

* [Microsoft toku](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Aplikační služba Azure WebJobs](../app-service-web/web-sites-create-web-jobs.md)

Všechny tyto služby jsou užitečné, když "připevnění" společně různorodých systémů. Že všechny definují, vstup, akce, podmínky a výstup. Každý z nich můžete spustit na plán nebo aktivační událost. Ale každé služby přidá jedinečnou sadu hodnotu a jejich porovnání není dotaz "služby, která je nejlepší hello?" ale jeden z "služby, která nejlépe vyhovuje v tomto případě?" Často kombinaci těchto služeb je nejlepším způsobem hello toorapidly vytvoření řešení škálovatelné a úplné integraci vybrané.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Tok vs. Logic Apps
Můžete probereme Flow Microsoft a Azure Logic Apps společně vzhledem k tomu, že jsou oba *konfigurace první* integrační služby, které umožňuje snadno toobuild procesy a pracovních postupů a integraci s různými SaaS a enterprise aplikace. 

* Tok je postavená na Logic Apps
* Mají hello stejné Návrhář postupu provádění
* [Konektory](../connectors/apis-list.md) že práci v jednom může spolupracovat taky v jiných hello

Toky umožňuje žádné Jednoduchá integrace office pracovní tooperform (např. získat SMS pro důležité e-mailů) bez průchodu přes vývojáři nebo IT. Na dobrý den druhé straně, Logic Apps můžete povolit rozšířené nebo důležité integrace (např. procesy B2B), kde jsou vyžadovány postupy zabezpečení a DevOps podnikové úrovni. Je typické pro toogrow pracovního postupu obchodní přesčasová složitost. Podle toho můžete začít s tokem na první a pak jej podle potřeby převod tooa aplikace logiky.

Hello následující tabulka vám pomůže určit, zda je nejvhodnější pro danou integrace toku nebo Logic Apps.

|  | Tok | Logic Apps |
| --- | --- | --- |
| Cílová skupina |Pracovníci kanceláře, podnikoví uživatelé |IT specialisté, vývojáři |
| Scénáře |Samoobslužné služby |Důležité |
| Nástroj pro návrh |V prohlížeči a mobilní aplikace, jenom uživatelského rozhraní |V prohlížeči a [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [Code zobrazení](../logic-apps/logic-apps-author-definitions.md) k dispozici |
| DevOps |Ad-hoc, vytvořte v produkčním prostředí |Zdroj ovládacího prvku, testování, podpora a automatizace a možností správy v [Správa prostředků Azure](../logic-apps/logic-apps-arm-provision.md) |
| Z pohledu správce |[https://Flow.microsoft.com](https://flow.microsoft.com) |[https://Portal.Azure.com](https://portal.azure.com) |
| Zabezpečení |Standardní postupy: [suverenity dat.](https://wikipedia.org/wiki/Technological_Sovereignty), [šifrování v klidovém stavu](https://wikipedia.org/wiki/Data_at_rest#Encryption) citlivých dat, atd. |Zajištění zabezpečení ve službě Azure: [zabezpečení Azure](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/), [protokoly auditu](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)a další. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Funkce vs. Webové úlohy
Můžete probereme funkce Azure a Azure App Service WebJobs společně vzhledem k tomu, že jsou oba *kód první* integrační služby a určený pro vývojáře. Umožňují vám toorun a skriptu nebo část kódu v odpovědi toovarious události, jako například [nové úložiště objektů blob](functions-bindings-storage.md) nebo [žádost Webhooku](functions-bindings-http-webhook.md). Zde jsou jejich podobnosti: 

* Obě jsou postaveny na [Azure App Service](../app-service/app-service-value-prop-what-is.md) a možnost využívat funkce, jako [ovládací prvek zdroje](../app-service-web/app-service-continuous-deployment.md), [ověřování](../app-service/app-service-authentication-overview.md), a [monitorování](../app-service-web/web-sites-monitor.md).
* Obě jsou zaměřené na vývojáře služby.
* Jak podporovat standardní skriptovací a programovací jazyky.
* Mají obě NuGet a NPM, které podporují.

Funkce hello přirozené vývoj webové úlohy se, že trvá nejlepší, co o Webjobech hello a vylepšuje je. Hello vylepšení zahrnují: 

* Zjednodušená vývoj, testování a spustit přímo v prohlížeči hello kód.
* Jako integraci s více Azure služby a služby 3. stran [Githubu Webhooky](https://developer.github.com/webhooks/creating/).
* Platím za použití, bez nutnosti toopay pro [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Automatické, [dynamické škálování](functions-scale.md).
* Pro stávající zákazníky služby App Service, systémem stále možné plán služby App Service (tootake výhod za využité prostředky).
* Integrace s Logic Apps.

Hello následující tabulka shrnuje hello rozdíly mezi funkcemi a webové úlohy:

|  | Funkce | Webové úlohy |
| --- | --- | --- |
| Škálování |Configurationless škálování |škálování se plán služby App Service |
| Ceny |Platím za použití nebo její část plánu služby App Service |Část plánu služby App Service |
| Spustit – typ |aktivována, naplánované (podle aktivační událost časovače) |spouštěná, průběžné, naplánované |
| Aktivační události |[časovač](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-documentdb.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [HTTP/Webhooku (Githubu, Slack)](functions-bindings-http-webhook.md), [Azure App Service Mobile Apps](functions-bindings-mobile-apps.md), [Azure Notification Hubs](functions-bindings-notification-hubs.md), [Azure Service Bus](functions-bindings-service-bus.md), [Azure Storage](functions-bindings-storage.md) |[Úložiště Azure](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure Service Bus](../app-service-web/websites-dotnet-webjobs-sdk-service-bus.md) |
| Vývoj v prohlížeči |Podporované | Nepodporuje se |
| Okno skriptování |experimentální |Podporované |
| PowerShell |experimentální |Podporované |
| C# |Podporované |Podporované |
| F# |Podporované |Nepodporuje se |
| Bash |experimentální |Podporované |
| PHP |experimentální |Podporované |
| Python |experimentální |Podporované |
| JavaScript |Podporované |Podporované |

Jestli funkce toouse nebo WebJobs závisí především na jaké už úlohy službou App Service. Pokud máte aplikace služby App Service, pro které chcete, aby fragmenty kódu toorun a chcete toomanage je společně v hello stejné prostředí DevOps, měli byste použít webové úlohy. Pokud chcete, aby fragmenty kódu toorun pro další služby Azure nebo aplikacím i 3. stran, nebo pokud chcete spravovat vaše fragmenty kódu integrace příliš odděleně od aplikace služby App Service nebo pokud chcete, aby toocall vaše fragmenty kódu z aplikace logiky, by měl využít výhod všech Hello vylepšení funkcí.  

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Tok, Logic Apps a funkce společně
Jak jsme uvedli služby, která je nejlépe hodí tooyou závisí na vaší situaci. 

* Optimalizace jednoduché podnikání potom použijte toku.
* Pokud váš scénář integrace je příliš rozšířený pro tok, nebo budete potřebovat možnosti DevOps a compliances zabezpečení, použijte Logic Apps.
* Pokud krok ve vašem scénáři integrace vyžaduje vysokou vlastní transformace nebo specializované kódu, zapište si aplikaci funkce a potom aktivovat funkci jako akce v aplikaci logiky.

Aplikace logiky můžete volat v toku. Také můžete volat funkci v aplikaci logiky a aplikace logiky ve funkci. integrace Hello toku, Logic Apps a funkce pokračovat tooimprove přesčasová. Můžete vytvořit v jedné službě něco a použít ho v hello dalších služeb. Proto je vhodné všechny investice, které provedete v těchto tří technologií.

## <a name="next-steps"></a>Další kroky
Začínáme s jednotlivými hello služby vytvoření vaší první toku aplikace logiky, funkce aplikace nebo webové úlohy. Klikněte na jakékoliv hello následující odkazy:

* [Začínáme s Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)
* [Vytvoření první funkce Azure](functions-create-first-azure-function.md)
* [Nasazení WebJobs pomocí sady Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

Nebo můžete získat další informace o těchto integračních služeb s hello následující odkazy:

* [Využívání Azure Functions & Azure App Service pro scénáře integrace podle Kryštof Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Integrace provedené Charlese Lamanna za jednoduché](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Aplikace logiky Live webové vysílání](http://aka.ms/logicappslive)
* [Microsoft toku nejčastější dotazy](https://flow.microsoft.com/documentation/frequently-asked-questions/)
* [Prostředky dokumentace Azure WebJobs](../app-service-web/websites-webjobs-resources.md)

