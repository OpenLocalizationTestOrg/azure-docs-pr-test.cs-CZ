---
title: "aaaCreate webová rozhraní .NET v Azure App Service | Microsoft Docs"
description: "Vytvořte vícevrstvou aplikaci pomocí ASP.NET MVC a Azure. Hello front end běží ve webové aplikaci ve službě Azure App Service a hello back end běží jako webová. aplikace Hello používá rozhraní Entity Framework, SQL Database a fronty Azure storage a objekty BLOB."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: mollybos
ms.assetid: 99cb9917-483a-45f8-a98d-07d19c68c753
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/14/2017
ms.author: glenga
ms.openlocfilehash: d92fc4b81cc0d58f8e900f257e747af56d32b911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-net-webjob-in-azure-app-service"></a><span data-ttu-id="c16bf-105">Vytvoření webové úlohy .NET ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c16bf-105">Create a .NET WebJob in Azure App Service</span></span>
<span data-ttu-id="c16bf-106">Tento kurz ukazuje, jak toowrite kód jednoduchou aplikaci ASP.NET MVC 5 několika vrstvami, která používá hello [WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="c16bf-106">This tutorial shows how toowrite code for a simple multi-tier ASP.NET MVC 5 application that uses hello [WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="c16bf-107">Hello účel hello [WebJobs SDK](websites-webjobs-resources.md) je toosimplify hello kód napsaný pro běžné úkoly, které může provádět webovou úlohu, například zpracování obrázků, zpracování fronty, RSS agregace, údržba souborů a odesílání e-mailů.</span><span class="sxs-lookup"><span data-stu-id="c16bf-107">hello purpose of hello [WebJobs SDK](websites-webjobs-resources.md) is toosimplify hello code you write for common tasks that a WebJob can perform, such as image processing, queue processing, RSS aggregation, file maintenance, and sending emails.</span></span> <span data-ttu-id="c16bf-108">Hello WebJobs SDK obsahuje integrované funkce pro práci s Azure Storage a Service Bus, plánování úloh a zpracování chyb a pro jiné běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="c16bf-108">hello WebJobs SDK has built-in features for working with Azure Storage and Service Bus, for scheduling tasks and handling errors, and for many other common scenarios.</span></span> <span data-ttu-id="c16bf-109">Kromě toho má určený toobe rozšiřitelný a je [otevřete zdrojové úložiště pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span><span class="sxs-lookup"><span data-stu-id="c16bf-109">In addition, it's designed toobe extensible, and there's an [open source repository for extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span></span>

<span data-ttu-id="c16bf-110">Hello vzorová aplikace je vývěsní Tabule pro inzerci.</span><span class="sxs-lookup"><span data-stu-id="c16bf-110">hello sample application is an advertising bulletin board.</span></span> <span data-ttu-id="c16bf-111">Uživatelé mohou odesílat obrázky pro reklamy a proces back-end převede toothumbnails hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c16bf-111">Users can upload images for ads, and a backend process converts hello images toothumbnails.</span></span> <span data-ttu-id="c16bf-112">Hello ad seznamu stránka zobrazuje hello miniatury a stránce s podrobnostmi o ad hello znázorňuje obrázek hello plné velikosti.</span><span class="sxs-lookup"><span data-stu-id="c16bf-112">hello ad list page shows hello thumbnails, and hello ad details page shows hello full-size image.</span></span> <span data-ttu-id="c16bf-113">Zde je snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c16bf-113">Here's a screenshot:</span></span>

![Seznam reklam](./media/websites-dotnet-webjobs-sdk-get-started/list.png)

<span data-ttu-id="c16bf-115">Tato ukázková aplikace funguje s [Azure fronty](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) a [objektů Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage).</span><span class="sxs-lookup"><span data-stu-id="c16bf-115">This sample application works with [Azure queues](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) and [Azure blobs](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage).</span></span> <span data-ttu-id="c16bf-116">Hello kurzu se dozvíte, jak toodeploy hello aplikace příliš[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).</span><span class="sxs-lookup"><span data-stu-id="c16bf-116">hello tutorial shows how toodeploy hello application too[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).</span></span>

## <span data-ttu-id="c16bf-117"><a id="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="c16bf-117"><a id="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="c16bf-118">Hello kurz předpokládá, že víte, jak toowork s [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projekty v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c16bf-118">hello tutorial assumes that you know how toowork with [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projects in Visual Studio.</span></span>

<span data-ttu-id="c16bf-119">kurz Hello byl původně zapsán pro Visual Studio 2013, ale lze použít s novější verzí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c16bf-119">hello tutorial was originally written for Visual Studio 2013, but can be used with later versions of Visual Studio.</span></span> <span data-ttu-id="c16bf-120">Pokud používáte Visual Studio 2015 nebo 2017, poznamenejte si před spuštěním aplikace hello místně, je nutné změnit hello `Data Source` součástí připojovací řetězec SQL serveru LocalDB hello v souborech Web.config a App.config hello z `Data Source=(localdb)\v11.0` příliš`Data Source=(LocalDb)\MSSQLLocalDB`.</span><span class="sxs-lookup"><span data-stu-id="c16bf-120">If you are using Visual Studio 2015 or 2017, make note that before you run hello application locally, you must change hello `Data Source` part of hello SQL Server LocalDB connection string in hello Web.config and App.config files from `Data Source=(localdb)\v11.0` too`Data Source=(LocalDb)\MSSQLLocalDB`.</span></span>

> [!NOTE]
> <span data-ttu-id="c16bf-121"><a name="note"></a>Tento kurz, musíte mít účtu Azure toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c16bf-121"><a name="note"></a>You must have an Azure account toocomplete this tutorial:</span></span>
>
> * <span data-ttu-id="c16bf-122">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): získáte kredity, můžete použít tootry na placené služby Azure, a až se vyčerpáte, můžete zachovat hello účet a používat bezplatné služby Azure, jako jsou weby.</span><span class="sxs-lookup"><span data-stu-id="c16bf-122">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): You get credits that you can use tootry out paid Azure services, and even after they're used up, you can keep hello account and use free Azure services, such as Websites.</span></span> <span data-ttu-id="c16bf-123">Platební karty nikdy odečte, není-li explicitně změnit nastavení a požádejte toobe účtovat.</span><span class="sxs-lookup"><span data-stu-id="c16bf-123">Your credit card will never be charged, unless you explicitly change your settings and ask toobe charged.</span></span>
> * <span data-ttu-id="c16bf-124">Můžete [aktivovat platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): vaše předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="c16bf-124">You can [activate Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Your subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="c16bf-125">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="c16bf-125">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c16bf-126">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="c16bf-126">No credit cards required; no commitments.</span></span>
>
>

## <span data-ttu-id="c16bf-127"><a id="learn"></a>Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="c16bf-127"><a id="learn"></a>What you'll learn</span></span>
<span data-ttu-id="c16bf-128">Hello kurzu se dozvíte, jak toodo hello následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="c16bf-128">hello tutorial shows how toodo hello following tasks:</span></span>

* <span data-ttu-id="c16bf-129">Povolte počítači pro vývoj pro Azure nainstalováním hello Azure SDK (pouze pro Visual Studio 2013 a uživatelé 2015).</span><span class="sxs-lookup"><span data-stu-id="c16bf-129">Enable your machine for Azure development by installing hello Azure SDK (only for Visual Studio 2013 and 2015 users).</span></span>
* <span data-ttu-id="c16bf-130">Vytvořte projekt konzolové aplikace, který automaticky nasadí jako webová úloha služby Azure, když nasazujete hello přidružené webového projektu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-130">Create a Console Application project that automatically deploys as an Azure WebJob when you deploy hello associated web project.</span></span>
* <span data-ttu-id="c16bf-131">Testování back-end WebJobs SDK místně na vývojovém počítači hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-131">Test a WebJobs SDK backend locally on hello development computer.</span></span>
* <span data-ttu-id="c16bf-132">Publikování aplikace pomocí WebJobs back-end tooa webové aplikace ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="c16bf-132">Publish an application with a WebJobs backend tooa web app in App Service.</span></span>
* <span data-ttu-id="c16bf-133">Odeslání souborů a uložit je do hello služby objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="c16bf-133">Upload files and store them in hello Azure Blob service.</span></span>
* <span data-ttu-id="c16bf-134">Použijte hello Azure WebJobs SDK toowork s Azure Storage fronty a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="c16bf-134">Use hello Azure WebJobs SDK toowork with Azure Storage queues and blobs.</span></span>

## <span data-ttu-id="c16bf-135"><a id="contosoads"></a>Architektura aplikace</span><span class="sxs-lookup"><span data-stu-id="c16bf-135"><a id="contosoads"></a>Application architecture</span></span>
<span data-ttu-id="c16bf-136">Hello ukázková aplikace používá hello [fronty způsob práce zaměřený](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff zatížení hello náročná na prostředky procesoru práci při vytváření miniatur tooa back-end procesu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-136">hello sample application uses hello [queue-centric work pattern](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-load hello CPU-intensive work of creating thumbnails tooa backend process.</span></span>

<span data-ttu-id="c16bf-137">Hello aplikace ukládá reklamy do databáze SQL pomocí Entity Framework Code First toocreate hello tabulky a přístup k datům hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-137">hello app stores ads in a SQL database, using Entity Framework Code First toocreate hello tables and access hello data.</span></span> <span data-ttu-id="c16bf-138">Pro každou reklamu hello databáze ukládá dvě adresy URL: jednu pro obrázek hello plné velikosti a druhou pro miniaturu hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-138">For each ad, hello database stores two URLs: one for hello full-size image and one for hello thumbnail.</span></span>

![Tabulka reklam](./media/websites-dotnet-webjobs-sdk-get-started/adtable.png)

<span data-ttu-id="c16bf-140">Když uživatel odešle obrázku, hello webové aplikace ukládá hello bitové kopie v [objektů blob v Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), a ukládá hello ad informace v databázi hello s adresou URL, který odkazuje objekt blob toohello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-140">When a user uploads an image, hello web app stores hello image in an [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and it stores hello ad information in hello database with a URL that points toohello blob.</span></span> <span data-ttu-id="c16bf-141">Na hello stejný čas, zapíše zpráva tooan fronty Azure.</span><span class="sxs-lookup"><span data-stu-id="c16bf-141">At hello same time, it writes a message tooan Azure queue.</span></span> <span data-ttu-id="c16bf-142">V back-end proces, který běží jako webová úloha služby Azure dotazuje hello WebJobs SDK hello fronty pro nové zprávy.</span><span class="sxs-lookup"><span data-stu-id="c16bf-142">In a backend process running as an Azure WebJob, hello WebJobs SDK polls hello queue for new messages.</span></span> <span data-ttu-id="c16bf-143">Když se objeví nová zpráva, hello webové úlohy vytvoří miniaturu obrázku a aktualizace hello miniatur databáze pole adresy URL pro tuto reklamu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-143">When a new message appears, hello WebJob creates a thumbnail for that image and updates hello thumbnail URL database field for that ad.</span></span> <span data-ttu-id="c16bf-144">Zde je diagram, který znázorňuje interakci částí hello hello aplikace:</span><span class="sxs-lookup"><span data-stu-id="c16bf-144">Here's a diagram that shows how hello parts of hello application interact:</span></span>

![Architektura Contoso Ads](./media/websites-dotnet-webjobs-sdk-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]  
<span data-ttu-id="c16bf-146">pokyny kurzu Hello použít tooAzure SDK pro .NET 2.7.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c16bf-146">hello tutorial instructions apply tooAzure SDK for .NET 2.7.1 or later.</span></span>

## <span data-ttu-id="c16bf-147"><a id="storage"></a>Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="c16bf-147"><a id="storage"></a>Create an Azure Storage account</span></span>
<span data-ttu-id="c16bf-148">Účet úložiště Azure poskytuje prostředky pro ukládání dat front a objektů blob v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-148">An Azure storage account provides resources for storing queue and blob data in hello cloud.</span></span> <span data-ttu-id="c16bf-149">Používá se také podle hello WebJobs SDK toostore protokolování dat pro řídicí panel hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-149">It's also used by hello WebJobs SDK toostore logging data for hello dashboard.</span></span>

<span data-ttu-id="c16bf-150">V reálné aplikaci obvykle vytvoříte samostatné účty pro data aplikací a pro data protokolování a samostatné účty pro testovací data a pro produkční data.</span><span class="sxs-lookup"><span data-stu-id="c16bf-150">In a real-world application, you typically create separate accounts for application data versus logging data and separate accounts for test data versus production data.</span></span> <span data-ttu-id="c16bf-151">V tomto kurzu budete používat jenom jeden účet.</span><span class="sxs-lookup"><span data-stu-id="c16bf-151">For this tutorial, you'll use just one account.</span></span>

1. <span data-ttu-id="c16bf-152">Otevřete hello **Průzkumníka serveru** oken v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c16bf-152">Open hello **Server Explorer** window in Visual Studio.</span></span>
2. <span data-ttu-id="c16bf-153">Klikněte pravým tlačítkem na hello **Azure** uzel a pak klikněte na tlačítko **připojit tooMicrosoft předplatné Azure...** .</span><span class="sxs-lookup"><span data-stu-id="c16bf-153">Right-click hello **Azure** node, and then click **Connect tooMicrosoft Azure Subscription...**.</span></span>
   
   ![Připojit tooAzure](./media/websites-dotnet-webjobs-sdk-get-started/connaz.png)

3. <span data-ttu-id="c16bf-155">Přihlaste se pomocí přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="c16bf-155">Sign in using your Azure credentials.</span></span>
4. <span data-ttu-id="c16bf-156">Klikněte pravým tlačítkem na **úložiště** v části hello Azure uzel a potom klikněte na **vytvořit účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-156">Right-click **Storage** under hello Azure node, and then click **Create Storage Account**.</span></span>
   
   ![Vytvořit účet úložiště](./media/websites-dotnet-webjobs-sdk-get-started/createstor.png)
   
5. <span data-ttu-id="c16bf-158">V hello **vytvořit účet úložiště** dialogové okno, zadejte název pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-158">In hello **Create Storage Account** dialog, enter a name for hello storage account.</span></span>

    <span data-ttu-id="c16bf-159">Název Hello musí být musí být jedinečné (žádný jiný účet úložiště Azure může mít hello stejný název).</span><span class="sxs-lookup"><span data-stu-id="c16bf-159">hello name must be must be unique (no other Azure storage account can have hello same name).</span></span> <span data-ttu-id="c16bf-160">Pokud zadáte název hello se už používá, získáte možnost toochange ho.</span><span class="sxs-lookup"><span data-stu-id="c16bf-160">If hello name you enter is already in use, you'll get a chance toochange it.</span></span>

    <span data-ttu-id="c16bf-161">Hello tooaccess adresa URL účtu úložiště bude *{name}*. core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="c16bf-161">hello URL tooaccess your storage account will be *{name}*.core.windows.net.</span></span>
6. <span data-ttu-id="c16bf-162">Sada hello **oblast nebo skupinu vztahů** rozevíracího seznamu toohello oblast nejbližší tooyou.</span><span class="sxs-lookup"><span data-stu-id="c16bf-162">Set hello **Region or Affinity Group** drop-down list toohello region closest tooyou.</span></span>

    <span data-ttu-id="c16bf-163">Toto nastavení určuje, které datové centrum Azure bude hostitelem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c16bf-163">This setting specifies which Azure datacenter will host your storage account.</span></span> <span data-ttu-id="c16bf-164">V tomto kurzu nebude Zkontrolujte své volby významné rozdíly.</span><span class="sxs-lookup"><span data-stu-id="c16bf-164">For this tutorial, your choice won't make a noticeable difference.</span></span> <span data-ttu-id="c16bf-165">Pro produkční webové aplikace, ale chcete na váš webový server a vaše toobe účet úložiště v hello stejnou oblast toominimize latenci a data výstupních poplatky.</span><span class="sxs-lookup"><span data-stu-id="c16bf-165">However, for a production web app, you want your web server and your storage account toobe in hello same region toominimize latency and data egress charges.</span></span> <span data-ttu-id="c16bf-166">Hello webové aplikace (aplikaci, kterou vytvoříte později) zavřete možném toohello prohlížeče přístup hello webové aplikace v pořadí toominimize latence musí být datového centra.</span><span class="sxs-lookup"><span data-stu-id="c16bf-166">hello web app (which you'll create later) datacenter should be as close as possible toohello browsers accessing hello web app in order toominimize latency.</span></span>
7. <span data-ttu-id="c16bf-167">Sada hello **replikace** rozevírací seznam příliš**místně redundantní**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-167">Set hello **Replication** drop-down list too**Locally redundant**.</span></span>

    <span data-ttu-id="c16bf-168">Když je povolené geografická replikace pro účet úložiště, hello uložený obsah je umístění toothat replikované tooa sekundárního datacentra tooenable převzetí služeb při selhání v případě významnější havárie v primárním umístění hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-168">When geo-replication is enabled for a storage account, hello stored content is replicated tooa secondary datacenter tooenable failover toothat location in case of a major disaster in hello primary location.</span></span> <span data-ttu-id="c16bf-169">Geografická replikace může způsobit dodatečné náklady.</span><span class="sxs-lookup"><span data-stu-id="c16bf-169">Geo-replication can incur additional costs.</span></span> <span data-ttu-id="c16bf-170">Pro testovací a vývojové účty obecně nechcete, aby toopay za geografickou replikaci.</span><span class="sxs-lookup"><span data-stu-id="c16bf-170">For test and development accounts, you generally don't want toopay for geo-replication.</span></span> <span data-ttu-id="c16bf-171">Další informace naleznete v článku o [vytvoření, správě nebo odstranění účtu úložiště](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="c16bf-171">For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).</span></span>
8. <span data-ttu-id="c16bf-172">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-172">Click **Create**.</span></span>

    ![Nový účet úložiště](./media/websites-dotnet-webjobs-sdk-get-started/newstorage.png)

## <span data-ttu-id="c16bf-174"><a id="download"></a>Stažení aplikace hello</span><span class="sxs-lookup"><span data-stu-id="c16bf-174"><a id="download"></a>Download hello application</span></span>
1. <span data-ttu-id="c16bf-175">Stáhněte a rozbalte hello [dokončené řešení](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).</span><span class="sxs-lookup"><span data-stu-id="c16bf-175">Download and unzip hello [completed solution](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).</span></span>
2. <span data-ttu-id="c16bf-176">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c16bf-176">Start Visual Studio.</span></span>
3. <span data-ttu-id="c16bf-177">Z hello **soubor** nabídce zvolte **otevřít > projekt nebo řešení**, přejděte toowhere jste si stáhli hello řešení a pak otevřete soubor řešení hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-177">From hello **File** menu choose **Open > Project/Solution**, navigate toowhere you downloaded hello solution, and then open hello solution file.</span></span>
4. <span data-ttu-id="c16bf-178">Stisknutím kombinace kláves CTRL + SHIFT + B toobuild hello řešení.</span><span class="sxs-lookup"><span data-stu-id="c16bf-178">Press CTRL+SHIFT+B toobuild hello solution.</span></span>

    <span data-ttu-id="c16bf-179">Visual Studio ve výchozím nastavení automaticky obnoví obsah balíčku NuGet hello, který nebyl součástí hello *.zip* souboru.</span><span class="sxs-lookup"><span data-stu-id="c16bf-179">By default, Visual Studio automatically restores hello NuGet package content, which was not included in hello *.zip* file.</span></span> <span data-ttu-id="c16bf-180">Pokud hello balíčky neobnoví, nainstalujte je ručně budete toohello **spravovat balíčky NuGet pro řešení** dialogové okno a kliknutím na hello **obnovení** tlačítko v pravé horní hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-180">If hello packages don't restore, install them manually by going toohello **Manage NuGet Packages for Solution** dialog and clicking hello **Restore** button at hello top right.</span></span>
5. <span data-ttu-id="c16bf-181">V **Průzkumníku řešení**, ujistěte se, že **ContosoAdsWeb** je vybrán jako spouštěný projekt hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-181">In **Solution Explorer**, make sure that **ContosoAdsWeb** is selected as hello startup project.</span></span>

## <span data-ttu-id="c16bf-182"><a id="configurestorage"></a>Konfigurace aplikace toouse hello účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="c16bf-182"><a id="configurestorage"></a>Configure hello application toouse your storage account</span></span>
1. <span data-ttu-id="c16bf-183">Otevřete aplikaci hello *Web.config* souboru v projektu ContosoAdsWeb hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-183">Open hello application *Web.config* file in hello ContosoAdsWeb project.</span></span>

    <span data-ttu-id="c16bf-184">Hello soubor obsahuje připojovací řetězec SQL a úložiště Azure připojovací řetězec pro práci s objekty BLOB a frontami.</span><span class="sxs-lookup"><span data-stu-id="c16bf-184">hello file contains a SQL connection string and an Azure storage connection string for working with blobs and queues.</span></span>

    <span data-ttu-id="c16bf-185">Hello připojovací řetězec SQL body tooa [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databáze.</span><span class="sxs-lookup"><span data-stu-id="c16bf-185">hello SQL connection string points tooa [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

    <span data-ttu-id="c16bf-186">připojovací řetězec úložiště Hello je příklad, který má zástupné symboly hello klíč účtu úložiště název a přístup.</span><span class="sxs-lookup"><span data-stu-id="c16bf-186">hello storage connection string is an example that has placeholders for hello storage account name and access key.</span></span> <span data-ttu-id="c16bf-187">Nahraďte ho budete s připojovacím řetězcem, který má hello název a klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c16bf-187">You'll replace this with a connection string that has hello name and key of your storage account.</span></span>  

    ```
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
    </connectionStrings>
    ```
    <span data-ttu-id="c16bf-188">připojovací řetězec úložiště Hello je pojmenovaná AzureWebJobsStorage vzhledem k tomu, který je hello hello název, jaký používá WebJobs SDK ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c16bf-188">hello storage connection string is named AzureWebJobsStorage because that's hello name hello WebJobs SDK uses by default.</span></span> <span data-ttu-id="c16bf-189">Hello stejný název se používá tady, takže budete mít řetězcovou hodnotu tooset pouze jedno připojení v hello prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="c16bf-189">hello same name is used here so you have tooset only one connection string value in hello Azure environment.</span></span>
2. <span data-ttu-id="c16bf-190">V **Průzkumníka serveru**, klikněte pravým tlačítkem na účtu úložiště v rámci hello **úložiště** uzel a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-190">In **Server Explorer**, right-click your storage account under hello **Storage** node, and then click **Properties**.</span></span>

    ![Klikněte na tlačítko Vlastnosti účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/storppty.png)
3. <span data-ttu-id="c16bf-192">V hello **vlastnosti** okně klikněte na tlačítko **klíče účtu úložiště**a potom klikněte na nabídku s výpustkou hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-192">In hello **Properties** window, click **Storage Account Keys**, and then click hello ellipsis.</span></span>

    ![Klíče účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/stor-account-keys.png)
4. <span data-ttu-id="c16bf-194">Kopírování hello **připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-194">Copy hello **Connection String**.</span></span>

    ![Dialogové okno klíče účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/cpak.png)
5. <span data-ttu-id="c16bf-196">Nahraďte hello připojovacího řetězce úložiště v hello *Web.config* soubor s hello připojovací řetězec jste právě zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="c16bf-196">Replace hello storage connection string in hello *Web.config* file with hello connection string you just copied.</span></span> <span data-ttu-id="c16bf-197">Ujistěte se, že jste vybrali, všechno uvnitř hello uvozovky, ale není včetně uvozovek hello před vkládání.</span><span class="sxs-lookup"><span data-stu-id="c16bf-197">Make sure you select everything inside hello quotation marks but not including hello quotation marks before pasting.</span></span>
6. <span data-ttu-id="c16bf-198">Otevřete hello *App.config* soubor v projektu ContosoAdsWebJob hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-198">Open hello *App.config* file in hello ContosoAdsWebJob project.</span></span>

    <span data-ttu-id="c16bf-199">Tento soubor má dva řetězce připojení úložiště, jeden pro data aplikací a jeden pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="c16bf-199">This file has two storage connection strings, one for application data and one for logging.</span></span> <span data-ttu-id="c16bf-200">Můžete použít samostatné úložiště účty pro data aplikací a protokolování, a můžete použít [účtů více úložiště pro data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="c16bf-200">You can use separate storage accounts for application data and logging, and you can use [multiple storage accounts for data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span> <span data-ttu-id="c16bf-201">V tomto kurzu budete používat účet jednoho úložiště.</span><span class="sxs-lookup"><span data-stu-id="c16bf-201">For this tutorial, you'll use a single storage account.</span></span> <span data-ttu-id="c16bf-202">připojovací řetězce Hello mít zástupné symboly hello klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c16bf-202">hello connection strings have placeholders for hello storage account keys.</span></span>

    ```
    <configuration>
        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;"/>
    </connectionStrings>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    </configuration>

    ```

    <span data-ttu-id="c16bf-203">Ve výchozím nastavení vyhledá připojovací řetězce s názvem AzureWebJobsStorage a AzureWebJobsDashboard hello WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="c16bf-203">By default, hello WebJobs SDK looks for connection strings named AzureWebJobsStorage and AzureWebJobsDashboard.</span></span> <span data-ttu-id="c16bf-204">Jako alternativu můžete [ukládání hello připojovací řetězec, ale chcete a předat jej v explicitně toohello `JobHost` objekt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).</span><span class="sxs-lookup"><span data-stu-id="c16bf-204">As an alternative, you can [store hello connection string however you want and pass it in explicitly toohello `JobHost` object](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).</span></span>
7. <span data-ttu-id="c16bf-205">Nahraďte oba připojovací řetězce úložiště hello připojovací řetězec, který jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="c16bf-205">Replace both storage connection strings with hello connection string you copied earlier.</span></span>
8. <span data-ttu-id="c16bf-206">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="c16bf-206">Save your changes.</span></span>

## <span data-ttu-id="c16bf-207"><a id="run"></a>Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="c16bf-207"><a id="run"></a>Run hello application locally</span></span>
1. <span data-ttu-id="c16bf-208">front-end webové hello toostart aplikace hello, stiskněte CTRL + F5.</span><span class="sxs-lookup"><span data-stu-id="c16bf-208">toostart hello web frontend of hello application, press CTRL+F5.</span></span>

    <span data-ttu-id="c16bf-209">Hello výchozí prohlížeč otevře domovskou stránku toohello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-209">hello default browser opens toohello home page.</span></span> <span data-ttu-id="c16bf-210">(hello webového projektu spouští vzhledem k tomu, které jste udělali hello spouštěný projekt.)</span><span class="sxs-lookup"><span data-stu-id="c16bf-210">(hello web project runs because you've made it hello startup project.)</span></span>

    ![Domovská stránka Contoso Ads](./media/websites-dotnet-webjobs-sdk-get-started/home.png)
2. <span data-ttu-id="c16bf-212">toostart hello webové úlohy back-end systému hello aplikace, klikněte pravým tlačítkem na projekt ContosoAdsWebJob hello v **Průzkumníku řešení**a potom klikněte na **ladění** > **spustit novou instanci** .</span><span class="sxs-lookup"><span data-stu-id="c16bf-212">toostart hello WebJob backend of hello application, right-click hello ContosoAdsWebJob project in **Solution Explorer**, and then click **Debug** > **Start new instance**.</span></span>

    <span data-ttu-id="c16bf-213">Okno konzoly aplikace spustí se protokolování zpráv, což značí, že objekt WebJobs SDK JobHost hello bylo zahájeno toorun.</span><span class="sxs-lookup"><span data-stu-id="c16bf-213">A console application window opens and displays logging messages indicating hello WebJobs SDK JobHost object has started toorun.</span></span>

    ![Okno konzoly aplikace zobrazuje tento back-end hello běží](./media/websites-dotnet-webjobs-sdk-get-started/backendrunning.png)
3. <span data-ttu-id="c16bf-215">V prohlížeči, klikněte na tlačítko **vytvořit reklamu**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-215">In your browser, click  **Create an Ad**.</span></span>
4. <span data-ttu-id="c16bf-216">Zadejte nějaká testovací data, vyberte tooupload image a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-216">Enter some test data, select an image tooupload, and then click **Create**.</span></span>

    ![Vytvoření stránky](./media/websites-dotnet-webjobs-sdk-get-started/create.png)

    <span data-ttu-id="c16bf-218">aplikace Hello přejde toohello indexovou stránku, ale nezobrazí miniaturu nové reklamy hello, protože toto zpracování ještě neproběhlo.</span><span class="sxs-lookup"><span data-stu-id="c16bf-218">hello app goes toohello Index page, but it doesn't show a thumbnail for hello new ad because that processing hasn't happened yet.</span></span>

    <span data-ttu-id="c16bf-219">Mezitím po krátké čekání protokolování se v okně aplikace konzoly hello ukazuje, že fronty zpráv byla přijata a byla zpracována.</span><span class="sxs-lookup"><span data-stu-id="c16bf-219">Meanwhile, after a short wait a logging message in hello console application window shows that a queue message was received and has been processed.</span></span>

    ![Okno konzoly aplikace zobrazující, že po zpracování zprávy fronty](./media/websites-dotnet-webjobs-sdk-get-started/backendlogs.png)
5. <span data-ttu-id="c16bf-221">Po uvidíte hello protokolování zprávy v okně aplikace hello konzoly, aktualizujte hello Index stránky toosee hello miniaturu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-221">After you see hello logging messages in hello console application window, refresh hello Index page toosee hello thumbnail.</span></span>

    ![Indexová stránka](./media/websites-dotnet-webjobs-sdk-get-started/list.png)
6. <span data-ttu-id="c16bf-223">Klikněte na tlačítko **podrobnosti** pro ad toosee hello plné velikosti bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c16bf-223">Click **Details** for your ad toosee hello full-size image.</span></span>

    ![Stránka podrobností](./media/websites-dotnet-webjobs-sdk-get-started/details.png)

<span data-ttu-id="c16bf-225">Aplikace hello jste byl spuštěn v místním počítači a používá SQL Server databáze nachází na váš počítač, ale funguje v případě front a objektů BLOB v hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-225">You've been running hello application on your local computer, and it's using a SQL Server database located on your computer, but it's working with queues and blobs in hello cloud.</span></span> <span data-ttu-id="c16bf-226">V následující části hello spustíte hello aplikaci v cloudu hello používání cloudové databáze a také objekty BLOB cloudové a front.</span><span class="sxs-lookup"><span data-stu-id="c16bf-226">In hello following section you'll run hello application in hello cloud, using a cloud database as well as cloud blobs and queues.</span></span>  

## <span data-ttu-id="c16bf-227"><a id="runincloud"></a>Spuštění aplikace hello v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="c16bf-227"><a id="runincloud"></a>Run hello application in hello cloud</span></span>
<span data-ttu-id="c16bf-228">Můžete to udělat následující kroky toorun hello aplikaci v cloudu hello hello:</span><span class="sxs-lookup"><span data-stu-id="c16bf-228">You'll do hello following steps toorun hello application in hello cloud:</span></span>

* <span data-ttu-id="c16bf-229">Nasazení aplikací tooWeb.</span><span class="sxs-lookup"><span data-stu-id="c16bf-229">Deploy tooWeb Apps.</span></span> <span data-ttu-id="c16bf-230">Visual Studio automaticky vytvoří nové webové aplikace v App Service a instanci databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="c16bf-230">Visual Studio automatically creates a new web app in App Service and a SQL Database instance.</span></span>
* <span data-ttu-id="c16bf-231">Nakonfigurujte hello webové aplikace toouse účtu Azure SQL database a úložiště.</span><span class="sxs-lookup"><span data-stu-id="c16bf-231">Configure hello web app toouse your Azure SQL database and storage account.</span></span>

<span data-ttu-id="c16bf-232">Po vytvoření některé reklamy při spuštění v cloudu hello budete zobrazit hello bohaté monitorovací funkce, které má toooffer hello toosee řídicím panelu WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="c16bf-232">After you've created some ads while running in hello cloud, you'll view hello WebJobs SDK dashboard toosee hello rich monitoring features it has toooffer.</span></span>

### <a name="deploy-tooweb-apps"></a><span data-ttu-id="c16bf-233">Nasazení aplikací tooWeb</span><span class="sxs-lookup"><span data-stu-id="c16bf-233">Deploy tooWeb Apps</span></span>

1. <span data-ttu-id="c16bf-234">Zavřete prohlížeč hello a okno aplikace hello konzoly.</span><span class="sxs-lookup"><span data-stu-id="c16bf-234">Close hello browser and hello console application window.</span></span>
2. <span data-ttu-id="c16bf-235">Postupujte podle kroků hello v hello [publikování tooAzure s databází SQL](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) části.</span><span class="sxs-lookup"><span data-stu-id="c16bf-235">Follow hello steps in hello [Publish tooAzure with SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) section.</span></span>
3. <span data-ttu-id="c16bf-236">Po dokončení hello kroky pro nasazování pokračujte hello zbývajících úkolech v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c16bf-236">After you complete hello steps for deploying, continue with hello remaining tasks in this article.</span></span>

### <a name="configure-hello-web-app-toouse-your-azure-sql-database-and-storage-account"></a><span data-ttu-id="c16bf-237">Konfigurace hello webové aplikace toouse účtu Azure SQL database a úložiště</span><span class="sxs-lookup"><span data-stu-id="c16bf-237">Configure hello web app toouse your Azure SQL database and storage account</span></span>
<span data-ttu-id="c16bf-238">Nejlepším postupem zabezpečení je příliš[neukládejte citlivé informace, jako je například připojovací řetězce v souborech, které jsou uložené v úložišť zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span><span class="sxs-lookup"><span data-stu-id="c16bf-238">It's a security best practice too[avoid putting sensitive information such as connection strings in files that are stored in source code repositories](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span></span> <span data-ttu-id="c16bf-239">Azure poskytuje toodo způsobem, který: můžete nastavit připojovací řetězec a další nastavení hodnoty v hello prostředí Azure a rozhraní API pro konfiguraci ASP.NET automaticky vyzvedne, až tyto hodnoty při spuštění aplikace hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="c16bf-239">Azure provides a way toodo that: you can set connection string and other setting values in hello Azure environment, and ASP.NET configuration APIs automatically pick up these values when hello app runs in Azure.</span></span> <span data-ttu-id="c16bf-240">Tyto hodnoty lze nastavit v Azure pomocí **Průzkumníka serveru**, hello portálu Azure, prostředí Windows PowerShell nebo hello multiplatformního rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c16bf-240">You can set these values in Azure by using **Server Explorer**, hello Azure Portal, Windows PowerShell, or hello cross-platform command-line interface.</span></span> <span data-ttu-id="c16bf-241">Další informace najdete v tématu [fungování řetězců aplikace a připojovacích řetězců](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="c16bf-241">For more information, see [How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span>

<span data-ttu-id="c16bf-242">V této části můžete použít **Průzkumníka serveru** tooset hodnoty připojovacího řetězce v Azure.</span><span class="sxs-lookup"><span data-stu-id="c16bf-242">In this section, you use **Server Explorer** tooset connection string values in Azure.</span></span>

1. <span data-ttu-id="c16bf-243">V **Průzkumníka serveru**, klikněte pravým tlačítkem na vaší webové aplikace v rámci **Azure > aplikační služby > {vaší skupiny prostředků}**a potom klikněte na **nastavení zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-243">In **Server Explorer**, right-click your web app under **Azure > App Service > {your resource group}**, and then click **View Settings**.</span></span>

    <span data-ttu-id="c16bf-244">Hello **webové aplikace Azure** na hello se otevře okno **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="c16bf-244">hello **Azure Web App** window opens on hello **Configuration** tab.</span></span>
2. <span data-ttu-id="c16bf-245">Název hello změnit název toohello hello objekt DefaultConnection připojovacího řetězce jste zvolili, když jste nakonfigurovali hello SQL database v hello [publikování tooAzure s databází SQL](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) článku.</span><span class="sxs-lookup"><span data-stu-id="c16bf-245">Change hello name of hello DefaultConnection connection string toohello name you chose when you configured hello SQL database in hello [Publish tooAzure with SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) article.</span></span>

    <span data-ttu-id="c16bf-246">Při vytváření hello webové aplikace s přidružené databáze, takže už je hello správný připojovací řetězec hodnotu, Azure automaticky vytvoří tento připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="c16bf-246">Azure automatically created this connection string when you created hello web app with an associated database, so it already has hello right connection string value.</span></span> <span data-ttu-id="c16bf-247">Měníte jenom název toowhat hello, které hledá kódu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-247">You're changing just hello name toowhat your code is looking for.</span></span>
3. <span data-ttu-id="c16bf-248">Přidejte dva nové připojovací řetězce s názvem AzureWebJobsStorage a AzureWebJobsDashboard.</span><span class="sxs-lookup"><span data-stu-id="c16bf-248">Add two new connection strings, named AzureWebJobsStorage and AzureWebJobsDashboard.</span></span> <span data-ttu-id="c16bf-249">Nastavte typ databáze hello příliš**vlastní**a sadu hello připojovací řetězec hodnotu toohello stejnou hodnotu, kterou jste použili předtím pro hello *Web.config* a *App.config* soubory.</span><span class="sxs-lookup"><span data-stu-id="c16bf-249">Set hello database type too**Custom**, and set hello connection string value toohello same value that you used earlier for hello *Web.config* and *App.config* files.</span></span> <span data-ttu-id="c16bf-250">(Nezapomeňte zahrnout hello celý připojovací řetězec, ne jenom hello přístupový klíč a neobsahují hello uvozovky.)</span><span class="sxs-lookup"><span data-stu-id="c16bf-250">(Be sure you include hello entire connection string, not just hello access key, and don't include hello quotation marks.)</span></span>

    <span data-ttu-id="c16bf-251">Tyto připojovací řetězce jsou používány hello WebJobs SDK, jeden pro data aplikací a jeden pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="c16bf-251">These connection strings are used by hello WebJobs SDK, one for application data and one for logging.</span></span> <span data-ttu-id="c16bf-252">Jak už jste viděli dříve, hello, jeden pro data aplikací také používá kód front-endu webové hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-252">As you saw earlier, hello one for application data is also used by hello web front-end code.</span></span>
4. <span data-ttu-id="c16bf-253">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-253">Click **Save**.</span></span>

    ![Připojovací řetězce v portálu Azure](./media/websites-dotnet-webjobs-sdk-get-started/azconnstr.png)
5. <span data-ttu-id="c16bf-255">V **Průzkumníka serveru**, klikněte pravým tlačítkem na hello webové aplikace a pak klikněte na tlačítko **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-255">In **Server Explorer**, right-click hello web app, and then click **Stop**.</span></span>
6. <span data-ttu-id="c16bf-256">Po hello webové aplikace se zastaví, znovu klikněte pravým tlačítkem hello webové aplikace a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-256">After hello web app stops, right-click hello web app again, and then click **Start**.</span></span>

   <span data-ttu-id="c16bf-257">Hello webová úloha se automaticky spustí, když publikujete, ale se zastaví, když provedete změnu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c16bf-257">hello WebJob automatically starts when you publish, but it stops when you make a configuration change.</span></span> <span data-ttu-id="c16bf-258">toorestart, můžete buď restartování webové aplikace hello nebo hello webové úlohy v hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="c16bf-258">toorestart it, you can either restart hello web app or restart hello WebJob in hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="c16bf-259">Po změně konfigurace je obecně doporučené toorestart hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c16bf-259">It's generally recommended toorestart hello web app after a configuration change.</span></span>
7. <span data-ttu-id="c16bf-260">Aktualizujte hello okno prohlížeče, který má adresa URL webové aplikace hello jeho panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="c16bf-260">Refresh hello browser window that has hello web app URL in its address bar.</span></span>

    <span data-ttu-id="c16bf-261">Hello Domovská stránka se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c16bf-261">hello home page appears.</span></span>
8. <span data-ttu-id="c16bf-262">Vytvoření ad, jako jste to udělali při jste [spustili aplikace hello místně](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).</span><span class="sxs-lookup"><span data-stu-id="c16bf-262">Create an ad, as you did when you [ran hello application locally](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).</span></span>

   <span data-ttu-id="c16bf-263">indexovou stránku Hello ukazuje bez miniaturu na první.</span><span class="sxs-lookup"><span data-stu-id="c16bf-263">hello Index page shows without a thumbnail at first.</span></span>
9. <span data-ttu-id="c16bf-264">Aktualizujte stránku hello za několik sekund a hello Miniatura se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c16bf-264">Refresh hello page after a few seconds and hello thumbnail appears.</span></span>

   <span data-ttu-id="c16bf-265">Pokud se nezobrazí miniaturu hello, může být toowait minutu pro webové úlohy toorestart hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-265">If hello thumbnail doesn't appear, you might have toowait a minute or so for hello WebJob toorestart.</span></span> <span data-ttu-id="c16bf-266">Pokud se po chvíli stále nevidíte, nemusí mít hello miniaturu při aktualizaci hello stránky, hello webové úlohy automaticky spustit.</span><span class="sxs-lookup"><span data-stu-id="c16bf-266">If after a while, you still don't see hello thumbnail when you refresh hello page, hello WebJob might not have started automatically.</span></span> <span data-ttu-id="c16bf-267">V takovém případě přejděte toohello **App Services** okno v hello [portál Azure](https://portal.azure.com/), vyhledejte vaší webové aplikace a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-267">In that case, go toohello **App Services** blade in hello [Azure portal](https://portal.azure.com/), locate your web app, and then click **Start**.</span></span>

### <a name="view-hello-webjobs-sdk-dashboard"></a><span data-ttu-id="c16bf-268">Hello zobrazení řídicího panelu WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="c16bf-268">View hello WebJobs SDK dashboard</span></span>
1. <span data-ttu-id="c16bf-269">V hello [portál Azure](https://portal.azure.com/), vyberte hello **okno App Services**, vyhledejte vaší webové aplikace a vyberte **WebJobs**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-269">In hello [Azure portal](https://portal.azure.com/), select hello **App Services blade**, locate your web app, and select **WebJobs**.</span></span>
3. <span data-ttu-id="c16bf-270">Vyberte hello **protokoly** kartě.</span><span class="sxs-lookup"><span data-stu-id="c16bf-270">Select hello **Logs** tab.</span></span>

    ![Karta protokoly](./media/websites-dotnet-webjobs-sdk-get-started/log-tab.png)

    <span data-ttu-id="c16bf-272">Otevře novou kartu prohlížeče toohello řídicím panelu WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="c16bf-272">A new browser tab opens toohello WebJobs SDK dashboard.</span></span> <span data-ttu-id="c16bf-273">Hello řídicího panelu ukazuje, že hello webová úloha běží a zobrazuje seznam funkcí v kódu této hello spuštění WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="c16bf-273">hello dashboard shows that hello WebJob is running and shows a list of functions in your code that hello WebJobs SDK triggered.</span></span>
4. <span data-ttu-id="c16bf-274">Klikněte na jednu z hello funkce toosee podrobnosti o jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="c16bf-274">Click one of hello functions toosee details about its execution.</span></span>

    ![Řídicí panel WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjdashboardhome.png)

    ![Řídicí panel WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjfunctiondetails.png)

    <span data-ttu-id="c16bf-277">Hello **funkce opětovného přehrání** tlačítko na této stránce způsobí, že hello WebJobs SDK framework toocall hello funkce znovu a nabízí funkce prvního toochange hello dat předávaných toohello nejdřív.</span><span class="sxs-lookup"><span data-stu-id="c16bf-277">hello **Replay Function** button on this page causes hello WebJobs SDK framework toocall hello function again, and it gives you a chance toochange hello data passed toohello function first.</span></span>

> [!NOTE]
> <span data-ttu-id="c16bf-278">Po dokončení testování, zvažte odstranění hello webové aplikace, účet úložiště a vaše instance databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="c16bf-278">When you're finished testing, consider deleting hello web app, storage account, and your SQL Database instance.</span></span> <span data-ttu-id="c16bf-279">Webová aplikace Hello je volné, ale hello účet úložiště SQL a instanci databáze nabíhat poplatky (i když, minimální kvůli toohello malá velikost).</span><span class="sxs-lookup"><span data-stu-id="c16bf-279">hello web app is free, but hello SQL storage account and database instance accrue charges (albeit, minimal due toohello small size).</span></span> <span data-ttu-id="c16bf-280">Navíc pokud hello webová aplikace spuštěná, každý, kdo najde vaši adresu URL můžete vytvořit a zobrazovat reklamy.</span><span class="sxs-lookup"><span data-stu-id="c16bf-280">Also, if you leave hello web app running, anyone who finds your URL can create and view ads.</span></span> 
>
>

### <a name="delete-your-web-app"></a><span data-ttu-id="c16bf-281">Odstranění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c16bf-281">Delete your web app</span></span>
<span data-ttu-id="c16bf-282">Hello portálu, přejděte toohello **App Services** okno, najděte a vyberte vaší webové aplikace a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-282">In hello portal, go toohello **App Services** blade, locate and select your web app, and then click **Delete**.</span></span> <span data-ttu-id="c16bf-283">Pokud chcete zabránit tootemporarily ostatním přístup hello webové aplikace, klikněte na tlačítko **Zastavit** místo.</span><span class="sxs-lookup"><span data-stu-id="c16bf-283">If you just want tootemporarily prevent others from accessing hello web app, click **Stop** instead.</span></span> <span data-ttu-id="c16bf-284">V takovém případě budou poplatky dál tooaccrue pro hello databáze SQL a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c16bf-284">In that case, charges will continue tooaccrue for hello SQL Database and Storage account.</span></span>

### <a name="delete-your-storage-account"></a><span data-ttu-id="c16bf-285">Odstranění účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="c16bf-285">Delete your storage account</span></span>
<span data-ttu-id="c16bf-286">toodelete účtu úložiště najdete v části [odstranit účet úložiště](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="c16bf-286">toodelete your storage account, see [Delete a storage account](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account).</span></span> 

### <a name="delete-your-database"></a><span data-ttu-id="c16bf-287">Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="c16bf-287">Delete your database</span></span>
<span data-ttu-id="c16bf-288">toodelete vaše SQL databáze najdete v tématu hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="c16bf-288">toodelete your SQL database, see hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) documentation.</span></span>

## <span data-ttu-id="c16bf-289"><a id="create"></a>Vytvoření aplikace hello od začátku</span><span class="sxs-lookup"><span data-stu-id="c16bf-289"><a id="create"></a>Create hello application from scratch</span></span>
<span data-ttu-id="c16bf-290">V této části, můžete to udělat hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="c16bf-290">In this section you'll do hello following tasks:</span></span>

* <span data-ttu-id="c16bf-291">Vytvoření řešení sady Visual Studio s webového projektu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-291">Create a Visual Studio solution with a web project.</span></span>
* <span data-ttu-id="c16bf-292">Přidání projektu knihovny tříd pro hello data vrstva přístupu, která jsou sdílena mezi hello front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="c16bf-292">Add a class library project for hello data access layer that is shared between hello front end and back end.</span></span>
* <span data-ttu-id="c16bf-293">Přidejte projekt konzolové aplikace pro hello back-end, se webové úlohy nasazení povolené.</span><span class="sxs-lookup"><span data-stu-id="c16bf-293">Add a Console Application project for hello backend, with WebJobs deployment enabled.</span></span>
* <span data-ttu-id="c16bf-294">Přidání balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="c16bf-294">Add NuGet packages.</span></span>
* <span data-ttu-id="c16bf-295">Nastavení odkazů projektu</span><span class="sxs-lookup"><span data-stu-id="c16bf-295">Set project references.</span></span>
* <span data-ttu-id="c16bf-296">Zkopírujte soubory kódu a konfigurační soubory aplikace z aplikace hello stáhli, kterou jste pracovali v předchozí části kurzu hello hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-296">Copy application code and configuration files from hello downloaded application that you worked with in hello previous section of hello tutorial.</span></span>
* <span data-ttu-id="c16bf-297">Projděte si části hello hello kódu, které fungují s Azure BLOB a frontami a hello WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="c16bf-297">Review hello parts of hello code that work with Azure blobs and queues and hello WebJobs SDK.</span></span>

### <a name="create-a-visual-studio-solution-with-a-web-project-and-class-library-project"></a><span data-ttu-id="c16bf-298">Vytvoření řešení Visual Studio s webový projekt a projekt knihovny tříd</span><span class="sxs-lookup"><span data-stu-id="c16bf-298">Create a Visual Studio solution with a web project and class library project</span></span>
1. <span data-ttu-id="c16bf-299">V sadě Visual Studio, vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-299">In Visual Studio, choose **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="c16bf-300">V hello **nový projekt** dialogovém okně, vyberte **Visual C#** > **webové** > **webové aplikace ASP.NET (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-300">In hello **New Project** dialog, choose **Visual C#** > **Web** > **ASP.NET Web Application (.NET Framework)**.</span></span>
3. <span data-ttu-id="c16bf-301">Název hello projektu ContosoAdsWeb, název řešení hello ContosoAdsWebJobsSDK (změnit název řešení hello, pokud jste ji umístíte v hello stejné složce jako hello stáhnout řešení) a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-301">Name hello project ContosoAdsWeb, name hello solution ContosoAdsWebJobsSDK (change hello solution name if you're putting it in hello same folder as hello downloaded solution), and then click **OK**.</span></span>

    ![Nový projekt](./media/websites-dotnet-webjobs-sdk-get-started/newproject.png)
4. <span data-ttu-id="c16bf-303">V hello **nové webové aplikace ASP.NET** dialogovém okně, vyberte šablonu MVC hello a vyberte **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-303">In hello **New ASP.NET Web Application** dialog, choose hello MVC template, and select **Change Authentication**.</span></span>

    ![Změna ověřování](./media/websites-dotnet-webjobs-sdk-get-started/chgauth.png)
5. <span data-ttu-id="c16bf-305">V hello **změna ověřování** dialogovém okně, vyberte **bez ověřování**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-305">In hello **Change Authentication** dialog, choose **No Authentication**, and then click **OK**.</span></span>

    ![Bez ověřování](./media/websites-dotnet-webjobs-sdk-get-started/noauth.png)
6. <span data-ttu-id="c16bf-307">V hello **nové webové aplikace ASP.NET** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-307">In hello **New ASP.NET Web Application** dialog, click **OK**.</span></span>

    <span data-ttu-id="c16bf-308">Visual Studio vytvoří řešení hello a hello webového projektu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-308">Visual Studio creates hello solution and hello web project.</span></span>
7. <span data-ttu-id="c16bf-309">V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení hello (ne hello projekt) a zvolte **přidat** > **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-309">In **Solution Explorer**, right-click hello solution (not hello project), and choose **Add** > **New Project**.</span></span>
8. <span data-ttu-id="c16bf-310">V hello **přidat nový projekt** dialogovém okně, vyberte **Visual C#** > **Windows Classic Desktop** > **třídy knihovny (.NET Framework)** šablony.</span><span class="sxs-lookup"><span data-stu-id="c16bf-310">In hello **Add New Project** dialog, choose **Visual C#** > **Windows Classic Desktop** > **Class Library (.NET Framework)** template.</span></span>  
9. <span data-ttu-id="c16bf-311">Název projektu hello *ContosoAdsCommon*a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-311">Name hello project *ContosoAdsCommon*, and then click **OK**.</span></span>

    <span data-ttu-id="c16bf-312">Tento projekt bude obsahovat hello na kontext Entity Framework a hello datového modelu, které oba hello front-end a back-end bude používat.</span><span class="sxs-lookup"><span data-stu-id="c16bf-312">This project will contain hello Entity Framework context and hello data model which both hello front end and back end will use.</span></span> <span data-ttu-id="c16bf-313">Jako alternativu můžete definovat třídy související s EF hello v hello webový projekt a odkazovat na takový projekt z projektu úlohy WebJob hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-313">As an alternative, you could define hello EF-related classes in hello web project and reference that project from hello WebJob project.</span></span> <span data-ttu-id="c16bf-314">Ale pak projekt webové úlohy by měla mít referenční tooweb sestavení, která nepotřebuje.</span><span class="sxs-lookup"><span data-stu-id="c16bf-314">But, then your WebJob project would have a reference tooweb assemblies, which it doesn't need.</span></span>

### <a name="add-a-console-application-project-that-has-webjobs-deployment-enabled"></a><span data-ttu-id="c16bf-315">Přidání projektu konzolové aplikace, který má povoleno nasazení webové úlohy</span><span class="sxs-lookup"><span data-stu-id="c16bf-315">Add a Console Application project that has WebJobs deployment enabled</span></span>
1. <span data-ttu-id="c16bf-316">Klikněte pravým tlačítkem na projekt webové hello (ne hello řešení nebo projektu knihovny tříd hello) a pak klikněte na **přidat** > **nový projekt webové úlohy Azure**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-316">Right-click hello web project (not hello solution or hello class library project), and then click **Add** > **New Azure WebJob Project**.</span></span>

    ![Nový projekt webové úlohy Azure výběr nabídky](./media/websites-dotnet-webjobs-sdk-get-started/newawjp.png)
2. <span data-ttu-id="c16bf-318">V hello **přidat webové úlohy Azure** dialogové okno, zadejte ContosoAdsWebJob jako obou hello **název projektu** a hello **název webové úlohy**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-318">In hello **Add Azure WebJob** dialog, enter ContosoAdsWebJob as both hello **Project name** and hello **WebJob name**.</span></span> <span data-ttu-id="c16bf-319">Nechte **webové úlohy spustit režim** nastavit příliš**spouštět nepřetržitě**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-319">Leave **WebJob run mode** set too**Run Continuously**.</span></span>
3. <span data-ttu-id="c16bf-320">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-320">Click **OK**.</span></span>

   <span data-ttu-id="c16bf-321">Visual Studio vytvoří konzolovou aplikaci, která je nakonfigurovaná toodeploy jako webová vždy, když je nasazení webového projektu hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-321">Visual Studio creates a Console application that is configured toodeploy as a WebJob whenever you deploy hello web project.</span></span> <span data-ttu-id="c16bf-322">toodo, že ji provést následující úlohy po vytvoření projektu hello hello:</span><span class="sxs-lookup"><span data-stu-id="c16bf-322">toodo that, it performed hello following tasks after creating hello project:</span></span>

   * <span data-ttu-id="c16bf-323">Přidat *webové úlohy publikovat settings.json* soubor ve složce vlastnosti projektu hello webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="c16bf-323">Added a *webjob-publish-settings.json* file in hello WebJob project Properties folder.</span></span>
   * <span data-ttu-id="c16bf-324">Přidat *webjobs list.json* soubor ve složce vlastnosti projektu webové hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-324">Added a *webjobs-list.json* file in hello web project Properties folder.</span></span>
   * <span data-ttu-id="c16bf-325">Hello balíček Microsoft.Web.WebJobs.Publish NuGet nainstalované v projektu úlohy WebJob hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-325">Installed hello Microsoft.Web.WebJobs.Publish NuGet package in hello WebJob project.</span></span>

   <span data-ttu-id="c16bf-326">Další informace o těchto změnách najdete v tématu [jak toodeploy webové úlohy pomocí sady Visual Studio](websites-dotnet-deploy-webjobs.md).</span><span class="sxs-lookup"><span data-stu-id="c16bf-326">For more information about these changes, see [How toodeploy WebJobs by using Visual Studio](websites-dotnet-deploy-webjobs.md).</span></span>

### <a name="add-nuget-packages"></a><span data-ttu-id="c16bf-327">Přidání balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="c16bf-327">Add NuGet packages</span></span>
<span data-ttu-id="c16bf-328">Hello nový projekt šablony pro projekt webové úlohy automaticky nainstaluje balíček NuGet pro webové úlohy SDK hello [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="c16bf-328">hello new-project template for a WebJob project automatically installs hello WebJobs SDK NuGet package [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) and its dependencies.</span></span>

<span data-ttu-id="c16bf-329">Jeden z hello je sada WebJobs SDK závislosti, které se instaluje automaticky v projektu úlohy WebJob hello hello Azure úložiště klienta knihovny (SCL).</span><span class="sxs-lookup"><span data-stu-id="c16bf-329">One of hello WebJobs SDK dependencies that is installed automatically in hello WebJob project is hello Azure Storage Client Library (SCL).</span></span> <span data-ttu-id="c16bf-330">Ale musíte tooadd ho toohello webového projektu toowork s objekty BLOB a frontami.</span><span class="sxs-lookup"><span data-stu-id="c16bf-330">However, you need tooadd it toohello web project toowork with blobs and queues.</span></span>

1. <span data-ttu-id="c16bf-331">Otevřete hello **spravovat balíčky NuGet** dialogové okno pro řešení hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-331">Open hello **Manage NuGet Packages** dialog for hello solution.</span></span>
2. <span data-ttu-id="c16bf-332">V levém podokně hello vyberte **nainstalované balíky**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-332">In hello left pane, select **Installed packages**.</span></span>
3. <span data-ttu-id="c16bf-333">Najde hello *Azure Storage* balíček a potom klikněte na **spravovat**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-333">Find hello *Azure Storage* package, and then click **Manage**.</span></span>
4. <span data-ttu-id="c16bf-334">V hello **vyberte projekty** pole, vyberte hello **ContosoAdsWeb** zaškrtněte políčko a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-334">In hello **Select Projects** box, select hello **ContosoAdsWeb** check box, and then click **OK**.</span></span>

    <span data-ttu-id="c16bf-335">Všechny tři projekty používat hello Entity Framework toowork s daty v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="c16bf-335">All three projects use hello Entity Framework toowork with data in SQL Database.</span></span>
5. <span data-ttu-id="c16bf-336">V levém podokně hello vyberte **Online**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-336">In hello left pane, select **Online**.</span></span>
6. <span data-ttu-id="c16bf-337">Najde hello *EntityFramework* NuGet balíček a nainstalujte ho do všech tří projektů.</span><span class="sxs-lookup"><span data-stu-id="c16bf-337">Find hello *EntityFramework* NuGet package, and install it in all three projects.</span></span>

### <a name="set-project-references"></a><span data-ttu-id="c16bf-338">Nastavení odkazů na projekty</span><span class="sxs-lookup"><span data-stu-id="c16bf-338">Set project references</span></span>
<span data-ttu-id="c16bf-339">Web a webové úlohy projekty pracovat databáze SQL hello, takže i musí být v projektu ContosoAdsCommon toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="c16bf-339">Both web and WebJob projects work with hello SQL database, so both need a reference toohello ContosoAdsCommon project.</span></span>

1. <span data-ttu-id="c16bf-340">V hello projektu ContosoAdsWeb nastavte odkaz na projekt ContosoAdsCommon toohello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-340">In hello ContosoAdsWeb project, set a reference toohello ContosoAdsCommon project.</span></span> <span data-ttu-id="c16bf-341">(Klikněte pravým tlačítkem na projekt ContosoAdsWeb hello a pak klikněte na **přidat** > **odkaz**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-341">(Right-click hello ContosoAdsWeb project, and then click **Add** > **Reference**.</span></span> 
2. <span data-ttu-id="c16bf-342">V hello **správce odkazů** dialogové okno, vyberte **projekty** > **řešení** > **ContosoAdsCommon**a potom klikněte na **OK**.)</span><span class="sxs-lookup"><span data-stu-id="c16bf-342">In hello **Reference Manager** dialog box, select **Projects** > **Solution** > **ContosoAdsCommon**, and then click **OK**.)</span></span>
   
    <span data-ttu-id="c16bf-343">projekt webové úlohy Hello potřebuje odkazy pro práci s obrázky a pro přístup k připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="c16bf-343">hello WebJob project needs references for working with images and for accessing connection strings.</span></span>

4. <span data-ttu-id="c16bf-344">V projektu ContosoAdsWebJob hello nastavte odkaz příliš`System.Drawing` a `System.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="c16bf-344">In hello ContosoAdsWebJob project, set a reference too`System.Drawing` and `System.Configuration`.</span></span>

### <a name="add-code-and-configuration-files"></a><span data-ttu-id="c16bf-345">Přidání kódu a konfiguračních souborů</span><span class="sxs-lookup"><span data-stu-id="c16bf-345">Add code and configuration files</span></span>
<span data-ttu-id="c16bf-346">V tomto kurzu nezobrazuje jak příliš[vytvořit kontrolery a zobrazení pomocí generování uživatelského rozhraní MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), jak příliš[napsat kód Entity Framework, který funguje s databází serveru SQL Server](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), nebo [hello základy asynchronní programování v technologii ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span><span class="sxs-lookup"><span data-stu-id="c16bf-346">This tutorial does not show how too[create MVC controllers and views using scaffolding](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), how too[write Entity Framework code that works with SQL Server databases](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), or [hello basics of asynchronous programming in ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span></span> <span data-ttu-id="c16bf-347">Všechno, co, takže je zůstane toodo kopírovat soubory kódu a konfigurace z řešení hello stáhli do nové řešení hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-347">So, all that remains toodo is copy code and configuration files from hello downloaded solution into hello new solution.</span></span> <span data-ttu-id="c16bf-348">Poté, co můžete udělat, hello následující části zobrazit a vysvětlí klíčová místa hello kódu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-348">After you do that, hello following sections show and explain key parts of hello code.</span></span>

<span data-ttu-id="c16bf-349">tooadd soubory tooa projektu nebo složky, klikněte pravým tlačítkem na projekt hello nebo složku a klikněte na tlačítko **přidat** > **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-349">tooadd files tooa project or a folder, right-click hello project or folder and click **Add** > **Existing Item**.</span></span> <span data-ttu-id="c16bf-350">Vyberte hello souborů, které jste chcete a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-350">Select hello files you want and click **Add**.</span></span> <span data-ttu-id="c16bf-351">Pokud se zobrazí dotaz, zda chcete, aby tooreplace existující soubory, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="c16bf-351">If asked whether you want tooreplace existing files, click **Yes**.</span></span>

1. <span data-ttu-id="c16bf-352">V projektu ContosoAdsCommon hello odstranit hello *Class1.cs* souboru a přidejte do jeho místní hello následující soubory z projektu hello stáhli.</span><span class="sxs-lookup"><span data-stu-id="c16bf-352">In hello ContosoAdsCommon project, delete hello *Class1.cs* file and add in its place hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="c16bf-353">*AD.cs*</span><span class="sxs-lookup"><span data-stu-id="c16bf-353">*Ad.cs*</span></span>
   * <span data-ttu-id="c16bf-354">*ContosoAdscontext.cs*</span><span class="sxs-lookup"><span data-stu-id="c16bf-354">*ContosoAdscontext.cs*</span></span>
   * <span data-ttu-id="c16bf-355">*BlobInformation.cs*</span><span class="sxs-lookup"><span data-stu-id="c16bf-355">*BlobInformation.cs*</span></span>
2. <span data-ttu-id="c16bf-356">V projektu ContosoAdsWeb hello přidejte následující soubory z projektu hello stáhli hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-356">In hello ContosoAdsWeb project, add hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="c16bf-357">*Soubor Web.config*</span><span class="sxs-lookup"><span data-stu-id="c16bf-357">*Web.config*</span></span>
   * <span data-ttu-id="c16bf-358">*Global.asax.cs*</span><span class="sxs-lookup"><span data-stu-id="c16bf-358">*Global.asax.cs*</span></span>  
   * <span data-ttu-id="c16bf-359">V hello *řadiče* složky: *AdController.cs*</span><span class="sxs-lookup"><span data-stu-id="c16bf-359">In hello *Controllers* folder: *AdController.cs*</span></span>
   * <span data-ttu-id="c16bf-360">V hello *Views\Shared* složky: *_Layout.cshtml* souboru</span><span class="sxs-lookup"><span data-stu-id="c16bf-360">In hello *Views\Shared* folder: *_Layout.cshtml* file</span></span>
   * <span data-ttu-id="c16bf-361">V hello *Views\Home* složky: *Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c16bf-361">In hello *Views\Home* folder: *Index.cshtml*</span></span>
   * <span data-ttu-id="c16bf-362">V hello *Views\Ad* složky (nejprve vytvořte složku hello): pět *.cshtml* soubory</span><span class="sxs-lookup"><span data-stu-id="c16bf-362">In hello *Views\Ad* folder (create hello folder first): five *.cshtml* files</span></span>
3. <span data-ttu-id="c16bf-363">V projektu ContosoAdsWebJob hello přidejte následující soubory z projektu hello stáhli hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-363">In hello ContosoAdsWebJob project, add hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="c16bf-364">*App.config* (změnit filtr typu souboru hello příliš**všechny soubory**)</span><span class="sxs-lookup"><span data-stu-id="c16bf-364">*App.config* (change hello file type filter too**All Files**)</span></span>
   * <span data-ttu-id="c16bf-365">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="c16bf-365">*Program.cs*</span></span>
   * <span data-ttu-id="c16bf-366">*Functions.cs*</span><span class="sxs-lookup"><span data-stu-id="c16bf-366">*Functions.cs*</span></span>

<span data-ttu-id="c16bf-367">Teď můžete sestavit, spuštění a nasazení aplikace hello podle pokynů v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-367">You can now build, run, and deploy hello application as instructed earlier in hello tutorial.</span></span> <span data-ttu-id="c16bf-368">Předtím, než můžete udělat, ale hello webové úlohy, který je stále spuštěná v hello první webové aplikace, které jste nasadili na zastavte.</span><span class="sxs-lookup"><span data-stu-id="c16bf-368">Before you do that, however, stop hello WebJob that is still running in hello first web app you deployed to.</span></span> <span data-ttu-id="c16bf-369">Jinak bude této webové úlohy zpracování zprávy fronty vytvoří místně nebo hello aplikace spuštěné v nové webové aplikace, protože všechny používá hello stejný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c16bf-369">Otherwise, that WebJob will process queue messages created locally or by hello app running in a new web app, since all are using hello same storage account.</span></span>

## <span data-ttu-id="c16bf-370"><a id="code"></a>Zkontrolujte kód aplikace hello</span><span class="sxs-lookup"><span data-stu-id="c16bf-370"><a id="code"></a>Review hello application code</span></span>
<span data-ttu-id="c16bf-371">Hello následující části popisují kód hello související tooworking s hello WebJobs SDK a objektů BLOB služby Azure Storage a fronty.</span><span class="sxs-lookup"><span data-stu-id="c16bf-371">hello following sections explain hello code related tooworking with hello WebJobs SDK and Azure Storage blobs and queues.</span></span>

> [!NOTE]
> <span data-ttu-id="c16bf-372">Hello kód konkrétní toohello WebJobs SDK, najdete toohello [Program.cs a Functions.cs](#programcs) oddíly.</span><span class="sxs-lookup"><span data-stu-id="c16bf-372">For hello code specific toohello WebJobs SDK, go toohello [Program.cs and Functions.cs](#programcs) sections.</span></span>
>
>

### <a name="contosoadscommon---adcs"></a><span data-ttu-id="c16bf-373">ContosoAdsCommon – Ad.cs</span><span class="sxs-lookup"><span data-stu-id="c16bf-373">ContosoAdsCommon - Ad.cs</span></span>
<span data-ttu-id="c16bf-374">Hello soubor Ad.cs definuje výčet kategorií reklam a třídu entity objektů POCO pro informace o reklamách.</span><span class="sxs-lookup"><span data-stu-id="c16bf-374">hello Ad.cs file defines an enum for ad categories and a POCO entity class for ad information.</span></span>

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

### <a name="contosoadscommon---contosoadscontextcs"></a><span data-ttu-id="c16bf-375">ContosoAdsCommon – ContosoAdsContext.cs</span><span class="sxs-lookup"><span data-stu-id="c16bf-375">ContosoAdsCommon - ContosoAdsContext.cs</span></span>
<span data-ttu-id="c16bf-376">Hello třída ContosoAdsContext Určuje, zda text hello Ad třída se používá v kolekci DbSet, kterou Entity Framework jsou uloženy v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="c16bf-376">hello ContosoAdsContext class specifies that hello Ad class is used in a DbSet collection, which Entity Framework stores in a SQL database.</span></span>

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

<span data-ttu-id="c16bf-377">Hello třída má dva konstruktory.</span><span class="sxs-lookup"><span data-stu-id="c16bf-377">hello class has two constructors.</span></span> <span data-ttu-id="c16bf-378">Hello nejprve je používané hello webovým projektem a určuje hello název připojovacího řetězce, který je uložený v souboru Web.config hello nebo hello Azure běhového prostředí.</span><span class="sxs-lookup"><span data-stu-id="c16bf-378">hello first is used by hello web project, and specifies hello name of a connection string that is stored in hello Web.config file or hello Azure runtime environment.</span></span> <span data-ttu-id="c16bf-379">Hello druhý konstruktor vám umožňuje toopass v hello samotný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="c16bf-379">hello second constructor enables you toopass in hello actual connection string.</span></span> <span data-ttu-id="c16bf-380">To vyžaduje projekt webové úlohy hello protože sám nemá soubor Web.config.</span><span class="sxs-lookup"><span data-stu-id="c16bf-380">That is needed by hello WebJob project since it doesn't have a Web.config file.</span></span> <span data-ttu-id="c16bf-381">Už jste viděli dříve tento připojovací řetězec uložil, kde uvidíte později jak hello kód načte hello připojovací řetězec při vytvoření instance třídy DbContext hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-381">You saw earlier where this connection string was stored, and you'll see later how hello code retrieves hello connection string when it instantiates hello DbContext class.</span></span>

### <a name="contosoadscommon---blobinformationcs"></a><span data-ttu-id="c16bf-382">ContosoAdsCommon – BlobInformation.cs</span><span class="sxs-lookup"><span data-stu-id="c16bf-382">ContosoAdsCommon - BlobInformation.cs</span></span>
<span data-ttu-id="c16bf-383">Hello `BlobInformation` třída je použité toostore informace o objektu blob bitové kopie do zprávy fronty.</span><span class="sxs-lookup"><span data-stu-id="c16bf-383">hello `BlobInformation` class is used toostore information about an image blob in a queue message.</span></span>

        public class BlobInformation
        {
            public Uri BlobUri { get; set; }

            public string BlobName
            {
                get
                {
                    return BlobUri.Segments[BlobUri.Segments.Length - 1];
                }
            }
            public string BlobNameWithoutExtension
            {
                get
                {
                    return Path.GetFileNameWithoutExtension(BlobName);
                }
            }
            public int AdId { get; set; }
        }


### <a name="contosoadsweb---globalasaxcs"></a><span data-ttu-id="c16bf-384">ContosoAdsWeb – Global.asax.cs</span><span class="sxs-lookup"><span data-stu-id="c16bf-384">ContosoAdsWeb - Global.asax.cs</span></span>
<span data-ttu-id="c16bf-385">Kód, který je volána z hello `Application_Start` metoda vytvoří *bitové kopie* kontejner objektů blob a *bitové kopie* fronty, pokud ještě neexistují.</span><span class="sxs-lookup"><span data-stu-id="c16bf-385">Code that is called from hello `Application_Start` method creates an *images* blob container and an *images* queue if they don't already exist.</span></span> <span data-ttu-id="c16bf-386">Tím se zajistí, že při každém spuštění pomocí nového účtu úložiště hello požadované, automaticky vytvoří kontejner objektů blob a fronty.</span><span class="sxs-lookup"><span data-stu-id="c16bf-386">This ensures that whenever you start using a new storage account, hello required blob container and queue are created automatically.</span></span>

<span data-ttu-id="c16bf-387">Hello kód získá přístup k účtu úložiště pro toohello pomocí připojovacího řetězce úložiště hello z hello *Web.config* soubor nebo Azure běhového prostředí.</span><span class="sxs-lookup"><span data-stu-id="c16bf-387">hello code gets access toohello storage account by using hello storage connection string from hello *Web.config* file or Azure runtime environment.</span></span>

        var storageAccount = CloudStorageAccount.Parse
            (ConfigurationManager.ConnectionStrings["AzureWebJobsStorage"].ToString());

<span data-ttu-id="c16bf-388">Potom získá odkaz na toohello *bitové kopie* kontejner objektů blob, vytvoří kontejner hello, pokud ještě neexistuje, a nastaví přístupová oprávnění na nový kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-388">Then, it gets a reference toohello *images* blob container, creates hello container if it doesn't already exist, and sets access permissions on hello new container.</span></span> <span data-ttu-id="c16bf-389">Ve výchozím nastavení povolí nové kontejnery jenom pro klienty s úložiště objektů BLOB tooaccess přihlašovací údaje účtu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-389">By default, new containers allow only clients with storage account credentials tooaccess blobs.</span></span> <span data-ttu-id="c16bf-390">Hello webové aplikace musí hello toobe veřejné objekty BLOB tak, aby se může zobrazit obrázky pomocí adres URL objekty BLOB tento bod toohello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c16bf-390">hello web app needs hello blobs toobe public so that it can display images using URLs that point toohello image blobs.</span></span>

        var blobClient = storageAccount.CreateCloudBlobClient();
        var imagesBlobContainer = blobClient.GetContainerReference("images");
        if (imagesBlobContainer.CreateIfNotExists())
        {
            imagesBlobContainer.SetPermissions(
                new BlobContainerPermissions
                {
                    PublicAccess = BlobContainerPublicAccessType.Blob
                });
        }

<span data-ttu-id="c16bf-391">Podobný kód získá odkaz na toohello *thumbnailrequest* fronty a vytvoří novou frontu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-391">Similar code gets a reference toohello *thumbnailrequest* queue and creates a new queue.</span></span> <span data-ttu-id="c16bf-392">V tomto případě není nutná žádná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c16bf-392">In this case no permissions change is needed.</span></span>

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("thumbnailrequest");
        imagesQueue.CreateIfNotExists();

### <a name="contosoadsweb---layoutcshtml"></a><span data-ttu-id="c16bf-393">ContosoAdsWeb – _Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="c16bf-393">ContosoAdsWeb - _Layout.cshtml</span></span>
<span data-ttu-id="c16bf-394">Hello *_Layout.cshtml* soubor nastaví název aplikace hello v hello záhlaví a zápatí a vytvoří položku nabídky "Reklamy".</span><span class="sxs-lookup"><span data-stu-id="c16bf-394">hello *_Layout.cshtml* file sets hello app name in hello header and footer, and creates an "Ads" menu entry.</span></span>

### <a name="contosoadsweb---viewshomeindexcshtml"></a><span data-ttu-id="c16bf-395">ContosoAdsWeb – Views\Home\Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c16bf-395">ContosoAdsWeb - Views\Home\Index.cshtml</span></span>
<span data-ttu-id="c16bf-396">Hello *Views\Home\Index.cshtml* souboru zobrazuje kategorie odkazy na domovskou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-396">hello *Views\Home\Index.cshtml* file displays category links on hello home page.</span></span> <span data-ttu-id="c16bf-397">Hello odkazy předají celočíselnou hodnotu hello hello `Category` výčtu na stránce služby Active Directory Index toohello proměnné řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-397">hello links pass hello integer value of hello `Category` enum in a querystring variable toohello Ads Index page.</span></span>

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### <a name="contosoadsweb---adcontrollercs"></a><span data-ttu-id="c16bf-398">ContosoAdsWeb – AdController.cs</span><span class="sxs-lookup"><span data-stu-id="c16bf-398">ContosoAdsWeb - AdController.cs</span></span>
<span data-ttu-id="c16bf-399">V hello *AdController.cs* soubor, hello konstruktor volání hello `InitializeStorage` metoda toocreate Klientská knihovna pro úložiště Azure objekty, které poskytují rozhraní API pro práci s objekty BLOB a frontami.</span><span class="sxs-lookup"><span data-stu-id="c16bf-399">In hello *AdController.cs* file, hello constructor calls hello `InitializeStorage` method toocreate Azure Storage Client Library objects that provide an API for working with blobs and queues.</span></span>

<span data-ttu-id="c16bf-400">Potom hello kód získá odkaz na toohello *bitové kopie* kontejner objektů blob, jako jste viděli dříve v *Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="c16bf-400">Then, hello code gets a reference toohello *images* blob container as you saw earlier in *Global.asax.cs*.</span></span> <span data-ttu-id="c16bf-401">Během toho, nastaví výchozí [zásady opakování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) vhodné pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c16bf-401">While doing that, it sets a default [retry policy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) appropriate for a web app.</span></span> <span data-ttu-id="c16bf-402">zásady opakování exponenciálního omezení rychlosti výchozí Hello může přečnívat hello webové aplikace po dobu delší než minutu při opakovaných pokusech přechodná chyba.</span><span class="sxs-lookup"><span data-stu-id="c16bf-402">hello default exponential backoff retry policy could hang hello web app for longer than a minute on repeated retries for a transient fault.</span></span> <span data-ttu-id="c16bf-403">Tady určené zásady opakování Hello čeká tří sekund po každém pokusu až toothree pokusů.</span><span class="sxs-lookup"><span data-stu-id="c16bf-403">hello retry policy specified here waits three seconds after each try for up toothree tries.</span></span>

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

<span data-ttu-id="c16bf-404">Podobný kód získá odkaz na toohello *bitové kopie* fronty.</span><span class="sxs-lookup"><span data-stu-id="c16bf-404">Similar code gets a reference toohello *images* queue.</span></span>

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("blobnamerequest");

<span data-ttu-id="c16bf-405">Většinu kódu kontroleru hello je typická pro práci s datovým modelem Entity Framework použití třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="c16bf-405">Most of hello controller code is typical for working with an Entity Framework data model using a DbContext class.</span></span> <span data-ttu-id="c16bf-406">Výjimkou je hello HttpPost `Create` metodu, která soubor odešle a uloží ho do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c16bf-406">An exception is hello HttpPost `Create` method, which uploads a file and saves it in blob storage.</span></span> <span data-ttu-id="c16bf-407">poskytuje vazač modelu Hello [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) objektu toohello metoda.</span><span class="sxs-lookup"><span data-stu-id="c16bf-407">hello model binder provides an [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) object toohello method.</span></span>

        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create(
            [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
            HttpPostedFileBase imageFile)

<span data-ttu-id="c16bf-408">Pokud hello uživatel vybral soubor tooupload, kód hello odešle soubor hello, uloží ho do objektu blob a aktualizuje hello záznam v databázi reklam pomocí adresy URL, který odkazuje objekt blob toohello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-408">If hello user selected a file tooupload, hello code uploads hello file, saves it in a blob, and updates hello Ad database record with a URL that points toohello blob.</span></span>

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            blob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = blob.Uri.ToString();
        }

<span data-ttu-id="c16bf-409">Hello kód, který hello odesílání je v hello `UploadAndSaveBlobAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="c16bf-409">hello code that does hello upload is in hello `UploadAndSaveBlobAsync` method.</span></span> <span data-ttu-id="c16bf-410">Vytvoří identifikátor GUID pro objekt blob hello, nahrávání a uloží hello soubor a vrátí objekt blob uložit toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="c16bf-410">It creates a GUID name for hello blob, uploads and saves hello file, and returns a reference toohello saved blob.</span></span>

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

<span data-ttu-id="c16bf-411">Po hello HttpPost `Create` metoda odešle objekt blob a aktualizace hello databázi, vytvoří frontu zpráv tooinform hello back-end proces bitová kopie je připravená pro převod tooa miniaturu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-411">After hello HttpPost `Create` method uploads a blob and updates hello database, it creates a queue message tooinform hello back-end process that an image is ready for conversion tooa thumbnail.</span></span>

        BlobInformation blobInfo = new BlobInformation() { AdId = ad.AdId, BlobUri = new Uri(ad.ImageURL) };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        await thumbnailRequestQueue.AddMessageAsync(queueMessage);

<span data-ttu-id="c16bf-412">Hello kód pro hello HttpPost `Edit` metoda je podobné, s tím rozdílem, že pokud hello uživatel vybere nový obrázkový soubor, musíte odstranit všechny objekty BLOB, které jsou už pro tento ad.</span><span class="sxs-lookup"><span data-stu-id="c16bf-412">hello code for hello HttpPost `Edit` method is similar, except that if hello user selects a new image file, any blobs that already exist for this ad must be deleted.</span></span>

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            await DeleteAdBlobsAsync(ad);
            imageBlob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = imageBlob.Uri.ToString();
        }

<span data-ttu-id="c16bf-413">Tady je hello kód, který odstraní objekty BLOB při odstranění ad:</span><span class="sxs-lookup"><span data-stu-id="c16bf-413">Here is hello code that deletes blobs when you delete an ad:</span></span>

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

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a><span data-ttu-id="c16bf-414">ContosoAdsWeb – Views\Ad\Index.cshtml a Details.cshtml</span><span class="sxs-lookup"><span data-stu-id="c16bf-414">ContosoAdsWeb - Views\Ad\Index.cshtml and Details.cshtml</span></span>
<span data-ttu-id="c16bf-415">Hello *Index.cshtml* souboru zobrazí miniatury s hello dalšími daty reklam:</span><span class="sxs-lookup"><span data-stu-id="c16bf-415">hello *Index.cshtml* file displays thumbnails with hello other ad data:</span></span>

        <img  src="@Html.Raw(item.ThumbnailURL)" />

<span data-ttu-id="c16bf-416">Hello *Details.cshtml* zobrazí obrázek plné velikosti hello:</span><span class="sxs-lookup"><span data-stu-id="c16bf-416">hello *Details.cshtml* file displays hello full-size image:</span></span>

        <img src="@Html.Raw(Model.ImageURL)" />

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a><span data-ttu-id="c16bf-417">ContosoAdsWeb – Views\Ad\Create.cshtml a Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="c16bf-417">ContosoAdsWeb - Views\Ad\Create.cshtml and Edit.cshtml</span></span>
<span data-ttu-id="c16bf-418">Hello *Create.cshtml* a *Edit.cshtml* soubory zadejte formuláře kódování, že umožňuje hello řadiče tooget hello `HttpPostedFileBase` objektu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-418">hello *Create.cshtml* and *Edit.cshtml* files specify form encoding that enables hello controller tooget hello `HttpPostedFileBase` object.</span></span>

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

<span data-ttu-id="c16bf-419">`<input>` Element informuje hello prohlížeče tooprovide dialogové okno pro výběr souboru.</span><span class="sxs-lookup"><span data-stu-id="c16bf-419">An `<input>` element tells hello browser tooprovide a file selection dialog.</span></span>

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### <span data-ttu-id="c16bf-420"><a id="programcs"></a>ContosoAdsWebJob - Program.cs</span><span class="sxs-lookup"><span data-stu-id="c16bf-420"><a id="programcs"></a>ContosoAdsWebJob - Program.cs</span></span>
<span data-ttu-id="c16bf-421">Když hello webové úlohy se spustí, hello `Main` volání metod hello WebJobs SDK `JobHost.RunAndBlock` metoda toobegin provádění spouštěná funkcí na aktuální vlákno hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-421">When hello WebJob starts, hello `Main` method calls hello WebJobs SDK `JobHost.RunAndBlock` method toobegin execution of triggered functions on hello current thread.</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

### <span data-ttu-id="c16bf-422"><a id="generatethumbnail"></a>Metoda GenerateThumbnail ContosoAdsWebJob - Functions.cs-</span><span class="sxs-lookup"><span data-stu-id="c16bf-422"><a id="generatethumbnail"></a>ContosoAdsWebJob - Functions.cs - GenerateThumbnail method</span></span>
<span data-ttu-id="c16bf-423">Hello WebJobs SDK volá tuto metodu, když je obdržena zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="c16bf-423">hello WebJobs SDK calls this method when a queue message is received.</span></span> <span data-ttu-id="c16bf-424">Metoda Hello vytvoří miniaturu a PUT hello miniaturu adresy URL v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-424">hello method creates a thumbnail and puts hello thumbnail URL in hello database.</span></span>

        public static void GenerateThumbnail(
        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,
        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)
        {
            using (Stream output = outputBlob.OpenWrite())
            {
                ConvertImageToThumbnailJPG(input, output);
                outputBlob.Properties.ContentType = "image/jpeg";
            }

            // Entity Framework context class is not thread-safe, so it must
            // be instantiated and disposed within hello function.
            using (ContosoAdsContext db = new ContosoAdsContext())
            {
                var id = blobInfo.AdId;
                Ad ad = db.Ads.Find(id);
                if (ad == null)
                {
                    throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", id.ToString()));
                }
                ad.ThumbnailURL = outputBlob.Uri.ToString();
                db.SaveChanges();
            }
        }

* <span data-ttu-id="c16bf-425">Hello `QueueTrigger` atribut přesměruje hello WebJobs SDK toocall tato metoda při přijetí nové zprávy ve frontě thumbnailrequest hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-425">hello `QueueTrigger` attribute directs hello WebJobs SDK toocall this method when a new message is received on hello thumbnailrequest queue.</span></span>

        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,

    <span data-ttu-id="c16bf-426">Hello `BlobInformation` objekt v uvítací zprávu fronty je automaticky deserializovat do hello `blobInfo` parametr.</span><span class="sxs-lookup"><span data-stu-id="c16bf-426">hello `BlobInformation` object in hello queue message is automatically deserialized into hello `blobInfo` parameter.</span></span> <span data-ttu-id="c16bf-427">Po dokončení hello metoda odstraní zprávu fronty hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-427">When hello method completes, hello queue message is deleted.</span></span> <span data-ttu-id="c16bf-428">V případě selhání hello metoda před dokončením hello fronty zpráv se neodstraní; Po vypršení platnosti zapůjčení 10 minut, je uvítací zprávu vydaná toobe zachyceny znovu a zpracovat.</span><span class="sxs-lookup"><span data-stu-id="c16bf-428">If hello method fails before completing, hello queue message is not deleted; after a 10-minute lease expires, hello message is released toobe picked up again and processed.</span></span> <span data-ttu-id="c16bf-429">Toto pořadí nebude opakuje bez omezení, pokud zprávu vždy dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="c16bf-429">This sequence won't be repeated indefinitely if a message always causes an exception.</span></span> <span data-ttu-id="c16bf-430">Po 5 neúspěšných pokusech tooprocess zprávu, zpráva hello je přesunutý tooa frontu s názvem {queuename}-poškozených.</span><span class="sxs-lookup"><span data-stu-id="c16bf-430">After 5 unsuccessful attempts tooprocess a message, hello message is moved tooa queue named {queuename}-poison.</span></span> <span data-ttu-id="c16bf-431">maximální počet pokusů o Hello je možné konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="c16bf-431">hello maximum number of attempts is configurable.</span></span>
* <span data-ttu-id="c16bf-432">dva Hello `Blob` atributy poskytují objekty, které jsou vázané tooblobs: jeden toohello existující objekt blob image a jeden tooa nové miniatur objekt blob, který vytvoří metoda hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-432">hello two `Blob` attributes provide objects that are bound tooblobs: one toohello existing image blob and one tooa new thumbnail blob that hello method creates.</span></span>

        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)

    <span data-ttu-id="c16bf-433">Názvy objektů BLOB pocházet z vlastnosti hello `BlobInformation` objektu došlo k uvítací zprávu fronty (`BlobName` a `BlobNameWithoutExtension`).</span><span class="sxs-lookup"><span data-stu-id="c16bf-433">Blob names come from properties of hello `BlobInformation` object received in hello queue message (`BlobName` and `BlobNameWithoutExtension`).</span></span> <span data-ttu-id="c16bf-434">tooget hello plné funkčnosti hello Klientská knihovna pro úložiště, můžete použít hello `CloudBlockBlob` třída toowork s objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="c16bf-434">tooget hello full functionality of hello Storage Client Library, you can use hello `CloudBlockBlob` class toowork with blobs.</span></span> <span data-ttu-id="c16bf-435">Pokud chcete, aby tooreuse kódu, která byla zapsána toowork s `Stream` objekty, můžete použít hello `Stream` třídy.</span><span class="sxs-lookup"><span data-stu-id="c16bf-435">If you want tooreuse code that was written toowork with `Stream` objects, you can use hello `Stream` class.</span></span>

<span data-ttu-id="c16bf-436">Další informace o tom, jak toowrite funkce, které používají atributy WebJobs SDK najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="c16bf-436">For more information about how toowrite functions that use  WebJobs SDK attributes, see hello following resources:</span></span>

* [<span data-ttu-id="c16bf-437">Jak toouse Azure fronty úložiště s hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="c16bf-437">How toouse Azure queue storage with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [<span data-ttu-id="c16bf-438">Jak toouse Azure blob storage s hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="c16bf-438">How toouse Azure blob storage with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [<span data-ttu-id="c16bf-439">Jak toouse Azure table storage s hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="c16bf-439">How toouse Azure table storage with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [<span data-ttu-id="c16bf-440">Jak toouse služba Azure Service Bus s hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="c16bf-440">How toouse Azure Service Bus with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-service-bus.md)

> [!NOTE]
> * <span data-ttu-id="c16bf-441">Pokud vaše webová aplikace běží na víc virtuálních počítačů, více webové úlohy budou používat současně a v některých případech může být výsledkem hello stejné dat zpracovává více než jednou.</span><span class="sxs-lookup"><span data-stu-id="c16bf-441">If your web app runs on multiple VMs, multiple WebJobs will be running simultaneously, and in some scenarios this can result in hello same data getting processed multiple times.</span></span> <span data-ttu-id="c16bf-442">To není problém, pokud používáte integrované fronty hello, objektů blob a aktivační události služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c16bf-442">This is not a problem if you use hello built-in queue, blob, and Service Bus triggers.</span></span> <span data-ttu-id="c16bf-443">Hello SDK zajistí, že funkce budou zpracovány pouze jednou pro každou zprávu nebo objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c16bf-443">hello SDK ensures that your functions will be processed only once for each message or blob.</span></span>
> * <span data-ttu-id="c16bf-444">Informace o řádné vypnutí tooimplement, najdete v části [řádné vypnutí](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).</span><span class="sxs-lookup"><span data-stu-id="c16bf-444">For information about how tooimplement graceful shutdown, see [Graceful Shutdown](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).</span></span>
> * <span data-ttu-id="c16bf-445">Hello kód v hello `ConvertImageToThumbnailJPG` – metoda (není vidět) používá třídy v hello `System.Drawing` obor názvů pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="c16bf-445">hello code in hello `ConvertImageToThumbnailJPG` method (not shown) uses classes in hello `System.Drawing` namespace for simplicity.</span></span> <span data-ttu-id="c16bf-446">Hello třídy v tomto oboru názvů však byly navrženy pro používání s formuláři Windows.</span><span class="sxs-lookup"><span data-stu-id="c16bf-446">However, hello classes in this namespace were designed for use with Windows Forms.</span></span> <span data-ttu-id="c16bf-447">Jejich používání není podporované ve Windows a službě ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c16bf-447">They are not supported for use in a Windows or ASP.NET service.</span></span> <span data-ttu-id="c16bf-448">Další informace o možnostech zpracování obrázků najdete v článcích o [dynamickém generování obrázků](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) a o [hloubkové změně velikosti uvnitř obrázků](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span><span class="sxs-lookup"><span data-stu-id="c16bf-448">For more information about image processing options, see [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) and [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="c16bf-449">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c16bf-449">Next steps</span></span>
<span data-ttu-id="c16bf-450">V tomto kurzu jste se seznámili jednoduché vícevrstvé aplikace, která používá hello WebJobs SDK pro zpracování back-end.</span><span class="sxs-lookup"><span data-stu-id="c16bf-450">In this tutorial, you've seen a simple multi-tier application that uses hello WebJobs SDK for backend processing.</span></span> <span data-ttu-id="c16bf-451">Tato část nabízí některé návrhy pro dozvědět více o vícevrstvých aplikací ASP.NET a webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="c16bf-451">This section offers some suggestions for learning more about ASP.NET multi-tier applications and WebJobs.</span></span>

### <a name="missing-features"></a><span data-ttu-id="c16bf-452">Chybějící funkce</span><span class="sxs-lookup"><span data-stu-id="c16bf-452">Missing features</span></span>
<span data-ttu-id="c16bf-453">aplikace Hello má jednoduché kurz – Začínáme.</span><span class="sxs-lookup"><span data-stu-id="c16bf-453">hello application has been kept simple for a getting-started tutorial.</span></span> <span data-ttu-id="c16bf-454">V reálné aplikaci byste implementovat [vkládání závislostí](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) a hello [úložiště a jednotky pracovních vzorů](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), použijte [rozhraní pro protokolování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), použijte [ Migrace Code First EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data modelu změny a použít [odolnost připojení EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage přechodných síťových chyb.</span><span class="sxs-lookup"><span data-stu-id="c16bf-454">In a real-world application you would implement [dependency injection](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) and hello [repository and unit of work patterns](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), use [an interface for logging](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), use [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data model changes, and use [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage transient network errors.</span></span>

### <a name="scaling-webjobs"></a><span data-ttu-id="c16bf-455">Škálování webové úlohy</span><span class="sxs-lookup"><span data-stu-id="c16bf-455">Scaling WebJobs</span></span>
<span data-ttu-id="c16bf-456">Webové úlohy spustit v kontextu hello webové aplikace a nejsou škálovatelné samostatně.</span><span class="sxs-lookup"><span data-stu-id="c16bf-456">WebJobs run in hello context of a web app and are not scalable separately.</span></span> <span data-ttu-id="c16bf-457">Například pokud máte jednu instanci standardní webové aplikace, máte jenom jednu instanci spuštěn proces na pozadí a používá některé prostředky serveru hello (procesoru, paměti, atd.), které by jinak byly k dispozici tooserve webového obsahu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-457">For example, if you have one Standard web app instance, you have only one instance of your background process running, and it is using some of hello server resources (CPU, memory, etc.) that otherwise would be available tooserve web content.</span></span>

<span data-ttu-id="c16bf-458">Pokud provoz se liší podle času den nebo den v týdnu a back-end hello zpracování, je nutné počkat toodo, vaše webové úlohy toorun může naplánovat na dobu s nízkým provozem.</span><span class="sxs-lookup"><span data-stu-id="c16bf-458">If traffic varies by time of day or day of week, and if hello backend processing you need toodo can wait, you could schedule your WebJobs toorun at low-traffic times.</span></span> <span data-ttu-id="c16bf-459">Pokud stále hello zatížení příliš vysoká. pro toto řešení, můžete spustit back-end hello jako webová úloha v samostatné webové aplikace, který je vyhrazený pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="c16bf-459">If hello load is still too high for that solution, you can run hello backend as a WebJob in a separate web app dedicated for that purpose.</span></span> <span data-ttu-id="c16bf-460">Webové aplikace back-end je pak možné škálovat nezávisle z vaší webové aplikace front-endu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-460">You can then scale your backend web app independently from your frontend web app.</span></span>

<span data-ttu-id="c16bf-461">Další informace najdete v tématu [škálování WebJobs](websites-webjobs-resources.md#scale).</span><span class="sxs-lookup"><span data-stu-id="c16bf-461">For more information, see [Scaling WebJobs](websites-webjobs-resources.md#scale).</span></span>

### <a name="avoiding-web-app-timeout-shut-downs"></a><span data-ttu-id="c16bf-462">Webové aplikace časový limit vypnutí seznamy vyloučení</span><span class="sxs-lookup"><span data-stu-id="c16bf-462">Avoiding web app timeout shut-downs</span></span>
<span data-ttu-id="c16bf-463">toomake, že vaše webové úlohy jsou vždy spuštěn, a spuštěná na všech instancích vaší webové aplikace, budete mít tooenable hello [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) funkce.</span><span class="sxs-lookup"><span data-stu-id="c16bf-463">toomake sure your WebJobs are always running, and running on all instances of your web app, you have tooenable hello [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) feature.</span></span>

### <a name="using-hello-webjobs-sdk-outside-of-webjobs"></a><span data-ttu-id="c16bf-464">Pomocí hello WebJobs SDK mimo webové úlohy</span><span class="sxs-lookup"><span data-stu-id="c16bf-464">Using hello WebJobs SDK outside of WebJobs</span></span>
<span data-ttu-id="c16bf-465">Program, který používá hello WebJobs SDK nemá toorun v Azure v webovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="c16bf-465">A program that uses hello WebJobs SDK doesn't have toorun in Azure in a WebJob.</span></span> <span data-ttu-id="c16bf-466">Můžete spustit místně a také může být spuštěn v jiných prostředích, jako je například roli pracovního procesu cloudové služby nebo služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c16bf-466">It can run locally, and it can also run in other environments such as a Cloud Service worker role or a Windows service.</span></span> <span data-ttu-id="c16bf-467">Hello řídicím panelu WebJobs SDK však můžete pouze přistupovat prostřednictvím webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="c16bf-467">However, you can only access hello WebJobs SDK dashboard through an Azure web app.</span></span> <span data-ttu-id="c16bf-468">řídicí panel hello toouse máte tooconnect hello webové aplikace toohello účtu úložiště používáte nastavením hello AzureWebJobsDashboard připojovací řetězec na hello **konfigurace** kartě portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="c16bf-468">toouse hello dashboard you have tooconnect hello web app toohello storage account you're using by setting hello AzureWebJobsDashboard connection string on hello **Configure** tab of hello classic portal.</span></span> <span data-ttu-id="c16bf-469">Potom můžete získat toohello řídicího panelu pomocí hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="c16bf-469">Then, you can get toohello Dashboard by using hello following URL:</span></span>

<span data-ttu-id="c16bf-470">https://{webappname}.SCM.azurewebsites.NET/azurejobs/#/functions</span><span class="sxs-lookup"><span data-stu-id="c16bf-470">https://{webappname}.scm.azurewebsites.net/azurejobs/#/functions</span></span>

<span data-ttu-id="c16bf-471">Další informace najdete v tématu [získávání řídicí panel pro místní vývoj s hello WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ale Všimněte si, že se zobrazí původní název připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="c16bf-471">For more information, see [Getting a dashboard for local development with hello WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), but note that it shows an old connection string name.</span></span>

### <a name="more-webjobs-documentation"></a><span data-ttu-id="c16bf-472">Další dokumentaci webové úlohy</span><span class="sxs-lookup"><span data-stu-id="c16bf-472">More WebJobs documentation</span></span>
<span data-ttu-id="c16bf-473">Další informace najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="c16bf-473">For more information, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span>
