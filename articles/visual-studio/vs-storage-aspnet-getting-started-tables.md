---
title: "Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET) | Microsoft Docs"
description: "Jak začít používat úložiště tabulek Azure po připojení k účtu úložiště pomocí Visual Studio připojené služby v projektu ASP.NET v sadě Visual Studio"
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
ms.openlocfilehash: 32a57e77bf6fe3cff88b9d6772ede9e6669ec75f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="21cb4-103">Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="21cb4-103">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="21cb4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="21cb4-104">Overview</span></span>

<span data-ttu-id="21cb4-105">Azure Table storage umožňuje ukládat velké množství strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="21cb4-105">Azure Table storage enables you to store large amounts of structured data.</span></span> <span data-ttu-id="21cb4-106">Služba je úložištěm dat typu NoSQL, která přijímá ověřených volání z uvnitř i vně cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="21cb4-106">The service is a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="21cb4-107">Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat.</span><span class="sxs-lookup"><span data-stu-id="21cb4-107">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="21cb4-108">Tento kurz ukazuje, jak napsat kód ASP.NET pro některé běžné scénáře s využitím Azure table storage entity.</span><span class="sxs-lookup"><span data-stu-id="21cb4-108">This tutorial shows how to write ASP.NET code for some common scenarios using Azure table storage entities.</span></span> <span data-ttu-id="21cb4-109">Mezi tyto scénáře patří vytvoření tabulky a přidání, dotazování a odstranění entity tabulky.</span><span class="sxs-lookup"><span data-stu-id="21cb4-109">These scenarios include creating a table, and adding, querying, and deleting table entities.</span></span> 

##<a name="prerequisites"></a><span data-ttu-id="21cb4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="21cb4-110">Prerequisites</span></span>

