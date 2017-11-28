---
title: "Azure Cosmos DB: Vývoj pomocí hello rozhraní API pro tabulky v rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak toodevelop s rozhraním API Azure Cosmos DB Table pomocí rozhraní .NET"
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
ms.openlocfilehash: 70c6985a1dffdbcdb07e377f8ad10355bb97712a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-table-api-in-net"></a><span data-ttu-id="a529c-103">Azure Cosmos DB: Vývoj pomocí hello rozhraní API pro tabulky v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="a529c-103">Azure Cosmos DB: Develop with hello Table API in .NET</span></span>

<span data-ttu-id="a529c-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="a529c-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="a529c-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a529c-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span>

<span data-ttu-id="a529c-106">Tento kurz se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="a529c-106">This tutorial covers hello following tasks:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="a529c-107">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a529c-107">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="a529c-108">Povolit funkci v souboru app.config hello</span><span class="sxs-lookup"><span data-stu-id="a529c-108">Enable functionality in hello app.config file</span></span> 
> * <span data-ttu-id="a529c-109">Vytvoření tabulky s použitím hello [tabulky API](table-introduction.md) (preview)</span><span class="sxs-lookup"><span data-stu-id="a529c-109">Create a table using hello [Table API](table-introduction.md) (preview)</span></span>
> * <span data-ttu-id="a529c-110">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="a529c-110">Add an entity tooa table</span></span> 
> * <span data-ttu-id="a529c-111">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="a529c-111">Insert a batch of entities</span></span> 
> * <span data-ttu-id="a529c-112">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="a529c-112">Retrieve a single entity</span></span> 
> * <span data-ttu-id="a529c-113">Entity dotazu pomocí automatické sekundární indexy</span><span class="sxs-lookup"><span data-stu-id="a529c-113">Query entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="a529c-114">Nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="a529c-114">Replace an entity</span></span> 
> * <span data-ttu-id="a529c-115">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="a529c-115">Delete an entity</span></span> 
> * <span data-ttu-id="a529c-116">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="a529c-116">Delete a table</span></span>
 
## <a name="tables-in-azure-cosmos-db"></a><span data-ttu-id="a529c-117">Tabulky v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a529c-117">Tables in Azure Cosmos DB</span></span> 

<span data-ttu-id="a529c-118">Azure Cosmos DB poskytuje hello [tabulky API](table-introduction.md) (Náhled) pro aplikace, které potřebují úložiště dvojic klíč hodnota s návrhem bez schématu.</span><span class="sxs-lookup"><span data-stu-id="a529c-118">Azure Cosmos DB provides hello [Table API](table-introduction.md) (preview) for applications that need a key-value store with a schema-less design.</span></span> <span data-ttu-id="a529c-119">[Azure Table storage](../storage/common/storage-introduction.md) sady SDK a rozhraní REST API lze použít toowork s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a529c-119">[Azure Table storage](../storage/common/storage-introduction.md) SDKs and REST APIs can be used toowork with Azure Cosmos DB.</span></span> <span data-ttu-id="a529c-120">Můžete použít Azure Cosmos DB toocreate tabulky s požadavky na vysoké propustnosti.</span><span class="sxs-lookup"><span data-stu-id="a529c-120">You can use Azure Cosmos DB toocreate tables with high throughput requirements.</span></span> <span data-ttu-id="a529c-121">Azure Cosmos DB podporuje tabulky s optimalizovanou propustností (neformálně označované jako „tabulky Premium“), aktuálně ve verzi Public Preview.</span><span class="sxs-lookup"><span data-stu-id="a529c-121">Azure Cosmos DB supports throughput-optimized tables (informally called "premium tables"), currently in public preview.</span></span> 

<span data-ttu-id="a529c-122">Můžete pokračovat v toouse Azure Table storage pro tabulky s vysokou úložiště a nižší požadavky na propustnost.</span><span class="sxs-lookup"><span data-stu-id="a529c-122">You can continue toouse Azure Table storage for tables with high storage and lower throughput requirements.</span></span> <span data-ttu-id="a529c-123">Azure Cosmos DB zavádí podporu pro úložiště optimalizované tabulky v budoucí aktualizaci a Azure Table stávající a nové účty úložiště bude plynule upgradovat tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a529c-123">Azure Cosmos DB will introduce support for storage-optimized tables in a future update, and existing and new Azure Table storage accounts will be seamlessly upgraded tooAzure Cosmos DB.</span></span>

<span data-ttu-id="a529c-124">Pokud aktuálně používáte Azure Table storage, získáte následující výhody s preview "premium tabulka" hello hello:</span><span class="sxs-lookup"><span data-stu-id="a529c-124">If you currently use Azure Table storage, you gain hello following benefits with hello "premium table" preview:</span></span>

