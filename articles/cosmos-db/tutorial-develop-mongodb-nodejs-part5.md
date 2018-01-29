---
title: "Kurz k MongoDB, Angular a Node pro Azure – Část 5 | Dokumentace Microsoftu"
description: "Pátá část série kurzů týkající se vytvoření aplikace MongoDB s Angular a Node postavené na službě Azure Cosmos DB s použitím stejných rozhraní API, jako používáte pro MongoDB."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/05/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: c06247279df9eaaedf128a2eeec801b4e323c535
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-5-use-mongoose-to-connect-to-azure-cosmos-db"></a>Vytvoření aplikace MongoDB s Angular a službou Azure Cosmos DB – Část 5: Připojení ke službě Azure Cosmos DB pomocí Mongoose

Tento vícedílný kurz ukazuje, jak vytvořit novou aplikaci [MongoDB API](mongodb-introduction.md) napsanou v Node.js s Express, Angular a databází Azure Cosmos DB.

Pátá část kurzu vychází z [části 4](tutorial-develop-mongodb-nodejs-part4.md) a zabývá se následujícími úlohami:

> [!div class="checklist"]
> * Připojení ke službě Azure Cosmos DB pomocí Mongoose
> * Získání informací o připojovacím řetězci ze služby Azure Cosmos DB
> * Vytvoření modelu hero
> * Vytvoření služby hero pro získání dat hrdinů
> * Místní spuštění aplikace

## <a name="video-walkthrough"></a>Video s návodem

