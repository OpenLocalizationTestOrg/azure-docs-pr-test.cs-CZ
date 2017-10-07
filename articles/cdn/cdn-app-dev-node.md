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
# <a name="get-started-with-azure-cdn-development"></a>Začínáme s vývojem pro Azure CDN
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

Můžete použít hello [Azure CDN SDK pro Node.js](https://www.npmjs.com/package/azure-arm-cdn) tooautomate vytváření a Správa profilů CDN a koncové body.  Tento kurz vás provede hello vytvoření jednoduché konzolovou aplikaci Node.js, která ukazuje několik operací hello k dispozici.  V tomto kurzu není určený toodescribe všechny aspekty hello Azure CDN SDK pro Node.js podrobně.

toocomplete v tomto kurzu byste již měli mít [Node.js](http://www.nodejs.org) **4.x.x** nebo vyšší nainstalována a nakonfigurována.  Můžete použít libovolný textový editor, který chcete toocreate aplikace Node.js.  toowrite tento kurz používá I [Visual Studio Code](https://code.visualstudio.com).  

> [!TIP]
> Hello [dokončený projekt z tohoto kurzu](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) je k dispozici ke stažení na webu MSDN.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Vytvořte projekt a přidejte NPM závislosti
Teď, když jsme vytvořili skupinu prostředků pro naše profily CDN a daného naše profilů CDN toomanage oprávnění aplikace Azure AD a koncové body v rámci dané skupiny, můžeme začít vytváření aplikace.

Vytvořte složku toostore vaší aplikace.  Z konzoly nástroje Node.js hello v aktuální cestě nastavte vaše aktuální umístění toothis novou složku a inicializovat spuštěním projektu:

    npm init

Pak bude zobrazovat řadu otázek tooinitialize projektu.  Pro **vstupní bod**, tento kurz používá *app.js*.  Můžete zobrazit Moje jiné možnosti hello následující ukázka.

![Init – výstupní NPM](./media/cdn-app-dev-node/cdn-npm-init.png)

Naše projektu je nyní inicializován s *packages.json* souboru.  Naše projektu přechází toouse některé knihovny Azure obsažené v NPM balíčky.  Použijeme hello Runtime klienta Azure pro platformu Node.js (ms-rest azure) a hello Azure CDN Klientská knihovna pro Node.js (azure arm cd).  Umožňuje přidat tyto toohello projekt jako závislosti.

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Po hello balíčky jsou provést instalaci, hello *package.json* soubor by měl vypadat podobně jako příklad toothis (verze čísla se mohou lišit):

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

Nakonec pomocí textového editoru, vytvořte prázdný textový soubor a uložit ho hello kořenové složky naše projektu jako *app.js*.  Nemohli jsme teď připravena toobegin psaní kódu.

## <a name="requires-constants-authentication-and-structure"></a>Vyžaduje, konstanty, ověřování a struktury
S *app.js* otevřete v našem editoru, Pojďme hello základní struktura naše programu zapsána.

1. Přidejte hello "vyžaduje" pro naše balíčky NPM v horní části hello s hello následující:
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. Potřebujeme toodefine některé konstanty, který bude používat naše metody.  Přidejte následující hello.  Být jisti tooreplace hello zástupné symboly, včetně hello  **&lt;lomené závorky&gt;**, s vlastní hodnoty, podle potřeby.
   
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
3. V dalším kroku jsme budete doložit hello CDN správy klienta a dejte mu naše přihlašovací údaje.
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Pokud používáte ověřování jednotlivých uživatelů, bude vypadat tyto dva řádky mírně lišit.
   
   > [!IMPORTANT]
   > Tato ukázka kódu použijte, pouze pokud zvolíte ověřování jednotlivých uživatelů toohave místo objekt služby.  Být opatrní tooguard pověření jednotlivé uživatele a bezpečně je tajný.
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Být jisti tooreplace hello položky v  **&lt;lomené závorky&gt;**  s hello opravte informace.  Pro `<redirect URI>`, použijte identifikátor URI, které jste zadali při registraci aplikace hello ve službě Azure AD hello přesměrování.
4. Naše konzolovou aplikaci Node.js přechází tootake některé parametry příkazového řádku.  Umožňuje ověřit, že aspoň jeden parametr byl předán.
   
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
5. Díky které nám toohello hlavní část naší programu, které jsme větev vypnout tooother funkce založené na jaké parametry byly předány.
   
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
6. Na několika místech v našem programu budeme potřebovat toomake hello správné počet parametrů byly předány v a zobrazit pomoc, pokud jejich nevypadají správné.  Umožňuje vytvořit funkce toodo který.
   
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
7. Nakonec jsou asynchronní, hello funkce, které budeme používat na klienta pro správu CDN hello, takže potřebují metoda toocall zpět v případě, že hotovi.  Můžeme si ho, že můžete zobrazit výstup hello hello CDN správy klienta (pokud existuje) a hello program řádně ukončit.
   
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

Teď, když je zapsán hello základní struktura naše programu, vytvoříme by měl hello funkce volané založené na našem parametrů.

## <a name="list-cdn-profiles-and-endpoints"></a>Koncové body a seznam profilů CDN
Začněme s toolist kódu, naše stávající profily a koncové body.  Vlastní kód komentář zadejte hello očekávána syntaxe, abychom věděli, kde každý parametr přejde.

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

## <a name="create-cdn-profiles-and-endpoints"></a>Vytvoření profilů CDN a koncové body
Dále jsme budete napište hello funkce toocreate profily a koncové body.

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

## <a name="purge-an-endpoint"></a>Vyprázdnění koncového bodu
Za předpokladu, že byl vytvořen koncový bod hello, běžných úloh, může chceme tooperform v našem programu je mazání obsahu v našem koncový bod.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>Odstranit profily CDN a koncové body
Hello poslední funkci, kterou jsme bude obsahovat odstraní koncových bodů a profily.

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

## <a name="running-hello-program"></a>Spuštění programu hello
Můžeme spouštět naše program Node.js pomocí našich oblíbených ladicí program nyní nebo v konzole hello.

> [!TIP]
> Pokud používáte Visual Studio Code jako ladicí program, budete potřebovat tooset do vašeho prostředí toopass v hello parametry příkazového řádku.  Visual Studio Code tomu v hello **lanuch.json** souboru.  Vyhledejte vlastnost s názvem **argumentů** a přidejte řetězcové hodnoty pro parametry, pole, aby vypadal podobně jako toothis: `"args": ["list", "profiles"]`.
> 
> 

Začněme tak, že uvedete naše profily.

![Seznam profilů](./media/cdn-app-dev-node/cdn-list-profiles.png)

My zpět prázdné pole.  Vzhledem k tomu, že nemáme žádné profily v naší skupiny prostředků, která se očekává.  Nyní vytvoříme profil.

![Vytvoření profilu](./media/cdn-app-dev-node/cdn-create-profile.png)

Nyní Pojďme přidat koncový bod.

![Vytvoření koncového bodu](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Umožňuje odstranit nakonec naše profil.

![Odstranit profil](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Další kroky
Projekt hello dokončit toosee z tohoto návodu [stažení ukázky hello](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

referenční dokumentace hello toosee pro hello Azure CDN SDK pro Node.js hello zobrazení [odkaz](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Další dokumentaci toofind na hello Azure SDK pro Node.js hello zobrazení [úplné referenční](http://azure.github.io/azure-sdk-for-node/).

Spravovat prostředky CDN s [prostředí PowerShell](cdn-manage-powershell.md).

