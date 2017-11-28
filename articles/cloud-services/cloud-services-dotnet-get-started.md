---
title: "aaaGet začít s Azure Cloud Services a technologií ASP.NET | Microsoft Docs"
description: "Zjistěte, jak toocreate vícevrstvé aplikace s použitím rozhraní ASP.NET MVC a Azure. Hello aplikace běží v cloudové službě a obsahuje webovou roli a roli pracovního procesu. Používá nástroj Entity Framework, službu Azure SQL Database a fronty a objekty blob služby Azure Storage."
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
ms.openlocfilehash: 86271c29b79fad3f01f8ea0e88fd00c7aefc970c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cloud-services-and-aspnet"></a><span data-ttu-id="6fc7c-105">Začínáme s cloudovými službami Azure Cloud Services a technologií ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6fc7c-105">Get started with Azure Cloud Services and ASP.NET</span></span>

## <a name="overview"></a><span data-ttu-id="6fc7c-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="6fc7c-106">Overview</span></span>
<span data-ttu-id="6fc7c-107">Tento kurz ukazuje, jak toocreate vícevrstvé aplikace .NET s front-endu, ASP.NET MVC a nasaďte ho tooan [cloudové služby Azure](cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-107">This tutorial shows how toocreate a multi-tier .NET application with an ASP.NET MVC front-end, and deploy it tooan [Azure cloud service](cloud-services-choose-me.md).</span></span> <span data-ttu-id="6fc7c-108">Hello používá aplikace [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), hello [služby objektů Blob Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage)a hello [službu front Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-108">hello application uses [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), hello [Azure Blob service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and hello [Azure Queue service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span></span> <span data-ttu-id="6fc7c-109">Můžete [stažení projektu sady Visual Studio hello](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) z hello galerie kódů MSDN.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-109">You can [download hello Visual Studio project](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) from hello MSDN Code Gallery.</span></span>

<span data-ttu-id="6fc7c-110">Hello kurz ukazuje, jak toobuild a spusťte hello aplikaci místně, jak toodeploy ho tooAzure a hello spuštění v cloudu a jak toobuild ho od začátku.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-110">hello tutorial shows you how toobuild and run hello application locally, how toodeploy it tooAzure and run in hello cloud, and how toobuild it from scratch.</span></span> <span data-ttu-id="6fc7c-111">Můžete začít tím, sestavíte od nuly a pak hello test a kroky nasazení Pokud dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-111">You can start by building from scratch and then do hello test and deploy steps afterward if you prefer.</span></span>

## <a name="contoso-ads-application"></a><span data-ttu-id="6fc7c-112">Aplikace Contoso Ads</span><span class="sxs-lookup"><span data-stu-id="6fc7c-112">Contoso Ads application</span></span>
<span data-ttu-id="6fc7c-113">aplikace Hello je vývěsní Tabule pro inzerci.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-113">hello application is an advertising bulletin board.</span></span> <span data-ttu-id="6fc7c-114">Uživatelé vytvářejí reklamu tak, že zadají text a odešlou obrázek.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-114">Users create an ad by entering text and uploading an image.</span></span> <span data-ttu-id="6fc7c-115">Vidí seznam reklam s obrázky miniatur a uvidí obrázek hello plné velikosti, když vyberou podrobnosti hello toosee služby ad.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-115">They can see a list of ads with thumbnail images, and they can see hello full-size image when they select an ad toosee hello details.</span></span>

![Seznam reklam](./media/cloud-services-dotnet-get-started/list.png)

<span data-ttu-id="6fc7c-117">aplikace Hello používá hello [fronty způsob práce zaměřený](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff zatížení hello náročná na prostředky procesoru práci při vytváření miniatur tooa back endovému procesu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-117">hello application uses hello [queue-centric work pattern](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-load hello CPU-intensive work of creating thumbnails tooa back-end process.</span></span>

## <a name="alternative-architecture-websites-and-webjobs"></a><span data-ttu-id="6fc7c-118">Alternativní architektura: weby a webové úlohy</span><span class="sxs-lookup"><span data-stu-id="6fc7c-118">Alternative architecture: Websites and WebJobs</span></span>
<span data-ttu-id="6fc7c-119">Tento kurz ukazuje, jak toorun front-end i back-end v Azure cloud service.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-119">This tutorial shows how toorun both front-end and back-end in an Azure cloud service.</span></span> <span data-ttu-id="6fc7c-120">Alternativou je toorun hello front-endu v [webu Azure](/services/web-sites/) a používat hello [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) funkce (momentálně ve verzi preview) pro hello back-end.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-120">An alternative is toorun hello front-end in an [Azure website](/services/web-sites/) and use hello [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) feature (currently in preview) for hello back-end.</span></span> <span data-ttu-id="6fc7c-121">Kurz, který používá webové úlohy, najdete v části [začít pracovat s hello Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-121">For a tutorial that uses WebJobs, see [Get Started with hello Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span></span> <span data-ttu-id="6fc7c-122">Informace o tom, jak toochoose hello služby, které nejlépe vyhovovat vašemu scénáři, najdete v části [porovnání webů Azure, cloudové služby a virtuální počítače](../app-service-web/choose-web-site-cloud-service-vm.md).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-122">For information about how toochoose hello services that best fit your scenario, see [Azure Websites, Cloud Services, and virtual machines comparison](../app-service-web/choose-web-site-cloud-service-vm.md).</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="6fc7c-123">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="6fc7c-123">What you'll learn</span></span>
* <span data-ttu-id="6fc7c-124">Jak tooenable počítači pro vývoj pro Azure nainstalováním hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-124">How tooenable your machine for Azure development by installing hello Azure SDK.</span></span>
* <span data-ttu-id="6fc7c-125">Jak toocreate sady Visual Studio cloudové služby projektu s webová role ASP.NET MVC a roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-125">How toocreate a Visual Studio cloud service project with an ASP.NET MVC web role and a worker role.</span></span>
* <span data-ttu-id="6fc7c-126">Jak tootest hello projekt cloudové služby místně, pomocí emulátoru úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-126">How tootest hello cloud service project locally, using hello Azure storage emulator.</span></span>
* <span data-ttu-id="6fc7c-127">Jak toopublish hello cloudu projektu tooan Azure Cloudová služba a testování pomocí účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-127">How toopublish hello cloud project tooan Azure cloud service and test using an Azure storage account.</span></span>
* <span data-ttu-id="6fc7c-128">Jak tooupload souborů a uložit je do služby objektů Blob Azure hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-128">How tooupload files and store them in hello Azure Blob service.</span></span>
* <span data-ttu-id="6fc7c-129">Jak toouse hello služby front Azure pro komunikaci mezi vrstvami.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-129">How toouse hello Azure Queue service for communication between tiers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6fc7c-130">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6fc7c-130">Prerequisites</span></span>
<span data-ttu-id="6fc7c-131">Hello kurz předpokládá, že rozumíte [základní koncepty Azure cloud services](cloud-services-choose-me.md) například *webovou roli* a *role pracovního procesu* terminologie.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-131">hello tutorial assumes that you understand [basic concepts about Azure cloud services](cloud-services-choose-me.md) such as *web role* and *worker role* terminology.</span></span>  <span data-ttu-id="6fc7c-132">Také předpokládá, že víte, jak toowork s [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) nebo [webových formulářů](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) projekty v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-132">It also assumes that you know how toowork with [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) or [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) projects in Visual Studio.</span></span> <span data-ttu-id="6fc7c-133">Hello ukázková aplikace používá MVC, ale většina kurzu hello platí také tooWeb formulářů.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-133">hello sample application uses MVC, but most of hello tutorial also applies tooWeb Forms.</span></span>

<span data-ttu-id="6fc7c-134">Můžete spustit aplikace hello místně bez předplatného Azure, ale budete potřebovat jeden toodeploy hello aplikace toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-134">You can run hello app locally without an Azure subscription, but you'll need one toodeploy hello application toohello cloud.</span></span> <span data-ttu-id="6fc7c-135">Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) nebo [si zaregistrovat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-135">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span></span>

<span data-ttu-id="6fc7c-136">Hello pokyny kurzu pracují s jedním z hello následující produkty:</span><span class="sxs-lookup"><span data-stu-id="6fc7c-136">hello tutorial instructions work with either of hello following products:</span></span>

* <span data-ttu-id="6fc7c-137">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6fc7c-137">Visual Studio 2013</span></span>
* <span data-ttu-id="6fc7c-138">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="6fc7c-138">Visual Studio 2015</span></span>
* <span data-ttu-id="6fc7c-139">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="6fc7c-139">Visual Studio 2017</span></span>

<span data-ttu-id="6fc7c-140">Pokud nemáte jednu z těchto, Visual Studio může být nainstalovaná automaticky při instalaci hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-140">If you don't have one of these, Visual Studio may be installed automatically when you install hello Azure SDK.</span></span>

## <a name="application-architecture"></a><span data-ttu-id="6fc7c-141">Architektura aplikace</span><span class="sxs-lookup"><span data-stu-id="6fc7c-141">Application architecture</span></span>
<span data-ttu-id="6fc7c-142">Hello aplikace ukládá reklamy do databáze SQL pomocí Entity Framework Code First toocreate hello tabulky a přístup k datům hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-142">hello app stores ads in a SQL database, using Entity Framework Code First toocreate hello tables and access hello data.</span></span> <span data-ttu-id="6fc7c-143">Pro každou reklamu hello hello databáze ukládá dvě adresy URL. jednu pro obrázek v plné velikosti a druhou pro miniaturu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-143">For each ad, hello database stores two URLs, one for hello full-size image and one for hello thumbnail.</span></span>

![Tabulka reklam](./media/cloud-services-dotnet-get-started/adtable.png)

<span data-ttu-id="6fc7c-145">Když uživatel odešle obrázek, front-end spuštěný hello ve webové roli ukládá hello bitové kopie v [objektů blob v Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), a ukládá hello ad informace v databázi hello s adresou URL, který odkazuje objekt blob toohello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-145">When a user uploads an image, hello front-end running in a web role stores hello image in an [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and it stores hello ad information in hello database with a URL that points toohello blob.</span></span> <span data-ttu-id="6fc7c-146">Na hello stejný čas, zapíše zpráva tooan fronty Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-146">At hello same time, it writes a message tooan Azure queue.</span></span> <span data-ttu-id="6fc7c-147">Back-end proces spuštěný v roli pracovního procesu pravidelně dotazuje hello fronty pro nové zprávy.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-147">A back-end process running in a worker role periodically polls hello queue for new messages.</span></span> <span data-ttu-id="6fc7c-148">Když se objeví nová zpráva, hello role pracovního procesu vytvoří miniaturu obrázku a aktualizace hello miniatur databáze pole adresy URL pro tuto reklamu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-148">When a new message appears, hello worker role creates a thumbnail for that image and updates hello thumbnail URL database field for that ad.</span></span> <span data-ttu-id="6fc7c-149">Hello následující diagram znázorňuje interakci částí hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-149">hello following diagram shows how hello parts of hello application interact.</span></span>

![Architektura Contoso Ads](./media/cloud-services-dotnet-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]

## <a name="download-and-run-hello-completed-solution"></a><span data-ttu-id="6fc7c-151">Stažení a spuštění řešení hello byla dokončena</span><span class="sxs-lookup"><span data-stu-id="6fc7c-151">Download and run hello completed solution</span></span>
1. <span data-ttu-id="6fc7c-152">Stáhněte a rozbalte hello [dokončené řešení](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-152">Download and unzip hello [completed solution](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span></span>
2. <span data-ttu-id="6fc7c-153">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-153">Start Visual Studio.</span></span>
3. <span data-ttu-id="6fc7c-154">Z hello **soubor** nabídce zvolte **otevřít projekt**, přejděte toowhere jste si stáhli hello řešení a pak otevřete soubor řešení hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-154">From hello **File** menu choose **Open Project**, navigate toowhere you downloaded hello solution, and then open hello solution file.</span></span>
4. <span data-ttu-id="6fc7c-155">Stisknutím kombinace kláves CTRL + SHIFT + B toobuild hello řešení.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-155">Press CTRL+SHIFT+B toobuild hello solution.</span></span>

    <span data-ttu-id="6fc7c-156">Visual Studio ve výchozím nastavení automaticky obnoví obsah balíčku NuGet hello, který nebyl součástí hello *.zip* souboru.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-156">By default, Visual Studio automatically restores hello NuGet package content, which was not included in hello *.zip* file.</span></span> <span data-ttu-id="6fc7c-157">Pokud hello balíčky neobnoví, nainstalujte je ručně budete toohello **spravovat balíčky NuGet pro řešení** dialogové okno a kliknutím na tlačítko hello **obnovení** tlačítko v pravé horní hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-157">If hello packages don't restore, install them manually by going toohello **Manage NuGet Packages for Solution** dialog box and clicking hello **Restore** button at hello top right.</span></span>
5. <span data-ttu-id="6fc7c-158">V **Průzkumníku řešení**, ujistěte se, že **ContosoAdsCloudService** je vybrán jako spouštěný projekt hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-158">In **Solution Explorer**, make sure that **ContosoAdsCloudService** is selected as hello startup project.</span></span>
6. <span data-ttu-id="6fc7c-159">Pokud používáte Visual Studio 2015 nebo vyšší, změňte připojovací řetězec SQL serveru hello v aplikaci hello *Web.config* souboru hello projektu ContosoAdsWeb a v hello *ServiceConfiguration.Local.cscfg* souboru projektu ContosoAdsCloudService hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-159">If you're using Visual Studio 2015 or higher, change hello SQL Server connection string in hello application *Web.config* file of hello ContosoAdsWeb project and in hello *ServiceConfiguration.Local.cscfg* file of hello ContosoAdsCloudService project.</span></span> <span data-ttu-id="6fc7c-160">V každém případě změňte "(localdb) \v11.0" příliš "\MSSQLLocalDB (localdb)".</span><span class="sxs-lookup"><span data-stu-id="6fc7c-160">In each case, change "(localdb)\v11.0" too"(localdb)\MSSQLLocalDB".</span></span>
7. <span data-ttu-id="6fc7c-161">Stisknutím kombinace kláves CTRL + F5 toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-161">Press CTRL+F5 toorun hello application.</span></span>

    <span data-ttu-id="6fc7c-162">Když spouštíte projekt cloudové služby místně, Visual Studio automaticky vyvolá hello Azure *emulátor služby výpočty* a Azure *emulátor úložiště*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-162">When you run a cloud service project locally, Visual Studio automatically invokes hello Azure *compute emulator* and Azure *storage emulator*.</span></span> <span data-ttu-id="6fc7c-163">Hello výpočetní emulátor používá počítače prostředky toosimulate hello webovou roli a prostředí role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-163">hello compute emulator uses your computer's resources toosimulate hello web role and worker role environments.</span></span> <span data-ttu-id="6fc7c-164">emulátor úložiště Hello používá [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databáze toosimulate cloudového úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-164">hello storage emulator uses a [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) database toosimulate Azure cloud storage.</span></span>

    <span data-ttu-id="6fc7c-165">Hello prvním spuštění projektu cloudové služby trvá minutu pro hello emulátorů toostart nahoru.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-165">hello first time you run a cloud service project, it takes a minute or so for hello emulators toostart up.</span></span> <span data-ttu-id="6fc7c-166">Pokud je spuštění emulátorů dokončené, hello výchozí prohlížeč otevře domovskou stránku toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-166">When emulator startup is finished, hello default browser opens toohello application home page.</span></span>

    ![Architektura Contoso Ads](./media/cloud-services-dotnet-get-started/home.png)
8. <span data-ttu-id="6fc7c-168">Klikněte na **Vytvořit reklamu**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-168">Click  **Create an Ad**.</span></span>
9. <span data-ttu-id="6fc7c-169">Zadejte nějaká testovací data a vyberte *.jpg* tooupload bitovou kopii a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-169">Enter some test data and select a *.jpg* image tooupload, and then click **Create**.</span></span>

    ![Vytvoření stránky](./media/cloud-services-dotnet-get-started/create.png)

    <span data-ttu-id="6fc7c-171">aplikace Hello přejde toohello indexovou stránku, ale nezobrazí miniaturu nové reklamy hello, protože toto zpracování ještě neproběhlo.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-171">hello app goes toohello Index page, but it doesn't show a thumbnail for hello new ad because that processing hasn't happened yet.</span></span>
10. <span data-ttu-id="6fc7c-172">Chvíli počkejte a pak aktualizujte hello Index stránky toosee hello miniaturu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-172">Wait a moment and then refresh hello Index page toosee hello thumbnail.</span></span>

     ![Indexová stránka](./media/cloud-services-dotnet-get-started/list.png)
11. <span data-ttu-id="6fc7c-174">Klikněte na tlačítko **podrobnosti** pro ad toosee hello plné velikosti bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-174">Click **Details** for your ad toosee hello full-size image.</span></span>

     ![Stránka podrobností](./media/cloud-services-dotnet-get-started/details.png)

<span data-ttu-id="6fc7c-176">Jste byla spuštěna aplikace hello zcela v místním počítači, bez připojení toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-176">You've been running hello application entirely on your local computer, with no connection toohello cloud.</span></span> <span data-ttu-id="6fc7c-177">Hello emulátor úložiště ukládá hello fronty a data objektů blob v databázi SQL serveru Express LocalDB a aplikace hello ukládá hello ad data do jiné databáze LocalDB.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-177">hello storage emulator stores hello queue and blob data in a SQL Server Express LocalDB database, and hello application stores hello ad data in another LocalDB database.</span></span> <span data-ttu-id="6fc7c-178">Databáze Entity Framework Code First automaticky vytvořený hello ad hello poprvé hello webové aplikace se pokusila tooaccess ho.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-178">Entity Framework Code First automatically created hello ad database hello first time hello web app tried tooaccess it.</span></span>

<span data-ttu-id="6fc7c-179">V následující části hello nakonfigurujete hello řešení toouse cloudové prostředky Azure pro fronty, objekty BLOB a databáze aplikace hello při spuštění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-179">In hello following section you'll configure hello solution toouse Azure cloud resources for queues, blobs, and hello application database when it runs in hello cloud.</span></span> <span data-ttu-id="6fc7c-180">Pokud jste chtěli toocontinue toorun místně, ale využívat cloudové prostředky úložiště a databáze, můžete to udělat.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-180">If you wanted toocontinue toorun locally but use cloud storage and database resources, you could do that.</span></span> <span data-ttu-id="6fc7c-181">Ho stačí nastavit připojovací řetězec, které se zobrazí jak toodo.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-181">It's just a matter of setting connection strings, which you'll see how toodo.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="6fc7c-182">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="6fc7c-182">Deploy hello application tooAzure</span></span>
<span data-ttu-id="6fc7c-183">Můžete to udělat následující kroky toorun hello aplikaci v cloudu hello hello:</span><span class="sxs-lookup"><span data-stu-id="6fc7c-183">You'll do hello following steps toorun hello application in hello cloud:</span></span>

* <span data-ttu-id="6fc7c-184">Vytvoření cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="6fc7c-184">Create an Azure cloud service.</span></span>
* <span data-ttu-id="6fc7c-185">Vytvoření databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="6fc7c-185">Create an Azure SQL database.</span></span>
* <span data-ttu-id="6fc7c-186">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="6fc7c-186">Create an Azure storage account.</span></span>
* <span data-ttu-id="6fc7c-187">Nakonfigurujte řešení toouse hello Azure SQL database při spuštění v Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-187">Configure hello solution toouse your Azure SQL database when it runs in Azure.</span></span>
* <span data-ttu-id="6fc7c-188">Nakonfigurujte řešení toouse hello účtu úložiště Azure při spuštění v Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-188">Configure hello solution toouse your Azure storage account when it runs in Azure.</span></span>
* <span data-ttu-id="6fc7c-189">Nasaďte hello projektu tooyour cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-189">Deploy hello project tooyour Azure cloud service.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="6fc7c-190">Vytvoření cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="6fc7c-190">Create an Azure cloud service</span></span>
<span data-ttu-id="6fc7c-191">Cloudové služby Azure je, že bude hello prostředí hello aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-191">An Azure cloud service is hello environment hello application will run in.</span></span>

1. <span data-ttu-id="6fc7c-192">V prohlížeči otevřete hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-192">In your browser, open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6fc7c-193">Klikněte na **Nový > Výpočty > Cloudová služba**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-193">Click **New > Compute > Cloud Service**.</span></span>

3. <span data-ttu-id="6fc7c-194">Hello DNS název vstupní pole zadejte předponu adresy URL pro hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-194">In hello DNS name input box, enter a URL prefix for hello cloud service.</span></span>

    <span data-ttu-id="6fc7c-195">Tato adresa URL má toobe jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-195">This URL has toobe unique.</span></span>  <span data-ttu-id="6fc7c-196">Budete zobrazí chybové hlášení, pokud zvolenou předponu hello je již používán.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-196">You'll get an error message if hello prefix you choose is already in use.</span></span>
4. <span data-ttu-id="6fc7c-197">Zadejte novou skupinu prostředků pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-197">Specify a new Resource group for hello  service.</span></span> <span data-ttu-id="6fc7c-198">Klikněte na tlačítko **vytvořit nový** a potom zadejte název hello prostředků skupiny vstupní pole, jako je například CS_contososadsRG.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-198">Click **Create new** and then type a name in hello Resource group input box, such as CS_contososadsRG.</span></span>

5. <span data-ttu-id="6fc7c-199">Zvolte hello oblasti, kde chcete toodeploy hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-199">Choose hello region where you want toodeploy hello application.</span></span>

    <span data-ttu-id="6fc7c-200">Toto pole určuje datové centrum, které bude hostovat vaše cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-200">This field specifies which datacenter your cloud service will be hosted in.</span></span> <span data-ttu-id="6fc7c-201">Pro produkční aplikace vyberte hello oblast nejbližší tooyour zákazníků.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-201">For a production application, you'd choose hello region closest tooyour customers.</span></span> <span data-ttu-id="6fc7c-202">V tomto kurzu zvolte nejbližší tooyou hello oblast.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-202">For this tutorial, choose hello region closest tooyou.</span></span>
5. <span data-ttu-id="6fc7c-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-203">Click **Create**.</span></span>

    <span data-ttu-id="6fc7c-204">V hello následující bitové kopie Cloudová služba se vytvoří s hello CSvccontosoads.cloudapp.net adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-204">In hello following image, a cloud service is created with hello URL CSvccontosoads.cloudapp.net.</span></span>

    ![Nová cloudová služba](./media/cloud-services-dotnet-get-started/newcs.png)

### <a name="create-an-azure-sql-database"></a><span data-ttu-id="6fc7c-206">Vytvoření databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="6fc7c-206">Create an Azure SQL database</span></span>
<span data-ttu-id="6fc7c-207">Když hello aplikace běží v cloudu hello, použije databáze založené na cloudu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-207">When hello app runs in hello cloud, it will use a cloud-based database.</span></span>

1. <span data-ttu-id="6fc7c-208">V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový > databáze > databáze SQL**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-208">In hello [Azure portal](https://portal.azure.com), click **New > Databases > SQL Database**.</span></span>
2. <span data-ttu-id="6fc7c-209">V hello **název databáze** zadejte *contosoads*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-209">In hello **Database Name** box, enter *contosoads*.</span></span>
3. <span data-ttu-id="6fc7c-210">V hello **skupiny prostředků**, klikněte na tlačítko **použít existující** a skupině prostředků vyberte hello používá pro hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-210">In hello **Resource group**, click **Use existing** and select hello resource group used for hello cloud service.</span></span>
4. <span data-ttu-id="6fc7c-211">Následující obrázek, klikněte v hello **Server – nakonfigurujte požadovaná nastavení** a **vytvořit nový server**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-211">In hello following image, click **Server - Configure required settings** and **Create a new server**.</span></span>

    ![Server toodatabase tunelového propojení](./media/cloud-services-dotnet-get-started/newdb.png)

    <span data-ttu-id="6fc7c-213">Případně pokud vaše předplatné už má server, můžete vybrat tento server z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-213">Alternatively, if your subscription already has a server, you can select that server from hello drop-down list.</span></span>
5. <span data-ttu-id="6fc7c-214">V hello **název serveru** zadejte *csvccontosodbserver*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-214">In hello **Server name** box, enter *csvccontosodbserver*.</span></span>

6. <span data-ttu-id="6fc7c-215">Zadejte **Přihlašovací jméno** a **Heslo** správce.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-215">Enter an administrator **Login Name** and **Password**.</span></span>

    <span data-ttu-id="6fc7c-216">Pokud jste vybrali možnost **Vytvořit nový server**, nebudete zadávat existující název a heslo.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-216">If you selected **Create a new server**, you aren't entering an existing name and password here.</span></span> <span data-ttu-id="6fc7c-217">Zadáváte nové jméno a heslo, že definujete teď toouse později při přístupu k databázi hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-217">You're entering a new name and password that you're defining now toouse later when you access hello database.</span></span> <span data-ttu-id="6fc7c-218">Pokud jste vybrali serveru, který jste vytvořili dříve, budete vyzváni pro hello heslo toohello účet správce jste již vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-218">If you selected a server that you created previously, you'll be prompted for hello password toohello administrative user account you already created.</span></span>
7. <span data-ttu-id="6fc7c-219">Vyberte stejný hello **umístění** kterou jste zvolili pro cloudové služby hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-219">Choose hello same **Location** that you chose for hello cloud service.</span></span>

    <span data-ttu-id="6fc7c-220">Když hello cloudové služby a databáze jsou v různých datových centrech (různých oblastech), zvýší se latence a vám bude účtována šířka pásma mimo datové centrum hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-220">When hello cloud service and database are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside hello data center.</span></span> <span data-ttu-id="6fc7c-221">Šířka pásma v rámci datového centra je zdarma.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-221">Bandwidth within a data center is free.</span></span>
8. <span data-ttu-id="6fc7c-222">Zkontrolujte **povolit službám azure tooaccess server**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-222">Check **Allow azure services tooaccess server**.</span></span>
9. <span data-ttu-id="6fc7c-223">Klikněte na tlačítko **vyberte** pro nový server hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-223">Click **Select** for hello new server.</span></span>

    ![Nový server služby SQL Database](./media/cloud-services-dotnet-get-started/newdbserver.png)
10. <span data-ttu-id="6fc7c-225">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-225">Click **Create**.</span></span>

### <a name="create-an-azure-storage-account"></a><span data-ttu-id="6fc7c-226">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="6fc7c-226">Create an Azure storage account</span></span>
<span data-ttu-id="6fc7c-227">Účet úložiště Azure poskytuje prostředky pro ukládání dat front a objektů blob v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-227">An Azure storage account provides resources for storing queue and blob data in hello cloud.</span></span>

<span data-ttu-id="6fc7c-228">V reálné aplikaci byste obvykle vytvořili samostatné účty pro data aplikací a pro data protokolování a samostatné účty pro testovací data a pro produkční data.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-228">In a real-world application, you would typically create separate accounts for application data versus logging data, and separate accounts for test data versus production data.</span></span> <span data-ttu-id="6fc7c-229">V tomto kurzu budete používat jenom jeden účet.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-229">For this tutorial, you'll use just one account.</span></span>

1. <span data-ttu-id="6fc7c-230">V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový > úložiště > účet úložiště – objekt blob, soubor, tabulka, fronta**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-230">In hello [Azure portal](https://portal.azure.com), click **New > Storage > Storage account - blob, file, table, queue**.</span></span>
2. <span data-ttu-id="6fc7c-231">V hello **název** zadejte předponu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-231">In hello **Name** box, enter a URL prefix.</span></span>

    <span data-ttu-id="6fc7c-232">Tato předpona a hello text, který se zobrazí pod polem hello bude účet úložiště tooyour jedinečnou adresu URL hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-232">This prefix plus hello text you see under hello box will be hello unique URL tooyour storage account.</span></span> <span data-ttu-id="6fc7c-233">Pokud zadáte předponu hello již byl použit někdo jiný, budete mít toochoose jinou předponu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-233">If hello prefix you enter has already been used by someone else, you'll have toochoose a different prefix.</span></span>
3. <span data-ttu-id="6fc7c-234">Sada hello **modelu nasazení** příliš*Classic*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-234">Set hello **Deployment model** too*Classic*.</span></span>

4. <span data-ttu-id="6fc7c-235">Sada hello **replikace** rozevírací seznam příliš**místně redundantního úložiště**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-235">Set hello **Replication** drop-down list too**Locally redundant storage**.</span></span>

    <span data-ttu-id="6fc7c-236">Pokud geografická replikace je povoleno pro účet úložiště, je hello uložený obsah případě významnější havárie v primárním umístění hello replikované tooa sekundárního datacentra tooenable převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-236">When geo-replication is enabled for a storage account, hello stored content is replicated tooa secondary datacenter tooenable failover if a major disaster occurs in hello primary location.</span></span> <span data-ttu-id="6fc7c-237">Geografická replikace může způsobit dodatečné náklady.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-237">Geo-replication can incur additional costs.</span></span> <span data-ttu-id="6fc7c-238">Pro testovací a vývojové účty obecně nechcete, aby toopay za geografickou replikaci.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-238">For test and development accounts, you generally don't want toopay for geo-replication.</span></span> <span data-ttu-id="6fc7c-239">Další informace naleznete v článku o [vytvoření, správě nebo odstranění účtu úložiště](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-239">For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).</span></span>

5. <span data-ttu-id="6fc7c-240">V hello **skupiny prostředků**, klikněte na tlačítko **použít existující** a skupině prostředků vyberte hello používá pro hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-240">In hello **Resource group**, click **Use existing** and select hello resource group used for hello cloud service.</span></span>
6. <span data-ttu-id="6fc7c-241">Sada hello **umístění** rozevíracího seznamu toohello stejné oblasti, které jste zvolili pro cloudové služby hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-241">Set hello **Location** drop-down list toohello same region you chose for hello cloud service.</span></span>

    <span data-ttu-id="6fc7c-242">Když jsou hello cloudové služby a účet úložiště v různých datových centrech (různých oblastech), zvýší se latence a vám bude účtována šířka pásma mimo datové centrum hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-242">When hello cloud service and storage account are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside hello data center.</span></span> <span data-ttu-id="6fc7c-243">Šířka pásma v rámci datového centra je zdarma.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-243">Bandwidth within a data center is free.</span></span>

    <span data-ttu-id="6fc7c-244">Skupina vztahů Azure nabízí mechanismus toominimize hello vzdálenosti mezi prostředky v datovém centru, můžete tak omezit latenci.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-244">Azure affinity groups provide a mechanism toominimize hello distance between resources in a data center, which can reduce latency.</span></span> <span data-ttu-id="6fc7c-245">V tomto kurzu skupinu vztahů nepoužíváme.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-245">This tutorial does not use affinity groups.</span></span> <span data-ttu-id="6fc7c-246">Další informace najdete v tématu [jak tooCreate spřažení skupiny v Azure](http://msdn.microsoft.com/library/jj156209.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-246">For more information, see [How tooCreate an Affinity Group in Azure](http://msdn.microsoft.com/library/jj156209.aspx).</span></span>
7. <span data-ttu-id="6fc7c-247">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-247">Click **Create**.</span></span>

    ![Nový účet úložiště](./media/cloud-services-dotnet-get-started/newstorage.png)

    <span data-ttu-id="6fc7c-249">Hello obrázku se vytvoří účet úložiště s adresou URL hello `csvccontosoads.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-249">In hello image, a storage account is created with hello URL `csvccontosoads.core.windows.net`.</span></span>

### <a name="configure-hello-solution-toouse-your-azure-sql-database-when-it-runs-in-azure"></a><span data-ttu-id="6fc7c-250">Konfigurace řešení toouse hello Azure SQL database při spuštění v Azure</span><span class="sxs-lookup"><span data-stu-id="6fc7c-250">Configure hello solution toouse your Azure SQL database when it runs in Azure</span></span>
<span data-ttu-id="6fc7c-251">Hello webový projekt a hello projekt role pracovního procesu každý má vlastní připojovací řetězec databáze a každý musí toopoint toohello Azure SQL database při spuštění aplikace hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-251">hello web project and hello worker role project each has its own database connection string, and each needs toopoint toohello Azure SQL database when hello app runs in Azure.</span></span>

<span data-ttu-id="6fc7c-252">Budete používat [transformaci Web.config](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) pro hello webovou roli a nastavení prostředí cloudové služby pro roli pracovního procesu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-252">You'll use a [Web.config transform](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) for hello web role and a cloud service environment setting for hello worker role.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc7c-253">V této části a hello další části uložíte přihlašovací údaje v souborech projektu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-253">In this section and hello next section, you store credentials in project files.</span></span> <span data-ttu-id="6fc7c-254">[Citlivá data neukládejte do veřejných úložišť  zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-254">[Don't store sensitive data in public source code repositories](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span></span>
>
>

1. <span data-ttu-id="6fc7c-255">Otevřete v projektu ContosoAdsWeb hello hello *Web.Release.config* transformační soubor pro aplikaci hello *Web.config* souboru, odstraňte blok komentáře hello, který obsahuje `<connectionStrings>` prvku a vložení Následující kód na příslušné místo Hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-255">In hello ContosoAdsWeb project, open hello *Web.Release.config* transform file for hello application *Web.config* file, delete hello comment block that contains a `<connectionStrings>` element, and paste hello following code in its place.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="{connectionstring}"
        providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
    </connectionStrings>
    ```

    <span data-ttu-id="6fc7c-256">Soubor hello nechte otevřený pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-256">Leave hello file open for editing.</span></span>
2. <span data-ttu-id="6fc7c-257">V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **databází SQL** v levém podokně hello, klikněte na hello databáze, které jste vytvořili v tomto kurzu a pak klikněte na tlačítko **zobrazit připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-257">In hello [Azure portal](https://portal.azure.com), click **SQL Databases** in hello left pane, click hello database you created for this tutorial, and then click **Show connection strings**.</span></span>

    ![Zobrazení připojovacích řetězců](./media/cloud-services-dotnet-get-started/showcs.png)

    <span data-ttu-id="6fc7c-259">Hello portál zobrazí připojovací řetězce s zástupný symbol pro hello heslo.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-259">hello portal displays connection strings, with a placeholder for hello password.</span></span>

    ![Připojovací řetězce](./media/cloud-services-dotnet-get-started/connstrings.png)
3. <span data-ttu-id="6fc7c-261">V hello *Web.Release.config* transformační soubor, odstraňte `{connectionstring}` a vložte jeho místní hello připojovací řetězec ADO.NET z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-261">In hello *Web.Release.config* transform file, delete `{connectionstring}` and paste in its place hello ADO.NET connection string from hello Azure portal.</span></span>
4. <span data-ttu-id="6fc7c-262">V hello připojovací řetězec, který jste vložili do hello *Web.Release.config* transformační soubor, nahraďte `{your_password_here}` hello heslem, které jste vytvořili pro novou databázi SQL hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-262">In hello connection string that you pasted into hello *Web.Release.config* transform file, replace `{your_password_here}` with hello password you created for hello new SQL database.</span></span>
5. <span data-ttu-id="6fc7c-263">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-263">Save hello file.</span></span>  
6. <span data-ttu-id="6fc7c-264">Vyberte a zkopírujte připojovací řetězec hello (bez okolních uvozovek hello) pro použití v následujících krocích konfigurace projektu role pracovního procesu hello hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-264">Select and copy hello connection string (without hello surrounding quotation marks) for use in hello following steps for configuring hello worker role project.</span></span>
7. <span data-ttu-id="6fc7c-265">V **Průzkumníku řešení**v části **role** v hello projekt cloudové služby, klikněte pravým tlačítkem na **ContosoAdsWorker** a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-265">In **Solution Explorer**, under **Roles** in hello cloud service project, right-click **ContosoAdsWorker** and then click **Properties**.</span></span>

    ![Vlastnosti rolí](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)
8. <span data-ttu-id="6fc7c-267">Klikněte na tlačítko hello **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-267">Click hello **Settings** tab.</span></span>
9. <span data-ttu-id="6fc7c-268">Změna **konfigurace služby** příliš**cloudu**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-268">Change **Service Configuration** too**Cloud**.</span></span>
10. <span data-ttu-id="6fc7c-269">Vyberte hello **hodnotu** pole pro hello `ContosoAdsDbConnectionString` nastavení a potom vložte hello připojovací řetězec, který jste zkopírovali z předchozí části kurzu hello hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-269">Select hello **Value** field for hello `ContosoAdsDbConnectionString` setting, and then paste hello connection string that you copied from hello previous section of hello tutorial.</span></span>

     ![Připojovací řetězec databáze pro roli pracovního procesu](./media/cloud-services-dotnet-get-started/workerdbcs.png)
11. <span data-ttu-id="6fc7c-271">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-271">Save your changes.</span></span>  

### <a name="configure-hello-solution-toouse-your-azure-storage-account-when-it-runs-in-azure"></a><span data-ttu-id="6fc7c-272">Konfigurace účtu úložiště Azure hello řešení toouse při spuštění v Azure</span><span class="sxs-lookup"><span data-stu-id="6fc7c-272">Configure hello solution toouse your Azure storage account when it runs in Azure</span></span>
<span data-ttu-id="6fc7c-273">Připojovací řetězce k účtu úložiště Azure pro projekt hello webové role i projekt role pracovního procesu hello jsou uloženy v nastavení prostředí v projektu cloudové služby hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-273">Azure storage account connection strings for both hello web role project and hello worker role project are stored in environment settings in hello cloud service project.</span></span> <span data-ttu-id="6fc7c-274">Pro každý projekt je samostatnou sadu nastavení toobe používá při spuštění aplikace hello místně a při spuštění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-274">For each project, there is a separate set of settings toobe used when hello application runs locally and when it runs in hello cloud.</span></span> <span data-ttu-id="6fc7c-275">Nastavení cloudového prostředí pro webové a pracovní projekty role hello budete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-275">You'll update hello cloud environment settings for both web and worker role projects.</span></span>

1. <span data-ttu-id="6fc7c-276">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **ContosoAdsWeb** pod **role** v hello **ContosoAdsCloudService** projektu a pak klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-276">In **Solution Explorer**, right-click **ContosoAdsWeb** under **Roles** in hello **ContosoAdsCloudService** project, and then click **Properties**.</span></span>

    ![Vlastnosti rolí](./media/cloud-services-dotnet-get-started/roleproperties.png)
2. <span data-ttu-id="6fc7c-278">Klikněte na tlačítko hello **nastavení** kartě. V hello **konfigurace služby** rozevíracího seznamu vyberte **cloudu**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-278">Click hello **Settings** tab. In hello **Service Configuration** drop-down box, choose **Cloud**.</span></span>

    ![Konfigurace cloudu](./media/cloud-services-dotnet-get-started/sccloud.png)
3. <span data-ttu-id="6fc7c-280">Vyberte hello **StorageConnectionString** položku a zobrazí se třemi tečkami (**...** ) tlačítko na pravém konci řádku hello hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-280">Select hello **StorageConnectionString** entry, and you'll see an ellipsis (**...**) button at hello right end of hello line.</span></span> <span data-ttu-id="6fc7c-281">Klikněte na tlačítko hello třemi tečkami tlačítko tooopen hello **vytvoření připojovacího řetězce účet úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-281">Click hello ellipsis button tooopen hello **Create Storage Account Connection String** dialog box.</span></span>

    ![Otevření pole Vytvoření připojovacího řetězce](./media/cloud-services-dotnet-get-started/opencscreate.png)
4. <span data-ttu-id="6fc7c-283">V hello **vytvoření připojovacího řetězce úložiště** dialogové okno, klikněte na tlačítko **vaše předplatné**, zvolte hello účet úložiště, který jste vytvořili dříve a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-283">In hello **Create Storage Connection String** dialog box, click **Your subscription**, choose hello storage account that you created earlier, and then click **OK**.</span></span> <span data-ttu-id="6fc7c-284">Pokud ještě nejste přihlášeni, budete vyzváni k zadání přihlašovacích údajů k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-284">If you're not already logged in, you'll be prompted for your Azure account credentials.</span></span>

    ![Vytvoření připojovacího řetězce k úložišti](./media/cloud-services-dotnet-get-started/createstoragecs.png)
5. <span data-ttu-id="6fc7c-286">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-286">Save your changes.</span></span>
6. <span data-ttu-id="6fc7c-287">Postupujte podle hello stejný postup, který jste použili pro hello `StorageConnectionString` připojovací řetězec tooset hello `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-287">Follow hello same procedure that you used for hello `StorageConnectionString` connection string tooset hello `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` connection string.</span></span>

    <span data-ttu-id="6fc7c-288">Tento připojovací řetězec se používá k protokolování.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-288">This connection string is used for logging.</span></span>
7. <span data-ttu-id="6fc7c-289">Postupujte podle hello stejný postup, který jste použili pro hello **ContosoAdsWeb** role tooset oba připojovací řetězce pro hello **ContosoAdsWorker** role.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-289">Follow hello same procedure that you used for hello **ContosoAdsWeb** role tooset both connection strings for hello **ContosoAdsWorker** role.</span></span> <span data-ttu-id="6fc7c-290">Nezapomeňte tooset **konfigurace služby** příliš**cloudu**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-290">Don't forget tooset **Service Configuration** too**Cloud**.</span></span>

<span data-ttu-id="6fc7c-291">nastavení prostředí role Hello, které jste nakonfigurovali pomocí rozhraní Visual Studia hello jsou uloženy v hello následující soubory v projektu ContosoAdsCloudService hello:</span><span class="sxs-lookup"><span data-stu-id="6fc7c-291">hello role environment settings that you have configured using hello Visual Studio UI are stored in hello following files in hello ContosoAdsCloudService project:</span></span>

* <span data-ttu-id="6fc7c-292">*ServiceDefinition.csdef* – definuje názvy nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-292">*ServiceDefinition.csdef* - Defines hello setting names.</span></span>
* <span data-ttu-id="6fc7c-293">*ServiceConfiguration.Cloud.cscfg* – poskytuje hodnoty pro spuštění aplikace hello v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-293">*ServiceConfiguration.Cloud.cscfg* - Provides values for when hello app runs in hello cloud.</span></span>
* <span data-ttu-id="6fc7c-294">*ServiceConfiguration.Local.cscfg* – poskytuje hodnoty pro spuštění aplikace hello místně.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-294">*ServiceConfiguration.Local.cscfg* - Provides values for when hello app runs locally.</span></span>

<span data-ttu-id="6fc7c-295">Například hello ServiceDefinition.csdef obsahuje následující definice hello:</span><span class="sxs-lookup"><span data-stu-id="6fc7c-295">For example, hello ServiceDefinition.csdef includes hello following definitions:</span></span>

```xml
<ConfigurationSettings>
    <Setting name="StorageConnectionString" />
    <Setting name="ContosoAdsDbConnectionString" />
</ConfigurationSettings>
```

<span data-ttu-id="6fc7c-296">A hello *ServiceConfiguration.Cloud.cscfg* soubor obsahuje hello hodnoty, které jste zadali pro tato nastavení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-296">And hello *ServiceConfiguration.Cloud.cscfg* file includes hello values you entered for those settings in Visual Studio.</span></span>

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

<span data-ttu-id="6fc7c-297">Hello `<Instances>` nastavení určuje hello počet virtuálních počítačů, které Azure spustí pracovní hello kód role na.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-297">hello `<Instances>` setting specifies hello number of virtual machines that Azure will run hello worker role code on.</span></span> <span data-ttu-id="6fc7c-298">Hello [další kroky](#next-steps) část obsahuje odkazy toomore informace o škálování cloudové služby</span><span class="sxs-lookup"><span data-stu-id="6fc7c-298">hello [Next steps](#next-steps) section includes links toomore information about scaling out a cloud service,</span></span>

### <a name="deploy-hello-project-tooazure"></a><span data-ttu-id="6fc7c-299">Nasazení projektu tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="6fc7c-299">Deploy hello project tooAzure</span></span>
1. <span data-ttu-id="6fc7c-300">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **ContosoAdsCloudService** cloudový projekt a potom vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-300">In **Solution Explorer**, right-click hello **ContosoAdsCloudService** cloud project and then select **Publish**.</span></span>

   ![Publikování nabídky](./media/cloud-services-dotnet-get-started/pubmenu.png)
2. <span data-ttu-id="6fc7c-302">V hello **přihlášení** krok hello **publikování aplikaci Azure** průvodce, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-302">In hello **Sign in** step of hello **Publish Azure Application** wizard, click **Next**.</span></span>

    ![Krok Přihlásit se](./media/cloud-services-dotnet-get-started/pubsignin.png)
3. <span data-ttu-id="6fc7c-304">V hello **nastavení** krok hello průvodce, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-304">In hello **Settings** step of hello wizard, click **Next**.</span></span>

    ![Krok Nastavení](./media/cloud-services-dotnet-get-started/pubsettings.png)

    <span data-ttu-id="6fc7c-306">Výchozí nastavení v hello Hello **Upřesnit** kartě jsou pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-306">hello default settings in hello **Advanced** tab are fine for this tutorial.</span></span> <span data-ttu-id="6fc7c-307">Informace o kartě Upřesnit hello najdete v tématu [Průvodci publikováním aplikace Azure](http://msdn.microsoft.com/library/hh535756.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-307">For information about hello advanced tab, see [Publish Azure Application Wizard](http://msdn.microsoft.com/library/hh535756.aspx).</span></span>
4. <span data-ttu-id="6fc7c-308">V hello **Souhrn** krok, klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-308">In hello **Summary** step, click **Publish**.</span></span>

    ![Krok Souhrn](./media/cloud-services-dotnet-get-started/pubsummary.png)

   <span data-ttu-id="6fc7c-310">Hello **protokol činnosti Azure** otevře se okno v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-310">hello **Azure Activity Log** window opens in Visual Studio.</span></span>
5. <span data-ttu-id="6fc7c-311">Klikněte na tlačítko hello šipku vpravo ikonu tooexpand hello podrobnosti o nasazení.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-311">Click hello right arrow icon tooexpand hello deployment details.</span></span>

    <span data-ttu-id="6fc7c-312">Hello nasazení může trvat až minut too5 nebo další toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-312">hello deployment can take up too5 minutes or more toocomplete.</span></span>

    ![Okno Protokol činnosti Azure](./media/cloud-services-dotnet-get-started/waal.png)
6. <span data-ttu-id="6fc7c-314">Po dokončení hello stav nasazení, klikněte na tlačítko hello **adresa URL webové aplikace** toostart hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-314">When hello deployment status is complete, click hello **Web app URL** toostart hello application.</span></span>
7. <span data-ttu-id="6fc7c-315">Nyní můžete otestovat aplikace hello vytváření, zobrazením a upravit některé reklamy, stejně jako při spuštění aplikace hello místně.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-315">You can now test hello app by creating, viewing, and editing some ads, as you did when you ran hello application locally.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc7c-316">Pokud jste dokončení testování, odstraňte nebo zastavte cloudovou službu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-316">When you're finished testing, delete or stop hello cloud service.</span></span> <span data-ttu-id="6fc7c-317">I v případě, že nepoužíváte hello cloudové služby, protože jsou pro ně vyhrazené prostředky virtuálního počítače totiž nabíhají poplatky.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-317">Even if you're not using hello cloud service, it's accruing charges because virtual machine resources are reserved for it.</span></span> <span data-ttu-id="6fc7c-318">A pokud je necháte spuštěné, může každý, kdo najde vaši adresu URL, vytvářet a zobrazovat reklamy.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-318">And if you leave it running, anyone who finds your URL can create and view ads.</span></span> <span data-ttu-id="6fc7c-319">V hello [portál Azure](https://portal.azure.com), přejděte toohello **přehled** pro cloudové služby a pak klikněte hello **odstranit** tlačítko hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-319">In hello [Azure portal](https://portal.azure.com), go toohello **Overview** tab for your cloud service, and then click hello **Delete** button at hello top of hello page.</span></span> <span data-ttu-id="6fc7c-320">Pokud chcete zabránit tootemporarily ostatním v přístupu k webu hello, klikněte na tlačítko **Zastavit** místo.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-320">If you just want tootemporarily prevent others from accessing hello site, click **Stop** instead.</span></span> <span data-ttu-id="6fc7c-321">V takovém případě budou poplatky dál tooaccrue.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-321">In that case, charges will continue tooaccrue.</span></span> <span data-ttu-id="6fc7c-322">Pokud je již nepotřebujete, můžete provést podobné hello toodelete postup SQL databáze a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-322">You can follow a similar procedure toodelete hello SQL database and storage account when you no longer need them.</span></span>
>
>

## <a name="create-hello-application-from-scratch"></a><span data-ttu-id="6fc7c-323">Vytvoření aplikace hello od začátku</span><span class="sxs-lookup"><span data-stu-id="6fc7c-323">Create hello application from scratch</span></span>
<span data-ttu-id="6fc7c-324">Pokud jste ještě nestáhli [aplikace hello Dokončit](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), udělejte to teď.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-324">If you haven't already downloaded [hello completed application](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), do that now.</span></span> <span data-ttu-id="6fc7c-325">Budete kopírovat soubory z projektu hello stáhli do nového projektu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-325">You'll copy files from hello downloaded project into hello new project.</span></span>

<span data-ttu-id="6fc7c-326">Vytvoření aplikace Contoso Ads hello zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6fc7c-326">Creating hello Contoso Ads application involves hello following steps:</span></span>

* <span data-ttu-id="6fc7c-327">Vytvoření řešení cloudové služby Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fc7c-327">Create a cloud service Visual Studio solution.</span></span>
* <span data-ttu-id="6fc7c-328">Aktualizace a přidání balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="6fc7c-328">Update and add NuGet packages.</span></span>
* <span data-ttu-id="6fc7c-329">Nastavení odkazů projektu</span><span class="sxs-lookup"><span data-stu-id="6fc7c-329">Set project references.</span></span>
* <span data-ttu-id="6fc7c-330">Konfigurace připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="6fc7c-330">Configure connection strings.</span></span>
* <span data-ttu-id="6fc7c-331">Přidání souborů s kódy</span><span class="sxs-lookup"><span data-stu-id="6fc7c-331">Add code files.</span></span>

<span data-ttu-id="6fc7c-332">Po vytvoření hello řešení zkontrolujete hello kód, který je jedinečný toocloud služby projekty a Azure objekty BLOB a fronty.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-332">After hello solution is created, you'll review hello code that is unique toocloud service projects and Azure blobs and queues.</span></span>

### <a name="create-a-cloud-service-visual-studio-solution"></a><span data-ttu-id="6fc7c-333">Vytvoření řešení cloudové služby Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fc7c-333">Create a cloud service Visual Studio solution</span></span>
1. <span data-ttu-id="6fc7c-334">V sadě Visual Studio, vyberte **nový projekt** z hello **souboru** nabídky.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-334">In Visual Studio, choose **New Project** from hello **File** menu.</span></span>
2. <span data-ttu-id="6fc7c-335">V levém podokně hello hello **nový projekt** dialogové okno, rozbalte seznam **Visual C#** a zvolte **cloudu** šablony a potom zvolte hello **CloudováslužbaAzure** šablony.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-335">In hello left pane of hello **New Project** dialog box, expand **Visual C#** and choose **Cloud** templates, and then choose hello **Azure Cloud Service** template.</span></span>
3. <span data-ttu-id="6fc7c-336">Název hello projektu a řešení ContosoAdsCloudService a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-336">Name hello project and solution ContosoAdsCloudService, and then click **OK**.</span></span>

    ![Nový projekt](./media/cloud-services-dotnet-get-started/newproject.png)
4. <span data-ttu-id="6fc7c-338">V hello **nová Cloudová služba Azure** dialogové okno pole, přidejte webovou roli a roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-338">In hello **New Azure Cloud Service** dialog box, add a web role and a worker role.</span></span> <span data-ttu-id="6fc7c-339">Název hello webovou roli ContosoAdsWeb a roli pracovního procesu hello název ContosoAdsWorker.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-339">Name hello web role ContosoAdsWeb, and name hello worker role ContosoAdsWorker.</span></span> <span data-ttu-id="6fc7c-340">(Použijte ikonu tužky hello v hello pravém podokně toochange hello výchozích názvů rolí hello.)</span><span class="sxs-lookup"><span data-stu-id="6fc7c-340">(Use hello pencil icon in hello right-hand pane toochange hello default names of hello roles.)</span></span>

    ![Nový projekt cloudové služby](./media/cloud-services-dotnet-get-started/newcsproj.png)
5. <span data-ttu-id="6fc7c-342">Až se zobrazí hello **nový projekt ASP.NET** dialogové okno pro webovou roli hello, zvolte šablonu MVC hello a pak klikněte na tlačítko **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-342">When you see hello **New ASP.NET Project** dialog box for hello web role, choose hello MVC template, and then click **Change Authentication**.</span></span>

    ![Změna ověřování](./media/cloud-services-dotnet-get-started/chgauth.png)
6. <span data-ttu-id="6fc7c-344">V hello **změna ověřování** dialogovém okně vyberte **bez ověřování**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-344">In hello **Change Authentication** dialog box, choose **No Authentication**, and then click **OK**.</span></span>

    ![Bez ověřování](./media/cloud-services-dotnet-get-started/noauth.png)
7. <span data-ttu-id="6fc7c-346">V hello **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-346">In hello **New ASP.NET Project** dialog, click **OK**.</span></span>
8. <span data-ttu-id="6fc7c-347">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello řešení (ne hello projektů) a zvolte **přidat – nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-347">In **Solution Explorer**, right-click hello solution (not one of hello projects), and choose **Add - New Project**.</span></span>
9. <span data-ttu-id="6fc7c-348">V hello **přidat nový projekt** dialogovém okně vyberte **Windows** pod **Visual C#** v levém podokně text hello a potom klikněte na hello **knihovny tříd** Šablona.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-348">In hello **Add New Project** dialog box, choose **Windows** under **Visual C#** in hello left pane, and then click hello **Class Library** template.</span></span>  
10. <span data-ttu-id="6fc7c-349">Název projektu hello *ContosoAdsCommon*a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-349">Name hello project *ContosoAdsCommon*, and then click **OK**.</span></span>

    <span data-ttu-id="6fc7c-350">Je nutné tooreference hello kontextu a hello datový model Entity Framework z projektů webové a pracovní role.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-350">You need tooreference hello Entity Framework context and hello data model from both web and worker role projects.</span></span> <span data-ttu-id="6fc7c-351">Jako alternativu můžete definovat třídy související s EF hello v hello projekt webové role a odkazovat na takový projekt z projektu role pracovního procesu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-351">As an alternative, you could define hello EF-related classes in hello web role project and reference that project from hello worker role project.</span></span> <span data-ttu-id="6fc7c-352">Ale v hello alternativní způsob, váš projekt role pracovního procesu být referenční tooweb sestavení, která nepotřebuje.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-352">But in hello alternative approach, your worker role project would have a reference tooweb assemblies that it doesn't need.</span></span>

### <a name="update-and-add-nuget-packages"></a><span data-ttu-id="6fc7c-353">Aktualizace a přidání balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="6fc7c-353">Update and add NuGet packages</span></span>
1. <span data-ttu-id="6fc7c-354">Otevřete hello **spravovat balíčky NuGet** dialogové okno pro řešení hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-354">Open hello **Manage NuGet Packages** dialog box for hello solution.</span></span>
2. <span data-ttu-id="6fc7c-355">Hello horní části okna hello, vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-355">At hello top of hello window, select **Updates**.</span></span>
3. <span data-ttu-id="6fc7c-356">Vyhledejte hello *WindowsAzure.Storage* balíčku a pokud je v seznamu hello, vyberte ho a vyberte hello webové a pracovní projekty tooupdate ho a pak klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-356">Look for hello *WindowsAzure.Storage* package, and if it's in hello list, select it and select hello web and worker projects tooupdate it in, and then click **Update**.</span></span>

    <span data-ttu-id="6fc7c-357">Klientská knihovna pro úložiště Hello je aktualizován častěji než šablony projektů Visual Studio, tak, že hello verze budete často zjistíte v toobe nově vytvořený projekt musí aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-357">hello storage client library is updated more frequently than Visual Studio project templates, so you'll often find that hello version in a newly-created project needs toobe updated.</span></span>
4. <span data-ttu-id="6fc7c-358">Hello horní části okna hello, vyberte **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-358">At hello top of hello window, select **Browse**.</span></span>
5. <span data-ttu-id="6fc7c-359">Najde hello *EntityFramework* NuGet balíček a nainstalujte ho do všech tří projektů.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-359">Find hello *EntityFramework* NuGet package, and install it in all three projects.</span></span>
6. <span data-ttu-id="6fc7c-360">Najde hello *Microsoft.WindowsAzure.ConfigurationManager* NuGet balíčku a nainstalujte ho do projektu role pracovního procesu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-360">Find hello *Microsoft.WindowsAzure.ConfigurationManager* NuGet package, and install it in hello worker role project.</span></span>

### <a name="set-project-references"></a><span data-ttu-id="6fc7c-361">Nastavení odkazů na projekty</span><span class="sxs-lookup"><span data-stu-id="6fc7c-361">Set project references</span></span>
1. <span data-ttu-id="6fc7c-362">V hello projektu ContosoAdsWeb nastavte odkaz na projekt ContosoAdsCommon toohello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-362">In hello ContosoAdsWeb project, set a reference toohello ContosoAdsCommon project.</span></span> <span data-ttu-id="6fc7c-363">Klikněte pravým tlačítkem na projekt ContosoAdsWeb hello a pak klikněte na **odkazy** - **přidat odkazy**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-363">Right-click hello ContosoAdsWeb project, and then click **References** - **Add References**.</span></span> <span data-ttu-id="6fc7c-364">V hello **správce odkazů** dialogové okno, vyberte **řešení – projekty** v levém podokně hello vyberte **ContosoAdsCommon**a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-364">In hello **Reference Manager** dialog box, select **Solution – Projects** in hello left pane, select **ContosoAdsCommon**, and then click **OK**.</span></span>
2. <span data-ttu-id="6fc7c-365">V hello projektu ContosoAdsWorker nastavte odkaz na projekt ContosAdsCommon toohello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-365">In hello ContosoAdsWorker project, set a reference toohello ContosAdsCommon project.</span></span>

    <span data-ttu-id="6fc7c-366">ContosoAdsCommon bude obsahovat hello Entity Framework datový model a třídu kontextu, který se použije i hello front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-366">ContosoAdsCommon will contain hello Entity Framework data model and context class, which will be used by both hello front-end and back-end.</span></span>
3. <span data-ttu-id="6fc7c-367">V hello projektu ContosoAdsWorker nastavte odkaz příliš`System.Drawing`.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-367">In hello ContosoAdsWorker project, set a reference too`System.Drawing`.</span></span>

    <span data-ttu-id="6fc7c-368">Toto sestavení používá toothumbnails bitové kopie hello tooconvert back-end.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-368">This assembly is used by hello back-end tooconvert images toothumbnails.</span></span>

### <a name="configure-connection-strings"></a><span data-ttu-id="6fc7c-369">Konfigurace připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="6fc7c-369">Configure connection strings</span></span>
<span data-ttu-id="6fc7c-370">V této části budete konfigurovat službu Azure Storage a připojovací řetězce SQL pro místní testování.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-370">In this section, you configure Azure Storage and SQL connection strings for testing locally.</span></span> <span data-ttu-id="6fc7c-371">Pokyny k nasazení Hello v kurzu hello vysvětlují, jak tooset až hello připojovací řetězce pro při hello aplikace běží v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-371">hello deployment instructions earlier in hello tutorial explain how tooset up hello connection strings for when hello app runs in hello cloud.</span></span>

1. <span data-ttu-id="6fc7c-372">V projektu ContosoAdsWeb hello hello otevřete aplikační soubor Web.config a vložení hello následující `connectionStrings` element po hello `configSections` elementu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-372">In hello ContosoAdsWeb project, open hello application Web.config file, and insert hello following `connectionStrings` element after hello `configSections` element.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    <span data-ttu-id="6fc7c-373">Pokud používáte Visual Studio 2015 nebo vyšší, nahraďte text „v11.0“ textem „MSSQLLocalDB“.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-373">If you're using Visual Studio 2015 or higher, replace "v11.0" with "MSSQLLocalDB".</span></span>
2. <span data-ttu-id="6fc7c-374">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-374">Save your changes.</span></span>
3. <span data-ttu-id="6fc7c-375">V projektu ContosoAdsCloudService hello, klikněte pravým tlačítkem na ContosoAdsWeb pod **role**a potom klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-375">In hello ContosoAdsCloudService project, right-click ContosoAdsWeb under **Roles**, and then click **Properties**.</span></span>

    ![Vlastnosti rolí](./media/cloud-services-dotnet-get-started/roleproperties.png)
4. <span data-ttu-id="6fc7c-377">V hello **ContosAdsWeb [Role]** okně vlastností klikněte na tlačítko hello **nastavení** a pak klikněte **přidat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-377">In hello **ContosAdsWeb [Role]** properties window, click hello **Settings** tab, and then click **Add Setting**.</span></span>

    <span data-ttu-id="6fc7c-378">Nechte **konfigurace služby** nastavit příliš**všechny konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-378">Leave **Service Configuration** set too**All Configurations**.</span></span>
5. <span data-ttu-id="6fc7c-379">Přidejte nastavení s názvem *StorageConnectionString*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-379">Add a setting named *StorageConnectionString*.</span></span> <span data-ttu-id="6fc7c-380">Nastavit **typ** příliš*ConnectionString*a nastavte **hodnotu** příliš*UseDevelopmentStorage = true*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-380">Set **Type** too*ConnectionString*, and set **Value** too*UseDevelopmentStorage=true*.</span></span>

    ![Nový připojovací řetězec](./media/cloud-services-dotnet-get-started/scall.png)
6. <span data-ttu-id="6fc7c-382">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-382">Save your changes.</span></span>
7. <span data-ttu-id="6fc7c-383">Postupujte podle hello stejný postup tooadd připojovací řetězec úložiště do vlastností role ContosoAdsWorker hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-383">Follow hello same procedure tooadd a storage connection string in hello ContosoAdsWorker role properties.</span></span>
8. <span data-ttu-id="6fc7c-384">Stále v hello **ContosoAdsWorker [Role]** vlastnosti – okno, přidejte další připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="6fc7c-384">Still in hello **ContosoAdsWorker [Role]** properties window, add another connection string:</span></span>

   * <span data-ttu-id="6fc7c-385">Název: ContosoAdsDbConnectionString</span><span class="sxs-lookup"><span data-stu-id="6fc7c-385">Name: ContosoAdsDbConnectionString</span></span>
   * <span data-ttu-id="6fc7c-386">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="6fc7c-386">Type: String</span></span>
   * <span data-ttu-id="6fc7c-387">Hodnota: Vložte hello stejný připojovací řetězec použitý pro projekt webové role hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-387">Value: Paste hello same connection string you used for hello web role project.</span></span> <span data-ttu-id="6fc7c-388">(hello následující ukázka je pro Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-388">(hello following example is for Visual Studio 2013.</span></span> <span data-ttu-id="6fc7c-389">Nezapomeňte toochange hello zdroj dat Pokud tento příklad kopírujete a používáte Visual Studio 2015 nebo vyšší.)</span><span class="sxs-lookup"><span data-stu-id="6fc7c-389">Don't forget toochange hello Data Source if you copy this example and you're using Visual Studio 2015 or higher.)</span></span>

       ```
       Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;
       ```

### <a name="add-code-files"></a><span data-ttu-id="6fc7c-390">Přidání souborů s kódy</span><span class="sxs-lookup"><span data-stu-id="6fc7c-390">Add code files</span></span>
<span data-ttu-id="6fc7c-391">V této části zkopírujete soubory kódu z řešení hello stáhli do nové řešení hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-391">In this section, you copy code files from hello downloaded solution into hello new solution.</span></span> <span data-ttu-id="6fc7c-392">Hello následující části se zobrazí a vysvětlí klíčová místa tohoto kódu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-392">hello following sections will show and explain key parts of this code.</span></span>

<span data-ttu-id="6fc7c-393">tooadd soubory tooa projektu nebo složky, klikněte pravým tlačítkem na projekt hello nebo složku a klikněte na tlačítko **přidat** - **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-393">tooadd files tooa project or a folder, right-click hello project or folder and click **Add** - **Existing Item**.</span></span> <span data-ttu-id="6fc7c-394">Vyberte soubory hello a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-394">Select hello files you want and then click **Add**.</span></span> <span data-ttu-id="6fc7c-395">Pokud se zobrazí dotaz, zda chcete, aby tooreplace existující soubory, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-395">If asked whether you want tooreplace existing files, click **Yes**.</span></span>

1. <span data-ttu-id="6fc7c-396">V projektu ContosoAdsCommon hello odstranit hello *Class1.cs* souboru a přidejte do jeho místní hello *Ad.cs* a *ContosoAdscontext.cs* stáhnout soubory z hello projektu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-396">In hello ContosoAdsCommon project, delete hello *Class1.cs* file and add in its place hello *Ad.cs* and *ContosoAdscontext.cs* files from hello downloaded project.</span></span>
2. <span data-ttu-id="6fc7c-397">V projektu ContosoAdsWeb hello přidejte následující soubory z projektu hello stáhli hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-397">In hello ContosoAdsWeb project, add hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="6fc7c-398">*Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-398">*Global.asax.cs*.</span></span>  
   * <span data-ttu-id="6fc7c-399">V hello *Views\Shared* složky:  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-399">In hello *Views\Shared* folder: *\_Layout.cshtml*.</span></span>
   * <span data-ttu-id="6fc7c-400">V hello *Views\Home* složky: *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-400">In hello *Views\Home* folder: *Index.cshtml*.</span></span>
   * <span data-ttu-id="6fc7c-401">V hello *řadiče* složky: *AdController.cs*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-401">In hello *Controllers* folder: *AdController.cs*.</span></span>
   * <span data-ttu-id="6fc7c-402">V hello *Views\Ad* složky (nejprve vytvořte složku hello): pět *.cshtml* soubory.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-402">In hello *Views\Ad* folder (create hello folder first): five *.cshtml* files.</span></span>
3. <span data-ttu-id="6fc7c-403">V projektu ContosoAdsWorker hello přidat *WorkerRole.cs* z hello stáhli projektu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-403">In hello ContosoAdsWorker project, add *WorkerRole.cs* from hello downloaded project.</span></span>

<span data-ttu-id="6fc7c-404">Teď můžete sestavit a spustit aplikaci hello podle pokynů v kurzu hello a hello aplikace bude používat místní databáze a prostředky emulátoru úložiště.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-404">You can now build and run hello application as instructed earlier in hello tutorial, and hello app will use local database and storage emulator resources.</span></span>

<span data-ttu-id="6fc7c-405">Hello následující části popisují kód hello související tooworking s hello prostředí Azure, objekty BLOB a fronty.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-405">hello following sections explain hello code related tooworking with hello Azure environment, blobs, and queues.</span></span> <span data-ttu-id="6fc7c-406">V tomto kurzu nevysvětluje, jak řadiče MVC toocreate a zobrazení pomocí generování uživatelského rozhraní, jak toowrite kódu rozhraní Entity Framework, funguje s databází serveru SQL Server nebo hello nepopisuje základy asynchronního programování v technologii ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-406">This tutorial does not explain how toocreate MVC controllers and views using scaffolding, how toowrite Entity Framework code that works with SQL Server databases, or hello basics of asynchronous programming in ASP.NET 4.5.</span></span> <span data-ttu-id="6fc7c-407">Informace o těchto tématech najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="6fc7c-407">For information about these topics, see hello following resources:</span></span>

* [<span data-ttu-id="6fc7c-408">Začínáme s MVC 5</span><span class="sxs-lookup"><span data-stu-id="6fc7c-408">Get started with MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="6fc7c-409">Začínáme s EF 6 a MVC 5</span><span class="sxs-lookup"><span data-stu-id="6fc7c-409">Get started with EF 6 and MVC 5</span></span>](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc)
* <span data-ttu-id="6fc7c-410">[Úvod tooasynchronous programování v rozhraní .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-410">[Introduction tooasynchronous programming in .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span></span>

### <a name="contosoadscommon---adcs"></a><span data-ttu-id="6fc7c-411">ContosoAdsCommon – Ad.cs</span><span class="sxs-lookup"><span data-stu-id="6fc7c-411">ContosoAdsCommon - Ad.cs</span></span>
<span data-ttu-id="6fc7c-412">Hello soubor Ad.cs definuje výčet kategorií reklam a třídu entity objektů POCO pro informace o reklamách.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-412">hello Ad.cs file defines an enum for ad categories and a POCO entity class for ad information.</span></span>

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

### <a name="contosoadscommon---contosoadscontextcs"></a><span data-ttu-id="6fc7c-413">ContosoAdsCommon – ContosoAdsContext.cs</span><span class="sxs-lookup"><span data-stu-id="6fc7c-413">ContosoAdsCommon - ContosoAdsContext.cs</span></span>
<span data-ttu-id="6fc7c-414">Hello třída ContosoAdsContext Určuje, zda text hello Ad třída se používá v kolekci DbSet, kterou Entity Framework uloží do databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-414">hello ContosoAdsContext class specifies that hello Ad class is used in a DbSet collection, which Entity Framework will store in a SQL database.</span></span>

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

<span data-ttu-id="6fc7c-415">Hello třída má dva konstruktory.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-415">hello class has two constructors.</span></span> <span data-ttu-id="6fc7c-416">Hello první z nich je používané hello webovým projektem a určuje název připojovacího řetězce, který je uložený v souboru Web.config hello hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-416">hello first of them is used by hello web project, and specifies hello name of a connection string that is stored in hello Web.config file.</span></span> <span data-ttu-id="6fc7c-417">Hello druhý konstruktor vám umožňuje toopass v hello samotný připojovací řetězec používaný hello projekt role pracovního procesu, protože sám nemá soubor Web.config.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-417">hello second constructor enables you toopass in hello actual connection string used by hello worker role project, since it doesn't have a Web.config file.</span></span> <span data-ttu-id="6fc7c-418">Už jste viděli dříve tento připojovací řetězec uložil, kde uvidíte později jak hello kód načte hello připojovací řetězec při vytvoření instance třídy DbContext hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-418">You saw earlier where this connection string was stored, and you'll see later how hello code retrieves hello connection string when it instantiates hello DbContext class.</span></span>

### <a name="contosoadsweb---globalasaxcs"></a><span data-ttu-id="6fc7c-419">ContosoAdsWeb – Global.asax.cs</span><span class="sxs-lookup"><span data-stu-id="6fc7c-419">ContosoAdsWeb - Global.asax.cs</span></span>
<span data-ttu-id="6fc7c-420">Kód, který je volána z hello `Application_Start` metoda vytvoří *bitové kopie* kontejner objektů blob a *bitové kopie* fronty, pokud ještě neexistují.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-420">Code that is called from hello `Application_Start` method creates an *images* blob container and an *images* queue if they don't already exist.</span></span> <span data-ttu-id="6fc7c-421">To zajistí, že při každém spuštění pomocí nového účtu úložiště, nebo pomocí emulátoru úložiště hello na nový počítač, hello požadovaný kontejner objektů blob a fronty se vytvoří automaticky.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-421">This ensures that whenever you start using a new storage account, or start using hello storage emulator on a new computer, hello required blob container and queue will be created automatically.</span></span>

<span data-ttu-id="6fc7c-422">Hello kód získá přístup k účtu úložiště pro toohello pomocí připojovacího řetězce úložiště hello z hello *.cscfg* souboru.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-422">hello code gets access toohello storage account by using hello storage connection string from hello *.cscfg* file.</span></span>

```csharp
var storageAccount = CloudStorageAccount.Parse
    (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));
```

<span data-ttu-id="6fc7c-423">Potom získá odkaz na toohello *bitové kopie* kontejner objektů blob, vytvoří kontejner hello, pokud ještě neexistuje, a nastaví přístupová oprávnění na nový kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-423">Then it gets a reference toohello *images* blob container, creates hello container if it doesn't already exist, and sets access permissions on hello new container.</span></span> <span data-ttu-id="6fc7c-424">Ve výchozím nastavení nové kontejnery jenom klientům s účtem úložiště objektů BLOB tooaccess přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-424">By default, new containers only allow clients with storage account credentials tooaccess blobs.</span></span> <span data-ttu-id="6fc7c-425">Hello webu musí hello toobe veřejné objekty BLOB tak, aby se může zobrazit obrázky pomocí adres URL objekty BLOB tento bod toohello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-425">hello website needs hello blobs toobe public so that it can display images using URLs that point toohello image blobs.</span></span>

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

<span data-ttu-id="6fc7c-426">Podobný kód získá odkaz na toohello *bitové kopie* fronty a vytvoří novou frontu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-426">Similar code gets a reference toohello *images* queue and creates a new queue.</span></span> <span data-ttu-id="6fc7c-427">V tomto případě nejsou nutná žádná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-427">In this case, no permissions change is needed.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
var imagesQueue = queueClient.GetQueueReference("images");
imagesQueue.CreateIfNotExists();
```

### <a name="contosoadsweb---layoutcshtml"></a><span data-ttu-id="6fc7c-428">ContosoAdsWeb – \_Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="6fc7c-428">ContosoAdsWeb - \_Layout.cshtml</span></span>
<span data-ttu-id="6fc7c-429">Hello *_Layout.cshtml* soubor nastaví název aplikace hello v hello záhlaví a zápatí a vytvoří položku nabídky "Reklamy".</span><span class="sxs-lookup"><span data-stu-id="6fc7c-429">hello *_Layout.cshtml* file sets hello app name in hello header and footer, and creates an "Ads" menu entry.</span></span>

### <a name="contosoadsweb---viewshomeindexcshtml"></a><span data-ttu-id="6fc7c-430">ContosoAdsWeb – Views\Home\Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6fc7c-430">ContosoAdsWeb - Views\Home\Index.cshtml</span></span>
<span data-ttu-id="6fc7c-431">Hello *Views\Home\Index.cshtml* souboru zobrazuje kategorie odkazy na domovskou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-431">hello *Views\Home\Index.cshtml* file displays category links on hello home page.</span></span> <span data-ttu-id="6fc7c-432">Hello odkazy předají celočíselnou hodnotu hello hello `Category` výčtu na stránce služby Active Directory Index toohello proměnné řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-432">hello links pass hello integer value of hello `Category` enum in a querystring variable toohello Ads Index page.</span></span>

```razor
<li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
<li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
<li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
<li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>
```

### <a name="contosoadsweb---adcontrollercs"></a><span data-ttu-id="6fc7c-433">ContosoAdsWeb – AdController.cs</span><span class="sxs-lookup"><span data-stu-id="6fc7c-433">ContosoAdsWeb - AdController.cs</span></span>
<span data-ttu-id="6fc7c-434">V hello *AdController.cs* soubor, hello konstruktor volání hello `InitializeStorage` metoda toocreate Klientská knihovna pro úložiště Azure objekty, které poskytují rozhraní API pro práci s objekty BLOB a frontami.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-434">In hello *AdController.cs* file, hello constructor calls hello `InitializeStorage` method toocreate Azure Storage Client Library objects that provide an API for working with blobs and queues.</span></span>

<span data-ttu-id="6fc7c-435">Pak hello kód získá odkaz na toohello *bitové kopie* kontejner objektů blob, jako jste viděli dříve v *Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-435">Then hello code gets a reference toohello *images* blob container as you saw earlier in *Global.asax.cs*.</span></span> <span data-ttu-id="6fc7c-436">Během toho nastaví výchozí [zásady opakování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling), které jsou vhodné pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-436">While doing that it sets a default [retry policy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) appropriate for a web app.</span></span> <span data-ttu-id="6fc7c-437">zásady opakování exponenciálního omezení rychlosti výchozí Hello může přečnívat hello webové aplikace po dobu delší než minutu při opakovaných pokusech přechodná chyba.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-437">hello default exponential backoff retry policy could hang hello web app for longer than a minute on repeated retries for a transient fault.</span></span> <span data-ttu-id="6fc7c-438">Tady určené zásady opakování Hello čeká tří sekund po každém pokusu až toothree pokusů.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-438">hello retry policy specified here waits three seconds after each try for up toothree tries.</span></span>

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesBlobContainer = blobClient.GetContainerReference("images");
```

<span data-ttu-id="6fc7c-439">Podobný kód získá odkaz na toohello *bitové kopie* fronty.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-439">Similar code gets a reference toohello *images* queue.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesQueue = queueClient.GetQueueReference("images");
```

<span data-ttu-id="6fc7c-440">Většinu kódu kontroleru hello je typická pro práci s datovým modelem Entity Framework použití třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-440">Most of hello controller code is typical for working with an Entity Framework data model using a DbContext class.</span></span> <span data-ttu-id="6fc7c-441">Výjimkou je hello HttpPost `Create` metodu, která soubor odešle a uloží ho do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-441">An exception is hello HttpPost `Create` method, which uploads a file and saves it in blob storage.</span></span> <span data-ttu-id="6fc7c-442">poskytuje vazač modelu Hello [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) objektu toohello metoda.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-442">hello model binder provides an [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) object toohello method.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<ActionResult> Create(
    [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
    HttpPostedFileBase imageFile)
```

<span data-ttu-id="6fc7c-443">Pokud hello uživatel vybral soubor tooupload, kód hello odešle soubor hello, uloží ho do objektu blob a aktualizuje hello záznam v databázi reklam pomocí adresy URL, který odkazuje objekt blob toohello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-443">If hello user selected a file tooupload, hello code uploads hello file, saves it in a blob, and updates hello Ad database record with a URL that points toohello blob.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    blob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = blob.Uri.ToString();
}
```

<span data-ttu-id="6fc7c-444">Hello kód, který hello odesílání je v hello `UploadAndSaveBlobAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-444">hello code that does hello upload is in hello `UploadAndSaveBlobAsync` method.</span></span> <span data-ttu-id="6fc7c-445">Vytvoří identifikátor GUID pro objekt blob hello, nahrávání a uloží hello soubor a vrátí objekt blob uložit toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-445">It creates a GUID name for hello blob, uploads and saves hello file, and returns a reference toohello saved blob.</span></span>

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

<span data-ttu-id="6fc7c-446">Po hello HttpPost `Create` metoda odešle objekt blob a aktualizace hello databázi, vytvoří tooinform zprávy fronty tohoto procesu back-end, že obrázek je připravený pro převod tooa miniaturu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-446">After hello HttpPost `Create` method uploads a blob and updates hello database, it creates a queue message tooinform that back-end process that an image is ready for conversion tooa thumbnail.</span></span>

```csharp
string queueMessageString = ad.AdId.ToString();
var queueMessage = new CloudQueueMessage(queueMessageString);
await queue.AddMessageAsync(queueMessage);
```

<span data-ttu-id="6fc7c-447">Hello kód pro hello HttpPost `Edit` metoda je podobný s tím rozdílem, že pokud hello uživatel vybere nový obrázkový soubor musíte odstranit všechny objekty BLOB, které již existují.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-447">hello code for hello HttpPost `Edit` method is similar except that if hello user selects a new image file any blobs that already exist must be deleted.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    await DeleteAdBlobsAsync(ad);
    imageBlob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = imageBlob.Uri.ToString();
}
```

<span data-ttu-id="6fc7c-448">Hello další příklad ukazuje hello kód, který odstraní objekty BLOB při odstranění ad.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-448">hello next example shows hello code that deletes blobs when you delete an ad.</span></span>

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

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a><span data-ttu-id="6fc7c-449">ContosoAdsWeb – Views\Ad\Index.cshtml a Details.cshtml</span><span class="sxs-lookup"><span data-stu-id="6fc7c-449">ContosoAdsWeb - Views\Ad\Index.cshtml and Details.cshtml</span></span>
<span data-ttu-id="6fc7c-450">Hello *Index.cshtml* souboru zobrazí miniatury s hello dalšími daty reklam.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-450">hello *Index.cshtml* file displays thumbnails with hello other ad data.</span></span>

```razor
<img src="@Html.Raw(item.ThumbnailURL)" />
```

<span data-ttu-id="6fc7c-451">Hello *Details.cshtml* zobrazí obrázek hello plné velikosti.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-451">hello *Details.cshtml* file displays hello full-size image.</span></span>

```razor
<img src="@Html.Raw(Model.ImageURL)" />
```

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a><span data-ttu-id="6fc7c-452">ContosoAdsWeb – Views\Ad\Create.cshtml a Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="6fc7c-452">ContosoAdsWeb - Views\Ad\Create.cshtml and Edit.cshtml</span></span>
<span data-ttu-id="6fc7c-453">Hello *Create.cshtml* a *Edit.cshtml* soubory zadejte formuláře kódování, že umožňuje hello řadiče tooget hello `HttpPostedFileBase` objektu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-453">hello *Create.cshtml* and *Edit.cshtml* files specify form encoding that enables hello controller tooget hello `HttpPostedFileBase` object.</span></span>

```razor
@using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))
```

<span data-ttu-id="6fc7c-454">`<input>` Element informuje hello prohlížeče tooprovide dialogové okno pro výběr souboru.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-454">An `<input>` element tells hello browser tooprovide a file selection dialog.</span></span>

```razor
<input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />
```

### <a name="contosoadsworker---workerrolecs---onstart-method"></a><span data-ttu-id="6fc7c-455">ContosoAdsWorker – WorkerRole.cs – metoda OnStart </span><span class="sxs-lookup"><span data-stu-id="6fc7c-455">ContosoAdsWorker - WorkerRole.cs - OnStart method</span></span>
<span data-ttu-id="6fc7c-456">prostředí role pracovního procesu Azure Hello volá hello `OnStart` metoda v hello `WorkerRole` třídy, když se spouští role pracovního procesu hello a zavolá hello `Run` metoda při hello `OnStart` Metoda dokončení.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-456">hello Azure worker role environment calls hello `OnStart` method in hello `WorkerRole` class when hello worker role is getting started, and it calls hello `Run` method when hello `OnStart` method finishes.</span></span>

<span data-ttu-id="6fc7c-457">Hello `OnStart` metoda získá připojovací řetězec databáze hello z hello *.cscfg* a předá ho toohello třídy Entity Framework DbContext.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-457">hello `OnStart` method gets hello database connection string from hello *.cscfg* file and passes it toohello Entity Framework DbContext class.</span></span> <span data-ttu-id="6fc7c-458">Poskytovatel Sqlclienta Hello se používá ve výchozím nastavení, takže hello zprostředkovatel nemá toobe zadaný.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-458">hello SQLClient provider is used by default, so hello provider does not have toobe specified.</span></span>

```csharp
var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
db = new ContosoAdsContext(dbConnString);
```

<span data-ttu-id="6fc7c-459">Potom hello metoda získá odkaz na účet úložiště toohello a vytvoří kontejner objektů blob hello a fronty, pokud ještě neexistují.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-459">After that, hello method gets a reference toohello storage account and creates hello blob container and queue if they don't exist.</span></span> <span data-ttu-id="6fc7c-460">Hello kód, pro který je podobný toowhat jste už viděli v hello webovou roli `Application_Start` metoda.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-460">hello code for that is similar toowhat you already saw in hello web role `Application_Start` method.</span></span>

### <a name="contosoadsworker---workerrolecs---run-method"></a><span data-ttu-id="6fc7c-461">ContosoAdsWorker – WorkerRole.cs – metoda Run</span><span class="sxs-lookup"><span data-stu-id="6fc7c-461">ContosoAdsWorker - WorkerRole.cs - Run method</span></span>
<span data-ttu-id="6fc7c-462">Hello `Run` metoda je volána, když hello `OnStart` metoda dokončí svoji inicializaci.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-462">hello `Run` method is called when hello `OnStart` method finishes its initialization work.</span></span> <span data-ttu-id="6fc7c-463">Hello metoda spustí nekonečnou smyčku, která sleduje nové zprávy fronty a po jejich příchodu je zpracuje.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-463">hello method executes an infinite loop that watches for new queue messages and processes them when they arrive.</span></span>

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

<span data-ttu-id="6fc7c-464">Po každé iteraci smyčky hello, pokud byla nalezena žádná zpráva fronty, hello program na sekundu uspí.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-464">After each iteration of hello loop, if no queue message was found, hello program sleeps for a second.</span></span> <span data-ttu-id="6fc7c-465">To brání tomu, aby role pracovního procesu hello by docházelo k nadměrnému využití procesoru čas a úložiště cena za transakci.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-465">This prevents hello worker role from incurring excessive CPU time and storage transaction costs.</span></span> <span data-ttu-id="6fc7c-466">Hello poradní tým Microsoftu vypráví příběh o vývojáři, který se zapomněl tooinclude tohoto nasazení tooproduction a odjel na dovolenou.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-466">hello Microsoft Customer Advisory Team tells a story about a  developer who forgot tooinclude this, deployed tooproduction, and left for vacation.</span></span> <span data-ttu-id="6fc7c-467">Když se vrátil zpět, byl účet za dohled vyšší než dovolenou hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-467">When he got back, his oversight cost more than hello vacation.</span></span>

<span data-ttu-id="6fc7c-468">Někdy hello obsah zprávy fronty výsledkem bude chyba ve zpracování.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-468">Sometimes hello content of a queue message causes an error in processing.</span></span> <span data-ttu-id="6fc7c-469">Tento postup se nazývá *nezpracovatelná zpráva*, a pokud jste právě zaprotokolovali chybu a restartování hello smyčky, může do nekonečna pokusíte tooprocess této zprávě.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-469">This is called a *poison message*, and if you just logged an error and restarted hello loop, you could endlessly try tooprocess that message.</span></span>  <span data-ttu-id="6fc7c-470">Proto blok catch hello zahrnuje příkaz, který kontroluje toosee kolikrát aplikace hello nezkusí tooprocess hello aktuální zprávu, a pokud to bylo víc než 5krát, odstraní hello zprávu z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-470">Therefore hello catch block includes an if statement that checks toosee how many times hello app has tried tooprocess hello current message, and if it has been more than 5 times, hello message is deleted from hello queue.</span></span>

<span data-ttu-id="6fc7c-471">`ProcessQueueMessage` se volá při nalezení zpráv fronty.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-471">`ProcessQueueMessage` is called when a queue message is found.</span></span>

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

<span data-ttu-id="6fc7c-472">Tento kód čte hello databáze tooget hello adresa URL obrázku, převede hello image tooa miniaturu, uloží miniaturu hello do objektu blob, aktualizuje hello databáze s adresou URL hello miniaturu v objektu blob a odstraní zprávu fronty hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-472">This code reads hello database tooget hello image URL, converts hello image tooa thumbnail, saves hello thumbnail in a blob, updates hello database with hello thumbnail blob URL, and deletes hello queue message.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc7c-473">Hello kód v hello `ConvertImageToThumbnailJPG` metoda používá třídy v oboru názvů System.Drawing hello pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-473">hello code in hello `ConvertImageToThumbnailJPG` method uses classes in hello System.Drawing namespace for simplicity.</span></span> <span data-ttu-id="6fc7c-474">Hello třídy v tomto oboru názvů však byly navrženy pro používání s formuláři Windows.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-474">However, hello classes in this namespace were designed for use with Windows Forms.</span></span> <span data-ttu-id="6fc7c-475">Jejich používání není podporované ve Windows a službě ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-475">They are not supported for use in a Windows or ASP.NET service.</span></span> <span data-ttu-id="6fc7c-476">Další informace o možnostech zpracování obrázků najdete v článcích o [dynamickém generování obrázků](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) a o [hloubkové změně velikosti uvnitř obrázků](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-476">For more information about image-processing options, see [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) and [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span></span>
>
>

## <a name="troubleshooting"></a><span data-ttu-id="6fc7c-477">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6fc7c-477">Troubleshooting</span></span>
<span data-ttu-id="6fc7c-478">V případě, že něco nefunguje vám při procházení kurzem hello pokyny v tomto kurzu, tady jsou některé běžné chyby a jak tooresolve je.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-478">In case something doesn't work while you're following hello instructions in this tutorial, here are some common errors and how tooresolve them.</span></span>

### <a name="serviceruntimeroleenvironmentexception"></a><span data-ttu-id="6fc7c-479">ServiceRuntime.RoleEnvironmentException</span><span class="sxs-lookup"><span data-stu-id="6fc7c-479">ServiceRuntime.RoleEnvironmentException</span></span>
<span data-ttu-id="6fc7c-480">Hello `RoleEnvironment` objekt při spuštění aplikace v Azure nebo při spuštění místně pomocí emulátoru služby výpočty Azure hello poskytuje Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-480">hello `RoleEnvironment` object is provided by Azure when you run an application in Azure or when you run locally using hello Azure compute emulator.</span></span>  <span data-ttu-id="6fc7c-481">Pokud k této chybě dojde, když spouštíte místně, ujistěte se, že jste hello projektu ContosoAdsCloudService nastavili jako spouštěný projekt hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-481">If you get this error when you're running locally, make sure that you have set hello ContosoAdsCloudService project as hello startup project.</span></span> <span data-ttu-id="6fc7c-482">Toto nastaví toorun hello projektu pomocí emulátoru služby výpočty Azure hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-482">This sets up hello project toorun using hello Azure compute emulator.</span></span>

<span data-ttu-id="6fc7c-483">Jednou z věcí hello hello hello aplikace používá RoleEnvironment Azure pro je tooget hello připojení řetězcové hodnoty, které jsou uložené v hello *.cscfg* souborům, takže další možnou příčinou této výjimky je chybějící připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-483">One of hello things hello application uses hello Azure RoleEnvironment for is tooget hello connection string values that are stored in hello *.cscfg* files, so another cause of this exception is a missing connection string.</span></span> <span data-ttu-id="6fc7c-484">Ujistěte se, který jste vytvořili hello nastavení StorageConnectionString pro Cloudovou i místní konfiguraci v projektu ContosoAdsWeb hello a který jste vytvořili oba připojovací řetězce pro obě konfigurace i v projektu ContosoAdsWorker hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-484">Make sure that you created hello StorageConnectionString setting for both Cloud and Local configurations in hello ContosoAdsWeb project, and that you created both connection strings for both configurations in hello ContosoAdsWorker project.</span></span> <span data-ttu-id="6fc7c-485">Pokud uděláte **najít všechny** vyhledávání storageconnectionstring v celé řešení hello měli zobrazení 9 časy v 6 soubory.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-485">If you do a **Find All** search for StorageConnectionString in hello entire solution, you should see it 9 times in 6 files.</span></span>

### <a name="cannot-override-tooport-xxx-new-port-below-minimum-allowed-value-8080-for-protocol-http"></a><span data-ttu-id="6fc7c-486">Nejde přepsat tooport xxx.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-486">Cannot override tooport xxx.</span></span> <span data-ttu-id="6fc7c-487">Nový port s nižší než minimální povolenou hodnotou 8080 pro protokol http</span><span class="sxs-lookup"><span data-stu-id="6fc7c-487">New port below minimum allowed value 8080 for protocol http</span></span>
<span data-ttu-id="6fc7c-488">Změňte číslo portu hello používá hello webového projektu.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-488">Try changing hello port number used by hello web project.</span></span> <span data-ttu-id="6fc7c-489">Klikněte pravým tlačítkem na projekt ContosoAdsWeb hello a pak klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-489">Right-click hello ContosoAdsWeb project, and then click **Properties**.</span></span> <span data-ttu-id="6fc7c-490">Klikněte na tlačítko hello **webové** a pak změňte číslo portu hello hello **adresa Url projektu** nastavení.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-490">Click hello **Web** tab, and then change hello port number in hello **Project Url** setting.</span></span>

<span data-ttu-id="6fc7c-491">Další alternativní řešení problému hello najdete v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-491">For another alternative that might resolve hello problem, see hello following  section.</span></span>

### <a name="other-errors-when-running-locally"></a><span data-ttu-id="6fc7c-492">Další chyby při místním spuštění</span><span class="sxs-lookup"><span data-stu-id="6fc7c-492">Other errors when running locally</span></span>
<span data-ttu-id="6fc7c-493">Výchozí nové cloudové služby projektů pomocí hello Azure výpočetní emulátor express toosimulate hello prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-493">By default new cloud service projects use hello Azure compute emulator express toosimulate hello Azure environment.</span></span> <span data-ttu-id="6fc7c-494">Toto je Odlehčená verze hello úplného emulátoru služby výpočty a za některých podmínek hello úplný emulátor fungovat, pokud hello expresní verze nepracuje.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-494">This is a lightweight version of hello full compute emulator, and under some conditions hello full emulator will work when hello express version does not.</span></span>  

<span data-ttu-id="6fc7c-495">toochange hello projektu toouse hello úplný emulátor, klikněte pravým tlačítkem na projekt ContosoAdsCloudService hello a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-495">toochange hello project toouse hello full emulator, right-click hello ContosoAdsCloudService project, and then click **Properties**.</span></span> <span data-ttu-id="6fc7c-496">V hello **vlastnosti** okna klikněte na tlačítko hello **webové** a pak klikněte hello **použít úplný emulátor** přepínač.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-496">In hello **Properties** window click hello **Web** tab, and then click hello **Use Full Emulator** radio button.</span></span>

<span data-ttu-id="6fc7c-497">V pořadí aplikace hello toorun s hello úplný emulátor máte tooopen Visual Studio s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-497">In order toorun hello application with hello full emulator, you have tooopen Visual Studio with administrator privileges.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6fc7c-498">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6fc7c-498">Next steps</span></span>
<span data-ttu-id="6fc7c-499">Hello aplikace Contoso Ads má záměrně jednoduchá kurz – Začínáme.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-499">hello Contoso Ads application has intentionally been kept simple for a getting-started tutorial.</span></span> <span data-ttu-id="6fc7c-500">Například neimplementuje [vkládání závislostí](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) nebo hello [úložiště a jednotky pracovních vzorů](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), nepodporuje [používání rozhraní k protokolování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), nepoužívá [ Migrace Code First EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage datový model změny nebo [odolnost připojení EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage pouze dočasné chyby sítě a tak dále.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-500">For example, it doesn't implement [dependency injection](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) or hello [repository and unit of work patterns](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), it doesn't [use an interface for logging](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), it doesn't use [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data model changes or [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage transient network errors, and so forth.</span></span>

<span data-ttu-id="6fc7c-501">Zde jsou některé cloudové služby ukázkové aplikace, které ukazují další reálného osvědčených kódovacích postupů, ze méně složitých toomore komplexní seznamu:</span><span class="sxs-lookup"><span data-stu-id="6fc7c-501">Here are some cloud service sample applications that demonstrate more real-world coding practices, listed from less complex toomore complex:</span></span>

* <span data-ttu-id="6fc7c-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span></span> <span data-ttu-id="6fc7c-503">Koncept podobný tooContoso služby Active Directory, ale implementuje víc funkcí a další realističtější postupy kódování.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-503">Similar in concept tooContoso Ads but implements more features and more real-world coding practices.</span></span>
* <span data-ttu-id="6fc7c-504">[Vícevrstvé aplikace cloudových služeb Azure s tabulkami, frontami a objekty blob](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-504">[Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36).</span></span> <span data-ttu-id="6fc7c-505">Představuje tabulky služby Azure Storage a také objekty blob a fronty.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-505">Introduces Azure Storage tables as well as blobs and queues.</span></span> <span data-ttu-id="6fc7c-506">Na základě na starší verzi hello Azure SDK pro .NET, bude vyžadovat některé změny toowork s aktuální verzí hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-506">Based on an older version of hello Azure SDK for .NET, will require some modifications toowork with hello current version.</span></span>
* <span data-ttu-id="6fc7c-507">[Základy cloudových služeb v Microsoft Azure Cloud](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-507">[Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span></span> <span data-ttu-id="6fc7c-508">Komplexní ukázka předvádějící širokou škálu osvědčených postupů, vyprodukované skupina Microsoft Patterns and Practices hello.</span><span class="sxs-lookup"><span data-stu-id="6fc7c-508">A comprehensive sample demonstrating a wide range of best practices, produced by hello Microsoft Patterns and Practices group.</span></span>

<span data-ttu-id="6fc7c-509">Obecné informace o vývoji pro hello cloud najdete v tématu [vytváření reálných cloudových aplikací s Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-509">For general information about developing for hello cloud, see [Building Real-World Cloud Apps with Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span></span>

<span data-ttu-id="6fc7c-510">Pro úvodní video tooAzure úložiště osvědčených postupů a vzorů, najdete v části [Microsoft Azure Storage – novinky, osvědčené postupy a vzory](http://channel9.msdn.com/Events/Build/2014/3-628).</span><span class="sxs-lookup"><span data-stu-id="6fc7c-510">For a video introduction tooAzure Storage best practices and patterns, see [Microsoft Azure Storage – What's New, Best Practices and Patterns](http://channel9.msdn.com/Events/Build/2014/3-628).</span></span>

<span data-ttu-id="6fc7c-511">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="6fc7c-511">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="6fc7c-512">Cloudové služby Azure Cloud Services část 1: Úvod</span><span class="sxs-lookup"><span data-stu-id="6fc7c-512">Azure Cloud Services Part 1: Introduction</span></span>](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [<span data-ttu-id="6fc7c-513">Jak toomanage cloudových služeb</span><span class="sxs-lookup"><span data-stu-id="6fc7c-513">How toomanage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="6fc7c-514">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6fc7c-514">Azure Storage</span></span>](/documentation/services/storage/)
* [<span data-ttu-id="6fc7c-515">Jak toochoose cloudu poskytovatel služeb</span><span class="sxs-lookup"><span data-stu-id="6fc7c-515">How toochoose a cloud service provider</span></span>](https://azure.microsoft.com/overview/choosing-a-cloud-service-provider/)
