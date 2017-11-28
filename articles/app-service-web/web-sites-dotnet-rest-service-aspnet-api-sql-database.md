---
title: "aaaCreate rozhraní REST API v Azure pomocí technologie ASP.NET a databáze SQL | Microsoft Docs"
description: "Kurz, se naučíte, jak toodeploy aplikaci, která používá hello tooan rozhraní ASP.NET Web API webové aplikace Azure pomocí sady Visual Studio."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
writer: Rick-Anderson
manager: erikre
editor: 
ms.assetid: f4916fc0-ea08-41f7-846b-73e41bc88149
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/29/2016
ms.author: riande
ms.openlocfilehash: 1ef45dd1582bfda367e53c39f863164422ad678b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a><span data-ttu-id="6f068-103">Vytvoření služby pomocí rozhraní ASP.NET Web API a databázi SQL v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6f068-103">Create a REST service using ASP.NET Web API and SQL Database in Azure App Service</span></span>
<span data-ttu-id="6f068-104">Tento kurz ukazuje, jak toodeploy technologie ASP.NET webové aplikace tooan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí Průvodce publikování webu hello v sadě Visual Studio 2013 nebo Visual Studio 2013 Community Edition.</span><span class="sxs-lookup"><span data-stu-id="6f068-104">This tutorial shows how toodeploy an ASP.NET web app tooan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by using hello Publish Web wizard in Visual Studio 2013 or Visual Studio 2013 Community Edition.</span></span> 

<span data-ttu-id="6f068-105">Můžete otevřít účet Azure zdarma a pokud ještě nemáte Visual Studio 2013, hello SDK automaticky nainstaluje Visual Studio 2013 pro produkt Web Express.</span><span class="sxs-lookup"><span data-stu-id="6f068-105">You can open an Azure account for free, and if you don't already have Visual Studio 2013, hello SDK automatically installs Visual Studio 2013 for Web Express.</span></span> <span data-ttu-id="6f068-106">Proto můžete spustit vývoj pro Azure zcela zdarma.</span><span class="sxs-lookup"><span data-stu-id="6f068-106">So you can start developing for Azure entirely for free.</span></span>

<span data-ttu-id="6f068-107">Tento kurz předpokládá, že máte žádné předchozí zkušenosti s používáním Azure.</span><span class="sxs-lookup"><span data-stu-id="6f068-107">This tutorial assumes that you have no prior experience using Azure.</span></span> <span data-ttu-id="6f068-108">Po dokončení tohoto kurzu, budete mít jednoduché webové aplikace nahoru a spouštění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-108">On completing this tutorial, you'll have a simple web app up and running in hello cloud.</span></span>

<span data-ttu-id="6f068-109">Naučíte se:</span><span class="sxs-lookup"><span data-stu-id="6f068-109">You'll learn:</span></span>

* <span data-ttu-id="6f068-110">Jak tooenable počítači pro vývoj pro Azure nainstalováním hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="6f068-110">How tooenable your machine for Azure development by installing hello Azure SDK.</span></span>
* <span data-ttu-id="6f068-111">Jak toocreate Visual Studio ASP.NET MVC 5 projektu a publikujete ho v tooan aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="6f068-111">How toocreate a Visual Studio ASP.NET MVC 5 project and publish it tooan Azure app.</span></span>
* <span data-ttu-id="6f068-112">Jakým způsobem volá toouse hello rozhraní ASP.NET Web API tooenable rozhraní Restful API.</span><span class="sxs-lookup"><span data-stu-id="6f068-112">How toouse hello ASP.NET Web API tooenable Restful API calls.</span></span>
* <span data-ttu-id="6f068-113">Jak toouse SQL databáze toostore data v Azure.</span><span class="sxs-lookup"><span data-stu-id="6f068-113">How toouse a SQL database toostore data in Azure.</span></span>
* <span data-ttu-id="6f068-114">Jak aplikace toopublish aktualizuje tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6f068-114">How toopublish application updates tooAzure.</span></span>

<span data-ttu-id="6f068-115">Budete vytvářet jednoduché seznamu kontaktů webovou aplikaci, která je založená na technologii ASP.NET MVC 5 a používá hello ADO.NET Entity Framework pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="6f068-115">You'll build a simple contact list web application that is built on ASP.NET MVC 5 and uses hello ADO.NET Entity Framework for database access.</span></span> <span data-ttu-id="6f068-116">Následující obrázek ukazuje hello Hello dokončil aplikace:</span><span class="sxs-lookup"><span data-stu-id="6f068-116">hello following illustration shows hello completed application:</span></span>

