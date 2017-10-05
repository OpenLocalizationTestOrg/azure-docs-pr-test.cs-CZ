---
title: "Nasazení webové aplikace Sails.js do služby Azure App Service | Microsoft Docs"
description: "Informace o nasazení aplikace Node.js Azure App Service. V tomto kurzu se dozvíte, jak nasadit webové aplikace Sails.js."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 8877ddc8-1476-45ae-9e7f-3c75917b4564
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: e36fc5f5273f98c3cca91973db9910f32443ec7c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Nasazení webové aplikace Sails.js do služby Azure App Service
V tomto kurzu se dozvíte, jak nasadit aplikace Sails.js do služby Azure App Service. V procesu můžete glean některé obecné znalosti o tom, jak nakonfigurovat aplikace Node.js ke spuštění ve službě App Service.

Zde se dozvíte, mají užitečné dovedností:

* Konfigurace aplikace Sails.js spustit ve službě App Service.
* Nasazení aplikace do služby App Service z příkazového řádku.
* Čtení stderr a stdout protokoly a vyřešte všechny problémy nasazení.
* Uložení proměnné prostředí mimo zdrojového kódu.
* Přístup k proměnným prostředí Azure z vaší aplikace.
* Připojení k databázi (MongoDB).

Měli byste mít praktické znalosti Sails.js. V tomto kurzu není určeno k vám pomohou s problémy související se spouštěním Sail.js obecně.

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

