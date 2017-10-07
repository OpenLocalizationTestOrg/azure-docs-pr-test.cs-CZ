---
title: "aaaCreate bez serveru API pomocí Azure Functions | Microsoft Docs"
description: "Jak toocreate bez serveru API pomocí Azure Functions"
services: functions
author: mattchenderson
manager: erikre
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: mahender
ms.custom: mvc
ms.openlocfilehash: 877e3b229d5477fc5fec594ccd284fb55d7f3c07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a>Vytvoření bez serveru API pomocí Azure Functions

V tomto kurzu se dozvíte, jak Azure Functions umožňuje toobuild vysoce škálovatelné rozhraní API. Azure Functions se dodává s kolekce integrované HTTP triggerů a vazeb, které umožňují snadno tooauthor koncový bod v různých jazycích, včetně Node.JS, C# a další. V tomto kurzu budete přizpůsobovat toohandle aktivace protokolu HTTP pro konkrétní akce při návrhu rozhraní API. Také připravíte pro rostoucí vaše rozhraní API pomocí integrace s funkcí proxy Azure a nastavením imitované rozhraní API. Všechny tyto dosahuje nad hello funkce bez serveru výpočetním prostředí, takže nemáte tooworry o škálování prostředky – stačí se můžete soustředit na logice rozhraní API.

## <a name="prerequisites"></a>Požadavky 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

Funkce výsledná Hello se použije pro hello zbytek tohoto kurzu.

### <a name="sign-in-tooazure"></a>Přihlaste se tooAzure

