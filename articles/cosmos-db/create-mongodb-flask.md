---
title: "Azure Cosmos DB: Sestavení webové aplikace Flask pomocí jazyka Python a rozhraní API MongoDB | Microsoft Docs"
description: "Představuje ukázku kódu Python Flask, kterou můžete použít k připojení a dotazování do rozhraní API MongoDB služby Azure Cosmos DB."
services: cosmos-db
documentationcenter: 
author: hshapiro
manager: scicoria
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 10/2/2017
ms.author: hshapiro
ms.openlocfilehash: f86c6cce82812e02f373d7307c76ace26ea3e99b
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="azure-cosmos-db-build-a-flask-app-with-the-mongodb-api"></a>Azure Cosmos DB: Sestavení aplikace Flask pomocí rozhraní API MongoDB

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.

Tato úvodní příručka používá následující [příklad Flask](https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample) a ukazuje, jak vytvořit jednoduchou aplikaci seznamu úkolů Flask pomocí [emulátoru Azure Cosmos DB](/local-emulator.md) místo MongoDB.

## <a name="prerequisites"></a>Požadavky

- Stáhněte si [emulátor služby Azure Cosmos DB](/local-emulator.md). Emulátor je momentálně podporovaný jenom ve Windows. Tato ukázka vysvětluje, jak ukázku použít s produkčním klíčem z Azure, což se dá udělat na libovolné platformě.

- Pokud ještě nemáte nainstalovaný editor Visual Studio Code, můžete rychle nainstalovat editor [VS Code](https://code.visualstudio.com/Download) pro vaši platformu (Windows, Mac, Linux).

- Nezapomeňte přidat podporu jazyka Python instalací některého z oblíbených rozšíření Pythonu.
    1. Vyberte rozšíření.
    2. Nainstalujte rozšíření zadáním příkazu `ext install` do palety příkazů `Ctrl+Shift+P`.

    Příklady v tomto dokumentu využívají oblíbené [rozšíření Python](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python) s plnou sadou funkcí, jehož autorem je Don Jayamanne.

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď naklonujeme aplikaci rozhraní API Flask-MongoDB z GitHubu, nastavíme připojovací řetězec a spustíme ji. Uvidíte, jak snadno se pracuje s daty prostřednictvím kódu programu.

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.
2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu.

    ```bash
    git clone https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git
    ```
3. Spuštěním následujícího příkazu nainstalujte moduly Pythonu.

    ```bash 
    pip install -r .\requirements.txt
    ```
4. Otevřete složku otevřete v editoru Visual Studio Code.

## <a name="review-the-code"></a>Kontrola kódu

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete soubor **app-py** v kořenovém adresáři a zjistíte, že tyto řádky kódu vytvářejí prostředky služby Azure Cosmos DB. Následující kód používá připojovací řetězec pro místní emulátor služby Azure Cosmos DB. Heslo je potřeba rozdělit, jak vidíte níže, aby se dala použít lomítka, která se jinak nedají analyzovat.

* Inicializujte klienta MongoDB, načtěte databázi a proveďte ověření.

    ```python
    client = MongoClient("mongodb://127.0.0.1:10250/?ssl=true") #host uri
    db = client.test    #Select the database
    db.authenticate(name="localhost",password='C2y6yDjf5' + r'/R' + '+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw' + r'/Jw==')
    ```

* Načtěte kolekci nebo ji vytvořte, pokud ještě neexistuje.

    ```python
    todos = db.todo #Select the collection
    ```

* Vytvoření aplikace

    ```Python
    app = Flask(__name__)
    title = "TODO with Flask"
    heading = "ToDo Reminder"
    ```
    
## <a name="run-the-web-app"></a>Spuštění webové aplikace

1. Zkontrolujte, jestli je spuštěný emulátor služby Azure Cosmos DB.

2. Otevřete okno a příkazem `cd` přejděte do adresáře, ve kterém je aplikace uložená.

3. Potom nastavte proměnnou prostředí pro aplikaci Flask na `set FLASK_APP=app.py`, případně `export FLASK_APP=app.py`, pokud používáte systém Mac.

4. Spusťte aplikaci pomocí příkazu `flask run` a přejděte na adresu [http://127.0.0.1:5000/](http://127.0.0.1:5000/).

5. Přidávejte a odebírejte úlohy a sledujte, jak se objevují a mění v rámci kolekce.

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Pokud chcete otestovat na aktivním účtu služby Azure Cosmos DB, přejděte na portál Azure, vytvořte účet a získejte připojovací řetězec. Potom ho zkopírujte do aplikace.

1. Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Připojovací řetězec** a potom klikněte na **Klíče pro čtení i zápis**. V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky uživatelské jméno, heslo a hostitele do souboru Dal.cs.

2. Otevřete soubor **app.py** v kořenovém adresáři.

3. Z portálu zkopírujte hodnotu **username** (pomocí tlačítka kopírování) a nastavte ji jako hodnotu položky **name** v souboru **app.py**.

4. Potom zkopírujte hodnotu **connection string** z portálu a nastavte ji jako hodnotu položky MongoClient v souboru **app.py**.

5. Nakonec z portálu zkopírujte hodnotu **password** a nastavte ji jako hodnotu **password** v souboru **app.py**.

Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB. Můžete ji spustit stejným způsobem jako předtím.

## <a name="deploy-to-azure"></a>Nasazení do Azure

Pokud chcete tuto aplikaci nasadit, můžete v Azure vytvořit novou webovou aplikaci a povolit průběžné nasazování ve forku tohoto úložiště GitHub. Postup nastavení průběžného nasazování pomocí GitHubu v Azure najdete v tomto [kurzu](https://docs.microsoft.com/azure/app-service-web/app-service-continuous-deployment).

Při nasazování do Azure je potřeba odebrat klíče aplikace a ujistit se, následující část není označená jako komentář:

```python
    client = MongoClient(os.getenv("MONGOURL"))
    db = client.test    #Select the database
    db.authenticate(name=os.getenv("MONGO_USERNAME"),password=os.getenv("MONGO_PASSWORD"))
```

Potom musíte přidat do nastavení aplikace vaše položky MONGOURL, MONGO_PASSWORD a MONGO_USERNAME. V tomto [kurzu](https://docs.microsoft.com/azure/app-service-web/web-sites-configure#application-settings) se dozvíte další informace o nastavení aplikace ve službě Azure Web Apps.

Pokud nechcete vytvořit fork tohoto úložiště, můžete také kliknout na níže uvedené tlačítko Nasazení do Azure. Pak byste měli přejít do Azure a upravit nastavení aplikace pomocí údajů o vašem účtu služby Cosmos DB.

<a href="https://deploy.azure.com/?repository=https://github.com/heatherbshapiro/To-Do-List---Flask-MongoDB-Example" target="_blank">
<img src="http://azuredeploy.net/deploybutton.png"/>
</a>

> [!NOTE]
> Pokud máte v plánu uložit svůj kód na GitHub nebo chcete použít jiné možnosti správy zdrojového kódu, nezapomeňte z kódu odebrat připojovací řetězce. Ty je možné místo toho nastavit pomocí nastavení dané webové aplikace.

## <a name="review-slas-in-the-azure-portal"></a>Ověření smluv SLA na webu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření účtu služby Azure Cosmos DB a spuštění aplikace Flask pomocí rozhraní API pro MongoDB. Teď můžete do svého účtu služby Cosmos DB importovat další data.

> [!div class="nextstepaction"]
> [Import dat do databáze Azure Cosmos pro rozhraní API MongoDB](mongodb-migrate.md)
