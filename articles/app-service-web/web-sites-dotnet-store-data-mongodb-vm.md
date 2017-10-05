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
# <a name="create-a-web-app-in-azure-that-connects-to-mongodb-running-on-a-virtual-machine"></a>Vytvoření webové aplikace v Azure, která se připojuje k MongoDB na virtuálním počítači
Pomocí Git, můžete nasadit aplikaci ASP.NET do Azure App Service Web Apps. V tomto kurzu vytvoříte jednoduchý front-endové rozhraní ASP.NET MVC aplikaci seznamu úkolů, která se připojuje k databázi MongoDB spuštěny na virtuálním počítači v Azure.  [MongoDB] [ MongoDB] je populární open source, vysoký výkon databáze NoSQL. Po spuštění a testování aplikace ASP.NET ve svém vývojovém počítači, bude nahrávat aplikace App Service Web Apps pomocí Git.

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="background-knowledge"></a>Znalostní báze pozadí
V tomto kurzu, přestože není povinný je užitečné znalosti o následující:

* C# ovladačů pro MongoDB. Další informace o vývoji aplikace C# pro MongoDB, najdete v článku MongoDB [CSharp jazyk Center][MongoC#LangCenter]. 
* Rozhraní ASP .NET webové aplikace. Se dozvíte v [webu ASP.net][ASP.NET].
* Architektura webových aplikací ASP .NET MVC. Se dozvíte v [webové stránky ASP.NET MVC][MVCWebSite].
* Azure. Abyste mohli začít, čtení v [Azure][WindowsAzure].

