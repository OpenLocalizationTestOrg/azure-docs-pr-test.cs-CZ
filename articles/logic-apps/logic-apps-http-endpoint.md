---
title: "aaaCall, aktivaci nebo vnořit pracovních s koncovými body HTTP - Azure Logic Apps | Microsoft Docs"
description: "Nastavení toocall koncové body HTTP, aktivační události nebo vnoření pracovní postupy pro Azure Logic Apps"
services: logic-apps
keywords: "pracovní postupy, koncových bodů protokolu HTTP"
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 072a314c3bff75ab7696f86bb063bb7c03c4ae89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a>Volání, aktivaci nebo vnořit pracovních s koncovými body HTTP v aplikacích logiky

Synchronní koncové body HTTP můžou nativně zpřístupnit jako triggery pro aplikace logiky, mohli aktivovat nebo volání aplikace logiky prostřednictvím adresy URL. Pracovní postupy lze také vnořit ve vašich logic apps pomocí vzor s koncových bodů.

Koncové body HTTP toocreate, můžete přidat tyto triggery tak, aby aplikace logiky může přijímat příchozí požadavky:

* [Požadavek](../connectors/connectors-native-reqres.md)

* [Rozhraní API připojení Webhooku](logic-apps-workflow-actions-triggers.md#api-connection-trigger)

* [HTTP Webhook](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > I když příklady použití hello **požadavku** aktivační událost, můžete použít některou z hello uvedené aktivace protokolu HTTP a všechny zásady stejně jako vztahují toohello jiné typy aktivační události.

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a>Nastavit koncový bod HTTP pro svou aplikaci logiky

toocreate koncový bod protokolu HTTP, přidejte aktivační událost, která může přijímat příchozí požadavky.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure"). Přejděte aplikace logiky tooyour a otevřete návrhář aplikace logiky.

2. Přidejte aktivační událost, která umožní aplikaci logiky přijímat příchozí požadavky. Například přidejte hello **požadavku** aplikace logiky tooyour aktivační události.

3.  V části **požadavku schématu JSON textu**, můžete volitelně zadat schématu JSON pro datové hello (data), které předpokládáte tooreceive hello aktivační události.

    Návrhář Hello používá toto schéma pro generování tokenů, aplikace logiky můžete tooconsume, analýzy a předejte data z hello aktivační události prostřednictvím pracovního postupu. 
    Další informace o [tokeny vygenerovat z JSON schémata](#generated-tokens).

    V tomto příkladu zadejte schéma hello v Návrháři hello zobrazeny:

    ```json
    {
      "type": "object",
      "properties": {
        "address": {
          "type": "string"
        }
      },
      "required": [
        "address"
      ]
    }
    ```

    ![Přidat akci požadavku hello][1]

    > [!TIP]
    > 
    > Schéma pro ukázku datové části JSON můžete vygenerovat z nástroje, jako je [jsonschema.net](http://jsonschema.net/), nebo v hello **požadavku** aktivační události tak, že zvolíte **použití ukázkové datové části toogenerate schématu**. 
    > Zadejte vaše datová část ukázky a vyberte **provádí**.

    Například tato ukázková datová část:

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    generuje toto schéma:

    ```json
    }
       "type": "object",
       "properties": {
          "address": {
             "type": "string" 
          }
       }
    }
    ```

4.  Uložte svou aplikaci logiky. V části **HTTP POST toothis URL**, nyní byste měli najít adresu URL generovaného zpětného volání, jako tento ukázkový:

    ![Adresa URL generovaného zpětného volání pro koncový bod](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    Tato adresa URL obsahuje klíč sdíleného přístupového podpisu (SAS) v hello parametry dotazu, které se používají pro ověřování. 
    Můžete také získat adresu URL koncového bodu hello HTTP z vaší přehledu aplikace logiky v hello portálu Azure. V části **historie aktivační událost**, vyberte aktivační událost:

    ![Získat adresu URL koncového bodu protokolu HTTP z portálu Azure][2]

    Nebo můžete získat adresu URL hello tím, že toto volání:

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-hello-http-method-for-your-trigger"></a>Změnit metodu HTTP hello aktivační události

Ve výchozím nastavení, hello **požadavku** aktivační událost očekává požadavek HTTP POST, ale můžete použít jinou metodu HTTP. 

> [!NOTE]
> Můžete zadat typ pouze jednu metodu.

1. Na vaše **požadavku** aktivovat, zvolte **zobrazit rozšířené možnosti**.

2. Otevřete hello **metoda** seznamu. V tomto příkladu vyberte **získat** , aby bylo později otestovat adresu URL vašeho koncového bodu protokolu HTTP.

    > [!NOTE]
    > Můžete vybrat jinou metodu HTTP, nebo zadat vlastní metodu pro aplikaci logiky.

    ![Změnit metodu HTTP](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a>Přijměte parametry prostřednictvím vaše adresa URL koncového bodu protokolu HTTP

Když chcete parametry tooaccept koncový bod adresy URL protokolu HTTP, přizpůsobit vaší aktivační událost relativní cestu.

1. Na vaše **požadavku** aktivovat, zvolte **zobrazit rozšířené možnosti**. 

2. V části **metoda**, zadejte metodu hello HTTP, které chcete toouse váš požadavek. V tomto příkladu vyberte hello **získat** metodu, pokud jste tak ještě neučinili, tak, aby bylo možné otestovat adresu URL vašeho koncového bodu protokolu HTTP.

      > [!NOTE]
      > Když zadáte relativní cesta aktivační události, musíte zadat metodu HTTP také explicitně aktivační události.

3. V části **relativní cestu**, zadejte relativní cestu hello hello parametru, který by měl přijímat vaše adresa URL, například `customers/{customerID}`.

    ![Zadejte metodu HTTP hello a relativní cestu pro parametr](./media/logic-apps-http-endpoint/relativeurl.png)

4. toouse hello parametr, přidejte **odpovědi** aplikace logiky tooyour akce. (V části aktivační událost, zvolte **nový krok** > **přidat akci** > **odpovědi**) 

5. Do odpovědi **textu**, zahrnout hello token pro hello parametr, který jste zadali v relativní cestě vaší aktivační události.

    Například tooreturn `Hello {customerID}`, aktualizovat vaši odpověď **textu** s `Hello {customerID token}`. 
    Hello dynamického obsahu seznamu by měl zobrazit a zobrazit hello `customerID` tokenu pro tooselect je.

    ![Přidat parametr tooresponse textu](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    Vaše **textu** by měl vypadat jako tento příklad:

    ![Text odpovědi s parametrem](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. Uložte svou aplikaci logiky. 

    Vaše adresa URL koncového bodu protokolu HTTP nyní zahrnuje hello relativní cesta, například: 

    https & # 58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...

7. tootest koncový bod protokolu HTTP, kopírování a vložení hello aktualizované adresu URL do jiného okna prohlížeče, ale nahraďte `{customerID}` s `123456`, a stiskněte klávesu Enter.

    Váš prohlížeč by měl zobrazit tento text: 

    `Hello 123456`

<a name="generated-tokens"></a>
### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a>Tokeny vygenerovat z JSON schémata pro svou aplikaci logiky

Když zadáte schématu JSON ve vaší **požadavku** aktivovat, hello logiku aplikace Návrhář generuje tokeny pro vlastnosti v tomto schématu. Pak můžete tyto tokeny pro předávání dat prostřednictvím pracovní postup aplikace logiky.

Pro tento příklad, pokud přidáte hello `title` a `name` vlastnosti schématu JSON tooyour, jejich tokeny jsou nyní k dispozici toouse v dalších krocích pracovního postupu. 

Tady je hello dokončení schématu JSON:

```json
{
   "type": "object",
   "properties": {
      "address": {
         "type": "string"
      },
      "title": {
         "type": "string"
      },
      "name": {
         "type": "string"
      }
   },
   "required": [
      "address",
      "title",
      "name"
   ]
}
```

## <a name="create-nested-workflows-for-logic-apps"></a>Vytvoření vnořené pracovní postupy pro logic apps

Pracovní postupy lze vnořit ve vaší aplikaci logiky můžete přidat další logiku aplikace, které může přijímat požadavky. tooinclude těchto aplikací logiky, přidejte hello **Azure Logic Apps – zvolte pracovní postup Logic Apps** tooyour akce aktivační události. Pak můžete vybrat z oprávněné logiku aplikace.

![Přidat další aplikace logiky](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a>Volání nebo aktivovat aplikace logiky prostřednictvím koncových bodů protokolu HTTP

Jakmile vytvoříte koncový bod protokolu HTTP, můžete aktivovat svou aplikaci logiky prostřednictvím `POST` metoda toohello úplnou adresu URL. Logiku aplikace mají integrovanou podporu pro koncové body přímý přístup.

## <a name="reference-content-from-an-incoming-request"></a>Odkaz na obsah z příchozího požadavku

Pokud je typ obsahu hello je `application/json`, vlastnosti, můžete odkazovat z příchozího požadavku hello. Jinak hodnota obsahu považovány za jednu jednotku binární, můžete předat tooother rozhraní API. Tento obsah uvnitř pracovního postupu hello tooreference, je nutné převést obsah. Například pokud předáte `application/xml` obsahu, můžete použít `@xpath()` pro extrakci XPath nebo `@json()` pro převod XML tooJSON. Další informace o [práce s typy obsahu](../logic-apps/logic-apps-content-type.md).

tooget hello výstup z příchozího požadavku, můžete použít hello `@triggerOutputs()` funkce. výstup Hello může vypadat jako tento ukázkový:

```json
{
    "headers": {
        "content-type" : "application/json"
    },
    "body": {
        "myProperty" : "property value"
    }
}
```

tooaccess hello `body` vlastnost můžete konkrétně použít hello `@triggerBody()` zástupce. 

## <a name="respond-toorequests"></a>Odpověď toorequests

Můžete chtít toorespond toocertain požadavků, které spustí aplikaci logiky vrácením obsahu toohello volajícího. tooconstruct hello stavový kód, záhlaví a text na vaši odpověď, můžete použít hello **odpovědi** akce. Tato akce může vyskytovat kdekoli v aplikaci logiky, nikoli pouze na konci hello pracovního postupu.

> [!NOTE] 
> Pokud svou aplikaci logiky neobsahuje **odpovědi**, odpoví koncový bod hello HTTP *okamžitě* s **202 platných** stavu. Navíc se pro hello původní žádost tooget hello odpověď, a všechny kroky potřebné k odpovědi hello musí dokončit hello [časový limit požadavku](./logic-apps-limits-and-config.md) Pokud volání hello pracovního postupu jako vnořené logiku aplikace. V případě žádná odpověď v rámci tohoto limitu hello příchozího požadavku vyprší časový limit a obdrží odpověď hello HTTP **408 časový limit klienta**. Pro vnořené logic apps pokračuje aplikace logiky nadřazené hello toowait pro odpověď, dokud nebude dokončena, bez ohledu na to, jak dlouho se vyžaduje.

### <a name="construct-hello-response"></a>Konstrukce hello odpovědi

Můžete zahrnout více než jedno záhlaví a libovolný typ obsahu odpovědi hello. V našem příkladu odpovědi hello záhlaví Určuje, že hello odpověď má typ obsahu `application/json`. a hello text obsahuje `title` a `name`, podle schématu JSON hello aktualizované dříve hello **požadavku** aktivační události.

![Akce odpovědi HTTP][3]

Odpovědi mít tyto vlastnosti:

| Vlastnost | Popis |
| --- | --- |
| statusCode |Určuje kód stavu hello HTTP pro odpovídá toohello příchozího požadavku. Tento kód může být jakýkoli platný stavový kód, který začíná 2xx, 4xx nebo 5xx. Stavové kódy 3xx však nejsou povoleny. |
| Záhlaví |Definuje libovolný počet tooinclude hlavičky odpovědi hello. |
| Text |Určuje objekt textu, který může být řetězec, objekt JSON nebo i binární obsah na něj odkazovat z předchozího kroku. |

Tady je co schématu JSON hello vypadá jako teď pro hello **odpovědi** akce:

``` json
"Response": {
   "inputs": {
      "body": {
         "title": "@{triggerBody()?['title']}",
         "name": "@{triggerBody()?['name']}"
      },
      "headers": {
           "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {},
   "type": "Response"
}
```

> [!TIP]
> Zvolte tooview hello dokončení JSON definici aplikace logiky na hello návrhář aplikace na základě logiky, **Code zobrazení**.

## <a name="q--a"></a>Dotazy a odpovědi

#### <a name="q-what-about-url-security"></a>Otázka: co o adresu URL zabezpečení?

Odpověď: azure bezpečně generuje logiku aplikace zpětného volání adresy URL pomocí sdíleného přístupového podpisu (SAS). Tento podpis projdou jako parametr dotazu a musí být ověřené před svou aplikaci logiky můžete aktivovat. Podpis hello jedinečná kombinace tajný klíč na aplikaci logiky, název aktivační události hello a hello operace, které se provádí pomocí vygeneruje Azure. Pokud má uživatel přístup toohello tajný logiku aplikace klíče, se proto nelze generovat platný podpis.

   > [!IMPORTANT]
   > Pro produkční a zabezpečených systémů důrazně doporučujeme před volání přímo z prohlížeče hello aplikace logiky, protože:
   > 
   > * Hello sdílený přístupový klíč se zobrazí v adrese URL hello.
   > * Nelze spravovat zabezpečení obsahu zásady z důvodu tooshared domén přes aplikaci logiky odběratele.

#### <a name="q-can-i-configure-http-endpoints-further"></a>Otázka: je možné nakonfigurovat další koncové body HTTP?

Odpověď: Ano koncových bodů protokolu HTTP podporují pokročilejší konfigurace prostřednictvím [ **API Management**](../api-management/api-management-key-concepts.md). Tato služba také nabízí možnost hello pro tooconsistently můžete spravovat všechna vaše rozhraní API, včetně aplikace logiky, nastavit vlastní názvy domén, použít další metody ověřování a informace, například:

* [Metoda žádosti o změnu hello](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [Segmenty adres URL hello hello žádosti o změnu](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* Nastavení domény API Management do hello [portál Azure](https://portal.azure.com/ "portálu Azure")
* Nastavení zásad toocheck pro základní ověřování

#### <a name="q-what-changed-when-hello-schema-migrated-from-hello-december-1-2014-preview"></a>Otázka: co se změnilo při hello schématu migraci z verze preview 1 prosince 2014 hello?

Odpověď: Zde je souhrn o tyto změny:

| Preview 1 prosince 2014 | 1. června 2016 |
| --- | --- |
| Klikněte na tlačítko **naslouchací proces protokolu HTTP** aplikace API |Klikněte na tlačítko **ruční aktivační událost** (žádná aplikace API povinné) |
| Naslouchací proces protokolu HTTP nastavení "*automaticky odešle odpověď*" |Buď zahrnout **odpovědi** akce nebo není v definici pracovního postupu hello |
| Konfigurace ověřování Basic nebo OAuth |přes správu rozhraní API |
| Konfigurace metody HTTP |V části **zobrazit rozšířené možnosti**, zvolte metodu HTTP |
| Nakonfigurujte relativní cestu |V části **zobrazit rozšířené možnosti**, přidejte relativní cestu |
| Referenční dokumentace příchozí textu hello prostřednictvím`@triggerOutputs().body.Content` |Referenční dokumentace prostřednictvím`@triggerOutputs().body` |
| **Odeslání odpovědi HTTP** akce hello naslouchací proces protokolu HTTP |Klikněte na tlačítko **reakce tooHTTP požadavku** (žádná aplikace API povinné) |

## <a name="get-help"></a>Podpora

tooask otázky, odpovědi na otázky a zjistěte, jaké další Azure Logic Apps uživatelé dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

* [Vytváření definic aplikací logiky](./logic-apps-author-definitions.md)
* [Zpracování chyb a výjimek](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png