![snímek obrazovky webové stránky][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-hello-project"></a><span data-ttu-id="6f068-118">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="6f068-118">Create hello project</span></span>
1. <span data-ttu-id="6f068-119">Spusťte Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="6f068-119">Start Visual Studio 2013.</span></span>
2. <span data-ttu-id="6f068-120">Z hello **soubor** nabídce klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="6f068-120">From hello **File** menu click **New Project**.</span></span>
3. <span data-ttu-id="6f068-121">V hello **nový projekt** dialogové okno, rozbalte seznam **Visual C#** a vyberte **webové** a pak vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="6f068-121">In hello **New Project** dialog box, expand **Visual C#** and select **Web**  and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="6f068-122">Název aplikace hello **ContactManager** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f068-122">Name hello application **ContactManager** and click **OK**.</span></span>
   
    ![Dialogové okno Nový projekt](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. <span data-ttu-id="6f068-124">V hello **nový projekt ASP.NET** dialogové okno, vyberte hello **MVC** šablony, zkontrolujte **webového rozhraní API** a pak klikněte na **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="6f068-124">In hello **New ASP.NET Project** dialog box, select hello **MVC** template, check **Web API** and then click **Change Authentication**.</span></span>
5. <span data-ttu-id="6f068-125">V hello **změna ověřování** dialogové okno, klikněte na tlačítko **bez ověřování**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f068-125">In hello **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span>
   
    ![Bez ověřování](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    <span data-ttu-id="6f068-127">Funkce, které vyžadují toolog uživatelé v nebude mít Hello ukázkovou aplikaci, kterou vytváříte.</span><span class="sxs-lookup"><span data-stu-id="6f068-127">hello sample application you're creating won't have features that require users toolog in.</span></span> <span data-ttu-id="6f068-128">Informace o tooimplement funkce ověřování a autorizace, najdete v části hello [další kroky](#nextsteps) oddíl hello konce tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6f068-128">For information about how tooimplement authentication and authorization features, see hello [Next Steps](#nextsteps) section at hello end of this tutorial.</span></span> 
6. <span data-ttu-id="6f068-129">V hello **nový projekt ASP.NET** dialogové okno, ujistěte se, že hello **hostitel v cloudu hello** je zaškrtnuté políčko a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f068-129">In hello **New ASP.NET Project** dialog box, make sure hello **Host in hello Cloud** is checked and click **OK**.</span></span>

<span data-ttu-id="6f068-130">Pokud nejste přihlášeni dříve tooAzure, bude výzvami toosign v.</span><span class="sxs-lookup"><span data-stu-id="6f068-130">If you have not previously signed in tooAzure, you will be prompted toosign in.</span></span>

1. <span data-ttu-id="6f068-131">Průvodce konfigurací Hello navrhne jedinečný název založený na *ContactManager* (viz následující obrázek hello).</span><span class="sxs-lookup"><span data-stu-id="6f068-131">hello configuration wizard will suggest a unique name based on *ContactManager* (see hello image below).</span></span> <span data-ttu-id="6f068-132">Vyberte oblast okolo vás.</span><span class="sxs-lookup"><span data-stu-id="6f068-132">Select a region near you.</span></span> <span data-ttu-id="6f068-133">Můžete použít [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") toofind hello nejnižší latenci datového centra.</span><span class="sxs-lookup"><span data-stu-id="6f068-133">You can use [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") toofind hello lowest latency data center.</span></span> 
2. <span data-ttu-id="6f068-134">Pokud jste dosud nevytvořili databázový server před, vyberte **vytvořit nový server**, zadejte jméno uživatele databáze a heslo.</span><span class="sxs-lookup"><span data-stu-id="6f068-134">If you haven't created a database server before, select **Create new server**, enter a database user name and password.</span></span>
   
    ![Konfigurace webu Azure](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

<span data-ttu-id="6f068-136">Pokud máte databázový server, použijte tento toocreate novou databázi.</span><span class="sxs-lookup"><span data-stu-id="6f068-136">If you have a database server, use that toocreate a new database.</span></span> <span data-ttu-id="6f068-137">Databázové servery jsou drahocenný prostředků a obvykle mají toocreate několik databází hello stejný server pro testování a vývoj, nikoli databázový server na databázi.</span><span class="sxs-lookup"><span data-stu-id="6f068-137">Database servers are a precious resource, and you generally want toocreate multiple databases on hello same server for testing and development rather than creating a database server per database.</span></span> <span data-ttu-id="6f068-138">Zkontrolujte, zda webový server a databáze jsou v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="6f068-138">Make sure your web site and database are in hello same region.</span></span>

![Konfigurace webu Azure](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-hello-page-header-and-footer"></a><span data-ttu-id="6f068-140">Nastavit hello záhlaví a zápatí stránky</span><span class="sxs-lookup"><span data-stu-id="6f068-140">Set hello page header and footer</span></span>
1. <span data-ttu-id="6f068-141">V **Průzkumníku řešení**, rozbalte položku hello *Views\Shared* složku a otevřete hello *_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="6f068-141">In **Solution Explorer**, expand hello *Views\Shared* folder and open hello *_Layout.cshtml* file.</span></span>
   
    ![_Layout.cshtml v Průzkumníku řešení][newapp004]
2. <span data-ttu-id="6f068-143">Nahraďte obsah hello hello *Views\Shared_Layout.cshtml* soubor s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="6f068-143">Replace hello contents of hello *Views\Shared_Layout.cshtml* file with hello following code:</span></span>

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="utf-8" />
            <title>@ViewBag.Title - Contact Manager</title>
            <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
            <meta name="viewport" content="width=device-width" />
            @Styles.Render("~/Content/css")
            @Scripts.Render("~/bundles/modernizr")
        </head>
        <body>
            <header>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p class="site-title">@Html.ActionLink("Contact Manager", "Index", "Home")</p>
                    </div>
                </div>
            </header>
            <div id="body">
                @RenderSection("featured", required: false)
                <section class="content-wrapper main-content clear-fix">
                    @RenderBody()
                </section>
            </div>
            <footer>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p>&copy; @DateTime.Now.Year - Contact Manager</p>
                    </div>
                </div>
            </footer>
            @Scripts.Render("~/bundles/jquery")
            @RenderSection("scripts", required: false)
        </body>
        </html>

<span data-ttu-id="6f068-144">Hello značek výše název aplikace hello změny z "Moje aplikace technologie ASP.NET" příliš "obraťte se na správce" a odebere odkazy hello příliš**Domů**, **o** a **kontaktujte**.</span><span class="sxs-lookup"><span data-stu-id="6f068-144">hello markup above changes hello app name from "My ASP.NET App" too"Contact Manager", and it removes hello links too**Home**, **About** and **Contact**.</span></span>

### <a name="run-hello-application-locally"></a><span data-ttu-id="6f068-145">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6f068-145">Run hello application locally</span></span>
1. <span data-ttu-id="6f068-146">Stisknutím kombinace kláves CTRL + F5 toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f068-146">Press CTRL+F5 toorun hello application.</span></span>
   <span data-ttu-id="6f068-147">domovskou stránku Hello aplikace se zobrazí v hello výchozí prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="6f068-147">hello application home page appears in hello default browser.</span></span>
    <span data-ttu-id="6f068-148">![tooDo seznamu domovskou stránku](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span><span class="sxs-lookup"><span data-stu-id="6f068-148">![tooDo List home page](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span></span>

<span data-ttu-id="6f068-149">To je vše, že je nutné toodo pro nyní toocreate hello aplikaci nasadíte tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6f068-149">This is all you need toodo for now toocreate hello application that you'll deploy tooAzure.</span></span> <span data-ttu-id="6f068-150">Později přidáte funkce databáze.</span><span class="sxs-lookup"><span data-stu-id="6f068-150">Later you'll add database functionality.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="6f068-151">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="6f068-151">Deploy hello application tooAzure</span></span>
1. <span data-ttu-id="6f068-152">V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a vyberte **publikovat** hello místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="6f068-152">In Visual Studio, right-click hello project in **Solution Explorer** and select **Publish** from hello context menu.</span></span>
   
    ![Publikování v kontextové nabídce projektu][PublishVSSolution]
   
    <span data-ttu-id="6f068-154">Hello **Publikovat Web** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="6f068-154">hello **Publish Web** wizard opens.</span></span>
2. <span data-ttu-id="6f068-155">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-155">Click **Publish**.</span></span>

![Karta nastavení](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

<span data-ttu-id="6f068-157">Sada Visual Studio spustí proces hello kopírování hello soubory toohello Azure server.</span><span class="sxs-lookup"><span data-stu-id="6f068-157">Visual Studio begins hello process of copying hello files toohello Azure server.</span></span> <span data-ttu-id="6f068-158">Hello **výstup** okno zobrazuje, jaké akce nasazení byly provedeny a hlásí úspěšné dokončení nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-158">hello **Output** window shows what deployment actions were taken and reports successful completion of hello deployment.</span></span>

1. <span data-ttu-id="6f068-159">Adresa URL toohello hello nasazené lokality se automaticky otevře v Hello výchozí prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="6f068-159">hello default browser automatically opens toohello URL of hello deployed site.</span></span>
   
   <span data-ttu-id="6f068-160">Hello aplikaci, kterou jste vytvořili je nyní spuštěna v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-160">hello application you created is now running in hello cloud.</span></span>
   
   ![tooDo seznamu domovskou stránku běžící v Azure][rxz2]

## <a name="add-a-database-toohello-application"></a><span data-ttu-id="6f068-162">Přidání aplikace toohello databáze</span><span class="sxs-lookup"><span data-stu-id="6f068-162">Add a database toohello application</span></span>
<span data-ttu-id="6f068-163">Dále budete aktualizovat hello MVC aplikace tooadd hello možnost toodisplay a aktualizovat kontakty a uložení hello dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="6f068-163">Next, you'll update hello MVC application tooadd hello ability toodisplay and update contacts and store hello data in a database.</span></span> <span data-ttu-id="6f068-164">aplikace Hello použije hello Entity Framework toocreate hello databáze a tooread a aktualizovat data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-164">hello application will use hello Entity Framework toocreate hello database and tooread and update data in hello database.</span></span>

### <a name="add-data-model-classes-for-hello-contacts"></a><span data-ttu-id="6f068-165">Přidání třídy modelu dat pro kontakty hello</span><span class="sxs-lookup"><span data-stu-id="6f068-165">Add data model classes for hello contacts</span></span>
<span data-ttu-id="6f068-166">Začněte vytvořením jednoduchého datového modelu v kódu.</span><span class="sxs-lookup"><span data-stu-id="6f068-166">You begin by creating a simple data model in code.</span></span>

1. <span data-ttu-id="6f068-167">V **Průzkumníku řešení**, klikněte pravým tlačítkem na složku modely hello, klikněte na **přidat**a potom **třída**.</span><span class="sxs-lookup"><span data-stu-id="6f068-167">In **Solution Explorer**, right-click hello Models folder, click **Add**, and then **Class**.</span></span>
   
    ![Přidání třídy v kontextové nabídce složku modely][adddb001]
2. <span data-ttu-id="6f068-169">V hello **přidat novou položku** dialogové okno, název hello nový soubor třídy *Contact.cs*a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-169">In hello **Add New Item** dialog box, name hello new class file *Contact.cs*, and then click **Add**.</span></span>
   
    ![Přidat novou položku – dialogové okno][adddb002]
3. <span data-ttu-id="6f068-171">Nahraďte hello obsah souboru Contacts.cs hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="6f068-171">Replace hello contents of hello Contacts.cs file with hello following code.</span></span>
   
        using System.Globalization;
        namespace ContactManager.Models
        {
            public class Contact
               {
                public int ContactId { get; set; }
                public string Name { get; set; }
                public string Address { get; set; }
                public string City { get; set; }
                public string State { get; set; }
                public string Zip { get; set; }
                public string Email { get; set; }
                public string Twitter { get; set; }
                public string Self
                {
                    get { return string.Format(CultureInfo.CurrentCulture,
                         "api/contacts/{0}", this.ContactId); }
                    set { }
                }
            }
        }

<span data-ttu-id="6f068-172">Hello **obraťte se na** třída definuje hello dat, které se uloží pro každý kontakt a primární klíč, KódKontaktu, která je potřeba hello databáze.</span><span class="sxs-lookup"><span data-stu-id="6f068-172">hello **Contact** class defines hello data that you will store for each contact, plus a primary key, ContactID, that is needed by hello database.</span></span> <span data-ttu-id="6f068-173">Můžete získat další informace o datových modelech v hello [další kroky](#nextsteps) oddíl hello konce tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6f068-173">You can get more information about data models in hello [Next Steps](#nextsteps) section at hello end of this tutorial.</span></span>

### <a name="create-web-pages-that-enable-app-users-toowork-with-hello-contacts"></a><span data-ttu-id="6f068-174">Vytvoření webové stránky, které umožňují uživatelům toowork aplikace s kontakty hello</span><span class="sxs-lookup"><span data-stu-id="6f068-174">Create web pages that enable app users toowork with hello contacts</span></span>
<span data-ttu-id="6f068-175">Hello funkce generování uživatelského rozhraní ASP.NET MVC hello může automaticky vygenerovat kód, který provádí vytvářet, číst, aktualizovat a odstraňovat akcemi (CRUD).</span><span class="sxs-lookup"><span data-stu-id="6f068-175">hello ASP.NET MVC hello scaffolding feature can automatically generate code that performs create, read, update, and delete (CRUD) actions.</span></span>

## <a name="add-a-controller-and-a-view-for-hello-data"></a><span data-ttu-id="6f068-176">Přidání Kontroleru a zobrazení pro hello data</span><span class="sxs-lookup"><span data-stu-id="6f068-176">Add a Controller and a view for hello data</span></span>
1. <span data-ttu-id="6f068-177">V **Průzkumníku**, rozbalte složku řadiče hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-177">In **Solution Explorer**, expand hello Controllers folder.</span></span>
2. <span data-ttu-id="6f068-178">Sestavení projektu hello **(Ctrl + Shift + B)**.</span><span class="sxs-lookup"><span data-stu-id="6f068-178">Build hello project **(Ctrl+Shift+B)**.</span></span> <span data-ttu-id="6f068-179">(Před použitím mechanismus generování uživatelského rozhraní musí sestavte projekt hello.)</span><span class="sxs-lookup"><span data-stu-id="6f068-179">(You must build hello project before using scaffolding mechanism.)</span></span> 
3. <span data-ttu-id="6f068-180">Klikněte pravým tlačítkem na složku hello řadiče a klikněte na tlačítko **přidat**a potom klikněte na **řadič**.</span><span class="sxs-lookup"><span data-stu-id="6f068-180">Right-click hello Controllers folder and click **Add**, and then click **Controller**.</span></span>
   
    ![Přidat řadič v kontextové nabídce řadiče složky][addcode001]
4. <span data-ttu-id="6f068-182">V hello **přidat vygenerované uživatelské rozhraní** dialogové okno, vyberte **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-182">In hello **Add Scaffold** dialog box, select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
   
   ![Přidání kontroleru](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. <span data-ttu-id="6f068-184">Nastavte název řadiče hello příliš**HomeController**.</span><span class="sxs-lookup"><span data-stu-id="6f068-184">Set hello controller name too**HomeController**.</span></span> <span data-ttu-id="6f068-185">Vyberte **kontaktujte** jako třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="6f068-185">Select **Contact** as your model class.</span></span> <span data-ttu-id="6f068-186">Klikněte na tlačítko hello **nový kontext dat** tlačítko a přijměte výchozí hello "ContactManager.Models.ContactManagerContext" pro hello **nový typ kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-186">Click hello **New data context** button and accept hello default "ContactManager.Models.ContactManagerContext" for hello **New data context type**.</span></span> <span data-ttu-id="6f068-187">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-187">Click **Add**.</span></span>

    <span data-ttu-id="6f068-188">Dialogové okno zobrazí výzvu: "soubor s názvem hello HomeController již ukončí.</span><span class="sxs-lookup"><span data-stu-id="6f068-188">A dialog box will prompt you: "A file with hello name HomeController already exits.</span></span> <span data-ttu-id="6f068-189">Chcete, aby tooreplace ho? ".</span><span class="sxs-lookup"><span data-stu-id="6f068-189">Do you want tooreplace it?".</span></span> <span data-ttu-id="6f068-190">Klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="6f068-190">Click **Yes**.</span></span> <span data-ttu-id="6f068-191">Přepisování jsme hello Domů řadiče, který byl vytvořen s hello nový projekt.</span><span class="sxs-lookup"><span data-stu-id="6f068-191">We are overwriting hello Home Controller that was created with hello new project.</span></span> <span data-ttu-id="6f068-192">Použijeme hello nové Domů řadiče pro naše seznamu kontaktů.</span><span class="sxs-lookup"><span data-stu-id="6f068-192">We will use hello new Home Controller for our contact list.</span></span>

    <span data-ttu-id="6f068-193">Visual Studio vytvoří metody kontroleru a zobrazení pro operace CRUD databáze pro **kontaktujte** objekty.</span><span class="sxs-lookup"><span data-stu-id="6f068-193">Visual Studio creates controller methods and views for CRUD database operations for **Contact** objects.</span></span>

## <a name="enable-migrations-create-hello-database-add-sample-data-and-a-data-initializer"></a><span data-ttu-id="6f068-194">Povolit migrace a vytvořit databázi hello, Přidání ukázkových dat a inicializátoru dat</span><span class="sxs-lookup"><span data-stu-id="6f068-194">Enable Migrations, create hello database, add sample data and a data initializer</span></span>
<span data-ttu-id="6f068-195">Další úlohou Hello je tooenable hello [migrace Code First](http://curah.microsoft.com/55220) funkce v pořadí toocreate hello databáze založené na datový model hello jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6f068-195">hello next task is tooenable hello [Code First Migrations](http://curah.microsoft.com/55220) feature in order toocreate hello database based on hello data model you created.</span></span>

1. <span data-ttu-id="6f068-196">V hello **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a potom **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="6f068-196">In hello **Tools** menu, select **Library Package Manager** and then **Package Manager Console**.</span></span>
   
    ![Konzola správce balíčků v nabídce Nástroje][addcode008]
2. <span data-ttu-id="6f068-198">V hello **Konzola správce balíčků** okno, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6f068-198">In hello **Package Manager Console** window, enter hello following command:</span></span>
   
        enable-migrations 
   
    <span data-ttu-id="6f068-199">Hello **enable-migrations se** příkaz vytvoří *migrace* složku a její uloží je v této složce *Configuration.cs* souboru, můžete upravit tooconfigure migrace.</span><span class="sxs-lookup"><span data-stu-id="6f068-199">hello **enable-migrations** command creates a *Migrations* folder and it puts in that folder a *Configuration.cs* file that you can edit tooconfigure Migrations.</span></span> 
3. <span data-ttu-id="6f068-200">V hello **Konzola správce balíčků** okno, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6f068-200">In hello **Package Manager Console** window, enter hello following command:</span></span>
   
        add-migration Initial
   
    <span data-ttu-id="6f068-201">Hello **přidat migrace počáteční** příkaz vygeneruje třídy s názvem  **&lt;date_stamp&gt;počáteční** vytvářející hello databáze.</span><span class="sxs-lookup"><span data-stu-id="6f068-201">hello **add-migration Initial** command generates a class named **&lt;date_stamp&gt;Initial** that creates hello database.</span></span> <span data-ttu-id="6f068-202">první parametr Hello ( *počáteční* ) je libovolný a slouží toocreate hello název souboru hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-202">hello first parameter ( *Initial* ) is arbitrary and used toocreate hello name of hello file.</span></span> <span data-ttu-id="6f068-203">Uvidíte hello nové třídy soubory ve **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="6f068-203">You can see hello new class files in **Solution Explorer**.</span></span>
   
    <span data-ttu-id="6f068-204">V hello **počáteční** třídy, hello **až** metoda vytvoří tabulku kontaktů hello a hello **dolů** – metoda (používá, když chcete, aby tooreturn toohello předchozí stav) se zahodí.</span><span class="sxs-lookup"><span data-stu-id="6f068-204">In hello **Initial** class, hello **Up** method creates hello Contacts table, and hello **Down** method (used when you want tooreturn toohello previous state) drops it.</span></span>
4. <span data-ttu-id="6f068-205">Otevřete hello *Migrations\Configuration.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="6f068-205">Open hello *Migrations\Configuration.cs* file.</span></span> 
5. <span data-ttu-id="6f068-206">Přidejte následující obory názvů hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-206">Add hello following namespaces.</span></span> 
   
         using ContactManager.Models;
6. <span data-ttu-id="6f068-207">Nahraďte hello *počáteční hodnoty* metoda s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="6f068-207">Replace hello *Seed* method with hello following code:</span></span>
   
        protected override void Seed(ContactManager.Models.ContactManagerContext context)
        {
            context.Contacts.AddOrUpdate(p => p.Name,
               new Contact
               {
                   Name = "Debra Garcia",
                   Address = "1234 Main St",
                   City = "Redmond",
                   State = "WA",
                   Zip = "10999",
                   Email = "debra@example.com",
                   Twitter = "debra_example"
               },
                new Contact
                {
                    Name = "Thorsten Weinrich",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "thorsten@example.com",
                    Twitter = "thorsten_example"
                },
                new Contact
                {
                    Name = "Yuhong Li",
                    Address = "9012 State st",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "yuhong@example.com",
                    Twitter = "yuhong_example"
                },
                new Contact
                {
                    Name = "Jon Orton",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "jon@example.com",
                    Twitter = "jon_example"
                },
                new Contact
                {
                    Name = "Diliana Alexieva-Bosseva",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "diliana@example.com",
                    Twitter = "diliana_example"
                }
                );
        }
   
    <span data-ttu-id="6f068-208">Tento kód výše inicializuje hello databáze s hello kontaktní informace.</span><span class="sxs-lookup"><span data-stu-id="6f068-208">This code above will initialize hello database with hello contact information.</span></span> <span data-ttu-id="6f068-209">Další informace o synchronizace replik indexů databáze hello najdete v tématu [ladění Entity Framework (EF) databází](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f068-209">For more information on seeding hello database, see [Debugging Entity Framework (EF) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span></span>
7. <span data-ttu-id="6f068-210">V hello **Konzola správce balíčků** zadejte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6f068-210">In hello **Package Manager Console** enter hello command:</span></span>
   
        update-database
   
    ![Konzola správce balíčků příkazy][addcode009]
   
    <span data-ttu-id="6f068-212">Hello **update-database** spustí hello první migrace, která vytvoří databázi hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-212">hello **update-database** runs hello first migration which creates hello database.</span></span> <span data-ttu-id="6f068-213">Ve výchozím nastavení je databáze hello vytvoří jako databáze SQL serveru Express LocalDB.</span><span class="sxs-lookup"><span data-stu-id="6f068-213">By default, hello database is created as a SQL Server Express LocalDB database.</span></span>
8. <span data-ttu-id="6f068-214">Stisknutím kombinace kláves CTRL + F5 toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f068-214">Press CTRL+F5 toorun hello application.</span></span> 

<span data-ttu-id="6f068-215">aplikace Hello zobrazuje hello počáteční hodnoty data a poskytuje úpravy, podrobnosti a odkazy odstranit.</span><span class="sxs-lookup"><span data-stu-id="6f068-215">hello application shows hello seed data and provides edit, details and delete links.</span></span>

![Zobrazení MVC dat][rxz3]

## <a name="edit-hello-view"></a><span data-ttu-id="6f068-217">Upravit hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="6f068-217">Edit hello View</span></span>
1. <span data-ttu-id="6f068-218">Otevřete hello *Views\Home\Index.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="6f068-218">Open hello *Views\Home\Index.cshtml* file.</span></span> <span data-ttu-id="6f068-219">V dalším kroku hello jsme nahradí hello vygeneruje kód s kódem, který používá [jQuery](http://jquery.com/) a [Knockout.js](http://knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="6f068-219">In hello next step, we will replace hello generated markup with code that uses [jQuery](http://jquery.com/) and [Knockout.js](http://knockoutjs.com/).</span></span> <span data-ttu-id="6f068-220">Tento nový kód načte hello seznamu kontaktů pomocí webového rozhraní API a JSON a pak vazby hello kontaktovat toohello data uživatelského rozhraní pomocí knockout.js.</span><span class="sxs-lookup"><span data-stu-id="6f068-220">This new code retrieves hello list of contacts from using web API and JSON and then binds hello contact data toohello UI using knockout.js.</span></span> <span data-ttu-id="6f068-221">Další informace najdete v tématu hello [další kroky](#nextsteps) oddíl hello konce tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6f068-221">For more information, see hello [Next Steps](#nextsteps) section at hello end of this tutorial.</span></span> 
2. <span data-ttu-id="6f068-222">Nahraďte obsah souboru hello hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="6f068-222">Replace hello contents of hello file with hello following code.</span></span>
   
        @model IEnumerable<ContactManager.Models.Contact>
        @{
            ViewBag.Title = "Home";
        }
        @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                function ContactsViewModel() {
                    var self = this;
                    self.contacts = ko.observableArray([]);
                    self.addContact = function () {
                        $.post("api/contacts",
                            $("#addContact").serialize(),
                            function (value) {
                                self.contacts.push(value);
                            },
                            "json");
                    }
                    self.removeContact = function (contact) {
                        $.ajax({
                            type: "DELETE",
                            url: contact.Self,
                            success: function () {
                                self.contacts.remove(contact);
                            }
                        });
                    }
   
                    $.getJSON("api/contacts", function (data) {
                        self.contacts(data);
                    });
                }
                ko.applyBindings(new ContactsViewModel());    
        </script>
        }
        <ul id="contacts" data-bind="foreach: contacts">
            <li class="ui-widget-content ui-corner-all">
                <h1 data-bind="text: Name" class="ui-widget-header"></h1>
                <div><span data-bind="text: $data.Address || 'Address?'"></span></div>
                <div>
                    <span data-bind="text: $data.City || 'City?'"></span>,
                    <span data-bind="text: $data.State || 'State?'"></span>
                    <span data-bind="text: $data.Zip || 'Zip?'"></span>
                </div>
                <div data-bind="if: $data.Email"><a data-bind="attr: { href: 'mailto:' + Email }, text: Email"></a></div>
                <div data-bind="ifnot: $data.Email"><span>Email?</span></div>
                <div data-bind="if: $data.Twitter"><a data-bind="attr: { href: 'http://twitter.com/' + Twitter }, text: '@@' + Twitter"></a></div>
                <div data-bind="ifnot: $data.Twitter"><span>Twitter?</span></div>
                <p><a data-bind="attr: { href: Self }, click: $root.removeContact" class="removeContact ui-state-default ui-corner-all">Remove</a></p>
            </li>
        </ul>
        <form id="addContact" data-bind="submit: addContact">
            <fieldset>
                <legend>Add New Contact</legend>
                <ol>
                    <li>
                        <label for="Name">Name</label>
                        <input type="text" name="Name" />
                    </li>
                    <li>
                        <label for="Address">Address</label>
                        <input type="text" name="Address" >
                    </li>
                    <li>
                        <label for="City">City</label>
                        <input type="text" name="City" />
                    </li>
                    <li>
                        <label for="State">State</label>
                        <input type="text" name="State" />
                    </li>
                    <li>
                        <label for="Zip">Zip</label>
                        <input type="text" name="Zip" />
                    </li>
                    <li>
                        <label for="Email">E-mail</label>
                        <input type="text" name="Email" />
                    </li>
                    <li>
                        <label for="Twitter">Twitter</label>
                        <input type="text" name="Twitter" />
                    </li>
                </ol>
                <input type="submit" value="Add" />
            </fieldset>
        </form>
3. <span data-ttu-id="6f068-223">Klikněte pravým tlačítkem na složku obsahu hello a klikněte na tlačítko **přidat**a pak klikněte na tlačítko **novou položku...** .</span><span class="sxs-lookup"><span data-stu-id="6f068-223">Right-click hello Content folder and click **Add**, and then click **New Item...**.</span></span>
   
    ![Přidání šablony stylů v kontextové nabídce složky obsahu][addcode005]
4. <span data-ttu-id="6f068-225">V hello **přidat novou položku** dialogovém okně zadejte **styl** v horním pravém vyhledávacího pole text hello a potom vyberte **list stylu**.</span><span class="sxs-lookup"><span data-stu-id="6f068-225">In hello **Add New Item** dialog box, enter **Style** in hello upper right search box and then select **Style Sheet**.</span></span>
    <span data-ttu-id="6f068-226">![Přidat novou položku – dialogové okno][rxStyle]</span><span class="sxs-lookup"><span data-stu-id="6f068-226">![Add New Item dialog box][rxStyle]</span></span>
5. <span data-ttu-id="6f068-227">Název souboru hello *Contacts.css* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-227">Name hello file *Contacts.css* and click **Add**.</span></span> <span data-ttu-id="6f068-228">Nahraďte obsah souboru hello hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="6f068-228">Replace hello contents of hello file with hello following code.</span></span>
   
        .column {
            float: left;
            width: 50%;
            padding: 0;
            margin: 5px 0;
        }
        form ol {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }
        form li {
            padding: 1px;
            margin: 3px;
        }
        form input[type="text"] {
            width: 100%;
        }
        #addContact {
            width: 300px;
            float: left;
            width:30%;
        }
        #contacts {
            list-style-type: none;
            margin: 0;
            padding: 0;
            float:left;
            width: 70%;
        }
        #contacts li {
            margin: 3px 3px 3px 0;
            padding: 1px;
            float: left;
            width: 300px;
            text-align: center;
            background-image: none;
            background-color: #F5F5F5;
        }
        #contacts li h1
        {
            padding: 0;
            margin: 0;
            background-image: none;
            background-color: Orange;
            color: White;
            font-family: Trebuchet MS, Tahoma, Verdana, Arial, sans-serif;
        }
        .removeContact, .viewImage
        {
            padding: 3px;
            text-decoration: none;
        }
   
    <span data-ttu-id="6f068-229">Budeme používat tuto šablonu stylů pro hello rozložení, barvy a styly využívané v aplikaci hello kontaktujte správce.</span><span class="sxs-lookup"><span data-stu-id="6f068-229">We will use this style sheet for hello layout, colors and styles used in hello contact manager app.</span></span>
6. <span data-ttu-id="6f068-230">Otevřete hello *App_Start\BundleConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="6f068-230">Open hello *App_Start\BundleConfig.cs* file.</span></span>
7. <span data-ttu-id="6f068-231">Přidejte následující kód tooregister hello hello [Knockout](http://knockoutjs.com/index.html "KO") modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="6f068-231">Add hello following code tooregister hello [Knockout](http://knockoutjs.com/index.html "KO") plugin.</span></span>
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    <span data-ttu-id="6f068-232">Tato ukázka pomocí knockout toosimplify dynamické JavaScript kód, který zpracovává šablony obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-232">This sample using knockout toosimplify dynamic JavaScript code that handles hello screen templates.</span></span>
8. <span data-ttu-id="6f068-233">Upravit hello obsah nebo šablon stylů css položka tooregister hello *contacts.css* šabloně stylů.</span><span class="sxs-lookup"><span data-stu-id="6f068-233">Modify hello contents/css entry tooregister hello *contacts.css* style sheet.</span></span> <span data-ttu-id="6f068-234">Změna hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="6f068-234">Change hello following line:</span></span>
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   <span data-ttu-id="6f068-235">na:</span><span class="sxs-lookup"><span data-stu-id="6f068-235">To:</span></span>
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. <span data-ttu-id="6f068-236">Hello Konzola správce balíčků spusťte následující příkaz tooinstall Knockout hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-236">In hello Package Manager Console, run hello following command tooinstall Knockout.</span></span>
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-hello-web-api-restful-interface"></a><span data-ttu-id="6f068-237">Přidání kontroleru rozhraní Web API Restful hello</span><span class="sxs-lookup"><span data-stu-id="6f068-237">Add a controller for hello Web API Restful interface</span></span>
1. <span data-ttu-id="6f068-238">V **Průzkumníku řešení**, klikněte pravým tlačítkem na řadiče a klikněte na **přidat** a potom **řadiče...**</span><span class="sxs-lookup"><span data-stu-id="6f068-238">In **Solution Explorer**, right-click Controllers and click **Add** and then **Controller....**</span></span> 
2. <span data-ttu-id="6f068-239">V hello **přidat vygenerované uživatelské rozhraní** dialogovém okně zadejte **webové 2 kontroler API s akcemi používající rozhraní Entity Framework** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-239">In hello **Add Scaffold** dialog box, enter **Web API 2 Controller with actions, using Entity Framework** and then click **Add**.</span></span>
   
    ![Přidat kontroler API](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. <span data-ttu-id="6f068-241">V hello **přidat kontroler** dialogovém okně zadejte "ContactsController" jako název řadiče.</span><span class="sxs-lookup"><span data-stu-id="6f068-241">In hello **Add Controller** dialog box, enter "ContactsController" as your controller name.</span></span> <span data-ttu-id="6f068-242">Vyberte "Kontakt (ContactManager.Models)" pro hello **třída modelu**.</span><span class="sxs-lookup"><span data-stu-id="6f068-242">Select "Contact (ContactManager.Models)" for hello **Model class**.</span></span>  <span data-ttu-id="6f068-243">Ponechte výchozí hodnotu hello pro hello **třída kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-243">Keep hello default value for hello **Data context class**.</span></span> 
4. <span data-ttu-id="6f068-244">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-244">Click **Add**.</span></span>

### <a name="run-hello-application-locally"></a><span data-ttu-id="6f068-245">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6f068-245">Run hello application locally</span></span>
1. <span data-ttu-id="6f068-246">Stisknutím kombinace kláves CTRL + F5 toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f068-246">Press CTRL+F5 toorun hello application.</span></span>
   
    ![Indexová stránka][intro001]
2. <span data-ttu-id="6f068-248">Zadejte kontakt a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-248">Enter a contact and click **Add**.</span></span> <span data-ttu-id="6f068-249">aplikace Hello vrátí toohello domovské stránce a zobrazí hello kontaktu, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="6f068-249">hello app returns toohello home page and displays hello contact you entered.</span></span>
   
    ![Index stránky s položkami seznamu úkolů][addwebapi004]
3. <span data-ttu-id="6f068-251">V prohlížeči hello připojit **/api/contacts** toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6f068-251">In hello browser, append **/api/contacts** toohello URL.</span></span>
   
    <span data-ttu-id="6f068-252">Výsledná adresa URL Hello bude vypadat http://localhost:1234/api/contacts.</span><span class="sxs-lookup"><span data-stu-id="6f068-252">hello resulting URL will resemble http://localhost:1234/api/contacts.</span></span> <span data-ttu-id="6f068-253">Hello RESTful webová rozhraní API, které jste přidali vrátí hello uložené kontakty.</span><span class="sxs-lookup"><span data-stu-id="6f068-253">hello RESTful web API you added returns hello stored contacts.</span></span> <span data-ttu-id="6f068-254">Firefox) a Chrome (zobrazí hello data ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="6f068-254">Firefox and Chrome will display hello data in XML format.</span></span>
   
    ![Index stránky s položkami seznamu úkolů][rxFFchrome]

    <span data-ttu-id="6f068-256">Aplikace Internet Explorer bude výzvu tooopen nebo ukládat kontakty hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-256">IE will prompt you tooopen or save hello contacts.</span></span>

    ![Dialogové okno Uložit webového rozhraní API][addwebapi006]


    <span data-ttu-id="6f068-258">Můžete otevřít hello kontakty, vrátí se v programu Poznámkový blok nebo prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6f068-258">You can open hello returned contacts in notepad or a browser.</span></span>

    <span data-ttu-id="6f068-259">Tento výstup mohou být spotřebovávána jiná aplikace, jako je například mobilní webové stránky nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f068-259">This output can be consumed by another application such as mobile web page or application.</span></span>

    ![Dialogové okno Uložit webového rozhraní API][addwebapi007]

    <span data-ttu-id="6f068-261">**Upozornění zabezpečení**: V tomto okamžiku je vaše aplikace tooCSRF nezabezpečené a stát terčem útoku.</span><span class="sxs-lookup"><span data-stu-id="6f068-261">**Security Warning**: At this point, your application is insecure and vulnerable tooCSRF attack.</span></span> <span data-ttu-id="6f068-262">Později v kurzu hello jsme se odebrat toto ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6f068-262">Later in hello tutorial we will remove this vulnerability.</span></span> <span data-ttu-id="6f068-263">Další informace najdete v části [útoky brání webů požadavku padělání (proti útokům CSRF)][prevent-csrf-attacks].</span><span class="sxs-lookup"><span data-stu-id="6f068-263">For more information see [Preventing Cross-Site Request Forgery (CSRF) Attacks][prevent-csrf-attacks].</span></span>
## <a name="add-xsrf-protection"></a><span data-ttu-id="6f068-264">Přidat ochranu XSRF</span><span class="sxs-lookup"><span data-stu-id="6f068-264">Add XSRF Protection</span></span>
<span data-ttu-id="6f068-265">Padělání požadavku posílaného mezi weby (také označované jako XSRF nebo proti útokům CSRF) je útok na hostované webové aplikace, které škodlivou webovou stránku můžete ovlivnit hello interakce mezi prohlížeče klienta a důvěřují prohlížeč tohoto webu.</span><span class="sxs-lookup"><span data-stu-id="6f068-265">Cross-site request forgery (also known as XSRF or CSRF) is an attack against web-hosted applications whereby a malicious website can influence hello interaction between a client browser and a website trusted by that browser.</span></span> <span data-ttu-id="6f068-266">Tyto útoky jsou možné, protože webových prohlížečů bude odesílat tokeny ověřování automaticky s každou žádost tooa webu.</span><span class="sxs-lookup"><span data-stu-id="6f068-266">These attacks are made possible because web browsers will send authentication tokens automatically with every request tooa website.</span></span> <span data-ttu-id="6f068-267">Příklad kanonický Hello je soubor cookie ověřování, jako je například ASP. Lístek ověřování pomocí formulářů pro Asp.net.</span><span class="sxs-lookup"><span data-stu-id="6f068-267">hello canonical example is an authentication cookie, such as ASP.NET's Forms Authentication ticket.</span></span> <span data-ttu-id="6f068-268">Tyto útoky však může být cílem weby, které použít žádné trvalé ověřovací mechanismus (například ověřování systému Windows, Basic a tak dále).</span><span class="sxs-lookup"><span data-stu-id="6f068-268">However, websites which use any persistent authentication mechanism (such as Windows Authentication, Basic, and so forth) can be targeted by these attacks.</span></span>

<span data-ttu-id="6f068-269">Útok XSRF se liší od útoky phishing.</span><span class="sxs-lookup"><span data-stu-id="6f068-269">An XSRF attack is distinct from a phishing attack.</span></span> <span data-ttu-id="6f068-270">Útoky phishing nevyžadovaly interakci postižené hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-270">Phishing attacks require interaction from hello victim.</span></span> <span data-ttu-id="6f068-271">V rámci útoku phishing škodlivou webovou stránku bude napodobovat hello cílového webu a postižené hello je oklamat do poskytování útočník toohello citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="6f068-271">In a phishing attack, a malicious website will mimic hello target website, and hello victim is fooled into providing sensitive information toohello attacker.</span></span> <span data-ttu-id="6f068-272">Při útoku XSRF není často žádná interakce potřebné z postižené hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-272">In an XSRF attack, there is often no interaction necessary from hello victim.</span></span> <span data-ttu-id="6f068-273">Místo toho je hello útočník spoléhat na hello prohlížeče automaticky odesílat všechny relevantní soubory cookie toohello cílového webu.</span><span class="sxs-lookup"><span data-stu-id="6f068-273">Rather, hello attacker is relying on hello browser automatically sending all relevant cookies toohello destination website.</span></span>

<span data-ttu-id="6f068-274">Další informace najdete v tématu hello [otevřete projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span><span class="sxs-lookup"><span data-stu-id="6f068-274">For more information, see hello [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span></span>

1. <span data-ttu-id="6f068-275">V **Průzkumníku řešení**, vpravo **ContactManager** projektu a klikněte na tlačítko **přidat** a pak klikněte na **třída**.</span><span class="sxs-lookup"><span data-stu-id="6f068-275">In **Solution Explorer**, right **ContactManager** project and click **Add** and then click **Class**.</span></span>
2. <span data-ttu-id="6f068-276">Název souboru hello *ValidateHttpAntiForgeryTokenAttribute.cs* a přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="6f068-276">Name hello file *ValidateHttpAntiForgeryTokenAttribute.cs* and add hello following code:</span></span>
   
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Helpers;
        using System.Web.Http.Controllers;
        using System.Web.Http.Filters;
        using System.Web.Mvc;
        namespace ContactManager.Filters
        {
            public class ValidateHttpAntiForgeryTokenAttribute : AuthorizationFilterAttribute
            {
                public override void OnAuthorization(HttpActionContext actionContext)
                {
                    HttpRequestMessage request = actionContext.ControllerContext.Request;
                    try
                    {
                        if (IsAjaxRequest(request))
                        {
                            ValidateRequestHeader(request);
                        }
                        else
                        {
                            AntiForgery.Validate();
                        }
                    }
                    catch (HttpAntiForgeryException e)
                    {
                        actionContext.Response = request.CreateErrorResponse(HttpStatusCode.Forbidden, e);
                    }
                }
                private bool IsAjaxRequest(HttpRequestMessage request)
                {
                    IEnumerable<string> xRequestedWithHeaders;
                    if (request.Headers.TryGetValues("X-Requested-With", out xRequestedWithHeaders))
                    {
                        string headerValue = xRequestedWithHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(headerValue))
                        {
                            return String.Equals(headerValue, "XMLHttpRequest", StringComparison.OrdinalIgnoreCase);
                        }
                    }
                    return false;
                }
                private void ValidateRequestHeader(HttpRequestMessage request)
                {
                    string cookieToken = String.Empty;
                    string formToken = String.Empty;
                    IEnumerable<string> tokenHeaders;
                    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
                    {
                        string tokenValue = tokenHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(tokenValue))
                        {
                            string[] tokens = tokenValue.Split(':');
                            if (tokens.Length == 2)
                            {
                                cookieToken = tokens[0].Trim();
                                formToken = tokens[1].Trim();
                            }
                        }
                    }
                    AntiForgery.Validate(cookieToken, formToken);
                }
            }
        }
3. <span data-ttu-id="6f068-277">Přidejte následující hello *pomocí* toohello příkaz měnící řadiče, abyste získali přístup toohello **[ValidateHttpAntiForgeryToken]** atribut.</span><span class="sxs-lookup"><span data-stu-id="6f068-277">Add hello following *using* statement toohello contracts controller so you have access toohello **[ValidateHttpAntiForgeryToken]** attribute.</span></span>
   
        using ContactManager.Filters;
4. <span data-ttu-id="6f068-278">Přidat hello **[ValidateHttpAntiForgeryToken]** atribut metody Post toohello hello **ContactsController** tooprotect z XSRF hrozeb.</span><span class="sxs-lookup"><span data-stu-id="6f068-278">Add hello **[ValidateHttpAntiForgeryToken]** attribute toohello Post methods of hello **ContactsController** tooprotect it from XSRF threats.</span></span> <span data-ttu-id="6f068-279">Můžete ho přidá toohello "PutContact", "PostContact" a **DeleteContact** metody akce.</span><span class="sxs-lookup"><span data-stu-id="6f068-279">You will add it toohello "PutContact",  "PostContact" and **DeleteContact** action methods.</span></span>
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. <span data-ttu-id="6f068-280">Aktualizace hello *skripty* části hello *Views\Home\Index.cshtml* souboru tooinclude kód tooget hello XSRF tokeny.</span><span class="sxs-lookup"><span data-stu-id="6f068-280">Update hello *Scripts* section of hello *Views\Home\Index.cshtml* file tooinclude code tooget hello XSRF tokens.</span></span>
   
         @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                @functions{
                   public string TokenHeaderValue()
                   {
                      string cookieToken, formToken;
                      AntiForgery.GetTokens(null, out cookieToken, out formToken);
                      return cookieToken + ":" + formToken;                
                   }
                }
   
               function ContactsViewModel() {
                  var self = this;
                  self.contacts = ko.observableArray([]);
                  self.addContact = function () {
   
                     $.ajax({
                        type: "post",
                        url: "api/contacts",
                        data: $("#addContact").serialize(),
                        dataType: "json",
                        success: function (value) {
                           self.contacts.push(value);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
                     });
   
                  }
                  self.removeContact = function (contact) {
                     $.ajax({
                        type: "DELETE",
                        url: contact.Self,
                        success: function () {
                           self.contacts.remove(contact);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
   
                     });
                  }
   
                  $.getJSON("api/contacts", function (data) {
                     self.contacts(data);
                  });
               }
               ko.applyBindings(new ContactsViewModel());
            </script>
         }

## <a name="publish-hello-application-update-tooazure-and-sql-database"></a><span data-ttu-id="6f068-281">Publikovat tooAzure aktualizace aplikace hello a databáze SQL</span><span class="sxs-lookup"><span data-stu-id="6f068-281">Publish hello application update tooAzure and SQL Database</span></span>
<span data-ttu-id="6f068-282">aplikace hello toopublish, opakujte hello postupu, který jste postupovali podle dříve.</span><span class="sxs-lookup"><span data-stu-id="6f068-282">toopublish hello application, you repeat hello procedure you followed earlier.</span></span>

1. <span data-ttu-id="6f068-283">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-283">In **Solution Explorer**, right click hello project and select **Publish**.</span></span>
   
    ![Publikování][rxP]
2. <span data-ttu-id="6f068-285">Klikněte na tlačítko hello **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="6f068-285">Click hello **Settings** tab.</span></span>
3. <span data-ttu-id="6f068-286">V části **ContactsManagerContext(ContactsManagerContext)**, klikněte na tlačítko hello **v** ikonu toochange *vzdáleného připojovací řetězec* toohello připojovací řetězec získáte hello databáze.</span><span class="sxs-lookup"><span data-stu-id="6f068-286">Under **ContactsManagerContext(ContactsManagerContext)**, click hello **v** icon toochange *Remote connection string* toohello connection string for hello contact database.</span></span> <span data-ttu-id="6f068-287">Klikněte na tlačítko **ContactDB**.</span><span class="sxs-lookup"><span data-stu-id="6f068-287">Click **ContactDB**.</span></span>
   
    ![Nastavení](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. <span data-ttu-id="6f068-289">Zaškrtněte políčko hello pro **spustit migrace Code First (spuštěno při spuštění aplikace)**.</span><span class="sxs-lookup"><span data-stu-id="6f068-289">Check hello box for **Execute Code First Migrations (runs on application start)**.</span></span>
5. <span data-ttu-id="6f068-290">Klikněte na tlačítko **Další** a pak klikněte na **Preview**.</span><span class="sxs-lookup"><span data-stu-id="6f068-290">Click **Next** and then click **Preview**.</span></span> <span data-ttu-id="6f068-291">Visual Studio zobrazí seznam hello souborů, které bude přidán nebo aktualizován.</span><span class="sxs-lookup"><span data-stu-id="6f068-291">Visual Studio displays a list of hello files that will be added or updated.</span></span>
6. <span data-ttu-id="6f068-292">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6f068-292">Click **Publish**.</span></span>
   <span data-ttu-id="6f068-293">Po dokončení nasazení hello hello prohlížeči se otevře toohello domovskou stránku hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f068-293">After hello deployment completes, hello browser opens toohello home page of hello application.</span></span>
   
    ![Index stránky s žádné kontakty.][intro001]
   
    <span data-ttu-id="6f068-295">Hello Visual Studio publikovat proces automaticky nakonfiguruje hello připojovací řetězec v hello nasazené *Web.config* soubor toopoint toohello SQL database.</span><span class="sxs-lookup"><span data-stu-id="6f068-295">hello Visual Studio publish process automatically configured hello connection string in hello deployed *Web.config* file toopoint toohello SQL database.</span></span> <span data-ttu-id="6f068-296">Také nakonfigurovat migrace Code First tooautomatically upgradu hello toohello nejnovější verze databáze, že hello první čas hello aplikace přistupuje k databázi hello po nasazení.</span><span class="sxs-lookup"><span data-stu-id="6f068-296">It also configured Code First Migrations tooautomatically upgrade hello database toohello latest version hello first time hello application accesses hello database after deployment.</span></span>
   
    <span data-ttu-id="6f068-297">V důsledku této konfiguraci Code First vytvořené databáze hello spuštěním kódu hello v hello **počáteční** třídu, která jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="6f068-297">As a result of this configuration, Code First created hello database by running hello code in hello **Initial** class that you created earlier.</span></span> <span data-ttu-id="6f068-298">Tato hello první čas hello aplikace se pokusila tooaccess hello databáze se nespustil po nasazení.</span><span class="sxs-lookup"><span data-stu-id="6f068-298">It did this hello first time hello application tried tooaccess hello database after deployment.</span></span>
7. <span data-ttu-id="6f068-299">Zadejte kontakt, jako jste to udělali při spuštění místně, aplikace hello tooverify o úspěšném nasazení databáze.</span><span class="sxs-lookup"><span data-stu-id="6f068-299">Enter a contact as you did when you ran hello app locally, tooverify that database deployment succeeded.</span></span>

<span data-ttu-id="6f068-300">Až uvidíte, že hello položku, kterou zadáte je uložena a zobrazí se na stránku hello kontaktujte správce, víte, že byla uložena v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-300">When you see that hello item you enter is saved and appears on hello contact manager page, you know that it has been stored in hello database.</span></span>

![Index stránky s kontakty][addwebapi004]

<span data-ttu-id="6f068-302">Hello aplikace je nyní spuštěna v cloudu hello pomocí SQL Database toostore jeho data.</span><span class="sxs-lookup"><span data-stu-id="6f068-302">hello application is now running in hello cloud, using SQL Database toostore its data.</span></span> <span data-ttu-id="6f068-303">Po dokončení testování hello aplikace v Azure, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="6f068-303">After you finish testing hello application in Azure, delete it.</span></span> <span data-ttu-id="6f068-304">aplikace Hello je veřejný a nemá přístup k toolimit mechanismus.</span><span class="sxs-lookup"><span data-stu-id="6f068-304">hello application is public and doesn't have a mechanism toolimit access.</span></span>

> [!NOTE]
> <span data-ttu-id="6f068-305">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="6f068-305">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="6f068-306">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="6f068-306">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6f068-307">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f068-307">Next Steps</span></span>
<span data-ttu-id="6f068-308">Jiný způsob toostore dat v aplikaci Azure je toouse úložiště Azure, které poskytují úložiště nerelační data ve formuláři hello objekty BLOB a tabulek.</span><span class="sxs-lookup"><span data-stu-id="6f068-308">Another way toostore data in an Azure application is toouse Azure storage, which provide non-relational data storage in hello form of blobs and tables.</span></span> <span data-ttu-id="6f068-309">Následující odkazy Hello poskytují další informace o webového rozhraní API, rozhraní ASP.NET MVC a okno Azure.</span><span class="sxs-lookup"><span data-stu-id="6f068-309">hello following links provide more information on Web API, ASP.NET MVC and Window Azure.</span></span>

* <span data-ttu-id="6f068-310">[Začínáme s MVC pomocí rozhraní Entity Framework][EFCodeFirstMVCTutorial]</span><span class="sxs-lookup"><span data-stu-id="6f068-310">[Getting Started with Entity Framework using MVC][EFCodeFirstMVCTutorial]</span></span>
* [<span data-ttu-id="6f068-311">Úvod tooASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="6f068-311">Intro tooASP.NET MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="6f068-312">První rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="6f068-312">Your First ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [<span data-ttu-id="6f068-313">Ladění WAWS</span><span class="sxs-lookup"><span data-stu-id="6f068-313">Debugging WAWS</span></span>](web-sites-dotnet-troubleshoot-visual-studio.md)

<span data-ttu-id="6f068-314">Ukázkovou aplikaci tohoto kurzu a hello napsal [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) s požádat o pomoc tní Dykstra a Jiří Dorrans (Twitter [ @blowdart ](https://twitter.com/blowdart)).</span><span class="sxs-lookup"><span data-stu-id="6f068-314">This tutorial and hello sample application was written by [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) with assistance from Tom Dykstra and Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart)).</span></span> 

<span data-ttu-id="6f068-315">Na co líbilo nebo co chcete toosee zlepšila, jenom o hello kurzu sám sebe, ale taky o hello produkty, které ukazuje prosím sdělit svůj názor.</span><span class="sxs-lookup"><span data-stu-id="6f068-315">Please leave feedback on what you liked or what you would like toosee improved, not only about hello tutorial itself but also about hello products that it demonstrates.</span></span> <span data-ttu-id="6f068-316">Vaše zpětná vazba pomůže stanovení priorit vylepšení.</span><span class="sxs-lookup"><span data-stu-id="6f068-316">Your feedback will help us prioritize improvements.</span></span> <span data-ttu-id="6f068-317">Zejména zajímá zjistit, kolik vás zajímají je v další automatizace procesu hello konfigurace a nasazení databáze členství hello.</span><span class="sxs-lookup"><span data-stu-id="6f068-317">We are especially interested in finding out how much interest there is in more automation for hello process of configuring and deploying hello membership database.</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="6f068-318">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="6f068-318">What's changed</span></span>
* <span data-ttu-id="6f068-319">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="6f068-319">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles toohello Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update hello Membership Database]:#ppd2
[setupdbenv]: #bkmk_setupdevenv
[setupwindowsazureenv]: #bkmk_setupwindowsazure
[createapplication]: #bkmk_createmvc4app
[deployapp1]: #bkmk_deploytowindowsazure1
[adddb]: #bkmk_addadatabase
[addcontroller]: #bkmk_addcontroller
[addwebapi]: #bkmk_addwebapi
[deploy2]: #bkmk_deploydatabaseupdate

<!-- links -->
[EFCodeFirstMVCTutorial]: http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
[dbcontext-link]: http://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx


<!-- images-->
[rxE]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxE.png
[rxP]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxP.png
[rx22]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/
[rxb2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxb2.png
[rxz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz.png
[rxzz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxzz.png
[rxz2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz2.png
[rxz3]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz3.png
[rxStyle]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxStyle.png
[rxz4]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz4.png
[rxz44]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz44.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxPrevDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPrevDB.png
[rxOverwrite]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxOverwrite.png
[rxPWS]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPWS.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxAddApiController]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxAddApiController.png
[rxFFchrome]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxFFchrome.png
[intro001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobil-intro-finished-web-app.png
[rxCreateWSwithDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxCreateWSwithDB.png
[setup007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-setup-azure-site-004.png
[setup009]: ../Media/dntutmobile-setup-azure-site-006.png
[newapp002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-002.png
[newapp004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-004.png
[firsdeploy007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-005.png
[firsdeploy009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-007.png
[adddb001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-001.png
[adddb002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-002.png
[addcode001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-context-menu.png
[addcode002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-controller-dialog.png
[addcode004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-index-context.png
[addcode005]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-contents-context-menu.png
[addcode007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-bundleconfig-context.png
[addcode008]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-menu.png
[addcode009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-console.png
[addwebapi004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-added-contact.png
[addwebapi006]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-save-returned-contacts.png
[addwebapi007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-contacts-in-notepad.png
[Add XSRF Protection]: #xsrf
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[Add XSRF Protection]: #xsrf
[ImportPublishSettings]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishSettings.png
[ImportPublishProfile]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishProfile.png
[PublishVSSolution]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/PublishVSSolution.png
[ValidateConnection]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ValidateConnection.png
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[prevent-csrf-attacks]: http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-(csrf)-attacks