Otevřete hello portálu Azure. toodo, přihlaste se příliš[https://portal.azure.com](https://portal.azure.com) s vaším účtem Azure.

## <a name="customize-your-http-function"></a>Přizpůsobení funkce protokolu HTTP

Ve výchozím nastavení, je funkce aktivované protokolem HTTP tooaccept nakonfigurovaný libovolné metody HTTP. Je také výchozí adresa URL hello formátu `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`. Pokud jste postupovali podle hello rychlý start, pak `<funcname>` pravděpodobně vypadá podobně jako "HttpTriggerJS1". V této části upravíte hello funkce toorespond pouze tooGET požadavky DHCP proti `/api/hello` směrovat místo. 

Přejděte tooyour funkce v hello portálu Azure. Vyberte **integrací** v levé navigační hello.

![Přizpůsobení funkce protokolu HTTP](./media/functions-create-serverless-api/customizing-http.png)

Použijte nastavení aktivační událost HTTP jako tabulka hello.

| Pole | Ukázková hodnota | Popis |
|---|---|---|
| Povolených metod HTTP | Vybrané metody | Určuje, jaké metody HTTP, může být použité tooinvoke této funkce |
| Vybrané metody HTTP | GET | Umožňuje pouze vybrané toobe metody HTTP použili tooinvoke tuto funkci |
| Šablona trasy | řetězec Ahoj | Určuje, jaké trasy je použité tooinvoke této funkce |

Všimněte si, že jste nezahrnuli hello `/api` základní cesta předponu hello šablonu trasy, jak to se provádí globální nastavení.

Klikněte na **Uložit**.

Další informace o přizpůsobení funkce protokolu HTTP v [vazby HTTP funkce Azure a webhooku](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).

### <a name="test-your-api"></a>Test rozhraní API

V dalším kroku otestujte váš funkce toosee práce s hello nový prostor pro rozhraní API.

Přejděte zpět toohello vývoj stránku kliknutím na název funkce hello v levé navigační hello.

Klikněte na tlačítko **získat adresu URL funkce** a zkopírujte adresu URL hello. Měli byste vidět, že používá hello `/api/hello` nyní trasy.

Zkopírujte adresu URL hello do novou kartu prohlížeče nebo vaše upřednostňované klienta REST. V prohlížečích se ve výchozím nastavení používá GET.

Spusťte hello funkce a zkontrolujte, že funguje. Může být nutné tooprovide hello "název" parametr jako kód rychlý start hello toosatisfy řetězec dotazu.

Můžete také zkusit volání hello koncový bod s jinou tooconfirm metoda HTTP že hello funkce není spuštěn. V takovém případě budete potřebovat toouse klienta REST, například cURL, Postman nebo Fiddler.

## <a name="proxies-overview"></a>Přehled proxy

V další části hello bude surface vaše rozhraní API prostřednictvím proxy serveru. Azure proxy funkce je funkce preview, která vám umožní tooforward požadavky tooother prostředky. Můžete definovat koncový bod protokolu HTTP, podobně jako pomocí triggeru protokolu HTTP, ale místo psaní kódu tooexecute při volání tohoto koncového bodu, zadáte vzdálené implementaci tooa adresy URL. To vám umožní toocompose zdroje několik rozhraní API do jednoho plochy rozhraní API, který je snadno tooconsume klientů. To je zvlášť užitečné, pokud chcete toobuild vaše rozhraní API jako mikroslužeb.

Proxy server můžete bodu tooany HTTP prostředků, například:
- Funkce Azure 
- API apps v [služby Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- Kontejnery docker v [služby App Service v systému Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)
- Další hostovaným rozhraním API

toolearn Další informace o proxy, najdete v části [práce s funkcí proxy Azure (preview)].

## <a name="create-your-first-proxy"></a>Vytvoření vašeho prvního proxy

V této části vytvoříte nový proxy které slouží jako front-endu tooyour celkové rozhraní API. 

### <a name="setting-up-hello-frontend-environment"></a>Nastavení prostředí front-endu hello

Opakujte kroky hello příliš[vytvořit aplikaci function app](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) toocreate novou aplikaci funkce v tím se vytvoří proxy. Toto nové aplikace bude sloužit jako hello front-endu pro naše API a hello funkce aplikace, kterou jste dříve upravovali bude sloužit jako back-end.

Přejděte tooyour nové front-endu funkce aplikace hello portálu.

Vyberte **nastavení**. Potom přepněte **povolit funkce proxy Azure (preview)** příliš "na".

Vyberte **nastavení platformy** a zvolte **nastavení aplikace**.

Posuňte se dolů příliš**nastavení aplikace** a vytvořit nové nastavení s klíčem "HELLO_HOST". Nastavte jeho hodnota toohello hostitel back-end funkce aplikace, jako je třeba `<YourApp>.azurewebsites.net`. Toto je část adresy URL hello, který jste zkopírovali dříve při testování funkce protokolu HTTP. Toto nastavení v konfiguraci hello později budete odkazovat.

> [!NOTE] 
> Nastavení aplikace – se doporučují pro tooprevent konfigurace hostitele hello závislost pevně prostředí pro proxy server hello. Pomocí nastavení aplikace znamená, že můžete přesunout hello konfiguraci proxy serveru mezi prostředími a nastavení prostředí aplikací hello se použijí.

Klikněte na **Uložit**.

### <a name="creating-a-proxy-on-hello-frontend"></a>Vytváření proxy server na front-endu hello

Přejděte zpět tooyour front-endu funkce aplikace hello portálu.

V levém navigačním hello, klikněte na tlačítko hello znak plus další '+' příliš "Proxy (preview)".

![Vytváření proxy server](./media/functions-create-serverless-api/creating-proxy.png)

Nastavení proxy serveru použijte jako tabulka hello.

| Pole | Ukázková hodnota | Popis |
|---|---|---|
| Name (Název) | HelloProxy | Popisný název, který se používá pouze pro správu |
| Šablona trasy | / api/hello | Určuje, jaké trasy je použité tooinvoke tento proxy server |
| Adresa URL back-end | https://%HELLO_HOST%/API/Hello | Určuje, že požadavku hello toowhich hello koncového bodu musí být směrovány přes proxy server |

Všimněte si, že proxy nenabízí hello `/api` základní cesta předponu a tento musí být součástí hello šablonu trasy.

Hello `%HELLO_HOST%` syntaxe bude odkazovat nastavení aplikace hello jste vytvořili dříve. Hello rozpoznat, že adresa URL bude odkazovat tooyour původní funkce.

Klikněte na možnost **Vytvořit**.

Můžete vyzkoušet nový proxy zkopírováním hello adresa URL proxy serveru a jeho otestováním v prohlížeči hello nebo pomocí vašeho oblíbeného klienta HTTP.

## <a name="create-a-mock-api"></a>Vytvoření imitované rozhraní API

V dalším kroku použijete proxy toocreate imitované rozhraní API pro vaše řešení. To umožňuje vývoje klienta tooprogress, bez nutnosti plně implementované back-end hello. Později v vývoj může vytvořit novou aplikaci funkce, která podporuje tuto logiku a přesměrování tooit vašeho proxy serveru.

toocreate to model rozhraní API, vytvoříme nový proxy, tentokrát pomocí hello [App Service Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor). tooget začít, přejděte tooyour funkce aplikace hello portálu. Vyberte **funkce** a najít **App Service Editor**. To kliknutím otevřete hello Editor služby aplikace na nové kartě.

Vyberte `proxies.json` v levé navigační hello. Toto je hello souboru, který ukládá hello konfiguraci pro všechny vaše servery proxy. Pokud použijete jeden hello [funkce metody nasazení](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), toto je soubor hello bude uchovávat ve správě zdrojového kódu. najdete v části Další informace o tento soubor toolearn [proxy Upřesnit konfiguraci](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).

Pokud jste jste postupovali podle dosavadní, vaše proxies.json by měl vypadat jako následující hello:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        }
    }
}
```

Dále přidáte imitované rozhraní API. Váš soubor proxies.json nahraďte hello následující:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        },
        "GetUserByName" : {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/users/{username}"
            },
            "responseOverrides": {
                "response.statusCode": "200",
                "response.headers.Content-Type" : "application/json",
                "response.body": {
                    "name": "{username}",
                    "description": "Awesome developer and master of serverless APIs",
                    "skills": [
                        "Serverless",
                        "APIs",
                        "Azure",
                        "Cloud"
                    ]
                }
            }
        }
    }
}
```

