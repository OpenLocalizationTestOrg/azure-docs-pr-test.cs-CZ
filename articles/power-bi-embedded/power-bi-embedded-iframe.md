---
title: "Jak používat Power BI Embedded se zbytkem | Microsoft Docs"
description: "Naučte se používat se zbytkem Power BI Embedded "
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 8bcef780-cca0-4f30-9a9b-9daa1a7ae865
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 02/06/2017
ms.author: asaxton
ms.openlocfilehash: 31624b9d15772a4f08cf013ac713b3aa636acfca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-power-bi-embedded-with-rest"></a>Jak používat Power BI Embedded službou REST

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI Embedded: Co je a co je pro

Přehled Power BI Embedded je popsán v oficiálním [Power BI Embedded lokality](https://azure.microsoft.com/services/power-bi-embedded/), ale podívejme se rychlé předtím, než se nám získat na podrobné informace o používání s REST.

Je to úplně jednoduché, skutečně. můžete chtít použít vizualizace dynamická data [Power BI](https://powerbi.microsoft.com) ve vaší vlastní aplikaci.

Většina vlastní aplikace potřebují k poskytování dat pro vlastní zákazníky a nemusí uživatelé ve vlastní organizaci. Například pokud jste doručování některé služby pro firmy A a B společnosti, uživatelům ve společnosti A by měl viděli pouze data pro své vlastní společnosti A. To znamená je potřeba více klientů pro doručení.

Vlastní aplikace může také nabízí vlastní metody ověřování, jako je například ověřování formulářů, základní ověřování, atd... Potom vnoření řešení musí spolupracovat s tento existující metody ověřování bezpečně. Je také nutné uživatelům umožnit používat tyto aplikace ISV bez další nákupu nebo licencování předplatného Power BI.

 **Power BI Embedded** je určená pro přesněji tyto druhy scénářů. Ano teď, když máme tento rychlý úvod stranou, se můžeme dát do některé podrobnosti

Můžete použít .NET \(C#) nebo Node.js SDK, snadno vytvářet aplikace s Power BI Embedded. Ale v tomto článku vám objasníme o toku HTTP \(včetně ověřovacího) z Power BI bez sady SDK. Principy tento tok, můžete vytvořit aplikaci **s žádný programovací jazyk**, a porozumíte hluboko podstatu Power BI Embedded.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Kolekce pracovních prostorů vytvořit Power BI a přístupový klíč get \(zřizování)

Power BI Embedded je jedním ze služby Azure. Pouze ISV, který používá portál Azure je účtovat poplatky za použití \(za každou hodinu uživatelské relace), a uživatel, který si prohlíží sestavy není účtován nebo dokonce předplatné Azure.
Před zahájením náš vývoj aplikací, jsme musí vytvořit **kolekce pracovních prostorů Power BI** pomocí portálu Azure.

Každý prostoru Power BI Embedded je pracovním prostoru pro každý zákazníků (klientů), a přidáme mnoho pracovní prostory v každé kolekci pracovních prostorů. Stejné přístupový klíč se používá v každé kolekci pracovních prostorů. V platit, kolekce pracovních prostorů je hranicí zabezpečení pro Power BI Embedded.

![](media/power-bi-embedded-iframe/create-workspace.png)

Když jsme vytvoření kolekce pracovních prostorů zkopírujte přístupový klíč z portálu Azure.

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> Také můžeme zřídit kolekce pracovních prostorů a získat přístupový klíč přes REST API. Další informace najdete v tématu [rozhraní API pro Power BI prostředků zprostředkovatele](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Vytvoření souboru .pbix s Power BI Desktop

V dalším kroku jsme musíte vytvořit datové připojení a sestavy, které má být vložen.
Pro tuto úlohu neexistuje žádné programování nebo kódu. Právě jsme pomocí Power BI Desktop.
V tomto článku jsme nebude projít informace o tom, jak používat Power BI Desktop. Pokud budete potřebovat některé zde, přečtěte [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). Pro náš příklad právě použijeme [prodejní analýzy ukázka](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Vytvořit pracovní prostor Power BI

Teď, když zajišťování vše na jednom, můžeme začít vytváření pracovního prostoru zákazníka v kolekci pracovních prostorů přes rozhraní REST API. Následující HTTP POST požadavku (REST) vytváří nový pracovní prostor v našem existující kolekci pracovních prostorů. Toto je [POST prostoru API](https://msdn.microsoft.com/library/azure/mt711503.aspx). V našem příkladu je název kolekce prostoru **mypbiapp**. Právě nastavujeme přístupový klíč, které jsme dříve zkopírovat, jako **AppKey**. Je velmi jednoduché ověřování!

**Požadavek HTTP**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**Odpověď HTTP**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Vrácený **workspaceId** se používá pro následující následných volání rozhraní API. Aplikace musí zachovat tuto hodnotu.

## <a name="import-pbix-file-into-the-workspace"></a>Import souboru .pbix do pracovního prostoru

Každé sestavy v pracovním prostoru odpovídá do jednoho souboru Power BI Desktop s datovou sadu \(včetně nastavení zdroje dat). Naše souboru .pbix jsme můžete importovat do pracovního prostoru, jak je znázorněno v následujícím kódu. Jak vidíte, jsme nahrát binárního souboru .pbix souboru MIME multipart pomocí protokolu HTTP.

Identifikátor uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** je ID pracovního prostoru a parametr dotazu **datasetDisplayName** je název datové sady, které chcete vytvořit. Vytvořenou datovou sadu obsahuje všechna data související s artefakty v .pbix souboru, například jako importovaných dat, má ukazatel na zdroji dat atd...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Může nějakou dobu spuštění této úlohy importu. Po dokončení můžete pokládat naší aplikaci pomocí id importu stav úlohy. V našem příkladu je id importu **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Toto je žádostí stavu pomocí tohoto id importu:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Pokud úloha není kompletní, může být odpověď HTTP takto:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Pokud úloha skončí, může být odpověď HTTP další takto:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Připojení zdroje dat \(a víceklientský dat)

Při téměř všechny artefaktů v souboru .pbix jsou importovány do našich pracovního prostoru, přihlašovací údaje pro zdroje dat nejsou. V důsledku toho, při použití **režimu DirectQuery**, vloženou sestavu nelze zobrazit správně. Ale při použití **režimu Import**, jsme můžete zobrazit sestavy pomocí stávající naimportovaná data. V takovém případě jsme musí nastavit přihlašovací údaje, pomocí následujících kroků prostřednictvím volání REST.

Nejprve se nám musí získat zdroj dat brány. Víme, datová sada **id** je dříve vrácený id.

**Požadavek HTTP**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**Odpověď HTTP**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Pomocí id vrácený brány a id zdroje dat \(najdete v předchozí **gatewayId** a **id** v vrácených výsledků), jsme následujícím způsobem změnit pověření pro tohoto zdroje dat:

**Požadavek HTTP**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**Odpověď HTTP**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

V produkčním prostředí jsme také můžete nastavit různé připojovací řetězec pro každý pracovní prostor pomocí rozhraní REST API. \(tj, jsme lze jednotlivé databáze pro každý zákazníky.)

Následující mění řetězec připojení zdroje dat přes REST.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Používáme zabezpečení na úrovni řádků v Power BI Embedded a jsme můžete oddělit data pro každého uživatele v jednu sestavu. As a result, jsme můžete zřídit každou zprávu zákazníků s stejné .pbix \(uživatelského rozhraní, atd.) a různé datové zdroje.

> [!NOTE]
> Pokud používáte **režimu Import** místo **režimu DirectQuery**, neexistuje žádný způsob, jak aktualizovat modely prostřednictvím rozhraní API. A v Power BI Embedded ještě není podporovaný místní zdroje dat prostřednictvím brány Power BI. Však budete Opravdu chcete dohlížet na [Power BI blog](https://powerbi.microsoft.com/blog/) pro co je nového a co Připravujeme v budoucnosti uvolní.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Ověřování a hostování (vnoření) sestavách v naší webové stránky

V předchozích REST API, můžeme použít přístupový klíč **AppKey** sám sebe jako hlavičku autorizace. Protože tyto volání může být zpracována na straně back-end serveru, je bezpečné.

Ale po tuto sestavu vložit naše webové stránce, tento druh informací o zabezpečení by být zpracována pomocí jazyka JavaScript \(front-endu). Pak musí být zabezpečená hodnota hlavičky ověřování. Pokud uživatel se zlými úmysly nebo škodlivý kód je zjištěno naše přístupový klíč, můžou volat jakékoli operace pomocí tohoto klíče.

Pokud tuto sestavu vložit naše webové stránce, jsme místo přístupový klíč musíte použít počítaný token **AppKey**. Aplikace musí vytvořit OAuth webových tokenů Json \(JWT) který se skládá z deklarací identity a počítaný digitální podpis. Jak je uvedeno níže, je tento token JWT OAuth tokeny řetězec s kódováním oddělené tečkou.

![](media/power-bi-embedded-iframe/oauth-jwt.png)

Nejdřív jsme musíte připravit vstupní hodnotu, která je podepsána později. Tato hodnota je řetězec ve formátu base64 kódovaná adresou url (rfc4648) následujícím kódu json, a ty jsou odděleny tečkou \(.) znaků. Později vysvětlíme, jak získat id sestavy.

> [!NOTE]
> Pokud nám chcete používat zabezpečení na úrovni řádků (RLS) s nástrojem Power BI Embedded, jsme musíte zadat také **uživatelské jméno** a **role** v deklaracích.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Dál musíte vytvořit řetězec s kódováním base64 z HMAC \(podpis) s algoritmem SHA256. Tato podepsaný vstupní hodnota je předchozích řetězec.

Poslední, jsme musíte kombinovat vstupní řetězec na hodnotu a podpis pomocí období \(.) znaků. Dokončené řetězec je tokenu aplikace pro vložení sestavy. I v případě, že uživatel se zlými úmysly zjišťováno tokenu aplikace, nemají přístup původní přístupový klíč. Tento token aplikace rychle vyprší.

Tady je příklad PHP pro tyto kroky:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Nakonec tuto sestavu vložte do webové stránky

Pro vkládání naše sestavy, nám musí získat adresu url vložení a sestava **id** pomocí následující rozhraní REST API.

**Požadavek HTTP**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**Odpověď HTTP**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

V našem webovou aplikaci pomocí předchozího tokenu aplikace jsme můžete tuto sestavu vložit.
Pokud se podíváme na Další ukázkový kód, dřív používaná část je stejný jako předchozí příklad. V pozdější části, tento příklad ukazuje **embedUrl** \(viz předchozí výsledek) v elementu iframe a je publikování tokenu aplikace do iframe.

> [!NOTE]
> Budete muset změnit hodnotu id sestavy na vlastním. Navíc způsobena chybou v našem systému správy obsahu, značku iframe v ukázce kódu je pro čtení oznámena. Odeberte vyloučením nejvyššího a nejnižšího text ze značky, pokud je kopírujete a vkládáte ukázkový kód.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

A tady je naše výsledek:

![](media/power-bi-embedded-iframe/view-report.png)

V tomto okamžiku Power BI Embedded pouze zobrazí sestavu v iframe. Ale dohlížet na [Power BI Blog](https://powerbi.microsoft.com/blog/). Budoucí vylepšení použít nové na straně klienta rozhraní API, která bude dejte nám odeslat informace do iframe a také získat informace o. Zajímavé věcem!

## <a name="see-also"></a>Viz také
* [Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)

Chcete se ještě na něco zeptat? [Vyzkoušejte komunitu Power BI](http://community.powerbi.com/)

