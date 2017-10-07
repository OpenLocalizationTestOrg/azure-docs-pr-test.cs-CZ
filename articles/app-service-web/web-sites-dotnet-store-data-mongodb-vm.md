---
title: "aaaCreate webové aplikace v Azure, která se připojuje tooMongoDB spuštěny na virtuálním počítači"
description: "Kurz, který se naučíte, jak toouse Git toodeploy tooAzure aplikace ASP.NET služby App Service, připojení tooMongoDB na virtuální počítač Azure."
tags: azure-portal
services: app-service\web, virtual-machines
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: adf7a472-ae00-45a8-aec4-06247e21318b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/29/2016
ms.author: cephalin
ms.openlocfilehash: 1f5f42c28c3c294d92c9ebf1499374931d47c010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-azure-that-connects-toomongodb-running-on-a-virtual-machine"></a><span data-ttu-id="9e62f-103">Vytvoření webové aplikace v Azure, která se připojuje tooMongoDB spuštěny na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="9e62f-103">Create a web app in Azure that connects tooMongoDB running on a virtual machine</span></span>
<span data-ttu-id="9e62f-104">Pomocí Git, můžete nasadit tooAzure aplikace ASP.NET App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="9e62f-104">Using Git, you can deploy an ASP.NET application tooAzure App Service Web Apps.</span></span> <span data-ttu-id="9e62f-105">V tomto kurzu vytvoříte jednoduchý front-endové rozhraní ASP.NET MVC aplikaci seznamu úkolů, která připojí databázi MongoDB tooa spuštěny na virtuálním počítači v Azure.</span><span class="sxs-lookup"><span data-stu-id="9e62f-105">In this tutorial, you will build a simple front-end ASP.NET MVC task list application that connects tooa MongoDB database running on a virtual machine in Azure.</span></span>  <span data-ttu-id="9e62f-106">[MongoDB] [ MongoDB] je populární open source, vysoký výkon databáze NoSQL.</span><span class="sxs-lookup"><span data-stu-id="9e62f-106">[MongoDB][MongoDB] is a popular open source, high performance NoSQL database.</span></span> <span data-ttu-id="9e62f-107">Po spuštění a testování aplikace ASP.NET hello ve svém vývojovém počítači, bude nahrávat aplikace tooApp hello Service Web Apps pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="9e62f-107">After running and testing hello ASP.NET application on your development computer, you will upload hello application tooApp Service Web Apps using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="9e62f-108">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="9e62f-108">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="9e62f-109">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="9e62f-109">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="background-knowledge"></a><span data-ttu-id="9e62f-110">Znalostní báze pozadí</span><span class="sxs-lookup"><span data-stu-id="9e62f-110">Background knowledge</span></span>
<span data-ttu-id="9e62f-111">V tomto kurzu, přestože není povinný je užitečné znalosti hello následující:</span><span class="sxs-lookup"><span data-stu-id="9e62f-111">Knowledge of hello following is useful for this tutorial, though not required:</span></span>

