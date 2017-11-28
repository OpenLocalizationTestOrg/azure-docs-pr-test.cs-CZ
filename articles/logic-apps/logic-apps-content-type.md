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
# <a name="handle-content-types-in-logic-apps"></a><span data-ttu-id="60ae8-103">Zpracování typů obsahu v aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="60ae8-103">Handle content types in logic apps</span></span>

<span data-ttu-id="60ae8-104">Mnoho různých typů obsahu můžete procházet skrz aplikace logiky, včetně JSON, XML, ploché soubory a binární data.</span><span class="sxs-lookup"><span data-stu-id="60ae8-104">Many different types of content can flow through a logic app, including JSON, XML, flat files, and binary data.</span></span> <span data-ttu-id="60ae8-105">I když hello logiku aplikace modul podporuje všechny typy obsahu, některé jsou nativně srozumitelné hello modul aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="60ae8-105">While hello Logic Apps Engine supports all content types, some are natively understood by hello Logic Apps Engine.</span></span> <span data-ttu-id="60ae8-106">Ostatní může vyžadovat přetypování nebo převody podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="60ae8-106">Others might require casting or conversions as necessary.</span></span> <span data-ttu-id="60ae8-107">Tento článek popisuje, jak hello modul zpracuje různých typů obsahu a jak toocorrectly zpracování těchto typů, pokud je to nezbytné.</span><span class="sxs-lookup"><span data-stu-id="60ae8-107">This article describes how hello engine handles different content types and how toocorrectly handle these types when necessary.</span></span>

## <a name="content-type-header"></a><span data-ttu-id="60ae8-108">Hlavička Content-Type</span><span class="sxs-lookup"><span data-stu-id="60ae8-108">Content-Type Header</span></span>

