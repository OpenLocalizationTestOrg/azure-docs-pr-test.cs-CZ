---
title: "Služba Azure Cosmos DB: Sestavení webové aplikace s ověřením přes Xamarin a Facebook | Dokumentace Microsoftu"
description: "Uvede ukázku kódu rozhraní .NET, můžete použít tooconnect tooand dotaz na databázi Azure Cosmos"
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
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 5f71dddd2b2f5bd117e481c96c17915fc58d2119
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-web-app-with-net-xamarin-and-facebook-authentication"></a>Služba Azure Cosmos DB: Sestavení webové aplikace s rozhraním .NET a ověřením přes Xamarin a Facebook

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure. Budete pak sestavení a nasazení webové aplikace seznamu úkolů založený na hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/)a modul autorizace Azure Cosmos DB hello. Hello todo seznamu webové aplikace implementuje vzor data na uživatele, který umožňuje uživatelům toologin pomocí ověřování sítě Facebook a spravovat své vlastní toodo položky.

## <a name="prerequisites"></a>Požadavky

Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Přidání kolekce

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Teď umožňuje nastavit připojovací řetězec hello klonování DocumentDB API aplikace z githubu a potom ho spusťte. Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.  

2. Spusťte následující příkaz tooclone hello Ukázka úložiště hello. 

    ```bash
    git clone https://github.com/Azure/azure-documentdb-dotnet.git
    ```

3. Pak otevřete soubor DocumentDBTodo.sln hello ze složky samples/xamarin/UserItems/xamarin.forms hello v sadě Visual Studio. 

## <a name="review-hello-code"></a>Zkontrolujte hello kódu

Hello kódu ve složce Xamarin hello obsahuje:

* Aplikaci Xamarin. aplikace Hello ukládá položky úkolů hello uživatele v dělenou kolekci s názvem UserItems.
* Rozhraní API zprostředkovatele tokenu prostředku. Prostředek Azure Cosmos DB toobroker jednoduché rozhraní ASP.NET Web API tokeny toohello přihlášení uživatele aplikace hello. Tokeny prostředků jsou krátkodobou přístupové tokeny, které aplikace hello poskytnout toohello přístup hello přihlášení uživatelská data.

tok ověřování a data Hello je znázorněno v následujícím diagramu hello.

* Hello UserItems kolekce je vytvořena s klíčem oddílu hello "/ userid'. Určení, že klíč oddílu pro kolekci nekonečnou umožňuje tooscale Azure Cosmos DB jako hello počet uživatelů a položky zvětšování.
* umožňuje aplikaci Xamarin Hello toologin uživatelů s přihlašovacími údaji Facebook.
* aplikace Xamarin Hello používá tooauthenticate tokenu přístupu Facebook s rozhraním API ResourceTokenBroker
* Hello prostředků zprostředkovatele tokenu rozhraní API ověří žádost o hello pomocí funkce ověřování aplikace služby a požadavky tokenu prostředků Azure Cosmos DB s dokumenty tooall přístup pro čtení a zápis sdílení klíč oddílu hello ověřeného uživatele.
* Token zprostředkovatele prostředků vrátí hello prostředků tokenu toohello klientskou aplikaci.
* aplikace Hello přístup k položek todo hello uživatele pomocí tokenu hello prostředků.

![Aplikace seznamu úkolů s ukázkovými daty](./media/create-documentdb-xamarin-dotnet/tokenbroker.png)
    
## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.

1. V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**. Do souboru Web.config hello v dalším kroku hello použijete hello tlačítka Kopírovat na pravé straně hello hello obrazovky toocopy hello URI a primární klíč.

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-xamarin-dotnet/keys.png)

2. V aplikaci Visual Studio 2017 otevřete soubor Web.config hello ve složce azure-documentdb-dotnet nebo ukázky nebo xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker hello. 

3. Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu hello accountUrl v souboru Web.config. 

    `<add key="accountUrl" value="{Azure Cosmos DB account URL}"/>`

4. Poté zkopírujte primární klíč hodnota z portálu hello a nastavit jej jako hodnota hello accountKey v Web.congif hello. 

    `<add key="accountKey" value="{Azure Cosmos DB secret}"/>`

Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB. 

## <a name="build-and-deploy-hello-web-app"></a>Vytvoření a nasazení webové aplikace hello

1. V hello portálu Azure vytvořte App Service Web toohost hello prostředků tokenu zprostředkovatele rozhraní API.
2. V hello portálu Azure otevřete okno nastavení aplikace hello hello prostředků zprostředkovatele tokenu web API. Vyplňte následující nastavení aplikace hello:

    * accountUrl – adresa URL účtu Azure Cosmos DB hello z karty hello klíčů účtu Azure Cosmos DB.
    * accountKey - hello Azure Cosmos DB účet hlavní klíč z karty hello klíčů účtu Azure Cosmos DB.
    * Parametr DatabaseId a collectionId vytvořené databáze a kolekce

3. Publikujte hello tooyour řešení ResourceTokenBroker vytvořit web.

4. Otevřete projekt Xamarin hello a přejděte tooTodoItemManager.cs. Vyplňte hodnoty hello accountURL, collectionId, hodnotu databaseId, jakož i resourceTokenBrokerURL hello adresy url základní https pro web tokenu zprostředkovatele prostředků hello.

5. Dokončení hello [jak tooconfigure přihlašování Facebook toouse aplikace služby App Service](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) ověřování Facebook kurz toosetup a nakonfigurovat web ResourceTokenBroker hello.

    Spuštění aplikace Xamarin hello.

## <a name="review-slas-in-hello-azure-portal"></a>Zkontrolujte SLA v hello portálu Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky: 

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředku, kterou jste právě vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tento rychlý start když jste se naučili jak toocreate účtu Azure Cosmos DB, vytvořte kolekci pomocí hello Průzkumníku dat a sestavení a nasazení aplikace Xamarin. Nyní můžete importovat další data tooyour Cosmos DB účtu. 

> [!div class="nextstepaction"]
> [Importování dat do služby Azure Cosmos DB](import-data.md)
