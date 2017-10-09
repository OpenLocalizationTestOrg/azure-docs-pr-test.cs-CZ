---
title: "aaaConnect tooAzure aplikace MongoDB Cosmos DB pomocí Node.js | Microsoft Docs"
description: "Zjistěte, jak tooconnect existující aplikace Node.js MongoDB tooAzure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 06/19/2017
ms.author: mimig
ms.openlocfilehash: 4bc4f17a31d8c18d1ce5e3f002462f4d48eeb1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-migrate-an-existing-nodejs-mongodb-web-app"></a>Služba Azure Cosmos DB: Migrace stávající webové aplikace MongoDB s podporou Node.js 

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento rychlý start předvádí jak toouse existující [MongoDB](mongodb-introduction.md) aplikace napsané v Node.js a připojte ho tooyour Azure Cosmos DB databáze, která podporuje připojení klienta MongoDB. Jinými slovy aplikace Node.js pouze ví, že se počítač připojuje tooa databáze pomocí rozhraní API MongoDB. Je transparentní toohello aplikace, která hello dat je uložen v Azure Cosmos DB.

Po dokončení budete mít ve službě [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) spuštěnou aplikaci MEAN (MongoDB, Express, AngularJS a Node.js). 

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/create-mongodb-nodejs/meanjs-in-azure.png)


