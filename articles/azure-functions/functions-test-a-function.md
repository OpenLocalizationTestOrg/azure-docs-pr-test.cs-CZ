---
title: aaaTesting Azure Functions | Microsoft Docs
description: "Azure functions otestujte pomocí Postman, cURL a Node.js."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure funkce, funkce, zpracování událostí, webhooků, dynamické výpočetní, bez serveru architektura testování"
ms.assetid: c00f3082-30d2-46b3-96ea-34faf2f15f77
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/02/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a084f8dbc8089356c3c19d789dc9098f2bb63052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a><span data-ttu-id="748e0-104">Strategie pro testování kódu v Azure Functions</span><span class="sxs-lookup"><span data-stu-id="748e0-104">Strategies for testing your code in Azure Functions</span></span>

<span data-ttu-id="748e0-105">Toto téma popisuje různé způsoby tootest funkce, včetně použití hello následující obecné blíží hello:</span><span class="sxs-lookup"><span data-stu-id="748e0-105">This topic demonstrates hello various ways tootest functions, including using hello following general approaches:</span></span>

+ <span data-ttu-id="748e0-106">Nástroje založené na protokolu HTTP, například cURL, Postman a i webový prohlížeč pro webové aktivační procedury</span><span class="sxs-lookup"><span data-stu-id="748e0-106">HTTP-based tools, such as cURL, Postman, and even a web browser for web-based triggers</span></span>
+ <span data-ttu-id="748e0-107">Azure Storage Explorer, aktivační události na základě Azure Storage tootest</span><span class="sxs-lookup"><span data-stu-id="748e0-107">Azure Storage Explorer, tootest Azure Storage-based triggers</span></span>
+ <span data-ttu-id="748e0-108">Karta testu na portálu Azure Functions hello</span><span class="sxs-lookup"><span data-stu-id="748e0-108">Test tab in hello Azure Functions portal</span></span>
+ <span data-ttu-id="748e0-109">Spustí časovač – funkce</span><span class="sxs-lookup"><span data-stu-id="748e0-109">Timer-triggered function</span></span>
+ <span data-ttu-id="748e0-110">Testování aplikace nebo framework</span><span class="sxs-lookup"><span data-stu-id="748e0-110">Testing application or framework</span></span>

<span data-ttu-id="748e0-111">Všechny tyto metody testování pomocí funkce aktivační události protokolu HTTP, který přijímá vstupu prostřednictvím buď dotaz řetězec, nebo parametr hello obsah žádosti.</span><span class="sxs-lookup"><span data-stu-id="748e0-111">All these testing methods use an HTTP trigger function that accepts input through either a query string parameter or hello request body.</span></span> <span data-ttu-id="748e0-112">Tato funkce vytvoříte v první části hello.</span><span class="sxs-lookup"><span data-stu-id="748e0-112">You create this function in hello first section.</span></span>

