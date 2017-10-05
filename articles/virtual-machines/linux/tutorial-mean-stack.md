---
title: "Vytvoření střední hodnotu zásobníku na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Naučte se vytvářet zásobníku MongoDB, Express, AngularJS a Node.js (střední) na virtuální počítač s Linuxem v Azure."
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
ms.openlocfilehash: 892d3481b4ec70fb8434cb25013c5cfd8ab85051
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a><span data-ttu-id="d807c-103">Na virtuální počítač s Linuxem v Azure vytvořit zásobníku MongoDB, Express, AngularJS a Node.js (střední)</span><span class="sxs-lookup"><span data-stu-id="d807c-103">Create a MongoDB, Express, AngularJS, and Node.js (MEAN) stack on a Linux VM in Azure</span></span>

<span data-ttu-id="d807c-104">V tomto kurzu se dozvíte, jak implementovat zásobníku MongoDB, Express, AngularJS a Node.js (střední) na virtuální počítač s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="d807c-104">This tutorial shows you how to implement a MongoDB, Express, AngularJS, and Node.js (MEAN) stack on a Linux VM in Azure.</span></span> <span data-ttu-id="d807c-105">Střední zásobníku, který vytvoříte umožňuje přidávání, odstraňování a výpis knihy v databázi.</span><span class="sxs-lookup"><span data-stu-id="d807c-105">The MEAN stack that you create enables adding, deleting, and listing books in a database.</span></span> <span data-ttu-id="d807c-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="d807c-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d807c-107">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="d807c-107">Create a Linux VM</span></span>
> * <span data-ttu-id="d807c-108">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="d807c-108">Install Node.js</span></span>
> * <span data-ttu-id="d807c-109">Nainstalujte MongoDB a nastavení serveru</span><span class="sxs-lookup"><span data-stu-id="d807c-109">Install MongoDB and set up the server</span></span>
> * <span data-ttu-id="d807c-110">Expresní instalace a nastavení trasy k serveru</span><span class="sxs-lookup"><span data-stu-id="d807c-110">Install Express and set up routes to the server</span></span>
> * <span data-ttu-id="d807c-111">Přístup k tras se AngularJS</span><span class="sxs-lookup"><span data-stu-id="d807c-111">Access the routes with AngularJS</span></span>
> * <span data-ttu-id="d807c-112">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="d807c-112">Run the application</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d807c-113">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d807c-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="d807c-114">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="d807c-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="d807c-115">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d807c-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>


## <a name="create-a-linux-vm"></a><span data-ttu-id="d807c-116">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="d807c-116">Create a Linux VM</span></span>

