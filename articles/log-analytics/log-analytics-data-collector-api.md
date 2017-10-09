---
title: aaaLog API kolekce dat protokolu HTTP Analytics | Microsoft Docs
description: "Můžete použít hello Log Analytics HTTP dat kolekce API tooadd POST JSON toohello analýzy protokolů úložiště data z libovolného klienta, který můžete volat hello REST API. Tento článek popisuje, jak toouse hello rozhraní API a příklady, jak má toopublish data pomocí různých programovacích jazyků."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: c2921082831c49da764d946ac9c4fab975a38185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-data-toolog-analytics-with-hello-http-data-collector-api"></a>Odeslání dat tooLog Analytics s hello rozhraní API sady kolekcí dat protokolu HTTP
Tento článek ukazuje, jak toouse hello tooLog data toosend Analytics rozhraní API sady kolekcí dat protokolu HTTP od klienta pro REST API.  Se popisuje, jak tooformat data shromažďovaná společností skriptu nebo aplikace, zahrnují v požadavku a mít této žádosti autorizovat analýzy protokolů.  Příklady jsou uvedené pro prostředí PowerShell, C# a Python.

## <a name="concepts"></a>Koncepty
Můžete použít hello rozhraní API sady kolekcí dat protokolu HTTP toosend data tooLog Analytics z libovolného klienta, který můžete volat rozhraní REST API.  To může být sady runbook ve službě Azure Automation, který shromažďuje správy dat z Azure nebo jiný cloud nebo ji mohou být alternativní správy systému, který používá tooconsolidate analýzy protokolů a analyzovat data.

Všechna data v úložišti analýzy protokolů hello uloženo jako záznam s konkrétní typ záznamu.  Vaše data toosend toohello rozhraní API sady kolekcí dat protokolu HTTP je formátovat jako více záznamů ve formátu JSON.  Při odesílání dat hello jednotlivé záznamy se vytvoří v hello úložiště pro každý záznam v hello datová část požadavku.


![Přehled kolekcí dat protokolu HTTP](media/log-analytics-data-collector-api/overview.png)



## <a name="create-a-request"></a>Vytvořit žádost o
toouse hello HTTP kolekcí dat rozhraní API, můžete vytvořit požadavek POST, která obsahuje hello data toosend v JavaScript Object Notation (JSON).  Hello následující tři tabulky seznam hello atributy, které jsou požadovány pro každý požadavek. Jsme popisují každý atribut podrobněji dále v článku hello.

### <a name="request-uri"></a>Identifikátor URI požadavku
| Atribut | Vlastnost |
|:--- |:--- |
| Metoda |POST |
| IDENTIFIKÁTOR URI |https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Typ obsahu |application/json |

### <a name="request-uri-parameters"></a>Parametry identifikátoru URI požadavku
| Parametr | Popis |
|:--- |:--- |
| CustomerID |Hello jedinečný identifikátor pro pracovní prostor hello Microsoft Operations Management Suite. |
| Prostředek |název prostředku Hello rozhraní API: / api/protokoly. |
| Verze rozhraní API |verze Hello toouse hello rozhraní API s touto žádostí. V současné době je 2016-04-01. |

### <a name="request-headers"></a>Hlavičky požadavku
| Záhlaví | Popis |
|:--- |:--- |
| Autorizace |podpis Hello autorizace. Dále v článku hello, si můžete přečíst o tom, záhlaví toocreate HMAC algoritmus SHA256. |
| Typ protokolu |Zadejte typ záznamu hello hello dat, která je odesílána. Typ protokolu hello v současné době podporuje pouze alfanumerické znaky. Nepodporuje se číslice nebo speciální znaky. |
| x-ms datum |Datum Hello zpracování tohoto požadavku hello, v dokumentu RFC 1123 formátu. |
| čas generované pole |Hello název pole v hello data, která obsahuje časové razítko hello položky dat hello. Pokud určíte pole a její obsah se používají pro **TimeGenerated**. Pokud toto pole není určena, výchozí hodnoty pro hello **TimeGenerated** je čas hello této hello je konzumována zprávy. postupujte podle Hello obsah pole zpráv hello hello ISO 8601 formát rrrr-MM-ddTHH. |