* <span data-ttu-id="9e62f-112">Hello C# ovladačů pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="9e62f-112">hello C# driver for MongoDB.</span></span> <span data-ttu-id="9e62f-113">Další informace o vývoji aplikace C# pro MongoDB, najdete v části hello MongoDB [CSharp jazyk Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="9e62f-113">For more information on developing C# applications against MongoDB, see hello MongoDB [CSharp Language Center][MongoC#LangCenter].</span></span> 
* <span data-ttu-id="9e62f-114">Architektura Hello ASP .NET webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e62f-114">hello ASP .NET web application framework.</span></span> <span data-ttu-id="9e62f-115">Se dozvíte v hello [webu ASP.net][ASP.NET].</span><span class="sxs-lookup"><span data-stu-id="9e62f-115">You can learn all about it at hello [ASP.net website][ASP.NET].</span></span>
* <span data-ttu-id="9e62f-116">Architektura Hello ASP .NET MVC webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e62f-116">hello ASP .NET MVC web application framework.</span></span> <span data-ttu-id="9e62f-117">Se dozvíte v hello [webové stránky ASP.NET MVC][MVCWebSite].</span><span class="sxs-lookup"><span data-stu-id="9e62f-117">You can learn all about it at hello [ASP.NET MVC website][MVCWebSite].</span></span>
* <span data-ttu-id="9e62f-118">Azure.</span><span class="sxs-lookup"><span data-stu-id="9e62f-118">Azure.</span></span> <span data-ttu-id="9e62f-119">Abyste mohli začít, čtení v [Azure][WindowsAzure].</span><span class="sxs-lookup"><span data-stu-id="9e62f-119">You can get started reading at [Azure][WindowsAzure].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e62f-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9e62f-120">Prerequisites</span></span>
* <span data-ttu-id="9e62f-121">[Visual Studio Express 2013 pro Web] [ VSEWeb] nebo [Visual Studio 2013][VSUlt]</span><span class="sxs-lookup"><span data-stu-id="9e62f-121">[Visual Studio Express 2013 for Web][VSEWeb] or [Visual Studio 2013][VSUlt]</span></span>
* [<span data-ttu-id="9e62f-122">Azure SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="9e62f-122">Azure SDK for .NET</span></span>](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* <span data-ttu-id="9e62f-123">Aktivní předplatné Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="9e62f-123">An active Microsoft Azure subscription</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a><span data-ttu-id="9e62f-124">Vytvoření virtuálního počítače a nainstalujte MongoDB</span><span class="sxs-lookup"><span data-stu-id="9e62f-124">Create a virtual machine and install MongoDB</span></span>
<span data-ttu-id="9e62f-125">Tento kurz předpokládá, že jste vytvořili virtuální počítač v Azure.</span><span class="sxs-lookup"><span data-stu-id="9e62f-125">This tutorial assumes you have created a virtual machine in Azure.</span></span> <span data-ttu-id="9e62f-126">Po vytvoření hello virtuálního počítače je nutné tooinstall MongoDB hello virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="9e62f-126">After creating hello virtual machine you need tooinstall MongoDB on hello virtual machine:</span></span>

* <span data-ttu-id="9e62f-127">toocreate virtuálního počítače s Windows a nainstalujte MongoDB, najdete v části [nainstalujte MongoDB ve virtuálním počítači s Windows serverem v Azure][InstallMongoOnWindowsVM].</span><span class="sxs-lookup"><span data-stu-id="9e62f-127">toocreate a Windows virtual machine and install MongoDB, see [Install MongoDB on a virtual machine running Windows Server in Azure][InstallMongoOnWindowsVM].</span></span>

<span data-ttu-id="9e62f-128">Po vytvoření hello virtuálního počítače v Azure a nainstalován MongoDB, být jisti tooremember hello název DNS hello virtuálního počítače ("testlinuxvm.cloudapp.net", např.) a hello externí port pro MongoDB, který jste zadali v hello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9e62f-128">After you have created hello virtual machine in Azure and installed MongoDB, be sure tooremember hello DNS name of hello virtual machine ("testlinuxvm.cloudapp.net", for example) and hello external port for MongoDB that you specified in hello endpoint.</span></span>  <span data-ttu-id="9e62f-129">Budete potřebovat později v kurzu hello tyto informace.</span><span class="sxs-lookup"><span data-stu-id="9e62f-129">You will need this information later in hello tutorial.</span></span>

<a id="createapp"></a>

## <a name="create-hello-application"></a><span data-ttu-id="9e62f-130">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9e62f-130">Create hello application</span></span>
<span data-ttu-id="9e62f-131">V této části vytvoříte aplikaci ASP.NET s názvem "Moje seznam úkolů" pomocí sady Visual Studio a provedení počáteční nasazení tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="9e62f-131">In this section you will create an ASP.NET application called "My Task List" by using Visual Studio and perform an initial deployment tooAzure App Service Web Apps.</span></span> <span data-ttu-id="9e62f-132">Spustí hello aplikaci místně, ale bude připojení tooyour virtuálního počítače na Azure a pomocí instance hello MongoDB, kterou jste vytvořili existuje.</span><span class="sxs-lookup"><span data-stu-id="9e62f-132">You will run hello application locally, but it will connect tooyour virtual machine on Azure and use hello MongoDB instance that you created there.</span></span>

1. <span data-ttu-id="9e62f-133">V sadě Visual Studio, klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-133">In Visual Studio, click **New Project**.</span></span>
   
    ![Spuštění nového projektu stránky][StartPageNewProject]
2. <span data-ttu-id="9e62f-135">V hello **nový projekt** okno, v hello levém podokně vyberte **Visual C#**a potom vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-135">In hello **New Project** window, in hello left pane, select **Visual C#**, and then select **Web**.</span></span> <span data-ttu-id="9e62f-136">V prostředním podokně hello vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-136">In hello middle pane, select **ASP.NET  Web Application**.</span></span> <span data-ttu-id="9e62f-137">V dolní části hello, pojmenujte svůj projekt "MyTaskListApp" a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-137">At hello bottom, name your project "MyTaskListApp," and then click **OK**.</span></span>
   
    ![Dialogové okno Nový projekt][NewProjectMyTaskListApp]
3. <span data-ttu-id="9e62f-139">V hello **nový projekt ASP.NET** dialogové okno, vyberte **MVC**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-139">In hello **New ASP.NET Project** dialog box, select **MVC**, and then click **OK**.</span></span>
   
    ![Vyberte šablonu MVC][VS2013SelectMVCTemplate]
4. <span data-ttu-id="9e62f-141">Pokud už nejste do Microsoft Azure, bude výzvami toosign v.</span><span class="sxs-lookup"><span data-stu-id="9e62f-141">If you aren't already signed into Microsoft Azure, you will be prompted toosign in.</span></span> <span data-ttu-id="9e62f-142">Postupujte podle pokynů toosign hello do Azure.</span><span class="sxs-lookup"><span data-stu-id="9e62f-142">Follow hello prompts toosign into Azure.</span></span>
5. <span data-ttu-id="9e62f-143">Jakmile se přihlásíte, můžete začít konfigurace webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="9e62f-143">Once you are signed in, you can start configuring your App Service web app.</span></span> <span data-ttu-id="9e62f-144">Zadejte hello **název webové aplikace**, **plán služby App Service**, **skupiny prostředků**, a **oblast**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-144">Specify hello **Web App name**, **App Service plan**, **Resource group**, and **Region**, then click **Create**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. <span data-ttu-id="9e62f-145">Po dokončení vytváření projektu hello počkejte hello webové aplikace toobe vytvořené v Azure App Service, jak je uvedeno v hello **aktivita služby Azure App Service** okno.</span><span class="sxs-lookup"><span data-stu-id="9e62f-145">After hello project creation completes, wait for hello web app toobe created in Azure App Service as indicated in hello **Azure App Service Activity** window.</span></span> <span data-ttu-id="9e62f-146">Potom klikněte na **nyní publikování MyTaskListApp toothis webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-146">Then, click **Publish MyTaskListApp toothis Web App now**.</span></span>
7. <span data-ttu-id="9e62f-147">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-147">Click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    <span data-ttu-id="9e62f-148">Jakmile vaše aplikace ASP.NET výchozí publikované tooAzure App Service Web Apps, spustí se v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="9e62f-148">Once your default ASP.NET application is published tooAzure App Service Web Apps, it will be launched in hello browser.</span></span>

## <a name="install-hello-mongodb-c-driver"></a><span data-ttu-id="9e62f-149">Nainstalujte hello ovladač MongoDB C#</span><span class="sxs-lookup"><span data-stu-id="9e62f-149">Install hello MongoDB C# driver</span></span>
<span data-ttu-id="9e62f-150">MongoDB nabízí podporu na straně klienta pro C# aplikace prostřednictvím ovladače, které budete potřebovat tooinstall ve svém místním vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="9e62f-150">MongoDB offers client-side support for C# applications through a driver, which you need tooinstall on your local development computer.</span></span> <span data-ttu-id="9e62f-151">je k dispozici prostřednictvím balíčku NuGet Hello C# ovladač.</span><span class="sxs-lookup"><span data-stu-id="9e62f-151">hello C# driver is available through NuGet.</span></span>

<span data-ttu-id="9e62f-152">hello tooinstall MongoDB C# ovladače:</span><span class="sxs-lookup"><span data-stu-id="9e62f-152">tooinstall hello MongoDB C# driver:</span></span>

1. <span data-ttu-id="9e62f-153">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **MyTaskListApp** projektu a vyberte **spravovat NuGetPackages**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-153">In **Solution Explorer**, right-click hello **MyTaskListApp** project and select **Manage NuGetPackages**.</span></span>
   
    ![Správa balíčků NuGet][VS2013ManageNuGetPackages]
2. <span data-ttu-id="9e62f-155">V hello **spravovat balíčky NuGet** klikněte na okno, v levém podokně hello **Online**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-155">In hello **Manage NuGet Packages** window, in hello left pane, click **Online**.</span></span> <span data-ttu-id="9e62f-156">V hello **Online hledání** pole na hello správné, zadejte "mongodb.driver".</span><span class="sxs-lookup"><span data-stu-id="9e62f-156">In hello **Search Online** box on hello right, type "mongodb.driver".</span></span>  <span data-ttu-id="9e62f-157">Klikněte na tlačítko **nainstalovat** tooinstall hello ovladačů.</span><span class="sxs-lookup"><span data-stu-id="9e62f-157">Click **Install** tooinstall hello driver.</span></span>
   
    ![Vyhledejte MongoDB C# ovladačů][SearchforMongoDBCSharpDriver]
3. <span data-ttu-id="9e62f-159">Klikněte na tlačítko **souhlasím** tooaccept hello 10gen, Inc. licenční podmínky.</span><span class="sxs-lookup"><span data-stu-id="9e62f-159">Click **I Accept** tooaccept hello 10gen, Inc. license terms.</span></span>
4. <span data-ttu-id="9e62f-160">Klikněte na tlačítko **Zavřít** po nainstalován ovladač hello.</span><span class="sxs-lookup"><span data-stu-id="9e62f-160">Click **Close** after hello driver has installed.</span></span>
    <span data-ttu-id="9e62f-161">![MongoDB C# ovladač nainstalován][MongoDBCsharpDriverInstalled]</span><span class="sxs-lookup"><span data-stu-id="9e62f-161">![MongoDB C# Driver Installed][MongoDBCsharpDriverInstalled]</span></span>

<span data-ttu-id="9e62f-162">Nyní je nainstalována Hello MongoDB C# ovladačů.</span><span class="sxs-lookup"><span data-stu-id="9e62f-162">hello MongoDB C# driver is now installed.</span></span>  <span data-ttu-id="9e62f-163">Odkazy na toohello **MongoDB.Bson**, **MongoDB.Driver**, a **MongoDB.Driver.Core** knihovny přidané toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="9e62f-163">References toohello **MongoDB.Bson**, **MongoDB.Driver**, and **MongoDB.Driver.Core**  libraries have been added toohello project.</span></span>

![Odkazy na MongoDB C# ovladačů][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a><span data-ttu-id="9e62f-165">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="9e62f-165">Add a model</span></span>
<span data-ttu-id="9e62f-166">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello *modely* složky a **přidat** nový **– třída** a pojmenujte ji *TaskModel.cs* .</span><span class="sxs-lookup"><span data-stu-id="9e62f-166">In **Solution Explorer**, right-click hello *Models* folder and **Add** a new **Class** and name it *TaskModel.cs*.</span></span>  <span data-ttu-id="9e62f-167">V *TaskModel.cs*, nahraďte existující kód hello hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="9e62f-167">In *TaskModel.cs*, replace hello existing code with hello following code:</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MongoDB.Bson.Serialization.Attributes;
    using MongoDB.Bson.Serialization.IdGenerators;
    using MongoDB.Bson;

    namespace MyTaskListApp.Models
    {
        public class MyTask
        {
            [BsonId(IdGenerator = typeof(CombGuidGenerator))]
            public Guid Id { get; set; }

            [BsonElement("Name")]
            public string Name { get; set; }

            [BsonElement("Category")]
            public string Category { get; set; }

            [BsonElement("Date")]
            public DateTime Date { get; set; }

            [BsonElement("CreatedDate")]
            public DateTime CreatedDate { get; set; }

        }
    }

## <a name="add-hello-data-access-layer"></a><span data-ttu-id="9e62f-168">Přidat hello vrstva</span><span class="sxs-lookup"><span data-stu-id="9e62f-168">Add hello data access layer</span></span>
<span data-ttu-id="9e62f-169">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello *MyTaskListApp* projektu a **přidat** **novou složku** s názvem *DAL*.</span><span class="sxs-lookup"><span data-stu-id="9e62f-169">In **Solution Explorer**, right-click hello *MyTaskListApp* project and **Add** a **New Folder** named *DAL*.</span></span>  <span data-ttu-id="9e62f-170">Klikněte pravým tlačítkem na hello *DAL* složky a **přidat** nový **třída**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-170">Right-click hello *DAL* folder and **Add** a new **Class**.</span></span> <span data-ttu-id="9e62f-171">Název souboru třída hello *Dal.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e62f-171">Name hello class file *Dal.cs*.</span></span>  <span data-ttu-id="9e62f-172">V *Dal.cs*, nahraďte existující kód hello hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="9e62f-172">In *Dal.cs*, replace hello existing code with hello following code:</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;


    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            private MongoServer mongoServer = null;
            private bool disposed = false;

            // toodo: update hello connection string with hello DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net"
            private string connectionString = "mongodb://mongodbsrv20151211.cloudapp.net";

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
                MongoClient client = new MongoClient(connectionString);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }

            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
                MongoClient client = new MongoClient(connectionString);
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
                        if (mongoServer != null)
                        {
                            this.mongoServer.Disconnect();
                        }
                    }
                }

                this.disposed = true;
            }

            # endregion
        }
    }

