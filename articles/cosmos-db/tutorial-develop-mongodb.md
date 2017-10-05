---
title: "Použít k vytvoření webové aplikace API Azure Cosmos DB pro MongoDB | Microsoft Docs"
description: "Azure Cosmos DB kurz, který vytváří online databáze webové aplikace pomocí rozhraní API pro MongoDB."
keywords: "Příklady mongodb"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 61a2ab3a-2fc3-4d49-a263-ed87c66628f6
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: ff277c7f88359cd977424f2e0958c69e2547a2af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-connect-to-a-mongodb-app-using-net"></a><span data-ttu-id="fd6e2-104">Azure Cosmos DB: Připojení k MongoDB aplikace pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="fd6e2-104">Azure Cosmos DB: Connect to a MongoDB app using .NET</span></span>

<span data-ttu-id="fd6e2-105">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-105">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="fd6e2-106">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-106">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="fd6e2-107">Tento kurz ukazuje, jak vytvořit účet Azure Cosmos DB pomocí portálu Azure a jak vytvořit databázi a kolekci k ukládání dat pomocí [MongoDB API](mongodb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fd6e2-107">This tutorial demonstrates how to create an Azure Cosmos DB account using the Azure portal, and how to create a database and collection to store data using the [MongoDB API](mongodb-introduction.md).</span></span> 

<span data-ttu-id="fd6e2-108">Tento kurz obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="fd6e2-108">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fd6e2-109">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fd6e2-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="fd6e2-110">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="fd6e2-110">Update your connection string</span></span>
> * <span data-ttu-id="fd6e2-111">Vytvoření aplikace pro MongoDB ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="fd6e2-111">Create a MongoDB app on a virtual machine</span></span> 


## <a name="create-a-database-account"></a><span data-ttu-id="fd6e2-112">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="fd6e2-112">Create a database account</span></span>

<span data-ttu-id="fd6e2-113">Začněme vytvořením účtu Azure Cosmos DB na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-113">Let's start by creating an Azure Cosmos DB account in the Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="fd6e2-114">Již máte účet Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="fd6e2-114">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="fd6e2-115">Pokud ano, přeskočit na [nastavit řešení sady Visual Studio](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="fd6e2-115">If so, skip ahead to [Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="fd6e2-116">Měli jste účet Azure DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="fd6e2-116">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="fd6e2-117">Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit na [nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="fd6e2-117">If so, your account is now an Azure Cosmos DB account and you can skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="fd6e2-118">Pokud používáte emulátor DB Cosmos Azure, postupujte podle kroků v [emulátoru DB Cosmos Azure](local-emulator.md) nastavit emulátoru a přeskočit na [nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="fd6e2-118">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a><span data-ttu-id="fd6e2-119">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="fd6e2-119">Update your connection string</span></span>

1. <span data-ttu-id="fd6e2-120">Na portálu Azure v **Azure Cosmos DB** vyberte rozhraní API pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-120">In the Azure portal, in the **Azure Cosmos DB** page, select the API for MongoDB account.</span></span> 
2. <span data-ttu-id="fd6e2-121">V levém panelu okno účtu, klikněte na **úvodní**.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-121">In the left bar of the account blade, click **Quick start**.</span></span> 
3. <span data-ttu-id="fd6e2-122">Vyberte svou platformu (*ovladačů rozhraní .NET*, *Node.js ovladač*, *MongoDB prostředí*, *Java ovladač*, *Python ovladač*).</span><span class="sxs-lookup"><span data-stu-id="fd6e2-122">Choose your platform (*.NET driver*, *Node.js driver*, *MongoDB Shell*, *Java driver*, *Python driver*).</span></span> <span data-ttu-id="fd6e2-123">Pokud nevidíte ovladače nebo nástroj uvedené, nemusíte si dělat starosti, budeme průběžně dokumentu další fragmenty kódu připojení.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-123">If you don't see your driver or tool listed, don't worry, we continuously document more connection code snippets.</span></span> 
4. <span data-ttu-id="fd6e2-124">Zkopírovat a Vložit fragment kódu do vaší aplikace MongoDB a jsou připravené na vynucování.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-124">Copy and paste the code snippet into your MongoDB app, and you are ready to go.</span></span>

## <a name="set-up-your-mongodb-app"></a><span data-ttu-id="fd6e2-125">Nastavit aplikaci MongoDB</span><span class="sxs-lookup"><span data-stu-id="fd6e2-125">Set up your MongoDB app</span></span>

<span data-ttu-id="fd6e2-126">Můžete použít [vytvořit webovou aplikaci v Azure, která se připojuje k MongoDB, které jsou spuštěny na virtuálním počítači](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) kurz s minimálním změnách, se rychle nastavit aplikaci MongoDB (buď místně nebo publikována na webové aplikace Azure), připojí do rozhraní API pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-126">You can use the [Create a web app in Azure that connects to MongoDB running on a virtual machine](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) tutorial, with minimal modification, to quickly setup a MongoDB application (either locally or published to an Azure web app) that connects to an API for MongoDB account.</span></span>  

1. <span data-ttu-id="fd6e2-127">Postupujte podle kurzu s jeden úpravy.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-127">Follow the tutorial, with one modification.</span></span>  <span data-ttu-id="fd6e2-128">Nahraďte kód Dal.cs toto:</span><span class="sxs-lookup"><span data-stu-id="fd6e2-128">Replace the Dal.cs code with this:</span></span>

    ```csharp   
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;
    using System.Security.Authentication;
   
    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            //private MongoServer mongoServer = null;
            private bool disposed = false;
   
            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net
            private string connectionString = "mongodb://localhost:27017";
            private string userName = "<your user name>";
            private string host = "<your host>";
            private string password = "<your password>";
   
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  The database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
   
            // Default constructor.        
            public Dal()
            {
            }
   
            // Gets all Task items from the MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }
   
            // Creates a Task and inserts it into the collection in MongoDB.
            public void CreateTask(MyTask task)
            {
                var collection = GetTasksCollectionForEdit();
                try
                {
                    collection.InsertOne(task);
                }
                catch (MongoCommandException ex)
                {
                    string msg = ex.Message;
                }
            }
   
            private IMongoCollection<MyTask> GetTasksCollection()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10255);
                settings.UseSsl = true;
                settings.SslSettings = new SslSettings();
                settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
   
                MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                MongoIdentityEvidence evidence = new PasswordEvidence(password);
   
                settings.Credentials = new List<MongoCredential>()
                {
                    new MongoCredential("SCRAM-SHA-1", identity, evidence)
                };
   
                MongoClient client = new MongoClient(settings);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10255);
                settings.UseSsl = true;
                settings.SslSettings = new SslSettings();
                settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
   
                MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                MongoIdentityEvidence evidence = new PasswordEvidence(password);
   
                settings.Credentials = new List<MongoCredential>()
                {
                    new MongoCredential("SCRAM-SHA-1", identity, evidence)
                };
                MongoClient client = new MongoClient(settings);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            # region IDisposable
   
            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }
   
            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                    }
                }
   
                this.disposed = true;
            }
   
            # endregion
        }
    }
    ```

2. <span data-ttu-id="fd6e2-129">Upravte následující proměnné v souboru Dal.cs na nastavení svého účtu na stránce klíče na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="fd6e2-129">Modify the following variables in the Dal.cs file per your account settings from the Keys page in the Azure portal:</span></span>

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. <span data-ttu-id="fd6e2-130">Použití aplikace!</span><span class="sxs-lookup"><span data-stu-id="fd6e2-130">Use the app!</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="fd6e2-131">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="fd6e2-131">Clean up resources</span></span>

<span data-ttu-id="fd6e2-132">Pokud nebudete tuto aplikaci nadále používat, pomocí následujícího postupu odstraňte všechny prostředky vytvořené tímto rychlým startem na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-132">If you're not going to continue to use this app, use the following steps to delete all resources created by this tutorial in the Azure portal.</span></span> 

1. <span data-ttu-id="fd6e2-133">V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-133">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="fd6e2-134">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-134">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd6e2-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fd6e2-135">Next steps</span></span>

<span data-ttu-id="fd6e2-136">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="fd6e2-136">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fd6e2-137">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fd6e2-137">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="fd6e2-138">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="fd6e2-138">Update your connection string</span></span>
> * <span data-ttu-id="fd6e2-139">Vytvoření aplikace pro MongoDB ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="fd6e2-139">Create a MongoDB app on a virtual machine</span></span>

<span data-ttu-id="fd6e2-140">Můžete pokračovat v dalším kurzu a importovat MongoDB data do Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fd6e2-140">You can proceed to the next tutorial and import your MongoDB data to Azure Cosmos DB.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="fd6e2-141">Importování dat MongoDB do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fd6e2-141">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)

