---
title: aaaWork s proxy Azure Functions | Microsoft Docs
description: "Základní informace o toouse funkce proxy Azure"
services: functions
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/11/2017
ms.author: mahender
ms.openlocfilehash: 4d94c89e8f8f2d2c771b01bae142bf9a4f3b7f2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-azure-functions-proxies-preview"></a>Práce s funkcí proxy Azure (preview)

> [!NOTE] 
> Azure proxy funkce je aktuálně ve verzi preview. Volné je během ve verzi preview, ale standardní funkce fakturace tooproxy spuštěních. Další informace najdete v tématu [ceny Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

Tento článek vysvětluje, jak tooconfigure a práce s Azure funkce proxy. Pomocí této funkce můžete určovat koncové body ve vaší aplikaci funkce, které jsou implementované pomocí jiný prostředek. Tyto servery proxy toobreak velké rozhraní API do více aplikací – funkce (jako architektury mikroslužby), můžete použít při stále poskytovat jeden prostor rozhraní API pro klienty.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <a name="enable"></a>Povolit proxy Azure Functions

Proxy nejsou povolené ve výchozím nastavení. Můžete vytvořit proxy hello funkce je zakázaná, ale spustit. proxy tooenable hello následující:

1. Otevřete hello [portál Azure], a potom přejděte tooyour funkce aplikace.
2. Vyberte **funkce nastavení aplikace**.
3. Přepínač **povolit funkce proxy Azure (preview)** příliš**na**.

Můžete se taky vrátit zde tooupdate hello proxy runtime nové funkce dostupná.


## <a name="create"></a>Vytvořit proxy

Tato část uvádí, jak toocreate a proxy serveru v hello Functions na portálu.

1. Otevřete hello [portál Azure], a potom přejděte tooyour funkce aplikace.
2. V levém podokně hello vyberte **nového proxy**.
3. Zadejte název pro proxy.
4. Konfigurace hello koncový bod, který je vystaven na tuto aplikaci funkce zadáním hello **šablonu trasy** a **metody HTTP**. Tyto parametry chovat podle pravidla toohello pro [aktivace protokolu HTTP].
5. Sada hello **URL back-end** tooanother koncový bod. Tento koncový bod může být funkce v jiné aplikaci funkce, nebo to může být ostatní rozhraní API. Hello hodnotu, nebude potřebovat toobe statické, a může obsahovat odkazy na [nastavení aplikace] a [parametry z původního požadavku klienta hello].
6. Klikněte na možnost **Vytvořit**.

Proxy nyní existuje jako nový koncový bod v aplikaci funkce. Z hlediska klienta je ekvivalentní tooan HttpTrigger v Azure Functions. Váš nový proxy server můžete vyzkoušet tak, že hello adresa URL proxy serveru a testování pomocí vašeho oblíbeného klienta HTTP.

## <a name="modify-requests-responses"></a>Upravit požadavky a odpovědi

S funkcí proxy Azure můžete upravit požadavky tooand odpovědí z back-end hello. Tyto transformace můžete používat proměnné, jak jsou definovány v [použijte proměnné].

### <a name="modify-backend-request"></a>Upravit požadavek back-end hello

Ve výchozím nastavení je požadavek back-end hello inicializován jako kopii hello původní žádost. Kromě toho toosetting hello back-end adresy URL, můžete provést změny metoda HTTP toohello, hlaviček a parametrů řetězce dotazu. Hello změny hodnot můžete odkazovat [nastavení aplikace] a [parametry z původního požadavku klienta hello].

V současné době není k dispozici žádné portálu prostředí pro úpravy požadavky back-end. toolearn jak tooapply tato funkce z proxies.json, najdete v části [definovat objekt requestOverrides].

### <a name="modify-response"></a>Upravit hello odpovědi

Ve výchozím nastavení je odpověď klienta hello inicializováno jako kopii hello back-end odpovědi. Můžete nastavit stavový kód odpovědi toohello změny, frázi důvodu, hlavičky a text. Hello změny hodnot můžete odkazovat [nastavení aplikace], [parametry z původního požadavku klienta hello], a [parametry z back-end odpovědi hello].

V současné době není k dispozici žádné portálu prostředí pro úpravy odpovědi. toolearn jak tooapply tato funkce z proxies.json, najdete v části [definovat objekt responseOverrides].

## <a name="using-variables"></a>Použití proměnných

Hello konfiguraci proxy serveru nemusí toobe statické. Můžete ho podmínky toouse proměnné z původní žádost hello, hello back-end odpovědi nebo nastavení aplikace.

### <a name="request-parameters"></a>Parametry žádosti referenční dokumentace

Parametry žádosti můžete použít jako vstup vlastnost adresa URL back-end toohello nebo jako součást úprava požadavky a odpovědi. Některé parametry mohou být vázány z šablony hello trasy, která je určená v konfiguraci základní proxy hello a ostatní mohou pocházet z vlastnosti hello příchozího požadavku.

#### <a name="route-template-parameters"></a>Parametry šablony trasy
Parametry, které se používají v šabloně trasy hello jsou k dispozici toobe odkazuje jeho názvem. názvy parametrů Hello jsou uzavřené do složených závorek ({}).

Pokud proxy server má šablonu trasy, jako například `/pets/{petId}`, adresa URL back-end hello může obsahovat hodnotu hello `{petId}`jako v `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Pokud šablonu trasy hello ukončí v zástupný znak, jako například `/api/{*restOfPath}`, hello hodnota `{restOfPath}` je řetězcová reprezentace hello zbývající segmenty cesty z příchozího požadavku hello.

#### <a name="additional-request-parameters"></a>Parametry další žádosti
Kromě toho toohello směrovat parametry šablony, hello následující hodnoty mohou být používány hodnoty konfigurace:

* **{request.method}** : hello metoda HTTP, který se používá na původní žádost hello.
* **{request.headers. \<HeaderName\>}**: hlavičku, který může číst z původního požadavku hello. Nahraďte  *\<HeaderName\>*  s názvem hello hello hlavičky, které chcete tooread. Pokud hlavička hello není k dispozici v požadavku hello, bude hodnota hello hello prázdný řetězec.
* **{request.querystring. \<ParameterName\>}**: parametr řetězce dotazu, který může číst z původního požadavku hello. Nahraďte  *\<ParameterName\>*  s názvem hello hello parametru, které chcete tooread. Pokud parametr hello není k dispozici v požadavku hello, bude hodnota hello hello prázdný řetězec.

### <a name="response-parameters"></a>Odkaz na back-end odpovědi parametry

Odpověď parametry můžete použít jako součást úprava hello odpovědi toohello klienta. Hello následující hodnoty lze použít v hodnotách konfigurace:

* **{backend.response.statusCode}** : hello stavový kód protokolu HTTP, která je vrácena v odpovědi hello back-end.
* **{backend.response.statusReason}** : frázi důvodu hello HTTP, která je vrácena v odpovědi hello back-end.
* **{backend.response.headers. \<HeaderName\>}**: hlavičku, který může číst z odpovědi hello back-end. Nahraďte  *\<HeaderName\>*  s názvem hello hello hlavičky chcete tooread. Pokud hlavička hello není k dispozici v požadavku hello, bude hodnota hello hello prázdný řetězec.

### <a name="use-appsettings"></a>Odkaz nastavení aplikace

Můžete taky odkazovat [nastavení aplikace, které jsou definovány pro funkce aplikace hello](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) tím, že název nastavení hello znaky procenta (%).

Například adresu URL back-end z *https://%ORDER_PROCESSING_HOST%/api/orders* by měla mít "% ORDER_PROCESSING_HOST %" nahrazen hodnotou hello hello ORDER_PROCESSING_HOST nastavení.

> [!TIP] 
> Použít nastavení aplikace pro hostitele back-end, když máte více nasazení nebo testovací prostředí. Tímto způsobem, abyste měli jistotu, že vždy mluvení toohello přímo zpět ukončení pro prostředí.

## <a name="advanced-configuration"></a>Pokročilá konfigurace

Hello proxy servery, které nakonfigurujete jsou uloženy v souboru proxies.json, který se nachází v hello kořenového adresáře aplikace funkce. Můžete ručně upravit tento soubor a nasadit jako součást aplikace, když použijete některou z hello [metody nasazení](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) této funkce podporuje. Hello funkce musí být [povoleno](#enable) pro toobe souboru hello zpracovat. 

> [!TIP] 
> Pokud jste nenastavili jednu z metod nasazení hello, můžete také pracovat se soubor proxies.json hello hello portálu. Přejděte tooyour funkce aplikace, vyberte **funkce**a potom vyberte **App Service Editor**. Díky tomu můžete zobrazit celý soubor struktura hello funkce aplikace a proveďte změny.

Proxies.JSON je definován objekt proxy, který se skládá z pojmenované proxy serverů a jejich definice. Případně, pokud vaše editor podporuje, můžete odkazovat [schématu JSON](http://json.schemastore.org/proxies) pro doplňování kódu. Příklad souboru může vypadat hello následující:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

Každý proxy má popisný název, například *proxy1* v předchozím příkladu hello. Hello odpovídající proxy definici objektu je definována hello následující vlastnosti:

* **matchCondition**: požadované – objekt definování hello požadavků, které aktivovat provedení hello tento proxy server. Obsahuje dvě vlastnosti, které jsou sdíleny s [aktivace protokolu HTTP]:
    * _metody_: pole hello HTTP metody, které hello proxy serveru reaguje na. Pokud není zadaný, odpoví hello proxy tooall metody HTTP na hello trasy.
    * _trasy_: požadované – definuje šablonu trasy hello, řízení, které adresy URL žádosti proxy odpoví. Na rozdíl od v aktivační události protokolu HTTP, není žádná výchozí hodnota.
* **backendUri**: adresa URL hello hello back-end prostředků toowhich hello žádosti by měla být směrovány přes proxy server. Tato hodnota může odkazovat nastavení aplikace a parametry z původní požadavek klienta hello. Pokud tuto vlastnost nezadáte, odpoví Azure Functions HTTP 200 OK.
* **requestOverrides**: objekt, který definuje požadavek back-end toohello transformace. V tématu [definovat objekt requestOverrides].
* **responseOverrides**: objekt, který definuje odpověď klienta toohello transformace. V tématu [definovat objekt responseOverrides].

> [!NOTE] 
> Vlastnost Hello trasy proxy funkce Azure nectí hello routePrefix vlastnost konfigurace hostitele funkce hello. Pokud chcete tooinclude předpona například /api, musí být zahrnut ve vlastnosti hello trasy.

### <a name="requestOverrides"></a>Definování objekt requestOverrides

objekt requestOverrides Hello definuje změny provedené toohello žádost, když hello back-end zdroj nebude volán. Hello objektu je definována hello následující vlastnosti:

* **backend.Request.Method**: hello metoda HTTP, který byl použit toocall hello back-end.
* **backend.Request.QueryString. \<ParameterName\>**: parametr řetězce dotazu, který se dá nastavit pro hello volání toohello back-end. Nahraďte  *\<ParameterName\>*  s názvem hello hello parametru, které chcete tooset. Pokud je zadaný hello prázdný řetězec, není parametr hello součástí hello požadavek back-end.
* **backend.Request.Headers. \<HeaderName\>**: hlavičku, která se dá nastavit pro hello volání toohello back-end. Nahraďte  *\<HeaderName\>*  s názvem hello hello hlavičky, které chcete tooset. Pokud zadáte hello prázdný řetězec, není k dispozici hello hlavičky v požadavku hello back-end.

Hodnoty, můžete odkazovat z původního požadavku klienta hello parametry a nastavení aplikace.

Příklad konfigurace může vypadat hello následující:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <a name="responseOverrides"></a>Definování objekt responseOverrides

objekt requestOverrides Hello definuje změny, které jsou vytvářeny toohello odpověď, který je předán zpět toohello klienta. Hello objektu je definována hello následující vlastnosti:

* **response.statusCode**: toobe kód stavu hello HTTP vrátil toohello klienta.
* **response.statusReason**: toobe frázi důvodu hello HTTP vrácená toohello klienta.
* **Response.body**: hello řetězcovou reprezentaci toobe textu hello vrátil toohello klienta.
* **Response.Headers. \<HeaderName\>**: hlavičku, která se dá nastavit pro hello odpovědi toohello klienta. Nahraďte  *\<HeaderName\>*  s názvem hello hello hlavičky, které chcete tooset. Pokud zadáte hello prázdný řetězec, není součástí hello hlavičky odpovědi hello.

Hodnoty můžete odkazovat aplikace nastavení, parametry z původního požadavku klienta hello a parametry z odpovědi hello back-end.

Příklad konfigurace může vypadat hello následující:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> V tomto příkladu textu hello se nastavuje přímo, takže ne `backendUri` vlastnost je vyžadována. Hello příklad ukazuje, jak je možné použít Azure funkce proxy pro mocking rozhraní API.

[portál Azure]: https://portal.azure.com
[aktivace protokolu HTTP]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modify hello back-end request]: #modify-backend-request
[Modify hello response]: #modify-response
[definovat objekt requestOverrides]: #requestOverrides
[definovat objekt responseOverrides]: #responseOverrides
[nastavení aplikace]: #use-appsettings
[použijte proměnné]: #using-variables
[parametry z původního požadavku klienta hello]: #request-parameters
[parametry z back-end odpovědi hello]: #response-parameters
