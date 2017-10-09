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
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="45e38-103">Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="45e38-103">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="45e38-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="45e38-104">Overview</span></span>

<span data-ttu-id="45e38-105">Azure Table storage vám umožní toostore velkých objemů strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="45e38-105">Azure Table storage enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="45e38-106">Hello služba je úložištěm dat typu NoSQL, která přijímá ověřených volání z vnitřní a vnější hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="45e38-106">hello service is a NoSQL datastore that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="45e38-107">Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.</span><span class="sxs-lookup"><span data-stu-id="45e38-107">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="45e38-108">Tento kurz ukazuje, jak kód toowrite ASP.NET pro některé běžné scénáře s využitím Azure table storage entity.</span><span class="sxs-lookup"><span data-stu-id="45e38-108">This tutorial shows how toowrite ASP.NET code for some common scenarios using Azure table storage entities.</span></span> <span data-ttu-id="45e38-109">Mezi tyto scénáře patří vytvoření tabulky a přidání, dotazování a odstranění entity tabulky.</span><span class="sxs-lookup"><span data-stu-id="45e38-109">These scenarios include creating a table, and adding, querying, and deleting table entities.</span></span> 

##<a name="prerequisites"></a><span data-ttu-id="45e38-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="45e38-110">Prerequisites</span></span>

