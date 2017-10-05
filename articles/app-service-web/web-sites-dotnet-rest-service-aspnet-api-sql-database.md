---
title: "Vytvořit rozhraní REST API v Azure pomocí technologie ASP.NET a SQL DB | Microsoft Docs"
description: "Kurz, který se naučíte, jak nasadit aplikaci, která používá rozhraní ASP.NET Web API pro webové aplikace Azure pomocí sady Visual Studio."
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
ms.openlocfilehash: 64c18f2cfabbb7af6ffd89b4c2a9095fca1cf799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a><span data-ttu-id="625c2-103">Vytvoření služby pomocí rozhraní ASP.NET Web API a databázi SQL v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="625c2-103">Create a REST service using ASP.NET Web API and SQL Database in Azure App Service</span></span>
<span data-ttu-id="625c2-104">Tento kurz ukazuje, jak nasadit webové aplikace ASP.NET do [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) pomocí Průvodce Publikovat Web v sadě Visual Studio 2013 nebo Visual Studio 2013 Community Edition.</span><span class="sxs-lookup"><span data-stu-id="625c2-104">This tutorial shows how to deploy an ASP.NET web app to an [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by using the Publish Web wizard in Visual Studio 2013 or Visual Studio 2013 Community Edition.</span></span> 

<span data-ttu-id="625c2-105">Můžete otevřít účet Azure zdarma a pokud ještě nemáte Visual Studio 2013, sady SDK automaticky nainstaluje Visual Studio 2013 pro produkt Web Express.</span><span class="sxs-lookup"><span data-stu-id="625c2-105">You can open an Azure account for free, and if you don't already have Visual Studio 2013, the SDK automatically installs Visual Studio 2013 for Web Express.</span></span> <span data-ttu-id="625c2-106">Proto můžete spustit vývoj pro Azure zcela zdarma.</span><span class="sxs-lookup"><span data-stu-id="625c2-106">So you can start developing for Azure entirely for free.</span></span>

<span data-ttu-id="625c2-107">Tento kurz předpokládá, že máte žádné předchozí zkušenosti s používáním Azure.</span><span class="sxs-lookup"><span data-stu-id="625c2-107">This tutorial assumes that you have no prior experience using Azure.</span></span> <span data-ttu-id="625c2-108">Po dokončení tohoto kurzu, budete mít jednoduché webové aplikace nahoru a běží v cloudu.</span><span class="sxs-lookup"><span data-stu-id="625c2-108">On completing this tutorial, you'll have a simple web app up and running in the cloud.</span></span>

<span data-ttu-id="625c2-109">Naučíte se:</span><span class="sxs-lookup"><span data-stu-id="625c2-109">You'll learn:</span></span>

* <span data-ttu-id="625c2-110">Postup zprovoznění počítače pro vývoj na platformě Azure nainstalováním sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="625c2-110">How to enable your machine for Azure development by installing the Azure SDK.</span></span>
* <span data-ttu-id="625c2-111">Postup vytvoření projektu Visual Studio ASP.NET MVC 5 a publikujete ho v aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="625c2-111">How to create a Visual Studio ASP.NET MVC 5 project and publish it to an Azure app.</span></span>
* <span data-ttu-id="625c2-112">Jak používat rozhraní ASP.NET Web API umožňující volání rozhraní Restful API.</span><span class="sxs-lookup"><span data-stu-id="625c2-112">How to use the ASP.NET Web API to enable Restful API calls.</span></span>
* <span data-ttu-id="625c2-113">Jak používat databázi SQL pro ukládání dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="625c2-113">How to use a SQL database to store data in Azure.</span></span>
* <span data-ttu-id="625c2-114">Jak publikovat aplikaci aktualizací do Azure.</span><span class="sxs-lookup"><span data-stu-id="625c2-114">How to publish application updates to Azure.</span></span>

<span data-ttu-id="625c2-115">Budete vytvářet jednoduché seznamu kontaktů webovou aplikaci, která je založená na technologii ASP.NET MVC 5 a používá ADO.NET Entity Framework pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="625c2-115">You'll build a simple contact list web application that is built on ASP.NET MVC 5 and uses the ADO.NET Entity Framework for database access.</span></span> <span data-ttu-id="625c2-116">Na následujícím obrázku je vidět hotová aplikace:</span><span class="sxs-lookup"><span data-stu-id="625c2-116">The following illustration shows the completed application:</span></span>

![snímek obrazovky webové stránky][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-the-project"></a><span data-ttu-id="625c2-118">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="625c2-118">Create the project</span></span>
1. <span data-ttu-id="625c2-119">Spusťte Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="625c2-119">Start Visual Studio 2013.</span></span>
2. <span data-ttu-id="625c2-120">Z **soubor** nabídce klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="625c2-120">From the **File** menu click **New Project**.</span></span>
3. <span data-ttu-id="625c2-121">V **nový projekt** dialogové okno, rozbalte seznam **Visual C#** a vyberte **webové** a pak vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="625c2-121">In the **New Project** dialog box, expand **Visual C#** and select **Web**  and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="625c2-122">Název aplikace **ContactManager** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="625c2-122">Name the application **ContactManager** and click **OK**.</span></span>
   
    ![Dialogové okno Nový projekt](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. <span data-ttu-id="625c2-124">V **nový projekt ASP.NET** dialogové okno, vyberte **MVC** šablony, zkontrolujte **webového rozhraní API** a pak klikněte na **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="625c2-124">In the **New ASP.NET Project** dialog box, select the **MVC** template, check **Web API** and then click **Change Authentication**.</span></span>
5. <span data-ttu-id="625c2-125">V dialogovém okně **Změna ověřování** klikněte na možnost **Bez ověřování** a poté klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="625c2-125">In the **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span>
   
    ![Bez ověřování](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    <span data-ttu-id="625c2-127">Ukázkovou aplikaci, kterou vytváříte, nebude mít funkcí, které vyžadují přihlášení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="625c2-127">The sample application you're creating won't have features that require users to log in.</span></span> <span data-ttu-id="625c2-128">Informace o tom, jak implementovat ověřování a autorizace funkce najdete v tématu [další kroky](#nextsteps) na konci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="625c2-128">For information about how to implement authentication and authorization features, see the [Next Steps](#nextsteps) section at the end of this tutorial.</span></span> 
6. <span data-ttu-id="625c2-129">V **nový projekt ASP.NET** dialogové okno zkontrolujte, zda **hostitel v cloudu** je zaškrtnuté políčko a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="625c2-129">In the **New ASP.NET Project** dialog box, make sure the **Host in the Cloud** is checked and click **OK**.</span></span>

<span data-ttu-id="625c2-130">Pokud nejste přihlášeni dříve do Azure, budete vyzváni k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="625c2-130">If you have not previously signed in to Azure, you will be prompted to sign in.</span></span>

1. <span data-ttu-id="625c2-131">Průvodce konfigurací navrhne jedinečný název založený na *ContactManager* (viz následující obrázek).</span><span class="sxs-lookup"><span data-stu-id="625c2-131">The configuration wizard will suggest a unique name based on *ContactManager* (see the image below).</span></span> <span data-ttu-id="625c2-132">Vyberte oblast okolo vás.</span><span class="sxs-lookup"><span data-stu-id="625c2-132">Select a region near you.</span></span> <span data-ttu-id="625c2-133">Můžete použít [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") najít datovém centru nejnižší latenci.</span><span class="sxs-lookup"><span data-stu-id="625c2-133">You can use [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") to find the lowest latency data center.</span></span> 
2. <span data-ttu-id="625c2-134">Pokud jste dosud nevytvořili databázový server před, vyberte **vytvořit nový server**, zadejte jméno uživatele databáze a heslo.</span><span class="sxs-lookup"><span data-stu-id="625c2-134">If you haven't created a database server before, select **Create new server**, enter a database user name and password.</span></span>
   
    ![Konfigurace webu Azure](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

<span data-ttu-id="625c2-136">Pokud máte databázový server, použijte k vytvoření nové databáze.</span><span class="sxs-lookup"><span data-stu-id="625c2-136">If you have a database server, use that to create a new database.</span></span> <span data-ttu-id="625c2-137">Databázové servery jsou drahocenný prostředků a chcete obecně vytvořit více databází na stejném serveru pro testování a vývoj, nikoli databázový server na databázi.</span><span class="sxs-lookup"><span data-stu-id="625c2-137">Database servers are a precious resource, and you generally want to create multiple databases on the same server for testing and development rather than creating a database server per database.</span></span> <span data-ttu-id="625c2-138">Ujistěte se, že webový server a databáze jsou ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="625c2-138">Make sure your web site and database are in the same region.</span></span>

![Konfigurace webu Azure](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-the-page-header-and-footer"></a><span data-ttu-id="625c2-140">Nastavit záhlaví a zápatí stránky</span><span class="sxs-lookup"><span data-stu-id="625c2-140">Set the page header and footer</span></span>
1. <span data-ttu-id="625c2-141">V **Průzkumníku řešení**, rozbalte *Views\Shared* složky a otevřete *_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="625c2-141">In **Solution Explorer**, expand the *Views\Shared* folder and open the *_Layout.cshtml* file.</span></span>
   
    ![_Layout.cshtml v Průzkumníku řešení][newapp004]
2. <span data-ttu-id="625c2-143">Nahraďte obsah *Views\Shared_Layout.cshtml* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="625c2-143">Replace the contents of the *Views\Shared_Layout.cshtml* file with the following code:</span></span>

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

<span data-ttu-id="625c2-144">Výše uvedený kód změní název aplikace z "Moje aplikace technologie ASP.NET" na "Obraťte se na správce" a odebere odkazy na **Domů**, **o** a **kontaktujte**.</span><span class="sxs-lookup"><span data-stu-id="625c2-144">The markup above changes the app name from "My ASP.NET App" to "Contact Manager", and it removes the links to **Home**, **About** and **Contact**.</span></span>

### <a name="run-the-application-locally"></a><span data-ttu-id="625c2-145">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="625c2-145">Run the application locally</span></span>
1. <span data-ttu-id="625c2-146">Stiskněte klávesy CTRL+F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="625c2-146">Press CTRL+F5 to run the application.</span></span>
   <span data-ttu-id="625c2-147">Domovská stránka aplikace se zobrazí výchozí prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="625c2-147">The application home page appears in the default browser.</span></span>
    <span data-ttu-id="625c2-148">![Seznam úkolů domovskou stránku](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span><span class="sxs-lookup"><span data-stu-id="625c2-148">![To Do List home page](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span></span>

<span data-ttu-id="625c2-149">To je vše, je potřeba udělat teď k vytvoření aplikace, které nasadíte do Azure.</span><span class="sxs-lookup"><span data-stu-id="625c2-149">This is all you need to do for now to create the application that you'll deploy to Azure.</span></span> <span data-ttu-id="625c2-150">Později přidáte funkce databáze.</span><span class="sxs-lookup"><span data-stu-id="625c2-150">Later you'll add database functionality.</span></span>

## <a name="deploy-the-application-to-azure"></a><span data-ttu-id="625c2-151">Nasazení aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="625c2-151">Deploy the application to Azure</span></span>
1. <span data-ttu-id="625c2-152">V sadě Visual Studio, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **publikovat** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="625c2-152">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
   
    ![Publikování v kontextové nabídce projektu][PublishVSSolution]
   
    <span data-ttu-id="625c2-154">**Publikovat Web** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="625c2-154">The **Publish Web** wizard opens.</span></span>
2. <span data-ttu-id="625c2-155">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-155">Click **Publish**.</span></span>

![Karta nastavení](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

<span data-ttu-id="625c2-157">Visual Studio spustí proces kopírování souborů na Azure server.</span><span class="sxs-lookup"><span data-stu-id="625c2-157">Visual Studio begins the process of copying the files to the Azure server.</span></span> <span data-ttu-id="625c2-158">**Výstup** okno zobrazuje, jaké akce nasazení byly provedeny a hlásí úspěšné dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="625c2-158">The **Output** window shows what deployment actions were taken and reports successful completion of the deployment.</span></span>

1. <span data-ttu-id="625c2-159">Na adresu URL nasazené lokality se automaticky otevře výchozí prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="625c2-159">The default browser automatically opens to the URL of the deployed site.</span></span>
   
   <span data-ttu-id="625c2-160">Aplikace, kterou jste vytvořili je nyní spuštěna v cloudu.</span><span class="sxs-lookup"><span data-stu-id="625c2-160">The application you created is now running in the cloud.</span></span>
   
   ![Seznam úkolů domovskou stránku běžící v Azure][rxz2]

## <a name="add-a-database-to-the-application"></a><span data-ttu-id="625c2-162">Přidání databáze do aplikace</span><span class="sxs-lookup"><span data-stu-id="625c2-162">Add a database to the application</span></span>
<span data-ttu-id="625c2-163">Dále budete aktualizovat aplikaci MVC přidáte možnost zobrazit a aktualizovat kontakty a uložení dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="625c2-163">Next, you'll update the MVC application to add the ability to display and update contacts and store the data in a database.</span></span> <span data-ttu-id="625c2-164">Aplikace bude používat rozhraní Entity Framework k vytvoření databáze a k číst a aktualizovat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="625c2-164">The application will use the Entity Framework to create the database and to read and update data in the database.</span></span>

### <a name="add-data-model-classes-for-the-contacts"></a><span data-ttu-id="625c2-165">Přidání třídy modelu dat pro kontaktů</span><span class="sxs-lookup"><span data-stu-id="625c2-165">Add data model classes for the contacts</span></span>
<span data-ttu-id="625c2-166">Začněte vytvořením jednoduchého datového modelu v kódu.</span><span class="sxs-lookup"><span data-stu-id="625c2-166">You begin by creating a simple data model in code.</span></span>

1. <span data-ttu-id="625c2-167">V **Průzkumníku řešení**, klikněte pravým tlačítkem na složku modely, klikněte na **přidat**a potom **třída**.</span><span class="sxs-lookup"><span data-stu-id="625c2-167">In **Solution Explorer**, right-click the Models folder, click **Add**, and then **Class**.</span></span>
   
    ![Přidání třídy v kontextové nabídce složku modely][adddb001]
2. <span data-ttu-id="625c2-169">V **přidat novou položku** dialogové okno, název nového souboru třídy *Contact.cs*a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-169">In the **Add New Item** dialog box, name the new class file *Contact.cs*, and then click **Add**.</span></span>
   
    ![Přidat novou položku – dialogové okno][adddb002]
3. <span data-ttu-id="625c2-171">Nahraďte obsah souboru Contacts.cs následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="625c2-171">Replace the contents of the Contacts.cs file with the following code.</span></span>
   
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

<span data-ttu-id="625c2-172">**Obraťte se na** třídy definují data, která se uloží pro každý kontakt a primární klíč, KódKontaktu, který je nutný pro databázi.</span><span class="sxs-lookup"><span data-stu-id="625c2-172">The **Contact** class defines the data that you will store for each contact, plus a primary key, ContactID, that is needed by the database.</span></span> <span data-ttu-id="625c2-173">Můžete získat další informace o datových modelech v [další kroky](#nextsteps) na konci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="625c2-173">You can get more information about data models in the [Next Steps](#nextsteps) section at the end of this tutorial.</span></span>

### <a name="create-web-pages-that-enable-app-users-to-work-with-the-contacts"></a><span data-ttu-id="625c2-174">Vytvoření webové stránky, které umožňují uživatelům aplikace pro práci s kontaktů</span><span class="sxs-lookup"><span data-stu-id="625c2-174">Create web pages that enable app users to work with the contacts</span></span>
<span data-ttu-id="625c2-175">ASP.NET MVC funkci generování uživatelského rozhraní můžete automaticky generovat kód, který provádí vytvářet, číst, aktualizovat a odstraňovat akcemi (CRUD).</span><span class="sxs-lookup"><span data-stu-id="625c2-175">The ASP.NET MVC the scaffolding feature can automatically generate code that performs create, read, update, and delete (CRUD) actions.</span></span>

## <a name="add-a-controller-and-a-view-for-the-data"></a><span data-ttu-id="625c2-176">Přidání Kontroleru a zobrazení dat</span><span class="sxs-lookup"><span data-stu-id="625c2-176">Add a Controller and a view for the data</span></span>
1. <span data-ttu-id="625c2-177">V **Průzkumníku**, rozbalte složku řadiče.</span><span class="sxs-lookup"><span data-stu-id="625c2-177">In **Solution Explorer**, expand the Controllers folder.</span></span>
2. <span data-ttu-id="625c2-178">Sestavení projektu **(Ctrl + Shift + B)**.</span><span class="sxs-lookup"><span data-stu-id="625c2-178">Build the project **(Ctrl+Shift+B)**.</span></span> <span data-ttu-id="625c2-179">(Před použitím mechanismus generování uživatelského rozhraní musí sestavte projekt.)</span><span class="sxs-lookup"><span data-stu-id="625c2-179">(You must build the project before using scaffolding mechanism.)</span></span> 
3. <span data-ttu-id="625c2-180">Klikněte pravým tlačítkem na složku řadiče a klikněte na tlačítko **přidat**a potom klikněte na **řadič**.</span><span class="sxs-lookup"><span data-stu-id="625c2-180">Right-click the Controllers folder and click **Add**, and then click **Controller**.</span></span>
   
    ![Přidat řadič v kontextové nabídce řadiče složky][addcode001]
4. <span data-ttu-id="625c2-182">V **přidat vygenerované uživatelské rozhraní** dialogové okno, vyberte **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-182">In the **Add Scaffold** dialog box, select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
   
   ![Přidání kontroleru](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. <span data-ttu-id="625c2-184">Nastavte název řadiče na **HomeController**.</span><span class="sxs-lookup"><span data-stu-id="625c2-184">Set the controller name to **HomeController**.</span></span> <span data-ttu-id="625c2-185">Vyberte **kontaktujte** jako třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="625c2-185">Select **Contact** as your model class.</span></span> <span data-ttu-id="625c2-186">Klikněte **nový kontext dat** tlačítko a přijměte výchozí nastavení "ContactManager.Models.ContactManagerContext" pro **nový typ kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-186">Click the **New data context** button and accept the default "ContactManager.Models.ContactManagerContext" for the **New data context type**.</span></span> <span data-ttu-id="625c2-187">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-187">Click **Add**.</span></span>

    <span data-ttu-id="625c2-188">Dialogové okno zobrazí výzvu: "soubor s názvem HomeController již ukončí.</span><span class="sxs-lookup"><span data-stu-id="625c2-188">A dialog box will prompt you: "A file with the name HomeController already exits.</span></span> <span data-ttu-id="625c2-189">Chcete ho nahradit? ".</span><span class="sxs-lookup"><span data-stu-id="625c2-189">Do you want to replace it?".</span></span> <span data-ttu-id="625c2-190">Klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="625c2-190">Click **Yes**.</span></span> <span data-ttu-id="625c2-191">Přepisování jsme řadič Domů, který byl vytvořen nový projekt.</span><span class="sxs-lookup"><span data-stu-id="625c2-191">We are overwriting the Home Controller that was created with the new project.</span></span> <span data-ttu-id="625c2-192">Nový řadič Domů budeme používat pro naše seznamu kontaktů.</span><span class="sxs-lookup"><span data-stu-id="625c2-192">We will use the new Home Controller for our contact list.</span></span>

    <span data-ttu-id="625c2-193">Visual Studio vytvoří metody kontroleru a zobrazení pro operace CRUD databáze pro **kontaktujte** objekty.</span><span class="sxs-lookup"><span data-stu-id="625c2-193">Visual Studio creates controller methods and views for CRUD database operations for **Contact** objects.</span></span>

## <a name="enable-migrations-create-the-database-add-sample-data-and-a-data-initializer"></a><span data-ttu-id="625c2-194">Povolit migrace a vytvořit databázi, Přidání ukázkových dat a inicializátoru dat</span><span class="sxs-lookup"><span data-stu-id="625c2-194">Enable Migrations, create the database, add sample data and a data initializer</span></span>
<span data-ttu-id="625c2-195">Dalším krokem je povolit [migrace Code First](http://curah.microsoft.com/55220) funkci pro vytvoření databáze založené na datový model, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="625c2-195">The next task is to enable the [Code First Migrations](http://curah.microsoft.com/55220) feature in order to create the database based on the data model you created.</span></span>

1. <span data-ttu-id="625c2-196">V **nástroje** nabídce vyberte možnost **Správce balíčků knihoven** a potom **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="625c2-196">In the **Tools** menu, select **Library Package Manager** and then **Package Manager Console**.</span></span>
   
    ![Konzola správce balíčků v nabídce Nástroje][addcode008]
2. <span data-ttu-id="625c2-198">V **Konzola správce balíčků** okno, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="625c2-198">In the **Package Manager Console** window, enter the following command:</span></span>
   
        enable-migrations 
   
    <span data-ttu-id="625c2-199">**Enable-migrations se** příkaz vytvoří *migrace* složku a její uloží je v této složce *Configuration.cs* soubor, který můžete upravit konfigurace migrací.</span><span class="sxs-lookup"><span data-stu-id="625c2-199">The **enable-migrations** command creates a *Migrations* folder and it puts in that folder a *Configuration.cs* file that you can edit to configure Migrations.</span></span> 
3. <span data-ttu-id="625c2-200">V **Konzola správce balíčků** okno, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="625c2-200">In the **Package Manager Console** window, enter the following command:</span></span>
   
        add-migration Initial
   
    <span data-ttu-id="625c2-201">**Přidat migrace počáteční** příkaz vygeneruje třídy s názvem  **&lt;date_stamp&gt;počáteční** vytvářející databáze.</span><span class="sxs-lookup"><span data-stu-id="625c2-201">The **add-migration Initial** command generates a class named **&lt;date_stamp&gt;Initial** that creates the database.</span></span> <span data-ttu-id="625c2-202">První parametr ( *počáteční* ) je libovolný a slouží k vytvoření názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="625c2-202">The first parameter ( *Initial* ) is arbitrary and used to create the name of the file.</span></span> <span data-ttu-id="625c2-203">Zobrazí se nové soubory tříd v **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="625c2-203">You can see the new class files in **Solution Explorer**.</span></span>
   
    <span data-ttu-id="625c2-204">V **počáteční** třídy, **až** metoda vytvoří tabulku kontaktů a **dolů** – metoda (používá, pokud chcete vrátit do předchozího stavu) se zahodí.</span><span class="sxs-lookup"><span data-stu-id="625c2-204">In the **Initial** class, the **Up** method creates the Contacts table, and the **Down** method (used when you want to return to the previous state) drops it.</span></span>
4. <span data-ttu-id="625c2-205">Otevřete *Migrations\Configuration.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="625c2-205">Open the *Migrations\Configuration.cs* file.</span></span> 
5. <span data-ttu-id="625c2-206">Přidejte následující obory názvů.</span><span class="sxs-lookup"><span data-stu-id="625c2-206">Add the following namespaces.</span></span> 
   
         using ContactManager.Models;
6. <span data-ttu-id="625c2-207">Nahraďte *počáteční hodnoty* metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="625c2-207">Replace the *Seed* method with the following code:</span></span>
   
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
   
    <span data-ttu-id="625c2-208">Tento kód výše inicializuje databázi s kontaktní informace.</span><span class="sxs-lookup"><span data-stu-id="625c2-208">This code above will initialize the database with the contact information.</span></span> <span data-ttu-id="625c2-209">Další informace o synchronizace replik indexů databáze najdete v tématu [ladění Entity Framework (EF) databází](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span><span class="sxs-lookup"><span data-stu-id="625c2-209">For more information on seeding the database, see [Debugging Entity Framework (EF) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span></span>
7. <span data-ttu-id="625c2-210">V **Konzola správce balíčků** zadejte příkaz:</span><span class="sxs-lookup"><span data-stu-id="625c2-210">In the **Package Manager Console** enter the command:</span></span>
   
        update-database
   
    ![Konzola správce balíčků příkazy][addcode009]
   
    <span data-ttu-id="625c2-212">**Update-database** spustí první migrace, která vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="625c2-212">The **update-database** runs the first migration which creates the database.</span></span> <span data-ttu-id="625c2-213">Databáze je ve výchozím nastavení vytvoří jako databáze SQL serveru Express LocalDB.</span><span class="sxs-lookup"><span data-stu-id="625c2-213">By default, the database is created as a SQL Server Express LocalDB database.</span></span>
8. <span data-ttu-id="625c2-214">Stiskněte klávesy CTRL+F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="625c2-214">Press CTRL+F5 to run the application.</span></span> 

<span data-ttu-id="625c2-215">Aplikace zobrazuje počáteční hodnoty data a poskytuje odkazy upravit, podrobnosti a odstranění.</span><span class="sxs-lookup"><span data-stu-id="625c2-215">The application shows the seed data and provides edit, details and delete links.</span></span>

![Zobrazení MVC dat][rxz3]

## <a name="edit-the-view"></a><span data-ttu-id="625c2-217">Upravit zobrazení</span><span class="sxs-lookup"><span data-stu-id="625c2-217">Edit the View</span></span>
1. <span data-ttu-id="625c2-218">Otevřete *Views\Home\Index.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="625c2-218">Open the *Views\Home\Index.cshtml* file.</span></span> <span data-ttu-id="625c2-219">V dalším kroku, jsme nahradí generovaný kód s kódem, který používá [jQuery](http://jquery.com/) a [Knockout.js](http://knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="625c2-219">In the next step, we will replace the generated markup with code that uses [jQuery](http://jquery.com/) and [Knockout.js](http://knockoutjs.com/).</span></span> <span data-ttu-id="625c2-220">Tento nový kód načte seznam kontaktů pomocí webového rozhraní API a JSON a pak uživatelského rozhraní pomocí knockout.js sváže kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="625c2-220">This new code retrieves the list of contacts from using web API and JSON and then binds the contact data to the UI using knockout.js.</span></span> <span data-ttu-id="625c2-221">Další informace najdete v tématu [další kroky](#nextsteps) na konci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="625c2-221">For more information, see the [Next Steps](#nextsteps) section at the end of this tutorial.</span></span> 
2. <span data-ttu-id="625c2-222">Obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="625c2-222">Replace the contents of the file with the following code.</span></span>
   
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
3. <span data-ttu-id="625c2-223">Klikněte pravým tlačítkem na složku obsahu a klikněte na tlačítko **přidat**a pak klikněte na tlačítko **novou položku...** .</span><span class="sxs-lookup"><span data-stu-id="625c2-223">Right-click the Content folder and click **Add**, and then click **New Item...**.</span></span>
   
    ![Přidání šablony stylů v kontextové nabídce složky obsahu][addcode005]
4. <span data-ttu-id="625c2-225">V **přidat novou položku** dialogovém okně zadejte **styl** v horní pravé pole pro vyhledávání a pak vyberte **list stylu**.</span><span class="sxs-lookup"><span data-stu-id="625c2-225">In the **Add New Item** dialog box, enter **Style** in the upper right search box and then select **Style Sheet**.</span></span>
    <span data-ttu-id="625c2-226">![Přidat novou položku – dialogové okno][rxStyle]</span><span class="sxs-lookup"><span data-stu-id="625c2-226">![Add New Item dialog box][rxStyle]</span></span>
5. <span data-ttu-id="625c2-227">Název souboru *Contacts.css* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-227">Name the file *Contacts.css* and click **Add**.</span></span> <span data-ttu-id="625c2-228">Obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="625c2-228">Replace the contents of the file with the following code.</span></span>
   
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
   
    <span data-ttu-id="625c2-229">Budeme používat tuto šablonu stylů pro rozložení, barvy a styly využívané v aplikaci kontaktujte správce.</span><span class="sxs-lookup"><span data-stu-id="625c2-229">We will use this style sheet for the layout, colors and styles used in the contact manager app.</span></span>
6. <span data-ttu-id="625c2-230">Otevřete *App_Start\BundleConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="625c2-230">Open the *App_Start\BundleConfig.cs* file.</span></span>
7. <span data-ttu-id="625c2-231">Přidejte následující kód k registraci [Knockout](http://knockoutjs.com/index.html "KO") modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="625c2-231">Add the following code to register the [Knockout](http://knockoutjs.com/index.html "KO") plugin.</span></span>
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    <span data-ttu-id="625c2-232">Tato ukázka pomocí knockout zjednodušit dynamické kódu jazyka JavaScript, která zpracovává šablony obrazovky.</span><span class="sxs-lookup"><span data-stu-id="625c2-232">This sample using knockout to simplify dynamic JavaScript code that handles the screen templates.</span></span>
8. <span data-ttu-id="625c2-233">Upravit obsah nebo šablon stylů css položka registrace *contacts.css* šabloně stylů.</span><span class="sxs-lookup"><span data-stu-id="625c2-233">Modify the contents/css entry to register the *contacts.css* style sheet.</span></span> <span data-ttu-id="625c2-234">Změňte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="625c2-234">Change the following line:</span></span>
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   <span data-ttu-id="625c2-235">na:</span><span class="sxs-lookup"><span data-stu-id="625c2-235">To:</span></span>
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. <span data-ttu-id="625c2-236">V konzole Správce balíčků, spusťte následující příkaz k instalaci Knockout.</span><span class="sxs-lookup"><span data-stu-id="625c2-236">In the Package Manager Console, run the following command to install Knockout.</span></span>
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-the-web-api-restful-interface"></a><span data-ttu-id="625c2-237">Přidat řadič pro rozhraní Restful webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="625c2-237">Add a controller for the Web API Restful interface</span></span>
1. <span data-ttu-id="625c2-238">V **Průzkumníku řešení**, klikněte pravým tlačítkem na řadiče a klikněte na **přidat** a potom **řadiče...**</span><span class="sxs-lookup"><span data-stu-id="625c2-238">In **Solution Explorer**, right-click Controllers and click **Add** and then **Controller....**</span></span> 
2. <span data-ttu-id="625c2-239">V **přidat vygenerované uživatelské rozhraní** dialogovém okně zadejte **webové 2 kontroler API s akcemi používající rozhraní Entity Framework** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-239">In the **Add Scaffold** dialog box, enter **Web API 2 Controller with actions, using Entity Framework** and then click **Add**.</span></span>
   
    ![Přidat kontroler API](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. <span data-ttu-id="625c2-241">V **přidat kontroler** dialogovém okně zadejte "ContactsController" jako název řadiče.</span><span class="sxs-lookup"><span data-stu-id="625c2-241">In the **Add Controller** dialog box, enter "ContactsController" as your controller name.</span></span> <span data-ttu-id="625c2-242">Vyberte "Kontakt (ContactManager.Models)" pro **třída modelu**.</span><span class="sxs-lookup"><span data-stu-id="625c2-242">Select "Contact (ContactManager.Models)" for the **Model class**.</span></span>  <span data-ttu-id="625c2-243">Ponechte výchozí hodnotu pro **třída kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-243">Keep the default value for the **Data context class**.</span></span> 
4. <span data-ttu-id="625c2-244">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-244">Click **Add**.</span></span>

### <a name="run-the-application-locally"></a><span data-ttu-id="625c2-245">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="625c2-245">Run the application locally</span></span>
1. <span data-ttu-id="625c2-246">Stiskněte klávesy CTRL+F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="625c2-246">Press CTRL+F5 to run the application.</span></span>
   
    ![Indexová stránka][intro001]
2. <span data-ttu-id="625c2-248">Zadejte kontakt a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-248">Enter a contact and click **Add**.</span></span> <span data-ttu-id="625c2-249">Aplikace vrací na domovskou stránku a zobrazí kontaktu, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="625c2-249">The app returns to the home page and displays the contact you entered.</span></span>
   
    ![Index stránky s položkami seznamu úkolů][addwebapi004]
3. <span data-ttu-id="625c2-251">V prohlížeči připojit **/api/contacts** na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="625c2-251">In the browser, append **/api/contacts** to the URL.</span></span>
   
    <span data-ttu-id="625c2-252">Výsledná adresa URL bude vypadat http://localhost:1234/api/contacts.</span><span class="sxs-lookup"><span data-stu-id="625c2-252">The resulting URL will resemble http://localhost:1234/api/contacts.</span></span> <span data-ttu-id="625c2-253">RESTful jste přidali webové rozhraní API vrátí uložené kontakty.</span><span class="sxs-lookup"><span data-stu-id="625c2-253">The RESTful web API you added returns the stored contacts.</span></span> <span data-ttu-id="625c2-254">Firefox) a Chrome (se zobrazí data ve formátu XML.</span><span class="sxs-lookup"><span data-stu-id="625c2-254">Firefox and Chrome will display the data in XML format.</span></span>
   
    ![Index stránky s položkami seznamu úkolů][rxFFchrome]

    <span data-ttu-id="625c2-256">IE vás vyzve k otevření nebo uložení kontaktů.</span><span class="sxs-lookup"><span data-stu-id="625c2-256">IE will prompt you to open or save the contacts.</span></span>

    ![Dialogové okno Uložit webového rozhraní API][addwebapi006]


    <span data-ttu-id="625c2-258">Vrácený kontakty můžete otevřít v programu Poznámkový blok nebo prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="625c2-258">You can open the returned contacts in notepad or a browser.</span></span>

    <span data-ttu-id="625c2-259">Tento výstup mohou být spotřebovávána jiná aplikace, jako je například mobilní webové stránky nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="625c2-259">This output can be consumed by another application such as mobile web page or application.</span></span>

    ![Dialogové okno Uložit webového rozhraní API][addwebapi007]

    <span data-ttu-id="625c2-261">**Upozornění zabezpečení**: V tomto okamžiku aplikace je nezabezpečené a odolné vůči útoku proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="625c2-261">**Security Warning**: At this point, your application is insecure and vulnerable to CSRF attack.</span></span> <span data-ttu-id="625c2-262">Později v tomto kurzu jsme se odebrat toto ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="625c2-262">Later in the tutorial we will remove this vulnerability.</span></span> <span data-ttu-id="625c2-263">Další informace najdete v části [útoky brání webů požadavku padělání (proti útokům CSRF)][prevent-csrf-attacks].</span><span class="sxs-lookup"><span data-stu-id="625c2-263">For more information see [Preventing Cross-Site Request Forgery (CSRF) Attacks][prevent-csrf-attacks].</span></span>
## <a name="add-xsrf-protection"></a><span data-ttu-id="625c2-264">Přidat ochranu XSRF</span><span class="sxs-lookup"><span data-stu-id="625c2-264">Add XSRF Protection</span></span>
<span data-ttu-id="625c2-265">Padělání požadavku posílaného mezi weby (také označované jako XSRF nebo proti útokům CSRF) je útok na hostované webové aplikace, které škodlivou webovou stránku můžete ovlivnit interakce mezi prohlížeče klienta a web důvěryhodný tento prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="625c2-265">Cross-site request forgery (also known as XSRF or CSRF) is an attack against web-hosted applications whereby a malicious website can influence the interaction between a client browser and a website trusted by that browser.</span></span> <span data-ttu-id="625c2-266">Tyto útoky jsou možné, protože webových prohlížečů bude odesílat tokeny ověřování automaticky s každou žádost na web.</span><span class="sxs-lookup"><span data-stu-id="625c2-266">These attacks are made possible because web browsers will send authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="625c2-267">V kanonickém tvaru příkladu je soubor cookie ověřování, jako je například ASP. Lístek ověřování pomocí formulářů pro Asp.net.</span><span class="sxs-lookup"><span data-stu-id="625c2-267">The canonical example is an authentication cookie, such as ASP.NET's Forms Authentication ticket.</span></span> <span data-ttu-id="625c2-268">Tyto útoky však může být cílem weby, které použít žádné trvalé ověřovací mechanismus (například ověřování systému Windows, Basic a tak dále).</span><span class="sxs-lookup"><span data-stu-id="625c2-268">However, websites which use any persistent authentication mechanism (such as Windows Authentication, Basic, and so forth) can be targeted by these attacks.</span></span>

<span data-ttu-id="625c2-269">Útok XSRF se liší od útoky phishing.</span><span class="sxs-lookup"><span data-stu-id="625c2-269">An XSRF attack is distinct from a phishing attack.</span></span> <span data-ttu-id="625c2-270">Útoky phishing vyžadovat interakci z napadeného.</span><span class="sxs-lookup"><span data-stu-id="625c2-270">Phishing attacks require interaction from the victim.</span></span> <span data-ttu-id="625c2-271">V rámci útoku phishing škodlivou webovou stránku bude napodobovat cílového webu a napadené je oklamat do poskytování útočník citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="625c2-271">In a phishing attack, a malicious website will mimic the target website, and the victim is fooled into providing sensitive information to the attacker.</span></span> <span data-ttu-id="625c2-272">Při útoku XSRF není často žádná interakce potřebné z napadeného.</span><span class="sxs-lookup"><span data-stu-id="625c2-272">In an XSRF attack, there is often no interaction necessary from the victim.</span></span> <span data-ttu-id="625c2-273">Místo toho je útočník spoléhat na prohlížeči všechny relevantní soubory cookie automaticky odesílání do cílového webu.</span><span class="sxs-lookup"><span data-stu-id="625c2-273">Rather, the attacker is relying on the browser automatically sending all relevant cookies to the destination website.</span></span>

<span data-ttu-id="625c2-274">Další informace najdete v tématu [otevřete projekt webové aplikace zabezpečení](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span><span class="sxs-lookup"><span data-stu-id="625c2-274">For more information, see the [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span></span>

1. <span data-ttu-id="625c2-275">V **Průzkumníku řešení**, vpravo **ContactManager** projektu a klikněte na tlačítko **přidat** a pak klikněte na **třída**.</span><span class="sxs-lookup"><span data-stu-id="625c2-275">In **Solution Explorer**, right **ContactManager** project and click **Add** and then click **Class**.</span></span>
2. <span data-ttu-id="625c2-276">Název souboru *ValidateHttpAntiForgeryTokenAttribute.cs* a přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="625c2-276">Name the file *ValidateHttpAntiForgeryTokenAttribute.cs* and add the following code:</span></span>
   
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
3. <span data-ttu-id="625c2-277">Přidejte následující *pomocí* příkaz kontrakty řadiče, abyste získali přístup k **[ValidateHttpAntiForgeryToken]** atribut.</span><span class="sxs-lookup"><span data-stu-id="625c2-277">Add the following *using* statement to the contracts controller so you have access to the **[ValidateHttpAntiForgeryToken]** attribute.</span></span>
   
        using ContactManager.Filters;
4. <span data-ttu-id="625c2-278">Přidat **[ValidateHttpAntiForgeryToken]** atribut metody Post **ContactsController** k ochraně před hrozbami XSRF.</span><span class="sxs-lookup"><span data-stu-id="625c2-278">Add the **[ValidateHttpAntiForgeryToken]** attribute to the Post methods of the **ContactsController** to protect it from XSRF threats.</span></span> <span data-ttu-id="625c2-279">Přidá k "PutContact", "PostContact" a **DeleteContact** metody akce.</span><span class="sxs-lookup"><span data-stu-id="625c2-279">You will add it to the "PutContact",  "PostContact" and **DeleteContact** action methods.</span></span>
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. <span data-ttu-id="625c2-280">Aktualizace *skripty* části *Views\Home\Index.cshtml* souboru kódu na získat tokeny XSRF.</span><span class="sxs-lookup"><span data-stu-id="625c2-280">Update the *Scripts* section of the *Views\Home\Index.cshtml* file to include code to get the XSRF tokens.</span></span>
   
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

## <a name="publish-the-application-update-to-azure-and-sql-database"></a><span data-ttu-id="625c2-281">Publikovat aktualizaci aplikace na Azure a SQL Database</span><span class="sxs-lookup"><span data-stu-id="625c2-281">Publish the application update to Azure and SQL Database</span></span>
<span data-ttu-id="625c2-282">Publikování aplikace, opakujte postup, který jste postupovali podle dříve.</span><span class="sxs-lookup"><span data-stu-id="625c2-282">To publish the application, you repeat the procedure you followed earlier.</span></span>

1. <span data-ttu-id="625c2-283">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-283">In **Solution Explorer**, right click the project and select **Publish**.</span></span>
   
    ![Publikování][rxP]
2. <span data-ttu-id="625c2-285">Klikněte na kartu **Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="625c2-285">Click the **Settings** tab.</span></span>
3. <span data-ttu-id="625c2-286">V části **ContactsManagerContext(ContactsManagerContext)**, klikněte na tlačítko **v** ikonu změníte *vzdáleného připojovací řetězec* do připojovacího řetězce pro databázi kontaktu.</span><span class="sxs-lookup"><span data-stu-id="625c2-286">Under **ContactsManagerContext(ContactsManagerContext)**, click the **v** icon to change *Remote connection string* to the connection string for the contact database.</span></span> <span data-ttu-id="625c2-287">Klikněte na tlačítko **ContactDB**.</span><span class="sxs-lookup"><span data-stu-id="625c2-287">Click **ContactDB**.</span></span>
   
    ![Nastavení](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. <span data-ttu-id="625c2-289">Zaškrtněte políčko pro **spustit migrace Code First (spuštěno při spuštění aplikace)**.</span><span class="sxs-lookup"><span data-stu-id="625c2-289">Check the box for **Execute Code First Migrations (runs on application start)**.</span></span>
5. <span data-ttu-id="625c2-290">Klikněte na tlačítko **Další** a pak klikněte na **Preview**.</span><span class="sxs-lookup"><span data-stu-id="625c2-290">Click **Next** and then click **Preview**.</span></span> <span data-ttu-id="625c2-291">Visual Studio zobrazí seznam souborů, které bude přidán nebo aktualizován.</span><span class="sxs-lookup"><span data-stu-id="625c2-291">Visual Studio displays a list of the files that will be added or updated.</span></span>
6. <span data-ttu-id="625c2-292">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="625c2-292">Click **Publish**.</span></span>
   <span data-ttu-id="625c2-293">Po dokončení nasazení otevře prohlížeč na domovskou stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="625c2-293">After the deployment completes, the browser opens to the home page of the application.</span></span>
   
    ![Index stránky s žádné kontakty.][intro001]
   
    <span data-ttu-id="625c2-295">Visual Studio publikovat proces automaticky nakonfigurované připojovací řetězec v nasazené *Web.config* souboru tak, aby odkazoval na databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="625c2-295">The Visual Studio publish process automatically configured the connection string in the deployed *Web.config* file to point to the SQL database.</span></span> <span data-ttu-id="625c2-296">Také nakonfigurovat migrace Code First automaticky upgradu databáze na nejnovější verzi aplikace přistupovat k databázi po nasazení poprvé.</span><span class="sxs-lookup"><span data-stu-id="625c2-296">It also configured Code First Migrations to automatically upgrade the database to the latest version the first time the application accesses the database after deployment.</span></span>
   
    <span data-ttu-id="625c2-297">V důsledku této konfiguraci Code First vytvořené databáze spuštěním kódu **počáteční** třídu, která jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="625c2-297">As a result of this configuration, Code First created the database by running the code in the **Initial** class that you created earlier.</span></span> <span data-ttu-id="625c2-298">Stejně to při prvním pokusu o přístup k databázi po nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="625c2-298">It did this the first time the application tried to access the database after deployment.</span></span>
7. <span data-ttu-id="625c2-299">Zadejte kontakt, jako jste to udělali při spuštění aplikace místně, chcete-li ověřit, že nasazení databáze bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="625c2-299">Enter a contact as you did when you ran the app locally, to verify that database deployment succeeded.</span></span>

<span data-ttu-id="625c2-300">Až uvidíte, že položku, kterou zadáte je uložena a zobrazí se na stránce kontaktujte správce, víte, že byla uložena v databázi.</span><span class="sxs-lookup"><span data-stu-id="625c2-300">When you see that the item you enter is saved and appears on the contact manager page, you know that it has been stored in the database.</span></span>

![Index stránky s kontakty][addwebapi004]

<span data-ttu-id="625c2-302">Aplikace je nyní spuštěna v cloudu, uložení svá data pomocí databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="625c2-302">The application is now running in the cloud, using SQL Database to store its data.</span></span> <span data-ttu-id="625c2-303">Po dokončení testování aplikace v Azure, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="625c2-303">After you finish testing the application in Azure, delete it.</span></span> <span data-ttu-id="625c2-304">Aplikace je veřejný a nemá žádný mechanismus pro omezení přístupu.</span><span class="sxs-lookup"><span data-stu-id="625c2-304">The application is public and doesn't have a mechanism to limit access.</span></span>

> [!NOTE]
> <span data-ttu-id="625c2-305">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="625c2-305">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="625c2-306">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="625c2-306">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="625c2-307">Další kroky</span><span class="sxs-lookup"><span data-stu-id="625c2-307">Next Steps</span></span>
<span data-ttu-id="625c2-308">Jiný způsob ukládání dat v aplikaci Azure je použití úložiště Azure, které poskytují úložiště nerelační data ve formě objekty BLOB a tabulek.</span><span class="sxs-lookup"><span data-stu-id="625c2-308">Another way to store data in an Azure application is to use Azure storage, which provide non-relational data storage in the form of blobs and tables.</span></span> <span data-ttu-id="625c2-309">Následující odkazy poskytují další informace o webového rozhraní API, rozhraní ASP.NET MVC a okno Azure.</span><span class="sxs-lookup"><span data-stu-id="625c2-309">The following links provide more information on Web API, ASP.NET MVC and Window Azure.</span></span>

* <span data-ttu-id="625c2-310">[Začínáme s MVC pomocí rozhraní Entity Framework][EFCodeFirstMVCTutorial]</span><span class="sxs-lookup"><span data-stu-id="625c2-310">[Getting Started with Entity Framework using MVC][EFCodeFirstMVCTutorial]</span></span>
* [<span data-ttu-id="625c2-311">Úvod do architektury ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="625c2-311">Intro to ASP.NET MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="625c2-312">První rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="625c2-312">Your First ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [<span data-ttu-id="625c2-313">Ladění WAWS</span><span class="sxs-lookup"><span data-stu-id="625c2-313">Debugging WAWS</span></span>](web-sites-dotnet-troubleshoot-visual-studio.md)

<span data-ttu-id="625c2-314">Tento kurz a ukázkové aplikace byla zapsána pomocí [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) s požádat o pomoc tní Dykstra a Jiří Dorrans (Twitter [ @blowdart ](https://twitter.com/blowdart)).</span><span class="sxs-lookup"><span data-stu-id="625c2-314">This tutorial and the sample application was written by [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) with assistance from Tom Dykstra and Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart)).</span></span> 

<span data-ttu-id="625c2-315">Prosím ponechejte zpětná vazba týkající se vám líbilo nebo co chcete najdete v části zlepšila, jenom o samotné kurz, ale taky o produkty, které ukazuje.</span><span class="sxs-lookup"><span data-stu-id="625c2-315">Please leave feedback on what you liked or what you would like to see improved, not only about the tutorial itself but also about the products that it demonstrates.</span></span> <span data-ttu-id="625c2-316">Vaše zpětná vazba pomůže stanovení priorit vylepšení.</span><span class="sxs-lookup"><span data-stu-id="625c2-316">Your feedback will help us prioritize improvements.</span></span> <span data-ttu-id="625c2-317">Zejména zajímá zjistit kolik zájmu v další automatizace procesu konfigurace a nasazení databáze členství.</span><span class="sxs-lookup"><span data-stu-id="625c2-317">We are especially interested in finding out how much interest there is in more automation for the process of configuring and deploying the membership database.</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="625c2-318">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="625c2-318">What's changed</span></span>
* <span data-ttu-id="625c2-319">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="625c2-319">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles to the Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update the Membership Database]:#ppd2
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

