---
title: "Zpracování typy obsahu – Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: ac67838344bbd10384299c086ff096fbe5dec6a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="handle-content-types-in-logic-apps"></a><span data-ttu-id="83b6e-103">Zpracování typů obsahu v aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="83b6e-103">Handle content types in logic apps</span></span>

<span data-ttu-id="83b6e-104">Mnoho různých typů obsahu můžete procházet skrz aplikace logiky, včetně JSON, XML, ploché soubory a binární data.</span><span class="sxs-lookup"><span data-stu-id="83b6e-104">Many different types of content can flow through a logic app, including JSON, XML, flat files, and binary data.</span></span> <span data-ttu-id="83b6e-105">I když modul logiku aplikace podporuje všechny typy obsahu, některé jsou nativně srozumitelné modul aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="83b6e-105">While the Logic Apps Engine supports all content types, some are natively understood by the Logic Apps Engine.</span></span> <span data-ttu-id="83b6e-106">Ostatní může vyžadovat přetypování nebo převody podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="83b6e-106">Others might require casting or conversions as necessary.</span></span> <span data-ttu-id="83b6e-107">Tento článek popisuje, jak modul zpracuje různých typů obsahu a jak se správně zpracovat tyto typy, pokud je to nezbytné.</span><span class="sxs-lookup"><span data-stu-id="83b6e-107">This article describes how the engine handles different content types and how to correctly handle these types when necessary.</span></span>

## <a name="content-type-header"></a><span data-ttu-id="83b6e-108">Hlavička Content-Type</span><span class="sxs-lookup"><span data-stu-id="83b6e-108">Content-Type Header</span></span>

