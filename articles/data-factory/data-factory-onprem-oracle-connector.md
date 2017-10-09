---
title: "aaaCopy data do nebo z databáze Oracle pomocí služby Data Factory | Microsoft Docs"
description: "Zjistěte, jak toocopy data z databáze Oracle, který je místně pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: adb6d5fbe38e18791616ac77e8179970bbea37fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a><span data-ttu-id="2eed9-103">Kopírování dat z místní Oracle pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="2eed9-103">Copy data to/from on-premises Oracle using Azure Data Factory</span></span>
<span data-ttu-id="2eed9-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze Oracle na místě.</span><span class="sxs-lookup"><span data-stu-id="2eed9-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from an on-premises Oracle database.</span></span> <span data-ttu-id="2eed9-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="2eed9-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="2eed9-106">Supported scenarios</span></span>
<span data-ttu-id="2eed9-107">Může kopírovat data **z databáze Oracle** toohello následující úložišť dat:</span><span class="sxs-lookup"><span data-stu-id="2eed9-107">You can copy data **from an Oracle database** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="2eed9-108">Data můžete zkopírovat z hello následující úložišť dat **databáze Oracle tooan**:</span><span class="sxs-lookup"><span data-stu-id="2eed9-108">You can copy data from hello following data stores **tooan Oracle database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a><span data-ttu-id="2eed9-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2eed9-109">Prerequisites</span></span>
<span data-ttu-id="2eed9-110">Objekt pro vytváření dat podporuje připojování zdroje Oracle tooon místní pomocí hello Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="2eed9-110">Data Factory supports connecting tooon-premises Oracle sources using hello Data Management Gateway.</span></span> <span data-ttu-id="2eed9-111">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) toolearn článek o Brána pro správu dat a [přesun dat z místní toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k nastavení brány hello datovém kanálu toomove data.</span><span class="sxs-lookup"><span data-stu-id="2eed9-111">See [Data Management Gateway](data-factory-data-management-gateway.md) article toolearn about Data Management Gateway and [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway a data pipeline toomove data.</span></span>

<span data-ttu-id="2eed9-112">Vyžaduje se brána, i když hello Oracle je hostovaná ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="2eed9-112">Gateway is required even if hello Oracle is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="2eed9-113">Hello brány můžete nainstalovat na stejný virtuální počítač IaaS jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit databázi toohello hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-113">You can install hello gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="2eed9-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="2eed9-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="2eed9-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="2eed9-115">Supported versions and installation</span></span>
<span data-ttu-id="2eed9-116">Tento konektor Oracle podporují dvě verze ovladače:</span><span class="sxs-lookup"><span data-stu-id="2eed9-116">This Oracle connector support two versions of drivers:</span></span>

- <span data-ttu-id="2eed9-117">**Ovladač Microsoft pro Oracle (doporučeno)**: spouštění z brány správy dat verze 2.7 ovladač Microsoft pro Oracle se nainstaluje automaticky společně hello brány, proto není nutné tooadditionally popisovač hello ovladače v pořadí tooOracle tooestablish připojení a může také nastat, lepší výkon kopírování pomocí tohoto ovladače.</span><span class="sxs-lookup"><span data-stu-id="2eed9-117">**Microsoft driver for Oracle (recommended)**: starting from Data Management Gateway version 2.7, a Microsoft driver for Oracle is automatically installed along with hello gateway, so you don't need tooadditionally handle hello driver in order tooestablish connectivity tooOracle, and you can also experience better copy performance using this driver.</span></span> <span data-ttu-id="2eed9-118">Následující verze systému Oracle jsou podporovány databází:</span><span class="sxs-lookup"><span data-stu-id="2eed9-118">Below versions of Oracle databases are supported:</span></span>
    - <span data-ttu-id="2eed9-119">R1 Oracle 12c (12.1)</span><span class="sxs-lookup"><span data-stu-id="2eed9-119">Oracle 12c R1 (12.1)</span></span>
    - <span data-ttu-id="2eed9-120">R1 Oracle 11g nebo R2 (11.1, 11.2)</span><span class="sxs-lookup"><span data-stu-id="2eed9-120">Oracle 11g R1, R2 (11.1, 11.2)</span></span>
    - <span data-ttu-id="2eed9-121">R1 Oracle 10g, R2 (10,1, 10.2)</span><span class="sxs-lookup"><span data-stu-id="2eed9-121">Oracle 10g R1, R2 (10.1, 10.2)</span></span>
    - <span data-ttu-id="2eed9-122">Oracle 9i R1, R2 (9.0.1, 9.2)</span><span class="sxs-lookup"><span data-stu-id="2eed9-122">Oracle 9i R1, R2 (9.0.1, 9.2)</span></span>
    - <span data-ttu-id="2eed9-123">Oracle 8i R3 (8.1.7)</span><span class="sxs-lookup"><span data-stu-id="2eed9-123">Oracle 8i R3 (8.1.7)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2eed9-124">Ovladač Microsoft pro Oracle aktuálně podporuje jenom kopírování dat z Oracle, ale není zápis tooOracle.</span><span class="sxs-lookup"><span data-stu-id="2eed9-124">Currently Microsoft driver for Oracle only supports copying data from Oracle but not writing tooOracle.</span></span> <span data-ttu-id="2eed9-125">A Všimněte si, že hello test připojení funkci karta diagnostiku brány správy dat nepodporuje tento ovladač.</span><span class="sxs-lookup"><span data-stu-id="2eed9-125">And note hello test connection capability in Data Management Gateway Diagnostics tab does not support this driver.</span></span> <span data-ttu-id="2eed9-126">Alternativně můžete použít hello kopie Průvodce toovalidate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="2eed9-126">Alternatively, you can use hello copy wizard toovalidate hello connectivity.</span></span>
>

- <span data-ttu-id="2eed9-127">**Zprostředkovatel dat Oracle pro .NET:** můžete také toouse poskytovatele dat Oracle toocopy data z / tooOracle.</span><span class="sxs-lookup"><span data-stu-id="2eed9-127">**Oracle Data Provider for .NET:** you can also choose toouse Oracle Data Provider toocopy data from/tooOracle.</span></span> <span data-ttu-id="2eed9-128">Tato součást je součástí [Oracle Data přístup součásti pro Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2eed9-128">This component is included in [Oracle Data Access Components for Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span></span> <span data-ttu-id="2eed9-129">Nainstalujte příslušnou verzi hello (32 nebo 64bitový) hello počítače, kde je nainstalován hello brány.</span><span class="sxs-lookup"><span data-stu-id="2eed9-129">Install hello appropriate version (32/64 bit) on hello machine where hello gateway is installed.</span></span> <span data-ttu-id="2eed9-130">[Zprostředkovatel dat Oracle .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) přístup tooOracle Database 10 g verze 2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2eed9-130">[Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) can access tooOracle Database 10g Release 2 or later.</span></span>

    <span data-ttu-id="2eed9-131">Pokud si zvolíte možnost "XCopy instalace", postupujte podle kroků v hello readme.htm.</span><span class="sxs-lookup"><span data-stu-id="2eed9-131">If you choose “XCopy Installation”, follow steps in hello readme.htm.</span></span> <span data-ttu-id="2eed9-132">Doporučujeme že vybrat instalační program hello pomocí uživatelského rozhraní (bez XCopy jeden).</span><span class="sxs-lookup"><span data-stu-id="2eed9-132">We recommend you choose hello installer with UI (non-XCopy one).</span></span>

    <span data-ttu-id="2eed9-133">Po instalaci poskytovatele hello **restartujte** hello hostitelská služba Brána pro správu dat na počítači pomocí služby aplet (nebo) Správce konfigurace brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="2eed9-133">After installing hello provider, **restart** hello Data Management Gateway host service on your machine using Services applet (or) Data Management Gateway Configuration Manager.</span></span>  

<span data-ttu-id="2eed9-134">Pokud používáte kopie Průvodce tooauthor hello kopie kanálu, typu ovladač hello bude určen automaticky.</span><span class="sxs-lookup"><span data-stu-id="2eed9-134">If you use copy wizard tooauthor hello copy pipeline, hello driver type will be auto-determined.</span></span> <span data-ttu-id="2eed9-135">Ovladač Microsoft použije ve výchozím nastavení, pokud vaše verze brány je nižší než 2.7 nebo zvolte Oracle jako jímku.</span><span class="sxs-lookup"><span data-stu-id="2eed9-135">Microsoft driver will be used by default, unless your gateway version is lower than 2.7 or you choose Oracle as sink.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2eed9-136">Začínáme</span><span class="sxs-lookup"><span data-stu-id="2eed9-136">Getting started</span></span>
<span data-ttu-id="2eed9-137">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z databáze Oracle místně pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2eed9-137">You can create a pipeline with a copy activity that moves data to/from an on-premises Oracle database by using different tools/APIs.</span></span>

<span data-ttu-id="2eed9-138">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="2eed9-138">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="2eed9-139">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-139">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="2eed9-140">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="2eed9-140">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="2eed9-141">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="2eed9-141">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="2eed9-142">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="2eed9-142">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="2eed9-143">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="2eed9-143">Create a **data factory**.</span></span> <span data-ttu-id="2eed9-144">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="2eed9-144">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="2eed9-145">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="2eed9-145">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="2eed9-146">Například pokud kopírování dat z databáze tooan Oralce úložiště objektů blob v Azure, vytvoříte dvě propojené služby toolink databáze Oracle a účet úložiště Azure tooyour služby data factory.</span><span class="sxs-lookup"><span data-stu-id="2eed9-146">For example, if you are copying data from an Oralce database tooan Azure blob storage, you create two linked services toolink your Oracle database and Azure storage account tooyour data factory.</span></span> <span data-ttu-id="2eed9-147">Vlastnosti propojené služby, které jsou specifické tooOracle, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="2eed9-147">For linked service properties that are specific tooOracle, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="2eed9-148">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="2eed9-148">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="2eed9-149">V příkladu hello uvedených v posledním kroku hello vytvoříte tabulku hello toospecify datové sady ve vaší databázi Oracle, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-149">In hello example mentioned in hello last step, you create a dataset toospecify hello table in your Oracle database that contains hello input data.</span></span> <span data-ttu-id="2eed9-150">Vytvoření kontejneru objektů blob hello toospecify s jinou datovou sadu a hello složky, která obsahuje hello data zkopírovaných z hello databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="2eed9-150">And, you create another dataset toospecify hello blob container and hello folder that holds hello data copied from hello Oracle database.</span></span> <span data-ttu-id="2eed9-151">Vlastnosti datové sady, které jsou specifické tooOracle, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="2eed9-151">For dataset properties that are specific tooOracle, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="2eed9-152">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="2eed9-152">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="2eed9-153">V příkladu hello již bylo zmíněno dříve použijete OracleSource jako zdroj a BlobSink jako jímku pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-153">In hello example mentioned earlier, you use OracleSource as a source and BlobSink as a sink for hello copy activity.</span></span> <span data-ttu-id="2eed9-154">Podobně pokud zkopírujete z Azure Blob Storage tooOracle databáze, použijte BlobSource a OracleSink v aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-154">Similarly, if you are copying from Azure Blob Storage tooOracle Database, you use BlobSource and OracleSink in hello copy activity.</span></span> <span data-ttu-id="2eed9-155">Vlastnosti aktivity kopírování, které jsou specifické tooOracle databáze, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="2eed9-155">For copy activity properties that are specific tooOracle database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="2eed9-156">Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.</span><span class="sxs-lookup"><span data-stu-id="2eed9-156">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span> 

<span data-ttu-id="2eed9-157">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="2eed9-157">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="2eed9-158">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-158">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="2eed9-159">Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo z databáze Oracle na místě, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-oracle-database) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="2eed9-159">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an on-premises Oracle database, see [JSON examples](#json-examples-for-copying-data-to-and-from-oracle-database) section of this article.</span></span>

<span data-ttu-id="2eed9-160">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine entit služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="2eed9-160">hello following sections provide details about JSON properties that are used toodefine Data Factory entities:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="2eed9-161">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="2eed9-161">Linked service properties</span></span>
<span data-ttu-id="2eed9-162">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooOracle propojené služby.</span><span class="sxs-lookup"><span data-stu-id="2eed9-162">hello following table provides description for JSON elements specific tooOracle linked service.</span></span>

| <span data-ttu-id="2eed9-163">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2eed9-163">Property</span></span> | <span data-ttu-id="2eed9-164">Popis</span><span class="sxs-lookup"><span data-stu-id="2eed9-164">Description</span></span> | <span data-ttu-id="2eed9-165">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="2eed9-165">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2eed9-166">type</span><span class="sxs-lookup"><span data-stu-id="2eed9-166">type</span></span> |<span data-ttu-id="2eed9-167">vlastnost typu Hello musí být nastavena na: **OnPremisesOracle**</span><span class="sxs-lookup"><span data-stu-id="2eed9-167">hello type property must be set to: **OnPremisesOracle**</span></span> |<span data-ttu-id="2eed9-168">Ano</span><span class="sxs-lookup"><span data-stu-id="2eed9-168">Yes</span></span> |
| <span data-ttu-id="2eed9-169">driverType</span><span class="sxs-lookup"><span data-stu-id="2eed9-169">driverType</span></span> | <span data-ttu-id="2eed9-170">Určete, jaká data toocopy toouse ovladače z / tooOracle databáze.</span><span class="sxs-lookup"><span data-stu-id="2eed9-170">Specify which driver toouse toocopy data from/tooOracle Database.</span></span> <span data-ttu-id="2eed9-171">Povolené hodnoty jsou **Microsoft** nebo **ODP** (výchozí).</span><span class="sxs-lookup"><span data-stu-id="2eed9-171">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="2eed9-172">V tématu [podporované verze a instalace](#supported-versions-and-installation) části na podrobnosti o ovladači.</span><span class="sxs-lookup"><span data-stu-id="2eed9-172">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="2eed9-173">Ne</span><span class="sxs-lookup"><span data-stu-id="2eed9-173">No</span></span> |
| <span data-ttu-id="2eed9-174">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-174">connectionString</span></span> | <span data-ttu-id="2eed9-175">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello databáze Oracle instance.</span><span class="sxs-lookup"><span data-stu-id="2eed9-175">Specify information needed tooconnect toohello Oracle Database instance for hello connectionString property.</span></span> | <span data-ttu-id="2eed9-176">Ano</span><span class="sxs-lookup"><span data-stu-id="2eed9-176">Yes</span></span> |
| <span data-ttu-id="2eed9-177">gatewayName</span><span class="sxs-lookup"><span data-stu-id="2eed9-177">gatewayName</span></span> | <span data-ttu-id="2eed9-178">Název hello brány, který je použité tooconnect toohello místního serveru Oracle</span><span class="sxs-lookup"><span data-stu-id="2eed9-178">Name of hello gateway that that is used tooconnect toohello on-premises Oracle server</span></span> |<span data-ttu-id="2eed9-179">Ano</span><span class="sxs-lookup"><span data-stu-id="2eed9-179">Yes</span></span> |

<span data-ttu-id="2eed9-180">**Příklad: pomocí ovladače Microsoft:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-180">**Example: using Microsoft driver:**</span></span>
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="2eed9-181">**Příklad: pomocí ODP ovladače**</span><span class="sxs-lookup"><span data-stu-id="2eed9-181">**Example: using ODP driver**</span></span>

<span data-ttu-id="2eed9-182">Odkazovat příliš[tento web](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) pro hello povolené formáty.</span><span class="sxs-lookup"><span data-stu-id="2eed9-182">Refer too[this site](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) for hello allowed formats.</span></span>

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="2eed9-183">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="2eed9-183">Dataset properties</span></span>
<span data-ttu-id="2eed9-184">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="2eed9-184">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="2eed9-185">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Oracle, objektů blob v Azure, Azure table atd.).</span><span class="sxs-lookup"><span data-stu-id="2eed9-185">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Oracle, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="2eed9-186">část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-186">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="2eed9-187">Hello rámci typeProperties části pro datovou sadu hello typu OracleTable má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="2eed9-187">hello typeProperties section for hello dataset of type OracleTable has hello following properties:</span></span>

| <span data-ttu-id="2eed9-188">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2eed9-188">Property</span></span> | <span data-ttu-id="2eed9-189">Popis</span><span class="sxs-lookup"><span data-stu-id="2eed9-189">Description</span></span> | <span data-ttu-id="2eed9-190">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="2eed9-190">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2eed9-191">tableName</span><span class="sxs-lookup"><span data-stu-id="2eed9-191">tableName</span></span> |<span data-ttu-id="2eed9-192">Název tabulky hello v hello databáze Oracle, který hello propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="2eed9-192">Name of hello table in hello Oracle Database that hello linked service refers to.</span></span> |<span data-ttu-id="2eed9-193">Ne (Pokud **oracleReaderQuery** z **OracleSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="2eed9-193">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="2eed9-194">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="2eed9-194">Copy activity properties</span></span>
<span data-ttu-id="2eed9-195">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="2eed9-195">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="2eed9-196">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="2eed9-196">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="2eed9-197">Hello aktivity kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="2eed9-197">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="2eed9-198">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="2eed9-198">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="2eed9-199">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="2eed9-199">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

### <a name="oraclesource"></a><span data-ttu-id="2eed9-200">OracleSource</span><span class="sxs-lookup"><span data-stu-id="2eed9-200">OracleSource</span></span>
<span data-ttu-id="2eed9-201">Při aktivitě kopírování, pokud je zdroj hello typu **OracleSource** hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="2eed9-201">In Copy activity, when hello source is of type **OracleSource** hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="2eed9-202">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2eed9-202">Property</span></span> | <span data-ttu-id="2eed9-203">Popis</span><span class="sxs-lookup"><span data-stu-id="2eed9-203">Description</span></span> | <span data-ttu-id="2eed9-204">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="2eed9-204">Allowed values</span></span> | <span data-ttu-id="2eed9-205">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="2eed9-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2eed9-206">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="2eed9-206">oracleReaderQuery</span></span> |<span data-ttu-id="2eed9-207">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="2eed9-207">Use hello custom query tooread data.</span></span> |<span data-ttu-id="2eed9-208">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="2eed9-208">SQL query string.</span></span> <span data-ttu-id="2eed9-209">Příklad: vybrat * z MyTable</span><span class="sxs-lookup"><span data-stu-id="2eed9-209">For example: select * from MyTable</span></span> <br/><br/><span data-ttu-id="2eed9-210">Pokud není zadaný, hello příkaz jazyka SQL, která se provedla: vybrat * z MyTable</span><span class="sxs-lookup"><span data-stu-id="2eed9-210">If not specified, hello SQL statement that is executed: select * from MyTable</span></span> |<span data-ttu-id="2eed9-211">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="2eed9-211">No (if **tableName** of **dataset** is specified)</span></span> |

### <a name="oraclesink"></a><span data-ttu-id="2eed9-212">OracleSink</span><span class="sxs-lookup"><span data-stu-id="2eed9-212">OracleSink</span></span>
<span data-ttu-id="2eed9-213">**OracleSink** podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="2eed9-213">**OracleSink** supports hello following properties:</span></span>

| <span data-ttu-id="2eed9-214">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2eed9-214">Property</span></span> | <span data-ttu-id="2eed9-215">Popis</span><span class="sxs-lookup"><span data-stu-id="2eed9-215">Description</span></span> | <span data-ttu-id="2eed9-216">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="2eed9-216">Allowed values</span></span> | <span data-ttu-id="2eed9-217">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="2eed9-217">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2eed9-218">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="2eed9-218">writeBatchTimeout</span></span> |<span data-ttu-id="2eed9-219">Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="2eed9-219">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="2eed9-220">Časový interval</span><span class="sxs-lookup"><span data-stu-id="2eed9-220">timespan</span></span><br/><br/> <span data-ttu-id="2eed9-221">Příklad: 00:30:00 (30 minut).</span><span class="sxs-lookup"><span data-stu-id="2eed9-221">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="2eed9-222">Ne</span><span class="sxs-lookup"><span data-stu-id="2eed9-222">No</span></span> |
| <span data-ttu-id="2eed9-223">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="2eed9-223">writeBatchSize</span></span> |<span data-ttu-id="2eed9-224">Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-224">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="2eed9-225">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="2eed9-225">Integer (number of rows)</span></span> |<span data-ttu-id="2eed9-226">Ne (výchozí: 100)</span><span class="sxs-lookup"><span data-stu-id="2eed9-226">No (default: 100)</span></span> |
| <span data-ttu-id="2eed9-227">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="2eed9-227">sqlWriterCleanupScript</span></span> |<span data-ttu-id="2eed9-228">Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="2eed9-228">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="2eed9-229">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="2eed9-229">A query statement.</span></span> |<span data-ttu-id="2eed9-230">Ne</span><span class="sxs-lookup"><span data-stu-id="2eed9-230">No</span></span> |
| <span data-ttu-id="2eed9-231">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="2eed9-231">sliceIdentifierColumnName</span></span> |<span data-ttu-id="2eed9-232">Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="2eed9-232">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="2eed9-233">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="2eed9-233">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="2eed9-234">Ne</span><span class="sxs-lookup"><span data-stu-id="2eed9-234">No</span></span> |

## <a name="json-examples-for-copying-data-tooand-from-oracle-database"></a><span data-ttu-id="2eed9-235">Příklady JSON pro kopírování data tooand z databáze Oracle</span><span class="sxs-lookup"><span data-stu-id="2eed9-235">JSON examples for copying data tooand from Oracle database</span></span>
<span data-ttu-id="2eed9-236">Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2eed9-236">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="2eed9-237">Ukazují jak toocopy data z / tooan Oracle databáze do/z Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2eed9-237">They show how toocopy data from/tooan Oracle database to/from Azure Blob Storage.</span></span> <span data-ttu-id="2eed9-238">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="2eed9-238">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

## <a name="example-copy-data-from-oracle-tooazure-blob"></a><span data-ttu-id="2eed9-239">Příklad: Kopírování dat z Oracle tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="2eed9-239">Example: Copy data from Oracle tooAzure Blob</span></span>

<span data-ttu-id="2eed9-240">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="2eed9-240">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="2eed9-241">Propojené služby typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2eed9-241">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="2eed9-242">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2eed9-242">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="2eed9-243">Vstup [datovou sadu](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2eed9-243">An input [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="2eed9-244">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2eed9-244">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="2eed9-245">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) jako zdroj a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) jako jímku.</span><span class="sxs-lookup"><span data-stu-id="2eed9-245">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) as source and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="2eed9-246">Ukázka Hello zkopíruje data z tabulky v objektu blob tooa databáze Oracle místní každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="2eed9-246">hello sample copies data from a table in an on-premises Oracle database tooa blob hourly.</span></span> <span data-ttu-id="2eed9-247">Další informace o různých vlastnosti používané v ukázce hello najdete v dokumentaci k v částech následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-247">For more information on various properties used in hello sample, see documentation in sections following hello samples.</span></span>

<span data-ttu-id="2eed9-248">**Oracle propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-248">**Oracle linked service:**</span></span>

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="2eed9-249">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-249">**Azure Blob storage linked service:**</span></span>

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

<span data-ttu-id="2eed9-250">**Oracle vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-250">**Oracle input dataset:**</span></span>

<span data-ttu-id="2eed9-251">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v Oracle a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="2eed9-251">hello sample assumes you have created a table “MyTable” in Oracle and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="2eed9-252">Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-252">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {     
            "externalData": {        
                "retryInterval": "00:01:00",    
                "retryTimeout": "00:10:00",       
                "maximumRetry": 3       
            }     
        }
    }
}
```

<span data-ttu-id="2eed9-253">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-253">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="2eed9-254">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="2eed9-254">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="2eed9-255">název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="2eed9-255">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="2eed9-256">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="2eed9-256">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="2eed9-257">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-257">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="2eed9-258">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="2eed9-258">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="2eed9-259">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**OracleSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="2eed9-259">In hello pipeline JSON definition, hello **source** type is set too**OracleSource** and **sink** type is set too**BlobSink**.</span></span>  <span data-ttu-id="2eed9-260">Zadaný dotaz SQL Hello **oracleReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="2eed9-260">hello SQL query specified with **oracleReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

## <a name="example-copy-data-from-azure-blob-toooracle"></a><span data-ttu-id="2eed9-261">Příklad: Kopírování dat z Azure Blob tooOracle</span><span class="sxs-lookup"><span data-stu-id="2eed9-261">Example: Copy data from Azure Blob tooOracle</span></span>
<span data-ttu-id="2eed9-262">Tento příklad ukazuje, jak toocopy data ze Azure Blob Storage tooan místní databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="2eed9-262">This sample shows how toocopy data from an Azure Blob Storage tooan on-premises Oracle database.</span></span> <span data-ttu-id="2eed9-263">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů hello uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="2eed9-263">However, data can be copied **directly** from any of hello sources stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="2eed9-264">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="2eed9-264">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="2eed9-265">Propojené služby typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2eed9-265">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="2eed9-266">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2eed9-266">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="2eed9-267">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2eed9-267">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="2eed9-268">Výstup [datovou sadu](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2eed9-268">An output [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="2eed9-269">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) jako zdroj [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) jako jímku.</span><span class="sxs-lookup"><span data-stu-id="2eed9-269">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) as source [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="2eed9-270">Ukázka Hello zkopíruje data z objektu blob tooa tabulky v databázi Oracle místní každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="2eed9-270">hello sample copies data from a blob tooa table in an on-premises Oracle database every hour.</span></span> <span data-ttu-id="2eed9-271">Další informace o různých vlastnosti používané v ukázce hello najdete v dokumentaci k v částech následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-271">For more information on various properties used in hello sample, see documentation in sections following hello samples.</span></span>

<span data-ttu-id="2eed9-272">**Oracle propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-272">**Oracle linked service:**</span></span>
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="2eed9-273">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-273">**Azure Blob storage linked service:**</span></span>
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

<span data-ttu-id="2eed9-274">**Azure vstupní datovou sadu objektu Blob**</span><span class="sxs-lookup"><span data-stu-id="2eed9-274">**Azure Blob input dataset**</span></span>

<span data-ttu-id="2eed9-275">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="2eed9-275">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="2eed9-276">název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="2eed9-276">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="2eed9-277">Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="2eed9-277">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="2eed9-278">"externí": "PRAVDA" nastavení informuje hello služba Data Factory, tato tabulka je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-278">“external”: “true” setting informs hello Data Factory service that this table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="2eed9-279">**Oracle výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-279">**Oracle output dataset:**</span></span>

<span data-ttu-id="2eed9-280">Ukázka Hello předpokládá, že jste vytvořili tabulku "MyTable" v Oracle.</span><span class="sxs-lookup"><span data-stu-id="2eed9-280">hello sample assumes you have created a table “MyTable” in Oracle.</span></span> <span data-ttu-id="2eed9-281">Vytvoření tabulky hello v Oracle s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-281">Create hello table in Oracle with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="2eed9-282">Přidávání řádků tabulky toohello každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="2eed9-282">New rows are added toohello table every hour.</span></span>

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

<span data-ttu-id="2eed9-283">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-283">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="2eed9-284">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="2eed9-284">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="2eed9-285">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a hello **podřízený** je typ nastaven příliš**OracleSink**.</span><span class="sxs-lookup"><span data-stu-id="2eed9-285">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and hello **sink** type is set too**OracleSink**.</span></span>  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```


