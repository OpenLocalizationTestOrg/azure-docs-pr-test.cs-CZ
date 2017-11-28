---
title: "aaaLoad terabajtů dat do SQL Data Warehouse | Microsoft Docs"
description: "Ukazuje, jak 1 TB dat je možné načíst do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: e63f60461b082b0e3979004cb631dbf4a6710de3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a><span data-ttu-id="69729-103">Načtení 1 TB do Azure SQL Data Warehouse pomocí služby Data Factory v části 15 minut</span><span class="sxs-lookup"><span data-stu-id="69729-103">Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Data Factory</span></span>
<span data-ttu-id="69729-104">[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) je databáze založená na cloudu, škálování dokáže zpracovávat ohromné objemy dat, relačních i nerelačních.</span><span class="sxs-lookup"><span data-stu-id="69729-104">[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span>  <span data-ttu-id="69729-105">Postavená na architektuře massively parallel processing (MPP), SQL Data Warehouse je optimalizovaná pro podnikové datového skladu úlohy.</span><span class="sxs-lookup"><span data-stu-id="69729-105">Built on massively parallel processing (MPP) architecture, SQL Data Warehouse is optimized for enterprise data warehouse workloads.</span></span>  <span data-ttu-id="69729-106">Nabízí cloud pružnost s hello flexibilitu tooscale úložiště a výpočetní nezávisle.</span><span class="sxs-lookup"><span data-stu-id="69729-106">It offers cloud elasticity with hello flexibility tooscale storage and compute independently.</span></span>

<span data-ttu-id="69729-107">Začínáme s Azure SQL Data Warehouse je teď jednodušší než kdy použití **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="69729-107">Getting started with Azure SQL Data Warehouse is now easier than ever using **Azure Data Factory**.</span></span>  <span data-ttu-id="69729-108">Azure Data Factory je plně spravovaná Cloudová datová služba integrace, který lze použít toopopulate SQL Data Warehouse s hello data z vašeho stávajícího systému a ukládání je hodně času při vyhodnocení SQL Data Warehouse a sestavování analytické údaje řešení.</span><span class="sxs-lookup"><span data-stu-id="69729-108">Azure Data Factory is a fully managed cloud-based data integration service, which can be used toopopulate a SQL Data Warehouse with hello data from your existing system, and saving you valuable time while evaluating SQL Data Warehouse and building your analytics solutions.</span></span> <span data-ttu-id="69729-109">Tady jsou klíčové výhody hello načítání dat do Azure SQL Data Warehouse pomocí Azure Data Factory:</span><span class="sxs-lookup"><span data-stu-id="69729-109">Here are hello key benefits of loading data into Azure SQL Data Warehouse using Azure Data Factory:</span></span>

* <span data-ttu-id="69729-110">**Snadno tooset až**: krok 5 intuitivní průvodce bez potřeby skriptování.</span><span class="sxs-lookup"><span data-stu-id="69729-110">**Easy tooset up**: 5-step intuitive wizard with no scripting required.</span></span>
* <span data-ttu-id="69729-111">**Podpora úložiště bohaté dat**: integrovanou podporu pro bohatou sadu místní a Cloudová datová úložiště.</span><span class="sxs-lookup"><span data-stu-id="69729-111">**Rich data store support**: built-in support for a rich set of on-premises and cloud-based data stores.</span></span>
* <span data-ttu-id="69729-112">**Zabezpečení a dodržování**: data se přenáší prostřednictvím protokolu HTTPS nebo ExpressRoute a přítomnost globální služby zajišťuje vaše data nikdy neopustí hranice zeměpisné hello</span><span class="sxs-lookup"><span data-stu-id="69729-112">**Secure and compliant**: data is transferred over HTTPS or ExpressRoute, and global service presence ensures your data never leaves hello geographical boundary</span></span>
* <span data-ttu-id="69729-113">**Bezkonkurenční výkonu pomocí funkce PolyBase** – je hello nejúčinnější způsob, jak toomove data do Azure SQL Data Warehouse pomocí Polybase.</span><span class="sxs-lookup"><span data-stu-id="69729-113">**Unparalleled performance by using PolyBase** – Using Polybase is hello most efficient way toomove data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="69729-114">Pomocí hello pracovní funkce objektů blob, můžete dosáhnout vysokého zatížení rychlosti ze všech typů úložišť dat kromě úložiště objektů Azure Blob, které hello Polybase podporuje ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="69729-114">Using hello staging blob feature, you can achieve high load speeds from all types of data stores besides Azure Blob storage, which hello Polybase supports by default.</span></span>

<span data-ttu-id="69729-115">Tento článek ukazuje, jak toouse Průvodce kopírováním služby Data Factory data tooload 1 TB z Azure Blob Storage do Azure SQL Data Warehouse v části 15 minut, při propustnost přes 1,2 GB/s.</span><span class="sxs-lookup"><span data-stu-id="69729-115">This article shows you how toouse Data Factory Copy Wizard tooload 1-TB data from Azure Blob Storage into Azure SQL Data Warehouse in under 15 minutes, at over 1.2 GBps throughput.</span></span>

<span data-ttu-id="69729-116">Tento článek obsahuje podrobné pokyny pro přesun dat do Azure SQL Data Warehouse pomocí Průvodce kopírováním hello.</span><span class="sxs-lookup"><span data-stu-id="69729-116">This article provides step-by-step instructions for moving data into Azure SQL Data Warehouse by using hello Copy Wizard.</span></span>

> [!NOTE]
>  <span data-ttu-id="69729-117">Obecné informace o možnostech objektu pro vytváření dat při přesunu dat do/z Azure SQL Data Warehouse najdete v tématu [přesunout tooand dat z Azure SQL Data Warehouse pomocí Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) článku.</span><span class="sxs-lookup"><span data-stu-id="69729-117">For general information about capabilities of Data Factory in moving data to/from Azure SQL Data Warehouse, see [Move data tooand from Azure SQL Data Warehouse using Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) article.</span></span>
>
> <span data-ttu-id="69729-118">Můžete také vytvořit pomocí portálu Azure, Visual Studio, prostředí PowerShell, kanály atd. V tématu [kurz: kopírování dat z tooAzure objektů Blob v Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) rychlé návod s podrobnými pokyny pro používání hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="69729-118">You can also build pipelines using Azure portal, Visual Studio, PowerShell, etc. See [Tutorial: Copy data from Azure Blob tooAzure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for a quick walkthrough with step-by-step instructions for using hello Copy Activity in Azure Data Factory.</span></span>  
>
>

## <a name="prerequisites"></a><span data-ttu-id="69729-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="69729-119">Prerequisites</span></span>
* <span data-ttu-id="69729-120">Azure Blob Storage: Tento experiment používá Azure Blob úložiště (GRS) pro ukládání TPC-H testování datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="69729-120">Azure Blob Storage: this experiment uses Azure Blob Storage (GRS) for storing TPC-H testing dataset.</span></span>  <span data-ttu-id="69729-121">Pokud nemáte účet úložiště Azure, přečtěte si [jak toocreate účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="69729-121">If you do not have an Azure storage account, learn [how toocreate a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>
* <span data-ttu-id="69729-122">[TPC-H](http://www.tpc.org/tpch/) data: přidáme toouse TPC-H jako hello testování datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="69729-122">[TPC-H](http://www.tpc.org/tpch/) data: we are going toouse TPC-H as hello testing dataset.</span></span>  <span data-ttu-id="69729-123">toodo, je nutné, aby toouse `dbgen` z TPC-H nástrojů, který umožňuje generovat datovou sadu hello.</span><span class="sxs-lookup"><span data-stu-id="69729-123">toodo that, you need toouse `dbgen` from TPC-H toolkit, which helps you generate hello dataset.</span></span>  <span data-ttu-id="69729-124">Můžete buď stáhnout zdrojového kódu pro `dbgen` z [TPC nástroje](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) a jeho kompilace sami nebo stažení hello kompilovány binární z [Githubu](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span><span class="sxs-lookup"><span data-stu-id="69729-124">You can either download source code for `dbgen` from [TPC Tools](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) and compile it yourself, or download hello compiled binary from [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span></span>  <span data-ttu-id="69729-125">Spuštění dbgen.exe s hello následující příkazy toogenerate 1 TB plochý soubor pro `lineitem` tabulky šíření mezi 10 souborů:</span><span class="sxs-lookup"><span data-stu-id="69729-125">Run dbgen.exe with hello following commands toogenerate 1 TB flat file for `lineitem` table spread across 10 files:</span></span>

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * <span data-ttu-id="69729-126">…</span><span class="sxs-lookup"><span data-stu-id="69729-126">…</span></span>
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    <span data-ttu-id="69729-127">Nyní kopie hello generování souborů tooAzure objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="69729-127">Now copy hello generated files tooAzure Blob.</span></span>  <span data-ttu-id="69729-128">Odkazovat příliš[přesunutí data tooand ze v místním systému souborů pomocí Azure Data Factory](data-factory-onprem-file-system-connector.md) jak toodo, pomocí kopírování ADF.</span><span class="sxs-lookup"><span data-stu-id="69729-128">Refer too[Move data tooand from an on-premises file system by using Azure Data Factory](data-factory-onprem-file-system-connector.md) for how toodo that using ADF Copy.</span></span>    
* <span data-ttu-id="69729-129">Azure SQL Data Warehouse: tohoto experimentu načte data do Azure SQL Data Warehouse vytvořené pomocí 6000 Dwu</span><span class="sxs-lookup"><span data-stu-id="69729-129">Azure SQL Data Warehouse: this experiment loads data into Azure SQL Data Warehouse created with 6,000 DWUs</span></span>

    <span data-ttu-id="69729-130">Odkazovat příliš[vytvoření Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) podrobné pokyny, jak server toocreate SQL datového skladu databáze.</span><span class="sxs-lookup"><span data-stu-id="69729-130">Refer too[Create an Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) for detailed instructions on how toocreate a SQL Data Warehouse database.</span></span>  <span data-ttu-id="69729-131">tooget hello nejlepší možný zatížení výkonu do SQL Data Warehouse pomocí Polybase, jsme vyberte maximální počet povolen v nastavení hello výkonu, které je 6000 Dwu jednotky datového skladu (Dwu).</span><span class="sxs-lookup"><span data-stu-id="69729-131">tooget hello best possible load performance into SQL Data Warehouse using Polybase, we choose maximum number of Data Warehouse Units (DWUs) allowed in hello Performance setting, which is 6,000 DWUs.</span></span>

  > [!NOTE]
  > <span data-ttu-id="69729-132">Při načítání z Azure Blob, načítání výkonu hello dat je přímo úměrná toohello počet Dwu nakonfigurujete na hello SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="69729-132">When loading from Azure Blob, hello data loading performance is directly proportional toohello number of DWUs you configure on hello SQL Data Warehouse:</span></span>
  >
  > <span data-ttu-id="69729-133">Načtení 1 TB do 1000 DWU SQL Data Warehouse přebírá načítání 1 TB 87 minut (propustnost ~ 200 MB/s) do 2 000 DWU SQL Data Warehouse trvá 46 minut (propustnost ~ 380 MB/s) načítání 1 TB do 6000 DWU SQL Data Warehouse přebírá 14 minut (propustnost ~1.2 GB/s)</span><span class="sxs-lookup"><span data-stu-id="69729-133">Loading 1 TB into 1,000 DWU SQL Data Warehouse takes 87 minutes (~200 MBps throughput) Loading 1 TB into 2,000 DWU SQL Data Warehouse takes 46 minutes (~380 MBps throughput) Loading 1 TB into 6,000 DWU SQL Data Warehouse takes 14 minutes (~1.2 GBps throughput)</span></span>
  >
  >

    <span data-ttu-id="69729-134">toocreate SQL Data Warehouse s 6000 Dwu, posuvník hello výkonu všechny toohello způsob hello práva:</span><span class="sxs-lookup"><span data-stu-id="69729-134">toocreate a SQL Data Warehouse with 6,000 DWUs, move hello Performance slider all hello way toohello right:</span></span>

    ![Výkon posuvníku](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    <span data-ttu-id="69729-136">Pro existující databázi, který není konfigurován s 6000 Dwu můžete postupně škálovat ji pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="69729-136">For an existing database that is not configured with 6,000 DWUs, you can scale it up using Azure portal.</span></span>  <span data-ttu-id="69729-137">Přejděte toohello databáze na portálu Azure a je **škálování** tlačítka na hello **přehled** panelu ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="69729-137">Navigate toohello database in Azure portal, and there is a **Scale** button in hello **Overview** panel shown in hello following image:</span></span>

    ![Stupnice – tlačítko](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    <span data-ttu-id="69729-139">Klikněte na tlačítko hello **škálování** panelu, přesunout hello posuvníku toohello maximální hodnotu a klikněte na tlačítko tooopen hello následující **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="69729-139">Click hello **Scale** button tooopen hello following panel, move hello slider toohello maximum value, and click **Save** button.</span></span>

    ![Dialogové okno škálování](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    <span data-ttu-id="69729-141">Tento experiment načte data do Azure SQL Data Warehouse pomocí `xlargerc` Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="69729-141">This experiment loads data into Azure SQL Data Warehouse using `xlargerc` resource class.</span></span>

    <span data-ttu-id="69729-142">nejlepší možné propustnost tooachieve kopie musí toobe provádí pomocí SQL Data Warehouse uživatel patřící příliš`xlargerc` Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="69729-142">tooachieve best possible throughput, copy needs toobe performed using a SQL Data Warehouse user belonging too`xlargerc` resource class.</span></span>  <span data-ttu-id="69729-143">Zjistěte, jak toodo, pomocí následujících [změnit v příkladu třída prostředků uživatele](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="69729-143">Learn how toodo that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>  
* <span data-ttu-id="69729-144">Vytvoření schématu cílové tabulky v databázi Azure SQL Data Warehouse, tak, že spustíte následující příkaz DDL hello:</span><span class="sxs-lookup"><span data-stu-id="69729-144">Create destination table schema in Azure SQL Data Warehouse database, by running hello following DDL statement:</span></span>

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
<span data-ttu-id="69729-145">Dokončení požadovaných krocích hello jsme jsou nyní aktivitou kopírování hello připravené tooconfigure pomocí Průvodce kopírováním hello.</span><span class="sxs-lookup"><span data-stu-id="69729-145">With hello prerequisite steps completed, we are now ready tooconfigure hello copy activity using hello Copy Wizard.</span></span>

## <a name="launch-copy-wizard"></a><span data-ttu-id="69729-146">Spuštění průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="69729-146">Launch Copy Wizard</span></span>
1. <span data-ttu-id="69729-147">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="69729-147">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="69729-148">Klikněte na tlačítko **+ nový** z levého horního rohu hello, klikněte na tlačítko **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="69729-148">Click **+ NEW** from hello top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="69729-149">V hello **nový objekt pro vytváření dat** okno:</span><span class="sxs-lookup"><span data-stu-id="69729-149">In hello **New data factory** blade:</span></span>

   1. <span data-ttu-id="69729-150">Zadejte **LoadIntoSQLDWDataFactory** pro hello **název**.</span><span class="sxs-lookup"><span data-stu-id="69729-150">Enter **LoadIntoSQLDWDataFactory** for hello **name**.</span></span>
       <span data-ttu-id="69729-151">Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="69729-151">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="69729-152">Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "LoadIntoSQLDWDataFactory" není k dispozici**, změňte hello název objektu pro vytváření dat hello (například yournameLoadIntoSQLDWDataFactory) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="69729-152">If you receive hello error: **Data factory name “LoadIntoSQLDWDataFactory” is not available**, change hello name of hello data factory (for example, yournameLoadIntoSQLDWDataFactory) and try creating again.</span></span> <span data-ttu-id="69729-153">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="69729-153">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>  
   2. <span data-ttu-id="69729-154">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="69729-154">Select your Azure **subscription**.</span></span>
   3. <span data-ttu-id="69729-155">Pro skupinu prostředků proveďte jednu z následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="69729-155">For Resource Group, do one of hello following steps:</span></span>
      1. <span data-ttu-id="69729-156">Vyberte **použít existující** tooselect existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="69729-156">Select **Use existing** tooselect an existing resource group.</span></span>
      2. <span data-ttu-id="69729-157">Vyberte **vytvořit nový** tooenter název pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="69729-157">Select **Create new** tooenter a name for a resource group.</span></span>
   4. <span data-ttu-id="69729-158">Vyberte **umístění** hello data Factory.</span><span class="sxs-lookup"><span data-stu-id="69729-158">Select a **location** for hello data factory.</span></span>
   5. <span data-ttu-id="69729-159">Vyberte **Pin toodashboard** políčko v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="69729-159">Select **Pin toodashboard** check box at hello bottom of hello blade.</span></span>  
   6. <span data-ttu-id="69729-160">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="69729-160">Click **Create**.</span></span>
4. <span data-ttu-id="69729-161">Po dokončení vytvoření hello uvidíte hello **Data Factory** jak je uvedeno v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="69729-161">After hello creation is complete, you see hello **Data Factory** blade as shown in hello following image:</span></span>

   ![Domovská stránka objektu pro vytváření dat](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. <span data-ttu-id="69729-163">Na domovské stránce objektu pro vytváření dat hello, klikněte na tlačítko hello **kopírování dat** dlaždici toolaunch **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="69729-163">On hello Data Factory home page, click hello **Copy data** tile toolaunch **Copy Wizard**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="69729-164">Pokud se zobrazí, že hello webový prohlížeč zasekl ve fázi "autorizace …", zakažte/zrušte zaškrtnutí políčka **blokovat soubory cookie třetích stran a data lokality** nastavení (nebo) zachovat povolené a vytvořte výjimku pro **login.microsoftonline.com**a poté se pokuste spustit hello průvodce znovu.</span><span class="sxs-lookup"><span data-stu-id="69729-164">If you see that hello web browser is stuck at "Authorizing...", disable/uncheck **Block third party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching hello wizard again.</span></span>
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a><span data-ttu-id="69729-165">Krok 1: Konfigurace plánu pro načítání dat</span><span class="sxs-lookup"><span data-stu-id="69729-165">Step 1: Configure data loading schedule</span></span>
<span data-ttu-id="69729-166">prvním krokem Hello je tooconfigure hello data načítání plánu.</span><span class="sxs-lookup"><span data-stu-id="69729-166">hello first step is tooconfigure hello data loading schedule.</span></span>  

<span data-ttu-id="69729-167">V hello **vlastnosti** stránky:</span><span class="sxs-lookup"><span data-stu-id="69729-167">In hello **Properties** page:</span></span>

1. <span data-ttu-id="69729-168">Zadejte **CopyFromBlobToAzureSqlDataWarehouse** pro **název úlohy**</span><span class="sxs-lookup"><span data-stu-id="69729-168">Enter **CopyFromBlobToAzureSqlDataWarehouse** for **Task name**</span></span>
2. <span data-ttu-id="69729-169">Vyberte **spustit jednou nyní** možnost.</span><span class="sxs-lookup"><span data-stu-id="69729-169">Select **Run once now** option.</span></span>   
3. <span data-ttu-id="69729-170">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="69729-170">Click **Next**.</span></span>  

    ![Průvodce kopírováním – stránky vlastností](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a><span data-ttu-id="69729-172">Krok 2: Konfigurace zdroje</span><span class="sxs-lookup"><span data-stu-id="69729-172">Step 2: Configure source</span></span>
<span data-ttu-id="69729-173">Tato část obsahuje hello kroky tooconfigure hello zdroj: Azure Blob obsahující hello 1 TB TPC-H položky na řádku soubory.</span><span class="sxs-lookup"><span data-stu-id="69729-173">This section shows you hello steps tooconfigure hello source: Azure Blob containing hello 1-TB TPC-H line item files.</span></span>

1. <span data-ttu-id="69729-174">Vyberte hello **Azure Blob Storage** jako hello data ukládat a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="69729-174">Select hello **Azure Blob Storage** as hello data store and click **Next**.</span></span>

    ![Průvodce kopírováním - vyberte zdroj stránky](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. <span data-ttu-id="69729-176">Vyplňte hello informace o připojení pro hello účtu úložiště Azure Blob a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="69729-176">Fill in hello connection information for hello Azure Blob storage account, and click **Next**.</span></span>

    ![Průvodce kopírováním - informace o připojení zdroje](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. <span data-ttu-id="69729-178">Zvolte hello **složky** obsahující hello TPC-H řádku položky souborů a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="69729-178">Choose hello **folder** containing hello TPC-H line item files and click **Next**.</span></span>

    ![Průvodce kopírováním – vyberte vstupní složky](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. <span data-ttu-id="69729-180">Po kliknutí na tlačítko **Další**, nastavení formátu souboru hello se rozpoznávají automaticky.</span><span class="sxs-lookup"><span data-stu-id="69729-180">Upon clicking **Next**, hello file format settings are detected automatically.</span></span>  <span data-ttu-id="69729-181">Zkontrolujte, zda je tento sloupec oddělovač toomake ' | 'místo hello čárkami výchozí','.</span><span class="sxs-lookup"><span data-stu-id="69729-181">Check toomake sure that column delimiter is ‘|’ instead of hello default comma ‘,’.</span></span>  <span data-ttu-id="69729-182">Klikněte na tlačítko **Další** po mít zobrazení náhledu dat hello.</span><span class="sxs-lookup"><span data-stu-id="69729-182">Click **Next** after you have previewed hello data.</span></span>

    ![Průvodce kopírováním – nastavení formátu souboru](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a><span data-ttu-id="69729-184">Krok 3: Konfigurace cílového</span><span class="sxs-lookup"><span data-stu-id="69729-184">Step 3: Configure destination</span></span>
<span data-ttu-id="69729-185">V této části se dozvíte, jak tooconfigure hello cílové: `lineitem` tabulky v databázi Azure SQL Data Warehouse hello.</span><span class="sxs-lookup"><span data-stu-id="69729-185">This section shows you how tooconfigure hello destination: `lineitem` table in hello Azure SQL Data Warehouse database.</span></span>

1. <span data-ttu-id="69729-186">Zvolte **Azure SQL Data Warehouse** jako hello cíl uložení a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="69729-186">Choose **Azure SQL Data Warehouse** as hello destination store and click **Next**.</span></span>

    ![Průvodce kopírováním - vyberte cílového úložiště dat](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. <span data-ttu-id="69729-188">Zadejte informace o připojení hello pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="69729-188">Fill in hello connection information for Azure SQL Data Warehouse.</span></span>  <span data-ttu-id="69729-189">Je nutné zadat hello uživatele, který je členem hello role `xlargerc` (viz hello **požadavky** části Podrobné pokyny) a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="69729-189">Make sure you specify hello user that is a member of hello role `xlargerc` (see hello **prerequisites** section for detailed instructions), and click **Next**.</span></span>

    ![Průvodce kopírováním - informace o cílovém připojení](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. <span data-ttu-id="69729-191">Zvolte hello cílové tabulky a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="69729-191">Choose hello destination table and click **Next**.</span></span>

    ![Zkopírujte - tabulky mapování stránku průvodce](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. <span data-ttu-id="69729-193">Schéma mapování stránce, nechte nezaškrtnuté možnost "Použijí mapování sloupce" a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="69729-193">In Schema mapping page, leave "Apply column mapping" option unchecked and click **Next**.</span></span>

## <a name="step-4-performance-settings"></a><span data-ttu-id="69729-194">Krok 4: Nastavení výkonu</span><span class="sxs-lookup"><span data-stu-id="69729-194">Step 4: Performance settings</span></span>

<span data-ttu-id="69729-195">**Povolit polybase** je ve výchozím nastavení zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="69729-195">**Allow polybase** is checked by default.</span></span>  <span data-ttu-id="69729-196">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="69729-196">Click **Next**.</span></span>

![Zkopírujte – schéma mapování stránku průvodce](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a><span data-ttu-id="69729-198">Krok 5: Nasazení a monitorování zatížení výsledky</span><span class="sxs-lookup"><span data-stu-id="69729-198">Step 5: Deploy and monitor load results</span></span>
1. <span data-ttu-id="69729-199">Klikněte na tlačítko **Dokončit** toodeploy tlačítko.</span><span class="sxs-lookup"><span data-stu-id="69729-199">Click **Finish** button toodeploy.</span></span>

    ![Průvodce kopírováním - souhrnná stránka](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. <span data-ttu-id="69729-201">Po dokončení nasazení hello klikněte na tlačítko `Click here toomonitor copy pipeline` kopie hello toomonitor spustit průběh.</span><span class="sxs-lookup"><span data-stu-id="69729-201">After hello deployment is complete, click `Click here toomonitor copy pipeline` toomonitor hello copy run progress.</span></span> <span data-ttu-id="69729-202">Vyberte hello kanál kopírování jste vytvořili v hello **aktivity Windows** seznamu.</span><span class="sxs-lookup"><span data-stu-id="69729-202">Select hello copy pipeline you created in hello **Activity Windows** list.</span></span>

    ![Průvodce kopírováním - souhrnná stránka](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    <span data-ttu-id="69729-204">Můžete zobrazit podrobnosti o spuštění v hello kopie hello **aktivity okno Průzkumníka** v pravém panelu hello, včetně hello datový svazek číst ze zdroje a zapsána do cílového, doba trvání a hello průměrnou propustností pro hello spustit.</span><span class="sxs-lookup"><span data-stu-id="69729-204">You can view hello copy run details in hello **Activity Window Explorer** in hello right panel, including hello data volume read from source and written into destination, duration, and hello average throughput for hello run.</span></span>

    <span data-ttu-id="69729-205">Jak vidíte z hello následující snímek obrazovky, kopírování 1 TB z Azure Blob Storage do SQL Data Warehouse trvalo 14 minut, efektivně dosahování 1.22 GB/s propustnosti!</span><span class="sxs-lookup"><span data-stu-id="69729-205">As you can see from hello following screen shot, copying 1 TB from Azure Blob Storage into SQL Data Warehouse took 14 minutes, effectively achieving 1.22 GBps throughput!</span></span>

    ![Průvodce kopírováním – dialogové okno úspěšně vytvořila.](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a><span data-ttu-id="69729-207">Osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="69729-207">Best practices</span></span>
<span data-ttu-id="69729-208">Tady je několik osvědčených postupů pro spuštění databáze Azure SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="69729-208">Here are a few best practices for running your Azure SQL Data Warehouse database:</span></span>

* <span data-ttu-id="69729-209">Při načítání do CLUSTEROVANÝ INDEX COLUMNSTORE, použijte větší Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="69729-209">Use a larger resource class when loading into a CLUSTERED COLUMNSTORE INDEX.</span></span>
* <span data-ttu-id="69729-210">Pro efektivnější spojení zvažte použití algoritmu hash distribuce podle vyberte sloupce místo výchozí round robin distribuční.</span><span class="sxs-lookup"><span data-stu-id="69729-210">For more efficient joins, consider using hash distribution by a select column instead of default round robin distribution.</span></span>
* <span data-ttu-id="69729-211">Vyšší rychlost zatížení zvažte použití haldy pro přechodný data.</span><span class="sxs-lookup"><span data-stu-id="69729-211">For faster load speeds, consider using heap for transient data.</span></span>
* <span data-ttu-id="69729-212">Vytvoření statistiky po dokončení načítání Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="69729-212">Create statistics after you finish loading Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="69729-213">V tématu [osvědčené postupy pro Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="69729-213">See [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69729-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69729-214">Next steps</span></span>
* <span data-ttu-id="69729-215">[Průvodce kopírováním služby Data Factory](data-factory-copy-wizard.md) – Tento článek obsahuje podrobné informace o hello Průvodce kopírováním.</span><span class="sxs-lookup"><span data-stu-id="69729-215">[Data Factory Copy Wizard](data-factory-copy-wizard.md) - This article provides details about hello Copy Wizard.</span></span>
* <span data-ttu-id="69729-216">[Zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md) – Tento článek obsahuje vyladění průvodce a měření výkonu odkaz hello.</span><span class="sxs-lookup"><span data-stu-id="69729-216">[Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) - This article contains hello reference performance measurements and tuning guide.</span></span>