## <a name="prerequisites"></a>Požadavky
* [Visual Studio Express 2013 pro Web] [ VSEWeb] nebo [Visual Studio 2013][VSUlt]
* [Azure SDK pro .NET](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* Aktivní předplatné Microsoft Azure

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a>Vytvoření virtuálního počítače a nainstalujte MongoDB
Tento kurz předpokládá, že jste vytvořili virtuální počítač v Azure. Po vytvoření virtuálního počítače je potřeba na virtuálním počítači nainstalujte MongoDB:

* Vytvoření virtuálního počítače s Windows a nainstalujte MongoDB, přečtěte si téma [nainstalujte MongoDB ve virtuálním počítači s Windows serverem v Azure][InstallMongoOnWindowsVM].

Po vytvoření virtuálního počítače v Azure a nainstalován MongoDB, je nutné pamatovat název DNS virtuálního počítače ("testlinuxvm.cloudapp.net", např.) a externí port pro MongoDB, který jste zadali v koncovém bodě.  Tyto informace později v tomto kurzu budete potřebovat.

<a id="createapp"></a>

## <a name="create-the-application"></a>Vytvoření aplikace
V této části vytvoříte aplikaci ASP.NET s názvem "Moje seznam úkolů" pomocí sady Visual Studio a provedení počátečního nasazení do Azure App Service Web Apps. Spustí aplikaci místně, ale bude připojení k virtuálnímu počítači na platformě Azure a pomocí instance MongoDB, kterou jste vytvořili existuje.

1. V sadě Visual Studio, klikněte na tlačítko **nový projekt**.
   
    ![Spuštění nového projektu stránky][StartPageNewProject]
2. V **nový projekt** okno, v levém podokně vyberte **Visual C#**a potom vyberte **webové**. V prostředním podokně vyberte **webové aplikace ASP.NET**. V dolní části, pojmenujte svůj projekt "MyTaskListApp" a pak klikněte na **OK**.
   
    ![Dialogové okno Nový projekt][NewProjectMyTaskListApp]
3. V **nový projekt ASP.NET** dialogové okno, vyberte **MVC**a potom klikněte na **OK**.
   
    ![Vyberte šablonu MVC][VS2013SelectMVCTemplate]
4. Pokud už nejste do Microsoft Azure, budete vyzváni k přihlášení. Postupujte podle pokynů pro přihlášení do Azure.
5. Jakmile se přihlásíte, můžete začít konfigurace webové aplikace služby App Service. Zadejte **název webové aplikace**, **plán služby App Service**, **skupiny prostředků**, a **oblast**, pak klikněte na tlačítko **vytvořit**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. Po vytvoření projektu webové aplikace, které mají být vytvořeny ve službě Azure App Service, jak je uvedeno v počkejte **aktivita služby Azure App Service** okno. Potom klikněte na **MyTaskListApp publikovat do této webové aplikace teď**.
7. Klikněte na **Publikovat**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    Jakmile výchozí aplikace ASP.NET je publikována do Azure App Service Web Apps, spustí se v prohlížeči.

## <a name="install-the-mongodb-c-driver"></a>Nainstalujte ovladač MongoDB C#
MongoDB nabízí podporu na straně klienta pro C# aplikace prostřednictvím ovladače, které je potřeba nainstalovat na svém místním vývojovém počítači. Je k dispozici prostřednictvím balíčku NuGet ovladač C#.

K instalaci ovladačů MongoDB C#:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **MyTaskListApp** projektu a vyberte **spravovat NuGetPackages**.
   
    ![Správa balíčků NuGet][VS2013ManageNuGetPackages]
2. V **spravovat balíčky NuGet** okno, v levém podokně klikněte na tlačítko **Online**. V **Online hledání** pole na pravé straně, zadejte "mongodb.driver".  Klikněte na tlačítko **nainstalovat** instalace ovladače.
   
    ![Vyhledejte MongoDB C# ovladačů][SearchforMongoDBCSharpDriver]
3. Klikněte na tlačítko **souhlasím** tak, aby přijímal 10gen, Inc. licenční podmínky.
4. Klikněte na tlačítko **Zavřít** po nainstaloval ovladač.
    ![MongoDB C# ovladač nainstalován][MongoDBCsharpDriverInstalled]

Nyní je nainstalován ovladač MongoDB C#.  Odkazuje na **MongoDB.Bson**, **MongoDB.Driver**, a **MongoDB.Driver.Core** projektu přidané knihovny.

![Odkazy na MongoDB C# ovladačů][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a>Přidání modelu
V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *modely* složky a **přidat** nový **– třída** a pojmenujte ji *TaskModel.cs*.  V *TaskModel.cs*, existujícího kódu nahraďte následujícím kódem:

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

## <a name="add-the-data-access-layer"></a>Přidat vrstva přístupu k datům
V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *MyTaskListApp* projektu a **přidat** **novou složku** s názvem *DAL*.  Klikněte pravým tlačítkem myši *DAL* složky a **přidat** nový **třída**. Název souboru třídy *Dal.cs*.  V *Dal.cs*, existujícího kódu nahraďte následujícím kódem:

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

## <a name="add-a-controller"></a>Přidání kontroleru
Otevřete *Controllers\HomeController.cs* souboru v **Průzkumníku řešení** a existujícího kódu nahraďte následujícím kódem:

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

## <a name="set-up-the-styles"></a>Nastavení stylů
Chcete-li změnit název v horní části stránky, otevřete *Views\Shared\\_Layout.cshtml* souboru v **Průzkumníku řešení** a nahraďte "Název aplikace" v hlavičce navigační panel "Moje seznamu úloh Aplikace"tak, že to vypadá, například tato:

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

Pokud chcete nastavit v nabídce seznam úloh, otevřete *\Views\Home\Index.cshtml* a existujícího kódu nahraďte následujícím kódem:

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


Chcete-li přidat možnost vytvořit novou úlohu, klikněte pravým tlačítkem *Views\Home\\*  složky a **přidat** **zobrazení**.  Název zobrazení *vytvořit*. Nahraďte kód tímto:

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

**Průzkumník řešení** by měla vypadat takto:

![Průzkumník řešení][SolutionExplorerMyTaskListApp]

## <a name="set-the-mongodb-connection-string"></a>Nastavení připojovacího řetězce MongoDB
V **Průzkumníku řešení**, otevřete *DAL/Dal.cs* souboru. Vyhledejte následující řádek kódu:

    private string connectionString = "mongodb://<vm-dns-name>";

Nahraďte `<vm-dns-name>` s DNS názvem virtuálního počítače spuštěného MongoDB, které jste vytvořili v [vytvořit virtuální počítač a nainstalujte MongoDB] [ Create a virtual machine and install MongoDB] krok tohoto kurzu.  Chcete-li najít název DNS virtuálního počítače, přejděte na portálu Azure, vyberte **virtuální počítače**a najít **název DNS**.

Pokud název DNS virtuálního počítače je "testlinuxvm.cloudapp.net" a MongoDB naslouchá na výchozím portu 27017, bude vypadat připojovací řetězec řádek kódu:

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

Pokud koncový bod virtuálního počítače určuje jiný externí port pro MongoDB, můžete zadejte port v připojovacím řetězci:

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

Další informace o MongoDB připojovacích řetězcích najdete v tématu [připojení][MongoConnectionStrings].

## <a name="test-the-local-deployment"></a>Testování místní nasazení
Chcete-li spustit aplikaci na vývojovém počítači, vyberte **spustit ladění** z **ladění** nabídky nebo stiskněte klávesu **F5**. Služba IIS Express se spustí a v prohlížeči se otevře a spustí na domovskou stránku aplikace.  Můžete přidat nové úlohy, které budou přidány do databáze MongoDB spuštěný ve virtuálním počítači v Azure.

![Moje aplikace seznamu úkolů][TaskListAppBlank]

## <a name="publish-to-azure-app-service-web-apps"></a>Publikování do služby Azure App Service Web Apps
V této části publikujete změny do Azure App Service Web Apps.

1. V Průzkumníku řešení klikněte pravým tlačítkem na **MyTaskListApp** znovu a klikněte na tlačítko **publikovat**.
2. Klikněte na **Publikovat**.
   
    Teď byste měli vidět vaší webové aplikace spuštěné v Azure App Service a přístup k databázi MongoDB v Azure Virtual Machines.

## <a name="summary"></a>Souhrn
Teď úspěšně jste nasadili aplikace ASP.NET Azure App Service Web Apps. Chcete-li zobrazit webové aplikace:

1. Přihlaste se k portálu Azure.
2. Klikněte na tlačítko **webové aplikace**. 
3. Vyberte webové aplikace v **webové aplikace** seznamu.

Další informace o vývoji aplikace C# pro MongoDB, najdete v části [CSharp jazyk Center][MongoC#LangCenter]. 

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