> [!VIDEO https://www.youtube.com/embed/sI5hw6KPPXI]


## <a name="prerequisites"></a>Požadavky

Před zahájením této části kurzu se ujistěte, že jste dokončili kroky v [části 4](tutorial-develop-mongodb-nodejs-part4.md) tohoto kurzu.

> [!TIP]
> Tento kurz vás postupně provede jednotlivými kroky vytvoření aplikace. Pokud si chcete stáhnout dokončený projekt, v [úložišti angular-cosmosdb](https://github.com/Azure-Samples/angular-cosmosdb) na GitHubu můžete získat hotovou aplikaci.

## <a name="use-mongoose-to-connect-to-azure-cosmos-db"></a>Připojení ke službě Azure Cosmos DB pomocí Mongoose

1. Nainstalujte modul npm mongoose, což je rozhraní API normálně používané ke komunikaci s MongoDB.

    ```bash
    npm i mongoose --save
    ```

2. Nyní vytvořte ve složce **server** nový soubor **mongo.js**. V tomto souboru přidáte veškeré informace o připojení k databázi Azure Cosmos DB.

3. Do souboru **mongo.js** zkopírujte následující kód. Tento kód:
    * Vyžaduje Mongoose.
    * Přepíše příslib Mongo tak, aby se používal základní příslib předdefinovaný v ES6/ES2015 a novějších.
    * Zavolá soubor env, který umožňuje nastavit určité chování v závislosti na tom, jestli jste v přípravném, produkčním nebo vývojovém prostředí. Tento soubor brzy vytvoříme.
    * Zahrne připojovací řetězec MongoDB, který bude nastavený v souboru env.
    * Vytvoří funkci connect, která volá Mongoose.

    ```javascript
    const mongoose = require('mongoose');
    /**
     * Set to Node.js native promises
     * Per http://mongoosejs.com/docs/promises.html
     */
    mongoose.Promise = global.Promise;

    const env = require('./env/environment');

    // eslint-disable-next-line max-len
    const mongoUri = `mongodb://${env.accountName}:${env.key}@${env.accountName}.documents.azure.com:${env.port}/${env.databaseName}?ssl=true`;

    function connect() {
     mongoose.set('debug', true);
     return mongoose.connect(mongoUri, { useMongoClient: true });
    }

    module.exports = {
      connect,
      mongoose
    };
    ```
    
4. V podokně Průzkumník vytvořte ve složce **server** složku **environment** a ve složce **environment** vytvořte nový soubor **environment.js**.

5. Ze souboru mongo.js víme, že potřebujeme zahrnout `dbName`, `key` a `cosmosPort`, proto do souboru **environment.js** zkopírujte následující kód.

    ```javascript
    // TODO: replace if yours are different
    module.exports = {
      accountName: 'your-cosmosdb-account-name-goes-here',
      databaseName: 'admin', 
      key: 'your-key-goes-here',
      port: 10255
    };
    ```

## <a name="get-the-connection-string-information"></a>Získání informací o připojovacím řetězci

1. V souboru **environment.js** změňte hodnotu `port` na 10255. (Port služby Cosmos DB najdete na webu Azure Portal.)

    ```javascript
    const port = 10255;
    ```

2. V souboru **environment.js** změňte hodnotu `accountName` na název účtu služby Azure Cosmos DB, který jste vytvořili v [kroku 4](tutorial-develop-mongodb-nodejs-part4.md). 

3. Načtěte primární klíč pro účet služby Azure Cosmos DB pomocí následujícího příkazu rozhraní příkazového řádku v okně terminálu: 

    ```azure-cli-interactive
    az cosmosdb list-keys --name <cosmosdb-name> -g myResourceGroup
    ```    
    
    * `<cosmosdb-name>` je název účtu služby Azure Cosmos DB, který jste vytvořili v [kroku 4](tutorial-develop-mongodb-nodejs-part4.md).

4. Zkopírujte primární klíč do souboru environment.js jako hodnotu `key`.

    Vaše aplikace teď má všechny informace, které potřebuje pro připojení ke službě Azure Cosmos DB. Tyto informace můžete získat také na portálu. Další informace najdete v tématu popisujícím [získání připojovacího řetězce MongoDB pro přizpůsobení](connect-mongodb-account.md#GetCustomConnection). Uživatelské jméno na portálu odpovídá hodnotě dbName v souboru environment.js. 

## <a name="create-a-hero-model"></a>Vytvoření modelu Hero

1.  V podokně Průzkumník vytvořte ve složce **server** soubor **hero.model.js**.

2. Do souboru **hero.model.js** zkopírujte následující kód. Tento kód:
   * Vyžaduje Mongoose.
   * Vytvoří nové schéma s ID, jménem (name) a slavným výrokem (saying).
   * Vytvoří model s použitím tohoto schématu.
   * Vyexportuje model. 
   * Pojmenuje kolekci Heroes (místo Heros, což by byl výchozí název kolekce podle pravidel pojmenování v množném čísle v Mongoose).

   ```javascript
   const mongoose = require('mongoose');

   const Schema = mongoose.Schema;

   const heroSchema = new Schema(
     {
       id: { type: Number, required: true, unique: true },
       name: String,
       saying: String
     },
     {
       collection: 'Heroes'
     }
   );

   const Hero = mongoose.model('Hero', heroSchema);

   module.exports = Hero;
   ```

## <a name="create-a-hero-service"></a>Vytvoření služby Hero

1.  V podokně Průzkumník vytvořte ve složce **server** soubor **hero.service.js**.

2. Do souboru **hero.service.js** zkopírujte následující kód. Tento kód:
   * Získá model, který jste právě vytvořili.
   * Připojí se k databázi.
   * Vytvoří proměnnou docquery, která pomocí metody hero.find definuje dotaz, který vrátí všechny hrdiny.
   * Pomocí metody docquery.exec spustí dotaz za účelem získání seznamu všech hrdinů se stavem odpovědi 200. 
   * Pokud je stav 500, odešle zpět chybovou zprávu.
   * Protože používáme moduly, získá hrdiny. 

   ```javascript
   const Hero = require('./hero.model');

   require('./mongo').connect();

   function getHeroes() {
     const docquery = Hero.find({});
     docquery
       .exec()
       .then(heroes => {
         res.status(200).json(heroes);
       })
       .catch(error => {
         res.status(500).send(error);
         return;
       });
   }

   module.exports = {
     getHeroes
   };
   ```

## <a name="add-the-hero-service-to-routesjs"></a>Přidání služby hero do souboru routes.js

1. Ve Visual Studio Code v souboru **routes.js** okomentujte funkci `res.send`, která odesílá ukázková data hrdinů, a místo ní přidejte řádek pro zavolání funkce `heroService.getHeroes`.

    ```javascript
    router.get('/heroes', (req, res) => {
      heroService.getHeroes(req, res);
    //  res.send(200, [
    //      {"id": 10, "name": "Starlord", "saying": "oh yeah"}
    //  ])
    });
    ```

2. V souboru **routes.js** vyžádejte službu hero:

    ```javascript
    const heroService = require('./hero.service'); 
    ```

3. V souboru **hero.service.js** aktualizujte následujícím způsobem funkci getHeroes tak, aby přijímala parametry `req` a `res`:

    ```javascript
    function getHeroes(req, res) {
    ```

    Pojďme si krátce projít a zkontrolovat řetěz volání, který tu máme. Nejprve přejdeme do souboru `index.js`, který nastaví server Node a upozornění, které nastaví a definuje naše trasy. Náš soubor routes.js pak sdělí službě hero, že má načíst naše funkce, jako je getHeroes, a předat žádost a odpověď. Soubor hero.service.js teď vezme model a připojí se k Mongo a pak při zavolání provede funkci getHeroes a vrátí odpověď 200. Ta pak projde řetězem zpět. 

## <a name="run-the-app"></a>Spuštění aplikace

1. Pojďme znovu spustit aplikaci. Ve Visual Studio Code uložte všechny provedené změny, klikněte na tlačítko **Ladit** ![Ikona Ladit ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part5/debug-button.png) na levé straně a pak klikněte na tlačítko **Spustit ladění** ![Ikona Spustit ladění ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part5/start-debugging-button.png).

3. Teď přejděte do prohlížeče, otevřete Vývojářské nástroje a kartu Síť a přejděte na adresu http://localhost:3000, kde najdete naši aplikaci.

    ![Nový účet služby Azure Cosmos DB na webu Azure Portal](./media/tutorial-develop-mongodb-nodejs-part5/azure-cosmos-db-heroes-app.png)

   V aplikaci ještě nejsou uložení žádní hrdinové, ale v dalším kroku kurzu přidáme funkce put, push a delete, abychom moli přidávat, aktualizovat a odstraňovat hrdiny z uživatelského prostředí pomocí připojení Mongoose k naší databázi Azure Cosmos DB. 

## <a name="next-steps"></a>Další kroky

V této části kurzu jste provedli následující:

> [!div class="checklist"]
> * Pomocí rozhraní Mongoose API jste připojili aplikaci Heroes ke službě Azure Cosmos DB. 
> * Přidali jste do aplikace funkci pro získání hrdinů.

Můžete přejít k další části kurzu a přidat do aplikace funkce Post, Put a Delete.

> [!div class="nextstepaction"]
> [Přidání funkcí Post, Put a Delete do aplikace](tutorial-develop-mongodb-nodejs-part6.md)