## <a name="add-a-controller"></a><span data-ttu-id="9e62f-173">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="9e62f-173">Add a controller</span></span>
<span data-ttu-id="9e62f-174">Otevřete hello *Controllers\HomeController.cs* souboru v **Průzkumníku řešení** a nahraďte existující kód hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="9e62f-174">Open hello *Controllers\HomeController.cs* file in **Solution Explorer** and replace hello existing code with hello following:</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using MyTaskListApp.Models;
    using System.Configuration;

    namespace MyTaskListApp.Controllers
    {
        public class HomeController : Controller, IDisposable
        {
            private Dal dal = new Dal();
            private bool disposed = false;
            //
            // GET: /MyTask/

            public ActionResult Index()
            {
                return View(dal.GetAllTasks());
            }

            //
            // GET: /MyTask/Create

            public ActionResult Create()
            {
                return View();
            }

            //
            // POST: /MyTask/Create

            [HttpPost]
            public ActionResult Create(MyTask task)
            {
                try
                {
                    dal.CreateTask(task);
                    return RedirectToAction("Index");
                }
                catch
                {
                    return View();
                }
            }

            public ActionResult About()
            {
                return View();
            }

            # region IDisposable

            new protected void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }

            new protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                        this.dal.Dispose();
                    }
                }

                this.disposed = true;
            }

            # endregion

        }
    }

