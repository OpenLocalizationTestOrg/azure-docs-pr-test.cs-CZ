### <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js

Vytvořte nový soubor JavaScript s názvem `sender.js`.

### <a name="add-hello-relay-npm-package"></a>Přidání balíčku NPM předávání hello

Spusťte z příkazového řádku uzlu ve složce projektu příkaz `npm install hyco-ws`.

### <a name="write-some-code-toosend-messages"></a>Napsat kód toosend zprávy

1. Přidejte následující hello `constants` toohello horní části hello `sender.js` souboru.
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. Přidejte následující konstanty toohello hello `sender.js` hello hybridní připojení podrobnosti v souboru. Nahraďte zástupné symboly hello v závorkách hello hodnoty, které jste získali při vytváření hello hybridní připojení.
   
   1. `const ns`-hello předávání názvů. Být jisti toouse hello obor názvů plně kvalifikovaný název; například `{namespace}.servicebus.windows.net`.
   2. `const path`-Název hello hello hybridní připojení.
   3. `const keyrule`-hello název klíče SAS hello.
   4. `const key`-hello hodnotu klíče SAS.

3. Přidejte následující kód toohello hello `sender.js` souboru:
   
    ```js
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```
    Soubor sender.js by měl vypadat takto:
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```

