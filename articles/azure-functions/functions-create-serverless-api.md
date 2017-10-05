---
title: "Vytvoření bez serveru API pomocí Azure Functions | Microsoft Docs"
description: "Postup vytvoření bez serveru API pomocí Azure Functions"
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
ms.openlocfilehash: 28056a385b058f7daeca2253ccb304116b49eba0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a>Vytvoření bez serveru API pomocí Azure Functions

V tomto kurzu se dozvíte, jak Azure Functions umožňuje vytvářet škálovatelné rozhraní API. Azure Functions se dodává s kolekce integrované HTTP triggerů a vazeb, které usnadňují vytváření koncový bod v různých jazycích, včetně Node.JS, C# a další. V tomto kurzu budete přizpůsobovat aktivační procedury HTTP pro zpracování určitých akcí v návrhu rozhraní API. Také připravíte pro rostoucí vaše rozhraní API pomocí integrace s funkcí proxy Azure a nastavením imitované rozhraní API. Všechny tyto dosahuje nad výpočetním prostředí bez serveru funkce, takže nemusíte si dělat starosti o škálování prostředky – stačí se můžete soustředit na logice rozhraní API.

## <a name="prerequisites"></a>Požadavky 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

Výsledný funkce se použije pro zbytek tohoto kurzu.

### <a name="sign-in-to-azure"></a>Přihlášení k Azure