## <a name="set-up-hello-styles"></a><span data-ttu-id="9e62f-175">Nastavení stylů hello</span><span class="sxs-lookup"><span data-stu-id="9e62f-175">Set up hello styles</span></span>
<span data-ttu-id="9e62f-176">toochange hello nadpis v horní části hello hello stránky, otevřete hello *Views\Shared\\_Layout.cshtml* souboru v **Průzkumníku řešení** a nahraďte "Název aplikace" v záhlaví navigační panel hello "Moje úloh Seznam aplikací"tak, aby vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9e62f-176">toochange hello title at hello top of hello page, open hello *Views\Shared\\_Layout.cshtml* file in **Solution Explorer** and replace "Application name" in hello navbar header with "My Task List Application" so that it looks like this:</span></span>

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

<span data-ttu-id="9e62f-177">V pořadí tooset nabídka hello seznam úloh, otevřete hello *\Views\Home\Index.cshtml* a nahraďte existující kód hello hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="9e62f-177">In order tooset up hello Task List menu, open hello *\Views\Home\Index.cshtml* file and replace hello existing code with hello following code:</span></span>

    @model IEnumerable<MyTaskListApp.Models.MyTask>

    @{
        ViewBag.Title = "My Task List";
    }

    <h2>My Task List</h2>

    <table border="1">
        <tr>
            <th>Task</th>
            <th>Category</th>
            <th>Date</th>

        </tr>

    @foreach (var item in Model) {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Name)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Category)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Date)
            </td>

        </tr>
    }

    </table>
    <div>  @Html.Partial("Create", new MyTaskListApp.Models.MyTask())</div>