<span data-ttu-id="d807c-117">Vytvořte skupinu prostředků s [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkazů a vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="d807c-117">Create a resource group with the [az group create](https://docs.microsoft.com/cli/azure/group#create) command and create a Linux VM with the [az vm create](https://docs.microsoft.com/cli/azure/vm#create) command.</span></span> <span data-ttu-id="d807c-118">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="d807c-118">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="d807c-119">Následující příklad používá rozhraní příkazového řádku Azure k vytvoření skupiny prostředků s názvem *myResourceGroupMEAN* v *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="d807c-119">The following example uses the Azure CLI to create a resource group named *myResourceGroupMEAN* in the *eastus* location.</span></span> <span data-ttu-id="d807c-120">Virtuální počítač je vytvořen s názvem *Můjvp* s klíči SSH, pokud už neexistují ve výchozím umístění klíče.</span><span class="sxs-lookup"><span data-stu-id="d807c-120">A VM is created named *myVM* with SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="d807c-121">Pokud chcete použít konkrétní sady klíčů, použijte možnost--ssh klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="d807c-121">To use a specific set of keys, use the --ssh-key-value option.</span></span>

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

<span data-ttu-id="d807c-122">Po vytvoření virtuálního počítače Azure CLI uvádí informace podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d807c-122">When the VM has been created, the Azure CLI shows information similar to the following example:</span></span> 

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
<span data-ttu-id="d807c-123">Poznamenejte si `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="d807c-123">Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="d807c-124">Tato adresa se používá pro přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="d807c-124">This address is used to access the VM.</span></span>

<span data-ttu-id="d807c-125">Použijte následující příkaz k vytvoření relace SSH s virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="d807c-125">Use the following command to create an SSH session with the VM.</span></span> <span data-ttu-id="d807c-126">Nezapomeňte použít správné veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d807c-126">Make sure to use the correct public IP address.</span></span> <span data-ttu-id="d807c-127">V našem příkladu výše naše IP adresa byla 13.72.77.9.</span><span class="sxs-lookup"><span data-stu-id="d807c-127">In our example above our IP address was 13.72.77.9.</span></span>

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a><span data-ttu-id="d807c-128">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="d807c-128">Install Node.js</span></span>

<span data-ttu-id="d807c-129">[Node.js](https://nodejs.org/en/) je prostředí JavaScript runtime, který je založený na JavaScript v8: modul pro Chrome.</span><span class="sxs-lookup"><span data-stu-id="d807c-129">[Node.js](https://nodejs.org/en/) is a JavaScript runtime built on Chrome's V8 JavaScript engine.</span></span> <span data-ttu-id="d807c-130">Node.js se používá v tomto kurzu nastavit Express trasy a AngularJS řadiče.</span><span class="sxs-lookup"><span data-stu-id="d807c-130">Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.</span></span>

<span data-ttu-id="d807c-131">Ve virtuálním počítači, pomocí prostředí bash, kterou jste otevřeli pomocí protokolu SSH nainstalujte si Node.js.</span><span class="sxs-lookup"><span data-stu-id="d807c-131">On the VM, using the bash shell that you opened with SSH, install Node.js.</span></span>

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-the-server"></a><span data-ttu-id="d807c-132">Nainstalujte MongoDB a nastavení serveru</span><span class="sxs-lookup"><span data-stu-id="d807c-132">Install MongoDB and set up the server</span></span>
<span data-ttu-id="d807c-133">[MongoDB](http://www.mongodb.com) ukládá data v dokumentech flexibilní, jako JSON.</span><span class="sxs-lookup"><span data-stu-id="d807c-133">[MongoDB](http://www.mongodb.com) stores data in flexible, JSON-like documents.</span></span> <span data-ttu-id="d807c-134">Pole v databázi se může lišit z dokumentu do dokumentu a struktura dat lze změnit v čase.</span><span class="sxs-lookup"><span data-stu-id="d807c-134">Fields in a database can vary from document to document and data structure can be changed over time.</span></span> <span data-ttu-id="d807c-135">Pro naše ukázková aplikace přidáváme kniha záznamy MongoDB, které obsahují název adresáře, číslo isbn, autora a počet stránek.</span><span class="sxs-lookup"><span data-stu-id="d807c-135">For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.</span></span> 

1. <span data-ttu-id="d807c-136">Ve virtuálním počítači, pomocí prostředí bash, kterou jste otevřeli pomocí protokolu SSH nastavte klíč MongoDB.</span><span class="sxs-lookup"><span data-stu-id="d807c-136">On the VM, using the bash shell that you opened with SSH, set the MongoDB key.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. <span data-ttu-id="d807c-137">Aktualizujte správce balíčků s klíčem.</span><span class="sxs-lookup"><span data-stu-id="d807c-137">Update the package manager with the key.</span></span>
  
    ```bash
    sudo apt-get update
    ```

3. <span data-ttu-id="d807c-138">Nainstalujte MongoDB.</span><span class="sxs-lookup"><span data-stu-id="d807c-138">Install MongoDB.</span></span>

    ```bash
    sudo apt-get install -y mongodb
    ```

4. <span data-ttu-id="d807c-139">Spuštění serveru.</span><span class="sxs-lookup"><span data-stu-id="d807c-139">Start the server.</span></span>

    ```bash
    sudo service mongodb start
    ```

5. <span data-ttu-id="d807c-140">Také je potřeba nainstalovat [textu analyzátor](https://www.npmjs.com/package/body-parser-json) balíček a pomoci tak zpracování JSON předané požadavky na server.</span><span class="sxs-lookup"><span data-stu-id="d807c-140">We also need to install the [body-parser](https://www.npmjs.com/package/body-parser-json) package to help us process the JSON passed in requests to the server.</span></span>

    <span data-ttu-id="d807c-141">Nainstalujte npm Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="d807c-141">Install the npm package manager.</span></span>

    ```bash
    sudo apt-get install npm
    ```

    <span data-ttu-id="d807c-142">Nainstalujte balíček analyzátor textu.</span><span class="sxs-lookup"><span data-stu-id="d807c-142">Install the body parser package.</span></span>
    
    ```bash
    sudo npm install body-parser
    ```

6. <span data-ttu-id="d807c-143">Vytvořte složku s názvem *knihy* a přidejte do ní s názvem souboru *server.js* obsahující konfigurace na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="d807c-143">Create a folder named *Books* and add a file to it named *server.js* that contains the configuration for the web server.</span></span>

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

## <a name="install-express-and-set-up-routes-to-the-server"></a><span data-ttu-id="d807c-144">Expresní instalace a nastavení trasy k serveru</span><span class="sxs-lookup"><span data-stu-id="d807c-144">Install Express and set up routes to the server</span></span>

<span data-ttu-id="d807c-145">[Express](https://expressjs.com) je minimalistické a flexibilní Node.js webové aplikace rozhraní, které poskytuje funkce pro webové a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d807c-145">[Express](https://expressjs.com) is a minimal and flexible Node.js web application framework that provides features for web and mobile applications.</span></span> <span data-ttu-id="d807c-146">Express se používá v tomto kurzu předat sešit informací na Internet a z našich databáze MongoDB.</span><span class="sxs-lookup"><span data-stu-id="d807c-146">Express is used in this tutorial to pass book information to and from our MongoDB database.</span></span> <span data-ttu-id="d807c-147">[Mongoose](http://mongoosejs.com) poskytuje jednoduché a na základě schématu řešení pro modelování data aplikací.</span><span class="sxs-lookup"><span data-stu-id="d807c-147">[Mongoose](http://mongoosejs.com) provides a straight-forward, schema-based solution to model your application data.</span></span> <span data-ttu-id="d807c-148">Mongoose se používá v tomto kurzu poskytuje schéma adresáře pro databázi.</span><span class="sxs-lookup"><span data-stu-id="d807c-148">Mongoose is used in this tutorial to provide a book schema for the database.</span></span>

1. <span data-ttu-id="d807c-149">Expresní instalace a Mongoose.</span><span class="sxs-lookup"><span data-stu-id="d807c-149">Install Express and Mongoose.</span></span>

    ```bash
    sudo npm install express mongoose
    ```

2. <span data-ttu-id="d807c-150">V *knihy* složky, vytvořte složku s názvem *aplikace* a přidejte do souboru s názvem *routes.js* s express trasy definované.</span><span class="sxs-lookup"><span data-stu-id="d807c-150">In the *Books* folder, create a folder named *apps* and add a file named *routes.js* with the express routes defined.</span></span>

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
            message: "Successfully deleted the book",
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

3. <span data-ttu-id="d807c-151">V *aplikace* složky, vytvořte složku s názvem *modely* a přidejte do souboru s názvem *book.js* pomocí modelu konfigurace adresáře definované.</span><span class="sxs-lookup"><span data-stu-id="d807c-151">In the *apps* folder, create a folder named *models* and add a file named *book.js* with the book model configuration defined.</span></span>  

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

## <a name="access-the-routes-with-angularjs"></a><span data-ttu-id="d807c-152">Přístup k tras se AngularJS</span><span class="sxs-lookup"><span data-stu-id="d807c-152">Access the routes with AngularJS</span></span>

<span data-ttu-id="d807c-153">[AngularJS](https://angularjs.org) poskytuje webové rozhraní pro vytváření dynamického zobrazení v webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="d807c-153">[AngularJS](https://angularjs.org) provides a web framework for creating dynamic views in your web applications.</span></span> <span data-ttu-id="d807c-154">V tomto kurzu používáme AngularJS připojit naše webové stránky s Express a provádět akce na databázi našich seznamu.</span><span class="sxs-lookup"><span data-stu-id="d807c-154">In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book database.</span></span>

1. <span data-ttu-id="d807c-155">Změna adresáře zpět do *knihy* (`cd ../..`) a pak vytvořte složku s názvem *veřejné* a přidejte do souboru s názvem *script.js* s konfigurací řadiče definované.</span><span class="sxs-lookup"><span data-stu-id="d807c-155">Change the directory back up to *Books* (`cd ../..`), and then create a folder named *public* and add a file named *script.js* with the controller configuration defined.</span></span>

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
    
2. <span data-ttu-id="d807c-156">V *veřejné* složky, vytvořte soubor s názvem *index.html* s webovou stránkou definované.</span><span class="sxs-lookup"><span data-stu-id="d807c-156">In the *public* folder, create a file named *index.html* with the web page defined.</span></span>

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

##  <a name="run-the-application"></a><span data-ttu-id="d807c-157">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="d807c-157">Run the application</span></span>

1. <span data-ttu-id="d807c-158">Změna adresáře zpět do *knihy* (`cd ..`) a spustí se server tak, že spustíte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="d807c-158">Change the directory back up to *Books* (`cd ..`) and start the server by running this command:</span></span>

    ```bash
    nodejs server.js
    ```

2. <span data-ttu-id="d807c-159">Otevřete webový prohlížeč na adrese, které jste si poznamenali pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d807c-159">Open a web browser to the address that you recorded for the VM.</span></span> <span data-ttu-id="d807c-160">Například *http://13.72.77.9:3300*.</span><span class="sxs-lookup"><span data-stu-id="d807c-160">For example, *http://13.72.77.9:3300*.</span></span> <span data-ttu-id="d807c-161">Měli byste vidět něco podobného jako na následující stránce:</span><span class="sxs-lookup"><span data-stu-id="d807c-161">You should see something like the following page:</span></span>

    ![Záznam adresáře](media/tutorial-mean/meanstack-init.png)

3. <span data-ttu-id="d807c-163">Zadejte data do textových polí a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d807c-163">Enter data into the textboxes and click **Add**.</span></span> <span data-ttu-id="d807c-164">Například:</span><span class="sxs-lookup"><span data-stu-id="d807c-164">For example:</span></span>

    ![Přidejte záznam adresáře](media/tutorial-mean/meanstack-add.png)

4. <span data-ttu-id="d807c-166">Po aktualizaci stránky, měli byste vidět něco podobného jako na této stránce:</span><span class="sxs-lookup"><span data-stu-id="d807c-166">After refreshing the page, you should see something like this page:</span></span>

    ![Seznam záznamů adresáře](media/tutorial-mean/meanstack-list.png)

5. <span data-ttu-id="d807c-168">Uživatel může klepnout **odstranit** a odebrat záznam adresáře z databáze.</span><span class="sxs-lookup"><span data-stu-id="d807c-168">You could click **Delete** and remove the book record from the database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d807c-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d807c-169">Next steps</span></span>

<span data-ttu-id="d807c-170">V tomto kurzu jste vytvořili webovou aplikaci, která uchovává informace o seznamu záznamů pomocí střední hodnotu zásobníku na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="d807c-170">In this tutorial, you created a web application that keeps track of book records using a MEAN stack on a Linux VM.</span></span> <span data-ttu-id="d807c-171">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="d807c-171">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d807c-172">Vytvoření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="d807c-172">Create a Linux VM</span></span>
> * <span data-ttu-id="d807c-173">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="d807c-173">Install Node.js</span></span>
> * <span data-ttu-id="d807c-174">Nainstalujte MongoDB a nastavení serveru</span><span class="sxs-lookup"><span data-stu-id="d807c-174">Install MongoDB and set up the server</span></span>
> * <span data-ttu-id="d807c-175">Expresní instalace a nastavení trasy k serveru</span><span class="sxs-lookup"><span data-stu-id="d807c-175">Install Express and set up routes to the server</span></span>
> * <span data-ttu-id="d807c-176">Přístup k tras se AngularJS</span><span class="sxs-lookup"><span data-stu-id="d807c-176">Access the routes with AngularJS</span></span>
> * <span data-ttu-id="d807c-177">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="d807c-177">Run the application</span></span>

<span data-ttu-id="d807c-178">Přechodu na v dalším kurzu se dozvíte, jak zabezpečit webové servery s certifikáty protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="d807c-178">Advance to the next tutorial to learn how to secure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d807c-179">Zabezpečení webového serveru pomocí protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="d807c-179">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)