Tento postup přidá nový proxy, "GetUserByName", bez hello backendUri vlastnosti. Namísto volání jiný prostředek, upraví hello výchozí odpověď od proxy pomocí přepsání odpovědi. Přepsání požadavku a odpovědi mohou sloužit také ve spojení s adresou URL back-end. To je zvlášť užitečné, když se starší verze systému tooa připojení přes proxy server, kde je nutné toomodify hlavičky, parametry dotazu, toolearn atd. Další informace o požadavku a odpovědi přepsání, [úprava požadavky a odpovědi v proxy](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).

Test imitované rozhraní API volání hello `/api/users/{username}` koncového bodu pomocí prohlížeče nebo vaše oblíbené klienta REST. Být jisti tooreplace _{username}_ s řetězcovou hodnotu představující uživatelské jméno.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili jak toobuild a přizpůsobení rozhraní API na Azure Functions. Také jste zjistili, jak toobring několik rozhraní API, včetně mocks, společně jako jednotné rozhraní API prostor. Můžete použít tyto techniky toobuild se rozhraní API jakékoli složitosti, všechny při spuštění v hello bez serveru výpočetní model poskytované Azure Functions.

Hello následující odkazy může být užitečné, když budete vyvíjet další rozhraní API:

- [Azure funkce protokolu HTTP a webhooku vazby](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- [práce s funkcí proxy Azure (preview)]
- [Dokumentace rozhraní API funkce Azure (preview)](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[práce s funkcí proxy Azure (preview)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies
