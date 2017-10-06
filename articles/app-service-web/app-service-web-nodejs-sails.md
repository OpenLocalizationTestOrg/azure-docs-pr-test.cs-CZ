---
title: "aaaDeploy Sails.js webové aplikace tooAzure služby App Service | Microsoft Docs"
description: "Zjistěte, jak toodeploy aplikace Node.js Azure App Service. Tento kurz ukazuje, jak toodeploy Sails.js webové aplikace."
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
ms.openlocfilehash: f5b2518b9c87c040845f7268763862be8c15e83e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-sailsjs-web-app-tooazure-app-service"></a>Nasazení aplikace tooAzure Sails.js webové služby App Service
Tento kurz ukazuje, jak toodeploy tooAzure Sails.js aplikace služby App Service. V procesu hello můžete glean některé obecné znalosti o tom, tooconfigure vaše toorun aplikace Node.js ve službě App Service.

Zde se dozvíte, mají užitečné dovedností:

* Konfigurace aplikace Sails.js spustit ve službě App Service.
* Nasaďte aplikaci tooApp služby z příkazového řádku hello.
* Přečtěte si protokoly tootroubleshoot stderr a stdout žádné problémy při nasazení.
* Uložení proměnné prostředí mimo zdrojového kódu.
* Přístup k proměnným prostředí Azure z vaší aplikace.
* Připojte databáze tooa (MongoDB).

Měli byste mít praktické znalosti Sails.js. V tomto kurzu není určený toohelp jste s problémy související s toorunning Sail.js obecně.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely
- [Azure CLI 2.0](app-service-web-nodejs-sails.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="prerequisites"></a>Požadavky
* [Node.js](https://nodejs.org/)
* [Sails.js](http://sailsjs.org/get-started)
* [Git](http://www.git-scm.com/downloads)
* [Azure CLI 2.0](/cli/azure/install-az-cli2)
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure. Vytvoření aplikace starter- and -play se pro až hodinu tooan – nepožaduje se žádná platební karta a bez jakýchkoli závazků.
> 
> 

## <a name="step-1-create-and-configure-a-sailsjs-app-locally"></a>Krok 1: Vytvoření a konfigurace aplikace Sails.js místně
Nejprve rychle vytvořte výchozí Sails.js aplikace ve vašem vývojovém prostředí pomocí následujících kroků:

1. Otevřete hello terminál příkazového řádku podle svého výběru a `CD` tooa pracovní adresář.
2. Vytvoření aplikace Sails.js a potom ho spusťte:

        sails new <app_name>
        cd <app_name>
        sails lift

    Ujistěte se, že přejdete toohello výchozí domovskou stránku v http://localhost:1377.

1. V dalším kroku povolení protokolování pro Azure. V kořenovém adresáři, vytvořte soubor s názvem `iisnode.yml` a přidejte následující dva řádky hello:

        loggingEnabled: true
        logDirectory: iisnode

    Hello je nyní povoleno protokolování [iisnode](https://github.com/tjanczuk/iisnode) server, že Azure App Service používá toorun aplikací Node.js. 
    Další informace o tom, jak to funguje, najdete v části [jak toodebug Node.js webové aplikace v Azure App Service](web-sites-nodejs-debug.md).

2. V dalším kroku nakonfigurujte hello Sails.js aplikace toouse prostředí Azure proměnné. Otevřete config/env/production.js tooconfigure provozním prostředí a nastavte `port` a `hookTimeout`:

        module.exports = {

            // Use process.env.port toohandle web requests toohello default HTTP port
            port: process.env.port,
            // Increase hooks timout too30 seconds
            // This avoids hello Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Tato nastavení konfigurace v dokumentaci můžete najít [Sails.js dokumentaci](http://sailsjs.org/documentation/reference/configuration/sails-config).

4. Dále používat pevné kódování hello verze Node.js, že chcete toouse. V souboru package.json, přidejte následující hello `engines` vlastnost tooset hello Node.js verze tooone, který má být.

        "engines": {
            "node": "6.9.1"
        },

5. Nakonec inicializovat úložiště Git a potvrďte svoje soubory. V hello kořenový adresář aplikace (kde package.json je), spusťte následující příkazy Gitu hello:

        git init
        git add .
        git commit -m "<your commit message>"

Váš kód je připraven toobe nasazení. 

## <a name="step-2-create-an-azure-app-and-deploy-sailsjs"></a>Krok 2: Vytvoření aplikace Azure a nasadit Sails.js

Dále vytvořte hello prostředek služby App Service v Azure a nasazení vaší aplikace tooit Sails.js.

1. přihlášení tooAzure takto:

        az login

    Postupujte podle hello výzva toocontinue hello přihlášení v prohlížeči pomocí účtu Microsoft, který obsahuje předplatné Azure.

3. Nastavit uživatele hello nasazení pro službu App Service. Později pomocí těchto přihlašovacích údajů nasadíte kód.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s názvem. V tomto kurzu Node.js nepotřebujete skutečně tooknow co je.

        az group create --location "<location>" --name my-sailsjs-app-group

    toosee jaké možné hodnoty, které můžete použít pro `<location>`, použijte hello `az appservice list-locations` rozhraní příkazového řádku příkaz.

3. Vytvoření "FREE" [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) s názvem. V tomto kurzu Node.js právě vědět, že vám nebude nic účtováno pro webové aplikace v tomto plánu.

        az appservice plan create --name my-sailsjs-appservice-plan --resource-group my-sailsjs-app-group --sku FREE

4. Vytvořte novou webovou aplikaci s jedinečným názvem ve značce `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-sailsjs-app-group --plan my-sailsjs-appservice-plan

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Krok 3: Konfigurace a nasazení aplikace Sails.js

1. Nakonfigurujte místní nasazení Git pro vaše nová webová aplikace s hello následující příkaz:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-sailsjs-app-group

    Zobrazí se výstup JSON tímto způsobem, což znamená, že je nakonfigurovaný vzdáleného úložiště Git této hello:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Adresa URL hello v hello JSON přidat jako vzdálené řízení Git pro místní úložiště (nazývá `azure` pro jednoduchost).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Nasazení vaší ukázkový kód toohello `azure` vzdálené Git. Pokud budete vyzváni, použijte hello přihlašovací údaje nasazení, které jste nakonfigurovali dříve.

        git push azure master

7. Právě nakonec spusťte živou aplikaci Azure v prohlížeči hello:

        az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

    Teď byste měli vidět text hello stejné Sails.js domovské stránky.

    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Řešení potíží s nasazením
Pokud vaše aplikace Sails.js selže z nějakého důvodu ve službě App Service, najít protokoly stderr hello toohelp vyřešte je.
Další informace najdete v tématu [jak toodebug Node.js webové aplikace v Azure App Service](web-sites-nodejs-debug.md).
Pokud aplikace hello byla úspěšně spuštěna, hello stdout protokolu by měl zobrazit jste obeznámeni uvítací zprávu:

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
    toosee your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    tooshut down Sails, press <CTRL> + C at any time.

Můžete řídit členitost protokoly stdout hello v hello [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) souboru.

## <a name="connect-tooa-database-in-azure"></a>Připojit databáze tooa v Azure
tooconnect tooa databáze v Azure, můžete vytvořit databázi hello zvoleného v Azure, například Azure SQL Database, MySQL, MongoDB, Azure (Redis) Cache, atd. a pomocí odpovídající hello [úložiště adaptér](https://github.com/balderdashy/sails#compatibility) tooconnect tooit. Hello kroky v této části ukazují, jak tooconnect tooMongoDB pomocí [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) databáze, který podporuje připojení klienta MongoDB.

1. [Vytvoření účtu Cosmos DB s podporou protokolů pro MongoDB](../documentdb/documentdb-create-mongodb-account.md).
2. [Vytvoření kolekce Cosmos databáze a databáze](../documentdb/documentdb-create-collection.md). Název Hello hello kolekce není důležité, ale při připojení z Sails.js bude nutné hello název databáze hello.
3. [Najít informace o připojení hello pro vaši databázi Cosmos DB](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).
2. Z příkazového řádku terminálu nainstalujte adaptér MongoDB hello:

        npm install sails-mongo --save

3. Otevřete config/connections.js a přidejte následující seznam toohello objekt připojení hello:

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
    > Hello `ssl: true` možnost je důležité, protože [Cosmos DB vyžaduje](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 
    >
    >

4. Pro každou proměnnou prostředí (`process.env.*`), je nutné tooset ji ve službě App Service. toodo se spuštění hello následujících příkazů z terminálu. Informace o připojení hello použijte pro vaše DB Cosmos.

        az appservice web config appsettings update --settings dbuser="<database user>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbpassword="<database password>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbhost="<database hostname>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbport="<database port>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbname="<database name>" --name <app_name> --resource-group my-sailsjs-app-group

    Uvedení nastavení v nastavení aplikace Azure zajistí citlivých dat mimo správy zdrojového kódu (Git). Dále je nutné nakonfigurovat váš vývojový prostředí toouse hello stejné informace o připojení.
5. Otevřete config/local.js a přidejte následující objekt připojení hello:

        connections: {
            docDbMongo: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>",
                database: "<database name>",
                ssl: true
            },
        },

    Tato konfigurace se přepíše hello nastavení v souboru config/connections.js pro místní prostředí hello. Tento soubor je vyloučený hello výchozí .gitignore ve vašem projektu, takže ho nebude uložena v úložišti Git. Teď můžete mít tooconnect tooyour Cosmos DB (MongoDB) databáze je z vaší webové aplikace Azure a z místního vývojového prostředí.
6. Otevřete config/env/production.js tooconfigure provozním prostředí a přidejte následující hello `models` objektu:

        models: {
            connection: 'docDbMongo',
            migrate: 'safe'
        },
7. Otevřete config/env/development.js tooconfigure vývojového prostředí a přidejte následující hello `models` objektu:

        models: {
            connection: 'docDbMongo',
            migrate: 'alter'
        },

    `migrate: 'alter'`Umožňuje použít toocreate funkce migrace databáze a snadno aktualizovat kolekce databáze nebo tabulky. Však `migrate: 'safe'` se používá pro vaše prostředí Azure (produkční), protože Sails.js neumožňuje toouse `migrate: 'alter'` v produkčním prostředí (viz [Sails.js dokumentace](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).
8. Z hello terminálu [generovat](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [plán, podle kterého rozhraní API](http://sailsjs.org/documentation/concepts/blueprints) jako za normálních okolností byste, pak spustili `sails lift` k vytvoření databáze hello s migrací Sails.js databáze. Například:

         sails generate api mywidget
         sails lift

    Hello `mywidget` model generované tento příkaz je prázdný, ale můžeme ho použít tooshow, že máme připojení k databázi.
    Při spuštění `sails lift`, vytvoří hello chybějící kolekce a tabulky pro hello modely vaše aplikace používá.
9. API plán, podle kterého hello přístupu, kterou jste právě vytvořili, v prohlížeči hello. Například:

        http://localhost:1337/mywidget/create

    Hello rozhraní API by měl vrátit zpět tooyou hello vytvořit položku v okně prohlížeče hello, což znamená, že vaše kolekce byla úspěšně vytvořena.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}
10. Nyní push vaší tooAzure změny a procházet tooyour aplikace toomake se, že stále funguje.

         git add .
         git commit -m "<your commit message>"
         git push azure master
         az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

11. Přístup k rozhraní API hello plán, podle kterého Azure webové aplikace. Například:

         http://<appname>.azurewebsites.net/mywidget/create

     Pokud hello rozhraní API vrátí jinou novou položku, je vaše webové aplikace Azure rozhovoru tooyour DB Cosmos (MongoDB) databáze.

## <a name="more-resources"></a>Další zdroje informací
* [Začínáme s webovými aplikacemi Node.js ve službě Azure App Service](app-service-web-get-started-nodejs.md)
* [Používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md)