[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="prerequisites"></a>Požadavky 
Kromě toho tooAzure rozhraní příkazového řádku, musíte [Node.js](https://nodejs.org/) a [Git](http://www.git-scm.com/downloads) nainstalovány místně toorun `npm` a `git` příkazy.

Měli byste mít praktickou znalost Node.js. Tento rychlý start není určený toohelp k vývoji aplikací Node.js obecně.

## <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.  

Spusťte následující příkazy tooclone hello Ukázka úložiště hello. Tato ukázka úložiště obsahuje výchozí hello [MEAN.js](http://meanjs.org/) aplikace. 

```bash
git clone https://github.com/prashanthmadi/mean
```

## <a name="run-hello-application"></a>Spuštění aplikace hello

Instalace hello požadované balíčky a spuštění aplikace hello.

```bash
cd mean
npm install
npm start
```

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Pokud používáte nainstalovaný Azure CLI, přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů. Pokud používáte hello prostředí cloudu Azure, můžete tento krok přeskočit.

```azurecli
az login 
``` 
   
## <a name="add-hello-azure-cosmos-db-module"></a>Přidat modul Azure Cosmos DB hello

Pokud používáte nainstalované rozhraní příkazového řádku Azure, zkontrolujte toosee Pokud hello `cosmosdb` spuštěním hello je již nainstalována součást `az` příkaz. Pokud `cosmosdb` je v seznamu příkazů, základní text hello, pokračovat toohello další příkaz. Pokud používáte hello prostředí cloudu Azure, můžete tento krok přeskočit.

Pokud `cosmosdb` není v seznamu příkazů, základní text hello, přeinstalujte [Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s hello [vytvořit skupinu az](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například webové aplikace, databáze a účty úložiště. 

Hello následující příklad vytvoří skupinu prostředků v oblasti západní Evropa hello. Vyberte jedinečný název pro skupinu prostředků hello.

Pokud používáte prostředí cloudu Azure, klikněte na tlačítko **zkuste ho**, postupujte podle pokynů na obrazovce toologin hello a pak zkopírujte hello příkazu do příkazového řádku hello.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

## <a name="create-an-azure-cosmos-db-account"></a>Vytvoření účtu služby Azure Cosmos DB

Vytvoření účtu Azure Cosmos DB s hello [vytvořit az cosmosdb](/cli/azure/cosmosdb#create) příkaz.

V hello následující příkaz, nahraďte prosím vlastní jedinečný název účtu Azure Cosmos DB, kde uvidíte hello `<cosmosdb-name>` zástupný symbol. Tento jedinečný název se použije jako součást váš koncový bod Azure Cosmos DB (`https://<cosmosdb-name>.documents.azure.com/`), takže název hello musí toobe jedinečný mezi všechny účty Azure Cosmos DB v Azure. 

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

Hello `--kind MongoDB` parametr povolí připojení klientů MongoDB.

Při vytvoření účtu Azure Cosmos DB hello hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace. 

> [!NOTE]
> Tento příklad používá JSON jako hello rozhraní příkazového řádku Azure výstupní formát, což je výchozí hello. toouse jiné výstup formátu najdete v tématu [výstup formátů pro příkazy Azure CLI 2.0](https://docs.microsoft.com/cli/azure/format-output-azure-cli).

```json
{
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb-name>.documents.azure.com:443/",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Document
DB/databaseAccounts/<cosmosdb-name>",
  "kind": "MongoDB",
  "location": "West Europe",
  "name": "<cosmosdb-name>",
  "readLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ]
} 
```

## <a name="connect-your-nodejs-application-toohello-database"></a>Připojit databáze toohello aplikace Node.js

V tomto kroku připojíte MEAN.js ukázkové aplikace tooan Azure Cosmos DB databázi, kterou jste právě vytvořili, pomocí připojovacího řetězce MongoDB. 

<a name="devconfig"></a>
## <a name="configure-hello-connection-string-in-your-nodejs-application"></a>Konfigurace v aplikaci Node.js hello připojovací řetězec

V úložišti MEAN.js otevřete `config/env/local-development.js`.

Nahraďte hello obsah tohoto souboru hello následující kód. Ujistěte se, tooalso nahradit hello dva `<cosmosdb-name>` zástupné texty názvem svého účtu Azure Cosmos DB.

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

## <a name="retrieve-hello-key"></a>Načíst klíč hello

Pořadí tooconnect tooan Azure Cosmos DB databáze je nutné klíč databáze hello. Použití hello [az cosmosdb seznamu klíčů](/cli/azure/cosmosdb#list-keys) příkaz tooretrieve hello primární klíč.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb-name> --resource-group myResourceGroup --query "primaryMasterKey"
```

Hello rozhraní příkazového řádku Azure výstupy informace podobné toohello následující ukázka. 

```json
"RUayjYjixJDWG5xTqIiXjC..."
```

Zkopírujte hodnotu hello `primaryMasterKey`. Vložit přes hello `<primary_master_key>` v `local-development.js`.

Uložte provedené změny.

### <a name="run-hello-application-again"></a>Spusťte aplikaci hello znovu.

Spusťte `npm start` znovu. 

```bash
npm start
```

Zprávy konzoly by měl nyní zjistíte, že prostředí vývoj hello je spuštěná. 

Přejděte příliš`http://localhost:3000` v prohlížeči. Klikněte na tlačítko **zaregistrovat** v horní nabídce a zkuste to toocreate hello dvě fiktivní uživatele. 

Hello MEAN.js ukázkové aplikace ukládá data uživatele v databázi hello. Pokud jste úspěšné a do automaticky přihlásí MEAN.js hello vytvořený uživatel a funkčnost připojení k databázi Azure Cosmos. 

![MEAN.js připojí úspěšně tooMongoDB](./media/create-mongodb-nodejs/mongodb-connect-success.png)

## <a name="view-data-in-data-explorer"></a>Zobrazení dat v Průzkumníku dat

Data uložená pomocí Azure DB Cosmos je k dispozici tooview, dotazů a spuštění obchodní logiky na v hello portálu Azure.

tooview, dotazování a pracovat s daty uživatele hello vytvořili v předchozím kroku hello přihlášení toohello [portál Azure](https://portal.azure.com) ve webovém prohlížeči.

Hello nejvyšší vyhledávacího pole zadejte Azure Cosmos DB. Po otevření okna účtu služby Cosmos DB vyberte účet Cosmos DB. V levé navigační hello klikněte na tlačítko Průzkumníku dat. Rozbalte v podokně Kolekce hello kolekce a pak můžete zobrazit dokumenty hello hello kolekce, hello dotaz na data a i vytvořit a spustit uložené procedury, triggery a UDF. 

![Průzkumník dat v hello portálu Azure](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)


## <a name="deploy-hello-nodejs-application-tooazure"></a>Nasazení tooAzure aplikace Node.js hello

V tomto kroku nasadíte vaší tooAzure aplikace Node.js připojené MongoDB Cosmos DB.

Může mít k tomu, že hello konfigurační soubor, který jste změnili dříve je pro hello vývojové prostředí (`/config/env/local-development.js`). Při nasazení vaší aplikace tooApp služby, se spustí v provozním prostředí hello ve výchozím nastavení. Proto nyní, budete potřebovat toomake hello stejné změnit toohello příslušného konfiguračního souboru.

V úložišti MEAN.js otevřete `config/env/production.js`.

V hello `db` objektu, nahraďte hodnotu hello `uri` jako zobrazit v hello následující ukázka. Být jisti tooreplace hello zástupné symboly jako před.

```javascript
'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean?ssl=true&sslverifycertificate=false',
```

> [!NOTE] 
> Hello `ssl=true` možnost je důležité, protože [Azure Cosmos DB vyžaduje SSL](connect-mongodb-account.md#connection-string-requirements). 
>
>

V terminálu hello potvrďte všechny změny do Git. Můžete zkopírovat i toorun příkazy dohromady.

```bash
git add .
git commit -m "configured MongoDB connection string"
```
## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tento rychlý start, když jste se naučili jak toocreate Azure DB Cosmos účtu a vytvořte kolekci MongoDB pomocí Průzkumníku dat hello. Nyní můžete migrovat data tooAzure vaše MongoDB Cosmos DB.  

> [!div class="nextstepaction"]
> [Importování dat MongoDB do služby Azure Cosmos DB](mongodb-migrate.md)
