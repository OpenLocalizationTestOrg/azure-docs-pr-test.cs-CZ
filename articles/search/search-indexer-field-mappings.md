---
title: "Mapování polí v Azure Search indexery"
description: "Konfigurace mapování polí indexer Azure Search, aby se zohlednily rozdíly v názvy polí a reprezentace dat"
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
ms.openlocfilehash: 57e91f070d9a42882a56e708f12b1ce238ed9191
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="field-mappings-in-azure-search-indexers"></a>Mapování polí v Azure Search indexery
Při použití indexerů Azure Search, můžete sami někdy nalézt v situacích, kde vstupní data poměrně neodpovídá schématu cílový index. V takových případech můžete použít **pole mapování** pro transformaci dat do požadovaného tvaru.

Některé situace, kdy jsou užitečné mapování polí:

* Váš zdroj dat má pole `_id`, ale Azure Search neumožňuje názvy polí začíná podtržítkem. Mapování polí umožňuje "přejmenovat" pole.
* Chcete naplnění několik polí v indexu se stejnými daty zdroje dat, například vzhledem k tomu, že chcete použít jiný analyzátorů těchto polí. Mapování polí umožňují "rozvětvit" pole datového zdroje.
* Budete potřebovat formátu Base64 zakódování nebo dekódování vaše data. Mapování polí podporují několik **mapování funkce**, včetně funkcí pro Base64 kódování a dekódování.   

## <a name="setting-up-field-mappings"></a>Nastavení mapování polí
Při vytváření nového pomocí indexeru můžete přidat mapování polí [vytvořit Indexer](https://msdn.microsoft.com/library/azure/dn946899.aspx) rozhraní API. Můžete spravovat mapování polí na indexování indexer pomocí [aktualizace Indexer](https://msdn.microsoft.com/library/azure/dn946892.aspx) rozhraní API.

Mapování polí se skládá ze 3 částí:

1. A `sourceFieldName`, která představuje pole ve zdroji dat. Tato vlastnost je vyžadována.
2. Volitelný `targetFieldName`, která představuje pole v indexu vyhledávání. Pokud tento parametr vynechán, stejný název jako zdroj dat se používá.
3. Volitelný `mappingFunction`, které můžete transformovat data pomocí jedné z několik předdefinovaných funkcí. Úplný seznam funkcí je [pod](#mappingFunctions).

Mapování polí jsou přidány do `fieldMappings` poli v definici indexer.

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
> Služba Azure Search používá porovnávání překládat názvy polí a funkce v mapování polí. To je vhodné (není třeba získat správné všechny malá a velká písmena), ale znamená, že nemůže mít zdroj dat nebo index pole, která se liší pouze v případě.  
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
Provede *URL bezpečných* vstupní řetězec kódování Base64. Předpokládá, že vstup je UTF-8.

#### <a name="sample-use-case"></a>Ukázka případ použití
Pouze znaky adresy URL bezpečných může vyskytovat v klíči dokumentu Azure Search (protože zákazníci musí být schopen adres pomocí rozhraní API pro vyhledávání, například dokument). Pokud vaše data obsahuje adresu URL unsafe znaky a chcete použít k naplnění klíčové pole v indexu vyhledávání, použijte tuto funkci.   

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
Provede Base64 dekódování vstupního řetězce. Vstup se předpokládá, že *URL bezpečných* řetězec s kódováním Base64.

#### <a name="sample-use-case"></a>Ukázka případ použití
Hodnoty vlastní metadata objektu BLOB musí být kódováním ASCII. Kódování Base64 můžete reprezentovat libovolný řetězců v kódu Unicode v vlastní metadata objektu blob. Ale aby vyhledávání smysl, můžete tuto funkci zapnout kódovaného data zpět do "Regulérní" řetězce při naplňování indexu vyhledávání.  

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
Rozdělí pole řetězce pomocí zadaného oddělovače a vybere token na zadané pozici v výsledné rozdělení.

Například, pokud je vstupní `Jane Doe`, `delimiter` je `" "`(místo) a `position` je 0, výsledkem je `Jane`; Pokud `position` je 1, výsledkem je `Doe`. Pokud pozice odkazuje na token, který neexistuje, bude vrácena chyba.

#### <a name="sample-use-case"></a>Ukázka případ použití
Zdroj dat obsahuje `PersonName` pole a chcete indexu jako dva samostatné `FirstName` a `LastName` pole. Tato funkce slouží k rozdělení vstupu pomocí znak mezery jako oddělovače.

#### <a name="parameters"></a>Parametry
* `delimiter`: řetězec, který chcete použít jako oddělovače při rozdělování vstupní řetězec.
* `position`: celé číslo pozice s nulovým základem tokenu a vyberte po vstupní řetězec rozdělen.    

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
Transformuje řetězec formátovaný jako pole JSON řetězců do pole řetězec, který slouží k naplnění `Collection(Edm.String)` pole v indexu.

Například, pokud je vstupní řetězec `["red", "white", "blue"]`, pak cílové pole typu `Collection(Edm.String)` vyplní pomocí tří hodnot `red`, `white` a `blue`. Pro vstupní hodnoty, které nelze analyzovat jako řetězec pole JSON bude vrácena chyba.

#### <a name="sample-use-case"></a>Ukázka případ použití
Databáze SQL Azure nemá předdefinované datový typ, který mapuje přirozeně `Collection(Edm.String)` pole ve službě Azure Search. K naplnění kolekci polí s řetězcem, formátování zdroj dat jako pole řetězců JSON a tuto funkci můžete používat.

#### <a name="example"></a>Příklad
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```

## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure Search
Pokud máte žádosti o funkce nebo vylepšení nápady, kontaktujte nás na našem [UserVoice lokality](https://feedback.azure.com/forums/263029-azure-search/).
