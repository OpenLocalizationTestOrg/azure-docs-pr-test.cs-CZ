### <a name="create-a-nodejs-application"></a><span data-ttu-id="79137-101">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="79137-101">Create a Node.js application</span></span>

<span data-ttu-id="79137-102">Vytvořte nový soubor JavaScript s názvem `sender.js`.</span><span class="sxs-lookup"><span data-stu-id="79137-102">Create a new JavaScript file called `sender.js`.</span></span>

### <a name="add-hello-relay-npm-package"></a><span data-ttu-id="79137-103">Přidání balíčku NPM předávání hello</span><span class="sxs-lookup"><span data-stu-id="79137-103">Add hello Relay NPM package</span></span>

<span data-ttu-id="79137-104">Spusťte z příkazového řádku uzlu ve složce projektu příkaz `npm install hyco-ws`.</span><span class="sxs-lookup"><span data-stu-id="79137-104">Run `npm install hyco-ws` from a Node command prompt in your project folder.</span></span>

### <a name="write-some-code-toosend-messages"></a><span data-ttu-id="79137-105">Napsat kód toosend zprávy</span><span class="sxs-lookup"><span data-stu-id="79137-105">Write some code toosend messages</span></span>

1. <span data-ttu-id="79137-106">Přidejte následující hello `constants` toohello horní části hello `sender.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="79137-106">Add hello following `constants` toohello top of hello `sender.js` file.</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. <span data-ttu-id="79137-107">Přidejte následující konstanty toohello hello `sender.js` hello hybridní připojení podrobnosti v souboru.</span><span class="sxs-lookup"><span data-stu-id="79137-107">Add hello following constants toohello `sender.js` file for hello hybrid connection details.</span></span> <span data-ttu-id="79137-108">Nahraďte zástupné symboly hello v závorkách hello hodnoty, které jste získali při vytváření hello hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="79137-108">Replace hello placeholders in brackets with hello values you obtained when you created hello hybrid connection.</span></span>
   
   1. <span data-ttu-id="79137-109">`const ns`-hello předávání názvů.</span><span class="sxs-lookup"><span data-stu-id="79137-109">`const ns` - hello Relay namespace.</span></span> <span data-ttu-id="79137-110">Být jisti toouse hello obor názvů plně kvalifikovaný název; například `{namespace}.servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="79137-110">Be sure toouse hello fully qualified namespace name; for example, `{namespace}.servicebus.windows.net`.</span></span>
   2. <span data-ttu-id="79137-111">`const path`-Název hello hello hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="79137-111">`const path` - hello name of hello hybrid connection.</span></span>
   3. <span data-ttu-id="79137-112">`const keyrule`-hello název klíče SAS hello.</span><span class="sxs-lookup"><span data-stu-id="79137-112">`const keyrule` - hello name of hello SAS key.</span></span>
   4. <span data-ttu-id="79137-113">`const key`-hello hodnotu klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="79137-113">`const key` - hello SAS key value.</span></span>

3. <span data-ttu-id="79137-114">Přidejte následující kód toohello hello `sender.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="79137-114">Add hello following code toohello `sender.js` file:</span></span>
   
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
    <span data-ttu-id="79137-115">Soubor sender.js by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="79137-115">Here is what your sender.js file should look like:</span></span>
   
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