<span data-ttu-id="83b6e-109">Pokud chcete spustit v podstatě, podíváme se na dvou `Content-Types` nevyžaduje převod nebo přetypování, který můžete použít v aplikaci logiky: `application/json` a `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="83b6e-109">To start basically, let's look at the two `Content-Types` that don't require conversion or casting that you can use in a logic app: `application/json` and `text/plain`.</span></span>

## <a name="applicationjson"></a><span data-ttu-id="83b6e-110">Application/JSON</span><span class="sxs-lookup"><span data-stu-id="83b6e-110">Application/JSON</span></span>

<span data-ttu-id="83b6e-111">Modul pracovních postupů spoléhá na `Content-Type` záhlaví z HTTP volá určit příslušné zpracování.</span><span class="sxs-lookup"><span data-stu-id="83b6e-111">The workflow engine relies on the `Content-Type` header from HTTP calls to determine the appropriate handling.</span></span> <span data-ttu-id="83b6e-112">Každá žádost s typem obsahu `application/json` uložena a zpracována jako objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="83b6e-112">Any request with the content type `application/json` is stored and handled as a JSON Object.</span></span> <span data-ttu-id="83b6e-113">Navíc můžete obsah JSON analyzovat ve výchozím nastavení bez nutnosti jakékoli přetypování.</span><span class="sxs-lookup"><span data-stu-id="83b6e-113">Also, JSON content can be parsed by default without needing any casting.</span></span> 

<span data-ttu-id="83b6e-114">Například může analyzovat požadavek, který obsahuje záhlaví typu obsahu `application/json ` v pracovním postupu pomocí výrazu jako `@body('myAction')['foo'][0]` k získání hodnoty `bar` v tomto případě:</span><span class="sxs-lookup"><span data-stu-id="83b6e-114">For example, you could parse a request that has the content type header `application/json ` in a workflow by using an expression like `@body('myAction')['foo'][0]` to get the value `bar` in this case:</span></span>

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

<span data-ttu-id="83b6e-115">Je potřeba žádné další přetypování.</span><span class="sxs-lookup"><span data-stu-id="83b6e-115">No additional casting is needed.</span></span> <span data-ttu-id="83b6e-116">Při práci s daty, která je JSON, ale neměly hlavičku zadána, můžete ručně obsadit ho pomocí JSON `@json()` funkce, například: `@json(triggerBody())['foo']`.</span><span class="sxs-lookup"><span data-stu-id="83b6e-116">If you are working with data that is JSON but didn't have a header specified, you can manually cast it to JSON using the `@json()` function, for example: `@json(triggerBody())['foo']`.</span></span>

### <a name="schema-and-schema-generator"></a><span data-ttu-id="83b6e-117">Schéma a schéma generátor</span><span class="sxs-lookup"><span data-stu-id="83b6e-117">Schema and schema generator</span></span>

<span data-ttu-id="83b6e-118">Aktivační událost požadavku umožňuje zadejte schéma JSON pro datové části, které chcete dostávat.</span><span class="sxs-lookup"><span data-stu-id="83b6e-118">The Request trigger lets you to enter a JSON schema for the payload you expect to receive.</span></span> <span data-ttu-id="83b6e-119">Toto schéma umožňuje návrháře generování tokenů, můžete využívat obsah žádosti.</span><span class="sxs-lookup"><span data-stu-id="83b6e-119">This schema lets the designer generate tokens so you can consume the content of the request.</span></span> <span data-ttu-id="83b6e-120">Pokud nemáte schéma připraven, vyberte **datová část ukázky použít ke generování schématu**, takže může generovat schéma JSON z ukázkové datové části.</span><span class="sxs-lookup"><span data-stu-id="83b6e-120">If you don't have a schema ready, select **Use sample payload to generate schema**, so you can generate a JSON schema from a sample payload.</span></span>

![Schéma](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a><span data-ttu-id="83b6e-122">Akce, analyzovat JSON.</span><span class="sxs-lookup"><span data-stu-id="83b6e-122">'Parse JSON' action</span></span>

<span data-ttu-id="83b6e-123">`Parse JSON` Akce umožňuje analyzovat obsah JSON do popisný tokenů pro používání aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="83b6e-123">The `Parse JSON` action lets you parse JSON content into friendly tokens for logic app consumption.</span></span> <span data-ttu-id="83b6e-124">Podobně jako na žádost o aktivaci, tato akce vám umožní zadat nebo Generovat schéma JSON pro obsah, který chcete analyzovat.</span><span class="sxs-lookup"><span data-stu-id="83b6e-124">Similar to the Request trigger, this action lets you enter or generate a JSON schema for the content you want to parse.</span></span> <span data-ttu-id="83b6e-125">Tento nástroj umožňuje využívání data ze služby Service Bus, Azure Cosmos DB a tak dále, mnohem jednodušší.</span><span class="sxs-lookup"><span data-stu-id="83b6e-125">This tool makes consuming data from Service Bus, Azure Cosmos DB, and so on, much easier.</span></span>

![Analyzovat JSON](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a><span data-ttu-id="83b6e-127">text/plain</span><span class="sxs-lookup"><span data-stu-id="83b6e-127">Text/plain</span></span>

<span data-ttu-id="83b6e-128">Podobně jako `application/json`, obdrželi s portálem zpráv protokolu HTTP `Content-Type` záhlaví `text/plain` jsou uloženy v základním formátu.</span><span class="sxs-lookup"><span data-stu-id="83b6e-128">Similar to `application/json`, HTTP messages received with the `Content-Type` header of `text/plain` are stored in raw form.</span></span> <span data-ttu-id="83b6e-129">Navíc pokud se tyto zprávy jsou zahrnuty v následných akcí bez přetypování, tyto požadavky přejděte s `Content-Type`: `text/plain` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="83b6e-129">Also, if those messages are included in subsequent actions without casting, those requests go out with  `Content-Type`: `text/plain` header.</span></span> <span data-ttu-id="83b6e-130">Například při práci s plochý soubor, vám může získat tento HTTP obsah jako `text/plain`:</span><span class="sxs-lookup"><span data-stu-id="83b6e-130">For example, when working with a flat file, you might get this HTTP content as `text/plain`:</span></span>

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

<span data-ttu-id="83b6e-131">Pokud v další akci, odešlete žádost jako text jinou žádost (`@body('flatfile')`), žádost by měla `text/plain` hlavičku Content-Type.</span><span class="sxs-lookup"><span data-stu-id="83b6e-131">If in the next action, you send the request as the body of another request (`@body('flatfile')`), the request would have a `text/plain` Content-Type header.</span></span> <span data-ttu-id="83b6e-132">Při práci s daty, která je prostý text, ale neměly hlavičku zadána, můžete ručně přetypovat data pomocí textu `@string()` funkce, například: `@string(triggerBody())`.</span><span class="sxs-lookup"><span data-stu-id="83b6e-132">If you are working with data that is plain text but didn't have a header specified, you can manually cast the data to text using the `@string()` function, for example: `@string(triggerBody())`.</span></span>

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a><span data-ttu-id="83b6e-133">Application/xml a funkce Application/octet-stream a převaděč</span><span class="sxs-lookup"><span data-stu-id="83b6e-133">Application/xml and Application/octet-stream and converter functions</span></span>

<span data-ttu-id="83b6e-134">Modul aplikace logiky vždy zachovává `Content-Type` přijatou v požadavku HTTP nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="83b6e-134">The Logic Apps Engine always preserves the `Content-Type` that was received on the HTTP request or response.</span></span> <span data-ttu-id="83b6e-135">Pokud modul přijímá obsah s `Content-Type` z `application/octet-stream`, a uvedete, že obsah v rámci následné akce bez přetypování, odchozí žádost má `Content-Type`: `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="83b6e-135">So if the engine receives content with the `Content-Type` of `application/octet-stream`, and you include that content in a subsequent action without casting, the outgoing request has `Content-Type`: `application/octet-stream`.</span></span> <span data-ttu-id="83b6e-136">Tímto způsobem modul může zaručit, že data nejsou ztraceny při přesouvání v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="83b6e-136">This way, the engine can guarantee data isn't lost while moving through the workflow.</span></span> <span data-ttu-id="83b6e-137">Však stavu akce (vstupy a výstupy) je uložena v objektu JSON, protože stav prochází přes pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="83b6e-137">However, the action state (inputs and outputs) is stored in a JSON object as the state moves through the workflow.</span></span> <span data-ttu-id="83b6e-138">Takže pokud chcete zachovat některé typy dat, modul převede obsah do binární kódováním base64 řetězec s příslušnou metadata, která chrání i `$content` a `$content-type`, které jsou automaticky převést.</span><span class="sxs-lookup"><span data-stu-id="83b6e-138">So to preserve some data types, the engine converts the content to a binary base64 encoded string with appropriate metadata that preserves both `$content` and `$content-type`, which are automatically be converted.</span></span> 

