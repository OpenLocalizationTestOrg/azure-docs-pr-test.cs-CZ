---
title: "aaaCreate střední hodnotu zásobníku na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate MongoDB, Express, AngularJS a Node.js (střední) zásobníku na virtuální počítač s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 82a8e34e60d2bb6e6670ee007faa1113ea78b716
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a><span data-ttu-id="f80e3-103">Na virtuální počítač s Linuxem v Azure vytvořit zásobníku MongoDB, Express, AngularJS a Node.js (střední)</span><span class="sxs-lookup"><span data-stu-id="f80e3-103">Create a MongoDB, Express, AngularJS, and Node.js (MEAN) stack on a Linux VM in Azure</span></span>

<span data-ttu-id="f80e3-104">Tento kurz ukazuje, jak tooimplement MongoDB, Express, AngularJS a Node.js (střední) zásobníku na virtuální počítač s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="f80e3-104">This tutorial shows you how tooimplement a MongoDB, Express, AngularJS, and Node.js (MEAN) stack on a Linux VM in Azure.</span></span> <span data-ttu-id="f80e3-105">Hello střední zásobníku, který vytvoříte umožňuje přidávání, odstraňování a výpis knihy v databázi.</span><span class="sxs-lookup"><span data-stu-id="f80e3-105">hello MEAN stack that you create enables adding, deleting, and listing books in a database.</span></span> <span data-ttu-id="f80e3-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="f80e3-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f80e3-107">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="f80e3-107">Create a Linux VM</span></span>
> * <span data-ttu-id="f80e3-108">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="f80e3-108">Install Node.js</span></span>
> * <span data-ttu-id="f80e3-109">Nainstalujte MongoDB a nastavit hello server</span><span class="sxs-lookup"><span data-stu-id="f80e3-109">Install MongoDB and set up hello server</span></span>
> * <span data-ttu-id="f80e3-110">Expresní instalace a nastavení serveru toohello trasy</span><span class="sxs-lookup"><span data-stu-id="f80e3-110">Install Express and set up routes toohello server</span></span>
> * <span data-ttu-id="f80e3-111">Přístup hello tras se AngularJS</span><span class="sxs-lookup"><span data-stu-id="f80e3-111">Access hello routes with AngularJS</span></span>
> * <span data-ttu-id="f80e3-112">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="f80e3-112">Run hello application</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f80e3-113">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f80e3-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="f80e3-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="f80e3-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="f80e3-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f80e3-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>


## <a name="create-a-linux-vm"></a><span data-ttu-id="f80e3-116">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="f80e3-116">Create a Linux VM</span></span>

