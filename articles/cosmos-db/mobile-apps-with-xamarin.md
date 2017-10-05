---
title: "Vytváření mobilních aplikací s Xamarin a Azure Cosmos DB | Microsoft Docs"
description: "Kurz, který vytvoří Xamarin iOS, Android nebo formuláře aplikace pomocí Azure Cosmos DB. Azure Cosmos DB je fast, planetu škálování, cloudu databáze pro mobilní aplikace."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: monicar
editor: 
ms.assetid: ff97881a-b41a-499d-b7ab-4f394df0e153
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: arramac
ms.openlocfilehash: 4dfe9c755f3e7d5414ae04dd4027defd6cef2e4a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="build-mobile-applications-with-xamarin-and-azure-cosmos-db"></a><span data-ttu-id="2c59c-104">Vytváření mobilních aplikací s Xamarin a Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2c59c-104">Build mobile applications with Xamarin and Azure Cosmos DB</span></span>
<span data-ttu-id="2c59c-105">Většina mobilních aplikací potřebovat k ukládání dat v cloudu a Azure Cosmos databáze je databáze cloudu pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c59c-105">Most mobile apps need to store data in the cloud, and Azure Cosmos DB is a cloud database for mobile apps.</span></span> <span data-ttu-id="2c59c-106">Obsahuje všechny objekty, které musí vývojář mobilní.</span><span class="sxs-lookup"><span data-stu-id="2c59c-106">It has everything a mobile developer needs.</span></span> <span data-ttu-id="2c59c-107">Je plně spravovaná databáze jako služba, která je škálovatelná na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="2c59c-107">It is a fully managed database as a service that scales on demand.</span></span> <span data-ttu-id="2c59c-108">Ho můžete Oživte svoje data do vaší aplikace transparentně, bez ohledu na umístění uživatele po celém světě.</span><span class="sxs-lookup"><span data-stu-id="2c59c-108">It can bring your data to your application transparently, wherever your users are located around the globe.</span></span> <span data-ttu-id="2c59c-109">Pomocí [Azure Cosmos DB .NET Core SDK](documentdb-sdk-dotnet-core.md), můžete povolit mobilní aplikace Xamarin pracovat přímo s Azure Cosmos DB bez střední vrstvy.</span><span class="sxs-lookup"><span data-stu-id="2c59c-109">By using the [Azure Cosmos DB .NET Core SDK](documentdb-sdk-dotnet-core.md), you can enable Xamarin mobile apps to interact directly with Azure Cosmos DB, without a middle tier.</span></span>

<span data-ttu-id="2c59c-110">Tento článek obsahuje návod pro vytváření mobilních aplikací s Xamarin a Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-110">This article provides a tutorial for building mobile apps with Xamarin and Azure Cosmos DB.</span></span> <span data-ttu-id="2c59c-111">Můžete najít úplný zdrojový kód pro tento kurz v [Xamarin a Azure DB Cosmos na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin), včetně toho, jak můžete spravovat uživatele a oprávnění.</span><span class="sxs-lookup"><span data-stu-id="2c59c-111">You can find the complete source code for the tutorial at [Xamarin and Azure Cosmos DB on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin), including how to manage users and permissions.</span></span>

## <a name="azure-cosmos-db-capabilities-for-mobile-apps"></a><span data-ttu-id="2c59c-112">Možnosti Azure Cosmos DB pro mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="2c59c-112">Azure Cosmos DB capabilities for mobile apps</span></span>
<span data-ttu-id="2c59c-113">Azure Cosmos DB poskytuje následující klíčové funkce pro vývojáře mobilních aplikací:</span><span class="sxs-lookup"><span data-stu-id="2c59c-113">Azure Cosmos DB provides the following key capabilities for mobile app developers:</span></span>

![Možnosti Azure Cosmos DB pro mobilní aplikace](media/mobile-apps-with-xamarin/documentdb-for-mobile.png)