* <span data-ttu-id="83b6e-139">`@json()`-vrhá dat`application/json`</span><span class="sxs-lookup"><span data-stu-id="83b6e-139">`@json()` - casts data to `application/json`</span></span>
* <span data-ttu-id="83b6e-140">`@xml()`-vrhá dat`application/xml`</span><span class="sxs-lookup"><span data-stu-id="83b6e-140">`@xml()` - casts data to `application/xml`</span></span>
* <span data-ttu-id="83b6e-141">`@binary()`-vrhá dat`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="83b6e-141">`@binary()` - casts data to `application/octet-stream`</span></span>
* <span data-ttu-id="83b6e-142">`@string()`-vrhá dat`text/plain`</span><span class="sxs-lookup"><span data-stu-id="83b6e-142">`@string()` - casts data to `text/plain`</span></span>
* <span data-ttu-id="83b6e-143">`@base64()`-Převede obsah na řetězec ve formátu base64</span><span class="sxs-lookup"><span data-stu-id="83b6e-143">`@base64()` - converts content to a base64 string</span></span>
* <span data-ttu-id="83b6e-144">`@base64toString()`-Převede řetězec s kódováním base64 do`text/plain`</span><span class="sxs-lookup"><span data-stu-id="83b6e-144">`@base64toString()` - converts a base64 encoded string to `text/plain`</span></span>
* <span data-ttu-id="83b6e-145">`@base64toBinary()`-Převede řetězec s kódováním base64 do`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="83b6e-145">`@base64toBinary()` - converts a base64 encoded string to `application/octet-stream`</span></span>
* <span data-ttu-id="83b6e-146">`@encodeDataUri()`-kóduje řetězce jako bajtové pole dataUri</span><span class="sxs-lookup"><span data-stu-id="83b6e-146">`@encodeDataUri()` - encodes a string as a dataUri byte array</span></span>
* <span data-ttu-id="83b6e-147">`@decodeDataUri()`-dekóduje dataUri do bajtového pole</span><span class="sxs-lookup"><span data-stu-id="83b6e-147">`@decodeDataUri()` - decodes a dataUri into a byte array</span></span>