* [<span data-ttu-id="45e38-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45e38-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="45e38-112">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="45e38-112">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="45e38-113">Vytvořit řadič MVC</span><span class="sxs-lookup"><span data-stu-id="45e38-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="45e38-114">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce hello, vyberte **Přidat -> řadiče**.</span><span class="sxs-lookup"><span data-stu-id="45e38-114">In hello **Solution Explorer**, right-click **Controllers**, and, from hello context menu, select **Add->Controller**.</span></span>

    ![Přidat řadič tooan aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. <span data-ttu-id="45e38-116">Na hello **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="45e38-116">On hello **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. <span data-ttu-id="45e38-118">Na hello **přidat kontroler** dialogové okno, název hello řadič *TablesController*a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="45e38-118">On hello **Add Controller** dialog, name hello controller *TablesController*, and select **Add**.</span></span>

    ![Název hello MVC jsou řadič MVC](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. <span data-ttu-id="45e38-120">Přidejte následující hello *pomocí* toohello direktivy `TablesController.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="45e38-120">Add hello following *using* directives toohello `TablesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a><span data-ttu-id="45e38-121">Vytvořte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="45e38-121">Create a model class</span></span>

<span data-ttu-id="45e38-122">Řadu hello příklady v tomto článku používají **TableEntity**-odvozené třídy s názvem **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="45e38-122">Many of hello examples in this article use a **TableEntity**-derived class called **CustomerEntity**.</span></span> <span data-ttu-id="45e38-123">Hello následující kroky vás provedou deklarace tuto třídu jako třídu modelu:</span><span class="sxs-lookup"><span data-stu-id="45e38-123">hello following steps guide you through declaring this class as a model class:</span></span>

1. <span data-ttu-id="45e38-124">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **modely**a v místní nabídce hello, vyberte **Přidat -> třída**.</span><span class="sxs-lookup"><span data-stu-id="45e38-124">In hello **Solution Explorer**, right-click **Models**, and, from hello context menu, select **Add->Class**.</span></span>

1. <span data-ttu-id="45e38-125">Na hello **přidat novou položku** dialogové okno, název třídy hello, **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="45e38-125">On hello **Add New Item** dialog, name hello class, **CustomerEntity**.</span></span>

1. <span data-ttu-id="45e38-126">Otevřete hello `CustomerEntity.cs` souboru a přidejte následující hello **pomocí** – direktiva:</span><span class="sxs-lookup"><span data-stu-id="45e38-126">Open hello `CustomerEntity.cs` file, and add hello following **using** directive:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. <span data-ttu-id="45e38-127">Třída hello upravte tak, aby po dokončení hello třída je deklarován jako hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="45e38-127">Modify hello class so that, when finished, hello class is declared as in hello following code.</span></span> <span data-ttu-id="45e38-128">Třída Hello deklaruje třídu entity, která je volána **CustomerEntity** že hello používá jméno zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-128">hello class declares an entity class called **CustomerEntity** that uses hello customer's first name as hello row key and last name as hello partition key.</span></span>

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

## <a name="create-a-table"></a><span data-ttu-id="45e38-129">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="45e38-129">Create a table</span></span>

<span data-ttu-id="45e38-130">Hello následující kroky popisují jak toocreate tabulku:</span><span class="sxs-lookup"><span data-stu-id="45e38-130">hello following steps illustrate how toocreate a table:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="45e38-131">V této části se předpokládá dokončení hello kroků v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="45e38-131">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="45e38-132">Otevřete hello `TablesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="45e38-132">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="45e38-133">Přidejte metodu s názvem **CreateTable** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="45e38-133">Add a method called **CreateTable** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateTable()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="45e38-134">V rámci hello **CreateTable** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="45e38-134">Within hello **CreateTable** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="45e38-135">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="45e38-135">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="45e38-136">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="45e38-136">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="45e38-137">Získání **CloudTable** objekt, který reprezentuje název požadované tabulky toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="45e38-137">Get a **CloudTable** object that represents a reference toohello desired table name.</span></span> <span data-ttu-id="45e38-138">Hello **CloudTableClient.GetTableReference** metoda neprovede požadavek tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="45e38-138">hello **CloudTableClient.GetTableReference** method does not make a request against table storage.</span></span> <span data-ttu-id="45e38-139">odkaz na Hello se vrátí, zda existuje tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-139">hello reference is returned whether or not hello table exists.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="45e38-140">Volání hello **CloudTable.CreateIfNotExists** metoda toocreate hello tabulky, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="45e38-140">Call hello **CloudTable.CreateIfNotExists** method toocreate hello table if it does not yet exist.</span></span> <span data-ttu-id="45e38-141">Hello **CloudTable.CreateIfNotExists** metoda vrátí **true** Pokud hello tabulka neexistuje a je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="45e38-141">hello **CloudTable.CreateIfNotExists** method returns **true** if hello table does not exist, and is successfully created.</span></span> <span data-ttu-id="45e38-142">V opačném **false** je vrácen.</span><span class="sxs-lookup"><span data-stu-id="45e38-142">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. <span data-ttu-id="45e38-143">Aktualizace hello **ViewBag** s názvem hello hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="45e38-143">Update hello **ViewBag** with hello name of hello table.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. <span data-ttu-id="45e38-144">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="45e38-144">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="45e38-145">Na hello **přidat zobrazení** dialogové okno, zadejte **CreateTable** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="45e38-145">On hello **Add View** dialog, enter **CreateTable** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="45e38-146">Otevřete `CreateTable.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="45e38-146">Open `CreateTable.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="45e38-147">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="45e38-147">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="45e38-148">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="45e38-148">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. <span data-ttu-id="45e38-149">Spuštění aplikace hello a vyberte **vytvořit tabulku** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="45e38-149">Run hello application, and select **Create table** toosee results similar toohello following screen shot:</span></span>
  
    ![Vytvoření tabulky](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    <span data-ttu-id="45e38-151">Jak je uvedeno nahoře, hello **CloudTable.CreateIfNotExists** metoda vrátí **true** pouze když neexistuje a k vytvoření tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-151">As mentioned previously, hello **CloudTable.CreateIfNotExists** method returns **true** only when hello table doesn't exist and is created.</span></span> <span data-ttu-id="45e38-152">Proto pokud hello aplikaci spustíte, když hello tabulka existuje, hello metoda vrátí **false**.</span><span class="sxs-lookup"><span data-stu-id="45e38-152">Therefore, if you run hello app when hello table exists, hello method returns **false**.</span></span> <span data-ttu-id="45e38-153">aplikace hello toorun vícekrát, je nutné odstranit tabulku hello před spuštěním aplikace hello znovu.</span><span class="sxs-lookup"><span data-stu-id="45e38-153">toorun hello app multiple times, you must delete hello table before running hello app again.</span></span> <span data-ttu-id="45e38-154">Odstranění hello tabulky, můžete to udělat pomocí hello **CloudTable.Delete** metoda.</span><span class="sxs-lookup"><span data-stu-id="45e38-154">Deleting hello table can be done via hello **CloudTable.Delete** method.</span></span> <span data-ttu-id="45e38-155">Můžete také odstranit hello tabulky pomocí hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="45e38-155">You can also delete hello table using hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="45e38-156">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="45e38-156">Add an entity tooa table</span></span>

<span data-ttu-id="45e38-157">*Entity* mapy tooC\# objekty pomocí vlastní třídy odvozené od **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="45e38-157">*Entities* map tooC\# objects by using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="45e38-158">tooadd tooa tabulka entity, vytvořte třídu, která definuje hello vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="45e38-158">tooadd an entity tooa table, create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="45e38-159">V této části se zobrazí, jak hello toodefine třídu entity, která používá jméno zákazníka jako klíč řádku hello a příjmení jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-159">In this section, you'll see how toodefine an entity class that uses hello customer's first name as hello row key and last name as hello partition key.</span></span> <span data-ttu-id="45e38-160">Společně oddílu a klíč řádku jednoznačně hello entity v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-160">Together, an entity's partition and row key uniquely identify hello entity in hello table.</span></span> <span data-ttu-id="45e38-161">Na entity se stejným klíčem oddílu je možné se (v porovnání s těmi, které mají různé klíče oddílů) rychleji dotazovat, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operací.</span><span class="sxs-lookup"><span data-stu-id="45e38-161">Entities with the same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="45e38-162">Jakákoli vlastnost, která by měly být uložené v hello služby table musí být vlastnost hello veřejná vlastnost podporovaného typu, který zveřejňuje nastavení i načítání hodnot.</span><span class="sxs-lookup"><span data-stu-id="45e38-162">For any property that should be stored in hello table service, hello property must be a public property of a supported type that exposes both setting and retrieving values.</span></span>
<span data-ttu-id="45e38-163">Hello třídy entita *musí* deklarovat veřejný konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="45e38-163">hello entity class *must* declare a public parameter-less constructor.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="45e38-164">V této části se předpokládá dokončení hello kroků v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="45e38-164">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="45e38-165">Otevřete hello `TablesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="45e38-165">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="45e38-166">Přidejte následující direktiva, která hello kód v hello hello `TablesController.cs` k souboru přístup hello **CustomerEntity** třídy:</span><span class="sxs-lookup"><span data-stu-id="45e38-166">Add hello following directive so that hello code in hello `TablesController.cs` file can access hello **CustomerEntity** class:</span></span>

    ```csharp
    using StorageAspnet.Models;
    ```

1. <span data-ttu-id="45e38-167">Přidejte metodu s názvem **AddEntity** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="45e38-167">Add a method called **AddEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="45e38-168">V rámci hello **AddEntity** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="45e38-168">Within hello **AddEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="45e38-169">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="45e38-169">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="45e38-170">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="45e38-170">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="45e38-171">Získání **CloudTable** objekt, který reprezentuje toowhich odkaz toohello tabulky budete tooadd hello novou entitu.</span><span class="sxs-lookup"><span data-stu-id="45e38-171">Get a **CloudTable** object that represents a reference toohello table toowhich you are going tooadd hello new entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="45e38-172">Vytváření instancí a inicializace hello **CustomerEntity** třídy.</span><span class="sxs-lookup"><span data-stu-id="45e38-172">Instantiate and initialize hello **CustomerEntity** class.</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. <span data-ttu-id="45e38-173">Vytvoření **TableOperation** objekt, který se vloží entitu zákazníka hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-173">Create a **TableOperation** object that inserts hello customer entity.</span></span>

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. <span data-ttu-id="45e38-174">Spuštění operace insert hello pomocí volání hello **CloudTable.Execute** metoda.</span><span class="sxs-lookup"><span data-stu-id="45e38-174">Execute hello insert operation by calling hello **CloudTable.Execute** method.</span></span> <span data-ttu-id="45e38-175">Můžete ověřit hello výsledek operace hello zkontrolováním hello **TableResult.HttpStatusCode** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="45e38-175">You can verify hello result of hello operation by inspecting hello **TableResult.HttpStatusCode** property.</span></span> <span data-ttu-id="45e38-176">Stavový kód 2xx označuje, že požadovaná klientem hello akce hello byl úspěšně zpracován.</span><span class="sxs-lookup"><span data-stu-id="45e38-176">A status code of 2xx indicates hello action requested by hello client was processed successfully.</span></span> <span data-ttu-id="45e38-177">Například úspěšné vložení nové entity výsledkem kód stavu HTTP 204, což znamená, že operace hello byl úspěšně zpracován a hello server nevrátil žádný obsah.</span><span class="sxs-lookup"><span data-stu-id="45e38-177">For example, successful insertions of new entities results in an HTTP status code of 204, meaning that hello operation was successfully processed and hello server did not return any content.</span></span>

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. <span data-ttu-id="45e38-178">Aktualizace hello **ViewBag** s hello název tabulky a hello výsledky operace insert hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-178">Update hello **ViewBag** with hello table name, and hello results of hello insert operation.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. <span data-ttu-id="45e38-179">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="45e38-179">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="45e38-180">Na hello **přidat zobrazení** dialogové okno, zadejte **AddEntity** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="45e38-180">On hello **Add View** dialog, enter **AddEntity** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="45e38-181">Otevřete `AddEntity.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="45e38-181">Open `AddEntity.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. <span data-ttu-id="45e38-182">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="45e38-182">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="45e38-183">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="45e38-183">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. <span data-ttu-id="45e38-184">Spuštění aplikace hello a vyberte **Přidat entitu** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="45e38-184">Run hello application, and select **Add entity** toosee results similar toohello following screen shot:</span></span>
  
    ![Přidání entity](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    <span data-ttu-id="45e38-186">Můžete ověřit, zda text hello entity přidány hello postupem v části hello, [získat jedné entity](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="45e38-186">You can verify that hello entity was added by following hello steps in hello section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="45e38-187">Můžete taky hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview všechny hello entity pro vaše tabulky.</span><span class="sxs-lookup"><span data-stu-id="45e38-187">You can also use hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview all hello entities for your tables.</span></span>

## <a name="add-a-batch-of-entities-tooa-table"></a><span data-ttu-id="45e38-188">Přidat dávky entit tooa tabulky</span><span class="sxs-lookup"><span data-stu-id="45e38-188">Add a batch of entities tooa table</span></span>

<span data-ttu-id="45e38-189">V přidání toobeing možné příliš[přidat tabulku tooa entity, jeden v čase](#add-an-entity-to-a-table), můžete také přidat entity v dávce.</span><span class="sxs-lookup"><span data-stu-id="45e38-189">In addition toobeing able too[add an entity tooa table one at a time](#add-an-entity-to-a-table), you can also add entities in batch.</span></span> <span data-ttu-id="45e38-190">Přidávání entit v dávce snižuje hello dobu odezvy mezi kódem a hello služby Azure table.</span><span class="sxs-lookup"><span data-stu-id="45e38-190">Adding entities in batch reduces hello number of round-trips between your code and hello Azure table service.</span></span> <span data-ttu-id="45e38-191">Hello následující kroky popisují, jak tooadd více entit tooa tabulky s operace jeden insert:</span><span class="sxs-lookup"><span data-stu-id="45e38-191">hello following steps illustrate how tooadd multiple entities tooa table with a single insert operation:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="45e38-192">V této části se předpokládá dokončení hello kroků v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="45e38-192">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="45e38-193">Otevřete hello `TablesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="45e38-193">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="45e38-194">Přidejte metodu s názvem **AddEntities** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="45e38-194">Add a method called **AddEntities** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntities()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="45e38-195">V rámci hello **AddEntities** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="45e38-195">Within hello **AddEntities** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="45e38-196">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="45e38-196">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="45e38-197">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="45e38-197">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="45e38-198">Získání **CloudTable** objekt, který reprezentuje toowhich odkaz toohello tabulky jsou probíhající tooadd hello nové entity.</span><span class="sxs-lookup"><span data-stu-id="45e38-198">Get a **CloudTable** object that represents a reference toohello table toowhich you are going tooadd hello new entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="45e38-199">Vytvoření instance některých objektů zákazníka podle hello **CustomerEntity** třída uvedené v části hello modelu [přidat tooa tabulka entity](#add-an-entity-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="45e38-199">Instantiate some customer objects based on hello **CustomerEntity** model class presented in hello section, [Add an entity tooa table](#add-an-entity-to-a-table).</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. <span data-ttu-id="45e38-200">Získání **TableBatchOperation** objektu.</span><span class="sxs-lookup"><span data-stu-id="45e38-200">Get a **TableBatchOperation** object.</span></span>

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. <span data-ttu-id="45e38-201">Přidejte objekt operace vložení dávky toohello entity.</span><span class="sxs-lookup"><span data-stu-id="45e38-201">Add entities toohello batch insert operation object.</span></span>

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. <span data-ttu-id="45e38-202">Spusťte operaci vložení dávky hello tak, že volání hello **CloudTable.ExecuteBatch** metoda.</span><span class="sxs-lookup"><span data-stu-id="45e38-202">Execute hello batch insert operation by calling hello **CloudTable.ExecuteBatch** method.</span></span>   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. <span data-ttu-id="45e38-203">Hello **CloudTable.ExecuteBatch** metoda vrátí seznam hodnot **TableResult** objekty kde každý **TableResult** objekt může být zkontrolován toodetermine hello úspěch nebo neúspěch každé operace.</span><span class="sxs-lookup"><span data-stu-id="45e38-203">hello **CloudTable.ExecuteBatch** method returns a list of **TableResult** objects where each **TableResult** object can be examined toodetermine hello success or failure of each individual operation.</span></span> <span data-ttu-id="45e38-204">V tomto příkladu předat zobrazení tooa seznamu hello a umožní zobrazení hello zobrazit hello výsledky jednotlivých operací.</span><span class="sxs-lookup"><span data-stu-id="45e38-204">For this example, pass hello list tooa view and let hello view display hello results of each operation.</span></span> 
 
    ```csharp
    return View(results);
    ```

1. <span data-ttu-id="45e38-205">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="45e38-205">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="45e38-206">Na hello **přidat zobrazení** dialogové okno, zadejte **AddEntities** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="45e38-206">On hello **Add View** dialog, enter **AddEntities** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="45e38-207">Otevřete `AddEntities.cshtml`a upravit ho tak, aby vypadal jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-207">Open `AddEntities.cshtml`, and modify it so that it looks like hello following.</span></span>

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

1. <span data-ttu-id="45e38-208">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="45e38-208">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="45e38-209">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="45e38-209">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. <span data-ttu-id="45e38-210">Spuštění aplikace hello a vyberte **přidat entity** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="45e38-210">Run hello application, and select **Add entities** toosee results similar toohello following screen shot:</span></span>
  
    ![Přidání entit](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    <span data-ttu-id="45e38-212">Můžete ověřit, zda text hello entity přidány hello postupem v části hello, [získat jedné entity](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="45e38-212">You can verify that hello entity was added by following hello steps in hello section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="45e38-213">Můžete taky hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview všechny hello entity pro vaše tabulky.</span><span class="sxs-lookup"><span data-stu-id="45e38-213">You can also use hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview all hello entities for your tables.</span></span>

## <a name="get-a-single-entity"></a><span data-ttu-id="45e38-214">Získat jedné entity</span><span class="sxs-lookup"><span data-stu-id="45e38-214">Get a single entity</span></span>

<span data-ttu-id="45e38-215">Tato část ukazuje, jak tooget jedné entity z tabulky pomocí hello klíč řádku entity a klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="45e38-215">This section illustrates how tooget a single entity from a table using hello entity's row key and partition key.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="45e38-216">Této části se předpokládá dokončení kroků hello v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment)a používá data z [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="45e38-216">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="45e38-217">Otevřete hello `TablesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="45e38-217">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="45e38-218">Přidejte metodu s názvem **GetSingle** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="45e38-218">Add a method called **GetSingle** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetSingle()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="45e38-219">V rámci hello **GetSingle** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="45e38-219">Within hello **GetSingle** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="45e38-220">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="45e38-220">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="45e38-221">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="45e38-221">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="45e38-222">Získání **CloudTable** objekt, který reprezentuje odkaz na tabulku toohello ze kterého jsou načítání hello entity.</span><span class="sxs-lookup"><span data-stu-id="45e38-222">Get a **CloudTable** object that represents a reference toohello table from which you are retrieving hello entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="45e38-223">Vytvořit objekt operaci načtení, která přebírá objekt entity, který je odvozen od **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="45e38-223">Create a retrieve operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="45e38-224">první parametr Hello je hello *partitionKey*, a druhý parametr hello je hello *rowKey*.</span><span class="sxs-lookup"><span data-stu-id="45e38-224">hello first parameter is hello *partitionKey*, and hello second parameter is hello *rowKey*.</span></span> <span data-ttu-id="45e38-225">Pomocí hello **CustomerEntity** třídy a data uvedená v části hello [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table), hello následující kód fragment kódu dotazy hello tabulku pro **CustomerEntity** entit *partitionKey* hodnotu "Smith" a *rowKey* hodnotu "Ben":</span><span class="sxs-lookup"><span data-stu-id="45e38-225">Using hello **CustomerEntity** class and data presented in hello section [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table), hello following code snippet queries hello table for a **CustomerEntity** entity with a *partitionKey* value of "Smith" and a *rowKey* value of "Ben":</span></span>

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. <span data-ttu-id="45e38-226">Provést operaci načtení hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-226">Execute hello retrieve operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. <span data-ttu-id="45e38-227">Předejte hello výsledek toohello zobrazení pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="45e38-227">Pass hello result toohello view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="45e38-228">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="45e38-228">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="45e38-229">Na hello **přidat zobrazení** dialogové okno, zadejte **GetSingle** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="45e38-229">On hello **Add View** dialog, enter **GetSingle** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="45e38-230">Otevřete `GetSingle.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="45e38-230">Open `GetSingle.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="45e38-231">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="45e38-231">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="45e38-232">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="45e38-232">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. <span data-ttu-id="45e38-233">Spuštění aplikace hello a vyberte **získat jeden** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="45e38-233">Run hello application, and select **Get Single** toosee results similar toohello following screen shot:</span></span>
  
    ![Získat jeden](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a><span data-ttu-id="45e38-235">Získání všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="45e38-235">Get all entities in a partition</span></span>

<span data-ttu-id="45e38-236">Jak je uvedeno v části hello [přidat tabulka entity tooa](#add-an-entity-to-a-table), kombinace hello oddílu a klíč řádku jednoznačně identifikují entitu v tabulce.</span><span class="sxs-lookup"><span data-stu-id="45e38-236">As mentioned in hello section, [Add an entity tooa table](#add-an-entity-to-a-table), hello combination of a partition and a row key uniquely identify an entity in a table.</span></span> <span data-ttu-id="45e38-237">Entity se stejným klíčem oddílu můžete položit dotaz na rychlejší než entity s různé klíče oddílů.</span><span class="sxs-lookup"><span data-stu-id="45e38-237">Entities with the same partition key can be queried faster than entities with different partition keys.</span></span> <span data-ttu-id="45e38-238">Tato část ukazuje způsob tooquery tabulky pro všechny entity hello z zadaný oddíl.</span><span class="sxs-lookup"><span data-stu-id="45e38-238">This section illustrates how tooquery a table for all hello entities from a specified partition.</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="45e38-239">Této části se předpokládá dokončení kroků hello v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment)a používá data z [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="45e38-239">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="45e38-240">Otevřete hello `TablesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="45e38-240">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="45e38-241">Přidejte metodu s názvem **GetPartition** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="45e38-241">Add a method called **GetPartition** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetPartition()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="45e38-242">V rámci hello **GetPartition** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="45e38-242">Within hello **GetPartition** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="45e38-243">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="45e38-243">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="45e38-244">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="45e38-244">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="45e38-245">Získání **CloudTable** objekt, který reprezentuje odkaz na tabulku toohello ze kterého jsou načítání hello entity.</span><span class="sxs-lookup"><span data-stu-id="45e38-245">Get a **CloudTable** object that represents a reference toohello table from which you are retrieving hello entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="45e38-246">Vytváření instancí **TableQuery** objekt zadat dotaz hello v hello **kde** klauzule.</span><span class="sxs-lookup"><span data-stu-id="45e38-246">Instantiate a **TableQuery** object specifying hello query in hello **Where** clause.</span></span> <span data-ttu-id="45e38-247">Pomocí hello **CustomerEntity** třídy a data uvedená v části hello [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table), hello následující kód fragment kódu dotazy hello tabulky pro všechny entity, kde hello  **PartitionKey** (příjmení zákazníka) má hodnotu "Smith":</span><span class="sxs-lookup"><span data-stu-id="45e38-247">Using hello **CustomerEntity** class and data presented in hello section [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table), hello following code snippet queries hello table for a all entities where hello **PartitionKey** (customer's last name) has a value of "Smith":</span></span>

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. <span data-ttu-id="45e38-248">V rámci smyčku, volání hello **CloudTable.ExecuteQuerySegmented** metoda předávání můžete vytvořit instanci objektu dotazu hello v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-248">Within a loop, call hello **CloudTable.ExecuteQuerySegmented** method passing hello query object you instantiated in hello previous step.</span></span>  <span data-ttu-id="45e38-249">Hello **CloudTable.ExecuteQuerySegmented** metoda vrátí **TableContinuationToken** objektu, který – když **null** – označuje, že neexistují žádné další entity tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="45e38-249">hello **CloudTable.ExecuteQuerySegmented** method returns a **TableContinuationToken** object that - when **null** - indicates that there are no more entities tooretrieve.</span></span> <span data-ttu-id="45e38-250">V rámci hello smyčky používají jiné tooiterate smyčky přes hello vrátí entity.</span><span class="sxs-lookup"><span data-stu-id="45e38-250">Within hello loop, use another loop tooiterate over hello returned entities.</span></span> <span data-ttu-id="45e38-251">V hello následující ukázka kódu se přidá každou vrácenou entitu tooa seznamu.</span><span class="sxs-lookup"><span data-stu-id="45e38-251">In hello following code example, each returned entity is added tooa list.</span></span> <span data-ttu-id="45e38-252">Jednou hello cyklus skončí, hello seznam je předán zobrazení tooa pro zobrazení:</span><span class="sxs-lookup"><span data-stu-id="45e38-252">Once hello loop ends, hello list is passed tooa view for display:</span></span> 

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

1. <span data-ttu-id="45e38-253">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="45e38-253">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="45e38-254">Na hello **přidat zobrazení** dialogové okno, zadejte **GetPartition** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="45e38-254">On hello **Add View** dialog, enter **GetPartition** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="45e38-255">Otevřete `GetPartition.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="45e38-255">Open `GetPartition.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="45e38-256">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="45e38-256">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="45e38-257">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="45e38-257">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. <span data-ttu-id="45e38-258">Spuštění aplikace hello a vyberte **získat oddílu** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="45e38-258">Run hello application, and select **Get Partition** toosee results similar toohello following screen shot:</span></span>
  
    ![Získat oddílu](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a><span data-ttu-id="45e38-260">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="45e38-260">Delete an entity</span></span>

<span data-ttu-id="45e38-261">Tato část ukazuje způsob toodelete entity z tabulky.</span><span class="sxs-lookup"><span data-stu-id="45e38-261">This section illustrates how toodelete an entity from a table.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="45e38-262">Této části se předpokládá dokončení kroků hello v [nastavení prostředí pro vývoj hello](#set-up-the-development-environment)a používá data z [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="45e38-262">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="45e38-263">Otevřete hello `TablesController.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="45e38-263">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="45e38-264">Přidejte metodu s názvem **DeleteEntity** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="45e38-264">Add a method called **DeleteEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="45e38-265">V rámci hello **DeleteEntity** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="45e38-265">Within hello **DeleteEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="45e38-266">Použití hello následující kód tooget hello připojovací řetězec a úložiště informace o účtu úložiště z konfigurace služby Azure hello: (Změna  *&lt;název účtu úložiště >* toohello název hello úložiště Azure účet, ke které přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="45e38-266">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="45e38-267">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="45e38-267">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="45e38-268">Získání **CloudTable** objekt, který reprezentuje referenční tabulku toohello, ze kterého chcete odstranit hello entity.</span><span class="sxs-lookup"><span data-stu-id="45e38-268">Get a **CloudTable** object that represents a reference toohello table from which you are deleting hello entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="45e38-269">Vytvořit objekt operaci odstranění, která přebírá objekt entity, který je odvozen od **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="45e38-269">Create a delete operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="45e38-270">V tomto případě používáme hello **CustomerEntity** třídy a data uvedená v části hello [přidat dávky entit tooa tabulky](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="45e38-270">In this case, we use hello **CustomerEntity** class and data presented in hello section [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> <span data-ttu-id="45e38-271">Hello entity **značka ETag** musí být nastavena tooa platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="45e38-271">hello entity's **ETag** must be set tooa valid value.</span></span>  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. <span data-ttu-id="45e38-272">Provést operace odstranění hello.</span><span class="sxs-lookup"><span data-stu-id="45e38-272">Execute hello delete operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. <span data-ttu-id="45e38-273">Předejte hello výsledek toohello zobrazení pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="45e38-273">Pass hello result toohello view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="45e38-274">V hello **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a z hello kontextové nabídky, vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="45e38-274">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="45e38-275">Na hello **přidat zobrazení** dialogové okno, zadejte **DeleteEntity** pro hello název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="45e38-275">On hello **Add View** dialog, enter **DeleteEntity** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="45e38-276">Otevřete `DeleteEntity.cshtml`a upravit ho tak, aby vypadal jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="45e38-276">Open `DeleteEntity.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="45e38-277">V hello **Průzkumníku řešení**, rozbalte položku hello **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="45e38-277">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="45e38-278">Po hello poslední **Html.ActionLink**, přidejte následující hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="45e38-278">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. <span data-ttu-id="45e38-279">Spuštění aplikace hello a vyberte **odstranit entity** toosee výsledky podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="45e38-279">Run hello application, and select **Delete entity** toosee results similar toohello following screen shot:</span></span>
  
    ![Získat jeden](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a><span data-ttu-id="45e38-281">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45e38-281">Next steps</span></span>
<span data-ttu-id="45e38-282">Zobrazte další funkce příručky toolearn o dalších možnostech pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="45e38-282">View more feature guides toolearn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="45e38-283">Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="45e38-283">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="45e38-284">Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="45e38-284">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](../storage/vs-storage-aspnet-getting-started-queues.md)
