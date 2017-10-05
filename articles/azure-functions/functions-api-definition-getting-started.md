---
title: "Začínáme s metadaty OpenAPI v Azure Functions | Microsoft Docs"
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
ms.openlocfilehash: 9b861aacf31e17866293732dc2323f56014c1877
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a>Vytváření OpenAPI 2.0 (Swagger) Metadata pro funkce aplikace (Preview)

Tento dokument vás provede krok za krokem procesu vytváření definici OpenAPI pro jednoduché rozhraní API hostované na Azure Functions.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a>Co je OpenAPI (Swagger)?
[Swagger Metadata](http://swagger.io/) je soubor, který definuje provozní režimy rozhraní API a funkce a umožňuje funkce hostování rozhraní REST API pro širokou škálu na ostatní software. Microsoft nabídky jako PowerApps a [aplikace API](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), a také 3. stran vývojářských nástrojů jako [Postman](https://www.getpostman.com/docs/importing_swagger) a [mnoho další balíčky](http://swagger.io/tools/) všechny vaše rozhraní API, který se má používat s povolit Definici swaggeru.

## <a name="prepare-function"></a>Vytvoření funkce pomocí jednoduché rozhraní API
  Vytvoření definice OpenAPI, musíme nejprve vytvořit funkci s jednoduché rozhraní API. Pokud již máte rozhraní API hostované na funkce aplikace, můžete přeskočit přímo k další části
1. Vytvořte novou aplikaci funkce.
    1. [Portál Azure](https://portal.azure.com)  >  `+ New` > vyhledejte "Aplikace funkce"
1. Vytvořte novou funkci Aktivace protokolu HTTP v nové funkce aplikace
    1. Funkce je již nastavena kód definování velmi jednoduché rozhraní REST API.
    1. Libovolný řetězec funkci byl předán jako parametr dotazu nebo v těle se vrátí jako "Hello {vstup}"
1. Přejděte na `Integrate` kartě novou funkci triggeru protokolu HTTP
    1. Přepnutí `Allowed HTTP methods` na`Selected methods`
    1. V `Selected HTTP methods` zrušte všechny operace s výjimkou POST.
    ![Metody vybrané HTTP](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)
    1. Tento krok zjednoduší definice rozhraní API později.

## <a name="enable"></a>Povolení podpory definice rozhraní API
1. Přejděte na `your function name`  >  `Platform Features`  >  `API Definition` 
 ![karta definice.](./media/functions-api-definition-getting-started/definitiontab.png)
1. Nastavit `API Definition Source` do`Function (preview)`
    1. Tento krok povoluje sada možností OpenAPI pro funkce aplikace, včetně koncový bod pro hostování soubor OpenAPI z domény aplikaci funkce, vložené kopie [OpenAPI Editor](http://editor.swagger.io)a generátor definice rychlý start.
![Povoleno definice](./media/functions-api-definition-getting-started/enabledefinition.png)

## <a name="create-definition"></a>Vytvoření vaší definice rozhraní API ze šablony
1. Klikněte na `Generate API Definition template`.
    1. Tento krok kontroluje funkce aplikace pro funkce triggeru protokolu HTTP a používá informace v functions.json k vygenerování dokument OpenAPI.
1. Přidat objekt operaci tak, aby`paths: /api/yourfunctionroute post:`
    1. Dokument OpenAPI rychlý start je přehled úplné OpenAPI dokumentů. To vyžaduje další metadata, aby se úplná definice OpenAPI, jako jsou operace objekty a odpovědi šablony.
    1. Objekt operace ukázka níže má vyplnění vytváří nebo využívá část, objekt parametr a objektu odpovědi.
    
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
    > Podívejte se na [přizpůsobit vaší definici Swaggeru pro PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) Další informace.

1. Klikněte na tlačítko `save` uložte provedené změny ![přidání definice šablony](./media/functions-api-definition-getting-started/addingtemplate.png)

## <a name="use-definition"></a>Pomocí vaší definice rozhraní API
1. Kopie vašeho `API definition URL` a vložte jej do novou kartu prohlížeče na nezpracovaná OpenAPI dokument.
1. Dokument OpenAPI můžete importovat do libovolný počet nástrojů pro testování a integrace s použitím tuto adresu URL.
    1. Mnoho prostředků Azure dokážou automaticky importovat OpenAPI dokumentu pomocí URL definice rozhraní API, který je uložený v nastavení aplikace funkce. Jako součást `Function` zdroj definice rozhraní API, budeme aktualizovat tuto adresu url pro vás.


## <a name="test-definition"></a>Testování vaší definice rozhraní API pomocí uživatelského rozhraní Swagger
Testujete nástrojů integrovaných zobrazení uživatelského rozhraní editoru vestavěného okna definice rozhraní API. Přidejte svůj klíč rozhraní API a potom pomocí `Try this operation` tlačítko jednotlivých metod. Nástroj použije vaše definice rozhraní API pro formát žádosti a úspěšné odpovědi bude znamenat, že vaše definice správné.

### <a name="steps"></a>pomocí následujících kroků:

1. Zkopírujte klíč rozhraní API – funkce
    1. Klíč rozhraní API najdete v aktivační události funkce protokolu HTTP v části `function name` > `Keys` > `Function Keys` 
   ![funkční klávesa](./media/functions-api-definition-getting-started/functionkey.png)
1. Přejděte do `API Definition` stránky.
    1. Klikněte na tlačítko `Authenticate` a přidejte klíč rozhraní API funkce k objektu zabezpečení v horní části.
  ![Klíč OpenAPI](./media/functions-api-definition-getting-started/definitionTest auth.png)
1. Vyberte`/api/yourfunctionroute` > `POST`
1. Klikněte na tlačítko `Try it out` a zadejte název pro testování
1. Měli byste vidět úspěšný výsledek pod `Pretty` 
 ![definice rozhraní API testu](./media/functions-api-definition-getting-started/definitionTest.png)

## <a name="next-steps"></a>Další kroky
* [Definice OpenAPI v přehled funkcí](functions-api-definition.md)
  * Úplnou dokumentaci pro další informace o podporovaných OpenAPI.
* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  * Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.
* [Úložiště GitHub – Azure Functions](https://github.com/Azure/Azure-Functions/)
  * Podívejte se na Githubu funkce pro vaše názory na preview podporu definice rozhraní API! Ujistěte se, problém Githubu nic, co chcete zobrazit aktualizovaný.