<span data-ttu-id="9e62f-178">tooadd hello možnost toocreate novou úlohu, klikněte pravým tlačítkem na hello *Views\Home\\*  složky a **přidat** **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-178">tooadd hello ability toocreate a new task, right-click hello *Views\Home\\* folder and **Add** a **View**.</span></span>  <span data-ttu-id="9e62f-179">Název zobrazení hello *vytvořit*.</span><span class="sxs-lookup"><span data-stu-id="9e62f-179">Name hello view *Create*.</span></span> <span data-ttu-id="9e62f-180">Nahraďte kód hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="9e62f-180">Replace hello code with hello following:</span></span>

    @model MyTaskListApp.Models.MyTask

    <script src="@Url.Content("~/Scripts/jquery-1.10.2.min.js")" type="text/javascript"></script>
    <script src="@Url.Content("~/Scripts/jquery.validate.min.js")" type="text/javascript"></script>
    <script src="@Url.Content("~/Scripts/jquery.validate.unobtrusive.min.js")" type="text/javascript"></script>

    @using (Html.BeginForm("Create", "Home")) {
        @Html.ValidationSummary(true)
        <fieldset>
            <legend>New Task</legend>

            <div class="editor-label">
                @Html.LabelFor(model => model.Name)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Name)
                @Html.ValidationMessageFor(model => model.Name)
            </div>

            <div class="editor-label">
                @Html.LabelFor(model => model.Category)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Category)
                @Html.ValidationMessageFor(model => model.Category)
            </div>

            <div class="editor-label">
                @Html.LabelFor(model => model.Date)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Date)
                @Html.ValidationMessageFor(model => model.Date)
            </div>

            <p>
                <input type="submit" value="Create" />
            </p>
        </fieldset>
    }

