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
# <a name="relay-hybrid-connections-node-api-overview"></a>Předávání API uzlu hybridní připojení – přehled

## <a name="overview"></a>Přehled

Hello [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) balíček uzel pro hybridní připojení předávání přes Azure je založená na a rozšiřuje hello ['ws'](https://www.npmjs.com/package/ws) balíčku NPM. Tento balíček znovu vyexportuje všechny exporty tento základní balíček a přidá nové export, které umožňují integrace s funkcí hybridní připojení služby předávání přes Azure hello. 

Existující aplikace, `require('ws')` můžete použít tento balíček s `require('hyco-ws')` místo toho, která umožňuje také hybridní scénáře, ve kterých můžete aplikaci přijímat připojení protokolu WebSocket místně z "hello firewallem" a pomocí hybridních připojení, všechny na Hello stejný čas.
  
## <a name="documentation"></a>Dokumentace

Hello rozhraní API jsou [zdokumentované v balíčku hlavní 'ws"hello](https://github.com/websockets/ws/blob/master/doc/ws.md). Tento článek popisuje, jak tento balíček se liší od tohoto směrného plánu. 

Hello klíčové rozdíly mezi základní balíček hello a tento 'hyco-ws, se přidá novou třídu serveru exportovat prostřednictvím `require('hyco-ws').RelayedServer`a několik pomocné metody.

### <a name="package-helper-methods"></a>Balíček pomocné metody

Na hello exportu balíček, který můžete použít následující jsou k dispozici několik pomocné metody:

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

Hello pomocné metody pro použití s tímto balíčkem jsou, ale mohou sloužit také serverem uzel pro povolení web nebo zařízení toocreate naslouchací procesy klienty nebo odesílatelé. Hello server používá tyto metody předáním identifikátory URI, který vložení krátkodobou tokeny. Tyto identifikátory URI můžete použít taky s běžné zásobníky protokolu WebSocket, které nepodporují nastavení hlavičky protokolu HTTP pro handshake hello protokolu WebSocket. Vkládání do hello identifikátor URI je podporována především pro tyto scénáře použití knihovny externí ověřování tokenů. 

#### <a name="createrelaylistenuri"></a>createRelayListenUri

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

Vytvoří platný Azure předávání hybridní připojení naslouchací proces identifikátor URI pro zadaný obor názvů a cestu hello. Tento identifikátor URI pak lze hello předávání verzi hello WebSocketServer třídy.

- `namespaceName`název domény hello předávání přes Azure obor názvů toouse (povinné) - hello.
- `path`název existující připojení hybridní předávání přes Azure v daném oboru názvů (povinné) - hello.
- `token`(volitelné) – pro přístup k dřív vydaných předávání token, který je součástí hello naslouchací proces identifikátor URI (viz následující ukázka hello).
- `id`(volitelné) – sledování identifikátor, který umožňuje sledování začátku do konce diagnostiky požadavků.

Hello `token` hodnota je volitelná a musí být použit pouze pokud není možné toosend HTTP hlavičky spolu s metodou handshake hello protokolu WebSocket, jako hello případě zásobník protokolu WebSocket W3C hello.                  


#### <a name="createrelaysenduri"></a>createRelaySendUri
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

Vytvoří platný odesílání Azure předávání hybridní připojení identifikátor URI pro zadaný obor názvů a cestu hello. Tento identifikátor URI lze použít s jakýmkoli klientem protokolu WebSocket.

- `namespaceName`název domény hello předávání přes Azure obor názvů toouse (povinné) - hello.
- `path`název existující připojení hybridní předávání přes Azure v daném oboru názvů (povinné) - hello.
- `token`(volitelné) – dřív vydaných předávání přístupový token, který je součástí hello odeslat identifikátor URI (viz následující ukázka hello).
- `id`(volitelné) – sledování identifikátor, který umožňuje sledování začátku do konce diagnostiky požadavků.

Hello `token` hodnota je volitelná a musí být použit pouze pokud není možné toosend HTTP hlavičky spolu s metodou handshake hello protokolu WebSocket, jako hello případě zásobník protokolu WebSocket W3C hello.                   


#### <a name="createrelaytoken"></a>createRelayToken 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Vytvoří token Azure předávání sdíleného přístupového podpisu (SAS) pro hello zadaný cílový identifikátor URI SAS pravidlo a pravidlo klíče SAS, který je platný pro zadaný počet sekund nebo jednu hodinu z aktuální hello prostřednictvím rychlých Pokud je vynechán hello vypršení platnosti argument hello.

- `uri`(povinné) - hello URI, pro které hello token je toobe vystavené. Hello identifikátor URI je schéma HTTP hello normalizovaný toouse a informací řetězce dotazu se odstraní.
- `ruleName`(povinné) - SAS pravidlo název buď hello entitu reprezentovanou objektem hello zadaný identifikátor URI, nebo pro hello obor názvů reprezentována hello část hostitele v identifikátoru URI.
- `key`(povinné) - platný klíč pro pravidlo SAS hello. 
- `expirationSeconds`(volitelné) – hello počet sekund do hello vygenerovat token by měla vypršet. Pokud není zadáno, výchozí hodnota hello je 1 hodina (3600).

token vydán Hello uděluje práva hello přiřazená hello zadané pravidlo SAS pro hello zadané doby trvání.

#### <a name="appendrelaytoken"></a>appendRelayToken

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Tato metoda je funkčně srovnatelný toohello `createRelayToken` metoda popsané dříve, ale vrátí hello tokenu správně připojením toohello vstup identifikátor URI.

### <a name="class-wsrelayedserver"></a>Třída ws. RelayedServer

Hello `hycows.RelayedServer` třída je alternativní toohello `ws.Server` třídu, která není naslouchat hello místní sítě, ale deleguje naslouchání toohello předávání přes Azure service.

Hello dvě třídy jsou většinou kontrakt kompatibilní, znamená to, že existující aplikace pomocí hello `ws.Server` třídy mohou být snadno změněné toouse hello přes předávací službu verze. Hello hlavní rozdíly jsou v konstruktoru hello a v hello dostupné možnosti.

#### <a name="constructor"></a>Konstruktor  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

Hello `RelayedServer` konstruktor podporuje jinou sadu argumentů než hello `Server`, protože není naslouchací proces samostatné, nebo může toobe vkládat do představuje existující rozhraní naslouchací proces protokolu HTTP. Existují také méně možností k dispozici od hello správy protokolu WebSocket je z velké části delegované toohello předávací služba.

Argumenty konstruktoru:

- `server`(povinné) - hello plně kvalifikovaný identifikátor URI pro hybridní připojení název na které toolisten obvykle zkonstruovat s hello WebSocket.createRelayListenUri() Pomocná metoda.
- `token`Tento argument (povinné) - obsahuje řetězec tokenu dřív vydaných nebo funkce zpětného volání, kterou lze volat tooobtain tokenu řetězec. Hello zpětného volání možnost je upřednostňované, protože umožňuje obnovení tokenu.

#### <a name="events"></a>Události

`RelayedServer`instance emitování tři události, které povolení jste toohandle příchozí požadavky, vytvořit připojení a zjistit chybové stavy. Se musíte přihlásit toohello `connect` toohandle zprávy o událostech. 

##### <a name="headers"></a>Záhlaví

```JavaScript 
function(headers)
```

Hello `headers` událost se vyvolá, těsně před přijmout příchozí připojení, povolení úprav hello hlavičky toosend toohello klienta. 

##### <a name="connection"></a>připojení

```JavaScript
function(socket)
```

Vygenerované při byla přijata nové připojení protokolu WebSocket. Hello objekt je typu `ws.WebSocket`, stejně jako s balíčkem základní hello.


##### <a name="error"></a>error

```JavaScript
function(error)
```

Pokud základní server hello vysílá chybu, je zde předají.  

#### <a name="helpers"></a>Pomocné rutiny

toosimplify spuštění, které přenosu serveru a okamžitě přihlášení k odběru tooincoming připojení hello balíčku zpřístupní jednoduché podpůrná funkce, která je použita také v příkladech hello následujícím způsobem:

##### <a name="createrelayedlistener"></a>createRelayedListener

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

##### <a name="createrelayedserver"></a>createRelayedServer

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

Tato metoda volá konstruktor toocreate hello novou instanci třídy hello RelayedServer a potom jako odběratel hello poskytuje zpětného volání toohello připojení událostí.
 
##### <a name="relayedconnect"></a>relayedConnect

Jednoduše zrcadlení hello `createRelayedServer` pomocné rutiny ve funkci `relayedConnect` vytvoří připojení klienta a přihlásí se k odběru události 'open' toohello výsledné soketem hello.

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

## <a name="next-steps"></a>Další kroky
Další informace o předávání přes Azure, toolearn naleznete pod těmito odkazy:
* [Co je Azure Relay?](relay-what-is-it.md)
* [K dispozici předávání přes rozhraní API](relay-api-overview.md)