- [Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
- [Azure CLI 2.0](app-service-web-nodejs-sails.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

## <a name="prerequisites"></a>Požadavky
* [Node.js](https://nodejs.org/)
* [Sails.js](http://sailsjs.org/get-started)
* [Git](http://www.git-scm.com/downloads)
* [Azure CLI 2.0](/cli/azure/install-az-cli2)
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure. Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.
> 
> 

## <a name="step-1-create-and-configure-a-sailsjs-app-locally"></a>Krok 1: Vytvoření a konfigurace aplikace Sails.js místně
Nejprve rychle vytvořte výchozí Sails.js aplikace ve vašem vývojovém prostředí pomocí následujících kroků:

1. Otevřete terminál příkazového řádku podle svého výběru a `CD` do pracovního adresáře.
2. Vytvoření aplikace Sails.js a potom ho spusťte:

        sails new <app_name>
        cd <app_name>
        sails lift

    Ujistěte se, že můžete přejít na domovskou stránku výchozí v http://localhost:1377.

1. V dalším kroku povolení protokolování pro Azure. V kořenovém adresáři, vytvořte soubor s názvem `iisnode.yml` a přidejte následující dva řádky:

        loggingEnabled: true
        logDirectory: iisnode

    Je nyní povoleno protokolování [iisnode](https://github.com/tjanczuk/iisnode) serveru, který používá Azure App Service spouští aplikace Node.js. 
    Další informace o tom, jak to funguje, najdete v části [postup ladění webové aplikace Node.js ve službě Azure App Service](web-sites-nodejs-debug.md).

2. V dalším kroku nakonfigurujte aplikace Sails.js používat proměnné prostředí Azure. Otevřete config/env/production.js konfigurovat provozním prostředí, a nastavte `port` a `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Tato nastavení konfigurace v dokumentaci můžete najít [Sails.js dokumentaci](http://sailsjs.org/documentation/reference/configuration/sails-config).

4. V dalším kroku závislé Node.js verze, kterou chcete použít. V souboru package.json, přidejte následující `engines` vlastnost nastavující verze Node.js na takový, který má být.

        "engines": {
            "node": "6.9.1"
        },

5. Nakonec inicializovat úložiště Git a potvrďte svoje soubory. V kořenovém adresáři aplikace (kde package.json je) spusťte následující příkazy Git:

        git init
        git add .
        git commit -m "<your commit message>"

Váš kód je připravená k nasazení. 

## <a name="step-2-create-an-azure-app-and-deploy-sailsjs"></a>Krok 2: Vytvoření aplikace Azure a nasadit Sails.js

V dalším kroku v Azure vytvořit prostředek služby App Service a nasazení aplikace Sails.js do ní.

1. Přihlaste se k Azure takto proto:

        az login

    Postupujte podle výzvy a pokračujte v přihlášení v prohlížeči pomocí účtu Microsoft, který obsahuje předplatné Azure.

3. Nastavte uživatele nasazení pro App Service. Později pomocí těchto přihlašovacích údajů nasadíte kód.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s názvem. V tomto kurzu Node.js nepotřebujete skutečně potřebujete vědět, co je.

        az group create --location "<location>" --name my-sailsjs-app-group

    K zobrazení možných hodnot, které se dají použít pro `<location>`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku.

3. Vytvoření "FREE" [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) s názvem. V tomto kurzu Node.js právě vědět, že vám nebude nic účtováno pro webové aplikace v tomto plánu.

        az appservice plan create --name my-sailsjs-appservice-plan --resource-group my-sailsjs-app-group --sku FREE

4. Vytvořte novou webovou aplikaci s jedinečným názvem ve značce `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-sailsjs-app-group --plan my-sailsjs-appservice-plan

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Krok 3: Konfigurace a nasazení aplikace Sails.js

1. Ke konfiguraci místního nasazení Gitu pro webovou aplikaci použijete následující příkaz:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-sailsjs-app-group

    Získáte výstup JSON podobný tomuto. To znamená, že vzdálené úložiště Git je nakonfigurované:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Přidejte adresu URL v kódu JSON jako vzdálené úložiště Git pro vaše místní úložiště (pro zjednodušení nazvané `azure`).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Nasaďte ukázkový kód do vzdáleného úložiště Git s názvem `azure`. Po zobrazení výzvy použijte přihlašovací údaje nasazení, které jste nakonfigurovali dříve.

        git push azure master

7. Právě nakonec spusťte živou aplikaci Azure v prohlížeči:

        az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

    Měli byste nyní vidět domovské stránce stejné Sails.js.

    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Řešení potíží s nasazením
Pokud vaše aplikace Sails.js selže z nějakého důvodu ve službě App Service, najděte protokoly stderr k jeho řešení.
Další informace najdete v tématu [postup ladění webové aplikace Node.js ve službě Azure App Service](web-sites-nodejs-debug.md).
Pokud aplikace byla úspěšně spuštěna, protokol stdout měl zobrazovat známé zprávu:

                   .-..-.
    
       Sails              <|    .-..-.
       v0.12.11            |\
                          /|.\
                         / || \
                       ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
       __---___--___---___--___---___--___
     ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Můžete řídit členitost protokoly stdout v [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) souboru.

## <a name="connect-to-a-database-in-azure"></a>Připojení k databázi v Azure
Pro připojení k databázi v Azure, vytvořte databázi zvoleného v Azure, například Azure SQL Database, MySQL, MongoDB, Azure (Redis) Cache, atd. a použijte příslušné [úložiště adaptér](https://github.com/balderdashy/sails#compatibility) připojení k tomuto. Kroky v této části ukazují, jak se připojit k MongoDB pomocí [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) databáze, který podporuje připojení klienta MongoDB.

1. [Vytvoření účtu Cosmos DB s podporou protokolů pro MongoDB](../documentdb/documentdb-create-mongodb-account.md).
2. [Vytvoření kolekce Cosmos databáze a databáze](../documentdb/documentdb-create-collection.md). Název kolekce není důležité, ale při připojení z Sails.js potřebujete název databáze.
3. [Najít informace o připojení pro vaši databázi Cosmos DB](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).
2. Z příkazového řádku terminálu nainstalujte adaptér MongoDB:

        npm install sails-mongo --save

3. Otevřete config/connections.js a přidejte následující objekt připojení do seznamu:

        docDbMongo: {
            adapter: 'sails-mongo',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost,
            port: process.env.dbport,
            database: process.env.dbname,
            ssl: true
        },

    > [!NOTE] 
    > `ssl: true` Možnost je důležité, protože [Cosmos DB vyžaduje](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 
    >
    >

4. Pro každou proměnnou prostředí (`process.env.*`), je nutné nastavit ve službě App Service. Chcete-li to provést, spusťte následující příkazy z terminálu. Použijte informace o připojení pro vaše DB Cosmos.

        az appservice web config appsettings update --settings dbuser="<database user>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbpassword="<database password>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbhost="<database hostname>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbport="<database port>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbname="<database name>" --name <app_name> --resource-group my-sailsjs-app-group

    Uvedení nastavení v nastavení aplikace Azure zajistí citlivých dat mimo správy zdrojového kódu (Git). Dále nakonfigurujete vývojového prostředí používat stejné informace o připojení.
5. Otevřete config/local.js a přidejte následující objekt připojení:

        connections: {
            docDbMongo: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>",
                database: "<database name>",
                ssl: true
            },
        },

    Tato konfigurace se přepíše nastavení v souboru config/connections.js pro místní prostředí. Tento soubor je vyloučená, a výchozí .gitignore ve vašem projektu, takže ho nebude uložena v úložišti Git. Nyní budete moci připojit k vaší databázi DB Cosmos (MongoDB) z vaší webové aplikace Azure a z místního vývojového prostředí.
6. Otevřete config/env/production.js konfigurace produkční prostředí a přidejte následující `models` objektu:

        models: {
            connection: 'docDbMongo',
            migrate: 'safe'
        },
7. Otevřete config/env/development.js konfigurace vývojového prostředí a přidejte následující `models` objektu:

        models: {
            connection: 'docDbMongo',
            migrate: 'alter'
        },

    `migrate: 'alter'`Umožňuje použít k vytváření a aktualizaci databáze kolekce nebo tabulky snadno funkce migrace databáze. Ale `migrate: 'safe'` se používá pro vaše prostředí Azure (produkční), protože Sails.js neumožňuje použití `migrate: 'alter'` v produkčním prostředí (v tématu [Sails.js dokumentace](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).
8. Z terminálu [generovat](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [plán, podle kterého rozhraní API](http://sailsjs.org/documentation/concepts/blueprints) jako za normálních okolností byste, pak spustili `sails lift` vytvořit databázi s migrací Sails.js databáze. Například:

         sails generate api mywidget
         sails lift

    `mywidget` Model generované tento příkaz je prázdný, ale nemůžeme můžete ji použít k ukazují, že jsme mají připojení k databázi.
    Při spuštění `sails lift`, vytvoří chybějící kolekce a tabulky pro modely vaše aplikace používá.
9. Přístup k matrici rozhraní API, které jste právě vytvořili, v prohlížeči. Například:

        http://localhost:1337/mywidget/create

    Rozhraní API by měl vrátit vytvořenou položku vám v okně prohlížeče, což znamená, že vaše kolekce byla úspěšně vytvořena.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}
10. Nyní odešlete své změny do Azure a přejděte do vaší aplikace a ujistěte se, že stále funguje.

         git add .
         git commit -m "<your commit message>"
         git push azure master
         az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

11. Přístup plán, podle kterého rozhraní API Azure webové aplikace. Například:

         http://<appname>.azurewebsites.net/mywidget/create

     Pokud rozhraní API vrátí jiné nový záznam, vaší webové aplikace Azure rozhovoru k vaší databázi DB Cosmos (MongoDB).

## <a name="more-resources"></a>Další zdroje informací
* [Začínáme s webovými aplikacemi Node.js ve službě Azure App Service](app-service-web-get-started-nodejs.md)
* [Používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md)
