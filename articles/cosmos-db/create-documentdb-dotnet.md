---
title: "Azure Cosmos DB: Sestavení webové aplikace pomocí rozhraní .NET a hello rozhraní API DocumentDB | Microsoft Docs"
description: "Uvede ukázku kódu .NET pomocí dotazu tooand tooconnect hello rozhraní API služby Azure Cosmos databáze DocumentDB"
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
ms.openlocfilehash: 35517e35d80c48662a51a99814652ffa1121fc5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-hello-azure-portal"></a>Azure Cosmos DB: Sestavení webové aplikace DocumentDB rozhraní API pomocí rozhraní .NET a hello portálu Azure

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure. Budete pak sestavení a nasazení webové aplikace seznamu úkolů založený na hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), jak ukazuje následující snímek obrazovky hello. 

![Aplikace seznamu úkolů s ukázkovými daty](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a>Požadavky

Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
## <a name="add-a-collection"></a>Přidání kolekce

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Přidání ukázkových dat

Nyní můžete přidat tooyour nové shromažďování dat pomocí Průzkumníku dat.

1. V Průzkumníku dat hello nové databáze se zobrazí v podokně Kolekce hello. Rozbalte hello **úlohy** databáze, rozbalte položku hello **položky** kolekce, klikněte na tlačítko **dokumenty**a potom klikněte na **nové dokumenty**. 

   ![Vytváření nových dokumentů v Průzkumníku dat v hello portálu Azure](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. Nyní přidejte toohello kolekce dokumentů s hello strukturu.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Po přidání hello json toohello **dokumenty** , klikněte na **Uložit**.

    ![Kopírovat json data a klikněte na Uložit v Průzkumníku dat v hello portálu Azure](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  Vytvořte a uložte jeden další dokument, kde je vložit jedinečnou hodnotu pro hello `id` vlastnost a změňte hello ostatní vlastnosti podle potřeby. Nové dokumenty můžou mít jakoukoli strukturu, protože Azure Cosmos DB neuplatňuje pro data žádné schéma.

     Nyní můžete vytvořit dotazy v Průzkumníku dat tooretrieve vaše data. Ve výchozím Průzkumníku dat používá `SELECT * FROM c` tooretrieve všechny dokumenty v hello kolekce, ale můžete změnit této tooa různých [dotazu SQL](documentdb-sql-query.md), jako například `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn všechny dokumenty hello v sestupném pořadí podle jejich časové razítko.
 
     Můžete také použít Průzkumníku dat toocreate uložené procedury, funkce UDF a aktivační události tooperform serverovou obchodní logiku také jako propustnost škálování. Průzkumník dat zpřístupní všechny hello předdefinované programový přístup k datům v hello rozhraní API k dispozici, ale poskytuje snadný přístup k datům tooyour v hello portálu Azure.

## <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Teď umožňuje přepnout tooworking s kódem. Pojďme klonovat aplikace DocumentDB API z Githubu, nastavte hello připojovací řetězec a spusťte ho. Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a `CD` tooa pracovní adresář.  

2. Spusťte následující příkaz tooclone hello Ukázka úložiště hello. 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. Poté otevřete soubor řešení hello todo v sadě Visual Studio. 

## <a name="review-hello-code"></a>Zkontrolujte hello kódu

Provedeme jejich stručný přehled o dění v aplikaci hello. Otevřete hello DocumentDBRepository.cs souboru a najdete v ní vytvořit tyto řádky kódu hello prostředky Azure Cosmos DB. 

* na řádku 73 je inicializován Hello DocumentClient.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* Na řádku 88 se vytvoří nová databáze.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Na řádku 107 se vytvoří nová kolekce.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.

1. V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**. Do souboru web.config hello v dalším kroku hello použijete hello tlačítka Kopírovat na pravé straně hello hello obrazovky toocopy hello URI a primární klíč.

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-dotnet/keys.png)

2. V aplikaci Visual Studio 2017 otevřete soubor web.config hello. 

3. Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu klíče hello koncový bod v souboru web.config. 

    `<add key="endpoint" value="FILLME" />`

4. Poté zkopírujte primární klíč hodnota z portálu hello a nastavit jej jako hello hodnotu hello authKey v souboru web.config. Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB. 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-hello-web-app"></a>Spouštění hello webové aplikace
1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a pak klikněte na **spravovat balíčky NuGet**. 

2. V hello NuGet **Procházet** zadejte *DocumentDB*.

3. Z výsledků hello nainstalovat hello **Microsoft.Azure.DocumentDB** knihovny. Tím se nainstaluje balíček Microsoft.Azure.DocumentDB hello a také všechny závislosti.

4. Klikněte na kombinaci kláves CTRL + F5 toorun hello aplikace. Aplikace se zobrazí v prohlížeči. 

5. Klikněte na tlačítko **vytvořit nový** v hello prohlížeče a vytvořit pár nové úlohy ve vaší aplikaci seznamu úkolů.

   ![Aplikace seznamu úkolů s ukázkovými daty](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

Teď můžete přejít zpět tooData Průzkumníka a zobrazit dotaz, upravit a pracovat s Tato nová data. 

## <a name="review-slas-in-hello-azure-portal"></a>Zkontrolujte SLA v hello portálu Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tento rychlý start když jste se naučili jak toocreate účtu Azure Cosmos DB, vytvořte kolekci pomocí hello Průzkumníku dat a spusťte webovou aplikaci. Nyní můžete importovat další data tooyour Cosmos DB účtu. 

> [!div class="nextstepaction"]
> [Importování dat do služby Azure Cosmos DB](import-data.md)


