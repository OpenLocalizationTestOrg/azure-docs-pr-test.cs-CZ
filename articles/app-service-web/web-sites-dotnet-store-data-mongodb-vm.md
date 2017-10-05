---
title: "Vytvoření webové aplikace v Azure, která se připojuje k MongoDB na virtuálním počítači"
description: "Kurz, který se naučíte, jak pomocí Git Nasaďte aplikaci ASP.NET do služby Azure App Service, připojený k MongoDB na virtuální počítač Azure."
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
ms.openlocfilehash: a3f289ed9c764d0859573de4f834e042d0f103c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-in-azure-that-connects-to-mongodb-running-on-a-virtual-machine"></a><span data-ttu-id="8930f-103">Vytvoření webové aplikace v Azure, která se připojuje k MongoDB na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="8930f-103">Create a web app in Azure that connects to MongoDB running on a virtual machine</span></span>
<span data-ttu-id="8930f-104">Pomocí Git, můžete nasadit aplikaci ASP.NET do Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="8930f-104">Using Git, you can deploy an ASP.NET application to Azure App Service Web Apps.</span></span> <span data-ttu-id="8930f-105">V tomto kurzu vytvoříte jednoduchý front-endové rozhraní ASP.NET MVC aplikaci seznamu úkolů, která se připojuje k databázi MongoDB spuštěny na virtuálním počítači v Azure.</span><span class="sxs-lookup"><span data-stu-id="8930f-105">In this tutorial, you will build a simple front-end ASP.NET MVC task list application that connects to a MongoDB database running on a virtual machine in Azure.</span></span>  <span data-ttu-id="8930f-106">[MongoDB] [ MongoDB] je populární open source, vysoký výkon databáze NoSQL.</span><span class="sxs-lookup"><span data-stu-id="8930f-106">[MongoDB][MongoDB] is a popular open source, high performance NoSQL database.</span></span> <span data-ttu-id="8930f-107">Po spuštění a testování aplikace ASP.NET ve svém vývojovém počítači, bude nahrávat aplikace App Service Web Apps pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="8930f-107">After running and testing the ASP.NET application on your development computer, you will upload the application to App Service Web Apps using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="8930f-108">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8930f-108">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="8930f-109">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="8930f-109">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="background-knowledge"></a><span data-ttu-id="8930f-110">Znalostní báze pozadí</span><span class="sxs-lookup"><span data-stu-id="8930f-110">Background knowledge</span></span>
<span data-ttu-id="8930f-111">V tomto kurzu, přestože není povinný je užitečné znalosti o následující:</span><span class="sxs-lookup"><span data-stu-id="8930f-111">Knowledge of the following is useful for this tutorial, though not required:</span></span>

