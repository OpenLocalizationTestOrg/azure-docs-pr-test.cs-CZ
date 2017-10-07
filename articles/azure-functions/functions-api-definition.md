---
title: metadata aaaOpenAPI v Azure Functions | Microsoft Docs
description: "Přehled podpory OpenAPI v Azure Functions"
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
ms.openlocfilehash: fff8f14110469a002a6c9dca03f641672003a3a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>Podpora metadat OpenAPI 2.0 v Azure Functions (preview)
OpenAPI 2.0 (dříve Swagger) podpora metadat v Azure Functions je ukázková funkce, které můžete použít toowrite OpenAPI 2.0 definice uvnitř funkce aplikace. Tento soubor pak můžete hostovat pomocí funkce aplikace hello.

[OpenAPI metadata](http://swagger.io/) umožňuje funkci, která je hostitelem rozhraní REST API toobe spotřebovávají širokou škálu na ostatní software. Tento software obsahuje Microsoft nabídky jako PowerApps a hello [funkce API Apps služby Azure App Service](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), jako jsou nástroje pro vývojáře třetích stran [Postman](https://www.getpostman.com/docs/importing_swagger), a [mnoho další balíčky](http://swagger.io/tools/).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
>Doporučujeme začít s hello [kurz Začínáme](./functions-api-definition-getting-started.md) a vrácením toothis dokumentu toolearn informace týkající se konkrétních funkcí.

## <a name="enable"></a>Povolit podporu definice OpenAPI
Můžete nakonfigurovat všechna nastavení OpenAPI na hello **definice rozhraní API** stránky v aplikaci funkce **funkce**.

generování hello tooenable definici hostované OpenAPI a definice rychlý start, nastavte **zdroj definice rozhraní API** příliš**– funkce (Preview)**. **Externí adresu URL** umožňuje vaší toouse funkce definici OpenAPI, který je hostovaný jinde.

## <a name="generate-definition"></a>Generovat kostru Swagger z vaší funkce metadat
Šablonu můžete zahájit zápis svou první OpenAPI definici. Hello definice šablony funkce vytvoří definici zhuštěných OpenAPI pomocí všechny hello metadata v souboru function.json hello pro jednotlivé funkce aktivace protokolu HTTP. Budete potřebovat toofill v další informace o rozhraní API z hello [OpenAPI specifikace](http://swagger.io/specification/), jako je například šablony žádostí a odpovědí.

Podrobné pokyny najdete v tématu hello [kurz Začínáme](./functions-api-definition-getting-started.md).

### <a name="templates"></a>Dostupné šablony

|Name (Název)| Popis |
|:-----|:-----|
|Vygenerovaný definice|Definici OpenAPI s maximální velikostí hello informace, které lze odvodit z existující metadata hello funkce.|

### <a name="quickstart-details"></a>Zahrnuté metadata v definici hello vygenerována

Následující tabulka představuje Hello hello nastavení portálu Azure a odpovídajících dat v function.json, jako je namapované toohello generované Swagger kostru.

|Swagger.JSON|Portál uživatelského rozhraní|Function.JSON|
|:----|:-----|:-----|
|[Hostitele](http://swagger.io/specification/#fixed-fields-15)|**Funkce nastavení aplikace** > **nastavení služby App Service** > **přehled** > **adresy URL**|*Není k dispozici*
|[Cesty](http://swagger.io/specification/#paths-object-29)|**Integrovat** > **metody vybrané HTTP**|Vazby: trasy
|[Položky cesty](http://swagger.io/specification/#path-item-object-32)|**Integrovat** > **šablonu trasy**|Vazby: metody
|[Zabezpečení](http://swagger.io/specification/#security-scheme-object-112)|**Klíče**|*Není k dispozici*|
|operationID *|**Trasy + povolené příkazy**|Trasy + povolených příkazů|

\*ID operace Hello se vyžaduje jenom pro integraci s PowerApps a toku.
> [!NOTE]
> rozšíření x-ms souhrn Hello poskytuje zobrazovaný název Logic Apps, PowerApps a toku.
>
> Další, najdete v části toolearn [přizpůsobit vaší definici Swaggeru pro PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).

## <a name="CICD"></a>Použijte definici CI/CD tooset rozhraní API

 Je nutné povolit rozhraní API definice hostování portálu hello před povolením zdroj ovládacího prvku toomodify vaše definice rozhraní API od správy zdrojového kódu. Postupujte podle těchto pokynů:

1. Procházet příliš**definice rozhraní API (preview)** v aplikaci nastavení funkce.
  1. Nastavit **zdroj definice rozhraní API** příliš**funkce**.
  1. Klikněte na tlačítko **šablony definice rozhraní API generovat** a potom **Uložit** toocreate definice šablony úpravy později.
  1. Poznámka: adresa URL definice rozhraní API a klíč.
1. [Nastavit nepřetržité integrace/průběžné nasazování (CI/CD)](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements).
2. Upravit swagger.json ve správě zdrojového kódu v \site\wwwroot\.azurefunctions\swagger\swagger.json.

Nyní změny jsou funkce aplikace na adrese URL definice rozhraní API hello hostované tooswagger.json do úložiště a klíč, který jste si poznamenali v kroku 1.c.

## <a name="next-steps"></a>Další kroky
* [Kurz Začínáme](functions-api-definition-getting-started.md). Zkuste naše toosee návod definici OpenAPI v akci.
* [Azure úložiště GitHub funkce](https://github.com/Azure/Azure-Functions/). Podívejte se na hello funkce úložiště toogive nám zpětnou vazbu na preview podporu definice hello rozhraní API. Ujistěte se, problém Githubu pro všechno, co chcete toosee aktualizovat.
* [Referenční informace pro vývojáře Azure Functions](functions-reference.md). Další informace o kódování funkcí a definování triggerů a vazeb.
