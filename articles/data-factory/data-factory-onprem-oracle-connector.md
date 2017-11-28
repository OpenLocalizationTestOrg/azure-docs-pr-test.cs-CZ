---
title: "Kopírování dat do nebo z databáze Oracle pomocí služby Data Factory | Microsoft Docs"
description: "Zjistěte, jak ke zkopírování dat z Oracle databázi, která je v místním prostředí pomocí Azure Data Factory."
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
ms.openlocfilehash: bb6af719fe6f1a30c5933ce4342a4c0c072f3ff4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a><span data-ttu-id="32227-103">Kopírování dat z místní Oracle pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="32227-103">Copy data to/from on-premises Oracle using Azure Data Factory</span></span>
<span data-ttu-id="32227-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z databáze Oracle na místě.</span><span class="sxs-lookup"><span data-stu-id="32227-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from an on-premises Oracle database.</span></span> <span data-ttu-id="32227-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="32227-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="32227-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="32227-106">Supported scenarios</span></span>
<span data-ttu-id="32227-107">Může kopírovat data **z databáze Oracle** ukládá do následující data:</span><span class="sxs-lookup"><span data-stu-id="32227-107">You can copy data **from an Oracle database** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="32227-108">Může kopírovat data z následujících datových úložišť **k databázi Oracle**:</span><span class="sxs-lookup"><span data-stu-id="32227-108">You can copy data from the following data stores **to an Oracle database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a><span data-ttu-id="32227-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="32227-109">Prerequisites</span></span>
<span data-ttu-id="32227-110">Objekt pro vytváření dat podporuje připojení k místním zdrojům Oracle pomocí Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="32227-110">Data Factory supports connecting to on-premises Oracle sources using the Data Management Gateway.</span></span> <span data-ttu-id="32227-111">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) článku se dozvíte o Brána pro správu dat a [přesun dat z lokálního prostředí do cloudu](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny o nastavení brány datovém kanálu pro přesun dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="32227-111">See [Data Management Gateway](data-factory-data-management-gateway.md) article to learn about Data Management Gateway and [Move data from on-premises to cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway a data pipeline to move data.</span></span>

<span data-ttu-id="32227-112">Vyžaduje se brána, i když Oracle je hostovaná ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="32227-112">Gateway is required even if the Oracle is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="32227-113">Bránu můžete nainstalovat na stejný virtuální počítač IaaS jako úložiště dat nebo na jiný virtuální počítač, dokud brána se může připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="32227-113">You can install the gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="32227-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="32227-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="32227-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="32227-115">Supported versions and installation</span></span>
<span data-ttu-id="32227-116">Tento konektor Oracle podporují dvě verze ovladače:</span><span class="sxs-lookup"><span data-stu-id="32227-116">This Oracle connector support two versions of drivers:</span></span>

- <span data-ttu-id="32227-117">**Ovladač Microsoft pro Oracle (doporučeno)**: od Brána pro správu dat. verze 2.7 ovladač Microsoft pro Oracle se nainstaluje automaticky společně bránu, takže nemusíte dále zpracovávat ovladačů, aby bylo možné navázat připojení do databáze Oracle a můžete také dochází k lepší výkon kopírování pomocí tento ovladač.</span><span class="sxs-lookup"><span data-stu-id="32227-117">**Microsoft driver for Oracle (recommended)**: starting from Data Management Gateway version 2.7, a Microsoft driver for Oracle is automatically installed along with the gateway, so you don't need to additionally handle the driver in order to establish connectivity to Oracle, and you can also experience better copy performance using this driver.</span></span> <span data-ttu-id="32227-118">Následující verze systému Oracle jsou podporovány databází:</span><span class="sxs-lookup"><span data-stu-id="32227-118">Below versions of Oracle databases are supported:</span></span>
    - <span data-ttu-id="32227-119">R1 Oracle 12c (12.1)</span><span class="sxs-lookup"><span data-stu-id="32227-119">Oracle 12c R1 (12.1)</span></span>
    - <span data-ttu-id="32227-120">R1 Oracle 11g nebo R2 (11.1, 11.2)</span><span class="sxs-lookup"><span data-stu-id="32227-120">Oracle 11g R1, R2 (11.1, 11.2)</span></span>
    - <span data-ttu-id="32227-121">R1 Oracle 10g, R2 (10,1, 10.2)</span><span class="sxs-lookup"><span data-stu-id="32227-121">Oracle 10g R1, R2 (10.1, 10.2)</span></span>
    - <span data-ttu-id="32227-122">Oracle 9i R1, R2 (9.0.1, 9.2)</span><span class="sxs-lookup"><span data-stu-id="32227-122">Oracle 9i R1, R2 (9.0.1, 9.2)</span></span>
    - <span data-ttu-id="32227-123">Oracle 8i R3 (8.1.7)</span><span class="sxs-lookup"><span data-stu-id="32227-123">Oracle 8i R3 (8.1.7)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32227-124">Ovladač Microsoft pro Oracle aktuálně podporuje jenom kopírování dat z Oracle, ale není zápis do databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="32227-124">Currently Microsoft driver for Oracle only supports copying data from Oracle but not writing to Oracle.</span></span> <span data-ttu-id="32227-125">A Všimněte si, že testovací připojení možnosti na kartě diagnostiku brány správy dat nepodporuje tento ovladač.</span><span class="sxs-lookup"><span data-stu-id="32227-125">And note the test connection capability in Data Management Gateway Diagnostics tab does not support this driver.</span></span> <span data-ttu-id="32227-126">Alternativně můžete použít Průvodce kopírováním k ověření připojení.</span><span class="sxs-lookup"><span data-stu-id="32227-126">Alternatively, you can use the copy wizard to validate the connectivity.</span></span>
>

- <span data-ttu-id="32227-127">**Zprostředkovatel dat Oracle pro .NET:** můžete také použít poskytovatele dat Oracle ke zkopírování dat z/do databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="32227-127">**Oracle Data Provider for .NET:** you can also choose to use Oracle Data Provider to copy data from/to Oracle.</span></span> <span data-ttu-id="32227-128">Tato součást je součástí [Oracle Data přístup součásti pro Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span><span class="sxs-lookup"><span data-stu-id="32227-128">This component is included in [Oracle Data Access Components for Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span></span> <span data-ttu-id="32227-129">Nainstalujte příslušnou verzi (32 nebo 64bitový) na počítači, kde se brána nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="32227-129">Install the appropriate version (32/64 bit) on the machine where the gateway is installed.</span></span> <span data-ttu-id="32227-130">[Zprostředkovatel dat Oracle .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) přístup k databázi Oracle Database 10 g verze 2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="32227-130">[Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) can access to Oracle Database 10g Release 2 or later.</span></span>

    <span data-ttu-id="32227-131">Pokud si zvolíte možnost "XCopy instalace", postupujte podle kroků v readme.htm.</span><span class="sxs-lookup"><span data-stu-id="32227-131">If you choose “XCopy Installation”, follow steps in the readme.htm.</span></span> <span data-ttu-id="32227-132">Doporučujeme že vybrat instalační program pomocí uživatelského rozhraní (bez XCopy jeden).</span><span class="sxs-lookup"><span data-stu-id="32227-132">We recommend you choose the installer with UI (non-XCopy one).</span></span>

    <span data-ttu-id="32227-133">Po instalaci poskytovatele, **restartujte** hostitelskou službu Brána pro správu dat v počítači pomocí služby aplet (nebo) Správce konfigurace brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="32227-133">After installing the provider, **restart** the Data Management Gateway host service on your machine using Services applet (or) Data Management Gateway Configuration Manager.</span></span>  

<span data-ttu-id="32227-134">Pokud použijete Průvodce kopírováním vytvářet kanál kopírování, typ ovladače bude automaticky určit.</span><span class="sxs-lookup"><span data-stu-id="32227-134">If you use copy wizard to author the copy pipeline, the driver type will be auto-determined.</span></span> <span data-ttu-id="32227-135">Ovladač Microsoft použije ve výchozím nastavení, pokud vaše verze brány je nižší než 2.7 nebo zvolte Oracle jako jímku.</span><span class="sxs-lookup"><span data-stu-id="32227-135">Microsoft driver will be used by default, unless your gateway version is lower than 2.7 or you choose Oracle as sink.</span></span>

## <a name="getting-started"></a><span data-ttu-id="32227-136">Začínáme</span><span class="sxs-lookup"><span data-stu-id="32227-136">Getting started</span></span>
<span data-ttu-id="32227-137">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z databáze Oracle místně pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="32227-137">You can create a pipeline with a copy activity that moves data to/from an on-premises Oracle database by using different tools/APIs.</span></span>

<span data-ttu-id="32227-138">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="32227-138">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="32227-139">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="32227-139">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="32227-140">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="32227-140">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="32227-141">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="32227-141">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="32227-142">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="32227-142">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="32227-143">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="32227-143">Create a **data factory**.</span></span> <span data-ttu-id="32227-144">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="32227-144">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="32227-145">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="32227-145">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="32227-146">Pokud jsou kopírování dat z databáze Oralce do Azure blob storage, například vytvoříte dvě propojené služby pro vytváření dat. propojte databáze Oracle a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="32227-146">For example, if you are copying data from an Oralce database to an Azure blob storage, you create two linked services to link your Oracle database and Azure storage account to your data factory.</span></span> <span data-ttu-id="32227-147">Vlastnosti propojené služby, které jsou specifické pro Oracle, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="32227-147">For linked service properties that are specific to Oracle, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="32227-148">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="32227-148">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="32227-149">V příkladu uvedených v posledním kroku vytvoříte datové sady a určete tabulky v databázi Oracle, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="32227-149">In the example mentioned in the last step, you create a dataset to specify the table in your Oracle database that contains the input data.</span></span> <span data-ttu-id="32227-150">A vytvořte jinou datovou sadu, která zadejte kontejner objektů blob a složky, která obsahuje data zkopírovat z databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="32227-150">And, you create another dataset to specify the blob container and the folder that holds the data copied from the Oracle database.</span></span> <span data-ttu-id="32227-151">Vlastnosti datové sady, které jsou specifické pro Oracle, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="32227-151">For dataset properties that are specific to Oracle, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="32227-152">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="32227-152">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="32227-153">V příkladu již bylo zmíněno dříve použijete OracleSource jako zdroj a BlobSink jako jímku pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="32227-153">In the example mentioned earlier, you use OracleSource as a source and BlobSink as a sink for the copy activity.</span></span> <span data-ttu-id="32227-154">Podobně pokud kopírujete z Azure Blob Storage do databáze Oracle, můžete použít BlobSource a OracleSink v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="32227-154">Similarly, if you are copying from Azure Blob Storage to Oracle Database, you use BlobSource and OracleSink in the copy activity.</span></span> <span data-ttu-id="32227-155">Kopírovat vlastnosti aktivity, které jsou specifické pro databáze Oracle, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="32227-155">For copy activity properties that are specific to Oracle database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="32227-156">Podrobnosti o tom, jak používat úložiště dat jako zdroj nebo jímka klikněte na odkaz v předchozí části pro data store.</span><span class="sxs-lookup"><span data-stu-id="32227-156">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span> 

<span data-ttu-id="32227-157">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="32227-157">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="32227-158">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="32227-158">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="32227-159">Ukázky s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat do nebo z databáze Oracle na místě, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-oracle-database) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="32227-159">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an on-premises Oracle database, see [JSON examples](#json-examples-for-copying-data-to-and-from-oracle-database) section of this article.</span></span>

<span data-ttu-id="32227-160">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení entit služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="32227-160">The following sections provide details about JSON properties that are used to define Data Factory entities:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="32227-161">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="32227-161">Linked service properties</span></span>
<span data-ttu-id="32227-162">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro Oracle propojené služby.</span><span class="sxs-lookup"><span data-stu-id="32227-162">The following table provides description for JSON elements specific to Oracle linked service.</span></span>

| <span data-ttu-id="32227-163">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="32227-163">Property</span></span> | <span data-ttu-id="32227-164">Popis</span><span class="sxs-lookup"><span data-stu-id="32227-164">Description</span></span> | <span data-ttu-id="32227-165">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="32227-165">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="32227-166">type</span><span class="sxs-lookup"><span data-stu-id="32227-166">type</span></span> |<span data-ttu-id="32227-167">Vlastnost typu musí být nastavena na: **OnPremisesOracle**</span><span class="sxs-lookup"><span data-stu-id="32227-167">The type property must be set to: **OnPremisesOracle**</span></span> |<span data-ttu-id="32227-168">Ano</span><span class="sxs-lookup"><span data-stu-id="32227-168">Yes</span></span> |
| <span data-ttu-id="32227-169">driverType</span><span class="sxs-lookup"><span data-stu-id="32227-169">driverType</span></span> | <span data-ttu-id="32227-170">Určete, který ovladač použít ke zkopírování dat z/do databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="32227-170">Specify which driver to use to copy data from/to Oracle Database.</span></span> <span data-ttu-id="32227-171">Povolené hodnoty jsou **Microsoft** nebo **ODP** (výchozí).</span><span class="sxs-lookup"><span data-stu-id="32227-171">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="32227-172">V tématu [podporované verze a instalace](#supported-versions-and-installation) části na podrobnosti o ovladači.</span><span class="sxs-lookup"><span data-stu-id="32227-172">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="32227-173">Ne</span><span class="sxs-lookup"><span data-stu-id="32227-173">No</span></span> |
| <span data-ttu-id="32227-174">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-174">connectionString</span></span> | <span data-ttu-id="32227-175">Zadejte informace potřebné pro připojení k instanci databáze Oracle pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="32227-175">Specify information needed to connect to the Oracle Database instance for the connectionString property.</span></span> | <span data-ttu-id="32227-176">Ano</span><span class="sxs-lookup"><span data-stu-id="32227-176">Yes</span></span> |
| <span data-ttu-id="32227-177">gatewayName</span><span class="sxs-lookup"><span data-stu-id="32227-177">gatewayName</span></span> | <span data-ttu-id="32227-178">Název brány, aby se používá pro připojení k místnímu serveru Oracle</span><span class="sxs-lookup"><span data-stu-id="32227-178">Name of the gateway that that is used to connect to the on-premises Oracle server</span></span> |<span data-ttu-id="32227-179">Ano</span><span class="sxs-lookup"><span data-stu-id="32227-179">Yes</span></span> |

<span data-ttu-id="32227-180">**Příklad: pomocí ovladače Microsoft:**</span><span class="sxs-lookup"><span data-stu-id="32227-180">**Example: using Microsoft driver:**</span></span>
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

<span data-ttu-id="32227-181">**Příklad: pomocí ODP ovladače**</span><span class="sxs-lookup"><span data-stu-id="32227-181">**Example: using ODP driver**</span></span>

<span data-ttu-id="32227-182">Odkazovat na [tento web](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) pro povolených formátech.</span><span class="sxs-lookup"><span data-stu-id="32227-182">Refer to [this site](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) for the allowed formats.</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="32227-183">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="32227-183">Dataset properties</span></span>
<span data-ttu-id="32227-184">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="32227-184">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="32227-185">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Oracle, objektů blob v Azure, Azure table atd.).</span><span class="sxs-lookup"><span data-stu-id="32227-185">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Oracle, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="32227-186">V rámci typeProperties části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="32227-186">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="32227-187">V rámci typeProperties části datové sady typu OracleTable má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="32227-187">The typeProperties section for the dataset of type OracleTable has the following properties:</span></span>

| <span data-ttu-id="32227-188">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="32227-188">Property</span></span> | <span data-ttu-id="32227-189">Popis</span><span class="sxs-lookup"><span data-stu-id="32227-189">Description</span></span> | <span data-ttu-id="32227-190">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="32227-190">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="32227-191">tableName</span><span class="sxs-lookup"><span data-stu-id="32227-191">tableName</span></span> |<span data-ttu-id="32227-192">Název tabulky v databázi Oracle, který propojená služba odkazuje na.</span><span class="sxs-lookup"><span data-stu-id="32227-192">Name of the table in the Oracle Database that the linked service refers to.</span></span> |<span data-ttu-id="32227-193">Ne (Pokud **oracleReaderQuery** z **OracleSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="32227-193">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="32227-194">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="32227-194">Copy activity properties</span></span>
<span data-ttu-id="32227-195">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="32227-195">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="32227-196">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="32227-196">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="32227-197">Aktivita kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="32227-197">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="32227-198">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="32227-198">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="32227-199">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="32227-199">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

### <a name="oraclesource"></a><span data-ttu-id="32227-200">OracleSource</span><span class="sxs-lookup"><span data-stu-id="32227-200">OracleSource</span></span>
<span data-ttu-id="32227-201">Při aktivitě kopírování, pokud je zdroj typu **OracleSource** následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="32227-201">In Copy activity, when the source is of type **OracleSource** the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="32227-202">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="32227-202">Property</span></span> | <span data-ttu-id="32227-203">Popis</span><span class="sxs-lookup"><span data-stu-id="32227-203">Description</span></span> | <span data-ttu-id="32227-204">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="32227-204">Allowed values</span></span> | <span data-ttu-id="32227-205">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="32227-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="32227-206">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="32227-206">oracleReaderQuery</span></span> |<span data-ttu-id="32227-207">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="32227-207">Use the custom query to read data.</span></span> |<span data-ttu-id="32227-208">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="32227-208">SQL query string.</span></span> <span data-ttu-id="32227-209">Příklad: vybrat * z MyTable</span><span class="sxs-lookup"><span data-stu-id="32227-209">For example: select * from MyTable</span></span> <br/><br/><span data-ttu-id="32227-210">Pokud není zadaný příkaz jazyka SQL, která se provedla: vybrat * z MyTable</span><span class="sxs-lookup"><span data-stu-id="32227-210">If not specified, the SQL statement that is executed: select * from MyTable</span></span> |<span data-ttu-id="32227-211">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="32227-211">No (if **tableName** of **dataset** is specified)</span></span> |

### <a name="oraclesink"></a><span data-ttu-id="32227-212">OracleSink</span><span class="sxs-lookup"><span data-stu-id="32227-212">OracleSink</span></span>
<span data-ttu-id="32227-213">**OracleSink** podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="32227-213">**OracleSink** supports the following properties:</span></span>

| <span data-ttu-id="32227-214">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="32227-214">Property</span></span> | <span data-ttu-id="32227-215">Popis</span><span class="sxs-lookup"><span data-stu-id="32227-215">Description</span></span> | <span data-ttu-id="32227-216">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="32227-216">Allowed values</span></span> | <span data-ttu-id="32227-217">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="32227-217">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="32227-218">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="32227-218">writeBatchTimeout</span></span> |<span data-ttu-id="32227-219">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.</span><span class="sxs-lookup"><span data-stu-id="32227-219">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="32227-220">Časový interval</span><span class="sxs-lookup"><span data-stu-id="32227-220">timespan</span></span><br/><br/> <span data-ttu-id="32227-221">Příklad: 00:30:00 (30 minut).</span><span class="sxs-lookup"><span data-stu-id="32227-221">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="32227-222">Ne</span><span class="sxs-lookup"><span data-stu-id="32227-222">No</span></span> |
| <span data-ttu-id="32227-223">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="32227-223">writeBatchSize</span></span> |<span data-ttu-id="32227-224">Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="32227-224">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="32227-225">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="32227-225">Integer (number of rows)</span></span> |<span data-ttu-id="32227-226">Ne (výchozí: 100)</span><span class="sxs-lookup"><span data-stu-id="32227-226">No (default: 100)</span></span> |
| <span data-ttu-id="32227-227">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="32227-227">sqlWriterCleanupScript</span></span> |<span data-ttu-id="32227-228">Zadejte dotaz pro aktivitu kopírování provést tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="32227-228">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="32227-229">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="32227-229">A query statement.</span></span> |<span data-ttu-id="32227-230">Ne</span><span class="sxs-lookup"><span data-stu-id="32227-230">No</span></span> |
| <span data-ttu-id="32227-231">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="32227-231">sliceIdentifierColumnName</span></span> |<span data-ttu-id="32227-232">Zadejte název sloupce pro aktivitu kopírování vyplníte identifikátor automaticky generovány řez, který se používá k vyčištění dat určitý řez při spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="32227-232">Specify column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="32227-233">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="32227-233">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="32227-234">Ne</span><span class="sxs-lookup"><span data-stu-id="32227-234">No</span></span> |

## <a name="json-examples-for-copying-data-to-and-from-oracle-database"></a><span data-ttu-id="32227-235">Příklady JSON pro kopírování dat do a z databáze Oracle</span><span class="sxs-lookup"><span data-stu-id="32227-235">JSON examples for copying data to and from Oracle database</span></span>
<span data-ttu-id="32227-236">Následující příklad uvádí ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="32227-236">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="32227-237">Se ukazují, jak ke zkopírování dat z/do databáze Oracle do/z Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="32227-237">They show how to copy data from/to an Oracle database to/from Azure Blob Storage.</span></span> <span data-ttu-id="32227-238">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="32227-238">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

## <a name="example-copy-data-from-oracle-to-azure-blob"></a><span data-ttu-id="32227-239">Příklad: Kopírování dat z databáze Oracle do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="32227-239">Example: Copy data from Oracle to Azure Blob</span></span>

<span data-ttu-id="32227-240">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="32227-240">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="32227-241">Propojené služby typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="32227-241">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="32227-242">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="32227-242">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="32227-243">Vstup [datovou sadu](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="32227-243">An input [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="32227-244">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="32227-244">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="32227-245">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) jako zdroj a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) jako jímku.</span><span class="sxs-lookup"><span data-stu-id="32227-245">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) as source and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="32227-246">Ukázka zkopíruje data z tabulky v databázi Oracle místně do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="32227-246">The sample copies data from a table in an on-premises Oracle database to a blob hourly.</span></span> <span data-ttu-id="32227-247">Další informace o různých vlastnosti používané ve vzorku naleznete v dokumentaci k v částech následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="32227-247">For more information on various properties used in the sample, see documentation in sections following the samples.</span></span>

<span data-ttu-id="32227-248">**Oracle propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="32227-248">**Oracle linked service:**</span></span>

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

<span data-ttu-id="32227-249">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="32227-249">**Azure Blob storage linked service:**</span></span>

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

<span data-ttu-id="32227-250">**Oracle vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="32227-250">**Oracle input dataset:**</span></span>

<span data-ttu-id="32227-251">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v Oracle a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="32227-251">The sample assumes you have created a table “MyTable” in Oracle and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="32227-252">Nastavení "externí": "PRAVDA" informuje služba Data Factory, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="32227-252">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="32227-253">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="32227-253">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="32227-254">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="32227-254">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="32227-255">Název složky a cesta k souboru pro tento objekt blob se vyhodnocují dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="32227-255">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="32227-256">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="32227-256">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="32227-257">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="32227-257">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="32227-258">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="32227-258">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="32227-259">V definici JSON kanálu **zdroj** je typ nastaven na **OracleSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="32227-259">In the pipeline JSON definition, the **source** type is set to **OracleSource** and **sink** type is set to **BlobSink**.</span></span>  <span data-ttu-id="32227-260">Příkaz jazyka SQL zadaným **oracleReaderQuery** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="32227-260">The SQL query specified with **oracleReaderQuery** property selects the data in the past hour to copy.</span></span>

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

## <a name="example-copy-data-from-azure-blob-to-oracle"></a><span data-ttu-id="32227-261">Příklad: Kopírování dat z objektu Blob Azure do Oracle</span><span class="sxs-lookup"><span data-stu-id="32227-261">Example: Copy data from Azure Blob to Oracle</span></span>
<span data-ttu-id="32227-262">Tento příklad ukazuje, jak ke zkopírování dat z úložiště objektů Blob Azure do místní databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="32227-262">This sample shows how to copy data from an Azure Blob Storage to an on-premises Oracle database.</span></span> <span data-ttu-id="32227-263">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="32227-263">However, data can be copied **directly** from any of the sources stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="32227-264">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="32227-264">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="32227-265">Propojené služby typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="32227-265">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="32227-266">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="32227-266">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="32227-267">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="32227-267">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="32227-268">Výstup [datovou sadu](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="32227-268">An output [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="32227-269">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) jako zdroj [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) jako jímku.</span><span class="sxs-lookup"><span data-stu-id="32227-269">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) as source [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="32227-270">Ukázka kopíruje data z objektu blob do tabulky v databázi Oracle místní každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="32227-270">The sample copies data from a blob to a table in an on-premises Oracle database every hour.</span></span> <span data-ttu-id="32227-271">Další informace o různých vlastnosti používané ve vzorku naleznete v dokumentaci k v částech následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="32227-271">For more information on various properties used in the sample, see documentation in sections following the samples.</span></span>

<span data-ttu-id="32227-272">**Oracle propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="32227-272">**Oracle linked service:**</span></span>
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

<span data-ttu-id="32227-273">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="32227-273">**Azure Blob storage linked service:**</span></span>
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

<span data-ttu-id="32227-274">**Azure vstupní datovou sadu objektu Blob**</span><span class="sxs-lookup"><span data-stu-id="32227-274">**Azure Blob input dataset**</span></span>

<span data-ttu-id="32227-275">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="32227-275">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="32227-276">Název složky a cesta k souboru pro tento objekt blob se vyhodnocují dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="32227-276">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="32227-277">Cesta ke složce používá rok, měsíc a den součástí čas spuštění a název souboru používá hodinu součástí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="32227-277">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="32227-278">"externí": "PRAVDA" nastavení informuje služba Data Factory, že tato tabulka je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="32227-278">“external”: “true” setting informs the Data Factory service that this table is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="32227-279">**Oracle výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="32227-279">**Oracle output dataset:**</span></span>

<span data-ttu-id="32227-280">Ukázka předpokládá, že jste vytvořili tabulku "MyTable" v Oracle.</span><span class="sxs-lookup"><span data-stu-id="32227-280">The sample assumes you have created a table “MyTable” in Oracle.</span></span> <span data-ttu-id="32227-281">Vytvořte v tabulce v Oracle s stejný počet sloupců, podle očekávání souboru CSV objektů Blob tak, aby obsahovala.</span><span class="sxs-lookup"><span data-stu-id="32227-281">Create the table in Oracle with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="32227-282">Nové záznamy se přidají do tabulky každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="32227-282">New rows are added to the table every hour.</span></span>

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

<span data-ttu-id="32227-283">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="32227-283">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="32227-284">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="32227-284">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="32227-285">V definici JSON kanálu **zdroj** je typ nastaven na **BlobSource** a **podřízený** je typ nastaven na **OracleSink**.</span><span class="sxs-lookup"><span data-stu-id="32227-285">In the pipeline JSON definition, the **source** type is set to **BlobSource** and the **sink** type is set to **OracleSink**.</span></span>  

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


## <a name="troubleshooting-tips"></a><span data-ttu-id="32227-286">Rady pro řešení potíží</span><span class="sxs-lookup"><span data-stu-id="32227-286">Troubleshooting tips</span></span>
### <a name="problem-1-net-framework-data-provider"></a><span data-ttu-id="32227-287">Problém 1: Zprostředkovatel dat .NET Framework</span><span class="sxs-lookup"><span data-stu-id="32227-287">Problem 1: .NET Framework Data Provider</span></span>

<span data-ttu-id="32227-288">Najdete zde **chybová zpráva**:</span><span class="sxs-lookup"><span data-stu-id="32227-288">You see the following **error message**:</span></span>

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

<span data-ttu-id="32227-289">**Možné příčiny:**</span><span class="sxs-lookup"><span data-stu-id="32227-289">**Possible causes:**</span></span>

1. <span data-ttu-id="32227-290">Zprostředkovatel dat .NET Framework pro Oracle nebyl nainstalován.</span><span class="sxs-lookup"><span data-stu-id="32227-290">The .NET Framework Data Provider for Oracle was not installed.</span></span>
2. <span data-ttu-id="32227-291">Zprostředkovatel dat .NET Framework pro Oracle byla nainstalována na rozhraní .NET Framework 2.0 a nebyl nalezen ve složkách rozhraní .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="32227-291">The .NET Framework Data Provider for Oracle was installed to .NET Framework 2.0 and is not found in the .NET Framework 4.0 folders.</span></span>

<span data-ttu-id="32227-292">**Řešení nebo alternativní řešení:**</span><span class="sxs-lookup"><span data-stu-id="32227-292">**Resolution/Workaround:**</span></span>

1. <span data-ttu-id="32227-293">Pokud jste nenainstalovali poskytovatele .NET pro Oracle, [ji nainstalovat](http://www.oracle.com/technetwork/topics/dotnet/downloads/) a opakujte tento scénář.</span><span class="sxs-lookup"><span data-stu-id="32227-293">If you haven't installed the .NET Provider for Oracle, [install it](http://www.oracle.com/technetwork/topics/dotnet/downloads/) and retry the scenario.</span></span>
2. <span data-ttu-id="32227-294">Pokud se zobrazí chybová zpráva i po instalaci poskytovatele, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="32227-294">If you get the error message even after installing the provider, do the following steps:</span></span>
   1. <span data-ttu-id="32227-295">Otevřete ve složce Konfigurace počítače rozhraní .NET 2.0: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span><span class="sxs-lookup"><span data-stu-id="32227-295">Open machine config of .NET 2.0 from the folder: <system disk>:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span></span>
   2. <span data-ttu-id="32227-296">Vyhledejte **poskytovatele dat Oracle pro .NET**, a mělo by být schopna nalézt položku, jak znázorňuje následující ukázka v části **system.data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="poskytovatele dat Oracle pro .NET</span><span class="sxs-lookup"><span data-stu-id="32227-296">Search for **Oracle Data Provider for .NET**, and you should be able to find an entry as shown in the following sample under **system.data** -> **DbProviderFactories**: “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET</span></span>" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" /><span data-ttu-id="32227-297">”</span><span class="sxs-lookup"><span data-stu-id="32227-297">”</span></span>
3. <span data-ttu-id="32227-298">Zkopírujte tento záznam do souboru machine.config v následující složce v4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config a změňte na verzi 4.xxx.x.x.</span><span class="sxs-lookup"><span data-stu-id="32227-298">Copy this entry to the machine.config file in the following v4.0 folder: <system disk>:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, and change the version to 4.xxx.x.x.</span></span>
4. <span data-ttu-id="32227-299">Instalace "< cesta instalace ODP.NET > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" do globální mezipaměti sestavení (GAC) spuštěním `gacutil /i [provider path]`. ## Tipy pro odstraňování potíží</span><span class="sxs-lookup"><span data-stu-id="32227-299">Install “<ODP.NET Installed Path>\11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll” into the global assembly cache (GAC) by running `gacutil /i [provider path]`.## Troubleshooting tips</span></span>

### <a name="problem-2-datetime-formatting"></a><span data-ttu-id="32227-300">Problém 2: formátování data a času</span><span class="sxs-lookup"><span data-stu-id="32227-300">Problem 2: datetime formatting</span></span>

<span data-ttu-id="32227-301">Najdete zde **chybová zpráva**:</span><span class="sxs-lookup"><span data-stu-id="32227-301">You see the following **error message**:</span></span>

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

<span data-ttu-id="32227-302">**Řešení nebo alternativní řešení:**</span><span class="sxs-lookup"><span data-stu-id="32227-302">**Resolution/Workaround:**</span></span>

<span data-ttu-id="32227-303">Budete muset upravit řetězce dotazu v aktivitě kopírování založené na tom, jak jsou nakonfigurované data do databáze Oracle, jak znázorňuje následující ukázka (pomocí funkce to_date):</span><span class="sxs-lookup"><span data-stu-id="32227-303">You may need to adjust the query string in your copy activity based on how dates are configured in your Oracle database, as shown in the following sample (using the to_date function):</span></span>

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a><span data-ttu-id="32227-304">Mapování typu pro Oracle</span><span class="sxs-lookup"><span data-stu-id="32227-304">Type mapping for Oracle</span></span>
<span data-ttu-id="32227-305">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup krok 2:</span><span class="sxs-lookup"><span data-stu-id="32227-305">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="32227-306">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="32227-306">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="32227-307">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="32227-307">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="32227-308">Při přesouvání dat z databáze Oracle, se používají následující mapování z typu Oracle dat na typ .NET a naopak.</span><span class="sxs-lookup"><span data-stu-id="32227-308">When moving data from Oracle, the following mappings are used from Oracle data type to .NET type and vice versa.</span></span>

| <span data-ttu-id="32227-309">Oracle datový typ</span><span class="sxs-lookup"><span data-stu-id="32227-309">Oracle data type</span></span> | <span data-ttu-id="32227-310">Datový typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="32227-310">.NET Framework data type</span></span> |
| --- | --- |
| <span data-ttu-id="32227-311">BFILE</span><span class="sxs-lookup"><span data-stu-id="32227-311">BFILE</span></span> |<span data-ttu-id="32227-312">Byte]</span><span class="sxs-lookup"><span data-stu-id="32227-312">Byte[]</span></span> |
| <span data-ttu-id="32227-313">OBJEKT BLOB</span><span class="sxs-lookup"><span data-stu-id="32227-313">BLOB</span></span> |<span data-ttu-id="32227-314">Byte]</span><span class="sxs-lookup"><span data-stu-id="32227-314">Byte[]</span></span> |
| <span data-ttu-id="32227-315">CHAR –</span><span class="sxs-lookup"><span data-stu-id="32227-315">CHAR</span></span> |<span data-ttu-id="32227-316">Řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-316">String</span></span> |
| <span data-ttu-id="32227-317">DATOVÝ TYP CLOB</span><span class="sxs-lookup"><span data-stu-id="32227-317">CLOB</span></span> |<span data-ttu-id="32227-318">Řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-318">String</span></span> |
| <span data-ttu-id="32227-319">DATUM</span><span class="sxs-lookup"><span data-stu-id="32227-319">DATE</span></span> |<span data-ttu-id="32227-320">Data a času</span><span class="sxs-lookup"><span data-stu-id="32227-320">DateTime</span></span> |
| <span data-ttu-id="32227-321">PLOVOUCÍ DESETINNÁ ČÁRKA</span><span class="sxs-lookup"><span data-stu-id="32227-321">FLOAT</span></span> |<span data-ttu-id="32227-322">Decimal, řetězec (Pokud přesnost > 28)</span><span class="sxs-lookup"><span data-stu-id="32227-322">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="32227-323">CELÉ ČÍSLO</span><span class="sxs-lookup"><span data-stu-id="32227-323">INTEGER</span></span> |<span data-ttu-id="32227-324">Decimal, řetězec (Pokud přesnost > 28)</span><span class="sxs-lookup"><span data-stu-id="32227-324">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="32227-325">INTERVAL ROK, MĚSÍC</span><span class="sxs-lookup"><span data-stu-id="32227-325">INTERVAL YEAR TO MONTH</span></span> |<span data-ttu-id="32227-326">Int32</span><span class="sxs-lookup"><span data-stu-id="32227-326">Int32</span></span> |
| <span data-ttu-id="32227-327">DENNÍ INTERVAL SEKUNDY.</span><span class="sxs-lookup"><span data-stu-id="32227-327">INTERVAL DAY TO SECOND</span></span> |<span data-ttu-id="32227-328">Časový interval</span><span class="sxs-lookup"><span data-stu-id="32227-328">TimeSpan</span></span> |
| <span data-ttu-id="32227-329">DLOUHÁ</span><span class="sxs-lookup"><span data-stu-id="32227-329">LONG</span></span> |<span data-ttu-id="32227-330">Řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-330">String</span></span> |
| <span data-ttu-id="32227-331">DLOUHO NEZPRACOVANÁ</span><span class="sxs-lookup"><span data-stu-id="32227-331">LONG RAW</span></span> |<span data-ttu-id="32227-332">Byte]</span><span class="sxs-lookup"><span data-stu-id="32227-332">Byte[]</span></span> |
| <span data-ttu-id="32227-333">NCHAR</span><span class="sxs-lookup"><span data-stu-id="32227-333">NCHAR</span></span> |<span data-ttu-id="32227-334">Řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-334">String</span></span> |
| <span data-ttu-id="32227-335">NCLOB</span><span class="sxs-lookup"><span data-stu-id="32227-335">NCLOB</span></span> |<span data-ttu-id="32227-336">Řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-336">String</span></span> |
| <span data-ttu-id="32227-337">ČÍSLO</span><span class="sxs-lookup"><span data-stu-id="32227-337">NUMBER</span></span> |<span data-ttu-id="32227-338">Decimal, řetězec (Pokud přesnost > 28)</span><span class="sxs-lookup"><span data-stu-id="32227-338">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="32227-339">NVARCHAR2</span><span class="sxs-lookup"><span data-stu-id="32227-339">NVARCHAR2</span></span> |<span data-ttu-id="32227-340">Řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-340">String</span></span> |
| <span data-ttu-id="32227-341">NEZPRACOVANÁ</span><span class="sxs-lookup"><span data-stu-id="32227-341">RAW</span></span> |<span data-ttu-id="32227-342">Byte]</span><span class="sxs-lookup"><span data-stu-id="32227-342">Byte[]</span></span> |
| <span data-ttu-id="32227-343">ID ŘÁDKU</span><span class="sxs-lookup"><span data-stu-id="32227-343">ROWID</span></span> |<span data-ttu-id="32227-344">Řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-344">String</span></span> |
| <span data-ttu-id="32227-345">ČASOVÉ RAZÍTKO</span><span class="sxs-lookup"><span data-stu-id="32227-345">TIMESTAMP</span></span> |<span data-ttu-id="32227-346">Data a času</span><span class="sxs-lookup"><span data-stu-id="32227-346">DateTime</span></span> |
| <span data-ttu-id="32227-347">ČASOVÉ RAZÍTKO S MÍSTNÍM ČASOVÉM PÁSMU</span><span class="sxs-lookup"><span data-stu-id="32227-347">TIMESTAMP WITH LOCAL TIME ZONE</span></span> |<span data-ttu-id="32227-348">Data a času</span><span class="sxs-lookup"><span data-stu-id="32227-348">DateTime</span></span> |
| <span data-ttu-id="32227-349">ČASOVÉ RAZÍTKO S ČASOVÝM PÁSMEM</span><span class="sxs-lookup"><span data-stu-id="32227-349">TIMESTAMP WITH TIME ZONE</span></span> |<span data-ttu-id="32227-350">Data a času</span><span class="sxs-lookup"><span data-stu-id="32227-350">DateTime</span></span> |
| <span data-ttu-id="32227-351">CELÉ ČÍSLO BEZ ZNAMÉNKA</span><span class="sxs-lookup"><span data-stu-id="32227-351">UNSIGNED INTEGER</span></span> |<span data-ttu-id="32227-352">Číslo</span><span class="sxs-lookup"><span data-stu-id="32227-352">Number</span></span> |
| <span data-ttu-id="32227-353">VARCHAR2</span><span class="sxs-lookup"><span data-stu-id="32227-353">VARCHAR2</span></span> |<span data-ttu-id="32227-354">Řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-354">String</span></span> |
| <span data-ttu-id="32227-355">XML</span><span class="sxs-lookup"><span data-stu-id="32227-355">XML</span></span> |<span data-ttu-id="32227-356">Řetězec</span><span class="sxs-lookup"><span data-stu-id="32227-356">String</span></span> |

> [!NOTE]
> <span data-ttu-id="32227-357">Datový typ **INTERVAL roku na měsíc** a **INTERVAL den na druhé** nejsou podporována při použití ovladače Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32227-357">Data type **INTERVAL YEAR TO MONTH** and **INTERVAL DAY TO SECOND** are not supported when using Microsoft driver.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="32227-358">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="32227-358">Map source to sink columns</span></span>
<span data-ttu-id="32227-359">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="32227-359">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="32227-360">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="32227-360">Repeatable read from relational sources</span></span>
<span data-ttu-id="32227-361">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="32227-361">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="32227-362">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="32227-362">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="32227-363">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="32227-363">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="32227-364">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="32227-364">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="32227-365">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="32227-365">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="32227-366">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="32227-366">Performance and Tuning</span></span>
<span data-ttu-id="32227-367">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="32227-367">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