* <span data-ttu-id="8930f-112">C# ovladačů pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8930f-112">The C# driver for MongoDB.</span></span> <span data-ttu-id="8930f-113">Další informace o vývoji aplikace C# pro MongoDB, najdete v článku MongoDB [CSharp jazyk Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="8930f-113">For more information on developing C# applications against MongoDB, see the MongoDB [CSharp Language Center][MongoC#LangCenter].</span></span> 
* <span data-ttu-id="8930f-114">Rozhraní ASP .NET webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8930f-114">The ASP .NET web application framework.</span></span> <span data-ttu-id="8930f-115">Se dozvíte v [webu ASP.net][ASP.NET].</span><span class="sxs-lookup"><span data-stu-id="8930f-115">You can learn all about it at the [ASP.net website][ASP.NET].</span></span>
* <span data-ttu-id="8930f-116">Architektura webových aplikací ASP .NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8930f-116">The ASP .NET MVC web application framework.</span></span> <span data-ttu-id="8930f-117">Se dozvíte v [webové stránky ASP.NET MVC][MVCWebSite].</span><span class="sxs-lookup"><span data-stu-id="8930f-117">You can learn all about it at the [ASP.NET MVC website][MVCWebSite].</span></span>
* <span data-ttu-id="8930f-118">Azure.</span><span class="sxs-lookup"><span data-stu-id="8930f-118">Azure.</span></span> <span data-ttu-id="8930f-119">Abyste mohli začít, čtení v [Azure][WindowsAzure].</span><span class="sxs-lookup"><span data-stu-id="8930f-119">You can get started reading at [Azure][WindowsAzure].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8930f-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8930f-120">Prerequisites</span></span>
* <span data-ttu-id="8930f-121">[Visual Studio Express 2013 pro Web] [ VSEWeb] nebo [Visual Studio 2013][VSUlt]</span><span class="sxs-lookup"><span data-stu-id="8930f-121">[Visual Studio Express 2013 for Web][VSEWeb] or [Visual Studio 2013][VSUlt]</span></span>
* [<span data-ttu-id="8930f-122">Azure SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="8930f-122">Azure SDK for .NET</span></span>](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* <span data-ttu-id="8930f-123">Aktivní předplatné Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="8930f-123">An active Microsoft Azure subscription</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a><span data-ttu-id="8930f-124">Vytvoření virtuálního počítače a nainstalujte MongoDB</span><span class="sxs-lookup"><span data-stu-id="8930f-124">Create a virtual machine and install MongoDB</span></span>
<span data-ttu-id="8930f-125">Tento kurz předpokládá, že jste vytvořili virtuální počítač v Azure.</span><span class="sxs-lookup"><span data-stu-id="8930f-125">This tutorial assumes you have created a virtual machine in Azure.</span></span> <span data-ttu-id="8930f-126">Po vytvoření virtuálního počítače je potřeba na virtuálním počítači nainstalujte MongoDB:</span><span class="sxs-lookup"><span data-stu-id="8930f-126">After creating the virtual machine you need to install MongoDB on the virtual machine:</span></span>

* <span data-ttu-id="8930f-127">Vytvoření virtuálního počítače s Windows a nainstalujte MongoDB, přečtěte si téma [nainstalujte MongoDB ve virtuálním počítači s Windows serverem v Azure][InstallMongoOnWindowsVM].</span><span class="sxs-lookup"><span data-stu-id="8930f-127">To create a Windows virtual machine and install MongoDB, see [Install MongoDB on a virtual machine running Windows Server in Azure][InstallMongoOnWindowsVM].</span></span>

<span data-ttu-id="8930f-128">Po vytvoření virtuálního počítače v Azure a nainstalován MongoDB, je nutné pamatovat název DNS virtuálního počítače ("testlinuxvm.cloudapp.net", např.) a externí port pro MongoDB, který jste zadali v koncovém bodě.</span><span class="sxs-lookup"><span data-stu-id="8930f-128">After you have created the virtual machine in Azure and installed MongoDB, be sure to remember the DNS name of the virtual machine ("testlinuxvm.cloudapp.net", for example) and the external port for MongoDB that you specified in the endpoint.</span></span>  <span data-ttu-id="8930f-129">Tyto informace později v tomto kurzu budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="8930f-129">You will need this information later in the tutorial.</span></span>

<a id="createapp"></a>

## <a name="create-the-application"></a><span data-ttu-id="8930f-130">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="8930f-130">Create the application</span></span>
<span data-ttu-id="8930f-131">V této části vytvoříte aplikaci ASP.NET s názvem "Moje seznam úkolů" pomocí sady Visual Studio a provedení počátečního nasazení do Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="8930f-131">In this section you will create an ASP.NET application called "My Task List" by using Visual Studio and perform an initial deployment to Azure App Service Web Apps.</span></span> <span data-ttu-id="8930f-132">Spustí aplikaci místně, ale bude připojení k virtuálnímu počítači na platformě Azure a pomocí instance MongoDB, kterou jste vytvořili existuje.</span><span class="sxs-lookup"><span data-stu-id="8930f-132">You will run the application locally, but it will connect to your virtual machine on Azure and use the MongoDB instance that you created there.</span></span>

1. <span data-ttu-id="8930f-133">V sadě Visual Studio, klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="8930f-133">In Visual Studio, click **New Project**.</span></span>
   
    ![Spuštění nového projektu stránky][StartPageNewProject]
2. <span data-ttu-id="8930f-135">V **nový projekt** okno, v levém podokně vyberte **Visual C#**a potom vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="8930f-135">In the **New Project** window, in the left pane, select **Visual C#**, and then select **Web**.</span></span> <span data-ttu-id="8930f-136">V prostředním podokně vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="8930f-136">In the middle pane, select **ASP.NET  Web Application**.</span></span> <span data-ttu-id="8930f-137">V dolní části, pojmenujte svůj projekt "MyTaskListApp" a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8930f-137">At the bottom, name your project "MyTaskListApp," and then click **OK**.</span></span>
   
    ![Dialogové okno Nový projekt][NewProjectMyTaskListApp]
3. <span data-ttu-id="8930f-139">V **nový projekt ASP.NET** dialogové okno, vyberte **MVC**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8930f-139">In the **New ASP.NET Project** dialog box, select **MVC**, and then click **OK**.</span></span>
   
    ![Vyberte šablonu MVC][VS2013SelectMVCTemplate]
4. <span data-ttu-id="8930f-141">Pokud už nejste do Microsoft Azure, budete vyzváni k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8930f-141">If you aren't already signed into Microsoft Azure, you will be prompted to sign in.</span></span> <span data-ttu-id="8930f-142">Postupujte podle pokynů pro přihlášení do Azure.</span><span class="sxs-lookup"><span data-stu-id="8930f-142">Follow the prompts to sign into Azure.</span></span>
5. <span data-ttu-id="8930f-143">Jakmile se přihlásíte, můžete začít konfigurace webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="8930f-143">Once you are signed in, you can start configuring your App Service web app.</span></span> <span data-ttu-id="8930f-144">Zadejte **název webové aplikace**, **plán služby App Service**, **skupiny prostředků**, a **oblast**, pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8930f-144">Specify the **Web App name**, **App Service plan**, **Resource group**, and **Region**, then click **Create**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. <span data-ttu-id="8930f-145">Po vytvoření projektu webové aplikace, které mají být vytvořeny ve službě Azure App Service, jak je uvedeno v počkejte **aktivita služby Azure App Service** okno.</span><span class="sxs-lookup"><span data-stu-id="8930f-145">After the project creation completes, wait for the web app to be created in Azure App Service as indicated in the **Azure App Service Activity** window.</span></span> <span data-ttu-id="8930f-146">Potom klikněte na **MyTaskListApp publikovat do této webové aplikace teď**.</span><span class="sxs-lookup"><span data-stu-id="8930f-146">Then, click **Publish MyTaskListApp to this Web App now**.</span></span>
7. <span data-ttu-id="8930f-147">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8930f-147">Click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    <span data-ttu-id="8930f-148">Jakmile výchozí aplikace ASP.NET je publikována do Azure App Service Web Apps, spustí se v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="8930f-148">Once your default ASP.NET application is published to Azure App Service Web Apps, it will be launched in the browser.</span></span>

## <a name="install-the-mongodb-c-driver"></a><span data-ttu-id="8930f-149">Nainstalujte ovladač MongoDB C#</span><span class="sxs-lookup"><span data-stu-id="8930f-149">Install the MongoDB C# driver</span></span>
<span data-ttu-id="8930f-150">MongoDB nabízí podporu na straně klienta pro C# aplikace prostřednictvím ovladače, které je potřeba nainstalovat na svém místním vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="8930f-150">MongoDB offers client-side support for C# applications through a driver, which you need to install on your local development computer.</span></span> <span data-ttu-id="8930f-151">Je k dispozici prostřednictvím balíčku NuGet ovladač C#.</span><span class="sxs-lookup"><span data-stu-id="8930f-151">The C# driver is available through NuGet.</span></span>

<span data-ttu-id="8930f-152">K instalaci ovladačů MongoDB C#:</span><span class="sxs-lookup"><span data-stu-id="8930f-152">To install the MongoDB C# driver:</span></span>

1. <span data-ttu-id="8930f-153">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **MyTaskListApp** projektu a vyberte **spravovat NuGetPackages**.</span><span class="sxs-lookup"><span data-stu-id="8930f-153">In **Solution Explorer**, right-click the **MyTaskListApp** project and select **Manage NuGetPackages**.</span></span>
   
    ![Správa balíčků NuGet][VS2013ManageNuGetPackages]
2. <span data-ttu-id="8930f-155">V **spravovat balíčky NuGet** okno, v levém podokně klikněte na tlačítko **Online**.</span><span class="sxs-lookup"><span data-stu-id="8930f-155">In the **Manage NuGet Packages** window, in the left pane, click **Online**.</span></span> <span data-ttu-id="8930f-156">V **Online hledání** pole na pravé straně, zadejte "mongodb.driver".</span><span class="sxs-lookup"><span data-stu-id="8930f-156">In the **Search Online** box on the right, type "mongodb.driver".</span></span>  <span data-ttu-id="8930f-157">Klikněte na tlačítko **nainstalovat** instalace ovladače.</span><span class="sxs-lookup"><span data-stu-id="8930f-157">Click **Install** to install the driver.</span></span>
   
    ![Vyhledejte MongoDB C# ovladačů][SearchforMongoDBCSharpDriver]
3. <span data-ttu-id="8930f-159">Klikněte na tlačítko **souhlasím** tak, aby přijímal 10gen, Inc. licenční podmínky.</span><span class="sxs-lookup"><span data-stu-id="8930f-159">Click **I Accept** to accept the 10gen, Inc. license terms.</span></span>
4. <span data-ttu-id="8930f-160">Klikněte na tlačítko **Zavřít** po nainstaloval ovladač.</span><span class="sxs-lookup"><span data-stu-id="8930f-160">Click **Close** after the driver has installed.</span></span>
    <span data-ttu-id="8930f-161">![MongoDB C# ovladač nainstalován][MongoDBCsharpDriverInstalled]</span><span class="sxs-lookup"><span data-stu-id="8930f-161">![MongoDB C# Driver Installed][MongoDBCsharpDriverInstalled]</span></span>

<span data-ttu-id="8930f-162">Nyní je nainstalován ovladač MongoDB C#.</span><span class="sxs-lookup"><span data-stu-id="8930f-162">The MongoDB C# driver is now installed.</span></span>  <span data-ttu-id="8930f-163">Odkazuje na **MongoDB.Bson**, **MongoDB.Driver**, a **MongoDB.Driver.Core** projektu přidané knihovny.</span><span class="sxs-lookup"><span data-stu-id="8930f-163">References to the **MongoDB.Bson**, **MongoDB.Driver**, and **MongoDB.Driver.Core**  libraries have been added to the project.</span></span>

![Odkazy na MongoDB C# ovladačů][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a><span data-ttu-id="8930f-165">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="8930f-165">Add a model</span></span>
<span data-ttu-id="8930f-166">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *modely* složky a **přidat** nový **– třída** a pojmenujte ji *TaskModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="8930f-166">In **Solution Explorer**, right-click the *Models* folder and **Add** a new **Class** and name it *TaskModel.cs*.</span></span>  <span data-ttu-id="8930f-167">V *TaskModel.cs*, existujícího kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="8930f-167">In *TaskModel.cs*, replace the existing code with the following code:</span></span>

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

## <a name="add-the-data-access-layer"></a><span data-ttu-id="8930f-168">Přidat vrstva přístupu k datům</span><span class="sxs-lookup"><span data-stu-id="8930f-168">Add the data access layer</span></span>
<span data-ttu-id="8930f-169">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *MyTaskListApp* projektu a **přidat** **novou složku** s názvem *DAL*.</span><span class="sxs-lookup"><span data-stu-id="8930f-169">In **Solution Explorer**, right-click the *MyTaskListApp* project and **Add** a **New Folder** named *DAL*.</span></span>  <span data-ttu-id="8930f-170">Klikněte pravým tlačítkem myši *DAL* složky a **přidat** nový **třída**.</span><span class="sxs-lookup"><span data-stu-id="8930f-170">Right-click the *DAL* folder and **Add** a new **Class**.</span></span> <span data-ttu-id="8930f-171">Název souboru třídy *Dal.cs*.</span><span class="sxs-lookup"><span data-stu-id="8930f-171">Name the class file *Dal.cs*.</span></span>  <span data-ttu-id="8930f-172">V *Dal.cs*, existujícího kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="8930f-172">In *Dal.cs*, replace the existing code with the following code:</span></span>

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

            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net"
            private string connectionString = "mongodb://mongodbsrv20151211.cloudapp.net";

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

## <a name="add-a-controller"></a><span data-ttu-id="8930f-173">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="8930f-173">Add a controller</span></span>
<span data-ttu-id="8930f-174">Otevřete *Controllers\HomeController.cs* souboru v **Průzkumníku řešení** a existujícího kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="8930f-174">Open the *Controllers\HomeController.cs* file in **Solution Explorer** and replace the existing code with the following:</span></span>

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

## <a name="set-up-the-styles"></a><span data-ttu-id="8930f-175">Nastavení stylů</span><span class="sxs-lookup"><span data-stu-id="8930f-175">Set up the styles</span></span>
<span data-ttu-id="8930f-176">Chcete-li změnit název v horní části stránky, otevřete *Views\Shared\\_Layout.cshtml* souboru v **Průzkumníku řešení** a nahraďte "Název aplikace" v hlavičce navigační panel "Moje seznamu úloh Aplikace"tak, že to vypadá, například tato:</span><span class="sxs-lookup"><span data-stu-id="8930f-176">To change the title at the top of the page, open the *Views\Shared\\_Layout.cshtml* file in **Solution Explorer** and replace "Application name" in the navbar header with "My Task List Application" so that it looks like this:</span></span>

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

<span data-ttu-id="8930f-177">Pokud chcete nastavit v nabídce seznam úloh, otevřete *\Views\Home\Index.cshtml* a existujícího kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="8930f-177">In order to set up the Task List menu, open the *\Views\Home\Index.cshtml* file and replace the existing code with the following code:</span></span>

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


<span data-ttu-id="8930f-178">Chcete-li přidat možnost vytvořit novou úlohu, klikněte pravým tlačítkem *Views\Home\\*  složky a **přidat** **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="8930f-178">To add the ability to create a new task, right-click the *Views\Home\\* folder and **Add** a **View**.</span></span>  <span data-ttu-id="8930f-179">Název zobrazení *vytvořit*.</span><span class="sxs-lookup"><span data-stu-id="8930f-179">Name the view *Create*.</span></span> <span data-ttu-id="8930f-180">Nahraďte kód tímto:</span><span class="sxs-lookup"><span data-stu-id="8930f-180">Replace the code with the following:</span></span>

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

<span data-ttu-id="8930f-181">**Průzkumník řešení** by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="8930f-181">**Solution Explorer** should look like this:</span></span>

![Průzkumník řešení][SolutionExplorerMyTaskListApp]

## <a name="set-the-mongodb-connection-string"></a><span data-ttu-id="8930f-183">Nastavení připojovacího řetězce MongoDB</span><span class="sxs-lookup"><span data-stu-id="8930f-183">Set the MongoDB connection string</span></span>
<span data-ttu-id="8930f-184">V **Průzkumníku řešení**, otevřete *DAL/Dal.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="8930f-184">In **Solution Explorer**, open the *DAL/Dal.cs* file.</span></span> <span data-ttu-id="8930f-185">Vyhledejte následující řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="8930f-185">Find the following line of code:</span></span>

    private string connectionString = "mongodb://<vm-dns-name>";

<span data-ttu-id="8930f-186">Nahraďte `<vm-dns-name>` s DNS názvem virtuálního počítače spuštěného MongoDB, které jste vytvořili v [vytvořit virtuální počítač a nainstalujte MongoDB] [ Create a virtual machine and install MongoDB] krok tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8930f-186">Replace `<vm-dns-name>` with the DNS name of the virtual machine running MongoDB you created in the [Create a virtual machine and install MongoDB][Create a virtual machine and install MongoDB] step of this tutorial.</span></span>  <span data-ttu-id="8930f-187">Chcete-li najít název DNS virtuálního počítače, přejděte na portálu Azure, vyberte **virtuální počítače**a najít **název DNS**.</span><span class="sxs-lookup"><span data-stu-id="8930f-187">To find the DNS name of your virtual machine, go to the Azure Portal, select **Virtual Machines**, and find **DNS Name**.</span></span>

<span data-ttu-id="8930f-188">Pokud název DNS virtuálního počítače je "testlinuxvm.cloudapp.net" a MongoDB naslouchá na výchozím portu 27017, bude vypadat připojovací řetězec řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="8930f-188">If the DNS name of the virtual machine is "testlinuxvm.cloudapp.net" and MongoDB is listening on the default port 27017, the connection string line of code will look like:</span></span>

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

<span data-ttu-id="8930f-189">Pokud koncový bod virtuálního počítače určuje jiný externí port pro MongoDB, můžete zadejte port v připojovacím řetězci:</span><span class="sxs-lookup"><span data-stu-id="8930f-189">If the virtual machine endpoint specifies a different external port for MongoDB, you can specifiy the port in the connection string:</span></span>

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

<span data-ttu-id="8930f-190">Další informace o MongoDB připojovacích řetězcích najdete v tématu [připojení][MongoConnectionStrings].</span><span class="sxs-lookup"><span data-stu-id="8930f-190">For more information on MongoDB connection strings, see [Connections][MongoConnectionStrings].</span></span>

## <a name="test-the-local-deployment"></a><span data-ttu-id="8930f-191">Testování místní nasazení</span><span class="sxs-lookup"><span data-stu-id="8930f-191">Test the local deployment</span></span>
<span data-ttu-id="8930f-192">Chcete-li spustit aplikaci na vývojovém počítači, vyberte **spustit ladění** z **ladění** nabídky nebo stiskněte klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="8930f-192">To run your application on your development computer, select **Start Debugging** from the **Debug** menu or hit **F5**.</span></span> <span data-ttu-id="8930f-193">Služba IIS Express se spustí a v prohlížeči se otevře a spustí na domovskou stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="8930f-193">IIS Express starts and a browser opens and launches the application's home page.</span></span>  <span data-ttu-id="8930f-194">Můžete přidat nové úlohy, které budou přidány do databáze MongoDB spuštěný ve virtuálním počítači v Azure.</span><span class="sxs-lookup"><span data-stu-id="8930f-194">You can add a new task, which will be added to the MongoDB database running on your virtual machine in Azure.</span></span>

![Moje aplikace seznamu úkolů][TaskListAppBlank]

## <a name="publish-to-azure-app-service-web-apps"></a><span data-ttu-id="8930f-196">Publikování do služby Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="8930f-196">Publish to Azure App Service Web Apps</span></span>
<span data-ttu-id="8930f-197">V této části publikujete změny do Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="8930f-197">In this section you will publish your changes to Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="8930f-198">V Průzkumníku řešení klikněte pravým tlačítkem na **MyTaskListApp** znovu a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8930f-198">In Solution Explorer, right-click **MyTaskListApp** again and click **Publish**.</span></span>
2. <span data-ttu-id="8930f-199">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="8930f-199">Click **Publish**.</span></span>
   
    <span data-ttu-id="8930f-200">Teď byste měli vidět vaší webové aplikace spuštěné v Azure App Service a přístup k databázi MongoDB v Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="8930f-200">You should now see your web app running in Azure App Service and accessing the MongoDB database in Azure Virtual Machines.</span></span>

## <a name="summary"></a><span data-ttu-id="8930f-201">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8930f-201">Summary</span></span>
<span data-ttu-id="8930f-202">Teď úspěšně jste nasadili aplikace ASP.NET Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="8930f-202">You have now successfully deployed your ASP.NET application to Azure App Service Web Apps.</span></span> <span data-ttu-id="8930f-203">Chcete-li zobrazit webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="8930f-203">To view the web app:</span></span>

1. <span data-ttu-id="8930f-204">Přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8930f-204">Log into the Azure Portal.</span></span>
2. <span data-ttu-id="8930f-205">Klikněte na tlačítko **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8930f-205">Click **Web apps**.</span></span> 
3. <span data-ttu-id="8930f-206">Vyberte webové aplikace v **webové aplikace** seznamu.</span><span class="sxs-lookup"><span data-stu-id="8930f-206">Select your web app in the **Web Apps** list.</span></span>

<span data-ttu-id="8930f-207">Další informace o vývoji aplikace C# pro MongoDB, najdete v části [CSharp jazyk Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="8930f-207">For more information on developing C# applications against MongoDB, see [CSharp Language Center][MongoC#LangCenter].</span></span> 

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
[Create and run the My Task List ASP.NET application on your development computer]: #createapp
[Create an Azure web site]: #createwebsite
[Deploy the ASP.NET application to the web site using Git]: #deployapp
