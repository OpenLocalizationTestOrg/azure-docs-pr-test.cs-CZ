### <a name="create-a-nodejs-application"></a><span data-ttu-id="a2c54-101">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="a2c54-101">Create a Node.js application</span></span>

<span data-ttu-id="a2c54-102">Vytvořte nový soubor JavaScript s názvem `listener.js`.</span><span class="sxs-lookup"><span data-stu-id="a2c54-102">Create a new JavaScript file called `listener.js`.</span></span>

### <a name="add-the-relay-npm-package"></a><span data-ttu-id="a2c54-103">Přidání balíčku NPM služby Relay</span><span class="sxs-lookup"><span data-stu-id="a2c54-103">Add the Relay NPM package</span></span>

<span data-ttu-id="a2c54-104">Spusťte z příkazového řádku uzlu ve složce projektu příkaz `npm install hyco-ws`.</span><span class="sxs-lookup"><span data-stu-id="a2c54-104">Run `npm install hyco-ws` from a Node command prompt in your project folder.</span></span>

### <a name="write-some-code-to-receive-messages"></a><span data-ttu-id="a2c54-105">Napsání kódu pro přijímání zpráv</span><span class="sxs-lookup"><span data-stu-id="a2c54-105">Write some code to receive messages</span></span>

1. <span data-ttu-id="a2c54-106">Na začátek souboru `listener.js` přidejte následující konstantu.</span><span class="sxs-lookup"><span data-stu-id="a2c54-106">Add the following constant to the top of the `listener.js` file.</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. <span data-ttu-id="a2c54-107">Do souboru `listener.js` přidejte následující konstanty s podrobnostmi o hybridním připojení.</span><span class="sxs-lookup"><span data-stu-id="a2c54-107">Add the following constants to the `listener.js` file for the hybrid connection details.</span></span> <span data-ttu-id="a2c54-108">Zástupné symboly v závorkách nahraďte hodnotami, které jste získali při vytváření hybridního připojení.</span><span class="sxs-lookup"><span data-stu-id="a2c54-108">Replace the placeholders in brackets with the values you obtained when you created the hybrid connection.</span></span>
   
   1. <span data-ttu-id="a2c54-109">`const ns` – Obor názvů služby Relay.</span><span class="sxs-lookup"><span data-stu-id="a2c54-109">`const ns` - The Relay namespace.</span></span> <span data-ttu-id="a2c54-110">Nezapomeňte použít plně kvalifikovaný obor názvů, například `{namespace}.servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="a2c54-110">Be sure to use the fully qualified namespace name; for example, `{namespace}.servicebus.windows.net`.</span></span>
   2. <span data-ttu-id="a2c54-111">`const path` – Název hybridního připojení.</span><span class="sxs-lookup"><span data-stu-id="a2c54-111">`const path` - The name of the hybrid connection.</span></span>
   3. <span data-ttu-id="a2c54-112">`const keyrule` – Název klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="a2c54-112">`const keyrule` - The name of the SAS key.</span></span>
   4. <span data-ttu-id="a2c54-113">`const key` – Hodnota klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="a2c54-113">`const key` - The SAS key value.</span></span>

3. <span data-ttu-id="a2c54-114">Do souboru `listener.js` přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="a2c54-114">Add the following code to the `listener.js` file:</span></span>
   
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
    <span data-ttu-id="a2c54-115">Soubor listener.js by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="a2c54-115">Here is what your listener.js file should look like:</span></span>
   
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