<span data-ttu-id="f80e3-117">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkazů a vytvoření virtuálního počítače s Linuxem pomocí hello [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f80e3-117">Create a resource group with hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command and create a Linux VM with hello [az vm create](https://docs.microsoft.com/cli/azure/vm#create) command.</span></span> <span data-ttu-id="f80e3-118">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="f80e3-118">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="f80e3-119">Hello následující příklad používá toocreate hello rozhraní příkazového řádku Azure s názvem skupiny prostředků *myResourceGroupMEAN* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="f80e3-119">hello following example uses hello Azure CLI toocreate a resource group named *myResourceGroupMEAN* in hello *eastus* location.</span></span> <span data-ttu-id="f80e3-120">Virtuální počítač je vytvořen s názvem *Můjvp* s klíči SSH, pokud už neexistují ve výchozím umístění klíče.</span><span class="sxs-lookup"><span data-stu-id="f80e3-120">A VM is created named *myVM* with SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="f80e3-121">toouse konkrétní sada klíčů, použijte hello – možnost ssh klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="f80e3-121">toouse a specific set of keys, use hello --ssh-key-value option.</span></span>

```azurecli-interactive
az group create --name myResourceGroupMEAN --location eastus
az vm create \
    --resource-group myResourceGroupMEAN \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password 'Azure12345678!' \
    --generate-ssh-keys
az vm open-port --port 3300 --resource-group myResourceGroupMEAN --name myVM
```

<span data-ttu-id="f80e3-122">Po vytvoření hello virtuálních počítačů, hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace:</span><span class="sxs-lookup"><span data-stu-id="f80e3-122">When hello VM has been created, hello Azure CLI shows information similar toohello following example:</span></span> 

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroupMEAN/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.72.77.9",
  "resourceGroup": "myResourceGroupMEAN"
}
```
<span data-ttu-id="f80e3-123">Poznamenejte si hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="f80e3-123">Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="f80e3-124">Tato adresa je použité tooaccess hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f80e3-124">This address is used tooaccess hello VM.</span></span>

<span data-ttu-id="f80e3-125">Použití hello následující příkaz toocreate na relace SSH s hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f80e3-125">Use hello following command toocreate an SSH session with hello VM.</span></span> <span data-ttu-id="f80e3-126">Zajistěte, aby toouse hello správné veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f80e3-126">Make sure toouse hello correct public IP address.</span></span> <span data-ttu-id="f80e3-127">V našem příkladu výše naše IP adresa byla 13.72.77.9.</span><span class="sxs-lookup"><span data-stu-id="f80e3-127">In our example above our IP address was 13.72.77.9.</span></span>

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a><span data-ttu-id="f80e3-128">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="f80e3-128">Install Node.js</span></span>

<span data-ttu-id="f80e3-129">[Node.js](https://nodejs.org/en/) je prostředí JavaScript runtime, který je založený na JavaScript v8: modul pro Chrome.</span><span class="sxs-lookup"><span data-stu-id="f80e3-129">[Node.js](https://nodejs.org/en/) is a JavaScript runtime built on Chrome's V8 JavaScript engine.</span></span> <span data-ttu-id="f80e3-130">Node.js se používá v tento kurz tooset hello Express tras a AngularJS řadiče.</span><span class="sxs-lookup"><span data-stu-id="f80e3-130">Node.js is used in this tutorial tooset up hello Express routes and AngularJS controllers.</span></span>

<span data-ttu-id="f80e3-131">Na hello virtuální počítač pomocí prostředí bash hello, kterou jste otevřeli pomocí protokolu SSH nainstalujte si Node.js.</span><span class="sxs-lookup"><span data-stu-id="f80e3-131">On hello VM, using hello bash shell that you opened with SSH, install Node.js.</span></span>

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-hello-server"></a><span data-ttu-id="f80e3-132">Nainstalujte MongoDB a nastavit hello server</span><span class="sxs-lookup"><span data-stu-id="f80e3-132">Install MongoDB and set up hello server</span></span>
<span data-ttu-id="f80e3-133">[MongoDB](http://www.mongodb.com) ukládá data v dokumentech flexibilní, jako JSON.</span><span class="sxs-lookup"><span data-stu-id="f80e3-133">[MongoDB](http://www.mongodb.com) stores data in flexible, JSON-like documents.</span></span> <span data-ttu-id="f80e3-134">Pole v databázi se můžou lišit od toodocument dokumentu a struktura dat lze změnit v čase.</span><span class="sxs-lookup"><span data-stu-id="f80e3-134">Fields in a database can vary from document toodocument and data structure can be changed over time.</span></span> <span data-ttu-id="f80e3-135">Pro naše ukázková aplikace přidáváme tooMongoDB kniha záznamy, které obsahují název adresáře, číslo isbn, autora a počet stránek.</span><span class="sxs-lookup"><span data-stu-id="f80e3-135">For our example application, we are adding book records tooMongoDB that contain book name, isbn number, author, and number of pages.</span></span> 

1. <span data-ttu-id="f80e3-136">Na hello virtuální počítač pomocí prostředí bash hello, kterou jste otevřeli pomocí protokolu SSH nastavte klíč hello MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f80e3-136">On hello VM, using hello bash shell that you opened with SSH, set hello MongoDB key.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. <span data-ttu-id="f80e3-137">Aktualizujte správce balíčků hello klíčem hello.</span><span class="sxs-lookup"><span data-stu-id="f80e3-137">Update hello package manager with hello key.</span></span>
  
    ```bash
    sudo apt-get update
    ```

3. <span data-ttu-id="f80e3-138">Nainstalujte MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f80e3-138">Install MongoDB.</span></span>

    ```bash
    sudo apt-get install -y mongodb
    ```

4. <span data-ttu-id="f80e3-139">Spusťte hello server.</span><span class="sxs-lookup"><span data-stu-id="f80e3-139">Start hello server.</span></span>

    ```bash
    sudo service mongodb start
    ```

5. <span data-ttu-id="f80e3-140">Potřebujeme tooinstall hello [textu analyzátor](https://www.npmjs.com/package/body-parser-json) balíček toohelp nám zpracovat hello JSON předané požadavky toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="f80e3-140">We also need tooinstall hello [body-parser](https://www.npmjs.com/package/body-parser-json) package toohelp us process hello JSON passed in requests toohello server.</span></span>

    <span data-ttu-id="f80e3-141">Nainstalujte Správce balíčků npm hello.</span><span class="sxs-lookup"><span data-stu-id="f80e3-141">Install hello npm package manager.</span></span>

    ```bash
    sudo apt-get install npm
    ```

    <span data-ttu-id="f80e3-142">Instalovat balíček analyzátor textu hello.</span><span class="sxs-lookup"><span data-stu-id="f80e3-142">Install hello body parser package.</span></span>
    
    ```bash
    sudo npm install body-parser
    ```

6. <span data-ttu-id="f80e3-143">Vytvořte složku s názvem *knihy* a přidejte souboru tooit s názvem *server.js* obsahující hello konfigurace pro webový server hello.</span><span class="sxs-lookup"><span data-stu-id="f80e3-143">Create a folder named *Books* and add a file tooit named *server.js* that contains hello configuration for hello web server.</span></span>

    ```node.js
    var express = require('express');
    var bodyParser = require('body-parser');
    var app = express();
    app.use(express.static(__dirname + '/public'));
    app.use(bodyParser.json());
    require('./apps/routes')(app);
    app.set('port', 3300);
    app.listen(app.get('port'), function() {
        console.log('Server up: http://localhost:' + app.get('port'));
    });
    ```

## <a name="install-express-and-set-up-routes-toohello-server"></a><span data-ttu-id="f80e3-144">Expresní instalace a nastavení serveru toohello trasy</span><span class="sxs-lookup"><span data-stu-id="f80e3-144">Install Express and set up routes toohello server</span></span>

<span data-ttu-id="f80e3-145">[Express](https://expressjs.com) je minimalistické a flexibilní Node.js webové aplikace rozhraní, které poskytuje funkce pro webové a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="f80e3-145">[Express](https://expressjs.com) is a minimal and flexible Node.js web application framework that provides features for web and mobile applications.</span></span> <span data-ttu-id="f80e3-146">Express se používá v tento kurz toopass kniha informace tooand v naší databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f80e3-146">Express is used in this tutorial toopass book information tooand from our MongoDB database.</span></span> <span data-ttu-id="f80e3-147">[Mongoose](http://mongoosejs.com) poskytuje jednoduché a na základě schématu řešení toomodel data aplikací.</span><span class="sxs-lookup"><span data-stu-id="f80e3-147">[Mongoose](http://mongoosejs.com) provides a straight-forward, schema-based solution toomodel your application data.</span></span> <span data-ttu-id="f80e3-148">Mongoose se používá v tento kurz tooprovide schématu adresáře pro databázi hello.</span><span class="sxs-lookup"><span data-stu-id="f80e3-148">Mongoose is used in this tutorial tooprovide a book schema for hello database.</span></span>

1. <span data-ttu-id="f80e3-149">Expresní instalace a Mongoose.</span><span class="sxs-lookup"><span data-stu-id="f80e3-149">Install Express and Mongoose.</span></span>

    ```bash
    sudo npm install express mongoose
    ```

2. <span data-ttu-id="f80e3-150">V hello *knihy* složky, vytvořte složku s názvem *aplikace* a přidejte do souboru s názvem *routes.js* s hello express trasy definované.</span><span class="sxs-lookup"><span data-stu-id="f80e3-150">In hello *Books* folder, create a folder named *apps* and add a file named *routes.js* with hello express routes defined.</span></span>

    ```node.js
    var Book = require('./models/book');
    module.exports = function(app) {
      app.get('/book', function(req, res) {
        Book.find({}, function(err, result) {
          if ( err ) throw err;
          res.json(result);
        });
      }); 
      app.post('/book', function(req, res) {
        var book = new Book( {
          name:req.body.name,
          isbn:req.body.isbn,
          author:req.body.author,
          pages:req.body.pages
        });
        book.save(function(err, result) {
          if ( err ) throw err;
          res.json( {
            message:"Successfully added book",
            book:result
          });
        });
      });
      app.delete("/book/:isbn", function(req, res) {
        Book.findOneAndRemove(req.query, function(err, result) {
          if ( err ) throw err;
          res.json( {
            message: "Successfully deleted hello book",
            book: result
          });
        });
      });
      var path = require('path');
      app.get('*', function(req, res) {
        res.sendfile(path.join(__dirname + '/public', 'index.html'));
      });
    };
    ```

3. <span data-ttu-id="f80e3-151">V hello *aplikace* složky, vytvořte složku s názvem *modely* a přidejte do souboru s názvem *book.js* pomocí hello kniha model konfigurace definované.</span><span class="sxs-lookup"><span data-stu-id="f80e3-151">In hello *apps* folder, create a folder named *models* and add a file named *book.js* with hello book model configuration defined.</span></span>  

    ```node.js
    var mongoose = require('mongoose');
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost);
    mongoose.connection;
    mongoose.set('debug', true);
    var bookSchema = mongoose.Schema( {
      name: String,
      isbn: {type: String, index: true},
      author: String,
      pages: Number
    });
    var Book = mongoose.model('Book', bookSchema);
    module.exports = mongoose.model('Book', bookSchema); 
    ```

## <a name="access-hello-routes-with-angularjs"></a><span data-ttu-id="f80e3-152">Přístup hello tras se AngularJS</span><span class="sxs-lookup"><span data-stu-id="f80e3-152">Access hello routes with AngularJS</span></span>

<span data-ttu-id="f80e3-153">[AngularJS](https://angularjs.org) poskytuje webové rozhraní pro vytváření dynamického zobrazení v webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="f80e3-153">[AngularJS](https://angularjs.org) provides a web framework for creating dynamic views in your web applications.</span></span> <span data-ttu-id="f80e3-154">V tomto kurzu jsme použít AngularJS tooconnect naše webové stránky s Express a provádět akce na našem databázi seznamu.</span><span class="sxs-lookup"><span data-stu-id="f80e3-154">In this tutorial, we use AngularJS tooconnect our web page with Express and perform actions on our book database.</span></span>

1. <span data-ttu-id="f80e3-155">Změnit adresář, hello zálohování příliš*knihy* (`cd ../..`) a pak vytvořte složku s názvem *veřejné* a přidejte do souboru s názvem *script.js* s řadičem hello konfigurace definované.</span><span class="sxs-lookup"><span data-stu-id="f80e3-155">Change hello directory back up too*Books* (`cd ../..`), and then create a folder named *public* and add a file named *script.js* with hello controller configuration defined.</span></span>

    ```node.js
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
      $http( {
        method: 'GET',
        url: '/book'
      }).then(function successCallback(response) {
        $scope.books = response.data;
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });
      $scope.del_book = function(book) {
        $http( {
          method: 'DELETE',
          url: '/book/:isbn',
          params: {'isbn': book.isbn}
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
      $scope.add_book = function() {
        var body = '{ "name": "' + $scope.Name + 
        '", "isbn": "' + $scope.Isbn +
        '", "author": "' + $scope.Author + 
        '", "pages": "' + $scope.Pages + '" }';
        $http({
          method: 'POST',
          url: '/book',
          data: body
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
    });
    ```
    
2. <span data-ttu-id="f80e3-156">V hello *veřejné* složky, vytvořte soubor s názvem *index.html* s webovou stránku hello definované.</span><span class="sxs-lookup"><span data-stu-id="f80e3-156">In hello *public* folder, create a file named *index.html* with hello web page defined.</span></span>

    ```html
    <!doctype html>
    <html ng-app="myApp" ng-controller="myCtrl">
      <head>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="script.js"></script>
      </head>
      <body>
        <div>
          <table>
            <tr>
              <td>Name:</td> 
              <td><input type="text" ng-model="Name"></td>
            </tr>
            <tr>
              <td>Isbn:</td>
              <td><input type="text" ng-model="Isbn"></td>
            </tr>
            <tr>
              <td>Author:</td> 
              <td><input type="text" ng-model="Author"></td>
            </tr>
            <tr>
              <td>Pages:</td>
              <td><input type="number" ng-model="Pages"></td>
            </tr>
          </table>
          <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
          <table>
            <tr>
              <th>Name</th>
              <th>Isbn</th>
              <th>Author</th>
              <th>Pages</th>
            </tr>
            <tr ng-repeat="book in books">
              <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
              <td>{{book.name}}</td>
              <td>{{book.isbn}}</td>
              <td>{{book.author}}</td>
              <td>{{book.pages}}</td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    ```

##  <a name="run-hello-application"></a><span data-ttu-id="f80e3-157">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="f80e3-157">Run hello application</span></span>

1. <span data-ttu-id="f80e3-158">Změnit adresář, hello zálohování příliš*knihy* (`cd ..`) a spusťte hello server spuštěním tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="f80e3-158">Change hello directory back up too*Books* (`cd ..`) and start hello server by running this command:</span></span>

    ```bash
    nodejs server.js
    ```

2. <span data-ttu-id="f80e3-159">Otevřete webovou adresu toohello prohlížeče, které jste si poznamenali pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f80e3-159">Open a web browser toohello address that you recorded for hello VM.</span></span> <span data-ttu-id="f80e3-160">Například *http://13.72.77.9:3300*.</span><span class="sxs-lookup"><span data-stu-id="f80e3-160">For example, *http://13.72.77.9:3300*.</span></span> <span data-ttu-id="f80e3-161">Měli byste vidět něco podobného jako hello následující stránky:</span><span class="sxs-lookup"><span data-stu-id="f80e3-161">You should see something like hello following page:</span></span>

    ![Záznam adresáře](media/tutorial-mean/meanstack-init.png)

3. <span data-ttu-id="f80e3-163">Zadejte data do textových polí hello a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f80e3-163">Enter data into hello textboxes and click **Add**.</span></span> <span data-ttu-id="f80e3-164">Například:</span><span class="sxs-lookup"><span data-stu-id="f80e3-164">For example:</span></span>

    ![Přidejte záznam adresáře](media/tutorial-mean/meanstack-add.png)

4. <span data-ttu-id="f80e3-166">Po obnovení hello stránky, byste měli vidět něco podobného jako na této stránce:</span><span class="sxs-lookup"><span data-stu-id="f80e3-166">After refreshing hello page, you should see something like this page:</span></span>

    ![Seznam záznamů adresáře](media/tutorial-mean/meanstack-list.png)

5. <span data-ttu-id="f80e3-168">Uživatel může klepnout **odstranit** a odebrat záznam adresáře hello z databáze hello.</span><span class="sxs-lookup"><span data-stu-id="f80e3-168">You could click **Delete** and remove hello book record from hello database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f80e3-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f80e3-169">Next steps</span></span>

<span data-ttu-id="f80e3-170">V tomto kurzu jste vytvořili webovou aplikaci, která uchovává informace o seznamu záznamů pomocí střední hodnotu zásobníku na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="f80e3-170">In this tutorial, you created a web application that keeps track of book records using a MEAN stack on a Linux VM.</span></span> <span data-ttu-id="f80e3-171">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="f80e3-171">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f80e3-172">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="f80e3-172">Create a Linux VM</span></span>
> * <span data-ttu-id="f80e3-173">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="f80e3-173">Install Node.js</span></span>
> * <span data-ttu-id="f80e3-174">Nainstalujte MongoDB a nastavit hello server</span><span class="sxs-lookup"><span data-stu-id="f80e3-174">Install MongoDB and set up hello server</span></span>
> * <span data-ttu-id="f80e3-175">Expresní instalace a nastavení serveru toohello trasy</span><span class="sxs-lookup"><span data-stu-id="f80e3-175">Install Express and set up routes toohello server</span></span>
> * <span data-ttu-id="f80e3-176">Přístup hello tras se AngularJS</span><span class="sxs-lookup"><span data-stu-id="f80e3-176">Access hello routes with AngularJS</span></span>
> * <span data-ttu-id="f80e3-177">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="f80e3-177">Run hello application</span></span>

<span data-ttu-id="f80e3-178">Jak zálohy další kurz toolearn toohello toosecure webových serverů s certifikáty protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="f80e3-178">Advance toohello next tutorial toolearn how toosecure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f80e3-179">Zabezpečení webového serveru pomocí protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="f80e3-179">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)