* <span data-ttu-id="2c59c-115">Bohaté dotazy nad datovým.</span><span class="sxs-lookup"><span data-stu-id="2c59c-115">Rich queries over schemaless data.</span></span> <span data-ttu-id="2c59c-116">Azure Cosmos DB ukládá data jako schemaless dokumenty JSON v heterogenní kolekce.</span><span class="sxs-lookup"><span data-stu-id="2c59c-116">Azure Cosmos DB stores data as schemaless JSON documents in heterogeneous collections.</span></span> <span data-ttu-id="2c59c-117">Nabízí [bohatý a rychlé dotazy](documentdb-sql-query.md) bez nutnosti starat o schémata nebo indexů.</span><span class="sxs-lookup"><span data-stu-id="2c59c-117">It offers [rich and fast queries](documentdb-sql-query.md) without the need to worry about schemas or indexes.</span></span>
* <span data-ttu-id="2c59c-118">Rychlé propustnost.</span><span class="sxs-lookup"><span data-stu-id="2c59c-118">Fast throughput.</span></span> <span data-ttu-id="2c59c-119">Jak dlouho trvá jen několik milisekund číst a zapisovat dokumentů s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-119">It takes only a few milliseconds to read and write documents with Azure Cosmos DB.</span></span> <span data-ttu-id="2c59c-120">Vývojářům můžete určit, které potřebují propustnost a Azure Cosmos DB ctí ho s SLA 99,99 %.</span><span class="sxs-lookup"><span data-stu-id="2c59c-120">Developers can specify the throughput they need, and Azure Cosmos DB honors it with 99.99 percent SLAs.</span></span>
* <span data-ttu-id="2c59c-121">Neomezený škálování.</span><span class="sxs-lookup"><span data-stu-id="2c59c-121">Limitless scale.</span></span> <span data-ttu-id="2c59c-122">Kolekce Azure Cosmos DB [růst s růstem aplikace](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="2c59c-122">Your Azure Cosmos DB collections [grow as your app grows](partition-data.md).</span></span> <span data-ttu-id="2c59c-123">Můžete spustit s malou velikost dat a propustnost stovky požadavků za sekundu.</span><span class="sxs-lookup"><span data-stu-id="2c59c-123">You can start with small data size and throughput of hundreds of requests per second.</span></span> <span data-ttu-id="2c59c-124">Kolekce můžou růst až po petabajty dat a libovolně velké propustnosti se stovkami miliony požadavků za sekundu.</span><span class="sxs-lookup"><span data-stu-id="2c59c-124">Your collections can grow to petabytes of data and arbitrarily large throughput with hundreds of millions of requests per second.</span></span>
* <span data-ttu-id="2c59c-125">Globálně distribuované.</span><span class="sxs-lookup"><span data-stu-id="2c59c-125">Globally distributed.</span></span> <span data-ttu-id="2c59c-126">Uživatelé mobilních aplikací jsou na cestách, často po celém světě.</span><span class="sxs-lookup"><span data-stu-id="2c59c-126">Mobile app users are on the go, often across the world.</span></span> <span data-ttu-id="2c59c-127">Je Azure Cosmos DB [globálně distribuované databáze](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="2c59c-127">Azure Cosmos DB is a [globally distributed database](distribute-data-globally.md).</span></span> <span data-ttu-id="2c59c-128">Klikněte na mapě tak, aby vaše data mají uživatelé přístup.</span><span class="sxs-lookup"><span data-stu-id="2c59c-128">Click the map to make your data accessible to your users.</span></span>
* <span data-ttu-id="2c59c-129">Předdefinované bohaté autorizace.</span><span class="sxs-lookup"><span data-stu-id="2c59c-129">Built-in rich authorization.</span></span> <span data-ttu-id="2c59c-130">S Azure Cosmos databáze, můžete snadno implementovat oblíbených vypadají podobně jako [uživatelská data](https://aka.ms/documentdb-xamarin-todouser) nebo s více uživateli sdílet data, bez komplexní vlastní autorizační kód.</span><span class="sxs-lookup"><span data-stu-id="2c59c-130">With Azure Cosmos DB, you can easily implement popular patterns like [per-user data](https://aka.ms/documentdb-xamarin-todouser) or multiuser shared data, without complex custom authorization code.</span></span>
* <span data-ttu-id="2c59c-131">Geoprostorové dotazy.</span><span class="sxs-lookup"><span data-stu-id="2c59c-131">Geospatial queries.</span></span> <span data-ttu-id="2c59c-132">Mnoha mobilních aplikacích nabídku geograficky kontextové prostředí ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="2c59c-132">Many mobile apps offer geo-contextual experiences today.</span></span> <span data-ttu-id="2c59c-133">S prvotřídní podporu pro [geoprostorové typy](geospatial.md), vytváření tyto činnosti snadno plnit díky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-133">With first-class support for [geospatial types](geospatial.md), Azure Cosmos DB makes creating these experiences easy to accomplish.</span></span>
* <span data-ttu-id="2c59c-134">Binární přílohy.</span><span class="sxs-lookup"><span data-stu-id="2c59c-134">Binary attachments.</span></span> <span data-ttu-id="2c59c-135">Data aplikací často obsahují binární objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-135">Your app data often includes binary blobs.</span></span> <span data-ttu-id="2c59c-136">Nativní podpora pro přílohy je jednodušší použít Azure Cosmos DB jako centralizované pro data aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c59c-136">Native support for attachments makes it easier to use Azure Cosmos DB as a one-stop shop for your app data.</span></span>

## <a name="azure-cosmos-db-and-xamarin-tutorial"></a><span data-ttu-id="2c59c-137">Kurz pro Azure Cosmos DB a Xamarin</span><span class="sxs-lookup"><span data-stu-id="2c59c-137">Azure Cosmos DB and Xamarin tutorial</span></span>
<span data-ttu-id="2c59c-138">Následující kurz ukazuje, jak vytvářet mobilních aplikací pomocí Xamarin a Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-138">The following tutorial shows how to build a mobile application by using Xamarin and Azure Cosmos DB.</span></span> <span data-ttu-id="2c59c-139">Můžete najít úplný zdrojový kód pro tento kurz v [Xamarin a Azure DB Cosmos na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).</span><span class="sxs-lookup"><span data-stu-id="2c59c-139">You can find the complete source code for the tutorial at [Xamarin and Azure Cosmos DB on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).</span></span>

### <a name="get-started"></a><span data-ttu-id="2c59c-140">Začínáme</span><span class="sxs-lookup"><span data-stu-id="2c59c-140">Get started</span></span>
<span data-ttu-id="2c59c-141">Je snadné začít s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-141">It's easy to get started with Azure Cosmos DB.</span></span> <span data-ttu-id="2c59c-142">Přejděte na portál Azure a vytvořit nový účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-142">Go to the Azure portal, and create a new Azure Cosmos DB account.</span></span> <span data-ttu-id="2c59c-143">Klikněte **úvodní** kartě. Stažení ukázky seznamu úkolů Xamarin Forms, který je již připojen k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-143">Click the **Quick start** tab. Download the Xamarin Forms to-do list sample that is already connected to your Azure Cosmos DB account.</span></span> 

![Azure Cosmos DB rychlý start pro mobilní aplikace](media/mobile-apps-with-xamarin/cosmos-db-quickstart.png)

<span data-ttu-id="2c59c-145">Nebo pokud máte existující aplikaci Xamarin, můžete přidat [balíčku NuGet pro Azure Cosmos DB](documentdb-sdk-dotnet-core.md).</span><span class="sxs-lookup"><span data-stu-id="2c59c-145">Or if you have an existing Xamarin app, you can add the [Azure Cosmos DB NuGet package](documentdb-sdk-dotnet-core.md).</span></span> <span data-ttu-id="2c59c-146">Podporuje Azure Cosmos DB Xamarin.IOS, Xamarin.Android, a Xamarin Forms sdílené knihovny.</span><span class="sxs-lookup"><span data-stu-id="2c59c-146">Azure Cosmos DB supports Xamarin.IOS, Xamarin.Android, and Xamarin Forms shared libraries.</span></span>

### <a name="work-with-data"></a><span data-ttu-id="2c59c-147">Práce s daty</span><span class="sxs-lookup"><span data-stu-id="2c59c-147">Work with data</span></span>
<span data-ttu-id="2c59c-148">Záznamy dat jsou uloženy v databázi Azure Cosmos jako schemaless dokumenty JSON v heterogenní kolekce.</span><span class="sxs-lookup"><span data-stu-id="2c59c-148">Your data records are stored in Azure Cosmos DB as schemaless JSON documents in heterogeneous collections.</span></span> <span data-ttu-id="2c59c-149">Dokumenty s jinou struktury můžete uložit ve stejné kolekci:</span><span class="sxs-lookup"><span data-stu-id="2c59c-149">You can store documents with different structures in the same collection:</span></span>

```cs
    var result = await client.CreateDocumentAsync(collectionLink, todoItem);
```

<span data-ttu-id="2c59c-150">V Xamarin projekty můžete použít dotazy na language-integrated nad datovým:</span><span class="sxs-lookup"><span data-stu-id="2c59c-150">In your Xamarin projects, you can use language-integrated queries over schemaless data:</span></span>

```cs
    var query = await client.CreateDocumentQuery<ToDoItem>(collectionLink)
                    .Where(todoItem => todoItem.Complete == false)
                    .AsDocumentQuery();

    Items = new List<TodoItem>();
    while (query.HasMoreResults) {
        Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
    }
```
### <a name="add-users"></a><span data-ttu-id="2c59c-151">Přidání uživatelů</span><span class="sxs-lookup"><span data-stu-id="2c59c-151">Add users</span></span>
<span data-ttu-id="2c59c-152">Jako mnoho získat Začínáme ukázky, Azure Cosmos DB ukázku, kterou jste stáhli ověřuje službě s použitím pevně zakódované hlavní klíč v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c59c-152">Like many get started samples, the Azure Cosmos DB sample you downloaded authenticates to the service by using a master key hardcoded in the app's code.</span></span> <span data-ttu-id="2c59c-153">Toto výchozí nastavení není vhodné pro aplikace, které chcete spustit kdekoliv s výjimkou na vaše místní emulátor.</span><span class="sxs-lookup"><span data-stu-id="2c59c-153">This default is not a good practice for an app you intend to run anywhere except on your local emulator.</span></span> <span data-ttu-id="2c59c-154">Pokud neoprávněný uživatel získat hlavního klíče, může dojít k ohrožení všechna data v účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-154">If an unauthorized user obtained the master key, all the data across your Azure Cosmos DB account could be compromised.</span></span> <span data-ttu-id="2c59c-155">Místo toho chcete aplikaci pro přístup k záznamů pouze pro přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c59c-155">Instead, you want your app to access only the records for the signed-in user.</span></span> <span data-ttu-id="2c59c-156">Azure Cosmos DB umožňuje vývojářům udělit aplikaci pro čtení nebo oprávnění pro čtení a zápis do kolekce, sada dokumenty seskupené podle klíč oddílu, nebo určitého dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2c59c-156">Azure Cosmos DB allows developers to grant application read or read/write permission to a collection, a set of documents grouped by a partition key, or a specific document.</span></span> 

<span data-ttu-id="2c59c-157">Tyto pokyny slouží k úpravě aplikace seznamu úkolů do aplikace seznamu úkolů s více uživateli:</span><span class="sxs-lookup"><span data-stu-id="2c59c-157">Follow these steps to modify the to-do list app to a multiuser to-do list app:</span></span> 

  1. <span data-ttu-id="2c59c-158">Přidání přihlášení do aplikace pomocí Facebooku, služby Active Directory nebo jiného poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="2c59c-158">Add Login to your app by using Facebook, Active Directory, or any other provider.</span></span>

  2. <span data-ttu-id="2c59c-159">Vytvořte kolekci Azure Cosmos DB UserItems s **/UserId** jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="2c59c-159">Create an Azure Cosmos DB UserItems collection with **/userId** as the partition key.</span></span> <span data-ttu-id="2c59c-160">Zadávání klíče oddílů pro vaše kolekce umožňuje Azure DB Cosmos škálování nekonečnou roste počet uživatelů vaší aplikace, můžete nadále nabízejí rychlý dotazy.</span><span class="sxs-lookup"><span data-stu-id="2c59c-160">Specifying the partition key for your collection allows Azure Cosmos DB to scale infinitely as the number of your app users grows, while continuing to offer fast queries.</span></span>

  3. <span data-ttu-id="2c59c-161">Přidejte Token zprostředkovatele prostředků Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-161">Add Azure Cosmos DB Resource Token Broker.</span></span> <span data-ttu-id="2c59c-162">Tento jednoduchý webového rozhraní API ověří uživatele a problémy krátkodobou tokeny pro přihlášeného uživatele s přístup jenom k dokumentům v rámci svého oddílu.</span><span class="sxs-lookup"><span data-stu-id="2c59c-162">This simple Web API authenticates users and issues short-lived tokens to signed-in users with access only to the documents within their partition.</span></span> <span data-ttu-id="2c59c-163">V tomto příkladu je Token zprostředkovatele prostředků hostované ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="2c59c-163">In this example, Resource Token Broker is hosted in App Service.</span></span>

  4. <span data-ttu-id="2c59c-164">Upravte aplikace k ověření tokenu zprostředkovatele prostředků se sítí Facebook a žádosti o tokeny prostředků pro přihlášeného uživatele služby Facebook.</span><span class="sxs-lookup"><span data-stu-id="2c59c-164">Modify the app to authenticate to Resource Token Broker with Facebook, and request the resource tokens for the signed-in Facebook users.</span></span> <span data-ttu-id="2c59c-165">Potom můžete přistupovat k datům v kolekci UserItems.</span><span class="sxs-lookup"><span data-stu-id="2c59c-165">You can then access their data in the UserItems collection.</span></span>  

<span data-ttu-id="2c59c-166">Najdete kompletní příklad tohoto vzoru v [tokenu zprostředkovatele prostředků na Githubu](http://aka.ms/documentdb-xamarin-todouser).</span><span class="sxs-lookup"><span data-stu-id="2c59c-166">You can find a complete code sample of this pattern at [Resource Token Broker on GitHub](http://aka.ms/documentdb-xamarin-todouser).</span></span> <span data-ttu-id="2c59c-167">Tento diagram znázorňuje řešení:</span><span class="sxs-lookup"><span data-stu-id="2c59c-167">This diagram illustrates the solution:</span></span>

![Zprostředkovatel uživatele a oprávnění Azure Cosmos DB](media/mobile-apps-with-xamarin/documentdb-resource-token-broker.png)

<span data-ttu-id="2c59c-169">Pokud chcete dva uživatelé měli přístup k seznamu úkolů, můžete přidat další oprávnění k přístupu k tokenu v tokenu zprostředkovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="2c59c-169">If you want two users to have access to the same to-do list, you can add additional permissions to the access token in Resource Token Broker.</span></span>

### <a name="scale-on-demand"></a><span data-ttu-id="2c59c-170">Škálování na vyžádání</span><span class="sxs-lookup"><span data-stu-id="2c59c-170">Scale on demand</span></span>
<span data-ttu-id="2c59c-171">Azure Cosmos DB je spravovaný databáze jako služba.</span><span class="sxs-lookup"><span data-stu-id="2c59c-171">Azure Cosmos DB is a managed database as a service.</span></span> <span data-ttu-id="2c59c-172">S růstem vaší uživatelské základny, nemusíte si dělat starosti o zřizování virtuálních počítačů nebo zvýšení jader.</span><span class="sxs-lookup"><span data-stu-id="2c59c-172">As your user base grows, you don't need to worry about provisioning VMs or increasing cores.</span></span> <span data-ttu-id="2c59c-173">Všechny budete muset zadat databázi Cosmos Azure je vaše aplikace musí kolik operací za sekundu (propustnost).</span><span class="sxs-lookup"><span data-stu-id="2c59c-173">All you need to tell Azure Cosmos DB is how many operations per second (throughput) your app needs.</span></span> <span data-ttu-id="2c59c-174">Můžete zadat propustnost prostřednictvím **škálování** karta pomocí míru propustnosti názvem jednotky žádosti (ruština) za sekundu.</span><span class="sxs-lookup"><span data-stu-id="2c59c-174">You can specify the throughput via the **Scale** tab by using a measure of throughput called Request Units (RUs) per second.</span></span> <span data-ttu-id="2c59c-175">Například operace čtení pro 1 KB dokumentu vyžaduje 1 RU.</span><span class="sxs-lookup"><span data-stu-id="2c59c-175">For example, a read operation on a 1-KB document requires 1 RU.</span></span> <span data-ttu-id="2c59c-176">Můžete také přidat výstrahy, které **propustnost** metrika se používá k monitorování provozu růstu a prostřednictvím kódu programu změnit propustnost jako výstrahy ještě efektivněji.</span><span class="sxs-lookup"><span data-stu-id="2c59c-176">You can also add alerts to the **Throughput** metric to monitor the traffic growth and programmatically change the throughput as alerts fire.</span></span>

![Azure Cosmos DB škálování propustnosti na vyžádání](media/mobile-apps-with-xamarin/cosmos-db-xamarin-scale.png)

### <a name="go-planet-scale"></a><span data-ttu-id="2c59c-178">Přejděte planetu škálování</span><span class="sxs-lookup"><span data-stu-id="2c59c-178">Go planet scale</span></span>
<span data-ttu-id="2c59c-179">Jako oblíbenosti zvýšení vaší aplikace může získat uživatelé po celém světě.</span><span class="sxs-lookup"><span data-stu-id="2c59c-179">As your app gains popularity, you might gain users across the globe.</span></span> <span data-ttu-id="2c59c-180">Nebo možná chcete připravit pro nepředpokládaného události.</span><span class="sxs-lookup"><span data-stu-id="2c59c-180">Or maybe you want to be prepared for unforeseen events.</span></span> <span data-ttu-id="2c59c-181">Přejděte na portál Azure a otevřete váš účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-181">Go to the Azure portal, and open your Azure Cosmos DB account.</span></span> <span data-ttu-id="2c59c-182">Klikněte na tlačítko mapu, aby vaše data nepřetržitě replikace pro libovolný počet oblastech po celém světě.</span><span class="sxs-lookup"><span data-stu-id="2c59c-182">Click the map to make your data continuously replicate to any number of regions across the world.</span></span> <span data-ttu-id="2c59c-183">Tato funkce zpřístupňuje data, kde jsou vaši uživatelé.</span><span class="sxs-lookup"><span data-stu-id="2c59c-183">This capability makes your data available wherever your users are.</span></span> <span data-ttu-id="2c59c-184">Můžete také přidat zásady převzetí služeb při selhání, abyste byli připraveni pro událostí odpovídajících odvětvím.</span><span class="sxs-lookup"><span data-stu-id="2c59c-184">You can also add failover policies to be prepared for contingencies.</span></span>

![Azure Cosmos DB škálování napříč zeměpisné oblasti](media/mobile-apps-with-xamarin/cosmos-db-xamarin-replicate.png)

<span data-ttu-id="2c59c-186">Blahopřejeme.</span><span class="sxs-lookup"><span data-stu-id="2c59c-186">Congratulations.</span></span> <span data-ttu-id="2c59c-187">Byly dokončeny řešení a mít mobilní aplikaci s Xamarin a Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-187">You have completed the solution and have a mobile app with Xamarin and Azure Cosmos DB.</span></span> <span data-ttu-id="2c59c-188">Postupujte podle podobným způsobem můžete vytvářet aplikace Cordova pomocí sady Azure Cosmos DB JavaScript SDK a nativní aplikace iOS nebo Android pomocí rozhraní API REST Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c59c-188">Follow similar steps to build Cordova apps by using the Azure Cosmos DB JavaScript SDK and native iOS/Android apps by using Azure Cosmos DB REST APIs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c59c-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c59c-189">Next steps</span></span>
* <span data-ttu-id="2c59c-190">Zobrazit zdrojový kód pro [Xamarin a Azure DB Cosmos na Githubu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).</span><span class="sxs-lookup"><span data-stu-id="2c59c-190">View the source code for [Xamarin and Azure Cosmos DB on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).</span></span>
* <span data-ttu-id="2c59c-191">Stažení [Cosmos Azure DB .NET Core SDK](documentdb-sdk-dotnet-core.md).</span><span class="sxs-lookup"><span data-stu-id="2c59c-191">Download the [Azure Cosmos DB .NET Core SDK](documentdb-sdk-dotnet-core.md).</span></span>
* <span data-ttu-id="2c59c-192">Najít další ukázky kódu pro [aplikací .NET](documentdb-dotnet-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2c59c-192">Find more code samples for [.NET applications](documentdb-dotnet-samples.md).</span></span>
* <span data-ttu-id="2c59c-193">Další informace o [Azure Cosmos DB bohaté dotazování možnosti](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="2c59c-193">Learn about [Azure Cosmos DB rich query capabilities](documentdb-sql-query.md).</span></span>
* <span data-ttu-id="2c59c-194">Další informace o [geoprostorové podpory v Azure Cosmos DB](geospatial.md).</span><span class="sxs-lookup"><span data-stu-id="2c59c-194">Learn about [geospatial support in Azure Cosmos DB](geospatial.md).</span></span>


