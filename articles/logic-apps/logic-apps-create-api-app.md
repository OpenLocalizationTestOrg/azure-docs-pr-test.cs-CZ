---
title: "aaaCreate webové rozhraní API a rozhraní REST API jako konektory - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření webové rozhraní API a rozhraní REST API toocall vaše rozhraní API, služby nebo systémy v pracovních postupech pro integrace systému službou Azure Logic Apps"
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
ms.openlocfilehash: 2792204d1d298a6f810e75211c1789ee204c2fdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-apis-as-connectors-for-logic-apps"></a>Vytvořte vlastní rozhraní API jako konektory pro logic apps

I když Azure Logic Apps nabízí [100 + předdefinované konektory](../connectors/apis-list.md) , můžete použít v pracovních postupech aplikace logiky, můžete chtít toocall rozhraní API, systémy a služby, které nejsou k dispozici jako konektory. Můžete vytvořit vlastní vlastní rozhraní API, která poskytují akce a aktivační události toouse v aplikacích logiky. Tady jsou z jiných důvodů, proč byste měli toocreate vlastní rozhraní API je příliš použít jako konektorů v logiku aplikace:

* Váš aktuální systém rozšiřte integrace a data integrace pracovních postupů.
* Pomoc zákazníkům používat vaše služba toomanage professional nebo osobní úkoly.
* Rozbalte hello reach, možnosti rozpoznání a použití služby.

