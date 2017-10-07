---
title: "aaaOverview hello rozhraní API Správce Azure předávání uzlu | Microsoft Docs"
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
ms.openlocfilehash: d231acc854be0eaa965dec0229cf63b08ff27067
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="relay-hybrid-connections-node-api-overview"></a><span data-ttu-id="5f82a-103">Předávání API uzlu hybridní připojení – přehled</span><span class="sxs-lookup"><span data-stu-id="5f82a-103">Relay Hybrid Connections Node API overview</span></span>

## <a name="overview"></a><span data-ttu-id="5f82a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5f82a-104">Overview</span></span>

<span data-ttu-id="5f82a-105">Hello [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) balíček uzel pro hybridní připojení předávání přes Azure je založená na a rozšiřuje hello ['ws'](https://www.npmjs.com/package/ws) balíčku NPM.</span><span class="sxs-lookup"><span data-stu-id="5f82a-105">hello [`hyco-ws`](https://www.npmjs.com/package/hyco-ws) Node package for Azure Relay Hybrid Connections is built on and extends hello ['ws'](https://www.npmjs.com/package/ws) NPM package.</span></span> <span data-ttu-id="5f82a-106">Tento balíček znovu vyexportuje všechny exporty tento základní balíček a přidá nové export, které umožňují integrace s funkcí hybridní připojení služby předávání přes Azure hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-106">This package re-exports all exports of that base package and adds new exports that enable integration with hello Azure Relay service Hybrid Connections feature.</span></span> 

<span data-ttu-id="5f82a-107">Existující aplikace, `require('ws')` můžete použít tento balíček s `require('hyco-ws')` místo toho, která umožňuje také hybridní scénáře, ve kterých můžete aplikaci přijímat připojení protokolu WebSocket místně z "hello firewallem" a pomocí hybridních připojení, všechny na Hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="5f82a-107">Existing applications that `require('ws')` can use this package with `require('hyco-ws')` instead, which also enables hybrid scenarios in which an application can listen for WebSocket connections locally from "inside hello firewall" and via Hybrid Connections, all at hello same time.</span></span>
  
## <a name="documentation"></a><span data-ttu-id="5f82a-108">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="5f82a-108">Documentation</span></span>

<span data-ttu-id="5f82a-109">Hello rozhraní API jsou [zdokumentované v balíčku hlavní 'ws"hello](https://github.com/websockets/ws/blob/master/doc/ws.md).</span><span class="sxs-lookup"><span data-stu-id="5f82a-109">hello APIs are [documented in hello main 'ws' package](https://github.com/websockets/ws/blob/master/doc/ws.md).</span></span> <span data-ttu-id="5f82a-110">Tento článek popisuje, jak tento balíček se liší od tohoto směrného plánu.</span><span class="sxs-lookup"><span data-stu-id="5f82a-110">This article describes how this package differs from that baseline.</span></span> 

<span data-ttu-id="5f82a-111">Hello klíčové rozdíly mezi základní balíček hello a tento 'hyco-ws, se přidá novou třídu serveru exportovat prostřednictvím `require('hyco-ws').RelayedServer`a několik pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="5f82a-111">hello key differences between hello base package and this 'hyco-ws' is that it adds a new server class, exported via `require('hyco-ws').RelayedServer`, and a few helper methods.</span></span>

### <a name="package-helper-methods"></a><span data-ttu-id="5f82a-112">Balíček pomocné metody</span><span class="sxs-lookup"><span data-stu-id="5f82a-112">Package helper methods</span></span>

<span data-ttu-id="5f82a-113">Na hello exportu balíček, který můžete použít následující jsou k dispozici několik pomocné metody:</span><span class="sxs-lookup"><span data-stu-id="5f82a-113">There are several utility methods available on hello package export that you can reference as follows:</span></span>

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

<span data-ttu-id="5f82a-114">Hello pomocné metody pro použití s tímto balíčkem jsou, ale mohou sloužit také serverem uzel pro povolení web nebo zařízení toocreate naslouchací procesy klienty nebo odesílatelé.</span><span class="sxs-lookup"><span data-stu-id="5f82a-114">hello helper methods are for use with this package, but can also be used by a Node server for enabling web or device clients toocreate listeners or senders.</span></span> <span data-ttu-id="5f82a-115">Hello server používá tyto metody předáním identifikátory URI, který vložení krátkodobou tokeny.</span><span class="sxs-lookup"><span data-stu-id="5f82a-115">hello server uses these methods by passing them URIs that embed short-lived tokens.</span></span> <span data-ttu-id="5f82a-116">Tyto identifikátory URI můžete použít taky s běžné zásobníky protokolu WebSocket, které nepodporují nastavení hlavičky protokolu HTTP pro handshake hello protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5f82a-116">These URIs can also be used with common WebSocket stacks that do not support setting HTTP headers for hello WebSocket handshake.</span></span> <span data-ttu-id="5f82a-117">Vkládání do hello identifikátor URI je podporována především pro tyto scénáře použití knihovny externí ověřování tokenů.</span><span class="sxs-lookup"><span data-stu-id="5f82a-117">Embedding authorization tokens into hello URI is supported primarily for those library-external usage scenarios.</span></span> 

#### <a name="createrelaylistenuri"></a><span data-ttu-id="5f82a-118">createRelayListenUri</span><span class="sxs-lookup"><span data-stu-id="5f82a-118">createRelayListenUri</span></span>

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="5f82a-119">Vytvoří platný Azure předávání hybridní připojení naslouchací proces identifikátor URI pro zadaný obor názvů a cestu hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-119">Creates a valid Azure Relay Hybrid Connection listener URI for hello given namespace and path.</span></span> <span data-ttu-id="5f82a-120">Tento identifikátor URI pak lze hello předávání verzi hello WebSocketServer třídy.</span><span class="sxs-lookup"><span data-stu-id="5f82a-120">This URI can then be used with hello relay version of hello WebSocketServer class.</span></span>

- <span data-ttu-id="5f82a-121">`namespaceName`název domény hello předávání přes Azure obor názvů toouse (povinné) - hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-121">`namespaceName` (required) - hello domain-qualified name of hello Azure Relay namespace toouse.</span></span>
- <span data-ttu-id="5f82a-122">`path`název existující připojení hybridní předávání přes Azure v daném oboru názvů (povinné) - hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-122">`path` (required) - hello name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="5f82a-123">`token`(volitelné) – pro přístup k dřív vydaných předávání token, který je součástí hello naslouchací proces identifikátor URI (viz následující ukázka hello).</span><span class="sxs-lookup"><span data-stu-id="5f82a-123">`token` (optional) - a previously issued Relay access token that is embedded in hello listener URI (see hello following example).</span></span>
- <span data-ttu-id="5f82a-124">`id`(volitelné) – sledování identifikátor, který umožňuje sledování začátku do konce diagnostiky požadavků.</span><span class="sxs-lookup"><span data-stu-id="5f82a-124">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="5f82a-125">Hello `token` hodnota je volitelná a musí být použit pouze pokud není možné toosend HTTP hlavičky spolu s metodou handshake hello protokolu WebSocket, jako hello případě zásobník protokolu WebSocket W3C hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-125">hello `token` value is optional and should only be used when it is not possible toosend HTTP headers along with hello WebSocket handshake, as is hello case with hello W3C WebSocket stack.</span></span>                  


#### <a name="createrelaysenduri"></a><span data-ttu-id="5f82a-126">createRelaySendUri</span><span class="sxs-lookup"><span data-stu-id="5f82a-126">createRelaySendUri</span></span>
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="5f82a-127">Vytvoří platný odesílání Azure předávání hybridní připojení identifikátor URI pro zadaný obor názvů a cestu hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-127">Creates a valid Azure Relay Hybrid Connection send URI for hello given namespace and path.</span></span> <span data-ttu-id="5f82a-128">Tento identifikátor URI lze použít s jakýmkoli klientem protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5f82a-128">This URI can be used with any WebSocket client.</span></span>

- <span data-ttu-id="5f82a-129">`namespaceName`název domény hello předávání přes Azure obor názvů toouse (povinné) - hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-129">`namespaceName` (required) - hello domain-qualified name of hello Azure Relay namespace toouse.</span></span>
- <span data-ttu-id="5f82a-130">`path`název existující připojení hybridní předávání přes Azure v daném oboru názvů (povinné) - hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-130">`path` (required) - hello name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="5f82a-131">`token`(volitelné) – dřív vydaných předávání přístupový token, který je součástí hello odeslat identifikátor URI (viz následující ukázka hello).</span><span class="sxs-lookup"><span data-stu-id="5f82a-131">`token` (optional) - a previously issued Relay access token that is embedded in hello send URI (see hello following example).</span></span>
- <span data-ttu-id="5f82a-132">`id`(volitelné) – sledování identifikátor, který umožňuje sledování začátku do konce diagnostiky požadavků.</span><span class="sxs-lookup"><span data-stu-id="5f82a-132">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="5f82a-133">Hello `token` hodnota je volitelná a musí být použit pouze pokud není možné toosend HTTP hlavičky spolu s metodou handshake hello protokolu WebSocket, jako hello případě zásobník protokolu WebSocket W3C hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-133">hello `token` value is optional and should only be used when it is not possible toosend HTTP headers along with hello WebSocket handshake, as is hello case with hello W3C WebSocket stack.</span></span>                   


#### <a name="createrelaytoken"></a><span data-ttu-id="5f82a-134">createRelayToken</span><span class="sxs-lookup"><span data-stu-id="5f82a-134">createRelayToken</span></span> 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="5f82a-135">Vytvoří token Azure předávání sdíleného přístupového podpisu (SAS) pro hello zadaný cílový identifikátor URI SAS pravidlo a pravidlo klíče SAS, který je platný pro zadaný počet sekund nebo jednu hodinu z aktuální hello prostřednictvím rychlých Pokud je vynechán hello vypršení platnosti argument hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-135">Creates an Azure Relay Shared Access Signature (SAS) token for hello given target URI, SAS rule, and SAS rule key that is valid for hello given number of seconds or for an hour from hello current instant if hello expiry argument is omitted.</span></span>

- <span data-ttu-id="5f82a-136">`uri`(povinné) - hello URI, pro které hello token je toobe vystavené.</span><span class="sxs-lookup"><span data-stu-id="5f82a-136">`uri` (required) - hello URI for which hello token is toobe issued.</span></span> <span data-ttu-id="5f82a-137">Hello identifikátor URI je schéma HTTP hello normalizovaný toouse a informací řetězce dotazu se odstraní.</span><span class="sxs-lookup"><span data-stu-id="5f82a-137">hello URI is normalized toouse hello HTTP scheme, and query string information is stripped.</span></span>
- <span data-ttu-id="5f82a-138">`ruleName`(povinné) - SAS pravidlo název buď hello entitu reprezentovanou objektem hello zadaný identifikátor URI, nebo pro hello obor názvů reprezentována hello část hostitele v identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="5f82a-138">`ruleName` (required) - SAS rule name for either hello entity represented by hello given URI, or for hello namespace represented by hello URI host portion.</span></span>
- <span data-ttu-id="5f82a-139">`key`(povinné) - platný klíč pro pravidlo SAS hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-139">`key` (required) - valid key for hello SAS rule.</span></span> 
- <span data-ttu-id="5f82a-140">`expirationSeconds`(volitelné) – hello počet sekund do hello vygenerovat token by měla vypršet.</span><span class="sxs-lookup"><span data-stu-id="5f82a-140">`expirationSeconds` (optional) - hello number of seconds until hello generated token should expire.</span></span> <span data-ttu-id="5f82a-141">Pokud není zadáno, výchozí hodnota hello je 1 hodina (3600).</span><span class="sxs-lookup"><span data-stu-id="5f82a-141">If not specified, hello default is 1 hour (3600).</span></span>

<span data-ttu-id="5f82a-142">token vydán Hello uděluje práva hello přiřazená hello zadané pravidlo SAS pro hello zadané doby trvání.</span><span class="sxs-lookup"><span data-stu-id="5f82a-142">hello issued token confers hello rights associated with hello specified SAS rule for hello given duration.</span></span>

#### <a name="appendrelaytoken"></a><span data-ttu-id="5f82a-143">appendRelayToken</span><span class="sxs-lookup"><span data-stu-id="5f82a-143">appendRelayToken</span></span>

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="5f82a-144">Tato metoda je funkčně srovnatelný toohello `createRelayToken` metoda popsané dříve, ale vrátí hello tokenu správně připojením toohello vstup identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="5f82a-144">This method is functionally equivalent toohello `createRelayToken` method documented previously, but returns hello token correctly appended toohello input URI.</span></span>

### <a name="class-wsrelayedserver"></a><span data-ttu-id="5f82a-145">Třída ws. RelayedServer</span><span class="sxs-lookup"><span data-stu-id="5f82a-145">Class ws.RelayedServer</span></span>

<span data-ttu-id="5f82a-146">Hello `hycows.RelayedServer` třída je alternativní toohello `ws.Server` třídu, která není naslouchat hello místní sítě, ale deleguje naslouchání toohello předávání přes Azure service.</span><span class="sxs-lookup"><span data-stu-id="5f82a-146">hello `hycows.RelayedServer` class is an alternative toohello `ws.Server` class that does not listen on hello local network, but delegates listening toohello Azure Relay service.</span></span>

<span data-ttu-id="5f82a-147">Hello dvě třídy jsou většinou kontrakt kompatibilní, znamená to, že existující aplikace pomocí hello `ws.Server` třídy mohou být snadno změněné toouse hello přes předávací službu verze.</span><span class="sxs-lookup"><span data-stu-id="5f82a-147">hello two classes are mostly contract compatible, meaning that an existing application using hello `ws.Server` class can easily be changed toouse hello relayed version.</span></span> <span data-ttu-id="5f82a-148">Hello hlavní rozdíly jsou v konstruktoru hello a v hello dostupné možnosti.</span><span class="sxs-lookup"><span data-stu-id="5f82a-148">hello main differences are in hello constructor and in hello available options.</span></span>

#### <a name="constructor"></a><span data-ttu-id="5f82a-149">Konstruktor</span><span class="sxs-lookup"><span data-stu-id="5f82a-149">Constructor</span></span>  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

<span data-ttu-id="5f82a-150">Hello `RelayedServer` konstruktor podporuje jinou sadu argumentů než hello `Server`, protože není naslouchací proces samostatné, nebo může toobe vkládat do představuje existující rozhraní naslouchací proces protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f82a-150">hello `RelayedServer` constructor supports a different set of arguments than hello `Server`, because it is not a standalone listener, or able toobe embedded into an existing HTTP listener framework.</span></span> <span data-ttu-id="5f82a-151">Existují také méně možností k dispozici od hello správy protokolu WebSocket je z velké části delegované toohello předávací služba.</span><span class="sxs-lookup"><span data-stu-id="5f82a-151">There are also fewer options available since hello WebSocket management is largely delegated toohello Relay service.</span></span>

<span data-ttu-id="5f82a-152">Argumenty konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="5f82a-152">Constructor arguments:</span></span>

- <span data-ttu-id="5f82a-153">`server`(povinné) - hello plně kvalifikovaný identifikátor URI pro hybridní připojení název na které toolisten obvykle zkonstruovat s hello WebSocket.createRelayListenUri() Pomocná metoda.</span><span class="sxs-lookup"><span data-stu-id="5f82a-153">`server` (required) - hello fully qualified URI for a Hybrid Connection name on which toolisten, usually constructed with hello WebSocket.createRelayListenUri() helper method.</span></span>
- <span data-ttu-id="5f82a-154">`token`Tento argument (povinné) - obsahuje řetězec tokenu dřív vydaných nebo funkce zpětného volání, kterou lze volat tooobtain tokenu řetězec.</span><span class="sxs-lookup"><span data-stu-id="5f82a-154">`token` (required) - this argument holds either a previously issued token string or a callback function that can be called tooobtain such a token string.</span></span> <span data-ttu-id="5f82a-155">Hello zpětného volání možnost je upřednostňované, protože umožňuje obnovení tokenu.</span><span class="sxs-lookup"><span data-stu-id="5f82a-155">hello callback option is preferred, as it enables token renewal.</span></span>

#### <a name="events"></a><span data-ttu-id="5f82a-156">Události</span><span class="sxs-lookup"><span data-stu-id="5f82a-156">Events</span></span>

<span data-ttu-id="5f82a-157">`RelayedServer`instance emitování tři události, které povolení jste toohandle příchozí požadavky, vytvořit připojení a zjistit chybové stavy.</span><span class="sxs-lookup"><span data-stu-id="5f82a-157">`RelayedServer` instances emit three events that enable you toohandle incoming requests, establish connections, and detect error conditions.</span></span> <span data-ttu-id="5f82a-158">Se musíte přihlásit toohello `connect` toohandle zprávy o událostech.</span><span class="sxs-lookup"><span data-stu-id="5f82a-158">You must subscribe toohello `connect` event toohandle messages.</span></span> 

##### <a name="headers"></a><span data-ttu-id="5f82a-159">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="5f82a-159">headers</span></span>

```JavaScript 
function(headers)
```

<span data-ttu-id="5f82a-160">Hello `headers` událost se vyvolá, těsně před přijmout příchozí připojení, povolení úprav hello hlavičky toosend toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="5f82a-160">hello `headers` event is raised just before an incoming connection is accepted, enabling modification of hello headers toosend toohello client.</span></span> 

##### <a name="connection"></a><span data-ttu-id="5f82a-161">připojení</span><span class="sxs-lookup"><span data-stu-id="5f82a-161">connection</span></span>

```JavaScript
function(socket)
```

<span data-ttu-id="5f82a-162">Vygenerované při byla přijata nové připojení protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5f82a-162">Emitted when a new WebSocket connection is accepted.</span></span> <span data-ttu-id="5f82a-163">Hello objekt je typu `ws.WebSocket`, stejně jako s balíčkem základní hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-163">hello object is of type `ws.WebSocket`, same as with hello base package.</span></span>


##### <a name="error"></a><span data-ttu-id="5f82a-164">error</span><span class="sxs-lookup"><span data-stu-id="5f82a-164">error</span></span>

```JavaScript
function(error)
```

<span data-ttu-id="5f82a-165">Pokud základní server hello vysílá chybu, je zde předají.</span><span class="sxs-lookup"><span data-stu-id="5f82a-165">If hello underlying server emits an error, it is forwarded here.</span></span>  

#### <a name="helpers"></a><span data-ttu-id="5f82a-166">Pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="5f82a-166">Helpers</span></span>

<span data-ttu-id="5f82a-167">toosimplify spuštění, které přenosu serveru a okamžitě přihlášení k odběru tooincoming připojení hello balíčku zpřístupní jednoduché podpůrná funkce, která je použita také v příkladech hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5f82a-167">toosimplify starting a relayed server and immediately subscribing tooincoming connections, hello package exposes a simple helper function, which is also used in hello examples, as follows:</span></span>

##### <a name="createrelayedlistener"></a><span data-ttu-id="5f82a-168">createRelayedListener</span><span class="sxs-lookup"><span data-stu-id="5f82a-168">createRelayedListener</span></span>

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

##### <a name="createrelayedserver"></a><span data-ttu-id="5f82a-169">createRelayedServer</span><span class="sxs-lookup"><span data-stu-id="5f82a-169">createRelayedServer</span></span>

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

<span data-ttu-id="5f82a-170">Tato metoda volá konstruktor toocreate hello novou instanci třídy hello RelayedServer a potom jako odběratel hello poskytuje zpětného volání toohello připojení událostí.</span><span class="sxs-lookup"><span data-stu-id="5f82a-170">This method calls hello constructor toocreate a new instance of hello RelayedServer and then subscribes hello provided callback toohello 'connection' event.</span></span>
 
##### <a name="relayedconnect"></a><span data-ttu-id="5f82a-171">relayedConnect</span><span class="sxs-lookup"><span data-stu-id="5f82a-171">relayedConnect</span></span>

<span data-ttu-id="5f82a-172">Jednoduše zrcadlení hello `createRelayedServer` pomocné rutiny ve funkci `relayedConnect` vytvoří připojení klienta a přihlásí se k odběru události 'open' toohello výsledné soketem hello.</span><span class="sxs-lookup"><span data-stu-id="5f82a-172">Simply mirroring hello `createRelayedServer` helper in function, `relayedConnect` creates a client connection and subscribes toohello 'open' event on hello resulting socket.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5f82a-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f82a-173">Next steps</span></span>
<span data-ttu-id="5f82a-174">Další informace o předávání přes Azure, toolearn naleznete pod těmito odkazy:</span><span class="sxs-lookup"><span data-stu-id="5f82a-174">toolearn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="5f82a-175">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="5f82a-175">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="5f82a-176">K dispozici předávání přes rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5f82a-176">Available Relay APIs</span></span>](relay-api-overview.md)
