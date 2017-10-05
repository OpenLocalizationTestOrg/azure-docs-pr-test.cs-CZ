---
title: "Začínáme s Azure CDN SDK pro Node.js | Microsoft Docs"
description: "Informace o psaní aplikací Node.js ke správě Azure CDN."
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
ms.openlocfilehash: 46ae8cd9775432d126cbde856c1fb06ea319297e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="ea599-103">Začínáme s vývojem pro Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ea599-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ea599-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="ea599-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="ea599-105">.NET</span><span class="sxs-lookup"><span data-stu-id="ea599-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="ea599-106">Můžete použít [Azure CDN SDK pro Node.js](https://www.npmjs.com/package/azure-arm-cdn) k automatizaci vytváření a Správa profilů CDN a koncové body.</span><span class="sxs-lookup"><span data-stu-id="ea599-106">You can use the [Azure CDN SDK for Node.js](https://www.npmjs.com/package/azure-arm-cdn) to automate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="ea599-107">Tento kurz vás provede vytvoření jednoduché konzolovou aplikaci Node.js, která ukazuje několik operací, k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ea599-107">This tutorial walks through the creation of a simple Node.js console application that demonstrates several of the available operations.</span></span>  <span data-ttu-id="ea599-108">V tomto kurzu není určeno k podrobně popisují všechny aspekty CDN Azure SDK pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="ea599-108">This tutorial is not intended to describe all aspects of the Azure CDN SDK for Node.js in detail.</span></span>

<span data-ttu-id="ea599-109">K dokončení tohoto kurzu, byste již měli mít [Node.js](http://www.nodejs.org) **4.x.x** nebo vyšší nainstalována a nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="ea599-109">To complete this tutorial, you should already have [Node.js](http://www.nodejs.org) **4.x.x** or higher installed and configured.</span></span>  <span data-ttu-id="ea599-110">Můžete použít libovolný textový editor, kterou chcete vytvořit aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="ea599-110">You can use any text editor you want to create your Node.js application.</span></span>  <span data-ttu-id="ea599-111">Zápis v tomto kurzu, použití [Visual Studio Code](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="ea599-111">To write this tutorial, I used [Visual Studio Code](https://code.visualstudio.com).</span></span>  

> [!TIP]
> <span data-ttu-id="ea599-112">[Dokončený projekt z tohoto kurzu](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) je k dispozici ke stažení na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="ea599-112">The [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a><span data-ttu-id="ea599-113">Vytvořte projekt a přidejte NPM závislosti</span><span class="sxs-lookup"><span data-stu-id="ea599-113">Create your project and add NPM dependencies</span></span>
<span data-ttu-id="ea599-114">Teď, když jsme vytvořili skupinu prostředků pro naše profily CDN a získá naše Azure AD aplikace oprávnění ke správě profilů CDN a koncové body v rámci dané skupiny, můžeme začít vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea599-114">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission to manage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="ea599-115">Vytvořte složku pro uložení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea599-115">Create a folder to store your application.</span></span>  <span data-ttu-id="ea599-116">Z konzoly nástroje Node.js v aktuální cestě nastavte vaše aktuální umístění do této nové složky a inicializovat spuštěním projektu:</span><span class="sxs-lookup"><span data-stu-id="ea599-116">From a console with the Node.js tools in your current path, set your current location to this new folder and initialize your project by executing:</span></span>

    npm init

<span data-ttu-id="ea599-117">Potom zobrazí řadu otázek, k chybě při inicializaci projektu.</span><span class="sxs-lookup"><span data-stu-id="ea599-117">You will then be presented a series of questions to initialize your project.</span></span>  <span data-ttu-id="ea599-118">Pro **vstupní bod**, tento kurz používá *app.js*.</span><span class="sxs-lookup"><span data-stu-id="ea599-118">For **entry point**, this tutorial uses *app.js*.</span></span>  <span data-ttu-id="ea599-119">Můžete zobrazit Moje další možnosti v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="ea599-119">You can see my other choices in the following example.</span></span>

![Init – výstupní NPM](./media/cdn-app-dev-node/cdn-npm-init.png)

<span data-ttu-id="ea599-121">Naše projektu je nyní inicializován s *packages.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="ea599-121">Our project is now initialized with a *packages.json* file.</span></span>  <span data-ttu-id="ea599-122">Naše projektu bude používat některé knihovny Azure obsažené v NPM balíčky.</span><span class="sxs-lookup"><span data-stu-id="ea599-122">Our project is going to use some Azure libraries contained in NPM packages.</span></span>  <span data-ttu-id="ea599-123">Použijeme modulu Runtime klienta Azure pro platformu Node.js (ms-rest azure) a Azure CDN klientské knihovny pro Node.js (azure arm cd).</span><span class="sxs-lookup"><span data-stu-id="ea599-123">We'll use the Azure Client Runtime for Node.js (ms-rest-azure) and the Azure CDN Client Library for Node.js (azure-arm-cd).</span></span>  <span data-ttu-id="ea599-124">Umožňuje přidat do projektu jako závislosti.</span><span class="sxs-lookup"><span data-stu-id="ea599-124">Let's add those to the project as dependencies.</span></span>

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

<span data-ttu-id="ea599-125">Poté, co jsou balíčky dokončí instalaci, *package.json* soubor by měl vypadat podobně jako tento příklad (verze čísla se mohou lišit):</span><span class="sxs-lookup"><span data-stu-id="ea599-125">After the packages are done installing, the *package.json* file should look similar to this example (version numbers may differ):</span></span>

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

<span data-ttu-id="ea599-126">Nakonec pomocí textového editoru, vytvořte prázdný textový soubor a uložte jej do kořenové složky naše projektu jako *app.js*.</span><span class="sxs-lookup"><span data-stu-id="ea599-126">Finally, using your text editor, create a blank text file and save it in the root of our project folder as *app.js*.</span></span>  <span data-ttu-id="ea599-127">Nyní připraveni začít psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="ea599-127">We're now ready to begin writing code.</span></span>

## <a name="requires-constants-authentication-and-structure"></a><span data-ttu-id="ea599-128">Vyžaduje, konstanty, ověřování a struktury</span><span class="sxs-lookup"><span data-stu-id="ea599-128">Requires, constants, authentication, and structure</span></span>
<span data-ttu-id="ea599-129">S *app.js* otevřete v našem editoru, Pojďme základní strukturu naše programu zapsána.</span><span class="sxs-lookup"><span data-stu-id="ea599-129">With *app.js* open in our editor, let's get the basic structure of our program written.</span></span>

1. <span data-ttu-id="ea599-130">Přidejte "vyžaduje" pro naše balíčky NPM v horní části s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="ea599-130">Add the "requires" for our NPM packages at the top with the following:</span></span>
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. <span data-ttu-id="ea599-131">Je potřeba definovat některé konstanty, který bude používat naše metody.</span><span class="sxs-lookup"><span data-stu-id="ea599-131">We need to define some constants our methods will use.</span></span>  <span data-ttu-id="ea599-132">Přidejte následující.</span><span class="sxs-lookup"><span data-stu-id="ea599-132">Add the following.</span></span>  <span data-ttu-id="ea599-133">Ujistěte se, zda jste nahradili zástupné symboly, včetně  **&lt;lomené závorky&gt;**, s vlastní hodnoty, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="ea599-133">Be sure to replace the placeholders, including the **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
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
3. <span data-ttu-id="ea599-134">V dalším kroku jsme budete doložit klient správy CDN a dejte mu naše přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="ea599-134">Next, we'll instantiate the CDN management client and give it our credentials.</span></span>
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="ea599-135">Pokud používáte ověřování jednotlivých uživatelů, bude vypadat tyto dva řádky mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="ea599-135">If you are using individual user authentication, these two lines will look slightly different.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="ea599-136">Tato ukázka kódu použijte, pouze pokud zvolíte ověřování jednotlivého uživatele místo objekt služby.</span><span class="sxs-lookup"><span data-stu-id="ea599-136">Only use this code sample if you are choosing to have individual user authentication instead of a service principal.</span></span>  <span data-ttu-id="ea599-137">Pečlivě chránit pověření jednotlivé uživatele a bezpečně je tajný.</span><span class="sxs-lookup"><span data-stu-id="ea599-137">Be careful to guard your individual user credentials and keep them secret.</span></span>
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    <span data-ttu-id="ea599-138">Nezapomeňte nahradit položky v  **&lt;lomené závorky&gt;**  se správnými informacemi.</span><span class="sxs-lookup"><span data-stu-id="ea599-138">Be sure to replace the items in **&lt;angle brackets&gt;** with the correct information.</span></span>  <span data-ttu-id="ea599-139">Pro `<redirect URI>`, použijte identifikátor URI, které jste zadali při registraci aplikace ve službě Azure AD přesměrování.</span><span class="sxs-lookup"><span data-stu-id="ea599-139">For `<redirect URI>`, use the redirect URI you entered when you registered the application in Azure AD.</span></span>
4. <span data-ttu-id="ea599-140">Naše konzolovou aplikaci Node.js bude trvat některé parametry příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ea599-140">Our Node.js console application is going to take some command-line parameters.</span></span>  <span data-ttu-id="ea599-141">Umožňuje ověřit, že aspoň jeden parametr byl předán.</span><span class="sxs-lookup"><span data-stu-id="ea599-141">Let's validate that at least one parameter was passed.</span></span>
   
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
5. <span data-ttu-id="ea599-142">Která přináší nám pro hlavní část naší programu, které jsme větev na jiné funkce založené na jaké parametry byly předány.</span><span class="sxs-lookup"><span data-stu-id="ea599-142">That brings us to the main part of our program, where we branch off to other functions based on what parameters were passed.</span></span>
   
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
6. <span data-ttu-id="ea599-143">Na několika místech v našem programu potřebujeme zkontrolujte, zda číslo správné parametry byly předány v a zobrazit pomoc, pokud jejich nevypadají správné.</span><span class="sxs-lookup"><span data-stu-id="ea599-143">At several places in our program, we'll need to make sure the right number of parameters were passed in and display some help if they don't look correct.</span></span>  <span data-ttu-id="ea599-144">Umožňuje vytvořit kvůli tomu funkce.</span><span class="sxs-lookup"><span data-stu-id="ea599-144">Let's create functions to do that.</span></span>
   
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
7. <span data-ttu-id="ea599-145">Nakonec jsou asynchronní, funkcích, které budeme používat v klientovi správu CDN, takže potřebují metodu volat zpět při jejich hotovi.</span><span class="sxs-lookup"><span data-stu-id="ea599-145">Finally, the functions we'll be using on the CDN management client are asynchronous, so they need a method to call back when they're done.</span></span>  <span data-ttu-id="ea599-146">Pojďme si ho, můžete zobrazit výstup z klienta správy CDN (pokud existuje) a ukončení programu řádně.</span><span class="sxs-lookup"><span data-stu-id="ea599-146">Let's make one that can display the output from the CDN management client (if any) and exit the program gracefully.</span></span>
   
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

<span data-ttu-id="ea599-147">Teď, když je zapsán základní struktura naše programu, vytvoříme by měl funkce volané založené na našem parametrů.</span><span class="sxs-lookup"><span data-stu-id="ea599-147">Now that the basic structure of our program is written, we should create the functions called based on our parameters.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="ea599-148">Koncové body a seznam profilů CDN</span><span class="sxs-lookup"><span data-stu-id="ea599-148">List CDN profiles and endpoints</span></span>
<span data-ttu-id="ea599-149">Začněme s kódem k zobrazení seznamu naše stávající profily a koncové body.</span><span class="sxs-lookup"><span data-stu-id="ea599-149">Let's start with code to list our existing profiles and endpoints.</span></span>  <span data-ttu-id="ea599-150">Vlastní kód komentář poskytují očekávaný syntaxe, abychom věděli, kde každý parametr přejde.</span><span class="sxs-lookup"><span data-stu-id="ea599-150">My code comments provide the expected syntax so we know where each parameter goes.</span></span>

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

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="ea599-151">Vytvoření profilů CDN a koncové body</span><span class="sxs-lookup"><span data-stu-id="ea599-151">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="ea599-152">V dalším kroku jsme budete napsat funkce vytvářet profily a koncové body.</span><span class="sxs-lookup"><span data-stu-id="ea599-152">Next, we'll write the functions to create profiles and endpoints.</span></span>

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

## <a name="purge-an-endpoint"></a><span data-ttu-id="ea599-153">Vyprázdnění koncového bodu</span><span class="sxs-lookup"><span data-stu-id="ea599-153">Purge an endpoint</span></span>
<span data-ttu-id="ea599-154">Za předpokladu, že byl vytvořen koncový bod, jeden běžný úkol, který jsme může být třeba provést v našem programu je mazání obsahu v našem koncový bod.</span><span class="sxs-lookup"><span data-stu-id="ea599-154">Assuming the endpoint has been created, one common task that we might want to perform in our program is purging content in our endpoint.</span></span>

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="ea599-155">Odstranit profily CDN a koncové body</span><span class="sxs-lookup"><span data-stu-id="ea599-155">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="ea599-156">Poslední funkci, kterou jsme bude obsahovat odstraní koncových bodů a profily.</span><span class="sxs-lookup"><span data-stu-id="ea599-156">The last function we will include deletes endpoints and profiles.</span></span>

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

## <a name="running-the-program"></a><span data-ttu-id="ea599-157">Spuštění programu</span><span class="sxs-lookup"><span data-stu-id="ea599-157">Running the program</span></span>
<span data-ttu-id="ea599-158">Můžeme spouštět naše program Node.js pomocí našich oblíbených ladicí program nyní nebo v konzole.</span><span class="sxs-lookup"><span data-stu-id="ea599-158">We can now execute our Node.js program using our favorite debugger or at the console.</span></span>

> [!TIP]
> <span data-ttu-id="ea599-159">Pokud používáte Visual Studio Code jako ladicí program, budete muset nastavit svoje prostředí předávat parametry příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ea599-159">If you're using Visual Studio Code as your debugger, you'll need to set up your environment to pass in the command-line parameters.</span></span>  <span data-ttu-id="ea599-160">Visual Studio Code tomu **lanuch.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="ea599-160">Visual Studio Code does this in the **lanuch.json** file.</span></span>  <span data-ttu-id="ea599-161">Vyhledejte vlastnost s názvem **argumentů** a přidejte řetězcové hodnoty pro parametry, pole, aby vypadal podobně jako tento: `"args": ["list", "profiles"]`.</span><span class="sxs-lookup"><span data-stu-id="ea599-161">Look for a property named **args** and add an array of string values for your parameters, so that it looks similar to this:  `"args": ["list", "profiles"]`.</span></span>
> 
> 

<span data-ttu-id="ea599-162">Začněme tak, že uvedete naše profily.</span><span class="sxs-lookup"><span data-stu-id="ea599-162">Let's start by listing our profiles.</span></span>

![Seznam profilů](./media/cdn-app-dev-node/cdn-list-profiles.png)

<span data-ttu-id="ea599-164">My zpět prázdné pole.</span><span class="sxs-lookup"><span data-stu-id="ea599-164">We got back an empty array.</span></span>  <span data-ttu-id="ea599-165">Vzhledem k tomu, že nemáme žádné profily v naší skupiny prostředků, která se očekává.</span><span class="sxs-lookup"><span data-stu-id="ea599-165">Since we don't have any profiles in our resource group, that's expected.</span></span>  <span data-ttu-id="ea599-166">Nyní vytvoříme profil.</span><span class="sxs-lookup"><span data-stu-id="ea599-166">Let's create a profile now.</span></span>

![Vytvoření profilu](./media/cdn-app-dev-node/cdn-create-profile.png)

<span data-ttu-id="ea599-168">Nyní Pojďme přidat koncový bod.</span><span class="sxs-lookup"><span data-stu-id="ea599-168">Now, let's add an endpoint.</span></span>

![Vytvoření koncového bodu](./media/cdn-app-dev-node/cdn-create-endpoint.png)

<span data-ttu-id="ea599-170">Umožňuje odstranit nakonec naše profil.</span><span class="sxs-lookup"><span data-stu-id="ea599-170">Finally, let's delete our profile.</span></span>

![Odstranit profil](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a><span data-ttu-id="ea599-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ea599-172">Next Steps</span></span>
<span data-ttu-id="ea599-173">Zobrazíte dokončený projekt z tohoto návodu [stáhnout vzorek](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span><span class="sxs-lookup"><span data-stu-id="ea599-173">To see the completed project from this walkthrough, [download the sample](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).</span></span>

<span data-ttu-id="ea599-174">Pokud chcete zobrazit odkaz pro Azure CDN SDK pro Node.js, zobrazit [odkaz](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span><span class="sxs-lookup"><span data-stu-id="ea599-174">To see the reference for the Azure CDN SDK for Node.js, view the [reference](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).</span></span>

<span data-ttu-id="ea599-175">Chcete-li zjistit další dokumentaci týkající se sady Azure SDK pro Node.js, podívejte se [úplné referenční](http://azure.github.io/azure-sdk-for-node/).</span><span class="sxs-lookup"><span data-stu-id="ea599-175">To find additional documentation on the Azure SDK for Node.js, view the [full reference](http://azure.github.io/azure-sdk-for-node/).</span></span>

<span data-ttu-id="ea599-176">Spravovat prostředky CDN s [prostředí PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ea599-176">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