V podstatě, konektory jsou webové rozhraní API, která pomocí REST pro modulární rozhraní [formátu metadat Swagger](http://swagger.io/specification/) na dokumentaci a formát JSON jako jejich formát dat systému exchange. Vzhledem k tomu, že konektory jsou rozhraní REST API, které komunikují prostřednictvím koncových bodů protokolu HTTP, můžete použít žádný jazyk, jako je rozhraní .NET, Java nebo Node.js, pro tvorbu konektorů. Vaše rozhraní API je také možné hostovat na [Azure App Service](../app-service/app-service-value-prop-what-is.md), platformy jako služba (PaaS) nabídka, která jedním ze způsobů nejlepší, nejjednodušší a nejvíce škálovatelným hello poskytuje pro hostování rozhraní API. 

Pro vlastní toowork rozhraní API s logic apps, může být vaše rozhraní API [ *akce* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) které provádějí konkrétní úlohy v pracovních postupech logiku aplikace. Rozhraní API se mohou chovat i jako [ *aktivační událost* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , spustí pracovní postup aplikace logiky, pokud nová data nebo událost splňuje zadanou podmínku. Toto téma popisuje běžných vzorů, které můžete provést pro tvorbu akce a aktivační události v rozhraní API, na základě hello chování, které chcete tooprovide vaše rozhraní API.

> [!TIP] 
> I když můžete nasadit vaše rozhraní API jako [webové aplikace](../app-service-web/app-service-web-overview.md), zvažte nasazení vašich rozhraní API jako [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md), který může usnadnit úlohu při sestavení, hostovat a používat rozhraní API v hello cloudu a místně. Nemáte toochange žádný kód ve vašich rozhraní API – aplikace kód tooan API jen nasadit. Zjistěte, jak příliš [sestavení aplikace API vytvořená s technologií ASP.NET](../app-service-api/app-service-api-dotnet-get-started.md), [Java](../app-service-api/app-service-api-java-api-app.md), nebo [Node.js](../app-service-api/app-service-api-nodejs-api-app.md). 
>
> Ukázky aplikace API vytvořená pro logic apps, najdete v článku hello [úložiště GitHub aplikace logiky Azure](http://github.com/logicappsio) nebo [blog](http://aka.ms/logicappsblog).

## <a name="helpful-tools"></a>Užitečné nástroje

Vlastní rozhraní API funguje nejlépe s logic apps při hello rozhraní API je také [dokumentu Swagger](http://swagger.io/specification/) operace hello rozhraní API a parametry, který popisuje.
Mnoho knihovny jako [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle), může automaticky generovat soubor Swagger hello za vás. soubor Swagger tooannotate hello zobrazované názvy, typy vlastností a tak dále, můžete použít také [TRex](https://github.com/nihaue/TRex) tak, aby vašeho souboru Swagger funguje dobře s logic apps.

<a name="actions"></a>

## <a name="action-patterns"></a>Vzory akce

Pro úlohy tooperform aplikace logiky, by měl poskytovat vlastního rozhraní API [ *akce*](./logic-apps-what-are-logic-apps.md#logic-app-concepts). Každé operace v rozhraní API mapuje tooan akce. Základní akce je kontroler, který přijímá požadavky protokolu HTTP a vrátí odpovědi HTTP. Aplikace logiky, například odešle HTTP žádost tooyour webové aplikace nebo aplikace API. Aplikace pak vrátí odpověď HTTP, spolu s obsahem, který hello logiku aplikace zpracovávat.

Pro standardní akci můžete napsat metodu požadavku HTTP v rozhraní API a popisují dané metody v souboru Swagger. Potom můžete volat rozhraní API přímo pomocí [akce HTTP](../connectors/connectors-native-http.md) nebo [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) akce. Ve výchozím nastavení, musí být odpovědi vrácené v rámci hello [časový limit požadavku](./logic-apps-limits-and-config.md). 

![Vzor standardní akce](./media/logic-apps-create-api-app/standard-action.png)

<a name="pattern-overview"></a>toomake aplikace logiky počkejte delší úloh dokončení vaše rozhraní API, rozhraní API můžete postupovat podle hello [dotazování asynchronní vzor](#async-pattern) nebo hello [webhooku asynchronní vzor](#webhook-actions) popsaných v tomto tématu. Analogie, který pomáhá vizualizovat tyto vzory různé chování Představte si hello proces řazení vlastní dort z Pekárna. vzor Hello dotazování zrcadlí hello chování, kde zavoláte hello Pekárna každých 20 minut toocheck zda dort hello je připraven. vzor webhooku Hello zrcadlí hello chování, kde Pekárna hello se vás zeptá na vaše telefonní číslo, můžou můžete volat při dort hello je připraven.

Ukázky, najdete v článku hello [úložiště GitHub aplikace logiky](https://github.com/logicappsio). Také další informace o [měření využití pro akce](logic-apps-pricing.md).

<a name="async-pattern"></a>

### <a name="perform-long-running-tasks-with-hello-polling-action-pattern"></a>Provedení dlouhotrvající úlohy s hello dotazování vzor akce

rozhraní API provádět úlohy, které by mohl spustit déle, než hello toohave [časový limit požadavku](./logic-apps-limits-and-config.md), můžete použít asynchronní dotazování vzor hello. Tento vzor má vaše rozhraní API práce v samostatné vlákno, ale zachovat modul aktivní připojení toohello Logic Apps. Tímto způsobem aplikace logiky hello nepodporuje vypršení časového limitu a pokračujte hello další krok v postupu hello před vaše rozhraní API dokončení práce.

Tady je obecný vzor hello:

1. Ujistěte se, že tento motor hello ví, že vaše rozhraní API přijato hello žádost a spuštěna.
2. Pokud modul hello provede odeslání dalších žádostí o stav úlohy, umožní hello modul vědět, pokud se vaše rozhraní API dokončení úkolů hello.
3. Vrátí modul toohello relevantní data, aby mohl pokračovat hello logiku aplikace pracovního postupu.

<a name="bakery-polling-action"></a>Teď platí hello předchozího vzoru názvů Pekárna analogie toohello dotazování a Představte si, že zavoláte Pekárna a pořadí vlastní dort pro doručení. Hello proces pro provedení hello dort trvá určitou dobu a nechcete, aby toowait na telefonu hello při hello Pekárna funguje na dort hello. Hello Pekárna potvrzuje vaši objednávku a má můžete volat každých 20 minut hello dort stavu. Po předat 20 minut, volání hello Pekárna, ale jejich zjistíte, že vaše dort neuděláte a že by měly volat v jiném 20 minut. Tento proces a zpět pokračuje, dokud zavoláte a hello Pekárna znamená, že vaši objednávku je připraven a přináší vaší dort. 

Proto umožňuje mapovat tento vzor dotazování zpět. Hello Pekárna představuje vlastního rozhraní API, zatímco budete hello dort zákazníka, představují hello Logic Apps modul. Když hello modul volá rozhraní API s žádostí, rozhraní API potvrdí hello požadavku a odpoví hello časový interval, když modul hello můžete zkontrolovat stav úlohy. modul Hello pokračuje v kontrole stavu úlohy, dokud vaše rozhraní API odpoví, že tuto úlohu hello je nastavená a vrátí data tooyour logiku aplikace, která pak bude pokračovat pracovního postupu. 

![Vzor akce dotazování](./media/logic-apps-create-api-app/custom-api-async-action-pattern.png)

Zde jsou hello konkrétní kroky pro vaše rozhraní API toofollow popsané z hlediska hello rozhraní API:

1. Pokud vaše rozhraní API získá HTTP žádost toostart pracovní, okamžitě návratový HTTP `202 ACCEPTED` odpověď se hello `location` záhlaví popsanou dále v tomto kroku. Tato odezva umožňuje hello Logic Apps modul vědět, že vaše rozhraní API získali hello požadavek přijatý hello datová část požadavku (vstupní data) a teď zpracovává. 
   
   Hello `202 ACCEPTED` odpověď by měla obsahovat tyto hlavičky:
   
   * *Požadované*: A `location` hlavičky, která určuje absolutní cestu adresy URL tooa hello kde modul hello Logic Apps můžete zkontrolovat stav úlohy vaše rozhraní API

   * *Volitelné*: A `retry-after` hlavičky, která určuje hello počet sekund, po které hello modul má čekat před zaškrtnutím hello `location` adresa URL pro stav úlohy. 

     Ve výchozím nastavení modul hello kontroluje každých 20 sekund. toospecify jiný interval, zahrnují hello `retry-after` záhlaví a hello počet sekund až hello další dotazování.

2. Po hello zadaný čas předává, hello Logic Apps modul hlasování hello `location` stav úlohy toocheck adresy URL. Rozhraní API měli provádět tyto kontroly a vrátit těchto odpovědí:
   
   * Pokud se provádí hello úlohy, vrátí HTTP `200 OK` odpověď, a to společně s datovou část odpovědi hello (vstup pro další krok hello).

   * Pokud stále zpracovává hello úlohy, vrátí jiné HTTP `202 ACCEPTED` odpovědi, ale s hello hlavičky stejné jako původní odpověď hello.

Pokud vaše rozhraní API následuje tento vzor, nemáte toodo nic v hello logiku aplikace definice pracovního postupu toocontinue Kontrola stavu úloh. Pokud modul hello získá HTTP `202 ACCEPTED` odpovědi a platná `location` záhlaví, hello modul ohledech hello asynchronní vzor a kontroly hello `location` záhlaví, dokud vaše rozhraní API vrátí odpověď bez 202.

> [!TIP]
> Pro příklad asynchronní vzor, přečtěte si to [ukázková odpověď asynchronní kontroler v Githubu](https://github.com/logicappsio/LogicAppsAsyncResponseSample).

<a name="webhook-actions"></a>

### <a name="perform-long-running-tasks-with-hello-webhook-action-pattern"></a>Dlouho běžící úlohy pomocí vzoru akce webhooku hello

Jako alternativu můžete hello webhooku vzor dlouhotrvající úlohy a asynchronní zpracování. Tento vzor má aplikace logiky hello pozastavení a počkejte "zpětného volání" z vašeho rozhraní API toofinish zpracování před pokračováním pracovního postupu. Je toto zpětné volání HTTP POST, který pošle adresu URL tooa zprávy, když se stane událost. 

<a name="bakery-webhook-action"></a>Teď použít hello předchozí Pekárna analogie toohello webhooku vzorek a Představte si, že zavoláte Pekárna a pořadí vlastní dort pro doručení. Hello proces pro provedení hello dort trvá určitou dobu a nechcete, aby toowait na telefonu hello při hello Pekárna funguje na dort hello. Hello Pekárna potvrzuje vaši objednávku, ale tentokrát, můžete jim poskytnout vaše telefonní číslo tak můžou můžete volat po dokončení dort hello. Tentokrát Pekárna hello poznáte, když vaši objednávku je připraven a doručí vaší dort.

Když jsme mapovat tento vzor webhooku zpět, představuje hello Pekárna vlastního rozhraní API, při můžete hello dort zákazníka, modul aplikace logiky představují hello. modul Hello volání rozhraní API se žádostí a zahrne adresu URL "zpětného volání".
Pokud se provádí hello úlohy, rozhraní API používá hello URL toonotify hello modul a vrátit data tooyour logiku aplikace, která pak bude pokračovat pracovního postupu. 

Pro tento vzor nastavit dva koncové body na vašem řadiči: `subscribe` a`unsubscribe`

*  `subscribe`koncový bod: při provádění dosáhne vaše rozhraní API akce v pracovním postupu hello, hello Logic Apps modul volání hello `subscribe` koncový bod. Tento krok způsobí, že hello logiku aplikace toocreate adresu URL zpětné volání, která ukládá vaše rozhraní API a potom počkejte hello zpětného volání z rozhraní API, když je práce dokončena. Rozhraní API pak zavolá zpátky adresa URL toohello HTTP POST a předá jakékoli vrácený obsah a hlavičky jako vstupní toohello aplikace logiky.

* `unsubscribe`koncový bod: Pokud zrušení spusťte aplikaci logiky hello hello Logic Apps modul volání hello `unsubscribe` koncový bod. Rozhraní API můžete zrušit registraci adresy URL hello zpětného volání a ukončení všech procesů podle potřeby.

![Vzor akce Webhooku](./media/logic-apps-create-api-app/custom-api-webhook-action-pattern.png)

> [!NOTE]
> V současné době nepodporuje hello návrhář aplikace na základě logiky zjišťování koncových bodů webhooku prostřednictvím Swagger. Proto tento vzor, je nutné tooadd [ **Webhooku** akce](../connectors/connectors-native-webhook.md) a zadejte adresu URL hello, hlavičky a obsah vaší žádosti. Viz také [akce pracovního postupu a aktivační události](logic-apps-workflow-actions-triggers.md#api-connection-webhook-action). toopass v adrese URL hello zpětného volání, můžete použít hello `@listCallbackUrl()` funkce workflowu v některém z předchozí pole hello podle potřeby.

> [!TIP]
> Pro webhook příklad vzoru zkontrolujte to [ukázka aktivační události webhooku v Githubu](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

<a name="triggers"></a>

## <a name="trigger-patterns"></a>Vzory aktivační události

Vlastního rozhraní API může fungovat jako [ *aktivační událost* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , spustí aplikace logiky, pokud nová data nebo událost splňuje zadanou podmínku. Této aktivační události můžete buď, pravidelně kontrolovat, nebo počkejte a naslouchat pro nová data nebo události na váš koncový bod služby. Pokud nová data nebo událost splňuje zadanou podmínku Dobrý den, Spouštěč hello spustí a spustí aplikace hello logiku, která naslouchá toothat aktivační události. aplikace logiky toostart tímto způsobem, rozhraní API můžete postupovat podle hello [ *cyklického dotazování aktivační událost* ](#polling-triggers) nebo hello [ *aktivační události webhooku* ](#webhook-triggers) vzor. Tyto vzory jsou podobné svými protějšky tootheir pro [dotazování akce](#async-pattern) a [akce webhooku](#webhook-actions). Také další informace o [měření využití pro aktivační procedury](logic-apps-pricing.md).

<a name="polling-triggers"></a>

### <a name="check-for-new-data-or-events-regularly-with-hello-polling-trigger-pattern"></a>Kontrola pro nová data nebo události pravidelně se hello cyklického dotazování aktivační událost vzor

A *cyklického dotazování aktivační událost* funguje podobně jako hello [dotazování akce](#async-pattern) dříve popsaných v tomto tématu. modul Logic Apps Hello pravidelně volá a zkontroluje hello koncový bod aktivační události pro události nebo nová data. Pokud modul hello najde nová data nebo událost, která splňuje zadanou podmínku, hello aktivační událost se aktivuje. Modul hello vytvoří instanci aplikace logiky, která zpracovává data hello jako vstup. 

![Cyklického dotazování aktivační událost vzor](./media/logic-apps-create-api-app/custom-api-polling-trigger-pattern.png)

> [!NOTE]
> Každý požadavek dotazování se počítá jako spuštění akce, i když je vytvořena žádná instance aplikace logiky. zpracování tooprevent hello stejná data vícekrát, aktivační událost by měl čištění data, která již byla přečtena a předána toohello aplikace logiky.

Zde jsou konkrétní kroky pro aktivační událost dotazování, popsané z hlediska hello rozhraní API:

| Najít nová data nebo událost?  | Odpověď rozhraní API | 
| ------------------------- | ------------ |
| Najít | Vrátí HTTP `200 OK` stav s datovou část odpovědi hello (vstup pro další krok). <br/>Tato odezva vytvoří instanci aplikace logiky a spustí pracovní postup hello. |
| Nebyl nalezen | Vrátí HTTP `202 ACCEPTED` stavu `location` záhlaví a `retry-after` záhlaví. <br/>Aktivační události, hello `location` záhlaví by obsahovat také `triggerState` parametr dotazu, který je obvykle "razítka". Rozhraní API můžete použít tento identifikátor tootrack hello posledního hello logiku aplikace byla spuštěna. |

Například tooperiodically zkontrolujte vaši službu pro nové soubory, může vytvořit cyklického dotazování aktivační událost, která má tyto chování:

| Žádost obsahuje `triggerState`? | Odpověď rozhraní API |
| -------------------------------- | -------------|
| Ne | Vrátí HTTP `202 ACCEPTED` stav plus `location` hlavička s `triggerState` nastavit toohello aktuální čas a hello `retry-after` too15 interval (sekundy). |
| Ano | Zkontrolujte vaši službu pro soubory, které jsou přidány po hello `DateTime` pro `triggerState`. |

| Počet nalezených souborů | Odpověď rozhraní API |
| --------------------- | -------------|
| Jeden soubor | Vrátí HTTP `200 OK` stavu a hello obsahu datové části, aktualizace `triggerState` toohello `DateTime` hello vrátil souboru a nastavit `retry-after` too15 interval (sekundy). |
| Více souborů | Vrátí jeden soubor na čas a HTTP `200 OK` stav, aktualizace `triggerState`a sadu hello `retry-after` too0 interval (sekundy). </br>Tyto kroky mohli hello modul vědět, že další data nejsou k dispozici, a kterou byl modul hello okamžitě by měl hello data žádosti z adresy URL hello v hello `location` záhlaví. |
| Žádné soubory | Vrátí HTTP `202 ACCEPTED` stav, neměnit `triggerState`a sadu hello `retry-after` too15 interval (sekundy). |

> [!TIP]
> Příklad cyklického dotazování aktivační událost vzor, zkontrolujte tato [cyklického dotazování aktivační událost řadiče ukázka v Githubu](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/PollTriggerController.cs).

<a name="webhook-triggers"></a>

### <a name="wait-and-listen-for-new-data-or-events-with-hello-webhook-trigger-pattern"></a>Počkejte a naslouchat nová data nebo událostí pomocí vzoru aktivační události webhooku hello

Aktivační události webhooku je *aktivační událost nabízené* které vyčká a čeká na nová data nebo události na váš koncový bod služby. Pokud nová data nebo událost splňuje hello zadaná podmínka, aktivuje se aktivační událost hello a vytvoří instanci aplikace logiky, která pak zpracovává hello data jako vstup.
Aktivační události Webhooku fungují podobně jako hello [akce webhooku](#webhook-actions) dříve popsané v tomto tématu a nastavení je `subscribe` a `unsubscribe` koncové body. 

* `subscribe`koncový bod: při přidání a uložit aktivační události webhooku aplikace logiky, hello Logic Apps modul volání hello `subscribe` koncový bod. Tento krok způsobí, že hello logiku aplikace toocreate adresu URL zpětné volání, která ukládá vaše rozhraní API. Pokud je nová data nebo hello událost, která splňuje zadanou podmínku, zavolá rozhraní API zpět adresa URL toohello HTTP POST. datová část obsahu Hello a hlavičky předat jako vstupní toohello aplikace logiky.

* `unsubscribe`koncový bod: Pokud se odstraní celý logiku aplikace nebo aktivační události webhooku hello, hello Logic Apps modul volání hello `unsubscribe` koncový bod. Rozhraní API můžete zrušit registraci adresy URL hello zpětného volání a ukončení všech procesů podle potřeby.

![Vzor aktivační události Webhooku](./media/logic-apps-create-api-app/custom-api-webhook-trigger-pattern.png)

> [!NOTE]
> V současné době nepodporuje hello návrhář aplikace na základě logiky zjišťování koncových bodů webhooku prostřednictvím Swagger. Proto tento vzor, je nutné tooadd [ **Webhooku** aktivační událost](../connectors/connectors-native-webhook.md) a zadejte adresu URL hello, hlavičky a obsah vaší žádosti. Viz také [aktivační událost HTTPWebhook](logic-apps-workflow-actions-triggers.md#httpwebhook-trigger). toopass v adrese URL hello zpětného volání, můžete použít hello `@listCallbackUrl()` funkce workflowu v některém z předchozí pole hello podle potřeby.
>
> zpracování tooprevent hello stejná data vícekrát, aktivační událost by měl čištění data, která již byla přečtena a předána toohello aplikace logiky.

> [!TIP]
> Pro webhook příklad vzoru zkontrolujte to [ukázka řadiče aktivační události webhooku v Githubu](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

## <a name="deploy-call-and-secure-custom-apis"></a>Nasazení, volání a zabezpečte vlastní rozhraní API

Po vytvoření vaše vlastní rozhraní API, takže je můžete bezpečně volat nastavte vaše rozhraní API pro nasazení. Zjistěte, jak příliš[nasazení, volání a zabezpečte vlastní rozhraní API pro logic apps](./logic-apps-custom-hosted-api.md).

## <a name="publish-custom-apis-tooazure"></a>Publikovat vlastní tooAzure rozhraní API

toomake vaše vlastní rozhraní API v Azure, veřejné používat odeslání vašeho kandidátů toohello [programu Microsoft Azure Certified](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Podpora

Obraťte se na nápovědu s vlastním rozhraním API, [ customapishelp@microsoft.com ](mailto:customapishelp@microsoft.com).

tooask otázky, odpovědi na dotazy a zjistit, jaké jiných uživatelů Azure Logic Apps dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp zlepšit konektory a aplikace logiky, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Další kroky

* [Měření využití pro akce a aktivační události](logic-apps-pricing.md)
* [Zpracování typů obsahu](./logic-apps-content-type.md)
* [Zpracování chyb a výjimek](./logic-apps-exception-handling.md)
* [Zabezpečení přístupu k tooyour logic apps](./logic-apps-securing-a-logic-app.md)
* [Volání, aktivaci nebo vnořit aplikace logiky s koncovými body HTTP](./logic-apps-http-endpoint.md)