<span data-ttu-id="9e62f-181">**Průzkumník řešení** by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="9e62f-181">**Solution Explorer** should look like this:</span></span>

![Průzkumník řešení][SolutionExplorerMyTaskListApp]

## <a name="set-hello-mongodb-connection-string"></a><span data-ttu-id="9e62f-183">Nastavit hello MongoDB připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="9e62f-183">Set hello MongoDB connection string</span></span>
<span data-ttu-id="9e62f-184">V **Průzkumníku řešení**, otevřete hello *DAL/Dal.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="9e62f-184">In **Solution Explorer**, open hello *DAL/Dal.cs* file.</span></span> <span data-ttu-id="9e62f-185">Najde hello následující řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="9e62f-185">Find hello following line of code:</span></span>

    private string connectionString = "mongodb://<vm-dns-name>";

<span data-ttu-id="9e62f-186">Nahraďte `<vm-dns-name>` s názvem DNS hello hello virtuální počítač se systémem MongoDB, které jste vytvořili v hello [vytvořit virtuální počítač a nainstalujte MongoDB] [ Create a virtual machine and install MongoDB] krok tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9e62f-186">Replace `<vm-dns-name>` with hello DNS name of hello virtual machine running MongoDB you created in hello [Create a virtual machine and install MongoDB][Create a virtual machine and install MongoDB] step of this tutorial.</span></span>  <span data-ttu-id="9e62f-187">název DNS hello toofind virtuálního počítače, přejděte toohello portálu Azure, vyberte **virtuální počítače**a najít **název DNS**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-187">toofind hello DNS name of your virtual machine, go toohello Azure Portal, select **Virtual Machines**, and find **DNS Name**.</span></span>

