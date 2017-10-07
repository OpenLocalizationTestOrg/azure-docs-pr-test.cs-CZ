---
title: "aaaUnderstand hello dotazovacího jazyka pro Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – popis dotazu jazyka SQL jako IoT Hub hello používá tooretrieve informace o úlohách ze služby IoT hub a dvojčata zařízení."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/17
ms.author: elioda
ms.openlocfilehash: 01a7c8ffdf44c6c27b834739d02c8fef1dd3d3fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a>Referenční dokumentace - IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv

Služba IoT Hub zajišťuje a výkonné jazyka SQL tooretrieve informace ohledně [dvojčata zařízení] [ lnk-twins] a [úlohy][lnk-jobs]a [směrování zpráv][lnk-devguide-messaging-routes]. Tento článek představuje:

* Úvod toohello hlavní funkce hello dotazovacího jazyka pro IoT Hub, a
* Hello podrobný popis jazyka hello.

## <a name="get-started-with-device-twin-queries"></a>Začínáme s dotazy twin zařízení
[Dvojčata zařízení] [ lnk-twins] může obsahovat libovolné objekty JSON vlastnosti a značky. IoT Hub můžete dvojčata zařízení tooquery jako jeden dokument JSON obsahující všechny informace o dvojici zařízení.
Předpokládejme například, vaše dvojčata zařízení IoT hub měli hello strukturu:

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

IoT Hub zpřístupní dvojčata zařízení hello jako kolekce dokumentů s názvem **zařízení**.
Proto hello následující dotaz načte hello celou sadu dvojčata zařízení:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Sady SDK služby Azure IoT] [ lnk-hub-sdks] podporu stránkování výsledků velký.

Centrum IoT vám umožní dvojčata zařízení tooretrieve filtrování pomocí libovolné podmínky. Pro instanci,

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

načte hello dvojčata zařízení s hello **location.region** značky nastavit příliš**USA**.
Logické operátory a aritmetické porovnání jsou podporovány také, například

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

načte všechny dvojčata zařízení nachází v hello nám nakonfigurované toosend telemetrie méně často než každou minutu. Pro potřeby je také možné toouse konstanty pole s hello **IN** a **NV** operátory (ne v). Pro instanci,

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

načte všechny dvojčata zařízení, které ohlásil Wi-Fi nebo drátové připojení. Ho je často nutné tooidentify všechny dvojčata zařízení, které obsahují určité vlastnosti. IoT Hub podporuje funkce hello `is_defined()` pro tento účel. Pro instanci,

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

načíst všechny dvojčata zařízení, které definují hello `connectivity` hlášené vlastnost. Odkazovat toohello [klauzule WHERE] [ lnk-query-where] části pro úplný přehled hello hello možností filtrování.

Seskupení a agregace jsou také podporovány. Pro instanci,

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

Vrátí hello počet zařízení hello každý stav konfigurace telemetrie.

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

Hello předchozí příklad znázorňuje situaci, kdy tři zařízení hlášené úspěšná konfigurace, dva jsou stále aplikování hello konfigurace a jeden ohlásil chybu.

