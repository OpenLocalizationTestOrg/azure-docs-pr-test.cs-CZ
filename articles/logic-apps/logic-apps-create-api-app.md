---
title: "Vytváření webového rozhraní API a rozhraní REST API jako konektory - Azure Logic Apps | Microsoft Docs"
description: "Vytváření webového rozhraní API a rozhraní REST API volat vaše rozhraní API, služby nebo systémy v pracovních postupech pro integrace systému službou Azure Logic Apps"
keywords: "webové rozhraní API, rozhraní REST API, konektorů, pracovní postupy, integrace systému"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: bd229179-7199-4aab-bae0-1baf072c7659
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/26/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 4ae98804aced23c0261c1d58721cb18d8152c6f1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-custom-apis-as-connectors-for-logic-apps"></a>Vytvořte vlastní rozhraní API jako konektory pro logic apps

I když Azure Logic Apps nabízí [100 + předdefinované konektory](../connectors/apis-list.md) , můžete použít v pracovních postupech aplikace logiky, můžete chtít volání rozhraní API, systémy a služby, které nejsou k dispozici jako konektory. Můžete vytvořit vlastní vlastní rozhraní API, která poskytují akce a aktivační události pro použití v aplikace logiky. Zde jsou z jiných důvodů, proč můžete chtít vytvořit vlastní rozhraní API pro použití jako konektory v aplikacích logiky:

* Váš aktuální systém rozšiřte integrace a data integrace pracovních postupů.
* Pomoc zákazníkům používat služby ke správě professional nebo osobní úloh.
* Rozbalte reach, možnosti rozpoznání a použití služby.

