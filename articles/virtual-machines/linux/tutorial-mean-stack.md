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
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a>Na virtuální počítač s Linuxem v Azure vytvořit zásobníku MongoDB, Express, AngularJS a Node.js (střední)

Tento kurz ukazuje, jak tooimplement MongoDB, Express, AngularJS a Node.js (střední) zásobníku na virtuální počítač s Linuxem v Azure. Hello střední zásobníku, který vytvoříte umožňuje přidávání, odstraňování a výpis knihy v databázi. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Linuxem
> * Instalovat Node.js
> * Nainstalujte MongoDB a nastavit hello server
> * Expresní instalace a nastavení serveru toohello trasy
> * Přístup hello tras se AngularJS
> * Spuštění aplikace hello

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).


## <a name="create-a-linux-vm"></a>Vytvoření virtuálního počítače s Linuxem

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkazů a vytvoření virtuálního počítače s Linuxem pomocí hello [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

Hello následující příklad používá toocreate hello rozhraní příkazového řádku Azure s názvem skupiny prostředků *myResourceGroupMEAN* v hello *eastus* umístění. Virtuální počítač je vytvořen s názvem *Můjvp* s klíči SSH, pokud už neexistují ve výchozím umístění klíče. toouse konkrétní sada klíčů, použijte hello – možnost ssh klíč hodnota.

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

Po vytvoření hello virtuálních počítačů, hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace: 

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
Poznamenejte si hello `publicIpAddress`. Tato adresa je použité tooaccess hello virtuálních počítačů.

Použití hello následující příkaz toocreate na relace SSH s hello virtuálních počítačů. Zajistěte, aby toouse hello správné veřejnou IP adresu. V našem příkladu výše naše IP adresa byla 13.72.77.9.

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a>Instalovat Node.js

[Node.js](https://nodejs.org/en/) je prostředí JavaScript runtime, který je založený na JavaScript v8: modul pro Chrome. Node.js se používá v tento kurz tooset hello Express tras a AngularJS řadiče.

Na hello virtuální počítač pomocí prostředí bash hello, kterou jste otevřeli pomocí protokolu SSH nainstalujte si Node.js.

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-hello-server"></a>Nainstalujte MongoDB a nastavit hello server
[MongoDB](http://www.mongodb.com) ukládá data v dokumentech flexibilní, jako JSON. Pole v databázi se můžou lišit od toodocument dokumentu a struktura dat lze změnit v čase. Pro naše ukázková aplikace přidáváme tooMongoDB kniha záznamy, které obsahují název adresáře, číslo isbn, autora a počet stránek. 

1. Na hello virtuální počítač pomocí prostředí bash hello, kterou jste otevřeli pomocí protokolu SSH nastavte klíč hello MongoDB.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. Aktualizujte správce balíčků hello klíčem hello.
  
    ```bash
    sudo apt-get update
    ```

3. Nainstalujte MongoDB.

    ```bash
    sudo apt-get install -y mongodb
    ```

4. Spusťte hello server.

    ```bash
    sudo service mongodb start
    ```

5. Potřebujeme tooinstall hello [textu analyzátor](https://www.npmjs.com/package/body-parser-json) balíček toohelp nám zpracovat hello JSON předané požadavky toohello serveru.

    Nainstalujte Správce balíčků npm hello.

    ```bash
    sudo apt-get install npm
    ```

    Instalovat balíček analyzátor textu hello.
    
    ```bash
    sudo npm install body-parser
    ```

6. Vytvořte složku s názvem *knihy* a přidejte souboru tooit s názvem *server.js* obsahující hello konfigurace pro webový server hello.

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

## <a name="install-express-and-set-up-routes-toohello-server"></a>Expresní instalace a nastavení serveru toohello trasy

[Express](https://expressjs.com) je minimalistické a flexibilní Node.js webové aplikace rozhraní, které poskytuje funkce pro webové a mobilní aplikace. Express se používá v tento kurz toopass kniha informace tooand v naší databázi MongoDB. [Mongoose](http://mongoosejs.com) poskytuje jednoduché a na základě schématu řešení toomodel data aplikací. Mongoose se používá v tento kurz tooprovide schématu adresáře pro databázi hello.

1. Expresní instalace a Mongoose.

    ```bash
    sudo npm install express mongoose
    ```

2. V hello *knihy* složky, vytvořte složku s názvem *aplikace* a přidejte do souboru s názvem *routes.js* s hello express trasy definované.

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

3. V hello *aplikace* složky, vytvořte složku s názvem *modely* a přidejte do souboru s názvem *book.js* pomocí hello kniha model konfigurace definované.  

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

## <a name="access-hello-routes-with-angularjs"></a>Přístup hello tras se AngularJS

[AngularJS](https://angularjs.org) poskytuje webové rozhraní pro vytváření dynamického zobrazení v webových aplikací. V tomto kurzu jsme použít AngularJS tooconnect naše webové stránky s Express a provádět akce na našem databázi seznamu.

1. Změnit adresář, hello zálohování příliš*knihy* (`cd ../..`) a pak vytvořte složku s názvem *veřejné* a přidejte do souboru s názvem *script.js* s řadičem hello konfigurace definované.

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
    
2. V hello *veřejné* složky, vytvořte soubor s názvem *index.html* s webovou stránku hello definované.

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

##  <a name="run-hello-application"></a>Spuštění aplikace hello

1. Změnit adresář, hello zálohování příliš*knihy* (`cd ..`) a spusťte hello server spuštěním tohoto příkazu:

    ```bash
    nodejs server.js
    ```

2. Otevřete webovou adresu toohello prohlížeče, které jste si poznamenali pro hello virtuálních počítačů. Například *http://13.72.77.9:3300*. Měli byste vidět něco podobného jako hello následující stránky:

    ![Záznam adresáře](media/tutorial-mean/meanstack-init.png)

3. Zadejte data do textových polí hello a klikněte na tlačítko **přidat**. Například:

    ![Přidejte záznam adresáře](media/tutorial-mean/meanstack-add.png)

4. Po obnovení hello stránky, byste měli vidět něco podobného jako na této stránce:

    ![Seznam záznamů adresáře](media/tutorial-mean/meanstack-list.png)

5. Uživatel může klepnout **odstranit** a odebrat záznam adresáře hello z databáze hello.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili webovou aplikaci, která uchovává informace o seznamu záznamů pomocí střední hodnotu zásobníku na virtuální počítač s Linuxem. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Linuxem
> * Instalovat Node.js
> * Nainstalujte MongoDB a nastavit hello server
> * Expresní instalace a nastavení serveru toohello trasy
> * Přístup hello tras se AngularJS
> * Spuštění aplikace hello

Jak zálohy další kurz toolearn toohello toosecure webových serverů s certifikáty protokolu SSL.

> [!div class="nextstepaction"]
> [Zabezpečení webového serveru pomocí protokolu SSL](tutorial-secure-web-server.md)