<span data-ttu-id="60ae8-109">toostart v podstatě, podíváme se na hello dva `Content-Types` nevyžaduje převod nebo přetypování, který můžete použít v aplikaci logiky: `application/json` a `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="60ae8-109">toostart basically, let's look at hello two `Content-Types` that don't require conversion or casting that you can use in a logic app: `application/json` and `text/plain`.</span></span>

## <a name="applicationjson"></a><span data-ttu-id="60ae8-110">Application/JSON</span><span class="sxs-lookup"><span data-stu-id="60ae8-110">Application/JSON</span></span>

<span data-ttu-id="60ae8-111">modul pracovních postupů Hello spoléhá na hello `Content-Type` záhlaví z HTTP volá toodetermine hello příslušné zpracování.</span><span class="sxs-lookup"><span data-stu-id="60ae8-111">hello workflow engine relies on hello `Content-Type` header from HTTP calls toodetermine hello appropriate handling.</span></span> <span data-ttu-id="60ae8-112">Každá žádost s typem obsahu hello `application/json` uložena a zpracována jako objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="60ae8-112">Any request with hello content type `application/json` is stored and handled as a JSON Object.</span></span> <span data-ttu-id="60ae8-113">Navíc můžete obsah JSON analyzovat ve výchozím nastavení bez nutnosti jakékoli přetypování.</span><span class="sxs-lookup"><span data-stu-id="60ae8-113">Also, JSON content can be parsed by default without needing any casting.</span></span> 

<span data-ttu-id="60ae8-114">Například může analyzovat požadavek, který obsahuje záhlaví typu obsahu hello `application/json ` v pracovním postupu pomocí výrazu jako `@body('myAction')['foo'][0]` tooget hello hodnota `bar` v tomto případě:</span><span class="sxs-lookup"><span data-stu-id="60ae8-114">For example, you could parse a request that has hello content type header `application/json ` in a workflow by using an expression like `@body('myAction')['foo'][0]` tooget hello value `bar` in this case:</span></span>

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

<span data-ttu-id="60ae8-115">Je potřeba žádné další přetypování.</span><span class="sxs-lookup"><span data-stu-id="60ae8-115">No additional casting is needed.</span></span> <span data-ttu-id="60ae8-116">Při práci s daty, která je JSON, ale neměly hlavičku zadána, můžete ručně vložíte ho tooJSON pomocí hello `@json()` funkce, například: `@json(triggerBody())['foo']`.</span><span class="sxs-lookup"><span data-stu-id="60ae8-116">If you are working with data that is JSON but didn't have a header specified, you can manually cast it tooJSON using hello `@json()` function, for example: `@json(triggerBody())['foo']`.</span></span>

### <a name="schema-and-schema-generator"></a><span data-ttu-id="60ae8-117">Schéma a schéma generátor</span><span class="sxs-lookup"><span data-stu-id="60ae8-117">Schema and schema generator</span></span>

<span data-ttu-id="60ae8-118">Hello požadavek aktivace vám umožní tooenter schématu JSON pro datové části hello očekáváte, že tooreceive.</span><span class="sxs-lookup"><span data-stu-id="60ae8-118">hello Request trigger lets you tooenter a JSON schema for hello payload you expect tooreceive.</span></span> <span data-ttu-id="60ae8-119">Toto schéma umožňuje hello Návrhář generování tokenů, můžete využívat obsah hello hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="60ae8-119">This schema lets hello designer generate tokens so you can consume hello content of hello request.</span></span> <span data-ttu-id="60ae8-120">Pokud nemáte schéma připraven, vyberte **použití ukázkové datové části toogenerate schématu**, takže může generovat schéma JSON z ukázkové datové části.</span><span class="sxs-lookup"><span data-stu-id="60ae8-120">If you don't have a schema ready, select **Use sample payload toogenerate schema**, so you can generate a JSON schema from a sample payload.</span></span>

![Schéma](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a><span data-ttu-id="60ae8-122">Akce, analyzovat JSON.</span><span class="sxs-lookup"><span data-stu-id="60ae8-122">'Parse JSON' action</span></span>

<span data-ttu-id="60ae8-123">Hello `Parse JSON` akce umožňuje analyzovat obsah JSON do popisný tokenů pro používání aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="60ae8-123">hello `Parse JSON` action lets you parse JSON content into friendly tokens for logic app consumption.</span></span> <span data-ttu-id="60ae8-124">Podobné toohello žádost o aktivaci tato akce vám umožní zadat nebo Generovat schéma JSON pro obsah, že který má tooparse hello.</span><span class="sxs-lookup"><span data-stu-id="60ae8-124">Similar toohello Request trigger, this action lets you enter or generate a JSON schema for hello content you want tooparse.</span></span> <span data-ttu-id="60ae8-125">Tento nástroj umožňuje využívání data ze služby Service Bus, Azure Cosmos DB a tak dále, mnohem jednodušší.</span><span class="sxs-lookup"><span data-stu-id="60ae8-125">This tool makes consuming data from Service Bus, Azure Cosmos DB, and so on, much easier.</span></span>

![Analyzovat JSON](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a><span data-ttu-id="60ae8-127">text/plain</span><span class="sxs-lookup"><span data-stu-id="60ae8-127">Text/plain</span></span>

<span data-ttu-id="60ae8-128">Podobně jako příliš`application/json`, zpráv protokolu HTTP přijatých s hello `Content-Type` záhlaví `text/plain` jsou uloženy v základním formátu.</span><span class="sxs-lookup"><span data-stu-id="60ae8-128">Similar too`application/json`, HTTP messages received with hello `Content-Type` header of `text/plain` are stored in raw form.</span></span> <span data-ttu-id="60ae8-129">Navíc pokud se tyto zprávy jsou zahrnuty v následných akcí bez přetypování, tyto požadavky přejděte s `Content-Type`: `text/plain` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="60ae8-129">Also, if those messages are included in subsequent actions without casting, those requests go out with  `Content-Type`: `text/plain` header.</span></span> <span data-ttu-id="60ae8-130">Například při práci s plochý soubor, vám může získat tento HTTP obsah jako `text/plain`:</span><span class="sxs-lookup"><span data-stu-id="60ae8-130">For example, when working with a flat file, you might get this HTTP content as `text/plain`:</span></span>

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

<span data-ttu-id="60ae8-131">Pokud v hello další akci, odeslání žádosti o hello jako text hello, jiné žádosti (`@body('flatfile')`), hello žádosti by měla mít `text/plain` hlavičku Content-Type.</span><span class="sxs-lookup"><span data-stu-id="60ae8-131">If in hello next action, you send hello request as hello body of another request (`@body('flatfile')`), hello request would have a `text/plain` Content-Type header.</span></span> <span data-ttu-id="60ae8-132">Při práci s daty, která je prostý text, ale neměly hlavičku zadána, můžete ručně přetypovat tootext hello dat pomocí hello `@string()` funkce, například: `@string(triggerBody())`.</span><span class="sxs-lookup"><span data-stu-id="60ae8-132">If you are working with data that is plain text but didn't have a header specified, you can manually cast hello data tootext using hello `@string()` function, for example: `@string(triggerBody())`.</span></span>

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a><span data-ttu-id="60ae8-133">Application/xml a funkce Application/octet-stream a převaděč</span><span class="sxs-lookup"><span data-stu-id="60ae8-133">Application/xml and Application/octet-stream and converter functions</span></span>

<span data-ttu-id="60ae8-134">Hello modul aplikace logiky vždy zachovává hello `Content-Type` přijatou v požadavku hello protokolu HTTP nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="60ae8-134">hello Logic Apps Engine always preserves hello `Content-Type` that was received on hello HTTP request or response.</span></span> <span data-ttu-id="60ae8-135">Pokud modul hello přijme obsah s hello `Content-Type` z `application/octet-stream`, a uvedete, že obsah v rámci následné akce bez přetypování, hello odchozí žádost obsahuje `Content-Type`: `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="60ae8-135">So if hello engine receives content with hello `Content-Type` of `application/octet-stream`, and you include that content in a subsequent action without casting, hello outgoing request has `Content-Type`: `application/octet-stream`.</span></span> <span data-ttu-id="60ae8-136">Tímto způsobem hello modul může zaručit, že data nejsou ztraceny při procházení hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="60ae8-136">This way, hello engine can guarantee data isn't lost while moving through hello workflow.</span></span> <span data-ttu-id="60ae8-137">Ale hello akce stav (vstupy a výstupy) je uložený v objektu JSON jako hello stavu přesune hello pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="60ae8-137">However, hello action state (inputs and outputs) is stored in a JSON object as hello state moves through hello workflow.</span></span> <span data-ttu-id="60ae8-138">Proto toopreserve převede některé typy dat, modul hello hello řetězec s kódováním binární base64 obsahu tooa s příslušnou metadata, která chrání i `$content` a `$content-type`, které jsou automaticky převést.</span><span class="sxs-lookup"><span data-stu-id="60ae8-138">So toopreserve some data types, hello engine converts hello content tooa binary base64 encoded string with appropriate metadata that preserves both `$content` and `$content-type`, which are automatically be converted.</span></span> 