### <a name="c-example"></a>Příklad jazyka C#
Hello dotazu funkce je zveřejněna rozhraním hello [C# sady SDK služby] [ lnk-hub-sdks] v hello **RegistryManager** třídy.
Tady je příklad jednoduchého dotazu:

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

Poznámka: jak hello **dotazu** vytvořit instanci objektu s velikostí stránky (až too1000) a pak může být více stránek načíst volání hello **GetNextAsTwinAsync** metody vícekrát.
Poznámka: Tento objekt dotazu hello vystavuje více **Další\***, v závislosti na možnost hello deserializaci vyžadované hello dotazu, například objekty zařízení twin nebo úlohy, nebo nešifrovaná toobe JSON pro používání projekce.

### <a name="nodejs-example"></a>Příklad Node.js
Hello dotazu funkce je zveřejněna rozhraním hello [sady SDK služby Azure IoT pro Node.js] [ lnk-hub-sdks] v hello **registru** objektu.
Tady je příklad jednoduchého dotazu:

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed toofetch hello results: ' + err.message);
    } else {
        // Do something with hello results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

Poznámka: jak hello **dotazu** vytvořit instanci objektu s velikostí stránky (až too1000) a pak může být více stránek načíst volání hello **nextAsTwin** metody vícekrát.
Poznámka: Tento objekt dotazu hello vystavuje více **Další\***, v závislosti na možnost hello deserializaci vyžadované hello dotazu, například objekty zařízení twin nebo úlohy, nebo nešifrovaná toobe JSON pro používání projekce.

### <a name="limitations"></a>Omezení
> [!IMPORTANT]
> Výsledky dotazu může mít několik minut zpoždění s ohledem toohello nejnovější hodnoty v dvojčata zařízení. Pokud dotazuje dvojčata jednotlivých zařízení podle id, vždycky je vhodnější toouse hello načíst twin rozhraní API pro zařízení, která vždy obsahuje nejnovější hodnoty hello a má, vyšší omezení.

V současné době porovnání jsou podporovány pouze mezi primitivní typy (žádné objekty), například `... WHERE properties.desired.config = properties.reported.config` je podporováno pouze v případě, že tyto vlastnosti mají primitivní hodnoty.

## <a name="get-started-with-jobs-queries"></a>Začínáme s dotazy úlohy
[Úlohy] [ lnk-jobs] poskytují způsob, jak tooexecute operace na sadu zařízení. Každý dvojče zařízení obsahuje informace o hello hello úloh, které je součástí v kolekci s názvem **úlohy**.
Logicky,

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

Tato kolekce je v současné době dotazovatelné jako **devices.jobs** v hello dotazovacího jazyka pro IoT Hub.

> [!IMPORTANT]
> V současné době je nikdy vrácena vlastnost úlohy hello při dotazování dvojčata zařízení (to znamená, dotazy, které obsahuje, ze zařízení"). Lze přistupovat pouze přímo s dotazy pomocí `FROM devices.jobs`.
>
>

Například tooget všechny úlohy (posledních a plánované) ovlivňující jedno zařízení, můžete použít hello následující dotaz:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

Všimněte si, jak tento dotaz poskytuje hello konkrétní zařízení stav (a případně hello přímá metoda odpověď) každé úlohy vrátila.
Je také možné toofilter s libovolnými Boolean podmínek na všechny vlastnosti objektu v hello **devices.jobs** kolekce.
Například hello následující dotaz:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

načte všechny dokončit zařízení twin aktualizace úlohy pro zařízení **myDeviceId** vytvořených po září 2016.

Je také možné tooretrieve hello podle zařízení výstupy jednu úlohu.

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Omezení
V současné době se dotazuje na **devices.jobs** nepodporují:

* Projekce, proto pouze `SELECT *` je možné.
* Podmínky, které odkazují dvojče zařízení toohello v přidání vlastnosti toojob (viz hello předcházející části).
* Provádění agregace, jako je počet, avg, Seskupit podle.

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a>Začínáme s výrazy dotazů trasy zprávu typu zařízení cloud

Pomocí [zařízení cloud trasy][lnk-devguide-messaging-routes], můžete nakonfigurovat IoT Hub toodispatch zařízení cloud zprávy koncové body toodifferent založené na výrazech vyhodnotit na základě jednotlivých zpráv.

Hello trasy [podmínku] [ lnk-query-expressions] hello používá stejné dotazovací jazyk IoT Hub jako podmínky v dotazech twin a úlohy. Podmínky trasy se vyhodnocují na hlavičky zpráv hello a text. Směrování výraz dotazu může zahrnovat pouze hlavičky zpráv, pouze tělo zprávy hello, nebo obě zpráva hlavičky a tělo zprávy. IoT Hub předpokládá konkrétní schéma pro hello hlavičky a tělo zprávy ve zprávách tooroute pořadí a hello následující části popisují, co je požadované pro trasu tooproperly IoT Hub:

### <a name="routing-on-message-headers"></a>Směrování v záhlaví zpráv

IoT Hub předpokládá hello následující reprezentace JSON hlavičky zpráv pro směrování zpráv:

```json
{
    "$messageId": "",
    "$enqueuedTime": "",
    "$to": "",
    "$expiryTimeUtc": "",
    "$correlationId": "",
    "$userId": "",
    "$ack": "",
    "$connectionDeviceId": "",
    "$connectionDeviceGenerationId": "",
    "$connectionAuthMethod": "",
    "$content-type": "",
    "$content-encoding": "",

    "userProperty1": "",
    "userProperty2": ""
}
```

Vlastnosti zprávu systému mají předponu hello `'$'` symbol.
Vlastnosti uživatelů jsou vždy přistupovat pomocí jeho názvu. Pokud název vlastnosti uživatele se stane toocoincide s vlastností systému (například `$to`), bude načten hello vlastnost uživatele s hello `$to` výraz.
Vždy přístup k vlastnosti systému hello pomocí závorek `{}`: například můžete použít výraz hello `{$to}` tooaccess hello vlastnost systému `to`. Názvy vlastností v závorkách vždy načítají hello odpovídající vlastnost systému.

Mějte na paměti, že jsou názvy vlastností malá a velká písmena.

> [!NOTE]
> Všechny vlastnosti zprávy jsou řetězce. Vlastnosti systému, jak je popsáno v hello [Příručka vývojáře][lnk-devguide-messaging-format], nejsou aktuálně k dispozici toouse v dotazech.
>

Například, pokud používáte `messageType` vlastnost, můžete chtít tooroute všechny endpoint tooone telemetrie a všechny výstrahy tooanother koncového bodu. Můžete napsat hello následující výraz tooroute hello telemetrie:

```sql
messageType = 'telemetry'
```

A hello následující výraz tooroute hello výstražných zpráv:

```sql
messageType = 'alert'
```

Logické výrazy a funkce jsou také podporovány. Tato funkce umožňuje toodistinguish mezi úroveň závažnosti, například:

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

Odkazovat toohello [výraz a podmínky] [ lnk-query-expressions] části hello úplný seznam podporovaných operátory a funkce.

### <a name="routing-on-message-bodies"></a>Směrování na obsah zpráv

IoT Hub můžete pouze směrování podle tělo zprávy obsah, pokud je text zprávy hello správně vytvořen JSON kódování UTF-8, UTF-16 nebo UTF-32. Je nutné nastavit typ obsahu hello hello zprávy příliš`application/json` a hello obsahu kódování tooone hello podporované kódování UTF v tooallow záhlaví zprávy hello IoT Hub tooroute uvítací zprávu podle obsah textu hello. Pokud není zadán buď hello hlaviček, nepokusí IoT Hub tooevaluate jakýkoli výraz dotazu zahrnující textu hello proti uvítací zprávu. Pokud vaše zpráva není zprávu JSON, nebo pokud uvítací zprávu neurčuje hello typu obsahu a kódování obsahu, můžete stále používat směrování podle hlavičky zpráv hello tooroute uvítací zprávu zprávy.

Můžete použít `$body` hello dotazu výraz tooroute hello zprávy. Jednoduchý text odkazu, odkaz na pole text nebo více odkazů text můžete použít ve výrazu dotazu hello. Výraz dotazu můžete také kombinovat textu odkaz s odkazem na záhlaví zprávy. Například následující hello jsou všechny výrazy platný dotaz:

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a>Základní informace o dotaz služby IoT Hub
Každé centrum IoT dotaz sestává s výběrem a klauzulí FROM a volitelné WHERE a GROUP BY – klauzule. Každý dotaz je spuštěn na kolekci dokumentů JSON, například dvojčata zařízení. klauzule FROM Hello označuje hello dokumentu kolekce toobe vstupní na (**zařízení** nebo **devices.jobs**). Potom hello filtru v hello v případě použití klauzule. S agregace, hello výsledky tohoto kroku seskupeny jako hello zadaný v klauzuli GROUP BY, a pro každou skupinu, je generována řádek uvedené v klauzuli SELECT hello.

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM – klauzule
Hello **z < from_specification >** klauzule můžete předpokládat pouze dvě hodnoty: **ze zařízení**, tooquery dvojčata zařízení, nebo **z devices.jobs**, tooquery úlohy na zařízení Podrobnosti.

## <a name="where-clause"></a>Klauzule WHERE
Hello **kde < filter_condition >** klauzule je volitelný. Určuje jeden nebo více podmínky, že dokumenty JSON hello v kolekci FROM hello musí splňovat toobe jako součást výsledků hello. Dokumentu JSON se musí vyhodnotit hello zadané podmínky příliš "true" toobe součástí hello výsledku.

Hello povolené podmínky jsou popsány v části [výrazy a podmínky][lnk-query-expressions].

## <a name="select-clause"></a>Klauzule SELECT
Klauzule SELECT Hello (**vyberte < select_list >**) je povinná a určuje, jaké hodnoty jsou načteny z dotazu hello. Určuje, že toobe hodnoty JSON hello používá toogenerate nové objekty JSON.
Pro každý element hello filtrované (a volitelně seskupené) podmnožinu hello FROM kolekce, fáze projekce hello vygeneruje nový objekt JSON, zkonstruovat s hello hodnot zadaných v klauzuli SELECT hello.

Toto je hello gramatika vyberte klauzule hello:

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

kde **attribute_name** odkazuje vlastnost tooany hello dokumentu JSON v kolekci FROM hello. Některé příklady klauzule FROM lze nalézt v hello [Začínáme s dotazy twin zařízení] [ lnk-query-getstarted] části.

V současné době výběr klauzule liší od **vyberte \***  jsou podporovány pouze v agregační dotazy na dvojčata zařízení.

## <a name="group-by-clause"></a>klauzule GROUP BY
Hello **GROUP BY < group_specification >** klauzule je volitelný krok, který lze provést po hello filtr zadaný v hello klauzule WHERE a před hello projekce určená v hello vyberte. Seskupuje dokumentů podle hello hodnotu atributu. Tyto skupiny jsou použité toogenerate agregovat hodnoty zadané v klauzuli SELECT hello.

Příklad dotazu pomocí skupiny je:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

Tady je syntax formální Hello Seskupit podle:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

kde **attribute_name** odkazuje vlastnost tooany hello dokumentu JSON v kolekci FROM hello.

Klauzule GROUP BY hello je v současné době podporována pouze při dotazování dvojčata zařízení.

## <a name="expressions-and-conditions"></a>Výrazy a podmínky
Na vysoké úrovni *výraz*:

* Vyhodnotí tooan instanci typu JSON (například logická hodnota, čísla, řetězce, pole nebo objekt), a
* Je definován manipulace s daty z dokumentu JSON hello zařízení a konstant pomocí předdefinovaných operátory a funkce.

*Podmínky* jsou výrazy, která se vyhodnotí tooa logická hodnota. Žádné jiné než logická hodnota konstanta **true** se považuje za **false** (včetně **null**, **nedefinované**, objekt nebo pole instance, všechny řetězce a jasně hello logická hodnota **false**).

Hello syntaxe pro výrazy je:

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

Kde:

| Symbol | Definice |
| --- | --- |
| attribute_name | Vlastnost hello dokumentu JSON v hello **FROM** kolekce. |
| binary_operator | Všechny binární operátor uvedené v hello [operátory](#operators) části. |
| Název_funkce| Všechny funkce uvedené v hello [funkce](#functions) části. |
| decimal_literal |Float, vyjádřené v desítkovém zápisu. |
| hexadecimal_literal |Číslo vyjádřená hello řetězec '0 x, za nímž následuje řetězec šestnáctkových číslic. |
| string_literal |Textové literály jsou reprezentována posloupnost nula nebo více znaků Unicode nebo řídicí sekvence řetězců v kódu Unicode. Textové literály jsou uzavřená do jednoduchých uvozovek (apostrof: ') nebo dvojité uvozovky (uvozovky: "). Povolené řídicí sekvence: `\'`, `\"`, `\\`, `\uXXXX` pro znaky znakové sady Unicode, které jsou definované 4 hexadecimální číslice. |

### <a name="operators"></a>Operátory
jsou podporovány Hello následující operátory:

| Rodina | Operátory |
| --- | --- |
| Aritmetické operace |+,-,*,/,% |
| Logické |A, NEBO NE |
| Porovnání |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>Funkce
Při dotazování úlohy a dvojčata hello podporovány pouze funkce je:

| Funkce | Popis |
| -------- | ----------- |
| IS_DEFINED(Property) | Vrátí logickou hodnotu udávající, pokud byla vlastnost hello přiřazenou hodnotu (včetně `null`). |

V podmínkách trasy jsou podporovány následující matematické funkce hello:

| Funkce | Popis |
| -------- | ----------- |
| Abs(x) | Vrátí hello absolutní (kladné) hello zadána hodnota číselného výrazu. |
| Exp(x) | Vrátí hodnotu exponenciálního hello Dobrý den zadán číselný výraz (e ^ x). |
| Power(x,y) | Vrátí hello hodnotu hello zadaný výraz toohello zadaný power (x ^ y).|
| Square(x) | Vrátí hello odmocnina z hello zadat číselnou hodnotu. |
| CEILING(x) | Vrátí hello nejmenší celé číslo větší než nebo rovno, hello zadaný číselný výraz. |
| Floor(x) | Vrátí hello největší celé číslo menší než nebo rovna toohello zadán číselný výraz. |
| Sign(x) | Vrátí hello kladnou (+ 1), nula (0), nebo záporné znaménko (-1) hello zadán číselný výraz.|
| Sqrt(x) | Vrátí hello odmocnina z hello zadat číselnou hodnotu. |

V podmínkách trasy jsou podporovány následující typ kontrolu a přetypování funkce hello:

| Funkce | Popis |
| -------- | ----------- |
| AS_NUMBER | Převede hello vstupní řetězec tooa číslo. `noop`Pokud vstup je číslo; `Undefined` Pokud řetězec nepředstavuje číslo.|
| IS_ARRAY | Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je pole. |
| IS_BOOL | Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je logická hodnota. |
| IS_DEFINED | Vrátí logickou hodnotu udávající, pokud byla vlastnost hello přiřazena hodnota. |
| IS_NULL | Vrátí logickou hodnotu udávající, pokud zadaný typ hello hello výraz má hodnotu null. |
| IS_NUMBER | Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je číslo. |
| IS_OBJECT | Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je objekt JSON. |
| IS_PRIMITIVE | Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je primitivní (řetězec, logická hodnota, číselná nebo `null`). |
| IS_STRING | Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz obsahuje řetězec. |

V podmínkách trasy jsou podporovány následující funkce řetězce hello:

| Funkce | Popis |
| -------- | ----------- |
| CONCAT(x,...) | Vrátí řetězec, který je výsledkem hello zřetězení dvou nebo více řetězcové hodnoty. |
| Length(x) | Vrátí hello počet znaků, který hello zadán řetězcovým výrazem.|
| LOWER(x) | Vrací výraz řetězce po převodu dat toolowercase velké písmeno. |
| UPPER(x) | Vrací výraz řetězce po převodu dat toouppercase malé písmeno. |
| Dílčí řetězec (string, spuštění [, délka]) | Vrátí část výrazu řetězce začínající hello zadaný znak pozice s nulovým základem a pokračuje v toohello Zadaná délka nebo toohello konce řetězce hello. |
| INDEX_OF (řetězec, fragment) | Vrátí počáteční pozici prvního výskytu hello hello druhý řetězcového výrazu v rámci zadaného řetězcového výrazu první hello nebo -1, pokud není nalezen řetězec hello hello.|
| STARTS_WITH (x, y) | Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu začíná hello druhý. |
| ENDS_WITH (x, y) | Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu končí hello druhý. |
| CONTAINS(x,y) | Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu obsahuje hello druhý. |

## <a name="next-steps"></a>Další kroky
Zjistěte, jak tooexecute dotazuje ve svých aplikacích pomocí [SDK služby Azure IoT][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messages-read-custom.md
[lnk-devguide-messaging-format]: iot-hub-devguide-messages-construct.md
[lnk-devguide-messaging-routes]: ./iot-hub-devguide-messages-read-custom.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
