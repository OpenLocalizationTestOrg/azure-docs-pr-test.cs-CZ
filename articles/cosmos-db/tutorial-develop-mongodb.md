---
title: "aaaUse Azure Cosmos DB na rozhraní API pro MongoDB toobuild webové aplikace | Microsoft Docs"
description: "Azure Cosmos DB kurz, který vytváří online databáze webové aplikace pomocí rozhraní API hello pro MongoDB."
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
ms.openlocfilehash: 6dfa7fef49fc53ea2fcfcfbad3b3fcf97ac18e94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-connect-tooa-mongodb-app-using-net"></a><span data-ttu-id="ecf14-104">Azure Cosmos DB: Připojení tooa MongoDB aplikace pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ecf14-104">Azure Cosmos DB: Connect tooa MongoDB app using .NET</span></span>

<span data-ttu-id="ecf14-105">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="ecf14-105">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="ecf14-106">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ecf14-106">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="ecf14-107">Tento kurz ukazuje, jak hello toocreate účtu Azure Cosmos DB pomocí portálu Azure a jak toocreate databázi a kolekci toostore dat pomocí hello [MongoDB API](mongodb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ecf14-107">This tutorial demonstrates how toocreate an Azure Cosmos DB account using hello Azure portal, and how toocreate a database and collection toostore data using hello [MongoDB API](mongodb-introduction.md).</span></span> 

<span data-ttu-id="ecf14-108">Tento kurz se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="ecf14-108">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ecf14-109">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ecf14-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="ecf14-110">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="ecf14-110">Update your connection string</span></span>
> * <span data-ttu-id="ecf14-111">Vytvoření aplikace pro MongoDB ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="ecf14-111">Create a MongoDB app on a virtual machine</span></span> 


## <a name="create-a-database-account"></a><span data-ttu-id="ecf14-112">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="ecf14-112">Create a database account</span></span>

<span data-ttu-id="ecf14-113">Začněme vytvořením účtu Azure Cosmos DB v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ecf14-113">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="ecf14-114">Již máte účet Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="ecf14-114">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="ecf14-115">Pokud ano, přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="ecf14-115">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="ecf14-116">Měli jste účet Azure DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="ecf14-116">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="ecf14-117">Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="ecf14-117">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="ecf14-118">Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="ecf14-118">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a><span data-ttu-id="ecf14-119">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="ecf14-119">Update your connection string</span></span>

1. <span data-ttu-id="ecf14-120">V portálu Azure, v hello hello **Azure Cosmos DB** vyberte hello rozhraní API pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="ecf14-120">In hello Azure portal, in hello **Azure Cosmos DB** page, select hello API for MongoDB account.</span></span> 
2. <span data-ttu-id="ecf14-121">V levém panelu hello hello účet okna klikněte na **úvodní**.</span><span class="sxs-lookup"><span data-stu-id="ecf14-121">In hello left bar of hello account blade, click **Quick start**.</span></span> 
3. <span data-ttu-id="ecf14-122">Vyberte svou platformu (*ovladačů rozhraní .NET*, *Node.js ovladač*, *MongoDB prostředí*, *Java ovladač*, *Python ovladač*).</span><span class="sxs-lookup"><span data-stu-id="ecf14-122">Choose your platform (*.NET driver*, *Node.js driver*, *MongoDB Shell*, *Java driver*, *Python driver*).</span></span> <span data-ttu-id="ecf14-123">Pokud nevidíte ovladače nebo nástroj uvedené, nemusíte si dělat starosti, budeme průběžně dokumentu další fragmenty kódu připojení.</span><span class="sxs-lookup"><span data-stu-id="ecf14-123">If you don't see your driver or tool listed, don't worry, we continuously document more connection code snippets.</span></span> 
4. <span data-ttu-id="ecf14-124">Zkopírujte a vložte fragment kódu hello do vaší aplikace pro MongoDB, a jsou připravené toogo.</span><span class="sxs-lookup"><span data-stu-id="ecf14-124">Copy and paste hello code snippet into your MongoDB app, and you are ready toogo.</span></span>

## <a name="set-up-your-mongodb-app"></a><span data-ttu-id="ecf14-125">Nastavit aplikaci MongoDB</span><span class="sxs-lookup"><span data-stu-id="ecf14-125">Set up your MongoDB app</span></span>

<span data-ttu-id="ecf14-126">Můžete použít hello [vytvořit webovou aplikaci v Azure, která se připojuje tooMongoDB spuštěny na virtuálním počítači](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) kurz s minimálním změnách, instalační program tooquickly MongoDB aplikace (buď místně nebo tooan publikované webové aplikace Azure), připojí tooan rozhraní API pro účet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="ecf14-126">You can use hello [Create a web app in Azure that connects tooMongoDB running on a virtual machine](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) tutorial, with minimal modification, tooquickly setup a MongoDB application (either locally or published tooan Azure web app) that connects tooan API for MongoDB account.</span></span>  

1. <span data-ttu-id="ecf14-127">Postupujte podle kurzu hello s jeden úpravy.</span><span class="sxs-lookup"><span data-stu-id="ecf14-127">Follow hello tutorial, with one modification.</span></span>  <span data-ttu-id="ecf14-128">Nahraďte kód Dal.cs hello toto:</span><span class="sxs-lookup"><span data-stu-id="ecf14-128">Replace hello Dal.cs code with this:</span></span>

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
   
            // toodo: update hello connection string with hello DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net
            private string connectionString = "mongodb://localhost:27017";
            private string userName = "<your user name>";
            private string host = "<your host>";
            private string password = "<your password>";
   
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  hello database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
   
            // Default constructor.        
            public Dal()
            {
            }
   
            // Gets all Task items from hello MongoDB server.        
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
   
            // Creates a Task and inserts it into hello collection in MongoDB.
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

2. <span data-ttu-id="ecf14-129">Upravte hello následující proměnné v souboru Dal.cs hello na nastavení svého účtu ze stránky klíče hello hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="ecf14-129">Modify hello following variables in hello Dal.cs file per your account settings from hello Keys page in hello Azure portal:</span></span>

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. <span data-ttu-id="ecf14-130">Použití aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ecf14-130">Use hello app!</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="ecf14-131">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="ecf14-131">Clean up resources</span></span>

<span data-ttu-id="ecf14-132">Pokud ale nebudete toocontinue toouse tuto aplikaci, použijte následující kroky toodelete všechny prostředky vytvořené v tomto kurzu v portálu Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="ecf14-132">If you're not going toocontinue toouse this app, use hello following steps toodelete all resources created by this tutorial in hello Azure portal.</span></span> 

1. <span data-ttu-id="ecf14-133">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ecf14-133">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="ecf14-134">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="ecf14-134">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecf14-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecf14-135">Next steps</span></span>

<span data-ttu-id="ecf14-136">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="ecf14-136">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ecf14-137">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ecf14-137">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="ecf14-138">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="ecf14-138">Update your connection string</span></span>
> * <span data-ttu-id="ecf14-139">Vytvoření aplikace pro MongoDB ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="ecf14-139">Create a MongoDB app on a virtual machine</span></span>

<span data-ttu-id="ecf14-140">Můžete pokračovat dalším kurzu toohello a importovat data tooAzure vaše MongoDB Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ecf14-140">You can proceed toohello next tutorial and import your MongoDB data tooAzure Cosmos DB.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="ecf14-141">Importování dat MongoDB do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ecf14-141">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)

