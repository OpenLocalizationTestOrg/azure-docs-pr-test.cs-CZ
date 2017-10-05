---
title: "Vytvořit webovou úlohu .NET v Azure App Service | Microsoft Docs"
description: "Vytvořte vícevrstvou aplikaci pomocí ASP.NET MVC a Azure. Front-endu spustí ve webové aplikaci ve službě Azure App Service a back end běží jako webová. Aplikace používá rozhraní Entity Framework, SQL Database a fronty Azure storage a objekty BLOB."
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
ms.openlocfilehash: a20b13058caecff75af14957468f20e63a3325c9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-net-webjob-in-azure-app-service"></a><span data-ttu-id="e1b16-105">Vytvoření webové úlohy .NET ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e1b16-105">Create a .NET WebJob in Azure App Service</span></span>
<span data-ttu-id="e1b16-106">Tento kurz ukazuje, jak napsat kód pro jednoduchou aplikaci ASP.NET MVC 5 několika vrstvami, která používá [WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="e1b16-106">This tutorial shows how to write code for a simple multi-tier ASP.NET MVC 5 application that uses the [WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="e1b16-107">Účelem [WebJobs SDK](websites-webjobs-resources.md) je můžete zjednodušit kód napsaný pro běžné úkoly, které může provádět webovou úlohu, například zpracování obrázků, zpracování fronty, RSS agregace, údržba souborů a odesílání e-mailů.</span><span class="sxs-lookup"><span data-stu-id="e1b16-107">The purpose of the [WebJobs SDK](websites-webjobs-resources.md) is to simplify the code you write for common tasks that a WebJob can perform, such as image processing, queue processing, RSS aggregation, file maintenance, and sending emails.</span></span> <span data-ttu-id="e1b16-108">Sada SDK webové úlohy obsahuje integrované funkce pro práci s Azure Storage a Service Bus, plánování úloh a zpracování chyb a pro jiné běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="e1b16-108">The WebJobs SDK has built-in features for working with Azure Storage and Service Bus, for scheduling tasks and handling errors, and for many other common scenarios.</span></span> <span data-ttu-id="e1b16-109">Kromě toho je určený pro rozšiřitelnost a dojde [otevřete zdrojové úložiště pro rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span><span class="sxs-lookup"><span data-stu-id="e1b16-109">In addition, it's designed to be extensible, and there's an [open source repository for extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span></span>

<span data-ttu-id="e1b16-110">Vzorová aplikace je vývěsní Tabule pro inzerci.</span><span class="sxs-lookup"><span data-stu-id="e1b16-110">The sample application is an advertising bulletin board.</span></span> <span data-ttu-id="e1b16-111">Uživatelé mohou odesílat obrázky pro reklamy a proces back-end převede obrázky miniatur.</span><span class="sxs-lookup"><span data-stu-id="e1b16-111">Users can upload images for ads, and a backend process converts the images to thumbnails.</span></span> <span data-ttu-id="e1b16-112">Stránka seznamu ad zobrazuje miniatur a stránce s podrobnostmi o ad zobrazí obrázek v plné velikosti.</span><span class="sxs-lookup"><span data-stu-id="e1b16-112">The ad list page shows the thumbnails, and the ad details page shows the full-size image.</span></span> <span data-ttu-id="e1b16-113">Zde je snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e1b16-113">Here's a screenshot:</span></span>

![Seznam reklam](./media/websites-dotnet-webjobs-sdk-get-started/list.png)

<span data-ttu-id="e1b16-115">Tato ukázková aplikace funguje s [Azure fronty](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) a [objektů Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage).</span><span class="sxs-lookup"><span data-stu-id="e1b16-115">This sample application works with [Azure queues](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) and [Azure blobs](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage).</span></span> <span data-ttu-id="e1b16-116">Tento kurz ukazuje, jak nasadit aplikaci do [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).</span><span class="sxs-lookup"><span data-stu-id="e1b16-116">The tutorial shows how to deploy the application to [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).</span></span>

## <span data-ttu-id="e1b16-117"><a id="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="e1b16-117"><a id="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="e1b16-118">Kurz předpokládá, že víte, jak pracovat s [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projekty v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e1b16-118">The tutorial assumes that you know how to work with [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projects in Visual Studio.</span></span>

<span data-ttu-id="e1b16-119">Tento kurz byl původně zapsán pro Visual Studio 2013, ale lze použít s novější verzí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e1b16-119">The tutorial was originally written for Visual Studio 2013, but can be used with later versions of Visual Studio.</span></span> <span data-ttu-id="e1b16-120">Pokud používáte Visual Studio 2015 nebo 2017, poznamenejte si, že předtím, než spustíte aplikaci místně, je nutné změnit `Data Source` součástí připojovací řetězec SQL serveru LocalDB v souborech Web.config a App.config z `Data Source=(localdb)\v11.0` k `Data Source=(LocalDb)\MSSQLLocalDB`.</span><span class="sxs-lookup"><span data-stu-id="e1b16-120">If you are using Visual Studio 2015 or 2017, make note that before you run the application locally, you must change the `Data Source` part of the SQL Server LocalDB connection string in the Web.config and App.config files from `Data Source=(localdb)\v11.0` to `Data Source=(LocalDb)\MSSQLLocalDB`.</span></span>

> [!NOTE]
> <span data-ttu-id="e1b16-121"><a name="note"></a>Musíte mít účet Azure k dokončení tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="e1b16-121"><a name="note"></a>You must have an Azure account to complete this tutorial:</span></span>
>
> * <span data-ttu-id="e1b16-122">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): získáte kredity, můžete použít k vyzkoušení placených služeb Azure, a až se vyčerpáte, můžete účet ponechat a používat bezplatné služby Azure, jako jsou weby.</span><span class="sxs-lookup"><span data-stu-id="e1b16-122">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): You get credits that you can use to try out paid Azure services, and even after they're used up, you can keep the account and use free Azure services, such as Websites.</span></span> <span data-ttu-id="e1b16-123">Nikdy vám nebudeme účtovat žádné poplatky, pokud si sami nezměníte nastavení a nezačnete používat placené služby.</span><span class="sxs-lookup"><span data-stu-id="e1b16-123">Your credit card will never be charged, unless you explicitly change your settings and ask to be charged.</span></span>
> * <span data-ttu-id="e1b16-124">Můžete [aktivovat platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): vaše předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b16-124">You can [activate Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Your subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="e1b16-125">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1b16-125">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="e1b16-126">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="e1b16-126">No credit cards required; no commitments.</span></span>
>
>

## <span data-ttu-id="e1b16-127"><a id="learn"></a>Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e1b16-127"><a id="learn"></a>What you'll learn</span></span>
<span data-ttu-id="e1b16-128">Tento kurz ukazuje, jak provést následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="e1b16-128">The tutorial shows how to do the following tasks:</span></span>

* <span data-ttu-id="e1b16-129">Povolte počítači pro vývoj pro Azure tak, že instalace sady Azure SDK (pouze pro Visual Studio 2013 a uživatelé 2015).</span><span class="sxs-lookup"><span data-stu-id="e1b16-129">Enable your machine for Azure development by installing the Azure SDK (only for Visual Studio 2013 and 2015 users).</span></span>
* <span data-ttu-id="e1b16-130">Vytvořte projekt konzolové aplikace, který automaticky nasadí jako webová úloha Azure při nasazení přidružené webového projektu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-130">Create a Console Application project that automatically deploys as an Azure WebJob when you deploy the associated web project.</span></span>
* <span data-ttu-id="e1b16-131">Testování back-end WebJobs SDK na vývojovém počítači místně.</span><span class="sxs-lookup"><span data-stu-id="e1b16-131">Test a WebJobs SDK backend locally on the development computer.</span></span>
* <span data-ttu-id="e1b16-132">Publikování aplikace pomocí backendu webové úlohy do webové aplikace ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="e1b16-132">Publish an application with a WebJobs backend to a web app in App Service.</span></span>
* <span data-ttu-id="e1b16-133">Odeslání souborů a jejich uložení ve službě Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="e1b16-133">Upload files and store them in the Azure Blob service.</span></span>
* <span data-ttu-id="e1b16-134">Práce s Azure Storage fronty a objekty BLOB pomocí Azure WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="e1b16-134">Use the Azure WebJobs SDK to work with Azure Storage queues and blobs.</span></span>

## <span data-ttu-id="e1b16-135"><a id="contosoads"></a>Architektura aplikace</span><span class="sxs-lookup"><span data-stu-id="e1b16-135"><a id="contosoads"></a>Application architecture</span></span>
<span data-ttu-id="e1b16-136">Ukázková aplikace používá [fronty způsob práce zaměřený](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) k vyvážila práci náročná na prostředky procesoru při vytváření miniatur procesu back-end.</span><span class="sxs-lookup"><span data-stu-id="e1b16-136">The sample application uses the [queue-centric work pattern](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) to off-load the CPU-intensive work of creating thumbnails to a backend process.</span></span>

<span data-ttu-id="e1b16-137">Aplikace ukládá reklamy do databáze SQL a k vytváření tabulky a přístupu k datům používá Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="e1b16-137">The app stores ads in a SQL database, using Entity Framework Code First to create the tables and access the data.</span></span> <span data-ttu-id="e1b16-138">U každé reklamy databáze ukládá dvě adresy URL: jednu pro obrázek v plné velikosti a druhou pro miniaturu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-138">For each ad, the database stores two URLs: one for the full-size image and one for the thumbnail.</span></span>

![Tabulka reklam](./media/websites-dotnet-webjobs-sdk-get-started/adtable.png)

<span data-ttu-id="e1b16-140">Když uživatel odešle obrázek, webové aplikace obrázek uloží [objektů blob v Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), a ukládá informace o reklamě do databáze s adresa URL, která odkazuje na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="e1b16-140">When a user uploads an image, the web app stores the image in an [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and it stores the ad information in the database with a URL that points to the blob.</span></span> <span data-ttu-id="e1b16-141">Ve stejný okamžik zapíše zprávu do fronty Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b16-141">At the same time, it writes a message to an Azure queue.</span></span> <span data-ttu-id="e1b16-142">V back-end proces, který běží jako webová úloha služby Azure dotazuje WebJobs SDK fronty pro nové zprávy.</span><span class="sxs-lookup"><span data-stu-id="e1b16-142">In a backend process running as an Azure WebJob, the WebJobs SDK polls the queue for new messages.</span></span> <span data-ttu-id="e1b16-143">Když se objeví nová zpráva, vytvářené webové úlohy vytvoří miniaturu obrázku a aktualizuje databázi pole miniatur adresa URL pro tuto reklamu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-143">When a new message appears, the WebJob creates a thumbnail for that image and updates the thumbnail URL database field for that ad.</span></span> <span data-ttu-id="e1b16-144">Zde je diagram, který znázorňuje interakci částí aplikace:</span><span class="sxs-lookup"><span data-stu-id="e1b16-144">Here's a diagram that shows how the parts of the application interact:</span></span>

![Architektura Contoso Ads](./media/websites-dotnet-webjobs-sdk-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]  
<span data-ttu-id="e1b16-146">Pokyny kurzu platí pro Azure SDK pro .NET 2.7.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e1b16-146">The tutorial instructions apply to Azure SDK for .NET 2.7.1 or later.</span></span>

## <span data-ttu-id="e1b16-147"><a id="storage"></a>Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="e1b16-147"><a id="storage"></a>Create an Azure Storage account</span></span>
<span data-ttu-id="e1b16-148">Účet úložiště Azure poskytuje prostředky pro ukládání dat front a objektů blob v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-148">An Azure storage account provides resources for storing queue and blob data in the cloud.</span></span> <span data-ttu-id="e1b16-149">Také se používá WebJobs SDK k ukládání dat protokolování pro řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="e1b16-149">It's also used by the WebJobs SDK to store logging data for the dashboard.</span></span>

<span data-ttu-id="e1b16-150">V reálné aplikaci obvykle vytvoříte samostatné účty pro data aplikací a pro data protokolování a samostatné účty pro testovací data a pro produkční data.</span><span class="sxs-lookup"><span data-stu-id="e1b16-150">In a real-world application, you typically create separate accounts for application data versus logging data and separate accounts for test data versus production data.</span></span> <span data-ttu-id="e1b16-151">V tomto kurzu budete používat jenom jeden účet.</span><span class="sxs-lookup"><span data-stu-id="e1b16-151">For this tutorial, you'll use just one account.</span></span>

1. <span data-ttu-id="e1b16-152">Otevřete **Průzkumníka serveru** oken v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e1b16-152">Open the **Server Explorer** window in Visual Studio.</span></span>
2. <span data-ttu-id="e1b16-153">Klikněte pravým tlačítkem myši **Azure** uzel a pak klikněte na tlačítko **připojit k předplatnému Microsoft Azure...** .</span><span class="sxs-lookup"><span data-stu-id="e1b16-153">Right-click the **Azure** node, and then click **Connect to Microsoft Azure Subscription...**.</span></span>
   
   ![Připojení k Azure](./media/websites-dotnet-webjobs-sdk-get-started/connaz.png)

3. <span data-ttu-id="e1b16-155">Přihlaste se pomocí přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b16-155">Sign in using your Azure credentials.</span></span>
4. <span data-ttu-id="e1b16-156">Klikněte pravým tlačítkem na **úložiště** pod Azure uzel a pak klikněte na tlačítko **vytvořit účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-156">Right-click **Storage** under the Azure node, and then click **Create Storage Account**.</span></span>
   
   ![Vytvořit účet úložiště](./media/websites-dotnet-webjobs-sdk-get-started/createstor.png)
   
5. <span data-ttu-id="e1b16-158">V **vytvořit účet úložiště** dialogové okno, zadejte název pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-158">In the **Create Storage Account** dialog, enter a name for the storage account.</span></span>

    <span data-ttu-id="e1b16-159">Název musí být musí být jedinečné (žádný jiný účet úložiště Azure můžete mít stejný název).</span><span class="sxs-lookup"><span data-stu-id="e1b16-159">The name must be must be unique (no other Azure storage account can have the same name).</span></span> <span data-ttu-id="e1b16-160">Pokud název, který zadáte, se už používá, získáte ho změnit.</span><span class="sxs-lookup"><span data-stu-id="e1b16-160">If the name you enter is already in use, you'll get a chance to change it.</span></span>

    <span data-ttu-id="e1b16-161">Adresa URL pro přístup k účtu úložiště bude *{name}*. core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="e1b16-161">The URL to access your storage account will be *{name}*.core.windows.net.</span></span>
6. <span data-ttu-id="e1b16-162">Nastavte **oblast nebo skupinu vztahů** rozevíracího seznamu pro danou oblast nejblíže k vám.</span><span class="sxs-lookup"><span data-stu-id="e1b16-162">Set the **Region or Affinity Group** drop-down list to the region closest to you.</span></span>

    <span data-ttu-id="e1b16-163">Toto nastavení určuje, které datové centrum Azure bude hostitelem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-163">This setting specifies which Azure datacenter will host your storage account.</span></span> <span data-ttu-id="e1b16-164">V tomto kurzu nebude Zkontrolujte své volby významné rozdíly.</span><span class="sxs-lookup"><span data-stu-id="e1b16-164">For this tutorial, your choice won't make a noticeable difference.</span></span> <span data-ttu-id="e1b16-165">Ale pro produkční webové aplikace, budete chtít váš webový server a účet úložiště ve stejné oblasti k minimalizaci latence a data s nimi spojeným nákladům.</span><span class="sxs-lookup"><span data-stu-id="e1b16-165">However, for a production web app, you want your web server and your storage account to be in the same region to minimize latency and data egress charges.</span></span> <span data-ttu-id="e1b16-166">Webové aplikace (aplikaci, kterou vytvoříte později) datacenter by měl být co nejblíže do prohlížečů, aby se minimalizoval latence přístupu k webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1b16-166">The web app (which you'll create later) datacenter should be as close as possible to the browsers accessing the web app in order to minimize latency.</span></span>
7. <span data-ttu-id="e1b16-167">V rozevíracím seznamu **Replikace** vyberte **Místně redundantní**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-167">Set the **Replication** drop-down list to **Locally redundant**.</span></span>

    <span data-ttu-id="e1b16-168">Když má účet úložiště povolenou geografickou replikaci, bude se uložený obsah replikovat do sekundárního datacentra, které zajistí převzetí služeb při selhání v případě významnější havárie v primárním umístění.</span><span class="sxs-lookup"><span data-stu-id="e1b16-168">When geo-replication is enabled for a storage account, the stored content is replicated to a secondary datacenter to enable failover to that location in case of a major disaster in the primary location.</span></span> <span data-ttu-id="e1b16-169">Geografická replikace může způsobit dodatečné náklady.</span><span class="sxs-lookup"><span data-stu-id="e1b16-169">Geo-replication can incur additional costs.</span></span> <span data-ttu-id="e1b16-170">V případě testovacích a vývojových účtů je zbytečné za geografickou replikaci platit.</span><span class="sxs-lookup"><span data-stu-id="e1b16-170">For test and development accounts, you generally don't want to pay for geo-replication.</span></span> <span data-ttu-id="e1b16-171">Další informace naleznete v článku o [vytvoření, správě nebo odstranění účtu úložiště](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="e1b16-171">For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).</span></span>
8. <span data-ttu-id="e1b16-172">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-172">Click **Create**.</span></span>

    ![Nový účet úložiště](./media/websites-dotnet-webjobs-sdk-get-started/newstorage.png)

## <span data-ttu-id="e1b16-174"><a id="download"></a>Stažení aplikace</span><span class="sxs-lookup"><span data-stu-id="e1b16-174"><a id="download"></a>Download the application</span></span>
1. <span data-ttu-id="e1b16-175">Stáhněte a rozbalte [dokončené řešení](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).</span><span class="sxs-lookup"><span data-stu-id="e1b16-175">Download and unzip the [completed solution](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).</span></span>
2. <span data-ttu-id="e1b16-176">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e1b16-176">Start Visual Studio.</span></span>
3. <span data-ttu-id="e1b16-177">Z **soubor** nabídce zvolte **otevřít > projekt nebo řešení**, přejděte na kterém jste řešení stáhli a pak otevřete soubor řešení.</span><span class="sxs-lookup"><span data-stu-id="e1b16-177">From the **File** menu choose **Open > Project/Solution**, navigate to where you downloaded the solution, and then open the solution file.</span></span>
4. <span data-ttu-id="e1b16-178">Stisknutím kláves CTRL+SHIFT+B řešení sestavíte.</span><span class="sxs-lookup"><span data-stu-id="e1b16-178">Press CTRL+SHIFT+B to build the solution.</span></span>

    <span data-ttu-id="e1b16-179">Visual Studio ve výchozím nastavení automaticky obnoví obsah balíčku NuGet, který nebyl součástí souboru *.zip*.</span><span class="sxs-lookup"><span data-stu-id="e1b16-179">By default, Visual Studio automatically restores the NuGet package content, which was not included in the *.zip* file.</span></span> <span data-ttu-id="e1b16-180">Pokud se balíčky neobnoví, nainstalujte je ručně tak, že přejdete do **spravovat balíčky NuGet pro řešení** dialogové okno a kliknutím na **obnovení** tlačítko v pravé horní.</span><span class="sxs-lookup"><span data-stu-id="e1b16-180">If the packages don't restore, install them manually by going to the **Manage NuGet Packages for Solution** dialog and clicking the **Restore** button at the top right.</span></span>
5. <span data-ttu-id="e1b16-181">V **Průzkumníku řešení**, ujistěte se, že **ContosoAdsWeb** je vybraná jako spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="e1b16-181">In **Solution Explorer**, make sure that **ContosoAdsWeb** is selected as the startup project.</span></span>

## <span data-ttu-id="e1b16-182"><a id="configurestorage"></a>Konfigurace aplikace pro použití účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="e1b16-182"><a id="configurestorage"></a>Configure the application to use your storage account</span></span>
1. <span data-ttu-id="e1b16-183">Otevřete aplikaci *Web.config* souboru v projektu ContosoAdsWeb.</span><span class="sxs-lookup"><span data-stu-id="e1b16-183">Open the application *Web.config* file in the ContosoAdsWeb project.</span></span>

    <span data-ttu-id="e1b16-184">Tento soubor obsahuje připojovací řetězec SQL a úložiště Azure připojovací řetězec pro práci s objekty BLOB a frontami.</span><span class="sxs-lookup"><span data-stu-id="e1b16-184">The file contains a SQL connection string and an Azure storage connection string for working with blobs and queues.</span></span>

    <span data-ttu-id="e1b16-185">Připojovací řetězec SQL odkazuje na [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databáze.</span><span class="sxs-lookup"><span data-stu-id="e1b16-185">The SQL connection string points to a [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

    <span data-ttu-id="e1b16-186">Připojovací řetězec úložiště je příklad, který má zástupné symboly pro klíč název a přístup k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-186">The storage connection string is an example that has placeholders for the storage account name and access key.</span></span> <span data-ttu-id="e1b16-187">Nahraďte ho budete s připojovacím řetězcem, který má název a klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-187">You'll replace this with a connection string that has the name and key of your storage account.</span></span>  

    ```
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
    </connectionStrings>
    ```
    <span data-ttu-id="e1b16-188">Připojovací řetězec úložiště je pojmenovaná AzureWebJobsStorage vzhledem k tomu, že se jedná o název, který využívá sadu WebJobs SDK ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e1b16-188">The storage connection string is named AzureWebJobsStorage because that's the name the WebJobs SDK uses by default.</span></span> <span data-ttu-id="e1b16-189">Se stejným názvem se tady je použita, takže budete muset nastavit pouze jednu hodnotu řetězce připojení v prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b16-189">The same name is used here so you have to set only one connection string value in the Azure environment.</span></span>
2. <span data-ttu-id="e1b16-190">V **Průzkumníka serveru**, klikněte pravým tlačítkem na účtu úložiště v rámci **úložiště** uzel a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-190">In **Server Explorer**, right-click your storage account under the **Storage** node, and then click **Properties**.</span></span>

    ![Klikněte na tlačítko Vlastnosti účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/storppty.png)
3. <span data-ttu-id="e1b16-192">V **vlastnosti** okně klikněte na tlačítko **klíče účtu úložiště**a pak klikněte na tlačítko se třemi tečkami.</span><span class="sxs-lookup"><span data-stu-id="e1b16-192">In the **Properties** window, click **Storage Account Keys**, and then click the ellipsis.</span></span>

    ![Klíče účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/stor-account-keys.png)
4. <span data-ttu-id="e1b16-194">Kopírování **připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-194">Copy the **Connection String**.</span></span>

    ![Dialogové okno klíče účtu úložiště](./media/websites-dotnet-webjobs-sdk-get-started/cpak.png)
5. <span data-ttu-id="e1b16-196">Nahraďte připojovací řetězec úložiště *Web.config* souboru připojovacím řetězcem, který jste právě zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="e1b16-196">Replace the storage connection string in the *Web.config* file with the connection string you just copied.</span></span> <span data-ttu-id="e1b16-197">Ujistěte se, že všechno, co vyberete v uvozovkách, ale ne včetně uvozovek před vkládání.</span><span class="sxs-lookup"><span data-stu-id="e1b16-197">Make sure you select everything inside the quotation marks but not including the quotation marks before pasting.</span></span>
6. <span data-ttu-id="e1b16-198">Otevřete *App.config* v ContosoAdsWebJob projektu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-198">Open the *App.config* file in the ContosoAdsWebJob project.</span></span>

    <span data-ttu-id="e1b16-199">Tento soubor má dva řetězce připojení úložiště, jeden pro data aplikací a jeden pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="e1b16-199">This file has two storage connection strings, one for application data and one for logging.</span></span> <span data-ttu-id="e1b16-200">Můžete použít samostatné úložiště účty pro data aplikací a protokolování, a můžete použít [účtů více úložiště pro data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="e1b16-200">You can use separate storage accounts for application data and logging, and you can use [multiple storage accounts for data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span> <span data-ttu-id="e1b16-201">V tomto kurzu budete používat účet jednoho úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-201">For this tutorial, you'll use a single storage account.</span></span> <span data-ttu-id="e1b16-202">Připojovací řetězce mají zástupné symboly pro klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-202">The connection strings have placeholders for the storage account keys.</span></span>

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

    <span data-ttu-id="e1b16-203">Ve výchozím nastavení vyhledá připojovací řetězce s názvem AzureWebJobsStorage a AzureWebJobsDashboard WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="e1b16-203">By default, the WebJobs SDK looks for connection strings named AzureWebJobsStorage and AzureWebJobsDashboard.</span></span> <span data-ttu-id="e1b16-204">Jako alternativu můžete [úložiště připojovací řetězec, ale chcete a předejte ji v explicitně na `JobHost` objekt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).</span><span class="sxs-lookup"><span data-stu-id="e1b16-204">As an alternative, you can [store the connection string however you want and pass it in explicitly to the `JobHost` object](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).</span></span>
7. <span data-ttu-id="e1b16-205">Nahraďte oba připojovací řetězce úložiště připojovací řetězec, který jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="e1b16-205">Replace both storage connection strings with the connection string you copied earlier.</span></span>
8. <span data-ttu-id="e1b16-206">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="e1b16-206">Save your changes.</span></span>

## <span data-ttu-id="e1b16-207"><a id="run"></a>Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e1b16-207"><a id="run"></a>Run the application locally</span></span>
1. <span data-ttu-id="e1b16-208">Front-endu webové aplikace, stiskněte CTRL + F5.</span><span class="sxs-lookup"><span data-stu-id="e1b16-208">To start the web frontend of the application, press CTRL+F5.</span></span>

    <span data-ttu-id="e1b16-209">Výchozí prohlížeč otevře domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="e1b16-209">The default browser opens to the home page.</span></span> <span data-ttu-id="e1b16-210">(Webový projekt běží vzhledem k tomu, že jste ji provedli projekt po spuštění).</span><span class="sxs-lookup"><span data-stu-id="e1b16-210">(The web project runs because you've made it the startup project.)</span></span>

    ![Domovská stránka Contoso Ads](./media/websites-dotnet-webjobs-sdk-get-started/home.png)
2. <span data-ttu-id="e1b16-212">Pokud chcete spustit úlohu back-end aplikace, klikněte pravým tlačítkem na projekt ContosoAdsWebJob v **Průzkumníku řešení**a potom klikněte na **ladění** > **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-212">To start the WebJob backend of the application, right-click the ContosoAdsWebJob project in **Solution Explorer**, and then click **Debug** > **Start new instance**.</span></span>

    <span data-ttu-id="e1b16-213">Okno aplikace konzoly se zobrazí protokolování zpráv oznamující, že objekt WebJobs SDK JobHost bylo zahájeno ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="e1b16-213">A console application window opens and displays logging messages indicating the WebJobs SDK JobHost object has started to run.</span></span>

    ![Okno konzoly aplikace zobrazuje, zda je spuštěna back-end](./media/websites-dotnet-webjobs-sdk-get-started/backendrunning.png)
3. <span data-ttu-id="e1b16-215">V prohlížeči, klikněte na tlačítko **vytvořit reklamu**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-215">In your browser, click  **Create an Ad**.</span></span>
4. <span data-ttu-id="e1b16-216">Zadejte nějaká testovací data, vyberte bitovou kopii a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-216">Enter some test data, select an image to upload, and then click **Create**.</span></span>

    ![Vytvoření stránky](./media/websites-dotnet-webjobs-sdk-get-started/create.png)

    <span data-ttu-id="e1b16-218">Aplikace přejde na indexovou stránku, ale nezobrazí miniaturu nové reklamy, protože toto zpracování ještě neproběhlo.</span><span class="sxs-lookup"><span data-stu-id="e1b16-218">The app goes to the Index page, but it doesn't show a thumbnail for the new ad because that processing hasn't happened yet.</span></span>

    <span data-ttu-id="e1b16-219">Mezitím po krátké čekání protokolování zpráv v okně konzoly aplikace ukazuje, že fronty zpráv byla přijata a byla zpracována.</span><span class="sxs-lookup"><span data-stu-id="e1b16-219">Meanwhile, after a short wait a logging message in the console application window shows that a queue message was received and has been processed.</span></span>

    ![Okno konzoly aplikace zobrazující, že po zpracování zprávy fronty](./media/websites-dotnet-webjobs-sdk-get-started/backendlogs.png)
5. <span data-ttu-id="e1b16-221">Po najdete v části protokolování zprávy v okně konzoly aplikace, aktualizujte indexovou stránku Miniatura se teď zobrazí.</span><span class="sxs-lookup"><span data-stu-id="e1b16-221">After you see the logging messages in the console application window, refresh the Index page to see the thumbnail.</span></span>

    ![Indexová stránka](./media/websites-dotnet-webjobs-sdk-get-started/list.png)
6. <span data-ttu-id="e1b16-223">Pokud chcete obrázek reklamy zobrazit v plné velikosti, klikněte na **Podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-223">Click **Details** for your ad to see the full-size image.</span></span>

    ![Stránka podrobností](./media/websites-dotnet-webjobs-sdk-get-started/details.png)

<span data-ttu-id="e1b16-225">Aplikaci jste byl spuštěn v místním počítači a používá SQL Server databáze nachází na váš počítač, ale funguje v případě front a objektů BLOB v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-225">You've been running the application on your local computer, and it's using a SQL Server database located on your computer, but it's working with queues and blobs in the cloud.</span></span> <span data-ttu-id="e1b16-226">V následující části spustíte aplikaci v cloudu, používání cloudové databáze a také objekty BLOB cloudové a front.</span><span class="sxs-lookup"><span data-stu-id="e1b16-226">In the following section you'll run the application in the cloud, using a cloud database as well as cloud blobs and queues.</span></span>  

## <span data-ttu-id="e1b16-227"><a id="runincloud"></a>Spusťte aplikaci v cloudu</span><span class="sxs-lookup"><span data-stu-id="e1b16-227"><a id="runincloud"></a>Run the application in the cloud</span></span>
<span data-ttu-id="e1b16-228">Pokud chcete aplikaci spustit v cloudu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e1b16-228">You'll do the following steps to run the application in the cloud:</span></span>

* <span data-ttu-id="e1b16-229">Nasazení do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1b16-229">Deploy to Web Apps.</span></span> <span data-ttu-id="e1b16-230">Visual Studio automaticky vytvoří nové webové aplikace v App Service a instanci databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="e1b16-230">Visual Studio automatically creates a new web app in App Service and a SQL Database instance.</span></span>
* <span data-ttu-id="e1b16-231">Konfigurovat webovou aplikaci k používání účtu Azure SQL database a úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-231">Configure the web app to use your Azure SQL database and storage account.</span></span>

<span data-ttu-id="e1b16-232">Po vytvoření některé reklamy při spuštění v cloudu, budete zobrazení řídicího panelu WebJobs SDK zobrazíte bohaté monitorovací funkce, které se má na nabídku.</span><span class="sxs-lookup"><span data-stu-id="e1b16-232">After you've created some ads while running in the cloud, you'll view the WebJobs SDK dashboard to see the rich monitoring features it has to offer.</span></span>

### <a name="deploy-to-web-apps"></a><span data-ttu-id="e1b16-233">Nasazení do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e1b16-233">Deploy to Web Apps</span></span>

1. <span data-ttu-id="e1b16-234">Zavřete prohlížeč a v okně konzoly aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1b16-234">Close the browser and the console application window.</span></span>
2. <span data-ttu-id="e1b16-235">Postupujte podle kroků v [publikovat do Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) části.</span><span class="sxs-lookup"><span data-stu-id="e1b16-235">Follow the steps in the [Publish to Azure with SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) section.</span></span>
3. <span data-ttu-id="e1b16-236">Po dokončení kroků pro nasazení, pokračujte ve zbývajících úkolech v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="e1b16-236">After you complete the steps for deploying, continue with the remaining tasks in this article.</span></span>

### <a name="configure-the-web-app-to-use-your-azure-sql-database-and-storage-account"></a><span data-ttu-id="e1b16-237">Konfigurovat webovou aplikaci k používání účtu Azure SQL database a úložiště</span><span class="sxs-lookup"><span data-stu-id="e1b16-237">Configure the web app to use your Azure SQL database and storage account</span></span>
<span data-ttu-id="e1b16-238">Je osvědčeným postupem zabezpečení [neukládejte citlivé informace, jako je například připojovací řetězce v souborech, které jsou uložené v úložišť zdrojového kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span><span class="sxs-lookup"><span data-stu-id="e1b16-238">It's a security best practice to [avoid putting sensitive information such as connection strings in files that are stored in source code repositories](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span></span> <span data-ttu-id="e1b16-239">Azure poskytuje způsob, jak to udělat: můžete nastavit připojovací řetězec a další nastavení hodnoty v prostředí Azure a rozhraní API pro konfiguraci ASP.NET automaticky vyzvedne, až tyto hodnoty při spuštění aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b16-239">Azure provides a way to do that: you can set connection string and other setting values in the Azure environment, and ASP.NET configuration APIs automatically pick up these values when the app runs in Azure.</span></span> <span data-ttu-id="e1b16-240">Tyto hodnoty lze nastavit v Azure pomocí **Průzkumníka serveru**, portálu Azure, prostředí Windows PowerShell nebo rozhraní příkazového řádku a platformy.</span><span class="sxs-lookup"><span data-stu-id="e1b16-240">You can set these values in Azure by using **Server Explorer**, the Azure Portal, Windows PowerShell, or the cross-platform command-line interface.</span></span> <span data-ttu-id="e1b16-241">Další informace najdete v tématu [fungování řetězců aplikace a připojovacích řetězců](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="e1b16-241">For more information, see [How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span>

<span data-ttu-id="e1b16-242">V této části můžete použít **Průzkumníka serveru** nastavit připojovací řetězec hodnoty v Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b16-242">In this section, you use **Server Explorer** to set connection string values in Azure.</span></span>

1. <span data-ttu-id="e1b16-243">V **Průzkumníka serveru**, klikněte pravým tlačítkem na vaší webové aplikace v rámci **Azure > aplikační služby > {vaší skupiny prostředků}**a potom klikněte na **nastavení zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-243">In **Server Explorer**, right-click your web app under **Azure > App Service > {your resource group}**, and then click **View Settings**.</span></span>

    <span data-ttu-id="e1b16-244">**Webové aplikace Azure** na se otevře okno **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="e1b16-244">The **Azure Web App** window opens on the **Configuration** tab.</span></span>
2. <span data-ttu-id="e1b16-245">Změňte název připojovacího řetězce možnost DefaultConnection na název, který jste zvolili, když jste nakonfigurovali v databázi SQL [publikovat do Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) článku.</span><span class="sxs-lookup"><span data-stu-id="e1b16-245">Change the name of the DefaultConnection connection string to the name you chose when you configured the SQL database in the [Publish to Azure with SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) article.</span></span>

    <span data-ttu-id="e1b16-246">Při vytváření webové aplikace s přidružené databáze, takže už je hodnota správný připojovací řetězec, Azure automaticky vytvoří tento připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="e1b16-246">Azure automatically created this connection string when you created the web app with an associated database, so it already has the right connection string value.</span></span> <span data-ttu-id="e1b16-247">Na co kódu hledá měníte jenom název.</span><span class="sxs-lookup"><span data-stu-id="e1b16-247">You're changing just the name to what your code is looking for.</span></span>
3. <span data-ttu-id="e1b16-248">Přidejte dva nové připojovací řetězce s názvem AzureWebJobsStorage a AzureWebJobsDashboard.</span><span class="sxs-lookup"><span data-stu-id="e1b16-248">Add two new connection strings, named AzureWebJobsStorage and AzureWebJobsDashboard.</span></span> <span data-ttu-id="e1b16-249">Nastavte typ databáze na **vlastní**a nastavte hodnotu připojovacího řetězce na stejnou hodnotu, jakou jste použili pro dříve *Web.config* a *App.config* soubory.</span><span class="sxs-lookup"><span data-stu-id="e1b16-249">Set the database type to **Custom**, and set the connection string value to the same value that you used earlier for the *Web.config* and *App.config* files.</span></span> <span data-ttu-id="e1b16-250">(Nezapomeňte zahrnout celý připojovací řetězec, ne jenom přístupový klíč a nechcete použít uvozovky.)</span><span class="sxs-lookup"><span data-stu-id="e1b16-250">(Be sure you include the entire connection string, not just the access key, and don't include the quotation marks.)</span></span>

    <span data-ttu-id="e1b16-251">Tyto připojovací řetězce jsou používány WebJobs SDK, jeden pro data aplikací a jeden pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="e1b16-251">These connection strings are used by the WebJobs SDK, one for application data and one for logging.</span></span> <span data-ttu-id="e1b16-252">Jak už jste viděli dříve, jeden pro data aplikací také používá kód front-endu webové.</span><span class="sxs-lookup"><span data-stu-id="e1b16-252">As you saw earlier, the one for application data is also used by the web front-end code.</span></span>
4. <span data-ttu-id="e1b16-253">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-253">Click **Save**.</span></span>

    ![Připojovací řetězce v portálu Azure](./media/websites-dotnet-webjobs-sdk-get-started/azconnstr.png)
5. <span data-ttu-id="e1b16-255">V **Průzkumníka serveru**, klikněte pravým tlačítkem na webovou aplikaci a pak klikněte na tlačítko **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-255">In **Server Explorer**, right-click the web app, and then click **Stop**.</span></span>
6. <span data-ttu-id="e1b16-256">Po zastavení webové aplikace, znovu klikněte pravým tlačítkem webovou aplikaci a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-256">After the web app stops, right-click the web app again, and then click **Start**.</span></span>

   <span data-ttu-id="e1b16-257">Vytvářené webové úlohy se automaticky spustí, když publikujete, ale se zastaví, když provedete změnu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e1b16-257">The WebJob automatically starts when you publish, but it stops when you make a configuration change.</span></span> <span data-ttu-id="e1b16-258">Chcete-li restartovat, můžete buď restartování webové aplikace nebo webové úlohy v [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="e1b16-258">To restart it, you can either restart the web app or restart the WebJob in the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="e1b16-259">Obecně se doporučuje k restartu webové aplikace po změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e1b16-259">It's generally recommended to restart the web app after a configuration change.</span></span>
7. <span data-ttu-id="e1b16-260">Aktualizujte okno prohlížeče, který má adresa URL webové aplikace v jeho panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="e1b16-260">Refresh the browser window that has the web app URL in its address bar.</span></span>

    <span data-ttu-id="e1b16-261">Zobrazí se na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="e1b16-261">The home page appears.</span></span>
8. <span data-ttu-id="e1b16-262">Vytvořte ad, jako když jste [byla aplikace spuštěná místně](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).</span><span class="sxs-lookup"><span data-stu-id="e1b16-262">Create an ad, as you did when you [ran the application locally](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).</span></span>

   <span data-ttu-id="e1b16-263">Index stránky zobrazí bez miniaturu na první.</span><span class="sxs-lookup"><span data-stu-id="e1b16-263">The Index page shows without a thumbnail at first.</span></span>
9. <span data-ttu-id="e1b16-264">Jakmile se objeví pár sekund a miniaturu, aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="e1b16-264">Refresh the page after a few seconds and the thumbnail appears.</span></span>

   <span data-ttu-id="e1b16-265">Pokud se nezobrazí miniaturu, můžete chtít Počkejte minutu pro webové úlohy znovu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-265">If the thumbnail doesn't appear, you might have to wait a minute or so for the WebJob to restart.</span></span> <span data-ttu-id="e1b16-266">Pokud po chvíli stále nevidíte miniaturu když obnovíte stránku, nemusí mít vytvářené webové úlohy automaticky spustit.</span><span class="sxs-lookup"><span data-stu-id="e1b16-266">If after a while, you still don't see the thumbnail when you refresh the page, the WebJob might not have started automatically.</span></span> <span data-ttu-id="e1b16-267">V takovém případě přejděte na **App Services** okno v [portál Azure](https://portal.azure.com/), vyhledejte vaší webové aplikace a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-267">In that case, go to the **App Services** blade in the [Azure portal](https://portal.azure.com/), locate your web app, and then click **Start**.</span></span>

### <a name="view-the-webjobs-sdk-dashboard"></a><span data-ttu-id="e1b16-268">Zobrazení řídicího panelu WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="e1b16-268">View the WebJobs SDK dashboard</span></span>
1. <span data-ttu-id="e1b16-269">V [portál Azure](https://portal.azure.com/), vyberte **okno App Services**, vyhledejte vaší webové aplikace a vyberte **WebJobs**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-269">In the [Azure portal](https://portal.azure.com/), select the **App Services blade**, locate your web app, and select **WebJobs**.</span></span>
3. <span data-ttu-id="e1b16-270">Vyberte **protokoly** kartě.</span><span class="sxs-lookup"><span data-stu-id="e1b16-270">Select the **Logs** tab.</span></span>

    ![Karta protokoly](./media/websites-dotnet-webjobs-sdk-get-started/log-tab.png)

    <span data-ttu-id="e1b16-272">Na řídicím panelu WebJobs SDK otevře novou kartu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="e1b16-272">A new browser tab opens to the WebJobs SDK dashboard.</span></span> <span data-ttu-id="e1b16-273">Řídicího panelu ukazuje, že vytvářené webové úlohy běží a zobrazí seznam funkcí ve vašem kódu, který aktivoval WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="e1b16-273">The dashboard shows that the WebJob is running and shows a list of functions in your code that the WebJobs SDK triggered.</span></span>
4. <span data-ttu-id="e1b16-274">Klikněte na některou z funkcí, které mají-li zobrazit podrobnosti o jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="e1b16-274">Click one of the functions to see details about its execution.</span></span>

    ![Řídicí panel WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjdashboardhome.png)

    ![Řídicí panel WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjfunctiondetails.png)

    <span data-ttu-id="e1b16-277">**Funkce opětovného přehrání** tlačítko na této stránce způsobí, že rozhraní WebJobs SDK na zavolejte funkci znovu a nabízí možnost změnit data nejprve předaný funkci.</span><span class="sxs-lookup"><span data-stu-id="e1b16-277">The **Replay Function** button on this page causes the WebJobs SDK framework to call the function again, and it gives you a chance to change the data passed to the function first.</span></span>

> [!NOTE]
> <span data-ttu-id="e1b16-278">Po dokončení testování, zvažte odstranění webové aplikace, účet úložiště a vaše instance databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="e1b16-278">When you're finished testing, consider deleting the web app, storage account, and your SQL Database instance.</span></span> <span data-ttu-id="e1b16-279">Tato webová aplikace je volné, ale nabíhat poplatky za úložiště účet a databáze instance SQL (i když, minimální kvůli malá velikost).</span><span class="sxs-lookup"><span data-stu-id="e1b16-279">The web app is free, but the SQL storage account and database instance accrue charges (albeit, minimal due to the small size).</span></span> <span data-ttu-id="e1b16-280">Navíc pokud webová aplikace spuštěná, každý, kdo najde vaši adresu URL můžete vytvořit a zobrazovat reklamy.</span><span class="sxs-lookup"><span data-stu-id="e1b16-280">Also, if you leave the web app running, anyone who finds your URL can create and view ads.</span></span> 
>
>

### <a name="delete-your-web-app"></a><span data-ttu-id="e1b16-281">Odstranění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="e1b16-281">Delete your web app</span></span>
<span data-ttu-id="e1b16-282">Na portálu, přejděte na **App Services** okno, najděte a vyberte vaší webové aplikace a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-282">In the portal, go to the **App Services** blade, locate and select your web app, and then click **Delete**.</span></span> <span data-ttu-id="e1b16-283">Pokud chcete dočasně jiným uživatelům zabránit v přístupu k webové aplikaci, klikněte na tlačítko **Zastavit** místo.</span><span class="sxs-lookup"><span data-stu-id="e1b16-283">If you just want to temporarily prevent others from accessing the web app, click **Stop** instead.</span></span> <span data-ttu-id="e1b16-284">V takovém případě budou poplatky dál nabíhat účtu SQL Database a úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-284">In that case, charges will continue to accrue for the SQL Database and Storage account.</span></span>

### <a name="delete-your-storage-account"></a><span data-ttu-id="e1b16-285">Odstranění účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="e1b16-285">Delete your storage account</span></span>
<span data-ttu-id="e1b16-286">Pokud chcete odstranit účet úložiště, najdete v části [odstranit účet úložiště](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="e1b16-286">To delete your storage account, see [Delete a storage account](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account).</span></span> 

### <a name="delete-your-database"></a><span data-ttu-id="e1b16-287">Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="e1b16-287">Delete your database</span></span>
<span data-ttu-id="e1b16-288">Pokud chcete odstranit vaší databázi SQL, najdete v článku [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e1b16-288">To delete your SQL database, see the [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) documentation.</span></span>

## <span data-ttu-id="e1b16-289"><a id="create"></a>Vytvoření aplikace od začátku</span><span class="sxs-lookup"><span data-stu-id="e1b16-289"><a id="create"></a>Create the application from scratch</span></span>
<span data-ttu-id="e1b16-290">V této části, můžete to udělat následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="e1b16-290">In this section you'll do the following tasks:</span></span>

* <span data-ttu-id="e1b16-291">Vytvoření řešení sady Visual Studio s webového projektu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-291">Create a Visual Studio solution with a web project.</span></span>
* <span data-ttu-id="e1b16-292">Přidání projektu knihovny tříd pro data vrstva přístupu, která jsou sdílena mezi front-endu a back-end.</span><span class="sxs-lookup"><span data-stu-id="e1b16-292">Add a class library project for the data access layer that is shared between the front end and back end.</span></span>
* <span data-ttu-id="e1b16-293">Přidejte projekt konzolové aplikace pro back-end, se webové úlohy nasazení povolené.</span><span class="sxs-lookup"><span data-stu-id="e1b16-293">Add a Console Application project for the backend, with WebJobs deployment enabled.</span></span>
* <span data-ttu-id="e1b16-294">Přidání balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="e1b16-294">Add NuGet packages.</span></span>
* <span data-ttu-id="e1b16-295">Nastavení odkazů projektu</span><span class="sxs-lookup"><span data-stu-id="e1b16-295">Set project references.</span></span>
* <span data-ttu-id="e1b16-296">Zkopírujte soubory kódu a konfigurace aplikací ze stažených aplikací, kterou jste pracovali v předchozí části kurzu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-296">Copy application code and configuration files from the downloaded application that you worked with in the previous section of the tutorial.</span></span>
* <span data-ttu-id="e1b16-297">Projděte si části kódu, které fungují s Azure BLOB a fronty a WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="e1b16-297">Review the parts of the code that work with Azure blobs and queues and the WebJobs SDK.</span></span>

### <a name="create-a-visual-studio-solution-with-a-web-project-and-class-library-project"></a><span data-ttu-id="e1b16-298">Vytvoření řešení Visual Studio s webový projekt a projekt knihovny tříd</span><span class="sxs-lookup"><span data-stu-id="e1b16-298">Create a Visual Studio solution with a web project and class library project</span></span>
1. <span data-ttu-id="e1b16-299">V sadě Visual Studio, vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-299">In Visual Studio, choose **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="e1b16-300">V **nový projekt** dialogovém okně, vyberte **Visual C#** > **webové** > **webové aplikace ASP.NET (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-300">In the **New Project** dialog, choose **Visual C#** > **Web** > **ASP.NET Web Application (.NET Framework)**.</span></span>
3. <span data-ttu-id="e1b16-301">Název projektu ContosoAdsWeb, název řešení ContosoAdsWebJobsSDK (změnit název řešení, pokud jste ji umístíte ve stejné složce jako staženého řešení) a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-301">Name the project ContosoAdsWeb, name the solution ContosoAdsWebJobsSDK (change the solution name if you're putting it in the same folder as the downloaded solution), and then click **OK**.</span></span>

    ![Nový projekt](./media/websites-dotnet-webjobs-sdk-get-started/newproject.png)
4. <span data-ttu-id="e1b16-303">V **nové webové aplikace ASP.NET** dialogovém okně, vyberte šablonu MVC a vyberte **změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-303">In the **New ASP.NET Web Application** dialog, choose the MVC template, and select **Change Authentication**.</span></span>

    ![Změna ověřování](./media/websites-dotnet-webjobs-sdk-get-started/chgauth.png)
5. <span data-ttu-id="e1b16-305">V **změna ověřování** dialogovém okně, vyberte **bez ověřování**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-305">In the **Change Authentication** dialog, choose **No Authentication**, and then click **OK**.</span></span>

    ![Bez ověřování](./media/websites-dotnet-webjobs-sdk-get-started/noauth.png)
6. <span data-ttu-id="e1b16-307">V **nové webové aplikace ASP.NET** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-307">In the **New ASP.NET Web Application** dialog, click **OK**.</span></span>

    <span data-ttu-id="e1b16-308">Visual Studio vytvoří řešení a webového projektu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-308">Visual Studio creates the solution and the web project.</span></span>
7. <span data-ttu-id="e1b16-309">V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení (nikoli projekt) a zvolte **přidat** > **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-309">In **Solution Explorer**, right-click the solution (not the project), and choose **Add** > **New Project**.</span></span>
8. <span data-ttu-id="e1b16-310">V **přidat nový projekt** dialogovém okně, vyberte **Visual C#** > **Windows Classic Desktop** > **knihovny tříd (rozhraní .NET Framework)**  šablony.</span><span class="sxs-lookup"><span data-stu-id="e1b16-310">In the **Add New Project** dialog, choose **Visual C#** > **Windows Classic Desktop** > **Class Library (.NET Framework)** template.</span></span>  
9. <span data-ttu-id="e1b16-311">Pojmenujte projekt *ContosoAdsCommon* a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-311">Name the project *ContosoAdsCommon*, and then click **OK**.</span></span>

    <span data-ttu-id="e1b16-312">Tento projekt bude obsahovat kontext Entity Framework a datový model, který použije front-end i back-end.</span><span class="sxs-lookup"><span data-stu-id="e1b16-312">This project will contain the Entity Framework context and the data model which both the front end and back end will use.</span></span> <span data-ttu-id="e1b16-313">Jako alternativu můžete třídy související s EF definovat v projektu webové a odkazovat na takový projekt z projektu webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="e1b16-313">As an alternative, you could define the EF-related classes in the web project and reference that project from the WebJob project.</span></span> <span data-ttu-id="e1b16-314">Ale pak projekt webové úlohy by mít odkaz na webová sestavení, která nepotřebuje.</span><span class="sxs-lookup"><span data-stu-id="e1b16-314">But, then your WebJob project would have a reference to web assemblies, which it doesn't need.</span></span>

### <a name="add-a-console-application-project-that-has-webjobs-deployment-enabled"></a><span data-ttu-id="e1b16-315">Přidání projektu konzolové aplikace, který má povoleno nasazení webové úlohy</span><span class="sxs-lookup"><span data-stu-id="e1b16-315">Add a Console Application project that has WebJobs deployment enabled</span></span>
1. <span data-ttu-id="e1b16-316">Klikněte pravým tlačítkem na projekt webové (ne řešení nebo projektu knihovny tříd) a pak klikněte na **přidat** > **nový projekt webové úlohy Azure**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-316">Right-click the web project (not the solution or the class library project), and then click **Add** > **New Azure WebJob Project**.</span></span>

    ![Nový projekt webové úlohy Azure výběr nabídky](./media/websites-dotnet-webjobs-sdk-get-started/newawjp.png)
2. <span data-ttu-id="e1b16-318">V **přidat webové úlohy Azure** dialogové okno, zadejte ContosoAdsWebJob jako i **název projektu** a **název webové úlohy**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-318">In the **Add Azure WebJob** dialog, enter ContosoAdsWebJob as both the **Project name** and the **WebJob name**.</span></span> <span data-ttu-id="e1b16-319">Nechte **webové úlohy spustit režim** nastavena na **spouštět nepřetržitě**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-319">Leave **WebJob run mode** set to **Run Continuously**.</span></span>
3. <span data-ttu-id="e1b16-320">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-320">Click **OK**.</span></span>

   <span data-ttu-id="e1b16-321">Visual Studio vytvoří konzolovou aplikaci, která je nakonfigurovat pro nasazení jako webová vždy, když je nasazení webového projektu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-321">Visual Studio creates a Console application that is configured to deploy as a WebJob whenever you deploy the web project.</span></span> <span data-ttu-id="e1b16-322">Kvůli tomu provádět následující úlohy po vytvoření projektu:</span><span class="sxs-lookup"><span data-stu-id="e1b16-322">To do that, it performed the following tasks after creating the project:</span></span>

   * <span data-ttu-id="e1b16-323">Přidat *webové úlohy publikovat settings.json* soubor ve složce vlastnosti projektu webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="e1b16-323">Added a *webjob-publish-settings.json* file in the WebJob project Properties folder.</span></span>
   * <span data-ttu-id="e1b16-324">Přidat *webjobs list.json* soubor ve složce webového projektu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e1b16-324">Added a *webjobs-list.json* file in the web project Properties folder.</span></span>
   * <span data-ttu-id="e1b16-325">Balíček Microsoft.Web.WebJobs.Publish NuGet nainstalované v projektu webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="e1b16-325">Installed the Microsoft.Web.WebJobs.Publish NuGet package in the WebJob project.</span></span>

   <span data-ttu-id="e1b16-326">Další informace o těchto změnách najdete v tématu [postup nasazení webové úlohy pomocí sady Visual Studio](websites-dotnet-deploy-webjobs.md).</span><span class="sxs-lookup"><span data-stu-id="e1b16-326">For more information about these changes, see [How to deploy WebJobs by using Visual Studio](websites-dotnet-deploy-webjobs.md).</span></span>

### <a name="add-nuget-packages"></a><span data-ttu-id="e1b16-327">Přidání balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="e1b16-327">Add NuGet packages</span></span>
<span data-ttu-id="e1b16-328">Nový projekt šablony pro projekt webové úlohy automaticky nainstaluje balíček NuGet pro webové úlohy SDK [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="e1b16-328">The new-project template for a WebJob project automatically installs the WebJobs SDK NuGet package [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) and its dependencies.</span></span>

<span data-ttu-id="e1b16-329">Jednu ze závislostí WebJobs SDK, které se instaluje automaticky v projektu webové úlohy je knihovna pro klienta úložiště Azure (SCL).</span><span class="sxs-lookup"><span data-stu-id="e1b16-329">One of the WebJobs SDK dependencies that is installed automatically in the WebJob project is the Azure Storage Client Library (SCL).</span></span> <span data-ttu-id="e1b16-330">Potřebujete přidat k webovému projektu pro práci s objekty BLOB a fronty.</span><span class="sxs-lookup"><span data-stu-id="e1b16-330">However, you need to add it to the web project to work with blobs and queues.</span></span>

1. <span data-ttu-id="e1b16-331">Otevřete **spravovat balíčky NuGet** dialogové okno pro řešení.</span><span class="sxs-lookup"><span data-stu-id="e1b16-331">Open the **Manage NuGet Packages** dialog for the solution.</span></span>
2. <span data-ttu-id="e1b16-332">V levém podokně vyberte **nainstalované balíky**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-332">In the left pane, select **Installed packages**.</span></span>
3. <span data-ttu-id="e1b16-333">Najít *Azure Storage* balíček a potom klikněte na **spravovat**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-333">Find the *Azure Storage* package, and then click **Manage**.</span></span>
4. <span data-ttu-id="e1b16-334">V **vyberte projekty** , vyberte **ContosoAdsWeb** zaškrtněte políčko a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-334">In the **Select Projects** box, select the **ContosoAdsWeb** check box, and then click **OK**.</span></span>

    <span data-ttu-id="e1b16-335">Všechny tři projekty pomocí rozhraní Entity Framework pro práci s daty v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="e1b16-335">All three projects use the Entity Framework to work with data in SQL Database.</span></span>
5. <span data-ttu-id="e1b16-336">V levém podokně vyberte **Online**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-336">In the left pane, select **Online**.</span></span>
6. <span data-ttu-id="e1b16-337">Najděte balíček NuGet *EntityFramework* a nainstalujte ho do všech tří projektů.</span><span class="sxs-lookup"><span data-stu-id="e1b16-337">Find the *EntityFramework* NuGet package, and install it in all three projects.</span></span>

### <a name="set-project-references"></a><span data-ttu-id="e1b16-338">Nastavení odkazů na projekty</span><span class="sxs-lookup"><span data-stu-id="e1b16-338">Set project references</span></span>
<span data-ttu-id="e1b16-339">Web a webové úlohy projekty pracovat databázi SQL, takže i musí být odkaz na projekt ContosoAdsCommon.</span><span class="sxs-lookup"><span data-stu-id="e1b16-339">Both web and WebJob projects work with the SQL database, so both need a reference to the ContosoAdsCommon project.</span></span>

1. <span data-ttu-id="e1b16-340">V projektu ContosoAdsWeb nastavte odkaz na projekt ContosoAdsCommon.</span><span class="sxs-lookup"><span data-stu-id="e1b16-340">In the ContosoAdsWeb project, set a reference to the ContosoAdsCommon project.</span></span> <span data-ttu-id="e1b16-341">(Klikněte pravým tlačítkem na projekt ContosoAdsWeb a potom klikněte na **přidat** > **odkaz**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-341">(Right-click the ContosoAdsWeb project, and then click **Add** > **Reference**.</span></span> 
2. <span data-ttu-id="e1b16-342">V **správce odkazů** dialogové okno, vyberte **projekty** > **řešení** > **ContosoAdsCommon**, a pak klikněte na **OK**.)</span><span class="sxs-lookup"><span data-stu-id="e1b16-342">In the **Reference Manager** dialog box, select **Projects** > **Solution** > **ContosoAdsCommon**, and then click **OK**.)</span></span>
   
    <span data-ttu-id="e1b16-343">Projekt webové úlohy musí odkazy pro práci s obrázky a pro přístup k připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="e1b16-343">The WebJob project needs references for working with images and for accessing connection strings.</span></span>

4. <span data-ttu-id="e1b16-344">V projektu ContosoAdsWebJob nastavte odkaz na `System.Drawing` a `System.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="e1b16-344">In the ContosoAdsWebJob project, set a reference to `System.Drawing` and `System.Configuration`.</span></span>

### <a name="add-code-and-configuration-files"></a><span data-ttu-id="e1b16-345">Přidání kódu a konfiguračních souborů</span><span class="sxs-lookup"><span data-stu-id="e1b16-345">Add code and configuration files</span></span>
<span data-ttu-id="e1b16-346">V tomto kurzu nezobrazuje postup [vytvořit kontrolery a zobrazení pomocí generování uživatelského rozhraní MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), jak k [napsat kód Entity Framework, který funguje s databází serveru SQL Server](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), nebo [základy asynchronní programování v technologii ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span><span class="sxs-lookup"><span data-stu-id="e1b16-346">This tutorial does not show how to [create MVC controllers and views using scaffolding](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), how to [write Entity Framework code that works with SQL Server databases](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), or [the basics of asynchronous programming in ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span></span> <span data-ttu-id="e1b16-347">Proto, všechno, co udělat zbývá kopie kódu a konfigurační soubory ze staženého řešení do nového řešení.</span><span class="sxs-lookup"><span data-stu-id="e1b16-347">So, all that remains to do is copy code and configuration files from the downloaded solution into the new solution.</span></span> <span data-ttu-id="e1b16-348">Poté, co můžete udělat, v následujících částech zobrazit a vysvětlí klíčová místa kódu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-348">After you do that, the following sections show and explain key parts of the code.</span></span>

<span data-ttu-id="e1b16-349">Pokud chcete přidat soubory do projektu nebo složky, klikněte pravým tlačítkem na projekt nebo složku a potom klikněte na **Přidat** > **Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-349">To add files to a project or a folder, right-click the project or folder and click **Add** > **Existing Item**.</span></span> <span data-ttu-id="e1b16-350">Vyberte soubory a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-350">Select the files you want and click **Add**.</span></span> <span data-ttu-id="e1b16-351">Pokud se zobrazí dotaz, jestli chcete nahradit existující soubory, klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="e1b16-351">If asked whether you want to replace existing files, click **Yes**.</span></span>

1. <span data-ttu-id="e1b16-352">V projektu ContosoAdsCommon odstraňte *Class1.cs* souborů a na jeho místo přidejte následující soubory ze staženého projektu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-352">In the ContosoAdsCommon project, delete the *Class1.cs* file and add in its place the following files from the downloaded project.</span></span>

   * <span data-ttu-id="e1b16-353">*AD.cs*</span><span class="sxs-lookup"><span data-stu-id="e1b16-353">*Ad.cs*</span></span>
   * <span data-ttu-id="e1b16-354">*ContosoAdscontext.cs*</span><span class="sxs-lookup"><span data-stu-id="e1b16-354">*ContosoAdscontext.cs*</span></span>
   * <span data-ttu-id="e1b16-355">*BlobInformation.cs*</span><span class="sxs-lookup"><span data-stu-id="e1b16-355">*BlobInformation.cs*</span></span>
2. <span data-ttu-id="e1b16-356">Do projektu ContosoAdsWeb přidejte následující soubory ze staženého projektu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-356">In the ContosoAdsWeb project, add the following files from the downloaded project.</span></span>

   * <span data-ttu-id="e1b16-357">*Soubor Web.config*</span><span class="sxs-lookup"><span data-stu-id="e1b16-357">*Web.config*</span></span>
   * <span data-ttu-id="e1b16-358">*Global.asax.cs*</span><span class="sxs-lookup"><span data-stu-id="e1b16-358">*Global.asax.cs*</span></span>  
   * <span data-ttu-id="e1b16-359">V *řadiče* složky: *AdController.cs*</span><span class="sxs-lookup"><span data-stu-id="e1b16-359">In the *Controllers* folder: *AdController.cs*</span></span>
   * <span data-ttu-id="e1b16-360">V *Views\Shared* složky: *_Layout.cshtml* souboru</span><span class="sxs-lookup"><span data-stu-id="e1b16-360">In the *Views\Shared* folder: *_Layout.cshtml* file</span></span>
   * <span data-ttu-id="e1b16-361">V *Views\Home* složky: *Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e1b16-361">In the *Views\Home* folder: *Index.cshtml*</span></span>
   * <span data-ttu-id="e1b16-362">V *Views\Ad* složky (nejdřív složku vytvořte): pět *.cshtml* soubory</span><span class="sxs-lookup"><span data-stu-id="e1b16-362">In the *Views\Ad* folder (create the folder first): five *.cshtml* files</span></span>
3. <span data-ttu-id="e1b16-363">V projektu ContosoAdsWebJob přidejte následující soubory ze staženého projektu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-363">In the ContosoAdsWebJob project, add the following files from the downloaded project.</span></span>

   * <span data-ttu-id="e1b16-364">*App.config* (změnit filtr typu souboru k **všechny soubory**)</span><span class="sxs-lookup"><span data-stu-id="e1b16-364">*App.config* (change the file type filter to **All Files**)</span></span>
   * <span data-ttu-id="e1b16-365">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="e1b16-365">*Program.cs*</span></span>
   * <span data-ttu-id="e1b16-366">*Functions.cs*</span><span class="sxs-lookup"><span data-stu-id="e1b16-366">*Functions.cs*</span></span>

<span data-ttu-id="e1b16-367">Teď můžete sestavit, spuštění a nasazení aplikace podle pokynů v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-367">You can now build, run, and deploy the application as instructed earlier in the tutorial.</span></span> <span data-ttu-id="e1b16-368">Předtím, než můžete udělat, ale Zastavte úlohu, která je stále spuštěná v první webové aplikace, který jste nasadili do.</span><span class="sxs-lookup"><span data-stu-id="e1b16-368">Before you do that, however, stop the WebJob that is still running in the first web app you deployed to.</span></span> <span data-ttu-id="e1b16-369">Tuto úlohu, jinak bude zpracovávat fronty zpráv vytvořených místně nebo v aplikaci spuštěné v nové webové aplikace, protože všechny používají stejný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-369">Otherwise, that WebJob will process queue messages created locally or by the app running in a new web app, since all are using the same storage account.</span></span>

## <span data-ttu-id="e1b16-370"><a id="code"></a>Zkontrolujte kódu aplikace</span><span class="sxs-lookup"><span data-stu-id="e1b16-370"><a id="code"></a>Review the application code</span></span>
<span data-ttu-id="e1b16-371">Následující části popisují kód týkající se práce s WebJobs SDK a úložiště Azure BLOB a fronty.</span><span class="sxs-lookup"><span data-stu-id="e1b16-371">The following sections explain the code related to working with the WebJobs SDK and Azure Storage blobs and queues.</span></span>

> [!NOTE]
> <span data-ttu-id="e1b16-372">Kód, které jsou specifické pro WebJobs SDK, najdete [Program.cs a Functions.cs](#programcs) oddíly.</span><span class="sxs-lookup"><span data-stu-id="e1b16-372">For the code specific to the WebJobs SDK, go to the [Program.cs and Functions.cs](#programcs) sections.</span></span>
>
>

### <a name="contosoadscommon---adcs"></a><span data-ttu-id="e1b16-373">ContosoAdsCommon – Ad.cs</span><span class="sxs-lookup"><span data-stu-id="e1b16-373">ContosoAdsCommon - Ad.cs</span></span>
<span data-ttu-id="e1b16-374">Soubor Ad.cs definuje výčet kategorií reklam a třídu entity objektů POCO pro informace o reklamách.</span><span class="sxs-lookup"><span data-stu-id="e1b16-374">The Ad.cs file defines an enum for ad categories and a POCO entity class for ad information.</span></span>

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

### <a name="contosoadscommon---contosoadscontextcs"></a><span data-ttu-id="e1b16-375">ContosoAdsCommon – ContosoAdsContext.cs</span><span class="sxs-lookup"><span data-stu-id="e1b16-375">ContosoAdsCommon - ContosoAdsContext.cs</span></span>
<span data-ttu-id="e1b16-376">Třída ContosoAdsContext Určuje, že Ad třída se používá v kolekci DbSet, kterou Entity Framework jsou uloženy v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="e1b16-376">The ContosoAdsContext class specifies that the Ad class is used in a DbSet collection, which Entity Framework stores in a SQL database.</span></span>

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

<span data-ttu-id="e1b16-377">Třída má dva konstruktory.</span><span class="sxs-lookup"><span data-stu-id="e1b16-377">The class has two constructors.</span></span> <span data-ttu-id="e1b16-378">První je používané webovým projektem a určuje název připojovacího řetězce, který je uložený v souboru Web.config nebo Azure běhové prostředí.</span><span class="sxs-lookup"><span data-stu-id="e1b16-378">The first is used by the web project, and specifies the name of a connection string that is stored in the Web.config file or the Azure runtime environment.</span></span> <span data-ttu-id="e1b16-379">Druhý konstruktor vám umožňuje předat samotný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="e1b16-379">The second constructor enables you to pass in the actual connection string.</span></span> <span data-ttu-id="e1b16-380">Projekt webové úlohy, je potřeba, protože sám nemá soubor Web.config.</span><span class="sxs-lookup"><span data-stu-id="e1b16-380">That is needed by the WebJob project since it doesn't have a Web.config file.</span></span> <span data-ttu-id="e1b16-381">Už dříve jste viděli, kam se tento připojovací řetězec uložil, a později uvidíte, jak kód získává připojovací řetězec při vytvoření instance třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="e1b16-381">You saw earlier where this connection string was stored, and you'll see later how the code retrieves the connection string when it instantiates the DbContext class.</span></span>

### <a name="contosoadscommon---blobinformationcs"></a><span data-ttu-id="e1b16-382">ContosoAdsCommon – BlobInformation.cs</span><span class="sxs-lookup"><span data-stu-id="e1b16-382">ContosoAdsCommon - BlobInformation.cs</span></span>
<span data-ttu-id="e1b16-383">`BlobInformation` Třída se používá k ukládání informací o objektu blob bitové kopie do zprávy fronty.</span><span class="sxs-lookup"><span data-stu-id="e1b16-383">The `BlobInformation` class is used to store information about an image blob in a queue message.</span></span>

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


### <a name="contosoadsweb---globalasaxcs"></a><span data-ttu-id="e1b16-384">ContosoAdsWeb – Global.asax.cs</span><span class="sxs-lookup"><span data-stu-id="e1b16-384">ContosoAdsWeb - Global.asax.cs</span></span>
<span data-ttu-id="e1b16-385">Kód, který se volá z metody `Application_Start`, vytvoří kontejner objektů blob s *obrázky* a frontu *obrázků*, pokud ještě neexistují.</span><span class="sxs-lookup"><span data-stu-id="e1b16-385">Code that is called from the `Application_Start` method creates an *images* blob container and an *images* queue if they don't already exist.</span></span> <span data-ttu-id="e1b16-386">To zajistí, že při každém spuštění pomocí nového účtu úložiště požadovaný kontejner objektů blob a fronty jsou vytvořeny automaticky.</span><span class="sxs-lookup"><span data-stu-id="e1b16-386">This ensures that whenever you start using a new storage account, the required blob container and queue are created automatically.</span></span>

<span data-ttu-id="e1b16-387">Kód získá přístup k účtu úložiště pomocí připojovacího řetězce úložiště ze *Web.config* soubor nebo Azure běhového prostředí.</span><span class="sxs-lookup"><span data-stu-id="e1b16-387">The code gets access to the storage account by using the storage connection string from the *Web.config* file or Azure runtime environment.</span></span>

        var storageAccount = CloudStorageAccount.Parse
            (ConfigurationManager.ConnectionStrings["AzureWebJobsStorage"].ToString());

<span data-ttu-id="e1b16-388">Potom získá odkaz na *bitové kopie* kontejner objektů blob, vytvoří kontejner, pokud ještě neexistuje, a nastaví přístupová oprávnění na nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="e1b16-388">Then, it gets a reference to the *images* blob container, creates the container if it doesn't already exist, and sets access permissions on the new container.</span></span> <span data-ttu-id="e1b16-389">Ve výchozím nastavení povolí nové kontejnery přístup k objektům BLOB jenom klientům pomocí přihlašovacích údajů účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e1b16-389">By default, new containers allow only clients with storage account credentials to access blobs.</span></span> <span data-ttu-id="e1b16-390">Webová aplikace musí objekty BLOB byly veřejné, tak, aby ho nemohl zobrazit obrázky pomocí adres URL, které odkazují na objekty BLOB bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="e1b16-390">The web app needs the blobs to be public so that it can display images using URLs that point to the image blobs.</span></span>

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

<span data-ttu-id="e1b16-391">Podobný kód získá odkaz na *thumbnailrequest* fronty a vytvoří novou frontu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-391">Similar code gets a reference to the *thumbnailrequest* queue and creates a new queue.</span></span> <span data-ttu-id="e1b16-392">V tomto případě není nutná žádná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e1b16-392">In this case no permissions change is needed.</span></span>

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("thumbnailrequest");
        imagesQueue.CreateIfNotExists();

### <a name="contosoadsweb---layoutcshtml"></a><span data-ttu-id="e1b16-393">ContosoAdsWeb – _Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="e1b16-393">ContosoAdsWeb - _Layout.cshtml</span></span>
<span data-ttu-id="e1b16-394">Soubor *_Layout.cshtml* nastaví název aplikace v záhlaví a zápatí a vytvoří položku nabídky „Reklamy“.</span><span class="sxs-lookup"><span data-stu-id="e1b16-394">The *_Layout.cshtml* file sets the app name in the header and footer, and creates an "Ads" menu entry.</span></span>

### <a name="contosoadsweb---viewshomeindexcshtml"></a><span data-ttu-id="e1b16-395">ContosoAdsWeb – Views\Home\Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="e1b16-395">ContosoAdsWeb - Views\Home\Index.cshtml</span></span>
<span data-ttu-id="e1b16-396">Soubor *Views\Home\Index.cshtml* zobrazuje na domovské stránce odkazy na kategorie.</span><span class="sxs-lookup"><span data-stu-id="e1b16-396">The *Views\Home\Index.cshtml* file displays category links on the home page.</span></span> <span data-ttu-id="e1b16-397">Odkazy předají celočíselnou hodnotu výčtu `Category` v proměnné řetězce dotazu na indexovou stránku reklam.</span><span class="sxs-lookup"><span data-stu-id="e1b16-397">The links pass the integer value of the `Category` enum in a querystring variable to the Ads Index page.</span></span>

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### <a name="contosoadsweb---adcontrollercs"></a><span data-ttu-id="e1b16-398">ContosoAdsWeb – AdController.cs</span><span class="sxs-lookup"><span data-stu-id="e1b16-398">ContosoAdsWeb - AdController.cs</span></span>
<span data-ttu-id="e1b16-399">V souboru *AdController.cs* volá konstruktor metodu `InitializeStorage`, aby vytvořil objekty knihovny klienta služby Azure Storage, které poskytují rozhraní API pro práci s objekty blob a frontami.</span><span class="sxs-lookup"><span data-stu-id="e1b16-399">In the *AdController.cs* file, the constructor calls the `InitializeStorage` method to create Azure Storage Client Library objects that provide an API for working with blobs and queues.</span></span>

<span data-ttu-id="e1b16-400">Potom kód získá odkaz na *bitové kopie* kontejner objektů blob, jako jste viděli dříve v *Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1b16-400">Then, the code gets a reference to the *images* blob container as you saw earlier in *Global.asax.cs*.</span></span> <span data-ttu-id="e1b16-401">Během toho, nastaví výchozí [zásady opakování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) vhodné pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1b16-401">While doing that, it sets a default [retry policy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) appropriate for a web app.</span></span> <span data-ttu-id="e1b16-402">Výchozí zásady opakování exponenciálního omezení rychlosti můžou způsobit, že webová aplikace přestane při opakovaných pokusech reagovat na dobu delší než jednu minutu. Důvodem může být přechodná chyba.</span><span class="sxs-lookup"><span data-stu-id="e1b16-402">The default exponential backoff retry policy could hang the web app for longer than a minute on repeated retries for a transient fault.</span></span> <span data-ttu-id="e1b16-403">Zde určené zásady opakování čekají po každém pokusu tři sekundy a celkem provádějí tři pokusy.</span><span class="sxs-lookup"><span data-stu-id="e1b16-403">The retry policy specified here waits three seconds after each try for up to three tries.</span></span>

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

<span data-ttu-id="e1b16-404">Podobný kód získá odkaz na frontu *obrázků*.</span><span class="sxs-lookup"><span data-stu-id="e1b16-404">Similar code gets a reference to the *images* queue.</span></span>

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("blobnamerequest");

<span data-ttu-id="e1b16-405">Většinu kódu kontroleru je typická pro práci s datovým modelem Entity Framework za použití třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="e1b16-405">Most of the controller code is typical for working with an Entity Framework data model using a DbContext class.</span></span> <span data-ttu-id="e1b16-406">Výjimkou je metoda HttpPost `Create`, která soubor odešle a uloží ho do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e1b16-406">An exception is the HttpPost `Create` method, which uploads a file and saves it in blob storage.</span></span> <span data-ttu-id="e1b16-407">Vazač modelu poskytuje metodě objekt [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx).</span><span class="sxs-lookup"><span data-stu-id="e1b16-407">The model binder provides an [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) object to the method.</span></span>

        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create(
            [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
            HttpPostedFileBase imageFile)

<span data-ttu-id="e1b16-408">Pokud uživatel vybral soubor k odeslání, kód soubor odešle, uloží ho do objektu blob a zaktualizuje záznam v databázi reklam pomocí adresy URL, která odkazuje na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="e1b16-408">If the user selected a file to upload, the code uploads the file, saves it in a blob, and updates the Ad database record with a URL that points to the blob.</span></span>

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            blob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = blob.Uri.ToString();
        }

<span data-ttu-id="e1b16-409">Kód, který odesílání provádí, je umístěný v metodě `UploadAndSaveBlobAsync`.</span><span class="sxs-lookup"><span data-stu-id="e1b16-409">The code that does the upload is in the `UploadAndSaveBlobAsync` method.</span></span> <span data-ttu-id="e1b16-410">Vytvoří identifikátor GUID pro objekt blob, odešle a uloží soubor a vrátí odkaz na uložený objekt blob.</span><span class="sxs-lookup"><span data-stu-id="e1b16-410">It creates a GUID name for the blob, uploads and saves the file, and returns a reference to the saved blob.</span></span>

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

<span data-ttu-id="e1b16-411">Po HttpPost `Create` metoda odešle objekt blob a zaktualizuje databázi, vytvoří zprávu fronty, aby proces back-end informovat, že obrázek je připravený k převodu na miniaturu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-411">After the HttpPost `Create` method uploads a blob and updates the database, it creates a queue message to inform the back-end process that an image is ready for conversion to a thumbnail.</span></span>

        BlobInformation blobInfo = new BlobInformation() { AdId = ad.AdId, BlobUri = new Uri(ad.ImageURL) };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        await thumbnailRequestQueue.AddMessageAsync(queueMessage);

<span data-ttu-id="e1b16-412">Kód pro HttpPost `Edit` metoda je podobné, s tím rozdílem, že pokud uživatel vybere nový obrázkový soubor, musíte odstranit všechny objekty BLOB, které jsou už pro tento ad.</span><span class="sxs-lookup"><span data-stu-id="e1b16-412">The code for the HttpPost `Edit` method is similar, except that if the user selects a new image file, any blobs that already exist for this ad must be deleted.</span></span>

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            await DeleteAdBlobsAsync(ad);
            imageBlob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = imageBlob.Uri.ToString();
        }

<span data-ttu-id="e1b16-413">Tady je kód, který odstraní objekty BLOB při odstranění ad:</span><span class="sxs-lookup"><span data-stu-id="e1b16-413">Here is the code that deletes blobs when you delete an ad:</span></span>

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

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a><span data-ttu-id="e1b16-414">ContosoAdsWeb – Views\Ad\Index.cshtml a Details.cshtml</span><span class="sxs-lookup"><span data-stu-id="e1b16-414">ContosoAdsWeb - Views\Ad\Index.cshtml and Details.cshtml</span></span>
<span data-ttu-id="e1b16-415">*Index.cshtml* zobrazí miniatury s dalšími daty reklam:</span><span class="sxs-lookup"><span data-stu-id="e1b16-415">The *Index.cshtml* file displays thumbnails with the other ad data:</span></span>

        <img  src="@Html.Raw(item.ThumbnailURL)" />

<span data-ttu-id="e1b16-416">*Details.cshtml* zobrazí obrázek v plné velikosti:</span><span class="sxs-lookup"><span data-stu-id="e1b16-416">The *Details.cshtml* file displays the full-size image:</span></span>

        <img src="@Html.Raw(Model.ImageURL)" />

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a><span data-ttu-id="e1b16-417">ContosoAdsWeb – Views\Ad\Create.cshtml a Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="e1b16-417">ContosoAdsWeb - Views\Ad\Create.cshtml and Edit.cshtml</span></span>
<span data-ttu-id="e1b16-418">Soubory *Create.cshtml* a *Edit.cshtml* určují kódování formuláře, které kontroleru umožňuje získání objektu `HttpPostedFileBase`.</span><span class="sxs-lookup"><span data-stu-id="e1b16-418">The *Create.cshtml* and *Edit.cshtml* files specify form encoding that enables the controller to get the `HttpPostedFileBase` object.</span></span>

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

<span data-ttu-id="e1b16-419">Prvek `<input>` sděluje prohlížeči, aby zobrazil dialogové okno pro výběr souboru.</span><span class="sxs-lookup"><span data-stu-id="e1b16-419">An `<input>` element tells the browser to provide a file selection dialog.</span></span>

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### <span data-ttu-id="e1b16-420"><a id="programcs"></a>ContosoAdsWebJob - Program.cs</span><span class="sxs-lookup"><span data-stu-id="e1b16-420"><a id="programcs"></a>ContosoAdsWebJob - Program.cs</span></span>
<span data-ttu-id="e1b16-421">Při spuštění vytvářené webové úlohy, `Main` metoda volá WebJobs SDK `JobHost.RunAndBlock` metoda zahájíte provádění aktivaci funkcí na aktuální vlákno.</span><span class="sxs-lookup"><span data-stu-id="e1b16-421">When the WebJob starts, the `Main` method calls the WebJobs SDK `JobHost.RunAndBlock` method to begin execution of triggered functions on the current thread.</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

### <span data-ttu-id="e1b16-422"><a id="generatethumbnail"></a>Metoda GenerateThumbnail ContosoAdsWebJob - Functions.cs-</span><span class="sxs-lookup"><span data-stu-id="e1b16-422"><a id="generatethumbnail"></a>ContosoAdsWebJob - Functions.cs - GenerateThumbnail method</span></span>
<span data-ttu-id="e1b16-423">Sada WebJobs SDK volá tuto metodu při příjmu zprávy fronty.</span><span class="sxs-lookup"><span data-stu-id="e1b16-423">The WebJobs SDK calls this method when a queue message is received.</span></span> <span data-ttu-id="e1b16-424">Metoda vytvoří miniaturu a uloží miniaturu adresy URL v databázi.</span><span class="sxs-lookup"><span data-stu-id="e1b16-424">The method creates a thumbnail and puts the thumbnail URL in the database.</span></span>

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
            // be instantiated and disposed within the function.
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

* <span data-ttu-id="e1b16-425">`QueueTrigger` Atribut přesměruje WebJobs SDK mohli volat tuto metodu, když je obdržena novou zprávu ve frontě thumbnailrequest.</span><span class="sxs-lookup"><span data-stu-id="e1b16-425">The `QueueTrigger` attribute directs the WebJobs SDK to call this method when a new message is received on the thumbnailrequest queue.</span></span>

        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,

    <span data-ttu-id="e1b16-426">`BlobInformation` Objekt v zprávy ve frontě je automaticky deserializovat do `blobInfo` parametr.</span><span class="sxs-lookup"><span data-stu-id="e1b16-426">The `BlobInformation` object in the queue message is automatically deserialized into the `blobInfo` parameter.</span></span> <span data-ttu-id="e1b16-427">Po dokončení metody odstraní zprávu fronty.</span><span class="sxs-lookup"><span data-stu-id="e1b16-427">When the method completes, the queue message is deleted.</span></span> <span data-ttu-id="e1b16-428">Pokud před dokončením selže metodu, nebude odstraněna zprávy ve frontě. Po vypršení platnosti zapůjčení 10 minut, zpráva vydané být zachyceny znovu a zpracovat.</span><span class="sxs-lookup"><span data-stu-id="e1b16-428">If the method fails before completing, the queue message is not deleted; after a 10-minute lease expires, the message is released to be picked up again and processed.</span></span> <span data-ttu-id="e1b16-429">Toto pořadí nebude opakuje bez omezení, pokud zprávu vždy dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="e1b16-429">This sequence won't be repeated indefinitely if a message always causes an exception.</span></span> <span data-ttu-id="e1b16-430">Po 5 neúspěšných pokusech o zpracovat zprávu, zpráva bude přesunuta do fronty s názvem {queuename}-poškozených.</span><span class="sxs-lookup"><span data-stu-id="e1b16-430">After 5 unsuccessful attempts to process a message, the message is moved to a queue named {queuename}-poison.</span></span> <span data-ttu-id="e1b16-431">Maximální počet pokusů o je možné konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="e1b16-431">The maximum number of attempts is configurable.</span></span>
* <span data-ttu-id="e1b16-432">Dva `Blob` atributy zadejte objekty, které jsou vázány na objekty BLOB: jednu pro existující objekt blob image a jeden pro nové miniatur objekt blob, který vytvoří metodu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-432">The two `Blob` attributes provide objects that are bound to blobs: one to the existing image blob and one to a new thumbnail blob that the method creates.</span></span>

        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)

    <span data-ttu-id="e1b16-433">Názvy objektů BLOB pocházet z vlastnosti `BlobInformation` objekt dostali zprávy ve frontě (`BlobName` a `BlobNameWithoutExtension`).</span><span class="sxs-lookup"><span data-stu-id="e1b16-433">Blob names come from properties of the `BlobInformation` object received in the queue message (`BlobName` and `BlobNameWithoutExtension`).</span></span> <span data-ttu-id="e1b16-434">Chcete-li získat úplné funkce Klientská knihovna pro úložiště, můžete použít `CloudBlockBlob` třídy pro práci s objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="e1b16-434">To get the full functionality of the Storage Client Library, you can use the `CloudBlockBlob` class to work with blobs.</span></span> <span data-ttu-id="e1b16-435">Pokud chcete znovu použít kód, který byl napsán pro práci s `Stream` objekty, můžete použít `Stream` třídy.</span><span class="sxs-lookup"><span data-stu-id="e1b16-435">If you want to reuse code that was written to work with `Stream` objects, you can use the `Stream` class.</span></span>

<span data-ttu-id="e1b16-436">Další informace o tom, jak psát funkce, které se používají atributy WebJobs SDK najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="e1b16-436">For more information about how to write functions that use  WebJobs SDK attributes, see the following resources:</span></span>

* [<span data-ttu-id="e1b16-437">Použití Azure Queue Storage se sadou WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="e1b16-437">How to use Azure queue storage with the WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [<span data-ttu-id="e1b16-438">Použití Azure Blob Storage se sadou WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="e1b16-438">How to use Azure blob storage with the WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [<span data-ttu-id="e1b16-439">Použití Azure Table Storage se sadou WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="e1b16-439">How to use Azure table storage with the WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [<span data-ttu-id="e1b16-440">Použití Azure Service Bus se sadou WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="e1b16-440">How to use Azure Service Bus with the WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-service-bus.md)

> [!NOTE]
> * <span data-ttu-id="e1b16-441">Pokud vaše webová aplikace běží na víc virtuálních počítačů, více webové úlohy budou používat současně a v některých případech může být výsledkem stejná data zpracovává více než jednou.</span><span class="sxs-lookup"><span data-stu-id="e1b16-441">If your web app runs on multiple VMs, multiple WebJobs will be running simultaneously, and in some scenarios this can result in the same data getting processed multiple times.</span></span> <span data-ttu-id="e1b16-442">To není problém, pokud používáte integrované fronty, objektů blob a aktivační události služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="e1b16-442">This is not a problem if you use the built-in queue, blob, and Service Bus triggers.</span></span> <span data-ttu-id="e1b16-443">Sada SDK zajistí, že funkce budou zpracovány pouze jednou pro každou zprávu nebo objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e1b16-443">The SDK ensures that your functions will be processed only once for each message or blob.</span></span>
> * <span data-ttu-id="e1b16-444">Informace o tom, jak implementovat řádné vypnutí najdete v tématu [řádné vypnutí](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).</span><span class="sxs-lookup"><span data-stu-id="e1b16-444">For information about how to implement graceful shutdown, see [Graceful Shutdown](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).</span></span>
> * <span data-ttu-id="e1b16-445">Kód `ConvertImageToThumbnailJPG` – metoda (není vidět) používá třídy v `System.Drawing` obor názvů pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="e1b16-445">The code in the `ConvertImageToThumbnailJPG` method (not shown) uses classes in the `System.Drawing` namespace for simplicity.</span></span> <span data-ttu-id="e1b16-446">Třídy v tomto oboru názvů však byly navrženy pro používání s formuláři Windows.</span><span class="sxs-lookup"><span data-stu-id="e1b16-446">However, the classes in this namespace were designed for use with Windows Forms.</span></span> <span data-ttu-id="e1b16-447">Jejich používání není podporované ve Windows a službě ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e1b16-447">They are not supported for use in a Windows or ASP.NET service.</span></span> <span data-ttu-id="e1b16-448">Další informace o možnostech zpracování obrázků najdete v článcích o [dynamickém generování obrázků](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) a o [hloubkové změně velikosti uvnitř obrázků](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span><span class="sxs-lookup"><span data-stu-id="e1b16-448">For more information about image processing options, see [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) and [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="e1b16-449">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1b16-449">Next steps</span></span>
<span data-ttu-id="e1b16-450">V tomto kurzu jste se seznámili jednoduché vícevrstvé aplikace, který využívá sadu WebJobs SDK pro zpracování back-end.</span><span class="sxs-lookup"><span data-stu-id="e1b16-450">In this tutorial, you've seen a simple multi-tier application that uses the WebJobs SDK for backend processing.</span></span> <span data-ttu-id="e1b16-451">Tato část nabízí některé návrhy pro dozvědět více o vícevrstvých aplikací ASP.NET a webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="e1b16-451">This section offers some suggestions for learning more about ASP.NET multi-tier applications and WebJobs.</span></span>

### <a name="missing-features"></a><span data-ttu-id="e1b16-452">Chybějící funkce</span><span class="sxs-lookup"><span data-stu-id="e1b16-452">Missing features</span></span>
<span data-ttu-id="e1b16-453">Aplikace má jednoduché kurz – Začínáme.</span><span class="sxs-lookup"><span data-stu-id="e1b16-453">The application has been kept simple for a getting-started tutorial.</span></span> <span data-ttu-id="e1b16-454">V reálné aplikaci byste implementovat [vkládání závislostí](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) a [úložiště a jednotky pracovních vzorů](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), použijte [rozhraní pro protokolování](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), použijte [Migrace Code First EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) ke správě změn datových modelů a použít [odolnost připojení EF](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) k správě přechodných síťových chyb.</span><span class="sxs-lookup"><span data-stu-id="e1b16-454">In a real-world application you would implement [dependency injection](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) and the [repository and unit of work patterns](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), use [an interface for logging](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), use [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) to manage data model changes, and use [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) to manage transient network errors.</span></span>

### <a name="scaling-webjobs"></a><span data-ttu-id="e1b16-455">Škálování webové úlohy</span><span class="sxs-lookup"><span data-stu-id="e1b16-455">Scaling WebJobs</span></span>
<span data-ttu-id="e1b16-456">Webové úlohy spustit v kontextu webové aplikace a nejsou škálovatelné samostatně.</span><span class="sxs-lookup"><span data-stu-id="e1b16-456">WebJobs run in the context of a web app and are not scalable separately.</span></span> <span data-ttu-id="e1b16-457">Pokud máte jednu instanci standardní webové aplikace, máte jenom jednu instanci spuštěn proces na pozadí a používá některé prostředky serveru (procesoru, paměti, atd.), které jinak by být k dispozici k obsluze webového obsahu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-457">For example, if you have one Standard web app instance, you have only one instance of your background process running, and it is using some of the server resources (CPU, memory, etc.) that otherwise would be available to serve web content.</span></span>

<span data-ttu-id="e1b16-458">Pokud provoz se liší podle času den nebo den v týdnu, a pokud zpracování back-end, budete muset počkat, naplánujete vaší webové úlohy na spuštění v časech špičky.</span><span class="sxs-lookup"><span data-stu-id="e1b16-458">If traffic varies by time of day or day of week, and if the backend processing you need to do can wait, you could schedule your WebJobs to run at low-traffic times.</span></span> <span data-ttu-id="e1b16-459">Pokud stále zatížení příliš vysoká. pro toto řešení, můžete spustit back-end jako webová úloha v samostatné webové aplikace, který je vyhrazený pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="e1b16-459">If the load is still too high for that solution, you can run the backend as a WebJob in a separate web app dedicated for that purpose.</span></span> <span data-ttu-id="e1b16-460">Webové aplikace back-end je pak možné škálovat nezávisle z vaší webové aplikace front-endu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-460">You can then scale your backend web app independently from your frontend web app.</span></span>

<span data-ttu-id="e1b16-461">Další informace najdete v tématu [škálování WebJobs](websites-webjobs-resources.md#scale).</span><span class="sxs-lookup"><span data-stu-id="e1b16-461">For more information, see [Scaling WebJobs](websites-webjobs-resources.md#scale).</span></span>

### <a name="avoiding-web-app-timeout-shut-downs"></a><span data-ttu-id="e1b16-462">Webové aplikace časový limit vypnutí seznamy vyloučení</span><span class="sxs-lookup"><span data-stu-id="e1b16-462">Avoiding web app timeout shut-downs</span></span>
<span data-ttu-id="e1b16-463">A ujistěte se, že vaše webové úlohy jsou vždy spuštěné a spuštěná na všech instancích vaší webové aplikace, je třeba povolit [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) funkce.</span><span class="sxs-lookup"><span data-stu-id="e1b16-463">To make sure your WebJobs are always running, and running on all instances of your web app, you have to enable the [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) feature.</span></span>

### <a name="using-the-webjobs-sdk-outside-of-webjobs"></a><span data-ttu-id="e1b16-464">Pomocí sady SDK webové úlohy mimo webové úlohy</span><span class="sxs-lookup"><span data-stu-id="e1b16-464">Using the WebJobs SDK outside of WebJobs</span></span>
<span data-ttu-id="e1b16-465">Program, který využívá sadu WebJobs SDK nemá na spuštění v Azure v webovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="e1b16-465">A program that uses the WebJobs SDK doesn't have to run in Azure in a WebJob.</span></span> <span data-ttu-id="e1b16-466">Můžete spustit místně a také může být spuštěn v jiných prostředích, jako je například roli pracovního procesu cloudové služby nebo služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e1b16-466">It can run locally, and it can also run in other environments such as a Cloud Service worker role or a Windows service.</span></span> <span data-ttu-id="e1b16-467">Ale můžete jenom přístup k řídicímu panelu WebJobs SDK prostřednictvím webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b16-467">However, you can only access the WebJobs SDK dashboard through an Azure web app.</span></span> <span data-ttu-id="e1b16-468">Použití řídicího panelu, budete muset webovou aplikaci připojit k účtu úložiště používáte nastavením AzureWebJobsDashboard připojovací řetězec **konfigurace** kartě portálu classic.</span><span class="sxs-lookup"><span data-stu-id="e1b16-468">To use the dashboard you have to connect the web app to the storage account you're using by setting the AzureWebJobsDashboard connection string on the **Configure** tab of the classic portal.</span></span> <span data-ttu-id="e1b16-469">Potom můžete získat na řídicí panel pomocí následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="e1b16-469">Then, you can get to the Dashboard by using the following URL:</span></span>

<span data-ttu-id="e1b16-470">https://{webappname}.SCM.azurewebsites.NET/azurejobs/#/functions</span><span class="sxs-lookup"><span data-stu-id="e1b16-470">https://{webappname}.scm.azurewebsites.net/azurejobs/#/functions</span></span>

<span data-ttu-id="e1b16-471">Další informace najdete v tématu [získávání řídicí panel pro místní vývoj pomocí WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ale Všimněte si, že se zobrazí původní název připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="e1b16-471">For more information, see [Getting a dashboard for local development with the WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), but note that it shows an old connection string name.</span></span>

### <a name="more-webjobs-documentation"></a><span data-ttu-id="e1b16-472">Další dokumentaci webové úlohy</span><span class="sxs-lookup"><span data-stu-id="e1b16-472">More WebJobs documentation</span></span>
<span data-ttu-id="e1b16-473">Další informace najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="e1b16-473">For more information, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span>
