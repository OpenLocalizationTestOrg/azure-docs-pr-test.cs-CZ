---
title: "Přehled rozhraní API Azure předávání uzlu | Microsoft Docs"
description: "Předávání přehled API uzlu"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b7d6e822-7c32-4cb5-a4b8-df7d009bdc85
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: 28526c05c7f364f0fcaaa362fc97857f850040ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="relay-hybrid-connections-node-api-overview"></a><span data-ttu-id="97a27-103">Předávání API uzlu hybridní připojení – přehled</span><span class="sxs-lookup"><span data-stu-id="97a27-103">Relay Hybrid Connections Node API overview</span></span>

## <a name="overview"></a><span data-ttu-id="97a27-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="97a27-104">Overview</span></span>

<span data-ttu-id="97a27-105">[ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) Balíček uzel pro hybridní připojení předávání přes Azure je založená na a rozšiřuje ['ws'](https://www.npmjs.com/package/ws) balíčku NPM.</span><span class="sxs-lookup"><span data-stu-id="97a27-105">The [`hyco-ws`](https://www.npmjs.com/package/hyco-ws) Node package for Azure Relay Hybrid Connections is built on and extends the ['ws'](https://www.npmjs.com/package/ws) NPM package.</span></span> <span data-ttu-id="97a27-106">Tento balíček znovu vyexportuje všechny exporty tento základní balíček a přidá nové export, které umožňují integrace s funkcí hybridní připojení služby předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="97a27-106">This package re-exports all exports of that base package and adds new exports that enable integration with the Azure Relay service Hybrid Connections feature.</span></span> 

<span data-ttu-id="97a27-107">Existující aplikace, `require('ws')` můžete použít tento balíček s `require('hyco-ws')` místo toho, která umožňuje také hybridní scénáře, ve kterých můžete aplikaci přijímat připojení protokolu WebSocket místně z "v bráně firewall" a pomocí hybridních připojení, všechny ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="97a27-107">Existing applications that `require('ws')` can use this package with `require('hyco-ws')` instead, which also enables hybrid scenarios in which an application can listen for WebSocket connections locally from "inside the firewall" and via Hybrid Connections, all at the same time.</span></span>
  
## <a name="documentation"></a><span data-ttu-id="97a27-108">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="97a27-108">Documentation</span></span>

<span data-ttu-id="97a27-109">Rozhraní API jsou [zdokumentované v balíčku hlavní 'ws'](https://github.com/websockets/ws/blob/master/doc/ws.md).</span><span class="sxs-lookup"><span data-stu-id="97a27-109">The APIs are [documented in the main 'ws' package](https://github.com/websockets/ws/blob/master/doc/ws.md).</span></span> <span data-ttu-id="97a27-110">Tento článek popisuje, jak tento balíček se liší od tohoto směrného plánu.</span><span class="sxs-lookup"><span data-stu-id="97a27-110">This article describes how this package differs from that baseline.</span></span> 

<span data-ttu-id="97a27-111">Je klíčové rozdíly mezi základní balíček a tato 'hyco-ws' Přidá novou třídu serveru exportovat prostřednictvím `require('hyco-ws').RelayedServer`a několik pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="97a27-111">The key differences between the base package and this 'hyco-ws' is that it adds a new server class, exported via `require('hyco-ws').RelayedServer`, and a few helper methods.</span></span>

### <a name="package-helper-methods"></a><span data-ttu-id="97a27-112">Balíček pomocné metody</span><span class="sxs-lookup"><span data-stu-id="97a27-112">Package helper methods</span></span>

<span data-ttu-id="97a27-113">Na exportu balíček, na kterou odkazujete takto jsou k dispozici několik pomocné metody:</span><span class="sxs-lookup"><span data-stu-id="97a27-113">There are several utility methods available on the package export that you can reference as follows:</span></span>

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

<span data-ttu-id="97a27-114">Pomocné metody pro použití s tímto balíčkem jsou, ale mohou sloužit také serverem uzel pro povolení vytvořit naslouchací procesy nebo odesílatelé web nebo zařízení klientům.</span><span class="sxs-lookup"><span data-stu-id="97a27-114">The helper methods are for use with this package, but can also be used by a Node server for enabling web or device clients to create listeners or senders.</span></span> <span data-ttu-id="97a27-115">Server používá tyto metody předáním identifikátory URI, který vložení krátkodobou tokeny.</span><span class="sxs-lookup"><span data-stu-id="97a27-115">The server uses these methods by passing them URIs that embed short-lived tokens.</span></span> <span data-ttu-id="97a27-116">Tyto identifikátory URI můžete použít taky s běžné zásobníky protokolu WebSocket, které nepodporují nastavení hlavičky protokolu HTTP pro handshake protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="97a27-116">These URIs can also be used with common WebSocket stacks that do not support setting HTTP headers for the WebSocket handshake.</span></span> <span data-ttu-id="97a27-117">Vložení do identifikátor URI autorizace tokeny je podporována především pro tyto scénáře použití knihovny externí.</span><span class="sxs-lookup"><span data-stu-id="97a27-117">Embedding authorization tokens into the URI is supported primarily for those library-external usage scenarios.</span></span> 

#### <a name="createrelaylistenuri"></a><span data-ttu-id="97a27-118">createRelayListenUri</span><span class="sxs-lookup"><span data-stu-id="97a27-118">createRelayListenUri</span></span>

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="97a27-119">Vytvoří platný Azure předávání hybridní připojení naslouchací proces identifikátor URI pro daný obor názvů a cestu.</span><span class="sxs-lookup"><span data-stu-id="97a27-119">Creates a valid Azure Relay Hybrid Connection listener URI for the given namespace and path.</span></span> <span data-ttu-id="97a27-120">Tento identifikátor URI pak lze verzí předávání WebSocketServer třídy.</span><span class="sxs-lookup"><span data-stu-id="97a27-120">This URI can then be used with the relay version of the WebSocketServer class.</span></span>

- <span data-ttu-id="97a27-121">`namespaceName`(požadovaná) domény kvalifikovaný název oboru názvů předávání přes Azure používat.</span><span class="sxs-lookup"><span data-stu-id="97a27-121">`namespaceName` (required) - the domain-qualified name of the Azure Relay namespace to use.</span></span>
- <span data-ttu-id="97a27-122">`path`(požadovaná) název existující připojení hybridní předávání přes Azure v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="97a27-122">`path` (required) - the name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="97a27-123">`token`(volitelné) – pro přístup k dřív vydaných předávání token, který je součástí naslouchací proces identifikátor URI (viz následující příklad).</span><span class="sxs-lookup"><span data-stu-id="97a27-123">`token` (optional) - a previously issued Relay access token that is embedded in the listener URI (see the following example).</span></span>
- <span data-ttu-id="97a27-124">`id`(volitelné) – sledování identifikátor, který umožňuje sledování začátku do konce diagnostiky požadavků.</span><span class="sxs-lookup"><span data-stu-id="97a27-124">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="97a27-125">`token` Hodnota je volitelná a musí být použit pouze pokud není možné k odeslání hlaviček protokolu HTTP společně s handshake protokolu WebSocket, jako je tomu u zásobník protokolu W3C WebSocket.</span><span class="sxs-lookup"><span data-stu-id="97a27-125">The `token` value is optional and should only be used when it is not possible to send HTTP headers along with the WebSocket handshake, as is the case with the W3C WebSocket stack.</span></span>                  


#### <a name="createrelaysenduri"></a><span data-ttu-id="97a27-126">createRelaySendUri</span><span class="sxs-lookup"><span data-stu-id="97a27-126">createRelaySendUri</span></span>
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="97a27-127">Vytvoří platný odesílání Azure předávání hybridní připojení identifikátor URI pro daný obor názvů a cestu.</span><span class="sxs-lookup"><span data-stu-id="97a27-127">Creates a valid Azure Relay Hybrid Connection send URI for the given namespace and path.</span></span> <span data-ttu-id="97a27-128">Tento identifikátor URI lze použít s jakýmkoli klientem protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="97a27-128">This URI can be used with any WebSocket client.</span></span>

- <span data-ttu-id="97a27-129">`namespaceName`(požadovaná) domény kvalifikovaný název oboru názvů předávání přes Azure používat.</span><span class="sxs-lookup"><span data-stu-id="97a27-129">`namespaceName` (required) - the domain-qualified name of the Azure Relay namespace to use.</span></span>
- <span data-ttu-id="97a27-130">`path`(požadovaná) název existující připojení hybridní předávání přes Azure v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="97a27-130">`path` (required) - the name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="97a27-131">`token`(volitelné) – pro přístup k dřív vydaných předávání token, který se vloží v odesílání identifikátor URI (viz následující příklad).</span><span class="sxs-lookup"><span data-stu-id="97a27-131">`token` (optional) - a previously issued Relay access token that is embedded in the send URI (see the following example).</span></span>
- <span data-ttu-id="97a27-132">`id`(volitelné) – sledování identifikátor, který umožňuje sledování začátku do konce diagnostiky požadavků.</span><span class="sxs-lookup"><span data-stu-id="97a27-132">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="97a27-133">`token` Hodnota je volitelná a musí být použit pouze pokud není možné k odeslání hlaviček protokolu HTTP společně s handshake protokolu WebSocket, jako je tomu u zásobník protokolu W3C WebSocket.</span><span class="sxs-lookup"><span data-stu-id="97a27-133">The `token` value is optional and should only be used when it is not possible to send HTTP headers along with the WebSocket handshake, as is the case with the W3C WebSocket stack.</span></span>                   


#### <a name="createrelaytoken"></a><span data-ttu-id="97a27-134">createRelayToken</span><span class="sxs-lookup"><span data-stu-id="97a27-134">createRelayToken</span></span> 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="97a27-135">Vytvoří token Azure předávání sdíleného přístupového podpisu (SAS) pro daný cílový identifikátor URI, pravidlo SAS a pravidlo klíče SAS, který je platný pro zadaný počet sekund nebo jednu hodinu z aktuální Pokud argument vypršení platnosti je vynechán.</span><span class="sxs-lookup"><span data-stu-id="97a27-135">Creates an Azure Relay Shared Access Signature (SAS) token for the given target URI, SAS rule, and SAS rule key that is valid for the given number of seconds or for an hour from the current instant if the expiry argument is omitted.</span></span>

- <span data-ttu-id="97a27-136">`uri`(povinné) - URI, pro kterou má být vydaný token.</span><span class="sxs-lookup"><span data-stu-id="97a27-136">`uri` (required) - the URI for which the token is to be issued.</span></span> <span data-ttu-id="97a27-137">Identifikátor URI je normalizovány na použít schéma HTTP a informací řetězce dotazu se odstraní.</span><span class="sxs-lookup"><span data-stu-id="97a27-137">The URI is normalized to use the HTTP scheme, and query string information is stripped.</span></span>
- <span data-ttu-id="97a27-138">`ruleName`(povinné) – název pro entitu reprezentovanou objektem daný identifikátor URI, nebo pro obor názvů reprezentována část hostitele identifikátor URI SAS pravidla.</span><span class="sxs-lookup"><span data-stu-id="97a27-138">`ruleName` (required) - SAS rule name for either the entity represented by the given URI, or for the namespace represented by the URI host portion.</span></span>
- <span data-ttu-id="97a27-139">`key`(povinné) - platný klíč SAS pravidla.</span><span class="sxs-lookup"><span data-stu-id="97a27-139">`key` (required) - valid key for the SAS rule.</span></span> 
- <span data-ttu-id="97a27-140">`expirationSeconds`(volitelné) – počet sekund, dokud by měla vypršet vygenerovaný token.</span><span class="sxs-lookup"><span data-stu-id="97a27-140">`expirationSeconds` (optional) - the number of seconds until the generated token should expire.</span></span> <span data-ttu-id="97a27-141">Pokud není zadáno, výchozí hodnota je 1 hodina (3600).</span><span class="sxs-lookup"><span data-stu-id="97a27-141">If not specified, the default is 1 hour (3600).</span></span>

<span data-ttu-id="97a27-142">Vystavený token uděluje práv spojených se zadaným pravidlem SAS pro danou dobu trvání.</span><span class="sxs-lookup"><span data-stu-id="97a27-142">The issued token confers the rights associated with the specified SAS rule for the given duration.</span></span>

#### <a name="appendrelaytoken"></a><span data-ttu-id="97a27-143">appendRelayToken</span><span class="sxs-lookup"><span data-stu-id="97a27-143">appendRelayToken</span></span>

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="97a27-144">Tato metoda je funkčně srovnatelný `createRelayToken` metoda dříve, ale vrátí token správně připojí k identifikátoru URI vstupu.</span><span class="sxs-lookup"><span data-stu-id="97a27-144">This method is functionally equivalent to the `createRelayToken` method documented previously, but returns the token correctly appended to the input URI.</span></span>

### <a name="class-wsrelayedserver"></a><span data-ttu-id="97a27-145">Třída ws. RelayedServer</span><span class="sxs-lookup"><span data-stu-id="97a27-145">Class ws.RelayedServer</span></span>

<span data-ttu-id="97a27-146">`hycows.RelayedServer` Třída je alternativa k `ws.Server` třídu, která komunikovat v místní síti, ale delegáti naslouchání službu předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="97a27-146">The `hycows.RelayedServer` class is an alternative to the `ws.Server` class that does not listen on the local network, but delegates listening to the Azure Relay service.</span></span>

<span data-ttu-id="97a27-147">Dvě třídy jsou většinou kontrakt kompatibilní, znamená to, že existující aplikace pomocí `ws.Server` třídy lze snadno změnit na použití přenosu verze.</span><span class="sxs-lookup"><span data-stu-id="97a27-147">The two classes are mostly contract compatible, meaning that an existing application using the `ws.Server` class can easily be changed to use the relayed version.</span></span> <span data-ttu-id="97a27-148">Hlavní rozdíly jsou v konstruktoru a dostupných možností.</span><span class="sxs-lookup"><span data-stu-id="97a27-148">The main differences are in the constructor and in the available options.</span></span>

#### <a name="constructor"></a><span data-ttu-id="97a27-149">Konstruktor</span><span class="sxs-lookup"><span data-stu-id="97a27-149">Constructor</span></span>  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

<span data-ttu-id="97a27-150">`RelayedServer` Konstruktor podporuje jinou sadu argumentů než `Server`, protože se nejedná o samostatnou naslouchací proces, nebo je možné jej vkládat do představuje existující rozhraní naslouchací proces protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="97a27-150">The `RelayedServer` constructor supports a different set of arguments than the `Server`, because it is not a standalone listener, or able to be embedded into an existing HTTP listener framework.</span></span> <span data-ttu-id="97a27-151">Existují také méně možností k dispozici vzhledem k tomu, že do předávací služby je z velké části delegování správy protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="97a27-151">There are also fewer options available since the WebSocket management is largely delegated to the Relay service.</span></span>

<span data-ttu-id="97a27-152">Argumenty konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="97a27-152">Constructor arguments:</span></span>

- <span data-ttu-id="97a27-153">`server`(povinné) - plně kvalifikovaného identifikátoru URI pro hybridní připojení název, na kterém se má naslouchat, obvykle zkonstruovat s pomocnou metodu WebSocket.createRelayListenUri().</span><span class="sxs-lookup"><span data-stu-id="97a27-153">`server` (required) - the fully qualified URI for a Hybrid Connection name on which to listen, usually constructed with the WebSocket.createRelayListenUri() helper method.</span></span>
- <span data-ttu-id="97a27-154">`token`Tento argument (povinné) - obsahuje řetězec tokenu dřív vydaných nebo funkce zpětného volání, která může být volána k získání tokenu řetězce.</span><span class="sxs-lookup"><span data-stu-id="97a27-154">`token` (required) - this argument holds either a previously issued token string or a callback function that can be called to obtain such a token string.</span></span> <span data-ttu-id="97a27-155">Na možnost je upřednostňované, protože umožňuje obnovení tokenu.</span><span class="sxs-lookup"><span data-stu-id="97a27-155">The callback option is preferred, as it enables token renewal.</span></span>

#### <a name="events"></a><span data-ttu-id="97a27-156">Události</span><span class="sxs-lookup"><span data-stu-id="97a27-156">Events</span></span>

<span data-ttu-id="97a27-157">`RelayedServer`instance emitování tři události, které vám umožní zpracovávat příchozí požadavky, vytvořit připojení a zjistit chybové stavy.</span><span class="sxs-lookup"><span data-stu-id="97a27-157">`RelayedServer` instances emit three events that enable you to handle incoming requests, establish connections, and detect error conditions.</span></span> <span data-ttu-id="97a27-158">Přihlášení k odběru `connect` událostí pro zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="97a27-158">You must subscribe to the `connect` event to handle messages.</span></span> 

##### <a name="headers"></a><span data-ttu-id="97a27-159">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="97a27-159">headers</span></span>

```JavaScript 
function(headers)
```

<span data-ttu-id="97a27-160">`headers` Událost se vyvolá, těsně před přijmout příchozí připojení, povolení úprav hlavičky k odeslání do klienta.</span><span class="sxs-lookup"><span data-stu-id="97a27-160">The `headers` event is raised just before an incoming connection is accepted, enabling modification of the headers to send to the client.</span></span> 

##### <a name="connection"></a><span data-ttu-id="97a27-161">připojení</span><span class="sxs-lookup"><span data-stu-id="97a27-161">connection</span></span>

```JavaScript
function(socket)
```

<span data-ttu-id="97a27-162">Vygenerované při byla přijata nové připojení protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="97a27-162">Emitted when a new WebSocket connection is accepted.</span></span> <span data-ttu-id="97a27-163">Objekt je typu `ws.WebSocket`, stejně jako s základní balíčku.</span><span class="sxs-lookup"><span data-stu-id="97a27-163">The object is of type `ws.WebSocket`, same as with the base package.</span></span>


##### <a name="error"></a><span data-ttu-id="97a27-164">error</span><span class="sxs-lookup"><span data-stu-id="97a27-164">error</span></span>

```JavaScript
function(error)
```

<span data-ttu-id="97a27-165">Pokud základní server vysílá chybu, je zde předají.</span><span class="sxs-lookup"><span data-stu-id="97a27-165">If the underlying server emits an error, it is forwarded here.</span></span>  

#### <a name="helpers"></a><span data-ttu-id="97a27-166">Pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="97a27-166">Helpers</span></span>

<span data-ttu-id="97a27-167">Pro zjednodušení spuštění přenosu serveru a okamžitě se přihlásíte k odběru příchozí připojení, zpřístupní balíček jednoduché podpůrná funkce, která je použita také v příkladech následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="97a27-167">To simplify starting a relayed server and immediately subscribing to incoming connections, the package exposes a simple helper function, which is also used in the examples, as follows:</span></span>

##### <a name="createrelayedlistener"></a><span data-ttu-id="97a27-168">createRelayedListener</span><span class="sxs-lookup"><span data-stu-id="97a27-168">createRelayedListener</span></span>

```JavaScript
var WebSocket = require('hyco-ws');

var wss = WebSocket.createRelayedServer(
    {
        server: WebSocket.createRelayListenUri(ns, path),
        token: function() { return WebSocket.createRelayToken('http://' + ns, keyrule, key); }
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(JSON.parse(event.data));
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
});
``` 

##### <a name="createrelayedserver"></a><span data-ttu-id="97a27-169">createRelayedServer</span><span class="sxs-lookup"><span data-stu-id="97a27-169">createRelayedServer</span></span>

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

<span data-ttu-id="97a27-170">Tato metoda volá konstruktor k vytvoření nové instance RelayedServer a potom jako odběratel zadané zpětné volání na událost, připojení.</span><span class="sxs-lookup"><span data-stu-id="97a27-170">This method calls the constructor to create a new instance of the RelayedServer and then subscribes the provided callback to the 'connection' event.</span></span>
 
##### <a name="relayedconnect"></a><span data-ttu-id="97a27-171">relayedConnect</span><span class="sxs-lookup"><span data-stu-id="97a27-171">relayedConnect</span></span>

<span data-ttu-id="97a27-172">Jednoduše zrcadlení `createRelayedServer` pomocné rutiny ve funkci `relayedConnect` vytvoří připojení klienta a přihlásí se k odběru události 'open' na výsledný soketu.</span><span class="sxs-lookup"><span data-stu-id="97a27-172">Simply mirroring the `createRelayedServer` helper in function, `relayedConnect` creates a client connection and subscribes to the 'open' event on the resulting socket.</span></span>

```JavaScript
var uri = WebSocket.createRelaySendUri(ns, path);
WebSocket.relayedConnect(
    uri,
    WebSocket.createRelayToken(uri, keyrule, key),
    function (socket) {
        ...
    }
);
```

## <a name="next-steps"></a><span data-ttu-id="97a27-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97a27-173">Next steps</span></span>
<span data-ttu-id="97a27-174">Další informace o předávání přes Azure, najdete pomocí těchto odkazů:</span><span class="sxs-lookup"><span data-stu-id="97a27-174">To learn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="97a27-175">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="97a27-175">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="97a27-176">K dispozici předávání přes rozhraní API</span><span class="sxs-lookup"><span data-stu-id="97a27-176">Available Relay APIs</span></span>](relay-api-overview.md)
