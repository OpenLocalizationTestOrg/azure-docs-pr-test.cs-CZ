### <a name="create-a-nodejs-application"></a><span data-ttu-id="42e17-101">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="42e17-101">Create a Node.js application</span></span>

<span data-ttu-id="42e17-102">Vytvořte nový soubor JavaScript s názvem `listener.js`.</span><span class="sxs-lookup"><span data-stu-id="42e17-102">Create a new JavaScript file called `listener.js`.</span></span>

### <a name="add-hello-relay-npm-package"></a><span data-ttu-id="42e17-103">Přidání balíčku NPM předávání hello</span><span class="sxs-lookup"><span data-stu-id="42e17-103">Add hello Relay NPM package</span></span>

<span data-ttu-id="42e17-104">Spusťte z příkazového řádku uzlu ve složce projektu příkaz `npm install hyco-ws`.</span><span class="sxs-lookup"><span data-stu-id="42e17-104">Run `npm install hyco-ws` from a Node command prompt in your project folder.</span></span>

### <a name="write-some-code-tooreceive-messages"></a><span data-ttu-id="42e17-105">Napsat kód tooreceive zprávy</span><span class="sxs-lookup"><span data-stu-id="42e17-105">Write some code tooreceive messages</span></span>

1. <span data-ttu-id="42e17-106">Přidejte následující konstanty toohello horní části hello hello `listener.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="42e17-106">Add hello following constant toohello top of hello `listener.js` file.</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. <span data-ttu-id="42e17-107">Přidejte následující konstanty toohello hello `listener.js` hello hybridní připojení podrobnosti v souboru.</span><span class="sxs-lookup"><span data-stu-id="42e17-107">Add hello following constants toohello `listener.js` file for hello hybrid connection details.</span></span> <span data-ttu-id="42e17-108">Nahraďte zástupné symboly hello v závorkách hello hodnoty, které jste získali při vytváření hello hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="42e17-108">Replace hello placeholders in brackets with hello values you obtained when you created hello hybrid connection.</span></span>
   
   1. <span data-ttu-id="42e17-109">`const ns`-hello předávání názvů.</span><span class="sxs-lookup"><span data-stu-id="42e17-109">`const ns` - hello Relay namespace.</span></span> <span data-ttu-id="42e17-110">Být jisti toouse hello obor názvů plně kvalifikovaný název; například `{namespace}.servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="42e17-110">Be sure toouse hello fully qualified namespace name; for example, `{namespace}.servicebus.windows.net`.</span></span>
   2. <span data-ttu-id="42e17-111">`const path`-Název hello hello hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="42e17-111">`const path` - hello name of hello hybrid connection.</span></span>
   3. <span data-ttu-id="42e17-112">`const keyrule`-hello název klíče SAS hello.</span><span class="sxs-lookup"><span data-stu-id="42e17-112">`const keyrule` - hello name of hello SAS key.</span></span>
   4. <span data-ttu-id="42e17-113">`const key`-hello hodnotu klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="42e17-113">`const key` - hello SAS key value.</span></span>

3. <span data-ttu-id="42e17-114">Přidejte následující kód toohello hello `listener.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="42e17-114">Add hello following code toohello `listener.js` file:</span></span>
   
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
    <span data-ttu-id="42e17-115">Soubor listener.js by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="42e17-115">Here is what your listener.js file should look like:</span></span>
   
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

