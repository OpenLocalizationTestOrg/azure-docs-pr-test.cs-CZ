---
title: typy obsahu aaaHandle - Azure Logic Apps | Microsoft Docs
description: "Jak se má Azure Logic Apps zacházet s typy obsahu v návrhu a prostředí runtime"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: cd1f08fd-8cde-4afc-86ff-2e5738cc8288
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: a823249c5388b15ae0aae450b40499b420ea005e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="handle-content-types-in-logic-apps"></a>Zpracování typů obsahu v aplikace logiky

Mnoho různých typů obsahu můžete procházet skrz aplikace logiky, včetně JSON, XML, ploché soubory a binární data. I když hello logiku aplikace modul podporuje všechny typy obsahu, některé jsou nativně srozumitelné hello modul aplikace logiky. Ostatní může vyžadovat přetypování nebo převody podle potřeby. Tento článek popisuje, jak hello modul zpracuje různých typů obsahu a jak toocorrectly zpracování těchto typů, pokud je to nezbytné.

## <a name="content-type-header"></a>Hlavička Content-Type

toostart v podstatě, podíváme se na hello dva `Content-Types` nevyžaduje převod nebo přetypování, který můžete použít v aplikaci logiky: `application/json` a `text/plain`.

## <a name="applicationjson"></a>Application/JSON

modul pracovních postupů Hello spoléhá na hello `Content-Type` záhlaví z HTTP volá toodetermine hello příslušné zpracování. Každá žádost s typem obsahu hello `application/json` uložena a zpracována jako objekt JSON. Navíc můžete obsah JSON analyzovat ve výchozím nastavení bez nutnosti jakékoli přetypování. 

Například může analyzovat požadavek, který obsahuje záhlaví typu obsahu hello `application/json ` v pracovním postupu pomocí výrazu jako `@body('myAction')['foo'][0]` tooget hello hodnota `bar` v tomto případě:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

Je potřeba žádné další přetypování. Při práci s daty, která je JSON, ale neměly hlavičku zadána, můžete ručně vložíte ho tooJSON pomocí hello `@json()` funkce, například: `@json(triggerBody())['foo']`.

### <a name="schema-and-schema-generator"></a>Schéma a schéma generátor

Hello požadavek aktivace vám umožní tooenter schématu JSON pro datové části hello očekáváte, že tooreceive. Toto schéma umožňuje hello Návrhář generování tokenů, můžete využívat obsah hello hello požadavku. Pokud nemáte schéma připraven, vyberte **použití ukázkové datové části toogenerate schématu**, takže může generovat schéma JSON z ukázkové datové části.

![Schéma](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a>Akce, analyzovat JSON.

Hello `Parse JSON` akce umožňuje analyzovat obsah JSON do popisný tokenů pro používání aplikace logiky. Podobné toohello žádost o aktivaci tato akce vám umožní zadat nebo Generovat schéma JSON pro obsah, že který má tooparse hello. Tento nástroj umožňuje využívání data ze služby Service Bus, Azure Cosmos DB a tak dále, mnohem jednodušší.

![Analyzovat JSON](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a>text/plain

Podobně jako příliš`application/json`, zpráv protokolu HTTP přijatých s hello `Content-Type` záhlaví `text/plain` jsou uloženy v základním formátu. Navíc pokud se tyto zprávy jsou zahrnuty v následných akcí bez přetypování, tyto požadavky přejděte s `Content-Type`: `text/plain` záhlaví. Například při práci s plochý soubor, vám může získat tento HTTP obsah jako `text/plain`:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

Pokud v hello další akci, odeslání žádosti o hello jako text hello, jiné žádosti (`@body('flatfile')`), hello žádosti by měla mít `text/plain` hlavičku Content-Type. Při práci s daty, která je prostý text, ale neměly hlavičku zadána, můžete ručně přetypovat tootext hello dat pomocí hello `@string()` funkce, například: `@string(triggerBody())`.

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Application/xml a funkce Application/octet-stream a převaděč

Hello modul aplikace logiky vždy zachovává hello `Content-Type` přijatou v požadavku hello protokolu HTTP nebo odpovědi. Pokud modul hello přijme obsah s hello `Content-Type` z `application/octet-stream`, a uvedete, že obsah v rámci následné akce bez přetypování, hello odchozí žádost obsahuje `Content-Type`: `application/octet-stream`. Tímto způsobem hello modul může zaručit, že data nejsou ztraceny při procházení hello pracovního postupu. Ale hello akce stav (vstupy a výstupy) je uložený v objektu JSON jako hello stavu přesune hello pracovním postupu. Proto toopreserve převede některé typy dat, modul hello hello řetězec s kódováním binární base64 obsahu tooa s příslušnou metadata, která chrání i `$content` a `$content-type`, které jsou automaticky převést. 

* `@json()`-příliš vrhá dat`application/json`
* `@xml()`-příliš vrhá dat`application/xml`
* `@binary()`-příliš vrhá dat`application/octet-stream`
* `@string()`-příliš vrhá dat`text/plain`
* `@base64()`-Převede řetězec base64 obsahu tooa
* `@base64toString()`-příliš převede řetězec s kódováním base64`text/plain`
* `@base64toBinary()`-příliš převede řetězec s kódováním base64`application/octet-stream`
* `@encodeDataUri()`-kóduje řetězce jako bajtové pole dataUri
* `@decodeDataUri()`-dekóduje dataUri do bajtového pole

Například, pokud se vám zobrazila požadavek HTTP s `Content-Type`: `application/xml`:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Může přetypování a pozdější použití se něco podobného jako `@xml(triggerBody())`, nebo ve funkci, jako je `@xpath(xml(triggerBody()), '/CustomerName')`.

## <a name="other-content-types"></a>Jiné typy obsahu

Jiné typy obsahu jsou podporovány a pracovat s logic apps, ale můžou vyžadovat ruční načítání tělo zprávy hello podle dekódování hello `$content`. Předpokládejme například, že spustíte `application/x-www-url-formencoded` požadavku kde `$content` je datová část hello kódovaná jako toopreserve řetězec base64 všechna data:

```
CustomerName=Frank&Address=123+Avenue
```

Protože hello požadavek není ve formátu prostého textu nebo JSON, hello požadavek uložen v akci hello následujícím způsobem:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

V současné době není k dispozici nativní funkci pro data formuláře, tak můžete pořád použít tato data v pracovním postupu ručně přístupu k hello dat pomocí funkce jako `@string(body('formdataAction'))`. Pokud byste chtěli hello odchozí požadavek tooalso mít hello `application/x-www-url-formencoded` záhlaví typu obsahu, můžete přidat hello požadavek toohello textu akce bez jakékoli přetypování jako `@body('formdataAction')`. Však tato metoda funguje jenom v případě textu hello se pouze parametr hello v hello `body` vstupní. Pokud se pokusíte toouse `@body('formdataAction')` v `application/json` požádat, můžete získat Chyba za běhu, protože je odeslán hello kódování textu.