V podstatě, konektory jsou webové rozhraní API, která pomocí REST pro modulární rozhraní [formátu metadat Swagger](http://swagger.io/specification/) na dokumentaci a formát JSON jako jejich formát dat systému exchange. Vzhledem k tomu, že konektory jsou rozhraní REST API, které komunikují prostřednictvím koncových bodů protokolu HTTP, můžete použít žádný jazyk, jako je rozhraní .NET, Java nebo Node.js, pro tvorbu konektorů. Vaše rozhraní API je také možné hostovat na [Azure App Service](../app-service/app-service-value-prop-what-is.md), platformy jako služba (PaaS) nabídka, která jedním ze způsobů nejlepší, nejjednodušší a nejvíce škálovatelným poskytuje pro hostování rozhraní API. 

Pro vlastní rozhraní API pro práci s logic apps, může být vaše rozhraní API [ *akce* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) které provádějí konkrétní úlohy v pracovních postupech logiku aplikace. Rozhraní API se mohou chovat i jako [ *aktivační událost* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , spustí pracovní postup aplikace logiky, pokud nová data nebo událost splňuje zadanou podmínku. Toto téma popisuje běžných vzorů, které můžete provést pro tvorbu akce a aktivační události v rozhraní API, na základě chování, které chcete poskytnout rozhraní API.

> [!TIP] 
> I když můžete nasadit vaše rozhraní API jako [webové aplikace](../app-service-web/app-service-web-overview.md), zvažte nasazení vašich rozhraní API jako [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md), který může usnadnit úlohu při sestavení, hostovat a používat rozhraní API v cloudu a místně. Není nutné změnit spuštěním kódu vaše rozhraní API – kód do aplikace API jen nasadit. Zjistěte, jak [sestavení aplikace API vytvořená s technologií ASP.NET](../app-service-api/app-service-api-dotnet-get-started.md), [Java](../app-service-api/app-service-api-java-api-app.md), nebo [Node.js](../app-service-api/app-service-api-nodejs-api-app.md). 
>
> Ukázky aplikace API vytvořená pro logic apps, naleznete [úložiště GitHub aplikace logiky Azure](http://github.com/logicappsio) nebo [blog](http://aka.ms/logicappsblog).

## <a name="helpful-tools"></a>Užitečné nástroje

Vlastní rozhraní API funguje nejlépe s logic apps při také obsahuje rozhraní API [dokumentu Swagger](http://swagger.io/specification/) operace rozhraní API a parametry, který popisuje.
Mnoho knihovny jako [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle), může automaticky generovat soubor Swagger pro vás. Umožňuje anotaci souboru Swagger pro zobrazované názvy, typy vlastností a tak dále, můžete také použít [TRex](https://github.com/nihaue/TRex) tak, aby vašeho souboru Swagger funguje dobře s logic apps.

<a name="actions"></a>

## <a name="action-patterns"></a>Vzory akce

Pro aplikace logiky k provádění úloh, by měl poskytovat vlastního rozhraní API [ *akce*](./logic-apps-what-are-logic-apps.md#logic-app-concepts). Každé operace v rozhraní API se mapuje na akci. Základní akce je kontroler, který přijímá požadavky protokolu HTTP a vrátí odpovědi HTTP. Aplikace logiky, například odešle požadavek HTTP pro webovou aplikaci nebo aplikaci API. Aplikace pak vrátí odpověď HTTP spolu s obsahem, který dokáže zpracovat aplikaci logiky.

Pro standardní akci můžete napsat metodu požadavku HTTP v rozhraní API a popisují dané metody v souboru Swagger. Potom můžete volat rozhraní API přímo pomocí [akce HTTP](../connectors/connectors-native-http.md) nebo [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) akce. Ve výchozím nastavení, musí být odpovědi vrácené v rámci [časový limit požadavku](./logic-apps-limits-and-config.md). 

![Vzor standardní akce](./media/logic-apps-create-api-app/standard-action.png)

<a name="pattern-overview"></a>Chcete-li aplikace logiky, počkejte na dokončení delší úloh rozhraní API, můžete postupovat podle rozhraní API [dotazování asynchronní vzor](#async-pattern) nebo [webhooku asynchronní vzor](#webhook-actions) popsaných v tomto tématu. Analogie, který pomáhá vizualizovat tyto vzory různé chování Představte si proces pro řazení vlastní dort z Pekárna. Vzor dotazování zrcadlí chování, kde zavoláte Pekárna každých 20 minut a zkontrolujte, zda dort připraven. Vzor webhooku zrcadlí chování, kde Pekárna se vás zeptá na vaše telefonní číslo, můžou můžete volat při dort je připraven.

Ukázky, najdete v článku [úložiště GitHub aplikace logiky](https://github.com/logicappsio). Také další informace o [měření využití pro akce](logic-apps-pricing.md).

<a name="async-pattern"></a>

### <a name="perform-long-running-tasks-with-the-polling-action-pattern"></a>Dlouho běžící úlohy pomocí vzoru akce dotazování

Má vaše rozhraní API provádět úlohy, které by mohl spustit delší než [časový limit požadavku](./logic-apps-limits-and-config.md), můžete použít vzor asynchronní dotazování. Tento vzor má vaše rozhraní API práce v samostatné vlákno, ale zachování aktivního připojení k modulu Logic Apps. Tímto způsobem aplikaci logiky nepodporuje vypršení časového limitu a pokračujte dalším krokem v pracovním postupu před vaše rozhraní API dokončení práce.

Tady je obecný vzor:

1. Ujistěte se, že modul ví, že vaše rozhraní API přijato žádost a spuštěna.
2. Pokud modul provede odeslání dalších žádostí o stav úlohy, umožní modul vědět, pokud se vaše rozhraní API dokončení úlohy.
3. Vrátí relevantní data do modulu, aby mohl pokračovat pracovní postup aplikace logiky.

<a name="bakery-polling-action"></a>Teď použít předchozí analogie Pekárna se vzorem cyklického dotazování a Představte si, že zavoláte Pekárna a pořadí vlastní dort pro doručení. Proces pro provedení dort trvá určitou dobu, a nechcete počkejte Pekárna funguje na dort na telefonu. Pekárna potvrzuje vaši objednávku a má můžete volat každých 20 minut, než dort stavu. Po předat 20 minut, volání Pekárna, ale jejich zjistíte, že vaše dort neuděláte a že by měly volat v jiném 20 minut. Tento proces a zpět pokračuje, dokud zavoláte a Pekárna znamená, že vaši objednávku je připraven a přináší vaší dort. 

Proto umožňuje mapovat tento vzor dotazování zpět. Pekárna představuje vlastního rozhraní API, zatímco budete dort zákazníka, představují modul Logic Apps. Když modul volá rozhraní API s žádostí, rozhraní API potvrdí žádosti a odpoví časový interval, když modul můžete zkontrolovat stav úlohy. Modul pokračuje, dokud vaše rozhraní API odpoví, že úloha probíhá kontrola stavu úloh a vrací data do aplikace logiky, která pak bude pokračovat pracovního postupu. 

![Vzor akce dotazování](./media/logic-apps-create-api-app/custom-api-async-action-pattern.png)

Zde jsou konkrétní kroky pro vaše rozhraní API podle, popsané z hlediska rozhraní API:

1. Pokud vaše rozhraní API získá požadavek HTTP, který má být zahájeno, okamžitě se vraťte HTTP `202 ACCEPTED` odpověď se `location` záhlaví popsanou dále v tomto kroku. Tato odezva umožňuje modul Logic Apps znát, aby vaše rozhraní API tu žádost, přijata datová část požadavku (vstupní data) a teď zpracovává. 
   
   `202 ACCEPTED` Odpověď by měla obsahovat tyto hlavičky:
   
   * *Požadované*: A `location` hlavičky, která určuje absolutní cesta na adresu URL, kde modul Logic Apps můžete zkontrolovat stav úlohy vaše rozhraní API

   * *Volitelné*: A `retry-after` hlavičky, která určuje počet sekund, které modul čekat, než kontrola `location` adresa URL pro stav úlohy. 

     Ve výchozím nastavení modul kontroluje každých 20 sekund. Chcete-li určit jiný interval, zahrňte `retry-after` záhlaví a počet sekund, dokud další dotazování.

2. Po uplynutí zadané doby Logic Apps modul hlasování `location` adresy URL a zkontrolujte stav úlohy. Rozhraní API měli provádět tyto kontroly a vrátit těchto odpovědí:
   
   * Pokud se provádí úlohy, vrátí HTTP `200 OK` odpověď, a to společně s datovou část odpovědi (vstup na další krok).

   * Pokud stále zpracovává úlohu, vrátí jiné HTTP `202 ACCEPTED` odpovědi, ale s hlavičkách stejné jako původní odpověď.

Pokud vaše rozhraní API následuje tento vzor, nemusíte dělat nic v definici pracovního postupu aplikace logiky pokračovat v kontrole stavu úlohy. Pokud modul získá HTTP `202 ACCEPTED` odpovědi a platná `location` záhlaví, modul respektuje asynchronní vzor a zkontroluje `location` záhlaví, dokud vaše rozhraní API vrátí odpověď bez 202.

> [!TIP]
> Pro příklad asynchronní vzor, přečtěte si to [ukázková odpověď asynchronní kontroler v Githubu](https://github.com/logicappsio/LogicAppsAsyncResponseSample).

<a name="webhook-actions"></a>

### <a name="perform-long-running-tasks-with-the-webhook-action-pattern"></a>Dlouho běžící úlohy pomocí vzoru akce webhooku

Jako alternativu můžete vzoru webhooku dlouhotrvající úlohy a asynchronní zpracování. Tento vzor má aplikace logiky, pozastavení a počkejte "zpětného volání" z rozhraní API dokončí zpracování, než budete pokračovat pracovního postupu. Je toto zpětné volání HTTP POST, který odešle zprávu na adresu URL, když se stane událost. 

<a name="bakery-webhook-action"></a>Teď použít předchozí analogie Pekárna se vzorem webhooku a Představte si, že zavoláte Pekárna a pořadí vlastní dort pro doručení. Proces pro provedení dort trvá určitou dobu, a nechcete počkejte Pekárna funguje na dort na telefonu. Pekárna potvrzuje vaši objednávku, ale tentokrát, můžete jim poskytnout vaše telefonní číslo, se může zavolat, pokud se provádí dort. Tentokrát Pekárna poznáte, když vaši objednávku je připraven a doručí vaší dort.

Když jsme mapovat tento vzor webhooku zpět, Pekárna představuje vlastního rozhraní API, zatímco budete dort zákazníka, představují modul Logic Apps. Modul volá rozhraní API se žádostí a zahrne adresu URL "zpětného volání".
Pokud se provádí úlohy, rozhraní API používá adresu URL Pokud chcete upozornit modul a vrátit data do aplikace logiky, která pak bude pokračovat pracovního postupu. 

Pro tento vzor nastavit dva koncové body na vašem řadiči: `subscribe` a`unsubscribe`

*  `subscribe`koncový bod: při provádění dosáhne vaše rozhraní API akce v pracovním postupu, Logic Apps modul volání `subscribe` koncový bod. Tento krok způsobí, že aplikace logiky vytvořit adresu URL zpětné volání, která ukládá vaše rozhraní API a potom počkejte zpětného volání z rozhraní API, když je práce dokončena. Rozhraní API pak zavolá zpátky s HTTP POST na adresu URL a předá všechny vrácený obsah a hlavičky jako vstup pro logiku aplikace.

* `unsubscribe`koncový bod: Pokud zrušení spusťte aplikaci logiky Logic Apps modul volání `unsubscribe` koncový bod. Rozhraní API můžete zrušit registraci adresy URL zpětného volání a ukončení všech procesů podle potřeby.

![Vzor akce Webhooku](./media/logic-apps-create-api-app/custom-api-webhook-action-pattern.png)

> [!NOTE]
> Návrhář aplikace logiky v současné době nepodporuje zjišťování koncových bodů webhooku prostřednictvím Swagger. Proto se u tohoto vzoru, budete muset přidat [ **Webhooku** akce](../connectors/connectors-native-webhook.md) a zadejte adresu URL, hlavičky a obsah vaší žádosti. Viz také [akce pracovního postupu a aktivační události](logic-apps-workflow-actions-triggers.md#api-connection-webhook-action). Chcete-li předat v adrese URL zpětné volání, můžete použít `@listCallbackUrl()` funkce workflowu v některém z předchozí pole podle potřeby.

> [!TIP]
> Pro webhook příklad vzoru zkontrolujte to [ukázka aktivační události webhooku v Githubu](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

<a name="triggers"></a>

## <a name="trigger-patterns"></a>Vzory aktivační události

Vlastního rozhraní API může fungovat jako [ *aktivační událost* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , spustí aplikace logiky, pokud nová data nebo událost splňuje zadanou podmínku. Této aktivační události můžete buď, pravidelně kontrolovat, nebo počkejte a naslouchat pro nová data nebo události na váš koncový bod služby. Pokud nová data nebo událost splňuje zadanou podmínku, aktivační událost se aktivuje a spustí aplikaci logiky, která naslouchá k této aktivační události. Při spuštění aplikace logiky tímto způsobem můžete postupujte vaše rozhraní API [ *cyklického dotazování aktivační událost* ](#polling-triggers) nebo [ *aktivační události webhooku* ](#webhook-triggers) vzor. Tyto vzory jsou podobné svým pro [dotazování akce](#async-pattern) a [akce webhooku](#webhook-actions). Také další informace o [měření využití pro aktivační procedury](logic-apps-pricing.md).

<a name="polling-triggers"></a>

### <a name="check-for-new-data-or-events-regularly-with-the-polling-trigger-pattern"></a>Zkontrolujte pro nová data nebo událostí pravidelně se vzorkem cyklického dotazování aktivační události

A *cyklického dotazování aktivační událost* funguje podobně jako [dotazování akce](#async-pattern) dříve popsaných v tomto tématu. Modul aplikace logiky pravidelně volá a zkontroluje koncový bod aktivační událost pro nová data nebo události. Pokud modul najde nová data nebo událost, která splňuje zadanou podmínku, aktivační událost se aktivuje. Modul poté vytvoří instanci aplikace logiky, která zpracovává data jako vstup. 

![Cyklického dotazování aktivační událost vzor](./media/logic-apps-create-api-app/custom-api-polling-trigger-pattern.png)

> [!NOTE]
> Každý požadavek dotazování se počítá jako spuštění akce, i když je vytvořena žádná instance aplikace logiky. Aby se zabránilo zpracování stejná data vícekrát, aktivační událost by měl vyčistit data, která již byla přečtena a předána do aplikace logiky.

Zde jsou konkrétní kroky pro aktivační událost dotazování, popsané z hlediska rozhraní API:

| Najít nová data nebo událost?  | Odpověď rozhraní API | 
| ------------------------- | ------------ |
| Najít | Vrátí HTTP `200 OK` stav s datovou část odpovědi (vstup pro další krok). <br/>Tato odezva vytvoří instanci aplikace logiky a spustí pracovní postup. |
| Nebyl nalezen | Vrátí HTTP `202 ACCEPTED` stavu `location` záhlaví a `retry-after` záhlaví. <br/>Pro aktivační události `location` záhlaví by obsahovat také `triggerState` parametr dotazu, který je obvykle "razítka". Rozhraní API můžete použít tento identifikátor ke sledování posledního byla aktivována aplikaci logiky. |

Například může pravidelně zkontrolujte vaši službu pro nové soubory, sestavte cyklického dotazování aktivační událost, která má tyto chování:

| Žádost obsahuje `triggerState`? | Odpověď rozhraní API |
| -------------------------------- | -------------|
| Ne | Vrátí HTTP `202 ACCEPTED` stav plus `location` hlavička s `triggerState` nastavena na aktuální čas a `retry-after` intervalech 15 sekund. |
| Ano | Zkontrolujte vaši službu pro soubory, které jsou přidány po `DateTime` pro `triggerState`. |

| Počet nalezených souborů | Odpověď rozhraní API |
| --------------------- | -------------|
| Jeden soubor | Vrátí HTTP `200 OK` aktualizovat stav a datové části obsahu `triggerState` k `DateTime` pro vrácené souboru a nastavte `retry-after` intervalech 15 sekund. |
| Více souborů | Vrátí jeden soubor na čas a HTTP `200 OK` stav, aktualizace `triggerState`a nastavte `retry-after` interval na 0 sekund. </br>Tyto kroky mohli modul vědět, že další data nejsou k dispozici, a že modul měli okamžitě požádat o data z adresy URL v `location` záhlaví. |
| Žádné soubory | Vrátí HTTP `202 ACCEPTED` stav, neměnit `triggerState`a nastavte `retry-after` intervalech 15 sekund. |

> [!TIP]
> Příklad cyklického dotazování aktivační událost vzor, zkontrolujte tato [cyklického dotazování aktivační událost řadiče ukázka v Githubu](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/PollTriggerController.cs).

<a name="webhook-triggers"></a>

### <a name="wait-and-listen-for-new-data-or-events-with-the-webhook-trigger-pattern"></a>Počkejte a naslouchat pro události se vzorkem aktivační události webhooku nebo nová data

Aktivační události webhooku je *aktivační událost nabízené* které vyčká a čeká na nová data nebo události na váš koncový bod služby. Pokud nová data nebo událost splňuje zadanou podmínku, aktivační událost se aktivuje a vytvoří instanci aplikace logiky, která pak zpracovává data jako vstup.
Aktivační události Webhooku fungují podobně jako [akce webhooku](#webhook-actions) dříve popsané v tomto tématu a nastavení je `subscribe` a `unsubscribe` koncové body. 

* `subscribe`koncový bod: při přidání a uložit aktivační události webhooku aplikace logiky, Logic Apps modul volání `subscribe` koncový bod. Tento krok způsobí, že aplikace logiky vytvořit adresu URL zpětné volání, která ukládá vaše rozhraní API. Když je nová data nebo událost, která splňuje zadanou podmínku, zavolá rozhraní API zpět pomocí HTTP POST na adresu URL. Datová část obsahu a hlavičky předat jako vstup pro logiku aplikace.

* `unsubscribe`koncový bod: Pokud odstranění aktivační události webhooku nebo celý logiku aplikace Logic Apps modul volání `unsubscribe` koncový bod. Rozhraní API můžete zrušit registraci adresy URL zpětného volání a ukončení všech procesů podle potřeby.

![Vzor aktivační události Webhooku](./media/logic-apps-create-api-app/custom-api-webhook-trigger-pattern.png)

> [!NOTE]
> Návrhář aplikace logiky v současné době nepodporuje zjišťování koncových bodů webhooku prostřednictvím Swagger. Proto se u tohoto vzoru, budete muset přidat [ **Webhooku** aktivační událost](../connectors/connectors-native-webhook.md) a zadejte adresu URL, hlavičky a obsah vaší žádosti. Viz také [aktivační událost HTTPWebhook](logic-apps-workflow-actions-triggers.md#httpwebhook-trigger). Chcete-li předat v adrese URL zpětné volání, můžete použít `@listCallbackUrl()` funkce workflowu v některém z předchozí pole podle potřeby.
>
> Aby se zabránilo zpracování stejná data vícekrát, aktivační událost by měl vyčistit data, která již byla přečtena a předána do aplikace logiky.

> [!TIP]
> Pro webhook příklad vzoru zkontrolujte to [ukázka řadiče aktivační události webhooku v Githubu](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

## <a name="deploy-call-and-secure-custom-apis"></a>Nasazení, volání a zabezpečte vlastní rozhraní API

Po vytvoření vaše vlastní rozhraní API, takže je můžete bezpečně volat nastavte vaše rozhraní API pro nasazení. Zjistěte, jak [nasazení, volání a zabezpečte vlastní rozhraní API pro logic apps](./logic-apps-custom-hosted-api.md).

## <a name="publish-custom-apis-to-azure"></a>Publikovat vlastní rozhraní API do Azure

Chcete-li k dispozici na veřejných vaše vlastní rozhraní API v Azure, odeslání vašeho kandidátů na [programu Microsoft Azure Certified](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Podpora

Obraťte se na nápovědu s vlastním rozhraním API, [ customapishelp@microsoft.com ](mailto:customapishelp@microsoft.com).

Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Pokud chcete pomoci při vylepšování Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Další kroky

* [Měření využití pro akce a aktivační události](logic-apps-pricing.md)
* [Zpracování typů obsahu](./logic-apps-content-type.md)
* [Zpracování chyb a výjimek](./logic-apps-exception-handling.md)
* [Zabezpečený přístup k aplikacím logiky](./logic-apps-securing-a-logic-app.md)
* [Volání, aktivaci nebo vnořit aplikace logiky s koncovými body HTTP](./logic-apps-http-endpoint.md)