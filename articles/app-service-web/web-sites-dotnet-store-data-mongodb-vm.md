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
# <a name="create-a-web-app-in-azure-that-connects-toomongodb-running-on-a-virtual-machine"></a>Vytvoření webové aplikace v Azure, která se připojuje tooMongoDB spuštěny na virtuálním počítači
Pomocí Git, můžete nasadit tooAzure aplikace ASP.NET App Service Web Apps. V tomto kurzu vytvoříte jednoduchý front-endové rozhraní ASP.NET MVC aplikaci seznamu úkolů, která připojí databázi MongoDB tooa spuštěny na virtuálním počítači v Azure.  [MongoDB] [ MongoDB] je populární open source, vysoký výkon databáze NoSQL. Po spuštění a testování aplikace ASP.NET hello ve svém vývojovém počítači, bude nahrávat aplikace tooApp hello Service Web Apps pomocí Git.

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="background-knowledge"></a>Znalostní báze pozadí
V tomto kurzu, přestože není povinný je užitečné znalosti hello následující:

* Hello C# ovladačů pro MongoDB. Další informace o vývoji aplikace C# pro MongoDB, najdete v části hello MongoDB [CSharp jazyk Center][MongoC#LangCenter]. 
* Architektura Hello ASP .NET webových aplikací. Se dozvíte v hello [webu ASP.net][ASP.NET].
* Architektura Hello ASP .NET MVC webových aplikací. Se dozvíte v hello [webové stránky ASP.NET MVC][MVCWebSite].
* Azure. Abyste mohli začít, čtení v [Azure][WindowsAzure].