## <a name="authorization"></a>Autorizace
Všechny žádosti o toohello Log Analytics HTTP dat kolekce API musí obsahovat hlavičku autorizace. tooauthenticate žádost, musíte se odhlásit hello žádost s hello primární nebo sekundární klíč hello pro hello pracovní prostor, který je vytváření hello požadavku. Pak předejte tento podpis jako součást požadavku hello.   

Tady je hello formát pro hello autorizační hlavičky:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* je hello jedinečný identifikátor pro pracovní prostor služby Operations Management Suite hello. *Podpis* je [Hash-based ověřování kódu metoda HMAC (Message)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , se vytvářejí na základě požadavku hello a pak vypočítaného pomocí hello [algoritmus SHA256](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Potom můžete zakódovat je pomocí kódování Base64.

Použijte tento formát tooencode hello **SharedKey** podpis řetězec:

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

Tady je příklad řetězce podpis:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Pokud máte hello podpis řetězec, zakódovat je pomocí hello algoritmus HMAC s klíčem SHA256 na hello řetězec kódovaný UTF-8 a pak kódování hello výsledek jako Base64. Použijte tento formát:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Ukázky Hello v dalších částech hello mít ukázkový kód toohelp vytvoříte autorizační hlavičky.

## <a name="request-body"></a>Text žádosti
Hello textu hello zprávy musí být ve formátu JSON. Musí obsahovat jeden nebo více záznamů s hello vlastnost páry název-hodnota v tomto formátu:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Více záznamů společně v jedné žádosti může batch pomocí hello formátu. Všechny záznamy hello musí být hello stejný typ záznamu.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Typ záznamu a vlastnosti
Při odesílání dat prostřednictvím hello Log Analytics HTTP dat kolekce API definujete vlastní typ záznamu. V současné době nelze zapisovat data tooexisting typy záznamů, které byly vytvořeny tak, že jiné datové typy a řešení. Analýzy protokolů přečte hello příchozích dat a poté vytvoří vlastnosti, které odpovídají hello datové typy hello hodnoty, které zadáte.

Každý požadavek toohello musí zahrnovat Log Analytics API **typ protokolu** záhlaví s názvem hello pro typ záznamu hello. přípona Hello **_CL** je automaticky připojením toohello název zadejte toodistinguish z jiných protokolu typy jako vlastní protokol. Například pokud zadáte název hello **MyNewRecordType**, analýzy protokolů vytvoří záznam s typem hello **MyNewRecordType_CL**. To pomáhá zajistit, že neexistují žádné konflikty mezi názvy typů vytvořené uživatelem a ty poskytuje současný nebo budoucí řešení společnosti Microsoft.

tooidentify vlastnost datový typ, analýzy protokolů přidává vlastnost toohello příponu. Pokud vlastnost obsahuje hodnotu null, hello vlastnost není součástí záznamů. Tato tabulka uvádí hello vlastnost datový typ a odpovídající příponu:

| Datový typ vlastnosti | Přípona |
|:--- |:--- |
| Řetězec |_M |
| Logická hodnota |_B |
| Double |_d |
| Datum a čas |_T – |
| IDENTIFIKÁTOR GUID |_g |

Hello datový typ, který používá analýzy protokolů pro každou vlastnost závisí na tom, jestli typ záznamu hello nový záznam o hello již existuje.

* Pokud typ záznamu hello neexistuje, analýzy protokolů vytvoří novou. Analýzy protokolů používá hello JSON typ odvození toodetermine hello datový typ pro každou vlastnost nový záznam o hello.
* Pokud typ záznamu hello neexistuje, analýzy protokolů pokusí toocreate nový záznam na základě existující vlastností. Pokud hello datový typ pro vlastnost v novém záznamu hello není odpovídají a nemůže být převedená toohello existující typ nebo pokud hello záznam obsahuje vlastnosti, která neexistuje, vytvoří novou vlastnost analýzy protokolů, který má příponu relevantní hello.

Například by tato položka odeslání vytvořit záznam s třemi vlastnostmi **number_d**, **boolean_b**, a **string_s**:

![Ukázka záznamu 1](media/log-analytics-data-collector-api/record-01.png)

Pokud pak odeslání této další položky, hodnoty všech formátu řetězce, nebude změnit vlastnosti hello. Tyto hodnoty mohou být převedená tooexisting datové typy:

![Ukázka záznamu 2](media/log-analytics-data-collector-api/record-02.png)

Ale pokud jste provedli poté tento další odeslání, analýzy protokolů by vytvořit hello nové vlastnosti **boolean_d** a **string_d**. Nelze převést tyto hodnoty:

![Ukázka záznamu 3](media/log-analytics-data-collector-api/record-03.png)

Pokud potom odeslán hello následující položky, před vytvořením hello typ záznamu, analýzy protokolů by vytvořit záznam s třemi vlastnostmi **úspěch**, **boolean_s**, a **string_s**. V této položce každé počáteční hodnoty hello je naformátovaná jako řetězec:

![Ukázka záznamu 4](media/log-analytics-data-collector-api/record-04.png)

## <a name="data-limits"></a>Omezení dat
Existují některá omezení kolem hello data odeslány toohello Log Analytics Data kolekce rozhraní API.

* Maximálně 30 MB za post tooLog analýzy dat kolekce rozhraní API. Toto je omezení velikosti pro jednu metodu post. Pokud hello data z jedné post, který je delší než 30 MB, je třeba rozdělit hello bloky dat si toosmaller velikosti a odešlete je současně.
* Maximální limit 32 KB pro pole hodnot. Pokud hodnota pole hello je větší než 32 KB, bude zkrácen hello data.
* Doporučený maximální počet polí pro daný typ je 50. Toto je praktické omezení z perspektivy prostředí vyhledávání a použitelnost.  

## <a name="return-codes"></a>Návratové kódy
Hello stavový kód HTTP 200 znamená, že byla přijata ke zpracování tohoto požadavku hello. To znamená, že hello operace byla úspěšně dokončena.

Tato tabulka uvádí hello kompletní sadu stavové kódy, které může služba hello vrátit:

| Kód | Status | Kód chyby | Popis |
|:--- |:--- |:--- |:--- |
| 200 |OK | |Hello žádost byla úspěšně přijata. |
| 400 |Chybný požadavek |InactiveCustomer |pracovní prostor Hello bylo ukončeno. |
| 400 |Chybný požadavek |InvalidApiVersion |verze Hello rozhraní API, který jste zadali nebyla rozpoznána službou hello. |
| 400 |Chybný požadavek |InvalidCustomerId |Zadané ID pracovního prostoru Hello je neplatný. |
| 400 |Chybný požadavek |InvalidDataFormat |Byla odeslána neplatná JSON. text odpovědi Hello může obsahovat další informace o tom, jak tooresolve hello chyby. |
| 400 |Chybný požadavek |InvalidLogType |Typ protokolu Hello zadat obsahují zvláštní znaky nebo číslice. |
| 400 |Chybný požadavek |MissingApiVersion |verze rozhraní API Hello nebyl zadán. |
| 400 |Chybný požadavek |MissingContentType |Typ obsahu Hello nebyl zadán. |
| 400 |Chybný požadavek |MissingLogType |Hello vyžaduje protokolu typ hodnoty nebyl zadán. |
| 400 |Chybný požadavek |UnsupportedContentType |Typ obsahu Hello nebyl nastaven příliš**application/json**. |
| 403 |Je zakázané |InvalidAuthorization |Hello služby se nezdařilo tooauthenticate hello požadavku. Ověřte platné klíči hello prostoru ID a připojení. |
| 404 |Nebyl nalezen | | Buď zadaná adresa URL hello je nesprávný, nebo hello požadavku je příliš velký. |
| 429 |Příliš mnoho požadavků | | Služba Hello dochází k velkému počtu data z účtu. Hello požadavek opakujte akci později. |
| 500 |Vnitřní chyba serveru |UnspecifiedError |Hello služby došlo k vnitřní chybě. Zkuste provést požadavek hello. |
| 503 |Služba není k dispozici |ServiceUnavailable |Hello služba je aktuálně k dispozici tooreceive požadavky. Opakujte žádost. |

## <a name="query-data"></a>Dotazování dat
tooquery data odeslaná při hello Log Analytics HTTP dat kolekce API, vyhledejte záznamy s **typ** který je rovna toohello **LogType** hodnotu, která jste zadali, spolu s **_CL**. Pokud jste použili například **MyCustomLog**, pak by vrátit všechny záznamy s **typ = MyCustomLog_CL**.

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak hello výše dotazu by změňte následující toohello.

> `MyCustomLog_CL`

## <a name="sample-requests"></a>Ukázka požadavků
V dalších částech hello, budete najít ukázky jak toosubmit data toohello Log Analytics HTTP dat kolekce API pomocí různých programovacích jazyků.

Pro každý vzorek proveďte tyto kroky tooset hello proměnných pro hello autorizační hlavičky:

1. V hello portál Operations Management Suite, vyberte hello **nastavení** dlaždici a potom vyberte hello **připojené zdroje** kartě.
2. toohello napravo od **ID pracovního prostoru**, vyberte ikonu kopírování hello a vložte hello ID jako hodnota hello hello **ID zákazníka** proměnné.
3. toohello napravo od **primární klíč**, vyberte ikonu kopírování hello a vložte hello ID jako hodnota hello hello **sdílený klíč** proměnné.

Alternativně můžete změnit hello proměnných pro typ protokolu hello a JSON data.

### <a name="powershell-sample"></a>Ukázkové prostředí PowerShell
```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify hello name of hello record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with hello created time for hello records
$TimeStampField = "DateValue"


# Create two records with hello same set of properties toocreate
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create hello function toocreate hello authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create hello function toocreate and post hello request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit hello data toohello API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>Ukázka v jazyce C#
```
using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;

namespace OIAPIExample
{
    class ApiExample
    {
        // An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField3"":""DemoValue3"",""DemoField4"":""DemoValue4""}]";

        // Update customerId tooyour Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either hello primary or hello secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of hello event type that is being submitted tooLog Analytics
        static string LogName = "DemoExample";

        // You can use an optional field toospecify hello timestamp from hello data. If hello time field is not specified, Log Analytics assumes hello time is hello message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for hello API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build hello API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

        // Send a request toohello POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            try
            {
                string url = "https://" + customerId + ".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";

                System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
                client.DefaultRequestHeaders.Add("Accept", "application/json");
                client.DefaultRequestHeaders.Add("Log-Type", LogName);
                client.DefaultRequestHeaders.Add("Authorization", signature);
                client.DefaultRequestHeaders.Add("x-ms-date", date);
                client.DefaultRequestHeaders.Add("time-generated-field", TimeStampField);

                System.Net.Http.HttpContent httpContent = new StringContent(json, Encoding.UTF8);
                httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                Task<System.Net.Http.HttpResponseMessage> response = client.PostAsync(new Uri(url), httpContent);

                System.Net.Http.HttpContent responseContent = response.Result.Content;
                string result = responseContent.ReadAsStringAsync().Result;
                Console.WriteLine("Return Result: " + result);
            }
            catch (Exception excep)
            {
                Console.WriteLine("API Post Exception: " + excep.Message);
            }
        }
    }
}

```

### <a name="python-sample"></a>Ukázka Pythonu
```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update hello customer ID tooyour Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For hello shared key, use either hello primary or hello secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# hello log type is hello name of hello event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build hello API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request toohello POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code >= 200 and response.status_code <= 299):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Další kroky
- Použití hello [rozhraní API pro vyhledávání protokolu](log-analytics-log-search-api.md) tooretrieve data z úložiště analýzy protokolů hello.