* <span data-ttu-id="60ae8-139">`@json()`-příliš vrhá dat`application/json`</span><span class="sxs-lookup"><span data-stu-id="60ae8-139">`@json()` - casts data too`application/json`</span></span>
* <span data-ttu-id="60ae8-140">`@xml()`-příliš vrhá dat`application/xml`</span><span class="sxs-lookup"><span data-stu-id="60ae8-140">`@xml()` - casts data too`application/xml`</span></span>
* <span data-ttu-id="60ae8-141">`@binary()`-příliš vrhá dat`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="60ae8-141">`@binary()` - casts data too`application/octet-stream`</span></span>
* <span data-ttu-id="60ae8-142">`@string()`-příliš vrhá dat`text/plain`</span><span class="sxs-lookup"><span data-stu-id="60ae8-142">`@string()` - casts data too`text/plain`</span></span>
* <span data-ttu-id="60ae8-143">`@base64()`-Převede řetězec base64 obsahu tooa</span><span class="sxs-lookup"><span data-stu-id="60ae8-143">`@base64()` - converts content tooa base64 string</span></span>
* <span data-ttu-id="60ae8-144">`@base64toString()`-příliš převede řetězec s kódováním base64`text/plain`</span><span class="sxs-lookup"><span data-stu-id="60ae8-144">`@base64toString()` - converts a base64 encoded string too`text/plain`</span></span>
* <span data-ttu-id="60ae8-145">`@base64toBinary()`-příliš převede řetězec s kódováním base64`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="60ae8-145">`@base64toBinary()` - converts a base64 encoded string too`application/octet-stream`</span></span>
* <span data-ttu-id="60ae8-146">`@encodeDataUri()`-kóduje řetězce jako bajtové pole dataUri</span><span class="sxs-lookup"><span data-stu-id="60ae8-146">`@encodeDataUri()` - encodes a string as a dataUri byte array</span></span>
* <span data-ttu-id="60ae8-147">`@decodeDataUri()`-dekóduje dataUri do bajtového pole</span><span class="sxs-lookup"><span data-stu-id="60ae8-147">`@decodeDataUri()` - decodes a dataUri into a byte array</span></span>