## <a name="troubleshooting-tips"></a><span data-ttu-id="2eed9-286">Rady pro řešení potíží</span><span class="sxs-lookup"><span data-stu-id="2eed9-286">Troubleshooting tips</span></span>
### <a name="problem-1-net-framework-data-provider"></a><span data-ttu-id="2eed9-287">Problém 1: Zprostředkovatel dat .NET Framework</span><span class="sxs-lookup"><span data-stu-id="2eed9-287">Problem 1: .NET Framework Data Provider</span></span>

<span data-ttu-id="2eed9-288">Zobrazí následující hello **chybová zpráva**:</span><span class="sxs-lookup"><span data-stu-id="2eed9-288">You see hello following **error message**:</span></span>

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable toofind hello requested .Net Framework Data Provider. It may not be installed”.  

<span data-ttu-id="2eed9-289">**Možné příčiny:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-289">**Possible causes:**</span></span>

1. <span data-ttu-id="2eed9-290">Zprostředkovatel dat .NET Framework pro Oracle Hello nebyl nainstalován.</span><span class="sxs-lookup"><span data-stu-id="2eed9-290">hello .NET Framework Data Provider for Oracle was not installed.</span></span>
2. <span data-ttu-id="2eed9-291">Hello zprostředkovatel dat .NET Framework pro Oracle byla nainstalovaná too.NET Framework 2.0 a nebyla nalezena ve složkách hello rozhraní .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="2eed9-291">hello .NET Framework Data Provider for Oracle was installed too.NET Framework 2.0 and is not found in hello .NET Framework 4.0 folders.</span></span>

