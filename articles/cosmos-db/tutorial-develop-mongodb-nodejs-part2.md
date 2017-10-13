---
title: "Kurz k MongoDB, Angular a Node pro Azure – Část 2 | Dokumentace Microsoftu"
description: "Druhá část série kurzů týkající se vytvoření aplikace MongoDB s Angular a Node postavené na službě Azure Cosmos DB s použitím stejných rozhraní API, jako používáte pro MongoDB."
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
ms.topic: hero-article
ms.date: 09/05/2017
ms.author: mimig
ms.openlocfilehash: 48b7da57b23dbd16571c8fa179efd900cdcf21ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-2-create-a-nodejs-express-app-with-the-angular-cli"></a>Vytvoření aplikace MongoDB s Angular a službou Azure Cosmos DB – Část 2: Vytvoření aplikace Node.js Express pomocí Angular CLI 

Tento vícedílný kurz ukazuje, jak vytvořit novou aplikaci [MongoDB API](mongodb-introduction.md) napsanou v Node.js s Express, Angular a databází Azure Cosmos DB.

Druhá část kurzu vychází z [úvodu](tutorial-develop-mongodb-nodejs.md) a zabývá se následujícími úlohami:

> [!div class="checklist"]
> * Instalace Angular CLI a jazyka TypeScript
> * Vytvoření nového projektu pomocí Angular
> * Sestavení aplikace pomocí rozhraní Express
> * Testování aplikace v nástroji Postman

## <a name="video-walkthrough"></a>Video s návodem