Otevřete portál Azure. Chcete-li to provést, přihlaste se k [https://portal.azure.com](https://portal.azure.com) s vaším účtem Azure.

## <a name="customize-your-http-function"></a>Přizpůsobení funkce protokolu HTTP

Ve výchozím nastavení je funkce aktivované protokolem HTTP nakonfigurovaná tak, aby přijímal libovolné metody HTTP. Je také výchozí adresy URL ve formátu `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`. Pokud jste postupovali podle rychlé spuštění, pak `<funcname>` pravděpodobně vypadá podobně jako "HttpTriggerJS1". V této části upravíte funkce odpovídal pouze na požadavky GET proti `/api/hello` směrovat místo. 

Přejděte do funkce na portálu Azure. Vyberte **integrací** v levém navigačním panelu.

![Přizpůsobení funkce protokolu HTTP](./media/functions-create-serverless-api/customizing-http.png)

Použijte nastavení aktivační událost HTTP jako zadaný v tabulce.

| Pole | Ukázková hodnota | Popis |
|---|---|---|
| Povolených metod HTTP | Vybrané metody | Určuje, jaké metody HTTP lze volat tuto funkci |
| Vybrané metody HTTP | GET | Umožňuje pouze vybrané HTTP metod, které bude použito ke volat tuto funkci |
| Šablona trasy | řetězec Ahoj | Určuje, jaké trasy je použito k volání této funkce |

Všimněte si, že nejsou zahrnuty `/api` základní cesta předpony v šabloně trasy, jak to se provádí globální nastavení.

Klikněte na **Uložit**.

Další informace o přizpůsobení funkce protokolu HTTP v [vazby HTTP funkce Azure a webhooku](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).

### <a name="test-your-api"></a>Test rozhraní API

V dalším kroku funkci nevidíte práce s novou plochy rozhraní API otestujte.

Přejděte zpět na stránku vývoj kliknutím na název funkce je v levém navigačním panelu.

Klikněte na tlačítko **získat adresu URL funkce** a zkopírujte adresu URL. Měli byste vidět, že používá `/api/hello` nyní trasy.

Zkopírujte adresu URL do novou kartu prohlížeče nebo vaše upřednostňované klienta REST. V prohlížečích se ve výchozím nastavení používá GET.

Spuštění funkce a potvrďte, že funguje. Musíte zadat parametr "název" jako řetězec dotazu tak, aby pokryl kód rychlý start.

Můžete také zkusit volání koncový bod s jinou metodu HTTP, potvrďte, že funkce není spuštěn. V takovém případě budete muset použít klienta REST, například cURL, Postman nebo Fiddler.

## <a name="proxies-overview"></a>Přehled proxy

V další části bude surface vaše rozhraní API prostřednictvím proxy serveru. Azure proxy funkce je funkce preview, která umožňuje předat požadavky na jiné prostředky. Můžete definovat koncový bod HTTP stejně jako s triggeru protokolu HTTP, ale místo psaní kódu pro spuštění, kdy se nazývá tohoto koncového bodu, zadejte adresu URL vzdálené implementaci. Umožňuje vytvořit více zdrojů rozhraní API do jednoho plochy rozhraní API, který je snadno klientům využívat. To je zvlášť užitečné, pokud chcete vytvořit rozhraní API jako mikroslužeb.

Proxy server může ukazovat na jakémukoli prostředku, HTTP, jako například:
- Funkce Azure 
- API apps v [služby Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- Kontejnery docker v [služby App Service v systému Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)
- Další hostovaným rozhraním API

Další informace o proxy najdete v tématu [práce s funkcí proxy Azure (preview)].

## <a name="create-your-first-proxy"></a>Vytvoření vašeho prvního proxy

V této části vytvoříte nový proxy, který slouží jako front-end pro celkový rozhraní API. 

### <a name="setting-up-the-frontend-environment"></a>Nastavení prostředí front-endu

Opakováním kroků [vytvořit aplikaci function app](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) k vytvoření nové aplikace funkce, ve kterém vytvoříte proxy. Toto nové aplikace bude sloužit jako front-endu pro naše API a funkce aplikace, kterou jste dříve upravovali bude sloužit jako back-end.

Přejděte do nové aplikace funkce front-endu na portálu.

Vyberte **nastavení**. Potom přepněte **povolit funkce proxy Azure (preview)** na "On".

Vyberte **nastavení platformy** a zvolte **nastavení aplikace**.

Přejděte dolů k položce **nastavení aplikace** a vytvořit nové nastavení s klíčem "HELLO_HOST". Jeho hodnotu nastavte hostitele back-end funkce aplikace, jako například `<YourApp>.azurewebsites.net`. Toto je část adresy URL, který jste zkopírovali dříve při testování funkce protokolu HTTP. Toto nastavení v konfiguraci později budete odkazovat.

> [!NOTE] 
> Nastavení aplikace – se doporučují pro konfigurace hostitele, aby se zabránilo závislost pevně prostředí pro proxy server. Pomocí nastavení aplikace znamená, že můžete přesunout konfiguraci proxy serveru mezi prostředími a nastavení prostředí aplikací se použijí.

Klikněte na **Uložit**.

### <a name="creating-a-proxy-on-the-frontend"></a>Vytváření proxy server na front-endu

Přejděte zpět do aplikace front-endu funkce na portálu.

V levém navigačním panelu, klikněte na symbol plus '+' vedle "Proxy (preview)".

![Vytváření proxy server](./media/functions-create-serverless-api/creating-proxy.png)

Použijte nastavení proxy serveru specifikovaný v tabulce.

| Pole | Ukázková hodnota | Popis |
|---|---|---|
| Name (Název) | HelloProxy | Popisný název, který se používá pouze pro správu |
| Šablona trasy | / api/hello | Určuje, jaké trasy se použije k vyvolání tento proxy server |
| Adresa URL back-end | https://%HELLO_HOST%/API/Hello | Určuje koncový bod, ke kterému by měla být žádost směrovány přes proxy server |

Všimněte si, že proxy neposkytuje `/api` základní cesta předponu a tento musí být součástí šablonu trasy.

`%HELLO_HOST%` Syntaxe bude odkazovat nastavení aplikace, který jste vytvořili dříve. Přeložit adresy URL bude odkazovat na původní funkce.

Klikněte na možnost **Vytvořit**.

Váš nový proxy server můžete vyzkoušet tak, že adresa URL proxy serveru a jeho otestováním v prohlížeči nebo pomocí vašeho oblíbeného klienta HTTP.

## <a name="create-a-mock-api"></a>Vytvoření imitované rozhraní API

V dalším kroku použijete proxy serveru k vytvoření imitované rozhraní API pro vaše řešení. To umožňuje vývoje klienta pokroku, bez nutnosti back-end plně implementováno. Později v vývoj může vytvořit novou aplikaci funkce, která podporuje tuto logiku a přesměrování proxy k němu.

Pokud chcete vytvořit toto imitované rozhraní API, vytvoříme nový proxy, toto současně pomocí [Editor služby aplikace](https://github.com/projectkudu/kudu/wiki/App-Service-Editor). Chcete-li začít, přejděte na funkce aplikace v portálu. Vyberte **funkce** a najít **App Service Editor**. To kliknutím otevřete Editor služby aplikace na nové kartě.

Vyberte `proxies.json` v levém navigačním panelu. Toto je soubor, který ukládá konfiguraci pro všechny vaše servery proxy. Pokud použijete jednu z [funkce metody nasazení](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), toto je soubor bude uchovávat ve správě zdrojového kódu. Další informace o tento soubor naleznete v tématu [proxy Upřesnit konfiguraci](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).

Pokud jste jste postupovali podle dosavadní, vaše proxies.json by měl vypadat asi takto:

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

Dále přidáte imitované rozhraní API. Nahraďte souboru proxies.json s následujícími službami:

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

Tento postup přidá nový proxy, "GetUserByName", bez backendUri vlastnosti. Namísto volání jiný prostředek, upraví odpověď výchozí proxy pomocí přepsání odpovědi. Přepsání požadavku a odpovědi mohou sloužit také ve spojení s adresou URL back-end. To je zvlášť užitečné, když připojení přes proxy server na starší systém, kde budete muset upravit hlavičky, dotaz parametry atd. Další informace o požadavku a odpovědi přepsání najdete v tématu [úprava požadavky a odpovědi v proxy](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).

Testovat imitované rozhraní API voláním `/api/users/{username}` koncového bodu pomocí prohlížeče nebo vaše oblíbené klienta REST. Nezapomeňte nahradit _{username}_ s řetězcovou hodnotu představující uživatelské jméno.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zjistili, jak vytvářet a přizpůsobit rozhraní API na Azure Functions. Také jste zjistili, jak probouzet několik rozhraní API, včetně mocks, společně jako jednotné rozhraní API prostor. Tyto postupy můžete použít k sestavení se rozhraní API jakékoli složitosti při spuštění v modelu bez serveru výpočetní poskytované Azure Functions.

Následující odkazy mohou být užitečné, když budete vyvíjet další rozhraní API:

- [Azure funkce protokolu HTTP a webhooku vazby](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- [práce s funkcí proxy Azure (preview)]
- [Dokumentace rozhraní API funkce Azure (preview)](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[práce s funkcí proxy Azure (preview)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies
