---
title: "Azure Cosmos DB: Vývoj s tabulkou rozhraní API v rozhraní .NET | Microsoft Docs"
description: "Naučte se vyvíjet s rozhraním API Azure Cosmos DB Table pomocí rozhraní .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 52cb5f2569b6c3a5301752b1e8bfb6cea13ff7f6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a><span data-ttu-id="47ae0-103">Azure Cosmos DB: Vývoj s tabulkou rozhraní API v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="47ae0-103">Azure Cosmos DB: Develop with the Table API in .NET</span></span>

<span data-ttu-id="47ae0-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="47ae0-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="47ae0-105">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="47ae0-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span>

<span data-ttu-id="47ae0-106">Tento kurz obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="47ae0-106">This tutorial covers the following tasks:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="47ae0-107">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="47ae0-107">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="47ae0-108">Povolit funkci v souboru app.config</span><span class="sxs-lookup"><span data-stu-id="47ae0-108">Enable functionality in the app.config file</span></span> 
> * <span data-ttu-id="47ae0-109">Vytvoření tabulky pomocí [tabulky API](table-introduction.md) (preview)</span><span class="sxs-lookup"><span data-stu-id="47ae0-109">Create a table using the [Table API](table-introduction.md) (preview)</span></span>
> * <span data-ttu-id="47ae0-110">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="47ae0-110">Add an entity to a table</span></span> 
> * <span data-ttu-id="47ae0-111">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="47ae0-111">Insert a batch of entities</span></span> 
> * <span data-ttu-id="47ae0-112">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="47ae0-112">Retrieve a single entity</span></span> 
> * <span data-ttu-id="47ae0-113">Entity dotazu pomocí automatické sekundární indexy</span><span class="sxs-lookup"><span data-stu-id="47ae0-113">Query entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="47ae0-114">Nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="47ae0-114">Replace an entity</span></span> 
> * <span data-ttu-id="47ae0-115">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="47ae0-115">Delete an entity</span></span> 
> * <span data-ttu-id="47ae0-116">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="47ae0-116">Delete a table</span></span>
 
## <a name="tables-in-azure-cosmos-db"></a><span data-ttu-id="47ae0-117">Tabulky v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="47ae0-117">Tables in Azure Cosmos DB</span></span> 

<span data-ttu-id="47ae0-118">Poskytuje Azure Cosmos DB [tabulky API](table-introduction.md) (Náhled) pro aplikace, které potřebují úložiště dvojic klíč hodnota s návrhem bez schématu.</span><span class="sxs-lookup"><span data-stu-id="47ae0-118">Azure Cosmos DB provides the [Table API](table-introduction.md) (preview) for applications that need a key-value store with a schema-less design.</span></span> <span data-ttu-id="47ae0-119">Pro práci s Azure Cosmos DB lze použít sady SDK [Azure Table Storage](../storage/common/storage-introduction.md) a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="47ae0-119">[Azure Table storage](../storage/common/storage-introduction.md) SDKs and REST APIs can be used to work with Azure Cosmos DB.</span></span> <span data-ttu-id="47ae0-120">Azure Cosmos DB můžete použít k vytvoření tabulek s požadavky na vysokou propustnost.</span><span class="sxs-lookup"><span data-stu-id="47ae0-120">You can use Azure Cosmos DB to create tables with high throughput requirements.</span></span> <span data-ttu-id="47ae0-121">Azure Cosmos DB podporuje tabulky s optimalizovanou propustností (neformálně označované jako „tabulky Premium“), aktuálně ve verzi Public Preview.</span><span class="sxs-lookup"><span data-stu-id="47ae0-121">Azure Cosmos DB supports throughput-optimized tables (informally called "premium tables"), currently in public preview.</span></span> 

<span data-ttu-id="47ae0-122">Azure Table Storage můžete dále používat pro tabulky s vysokými požadavky na úložiště a nižšími nároky na propustnost.</span><span class="sxs-lookup"><span data-stu-id="47ae0-122">You can continue to use Azure Table storage for tables with high storage and lower throughput requirements.</span></span> <span data-ttu-id="47ae0-123">Azure Cosmos DB zavádí podporu pro úložiště optimalizované tabulky v budoucí aktualizaci a stávající i nové účty úložiště Azure Table se bezproblémově upgraduje databázi Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="47ae0-123">Azure Cosmos DB will introduce support for storage-optimized tables in a future update, and existing and new Azure Table storage accounts will be seamlessly upgraded to Azure Cosmos DB.</span></span>

<span data-ttu-id="47ae0-124">Pokud aktuálně používáte Azure Table storage, získáte následující výhody s náhledem "premium tabulka":</span><span class="sxs-lookup"><span data-stu-id="47ae0-124">If you currently use Azure Table storage, you gain the following benefits with the "premium table" preview:</span></span>

- <span data-ttu-id="47ae0-125">Klíč [globální distribuční](distribute-data-globally.md) s více domovských stránek a [automatickou a ruční převzetí služeb při selhání](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="47ae0-125">Turn-key [global distribution](distribute-data-globally.md) with multi-homing and [automatic and manual failovers](regional-failover.md)</span></span>
- <span data-ttu-id="47ae0-126">Podpora pro automatické indexování pro všechny vlastnosti (dále jen "sekundární indexy") a rychlé dotazy schématu vznikl</span><span class="sxs-lookup"><span data-stu-id="47ae0-126">Support for automatic schema-agnostic indexing against all properties ("secondary indexes"), and fast queries</span></span> 
- <span data-ttu-id="47ae0-127">Podpora pro [nezávislé škálování úložiště a propustnost](partition-data.md), napříč libovolný počet oblastí</span><span class="sxs-lookup"><span data-stu-id="47ae0-127">Support for [independent scaling of storage and throughput](partition-data.md), across any number of regions</span></span>
- <span data-ttu-id="47ae0-128">Podpora pro [vyhrazené propustnost za tabulky](request-units.md) , je možné rozšířit stovek na miliony požadavků za sekundu</span><span class="sxs-lookup"><span data-stu-id="47ae0-128">Support for [dedicated throughput per table](request-units.md) that can be scaled from hundreds to millions of requests per second</span></span>
- <span data-ttu-id="47ae0-129">Podpora pro [pět přizpůsobitelné úrovně konzistence](consistency-levels.md) pro kompromisy dostupnost, latence a konzistence závisí na vaší aplikaci musí</span><span class="sxs-lookup"><span data-stu-id="47ae0-129">Support for [five tunable consistency levels](consistency-levels.md) to trade off availability, latency, and consistency based on your application needs</span></span>
- <span data-ttu-id="47ae0-130">99,99 % dostupnost v rámci jedné oblasti a možnost přidávat další oblasti pro vyšší dostupnosti, a [komplexní SLA špičkový](https://azure.microsoft.com/support/legal/sla/cosmos-db/) na obecné dostupnosti</span><span class="sxs-lookup"><span data-stu-id="47ae0-130">99.99% availability within a single region, and ability to add more regions for higher availability, and [industry-leading comprehensive SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) on general availability</span></span>
- <span data-ttu-id="47ae0-131">Práce s existující úložiště Azure .NET SDK a beze změn kódu do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="47ae0-131">Work with the existing Azure storage .NET SDK, and no code changes to your application</span></span>

<span data-ttu-id="47ae0-132">Ve verzi Preview podporuje Azure Cosmos DB rozhraní API tabulky pomocí sady .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="47ae0-132">During the preview, Azure Cosmos DB supports the Table API using the .NET SDK.</span></span> <span data-ttu-id="47ae0-133">Si můžete stáhnout [SDK Preview úložiště Azure](https://aka.ms/premiumtablenuget) z NuGet, který má stejné třídy a metody podpisy jako [sada SDK úložiště Azure](https://www.nuget.org/packages/WindowsAzure.Storage), ale také může připojit k Azure Cosmos DB účty pomocí rozhraní API tabulky.</span><span class="sxs-lookup"><span data-stu-id="47ae0-133">You can download the [Azure Storage Preview SDK](https://aka.ms/premiumtablenuget) from NuGet, that has the same classes and method signatures as the [Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), but also can connect to Azure Cosmos DB accounts using the Table API.</span></span>

<span data-ttu-id="47ae0-134">Další informace o složité úlohy Azure Table storage, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="47ae0-134">To learn more about complex Azure Table storage tasks, see:</span></span>

* [<span data-ttu-id="47ae0-135">Úvod do Azure Cosmos DB: tabulky rozhraní API</span><span class="sxs-lookup"><span data-stu-id="47ae0-135">Introduction to Azure Cosmos DB: Table API</span></span>](table-introduction.md)
* <span data-ttu-id="47ae0-136">V tabulce referenční dokumentaci ke službě kompletní informace o dostupných rozhraních API [Klientská knihovna pro úložiště pro .NET – referenční informace](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="47ae0-136">The Table service reference documentation for complete details about available APIs [Storage Client Library for .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="47ae0-137">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="47ae0-137">About this tutorial</span></span>
<span data-ttu-id="47ae0-138">V tomto kurzu pro vývojáře, kteří se seznámíte s Azure Table storage SDK a chcete použít k dispozici prémiové funkce používá Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="47ae0-138">This tutorial is for developers who are familiar with the Azure Table storage SDK, and would like to use the premium features available using Azure Cosmos DB.</span></span> <span data-ttu-id="47ae0-139">Je založena na [Začínáme s Azure Table storage pomocí rozhraní .NET](table-storage-how-to-use-dotnet.md) a ukazuje, jak využít další možnosti jako sekundární indexy, zřízená propustnost a více domovských stránek.</span><span class="sxs-lookup"><span data-stu-id="47ae0-139">It is based on [Get Started with Azure Table storage using .NET](table-storage-how-to-use-dotnet.md) and shows how to take advantage of additional capabilities like secondary indexes, provisioned throughput, and multi-homing.</span></span> <span data-ttu-id="47ae0-140">Nabídneme použití portálu Azure k vytvoření účtu Azure Cosmos DB sestavení a nasazení aplikace tabulky.</span><span class="sxs-lookup"><span data-stu-id="47ae0-140">We cover how to use the Azure portal to create an Azure Cosmos DB account, and then build and deploy a Table application.</span></span> <span data-ttu-id="47ae0-141">Můžeme také provede příklady .NET pro vytváření a odstraňování tabulek a vkládání, aktualizaci, odstranění a dotazování dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="47ae0-141">We also walk through .NET examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span> 

<span data-ttu-id="47ae0-142">Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="47ae0-142">If you don't already have Visual Studio 2017 installed, you can download and use the **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="47ae0-143">Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.</span><span class="sxs-lookup"><span data-stu-id="47ae0-143">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="47ae0-144">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="47ae0-144">Create a database account</span></span>

<span data-ttu-id="47ae0-145">Začněme vytvořením účtu Azure Cosmos DB na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="47ae0-145">Let's start by creating an Azure Cosmos DB account in the Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="47ae0-146">Již máte účet Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="47ae0-146">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="47ae0-147">Pokud ano, přeskočit na [nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="47ae0-147">If so, skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>
> * <span data-ttu-id="47ae0-148">Měli jste účet Azure DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="47ae0-148">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="47ae0-149">Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit na [nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="47ae0-149">If so, your account is now an Azure Cosmos DB account and you can skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="47ae0-150">Pokud používáte emulátor DB Cosmos Azure, postupujte podle kroků v [emulátoru DB Cosmos Azure](local-emulator.md) nastavit emulátoru a přeskočit na [nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="47ae0-150">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Visual Studio Solution](#SetupVS).</span></span>
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting to any location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a><span data-ttu-id="47ae0-151">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="47ae0-151">Clone the sample application</span></span>

<span data-ttu-id="47ae0-152">Teď naklonujeme aplikaci Table z GitHubu, nastavíme připojovací řetězec a spustíme ji.</span><span class="sxs-lookup"><span data-stu-id="47ae0-152">Now let's clone a Table app from github, set the connection string, and run it.</span></span>

1. <span data-ttu-id="47ae0-153">Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="47ae0-153">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

2. <span data-ttu-id="47ae0-154">Ukázkové úložiště naklonujete spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="47ae0-154">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. <span data-ttu-id="47ae0-155">Potom otevřete soubor řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="47ae0-155">Then open the solution file in Visual Studio.</span></span>

## <a name="update-your-connection-string"></a><span data-ttu-id="47ae0-156">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="47ae0-156">Update your connection string</span></span>

<span data-ttu-id="47ae0-157">Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.</span><span class="sxs-lookup"><span data-stu-id="47ae0-157">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="47ae0-158">Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Klíče** a potom klikněte na **Klíče pro čtení i zápis**.</span><span class="sxs-lookup"><span data-stu-id="47ae0-158">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="47ae0-159">Pomocí tlačítek kopírovat na pravé straně obrazovky budete zkopírujte připojovací řetězec do souboru app.config v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="47ae0-159">You'll use the copy buttons on the right side of the screen to copy the connection string into the app.config file in the next step.</span></span>

2. <span data-ttu-id="47ae0-160">V sadě Visual Studio otevřete soubor app.config.</span><span class="sxs-lookup"><span data-stu-id="47ae0-160">In Visual Studio, open the app.config file.</span></span> 

3. <span data-ttu-id="47ae0-161">Zkopírujte URI hodnota z portálu (pomocí tlačítko Kopírovat) a nastavit jej jako hodnota klíč účtu v souboru app.config. Použijte název účtu, který je vytvořený pro název účtu v souboru app.config.</span><span class="sxs-lookup"><span data-stu-id="47ae0-161">Copy your URI value from the portal (using the copy button) and make it the value of the account-key in app.config. Use the account name created earlier for account-name in app.config.</span></span>
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> <span data-ttu-id="47ae0-162">Chcete-li používat tuto aplikaci s standardní Azure Table Storage, je potřeba změnit připojovací řetězec v `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-162">To use this app with standard Azure Table Storage, you need to change the connection string in `app.config file`.</span></span> <span data-ttu-id="47ae0-163">Název účtu použijte jako název tabulky účtu a klíč jako Azure úložiště primární klíč.</span><span class="sxs-lookup"><span data-stu-id="47ae0-163">Use the account name as Table-account name and key as Azure Storage Primary key.</span></span> <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-the-app"></a><span data-ttu-id="47ae0-164">Sestavení a nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="47ae0-164">Build and deploy the app</span></span>
1. <span data-ttu-id="47ae0-165">V sadě Visual Studio klikněte v **Průzkumníku řešení** pravým tlačítkem myši na projekt a potom klikněte na **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="47ae0-165">In Visual Studio, right-click on the project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="47ae0-166">Do pole **Procházet** v NuGetu zadejte ***WindowsAzure.Storage-PremiumTable***.</span><span class="sxs-lookup"><span data-stu-id="47ae0-166">In the NuGet **Browse** box, type ***WindowsAzure.Storage-PremiumTable***.</span></span> <span data-ttu-id="47ae0-167">Zkontrolujte **zahrnují předprodejní verze**.</span><span class="sxs-lookup"><span data-stu-id="47ae0-167">Check **Include prerelease versions**.</span></span>

3. <span data-ttu-id="47ae0-168">Ve výsledcích nainstalovat **WindowsAzure.Storage PremiumTable** a zvolte buildu preview `0.0.1-preview`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-168">From the results, install the **WindowsAzure.Storage-PremiumTable** and choose the preview build `0.0.1-preview`.</span></span> <span data-ttu-id="47ae0-169">Tato akce nainstaluje balíček úložiště Azure Table a všechny závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="47ae0-169">This action installs the Azure Table storage package and all dependencies.</span></span>

4. <span data-ttu-id="47ae0-170">Spusťte aplikaci stisknutím CTRL+F5.</span><span class="sxs-lookup"><span data-stu-id="47ae0-170">Click CTRL + F5 to run the application.</span></span> 

<span data-ttu-id="47ae0-171">Teď můžete přejít zpět do Průzkumníku dat a zobrazit dotaz, upravit a práci s daty této tabulky.</span><span class="sxs-lookup"><span data-stu-id="47ae0-171">You can now go back to Data Explorer and see query, modify, and work with this table data.</span></span> 

> [!NOTE]
> <span data-ttu-id="47ae0-172">Pro použití této aplikace pomocí emulátoru DB Cosmos Azure, stejně musíte změnit připojovací řetězec v `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-172">To use this app with an Azure Cosmos DB Emulator, you just need to change the connection string in `app.config file`.</span></span> <span data-ttu-id="47ae0-173">Použijte menší než hodnota pro emulátor.</span><span class="sxs-lookup"><span data-stu-id="47ae0-173">Use the below value for emulator.</span></span> <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a><span data-ttu-id="47ae0-174">Možnosti Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="47ae0-174">Azure Cosmos DB capabilities</span></span>
<span data-ttu-id="47ae0-175">Azure Cosmos DB podporuje několik možností, které nejsou k dispozici ve službě Azure Table storage rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="47ae0-175">Azure Cosmos DB supports a number of capabilities that are not available in the Azure Table storage API.</span></span> <span data-ttu-id="47ae0-176">Nové funkce se dá povolit buď následující `appSettings` hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="47ae0-176">The new functionality can be enabled via the following `appSettings` configuration values.</span></span> <span data-ttu-id="47ae0-177">Zavedeme nejsou žádné nové podpisy nebo přetížení do verze Preview SDK úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="47ae0-177">We did not introduce any new signatures or overloads to the preview Azure Storage SDK.</span></span> <span data-ttu-id="47ae0-178">To umožňuje připojit standard a premium tabulky a pracovat s jinými službami Azure Storage jako objekty BLOB a fronty.</span><span class="sxs-lookup"><span data-stu-id="47ae0-178">This allows you to connect to both standard and premium tables, and work with other Azure Storage services like Blobs and Queues.</span></span> 


| <span data-ttu-id="47ae0-179">Klíč</span><span class="sxs-lookup"><span data-stu-id="47ae0-179">Key</span></span> | <span data-ttu-id="47ae0-180">Popis</span><span class="sxs-lookup"><span data-stu-id="47ae0-180">Description</span></span> |
| --- | --- |
| <span data-ttu-id="47ae0-181">TableConnectionMode</span><span class="sxs-lookup"><span data-stu-id="47ae0-181">TableConnectionMode</span></span>  | <span data-ttu-id="47ae0-182">Azure Cosmos DB podporuje dva režimy připojení.</span><span class="sxs-lookup"><span data-stu-id="47ae0-182">Azure Cosmos DB supports two connectivity modes.</span></span> <span data-ttu-id="47ae0-183">V `Gateway` režim, jsou vždy vytvářeny požadavky k bráně Azure Cosmos DB, který předává do oddílů odpovídající data.</span><span class="sxs-lookup"><span data-stu-id="47ae0-183">In `Gateway` mode, requests are always made to the Azure Cosmos DB gateway, which forwards it to the corresponding data partitions.</span></span> <span data-ttu-id="47ae0-184">V `Direct` režim připojení, klient načte mapování tabulek do oddílů a jsou vytvářeny požadavky přímo na oddíly data.</span><span class="sxs-lookup"><span data-stu-id="47ae0-184">In `Direct` connectivity mode, the client fetches the mapping of tables to partitions, and requests are made directly against data partitions.</span></span> <span data-ttu-id="47ae0-185">Doporučujeme, abyste `Direct`, výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="47ae0-185">We recommend `Direct`, the default.</span></span>  |
| <span data-ttu-id="47ae0-186">TableConnectionProtocol</span><span class="sxs-lookup"><span data-stu-id="47ae0-186">TableConnectionProtocol</span></span> | <span data-ttu-id="47ae0-187">Azure Cosmos DB podporuje dva protokoly připojení - `Https` a `Tcp`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-187">Azure Cosmos DB supports two connection protocols - `Https` and `Tcp`.</span></span> <span data-ttu-id="47ae0-188">`Tcp`je výchozí a doporučuje, protože je jednodušší.</span><span class="sxs-lookup"><span data-stu-id="47ae0-188">`Tcp` is the default, and recommended because it is more lightweight.</span></span> |
| <span data-ttu-id="47ae0-189">TablePreferredLocations</span><span class="sxs-lookup"><span data-stu-id="47ae0-189">TablePreferredLocations</span></span> | <span data-ttu-id="47ae0-190">Čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení.</span><span class="sxs-lookup"><span data-stu-id="47ae0-190">Comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="47ae0-191">Každý účet Azure Cosmos DB lze přidružit 1 – 30 + oblasti.</span><span class="sxs-lookup"><span data-stu-id="47ae0-191">Each Azure Cosmos DB account can be associated with 1-30+ regions.</span></span> <span data-ttu-id="47ae0-192">Každá instance klienta můžete určit podmnožinu těchto oblastí v upřednostňovaném pořadí pro čtení s nízkou latencí.</span><span class="sxs-lookup"><span data-stu-id="47ae0-192">Each client instance can specify a subset of these regions in the preferred order for low latency reads.</span></span> <span data-ttu-id="47ae0-193">Oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-193">The regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span> <span data-ttu-id="47ae0-194">Viz také [více domovských stránek rozhraní API](tutorial-global-distribution-table.md).</span><span class="sxs-lookup"><span data-stu-id="47ae0-194">Also see [Multi-homing APIs](tutorial-global-distribution-table.md).</span></span>
| <span data-ttu-id="47ae0-195">TableConsistencyLevel</span><span class="sxs-lookup"><span data-stu-id="47ae0-195">TableConsistencyLevel</span></span> | <span data-ttu-id="47ae0-196">Můžete se kompromisy mezi latence, konzistence a dostupnosti výběrem mezi pěti dobře definované úrovně konzistence: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, a `Eventual`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-196">You can trade off between latency, consistency, and availability by choosing between five well-defined consistency levels: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, and `Eventual`.</span></span> <span data-ttu-id="47ae0-197">Výchozí hodnota je `Session`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-197">Default is `Session`.</span></span> <span data-ttu-id="47ae0-198">Volba úrovně konzistence díky významně zvýšit výkon rozdíl ve více oblastech nastavení.</span><span class="sxs-lookup"><span data-stu-id="47ae0-198">The choice of consistency level makes a significant performance difference in multi-region setups.</span></span> <span data-ttu-id="47ae0-199">V tématu [úrovně konzistence](consistency-levels.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="47ae0-199">See [Consistency levels](consistency-levels.md) for details.</span></span> |
| <span data-ttu-id="47ae0-200">TableThroughput</span><span class="sxs-lookup"><span data-stu-id="47ae0-200">TableThroughput</span></span> | <span data-ttu-id="47ae0-201">Vyhrazenou propustností pro tabulku vyjádřené v jednotek žádosti (RU) za sekundu.</span><span class="sxs-lookup"><span data-stu-id="47ae0-201">Reserved throughput for the table expressed in request units (RU) per second.</span></span> <span data-ttu-id="47ae0-202">Jedné tabulky může podporovat 100s miliony RU/s.</span><span class="sxs-lookup"><span data-stu-id="47ae0-202">Single tables can support 100s-millions of RU/s.</span></span> <span data-ttu-id="47ae0-203">V tématu [požadované jednotky](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="47ae0-203">See [Request units](request-units.md).</span></span> <span data-ttu-id="47ae0-204">Výchozí hodnota je`400`</span><span class="sxs-lookup"><span data-stu-id="47ae0-204">Default is `400`</span></span> |
| <span data-ttu-id="47ae0-205">TableIndexingPolicy</span><span class="sxs-lookup"><span data-stu-id="47ae0-205">TableIndexingPolicy</span></span> | <span data-ttu-id="47ae0-206">Konzistentní a automatické sekundární indexování všech sloupců v rámci tabulky</span><span class="sxs-lookup"><span data-stu-id="47ae0-206">Consistent and automatic secondary indexing of all columns within tables</span></span> | <span data-ttu-id="47ae0-207">Řetězce formátu JSON, které odpovídají specifikace zásady indexování.</span><span class="sxs-lookup"><span data-stu-id="47ae0-207">JSON string conforming to the indexing policy specification.</span></span> <span data-ttu-id="47ae0-208">V tématu [zásady indexování](indexing-policies.md) zobrazíte, jak můžete změnit zásady indexování pro zahrnutí a vyloučení určité sloupce.</span><span class="sxs-lookup"><span data-stu-id="47ae0-208">See [Indexing Policy](indexing-policies.md) to see how you can change indexing policy to include/exclude specific columns.</span></span> | <span data-ttu-id="47ae0-209">Automatické indexování všech vlastností (hodnota hash pro řetězce) a rozsah čísel</span><span class="sxs-lookup"><span data-stu-id="47ae0-209">Automatic indexing of all properties (hash for strings, and range for numbers)</span></span> |
| <span data-ttu-id="47ae0-210">TableQueryMaxItemCount</span><span class="sxs-lookup"><span data-stu-id="47ae0-210">TableQueryMaxItemCount</span></span> | <span data-ttu-id="47ae0-211">Nakonfigurujte maximální počet položek vrátí na dotaz tabulky v jednom dobu odezvy.</span><span class="sxs-lookup"><span data-stu-id="47ae0-211">Configure the maximum number of items returned per table query in a single round trip.</span></span> <span data-ttu-id="47ae0-212">Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu za běhu.</span><span class="sxs-lookup"><span data-stu-id="47ae0-212">Default is `-1`, which lets Azure Cosmos DB dynamically determine the value at runtime.</span></span> |
| <span data-ttu-id="47ae0-213">TableQueryEnableScan</span><span class="sxs-lookup"><span data-stu-id="47ae0-213">TableQueryEnableScan</span></span> | <span data-ttu-id="47ae0-214">Pokud dotaz nelze použít pro libovolný filtr index, spusťte ji přesto prostřednictvím kontrolu.</span><span class="sxs-lookup"><span data-stu-id="47ae0-214">If the query cannot use the index for any filter, then run it anyway via a scan.</span></span> <span data-ttu-id="47ae0-215">Výchozí hodnota je `false`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-215">Default is `false`.</span></span>|
| <span data-ttu-id="47ae0-216">TableQueryMaxDegreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="47ae0-216">TableQueryMaxDegreeOfParallelism</span></span> | <span data-ttu-id="47ae0-217">Stupně paralelního zpracování pro spuštění dotazu mezi oddílu.</span><span class="sxs-lookup"><span data-stu-id="47ae0-217">The degree of parallelism for execution of a cross-partition query.</span></span> <span data-ttu-id="47ae0-218">`0`sériový s žádné předem načítání, `1` je sériové s předem načítání a vyšší hodnoty zvýšit rychlost paralelního zpracování.</span><span class="sxs-lookup"><span data-stu-id="47ae0-218">`0` is serial with no pre-fetching, `1` is serial with pre-fetching, and higher values increase the rate of parallelism.</span></span> <span data-ttu-id="47ae0-219">Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu za běhu.</span><span class="sxs-lookup"><span data-stu-id="47ae0-219">Default is `-1`, which lets Azure Cosmos DB dynamically determine the value at runtime.</span></span> |

<span data-ttu-id="47ae0-220">Chcete-li změnit výchozí hodnotu, otevřete `app.config` soubor v Průzkumníku řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="47ae0-220">To change the default value, open the `app.config` file from Solution Explorer in Visual Studio.</span></span> <span data-ttu-id="47ae0-221">Přidejte obsah níže uvedeného prvku `<appSettings>`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-221">Add the contents of the `<appSettings>` element shown below.</span></span> <span data-ttu-id="47ae0-222">Nahraďte `account-name` s názvem účtu úložiště a `account-key` nahraďte svým klíčem účtu přístup.</span><span class="sxs-lookup"><span data-stu-id="47ae0-222">Replace `account-name` with the name of your storage account, and `account-key` with your account access key.</span></span> 

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
      <!-- Client options -->
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />
      <add key="TableConnectionMode" value="Direct"/>
      <add key="TableConnectionProtocol" value="Tcp"/>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>
      <add key="TableConsistencyLevel" value="Eventual"/>

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}"/>

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

<span data-ttu-id="47ae0-223">Ještě jednou se stručně podívejme na to, co se v aplikaci děje.</span><span class="sxs-lookup"><span data-stu-id="47ae0-223">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="47ae0-224">Otevřete `Program.cs` souboru a najít, že tyto řádky kódu vytvořit tabulku prostředky.</span><span class="sxs-lookup"><span data-stu-id="47ae0-224">Open the `Program.cs` file and you find that these lines of code create the Table resources.</span></span> 

## <a name="create-the-table-client"></a><span data-ttu-id="47ae0-225">Vytvoření tabulky klienta</span><span class="sxs-lookup"><span data-stu-id="47ae0-225">Create the table client</span></span>
<span data-ttu-id="47ae0-226">Inicializaci `CloudTableClient` pro připojení k účtu tabulky.</span><span class="sxs-lookup"><span data-stu-id="47ae0-226">You initialize a `CloudTableClient` to connect to the table account.</span></span>

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
<span data-ttu-id="47ae0-227">Tento klient je inicializována pomocí `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, a `TablePreferredLocations` konfigurační hodnoty, pokud zadaný v nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="47ae0-227">This client is initialized using the `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, and `TablePreferredLocations` configuration values if specified in the app settings.</span></span>
    
## <a name="create-a-table"></a><span data-ttu-id="47ae0-228">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="47ae0-228">Create a table</span></span>
<span data-ttu-id="47ae0-229">Pak vytvořte tabulku pomocí `CloudTable`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-229">Then, you create a table using `CloudTable`.</span></span> <span data-ttu-id="47ae0-230">Tabulky v databázi Cosmos Azure můžete nezávisle škálovat z hlediska úložiště a propustnost a dělení se automaticky zpracovává službou.</span><span class="sxs-lookup"><span data-stu-id="47ae0-230">Tables in Azure Cosmos DB can scale independently in terms of storage and throughput, and partitioning is handled automatically by the service.</span></span> <span data-ttu-id="47ae0-231">Azure Cosmos DB podporuje pevné velikosti a neomezená tabulky.</span><span class="sxs-lookup"><span data-stu-id="47ae0-231">Azure Cosmos DB supports both fixed size and unlimited tables.</span></span> <span data-ttu-id="47ae0-232">V tématu [vytváření oddílů v Azure Cosmos DB](partition-data.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="47ae0-232">See [Partitioning in Azure Cosmos DB](partition-data.md) for details.</span></span> 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

<span data-ttu-id="47ae0-233">Je důležité rozdíly v vytváření tabulek.</span><span class="sxs-lookup"><span data-stu-id="47ae0-233">There is an important difference in how tables are created.</span></span> <span data-ttu-id="47ae0-234">Azure Cosmos DB si vyhrazuje propustnost, na rozdíl od modelu na základě spotřeby úložiště Azure pro transakce.</span><span class="sxs-lookup"><span data-stu-id="47ae0-234">Azure Cosmos DB reserves throughput, unlike Azure storage's consumption-based model for transactions.</span></span> <span data-ttu-id="47ae0-235">Rezervace model má dvě klíčové výhody:</span><span class="sxs-lookup"><span data-stu-id="47ae0-235">The reservation model has two key benefits:</span></span>

* <span data-ttu-id="47ae0-236">Vaše propustnost je vyhrazené nebo vyhrazené, tak můžete nikdy získat omezeny Pokud je vaše četnost požadavků nebo zřízené propustnosti pod ní.</span><span class="sxs-lookup"><span data-stu-id="47ae0-236">Your throughput is dedicated/reserved, so you never get throttled if your request rate is at or below your provisioned throughput</span></span>
* <span data-ttu-id="47ae0-237">Model rezervace je další [nákladově efektivní pro úlohy náročné propustnost](key-value-store-cost.md)</span><span class="sxs-lookup"><span data-stu-id="47ae0-237">The reservation model is more [cost effective for throughput-heavy workloads](key-value-store-cost.md)</span></span>

<span data-ttu-id="47ae0-238">Můžete nakonfigurovat výchozí propustnosti tím, že nakonfigurujete nastavení `TableThroughput` z hlediska RU (jednotek žádosti) za sekundu.</span><span class="sxs-lookup"><span data-stu-id="47ae0-238">You can configure the default throughput by configuring the setting for `TableThroughput` in terms of RU (request units) per second.</span></span> 

<span data-ttu-id="47ae0-239">Čtení entity 1 KB normalizována jako 1 RU a další operace jsou normalizovány na pevnou hodnotu RU na základě jejich spotřeby procesoru, paměti a procesorů.</span><span class="sxs-lookup"><span data-stu-id="47ae0-239">A read of a 1-KB entity is normalized as 1 RU, and other operations are normalized to a fixed RU value based on their CPU, memory, and IOPS consumption.</span></span> <span data-ttu-id="47ae0-240">Další informace o [požadované jednotky v Azure Cosmos DB](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="47ae0-240">Learn more about [Request units in Azure Cosmos DB](request-units.md).</span></span>

> [!NOTE]
> <span data-ttu-id="47ae0-241">Zatímco úložiště Table SDK aktuálně nepodporuje změny propustnosti, můžete změnit propustnost okamžitě později pomocí portálu Azure nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="47ae0-241">While Table storage SDK does not currently support modifying throughput, you can change the throughput instantaneously at any time using the Azure portal or Azure CLI.</span></span>

<span data-ttu-id="47ae0-242">Potom provede jednoduché pro čtení a zápisu operace pomocí Azure Table storage SDK.</span><span class="sxs-lookup"><span data-stu-id="47ae0-242">Next, we walk through the simple read and write (CRUD) operations using the Azure Table storage SDK.</span></span> <span data-ttu-id="47ae0-243">Tento kurz představuje předvídatelný nízkou milisekundu jednociferné latenci a rychlé dotazy, které poskytuje Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="47ae0-243">This tutorial demonstrates predictable low single-digit millisecond latencies and fast queries provided by Azure Cosmos DB.</span></span>

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="47ae0-244">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="47ae0-244">Add an entity to a table</span></span>
<span data-ttu-id="47ae0-245">Entity v Azure Table storage vycházet z `TableEntity` třídy a musí mít `PartitionKey` a `RowKey` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="47ae0-245">Entities in Azure Table storage extend from the `TableEntity` class and must have `PartitionKey` and `RowKey` properties.</span></span> <span data-ttu-id="47ae0-246">Zde je ukázka definice pro entitu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="47ae0-246">Here's a sample definition for a customer entity.</span></span>

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

<span data-ttu-id="47ae0-247">Následující fragment kódu ukazuje, jak vloží entitu s Azure storage SDK.</span><span class="sxs-lookup"><span data-stu-id="47ae0-247">The following snippet shows how to insert an entity with the Azure storage SDK.</span></span> <span data-ttu-id="47ae0-248">Azure Cosmos DB je určená pro zaručit s nízkou latencí v jakémkoli měřítku po celém světě.</span><span class="sxs-lookup"><span data-stu-id="47ae0-248">Azure Cosmos DB is designed for guaranteed low latency at any scale, across the world.</span></span>

<span data-ttu-id="47ae0-249">Dokončení zápisu < 15 ms v p99 a ms ~ 6 v p50 pro aplikace spuštěné ve stejné oblasti jako účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="47ae0-249">Writes complete <15 ms at p99 and ~6 ms at p50 for applications running in the same region as the Azure Cosmos DB account.</span></span> <span data-ttu-id="47ae0-250">A tato doba trvání účty pro fakt, že zápisy se až po jejich synchronně replikovány, spolehlivě potvrzené, a veškerý obsah je indexovaný potvrzené zpět do klienta.</span><span class="sxs-lookup"><span data-stu-id="47ae0-250">And this duration accounts for the fact that writes are acknowledged back to the client only after they are synchronously replicated, durably committed, and all content is indexed.</span></span>

<span data-ttu-id="47ae0-251">Rozhraní API tabulky pro Azure Cosmos DB je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="47ae0-251">The Table API for Azure Cosmos DB is in preview.</span></span> <span data-ttu-id="47ae0-252">Při obecné dostupnosti záruky latence p99 jsou zajišťované SLA jako jiná rozhraní API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="47ae0-252">At general availability, the p99 latency guarantees are backed by SLAs like other Azure Cosmos DB APIs.</span></span> 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="47ae0-253">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="47ae0-253">Insert a batch of entities</span></span>
<span data-ttu-id="47ae0-254">Azure Table storage podporuje dávkové operace rozhraní API, která umožňuje zkombinovat aktualizace, odstranění a vložení v jedné dávkové operace.</span><span class="sxs-lookup"><span data-stu-id="47ae0-254">Azure Table storage supports a batch operation API, that lets you combine updates, deletes, and inserts in the same single batch operation.</span></span> <span data-ttu-id="47ae0-255">Azure Cosmos DB nemá některá omezení na rozhraní API pro dávkové jako Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="47ae0-255">Azure Cosmos DB does not have some of the limitations on the batch API as Azure Table storage.</span></span> <span data-ttu-id="47ae0-256">Například můžete provádět vícenásobné čtení v dávce, můžete provést několik zápisy na stejnou entitu v dávce a neexistuje žádné omezení na 100 operace na jednu dávku.</span><span class="sxs-lookup"><span data-stu-id="47ae0-256">For example, you can perform multiple reads within a batch, you can perform multiple writes to the same entity within a batch, and there is no limit on 100 operations per batch.</span></span> 

```csharp
// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a><span data-ttu-id="47ae0-257">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="47ae0-257">Retrieve a single entity</span></span>
<span data-ttu-id="47ae0-258">Načte (získá) v Azure DB Cosmos dokončení < 10 ms v p99 a ~ 1 ms v p50 ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="47ae0-258">Retrieves (GETs) in Azure Cosmos DB complete <10 ms at p99 and ~1 ms at p50 in the same Azure region.</span></span> <span data-ttu-id="47ae0-259">Můžete přidat jako v mnoha oblastech ke svému účtu pro čtení s nízkou latencí a nasadit aplikace do čtení ze své místní oblast ("s více adresami") nastavením `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="47ae0-259">You can add as many regions to your account for low latency reads, and deploy applications to read from their local region ("multi-homed") by setting `TablePreferredLocations`.</span></span> 

<span data-ttu-id="47ae0-260">Můžete načíst jednu entitu pomocí následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="47ae0-260">You can retrieve a single entity using the following snippet:</span></span>

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> <span data-ttu-id="47ae0-261">Další informace o více funkci rozhraní API v [vývoj s několika oblastí](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="47ae0-261">Learn about multi-homing APIs at [Developing with multiple regions](tutorial-global-distribution-table.md)</span></span>
>

## <a name="query-entities-using-automatic-secondary-indexes"></a><span data-ttu-id="47ae0-262">Entity dotazu pomocí automatické sekundární indexy</span><span class="sxs-lookup"><span data-stu-id="47ae0-262">Query entities using automatic secondary indexes</span></span>
<span data-ttu-id="47ae0-263">Tabulky lze dotazovat pomocí `TableQuery` třídy.</span><span class="sxs-lookup"><span data-stu-id="47ae0-263">Tables can be queried using the `TableQuery` class.</span></span> <span data-ttu-id="47ae0-264">Azure Cosmos DB má optimalizované zápisu databázový stroj, který automaticky indexuje všechny sloupce v tabulce.</span><span class="sxs-lookup"><span data-stu-id="47ae0-264">Azure Cosmos DB has a write-optimized database engine that automatically indexes all columns within your table.</span></span> <span data-ttu-id="47ae0-265">Indexování v Azure Cosmos DB nerozlišuje schématu.</span><span class="sxs-lookup"><span data-stu-id="47ae0-265">Indexing in Azure Cosmos DB is agnostic to schema.</span></span> <span data-ttu-id="47ae0-266">Proto i v případě, že vaše schéma je odlišné mezi řádky nebo pokud schéma vyvíjí v průběhu času, je automaticky indexován.</span><span class="sxs-lookup"><span data-stu-id="47ae0-266">Therefore, even if your schema is different between rows, or if the schema evolves over time, it is automatically indexed.</span></span> <span data-ttu-id="47ae0-267">Vzhledem k tomu, že Azure Cosmos DB podporuje automatické sekundární indexy, dotazy pro vlastnost, můžete použít index a efektivně obsluhovat.</span><span class="sxs-lookup"><span data-stu-id="47ae0-267">Since Azure Cosmos DB supports automatic secondary indexes, queries against any property can use the index and be served efficiently.</span></span>

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

<span data-ttu-id="47ae0-268">Ve verzi preview Azure Cosmos DB podporuje stejné funkce jako Azure Table storage dotazu pro rozhraní API tabulky.</span><span class="sxs-lookup"><span data-stu-id="47ae0-268">In preview, Azure Cosmos DB supports the same query functionality as Azure Table storage for the Table API.</span></span> <span data-ttu-id="47ae0-269">Azure Cosmos DB také podporuje řazení, agregace, geoprostorové dotazu, hierarchie a širokou škálu integrované funkce.</span><span class="sxs-lookup"><span data-stu-id="47ae0-269">Azure Cosmos DB also supports sorting, aggregates, geospatial query, hierarchy, and a wide range of built-in functions.</span></span> <span data-ttu-id="47ae0-270">Další funkce bude k dispozici v rozhraní API tabulky v aktualizaci budoucí služby.</span><span class="sxs-lookup"><span data-stu-id="47ae0-270">The additional functionality will be provided in the Table API in a future service update.</span></span> <span data-ttu-id="47ae0-271">V tématu [Azure Cosmos DB dotazu](documentdb-sql-query.md) přehled těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="47ae0-271">See [Azure Cosmos DB query](documentdb-sql-query.md) for an overview of these capabilities.</span></span> 

## <a name="replace-an-entity"></a><span data-ttu-id="47ae0-272">Nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="47ae0-272">Replace an entity</span></span>
<span data-ttu-id="47ae0-273">Pokud chcete entitu aktualizovat, načtěte ji ze služby Table, upravte objekt entity a potom uložte změny zpět do služby Table.</span><span class="sxs-lookup"><span data-stu-id="47ae0-273">To update an entity, retrieve it from the Table service, modify the entity object, and then save the changes back to the Table service.</span></span> <span data-ttu-id="47ae0-274">Následující kód změní telefonní číslo stávajícího zákazníka.</span><span class="sxs-lookup"><span data-stu-id="47ae0-274">The following code changes an existing customer's phone number.</span></span> 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
<span data-ttu-id="47ae0-275">Podobně můžete provádět `InsertOrMerge` nebo `Merge` operace.</span><span class="sxs-lookup"><span data-stu-id="47ae0-275">Similarly, you can perform `InsertOrMerge` or `Merge` operations.</span></span>  

## <a name="delete-an-entity"></a><span data-ttu-id="47ae0-276">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="47ae0-276">Delete an entity</span></span>
<span data-ttu-id="47ae0-277">Entitu můžete po jejím načtení snadno odstranit, a to pomocí stejného vzoru zobrazovaného pro aktualizaci entity.</span><span class="sxs-lookup"><span data-stu-id="47ae0-277">You can easily delete an entity after you have retrieved it by using the same pattern shown for updating an entity.</span></span> <span data-ttu-id="47ae0-278">Následující kód načte a odstraní entitu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="47ae0-278">The following code retrieves and deletes a customer entity.</span></span>

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a><span data-ttu-id="47ae0-279">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="47ae0-279">Delete a table</span></span>
<span data-ttu-id="47ae0-280">Následující příklad kódu nakonec odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="47ae0-280">Finally, the following code example deletes a table from a storage account.</span></span> <span data-ttu-id="47ae0-281">Můžete odstranit a znovu vytvořte tabulku okamžitě s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="47ae0-281">You can delete and recreate a table immediately with Azure Cosmos DB.</span></span>

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a><span data-ttu-id="47ae0-282">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="47ae0-282">Clean up resources</span></span> 

<span data-ttu-id="47ae0-283">Pokud nebudete tuto aplikaci nadále používat, pomocí následujícího postupu odstraňte všechny prostředky vytvořené tímto rychlým startem na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="47ae0-283">If you're not going to continue to use this app, use the following steps to delete all resources created by this tutorial in the Azure portal.</span></span>   

1. <span data-ttu-id="47ae0-284">V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="47ae0-284">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span>  
2. <span data-ttu-id="47ae0-285">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="47ae0-285">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="47ae0-286">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47ae0-286">Next steps</span></span>

<span data-ttu-id="47ae0-287">V tomto kurzu jsme popsané postupy a začněte využívat Azure Cosmos DB s rozhraním API pro tabulky a jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="47ae0-287">In this tutorial, we covered how to get started using Azure Cosmos DB with the Table API, and you've done the following:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="47ae0-288">Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="47ae0-288">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="47ae0-289">Povolená funkce v souboru app.config</span><span class="sxs-lookup"><span data-stu-id="47ae0-289">Enabled functionality in the app.config file</span></span> 
> * <span data-ttu-id="47ae0-290">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="47ae0-290">Created a table</span></span> 
> * <span data-ttu-id="47ae0-291">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="47ae0-291">Added an entity to a table</span></span> 
> * <span data-ttu-id="47ae0-292">Vložit dávku entit</span><span class="sxs-lookup"><span data-stu-id="47ae0-292">Inserted a batch of entities</span></span> 
> * <span data-ttu-id="47ae0-293">Načíst jednu entitu</span><span class="sxs-lookup"><span data-stu-id="47ae0-293">Retrieved a single entity</span></span> 
> * <span data-ttu-id="47ae0-294">Dotazovaný entity pomocí automatické sekundární indexy</span><span class="sxs-lookup"><span data-stu-id="47ae0-294">Queried entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="47ae0-295">Nahradí entitu</span><span class="sxs-lookup"><span data-stu-id="47ae0-295">Replaced an entity</span></span> 
> * <span data-ttu-id="47ae0-296">Odstranit entity</span><span class="sxs-lookup"><span data-stu-id="47ae0-296">Deleted an entity</span></span> 
> * <span data-ttu-id="47ae0-297">Odstranit tabulku</span><span class="sxs-lookup"><span data-stu-id="47ae0-297">Deleted a table</span></span>  

<span data-ttu-id="47ae0-298">Teď můžete pokračovat v dalším kurzu a další informace o dotazování dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="47ae0-298">You can now proceed to the next tutorial and learn more about querying table data.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="47ae0-299">Dotaz s tabulkou rozhraní API</span><span class="sxs-lookup"><span data-stu-id="47ae0-299">Query with the Table API</span></span>](tutorial-query-table.md)
