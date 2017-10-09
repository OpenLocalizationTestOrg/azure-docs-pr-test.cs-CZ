### <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js

Vytvořte nový soubor JavaScript s názvem `listener.js`.

### <a name="add-hello-relay-npm-package"></a>Přidání balíčku NPM předávání hello

Spusťte z příkazového řádku uzlu ve složce projektu příkaz `npm install hyco-ws`.

### <a name="write-some-code-tooreceive-messages"></a>Napsat kód tooreceive zprávy

1. Přidejte následující konstanty toohello horní části hello hello `listener.js` souboru.
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. Přidejte následující konstanty toohello hello `listener.js` hello hybridní připojení podrobnosti v souboru. Nahraďte zástupné symboly hello v závorkách hello hodnoty, které jste získali při vytváření hello hybridní připojení.
   
   1. `const ns`-hello předávání názvů. Být jisti toouse hello obor názvů plně kvalifikovaný název; například `{namespace}.servicebus.windows.net`.
   2. `const path`-Název hello hello hybridní připojení.
   3. `const keyrule`-hello název klíče SAS hello.
   4. `const key`-hello hodnotu klíče SAS.

3. Přidejte následující kód toohello hello `listener.js` souboru:
   
    ```js
    var wss = WebSocket.createRelayedServer(
    {
        server : WebSocket.createRelayListenUri(ns, path),
        token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(event.data);
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```
    Soubor listener.js by měl vypadat takto:
   
    ```js
    const WebSocket = require('hyco-ws');
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    var wss = WebSocket.createRelayedServer(
        {
            server : WebSocket.createRelayListenUri(ns, path),
            token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
        }, 
        function (ws) {
            console.log('connection accepted');
            ws.onmessage = function (event) {
                console.log(event.data);
            };
            ws.on('close', function () {
                console.log('connection closed');
            });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```

