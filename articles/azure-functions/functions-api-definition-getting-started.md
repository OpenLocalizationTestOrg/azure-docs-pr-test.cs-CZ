---
title: "aaaGetting Začínáme s metadaty OpenAPI v Azure Functions | Microsoft Docs"
description: "Začínáme s podporou OpenAPI v Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: fee3464c9a0a11b6d3891ccd0e9c5343d6bedcad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a>Vytváření OpenAPI 2.0 (Swagger) Metadata pro funkce aplikace (Preview)

Tento dokument vás provede procesem hello krok za krokem vytvoření definici OpenAPI pro jednoduché rozhraní API hostované na Azure Functions.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a>Co je OpenAPI (Swagger)?
[Swagger Metadata](http://swagger.io/) je soubor, který definuje hello funkce operační režimy rozhraní API a umožňuje funkce hostování REST API toobe spotřebovávají širokou škálu na ostatní software. Microsoft nabídky jako PowerApps a [aplikace API](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), a také 3. stran vývojářských nástrojů jako [Postman](https://www.getpostman.com/docs/importing_swagger) a [mnoho další balíčky](http://swagger.io/tools/) všechny vaše rozhraní API toobe spotřebované s povolit Definici swaggeru.

## <a name="prepare-function"></a>Vytvoření funkce pomocí jednoduché rozhraní API
  toocreate definici OpenAPI, potřebujeme nejprve toocreate funkce s jednoduché rozhraní API. Pokud již máte rozhraní API hostované na funkce aplikace, můžete přeskočit přímých toohello další části
1. Vytvořte novou aplikaci funkce.
    1. [Portál Azure](https://portal.azure.com)  >  `+ New` > vyhledejte "Aplikace funkce"
1. Vytvořte novou funkci Aktivace protokolu HTTP v nové funkce aplikace
    1. Funkce je již nastavena kód definování velmi jednoduché rozhraní REST API.
    1. Libovolný řetězec úspěšnou toohello funkce jako parametr dotazu nebo v textu hello se vrátí jako "Hello {vstup}"
1. Přejděte toohello `Integrate` kartě novou funkci triggeru protokolu HTTP
    1. Přepnutí `Allowed HTTP methods` příliš`Selected methods`
    1. V `Selected HTTP methods` zrušte všechny operace s výjimkou POST.
    ![Metody vybrané HTTP](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)
    1. Tento krok zjednoduší definice rozhraní API později.

## <a name="enable"></a>Povolení podpory definice rozhraní API
1. Přejděte příliš`your function name` > `Platform Features` > `API Definition`
![karta definice.](./media/functions-api-definition-getting-started/definitiontab.png)
1. Nastavit `API Definition Source` příliš`Function (preview)`
    1. Tento krok povoluje sada možností OpenAPI pro funkce aplikace, včetně koncový bod toohost soubor OpenAPI z domény aplikaci funkce, jako vložené kopie hello [OpenAPI Editor](http://editor.swagger.io)a generátor definice rychlý start.
![Povoleno definice](./media/functions-api-definition-getting-started/enabledefinition.png)

## <a name="create-definition"></a>Vytvoření vaší definice rozhraní API ze šablony
1. Klikněte na `Generate API Definition template`.
    1. Tento krok kontroluje funkce aplikace pro funkce triggeru protokolu HTTP a použije hello údaje v functions.json toogenerate dokument OpenAPI.
1. Přidejte objekt operaci příliš`paths: /api/yourfunctionroute post:`
    1. Hello rychlý start OpenAPI dokument je přehled úplné OpenAPI dokumentů. To vyžaduje další metadata toobe úplné definice OpenAPI, například operace objekty a odpovědi šablony.
    1. vytváří nebo využívá část, objekt parametr a objekt odpovědi je vyplnění Hello ukázka operaci objekt níže.
    
    ```yaml
      post:
        operationId: /api/yourfunctionroute/post
        consumes: [application/json]
        produces: [application/json]
        parameters:
          - name: name
            in: body
            description: Your name
            x-ms-summary: Your name
            required: true
            schema:
              type: object
              properties:
                name:
                  type: string
        description: >-
          Replace with Operation Object
          #http://swagger.io/specification/#operationObject
        responses:
          '200':
            description: A Greeting
            x-ms-summary: Your name
            schema:
              type: string
        security:
          - apikeyQuery: []
    ```
    
    > [!NOTE]
    >  x-ms souhrn poskytuje zobrazovaný název Logic Apps, toku a PowerApps.
    >
    > Podívejte se na [přizpůsobit vaší definici Swaggeru pro PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) toolearn Další.

1. Klikněte na tlačítko `save` toosave změny ![přidání definice šablony](./media/functions-api-definition-getting-started/addingtemplate.png)

## <a name="use-definition"></a>Pomocí vaší definice rozhraní API
1. Kopie vašeho `API definition URL` a vložte jej do nového tooview karta prohlížeče nezpracovaná OpenAPI dokumentu.
1. Můžete importovat vaše OpenAPI tooany číslem nástrojů pro testování a integrace s použitím tuto adresu URL.
    1. Mnoho prostředků Azure jsou možné tooautomatically import dokumentu OpenAPI pomocí hello URL definice rozhraní API, který je uložený v nastavení aplikace funkce. Jako součást hello `Function` zdroj definice rozhraní API, budeme aktualizovat tuto adresu url pro vás.


## <a name="test-definition"></a>Pomocí tootest hello uživatelské rozhraní Swagger, vaše definice rozhraní API
Testujete nástrojů integrovaných v zobrazení uživatelského rozhraní toohello editoru definice rozhraní API hello vestavěného okna. Přidejte svůj klíč rozhraní API a pak použijte hello `Try this operation` tlačítko každá z metod. Nástroj pro Hello bude používat své žádosti hello tooformat definice rozhraní API a úspěšné odpovědi bude znamenat, že vaše definice správné.

### <a name="steps"></a>pomocí následujících kroků:

1. Zkopírujte klíč rozhraní API – funkce
    1. klíč Hello rozhraní API lze nalézt v aktivační události funkce protokolu HTTP v části `function name` > `Keys` > `Function Keys` 
   ![funkční klávesa](./media/functions-api-definition-getting-started/functionkey.png)
1. Přejděte toohello `API Definition` stránky.
    1. Klikněte na tlačítko `Authenticate` a přidejte objekt zabezpečení klíčů toohello vaše rozhraní API funkce v horní části hello.
  ![Klíč OpenAPI](./media/functions-api-definition-getting-started/definitionTest auth.png)
1. Vyberte`/api/yourfunctionroute` > `POST`
1. Klikněte na tlačítko `Try it out` a zadejte název tootest
1. Měli byste vidět úspěšný výsledek pod `Pretty` 
 ![definice rozhraní API testu](./media/functions-api-definition-getting-started/definitionTest.png)

## <a name="next-steps"></a>Další kroky
* [Definice OpenAPI v přehled funkcí](functions-api-definition.md)
  * Přečtěte si dokumentaci úplné hello Další informace o podporovaných OpenAPI.
* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  * Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.
* [Úložiště GitHub – Azure Functions](https://github.com/Azure/Azure-Functions/)
  * Podívejte se na Githubu funkce toogive hello nám zpětnou vazbu na hello rozhraní API definice podporu preview! Ujistěte se, problém Githubu pro všechno, co byste chtěli toosee aktualizovat.