<span data-ttu-id="83b6e-148">Například, pokud se vám zobrazila požadavek HTTP s `Content-Type`: `application/xml`:</span><span class="sxs-lookup"><span data-stu-id="83b6e-148">For example, if you received an HTTP request with `Content-Type`: `application/xml`:</span></span>

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

<span data-ttu-id="83b6e-149">Může přetypování a pozdější použití se něco podobného jako `@xml(triggerBody())`, nebo ve funkci, jako je `@xpath(xml(triggerBody()), '/CustomerName')`.</span><span class="sxs-lookup"><span data-stu-id="83b6e-149">You could cast and use later with something like `@xml(triggerBody())`, or in a function like `@xpath(xml(triggerBody()), '/CustomerName')`.</span></span>

## <a name="other-content-types"></a><span data-ttu-id="83b6e-150">Jiné typy obsahu</span><span class="sxs-lookup"><span data-stu-id="83b6e-150">Other content types</span></span>

<span data-ttu-id="83b6e-151">Jiné typy obsahu jsou podporovány a pracovat s logic apps, ale můžou vyžadovat ruční načítání textu zprávy podle dekódování `$content`.</span><span class="sxs-lookup"><span data-stu-id="83b6e-151">Other content types are supported and work with logic apps, but might require manually retrieving the message body by decoding the `$content`.</span></span> <span data-ttu-id="83b6e-152">Předpokládejme například, že spustíte `application/x-www-url-formencoded` požadavku kde `$content` je datová část kódovaný jako base64 řetězec pro zachování všech dat:</span><span class="sxs-lookup"><span data-stu-id="83b6e-152">For example, suppose you trigger an `application/x-www-url-formencoded` request where `$content` is the payload encoded as a base64 string to preserve all data:</span></span>

```
CustomerName=Frank&Address=123+Avenue
```

<span data-ttu-id="83b6e-153">Vzhledem k tomu, že daný požadavek není ve formátu prostého textu nebo JSON, se v akci požadavku ukládají následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="83b6e-153">Because the request isn't plain text or JSON, the request is stored in the action as follows:</span></span>

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

V současné době není k dispozici nativní funkci pro data formuláře, tak můžete pořád použít tato data v pracovním postupu ručně přístupu k dat pomocí funkce jako `@string(body('formdataAction'))`. Pokud byste chtěli odchozí požadavek také mít `application/x-www-url-formencoded` záhlaví typu obsahu, můžete přidat požadavku k tělu akce bez jakékoli přetypování jako `@body('formdataAction')`. Však tato metoda funguje jenom v případě je jediný parametr v těle `body` vstupní. <span data-ttu-id="83b6e-157">Pokud se pokusíte použít `@body('formdataAction')` v `application/json` požádat, můžete získat Chyba za běhu, protože je odeslán kódovaného textu.</span><span class="sxs-lookup"><span data-stu-id="83b6e-157">If you try to use `@body('formdataAction')` in an `application/json` request, you get a runtime error because the encoded body is sent.</span></span>