<span data-ttu-id="9e62f-188">Pokud název DNS hello hello virtuálního počítače je "testlinuxvm.cloudapp.net" a MongoDB naslouchá na portu výchozí hello 27017, bude vypadat hello připojovací řetězec řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="9e62f-188">If hello DNS name of hello virtual machine is "testlinuxvm.cloudapp.net" and MongoDB is listening on hello default port 27017, hello connection string line of code will look like:</span></span>

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

<span data-ttu-id="9e62f-189">Pokud koncový bod virtuálního počítače hello určuje jiný externí port pro MongoDB, můžete zadejte hello port v hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="9e62f-189">If hello virtual machine endpoint specifies a different external port for MongoDB, you can specifiy hello port in hello connection string:</span></span>

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

<span data-ttu-id="9e62f-190">Další informace o MongoDB připojovacích řetězcích najdete v tématu [připojení][MongoConnectionStrings].</span><span class="sxs-lookup"><span data-stu-id="9e62f-190">For more information on MongoDB connection strings, see [Connections][MongoConnectionStrings].</span></span>

## <a name="test-hello-local-deployment"></a><span data-ttu-id="9e62f-191">Testovací nasazení pro místní hello</span><span class="sxs-lookup"><span data-stu-id="9e62f-191">Test hello local deployment</span></span>
<span data-ttu-id="9e62f-192">Vyberte aplikaci na vývojovém počítači, toorun **spustit ladění** z hello **ladění** nabídky nebo stiskněte klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-192">toorun your application on your development computer, select **Start Debugging** from hello **Debug** menu or hit **F5**.</span></span> <span data-ttu-id="9e62f-193">Služba IIS Express se spustí a v prohlížeči se otevře a spustí domovskou stránku hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e62f-193">IIS Express starts and a browser opens and launches hello application's home page.</span></span>  <span data-ttu-id="9e62f-194">Můžete přidat nové úlohy, které se přidají databázi MongoDB toohello běžící na virtuálním počítači v Azure.</span><span class="sxs-lookup"><span data-stu-id="9e62f-194">You can add a new task, which will be added toohello MongoDB database running on your virtual machine in Azure.</span></span>

![Moje aplikace seznamu úkolů][TaskListAppBlank]

## <a name="publish-tooazure-app-service-web-apps"></a><span data-ttu-id="9e62f-196">Publikování tooAzure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="9e62f-196">Publish tooAzure App Service Web Apps</span></span>
<span data-ttu-id="9e62f-197">V této části budete publikovat vaše změny tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="9e62f-197">In this section you will publish your changes tooAzure App Service Web Apps.</span></span>

1. <span data-ttu-id="9e62f-198">V Průzkumníku řešení klikněte pravým tlačítkem na **MyTaskListApp** znovu a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-198">In Solution Explorer, right-click **MyTaskListApp** again and click **Publish**.</span></span>
2. <span data-ttu-id="9e62f-199">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-199">Click **Publish**.</span></span>
   
    <span data-ttu-id="9e62f-200">Teď byste měli vidět vaší webové aplikace spuštěné v Azure App Service a přístup k databázi MongoDB hello v Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="9e62f-200">You should now see your web app running in Azure App Service and accessing hello MongoDB database in Azure Virtual Machines.</span></span>

