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
# <a name="azure-cosmos-db-connect-tooa-mongodb-app-using-net"></a>Azure Cosmos DB: Připojení tooa MongoDB aplikace pomocí rozhraní .NET

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento kurz ukazuje, jak hello toocreate účtu Azure Cosmos DB pomocí portálu Azure a jak toocreate databázi a kolekci toostore dat pomocí hello [MongoDB API](mongodb-introduction.md). 

Tento kurz se zabývá hello následující úlohy:

> [!div class="checklist"]
> * Vytvoření účtu služby Azure Cosmos DB 
> * Aktualizace připojovacího řetězce
> * Vytvoření aplikace pro MongoDB ve virtuálním počítači 


## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Začněme vytvořením účtu Azure Cosmos DB v hello portálu Azure.  

> [!TIP]
> * Již máte účet Azure Cosmos DB? Pokud ano, přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS)
> * Měli jste účet Azure DocumentDB? Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).  
> * Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

1. V portálu Azure, v hello hello **Azure Cosmos DB** vyberte hello rozhraní API pro účet MongoDB. 
2. V levém panelu hello hello účet okna klikněte na **úvodní**. 
3. Vyberte svou platformu (*ovladačů rozhraní .NET*, *Node.js ovladač*, *MongoDB prostředí*, *Java ovladač*, *Python ovladač*). Pokud nevidíte ovladače nebo nástroj uvedené, nemusíte si dělat starosti, budeme průběžně dokumentu další fragmenty kódu připojení. 
4. Zkopírujte a vložte fragment kódu hello do vaší aplikace pro MongoDB, a jsou připravené toogo.

## <a name="set-up-your-mongodb-app"></a>Nastavit aplikaci MongoDB

Můžete použít hello [vytvořit webovou aplikaci v Azure, která se připojuje tooMongoDB spuštěny na virtuálním počítači](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) kurz s minimálním změnách, instalační program tooquickly MongoDB aplikace (buď místně nebo tooan publikované webové aplikace Azure), připojí tooan rozhraní API pro účet MongoDB.  

1. Postupujte podle kurzu hello s jeden úpravy.  Nahraďte kód Dal.cs hello toto:

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

2. Upravte hello následující proměnné v souboru Dal.cs hello na nastavení svého účtu ze stránky klíče hello hello portálu Azure:

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. Použití aplikace hello!

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse tuto aplikaci, použijte následující kroky toodelete všechny prostředky vytvořené v tomto kurzu v portálu Azure hello hello. 

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Vytvoření účtu služby Azure Cosmos DB 
> * Aktualizace připojovacího řetězce
> * Vytvoření aplikace pro MongoDB ve virtuálním počítači

Můžete pokračovat dalším kurzu toohello a importovat data tooAzure vaše MongoDB Cosmos DB.  

> [!div class="nextstepaction"]
> [Importování dat MongoDB do služby Azure Cosmos DB](mongodb-migrate.md)

