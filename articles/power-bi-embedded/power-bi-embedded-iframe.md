---
title: Power BI Embedded se zbytkem aaaHow toouse | Microsoft Docs
description: "Zjistěte, jak Power BI Embedded se zbytkem toouse "
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
ms.openlocfilehash: 98057724e60ba868f9c93de8c50383569eb8852d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-power-bi-embedded-with-rest"></a>Jak toouse Power BI Embedded s REST

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI Embedded: Co je a co je pro

Přehled Power BI Embedded je popsaná v oficiální hello [Power BI Embedded lokality](https://azure.microsoft.com/services/power-bi-embedded/), ale podívejme se rychlé předtím, než se nám získat do hello podrobnosti o použití s REST.

Je to úplně jednoduché, skutečně. může být vhodné vizualizace dynamická data hello toouse [Power BI](https://powerbi.microsoft.com) ve vaší vlastní aplikaci.

Většina vlastních aplikací musí toodeliver hello data pro vlastní zákazníky, nikoli nutně uživatele ve vlastní organizaci. Například pokud jste doručování některé služby pro firmy A a B společnosti, uživatelům ve společnosti A by měl viděli pouze data pro své vlastní společnosti A. To znamená je potřeba hello víceklientský pro doručení hello.

vlastní aplikace Hello může také nabízí vlastní metody ověřování, jako je například ověřování formulářů, základní ověřování, atd... Potom hello vložení řešení musí spolupracovat s tento existující metody ověřování bezpečně. Je také nezbytné pro možnost toouse toobe uživatelé tyto aplikace ISV bez hello navíc nákupu nebo licencování předplatného Power BI.

 **Power BI Embedded** je určená pro přesněji tyto druhy scénářů. Ano teď, když máme tento rychlý úvod mimo hello způsobem, se můžeme dát do některé podrobnosti

Můžete použít hello .NET \(C#) nebo Node.js SDK, tooeasily sestavení vaší aplikace pomocí Power BI Embedded. Ale v tomto článku vám objasníme o toku HTTP \(včetně ověřovacího) z Power BI bez sady SDK. Principy tento tok, můžete vytvořit aplikaci **s žádný programovací jazyk**, a porozumíte hluboko hello podstatu Power BI Embedded.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Kolekce pracovních prostorů vytvořit Power BI a přístupový klíč get \(zřizování)

Power BI Embedded je jedním z hello služby Azure. Pouze hello ISV, který používá portál Azure je účtovat poplatky za použití \(za každou hodinu uživatelské relace), a hello uživatele, který sestavu hello zobrazení není účtovat nebo dokonce vyžadovat předplatné Azure.
Před zahájením náš vývoj aplikací, musí vytvoříme hello **kolekce pracovních prostorů Power BI** pomocí portálu Azure.

Každý prostoru Power BI Embedded je hello prostoru pro každý zákazníků (klientů), a přidáme mnoho pracovní prostory v každé kolekci pracovních prostorů. Hello stejné přístupový klíč se používá v každé kolekci pracovních prostorů. V platit, kolekce pracovních prostorů hello je hello hranice zabezpečení pro Power BI Embedded.

![](media/power-bi-embedded-iframe/create-workspace.png)

Když jsme vytvoření kolekce pracovních prostorů hello zkopírujte hello přístupový klíč z portálu Azure.

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> Také můžeme zřídit hello kolekce pracovních prostorů a získat přístupový klíč přes REST API. Další, najdete v části toolearn [rozhraní API pro Power BI prostředků zprostředkovatele](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Vytvoření souboru .pbix s Power BI Desktop

V dalším kroku musí vytvoříme hello datové připojení a vložených toobe sestavy.
Pro tuto úlohu neexistuje žádné programování nebo kódu. Právě jsme pomocí Power BI Desktop.
V tomto článku jsme nebude projít hello podrobnosti o způsobu toouse Power BI Desktop. Pokud budete potřebovat některé zde, přečtěte [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). Pro náš příklad právě použijeme hello [prodejní analýzy ukázka](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Vytvořit pracovní prostor Power BI

Teď, když zřizování hello vše na jednom, můžeme začít vytváření pracovního prostoru zákazníka v kolekci pracovních prostorů hello přes rozhraní REST API. Hello následující HTTP POST požadavku (REST) vytváří nový pracovní prostor hello v našem existující kolekci pracovních prostorů. Toto je hello [POST prostoru API](https://msdn.microsoft.com/library/azure/mt711503.aspx). V našem příkladu se název kolekce prostoru hello **mypbiapp**. Právě nastavujeme hello přístupový klíč, které jsme dříve zkopírovat, jako **AppKey**. Je velmi jednoduché ověřování!

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

Hello vrátil **workspaceId** se používá pro hello následující následných volání rozhraní API. Aplikace musí zachovat tuto hodnotu.

## <a name="import-pbix-file-into-hello-workspace"></a>Import souboru .pbix do pracovního prostoru hello

Každé sestavy v pracovním prostoru odpovídá tooa jednoho souboru Power BI Desktop s datovou sadu \(včetně nastavení zdroje dat). Jsme můžete importovat naše .pbix souboru toohello prostoru jak je znázorněno v následující kód hello. Jak vidíte, jsme nahrát hello binární souboru .pbix MIME multipart pomocí protokolu HTTP.

Hello uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** hello ID pracovního prostoru, a parametr dotazu **datasetDisplayName** je toocreate název datové sady hello. Hello vytvořit datová sada obsahuje všechna data související s artefakty v souboru .pbix, například importovaných dat hello zdroj dat toohello ukazatel atd...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{hello content (binary) of .pbix file}
--A300testx--
```

Může nějakou dobu spuštění této úlohy importu. Po dokončení můžete pokládat naše aplikace hello stav úlohy pomocí id importu. V našem příkladu je id importu hello **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Následující Hello je žádostí stavu pomocí tohoto id importu:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Pokud úloha hello není kompletní, hello odpovědi HTTP může být například takto:

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

Pokud hello úkol je dokončen, může být hello odpověď HTTP další takto:

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

Při téměř všechny hello artefaktů v souboru .pbix jsou importovány do našich pracovního prostoru, hello přihlašovací údaje pro zdroje dat nejsou. V důsledku toho, při použití **režimu DirectQuery**, hello vloženou sestavu nelze zobrazit správně. Ale při použití **režimu Import**, jsme můžete zobrazit sestavy hello pomocí stávající importovaných dat hello. V takovém případě musí nastaví hello pověření pomocí následujících kroků prostřednictvím volání REST hello.

Nejprve se nám musí získat hello gateway datasource. Víme, datová sada hello **id** je hello předtím vrátila id.

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

Pomocí hello vrátil id brány a id zdroje dat \(najdete v části hello předchozí **gatewayId** a **id** v hello vrátí výsledek), jsme následujícím způsobem změnit pověření hello tohoto zdroje dat:

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

V produkčním prostředí jsme můžete také nastavit hello různých připojovací řetězec pro každý pracovní prostor pomocí rozhraní REST API. \(tj, jsme můžete oddělit hello databáze pro každý zákazníky.)

Následující Hello mění hello připojovací řetězec zdroje dat přes REST.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Používáme zabezpečení na úrovni řádků v Power BI Embedded a jsme můžete oddělit hello dat pro každého uživatele v jednu sestavu. As a result, jsme můžete zřídit každou zprávu zákazníků s stejné .pbix \(uživatelského rozhraní, atd.) a různé datové zdroje.

> [!NOTE]
> Pokud používáte **režimu Import** místo **režimu DirectQuery**, neexistuje žádný způsob, jak toorefresh modely prostřednictvím rozhraní API. A v Power BI Embedded ještě není podporovaný místní zdroje dat prostřednictvím brány Power BI. Však budete opravdu chtějí tookeep přehled o hello [Power BI blog](https://powerbi.microsoft.com/blog/) pro co je nového a co Připravujeme v budoucnosti uvolní.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Ověřování a hostování (vnoření) sestavách v naší webové stránky

V hello předchozí REST API, můžeme použít hello přístupový klíč **AppKey** sám sebe jako hello autorizační hlavičky. Protože tyto volání může být zpracována na straně serveru back-end hello, je bezpečné.

Ale po jsme vložení sestavy hello naše webové stránce, tento druh informací o zabezpečení by být zpracována pomocí jazyka JavaScript \(front-endu). Pak musí být zabezpečená hello hodnota hlavičky ověřování. Pokud uživatel se zlými úmysly nebo škodlivý kód je zjištěno naše přístupový klíč, můžou volat jakékoli operace pomocí tohoto klíče.

Když jsme vložení sestavy hello naše webové stránce, musí používáme počítaný token hello místo přístupový klíč **AppKey**. Naše aplikace musíte vytvořit hello OAuth webových tokenů Json \(JWT) který se skládá z hello deklarace identity a hello počítaný digitální podpis. Jak je uvedeno níže, je tento token JWT OAuth tokeny řetězec s kódováním oddělené tečkou.

![](media/power-bi-embedded-iframe/oauth-jwt.png)

Nejdřív jsme musíte připravit hello vstupní hodnoty, který je podepsaný později. Tato hodnota je řetězec kódovaná adresou url (rfc4648) base64 hello hello následující json a ty jsou odděleny tečkou hello \(.) znaků. Později vysvětlíme, jak tooget hello id sestavy.

> [!NOTE]
> Pokud chceme toouse zabezpečení úrovni řádků (RLS) s Power BI Embedded, jsme musíte zadat také **uživatelské jméno** a **role** v deklaracích identity hello.

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

V dalším kroku musí vytvoříme hello řetězec s kódováním base64 z HMAC \(hello podpis) s algoritmem SHA256. Tato podepsaný vstupní hodnota je řetězec předchozí hello.

Poslední, jsme musíte kombinovat hello vstupní hodnoty a řetězec podpis pomocí období \(.) znaků. řetězec Hello dokončit je hello tokenu aplikace pro vložení sestavy hello. I v případě, že uživatel se zlými úmysly zjišťováno tokenu aplikace hello, nemají přístup hello původní přístupový klíč. Tento token aplikace rychle vyprší.

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

// 4. show result (which is hello apptoken)
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

## <a name="finally-embed-hello-report-into-hello-web-page"></a>Nakonec vložení sestavy hello do hello webové stránky

Pro vkládání naše sestavy, nám musí získat hello vložte adresu url a sestava **id** pomocí hello následující rozhraní REST API.

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

V našem webovou aplikaci pomocí tokenu aplikace předchozí hello jsme můžete vložení sestavy hello.
Pokud se podíváme na hello Další ukázkový kód, dřív používaná část hello je hello stejný jako předchozí příklad hello. V pozdější části hello, tento příklad ukazuje hello **embedUrl** \(viz předchozí výsledek hello) v hello iframe a je publikování tokenu aplikace hello do hello iframe.

> [!NOTE]
> Budete potřebovat toochange hello sestavy id hodnota tooone vlastní. Navíc z důvodu chyb tooa v našem systému správy obsahu, značku iframe hello v ukázce kódu hello je pro čtení oznámena. Odeberte textu hello omezená ze hello značky, pokud je kopírujete a vkládáte ukázkový kód.

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

V tomto okamžiku Power BI Embedded pouze zobrazuje hello sestavu v hello iframe. Ale dohlížet na hello [Power BI Blog](https://powerbi.microsoft.com/blog/). Budoucí vylepšení použít nové na straně klienta rozhraní API, která bude dejte nám odeslat informace do hello iframe a také získat informace o. Zajímavé věcem!

## <a name="see-also"></a>Viz také
* [Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)

Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)