- <span data-ttu-id="a529c-125">Klíč [globální distribuční](distribute-data-globally.md) s více domovských stránek a [automatickou a ruční převzetí služeb při selhání](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="a529c-125">Turn-key [global distribution](distribute-data-globally.md) with multi-homing and [automatic and manual failovers](regional-failover.md)</span></span>
- <span data-ttu-id="a529c-126">Podpora pro automatické indexování pro všechny vlastnosti (dále jen "sekundární indexy") a rychlé dotazy schématu vznikl</span><span class="sxs-lookup"><span data-stu-id="a529c-126">Support for automatic schema-agnostic indexing against all properties ("secondary indexes"), and fast queries</span></span> 
- <span data-ttu-id="a529c-127">Podpora pro [nezávislé škálování úložiště a propustnost](partition-data.md), napříč libovolný počet oblastí</span><span class="sxs-lookup"><span data-stu-id="a529c-127">Support for [independent scaling of storage and throughput](partition-data.md), across any number of regions</span></span>
- <span data-ttu-id="a529c-128">Podpora pro [vyhrazené propustnost za tabulky](request-units.md) , je možné rozšířit stovek toomillions požadavků za sekundu</span><span class="sxs-lookup"><span data-stu-id="a529c-128">Support for [dedicated throughput per table](request-units.md) that can be scaled from hundreds toomillions of requests per second</span></span>
- <span data-ttu-id="a529c-129">Podpora pro [pět přizpůsobitelné úrovně konzistence](consistency-levels.md) tootrade vypnout dostupnost, latence a konzistence podle potřebám vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="a529c-129">Support for [five tunable consistency levels](consistency-levels.md) tootrade off availability, latency, and consistency based on your application needs</span></span>
- <span data-ttu-id="a529c-130">99,99 % dostupnost v rámci jedné oblasti a možnost tooadd další oblasti pro vyšší dostupnosti, a [komplexní SLA špičkový](https://azure.microsoft.com/support/legal/sla/cosmos-db/) na obecné dostupnosti</span><span class="sxs-lookup"><span data-stu-id="a529c-130">99.99% availability within a single region, and ability tooadd more regions for higher availability, and [industry-leading comprehensive SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) on general availability</span></span>
- <span data-ttu-id="a529c-131">Práce s hello existující úložiště Azure .NET SDK a žádná aplikace tooyour změny kódu</span><span class="sxs-lookup"><span data-stu-id="a529c-131">Work with hello existing Azure storage .NET SDK, and no code changes tooyour application</span></span>

<span data-ttu-id="a529c-132">Během hello preview podporuje Azure Cosmos DB hello API tabulky pomocí hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="a529c-132">During hello preview, Azure Cosmos DB supports hello Table API using hello .NET SDK.</span></span> <span data-ttu-id="a529c-133">Si můžete stáhnout hello [SDK Preview úložiště Azure](https://aka.ms/premiumtablenuget) z NuGet, který má hello stejné třídy a metody podpisy jako hello [sada SDK úložiště Azure](https://www.nuget.org/packages/WindowsAzure.Storage), ale také můžete připojit tooAzure Cosmos DB účty pomocí hello Tabulka rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a529c-133">You can download hello [Azure Storage Preview SDK](https://aka.ms/premiumtablenuget) from NuGet, that has hello same classes and method signatures as hello [Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), but also can connect tooAzure Cosmos DB accounts using hello Table API.</span></span>

<span data-ttu-id="a529c-134">toolearn Další informace o složité úlohy Azure Table storage, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="a529c-134">toolearn more about complex Azure Table storage tasks, see:</span></span>

* [<span data-ttu-id="a529c-135">Úvod tooAzure Cosmos DB: tabulka rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a529c-135">Introduction tooAzure Cosmos DB: Table API</span></span>](table-introduction.md)
* <span data-ttu-id="a529c-136">Hello tabulky referenční dokumentaci ke službě kompletní informace o dostupných rozhraních API [Klientská knihovna pro úložiště pro .NET – referenční informace](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="a529c-136">hello Table service reference documentation for complete details about available APIs [Storage Client Library for .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="a529c-137">O tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="a529c-137">About this tutorial</span></span>
<span data-ttu-id="a529c-138">V tomto kurzu je pro vývojáře, kteří znají hello úložiště tabulek Azure SDK a chcete toouse hello prémiové funkce dostupné pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a529c-138">This tutorial is for developers who are familiar with hello Azure Table storage SDK, and would like toouse hello premium features available using Azure Cosmos DB.</span></span> <span data-ttu-id="a529c-139">Je založena na [Začínáme s Azure Table storage pomocí rozhraní .NET](table-storage-how-to-use-dotnet.md) a ukazuje, jak tootake využívat další funkce jako sekundární indexy, zřízená propustnost a více domovských stránek.</span><span class="sxs-lookup"><span data-stu-id="a529c-139">It is based on [Get Started with Azure Table storage using .NET](table-storage-how-to-use-dotnet.md) and shows how tootake advantage of additional capabilities like secondary indexes, provisioned throughput, and multi-homing.</span></span> <span data-ttu-id="a529c-140">Nabídneme jak toouse hello Azure portálu toocreate účet Azure Cosmos DB sestavení a nasazení aplikace tabulky.</span><span class="sxs-lookup"><span data-stu-id="a529c-140">We cover how toouse hello Azure portal toocreate an Azure Cosmos DB account, and then build and deploy a Table application.</span></span> <span data-ttu-id="a529c-141">Můžeme také provede příklady .NET pro vytváření a odstraňování tabulek a vkládání, aktualizaci, odstranění a dotazování dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="a529c-141">We also walk through .NET examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span> 

<span data-ttu-id="a529c-142">Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="a529c-142">If you don't already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="a529c-143">Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="a529c-143">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="a529c-144">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="a529c-144">Create a database account</span></span>

<span data-ttu-id="a529c-145">Začněme vytvořením účtu Azure Cosmos DB v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a529c-145">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="a529c-146">Již máte účet Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="a529c-146">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="a529c-147">Pokud ano, přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="a529c-147">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>
> * <span data-ttu-id="a529c-148">Měli jste účet Azure DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="a529c-148">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="a529c-149">Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="a529c-149">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="a529c-150">Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="a529c-150">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span>
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting tooany location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-hello-sample-application"></a><span data-ttu-id="a529c-151">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="a529c-151">Clone hello sample application</span></span>

<span data-ttu-id="a529c-152">Nyní Pojďme klonovat aplikace tabulky z githubu, nastavte hello připojovací řetězec a spusťte ho.</span><span class="sxs-lookup"><span data-stu-id="a529c-152">Now let's clone a Table app from github, set hello connection string, and run it.</span></span>

1. <span data-ttu-id="a529c-153">Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="a529c-153">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="a529c-154">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="a529c-154">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. <span data-ttu-id="a529c-155">Poté otevřete soubor řešení hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a529c-155">Then open hello solution file in Visual Studio.</span></span>

## <a name="update-your-connection-string"></a><span data-ttu-id="a529c-156">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="a529c-156">Update your connection string</span></span>

<span data-ttu-id="a529c-157">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a529c-157">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="a529c-158">V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="a529c-158">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="a529c-159">Kopírování tlačítek hello použijete na pravé straně hello hello obrazovky toocopy hello připojovací řetězec do souboru app.config hello v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="a529c-159">You'll use hello copy buttons on hello right side of hello screen toocopy hello connection string into hello app.config file in hello next step.</span></span>

2. <span data-ttu-id="a529c-160">V sadě Visual Studio otevřete soubor app.config hello.</span><span class="sxs-lookup"><span data-stu-id="a529c-160">In Visual Studio, open hello app.config file.</span></span> 

3. <span data-ttu-id="a529c-161">Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu klíč účtu hello v souboru app.config. Použijte název účtu hello dříve vytvořili pro název účtu v souboru app.config.</span><span class="sxs-lookup"><span data-stu-id="a529c-161">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello account-key in app.config. Use hello account name created earlier for account-name in app.config.</span></span>
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> <span data-ttu-id="a529c-162">toouse této aplikaci pomocí standardní Azure Table Storage je nutné toochange hello připojovací řetězec v `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="a529c-162">toouse this app with standard Azure Table Storage, you need toochange hello connection string in `app.config file`.</span></span> <span data-ttu-id="a529c-163">Název účtu hello použijte jako název tabulky účtu a klíč jako Azure úložiště primární klíč.</span><span class="sxs-lookup"><span data-stu-id="a529c-163">Use hello account name as Table-account name and key as Azure Storage Primary key.</span></span> <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-hello-app"></a><span data-ttu-id="a529c-164">Sestavení a nasazení aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a529c-164">Build and deploy hello app</span></span>
1. <span data-ttu-id="a529c-165">V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a529c-165">In Visual Studio, right-click on hello project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="a529c-166">V hello NuGet **Procházet** zadejte ***WindowsAzure.Storage PremiumTable***.</span><span class="sxs-lookup"><span data-stu-id="a529c-166">In hello NuGet **Browse** box, type ***WindowsAzure.Storage-PremiumTable***.</span></span> <span data-ttu-id="a529c-167">Zkontrolujte **zahrnují předprodejní verze**.</span><span class="sxs-lookup"><span data-stu-id="a529c-167">Check **Include prerelease versions**.</span></span>

3. <span data-ttu-id="a529c-168">Z výsledků hello nainstalovat hello **WindowsAzure.Storage PremiumTable** a zvolte buildu preview hello `0.0.1-preview`.</span><span class="sxs-lookup"><span data-stu-id="a529c-168">From hello results, install hello **WindowsAzure.Storage-PremiumTable** and choose hello preview build `0.0.1-preview`.</span></span> <span data-ttu-id="a529c-169">Tato akce nainstaluje balíček úložiště Azure Table hello a všechny závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="a529c-169">This action installs hello Azure Table storage package and all dependencies.</span></span>

4. <span data-ttu-id="a529c-170">Klikněte na kombinaci kláves CTRL + F5 toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a529c-170">Click CTRL + F5 toorun hello application.</span></span> 

<span data-ttu-id="a529c-171">Teď můžete vrátit tooData Průzkumníka a zobrazit dotaz, upravit a práci s daty této tabulky.</span><span class="sxs-lookup"><span data-stu-id="a529c-171">You can now go back tooData Explorer and see query, modify, and work with this table data.</span></span> 

> [!NOTE]
> <span data-ttu-id="a529c-172">toouse této aplikace pomocí emulátoru DB Cosmos Azure, můžete jednoduše potřebovat toochange hello připojovací řetězec v `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="a529c-172">toouse this app with an Azure Cosmos DB Emulator, you just need toochange hello connection string in `app.config file`.</span></span> <span data-ttu-id="a529c-173">Použijte hello menší než hodnota pro emulátor.</span><span class="sxs-lookup"><span data-stu-id="a529c-173">Use hello below value for emulator.</span></span> <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a><span data-ttu-id="a529c-174">Možnosti Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a529c-174">Azure Cosmos DB capabilities</span></span>
<span data-ttu-id="a529c-175">Azure Cosmos DB podporuje několik možností, které nejsou k dispozici v hello Azure Table storage rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a529c-175">Azure Cosmos DB supports a number of capabilities that are not available in hello Azure Table storage API.</span></span> <span data-ttu-id="a529c-176">Hello nové funkce se dá povolit buď hello následující `appSettings` hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a529c-176">hello new functionality can be enabled via hello following `appSettings` configuration values.</span></span> <span data-ttu-id="a529c-177">Zavedeme nejsou žádné nové podpisy nebo přetížení toohello preview SDK úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a529c-177">We did not introduce any new signatures or overloads toohello preview Azure Storage SDK.</span></span> <span data-ttu-id="a529c-178">To vám umožní tooconnect tooboth standard a premium tabulky a práci s jinými službami Azure Storage jako objekty BLOB a fronty.</span><span class="sxs-lookup"><span data-stu-id="a529c-178">This allows you tooconnect tooboth standard and premium tables, and work with other Azure Storage services like Blobs and Queues.</span></span> 


| <span data-ttu-id="a529c-179">Klíč</span><span class="sxs-lookup"><span data-stu-id="a529c-179">Key</span></span> | <span data-ttu-id="a529c-180">Popis</span><span class="sxs-lookup"><span data-stu-id="a529c-180">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a529c-181">TableConnectionMode</span><span class="sxs-lookup"><span data-stu-id="a529c-181">TableConnectionMode</span></span>  | <span data-ttu-id="a529c-182">Azure Cosmos DB podporuje dva režimy připojení.</span><span class="sxs-lookup"><span data-stu-id="a529c-182">Azure Cosmos DB supports two connectivity modes.</span></span> <span data-ttu-id="a529c-183">V `Gateway` režimu, požadavky se budou vždy provádět toohello Azure Cosmos DB brána, která předá ho toohello odpovídající data oddíly.</span><span class="sxs-lookup"><span data-stu-id="a529c-183">In `Gateway` mode, requests are always made toohello Azure Cosmos DB gateway, which forwards it toohello corresponding data partitions.</span></span> <span data-ttu-id="a529c-184">V `Direct` režim připojení klienta hello načte hello mapování tabulek toopartitions a přímo na data oddíly jsou vytvářeny požadavky.</span><span class="sxs-lookup"><span data-stu-id="a529c-184">In `Direct` connectivity mode, hello client fetches hello mapping of tables toopartitions, and requests are made directly against data partitions.</span></span> <span data-ttu-id="a529c-185">Doporučujeme, abyste `Direct`, hello výchozí.</span><span class="sxs-lookup"><span data-stu-id="a529c-185">We recommend `Direct`, hello default.</span></span>  |
| <span data-ttu-id="a529c-186">TableConnectionProtocol</span><span class="sxs-lookup"><span data-stu-id="a529c-186">TableConnectionProtocol</span></span> | <span data-ttu-id="a529c-187">Azure Cosmos DB podporuje dva protokoly připojení - `Https` a `Tcp`.</span><span class="sxs-lookup"><span data-stu-id="a529c-187">Azure Cosmos DB supports two connection protocols - `Https` and `Tcp`.</span></span> <span data-ttu-id="a529c-188">`Tcp`hello výchozí a doporučuje, protože je jednodušší.</span><span class="sxs-lookup"><span data-stu-id="a529c-188">`Tcp` is hello default, and recommended because it is more lightweight.</span></span> |
| <span data-ttu-id="a529c-189">TablePreferredLocations</span><span class="sxs-lookup"><span data-stu-id="a529c-189">TablePreferredLocations</span></span> | <span data-ttu-id="a529c-190">Čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení.</span><span class="sxs-lookup"><span data-stu-id="a529c-190">Comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="a529c-191">Každý účet Azure Cosmos DB lze přidružit 1 – 30 + oblasti.</span><span class="sxs-lookup"><span data-stu-id="a529c-191">Each Azure Cosmos DB account can be associated with 1-30+ regions.</span></span> <span data-ttu-id="a529c-192">Každá instance klienta můžete určit podmnožinu těchto oblastí v hello upřednostňované pořadí pro čtení s nízkou latencí.</span><span class="sxs-lookup"><span data-stu-id="a529c-192">Each client instance can specify a subset of these regions in hello preferred order for low latency reads.</span></span> <span data-ttu-id="a529c-193">Hello oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`.</span><span class="sxs-lookup"><span data-stu-id="a529c-193">hello regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span> <span data-ttu-id="a529c-194">Viz také [více domovských stránek rozhraní API](tutorial-global-distribution-table.md).</span><span class="sxs-lookup"><span data-stu-id="a529c-194">Also see [Multi-homing APIs](tutorial-global-distribution-table.md).</span></span>
| <span data-ttu-id="a529c-195">TableConsistencyLevel</span><span class="sxs-lookup"><span data-stu-id="a529c-195">TableConsistencyLevel</span></span> | <span data-ttu-id="a529c-196">Můžete se kompromisy mezi latence, konzistence a dostupnosti výběrem mezi pěti dobře definované úrovně konzistence: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, a `Eventual`.</span><span class="sxs-lookup"><span data-stu-id="a529c-196">You can trade off between latency, consistency, and availability by choosing between five well-defined consistency levels: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, and `Eventual`.</span></span> <span data-ttu-id="a529c-197">Výchozí hodnota je `Session`.</span><span class="sxs-lookup"><span data-stu-id="a529c-197">Default is `Session`.</span></span> <span data-ttu-id="a529c-198">Hello Volba úrovně konzistence díky významně zvýšit výkon rozdíl ve více oblastech nastavení.</span><span class="sxs-lookup"><span data-stu-id="a529c-198">hello choice of consistency level makes a significant performance difference in multi-region setups.</span></span> <span data-ttu-id="a529c-199">V tématu [úrovně konzistence](consistency-levels.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a529c-199">See [Consistency levels](consistency-levels.md) for details.</span></span> |
| <span data-ttu-id="a529c-200">TableThroughput</span><span class="sxs-lookup"><span data-stu-id="a529c-200">TableThroughput</span></span> | <span data-ttu-id="a529c-201">Vyhrazenou propustností pro tabulku hello vyjádřené v jednotek žádosti (RU) za sekundu.</span><span class="sxs-lookup"><span data-stu-id="a529c-201">Reserved throughput for hello table expressed in request units (RU) per second.</span></span> <span data-ttu-id="a529c-202">Jedné tabulky může podporovat 100s miliony RU/s.</span><span class="sxs-lookup"><span data-stu-id="a529c-202">Single tables can support 100s-millions of RU/s.</span></span> <span data-ttu-id="a529c-203">V tématu [požadované jednotky](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="a529c-203">See [Request units](request-units.md).</span></span> <span data-ttu-id="a529c-204">Výchozí hodnota je`400`</span><span class="sxs-lookup"><span data-stu-id="a529c-204">Default is `400`</span></span> |
| <span data-ttu-id="a529c-205">TableIndexingPolicy</span><span class="sxs-lookup"><span data-stu-id="a529c-205">TableIndexingPolicy</span></span> | <span data-ttu-id="a529c-206">Konzistentní a automatické sekundární indexování všech sloupců v rámci tabulky</span><span class="sxs-lookup"><span data-stu-id="a529c-206">Consistent and automatic secondary indexing of all columns within tables</span></span> | <span data-ttu-id="a529c-207">JSON řetězec vyhovující toohello indexování specifikace zásad.</span><span class="sxs-lookup"><span data-stu-id="a529c-207">JSON string conforming toohello indexing policy specification.</span></span> <span data-ttu-id="a529c-208">V tématu [zásady indexování](indexing-policies.md) toosee jak můžete změnit indexování zásad tooinclude/vyloučit konkrétní sloupce.</span><span class="sxs-lookup"><span data-stu-id="a529c-208">See [Indexing Policy](indexing-policies.md) toosee how you can change indexing policy tooinclude/exclude specific columns.</span></span> | <span data-ttu-id="a529c-209">Automatické indexování všech vlastností (hodnota hash pro řetězce) a rozsah čísel</span><span class="sxs-lookup"><span data-stu-id="a529c-209">Automatic indexing of all properties (hash for strings, and range for numbers)</span></span> |
| <span data-ttu-id="a529c-210">TableQueryMaxItemCount</span><span class="sxs-lookup"><span data-stu-id="a529c-210">TableQueryMaxItemCount</span></span> | <span data-ttu-id="a529c-211">Nakonfigurujte hello maximální počet položek vrátí na dotaz tabulky v jednom dobu odezvy.</span><span class="sxs-lookup"><span data-stu-id="a529c-211">Configure hello maximum number of items returned per table query in a single round trip.</span></span> <span data-ttu-id="a529c-212">Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu hello za běhu.</span><span class="sxs-lookup"><span data-stu-id="a529c-212">Default is `-1`, which lets Azure Cosmos DB dynamically determine hello value at runtime.</span></span> |
| <span data-ttu-id="a529c-213">TableQueryEnableScan</span><span class="sxs-lookup"><span data-stu-id="a529c-213">TableQueryEnableScan</span></span> | <span data-ttu-id="a529c-214">Pokud hello dotazu nelze použít pro libovolný filtr hello index, spusťte ji přesto prostřednictvím kontrolu.</span><span class="sxs-lookup"><span data-stu-id="a529c-214">If hello query cannot use hello index for any filter, then run it anyway via a scan.</span></span> <span data-ttu-id="a529c-215">Výchozí hodnota je `false`.</span><span class="sxs-lookup"><span data-stu-id="a529c-215">Default is `false`.</span></span>|
| <span data-ttu-id="a529c-216">TableQueryMaxDegreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="a529c-216">TableQueryMaxDegreeOfParallelism</span></span> | <span data-ttu-id="a529c-217">Hello stupně paralelního zpracování pro spuštění dotazu mezi oddílu.</span><span class="sxs-lookup"><span data-stu-id="a529c-217">hello degree of parallelism for execution of a cross-partition query.</span></span> <span data-ttu-id="a529c-218">`0`sériový s žádné předem načítání, `1` je sériové s předem načítání a vyšší hodnoty zvýšit rychlost hello paralelního zpracování.</span><span class="sxs-lookup"><span data-stu-id="a529c-218">`0` is serial with no pre-fetching, `1` is serial with pre-fetching, and higher values increase hello rate of parallelism.</span></span> <span data-ttu-id="a529c-219">Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu hello za běhu.</span><span class="sxs-lookup"><span data-stu-id="a529c-219">Default is `-1`, which lets Azure Cosmos DB dynamically determine hello value at runtime.</span></span> |

<span data-ttu-id="a529c-220">toochange hello výchozí hodnotu, otevřete hello `app.config` soubor v Průzkumníku řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a529c-220">toochange hello default value, open hello `app.config` file from Solution Explorer in Visual Studio.</span></span> <span data-ttu-id="a529c-221">Přidat obsah hello hello `<appSettings>` níže uvedeného prvku.</span><span class="sxs-lookup"><span data-stu-id="a529c-221">Add hello contents of hello `<appSettings>` element shown below.</span></span> <span data-ttu-id="a529c-222">Nahraďte `account-name` hello název účtu úložiště a `account-key` nahraďte svým klíčem účtu přístup.</span><span class="sxs-lookup"><span data-stu-id="a529c-222">Replace `account-name` with hello name of your storage account, and `account-key` with your account access key.</span></span> 

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

<span data-ttu-id="a529c-223">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="a529c-223">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="a529c-224">Otevřete hello `Program.cs` souboru a najít vytvořit tyto řádky kódu hello tabulky prostředky.</span><span class="sxs-lookup"><span data-stu-id="a529c-224">Open hello `Program.cs` file and you find that these lines of code create hello Table resources.</span></span> 

## <a name="create-hello-table-client"></a><span data-ttu-id="a529c-225">Vytvoření klienta tabulky hello</span><span class="sxs-lookup"><span data-stu-id="a529c-225">Create hello table client</span></span>
<span data-ttu-id="a529c-226">Inicializaci `CloudTableClient` tooconnect toohello tabulky účtu.</span><span class="sxs-lookup"><span data-stu-id="a529c-226">You initialize a `CloudTableClient` tooconnect toohello table account.</span></span>

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
<span data-ttu-id="a529c-227">Tento klient je inicializována pomocí hello `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, a `TablePreferredLocations` konfigurační hodnoty, pokud zadaný v nastavení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a529c-227">This client is initialized using hello `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, and `TablePreferredLocations` configuration values if specified in hello app settings.</span></span>
    
## <a name="create-a-table"></a><span data-ttu-id="a529c-228">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="a529c-228">Create a table</span></span>
<span data-ttu-id="a529c-229">Pak vytvořte tabulku pomocí `CloudTable`.</span><span class="sxs-lookup"><span data-stu-id="a529c-229">Then, you create a table using `CloudTable`.</span></span> <span data-ttu-id="a529c-230">Tabulky v databázi Cosmos Azure můžete nezávisle škálovat z hlediska úložiště a propustnost a dělení se automaticky zpracovává službou hello.</span><span class="sxs-lookup"><span data-stu-id="a529c-230">Tables in Azure Cosmos DB can scale independently in terms of storage and throughput, and partitioning is handled automatically by hello service.</span></span> <span data-ttu-id="a529c-231">Azure Cosmos DB podporuje pevné velikosti a neomezená tabulky.</span><span class="sxs-lookup"><span data-stu-id="a529c-231">Azure Cosmos DB supports both fixed size and unlimited tables.</span></span> <span data-ttu-id="a529c-232">V tématu [vytváření oddílů v Azure Cosmos DB](partition-data.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a529c-232">See [Partitioning in Azure Cosmos DB](partition-data.md) for details.</span></span> 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

<span data-ttu-id="a529c-233">Je důležité rozdíly v vytváření tabulek.</span><span class="sxs-lookup"><span data-stu-id="a529c-233">There is an important difference in how tables are created.</span></span> <span data-ttu-id="a529c-234">Azure Cosmos DB si vyhrazuje propustnost, na rozdíl od modelu na základě spotřeby úložiště Azure pro transakce.</span><span class="sxs-lookup"><span data-stu-id="a529c-234">Azure Cosmos DB reserves throughput, unlike Azure storage's consumption-based model for transactions.</span></span> <span data-ttu-id="a529c-235">model rezervace Hello přináší dvě klíčové výhody:</span><span class="sxs-lookup"><span data-stu-id="a529c-235">hello reservation model has two key benefits:</span></span>

* <span data-ttu-id="a529c-236">Vaše propustnost je vyhrazené nebo vyhrazené, tak můžete nikdy získat omezeny Pokud je vaše četnost požadavků nebo zřízené propustnosti pod ní.</span><span class="sxs-lookup"><span data-stu-id="a529c-236">Your throughput is dedicated/reserved, so you never get throttled if your request rate is at or below your provisioned throughput</span></span>
* <span data-ttu-id="a529c-237">model rezervace Hello je další [nákladově efektivní pro úlohy náročné propustnost](key-value-store-cost.md)</span><span class="sxs-lookup"><span data-stu-id="a529c-237">hello reservation model is more [cost effective for throughput-heavy workloads](key-value-store-cost.md)</span></span>

<span data-ttu-id="a529c-238">Propustnost výchozí hello můžete nakonfigurovat tak, že nakonfigurujete nastavení hello `TableThroughput` z hlediska RU (jednotek žádosti) za sekundu.</span><span class="sxs-lookup"><span data-stu-id="a529c-238">You can configure hello default throughput by configuring hello setting for `TableThroughput` in terms of RU (request units) per second.</span></span> 

<span data-ttu-id="a529c-239">Čtení entity 1 KB normalizována jako 1 RU a další operace jsou normalizovaný tooa pevná hodnota RU na základě jejich spotřeby procesoru, paměti a procesorů.</span><span class="sxs-lookup"><span data-stu-id="a529c-239">A read of a 1-KB entity is normalized as 1 RU, and other operations are normalized tooa fixed RU value based on their CPU, memory, and IOPS consumption.</span></span> <span data-ttu-id="a529c-240">Další informace o [požadované jednotky v Azure Cosmos DB](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="a529c-240">Learn more about [Request units in Azure Cosmos DB](request-units.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a529c-241">Zatímco úložiště Table SDK aktuálně nepodporuje změny propustnosti, můžete změnit hello propustnost okamžitě kdykoli použití hello portál Azure nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a529c-241">While Table storage SDK does not currently support modifying throughput, you can change hello throughput instantaneously at any time using hello Azure portal or Azure CLI.</span></span>

<span data-ttu-id="a529c-242">Potom provede hello jednoduché pro čtení a zápisu operace pomocí Azure Table storage hello SDK.</span><span class="sxs-lookup"><span data-stu-id="a529c-242">Next, we walk through hello simple read and write (CRUD) operations using hello Azure Table storage SDK.</span></span> <span data-ttu-id="a529c-243">Tento kurz představuje předvídatelný nízkou milisekundu jednociferné latenci a rychlé dotazy, které poskytuje Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a529c-243">This tutorial demonstrates predictable low single-digit millisecond latencies and fast queries provided by Azure Cosmos DB.</span></span>

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="a529c-244">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="a529c-244">Add an entity tooa table</span></span>
<span data-ttu-id="a529c-245">Entity v Azure Table storage vycházet z hello `TableEntity` třídy a musí mít `PartitionKey` a `RowKey` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a529c-245">Entities in Azure Table storage extend from hello `TableEntity` class and must have `PartitionKey` and `RowKey` properties.</span></span> <span data-ttu-id="a529c-246">Zde je ukázka definice pro entitu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="a529c-246">Here's a sample definition for a customer entity.</span></span>

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

<span data-ttu-id="a529c-247">Hello následující fragment kódu ukazuje, jak tooinsert entity s hello úložiště Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="a529c-247">hello following snippet shows how tooinsert an entity with hello Azure storage SDK.</span></span> <span data-ttu-id="a529c-248">Azure Cosmos DB je určená pro zaručit s nízkou latencí v jakémkoli měřítku napříč hello, world.</span><span class="sxs-lookup"><span data-stu-id="a529c-248">Azure Cosmos DB is designed for guaranteed low latency at any scale, across hello world.</span></span>

<span data-ttu-id="a529c-249">Dokončení zápisu < hello 15 ms v p99 a ms ~ 6 v p50 pro aplikace spuštěné stejné oblasti jako hello účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a529c-249">Writes complete <15 ms at p99 and ~6 ms at p50 for applications running in hello same region as hello Azure Cosmos DB account.</span></span> <span data-ttu-id="a529c-250">A tato doba trvání účty pro hello fakt, který zapíše jsou potvrzené back toohello klienta, až po jejich synchronně replikovány, spolehlivě potvrzené, a veškerý obsah je indexovaný.</span><span class="sxs-lookup"><span data-stu-id="a529c-250">And this duration accounts for hello fact that writes are acknowledged back toohello client only after they are synchronously replicated, durably committed, and all content is indexed.</span></span>

<span data-ttu-id="a529c-251">Hello tabulky rozhraní API pro Azure Cosmos DB je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="a529c-251">hello Table API for Azure Cosmos DB is in preview.</span></span> <span data-ttu-id="a529c-252">Při obecné dostupnosti hello p99 latence záruky jsou zajišťované SLA jako jiná rozhraní API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a529c-252">At general availability, hello p99 latency guarantees are backed by SLAs like other Azure Cosmos DB APIs.</span></span> 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute hello insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="a529c-253">Vložení dávky entit</span><span class="sxs-lookup"><span data-stu-id="a529c-253">Insert a batch of entities</span></span>
<span data-ttu-id="a529c-254">Azure Table storage podporuje rozhraní API dávkové operace, které vám umožní sloučit aktualizace, odstranění, a vloží v hello jedné dávkové operace.</span><span class="sxs-lookup"><span data-stu-id="a529c-254">Azure Table storage supports a batch operation API, that lets you combine updates, deletes, and inserts in hello same single batch operation.</span></span> <span data-ttu-id="a529c-255">Azure Cosmos DB nemá některé hello omezení na rozhraní API pro dávkové hello jako Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="a529c-255">Azure Cosmos DB does not have some of hello limitations on hello batch API as Azure Table storage.</span></span> <span data-ttu-id="a529c-256">Například můžete provádět vícenásobné čtení v dávce, můžete provést několik toohello zápisy stejné entity v dávce, a není nijak omezen na 100 operace na jednu dávku.</span><span class="sxs-lookup"><span data-stu-id="a529c-256">For example, you can perform multiple reads within a batch, you can perform multiple writes toohello same entity within a batch, and there is no limit on 100 operations per batch.</span></span> 

```csharp
// Create hello batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it toohello table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it toohello table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities toohello batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute hello batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a><span data-ttu-id="a529c-257">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="a529c-257">Retrieve a single entity</span></span>
<span data-ttu-id="a529c-258">Načte (získá) v Azure DB Cosmos dokončení < 10 ms v p99 a ~ 1 hello ms v p50 ve stejné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="a529c-258">Retrieves (GETs) in Azure Cosmos DB complete <10 ms at p99 and ~1 ms at p50 in hello same Azure region.</span></span> <span data-ttu-id="a529c-259">Můžete přidat jako v mnoha oblastech tooyour účet pro čtení s nízkou latencí a nasadit aplikace tooread ze své místní oblast ("s více adresami") nastavením `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="a529c-259">You can add as many regions tooyour account for low latency reads, and deploy applications tooread from their local region ("multi-homed") by setting `TablePreferredLocations`.</span></span> 

<span data-ttu-id="a529c-260">Můžete načíst jednu entitu pomocí hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="a529c-260">You can retrieve a single entity using hello following snippet:</span></span>

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> <span data-ttu-id="a529c-261">Další informace o více funkci rozhraní API v [vývoj s několika oblastí](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="a529c-261">Learn about multi-homing APIs at [Developing with multiple regions](tutorial-global-distribution-table.md)</span></span>
>

## <a name="query-entities-using-automatic-secondary-indexes"></a><span data-ttu-id="a529c-262">Entity dotazu pomocí automatické sekundární indexy</span><span class="sxs-lookup"><span data-stu-id="a529c-262">Query entities using automatic secondary indexes</span></span>
<span data-ttu-id="a529c-263">Tabulky lze dotazovat pomocí hello `TableQuery` třídy.</span><span class="sxs-lookup"><span data-stu-id="a529c-263">Tables can be queried using hello `TableQuery` class.</span></span> <span data-ttu-id="a529c-264">Azure Cosmos DB má optimalizované zápisu databázový stroj, který automaticky indexuje všechny sloupce v tabulce.</span><span class="sxs-lookup"><span data-stu-id="a529c-264">Azure Cosmos DB has a write-optimized database engine that automatically indexes all columns within your table.</span></span> <span data-ttu-id="a529c-265">Indexování v Azure Cosmos DB je lhostejné tooschema.</span><span class="sxs-lookup"><span data-stu-id="a529c-265">Indexing in Azure Cosmos DB is agnostic tooschema.</span></span> <span data-ttu-id="a529c-266">Proto i v případě, že vaše schéma je odlišné mezi řádky nebo pokud schéma hello vyvíjí v průběhu času, je automaticky indexován.</span><span class="sxs-lookup"><span data-stu-id="a529c-266">Therefore, even if your schema is different between rows, or if hello schema evolves over time, it is automatically indexed.</span></span> <span data-ttu-id="a529c-267">Vzhledem k tomu, že Azure Cosmos DB podporuje automatické sekundární indexy, dotazy pro vlastnost, můžete použít hello index a efektivně obsluhovat.</span><span class="sxs-lookup"><span data-stu-id="a529c-267">Since Azure Cosmos DB supports automatic secondary indexes, queries against any property can use hello index and be served efficiently.</span></span>

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

<span data-ttu-id="a529c-268">Ve verzi preview podporuje Azure Cosmos DB hello stejné funkce jako úložiště Azure Table pro hello API tabulka dotazu.</span><span class="sxs-lookup"><span data-stu-id="a529c-268">In preview, Azure Cosmos DB supports hello same query functionality as Azure Table storage for hello Table API.</span></span> <span data-ttu-id="a529c-269">Azure Cosmos DB také podporuje řazení, agregace, geoprostorové dotazu, hierarchie a širokou škálu integrované funkce.</span><span class="sxs-lookup"><span data-stu-id="a529c-269">Azure Cosmos DB also supports sorting, aggregates, geospatial query, hierarchy, and a wide range of built-in functions.</span></span> <span data-ttu-id="a529c-270">Hello další funkce bude k dispozici v hello API tabulky v aktualizaci budoucí služby.</span><span class="sxs-lookup"><span data-stu-id="a529c-270">hello additional functionality will be provided in hello Table API in a future service update.</span></span> <span data-ttu-id="a529c-271">V tématu [Azure Cosmos DB dotazu](documentdb-sql-query.md) přehled těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="a529c-271">See [Azure Cosmos DB query](documentdb-sql-query.md) for an overview of these capabilities.</span></span> 

## <a name="replace-an-entity"></a><span data-ttu-id="a529c-272">Nahrazení entity</span><span class="sxs-lookup"><span data-stu-id="a529c-272">Replace an entity</span></span>
<span data-ttu-id="a529c-273">tooupdate entity, načtěte ji ze služby Table hello, upravte objekt entity hello a potom uložte změny hello zpět toohello služby Table.</span><span class="sxs-lookup"><span data-stu-id="a529c-273">tooupdate an entity, retrieve it from hello Table service, modify hello entity object, and then save hello changes back toohello Table service.</span></span> <span data-ttu-id="a529c-274">Hello následující kód změní telefonní číslo stávajícího zákazníka.</span><span class="sxs-lookup"><span data-stu-id="a529c-274">hello following code changes an existing customer's phone number.</span></span> 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
<span data-ttu-id="a529c-275">Podobně můžete provádět `InsertOrMerge` nebo `Merge` operace.</span><span class="sxs-lookup"><span data-stu-id="a529c-275">Similarly, you can perform `InsertOrMerge` or `Merge` operations.</span></span>  

## <a name="delete-an-entity"></a><span data-ttu-id="a529c-276">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="a529c-276">Delete an entity</span></span>
<span data-ttu-id="a529c-277">Po jejím načtení pomocí hello můžete snadno odstranit entity stejného vzoru zobrazovaného pro aktualizaci entity.</span><span class="sxs-lookup"><span data-stu-id="a529c-277">You can easily delete an entity after you have retrieved it by using hello same pattern shown for updating an entity.</span></span> <span data-ttu-id="a529c-278">Hello následující kód načte a odstraní entitu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="a529c-278">hello following code retrieves and deletes a customer entity.</span></span>

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a><span data-ttu-id="a529c-279">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="a529c-279">Delete a table</span></span>
<span data-ttu-id="a529c-280">Hello následující příklad kódu nakonec odstraní tabulku z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a529c-280">Finally, hello following code example deletes a table from a storage account.</span></span> <span data-ttu-id="a529c-281">Můžete odstranit a znovu vytvořte tabulku okamžitě s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a529c-281">You can delete and recreate a table immediately with Azure Cosmos DB.</span></span>

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a><span data-ttu-id="a529c-282">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="a529c-282">Clean up resources</span></span> 

<span data-ttu-id="a529c-283">Pokud ale nebudete toocontinue toouse tuto aplikaci, použijte následující kroky toodelete všechny prostředky vytvořené v tomto kurzu v portálu Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="a529c-283">If you're not going toocontinue toouse this app, use hello following steps toodelete all resources created by this tutorial in hello Azure portal.</span></span>   

1. <span data-ttu-id="a529c-284">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a529c-284">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span>  
2. <span data-ttu-id="a529c-285">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="a529c-285">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a529c-286">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a529c-286">Next steps</span></span>

<span data-ttu-id="a529c-287">V tomto kurzu jsme popsaná způsob tooget spuštění pomocí Azure Cosmos DB hello tabulky rozhraní API a uděláte hello následující:</span><span class="sxs-lookup"><span data-stu-id="a529c-287">In this tutorial, we covered how tooget started using Azure Cosmos DB with hello Table API, and you've done hello following:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="a529c-288">Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a529c-288">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="a529c-289">Povolená funkce v souboru app.config hello</span><span class="sxs-lookup"><span data-stu-id="a529c-289">Enabled functionality in hello app.config file</span></span> 
> * <span data-ttu-id="a529c-290">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="a529c-290">Created a table</span></span> 
> * <span data-ttu-id="a529c-291">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="a529c-291">Added an entity tooa table</span></span> 
> * <span data-ttu-id="a529c-292">Vložit dávku entit</span><span class="sxs-lookup"><span data-stu-id="a529c-292">Inserted a batch of entities</span></span> 
> * <span data-ttu-id="a529c-293">Načíst jednu entitu</span><span class="sxs-lookup"><span data-stu-id="a529c-293">Retrieved a single entity</span></span> 
> * <span data-ttu-id="a529c-294">Dotazovaný entity pomocí automatické sekundární indexy</span><span class="sxs-lookup"><span data-stu-id="a529c-294">Queried entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="a529c-295">Nahradí entitu</span><span class="sxs-lookup"><span data-stu-id="a529c-295">Replaced an entity</span></span> 
> * <span data-ttu-id="a529c-296">Odstranit entity</span><span class="sxs-lookup"><span data-stu-id="a529c-296">Deleted an entity</span></span> 
> * <span data-ttu-id="a529c-297">Odstranit tabulku</span><span class="sxs-lookup"><span data-stu-id="a529c-297">Deleted a table</span></span>  

<span data-ttu-id="a529c-298">Teď můžete pokračovat dalším kurzu toohello a další informace o dotazování dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="a529c-298">You can now proceed toohello next tutorial and learn more about querying table data.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="a529c-299">Dotaz s hello tabulky rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a529c-299">Query with hello Table API</span></span>](tutorial-query-table.md)
