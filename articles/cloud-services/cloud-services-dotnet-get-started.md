---
title: "Začínáme s cloudovými službami službou Azure Cloud Services a technologií ASP.NET | Dokumentace Microsoftu"
description: "Naučte se vytvářet vícevrstvé aplikace s použitím technologie ASP.NET MVC a Azure. Aplikace běží v cloudové službě a obsahuje webovou roli a roli pracovního procesu. Používá nástroj Entity Framework, službu Azure SQL Database a fronty a objekty blob služby Azure Storage."
services: cloud-services, storage
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: d7aa440d-af4a-4f80-b804-cc46178df4f9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/15/2017
ms.author: adegeo
ms.openlocfilehash: d2c197db73477d06d86d1c4faa8c4a2f58c7b391
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-cloud-services-and-aspnet"></a><span data-ttu-id="71c45-105">Začínáme s cloudovými službami Azure Cloud Services a technologií ASP.NET</span><span class="sxs-lookup"><span data-stu-id="71c45-105">Get started with Azure Cloud Services and ASP.NET</span></span>

## <a name="overview"></a><span data-ttu-id="71c45-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="71c45-106">Overview</span></span>
<span data-ttu-id="71c45-107">Tento kurz ukazuje, jak lze vytvářet vícevrstvé aplikace .NET s front-endem ASP.NET MVC a jak je nasadit do [cloudové služby Azure](cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="71c45-107">This tutorial shows how to create a multi-tier .NET application with an ASP.NET MVC front-end, and deploy it to an [Azure cloud service](cloud-services-choose-me.md).</span></span> <span data-ttu-id="71c45-108">Aplikace používá [službu Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279),  [službu objektů blob Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage) a [službu front Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span><span class="sxs-lookup"><span data-stu-id="71c45-108">The application uses [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), the [Azure Blob service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and the [Azure Queue service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span></span> <span data-ttu-id="71c45-109">[Projekt sady Visual Studio můžete stáhnout](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) z galerie kódů MSDN.</span><span class="sxs-lookup"><span data-stu-id="71c45-109">You can [download the Visual Studio project](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) from the MSDN Code Gallery.</span></span>

<span data-ttu-id="71c45-110">V kurzu se dozvíte, jak sestavit a spustit aplikaci místně, jak ji nasadit do Azure a spustit v cloudu a jak ji sestavit od nuly.</span><span class="sxs-lookup"><span data-stu-id="71c45-110">The tutorial shows you how to build and run the application locally, how to deploy it to Azure and run in the cloud, and how to build it from scratch.</span></span> <span data-ttu-id="71c45-111">Pokud chcete, můžete začít tím, že ji sestavíte od nuly, potom ji otestujete a nakonec provedete kroky nasazení.</span><span class="sxs-lookup"><span data-stu-id="71c45-111">You can start by building from scratch and then do the test and deploy steps afterward if you prefer.</span></span>

## <a name="contoso-ads-application"></a><span data-ttu-id="71c45-112">Aplikace Contoso Ads</span><span class="sxs-lookup"><span data-stu-id="71c45-112">Contoso Ads application</span></span>
<span data-ttu-id="71c45-113">Aplikace slouží jako vývěsní tabule pro inzerci.</span><span class="sxs-lookup"><span data-stu-id="71c45-113">The application is an advertising bulletin board.</span></span> <span data-ttu-id="71c45-114">Uživatelé vytvářejí reklamu tak, že zadají text a odešlou obrázek.</span><span class="sxs-lookup"><span data-stu-id="71c45-114">Users create an ad by entering text and uploading an image.</span></span> <span data-ttu-id="71c45-115">Před sebou vidí seznam reklam s obrázky miniatur a plnou velikost obrázku s podrobnostmi si mohou zobrazit výběrem požadované reklamy.</span><span class="sxs-lookup"><span data-stu-id="71c45-115">They can see a list of ads with thumbnail images, and they can see the full-size image when they select an ad to see the details.</span></span>

![Seznam reklam](./media/cloud-services-dotnet-get-started/list.png)

<span data-ttu-id="71c45-117">Aplikace používá [způsob práce zaměřený na fronty](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern), aby vyvážila práci při vytváření miniatur (která je náročná na prostředky procesoru) vůči back-endovému procesu.</span><span class="sxs-lookup"><span data-stu-id="71c45-117">The application uses the [queue-centric work pattern](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) to off-load the CPU-intensive work of creating thumbnails to a back-end process.</span></span>

## <a name="alternative-architecture-websites-and-webjobs"></a><span data-ttu-id="71c45-118">Alternativní architektura: weby a webové úlohy</span><span class="sxs-lookup"><span data-stu-id="71c45-118">Alternative architecture: Websites and WebJobs</span></span>
<span data-ttu-id="71c45-119">Tento kurz ukazuje, jak spustit front-end i back-end v cloudové službě Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-119">This tutorial shows how to run both front-end and back-end in an Azure cloud service.</span></span> <span data-ttu-id="71c45-120">Alternativou je spuštění front-endu na [webu Azure](/services/web-sites/) a použití funkce [webových úloh](http://go.microsoft.com/fwlink/?LinkId=390226) (momentálně ve verzi Preview) pro back-end.</span><span class="sxs-lookup"><span data-stu-id="71c45-120">An alternative is to run the front-end in an [Azure website](/services/web-sites/) and use the [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) feature (currently in preview) for the back-end.</span></span> <span data-ttu-id="71c45-121">Kurz, který používá webové úlohy, najdete v článku [Začínáme se sadou SDK pro webové úlohy Azure](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="71c45-121">For a tutorial that uses WebJobs, see [Get Started with the Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span></span> <span data-ttu-id="71c45-122">Informace o tom, jak zvolit služby, které budou nejlépe vyhovovat vašemu scénáři, najdete v článku o [porovnání webů Azure, služeb Cloud Services a virtuálních počítačů](../app-service-web/choose-web-site-cloud-service-vm.md).</span><span class="sxs-lookup"><span data-stu-id="71c45-122">For information about how to choose the services that best fit your scenario, see [Azure Websites, Cloud Services, and virtual machines comparison](../app-service-web/choose-web-site-cloud-service-vm.md).</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="71c45-123">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="71c45-123">What you'll learn</span></span>
* <span data-ttu-id="71c45-124">Postup zprovoznění počítače pro vývoj na platformě Azure nainstalováním sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="71c45-124">How to enable your machine for Azure development by installing the Azure SDK.</span></span>
* <span data-ttu-id="71c45-125">Vytvoření projektu cloudových služeb sady Visual Studio s webovou rolí a rolí pracovního procesu technologie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="71c45-125">How to create a Visual Studio cloud service project with an ASP.NET MVC web role and a worker role.</span></span>
* <span data-ttu-id="71c45-126">Postup místního testování projektu cloudových služeb pomocí emulátoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-126">How to test the cloud service project locally, using the Azure storage emulator.</span></span>
* <span data-ttu-id="71c45-127">Postup publikování cloudového projektu do cloudové služby Azure a testování pomocí účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-127">How to publish the cloud project to an Azure cloud service and test using an Azure storage account.</span></span>
* <span data-ttu-id="71c45-128">Odeslání souborů a jejich uložení do služby objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-128">How to upload files and store them in the Azure Blob service.</span></span>
* <span data-ttu-id="71c45-129">Používání služby front Azure pro komunikaci mezi vrstvami.</span><span class="sxs-lookup"><span data-stu-id="71c45-129">How to use the Azure Queue service for communication between tiers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71c45-130">Požadavky</span><span class="sxs-lookup"><span data-stu-id="71c45-130">Prerequisites</span></span>
<span data-ttu-id="71c45-131">Kurz předpokládá, že rozumíte [základnímu konceptu cloudových služeb Azure](cloud-services-choose-me.md), například terminologii *webových rolí* a *rolí pracovních procesů*.</span><span class="sxs-lookup"><span data-stu-id="71c45-131">The tutorial assumes that you understand [basic concepts about Azure cloud services](cloud-services-choose-me.md) such as *web role* and *worker role* terminology.</span></span>  <span data-ttu-id="71c45-132">Předpokládá také, že víte, jak pracovat s technologií [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) a s projekty [webových formulářů](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) ve Visual Studiu.</span><span class="sxs-lookup"><span data-stu-id="71c45-132">It also assumes that you know how to work with [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) or [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) projects in Visual Studio.</span></span> <span data-ttu-id="71c45-133">Ukázková aplikace používá MVC, ale většina kurzu platí i pro webové formuláře.</span><span class="sxs-lookup"><span data-stu-id="71c45-133">The sample application uses MVC, but most of the tutorial also applies to Web Forms.</span></span>

<span data-ttu-id="71c45-134">Aplikaci můžete spustit místně bez předplatného Azure, ale k nasazení aplikace do cloudu budete předplatné potřebovat.</span><span class="sxs-lookup"><span data-stu-id="71c45-134">You can run the app locally without an Azure subscription, but you'll need one to deploy the application to the cloud.</span></span> <span data-ttu-id="71c45-135">Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) nebo [si zaregistrovat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span><span class="sxs-lookup"><span data-stu-id="71c45-135">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span></span>

<span data-ttu-id="71c45-136">Pokyny kurzu pracují s jedním z následujících produktů:</span><span class="sxs-lookup"><span data-stu-id="71c45-136">The tutorial instructions work with either of the following products:</span></span>

* <span data-ttu-id="71c45-137">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="71c45-137">Visual Studio 2013</span></span>
* <span data-ttu-id="71c45-138">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="71c45-138">Visual Studio 2015</span></span>
* <span data-ttu-id="71c45-139">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="71c45-139">Visual Studio 2017</span></span>

<span data-ttu-id="71c45-140">Pokud je nemáte, Visual Studio se vám může nainstalovat automaticky při instalaci sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="71c45-140">If you don't have one of these, Visual Studio may be installed automatically when you install the Azure SDK.</span></span>

## <a name="application-architecture"></a><span data-ttu-id="71c45-141">Architektura aplikace</span><span class="sxs-lookup"><span data-stu-id="71c45-141">Application architecture</span></span>
<span data-ttu-id="71c45-142">Aplikace ukládá reklamy do databáze SQL a k vytváření tabulky a přístupu k datům používá Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="71c45-142">The app stores ads in a SQL database, using Entity Framework Code First to create the tables and access the data.</span></span> <span data-ttu-id="71c45-143">U každé reklamy databáze ukládá dvě adresy URL. Jednu pro obrázek v plné velikosti a druhou pro miniaturu.</span><span class="sxs-lookup"><span data-stu-id="71c45-143">For each ad, the database stores two URLs, one for the full-size image and one for the thumbnail.</span></span>

![Tabulka reklam](./media/cloud-services-dotnet-get-started/adtable.png)

<span data-ttu-id="71c45-145">Když uživatel odešle obrázek, front-end spuštěný ve webové roli obrázek uloží do [objektu blob Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), který ukládá informace o reklamě do databáze s adresou URL, která odkazuje na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="71c45-145">When a user uploads an image, the front-end running in a web role stores the image in an [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and it stores the ad information in the database with a URL that points to the blob.</span></span> <span data-ttu-id="71c45-146">Ve stejný okamžik zapíše zprávu do fronty Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-146">At the same time, it writes a message to an Azure queue.</span></span> <span data-ttu-id="71c45-147">Back-endový proces spuštěný v roli pracovního procesu se pravidelně dotazuje fronty na nové zprávy.</span><span class="sxs-lookup"><span data-stu-id="71c45-147">A back-end process running in a worker role periodically polls the queue for new messages.</span></span> <span data-ttu-id="71c45-148">Když se objeví nová zpráva, role pracovního procesu vytvoří miniaturu obrázku a zaktualizuje pole v databázi s adresou URL miniatury pro tuto reklamu.</span><span class="sxs-lookup"><span data-stu-id="71c45-148">When a new message appears, the worker role creates a thumbnail for that image and updates the thumbnail URL database field for that ad.</span></span> <span data-ttu-id="71c45-149">Následující diagram znázorňuje interakci částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="71c45-149">The following diagram shows how the parts of the application interact.</span></span>

![Architektura Contoso Ads](./media/cloud-services-dotnet-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]

## <a name="download-and-run-the-completed-solution"></a><span data-ttu-id="71c45-151">Stažení a spuštění dokončeného řešení</span><span class="sxs-lookup"><span data-stu-id="71c45-151">Download and run the completed solution</span></span>
1. <span data-ttu-id="71c45-152">Stáhněte a rozbalte [dokončené řešení](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span><span class="sxs-lookup"><span data-stu-id="71c45-152">Download and unzip the [completed solution](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span></span>
2. <span data-ttu-id="71c45-153">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71c45-153">Start Visual Studio.</span></span>
3. <span data-ttu-id="71c45-154">V nabídce **Soubor** zvolte **Otevřít projekt**, přejděte do místa, kam jste řešení stáhli, a potom otevřete soubor řešení.</span><span class="sxs-lookup"><span data-stu-id="71c45-154">From the **File** menu choose **Open Project**, navigate to where you downloaded the solution, and then open the solution file.</span></span>
4. <span data-ttu-id="71c45-155">Stisknutím kláves CTRL+SHIFT+B řešení sestavíte.</span><span class="sxs-lookup"><span data-stu-id="71c45-155">Press CTRL+SHIFT+B to build the solution.</span></span>

    <span data-ttu-id="71c45-156">Visual Studio ve výchozím nastavení automaticky obnoví obsah balíčku NuGet, který nebyl součástí souboru *.zip*.</span><span class="sxs-lookup"><span data-stu-id="71c45-156">By default, Visual Studio automatically restores the NuGet package content, which was not included in the *.zip* file.</span></span> <span data-ttu-id="71c45-157">Pokud se balíčky neobnoví, nainstalujte je ručně tak, že přejdete do dialogového okna **Správa balíčků NuGet pro řešení** a kliknete na tlačítko **Obnovit**, které je vpravo nahoře.</span><span class="sxs-lookup"><span data-stu-id="71c45-157">If the packages don't restore, install them manually by going to the **Manage NuGet Packages for Solution** dialog box and clicking the **Restore** button at the top right.</span></span>
5. <span data-ttu-id="71c45-158">V **Průzkumníku řešení** zkontrolujte, jestli je jako spouštěný projekt vybraná možnost **ContosoAdsCloudService**.</span><span class="sxs-lookup"><span data-stu-id="71c45-158">In **Solution Explorer**, make sure that **ContosoAdsCloudService** is selected as the startup project.</span></span>
6. <span data-ttu-id="71c45-159">Pokud používáte Visual Studio 2015 nebo vyšší, změňte připojovací řetězec serveru SQL v aplikačním souboru *Web.config* projektu ContosoAdsWeb a v souboru *ServiceConfiguration.Local.cscfg* projektu ContosoAdsCloudService.</span><span class="sxs-lookup"><span data-stu-id="71c45-159">If you're using Visual Studio 2015 or higher, change the SQL Server connection string in the application *Web.config* file of the ContosoAdsWeb project and in the *ServiceConfiguration.Local.cscfg* file of the ContosoAdsCloudService project.</span></span> <span data-ttu-id="71c45-160">V každém případě změňte „(localdb) \v11.0“ na „\MSSQLLocalDB (localdb)“.</span><span class="sxs-lookup"><span data-stu-id="71c45-160">In each case, change "(localdb)\v11.0" to "(localdb)\MSSQLLocalDB".</span></span>
7. <span data-ttu-id="71c45-161">Stiskněte klávesy CTRL+F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="71c45-161">Press CTRL+F5 to run the application.</span></span>

    <span data-ttu-id="71c45-162">Když spouštíte projekt cloudové služby místně, Visual Studio automaticky vyvolá *emulátor služby Výpočty* Azure a *emulátor úložiště* Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-162">When you run a cloud service project locally, Visual Studio automatically invokes the Azure *compute emulator* and Azure *storage emulator*.</span></span> <span data-ttu-id="71c45-163">Emulátor služby Výpočty využívá prostředky počítače k simulaci prostředí webové role a role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="71c45-163">The compute emulator uses your computer's resources to simulate the web role and worker role environments.</span></span> <span data-ttu-id="71c45-164">Emulátor úložiště používá databázi serveru [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) k simulaci cloudového úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-164">The storage emulator uses a [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) database to simulate Azure cloud storage.</span></span>

    <span data-ttu-id="71c45-165">Při prvním spuštění projektu cloudové služby může spuštění emulátorů trvat zhruba minutu.</span><span class="sxs-lookup"><span data-stu-id="71c45-165">The first time you run a cloud service project, it takes a minute or so for the emulators to start up.</span></span> <span data-ttu-id="71c45-166">Když je spuštění emulátorů dokončené, výchozí prohlížeč otevře domovskou stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="71c45-166">When emulator startup is finished, the default browser opens to the application home page.</span></span>

    ![Architektura Contoso Ads](./media/cloud-services-dotnet-get-started/home.png)
8. <span data-ttu-id="71c45-168">Klikněte na **Vytvořit reklamu**.</span><span class="sxs-lookup"><span data-stu-id="71c45-168">Click  **Create an Ad**.</span></span>
9. <span data-ttu-id="71c45-169">Zadejte nějaká testovací data, vyberte obrázek *.jpg*, který chcete odeslat, a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="71c45-169">Enter some test data and select a *.jpg* image to upload, and then click **Create**.</span></span>

    ![Vytvoření stránky](./media/cloud-services-dotnet-get-started/create.png)

    <span data-ttu-id="71c45-171">Aplikace přejde na indexovou stránku, ale nezobrazí miniaturu nové reklamy, protože toto zpracování ještě neproběhlo.</span><span class="sxs-lookup"><span data-stu-id="71c45-171">The app goes to the Index page, but it doesn't show a thumbnail for the new ad because that processing hasn't happened yet.</span></span>
10. <span data-ttu-id="71c45-172">Chvíli počkejte a potom indexovou stránku aktualizujte. Miniatura se teď zobrazí.</span><span class="sxs-lookup"><span data-stu-id="71c45-172">Wait a moment and then refresh the Index page to see the thumbnail.</span></span>

     ![Indexová stránka](./media/cloud-services-dotnet-get-started/list.png)
11. <span data-ttu-id="71c45-174">Pokud chcete obrázek reklamy zobrazit v plné velikosti, klikněte na **Podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="71c45-174">Click **Details** for your ad to see the full-size image.</span></span>

     ![Stránka podrobností](./media/cloud-services-dotnet-get-started/details.png)

<span data-ttu-id="71c45-176">Aplikace běží výhradně na místním počítači bez připojení ke cloudu.</span><span class="sxs-lookup"><span data-stu-id="71c45-176">You've been running the application entirely on your local computer, with no connection to the cloud.</span></span> <span data-ttu-id="71c45-177">Emulátor úložiště ukládá data fronty a objektů blob do databáze serveru SQL Server Express LocalDB, ale aplikace ukládá data reklamy do jiné databáze LocalDB.</span><span class="sxs-lookup"><span data-stu-id="71c45-177">The storage emulator stores the queue and blob data in a SQL Server Express LocalDB database, and the application stores the ad data in another LocalDB database.</span></span> <span data-ttu-id="71c45-178">Entity Framework Code First automaticky vytvoří databázi reklam v okamžiku, kdy se webová aplikace poprvé pokusí k databázi připojit.</span><span class="sxs-lookup"><span data-stu-id="71c45-178">Entity Framework Code First automatically created the ad database the first time the web app tried to access it.</span></span>

<span data-ttu-id="71c45-179">V následující části budete konfigurovat řešení tak, aby při spuštění v cloudu používalo cloudové prostředky Azure pro fronty a objekty blob a také databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="71c45-179">In the following section you'll configure the solution to use Azure cloud resources for queues, blobs, and the application database when it runs in the cloud.</span></span> <span data-ttu-id="71c45-180">Pokud chcete aplikaci i nadále spouštět místně, ale používat cloudové úložiště a databázové prostředky, tak můžete.</span><span class="sxs-lookup"><span data-stu-id="71c45-180">If you wanted to continue to run locally but use cloud storage and database resources, you could do that.</span></span> <span data-ttu-id="71c45-181">Stačí nastavit připojovací řetězce a my vám ukážeme, jak na to.</span><span class="sxs-lookup"><span data-stu-id="71c45-181">It's just a matter of setting connection strings, which you'll see how to do.</span></span>

## <a name="deploy-the-application-to-azure"></a><span data-ttu-id="71c45-182">Nasazení aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-182">Deploy the application to Azure</span></span>
<span data-ttu-id="71c45-183">Pokud chcete aplikaci spustit v cloudu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="71c45-183">You'll do the following steps to run the application in the cloud:</span></span>

* <span data-ttu-id="71c45-184">Vytvoření cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-184">Create an Azure cloud service.</span></span>
* <span data-ttu-id="71c45-185">Vytvoření databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-185">Create an Azure SQL database.</span></span>
* <span data-ttu-id="71c45-186">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-186">Create an Azure storage account.</span></span>
* <span data-ttu-id="71c45-187">Nakonfigurujte řešení, aby při spuštění v Azure používalo databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-187">Configure the solution to use your Azure SQL database when it runs in Azure.</span></span>
* <span data-ttu-id="71c45-188">Nakonfigurujte řešení, aby při spuštění v Azure používalo účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-188">Configure the solution to use your Azure storage account when it runs in Azure.</span></span>
* <span data-ttu-id="71c45-189">Nasaďte projekt do cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-189">Deploy the project to your Azure cloud service.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="71c45-190">Vytvoření cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-190">Create an Azure cloud service</span></span>
<span data-ttu-id="71c45-191">Cloudová služba Azure je prostředí, ve kterém bude aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="71c45-191">An Azure cloud service is the environment the application will run in.</span></span>

1. <span data-ttu-id="71c45-192">Otevřete v prohlížeči portál [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="71c45-192">In your browser, open the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="71c45-193">Klikněte na **Nový > Výpočty > Cloudová služba**.</span><span class="sxs-lookup"><span data-stu-id="71c45-193">Click **New > Compute > Cloud Service**.</span></span>

3. <span data-ttu-id="71c45-194">Do vstupního pole název DNS zadejte předponu adresy URL pro cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="71c45-194">In the DNS name input box, enter a URL prefix for the cloud service.</span></span>

    <span data-ttu-id="71c45-195">Tato adresa URL musí být jedinečná.</span><span class="sxs-lookup"><span data-stu-id="71c45-195">This URL has to be unique.</span></span>  <span data-ttu-id="71c45-196">Pokud se zvolená předpona už používá, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="71c45-196">You'll get an error message if the prefix you choose is already in use.</span></span>
4. <span data-ttu-id="71c45-197">Zadejte pro tuto službu novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="71c45-197">Specify a new Resource group for the  service.</span></span> <span data-ttu-id="71c45-198">Klikněte na **Vytvořit nový** a potom zadejte název do vstupního pole Skupina prostředků – například CS_contososadsRG.</span><span class="sxs-lookup"><span data-stu-id="71c45-198">Click **Create new** and then type a name in the Resource group input box, such as CS_contososadsRG.</span></span>

5. <span data-ttu-id="71c45-199">Vyberte oblast, ve které chcete aplikaci nasadit.</span><span class="sxs-lookup"><span data-stu-id="71c45-199">Choose the region where you want to deploy the application.</span></span>

    <span data-ttu-id="71c45-200">Toto pole určuje datové centrum, které bude hostovat vaše cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="71c45-200">This field specifies which datacenter your cloud service will be hosted in.</span></span> <span data-ttu-id="71c45-201">V případě produkční aplikace vyberte oblast, která je nejblíž k vašim zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="71c45-201">For a production application, you'd choose the region closest to your customers.</span></span> <span data-ttu-id="71c45-202">V tomto kurzu vyberte oblast, která je nejblíž k vám.</span><span class="sxs-lookup"><span data-stu-id="71c45-202">For this tutorial, choose the region closest to you.</span></span>
5. <span data-ttu-id="71c45-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="71c45-203">Click **Create**.</span></span>

    <span data-ttu-id="71c45-204">Na následujícím obrázku vidíte vytvoření cloudové služby s adresou URL CSvccontosoads.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="71c45-204">In the following image, a cloud service is created with the URL CSvccontosoads.cloudapp.net.</span></span>

    ![Nová cloudová služba](./media/cloud-services-dotnet-get-started/newcs.png)

### <a name="create-an-azure-sql-database"></a><span data-ttu-id="71c45-206">Vytvoření databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-206">Create an Azure SQL database</span></span>
<span data-ttu-id="71c45-207">Když aplikace běží v cloudu, používá cloudovou databázi.</span><span class="sxs-lookup"><span data-stu-id="71c45-207">When the app runs in the cloud, it will use a cloud-based database.</span></span>

1. <span data-ttu-id="71c45-208">Na portálu [Azure Portal](https://portal.azure.com) klikněte na **Nový > Databáze > Databáze SQL**.</span><span class="sxs-lookup"><span data-stu-id="71c45-208">In the [Azure portal](https://portal.azure.com), click **New > Databases > SQL Database**.</span></span>
2. <span data-ttu-id="71c45-209">Do pole **Název databáze** zadejte text *contosoads*.</span><span class="sxs-lookup"><span data-stu-id="71c45-209">In the **Database Name** box, enter *contosoads*.</span></span>
3. <span data-ttu-id="71c45-210">V části **Skupina prostředků** klikněte na **Použít existující** a vyberte skupinu prostředků použitou pro cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="71c45-210">In the **Resource group**, click **Use existing** and select the resource group used for the cloud service.</span></span>
4. <span data-ttu-id="71c45-211">Na následujícím obrázku klikněte na **Server – Konfigurovat požadovaná nastavení** a **Vytvořit nový server**.</span><span class="sxs-lookup"><span data-stu-id="71c45-211">In the following image, click **Server - Configure required settings** and **Create a new server**.</span></span>

    ![Tunel pro databázový server](./media/cloud-services-dotnet-get-started/newdb.png)

    <span data-ttu-id="71c45-213">Pokud vaše předplatné obsahuje server, můžete alternativně vybrat tento server v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="71c45-213">Alternatively, if your subscription already has a server, you can select that server from the drop-down list.</span></span>
5. <span data-ttu-id="71c45-214">Do pole **Název serveru** zadejte *csvccontosodbserver*.</span><span class="sxs-lookup"><span data-stu-id="71c45-214">In the **Server name** box, enter *csvccontosodbserver*.</span></span>

6. <span data-ttu-id="71c45-215">Zadejte **Přihlašovací jméno** a **Heslo** správce.</span><span class="sxs-lookup"><span data-stu-id="71c45-215">Enter an administrator **Login Name** and **Password**.</span></span>

    <span data-ttu-id="71c45-216">Pokud jste vybrali možnost **Vytvořit nový server**, nebudete zadávat existující název a heslo.</span><span class="sxs-lookup"><span data-stu-id="71c45-216">If you selected **Create a new server**, you aren't entering an existing name and password here.</span></span> <span data-ttu-id="71c45-217">Zadáváte nový název a heslo, které teď definujete pro pozdější použití, až budete chtít získat přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="71c45-217">You're entering a new name and password that you're defining now to use later when you access the database.</span></span> <span data-ttu-id="71c45-218">Pokud jste vybrali serveru, který jste vytvořili dříve, budete vyzváni k zadání hesla pro uživatelský účet správce jste již vytvořili.</span><span class="sxs-lookup"><span data-stu-id="71c45-218">If you selected a server that you created previously, you'll be prompted for the password to the administrative user account you already created.</span></span>
7. <span data-ttu-id="71c45-219">Vyberte stejné **Umístění** jako pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="71c45-219">Choose the same **Location** that you chose for the cloud service.</span></span>

    <span data-ttu-id="71c45-220">Když jsou cloudové služby a databáze v různých datových centrech (různých oblastech), zvýší se latence a bude vám účtována šířka pásma mimo datové centrum.</span><span class="sxs-lookup"><span data-stu-id="71c45-220">When the cloud service and database are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside the data center.</span></span> <span data-ttu-id="71c45-221">Šířka pásma v rámci datového centra je zdarma.</span><span class="sxs-lookup"><span data-stu-id="71c45-221">Bandwidth within a data center is free.</span></span>
8. <span data-ttu-id="71c45-222">Zkontrolujte možnost **Povolit službám Azure přístup k serveru**.</span><span class="sxs-lookup"><span data-stu-id="71c45-222">Check **Allow azure services to access server**.</span></span>
9. <span data-ttu-id="71c45-223">Klikněte na možnost **Vybrat** u nového serveru.</span><span class="sxs-lookup"><span data-stu-id="71c45-223">Click **Select** for the new server.</span></span>

    ![Nový server služby SQL Database](./media/cloud-services-dotnet-get-started/newdbserver.png)
10. <span data-ttu-id="71c45-225">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="71c45-225">Click **Create**.</span></span>

### <a name="create-an-azure-storage-account"></a><span data-ttu-id="71c45-226">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-226">Create an Azure storage account</span></span>
<span data-ttu-id="71c45-227">Účet úložiště Azure poskytuje prostředky pro ukládání dat front a objektů blob v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71c45-227">An Azure storage account provides resources for storing queue and blob data in the cloud.</span></span>

<span data-ttu-id="71c45-228">V reálné aplikaci byste obvykle vytvořili samostatné účty pro data aplikací a pro data protokolování a samostatné účty pro testovací data a pro produkční data.</span><span class="sxs-lookup"><span data-stu-id="71c45-228">In a real-world application, you would typically create separate accounts for application data versus logging data, and separate accounts for test data versus production data.</span></span> <span data-ttu-id="71c45-229">V tomto kurzu budete používat jenom jeden účet.</span><span class="sxs-lookup"><span data-stu-id="71c45-229">For this tutorial, you'll use just one account.</span></span>

1. <span data-ttu-id="71c45-230">Na portálu [Azure Portal](https://portal.azure.com) klikněte na **Nový > Úložiště > Účet úložiště – objekt blob, soubor, tabulka, fronta**.</span><span class="sxs-lookup"><span data-stu-id="71c45-230">In the [Azure portal](https://portal.azure.com), click **New > Storage > Storage account - blob, file, table, queue**.</span></span>
2. <span data-ttu-id="71c45-231">Do pole **Název** zadejte předponu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="71c45-231">In the **Name** box, enter a URL prefix.</span></span>

    <span data-ttu-id="71c45-232">Tato předpona a text zobrazený pod polem budou tvořit jedinečnou adresu URL k vašemu účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="71c45-232">This prefix plus the text you see under the box will be the unique URL to your storage account.</span></span> <span data-ttu-id="71c45-233">Pokud vybranou předponu používá někdo jiný, budete si muset zvolit jinou.</span><span class="sxs-lookup"><span data-stu-id="71c45-233">If the prefix you enter has already been used by someone else, you'll have to choose a different prefix.</span></span>
3. <span data-ttu-id="71c45-234">U **Modelu nasazení** nastavte *Classic*.</span><span class="sxs-lookup"><span data-stu-id="71c45-234">Set the **Deployment model** to *Classic*.</span></span>

4. <span data-ttu-id="71c45-235">V rozevíracím seznamu **Replikace** vyberte **Místně redundantní úložiště**.</span><span class="sxs-lookup"><span data-stu-id="71c45-235">Set the **Replication** drop-down list to **Locally redundant storage**.</span></span>

    <span data-ttu-id="71c45-236">Když má účet úložiště povolenou geografickou replikaci, bude se uložený obsah replikovat do sekundárního datacentra, které zajistí převzetí služeb při selhání v případě významnější havárie v primárním umístění.</span><span class="sxs-lookup"><span data-stu-id="71c45-236">When geo-replication is enabled for a storage account, the stored content is replicated to a secondary datacenter to enable failover if a major disaster occurs in the primary location.</span></span> <span data-ttu-id="71c45-237">Geografická replikace může způsobit dodatečné náklady.</span><span class="sxs-lookup"><span data-stu-id="71c45-237">Geo-replication can incur additional costs.</span></span> <span data-ttu-id="71c45-238">V případě testovacích a vývojových účtů je zbytečné za geografickou replikaci platit.</span><span class="sxs-lookup"><span data-stu-id="71c45-238">For test and development accounts, you generally don't want to pay for geo-replication.</span></span> <span data-ttu-id="71c45-239">Další informace naleznete v článku o [vytvoření, správě nebo odstranění účtu úložiště](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="71c45-239">For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).</span></span>

5. <span data-ttu-id="71c45-240">V části **Skupina prostředků** klikněte na **Použít existující** a vyberte skupinu prostředků použitou pro cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="71c45-240">In the **Resource group**, click **Use existing** and select the resource group used for the cloud service.</span></span>
6. <span data-ttu-id="71c45-241">V rozevíracím seznamu **Umístění** vyberte stejnou oblast, jakou jste zvolili pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="71c45-241">Set the **Location** drop-down list to the same region you chose for the cloud service.</span></span>

    <span data-ttu-id="71c45-242">Když jsou cloudové služby a účet úložiště v různých datacentrech (různých oblastech), zvýší se latence a bude vám účtována šířka pásma mimo datové centrum.</span><span class="sxs-lookup"><span data-stu-id="71c45-242">When the cloud service and storage account are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside the data center.</span></span> <span data-ttu-id="71c45-243">Šířka pásma v rámci datového centra je zdarma.</span><span class="sxs-lookup"><span data-stu-id="71c45-243">Bandwidth within a data center is free.</span></span>

    <span data-ttu-id="71c45-244">Skupina vztahů Azure nabízí mechanismus pro minimalizaci vzdálenosti mezi prostředky v datovém centru (můžete tak omezit latenci).</span><span class="sxs-lookup"><span data-stu-id="71c45-244">Azure affinity groups provide a mechanism to minimize the distance between resources in a data center, which can reduce latency.</span></span> <span data-ttu-id="71c45-245">V tomto kurzu skupinu vztahů nepoužíváme.</span><span class="sxs-lookup"><span data-stu-id="71c45-245">This tutorial does not use affinity groups.</span></span> <span data-ttu-id="71c45-246">Další informace naleznete v článku o [vytváření skupiny vztahů v Azure](http://msdn.microsoft.com/library/jj156209.aspx).</span><span class="sxs-lookup"><span data-stu-id="71c45-246">For more information, see [How to Create an Affinity Group in Azure](http://msdn.microsoft.com/library/jj156209.aspx).</span></span>
7. <span data-ttu-id="71c45-247">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="71c45-247">Click **Create**.</span></span>

    ![Nový účet úložiště](./media/cloud-services-dotnet-get-started/newstorage.png)

    <span data-ttu-id="71c45-249">Na obrázku vidíte vytvoření účtu úložiště s adresou URL `csvccontosoads.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="71c45-249">In the image, a storage account is created with the URL `csvccontosoads.core.windows.net`.</span></span>

### <a name="configure-the-solution-to-use-your-azure-sql-database-when-it-runs-in-azure"></a><span data-ttu-id="71c45-250">Konfigurace řešení, aby při spuštění v Azure používalo databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-250">Configure the solution to use your Azure SQL database when it runs in Azure</span></span>
<span data-ttu-id="71c45-251">Webový projekt a projekt role pracovního procesu mají každý svůj vlastní připojovací řetězec k databázi a každý musí při spuštění aplikace v Azure odkazovat na databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-251">The web project and the worker role project each has its own database connection string, and each needs to point to the Azure SQL database when the app runs in Azure.</span></span>

<span data-ttu-id="71c45-252">Pro webovou roli a nastavení prostředí cloudové služby pro roli pracovního procesu budete používat [transformaci Web.config](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations).</span><span class="sxs-lookup"><span data-stu-id="71c45-252">You'll use a [Web.config transform](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) for the web role and a cloud service environment setting for the worker role.</span></span>

> [!NOTE]
> <span data-ttu-id="71c45-253">V této a v další části uložíte přihlašovací údaje do souborů projektu.</span><span class="sxs-lookup"><span data-stu-id="71c45-253">In this section and the next section, you store credentials in project files.</span></span> <span data-ttu-id="71c45-254">[Citlivá data neukládejte do veřejných úložišť  zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span><span class="sxs-lookup"><span data-stu-id="71c45-254">[Don't store sensitive data in public source code repositories](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span></span>
>
>

1. <span data-ttu-id="71c45-255">V projektu ContosoAdsWeb otevřete transformační soubor *Web.Release.config* pro soubor *Web.config* aplikace, odstraňte blok komentáře, který obsahuje prvek `<connectionStrings>`, a vložte následující kód na příslušné místo.</span><span class="sxs-lookup"><span data-stu-id="71c45-255">In the ContosoAdsWeb project, open the *Web.Release.config* transform file for the application *Web.config* file, delete the comment block that contains a `<connectionStrings>` element, and paste the following code in its place.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="{connectionstring}"
        providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
    </connectionStrings>
    ```

    <span data-ttu-id="71c45-256">Nechte soubor otevřený pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="71c45-256">Leave the file open for editing.</span></span>
2. <span data-ttu-id="71c45-257">Na portálu [Azure Portal](https://portal.azure.com) klikněte v levém podokně na **Databáze SQL**, klikněte na databázi, kterou jste si pro tento kurz vytvořili, a potom klikněte na **Zobrazit připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="71c45-257">In the [Azure portal](https://portal.azure.com), click **SQL Databases** in the left pane, click the database you created for this tutorial, and then click **Show connection strings**.</span></span>

    ![Zobrazení připojovacích řetězců](./media/cloud-services-dotnet-get-started/showcs.png)

    <span data-ttu-id="71c45-259">Portál zobrazí připojovací řetězce, místo hesla uvidíte zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="71c45-259">The portal displays connection strings, with a placeholder for the password.</span></span>

    ![Připojovací řetězce](./media/cloud-services-dotnet-get-started/connstrings.png)
3. <span data-ttu-id="71c45-261">V transformačním souboru *Web.Release.config* odstraňte text `{connectionstring}` a na jeho místo vložte připojovací řetězec ADO.NET z portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="71c45-261">In the *Web.Release.config* transform file, delete `{connectionstring}` and paste in its place the ADO.NET connection string from the Azure portal.</span></span>
4. <span data-ttu-id="71c45-262">V připojovacím řetězci, který jste vložili do transformačního souboru*Web.Release.config*, nahraďte text `{your_password_here}` heslem, které jste vytvořili pro novou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="71c45-262">In the connection string that you pasted into the *Web.Release.config* transform file, replace `{your_password_here}` with the password you created for the new SQL database.</span></span>
5. <span data-ttu-id="71c45-263">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="71c45-263">Save the file.</span></span>  
6. <span data-ttu-id="71c45-264">Vyberte a zkopírujte připojovací řetězec (bez okolních uvozovek), abyste ho mohli použít v následujících krocích konfigurace projektu role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="71c45-264">Select and copy the connection string (without the surrounding quotation marks) for use in the following steps for configuring the worker role project.</span></span>
7. <span data-ttu-id="71c45-265">V **Průzkumníku řešení** v části **Role** v projektu cloudové služby klikněte pravým tlačítkem na **ContosoAdsWorker** a potom klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="71c45-265">In **Solution Explorer**, under **Roles** in the cloud service project, right-click **ContosoAdsWorker** and then click **Properties**.</span></span>

    ![Vlastnosti rolí](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)
8. <span data-ttu-id="71c45-267">Klikněte na kartu **Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="71c45-267">Click the **Settings** tab.</span></span>
9. <span data-ttu-id="71c45-268">Změňte nastavení **Konfigurace služby** na **Cloud**.</span><span class="sxs-lookup"><span data-stu-id="71c45-268">Change **Service Configuration** to **Cloud**.</span></span>
10. <span data-ttu-id="71c45-269">V nastavení `ContosoAdsDbConnectionString` vyberte pole **Hodnota** a vložte do něj připojovací řetězec, který jste zkopírovali v předchozí části kurzu.</span><span class="sxs-lookup"><span data-stu-id="71c45-269">Select the **Value** field for the `ContosoAdsDbConnectionString` setting, and then paste the connection string that you copied from the previous section of the tutorial.</span></span>

     ![Připojovací řetězec databáze pro roli pracovního procesu](./media/cloud-services-dotnet-get-started/workerdbcs.png)
11. <span data-ttu-id="71c45-271">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="71c45-271">Save your changes.</span></span>  

### <a name="configure-the-solution-to-use-your-azure-storage-account-when-it-runs-in-azure"></a><span data-ttu-id="71c45-272">Konfigurace řešení, aby při spuštění v Azure používalo účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-272">Configure the solution to use your Azure storage account when it runs in Azure</span></span>
<span data-ttu-id="71c45-273">Připojovací řetězce k účtu úložiště Azure pro projekt webové role i projekt role pracovního procesu jsou uložené v nastavení prostředí v projektu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="71c45-273">Azure storage account connection strings for both the web role project and the worker role project are stored in environment settings in the cloud service project.</span></span> <span data-ttu-id="71c45-274">Každý projekt má samostatnou sadu nastavení, která se použije při spuštění aplikace místně a při spuštění v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71c45-274">For each project, there is a separate set of settings to be used when the application runs locally and when it runs in the cloud.</span></span> <span data-ttu-id="71c45-275">Nastavení cloudového prostředí budete aktualizovat pro webový projekt i pro projekt role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="71c45-275">You'll update the cloud environment settings for both web and worker role projects.</span></span>

1. <span data-ttu-id="71c45-276">V **Průzkumníku řešení** v části **Role** v projektu **ContosoAdsCloudService** klikněte pravým tlačítkem na **ContosoAdsWeb** a potom na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="71c45-276">In **Solution Explorer**, right-click **ContosoAdsWeb** under **Roles** in the **ContosoAdsCloudService** project, and then click **Properties**.</span></span>

    ![Vlastnosti rolí](./media/cloud-services-dotnet-get-started/roleproperties.png)
2. <span data-ttu-id="71c45-278">Klikněte na kartu **Nastavení**. V rozevíracím seznamu **Konfigurace služby** vyberte **Cloud**.</span><span class="sxs-lookup"><span data-stu-id="71c45-278">Click the **Settings** tab. In the **Service Configuration** drop-down box, choose **Cloud**.</span></span>

    ![Konfigurace cloudu](./media/cloud-services-dotnet-get-started/sccloud.png)
3. <span data-ttu-id="71c45-280">Vyberte položku **StorageConnectionString** a na pravém konci řádku se zobrazí tlačítko se třemi tečkami (**...**) .</span><span class="sxs-lookup"><span data-stu-id="71c45-280">Select the **StorageConnectionString** entry, and you'll see an ellipsis (**...**) button at the right end of the line.</span></span> <span data-ttu-id="71c45-281">Kliknutím na tlačítko se třemi tečkami otevřete dialogové okno **Vytvoření připojovací řetězce k účtu úložiště**.</span><span class="sxs-lookup"><span data-stu-id="71c45-281">Click the ellipsis button to open the **Create Storage Account Connection String** dialog box.</span></span>

    ![Otevření pole Vytvoření připojovacího řetězce](./media/cloud-services-dotnet-get-started/opencscreate.png)
4. <span data-ttu-id="71c45-283">V dialogovém okně **Vytvoření připojovacího řetězce k úložišti** klikněte na **Předplatné**, zvolte účet úložiště, které jste už dříve vytvořili, a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="71c45-283">In the **Create Storage Connection String** dialog box, click **Your subscription**, choose the storage account that you created earlier, and then click **OK**.</span></span> <span data-ttu-id="71c45-284">Pokud ještě nejste přihlášeni, budete vyzváni k zadání přihlašovacích údajů k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-284">If you're not already logged in, you'll be prompted for your Azure account credentials.</span></span>

    ![Vytvoření připojovacího řetězce k úložišti](./media/cloud-services-dotnet-get-started/createstoragecs.png)
5. <span data-ttu-id="71c45-286">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="71c45-286">Save your changes.</span></span>
6. <span data-ttu-id="71c45-287">Postupujte stejným způsobem, který jste použili pro připojovací řetězec `StorageConnectionString`, a nastavte připojovací řetězec `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="71c45-287">Follow the same procedure that you used for the `StorageConnectionString` connection string to set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` connection string.</span></span>

    <span data-ttu-id="71c45-288">Tento připojovací řetězec se používá k protokolování.</span><span class="sxs-lookup"><span data-stu-id="71c45-288">This connection string is used for logging.</span></span>
7. <span data-ttu-id="71c45-289">Postupujte stejným způsobem, který jste použili pro roli **ContosoAdsWeb**, a nastavte oba připojovací řetězce pro roli **ContosoAdsWorker**.</span><span class="sxs-lookup"><span data-stu-id="71c45-289">Follow the same procedure that you used for the **ContosoAdsWeb** role to set both connection strings for the **ContosoAdsWorker** role.</span></span> <span data-ttu-id="71c45-290">Nezapomeňte nastavit hodnotu **Konfigurace služby** na **Cloud**.</span><span class="sxs-lookup"><span data-stu-id="71c45-290">Don't forget to set **Service Configuration** to **Cloud**.</span></span>

<span data-ttu-id="71c45-291">Nastavení prostředí role, které jste nakonfigurovali pomocí rozhraní Visual Studia, jsou uložená v následujících souborech v projektu ContosoAdsCloudService:</span><span class="sxs-lookup"><span data-stu-id="71c45-291">The role environment settings that you have configured using the Visual Studio UI are stored in the following files in the ContosoAdsCloudService project:</span></span>

* <span data-ttu-id="71c45-292">*ServiceDefinition.csdef* – definuje názvy nastavení.</span><span class="sxs-lookup"><span data-stu-id="71c45-292">*ServiceDefinition.csdef* - Defines the setting names.</span></span>
* <span data-ttu-id="71c45-293">*ServiceConfiguration.Cloud.cscfg* – poskytuje hodnoty pro situace, kdy aplikace běží v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71c45-293">*ServiceConfiguration.Cloud.cscfg* - Provides values for when the app runs in the cloud.</span></span>
* <span data-ttu-id="71c45-294">*ServiceConfiguration.Local.cscfg* – poskytuje hodnoty pro situace, kdy aplikace běží místně.</span><span class="sxs-lookup"><span data-stu-id="71c45-294">*ServiceConfiguration.Local.cscfg* - Provides values for when the app runs locally.</span></span>

<span data-ttu-id="71c45-295">Například soubor ServiceDefinition.csdef obsahuje následující definice:</span><span class="sxs-lookup"><span data-stu-id="71c45-295">For example, the ServiceDefinition.csdef includes the following definitions:</span></span>

```xml
<ConfigurationSettings>
    <Setting name="StorageConnectionString" />
    <Setting name="ContosoAdsDbConnectionString" />
</ConfigurationSettings>
```

<span data-ttu-id="71c45-296">A soubor *ServiceConfiguration.Cloud.cscfg* obsahuje hodnoty, které jste pro tato nastavení zadali ve Visual Studiu.</span><span class="sxs-lookup"><span data-stu-id="71c45-296">And the *ServiceConfiguration.Cloud.cscfg* file includes the values you entered for those settings in Visual Studio.</span></span>

```xml
<Role name="ContosoAdsWorker">
    <Instances count="1" />
    <ConfigurationSettings>
        <Setting name="StorageConnectionString" value="{yourconnectionstring}" />
        <Setting name="ContosoAdsDbConnectionString" value="{yourconnectionstring}" />
        <!-- other settings not shown -->

    </ConfigurationSettings>
    <!-- other settings not shown -->

</Role>
```

<span data-ttu-id="71c45-297">Nastavení `<Instances>` určuje počet virtuálních počítačů, na kterých Azure spustí kód role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="71c45-297">The `<Instances>` setting specifies the number of virtual machines that Azure will run the worker role code on.</span></span> <span data-ttu-id="71c45-298">Část [Další kroky](#next-steps) obsahuje odkazy na další informace o škálování cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="71c45-298">The [Next steps](#next-steps) section includes links to more information about scaling out a cloud service,</span></span>

### <a name="deploy-the-project-to-azure"></a><span data-ttu-id="71c45-299">Nasazení projektu do Azure</span><span class="sxs-lookup"><span data-stu-id="71c45-299">Deploy the project to Azure</span></span>
1. <span data-ttu-id="71c45-300">V **Průzkumníku řešení** klikněte pravým tlačítkem na cloudový projekt **ContosoAdsCloudService** a potom vyberte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="71c45-300">In **Solution Explorer**, right-click the **ContosoAdsCloudService** cloud project and then select **Publish**.</span></span>

   ![Publikování nabídky](./media/cloud-services-dotnet-get-started/pubmenu.png)
2. <span data-ttu-id="71c45-302">V průvodci **Publikování aplikaci Azure** v kroku **Přihlásit se** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="71c45-302">In the **Sign in** step of the **Publish Azure Application** wizard, click **Next**.</span></span>

    ![Krok Přihlásit se](./media/cloud-services-dotnet-get-started/pubsignin.png)
3. <span data-ttu-id="71c45-304">Klikněte v průvodci v kroku **Nastavení** na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="71c45-304">In the **Settings** step of the wizard, click **Next**.</span></span>

    ![Krok Nastavení](./media/cloud-services-dotnet-get-started/pubsettings.png)

    <span data-ttu-id="71c45-306">Výchozí nastavení na kartě **Upřesnit** jsou pro účely tohoto kurzu vyhovující.</span><span class="sxs-lookup"><span data-stu-id="71c45-306">The default settings in the **Advanced** tab are fine for this tutorial.</span></span> <span data-ttu-id="71c45-307">Informace o kartě Upřesnit naleznete v článku o [průvodci publikováním aplikace Azure](http://msdn.microsoft.com/library/hh535756.aspx).</span><span class="sxs-lookup"><span data-stu-id="71c45-307">For information about the advanced tab, see [Publish Azure Application Wizard](http://msdn.microsoft.com/library/hh535756.aspx).</span></span>
4. <span data-ttu-id="71c45-308">V kroku **Souhrn** klikněte na tlačítko **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="71c45-308">In the **Summary** step, click **Publish**.</span></span>

    ![Krok Souhrn](./media/cloud-services-dotnet-get-started/pubsummary.png)

   <span data-ttu-id="71c45-310">Ve Visual Studiu se otevře okno **Protokol činnosti Azure**.</span><span class="sxs-lookup"><span data-stu-id="71c45-310">The **Azure Activity Log** window opens in Visual Studio.</span></span>
5. <span data-ttu-id="71c45-311">Kliknutím na ikonu se šipkou doprava rozbalíte podrobnosti o nasazení.</span><span class="sxs-lookup"><span data-stu-id="71c45-311">Click the right arrow icon to expand the deployment details.</span></span>

    <span data-ttu-id="71c45-312">Dokončení nasazení může trvat 5 minut i déle.</span><span class="sxs-lookup"><span data-stu-id="71c45-312">The deployment can take up to 5 minutes or more to complete.</span></span>

    ![Okno Protokol činnosti Azure](./media/cloud-services-dotnet-get-started/waal.png)
6. <span data-ttu-id="71c45-314">Po dokončení nasazení klikněte na **Adresa URL webové aplikace** a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="71c45-314">When the deployment status is complete, click the **Web app URL** to start the application.</span></span>
7. <span data-ttu-id="71c45-315">Teď můžete aplikaci otestovat a vytvořit, zobrazit nebo upravit některé reklamy, stejně jako jste to dělali, když byla aplikace spuštěná místně.</span><span class="sxs-lookup"><span data-stu-id="71c45-315">You can now test the app by creating, viewing, and editing some ads, as you did when you ran the application locally.</span></span>

> [!NOTE]
> <span data-ttu-id="71c45-316">Až budete s testováním hotovi, odstraňte nebo zastavte cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="71c45-316">When you're finished testing, delete or stop the cloud service.</span></span> <span data-ttu-id="71c45-317">Poplatky vám totiž nabíhají i když cloudové služby nepoužíváte, protože jsou pro ně vyhrazené prostředky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="71c45-317">Even if you're not using the cloud service, it's accruing charges because virtual machine resources are reserved for it.</span></span> <span data-ttu-id="71c45-318">A pokud je necháte spuštěné, může každý, kdo najde vaši adresu URL, vytvářet a zobrazovat reklamy.</span><span class="sxs-lookup"><span data-stu-id="71c45-318">And if you leave it running, anyone who finds your URL can create and view ads.</span></span> <span data-ttu-id="71c45-319">Na portálu [Azure Portal](https://portal.azure.com) přejděte na kartu **Přehled** vaší cloudové služby a potom v horní části stránky klikněte na tlačítko **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="71c45-319">In the [Azure portal](https://portal.azure.com), go to the **Overview** tab for your cloud service, and then click the **Delete** button at the top of the page.</span></span> <span data-ttu-id="71c45-320">Pokud chcete ostatním jen dočasně znemožnit přístup na web, klikněte místo toho na tlačítko **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="71c45-320">If you just want to temporarily prevent others from accessing the site, click **Stop** instead.</span></span> <span data-ttu-id="71c45-321">V takovém případě budou poplatky dál nabíhat.</span><span class="sxs-lookup"><span data-stu-id="71c45-321">In that case, charges will continue to accrue.</span></span> <span data-ttu-id="71c45-322">Podobným způsobem můžete odstranit nepotřebnou databázi SQL a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="71c45-322">You can follow a similar procedure to delete the SQL database and storage account when you no longer need them.</span></span>
>
>

## <a name="create-the-application-from-scratch"></a><span data-ttu-id="71c45-323">Vytvoření aplikace od začátku</span><span class="sxs-lookup"><span data-stu-id="71c45-323">Create the application from scratch</span></span>
<span data-ttu-id="71c45-324">Pokud jste [dokončenou aplikaci](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) ještě nestáhli, udělejte to teď.</span><span class="sxs-lookup"><span data-stu-id="71c45-324">If you haven't already downloaded [the completed application](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), do that now.</span></span> <span data-ttu-id="71c45-325">Soubory ze staženého projektu budete kopírovat do nového projektu.</span><span class="sxs-lookup"><span data-stu-id="71c45-325">You'll copy files from the downloaded project into the new project.</span></span>

<span data-ttu-id="71c45-326">Vytvoření aplikace Contoso Ads zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="71c45-326">Creating the Contoso Ads application involves the following steps:</span></span>

* <span data-ttu-id="71c45-327">Vytvoření řešení cloudové služby Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71c45-327">Create a cloud service Visual Studio solution.</span></span>
* <span data-ttu-id="71c45-328">Aktualizace a přidání balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="71c45-328">Update and add NuGet packages.</span></span>
* <span data-ttu-id="71c45-329">Nastavení odkazů projektu</span><span class="sxs-lookup"><span data-stu-id="71c45-329">Set project references.</span></span>
* <span data-ttu-id="71c45-330">Konfigurace připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="71c45-330">Configure connection strings.</span></span>
* <span data-ttu-id="71c45-331">Přidání souborů s kódy</span><span class="sxs-lookup"><span data-stu-id="71c45-331">Add code files.</span></span>

<span data-ttu-id="71c45-332">Po vytvoření řešení zkontrolujete kód, který je pro projekty cloudových služeb a objekty blob a fronty Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="71c45-332">After the solution is created, you'll review the code that is unique to cloud service projects and Azure blobs and queues.</span></span>

### <a name="create-a-cloud-service-visual-studio-solution"></a><span data-ttu-id="71c45-333">Vytvoření řešení cloudové služby Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71c45-333">Create a cloud service Visual Studio solution</span></span>
1. <span data-ttu-id="71c45-334">Ve Visual Studiu zvolte v nabídce **Soubor** možnost **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="71c45-334">In Visual Studio, choose **New Project** from the **File** menu.</span></span>
2. <span data-ttu-id="71c45-335">V levém podokně dialogového okna **Nový projekt** rozbalte položku **Visual C#**, vyberte šablonu **Cloud** a potom klikněte na šablonu **Cloudová služba Azure**.</span><span class="sxs-lookup"><span data-stu-id="71c45-335">In the left pane of the **New Project** dialog box, expand **Visual C#** and choose **Cloud** templates, and then choose the **Azure Cloud Service** template.</span></span>
3. <span data-ttu-id="71c45-336">Pojmenujte projekt a řešení ContosoAdsCloudService a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="71c45-336">Name the project and solution ContosoAdsCloudService, and then click **OK**.</span></span>

    ![Nový projekt](./media/cloud-services-dotnet-get-started/newproject.png)
4. <span data-ttu-id="71c45-338">V dialogovém okně **Nová cloudová služba Azure** přidejte webovou roli a roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="71c45-338">In the **New Azure Cloud Service** dialog box, add a web role and a worker role.</span></span> <span data-ttu-id="71c45-339">Pojmenujte webovou roli ContosoAdsWeb a roli pracovního procesu ContosoAdsWorker.</span><span class="sxs-lookup"><span data-stu-id="71c45-339">Name the web role ContosoAdsWeb, and name the worker role ContosoAdsWorker.</span></span> <span data-ttu-id="71c45-340">(Ke změně výchozích názvů rolí použijte ikonu tužky v pravém podokně.)</span><span class="sxs-lookup"><span data-stu-id="71c45-340">(Use the pencil icon in the right-hand pane to change the default names of the roles.)</span></span>

    ![Nový projekt cloudové služby](./media/cloud-services-dotnet-get-started/newcsproj.png)
5. <span data-ttu-id="71c45-342">Po zobrazení dialogového okna **Nový projekt ASP.NET** pro webovou roli vyberte šablonu MVC a potom klikněte na **Změnit ověřování**.</span><span class="sxs-lookup"><span data-stu-id="71c45-342">When you see the **New ASP.NET Project** dialog box for the web role, choose the MVC template, and then click **Change Authentication**.</span></span>

    ![Změna ověřování](./media/cloud-services-dotnet-get-started/chgauth.png)
6. <span data-ttu-id="71c45-344">V dialogovém okně **Změna ověřování** vyberte **Bez ověřování** a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="71c45-344">In the **Change Authentication** dialog box, choose **No Authentication**, and then click **OK**.</span></span>

    ![Bez ověřování](./media/cloud-services-dotnet-get-started/noauth.png)
7. <span data-ttu-id="71c45-346">V dialogovém okně **Nový projekt ASP.NET** klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="71c45-346">In the **New ASP.NET Project** dialog, click **OK**.</span></span>
8. <span data-ttu-id="71c45-347">V **Průzkumníku řešení** klikněte pravým tlačítkem na řešení (ne na projekt) a zvolte **Přidat – nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="71c45-347">In **Solution Explorer**, right-click the solution (not one of the projects), and choose **Add - New Project**.</span></span>
9. <span data-ttu-id="71c45-348">V dialogovém okně **Přidání nového projektu**  klikněte v levém podokně v části **Visual C#** na tlačítko **Windows** a potom klikněte na šablonu **Knihovna tříd**.</span><span class="sxs-lookup"><span data-stu-id="71c45-348">In the **Add New Project** dialog box, choose **Windows** under **Visual C#** in the left pane, and then click the **Class Library** template.</span></span>  
10. <span data-ttu-id="71c45-349">Pojmenujte projekt *ContosoAdsCommon* a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="71c45-349">Name the project *ContosoAdsCommon*, and then click **OK**.</span></span>

    <span data-ttu-id="71c45-350">Na kontext Entity Framework a datový model je třeba odkazovat z projektů webové role i role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="71c45-350">You need to reference the Entity Framework context and the data model from both web and worker role projects.</span></span> <span data-ttu-id="71c45-351">Jako alternativu můžete třídy související s EF definovat v projektu webové role a odkazovat na takový projekt z projektu role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="71c45-351">As an alternative, you could define the EF-related classes in the web role project and reference that project from the worker role project.</span></span> <span data-ttu-id="71c45-352">V tomto alternativním přístupu bude projekt role pracovního procesu obsahovat odkaz na webová sestavení, která nepotřebuje.</span><span class="sxs-lookup"><span data-stu-id="71c45-352">But in the alternative approach, your worker role project would have a reference to web assemblies that it doesn't need.</span></span>

### <a name="update-and-add-nuget-packages"></a><span data-ttu-id="71c45-353">Aktualizace a přidání balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="71c45-353">Update and add NuGet packages</span></span>
1. <span data-ttu-id="71c45-354">Otevřete dialogové okno **Správa balíčků NuGet** řešení.</span><span class="sxs-lookup"><span data-stu-id="71c45-354">Open the **Manage NuGet Packages** dialog box for the solution.</span></span>
2. <span data-ttu-id="71c45-355">V horní části okna vyberte **Aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="71c45-355">At the top of the window, select **Updates**.</span></span>
3. <span data-ttu-id="71c45-356">Najděte balíček *WindowsAzure.Storage* a pokud je v seznamu, vyberte ho a vyberte webový projekt a projekt pracovního procesu, ve kterých ho chcete aktualizovat, a potom klikněte na **Aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="71c45-356">Look for the *WindowsAzure.Storage* package, and if it's in the list, select it and select the web and worker projects to update it in, and then click **Update**.</span></span>

    <span data-ttu-id="71c45-357">Knihovna klienta úložiště se aktualizuje častěji než šablony projektů Visual Studio, takže se často stává, že verzi u nově vytvořeného projektu je potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="71c45-357">The storage client library is updated more frequently than Visual Studio project templates, so you'll often find that the version in a newly-created project needs to be updated.</span></span>
4. <span data-ttu-id="71c45-358">V horní části okna vyberte **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="71c45-358">At the top of the window, select **Browse**.</span></span>
5. <span data-ttu-id="71c45-359">Najděte balíček NuGet *EntityFramework* a nainstalujte ho do všech tří projektů.</span><span class="sxs-lookup"><span data-stu-id="71c45-359">Find the *EntityFramework* NuGet package, and install it in all three projects.</span></span>
6. <span data-ttu-id="71c45-360">Najděte balíček NuGet *Microsoft.WindowsAzure.ConfigurationManager* a nainstalujte ho do projektu role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="71c45-360">Find the *Microsoft.WindowsAzure.ConfigurationManager* NuGet package, and install it in the worker role project.</span></span>

### <a name="set-project-references"></a><span data-ttu-id="71c45-361">Nastavení odkazů na projekty</span><span class="sxs-lookup"><span data-stu-id="71c45-361">Set project references</span></span>
1. <span data-ttu-id="71c45-362">V projektu ContosoAdsWeb nastavte odkaz na projekt ContosoAdsCommon.</span><span class="sxs-lookup"><span data-stu-id="71c45-362">In the ContosoAdsWeb project, set a reference to the ContosoAdsCommon project.</span></span> <span data-ttu-id="71c45-363">Klikněte pravým tlačítkem na projekt ContosoAdsWeb a potom klikněte na **Odkazy** - **Přidat odkazy**.</span><span class="sxs-lookup"><span data-stu-id="71c45-363">Right-click the ContosoAdsWeb project, and then click **References** - **Add References**.</span></span> <span data-ttu-id="71c45-364">V dialogovém okně **Správce odkazů** vyberte v levém podokně **Řešení – projekty**, vyberte **ContosoAdsCommon** a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="71c45-364">In the **Reference Manager** dialog box, select **Solution – Projects** in the left pane, select **ContosoAdsCommon**, and then click **OK**.</span></span>
2. <span data-ttu-id="71c45-365">V projektu ContosoAdsWorker nastavte odkaz na projekt ContosAdsCommon.</span><span class="sxs-lookup"><span data-stu-id="71c45-365">In the ContosoAdsWorker project, set a reference to the ContosAdsCommon project.</span></span>

    <span data-ttu-id="71c45-366">ContosoAdsCommon bude obsahovat datový model a třídu kontextu Entity Framework, které použije front-end i back-end.</span><span class="sxs-lookup"><span data-stu-id="71c45-366">ContosoAdsCommon will contain the Entity Framework data model and context class, which will be used by both the front-end and back-end.</span></span>
3. <span data-ttu-id="71c45-367">V projektu ContosoAdsWorker nastavte odkaz na `System.Drawing`.</span><span class="sxs-lookup"><span data-stu-id="71c45-367">In the ContosoAdsWorker project, set a reference to `System.Drawing`.</span></span>

    <span data-ttu-id="71c45-368">Back-end toto sestavení používá k převodu obrázků na miniatury.</span><span class="sxs-lookup"><span data-stu-id="71c45-368">This assembly is used by the back-end to convert images to thumbnails.</span></span>

### <a name="configure-connection-strings"></a><span data-ttu-id="71c45-369">Konfigurace připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="71c45-369">Configure connection strings</span></span>
<span data-ttu-id="71c45-370">V této části budete konfigurovat službu Azure Storage a připojovací řetězce SQL pro místní testování.</span><span class="sxs-lookup"><span data-stu-id="71c45-370">In this section, you configure Azure Storage and SQL connection strings for testing locally.</span></span> <span data-ttu-id="71c45-371">Pokyny pro nasazení (uvedené už dříve) vysvětlují, jak nastavit připojovací řetězce pro situaci, kdy aplikace běží v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71c45-371">The deployment instructions earlier in the tutorial explain how to set up the connection strings for when the app runs in the cloud.</span></span>

1. <span data-ttu-id="71c45-372">V projektu ContosoAdsWeb otevřete aplikační soubor Web.config a vložte následující prvek `connectionStrings` za prvek `configSections`.</span><span class="sxs-lookup"><span data-stu-id="71c45-372">In the ContosoAdsWeb project, open the application Web.config file, and insert the following `connectionStrings` element after the `configSections` element.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    <span data-ttu-id="71c45-373">Pokud používáte Visual Studio 2015 nebo vyšší, nahraďte text „v11.0“ textem „MSSQLLocalDB“.</span><span class="sxs-lookup"><span data-stu-id="71c45-373">If you're using Visual Studio 2015 or higher, replace "v11.0" with "MSSQLLocalDB".</span></span>
2. <span data-ttu-id="71c45-374">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="71c45-374">Save your changes.</span></span>
3. <span data-ttu-id="71c45-375">Klikněte v projektu ContosoAdsCloudService pravým tlačítkem v části **Role** na ContosoAdsWeb a potom klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="71c45-375">In the ContosoAdsCloudService project, right-click ContosoAdsWeb under **Roles**, and then click **Properties**.</span></span>

    ![Vlastnosti rolí](./media/cloud-services-dotnet-get-started/roleproperties.png)
4. <span data-ttu-id="71c45-377">V okně vlastností **ContosAdsWeb [Role]** klikněte na kartu **Nastavení** a potom na **Přidat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="71c45-377">In the **ContosAdsWeb [Role]** properties window, click the **Settings** tab, and then click **Add Setting**.</span></span>

    <span data-ttu-id="71c45-378">Možnost **Konfigurace služby** nechte nastavenou na **Všechny konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="71c45-378">Leave **Service Configuration** set to **All Configurations**.</span></span>
5. <span data-ttu-id="71c45-379">Přidejte nastavení s názvem *StorageConnectionString*.</span><span class="sxs-lookup"><span data-stu-id="71c45-379">Add a setting named *StorageConnectionString*.</span></span> <span data-ttu-id="71c45-380">Nastavte **Typ** na *ConnectionString* a možnost **Hodnota** nastavte na *UseDevelopmentStorage=true*.</span><span class="sxs-lookup"><span data-stu-id="71c45-380">Set **Type** to *ConnectionString*, and set **Value** to *UseDevelopmentStorage=true*.</span></span>

    ![Nový připojovací řetězec](./media/cloud-services-dotnet-get-started/scall.png)
6. <span data-ttu-id="71c45-382">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="71c45-382">Save your changes.</span></span>
7. <span data-ttu-id="71c45-383">Postupujte stejným způsobem a přidejte připojovací řetězec úložiště do vlastností role ContosoAdsWorker.</span><span class="sxs-lookup"><span data-stu-id="71c45-383">Follow the same procedure to add a storage connection string in the ContosoAdsWorker role properties.</span></span>
8. <span data-ttu-id="71c45-384">Ještě v okně vlastností **ContosoAdsWorker [Role]** přidejte další připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="71c45-384">Still in the **ContosoAdsWorker [Role]** properties window, add another connection string:</span></span>

   * <span data-ttu-id="71c45-385">Název: ContosoAdsDbConnectionString</span><span class="sxs-lookup"><span data-stu-id="71c45-385">Name: ContosoAdsDbConnectionString</span></span>
   * <span data-ttu-id="71c45-386">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="71c45-386">Type: String</span></span>
   * <span data-ttu-id="71c45-387">Hodnota: Vložte stejný připojovací řetězec, který jste použili pro projekt webové role.</span><span class="sxs-lookup"><span data-stu-id="71c45-387">Value: Paste the same connection string you used for the web role project.</span></span> <span data-ttu-id="71c45-388">(Následující příklad je určený pro Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="71c45-388">(The following example is for Visual Studio 2013.</span></span> <span data-ttu-id="71c45-389">Pokud tento příklad kopírujete a používáte Visual Studio 2015 nebo vyšší, nezapomeňte změnit zdroj dat.)</span><span class="sxs-lookup"><span data-stu-id="71c45-389">Don't forget to change the Data Source if you copy this example and you're using Visual Studio 2015 or higher.)</span></span>

       ```
       Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;
       ```

### <a name="add-code-files"></a><span data-ttu-id="71c45-390">Přidání souborů s kódy</span><span class="sxs-lookup"><span data-stu-id="71c45-390">Add code files</span></span>
<span data-ttu-id="71c45-391">V této části zkopírujete soubory s kódy ze staženého řešení do nového řešení.</span><span class="sxs-lookup"><span data-stu-id="71c45-391">In this section, you copy code files from the downloaded solution into the new solution.</span></span> <span data-ttu-id="71c45-392">Následující části vám ukáží a vysvětlí klíčová místa tohoto kódu.</span><span class="sxs-lookup"><span data-stu-id="71c45-392">The following sections will show and explain key parts of this code.</span></span>

<span data-ttu-id="71c45-393">Pokud chcete přidat soubory do projektu nebo složky, klikněte pravým tlačítkem na projekt nebo složku a potom klikněte na **Přidat** - **Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="71c45-393">To add files to a project or a folder, right-click the project or folder and click **Add** - **Existing Item**.</span></span> <span data-ttu-id="71c45-394">Vyberte požadované soubory a potom klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="71c45-394">Select the files you want and then click **Add**.</span></span> <span data-ttu-id="71c45-395">Pokud se zobrazí dotaz, jestli chcete nahradit existující soubory, klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="71c45-395">If asked whether you want to replace existing files, click **Yes**.</span></span>

1. <span data-ttu-id="71c45-396">V projektu ContosoAdsCommon odstraňte soubor *Class1.cs* a na jeho místo přidejte soubory *Ad.cs* a *ContosoAdscontext.cs* ze staženého projektu.</span><span class="sxs-lookup"><span data-stu-id="71c45-396">In the ContosoAdsCommon project, delete the *Class1.cs* file and add in its place the *Ad.cs* and *ContosoAdscontext.cs* files from the downloaded project.</span></span>
2. <span data-ttu-id="71c45-397">Do projektu ContosoAdsWeb přidejte následující soubory ze staženého projektu.</span><span class="sxs-lookup"><span data-stu-id="71c45-397">In the ContosoAdsWeb project, add the following files from the downloaded project.</span></span>

   * <span data-ttu-id="71c45-398">*Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="71c45-398">*Global.asax.cs*.</span></span>  
   * <span data-ttu-id="71c45-399">Do složky *Views\Shared*: *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="71c45-399">In the *Views\Shared* folder: *\_Layout.cshtml*.</span></span>
   * <span data-ttu-id="71c45-400">Do složky *Views\Home*: *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="71c45-400">In the *Views\Home* folder: *Index.cshtml*.</span></span>
   * <span data-ttu-id="71c45-401">Do složky *Controllers*: *AdController.cs*.</span><span class="sxs-lookup"><span data-stu-id="71c45-401">In the *Controllers* folder: *AdController.cs*.</span></span>
   * <span data-ttu-id="71c45-402">Do složky *Views\Ad* (nejdřív složku vytvořte): pět souborů *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="71c45-402">In the *Views\Ad* folder (create the folder first): five *.cshtml* files.</span></span>
3. <span data-ttu-id="71c45-403">Do projektu ContosoAdsWorker přidejte soubor *WorkerRole.cs* ze staženého projektu.</span><span class="sxs-lookup"><span data-stu-id="71c45-403">In the ContosoAdsWorker project, add *WorkerRole.cs* from the downloaded project.</span></span>

<span data-ttu-id="71c45-404">Teď můžete sestavit a spustit aplikaci podle dříve uvedených pokynů tohoto kurzu a aplikace bude používat prostředky místní databáze a emulátoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="71c45-404">You can now build and run the application as instructed earlier in the tutorial, and the app will use local database and storage emulator resources.</span></span>

<span data-ttu-id="71c45-405">Následující části popisují kód týkající se práce s prostředím Azure, objekty blob a frontami.</span><span class="sxs-lookup"><span data-stu-id="71c45-405">The following sections explain the code related to working with the Azure environment, blobs, and queues.</span></span> <span data-ttu-id="71c45-406">Tento kurzu nevysvětluje, jak máte vytvořit kontrolery a zobrazení MVC pomocí generování uživatelského rozhraní, jak napsat kód Entity Framework, který funguje s databází serveru SQL Server, ani nepopisuje základy asynchronního programování v technologii ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="71c45-406">This tutorial does not explain how to create MVC controllers and views using scaffolding, how to write Entity Framework code that works with SQL Server databases, or the basics of asynchronous programming in ASP.NET 4.5.</span></span> <span data-ttu-id="71c45-407">Informace o těchto tématech naleznete v následujících zdrojích:</span><span class="sxs-lookup"><span data-stu-id="71c45-407">For information about these topics, see the following resources:</span></span>

* [<span data-ttu-id="71c45-408">Začínáme s MVC 5</span><span class="sxs-lookup"><span data-stu-id="71c45-408">Get started with MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="71c45-409">Začínáme s EF 6 a MVC 5</span><span class="sxs-lookup"><span data-stu-id="71c45-409">Get started with EF 6 and MVC 5</span></span>](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc)
* <span data-ttu-id="71c45-410">[Úvod do asynchronního programování na platformě .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span><span class="sxs-lookup"><span data-stu-id="71c45-410">[Introduction to asynchronous programming in .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span></span>

### <a name="contosoadscommon---adcs"></a><span data-ttu-id="71c45-411">ContosoAdsCommon – Ad.cs</span><span class="sxs-lookup"><span data-stu-id="71c45-411">ContosoAdsCommon - Ad.cs</span></span>
<span data-ttu-id="71c45-412">Soubor Ad.cs definuje výčet kategorií reklam a třídu entity objektů POCO pro informace o reklamách.</span><span class="sxs-lookup"><span data-stu-id="71c45-412">The Ad.cs file defines an enum for ad categories and a POCO entity class for ad information.</span></span>

```csharp
public enum Category
{
    Cars,
    [Display(Name="Real Estate")]
    RealEstate,
    [Display(Name = "Free Stuff")]
    FreeStuff
}

public class Ad
{
    public int AdId { get; set; }

    [StringLength(100)]
    public string Title { get; set; }

    public int Price { get; set; }

    [StringLength(1000)]
    [DataType(DataType.MultilineText)]
    public string Description { get; set; }

    [StringLength(1000)]
    [DisplayName("Full-size Image")]
    public string ImageURL { get; set; }

    [StringLength(1000)]
    [DisplayName("Thumbnail")]
    public string ThumbnailURL { get; set; }

    [DataType(DataType.Date)]
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
    public DateTime PostedDate { get; set; }

    public Category? Category { get; set; }
    [StringLength(12)]
    public string Phone { get; set; }
}
```

### <a name="contosoadscommon---contosoadscontextcs"></a><span data-ttu-id="71c45-413">ContosoAdsCommon – ContosoAdsContext.cs</span><span class="sxs-lookup"><span data-stu-id="71c45-413">ContosoAdsCommon - ContosoAdsContext.cs</span></span>
<span data-ttu-id="71c45-414">Třída ContosoAdsContext určuje použití třídy reklamy v kolekci DbSet, kterou Entity Framework uloží do databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="71c45-414">The ContosoAdsContext class specifies that the Ad class is used in a DbSet collection, which Entity Framework will store in a SQL database.</span></span>

```csharp
public class ContosoAdsContext : DbContext
{
    public ContosoAdsContext() : base("name=ContosoAdsContext")
    {
    }
    public ContosoAdsContext(string connString)
        : base(connString)
    {
    }
    public System.Data.Entity.DbSet<Ad> Ads { get; set; }
}
```

<span data-ttu-id="71c45-415">Třída má dva konstruktory.</span><span class="sxs-lookup"><span data-stu-id="71c45-415">The class has two constructors.</span></span> <span data-ttu-id="71c45-416">První z nich používán webovým projektem a určuje název připojovacího řetězce, který je uložený v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="71c45-416">The first of them is used by the web project, and specifies the name of a connection string that is stored in the Web.config file.</span></span> <span data-ttu-id="71c45-417">Druhý konstruktor vám umožňuje předat samotný připojovací řetězec používaný projektem role pracovního projektu, protože nemá soubor Web.config.</span><span class="sxs-lookup"><span data-stu-id="71c45-417">The second constructor enables you to pass in the actual connection string used by the worker role project, since it doesn't have a Web.config file.</span></span> <span data-ttu-id="71c45-418">Už dříve jste viděli, kam se tento připojovací řetězec uložil, a později uvidíte, jak kód získává připojovací řetězec při vytvoření instance třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="71c45-418">You saw earlier where this connection string was stored, and you'll see later how the code retrieves the connection string when it instantiates the DbContext class.</span></span>

### <a name="contosoadsweb---globalasaxcs"></a><span data-ttu-id="71c45-419">ContosoAdsWeb – Global.asax.cs</span><span class="sxs-lookup"><span data-stu-id="71c45-419">ContosoAdsWeb - Global.asax.cs</span></span>
<span data-ttu-id="71c45-420">Kód, který se volá z metody `Application_Start`, vytvoří kontejner objektů blob s *obrázky* a frontu *obrázků*, pokud ještě neexistují.</span><span class="sxs-lookup"><span data-stu-id="71c45-420">Code that is called from the `Application_Start` method creates an *images* blob container and an *images* queue if they don't already exist.</span></span> <span data-ttu-id="71c45-421">To zajišťuje, že při každém spuštění pomocí nového účtu úložiště nebo při spuštění pomocí emulátoru úložiště v novém počítači budou požadovaný kontejner objektů blob a fronta vytvořeny automaticky.</span><span class="sxs-lookup"><span data-stu-id="71c45-421">This ensures that whenever you start using a new storage account, or start using the storage emulator on a new computer, the required blob container and queue will be created automatically.</span></span>

<span data-ttu-id="71c45-422">Kód získá přístup k účtu úložiště pomocí připojovacího řetězec úložiště ze souboru *.cscfg*.</span><span class="sxs-lookup"><span data-stu-id="71c45-422">The code gets access to the storage account by using the storage connection string from the *.cscfg* file.</span></span>

```csharp
var storageAccount = CloudStorageAccount.Parse
    (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));
```

<span data-ttu-id="71c45-423">Potom získá odkaz na kontejner objektů blob s *obrázky*, pokud kontejner neexistuje, tak ho vytvoří, a nastaví přístupová oprávnění na nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="71c45-423">Then it gets a reference to the *images* blob container, creates the container if it doesn't already exist, and sets access permissions on the new container.</span></span> <span data-ttu-id="71c45-424">Ve výchozím nastavení povolí nové kontejnery přístup k objektům blob jenom klientům s přihlašovacími údaji k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="71c45-424">By default, new containers only allow clients with storage account credentials to access blobs.</span></span> <span data-ttu-id="71c45-425">Web potřebuje, aby objekty blob byly veřejné, jinak by nemohl zobrazit obrázky pomocí adres URL, které odkazují na objekty blob s obrázky.</span><span class="sxs-lookup"><span data-stu-id="71c45-425">The website needs the blobs to be public so that it can display images using URLs that point to the image blobs.</span></span>

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
var imagesBlobContainer = blobClient.GetContainerReference("images");
if (imagesBlobContainer.CreateIfNotExists())
{
    imagesBlobContainer.SetPermissions(
        new BlobContainerPermissions
        {
            PublicAccess =BlobContainerPublicAccessType.Blob
        });
}
```

<span data-ttu-id="71c45-426">Podobný kód získá odkaz na frontu *obrázků* a vytvoří novou frontu.</span><span class="sxs-lookup"><span data-stu-id="71c45-426">Similar code gets a reference to the *images* queue and creates a new queue.</span></span> <span data-ttu-id="71c45-427">V tomto případě nejsou nutná žádná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="71c45-427">In this case, no permissions change is needed.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
var imagesQueue = queueClient.GetQueueReference("images");
imagesQueue.CreateIfNotExists();
```

### <a name="contosoadsweb---layoutcshtml"></a><span data-ttu-id="71c45-428">ContosoAdsWeb – \_Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="71c45-428">ContosoAdsWeb - \_Layout.cshtml</span></span>
<span data-ttu-id="71c45-429">Soubor *_Layout.cshtml* nastaví název aplikace v záhlaví a zápatí a vytvoří položku nabídky „Reklamy“.</span><span class="sxs-lookup"><span data-stu-id="71c45-429">The *_Layout.cshtml* file sets the app name in the header and footer, and creates an "Ads" menu entry.</span></span>

### <a name="contosoadsweb---viewshomeindexcshtml"></a><span data-ttu-id="71c45-430">ContosoAdsWeb – Views\Home\Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="71c45-430">ContosoAdsWeb - Views\Home\Index.cshtml</span></span>
<span data-ttu-id="71c45-431">Soubor *Views\Home\Index.cshtml* zobrazuje na domovské stránce odkazy na kategorie.</span><span class="sxs-lookup"><span data-stu-id="71c45-431">The *Views\Home\Index.cshtml* file displays category links on the home page.</span></span> <span data-ttu-id="71c45-432">Odkazy předají celočíselnou hodnotu výčtu `Category` v proměnné řetězce dotazu na indexovou stránku reklam.</span><span class="sxs-lookup"><span data-stu-id="71c45-432">The links pass the integer value of the `Category` enum in a querystring variable to the Ads Index page.</span></span>

```razor
<li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
<li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
<li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
<li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>
```

### <a name="contosoadsweb---adcontrollercs"></a><span data-ttu-id="71c45-433">ContosoAdsWeb – AdController.cs</span><span class="sxs-lookup"><span data-stu-id="71c45-433">ContosoAdsWeb - AdController.cs</span></span>
<span data-ttu-id="71c45-434">V souboru *AdController.cs* volá konstruktor metodu `InitializeStorage`, aby vytvořil objekty knihovny klienta služby Azure Storage, které poskytují rozhraní API pro práci s objekty blob a frontami.</span><span class="sxs-lookup"><span data-stu-id="71c45-434">In the *AdController.cs* file, the constructor calls the `InitializeStorage` method to create Azure Storage Client Library objects that provide an API for working with blobs and queues.</span></span>

<span data-ttu-id="71c45-435">Potom kód získá odkaz na kontejner objektů blob s *obrázky*, jak už jste viděli v souboru *Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="71c45-435">Then the code gets a reference to the *images* blob container as you saw earlier in *Global.asax.cs*.</span></span> <span data-ttu-id="71c45-436">Během toho nastaví výchozí [zásady opakování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling), které jsou vhodné pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="71c45-436">While doing that it sets a default [retry policy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) appropriate for a web app.</span></span> <span data-ttu-id="71c45-437">Výchozí zásady opakování exponenciálního omezení rychlosti můžou způsobit, že webová aplikace přestane při opakovaných pokusech reagovat na dobu delší než jednu minutu. Důvodem může být přechodná chyba.</span><span class="sxs-lookup"><span data-stu-id="71c45-437">The default exponential backoff retry policy could hang the web app for longer than a minute on repeated retries for a transient fault.</span></span> <span data-ttu-id="71c45-438">Zde určené zásady opakování čekají po každém pokusu tři sekundy a celkem provádějí tři pokusy.</span><span class="sxs-lookup"><span data-stu-id="71c45-438">The retry policy specified here waits three seconds after each try for up to three tries.</span></span>

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesBlobContainer = blobClient.GetContainerReference("images");
```

<span data-ttu-id="71c45-439">Podobný kód získá odkaz na frontu *obrázků*.</span><span class="sxs-lookup"><span data-stu-id="71c45-439">Similar code gets a reference to the *images* queue.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesQueue = queueClient.GetQueueReference("images");
```

<span data-ttu-id="71c45-440">Většinu kódu kontroleru je typická pro práci s datovým modelem Entity Framework za použití třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="71c45-440">Most of the controller code is typical for working with an Entity Framework data model using a DbContext class.</span></span> <span data-ttu-id="71c45-441">Výjimkou je metoda HttpPost `Create`, která soubor odešle a uloží ho do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="71c45-441">An exception is the HttpPost `Create` method, which uploads a file and saves it in blob storage.</span></span> <span data-ttu-id="71c45-442">Vazač modelu poskytuje metodě objekt [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx).</span><span class="sxs-lookup"><span data-stu-id="71c45-442">The model binder provides an [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) object to the method.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<ActionResult> Create(
    [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
    HttpPostedFileBase imageFile)
```

<span data-ttu-id="71c45-443">Pokud uživatel vybral soubor k odeslání, kód soubor odešle, uloží ho do objektu blob a zaktualizuje záznam v databázi reklam pomocí adresy URL, která odkazuje na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="71c45-443">If the user selected a file to upload, the code uploads the file, saves it in a blob, and updates the Ad database record with a URL that points to the blob.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    blob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = blob.Uri.ToString();
}
```

<span data-ttu-id="71c45-444">Kód, který odesílání provádí, je umístěný v metodě `UploadAndSaveBlobAsync`.</span><span class="sxs-lookup"><span data-stu-id="71c45-444">The code that does the upload is in the `UploadAndSaveBlobAsync` method.</span></span> <span data-ttu-id="71c45-445">Vytvoří identifikátor GUID pro objekt blob, odešle a uloží soubor a vrátí odkaz na uložený objekt blob.</span><span class="sxs-lookup"><span data-stu-id="71c45-445">It creates a GUID name for the blob, uploads and saves the file, and returns a reference to the saved blob.</span></span>

```csharp
private async Task<CloudBlockBlob> UploadAndSaveBlobAsync(HttpPostedFileBase imageFile)
{
    string blobName = Guid.NewGuid().ToString() + Path.GetExtension(imageFile.FileName);
    CloudBlockBlob imageBlob = imagesBlobContainer.GetBlockBlobReference(blobName);
    using (var fileStream = imageFile.InputStream)
    {
        await imageBlob.UploadFromStreamAsync(fileStream);
    }
    return imageBlob;
}
```

<span data-ttu-id="71c45-446">Když metoda HttpPost `Create` odešle objekt blob a zaktualizuje databázi, vytvoří zprávu fronty, aby informovala back-endový proces, že obrázek je připravený k převodu na miniaturu.</span><span class="sxs-lookup"><span data-stu-id="71c45-446">After the HttpPost `Create` method uploads a blob and updates the database, it creates a queue message to inform that back-end process that an image is ready for conversion to a thumbnail.</span></span>

```csharp
string queueMessageString = ad.AdId.ToString();
var queueMessage = new CloudQueueMessage(queueMessageString);
await queue.AddMessageAsync(queueMessage);
```

<span data-ttu-id="71c45-447">Kód metody HttpPost `Edit` je podobný s tím rozdílem, že pokud uživatel vybere nový obrázkový soubor, veškeré existující objekty blob musí být odstraněny.</span><span class="sxs-lookup"><span data-stu-id="71c45-447">The code for the HttpPost `Edit` method is similar except that if the user selects a new image file any blobs that already exist must be deleted.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    await DeleteAdBlobsAsync(ad);
    imageBlob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = imageBlob.Uri.ToString();
}
```

<span data-ttu-id="71c45-448">Další příklad ukazuje kód, který odstraní objekty blob při odstranění reklamy.</span><span class="sxs-lookup"><span data-stu-id="71c45-448">The next example shows the code that deletes blobs when you delete an ad.</span></span>

```csharp
private async Task DeleteAdBlobsAsync(Ad ad)
{
    if (!string.IsNullOrWhiteSpace(ad.ImageURL))
    {
        Uri blobUri = new Uri(ad.ImageURL);
        await DeleteAdBlobAsync(blobUri);
    }
    if (!string.IsNullOrWhiteSpace(ad.ThumbnailURL))
    {
        Uri blobUri = new Uri(ad.ThumbnailURL);
        await DeleteAdBlobAsync(blobUri);
    }
}
private static async Task DeleteAdBlobAsync(Uri blobUri)
{
    string blobName = blobUri.Segments[blobUri.Segments.Length - 1];
    CloudBlockBlob blobToDelete = imagesBlobContainer.GetBlockBlobReference(blobName);
    await blobToDelete.DeleteAsync();
}
```

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a><span data-ttu-id="71c45-449">ContosoAdsWeb – Views\Ad\Index.cshtml a Details.cshtml</span><span class="sxs-lookup"><span data-stu-id="71c45-449">ContosoAdsWeb - Views\Ad\Index.cshtml and Details.cshtml</span></span>
<span data-ttu-id="71c45-450">Soubor *Index.cshtml* zobrazí miniatury s dalšími daty reklam.</span><span class="sxs-lookup"><span data-stu-id="71c45-450">The *Index.cshtml* file displays thumbnails with the other ad data.</span></span>

```razor
<img src="@Html.Raw(item.ThumbnailURL)" />
```

<span data-ttu-id="71c45-451">Soubor *Details.cshtml* zobrazí obrázek v plné velikosti.</span><span class="sxs-lookup"><span data-stu-id="71c45-451">The *Details.cshtml* file displays the full-size image.</span></span>

```razor
<img src="@Html.Raw(Model.ImageURL)" />
```

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a><span data-ttu-id="71c45-452">ContosoAdsWeb – Views\Ad\Create.cshtml a Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="71c45-452">ContosoAdsWeb - Views\Ad\Create.cshtml and Edit.cshtml</span></span>
<span data-ttu-id="71c45-453">Soubory *Create.cshtml* a *Edit.cshtml* určují kódování formuláře, které kontroleru umožňuje získání objektu `HttpPostedFileBase`.</span><span class="sxs-lookup"><span data-stu-id="71c45-453">The *Create.cshtml* and *Edit.cshtml* files specify form encoding that enables the controller to get the `HttpPostedFileBase` object.</span></span>

```razor
@using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))
```

<span data-ttu-id="71c45-454">Prvek `<input>` sděluje prohlížeči, aby zobrazil dialogové okno pro výběr souboru.</span><span class="sxs-lookup"><span data-stu-id="71c45-454">An `<input>` element tells the browser to provide a file selection dialog.</span></span>

```razor
<input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />
```

### <a name="contosoadsworker---workerrolecs---onstart-method"></a><span data-ttu-id="71c45-455">ContosoAdsWorker – WorkerRole.cs – metoda OnStart </span><span class="sxs-lookup"><span data-stu-id="71c45-455">ContosoAdsWorker - WorkerRole.cs - OnStart method</span></span>
<span data-ttu-id="71c45-456">Prostředí role pracovního procesu Azure volá metodu `OnStart` ve třídě `WorkerRole`, když se spouští role pracovního procesu, a volá metodu `Run`, když se metoda `OnStart` dokončí.</span><span class="sxs-lookup"><span data-stu-id="71c45-456">The Azure worker role environment calls the `OnStart` method in the `WorkerRole` class when the worker role is getting started, and it calls the `Run` method when the `OnStart` method finishes.</span></span>

<span data-ttu-id="71c45-457">Metoda `OnStart` získá připojovací řetězec databáze ze souboru *.cscfg* a předá ho do třídy DbContext v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="71c45-457">The `OnStart` method gets the database connection string from the *.cscfg* file and passes it to the Entity Framework DbContext class.</span></span> <span data-ttu-id="71c45-458">Poskytovatel SQLClienta se používá ve výchozím nastavení, takže ho není nutné zadávat.</span><span class="sxs-lookup"><span data-stu-id="71c45-458">The SQLClient provider is used by default, so the provider does not have to be specified.</span></span>

```csharp
var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
db = new ContosoAdsContext(dbConnString);
```

<span data-ttu-id="71c45-459">Potom metoda získá odkaz na účet úložiště a vytvoří kontejner objektů blob a frontu (pokud ještě neexistují).</span><span class="sxs-lookup"><span data-stu-id="71c45-459">After that, the method gets a reference to the storage account and creates the blob container and queue if they don't exist.</span></span> <span data-ttu-id="71c45-460">Kód pro tuto akci je podobný kódu, který jste už viděli v metodě webové role `Application_Start`.</span><span class="sxs-lookup"><span data-stu-id="71c45-460">The code for that is similar to what you already saw in the web role `Application_Start` method.</span></span>

### <a name="contosoadsworker---workerrolecs---run-method"></a><span data-ttu-id="71c45-461">ContosoAdsWorker – WorkerRole.cs – metoda Run</span><span class="sxs-lookup"><span data-stu-id="71c45-461">ContosoAdsWorker - WorkerRole.cs - Run method</span></span>
<span data-ttu-id="71c45-462">Metoda `Run` se volá, když metoda `OnStart` dokončí svoji inicializaci.</span><span class="sxs-lookup"><span data-stu-id="71c45-462">The `Run` method is called when the `OnStart` method finishes its initialization work.</span></span> <span data-ttu-id="71c45-463">Metoda spustí nekonečnou smyčku, která sleduje nové zprávy fronty a po jejich příchodu je zpracuje.</span><span class="sxs-lookup"><span data-stu-id="71c45-463">The method executes an infinite loop that watches for new queue messages and processes them when they arrive.</span></span>

```csharp
public override void Run()
{
    CloudQueueMessage msg = null;

    while (true)
    {
        try
        {
            msg = this.imagesQueue.GetMessage();
            if (msg != null)
            {
                ProcessQueueMessage(msg);
            }
            else
            {
                System.Threading.Thread.Sleep(1000);
            }
        }
        catch (StorageException e)
        {
            if (msg != null && msg.DequeueCount > 5)
            {
                this.imagesQueue.DeleteMessage(msg);
            }
            System.Threading.Thread.Sleep(5000);
        }
    }
}
```

<span data-ttu-id="71c45-464">Po každé iteraci smyčky, kdy nebyla nalezena žádná zpráva fronty, se program na sekundu uspí.</span><span class="sxs-lookup"><span data-stu-id="71c45-464">After each iteration of the loop, if no queue message was found, the program sleeps for a second.</span></span> <span data-ttu-id="71c45-465">Tím se roli pracovního procesu zabrání, aby nadměrně nezvyšovala náklady na čas procesoru a transakce úložiště.</span><span class="sxs-lookup"><span data-stu-id="71c45-465">This prevents the worker role from incurring excessive CPU time and storage transaction costs.</span></span> <span data-ttu-id="71c45-466">Poradní tým Microsoftu vypráví příběh o vývojáři, který tohle zapomněl zohlednit, nasadil kód do výroby a odjel na dovolenou.</span><span class="sxs-lookup"><span data-stu-id="71c45-466">The Microsoft Customer Advisory Team tells a story about a  developer who forgot to include this, deployed to production, and left for vacation.</span></span> <span data-ttu-id="71c45-467">Když se vrátil zpět, byl účet za dohled vyšší než cena jeho dovolené.</span><span class="sxs-lookup"><span data-stu-id="71c45-467">When he got back, his oversight cost more than the vacation.</span></span>

<span data-ttu-id="71c45-468">Obsah zprávy fronty občas způsobí chybu při zpracování.</span><span class="sxs-lookup"><span data-stu-id="71c45-468">Sometimes the content of a queue message causes an error in processing.</span></span> <span data-ttu-id="71c45-469">Takové zprávě se říká *nezpracovatelná zpráva* a pokud jste právě zaprotokolovali chybu a restartovali smyčku, můžete se pokoušet o zpracování této zprávy do nekonečna.</span><span class="sxs-lookup"><span data-stu-id="71c45-469">This is called a *poison message*, and if you just logged an error and restarted the loop, you could endlessly try to process that message.</span></span>  <span data-ttu-id="71c45-470">Zachycující blok proto zahrnuje podmínku, která kontroluje, jak často se aplikace pokusila aktuální zprávu zpracovat a pokud to bylo víc než pětkrát, odstraní zprávu z fronty.</span><span class="sxs-lookup"><span data-stu-id="71c45-470">Therefore the catch block includes an if statement that checks to see how many times the app has tried to process the current message, and if it has been more than 5 times, the message is deleted from the queue.</span></span>

<span data-ttu-id="71c45-471">`ProcessQueueMessage` se volá při nalezení zpráv fronty.</span><span class="sxs-lookup"><span data-stu-id="71c45-471">`ProcessQueueMessage` is called when a queue message is found.</span></span>

```csharp
private void ProcessQueueMessage(CloudQueueMessage msg)
{
    var adId = int.Parse(msg.AsString);
    Ad ad = db.Ads.Find(adId);
    if (ad == null)
    {
        throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", adId.ToString()));
    }

    CloudBlockBlob inputBlob = this.imagesBlobContainer.GetBlockBlobReference(ad.ImageURL);

    string thumbnailName = Path.GetFileNameWithoutExtension(inputBlob.Name) + "thumb.jpg";
    CloudBlockBlob outputBlob = this.imagesBlobContainer.GetBlockBlobReference(thumbnailName);

    using (Stream input = inputBlob.OpenRead())
    using (Stream output = outputBlob.OpenWrite())
    {
        ConvertImageToThumbnailJPG(input, output);
        outputBlob.Properties.ContentType = "image/jpeg";
    }

    ad.ThumbnailURL = outputBlob.Uri.ToString();
    db.SaveChanges();

    this.imagesQueue.DeleteMessage(msg);
}
```

<span data-ttu-id="71c45-472">Tento kód čte databázi, aby získal adresu URL obrázku, převede obrázek na miniaturu, uloží miniaturu do objektu blob, zaktualizuje databázi pomocí adresy URL odkazující na miniaturu v objektu blob a odstraní zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="71c45-472">This code reads the database to get the image URL, converts the image to a thumbnail, saves the thumbnail in a blob, updates the database with the thumbnail blob URL, and deletes the queue message.</span></span>

> [!NOTE]
> <span data-ttu-id="71c45-473">Kód v metodě `ConvertImageToThumbnailJPG` používá pro zjednodušení třídy v oboru názvů System.Drawing.</span><span class="sxs-lookup"><span data-stu-id="71c45-473">The code in the `ConvertImageToThumbnailJPG` method uses classes in the System.Drawing namespace for simplicity.</span></span> <span data-ttu-id="71c45-474">Třídy v tomto oboru názvů však byly navrženy pro používání s formuláři Windows.</span><span class="sxs-lookup"><span data-stu-id="71c45-474">However, the classes in this namespace were designed for use with Windows Forms.</span></span> <span data-ttu-id="71c45-475">Jejich používání není podporované ve Windows a službě ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71c45-475">They are not supported for use in a Windows or ASP.NET service.</span></span> <span data-ttu-id="71c45-476">Další informace o možnostech zpracování obrázků najdete v článcích o [dynamickém generování obrázků](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) a o [hloubkové změně velikosti uvnitř obrázků](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span><span class="sxs-lookup"><span data-stu-id="71c45-476">For more information about image-processing options, see [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) and [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span></span>
>
>

## <a name="troubleshooting"></a><span data-ttu-id="71c45-477">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="71c45-477">Troubleshooting</span></span>
<span data-ttu-id="71c45-478">Pokud by vám při procházení kurzem něco nefungovalo, následuje přehled běžných chyb a jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="71c45-478">In case something doesn't work while you're following the instructions in this tutorial, here are some common errors and how to resolve them.</span></span>

### <a name="serviceruntimeroleenvironmentexception"></a><span data-ttu-id="71c45-479">ServiceRuntime.RoleEnvironmentException</span><span class="sxs-lookup"><span data-stu-id="71c45-479">ServiceRuntime.RoleEnvironmentException</span></span>
<span data-ttu-id="71c45-480">Azure poskytne objekt `RoleEnvironment` při spuštění aplikace v Azure nebo při spuštění místně pomocí emulátoru služby Výpočty v Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-480">The `RoleEnvironment` object is provided by Azure when you run an application in Azure or when you run locally using the Azure compute emulator.</span></span>  <span data-ttu-id="71c45-481">Pokud se tato chyba objeví, když aplikaci spouštíte místně, zkontrolujte, jestli jste projekt ContosoAdsCloudService nastavili jako spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="71c45-481">If you get this error when you're running locally, make sure that you have set the ContosoAdsCloudService project as the startup project.</span></span> <span data-ttu-id="71c45-482">Toto nastaví projekt tak, aby běžel pomocí emulátoru služby Výpočty v Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-482">This sets up the project to run using the Azure compute emulator.</span></span>

<span data-ttu-id="71c45-483">Jedna z věcí, ke kterým aplikace používá RoleEnvironment Azure, je získání hodnot připojovacích řetězců, které jsou uložené v souborech *.cscfg*, takže další možnou příčinou této výjimky je chybějící připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="71c45-483">One of the things the application uses the Azure RoleEnvironment for is to get the connection string values that are stored in the *.cscfg* files, so another cause of this exception is a missing connection string.</span></span> <span data-ttu-id="71c45-484">Zkontrolujte, jestli jste v projektu ContosoAdsWeb vytvořili nastavení StorageConnectionString pro cloudovou i místní konfiguraci a jestli jste vytvořili oba připojovací řetězce pro obě konfigurace i v projektu ContosoAdsWorker.</span><span class="sxs-lookup"><span data-stu-id="71c45-484">Make sure that you created the StorageConnectionString setting for both Cloud and Local configurations in the ContosoAdsWeb project, and that you created both connection strings for both configurations in the ContosoAdsWorker project.</span></span> <span data-ttu-id="71c45-485">Pokud budete StorageConnectionString hledat pomocí možnosti **Najít všechny** v celém řešení, mělo by se zobrazit devětkrát v šesti souborech.</span><span class="sxs-lookup"><span data-stu-id="71c45-485">If you do a **Find All** search for StorageConnectionString in the entire solution, you should see it 9 times in 6 files.</span></span>

### <a name="cannot-override-to-port-xxx-new-port-below-minimum-allowed-value-8080-for-protocol-http"></a><span data-ttu-id="71c45-486">Nejde přepsat na port xxx.</span><span class="sxs-lookup"><span data-stu-id="71c45-486">Cannot override to port xxx.</span></span> <span data-ttu-id="71c45-487">Nový port s nižší než minimální povolenou hodnotou 8080 pro protokol http</span><span class="sxs-lookup"><span data-stu-id="71c45-487">New port below minimum allowed value 8080 for protocol http</span></span>
<span data-ttu-id="71c45-488">Změňte číslo portu, který používáte pro webový projekt.</span><span class="sxs-lookup"><span data-stu-id="71c45-488">Try changing the port number used by the web project.</span></span> <span data-ttu-id="71c45-489">Klikněte pravým tlačítkem na projekt ContosoAdsWeb a potom klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="71c45-489">Right-click the ContosoAdsWeb project, and then click **Properties**.</span></span> <span data-ttu-id="71c45-490">Klikněte na kartu **Web** a potom v nastavení **Adresa URL projektu** změňte číslo portu.</span><span class="sxs-lookup"><span data-stu-id="71c45-490">Click the **Web** tab, and then change the port number in the **Project Url** setting.</span></span>

<span data-ttu-id="71c45-491">Další alternativní řešení problému najdete v následující části.</span><span class="sxs-lookup"><span data-stu-id="71c45-491">For another alternative that might resolve the problem, see the following  section.</span></span>

### <a name="other-errors-when-running-locally"></a><span data-ttu-id="71c45-492">Další chyby při místním spuštění</span><span class="sxs-lookup"><span data-stu-id="71c45-492">Other errors when running locally</span></span>
<span data-ttu-id="71c45-493">Nové projekty cloudových služeb ve výchozím nastavení používají expresní emulátor služby Výpočty v Azure k simulaci prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="71c45-493">By default new cloud service projects use the Azure compute emulator express to simulate the Azure environment.</span></span> <span data-ttu-id="71c45-494">Jedná se o odlehčenou verzi úplného emulátoru služby Výpočty a za určitých podmínek bude úplný emulátor fungovat, když expresní verze nepracuje.</span><span class="sxs-lookup"><span data-stu-id="71c45-494">This is a lightweight version of the full compute emulator, and under some conditions the full emulator will work when the express version does not.</span></span>  

<span data-ttu-id="71c45-495">Pokud chcete změnit projekt, který používá úplný emulátor, klikněte pravým tlačítkem na projekt ContosoAdsCloudService a potom na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="71c45-495">To change the project to use the full emulator, right-click the ContosoAdsCloudService project, and then click **Properties**.</span></span> <span data-ttu-id="71c45-496">V okně **Vlastnosti** klikněte na kartu **Web** a potom na přepínač **Použít úplný emulátor**.</span><span class="sxs-lookup"><span data-stu-id="71c45-496">In the **Properties** window click the **Web** tab, and then click the **Use Full Emulator** radio button.</span></span>

<span data-ttu-id="71c45-497">Pokud chcete aplikaci spustit s úplným emulátorem, otevřete Visual Studio s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="71c45-497">In order to run the application with the full emulator, you have to open Visual Studio with administrator privileges.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71c45-498">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71c45-498">Next steps</span></span>
<span data-ttu-id="71c45-499">Aplikace Contoso Ads je kvůli úvodnímu kurzu záměrně jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="71c45-499">The Contoso Ads application has intentionally been kept simple for a getting-started tutorial.</span></span> <span data-ttu-id="71c45-500">Například neimplementuje [vkládání závislostí](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) nebo [úložiště a jednotky pracovních vzorů](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), nepodporuje [používání rozhraní k protokolování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), nepoužívá [migrace Code First EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) ke správě změn datových modelů nebo [odolnost připojení EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) ke správě přechodných síťových chyb a tak dále.</span><span class="sxs-lookup"><span data-stu-id="71c45-500">For example, it doesn't implement [dependency injection](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) or the [repository and unit of work patterns](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), it doesn't [use an interface for logging](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), it doesn't use [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) to manage data model changes or [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) to manage transient network errors, and so forth.</span></span>

<span data-ttu-id="71c45-501">Níže uvádíme několik ukázkových aplikací cloudových služeb, které předvádějí realističtější postupy kódování (jsou řazené od méně složitých po složitější):</span><span class="sxs-lookup"><span data-stu-id="71c45-501">Here are some cloud service sample applications that demonstrate more real-world coding practices, listed from less complex to more complex:</span></span>

* <span data-ttu-id="71c45-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span><span class="sxs-lookup"><span data-stu-id="71c45-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span></span> <span data-ttu-id="71c45-503">Podobný koncept jako Contoso Ads, ale implementuje víc funkcí a realističtější postupy kódování.</span><span class="sxs-lookup"><span data-stu-id="71c45-503">Similar in concept to Contoso Ads but implements more features and more real-world coding practices.</span></span>
* <span data-ttu-id="71c45-504">[Vícevrstvé aplikace cloudových služeb Azure s tabulkami, frontami a objekty blob](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36).</span><span class="sxs-lookup"><span data-stu-id="71c45-504">[Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36).</span></span> <span data-ttu-id="71c45-505">Představuje tabulky služby Azure Storage a také objekty blob a fronty.</span><span class="sxs-lookup"><span data-stu-id="71c45-505">Introduces Azure Storage tables as well as blobs and queues.</span></span> <span data-ttu-id="71c45-506">Vychází ze starší verzi sady Azure SDK pro .NET. Bude vyžadovat určité změny pro práci s aktuální verzí.</span><span class="sxs-lookup"><span data-stu-id="71c45-506">Based on an older version of the Azure SDK for .NET, will require some modifications to work with the current version.</span></span>
* <span data-ttu-id="71c45-507">[Základy cloudových služeb v Microsoft Azure Cloud](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span><span class="sxs-lookup"><span data-stu-id="71c45-507">[Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span></span> <span data-ttu-id="71c45-508">Komplexní ukázka předvádějící širokou škálu osvědčených postupů, kterou vytvořila skupina Microsoft Patterns and Practices.</span><span class="sxs-lookup"><span data-stu-id="71c45-508">A comprehensive sample demonstrating a wide range of best practices, produced by the Microsoft Patterns and Practices group.</span></span>

<span data-ttu-id="71c45-509">Obecné informace o vývoji pro cloud najdete v článku o [vytváření reálných cloudových aplikací s Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span><span class="sxs-lookup"><span data-stu-id="71c45-509">For general information about developing for the cloud, see [Building Real-World Cloud Apps with Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span></span>

<span data-ttu-id="71c45-510">Video úvod do osvědčených postupů a vzorů služby Azure Storage najdete v článku [Služba Microsoft Azure Storage – novinky, osvědčené postupy a vzory](http://channel9.msdn.com/Events/Build/2014/3-628).</span><span class="sxs-lookup"><span data-stu-id="71c45-510">For a video introduction to Azure Storage best practices and patterns, see [Microsoft Azure Storage – What's New, Best Practices and Patterns](http://channel9.msdn.com/Events/Build/2014/3-628).</span></span>

<span data-ttu-id="71c45-511">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="71c45-511">For more information, see the following resources:</span></span>

* [<span data-ttu-id="71c45-512">Cloudové služby Azure Cloud Services část 1: Úvod</span><span class="sxs-lookup"><span data-stu-id="71c45-512">Azure Cloud Services Part 1: Introduction</span></span>](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [<span data-ttu-id="71c45-513">Jak spravovat Cloud Services</span><span class="sxs-lookup"><span data-stu-id="71c45-513">How to manage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="71c45-514">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="71c45-514">Azure Storage</span></span>](/documentation/services/storage/)
* [<span data-ttu-id="71c45-515">Jak vybrat poskytovatele cloudových služeb</span><span class="sxs-lookup"><span data-stu-id="71c45-515">How to choose a cloud service provider</span></span>](https://azure.microsoft.com/overview/choosing-a-cloud-service-provider/)