## <a name="prerequisites"></a>Požadavky
* [Visual Studio Express 2013 pro Web] [ VSEWeb] nebo [Visual Studio 2013][VSUlt]
* [Azure SDK pro .NET](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* Aktivní předplatné Microsoft Azure

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a>Vytvoření virtuálního počítače a nainstalujte MongoDB
Tento kurz předpokládá, že jste vytvořili virtuální počítač v Azure. Po vytvoření hello virtuálního počítače je nutné tooinstall MongoDB hello virtuálního počítače:

* toocreate virtuálního počítače s Windows a nainstalujte MongoDB, najdete v části [nainstalujte MongoDB ve virtuálním počítači s Windows serverem v Azure][InstallMongoOnWindowsVM].

Po vytvoření hello virtuálního počítače v Azure a nainstalován MongoDB, být jisti tooremember hello název DNS hello virtuálního počítače ("testlinuxvm.cloudapp.net", např.) a hello externí port pro MongoDB, který jste zadali v hello koncový bod.  Budete potřebovat později v kurzu hello tyto informace.

<a id="createapp"></a>

## <a name="create-hello-application"></a>Vytvoření aplikace hello
V této části vytvoříte aplikaci ASP.NET s názvem "Moje seznam úkolů" pomocí sady Visual Studio a provedení počáteční nasazení tooAzure App Service Web Apps. Spustí hello aplikaci místně, ale bude připojení tooyour virtuálního počítače na Azure a pomocí instance hello MongoDB, kterou jste vytvořili existuje.

1. V sadě Visual Studio, klikněte na tlačítko **nový projekt**.
   
    ![Spuštění nového projektu stránky][StartPageNewProject]
2. V hello **nový projekt** okno, v hello levém podokně vyberte **Visual C#**a potom vyberte **webové**. V prostředním podokně hello vyberte **webové aplikace ASP.NET**. V dolní části hello, pojmenujte svůj projekt "MyTaskListApp" a pak klikněte na **OK**.
   
    ![Dialogové okno Nový projekt][NewProjectMyTaskListApp]
3. V hello **nový projekt ASP.NET** dialogové okno, vyberte **MVC**a potom klikněte na **OK**.
   
    ![Vyberte šablonu MVC][VS2013SelectMVCTemplate]
4. Pokud už nejste do Microsoft Azure, bude výzvami toosign v. Postupujte podle pokynů toosign hello do Azure.
5. Jakmile se přihlásíte, můžete začít konfigurace webové aplikace služby App Service. Zadejte hello **název webové aplikace**, **plán služby App Service**, **skupiny prostředků**, a **oblast**, pak klikněte na tlačítko **vytvořit**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. Po dokončení vytváření projektu hello počkejte hello webové aplikace toobe vytvořené v Azure App Service, jak je uvedeno v hello **aktivita služby Azure App Service** okno. Potom klikněte na **nyní publikování MyTaskListApp toothis webové aplikace**.
7. Klikněte na **Publikovat**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    Jakmile vaše aplikace ASP.NET výchozí publikované tooAzure App Service Web Apps, spustí se v prohlížeči hello.

## <a name="install-hello-mongodb-c-driver"></a>Nainstalujte hello ovladač MongoDB C#
MongoDB nabízí podporu na straně klienta pro C# aplikace prostřednictvím ovladače, které budete potřebovat tooinstall ve svém místním vývojovém počítači. je k dispozici prostřednictvím balíčku NuGet Hello C# ovladač.

hello tooinstall MongoDB C# ovladače:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **MyTaskListApp** projektu a vyberte **spravovat NuGetPackages**.
   
    ![Správa balíčků NuGet][VS2013ManageNuGetPackages]
2. V hello **spravovat balíčky NuGet** klikněte na okno, v levém podokně hello **Online**. V hello **Online hledání** pole na hello správné, zadejte "mongodb.driver".  Klikněte na tlačítko **nainstalovat** tooinstall hello ovladačů.
   
    ![Vyhledejte MongoDB C# ovladačů][SearchforMongoDBCSharpDriver]
3. Klikněte na tlačítko **souhlasím** tooaccept hello 10gen, Inc. licenční podmínky.
4. Klikněte na tlačítko **Zavřít** po nainstalován ovladač hello.
    ![MongoDB C# ovladač nainstalován][MongoDBCsharpDriverInstalled]

Nyní je nainstalována Hello MongoDB C# ovladačů.  Odkazy na toohello **MongoDB.Bson**, **MongoDB.Driver**, a **MongoDB.Driver.Core** knihovny přidané toohello projektu.

![Odkazy na MongoDB C# ovladačů][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a>Přidání modelu
V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello *modely* složky a **přidat** nový **– třída** a pojmenujte ji *TaskModel.cs* .  V *TaskModel.cs*, nahraďte existující kód hello hello následující kód:

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

## <a name="add-hello-data-access-layer"></a>Přidat hello vrstva
V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello *MyTaskListApp* projektu a **přidat** **novou složku** s názvem *DAL*.  Klikněte pravým tlačítkem na hello *DAL* složky a **přidat** nový **třída**. Název souboru třída hello *Dal.cs*.  V *Dal.cs*, nahraďte existující kód hello hello následující kód:

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

## <a name="add-a-controller"></a>Přidání kontroleru
Otevřete hello *Controllers\HomeController.cs* souboru v **Průzkumníku řešení** a nahraďte existující kód hello hello následující:

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

## <a name="set-up-hello-styles"></a>Nastavení stylů hello
toochange hello nadpis v horní části hello hello stránky, otevřete hello *Views\Shared\\_Layout.cshtml* souboru v **Průzkumníku řešení** a nahraďte "Název aplikace" v záhlaví navigační panel hello "Moje úloh Seznam aplikací"tak, aby vypadá takto:

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

V pořadí tooset nabídka hello seznam úloh, otevřete hello *\Views\Home\Index.cshtml* a nahraďte existující kód hello hello následující kód:

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


tooadd hello možnost toocreate novou úlohu, klikněte pravým tlačítkem na hello *Views\Home\\*  složky a **přidat** **zobrazení**.  Název zobrazení hello *vytvořit*. Nahraďte kód hello hello následující:

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

## <a name="set-hello-mongodb-connection-string"></a>Nastavit hello MongoDB připojovací řetězec
V **Průzkumníku řešení**, otevřete hello *DAL/Dal.cs* souboru. Najde hello následující řádek kódu:

    private string connectionString = "mongodb://<vm-dns-name>";

Nahraďte `<vm-dns-name>` s názvem DNS hello hello virtuální počítač se systémem MongoDB, které jste vytvořili v hello [vytvořit virtuální počítač a nainstalujte MongoDB] [ Create a virtual machine and install MongoDB] krok tohoto kurzu.  název DNS hello toofind virtuálního počítače, přejděte toohello portálu Azure, vyberte **virtuální počítače**a najít **název DNS**.

Pokud název DNS hello hello virtuálního počítače je "testlinuxvm.cloudapp.net" a MongoDB naslouchá na portu výchozí hello 27017, bude vypadat hello připojovací řetězec řádek kódu:

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

Pokud koncový bod virtuálního počítače hello určuje jiný externí port pro MongoDB, můžete zadejte hello port v hello připojovací řetězec:

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

Další informace o MongoDB připojovacích řetězcích najdete v tématu [připojení][MongoConnectionStrings].

## <a name="test-hello-local-deployment"></a>Testovací nasazení pro místní hello
Vyberte aplikaci na vývojovém počítači, toorun **spustit ladění** z hello **ladění** nabídky nebo stiskněte klávesu **F5**. Služba IIS Express se spustí a v prohlížeči se otevře a spustí domovskou stránku hello aplikace.  Můžete přidat nové úlohy, které se přidají databázi MongoDB toohello běžící na virtuálním počítači v Azure.

![Moje aplikace seznamu úkolů][TaskListAppBlank]

## <a name="publish-tooazure-app-service-web-apps"></a>Publikování tooAzure App Service Web Apps
V této části budete publikovat vaše změny tooAzure App Service Web Apps.

1. V Průzkumníku řešení klikněte pravým tlačítkem na **MyTaskListApp** znovu a klikněte na tlačítko **publikovat**.
2. Klikněte na **Publikovat**.
   
    Teď byste měli vidět vaší webové aplikace spuštěné v Azure App Service a přístup k databázi MongoDB hello v Azure Virtual Machines.

## <a name="summary"></a>Souhrn
Teď úspěšně jste nasadili vaší tooAzure aplikace ASP.NET App Service Web Apps. tooview hello webové aplikace:

1. Přihlaste se k portálu Azure hello.
2. Klikněte na tlačítko **webové aplikace**. 
3. Vyberte webové aplikace v hello **webové aplikace** seznamu.

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
[Create and run hello My Task List ASP.NET application on your development computer]: #createapp
[Create an Azure web site]: #createwebsite
[Deploy hello ASP.NET application toohello web site using Git]: #deployapp