> [!VIDEO https://www.youtube.com/embed/lIwJIYcGSUg]

## <a name="prerequisites"></a>Požadavky

Před zahájením této části kurzu se nezapomeňte podívat na [úvodní video](tutorial-develop-mongodb-nodejs.md).

V tomto kurzu budete také potřebovat: 
* [Node.js](https://nodejs.org/) verze 8.4.0 nebo vyšší.
* [Postman](https://www.getpostman.com/)
* [Visual Studio Code](https://code.visualstudio.com/) nebo váš oblíbený editor kódu.

> [!TIP]
> Tento kurz vás postupně provede jednotlivými kroky vytvoření aplikace. Pokud si chcete stáhnout dokončený projekt, v [úložišti angular-cosmosdb](https://github.com/Azure-Samples/angular-cosmosdb) na GitHubu můžete získat hotovou aplikaci.

## <a name="install-the-angular-cli-and-typescript"></a>Instalace Angular CLI a jazyka TypeScript

1. Otevřete okno příkazového řádku ve Windows nebo okno Terminálu na Macu a nainstalujte Angular CLI.

    ```bash
    npm install -g @angular/cli
    ```

2. Nainstalujte TypeScript zadáním následujícího příkazu na příkazovém řádku. 

    ```bash
    npm install -g typescript
    ```

## <a name="use-the-angular-cli-to-create-a-new-project"></a>Vytvoření nového projektu pomocí Angular CLI

1. Na příkazovém řádku přejděte do složky, ve které chcete vytvořit nový projekt, a spusťte následující příkaz. Tento příkaz vytvoří novou složku a projekt angular-cosmosdb a nainstaluje komponenty Angular požadované pro novou aplikaci. Kromě toho nainstaluje zdrojový kód ve složce src/client (-sd src/client), použije minimální instalaci (--minimal) a určí, že projekt používá Sass, což je syntax podobná šablonám stylů CSS (pomocí příznaku --style scss).

    ```bash
    ng new angular-cosmosdb -sd src/client --minimal --style scss
    ```

2. Po dokončení příkazu změňte adresář na složku src/client.

    ```bash
    cd angular-cosmosdb
    ```

3. Potom tuto složku otevřete ve Visual Studio Code.

    ```bash
    code .
    ```

## <a name="build-the-app-using-the-express-framework"></a>Sestavení aplikace pomocí rozhraní Express

1. Ve Visual Studio Code v podokně **Průzkumník** klikněte pravým tlačítkem na složku **src**, klikněte na **Nová složka** a pojmenujte novou složku *server*.

2. V podokně **Průzkumník** klikněte pravým tlačítkem na složku **server**, klikněte na **Nový soubor** a pojmenujte nový soubor *index.js*.

3. Zpět na příkazovém řádku pomocí následujícího příkazu nainstalujte analyzátor textu. Ten pomáhá aplikaci parsovat data JSON předávaná prostřednictvím rozhraní API.

    ```bash
    npm i express body-parser --save
    ```

4. Ve Visual Studio Code zkopírujte následující kód do souboru index.js. Tento kód:
    * Odkazuje na Express.
    * Přetáhne analyzátor textu pro čtení dat JSON v textech žádostí.
    * Použije integrovanou funkci path.
    * Nastaví kořenové proměnné pro snadnější hledání kódu.
    * Nastaví port.
    * Nastartuje Express.
    * Sdělí aplikaci, jak používat middleware, který budeme používat k obsluze serveru.
    * Obsluhuje všechno ve složce dist, což bude statický obsah.
    * Obsluhuje aplikaci a na každý požadavek GET, který se nenajde na serveru (pro přímé odkazy), vrátí soubor index.html.
    * Spustí server pomocí funkce app.listen.
    * Používá funkci šipky k zaprotokolování aktivního stavu portu.
    
   ```node
   const express = require('express');
   const bodyParser = require('body-parser');
   const path = require('path');
   const routes = require('./routes');

   const root = './';
   const port = process.env.PORT || '3000';
   const app = express();

   app.use(bodyParser.json());
   app.use(bodyParser.urlencoded({ extended: false }));
   app.use(express.static(path.join(root, 'dist')));
   app.use('/api', routes);
   app.get('*', (req, res) => {
     res.sendFile('dist/index.html', {root});
   });

   app.listen(port, () => console.log(`API running on localhost:${port}`));
   ```

5. Ve Visual Studio Code v podokně **Průzkumník** klikněte pravým tlačítkem na složku **server** a pak klikněte na **Nový soubor**. Pojmenujte nový soubor *routes.js*. 

6. Do souboru **routes.js** zkopírujte následující kód. Tento kód:
   * Přidá referenci na směrovač Express.
   * Získá hrdiny.
   * Odešle zpět JSON pro definovaného hrdinu.

   ```node
   const express = require('express');
   const router = express.Router();

   router.get('/heroes', (req, res) => {
    res.send(200, [
       {"id": 10, "name": "Starlord", "saying": "oh yeah"}
    ])
   });

   module.exports=router;
   ```

7. Uložte všechny upravené soubory. 

8. Ve Visual Studio Code klikněte na tlačítko **Ladit** ![Ikona Ladit ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part2/debug-button.png), klikněte na tlačítko ozubeného kola ![Tlačítko ozubeného kola ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part2/gear-button.png) a pak vyberte **Node.js** pro vytvoření konfigurace.

   Ve Visual Studio Code se otevře nový soubor launch.js.

8. Na řádku 11 souboru launch.json změňte `"program": "${file}"` na `"program": "${workspaceRoot}/src/server/index.js"` a uložte soubor.

9. Kliknutím na tlačítko **Spustit ladění** ![Ikona Spustit ladění ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part2/start-debugging-button.png) spusťte aplikaci.

    Aplikace by se měla spustit bez chyb.

## <a name="use-postman-to-test-the-app"></a>Testování aplikace pomocí nástroje Postman

1. Teď otevřete nástroj Postman a do pole GET vložte `http://localhost:3000/api/heroes`. 

2. Klikněte na tlačítko **Send** (Odeslat) a získejte odpověď aplikace ve formátu JSON. 

    Tato odpověď značí, že aplikace funguje a je spuštěná v místním prostředí. 

    ![Postman se zobrazeným požadavkem a odpovědí](./media/tutorial-develop-mongodb-nodejs-part2/azure-cosmos-db-postman.png)


## <a name="next-steps"></a>Další kroky

V této části kurzu jste provedli následující:

> [!div class="checklist"]
> * Vytvořili jste projekt Node.js pomocí Angular CLI.
> * Otestovali jste aplikaci pomocí nástroje Postman.

Můžete přejít k další části kurzu a vytvořit uživatelské rozhraní.

> [!div class="nextstepaction"]
> [Vytvoření uživatelského rozhraní pomocí Angular](tutorial-develop-mongodb-nodejs-part3.md)
