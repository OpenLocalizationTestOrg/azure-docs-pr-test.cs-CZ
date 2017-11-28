---
title: "mapování aaaField v Azure Search indexery"
description: "Konfigurace Azure Search indexer pole mapování tooaccount pro rozdíly v reprezentace pole názvy a data"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 0325a4de-0190-4dd5-a64d-4e56601d973b
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: eugenesh
ms.openlocfilehash: 009d5dbc12cb9e8d9cfd3e8042e907ca88399ad7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="field-mappings-in-azure-search-indexers"></a>Mapování polí v Azure Search indexery
Při použití indexerů Azure Search, můžete sami někdy nalézt v situacích, kde vstupní data poměrně neodpovídá hello schéma cílový index. V takových případech můžete použít **pole mapování** tootransform dat do aplikace hello požadovaného tvaru.

Některé situace, kdy jsou užitečné mapování polí:

* Váš zdroj dat má pole `_id`, ale Azure Search neumožňuje názvy polí začíná podtržítkem. Mapování polí umožňuje příliš "přejmenujete" pole.
* Chcete-li toopopulate několik polí v hello indexu s hello stejný datový zdroj dat, například, protože chcete tooapply různých analyzátorů toothose pole. Mapování polí umožňují "rozvětvit" pole datového zdroje.
* Je třeba tooBase64 zakódování nebo dekódování vaše data. Mapování polí podporují několik **mapování funkce**, včetně funkcí pro Base64 kódování a dekódování.   

## <a name="setting-up-field-mappings"></a>Nastavení mapování polí
Můžete přidat mapování polí při vytváření nové indexer pomocí hello [vytvořit Indexer](https://msdn.microsoft.com/library/azure/dn946899.aspx) rozhraní API. Můžete spravovat mapování polí na indexer indexování pomocí hello [aktualizace Indexer](https://msdn.microsoft.com/library/azure/dn946892.aspx) rozhraní API.

Mapování polí se skládá ze 3 částí:

1. A `sourceFieldName`, která představuje pole ve zdroji dat. Tato vlastnost je vyžadována.
2. Volitelný `targetFieldName`, která představuje pole v indexu vyhledávání. Pokud tento parametr vynechán, hello stejný název jako zdroj dat hello se používá.
3. Volitelný `mappingFunction`, které můžete transformovat data pomocí jedné z několik předdefinovaných funkcí. Hello úplný seznam funkcí je [pod](#mappingFunctions).

Mapování polí jsou přidány toohello `fieldMappings` poli v definici indexer hello.

Můžete zde je ukázka, jak můžete přizpůsobilo rozdílům v názvy polí:

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
}
```

Indexer může mít několik mapování polí. Například je zde jak vám může "rozvětvit" pole:

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" },
]
```

> [!NOTE]
> Služba Azure Search používá názvy porovnávání tooresolve hello pole a funkce v mapování polí. To je vhodné (nemáte tooget všechny hello velká a malá písmena správné), ale znamená, že nemůže mít zdroj dat nebo index pole, která se liší pouze v případě.  
>
>

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Funkce mapování polí
Tyto funkce jsou aktuálně podporovány:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

### <a name="base64encode"></a>base64Encode
Provede *URL bezpečných* kódování Base64 hello vstupní řetězec. Předpokládá, že vstup hello je UTF-8.

#### <a name="sample-use-case"></a>Ukázka případ použití
Pouze znaky adresy URL bezpečných může vyskytovat v klíči dokumentu Azure Search (protože zákazníci musí být schopný tooaddress hello dokumentu hello rozhraní API pro vyhledávání, například pomocí). Pokud vaše data obsahuje adresu URL unsafe znaky a chcete toouse ho toopopulate klíčové pole v indexu vyhledávání, použijte tuto funkci.   

#### <a name="example"></a>Příklad
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Path",
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" }
  }]
```

<a name="base64DecodeFunction"></a>

### <a name="base64decode"></a>base64Decode
Provede dekódování hello vstupní řetězec Base64. Hello vstup se předpokládá, že tooa *URL bezpečných* řetězec s kódováním Base64.

#### <a name="sample-use-case"></a>Ukázka případ použití
Hodnoty vlastní metadata objektu BLOB musí být kódováním ASCII. Můžete vytvořit Base64 kódování toorepresent libovolný řetězců v kódu Unicode v vlastní metadata objektu blob. Však hledání toomake smysluplný, můžete tato funkce tooturn hello kódovaný data zpět do "Regulérní" řetězce při naplňování indexu vyhledávání.  

#### <a name="example"></a>Příklad
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" }
  }]
```

<a name="extractTokenAtPositionFunction"></a>

### <a name="extracttokenatposition"></a>extractTokenAtPosition
Rozdělení specifikovány pole řetězce pomocí hello oddělovač a vyskladnění hello token v hello zadané pozici v hello výsledné rozdělení.

Například pokud hello vstup je `Jane Doe`, hello `delimiter` je `" "`(místo) a hello `position` je 0, výsledek hello `Jane`; Pokud hello `position` je 1, hello výsledkem je `Doe`. Pokud hello pozice odkazuje tooa token, který neexistuje, bude vrácena chyba.

#### <a name="sample-use-case"></a>Ukázka případ použití
Zdroj dat obsahuje `PersonName` pole a chcete, aby tooindex jej jako dva samostatné `FirstName` a `LastName` pole. Pomocí této funkce toosplit hello vstup pomocí hello znak jako hello oddělovač.

#### <a name="parameters"></a>Parametry
* `delimiter`: řetězec toouse jako oddělovače hello při rozdělování hello vstupní řetězec.
* `position`: je rozdělená celé číslo pozice s nulovým základem z tokenu toopick hello po hello vstupní řetězec.    

#### <a name="example"></a>Příklad
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } }
  },
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } }
  }]
```

<a name="jsonArrayToStringCollectionFunction"></a>

### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection
Transformace řetězec formátu JSON pole řetězců do pole řetězců, které můžou být použité toopopulate `Collection(Edm.String)` pole v indexu hello.

Například pokud hello vstupní řetězec je `["red", "white", "blue"]`, pak hello cílové pole typu `Collection(Edm.String)` vyplní hodnotami hello tři `red`, `white` a `blue`. Pro vstupní hodnoty, které nelze analyzovat jako řetězec pole JSON bude vrácena chyba.

#### <a name="sample-use-case"></a>Ukázka případ použití
Databáze SQL Azure nemá předdefinované datový typ, který přirozeně mapuje příliš`Collection(Edm.String)` pole ve službě Azure Search. toopopulate řetězec kolekci polí, formátování zdroj dat jako pole řetězců JSON a tuto funkci můžete používat.

#### <a name="example"></a>Příklad
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```

## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure Search
Pokud máte žádosti o funkce nebo vylepšení nápady, prosím oslovení toous na našem [UserVoice lokality](https://feedback.azure.com/forums/263029-azure-search/).