## <a name="summary"></a><span data-ttu-id="9e62f-201">Souhrn</span><span class="sxs-lookup"><span data-stu-id="9e62f-201">Summary</span></span>
<span data-ttu-id="9e62f-202">Teď úspěšně jste nasadili vaší tooAzure aplikace ASP.NET App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="9e62f-202">You have now successfully deployed your ASP.NET application tooAzure App Service Web Apps.</span></span> <span data-ttu-id="9e62f-203">tooview hello webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="9e62f-203">tooview hello web app:</span></span>

1. <span data-ttu-id="9e62f-204">Přihlaste se k portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="9e62f-204">Log into hello Azure Portal.</span></span>
2. <span data-ttu-id="9e62f-205">Klikněte na tlačítko **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e62f-205">Click **Web apps**.</span></span> 
3. <span data-ttu-id="9e62f-206">Vyberte webové aplikace v hello **webové aplikace** seznamu.</span><span class="sxs-lookup"><span data-stu-id="9e62f-206">Select your web app in hello **Web Apps** list.</span></span>

<span data-ttu-id="9e62f-207">Další informace o vývoji aplikace C# pro MongoDB, najdete v části [CSharp jazyk Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="9e62f-207">For more information on developing C# applications against MongoDB, see [CSharp Language Center][MongoC#LangCenter].</span></span> 

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

<!-- HYPERLINKS -->

[AzurePortal]: http://manage.windowsazure.com
[WindowsAzure]: http://www.windowsazure.com
[MongoC#LangCenter]: http://docs.mongodb.org/ecosystem/drivers/csharp/
[MVCWebSite]: http://www.asp.net/mvc
[ASP.NET]: http://www.asp.net/
[MongoConnectionStrings]: http://www.mongodb.org/display/DOCS/Connections
[MongoDB]: http://www.mongodb.org
[InstallMongoOnWindowsVM]:../virtual-machines/windows/classic/install-mongodb.md
[VSEWeb]: http://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express
[VSUlt]: http://www.microsoft.com/visualstudio/eng/2013-downloads

<!-- IMAGES -->


[StartPageNewProject]: ./media/web-sites-dotnet-store-data-mongodb-vm/NewProject.png
[NewProjectMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/NewProjectMyTaskListApp.png
[VS2013SelectMVCTemplate]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013SelectMVCTemplate.png
[VS2013DefaultMVCApplication]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013DefaultMVCApplication.png
[VS2013ManageNuGetPackages]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013ManageNuGetPackages.png
[SearchforMongoDBCSharpDriver]: ./media/web-sites-dotnet-store-data-mongodb-vm/SearchforMongoDBCSharpDriver.png
[MongoDBCsharpDriverInstalled]: ./media/web-sites-dotnet-store-data-mongodb-vm/MongoDBCsharpDriverInstalled.png
[MongoDBCSharpDriverReferences]: ./media/web-sites-dotnet-store-data-mongodb-vm/MongoDBCSharpDriverReferences.png
[SolutionExplorerMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/SolutionExplorerMyTaskListApp.png
[TaskListAppBlank]: ./media/web-sites-dotnet-store-data-mongodb-vm/TaskListAppBlank.png
[WAWSCreateWebSite]: ./media/web-sites-dotnet-store-data-mongodb-vm/WAWSCreateWebSite.png
[WAWSDashboardMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/WAWSDashboardMyTaskListApp.png
[Image9]: ./media/web-sites-dotnet-store-data-mongodb-vm/RepoReady.png
[Image10]: ./media/web-sites-dotnet-store-data-mongodb-vm/GitInstructions.png
[Image11]: ./media/web-sites-dotnet-store-data-mongodb-vm/GitDeploymentComplete.png

<!-- TOC BOOKMARKS -->
[Create a virtual machine and install MongoDB]: #virtualmachine
[Create and run hello My Task List ASP.NET application on your development computer]: #createapp
[Create an Azure web site]: #createwebsite
[Deploy hello ASP.NET application toohello web site using Git]: #deployapp
