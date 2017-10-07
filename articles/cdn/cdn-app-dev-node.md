---
title: "aaaGet práce s hello Azure CDN SDK pro Node.js | Microsoft Docs"
description: "Zjistěte, jak toomanage aplikace Node.js toowrite Azure CDN."
services: cdn
documentationcenter: nodejs
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c4bb6a61-de3d-4f0c-9dca-202554c43dfa
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6c805e5fb8e0b471e8b248cb2f4b29efd6c85940
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="5d6fb-103">Začínáme s vývojem pro Azure CDN</span><span class="sxs-lookup"><span data-stu-id="5d6fb-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5d6fb-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="5d6fb-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="5d6fb-105">.NET</span><span class="sxs-lookup"><span data-stu-id="5d6fb-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="5d6fb-106">Můžete použít hello [Azure CDN SDK pro Node.js](https://www.npmjs.com/package/azure-arm-cdn) tooautomate vytváření a Správa profilů CDN a koncové body.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-106">You can use hello [Azure CDN SDK for Node.js](https://www.npmjs.com/package/azure-arm-cdn) tooautomate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="5d6fb-107">Tento kurz vás provede hello vytvoření jednoduché konzolovou aplikaci Node.js, která ukazuje několik operací hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-107">This tutorial walks through hello creation of a simple Node.js console application that demonstrates several of hello available operations.</span></span>  <span data-ttu-id="5d6fb-108">V tomto kurzu není určený toodescribe všechny aspekty hello Azure CDN SDK pro Node.js podrobně.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-108">This tutorial is not intended toodescribe all aspects of hello Azure CDN SDK for Node.js in detail.</span></span>

<span data-ttu-id="5d6fb-109">toocomplete v tomto kurzu byste již měli mít [Node.js](http://www.nodejs.org) **4.x.x** nebo vyšší nainstalována a nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-109">toocomplete this tutorial, you should already have [Node.js](http://www.nodejs.org) **4.x.x** or higher installed and configured.</span></span>  <span data-ttu-id="5d6fb-110">Můžete použít libovolný textový editor, který chcete toocreate aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-110">You can use any text editor you want toocreate your Node.js application.</span></span>  <span data-ttu-id="5d6fb-111">toowrite tento kurz používá I [Visual Studio Code](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="5d6fb-111">toowrite this tutorial, I used [Visual Studio Code](https://code.visualstudio.com).</span></span>  

> [!TIP]
> <span data-ttu-id="5d6fb-112">Hello [dokončený projekt z tohoto kurzu](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) je k dispozici ke stažení na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-112">hello [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a><span data-ttu-id="5d6fb-113">Vytvořte projekt a přidejte NPM závislosti</span><span class="sxs-lookup"><span data-stu-id="5d6fb-113">Create your project and add NPM dependencies</span></span>
<span data-ttu-id="5d6fb-114">Teď, když jsme vytvořili skupinu prostředků pro naše profily CDN a daného naše profilů CDN toomanage oprávnění aplikace Azure AD a koncové body v rámci dané skupiny, můžeme začít vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-114">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission toomanage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="5d6fb-115">Vytvořte složku toostore vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-115">Create a folder toostore your application.</span></span>  <span data-ttu-id="5d6fb-116">Z konzoly nástroje Node.js hello v aktuální cestě nastavte vaše aktuální umístění toothis novou složku a inicializovat spuštěním projektu:</span><span class="sxs-lookup"><span data-stu-id="5d6fb-116">From a console with hello Node.js tools in your current path, set your current location toothis new folder and initialize your project by executing:</span></span>

    npm init

<span data-ttu-id="5d6fb-117">Pak bude zobrazovat řadu otázek tooinitialize projektu.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-117">You will then be presented a series of questions tooinitialize your project.</span></span>  <span data-ttu-id="5d6fb-118">Pro **vstupní bod**, tento kurz používá *app.js*.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-118">For **entry point**, this tutorial uses *app.js*.</span></span>  <span data-ttu-id="5d6fb-119">Můžete zobrazit Moje jiné možnosti hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-119">You can see my other choices in hello following example.</span></span>

![Init – výstupní NPM](./media/cdn-app-dev-node/cdn-npm-init.png)

<span data-ttu-id="5d6fb-121">Naše projektu je nyní inicializován s *packages.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-121">Our project is now initialized with a *packages.json* file.</span></span>  <span data-ttu-id="5d6fb-122">Naše projektu přechází toouse některé knihovny Azure obsažené v NPM balíčky.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-122">Our project is going toouse some Azure libraries contained in NPM packages.</span></span>  <span data-ttu-id="5d6fb-123">Použijeme hello Runtime klienta Azure pro platformu Node.js (ms-rest azure) a hello Azure CDN Klientská knihovna pro Node.js (azure arm cd).</span><span class="sxs-lookup"><span data-stu-id="5d6fb-123">We'll use hello Azure Client Runtime for Node.js (ms-rest-azure) and hello Azure CDN Client Library for Node.js (azure-arm-cd).</span></span>  <span data-ttu-id="5d6fb-124">Umožňuje přidat tyto toohello projekt jako závislosti.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-124">Let's add those toohello project as dependencies.</span></span>

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

<span data-ttu-id="5d6fb-125">Po hello balíčky jsou provést instalaci, hello *package.json* soubor by měl vypadat podobně jako příklad toothis (verze čísla se mohou lišit):</span><span class="sxs-lookup"><span data-stu-id="5d6fb-125">After hello packages are done installing, hello *package.json* file should look similar toothis example (version numbers may differ):</span></span>

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

<span data-ttu-id="5d6fb-126">Nakonec pomocí textového editoru, vytvořte prázdný textový soubor a uložit ho hello kořenové složky naše projektu jako *app.js*.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-126">Finally, using your text editor, create a blank text file and save it in hello root of our project folder as *app.js*.</span></span>  <span data-ttu-id="5d6fb-127">Nemohli jsme teď připravena toobegin psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-127">We're now ready toobegin writing code.</span></span>

## <a name="requires-constants-authentication-and-structure"></a><span data-ttu-id="5d6fb-128">Vyžaduje, konstanty, ověřování a struktury</span><span class="sxs-lookup"><span data-stu-id="5d6fb-128">Requires, constants, authentication, and structure</span></span>
<span data-ttu-id="5d6fb-129">S *app.js* otevřete v našem editoru, Pojďme hello základní struktura naše programu zapsána.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-129">With *app.js* open in our editor, let's get hello basic structure of our program written.</span></span>

1. <span data-ttu-id="5d6fb-130">Přidejte hello "vyžaduje" pro naše balíčky NPM v horní části hello s hello následující:</span><span class="sxs-lookup"><span data-stu-id="5d6fb-130">Add hello "requires" for our NPM packages at hello top with hello following:</span></span>
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. <span data-ttu-id="5d6fb-131">Potřebujeme toodefine některé konstanty, který bude používat naše metody.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-131">We need toodefine some constants our methods will use.</span></span>  <span data-ttu-id="5d6fb-132">Přidejte následující hello.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-132">Add hello following.</span></span>  <span data-ttu-id="5d6fb-133">Být jisti tooreplace hello zástupné symboly, včetně hello  **&lt;lomené závorky&gt;**, s vlastní hodnoty, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-133">Be sure tooreplace hello placeholders, including hello **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";
   
    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. <span data-ttu-id="5d6fb-134">V dalším kroku jsme budete doložit hello CDN správy klienta a dejte mu naše přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-134">Next, we'll instantiate hello CDN management client and give it our credentials.</span></span>
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="5d6fb-135">Pokud používáte ověřování jednotlivých uživatelů, bude vypadat tyto dva řádky mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-135">If you are using individual user authentication, these two lines will look slightly different.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="5d6fb-136">Tato ukázka kódu použijte, pouze pokud zvolíte ověřování jednotlivých uživatelů toohave místo objekt služby.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-136">Only use this code sample if you are choosing toohave individual user authentication instead of a service principal.</span></span>  <span data-ttu-id="5d6fb-137">Být opatrní tooguard pověření jednotlivé uživatele a bezpečně je tajný.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-137">Be careful tooguard your individual user credentials and keep them secret.</span></span>
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="5d6fb-138">Být jisti tooreplace hello položky v  **&lt;lomené závorky&gt;**  s hello opravte informace.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-138">Be sure tooreplace hello items in **&lt;angle brackets&gt;** with hello correct information.</span></span>  <span data-ttu-id="5d6fb-139">Pro `<redirect URI>`, použijte identifikátor URI, které jste zadali při registraci aplikace hello ve službě Azure AD hello přesměrování.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-139">For `<redirect URI>`, use hello redirect URI you entered when you registered hello application in Azure AD.</span></span>
4. <span data-ttu-id="5d6fb-140">Naše konzolovou aplikaci Node.js přechází tootake některé parametry příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-140">Our Node.js console application is going tootake some command-line parameters.</span></span>  <span data-ttu-id="5d6fb-141">Umožňuje ověřit, že aspoň jeden parametr byl předán.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-141">Let's validate that at least one parameter was passed.</span></span>
   
   ```javascript
   //Collect command-line parameters
   var parms = process.argv.slice(2);
   
   //Do we have parameters?
   if(parms == null || parms.length == 0)
   {
       console.log("Not enough parameters!");
       console.log("Valid commands are list, delete, create, and purge.");
       process.exit(1);
   }
   ```
5. <span data-ttu-id="5d6fb-142">Díky které nám toohello hlavní část naší programu, které jsme větev vypnout tooother funkce založené na jaké parametry byly předány.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-142">That brings us toohello main part of our program, where we branch off tooother functions based on what parameters were passed.</span></span>
   
    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;
   
        case "create":
            cdnCreate();
            break;
   
        case "delete":
            cdnDelete();
            break;
   
        case "purge":
            cdnPurge();
            break;
   
        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```
6. <span data-ttu-id="5d6fb-143">Na několika místech v našem programu budeme potřebovat toomake hello správné počet parametrů byly předány v a zobrazit pomoc, pokud jejich nevypadají správné.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-143">At several places in our program, we'll need toomake sure hello right number of parameters were passed in and display some help if they don't look correct.</span></span>  <span data-ttu-id="5d6fb-144">Umožňuje vytvořit funkce toodo který.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-144">Let's create functions toodo that.</span></span>
   
   ```javascript
   function requireParms(parmCount) {
       if(parms.length < parmCount) {
           usageHelp(parms[0].toLowerCase());
           process.exit(1);
       }
   }
   
   function usageHelp(cmd) {
       console.log("Usage for " + cmd + ":");
       switch(cmd)
       {
           case "list":
               console.log("list profiles");
               console.log("list endpoints <profile name>");
               break;
   
           case "create":
               console.log("create profile <profile name>");
               console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
               break;
   
           case "delete":
               console.log("delete profile <profile name>");
               console.log("delete endpoint <profile name> <endpoint name>");
               break;
   
           case "purge":
               console.log("purge <profile name> <endpoint name> <path>");
               break;
   
           default:
               console.log("Invalid command.");
       }
   }
   ```
7. <span data-ttu-id="5d6fb-145">Nakonec jsou asynchronní, hello funkce, které budeme používat na klienta pro správu CDN hello, takže potřebují metoda toocall zpět v případě, že hotovi.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-145">Finally, hello functions we'll be using on hello CDN management client are asynchronous, so they need a method toocall back when they're done.</span></span>  <span data-ttu-id="5d6fb-146">Můžeme si ho, že můžete zobrazit výstup hello hello CDN správy klienta (pokud existuje) a hello program řádně ukončit.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-146">Let's make one that can display hello output from hello CDN management client (if any) and exit hello program gracefully.</span></span>
   
    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

<span data-ttu-id="5d6fb-147">Teď, když je zapsán hello základní struktura naše programu, vytvoříme by měl hello funkce volané založené na našem parametrů.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-147">Now that hello basic structure of our program is written, we should create hello functions called based on our parameters.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="5d6fb-148">Koncové body a seznam profilů CDN</span><span class="sxs-lookup"><span data-stu-id="5d6fb-148">List CDN profiles and endpoints</span></span>
<span data-ttu-id="5d6fb-149">Začněme s toolist kódu, naše stávající profily a koncové body.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-149">Let's start with code toolist our existing profiles and endpoints.</span></span>  <span data-ttu-id="5d6fb-150">Vlastní kód komentář zadejte hello očekávána syntaxe, abychom věděli, kde každý parametr přejde.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-150">My code comments provide hello expected syntax so we know where each parameter goes.</span></span>

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="5d6fb-151">Vytvoření profilů CDN a koncové body</span><span class="sxs-lookup"><span data-stu-id="5d6fb-151">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="5d6fb-152">Dále jsme budete napište hello funkce toocreate profily a koncové body.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-152">Next, we'll write hello functions toocreate profiles and endpoints.</span></span>

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a><span data-ttu-id="5d6fb-153">Vyprázdnění koncového bodu</span><span class="sxs-lookup"><span data-stu-id="5d6fb-153">Purge an endpoint</span></span>
<span data-ttu-id="5d6fb-154">Za předpokladu, že byl vytvořen koncový bod hello, běžných úloh, může chceme tooperform v našem programu je mazání obsahu v našem koncový bod.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-154">Assuming hello endpoint has been created, one common task that we might want tooperform in our program is purging content in our endpoint.</span></span>

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="5d6fb-155">Odstranit profily CDN a koncové body</span><span class="sxs-lookup"><span data-stu-id="5d6fb-155">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="5d6fb-156">Hello poslední funkci, kterou jsme bude obsahovat odstraní koncových bodů a profily.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-156">hello last function we will include deletes endpoints and profiles.</span></span>

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-hello-program"></a><span data-ttu-id="5d6fb-157">Spuštění programu hello</span><span class="sxs-lookup"><span data-stu-id="5d6fb-157">Running hello program</span></span>
<span data-ttu-id="5d6fb-158">Můžeme spouštět naše program Node.js pomocí našich oblíbených ladicí program nyní nebo v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-158">We can now execute our Node.js program using our favorite debugger or at hello console.</span></span>

> [!TIP]
> <span data-ttu-id="5d6fb-159">Pokud používáte Visual Studio Code jako ladicí program, budete potřebovat tooset do vašeho prostředí toopass v hello parametry příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-159">If you're using Visual Studio Code as your debugger, you'll need tooset up your environment toopass in hello command-line parameters.</span></span>  <span data-ttu-id="5d6fb-160">Visual Studio Code tomu v hello **lanuch.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-160">Visual Studio Code does this in hello **lanuch.json** file.</span></span>  <span data-ttu-id="5d6fb-161">Vyhledejte vlastnost s názvem **argumentů** a přidejte řetězcové hodnoty pro parametry, pole, aby vypadal podobně jako toothis: `"args": ["list", "profiles"]`.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-161">Look for a property named **args** and add an array of string values for your parameters, so that it looks similar toothis:  `"args": ["list", "profiles"]`.</span></span>
> 
> 

<span data-ttu-id="5d6fb-162">Začněme tak, že uvedete naše profily.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-162">Let's start by listing our profiles.</span></span>

![Seznam profilů](./media/cdn-app-dev-node/cdn-list-profiles.png)

<span data-ttu-id="5d6fb-164">My zpět prázdné pole.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-164">We got back an empty array.</span></span>  <span data-ttu-id="5d6fb-165">Vzhledem k tomu, že nemáme žádné profily v naší skupiny prostředků, která se očekává.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-165">Since we don't have any profiles in our resource group, that's expected.</span></span>  <span data-ttu-id="5d6fb-166">Nyní vytvoříme profil.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-166">Let's create a profile now.</span></span>

![Vytvoření profilu](./media/cdn-app-dev-node/cdn-create-profile.png)

<span data-ttu-id="5d6fb-168">Nyní Pojďme přidat koncový bod.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-168">Now, let's add an endpoint.</span></span>

![Vytvoření koncového bodu](./media/cdn-app-dev-node/cdn-create-endpoint.png)

<span data-ttu-id="5d6fb-170">Umožňuje odstranit nakonec naše profil.</span><span class="sxs-lookup"><span data-stu-id="5d6fb-170">Finally, let's delete our profile.</span></span>

![Odstranit profil](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a><span data-ttu-id="5d6fb-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d6fb-172">Next Steps</span></span>
<span data-ttu-id="5d6fb-173">Projekt hello dokončit toosee z tohoto návodu [stažení ukázky hello](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span><span class="sxs-lookup"><span data-stu-id="5d6fb-173">toosee hello completed project from this walkthrough, [download hello sample](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span></span>

<span data-ttu-id="5d6fb-174">referenční dokumentace hello toosee pro hello Azure CDN SDK pro Node.js hello zobrazení [odkaz](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span><span class="sxs-lookup"><span data-stu-id="5d6fb-174">toosee hello reference for hello Azure CDN SDK for Node.js, view hello [reference](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span></span>

<span data-ttu-id="5d6fb-175">Další dokumentaci toofind na hello Azure SDK pro Node.js hello zobrazení [úplné referenční](http://azure.github.io/azure-sdk-for-node/).</span><span class="sxs-lookup"><span data-stu-id="5d6fb-175">toofind additional documentation on hello Azure SDK for Node.js, view hello [full reference](http://azure.github.io/azure-sdk-for-node/).</span></span>

<span data-ttu-id="5d6fb-176">Spravovat prostředky CDN s [prostředí PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5d6fb-176">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