<span data-ttu-id="2eed9-292">**Řešení nebo alternativní řešení:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-292">**Resolution/Workaround:**</span></span>

1. <span data-ttu-id="2eed9-293">Pokud jste nenainstalovali hello poskytovatele .NET pro Oracle, [ji nainstalovat](http://www.oracle.com/technetwork/topics/dotnet/downloads/) a opakujte hello scénář.</span><span class="sxs-lookup"><span data-stu-id="2eed9-293">If you haven't installed hello .NET Provider for Oracle, [install it](http://www.oracle.com/technetwork/topics/dotnet/downloads/) and retry hello scenario.</span></span>
2. <span data-ttu-id="2eed9-294">Pokud se zobrazí chybová zpráva hello i po instalaci poskytovatele hello, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2eed9-294">If you get hello error message even after installing hello provider, do hello following steps:</span></span>
   1. <span data-ttu-id="2eed9-295">Konfigurace počítače rozhraní .NET 2.0 otevřít ze složky hello: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span><span class="sxs-lookup"><span data-stu-id="2eed9-295">Open machine config of .NET 2.0 from hello folder: <system disk>:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span></span>
   2. <span data-ttu-id="2eed9-296">Vyhledejte **poskytovatele dat Oracle pro .NET**, a mělo by být možné toofind položku, jak je znázorněno v následující ukázka v části hello **system.data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Poskytovatele dat oracle pro .NET</span><span class="sxs-lookup"><span data-stu-id="2eed9-296">Search for **Oracle Data Provider for .NET**, and you should be able toofind an entry as shown in hello following sample under **system.data** -> **DbProviderFactories**: “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET</span></span>" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" /><span data-ttu-id="2eed9-297">”</span><span class="sxs-lookup"><span data-stu-id="2eed9-297">”</span></span>
3. <span data-ttu-id="2eed9-298">Kopírování souboru machine.config. Tato položka toohello v následující složce v4.0 hello: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config a too4.xxx.x.x verze změnu hello.</span><span class="sxs-lookup"><span data-stu-id="2eed9-298">Copy this entry toohello machine.config file in hello following v4.0 folder: <system disk>:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, and change hello version too4.xxx.x.x.</span></span>
4. <span data-ttu-id="2eed9-299">Instalace "< cesta instalace ODP.NET > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" do hello globální mezipaměti sestavení (GAC) spuštěním `gacutil /i [provider path]`. ## Tipy pro odstraňování potíží</span><span class="sxs-lookup"><span data-stu-id="2eed9-299">Install “<ODP.NET Installed Path>\11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll” into hello global assembly cache (GAC) by running `gacutil /i [provider path]`.## Troubleshooting tips</span></span>

### <a name="problem-2-datetime-formatting"></a><span data-ttu-id="2eed9-300">Problém 2: formátování data a času</span><span class="sxs-lookup"><span data-stu-id="2eed9-300">Problem 2: datetime formatting</span></span>

<span data-ttu-id="2eed9-301">Zobrazí následující hello **chybová zpráva**:</span><span class="sxs-lookup"><span data-stu-id="2eed9-301">You see hello following **error message**:</span></span>

    Message=Operation failed in Oracle Database with hello following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

<span data-ttu-id="2eed9-302">**Řešení nebo alternativní řešení:**</span><span class="sxs-lookup"><span data-stu-id="2eed9-302">**Resolution/Workaround:**</span></span>

<span data-ttu-id="2eed9-303">Řetězec dotazu hello tooadjust může být nutné v aktivitě kopírování založené na tom, jak jsou nakonfigurované data do databáze Oracle, jak je znázorněno v následující hello ukázkové (pomocí funkce to_date hello):</span><span class="sxs-lookup"><span data-stu-id="2eed9-303">You may need tooadjust hello query string in your copy activity based on how dates are configured in your Oracle database, as shown in hello following sample (using hello to_date function):</span></span>

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a><span data-ttu-id="2eed9-304">Mapování typu pro Oracle</span><span class="sxs-lookup"><span data-stu-id="2eed9-304">Type mapping for Oracle</span></span>
<span data-ttu-id="2eed9-305">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:</span><span class="sxs-lookup"><span data-stu-id="2eed9-305">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="2eed9-306">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="2eed9-306">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="2eed9-307">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="2eed9-307">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="2eed9-308">Při přesouvání dat z databáze Oracle, se používají hello následující mapování z typu too.NET typ dat Oracle a naopak.</span><span class="sxs-lookup"><span data-stu-id="2eed9-308">When moving data from Oracle, hello following mappings are used from Oracle data type too.NET type and vice versa.</span></span>

| <span data-ttu-id="2eed9-309">Oracle datový typ</span><span class="sxs-lookup"><span data-stu-id="2eed9-309">Oracle data type</span></span> | <span data-ttu-id="2eed9-310">Datový typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="2eed9-310">.NET Framework data type</span></span> |
| --- | --- |
| <span data-ttu-id="2eed9-311">BFILE</span><span class="sxs-lookup"><span data-stu-id="2eed9-311">BFILE</span></span> |<span data-ttu-id="2eed9-312">Byte]</span><span class="sxs-lookup"><span data-stu-id="2eed9-312">Byte[]</span></span> |
| <span data-ttu-id="2eed9-313">OBJEKT BLOB</span><span class="sxs-lookup"><span data-stu-id="2eed9-313">BLOB</span></span> |<span data-ttu-id="2eed9-314">Byte]</span><span class="sxs-lookup"><span data-stu-id="2eed9-314">Byte[]</span></span> |
| <span data-ttu-id="2eed9-315">CHAR –</span><span class="sxs-lookup"><span data-stu-id="2eed9-315">CHAR</span></span> |<span data-ttu-id="2eed9-316">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-316">String</span></span> |
| <span data-ttu-id="2eed9-317">DATOVÝ TYP CLOB</span><span class="sxs-lookup"><span data-stu-id="2eed9-317">CLOB</span></span> |<span data-ttu-id="2eed9-318">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-318">String</span></span> |
| <span data-ttu-id="2eed9-319">DATUM</span><span class="sxs-lookup"><span data-stu-id="2eed9-319">DATE</span></span> |<span data-ttu-id="2eed9-320">Data a času</span><span class="sxs-lookup"><span data-stu-id="2eed9-320">DateTime</span></span> |
| <span data-ttu-id="2eed9-321">PLOVOUCÍ DESETINNÁ ČÁRKA</span><span class="sxs-lookup"><span data-stu-id="2eed9-321">FLOAT</span></span> |<span data-ttu-id="2eed9-322">Decimal, řetězec (Pokud přesnost > 28)</span><span class="sxs-lookup"><span data-stu-id="2eed9-322">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="2eed9-323">CELÉ ČÍSLO</span><span class="sxs-lookup"><span data-stu-id="2eed9-323">INTEGER</span></span> |<span data-ttu-id="2eed9-324">Decimal, řetězec (Pokud přesnost > 28)</span><span class="sxs-lookup"><span data-stu-id="2eed9-324">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="2eed9-325">INTERVAL tooMONTH roku</span><span class="sxs-lookup"><span data-stu-id="2eed9-325">INTERVAL YEAR tooMONTH</span></span> |<span data-ttu-id="2eed9-326">Int32</span><span class="sxs-lookup"><span data-stu-id="2eed9-326">Int32</span></span> |
| <span data-ttu-id="2eed9-327">INTERVAL den tooSECOND</span><span class="sxs-lookup"><span data-stu-id="2eed9-327">INTERVAL DAY tooSECOND</span></span> |<span data-ttu-id="2eed9-328">Časový interval</span><span class="sxs-lookup"><span data-stu-id="2eed9-328">TimeSpan</span></span> |
| <span data-ttu-id="2eed9-329">DLOUHÁ</span><span class="sxs-lookup"><span data-stu-id="2eed9-329">LONG</span></span> |<span data-ttu-id="2eed9-330">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-330">String</span></span> |
| <span data-ttu-id="2eed9-331">DLOUHO NEZPRACOVANÁ</span><span class="sxs-lookup"><span data-stu-id="2eed9-331">LONG RAW</span></span> |<span data-ttu-id="2eed9-332">Byte]</span><span class="sxs-lookup"><span data-stu-id="2eed9-332">Byte[]</span></span> |
| <span data-ttu-id="2eed9-333">NCHAR</span><span class="sxs-lookup"><span data-stu-id="2eed9-333">NCHAR</span></span> |<span data-ttu-id="2eed9-334">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-334">String</span></span> |
| <span data-ttu-id="2eed9-335">NCLOB</span><span class="sxs-lookup"><span data-stu-id="2eed9-335">NCLOB</span></span> |<span data-ttu-id="2eed9-336">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-336">String</span></span> |
| <span data-ttu-id="2eed9-337">ČÍSLO</span><span class="sxs-lookup"><span data-stu-id="2eed9-337">NUMBER</span></span> |<span data-ttu-id="2eed9-338">Decimal, řetězec (Pokud přesnost > 28)</span><span class="sxs-lookup"><span data-stu-id="2eed9-338">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="2eed9-339">NVARCHAR2</span><span class="sxs-lookup"><span data-stu-id="2eed9-339">NVARCHAR2</span></span> |<span data-ttu-id="2eed9-340">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-340">String</span></span> |
| <span data-ttu-id="2eed9-341">NEZPRACOVANÁ</span><span class="sxs-lookup"><span data-stu-id="2eed9-341">RAW</span></span> |<span data-ttu-id="2eed9-342">Byte]</span><span class="sxs-lookup"><span data-stu-id="2eed9-342">Byte[]</span></span> |
| <span data-ttu-id="2eed9-343">ID ŘÁDKU</span><span class="sxs-lookup"><span data-stu-id="2eed9-343">ROWID</span></span> |<span data-ttu-id="2eed9-344">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-344">String</span></span> |
| <span data-ttu-id="2eed9-345">ČASOVÉ RAZÍTKO</span><span class="sxs-lookup"><span data-stu-id="2eed9-345">TIMESTAMP</span></span> |<span data-ttu-id="2eed9-346">Data a času</span><span class="sxs-lookup"><span data-stu-id="2eed9-346">DateTime</span></span> |
| <span data-ttu-id="2eed9-347">ČASOVÉ RAZÍTKO S MÍSTNÍM ČASOVÉM PÁSMU</span><span class="sxs-lookup"><span data-stu-id="2eed9-347">TIMESTAMP WITH LOCAL TIME ZONE</span></span> |<span data-ttu-id="2eed9-348">Data a času</span><span class="sxs-lookup"><span data-stu-id="2eed9-348">DateTime</span></span> |
| <span data-ttu-id="2eed9-349">ČASOVÉ RAZÍTKO S ČASOVÝM PÁSMEM</span><span class="sxs-lookup"><span data-stu-id="2eed9-349">TIMESTAMP WITH TIME ZONE</span></span> |<span data-ttu-id="2eed9-350">Data a času</span><span class="sxs-lookup"><span data-stu-id="2eed9-350">DateTime</span></span> |
| <span data-ttu-id="2eed9-351">CELÉ ČÍSLO BEZ ZNAMÉNKA</span><span class="sxs-lookup"><span data-stu-id="2eed9-351">UNSIGNED INTEGER</span></span> |<span data-ttu-id="2eed9-352">Číslo</span><span class="sxs-lookup"><span data-stu-id="2eed9-352">Number</span></span> |
| <span data-ttu-id="2eed9-353">VARCHAR2</span><span class="sxs-lookup"><span data-stu-id="2eed9-353">VARCHAR2</span></span> |<span data-ttu-id="2eed9-354">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-354">String</span></span> |
| <span data-ttu-id="2eed9-355">XML</span><span class="sxs-lookup"><span data-stu-id="2eed9-355">XML</span></span> |<span data-ttu-id="2eed9-356">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2eed9-356">String</span></span> |

> [!NOTE]
> <span data-ttu-id="2eed9-357">Datový typ **INTERVAL roku tooMONTH** a **INTERVAL den tooSECOND** nejsou podporována při použití ovladače Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2eed9-357">Data type **INTERVAL YEAR tooMONTH** and **INTERVAL DAY tooSECOND** are not supported when using Microsoft driver.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="2eed9-358">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="2eed9-358">Map source toosink columns</span></span>
<span data-ttu-id="2eed9-359">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="2eed9-359">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="2eed9-360">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="2eed9-360">Repeatable read from relational sources</span></span>
<span data-ttu-id="2eed9-361">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="2eed9-361">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="2eed9-362">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="2eed9-362">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="2eed9-363">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="2eed9-363">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="2eed9-364">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="2eed9-364">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="2eed9-365">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="2eed9-365">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="2eed9-366">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="2eed9-366">Performance and Tuning</span></span>
<span data-ttu-id="2eed9-367">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="2eed9-367">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
