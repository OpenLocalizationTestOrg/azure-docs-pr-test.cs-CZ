---
title: "aaaGet začít s Azure table storage a Visual Studio připojené služby (ASP.NET) | Microsoft Docs"
description: "Způsob tooget spuštění pomocí úložiště tabulek Azure po připojení účtu úložiště tooa pomocí Visual Studio připojené služby v projektu ASP.NET v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: kraigb
ms.openlocfilehash: 04f79db7aad60ca51c3c866da1f4b01d9e11ac52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a>Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Přehled

Azure Table storage vám umožní toostore velkých objemů strukturovaná data. Hello služba je úložištěm dat typu NoSQL, která přijímá ověřených volání z vnitřní a vnější hello cloudu Azure. Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.

Tento kurz ukazuje, jak kód toowrite ASP.NET pro některé běžné scénáře s využitím Azure table storage entity. Mezi tyto scénáře patří vytvoření tabulky a přidání, dotazování a odstranění entity tabulky. 

##<a name="prerequisites"></a>Požadavky

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Účet služby Azure Storage](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Vytvořit řadič MVC 

1. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce hello, vyberte **Přidat -> řadiče**.

    ![Přidat řadič tooan aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. Na hello **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. Na hello **přidat kontroler** dialogové okno, název hello řadič *TablesController*a vyberte **přidat**.

    ![Název hello MVC jsou řadič MVC](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. Přidejte následující hello *pomocí* toohello direktivy `TablesController.cs` souboru:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a>Vytvořte třídu modelu

Řadu hello příklady v tomto článku používají **TableEntity**-odvozené třídy s názvem **CustomerEntity**. Hello následující kroky vás provedou deklarace tuto třídu jako třídu modelu:

1. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **modely**a v místní nabídce hello, vyberte **Přidat -> třída**.

1. Na hello **přidat novou položku** dialogové okno, název třídy hello, **CustomerEntity**.

1. Otevřete hello `CustomerEntity.cs` souboru a přidejte následující hello **pomocí** – direktiva:

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. Třída hello upravte tak, aby po dokončení hello třída je deklarován jako hello následující kód. Třída Hello deklaruje třídu entity, která je volána **CustomerEntity** že hello používá jméno zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello.

    ```csharp
    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }
    }
    ```

## <a name="create-a-table"></a>Vytvoření tabulky

Hello následující kroky popisují jak toocreate tabulku:

> [!NOTE]
> 
> V této části se předpokládá dokončení hello kroků v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment). 

1. Otevřete hello `TablesController.cs` souboru.

1. Přidejte metodu s názvem **CreateTable** , který vrací **ActionResult**.

    ```csharp
    public ActionResult CreateTable()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. V rámci hello **CreateTable** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Získání **CloudTableClient** objekt představuje klienta služby table.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Získání **CloudTable** objekt, který reprezentuje název požadované tabulky toohello odkaz. Hello **CloudTableClient.GetTableReference** metoda neprovede požadavek tabulka úložiště. odkaz na Hello se vrátí, zda existuje tabulka hello. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Volání hello **CloudTable.CreateIfNotExists** metoda toocreate hello tabulky, pokud ještě neexistuje. Hello **CloudTable.CreateIfNotExists** metoda vrátí **true** Pokud hello tabulka neexistuje a je úspěšně vytvořen. V opačném **false** je vrácen.    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. Aktualizace hello **ViewBag** s názvem hello hello tabulky.

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **CreateTable** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `CreateTable.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. Spuštění aplikace hello a vyberte **vytvořit tabulku** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Vytvoření tabulky](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    Jak je uvedeno nahoře, hello **CloudTable.CreateIfNotExists** metoda vrátí **true** pouze když neexistuje a k vytvoření tabulky hello. Proto pokud hello aplikaci spustíte, když hello tabulka existuje, hello metoda vrátí **false**. aplikace hello toorun vícekrát, je nutné odstranit tabulku hello před spuštěním aplikace hello znovu. Odstranění hello tabulky, můžete to udělat pomocí hello **CloudTable.Delete** metoda. Můžete také odstranit hello tabulky pomocí hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-an-entity-tooa-table"></a>Přidat tooa tabulka entity

*Entity* mapy tooC\# objekty pomocí vlastní třídy odvozené od **TableEntity**. tooadd tooa tabulka entity, vytvořte třídu, která definuje hello vlastnosti vaší entity. V této části se zobrazí, jak hello toodefine třídu entity, která používá jméno zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello. Společně oddílu a klíč řádku jednoznačně hello entity v tabulce hello. Na entity se stejným klíčem oddílu je možné se (v porovnání s těmi, které mají různé klíče oddílů) rychleji dotazovat, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operací. Jakákoli vlastnost, která by měly být uložené v hello služby table musí být vlastnost hello veřejná vlastnost podporovaného typu, který zveřejňuje nastavení i načítání hodnot.
Hello třídy entita *musí* deklarovat veřejný konstruktor bez parametrů.

> [!NOTE]
> 
> V této části se předpokládá dokončení hello kroků v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).

1. Otevřete hello `TablesController.cs` souboru.

1. Přidejte následující direktiva, která hello kód v hello hello `TablesController.cs` k souboru přístup hello **CustomerEntity** třídy:

    ```csharp
    using StorageAspnet.Models;
    ```

1. Přidejte metodu s názvem **AddEntity** , který vrací **ActionResult**.

    ```csharp
    public ActionResult AddEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. V rámci hello **AddEntity** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Získání **CloudTableClient** objekt představuje klienta služby table.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Získání **CloudTable** objekt, který reprezentuje toowhich odkaz toohello tabulky budete tooadd hello novou entitu. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Vytváření instancí a inicializace hello **CustomerEntity** třídy.

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. Vytvoření **TableOperation** objekt, který se vloží entitu zákazníka hello.

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. Spuštění operace insert hello pomocí volání hello **CloudTable.Execute** metoda. Můžete ověřit hello výsledek operace hello zkontrolováním hello **TableResult.HttpStatusCode** vlastnost. Stavový kód 2xx označuje, že požadovaná klientem hello akce hello byl úspěšně zpracován. Například úspěšné vložení nové entity výsledkem kód stavu HTTP 204, což znamená, že operace hello byl úspěšně zpracován a hello server nevrátil žádný obsah.

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. Aktualizace hello **ViewBag** s hello název tabulky a hello výsledky operace insert hello.

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **AddEntity** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `AddEntity.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. Spuštění aplikace hello a vyberte **Přidat entitu** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Přidání entity](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    Můžete ověřit, zda text hello entity přidány hello postupem v části hello, [získat jedné entity](#get-a-single-entity). Můžete taky hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview všechny hello entity pro vaše tabulky.

## <a name="add-a-batch-of-entities-tooa-table"></a>Přidat dávky entit tooa tabulky

V přidání toobeing možné příliš[přidat tabulku tooa entity, jeden v čase](#add-an-entity-to-a-table), můžete také přidat entity v dávce. Přidávání entit v dávce snižuje hello dobu odezvy mezi kódem a hello služby Azure table. Hello následující kroky popisují, jak tooadd více entit tooa tabulky s operace jeden insert:

> [!NOTE]
> 
> V této části se předpokládá dokončení hello kroků v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).

1. Otevřete hello `TablesController.cs` souboru.

1. Přidejte metodu s názvem **AddEntities** , který vrací **ActionResult**.

    ```csharp
    public ActionResult AddEntities()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. V rámci hello **AddEntities** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Získání **CloudTableClient** objekt představuje klienta služby table.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Získání **CloudTable** objekt, který reprezentuje toowhich odkaz toohello tabulky jsou probíhající tooadd hello nové entity. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Vytvoření instance některých objektů zákazníka podle hello **CustomerEntity** třída uvedené v části hello modelu [přidat tooa tabulka entity](#add-an-entity-to-a-table).

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. Získání **TableBatchOperation** objektu.

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. Přidejte objekt operace vložení dávky toohello entity.

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. Spusťte operaci vložení dávky hello tak, že volání hello **CloudTable.ExecuteBatch** metoda.   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. Hello **CloudTable.ExecuteBatch** metoda vrátí seznam hodnot **TableResult** objekty kde každý **TableResult** objekt může být zkontrolován toodetermine hello úspěch nebo neúspěch každé operace. V tomto příkladu předat zobrazení tooa seznamu hello a umožní zobrazení hello zobrazit hello výsledky jednotlivých operací. 
 
    ```csharp
    return View(results);
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **AddEntities** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `AddEntities.cshtml`a upravit ho tak, aby vypadal jako následující hello.

    ```csharp
    @model IEnumerable<Microsoft.WindowsAzure.Storage.Table.TableResult>
    @{
        ViewBag.Title = "AddEntities";
    }
    
    <h2>Add-entities results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        @foreach (var result in Model)
        {
        <tr>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@result.HttpStatusCode</td>
        </tr>
        }
    </table>
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. Spuštění aplikace hello a vyberte **přidat entity** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Přidání entit](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    Můžete ověřit, zda text hello entity přidány hello postupem v části hello, [získat jedné entity](#get-a-single-entity). Můžete taky hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview všechny hello entity pro vaše tabulky.

## <a name="get-a-single-entity"></a>Získat jedné entity

Tato část ukazuje, jak tooget jedné entity z tabulky pomocí hello klíč řádku entity a klíč oddílu. 

> [!NOTE]
> 
> Této části se předpokládá dokončení kroků hello v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment)a používá data z [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table). 

1. Otevřete hello `TablesController.cs` souboru.

1. Přidejte metodu s názvem **GetSingle** , který vrací **ActionResult**.

    ```csharp
    public ActionResult GetSingle()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. V rámci hello **GetSingle** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Získání **CloudTableClient** objekt představuje klienta služby table.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Získání **CloudTable** objekt, který reprezentuje odkaz na tabulku toohello ze kterého jsou načítání hello entity. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Vytvořit objekt operaci načtení, která přebírá objekt entity, který je odvozen od **TableEntity**. první parametr Hello je hello *partitionKey*, a druhý parametr hello je hello *rowKey*. Pomocí hello **CustomerEntity** třídy a data uvedená v části hello [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table), hello následující kód fragment kódu dotazy hello tabulku pro **CustomerEntity** entit *partitionKey* hodnotu "Smith" a *rowKey* hodnotu "Ben":

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. Provést operaci načtení hello.   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. Předejte hello výsledek toohello zobrazení pro zobrazení.

    ```csharp
    return View(result);
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **GetSingle** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `GetSingle.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "GetSingle";
    }
    
    <h2>Get Single results</h2>
    
    <table border="1">
        <tr>
            <th>HTTP result</th>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        <tr>
            <td>@Model.HttpStatusCode</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).Email)</td>
        </tr>
    </table>
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. Spuštění aplikace hello a vyberte **získat jeden** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Získat jeden](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a>Získání všech entit v oddílu

Jak je uvedeno v části hello [přidat tabulka entity tooa](#add-an-entity-to-a-table), kombinace hello oddílu a klíč řádku jednoznačně identifikují entitu v tabulce. Entity se stejným klíčem oddílu můžete položit dotaz na rychlejší než entity s různé klíče oddílů. Tato část ukazuje způsob tooquery tabulky pro všechny entity hello z zadaný oddíl.  

> [!NOTE]
> 
> Této části se předpokládá dokončení kroků hello v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment)a používá data z [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table). 

1. Otevřete hello `TablesController.cs` souboru.

1. Přidejte metodu s názvem **GetPartition** , který vrací **ActionResult**.

    ```csharp
    public ActionResult GetPartition()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. V rámci hello **GetPartition** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Získání **CloudTableClient** objekt představuje klienta služby table.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Získání **CloudTable** objekt, který reprezentuje odkaz na tabulku toohello ze kterého jsou načítání hello entity. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Vytváření instancí **TableQuery** objekt zadat dotaz hello v hello **kde** klauzule. Pomocí hello **CustomerEntity** třídy a data uvedená v části hello [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table), hello následující kód fragment kódu dotazy hello tabulky pro všechny entity, kde hello  **PartitionKey** (příjmení zákazníka) má hodnotu "Smith":

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. V rámci smyčku, volání hello **CloudTable.ExecuteQuerySegmented** metoda předávání můžete vytvořit instanci objektu dotazu hello v předchozím kroku hello.  Hello **CloudTable.ExecuteQuerySegmented** metoda vrátí **TableContinuationToken** objektu, který – když **null** – označuje, že neexistují žádné další entity tooretrieve. V rámci hello smyčky používají jiné tooiterate smyčky přes hello vrátí entity. V hello následující ukázka kódu se přidá každou vrácenou entitu tooa seznamu. Jednou hello cyklus skončí, hello seznam je předán zobrazení tooa pro zobrazení: 

    ```csharp
    List<CustomerEntity> customers = new List<CustomerEntity>();
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = table.ExecuteQuerySegmented(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity customer in resultSegment.Results)
        {
            customers.Add(customer);
        }
    } while (token != null);

    return View(customers);
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **GetPartition** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `GetPartition.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @model IEnumerable<StorageAspnet.Models.CustomerEntity>
    @{
        ViewBag.Title = "GetPartition";
    }
    
    <h2>Get Partition results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        @foreach (var customer in Model)
        {
        <tr>
            <td>@(customer.RowKey)</td>
            <td>@(customer.PartitionKey)</td>
            <td>@(customer.Email)</td>
        </tr>
        }
    </table>
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. Spuštění aplikace hello a vyberte **získat oddílu** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Získat oddílu](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a>Odstranění entity

Tato část ukazuje způsob toodelete entity z tabulky.

> [!NOTE]
> 
> Této části se předpokládá dokončení kroků hello v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment)a používá data z [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table). 

1. Otevřete hello `TablesController.cs` souboru.

1. Přidejte metodu s názvem **DeleteEntity** , který vrací **ActionResult**.

    ```csharp
    public ActionResult DeleteEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. V rámci hello **DeleteEntity** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Získání **CloudTableClient** objekt představuje klienta služby table.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Získání **CloudTable** objekt, který reprezentuje referenční tabulku toohello, ze kterého chcete odstranit hello entity. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Vytvořit objekt operaci odstranění, která přebírá objekt entity, který je odvozen od **TableEntity**. V tomto případě používáme hello **CustomerEntity** třídy a data uvedená v části hello [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table). Hello entity **značka ETag** musí být nastavena tooa platnou hodnotu.  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. Provést operace odstranění hello.   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. Předejte hello výsledek toohello zobrazení pro zobrazení.

    ```csharp
    return View(result);
    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.

1. Na hello **přidat zobrazení** dialogové okno, zadejte **DeleteEntity** pro hello název zobrazení, vyberte **přidat**.

1. Otevřete `DeleteEntity.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "DeleteEntity";
    }
    
    <h2>Delete Entity results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        <tr>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@Model.HttpStatusCode</td>
        </tr>
    </table>

    ```

1. V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. Spuštění aplikace hello a vyberte **odstranit entity** toosee výsledky podobné toohello následující snímek obrazovky:
  
    ![Získat jeden](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a>Další kroky
Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.

  * [Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)](../storage/vs-storage-aspnet-getting-started-queues.md)