<span data-ttu-id="60ae8-148">Například, pokud se vám zobrazila požadavek HTTP s `Content-Type`: `application/xml`:</span><span class="sxs-lookup"><span data-stu-id="60ae8-148">For example, if you received an HTTP request with `Content-Type`: `application/xml`:</span></span>

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

<span data-ttu-id="60ae8-149">Může přetypování a pozdější použití se něco podobného jako `@xml(triggerBody())`, nebo ve funkci, jako je `@xpath(xml(triggerBody()), '/CustomerName')`.</span><span class="sxs-lookup"><span data-stu-id="60ae8-149">You could cast and use later with something like `@xml(triggerBody())`, or in a function like `@xpath(xml(triggerBody()), '/CustomerName')`.</span></span>

## <a name="other-content-types"></a><span data-ttu-id="60ae8-150">Jiné typy obsahu</span><span class="sxs-lookup"><span data-stu-id="60ae8-150">Other content types</span></span>

<span data-ttu-id="60ae8-151">Jiné typy obsahu jsou podporovány a pracovat s logic apps, ale můžou vyžadovat ruční načítání tělo zprávy hello podle dekódování hello `$content`.</span><span class="sxs-lookup"><span data-stu-id="60ae8-151">Other content types are supported and work with logic apps, but might require manually retrieving hello message body by decoding hello `$content`.</span></span> <span data-ttu-id="60ae8-152">Předpokládejme například, že spustíte `application/x-www-url-formencoded` požadavku kde `$content` je datová část hello kódovaná jako toopreserve řetězec base64 všechna data:</span><span class="sxs-lookup"><span data-stu-id="60ae8-152">For example, suppose you trigger an `application/x-www-url-formencoded` request where `$content` is hello payload encoded as a base64 string toopreserve all data:</span></span>

```
CustomerName=Frank&Address=123+Avenue
```

<span data-ttu-id="60ae8-153">Protože hello požadavek není ve formátu prostého textu nebo JSON, hello požadavek uložen v akci hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="60ae8-153">Because hello request isn't plain text or JSON, hello request is stored in hello action as follows:</span></span>

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

V současné době není k dispozici nativní funkci pro data formuláře, tak můžete pořád použít tato data v pracovním postupu ručně přístupu k hello dat pomocí funkce jako `@string(body('formdataAction'))`. Pokud byste chtěli hello odchozí požadavek tooalso mít hello `application/x-www-url-formencoded` záhlaví typu obsahu, můžete přidat hello požadavek toohello textu akce bez jakékoli přetypování jako `@body('formdataAction')`. Však tato metoda funguje jenom v případě textu hello se pouze parametr hello v hello `body` vstupní. <span data-ttu-id="60ae8-157">Pokud se pokusíte toouse `@body('formdataAction')` v `application/json` požádat, můžete získat Chyba za běhu, protože je odeslán hello kódování textu.</span><span class="sxs-lookup"><span data-stu-id="60ae8-157">If you try toouse `@body('formdataAction')` in an `application/json` request, you get a runtime error because hello encoded body is sent.</span></span>