* [<span data-ttu-id="21cb4-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21cb4-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="21cb4-112">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="21cb4-112">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="21cb4-113">Vytvořit řadič MVC</span><span class="sxs-lookup"><span data-stu-id="21cb4-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="21cb4-114">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce vyberte **Přidat -> řadiče**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-114">In the **Solution Explorer**, right-click **Controllers**, and, from the context menu, select **Add->Controller**.</span></span>

    ![Přidat řadič do aplikace ASP.NET MVC](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. <span data-ttu-id="21cb4-116">Na **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC 5 – prázdný**a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-116">On the **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. <span data-ttu-id="21cb4-118">Na **přidat kontroler** dialogové okno, názvu kontroleru *TablesController*a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-118">On the **Add Controller** dialog, name the controller *TablesController*, and select **Add**.</span></span>

    ![Název řadiče MVC](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. <span data-ttu-id="21cb4-120">Přidejte následující *pomocí* direktivy pro `TablesController.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="21cb4-120">Add the following *using* directives to the `TablesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a><span data-ttu-id="21cb4-121">Vytvořte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="21cb4-121">Create a model class</span></span>

<span data-ttu-id="21cb4-122">Řadu příklady v tomto článku používají **TableEntity**-odvozené třídy s názvem **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-122">Many of the examples in this article use a **TableEntity**-derived class called **CustomerEntity**.</span></span> <span data-ttu-id="21cb4-123">Následující postup vás provede deklarace tuto třídu jako třídu modelu:</span><span class="sxs-lookup"><span data-stu-id="21cb4-123">The following steps guide you through declaring this class as a model class:</span></span>

1. <span data-ttu-id="21cb4-124">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **modely**a v místní nabídce vyberte **Přidat -> třída**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-124">In the **Solution Explorer**, right-click **Models**, and, from the context menu, select **Add->Class**.</span></span>

1. <span data-ttu-id="21cb4-125">Na **přidat novou položku** dialogové okno, název třídy, **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-125">On the **Add New Item** dialog, name the class, **CustomerEntity**.</span></span>

1. <span data-ttu-id="21cb4-126">Otevřete `CustomerEntity.cs` souboru a přidejte následující **pomocí** – direktiva:</span><span class="sxs-lookup"><span data-stu-id="21cb4-126">Open the `CustomerEntity.cs` file, and add the following **using** directive:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. <span data-ttu-id="21cb4-127">Třída upravte tak, aby po dokončení třída je deklarován jako v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-127">Modify the class so that, when finished, the class is declared as in the following code.</span></span> <span data-ttu-id="21cb4-128">Třída deklaruje třídu entity, která je volána **CustomerEntity** která používá jméno zákazníka jako klíč řádku a jeho příjmení jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-128">The class declares an entity class called **CustomerEntity** that uses the customer's first name as the row key and last name as the partition key.</span></span>

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

## <a name="create-a-table"></a><span data-ttu-id="21cb4-129">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="21cb4-129">Create a table</span></span>

<span data-ttu-id="21cb4-130">Následující kroky ukazují, jak vytvořit tabulku:</span><span class="sxs-lookup"><span data-stu-id="21cb4-130">The following steps illustrate how to create a table:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="21cb4-131">V této části se předpokládá dokončení kroků v [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="21cb4-131">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="21cb4-132">Otevřete soubor `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-132">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="21cb4-133">Přidejte metodu s názvem **CreateTable** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-133">Add a method called **CreateTable** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateTable()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="21cb4-134">V rámci **CreateTable** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="21cb4-134">Within the **CreateTable** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="21cb4-135">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="21cb4-135">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="21cb4-136">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="21cb4-136">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="21cb4-137">Získání **CloudTable** objekt, který reprezentuje odkaz na název požadované tabulky.</span><span class="sxs-lookup"><span data-stu-id="21cb4-137">Get a **CloudTable** object that represents a reference to the desired table name.</span></span> <span data-ttu-id="21cb4-138">**CloudTableClient.GetTableReference** metoda neprovede požadavek tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="21cb4-138">The **CloudTableClient.GetTableReference** method does not make a request against table storage.</span></span> <span data-ttu-id="21cb4-139">Odkaz se vrátí, zda existuje v tabulce.</span><span class="sxs-lookup"><span data-stu-id="21cb4-139">The reference is returned whether or not the table exists.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="21cb4-140">Volání **CloudTable.CreateIfNotExists** metodu pro vytvoření tabulky, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="21cb4-140">Call the **CloudTable.CreateIfNotExists** method to create the table if it does not yet exist.</span></span> <span data-ttu-id="21cb4-141">**CloudTable.CreateIfNotExists** metoda vrátí **true** Pokud tabulka neexistuje a je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="21cb4-141">The **CloudTable.CreateIfNotExists** method returns **true** if the table does not exist, and is successfully created.</span></span> <span data-ttu-id="21cb4-142">V opačném **false** je vrácen.</span><span class="sxs-lookup"><span data-stu-id="21cb4-142">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. <span data-ttu-id="21cb4-143">Aktualizace **ViewBag** s názvem tabulky.</span><span class="sxs-lookup"><span data-stu-id="21cb4-143">Update the **ViewBag** with the name of the table.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. <span data-ttu-id="21cb4-144">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-144">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="21cb4-145">Na **přidat zobrazení** dialogové okno, zadejte **CreateTable** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-145">On the **Add View** dialog, enter **CreateTable** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="21cb4-146">Otevřete `CreateTable.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="21cb4-146">Open `CreateTable.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="21cb4-147">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-147">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="21cb4-148">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="21cb4-148">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. <span data-ttu-id="21cb4-149">Spusťte aplikaci a vyberte **vytvořit tabulku** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="21cb4-149">Run the application, and select **Create table** to see results similar to the following screen shot:</span></span>
  
    ![Vytvoření tabulky](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    <span data-ttu-id="21cb4-151">Jak je uvedeno nahoře, **CloudTable.CreateIfNotExists** metoda vrátí **true** pouze když neexistuje a k vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="21cb4-151">As mentioned previously, the **CloudTable.CreateIfNotExists** method returns **true** only when the table doesn't exist and is created.</span></span> <span data-ttu-id="21cb4-152">Proto pokud spustíte aplikaci v tabulce existuje, vrátí metoda **false**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-152">Therefore, if you run the app when the table exists, the method returns **false**.</span></span> <span data-ttu-id="21cb4-153">Aplikace je třeba spustit vícekrát, je nutné odstranit tabulky před spuštěním aplikace znovu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-153">To run the app multiple times, you must delete the table before running the app again.</span></span> <span data-ttu-id="21cb4-154">Odstraňování tabulky, můžete to udělat pomocí **CloudTable.Delete** metoda.</span><span class="sxs-lookup"><span data-stu-id="21cb4-154">Deleting the table can be done via the **CloudTable.Delete** method.</span></span> <span data-ttu-id="21cb4-155">Můžete také odstranit pomocí tabulky [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="21cb4-155">You can also delete the table using the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="21cb4-156">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="21cb4-156">Add an entity to a table</span></span>

<span data-ttu-id="21cb4-157">*Entity* mapu, která C\# objekty pomocí vlastní třídy odvozené od **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-157">*Entities* map to C\# objects by using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="21cb4-158">Když budete chtít do tabulky přidat entitu, vytvořte třídu, která definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="21cb4-158">To add an entity to a table, create a class that defines the properties of your entity.</span></span> <span data-ttu-id="21cb4-159">V této části se zobrazí, jak definovat třídu entity, která používá jméno zákazníka jako klíč řádku a jeho příjmení jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-159">In this section, you'll see how to define an entity class that uses the customer's first name as the row key and last name as the partition key.</span></span> <span data-ttu-id="21cb4-160">Společně pak klíč oddílu a řádku entity jednoznačně identifikují entitu v tabulce.</span><span class="sxs-lookup"><span data-stu-id="21cb4-160">Together, an entity's partition and row key uniquely identify the entity in the table.</span></span> <span data-ttu-id="21cb4-161">Na entity se stejným klíčem oddílu je možné se (v porovnání s těmi, které mají různé klíče oddílů) rychleji dotazovat, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operací.</span><span class="sxs-lookup"><span data-stu-id="21cb4-161">Entities with the same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="21cb4-162">Jakákoli vlastnost, která by měly být uložené ve službě table musí být vlastnost veřejná vlastnost podporovaného typu, který zveřejňuje nastavení i načítání hodnot.</span><span class="sxs-lookup"><span data-stu-id="21cb4-162">For any property that should be stored in the table service, the property must be a public property of a supported type that exposes both setting and retrieving values.</span></span>
<span data-ttu-id="21cb4-163">Třídy entita *musí* deklarovat veřejný konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="21cb4-163">The entity class *must* declare a public parameter-less constructor.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="21cb4-164">V této části se předpokládá dokončení kroků v [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="21cb4-164">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="21cb4-165">Otevřete soubor `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-165">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="21cb4-166">Přidejte následující direktivu tak, aby kód `TablesController.cs` můžete přístup k souboru **CustomerEntity** – třída:</span><span class="sxs-lookup"><span data-stu-id="21cb4-166">Add the following directive so that the code in the `TablesController.cs` file can access the **CustomerEntity** class:</span></span>

    ```csharp
    using StorageAspnet.Models;
    ```

1. <span data-ttu-id="21cb4-167">Přidejte metodu s názvem **AddEntity** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-167">Add a method called **AddEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="21cb4-168">V rámci **AddEntity** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="21cb4-168">Within the **AddEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="21cb4-169">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="21cb4-169">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="21cb4-170">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="21cb4-170">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="21cb4-171">Získání **CloudTable** objekt, který reprezentuje odkaz na tabulku, do které chcete přidat novou entitu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-171">Get a **CloudTable** object that represents a reference to the table to which you are going to add the new entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="21cb4-172">Vytváření instancí a inicializace **CustomerEntity** třídy.</span><span class="sxs-lookup"><span data-stu-id="21cb4-172">Instantiate and initialize the **CustomerEntity** class.</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. <span data-ttu-id="21cb4-173">Vytvoření **TableOperation** objekt, který se vloží entitu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="21cb4-173">Create a **TableOperation** object that inserts the customer entity.</span></span>

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. <span data-ttu-id="21cb4-174">Spuštění operace insert voláním **CloudTable.Execute** metoda.</span><span class="sxs-lookup"><span data-stu-id="21cb4-174">Execute the insert operation by calling the **CloudTable.Execute** method.</span></span> <span data-ttu-id="21cb4-175">Výsledek operace můžete ověřit zkontrolováním **TableResult.HttpStatusCode** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="21cb4-175">You can verify the result of the operation by inspecting the **TableResult.HttpStatusCode** property.</span></span> <span data-ttu-id="21cb4-176">Stavový kód 2xx označuje, že akce, kterou klient požaduje byl úspěšně zpracován.</span><span class="sxs-lookup"><span data-stu-id="21cb4-176">A status code of 2xx indicates the action requested by the client was processed successfully.</span></span> <span data-ttu-id="21cb4-177">Například úspěšné vložení výsledků nové entity v kód stavu HTTP 204, což znamená, že operace byla úspěšně zpracována a server nevrátil žádný obsah.</span><span class="sxs-lookup"><span data-stu-id="21cb4-177">For example, successful insertions of new entities results in an HTTP status code of 204, meaning that the operation was successfully processed and the server did not return any content.</span></span>

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. <span data-ttu-id="21cb4-178">Aktualizace **ViewBag** pomocí názvu tabulky a výsledky operace insert.</span><span class="sxs-lookup"><span data-stu-id="21cb4-178">Update the **ViewBag** with the table name, and the results of the insert operation.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. <span data-ttu-id="21cb4-179">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-179">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="21cb4-180">Na **přidat zobrazení** dialogové okno, zadejte **AddEntity** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-180">On the **Add View** dialog, enter **AddEntity** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="21cb4-181">Otevřete `AddEntity.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="21cb4-181">Open `AddEntity.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. <span data-ttu-id="21cb4-182">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-182">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="21cb4-183">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="21cb4-183">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. <span data-ttu-id="21cb4-184">Spusťte aplikaci a vyberte **Přidat entitu** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="21cb4-184">Run the application, and select **Add entity** to see results similar to the following screen shot:</span></span>
  
    ![Přidání entity](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    <span data-ttu-id="21cb4-186">Můžete ověřit, že byl přidán entity podle pokynů v části [získat jedné entity](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="21cb4-186">You can verify that the entity was added by following the steps in the section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="21cb4-187">Můžete také [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) zobrazíte všechny entity pro vaše tabulky.</span><span class="sxs-lookup"><span data-stu-id="21cb4-187">You can also use the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) to view all the entities for your tables.</span></span>

## <a name="add-a-batch-of-entities-to-a-table"></a><span data-ttu-id="21cb4-188">Přidat do tabulky dávky entit</span><span class="sxs-lookup"><span data-stu-id="21cb4-188">Add a batch of entities to a table</span></span>

<span data-ttu-id="21cb4-189">Kromě toho moct [přidání entity do tabulky, jeden v čase](#add-an-entity-to-a-table), můžete také přidat entity v dávce.</span><span class="sxs-lookup"><span data-stu-id="21cb4-189">In addition to being able to [add an entity to a table one at a time](#add-an-entity-to-a-table), you can also add entities in batch.</span></span> <span data-ttu-id="21cb4-190">Přidávání entit v dávce zkracuje dobu odezvy mezi kódu a služby Azure table.</span><span class="sxs-lookup"><span data-stu-id="21cb4-190">Adding entities in batch reduces the number of round-trips between your code and the Azure table service.</span></span> <span data-ttu-id="21cb4-191">Následující kroky ukazují, jak přidat více entit do tabulky s jedním vložit operace:</span><span class="sxs-lookup"><span data-stu-id="21cb4-191">The following steps illustrate how to add multiple entities to a table with a single insert operation:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="21cb4-192">V této části se předpokládá dokončení kroků v [nastavení vývojového prostředí](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="21cb4-192">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="21cb4-193">Otevřete soubor `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-193">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="21cb4-194">Přidejte metodu s názvem **AddEntities** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-194">Add a method called **AddEntities** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntities()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="21cb4-195">V rámci **AddEntities** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="21cb4-195">Within the **AddEntities** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="21cb4-196">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="21cb4-196">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="21cb4-197">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="21cb4-197">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="21cb4-198">Získání **CloudTable** objekt, který reprezentuje odkaz na tabulku, do které chcete přidat nové entity.</span><span class="sxs-lookup"><span data-stu-id="21cb4-198">Get a **CloudTable** object that represents a reference to the table to which you are going to add the new entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="21cb4-199">Vytvoření instance některých objektů zákazníka na základě **CustomerEntity** třída uvedené v části modelu [do tabulky přidat entitu](#add-an-entity-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="21cb4-199">Instantiate some customer objects based on the **CustomerEntity** model class presented in the section, [Add an entity to a table](#add-an-entity-to-a-table).</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. <span data-ttu-id="21cb4-200">Získání **TableBatchOperation** objektu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-200">Get a **TableBatchOperation** object.</span></span>

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. <span data-ttu-id="21cb4-201">Přidání entity do objektu operace dávkové vložení.</span><span class="sxs-lookup"><span data-stu-id="21cb4-201">Add entities to the batch insert operation object.</span></span>

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. <span data-ttu-id="21cb4-202">Spuštění operace insert batch voláním **CloudTable.ExecuteBatch** metoda.</span><span class="sxs-lookup"><span data-stu-id="21cb4-202">Execute the batch insert operation by calling the **CloudTable.ExecuteBatch** method.</span></span>   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. <span data-ttu-id="21cb4-203">**CloudTable.ExecuteBatch** metoda vrátí seznam hodnot **TableResult** objekty kde každý **TableResult** objekt můžete prověřit, abyste zjistili úspěch nebo neúspěch každé operace.</span><span class="sxs-lookup"><span data-stu-id="21cb4-203">The **CloudTable.ExecuteBatch** method returns a list of **TableResult** objects where each **TableResult** object can be examined to determine the success or failure of each individual operation.</span></span> <span data-ttu-id="21cb4-204">V tomto příkladu předat zobrazení seznamu a umožní zobrazení zobrazit výsledky jednotlivých operací.</span><span class="sxs-lookup"><span data-stu-id="21cb4-204">For this example, pass the list to a view and let the view display the results of each operation.</span></span> 
 
    ```csharp
    return View(results);
    ```

1. <span data-ttu-id="21cb4-205">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-205">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="21cb4-206">Na **přidat zobrazení** dialogové okno, zadejte **AddEntities** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-206">On the **Add View** dialog, enter **AddEntities** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="21cb4-207">Otevřete `AddEntities.cshtml`a upravit ho tak, aby vypadal jako následující.</span><span class="sxs-lookup"><span data-stu-id="21cb4-207">Open `AddEntities.cshtml`, and modify it so that it looks like the following.</span></span>

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

1. <span data-ttu-id="21cb4-208">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-208">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="21cb4-209">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="21cb4-209">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. <span data-ttu-id="21cb4-210">Spusťte aplikaci a vyberte **přidat entity** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="21cb4-210">Run the application, and select **Add entities** to see results similar to the following screen shot:</span></span>
  
    ![Přidání entit](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    <span data-ttu-id="21cb4-212">Můžete ověřit, že byl přidán entity podle pokynů v části [získat jedné entity](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="21cb4-212">You can verify that the entity was added by following the steps in the section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="21cb4-213">Můžete také [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) zobrazíte všechny entity pro vaše tabulky.</span><span class="sxs-lookup"><span data-stu-id="21cb4-213">You can also use the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) to view all the entities for your tables.</span></span>

## <a name="get-a-single-entity"></a><span data-ttu-id="21cb4-214">Získat jedné entity</span><span class="sxs-lookup"><span data-stu-id="21cb4-214">Get a single entity</span></span>

<span data-ttu-id="21cb4-215">Tato část ukazuje, jak získat jedné entity z tabulky pomocí klíč řádku entity a klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-215">This section illustrates how to get a single entity from a table using the entity's row key and partition key.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="21cb4-216">V této části se předpokládá dokončení kroků v [nastavení vývojového prostředí](#set-up-the-development-environment)a používá data z [dávky entit přidat do tabulky](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="21cb4-216">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="21cb4-217">Otevřete soubor `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-217">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="21cb4-218">Přidejte metodu s názvem **GetSingle** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-218">Add a method called **GetSingle** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetSingle()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="21cb4-219">V rámci **GetSingle** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="21cb4-219">Within the **GetSingle** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="21cb4-220">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="21cb4-220">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="21cb4-221">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="21cb4-221">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="21cb4-222">Získání **CloudTable** objekt, který reprezentuje odkaz na tabulku, ze kterého jsou načítání entity.</span><span class="sxs-lookup"><span data-stu-id="21cb4-222">Get a **CloudTable** object that represents a reference to the table from which you are retrieving the entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="21cb4-223">Vytvořit objekt operaci načtení, která přebírá objekt entity, který je odvozen od **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-223">Create a retrieve operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="21cb4-224">První parametr je *partitionKey*, a druhý parametr je *rowKey*.</span><span class="sxs-lookup"><span data-stu-id="21cb4-224">The first parameter is the *partitionKey*, and the second parameter is the *rowKey*.</span></span> <span data-ttu-id="21cb4-225">Pomocí **CustomerEntity** třídy a data uvedená v části [dávky entit přidat do tabulky](#add-a-batch-of-entities-to-a-table), následující fragment kódu dotazuje tabulku pro **CustomerEntity** entita s *partitionKey* hodnotu "Smith" a *rowKey* hodnotu "Ben":</span><span class="sxs-lookup"><span data-stu-id="21cb4-225">Using the **CustomerEntity** class and data presented in the section [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table), the following code snippet queries the table for a **CustomerEntity** entity with a *partitionKey* value of "Smith" and a *rowKey* value of "Ben":</span></span>

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. <span data-ttu-id="21cb4-226">Provedení operace načtení.</span><span class="sxs-lookup"><span data-stu-id="21cb4-226">Execute the retrieve operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. <span data-ttu-id="21cb4-227">Výsledek předejte zobrazení pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="21cb4-227">Pass the result to the view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="21cb4-228">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-228">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="21cb4-229">Na **přidat zobrazení** dialogové okno, zadejte **GetSingle** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-229">On the **Add View** dialog, enter **GetSingle** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="21cb4-230">Otevřete `GetSingle.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="21cb4-230">Open `GetSingle.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="21cb4-231">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-231">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="21cb4-232">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="21cb4-232">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. <span data-ttu-id="21cb4-233">Spusťte aplikaci a vyberte **získat jeden** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="21cb4-233">Run the application, and select **Get Single** to see results similar to the following screen shot:</span></span>
  
    ![Získat jeden](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a><span data-ttu-id="21cb4-235">Získání všech entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="21cb4-235">Get all entities in a partition</span></span>

<span data-ttu-id="21cb4-236">Jak je uvedeno v části [do tabulky přidat entitu](#add-an-entity-to-a-table), kombinace oddílu a klíč řádku jednoznačně identifikují entitu v tabulce.</span><span class="sxs-lookup"><span data-stu-id="21cb4-236">As mentioned in the section, [Add an entity to a table](#add-an-entity-to-a-table), the combination of a partition and a row key uniquely identify an entity in a table.</span></span> <span data-ttu-id="21cb4-237">Entity se stejným klíčem oddílu můžete položit dotaz na rychlejší než entity s různé klíče oddílů.</span><span class="sxs-lookup"><span data-stu-id="21cb4-237">Entities with the same partition key can be queried faster than entities with different partition keys.</span></span> <span data-ttu-id="21cb4-238">V této části ukazuje, jak dotaz na tabulku pro všechny entity ze zadaného oddílu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-238">This section illustrates how to query a table for all the entities from a specified partition.</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="21cb4-239">V této části se předpokládá dokončení kroků v [nastavení vývojového prostředí](#set-up-the-development-environment)a používá data z [dávky entit přidat do tabulky](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="21cb4-239">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="21cb4-240">Otevřete soubor `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-240">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="21cb4-241">Přidejte metodu s názvem **GetPartition** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-241">Add a method called **GetPartition** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetPartition()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="21cb4-242">V rámci **GetPartition** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="21cb4-242">Within the **GetPartition** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="21cb4-243">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="21cb4-243">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="21cb4-244">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="21cb4-244">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="21cb4-245">Získání **CloudTable** objekt, který reprezentuje odkaz na tabulku, ze kterého jsou načítání entity.</span><span class="sxs-lookup"><span data-stu-id="21cb4-245">Get a **CloudTable** object that represents a reference to the table from which you are retrieving the entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="21cb4-246">Vytváření instancí **TableQuery** zadat dotaz v objektu **kde** klauzule.</span><span class="sxs-lookup"><span data-stu-id="21cb4-246">Instantiate a **TableQuery** object specifying the query in the **Where** clause.</span></span> <span data-ttu-id="21cb4-247">Pomocí **CustomerEntity** třídy a data uvedená v části [dávky entit přidat do tabulky](#add-a-batch-of-entities-to-a-table), následující fragment kódu dotazuje tabulku pro všechny entity kde **PartitionKey** (příjmení zákazníka) má hodnotu "Smith":</span><span class="sxs-lookup"><span data-stu-id="21cb4-247">Using the **CustomerEntity** class and data presented in the section [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table), the following code snippet queries the table for a all entities where the **PartitionKey** (customer's last name) has a value of "Smith":</span></span>

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. <span data-ttu-id="21cb4-248">V rámci smyčku, volání **CloudTable.ExecuteQuerySegmented** metoda předání objektu dotazu instanci v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="21cb4-248">Within a loop, call the **CloudTable.ExecuteQuerySegmented** method passing the query object you instantiated in the previous step.</span></span>  <span data-ttu-id="21cb4-249">**CloudTable.ExecuteQuerySegmented** metoda vrátí **TableContinuationToken** objektu, který - při **null** – označuje, že neexistují žádné další entity načíst.</span><span class="sxs-lookup"><span data-stu-id="21cb4-249">The **CloudTable.ExecuteQuerySegmented** method returns a **TableContinuationToken** object that - when **null** - indicates that there are no more entities to retrieve.</span></span> <span data-ttu-id="21cb4-250">V rámci smyčky Iterujte přes vrácené entity pomocí jiné smyčky.</span><span class="sxs-lookup"><span data-stu-id="21cb4-250">Within the loop, use another loop to iterate over the returned entities.</span></span> <span data-ttu-id="21cb4-251">V následujícím příkladu kódu je každou vrácenou entitu přidat do seznamu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-251">In the following code example, each returned entity is added to a list.</span></span> <span data-ttu-id="21cb4-252">Po skončení smyčky seznamu byla předána do zobrazení pro zobrazení:</span><span class="sxs-lookup"><span data-stu-id="21cb4-252">Once the loop ends, the list is passed to a view for display:</span></span> 

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

1. <span data-ttu-id="21cb4-253">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-253">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="21cb4-254">Na **přidat zobrazení** dialogové okno, zadejte **GetPartition** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-254">On the **Add View** dialog, enter **GetPartition** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="21cb4-255">Otevřete `GetPartition.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="21cb4-255">Open `GetPartition.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="21cb4-256">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-256">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="21cb4-257">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="21cb4-257">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. <span data-ttu-id="21cb4-258">Spusťte aplikaci a vyberte **získat oddílu** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="21cb4-258">Run the application, and select **Get Partition** to see results similar to the following screen shot:</span></span>
  
    ![Získat oddílu](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a><span data-ttu-id="21cb4-260">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="21cb4-260">Delete an entity</span></span>

<span data-ttu-id="21cb4-261">Tato část ukazuje postup odstranění entity z tabulky.</span><span class="sxs-lookup"><span data-stu-id="21cb4-261">This section illustrates how to delete an entity from a table.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="21cb4-262">V této části se předpokládá dokončení kroků v [nastavení vývojového prostředí](#set-up-the-development-environment)a používá data z [dávky entit přidat do tabulky](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="21cb4-262">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="21cb4-263">Otevřete soubor `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-263">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="21cb4-264">Přidejte metodu s názvem **DeleteEntity** , který vrací **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-264">Add a method called **DeleteEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="21cb4-265">V rámci **DeleteEntity** metody get **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="21cb4-265">Within the **DeleteEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="21cb4-266">Použít následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure: (Změna  *&lt;název účtu úložiště >* k názvu účtu úložiště Azure přistupujete.)</span><span class="sxs-lookup"><span data-stu-id="21cb4-266">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="21cb4-267">Získání **CloudTableClient** objekt představuje klienta služby table.</span><span class="sxs-lookup"><span data-stu-id="21cb4-267">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="21cb4-268">Získání **CloudTable** objekt, který reprezentuje odkaz na tabulku, ze kterého chcete odstranit entitu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-268">Get a **CloudTable** object that represents a reference to the table from which you are deleting the entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="21cb4-269">Vytvořit objekt operaci odstranění, která přebírá objekt entity, který je odvozen od **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-269">Create a delete operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="21cb4-270">V tomto případě používáme **CustomerEntity** třídy a data uvedená v části [dávky entit přidat do tabulky](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="21cb4-270">In this case, we use the **CustomerEntity** class and data presented in the section [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> <span data-ttu-id="21cb4-271">Entity **značka ETag** musí být nastavena na platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="21cb4-271">The entity's **ETag** must be set to a valid value.</span></span>  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. <span data-ttu-id="21cb4-272">Provést operaci odstranění.</span><span class="sxs-lookup"><span data-stu-id="21cb4-272">Execute the delete operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. <span data-ttu-id="21cb4-273">Výsledek předejte zobrazení pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="21cb4-273">Pass the result to the view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="21cb4-274">V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **tabulky**a v místní nabídce vyberte **Přidat -> zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-274">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="21cb4-275">Na **přidat zobrazení** dialogové okno, zadejte **DeleteEntity** pro název zobrazení, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="21cb4-275">On the **Add View** dialog, enter **DeleteEntity** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="21cb4-276">Otevřete `DeleteEntity.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="21cb4-276">Open `DeleteEntity.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="21cb4-277">V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="21cb4-277">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="21cb4-278">Za poslední **Html.ActionLink**, přidejte následující **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="21cb4-278">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. <span data-ttu-id="21cb4-279">Spusťte aplikaci a vyberte **odstranit entity** a zobrazte výsledky podobné následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="21cb4-279">Run the application, and select **Delete entity** to see results similar to the following screen shot:</span></span>
  
    ![Získat jeden](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a><span data-ttu-id="21cb4-281">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21cb4-281">Next steps</span></span>
<span data-ttu-id="21cb4-282">Projděte si další průvodce funkcemi, kde najdete další informace o dalších možnostech pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="21cb4-282">View more feature guides to learn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="21cb4-283">Začínáme s Azure blob storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="21cb4-283">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="21cb4-284">Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="21cb4-284">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](../storage/vs-storage-aspnet-getting-started-queues.md)