## <a name="create-a-function-for-testing"></a><span data-ttu-id="748e0-113">Vytvořit funkci pro testování</span><span class="sxs-lookup"><span data-stu-id="748e0-113">Create a function for testing</span></span>
<span data-ttu-id="748e0-114">Pro většinu v tomto kurzu používáme mírně upravenou verzi hello šablony funkce HttpTrigger JavaScript, která je dostupná, když vytvoříte funkci.</span><span class="sxs-lookup"><span data-stu-id="748e0-114">For most of this tutorial, we use a slightly modified version of hello HttpTrigger JavaScript function template that is available when you create a function.</span></span> <span data-ttu-id="748e0-115">Pokud potřebujete pomoc, vytváření funkce, přečtěte si to [kurzu](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="748e0-115">If you need help creating a function, review this [tutorial](functions-create-first-azure-function.md).</span></span> <span data-ttu-id="748e0-116">Zvolte hello **HttpTrigger - JavaScript** šablonu při vytváření hello testovací funkce v hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="748e0-116">Choose hello **HttpTrigger- JavaScript** template when creating hello test function in hello [Azure portal].</span></span>

<span data-ttu-id="748e0-117">Hello výchozí šablony funkce je v podstatě "hello, world" funkce, která vrátí název back hello z hello textu nebo dotaz, řetězec parametr žádosti, `name=<your name>`.</span><span class="sxs-lookup"><span data-stu-id="748e0-117">hello default function template is basically a "hello world" function that echoes back hello name from hello request body or query string parameter, `name=<your name>`.</span></span>  <span data-ttu-id="748e0-118">Budeme budete aktualizovat kód hello tooalso umožňují tooprovide hello název a adresu jako obsah JSON v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="748e0-118">We'll update hello code tooalso allow you tooprovide hello name and an address as JSON content in hello request body.</span></span> <span data-ttu-id="748e0-119">Potom hello funkce vrátí tyto back toohello klienta, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="748e0-119">Then hello function echoes these back toohello client when available.</span></span>   

<span data-ttu-id="748e0-120">Aktualizujte hello funkce hello následující kód, který budeme používat pro testování:</span><span class="sxs-lookup"><span data-stu-id="748e0-120">Update hello function with hello following code, which we will use for testing:</span></span>

```javascript
module.exports = function (context, req) {
    context.log("HTTP trigger function processed a request. RequestUri=%s", req.originalUrl);
    context.log("Request Headers = " + JSON.stringify(req.headers));
    var res;

    if (req.query.name || (req.body && req.body.name)) {
        if (typeof req.query.name != "undefined") {
            context.log("Name was provided as a query string param...");
            res = ProcessNewUserInformation(context, req.query.name);
        }
        else {
            context.log("Processing user info from request body...");
            res = ProcessNewUserInformation(context, req.body.name, req.body.address);
        }
    }
    else {
        res = {
            status: 400,
            body: "Please pass a name on hello query string or in hello request body"
        };
    }
    context.done(null, res);
};
function ProcessNewUserInformation(context, name, address) {
    context.log("Processing user information...");
    context.log("name = " + name);
    var echoString = "Hello " + name;
    var res;

    if (typeof address != "undefined") {
        echoString += "\n" + "hello address you provided is " + address;
        context.log("address = " + address);
    }
    res = {
        // status: 200, /* Defaults too200 */
        body: echoString
    };
    return res;
}
```

## <a name="test-a-function-with-tools"></a><span data-ttu-id="748e0-121">Testování funkce nástroje</span><span class="sxs-lookup"><span data-stu-id="748e0-121">Test a function with tools</span></span>
<span data-ttu-id="748e0-122">Mimo hello portálu Azure existují různé nástroje, které jsou používané tootrigger funkcí pro testování.</span><span class="sxs-lookup"><span data-stu-id="748e0-122">Outside hello Azure portal, there are various tools that you can use tootrigger your functions for testing.</span></span> <span data-ttu-id="748e0-123">Mezi ně patří HTTP testování nástroje (na základě uživatelského rozhraní i příkaz řádku), nástroje pro Azure Storage přístupu a i jednoduchý webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="748e0-123">These include HTTP testing tools (both UI-based and command line), Azure Storage access tools, and even a simple web browser.</span></span>

### <a name="test-with-a-browser"></a><span data-ttu-id="748e0-124">Testování s prohlížečem</span><span class="sxs-lookup"><span data-stu-id="748e0-124">Test with a browser</span></span>
<span data-ttu-id="748e0-125">webový prohlížeč Hello je funkce tootrigger jednoduchý způsob prostřednictvím protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="748e0-125">hello web browser is a simple way tootrigger functions via HTTP.</span></span> <span data-ttu-id="748e0-126">Můžete použít prohlížeč pro požadavky GET, které nevyžadují datovou část textu a pouze dotazu použijte řetězec parametry.</span><span class="sxs-lookup"><span data-stu-id="748e0-126">You can use a browser for GET requests that do not require a body payload, and that use only query string parameters.</span></span>

<span data-ttu-id="748e0-127">Funkce hello tootest definovaného dříve, kopie hello **Url funkce** z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="748e0-127">tootest hello function we defined earlier, copy hello **Function Url** from hello portal.</span></span> <span data-ttu-id="748e0-128">Má následující formulář hello:</span><span class="sxs-lookup"><span data-stu-id="748e0-128">It has hello following form:</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="748e0-129">Připojit hello `name` parametr řetězce dotazu toohello.</span><span class="sxs-lookup"><span data-stu-id="748e0-129">Append hello `name` parameter toohello query string.</span></span> <span data-ttu-id="748e0-130">Použijte skutečný název pro hello `<Enter a name here>` zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="748e0-130">Use an actual name for hello `<Enter a name here>` placeholder.</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

<span data-ttu-id="748e0-131">Adresa URL hello vložit do prohlížeče a měli získat následující toohello podobné a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="748e0-131">Paste hello URL into your browser, and you should get a response similar toohello following.</span></span>

![Snímek obrazovky Chrome kartu prohlížeče s odpovědí testu](./media/functions-test-a-function/browser-test.png)

<span data-ttu-id="748e0-133">V tomto příkladu je prohlížeč Chrome hello, který zabalí hello vrátil řetězec v kódu XML.</span><span class="sxs-lookup"><span data-stu-id="748e0-133">This example is hello Chrome browser, which wraps hello returned string in XML.</span></span> <span data-ttu-id="748e0-134">Jiné prohlížeče zobrazí právě hello řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="748e0-134">Other browsers display just hello string value.</span></span>

<span data-ttu-id="748e0-135">V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:</span><span class="sxs-lookup"><span data-stu-id="748e0-135">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T07:34:59  Welcome, you are now connected toolog-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a><span data-ttu-id="748e0-136">Testování s Postman</span><span class="sxs-lookup"><span data-stu-id="748e0-136">Test with Postman</span></span>
<span data-ttu-id="748e0-137">Hello doporučuje nástroj tootest většinu funkcí je Postman, který se integruje s prohlížeč Chrome hello.</span><span class="sxs-lookup"><span data-stu-id="748e0-137">hello recommended tool tootest most of your functions is Postman, which integrates with hello Chrome browser.</span></span> <span data-ttu-id="748e0-138">tooinstall Postman, najdete v části [získat Postman](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="748e0-138">tooinstall Postman, see [Get Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="748e0-139">Postman umožňuje řídit mnoho dalších atributů požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="748e0-139">Postman provides control over many more attributes of an HTTP request.</span></span>

> [!TIP]
> <span data-ttu-id="748e0-140">Pomocí protokolu HTTP hello testování se nástroj, který jste nejvíce vyhovuje.</span><span class="sxs-lookup"><span data-stu-id="748e0-140">Use hello HTTP testing tool that you are most comfortable with.</span></span> <span data-ttu-id="748e0-141">Zde jsou některé tooPostman alternativy:</span><span class="sxs-lookup"><span data-stu-id="748e0-141">Here are some alternatives tooPostman:</span></span>  
>
> * [<span data-ttu-id="748e0-142">Fiddler</span><span class="sxs-lookup"><span data-stu-id="748e0-142">Fiddler</span></span>](http://www.telerik.com/fiddler)  
> * [<span data-ttu-id="748e0-143">Tlapa</span><span class="sxs-lookup"><span data-stu-id="748e0-143">Paw</span></span>](https://luckymarmot.com/paw)  
>
>

<span data-ttu-id="748e0-144">Funkce hello tootest s obsah žádosti v Postman:</span><span class="sxs-lookup"><span data-stu-id="748e0-144">tootest hello function with a request body in Postman:</span></span>

1. <span data-ttu-id="748e0-145">Spusťte Postman z hello **aplikace** tlačítka na hello levém horním rohu okna prohlížeče Chrome.</span><span class="sxs-lookup"><span data-stu-id="748e0-145">Start Postman from hello **Apps** button in hello upper-left corner of a Chrome browser window.</span></span>
2. <span data-ttu-id="748e0-146">Kopie vašeho **Url funkce**a vložte jej do Postman.</span><span class="sxs-lookup"><span data-stu-id="748e0-146">Copy your **Function Url**, and paste it into Postman.</span></span> <span data-ttu-id="748e0-147">Obsahuje parametr řetězce dotazu hello přístupový kód.</span><span class="sxs-lookup"><span data-stu-id="748e0-147">It includes hello access code query string parameter.</span></span>
3. <span data-ttu-id="748e0-148">Změnit metodu hello HTTP příliš**POST**.</span><span class="sxs-lookup"><span data-stu-id="748e0-148">Change hello HTTP method too**POST**.</span></span>
4. <span data-ttu-id="748e0-149">Klikněte na tlačítko **textu** > **nezpracovaná**a přidejte JSON žádosti podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="748e0-149">Click **Body** > **raw**, and add a JSON request body similar toohello following:</span></span>

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. <span data-ttu-id="748e0-150">Klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="748e0-150">Click **Send**.</span></span>

<span data-ttu-id="748e0-151">Hello následující obrázek ukazuje testování hello jednoduché echo funkce příklad v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="748e0-151">hello following image shows testing hello simple echo function example in this tutorial.</span></span>

![Snímek obrazovky Postman uživatelské rozhraní](./media/functions-test-a-function/postman-test.png)

<span data-ttu-id="748e0-153">V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:</span><span class="sxs-lookup"><span data-stu-id="748e0-153">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T08:04:51  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-hello-command-line"></a><span data-ttu-id="748e0-154">Testování pomocí cURL z příkazového řádku hello</span><span class="sxs-lookup"><span data-stu-id="748e0-154">Test with cURL from hello command line</span></span>
<span data-ttu-id="748e0-155">Často, když testujete softwaru, není nutné toolook všechny další než hello příkazového řádku toohelp ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="748e0-155">Often when you're testing software, it's not necessary toolook any further than hello command line toohelp debug your application.</span></span> <span data-ttu-id="748e0-156">Nijak se to neliší při testování funkcí.</span><span class="sxs-lookup"><span data-stu-id="748e0-156">This is no different with testing functions.</span></span> <span data-ttu-id="748e0-157">Všimněte si, že hello cURL je k dispozici ve výchozím nastavení počítače se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="748e0-157">Note that hello cURL is available by default on Linux-based systems.</span></span> <span data-ttu-id="748e0-158">V systému Windows, musíte nejdřív stáhnout a nainstalovat hello [cURL nástroj](https://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="748e0-158">On Windows, you must first download and install hello [cURL tool](https://curl.haxx.se/).</span></span>

<span data-ttu-id="748e0-159">Funkce hello tootest že definovaného dříve, kopie hello **URL funkce** z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="748e0-159">tootest hello function that we defined earlier, copy hello **Function URL** from hello portal.</span></span> <span data-ttu-id="748e0-160">Má následující formulář hello:</span><span class="sxs-lookup"><span data-stu-id="748e0-160">It has hello following form:</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="748e0-161">Toto je hello URL pro spuštění funkce.</span><span class="sxs-lookup"><span data-stu-id="748e0-161">This is hello URL for triggering your function.</span></span> <span data-ttu-id="748e0-162">Otestování tohoto pomocí hello cURL příkazu na příkazovém řádku toomake hello GET (`-G` nebo `--get`) požadavku vůči hello funkce:</span><span class="sxs-lookup"><span data-stu-id="748e0-162">Test this by using hello cURL command on hello command line toomake a GET (`-G` or `--get`) request against hello function:</span></span>

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="748e0-163">Tento příklad konkrétní vyžaduje parametr řetězce dotazu, které lze předat jako Data (`-d`) v hello cURL příkaz:</span><span class="sxs-lookup"><span data-stu-id="748e0-163">This particular example requires a query string parameter, which can be passed as Data (`-d`) in hello cURL command:</span></span>

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

<span data-ttu-id="748e0-164">Příkaz spusťte hello a najdete hello následující výstup hello funkce na příkazovém řádku hello:</span><span class="sxs-lookup"><span data-stu-id="748e0-164">Run hello command, and you see hello following output of hello function on hello command line:</span></span>

![Výstup – snímek obrazovky příkazového řádku](./media/functions-test-a-function/curl-test.png)

<span data-ttu-id="748e0-166">V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:</span><span class="sxs-lookup"><span data-stu-id="748e0-166">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-04-05T21:55:09  Welcome, you are now connected toolog-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a><span data-ttu-id="748e0-167">Testovací aktivační události objektu blob pomocí Průzkumníka úložiště</span><span class="sxs-lookup"><span data-stu-id="748e0-167">Test a blob trigger by using Storage Explorer</span></span>
<span data-ttu-id="748e0-168">Funkce aktivační události objektu blob můžete otestovat pomocí [Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="748e0-168">You can test a blob trigger function by using [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

1. <span data-ttu-id="748e0-169">V hello [portál Azure] pro vaši aplikaci funkce vytvořit jazyka C#, F # nebo JavaScript funkci aktivační události objektu blob.</span><span class="sxs-lookup"><span data-stu-id="748e0-169">In hello [Azure portal] for your function app, create a C#, F# or JavaScript blob trigger function.</span></span> <span data-ttu-id="748e0-170">Nastavit hello toomonitor toohello název cesty kontejnerech objektů blob.</span><span class="sxs-lookup"><span data-stu-id="748e0-170">Set hello path toomonitor toohello name of your blob container.</span></span> <span data-ttu-id="748e0-171">Například:</span><span class="sxs-lookup"><span data-stu-id="748e0-171">For example:</span></span>

        files
2. <span data-ttu-id="748e0-172">Klikněte na tlačítko hello  **+**  tlačítko tooselect nebo vytvořit účet úložiště hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="748e0-172">Click hello **+** button tooselect or create hello storage account you want toouse.</span></span> <span data-ttu-id="748e0-173">Poté klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="748e0-173">Then click **Create**.</span></span>
3. <span data-ttu-id="748e0-174">Vytvořte textový soubor s hello následující text a uložte jej:</span><span class="sxs-lookup"><span data-stu-id="748e0-174">Create a text file with hello following text, and save it:</span></span>

        A text file for blob trigger function testing.
4. <span data-ttu-id="748e0-175">Spustit [Azure Storage Explorer](http://storageexplorer.com/)a připojte toohello kontejneru objektů blob v účtu úložiště hello monitorovány.</span><span class="sxs-lookup"><span data-stu-id="748e0-175">Run [Azure Storage Explorer](http://storageexplorer.com/), and connect toohello blob container in hello storage account being monitored.</span></span>
5. <span data-ttu-id="748e0-176">Klikněte na tlačítko **nahrát** tooupload hello textového souboru.</span><span class="sxs-lookup"><span data-stu-id="748e0-176">Click **Upload** tooupload hello text file.</span></span>

    ![Snímek obrazovky Průzkumníka úložiště](./media/functions-test-a-function/azure-storage-explorer-test.png)

<span data-ttu-id="748e0-178">Kód funkce aktivační události objektu blob výchozí Hello sestavy hello zpracování objektu blob hello hello protokolů:</span><span class="sxs-lookup"><span data-stu-id="748e0-178">hello default blob trigger function code reports hello processing of hello blob in hello logs:</span></span>

    2016-03-24T11:30:10  Welcome, you are now connected toolog-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a><span data-ttu-id="748e0-179">Testování funkce v rámci funkcí</span><span class="sxs-lookup"><span data-stu-id="748e0-179">Test a function within functions</span></span>
<span data-ttu-id="748e0-180">Hello portálu Azure Functions je určená toolet testování HTTP a časovač aktivuje funkce.</span><span class="sxs-lookup"><span data-stu-id="748e0-180">hello Azure Functions portal is designed toolet you test HTTP and timer triggered functions.</span></span> <span data-ttu-id="748e0-181">Můžete také vytvořit funkce tootrigger dalších funkcí, které jsou testování.</span><span class="sxs-lookup"><span data-stu-id="748e0-181">You can also create functions tootrigger other functions that you are testing.</span></span>

### <a name="test-with-hello-functions-portal-run-button"></a><span data-ttu-id="748e0-182">Testování s tlačítko Spustit hello funkce portálu</span><span class="sxs-lookup"><span data-stu-id="748e0-182">Test with hello Functions portal Run button</span></span>
<span data-ttu-id="748e0-183">poskytuje Hello portál **spustit** tlačítko, které můžete použít toodo některé omezené testování.</span><span class="sxs-lookup"><span data-stu-id="748e0-183">hello portal provides a **Run** button that you can use toodo some limited testing.</span></span> <span data-ttu-id="748e0-184">Obsah žádosti můžete zadat pomocí tlačítka hello, ale nelze zadat parametrů řetězce dotazu nebo aktualizovat hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="748e0-184">You can provide a request body by using hello button, but you can't provide query string parameters or update request headers.</span></span>

<span data-ttu-id="748e0-185">Testování funkce aktivační událost hello HTTP jsme vytvořili předtím přidáním toohello podobné řetězec JSON následující hello **text žádosti** pole.</span><span class="sxs-lookup"><span data-stu-id="748e0-185">Test hello HTTP trigger function we created earlier by adding a JSON string similar toohello following in hello **Request body** field.</span></span> <span data-ttu-id="748e0-186">Pak klikněte na tlačítko hello **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="748e0-186">Then click hello **Run** button.</span></span>

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

<span data-ttu-id="748e0-187">V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:</span><span class="sxs-lookup"><span data-stu-id="748e0-187">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T08:03:12  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a><span data-ttu-id="748e0-188">Testování s aktivační událost časovače</span><span class="sxs-lookup"><span data-stu-id="748e0-188">Test with a timer trigger</span></span>
<span data-ttu-id="748e0-189">Některé funkce nelze testovat adekvátní nástroje hello již bylo zmíněno dříve.</span><span class="sxs-lookup"><span data-stu-id="748e0-189">Some functions can't be adequately tested with hello tools mentioned previously.</span></span> <span data-ttu-id="748e0-190">Představte si třeba funkci fronty aktivační událost, která se spustí v případě, že je vyřazeno zprávu do [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="748e0-190">For example, consider a queue trigger function that runs when a message is dropped into [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span> <span data-ttu-id="748e0-191">Lze vždy psát kód toodrop zprávu do fronty, a příklady v konzolovém projektu se poskytuje později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="748e0-191">You can always write code toodrop a message into your queue, and an example of this in a console project is provided later in this article.</span></span> <span data-ttu-id="748e0-192">Je však jiný přístup, které můžete použít, který testuje funkce přímo.</span><span class="sxs-lookup"><span data-stu-id="748e0-192">However, there is another approach you can use that tests functions directly.</span></span>  

<span data-ttu-id="748e0-193">Můžete použít aktivační událost časovače nakonfigurovaný s frontou výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="748e0-193">You can use a timer trigger configured with a queue output binding.</span></span> <span data-ttu-id="748e0-194">Tento časovač aktivační kód pak můžete napsat hello zkušební zprávy toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="748e0-194">That timer trigger code can then write hello test messages toohello queue.</span></span> <span data-ttu-id="748e0-195">Tato část vás provede příklad.</span><span class="sxs-lookup"><span data-stu-id="748e0-195">This section walks through an example.</span></span>

<span data-ttu-id="748e0-196">Další podrobné informace o používání vazby s Azure Functions najdete v tématu hello [referenční informace pro vývojáře Azure Functions](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="748e0-196">For more in-depth information on using bindings with Azure Functions, see hello [Azure Functions developer reference](functions-reference.md).</span></span>

#### <a name="create-a-queue-trigger-for-testing"></a><span data-ttu-id="748e0-197">Vytvořit aktivační událost fronty pro testování</span><span class="sxs-lookup"><span data-stu-id="748e0-197">Create a queue trigger for testing</span></span>
<span data-ttu-id="748e0-198">toodemonstrate tento přístup, jsme nejprve vytvořit funkci fronty aktivační událost, která chceme tootest pro frontu s názvem `queue-newusers`.</span><span class="sxs-lookup"><span data-stu-id="748e0-198">toodemonstrate this approach, we first create a queue trigger function that we want tootest for a queue named `queue-newusers`.</span></span> <span data-ttu-id="748e0-199">Tato funkce zpracovává informace o názvu a adresy do fronty úložiště pro nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="748e0-199">This function processes name and address information dropped into Queue storage for a new user.</span></span>

> [!NOTE]
> <span data-ttu-id="748e0-200">Pokud používáte jiný fronty názvu, ujistěte se, můžete použít název hello vyhovuje toohello [pojmenování front a MetaData](https://msdn.microsoft.com/library/dd179349.aspx) pravidla.</span><span class="sxs-lookup"><span data-stu-id="748e0-200">If you use a different queue name, make sure hello name you use conforms toohello [Naming Queues and MetaData](https://msdn.microsoft.com/library/dd179349.aspx) rules.</span></span> <span data-ttu-id="748e0-201">Jinak dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="748e0-201">Otherwise, you get an error.</span></span>
>
>

1. <span data-ttu-id="748e0-202">V hello [portál Azure] funkce aplikace, klikněte na tlačítko **novou funkci** > **QueueTrigger - C#**.</span><span class="sxs-lookup"><span data-stu-id="748e0-202">In hello [Azure portal] for your function app, click **New Function** > **QueueTrigger - C#**.</span></span>
2. <span data-ttu-id="748e0-203">Zadejte toobe název fronty hello monitorovat pomocí funkce fronty hello:</span><span class="sxs-lookup"><span data-stu-id="748e0-203">Enter hello queue name toobe monitored by hello queue function:</span></span>

        queue-newusers
3. <span data-ttu-id="748e0-204">Klikněte na tlačítko hello  **+**  tlačítko tooselect nebo vytvořit účet úložiště hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="748e0-204">Click hello **+** button tooselect or create hello storage account you want toouse.</span></span> <span data-ttu-id="748e0-205">Poté klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="748e0-205">Then click **Create**.</span></span>
4. <span data-ttu-id="748e0-206">Nechte toto okno prohlížeče portálu otevřené, tak můžete monitorovat hello položky protokolu pro kód šablony funkce fronty výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="748e0-206">Leave this portal browser window open, so you can monitor hello log entries for hello default queue function template code.</span></span>

#### <a name="create-a-timer-trigger-toodrop-a-message-in-hello-queue"></a><span data-ttu-id="748e0-207">Vytvořte toodrop aktivační události časovač zprávu ve frontě hello</span><span class="sxs-lookup"><span data-stu-id="748e0-207">Create a timer trigger toodrop a message in hello queue</span></span>
1. <span data-ttu-id="748e0-208">Otevřete hello [portál Azure] v nové okno prohlížeče a přejděte tooyour funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="748e0-208">Open hello [Azure portal] in a new browser window, and navigate tooyour function app.</span></span>
2. <span data-ttu-id="748e0-209">Klikněte na tlačítko **novou funkci** > **TimerTrigger - C#**.</span><span class="sxs-lookup"><span data-stu-id="748e0-209">Click **New Function** > **TimerTrigger - C#**.</span></span> <span data-ttu-id="748e0-210">Zadejte tooset výraz cron jak často hello časovače kód testuje funkce fronty.</span><span class="sxs-lookup"><span data-stu-id="748e0-210">Enter a cron expression tooset how often hello timer code tests your queue function.</span></span> <span data-ttu-id="748e0-211">Poté klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="748e0-211">Then click **Create**.</span></span> <span data-ttu-id="748e0-212">Pokud chcete testovací toorun hello každých 30 sekund, můžete použít následující hello [výraz CRON](https://wikipedia.org/wiki/Cron#CRON_expression):</span><span class="sxs-lookup"><span data-stu-id="748e0-212">If you want hello test toorun every 30 seconds, you can use hello following [CRON expression](https://wikipedia.org/wiki/Cron#CRON_expression):</span></span>

        */30 * * * * *
3. <span data-ttu-id="748e0-213">Klikněte na tlačítko hello **integrací** kartě nové aktivační události časovače.</span><span class="sxs-lookup"><span data-stu-id="748e0-213">Click hello **Integrate** tab for your new timer trigger.</span></span>
4. <span data-ttu-id="748e0-214">V části **výstup**, klikněte na tlačítko **+ nový výstupní**.</span><span class="sxs-lookup"><span data-stu-id="748e0-214">Under **Output**, click **+ New Output**.</span></span> <span data-ttu-id="748e0-215">Pak klikněte na tlačítko **fronty** a **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="748e0-215">Then click **queue** and **Select**.</span></span>
5. <span data-ttu-id="748e0-216">Název poznámky hello použijete pro hello **objekt fronty zpráv**.</span><span class="sxs-lookup"><span data-stu-id="748e0-216">Note hello name you use for hello **queue message object**.</span></span> <span data-ttu-id="748e0-217">Můžete použít v kódu funkce časovače hello.</span><span class="sxs-lookup"><span data-stu-id="748e0-217">You use this in hello timer function code.</span></span>

        myQueue
6. <span data-ttu-id="748e0-218">Zadejte název fronty hello, kde je odeslána zpráva hello:</span><span class="sxs-lookup"><span data-stu-id="748e0-218">Enter hello queue name where hello message is sent:</span></span>

        queue-newusers
7. <span data-ttu-id="748e0-219">Klikněte na tlačítko hello  **+**  tlačítko účet úložiště hello tooselect jste použili předtím se aktivační událostí hello fronty.</span><span class="sxs-lookup"><span data-stu-id="748e0-219">Click hello **+** button tooselect hello storage account you used previously with hello queue trigger.</span></span> <span data-ttu-id="748e0-220">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="748e0-220">Then click **Save**.</span></span>
8. <span data-ttu-id="748e0-221">Klikněte na tlačítko hello **vývoj** kartě aktivační události časovače.</span><span class="sxs-lookup"><span data-stu-id="748e0-221">Click hello **Develop** tab for your timer trigger.</span></span>
9. <span data-ttu-id="748e0-222">Hello následující kód pro funkce časovače hello C#, můžete použít, dokud jste použili hello stejné fronty zpráv název objektu uvedena výše.</span><span class="sxs-lookup"><span data-stu-id="748e0-222">You can use hello following code for hello C# timer function, as long as you used hello same queue message object name shown earlier.</span></span> <span data-ttu-id="748e0-223">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="748e0-223">Then click **Save**.</span></span>

    ```cs
    using System;

    public static void Run(TimerInfo myTimer, out String myQueue, TraceWriter log)
    {
        String newUser =
        "{\"name\":\"User testing from C# timer function\",\"address\":\"XYZ\"}";

        log.Verbose($"C# Timer trigger function executed at: {DateTime.Now}");   
        log.Verbose($"{newUser}");   

        myQueue = newUser;
    }
    ```

<span data-ttu-id="748e0-224">V tomto okamžiku hello časovače funkce jazyka C# spustí každých 30 sekund, pokud jste použili výraz cron příklad hello.</span><span class="sxs-lookup"><span data-stu-id="748e0-224">At this point, hello C# timer function executes every 30 seconds if you used hello example cron expression.</span></span> <span data-ttu-id="748e0-225">Hello protokoly pro funkci časovače hello sestav každé spuštění:</span><span class="sxs-lookup"><span data-stu-id="748e0-225">hello logs for hello timer function report each execution:</span></span>

    2016-03-24T10:27:02  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

<span data-ttu-id="748e0-226">V okně prohlížeče hello hello fronty funkce můžete zjistit každou zprávu zpracovává:</span><span class="sxs-lookup"><span data-stu-id="748e0-226">In hello browser window for hello queue function, you can see each message being processed:</span></span>

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a><span data-ttu-id="748e0-227">Testování funkce pomocí kódu</span><span class="sxs-lookup"><span data-stu-id="748e0-227">Test a function with code</span></span>
<span data-ttu-id="748e0-228">Může být nutné toocreate externí aplikace nebo framework tootest funkcí.</span><span class="sxs-lookup"><span data-stu-id="748e0-228">You may need toocreate an external application or framework tootest your functions.</span></span>

### <a name="test-an-http-trigger-function-with-code-nodejs"></a><span data-ttu-id="748e0-229">Testování funkce aktivace protokolu HTTP s kódem: Node.js</span><span class="sxs-lookup"><span data-stu-id="748e0-229">Test an HTTP trigger function with code: Node.js</span></span>
<span data-ttu-id="748e0-230">Můžete vytvořit tooexecute aplikace Node.js tootest požadavku HTTP funkce.</span><span class="sxs-lookup"><span data-stu-id="748e0-230">You can use a Node.js app tooexecute an HTTP request tootest your function.</span></span>
<span data-ttu-id="748e0-231">Ujistěte se, že tooset:</span><span class="sxs-lookup"><span data-stu-id="748e0-231">Make sure tooset:</span></span>

* <span data-ttu-id="748e0-232">Hello `host` hello požadavek možnosti tooyour funkce aplikace hostiteli.</span><span class="sxs-lookup"><span data-stu-id="748e0-232">hello `host` in hello request options tooyour function app host.</span></span>
* <span data-ttu-id="748e0-233">Název funkce v hello `path`.</span><span class="sxs-lookup"><span data-stu-id="748e0-233">Your function name in hello `path`.</span></span>
* <span data-ttu-id="748e0-234">Přístupový kód (`<your code>`) v hello `path`.</span><span class="sxs-lookup"><span data-stu-id="748e0-234">Your access code (`<your code>`) in hello `path`.</span></span>

<span data-ttu-id="748e0-235">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="748e0-235">Code example:</span></span>

```javascript
var http = require("http");

var nameQueryString = "name=Wes%20Query%20String%20Test%20From%20Node.js";

var nameBodyJSON = {
    name : "Wes testing with Node.JS code",
    address : "Dallas, T.X. 75201"
};

var bodyString = JSON.stringify(nameBodyJSON);

var options = {
  host: "functions841def78.azurewebsites.net",
  //path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9&" + nameQueryString,
  path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9",
  method: "POST",
  headers : {
      "Content-Type":"application/json",
      "Content-Length": Buffer.byteLength(bodyString)
    }    
};

callback = function(response) {
  var str = ""
  response.on("data", function (chunk) {
    str += chunk;
  });

  response.on("end", function () {
    console.log(str);
  });
}

var req = http.request(options, callback);
console.log("*** Sending name and address in body ***");
console.log(bodyString);
req.end(bodyString);
```


<span data-ttu-id="748e0-236">Výstup:</span><span class="sxs-lookup"><span data-stu-id="748e0-236">Output:</span></span>

    C:\Users\Wesley\testing\Node.js>node testHttpTriggerExample.js
    *** Sending name and address in body ***
    {"name" : "Wes testing with Node.JS code","address" : "Dallas, T.X. 75201"}
    Hello Wes testing with Node.JS code
    hello address you provided is Dallas, T.X. 75201

<span data-ttu-id="748e0-237">V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:</span><span class="sxs-lookup"><span data-stu-id="748e0-237">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T08:08:55  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a><span data-ttu-id="748e0-238">Testování funkce aktivační událost fronty s kódem: C#</span><span class="sxs-lookup"><span data-stu-id="748e0-238">Test a queue trigger function with code: C#</span></span> #
<span data-ttu-id="748e0-239">Jsme již bylo zmíněno dříve, můžete otestovat aktivační procedury fronty pomocí kódu toodrop zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="748e0-239">We mentioned earlier that you can test a queue trigger by using code toodrop a message in your queue.</span></span> <span data-ttu-id="748e0-240">Následující příklad kódu Hello podle hello C# kód uvedený v hello [Začínáme s Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="748e0-240">hello following example code is based on hello C# code presented in hello [Getting started with Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) tutorial.</span></span> <span data-ttu-id="748e0-241">Kód pro jiné jazyky je také k dispozici prostřednictvím tohoto připojení.</span><span class="sxs-lookup"><span data-stu-id="748e0-241">Code for other languages is also available from that link.</span></span>

<span data-ttu-id="748e0-242">tootest tento kód v konzolovou aplikaci, musíte:</span><span class="sxs-lookup"><span data-stu-id="748e0-242">tootest this code in a console app, you must:</span></span>

* <span data-ttu-id="748e0-243">[Konfigurace připojovacího řetězce úložiště v souboru app.config hello](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="748e0-243">[Configure your storage connection string in hello app.config file](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>
* <span data-ttu-id="748e0-244">Předat `name` a `address` jako parametry toohello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="748e0-244">Pass a `name` and `address` as parameters toohello app.</span></span> <span data-ttu-id="748e0-245">Například, `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`.</span><span class="sxs-lookup"><span data-stu-id="748e0-245">For example, `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`.</span></span> <span data-ttu-id="748e0-246">(Tento kód přijímá hello název a adresu pro nového uživatele jako argumenty příkazového řádku za běhu.)</span><span class="sxs-lookup"><span data-stu-id="748e0-246">(This code accepts hello name and address for a new user as command-line arguments during runtime.)</span></span>

<span data-ttu-id="748e0-247">Příklad C# kódu:</span><span class="sxs-lookup"><span data-stu-id="748e0-247">Example C# code:</span></span>

```cs
static void Main(string[] args)
{
    string name = null;
    string address = null;
    string queueName = "queue-newusers";
    string JSON = null;

    if (args.Length > 0)
    {
        name = args[0];
    }
    if (args.Length > 1)
    {
        address = args[1];
    }

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["StorageConnectionString"]);

    // Create hello queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference tooa queue
    CloudQueue queue = queueClient.GetQueueReference(queueName);

    // Create hello queue if it doesn't already exist
    queue.CreateIfNotExists();

    // Create a message and add it toohello queue.
    if (name != null)
    {
        if (address != null)
            JSON = String.Format("{{\"name\":\"{0}\",\"address\":\"{1}\"}}", name, address);
        else
            JSON = String.Format("{{\"name\":\"{0}\"}}", name);
    }

    Console.WriteLine("Adding message too" + queueName + "...");
    Console.WriteLine(JSON);

    CloudQueueMessage message = new CloudQueueMessage(JSON);
    queue.AddMessage(message);
}
```

<span data-ttu-id="748e0-248">V okně prohlížeče hello hello fronty funkce můžete zjistit každou zprávu zpracovává:</span><span class="sxs-lookup"><span data-stu-id="748e0-248">In hello browser window for hello queue function, you can see each message being processed:</span></span>

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[portál Azure]: https://portal.azure.com
