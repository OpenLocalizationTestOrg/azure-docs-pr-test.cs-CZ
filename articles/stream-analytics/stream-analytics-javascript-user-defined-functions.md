---
title: "uživatelem definované funkce Stream Analytics JavaScript aaaAzure | Microsoft Docs"
description: "Provedení mechanismy rozšířený dotaz s uživatelem definované funkce jazyka JavaScript"
keywords: "JavaScript, funkce udf definované uživatelem"
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 28eeb8f6437c23989e8887687b950361fed4414c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-functions"></a>Uživatelem definované funkce Azure Stream Analytics JavaScript
Azure Stream Analytics podporuje uživatelsky definované funkce, které jsou napsané v jazyce JavaScript. S bohatou sadu hello **řetězec**, **RegExp**, **matematické**, **pole**, a **datum** metody, Transformace komplexní dat pomocí úlohy Stream Analytics poskytuje JavaScript, stane jednodušší toocreate.

## <a name="javascript-user-defined-functions"></a>Uživatelem definované funkce jazyka JavaScript
Uživatelem definované funkce jazyka JavaScript podporovat bezstavové, pouze výpočetní skalární funkce, které nevyžadují externí připojení. Hello návratové hodnoty funkce lze pouze skalární hodnotu (jeden). Po přidání úlohu tooa uživatelsky definované funkce JavaScript, je funkce hello použít kdekoli v hello dotazu, jako je integrovaná skalární funkce.

Tady je několik scénářů, kde mohou být užitečné JavaScript uživatelsky definované funkce:
* Analýza a manipulace s řetězci, které mají regulární výraz funkce, například **Regexp_Replace()** a **Regexp_Extract()**
* Kódování dat, například binární šestnáctkově převod a dekódování
* Provádění mathematic výpočtů v jazyce JavaScript **matematické** funkce
* Provádění operací pole jako řazení, spojení, najít a výplně

Zde jsou některé věci, které nemůžete dělat s uživatelem definované funkce jazyka JavaScript v Stream Analytics:
* Volání na externí koncové body REST, například provádění obrátíte vyhledávání IP nebo vyžádání referenční data z externího zdroje
* Deserializace na vstupy/výstupy nebo provést vlastní události formát serializace
* Vytvořte vlastní agregace

I když funguje jako **Date.GetDate()** nebo **Math.random()** nejsou blokována v definici funkce hello byste neměli používat je. Tyto funkce **nepodporují** návratový hello stejné vést pokaždé, když je volají a hello služby Azure Stream Analytics nezachovat deník volání funkce a vrátí výsledky. Pokud funkce vrátí výsledek různých na hello stejné události, opakovatelnost není zaručena při restartování úlohy vámi nebo služby Stream Analytics hello.

## <a name="add-a-javascript-user-defined-function-in-hello-azure-portal"></a>Přidání uživatelem definované funkce jazyka JavaScript v hello portálu Azure
toocreate jednoduché funkce jazyka JavaScript uživatelem definované v části existující úlohy Stream Analytics, proveďte tyto kroky:

1.  V hello portálu Azure najděte úlohu služby Stream Analytics.
2.  V části **úlohy TOPOLOGIE**, vyberte funkce. Zobrazí se prázdný seznam funkcí.
3.  toocreate nové uživatelsky definované funkce, vyberte **přidat**.
4.  Na hello **novou funkci** okně pro **typ funkce**, vyberte **JavaScript**. Funkce výchozí šablona služby se zobrazí v editoru hello.
5.  Pro hello **UDF alias**, zadejte **hex2Int**a změňte implementace funkce hello následujícím způsobem:

    ```
    // Convert Hex value toointeger.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  Vyberte **Uložit**. Funkce se zobrazí v seznamu hello funkcí.
7.  Vyberte hello nové **hex2Int** funkce a zkontrolujte definici funkce hello. Mají všechny funkce **UDF** předponu přidané toohello funkce alias. Je třeba příliš*obsahovat předponu hello* při volání funkce hello v dotazu Stream Analytics. V takovém případě volání **UDF.hex2Int**.

## <a name="call-a-javascript-user-defined-function-in-a-query"></a>Volání uživatelem definované funkce JavaScript v dotazu

1. V hello dotazu editor, v části **úlohy TOPOLOGIE**, vyberte **dotazu**.
2.  Upravit dotaz a pak zavolají hello uživatelsky definované funkce, například takto:

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  tooupload hello ukázkový datový soubor, klikněte pravým tlačítkem na hello úlohy vstup.
4.  tootest dotazu, vyberte **Test**.


## <a name="supported-javascript-objects"></a>Podporované JavaScript – objekty
Uživatelem definované funkce Azure Stream Analytics JavaScript podporují standard, předdefinovaných objektů jazyka JavaScript. Seznam těchto objektů najdete v tématu [globální objekty](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

### <a name="stream-analytics-and-javascript-type-conversion"></a>Stream Analytics a JavaScript typ – převod

Existují rozdíly v hello typy, které podporují hello Stream Analytics dotazovací jazyk a JavaScript. Tato tabulka uvádí hello převod mapování mezi hello dva:

Stream Analytics | JavaScript
--- | ---
bigint | Číslo (JavaScript může představovat jenom celá čísla nahoru tooprecisely 2 ^ 53)
Data a času | Datum (JavaScript pouze podporuje v milisekundách)
Double | Číslo
nvarchar(max) | Řetězec
záznam | Objekt
Pole | Pole
HODNOTU NULL | Hodnotu Null


Zde jsou převody JavaScript Stream Analytics:


JavaScript | Stream Analytics
--- | ---
Číslo | Bigint (Pokud hello číslo se zaokrouhlí a mezi dlouho. MinValue a dlouho. MaxValue; jinak je dvojité)
Datum | Data a času
Řetězec | nvarchar(max)
Objekt | záznam
Pole | Pole
Hodnotu Null, Nedefinovaná | HODNOTU NULL
Jiný typ (například funkce nebo chyba) | Není podporované (výsledky v chybě za běhu)

## <a name="troubleshooting"></a>Řešení potíží
Chyby jazyka JavaScript runtime jsou považovány za kritické a jsou prezentované prostřednictvím protokolu aktivit hello. tooretrieve hello přihlášení, hello portálu Azure, přejděte tooyour úlohy a vyberte **protokol aktivit**.


## <a name="other-javascript-user-defined-function-patterns"></a>Dalšími vzory uživatelsky definované funkce jazyka JavaScript

### <a name="write-nested-json-toooutput"></a>Zápis vnořené toooutput JSON
Pokud máte následné zpracování krok, který používá úloha Stream Analytics výstup jako vstup a vyžaduje formátu JSON, můžete napsat toooutput řetězec formátu JSON. Další příklad Hello volá hello **JSON.stringify()** funkce toopack všechny dvojice název hodnota hello vstup a zapsat je jako jeden řetězec hodnotu ve výstupu.

**Definice JavaScript uživatelsky definované funkce:**

```
function main(x) {
return JSON.stringify(x);
}
```

**Ukázkový dotaz:**
```
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.json_stringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="get-help"></a>Podpora
Potřebujete další pomoc, zkuste naši [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language – referenční informace](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Správa Azure Stream Analytics odkazu k REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
