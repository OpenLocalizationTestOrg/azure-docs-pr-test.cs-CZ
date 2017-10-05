---
title: "Přesun dat z PostgreSQL pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z databáze PostgreSQL pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: fd26f0d03f8b0b352a6544a81ad952d2e2a1b7a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a><span data-ttu-id="49ab1-103">Přesun dat z PostgreSQL pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="49ab1-103">Move data from PostgreSQL using Azure Data Factory</span></span>
<span data-ttu-id="49ab1-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z místní databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="49ab1-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises PostgreSQL database.</span></span> <span data-ttu-id="49ab1-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="49ab1-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="49ab1-106">Z úložiště dat PostgreSQL místní může kopírovat data do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="49ab1-106">You can copy data from an on-premises PostgreSQL data store to any supported sink data store.</span></span> <span data-ttu-id="49ab1-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="49ab1-107">For a list of data stores supported as sinks by the copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="49ab1-108">Objekt pro vytváření dat aktuálně podporuje přesunutí dat z databáze PostgreSQL k jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat k databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="49ab1-108">Data factory currently supports moving data from a PostgreSQL database to other data stores, but not for moving data from other data stores to an PostgreSQL database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="49ab1-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="49ab1-109">prerequisites</span></span>

<span data-ttu-id="49ab1-110">Služba data Factory podporuje připojení k místním zdrojům PostgreSQL pomocí Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="49ab1-110">Data Factory service supports connecting to on-premises PostgreSQL sources using the Data Management Gateway.</span></span> <span data-ttu-id="49ab1-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku se dozvíte o Brána pro správu dat a podrobné pokyny o nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="49ab1-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="49ab1-112">Vyžaduje se brána, i když je databáze PostgreSQL hostována v virtuálního počítače Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="49ab1-112">Gateway is required even if the PostgreSQL database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="49ab1-113">Brány můžete nainstalovat na stejný virtuální počítač IaaS jako úložiště dat nebo na jiný virtuální počítač, dokud brána se může připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="49ab1-113">You can install gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="49ab1-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="49ab1-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="49ab1-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="49ab1-115">Supported versions and installation</span></span>
<span data-ttu-id="49ab1-116">Brána pro správu dat pro připojení k databázi PostgreSQL, nainstalujte [Ngpsql zprostředkovatele dat pro PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 nebo výše uvedené ve stejném systému jako brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="49ab1-116">For Data Management Gateway to connect to the PostgreSQL Database, install the [Ngpsql data provider for PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 or above on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="49ab1-117">PostgreSQL verze 7.4 a vyšší je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="49ab1-117">PostgreSQL version 7.4 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="49ab1-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="49ab1-118">Getting started</span></span>
<span data-ttu-id="49ab1-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní PostgreSQL data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="49ab1-119">You can create a pipeline with a copy activity that moves data from an on-premises PostgreSQL data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="49ab1-120">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="49ab1-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="49ab1-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="49ab1-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="49ab1-122">Tyto nástroje můžete taky vytvořit kanál:</span><span class="sxs-lookup"><span data-stu-id="49ab1-122">You can also use the following tools to create a pipeline:</span></span> 
    - <span data-ttu-id="49ab1-123">portál Azure</span><span class="sxs-lookup"><span data-stu-id="49ab1-123">Azure portal</span></span>
    - <span data-ttu-id="49ab1-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49ab1-124">Visual Studio</span></span>
    - <span data-ttu-id="49ab1-125">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="49ab1-125">Azure PowerShell</span></span>
    - <span data-ttu-id="49ab1-126">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="49ab1-126">Azure Resource Manager template</span></span>
    - <span data-ttu-id="49ab1-127">.NET API</span><span class="sxs-lookup"><span data-stu-id="49ab1-127">.NET API</span></span>
    - <span data-ttu-id="49ab1-128">REST API</span><span class="sxs-lookup"><span data-stu-id="49ab1-128">REST API</span></span>

     <span data-ttu-id="49ab1-129">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="49ab1-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="49ab1-130">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="49ab1-130">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="49ab1-131">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="49ab1-131">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="49ab1-132">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="49ab1-132">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="49ab1-133">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="49ab1-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="49ab1-134">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="49ab1-134">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="49ab1-135">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="49ab1-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="49ab1-136">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat PostgreSQL místní, naleznete v tématu [JSON příklad: kopírování dat z PostgreSQL do objektu Blob Azure](#json-example-copy-data-from-postgresql-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="49ab1-136">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises PostgreSQL data store, see [JSON example: Copy data from PostgreSQL to Azure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="49ab1-137">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k úložišti dat PostgreSQL:</span><span class="sxs-lookup"><span data-stu-id="49ab1-137">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a PostgreSQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="49ab1-138">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="49ab1-138">Linked service properties</span></span>
<span data-ttu-id="49ab1-139">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro PostgreSQL propojené služby.</span><span class="sxs-lookup"><span data-stu-id="49ab1-139">The following table provides description for JSON elements specific to PostgreSQL linked service.</span></span>

| <span data-ttu-id="49ab1-140">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="49ab1-140">Property</span></span> | <span data-ttu-id="49ab1-141">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab1-141">Description</span></span> | <span data-ttu-id="49ab1-142">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="49ab1-142">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="49ab1-143">type</span><span class="sxs-lookup"><span data-stu-id="49ab1-143">type</span></span> |<span data-ttu-id="49ab1-144">Vlastnost typu musí být nastavena na: **OnPremisesPostgreSql**</span><span class="sxs-lookup"><span data-stu-id="49ab1-144">The type property must be set to: **OnPremisesPostgreSql**</span></span> |<span data-ttu-id="49ab1-145">Ano</span><span class="sxs-lookup"><span data-stu-id="49ab1-145">Yes</span></span> |
| <span data-ttu-id="49ab1-146">server</span><span class="sxs-lookup"><span data-stu-id="49ab1-146">server</span></span> |<span data-ttu-id="49ab1-147">Název serveru PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="49ab1-147">Name of the PostgreSQL server.</span></span> |<span data-ttu-id="49ab1-148">Ano</span><span class="sxs-lookup"><span data-stu-id="49ab1-148">Yes</span></span> |
| <span data-ttu-id="49ab1-149">Databáze</span><span class="sxs-lookup"><span data-stu-id="49ab1-149">database</span></span> |<span data-ttu-id="49ab1-150">Název databáze PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="49ab1-150">Name of the PostgreSQL database.</span></span> |<span data-ttu-id="49ab1-151">Ano</span><span class="sxs-lookup"><span data-stu-id="49ab1-151">Yes</span></span> |
| <span data-ttu-id="49ab1-152">Schéma</span><span class="sxs-lookup"><span data-stu-id="49ab1-152">schema</span></span> |<span data-ttu-id="49ab1-153">Název schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="49ab1-153">Name of the schema in the database.</span></span> <span data-ttu-id="49ab1-154">Název schématu rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="49ab1-154">The schema name is case-sensitive.</span></span> |<span data-ttu-id="49ab1-155">Ne</span><span class="sxs-lookup"><span data-stu-id="49ab1-155">No</span></span> |
| <span data-ttu-id="49ab1-156">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="49ab1-156">authenticationType</span></span> |<span data-ttu-id="49ab1-157">Typ ověřování používaný pro připojení k databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="49ab1-157">Type of authentication used to connect to the PostgreSQL database.</span></span> <span data-ttu-id="49ab1-158">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="49ab1-158">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="49ab1-159">Ano</span><span class="sxs-lookup"><span data-stu-id="49ab1-159">Yes</span></span> |
| <span data-ttu-id="49ab1-160">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="49ab1-160">username</span></span> |<span data-ttu-id="49ab1-161">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="49ab1-161">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="49ab1-162">Ne</span><span class="sxs-lookup"><span data-stu-id="49ab1-162">No</span></span> |
| <span data-ttu-id="49ab1-163">heslo</span><span class="sxs-lookup"><span data-stu-id="49ab1-163">password</span></span> |<span data-ttu-id="49ab1-164">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="49ab1-164">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="49ab1-165">Ne</span><span class="sxs-lookup"><span data-stu-id="49ab1-165">No</span></span> |
| <span data-ttu-id="49ab1-166">gatewayName</span><span class="sxs-lookup"><span data-stu-id="49ab1-166">gatewayName</span></span> |<span data-ttu-id="49ab1-167">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="49ab1-167">Name of the gateway that the Data Factory service should use to connect to the on-premises PostgreSQL database.</span></span> |<span data-ttu-id="49ab1-168">Ano</span><span class="sxs-lookup"><span data-stu-id="49ab1-168">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="49ab1-169">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="49ab1-169">Dataset properties</span></span>
<span data-ttu-id="49ab1-170">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="49ab1-170">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="49ab1-171">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="49ab1-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="49ab1-172">V rámci typeProperties části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="49ab1-172">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="49ab1-173">Rámci typeProperties část datové sady typ **RelationalTable** (což zahrnuje datová sada PostgreSQL) má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="49ab1-173">The typeProperties section for dataset of type **RelationalTable** (which includes PostgreSQL dataset) has the following properties:</span></span>

| <span data-ttu-id="49ab1-174">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="49ab1-174">Property</span></span> | <span data-ttu-id="49ab1-175">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab1-175">Description</span></span> | <span data-ttu-id="49ab1-176">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="49ab1-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="49ab1-177">tableName</span><span class="sxs-lookup"><span data-stu-id="49ab1-177">tableName</span></span> |<span data-ttu-id="49ab1-178">Název tabulky instance databáze PostgreSQL, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="49ab1-178">Name of the table in the PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="49ab1-179">TableName rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="49ab1-179">The tableName is case-sensitive.</span></span> |<span data-ttu-id="49ab1-180">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="49ab1-180">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="49ab1-181">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="49ab1-181">Copy activity properties</span></span>
<span data-ttu-id="49ab1-182">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="49ab1-182">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="49ab1-183">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="49ab1-183">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="49ab1-184">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="49ab1-184">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="49ab1-185">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="49ab1-185">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="49ab1-186">Pokud je zdroj typu **RelationalSource** (která zahrnuje PostgreSQL), následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="49ab1-186">When source is of type **RelationalSource** (which includes PostgreSQL), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="49ab1-187">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="49ab1-187">Property</span></span> | <span data-ttu-id="49ab1-188">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab1-188">Description</span></span> | <span data-ttu-id="49ab1-189">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="49ab1-189">Allowed values</span></span> | <span data-ttu-id="49ab1-190">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="49ab1-190">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="49ab1-191">query</span><span class="sxs-lookup"><span data-stu-id="49ab1-191">query</span></span> |<span data-ttu-id="49ab1-192">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="49ab1-192">Use the custom query to read data.</span></span> |<span data-ttu-id="49ab1-193">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="49ab1-193">SQL query string.</span></span> <span data-ttu-id="49ab1-194">Například: "dotaz": "vybrat * z \"MySchema\".\" MyTable\"".</span><span class="sxs-lookup"><span data-stu-id="49ab1-194">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="49ab1-195">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="49ab1-195">No (if **tableName** of **dataset** is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="49ab1-196">Schéma a tabulku názvy rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="49ab1-196">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="49ab1-197">Uzavřete je do `""` (dvojité uvozovky) v dotazu.</span><span class="sxs-lookup"><span data-stu-id="49ab1-197">Enclose them in `""` (double quotes) in the query.</span></span>  

<span data-ttu-id="49ab1-198">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="49ab1-198">**Example:**</span></span>

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-to-azure-blob"></a><span data-ttu-id="49ab1-199">Příklad JSON: kopírování dat z PostgreSQL do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="49ab1-199">JSON example: Copy data from PostgreSQL to Azure Blob</span></span>
<span data-ttu-id="49ab1-200">Tento příklad obsahuje ukázkové JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="49ab1-200">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="49ab1-201">Se ukazují, jak zkopírovat data z databáze PostgreSQL do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="49ab1-201">They show how to copy data from PostgreSQL database to Azure Blob Storage.</span></span> <span data-ttu-id="49ab1-202">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="49ab1-202">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="49ab1-203">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="49ab1-203">This sample provides JSON snippets.</span></span> <span data-ttu-id="49ab1-204">Podrobné pokyny pro vytvoření objektu pro vytváření dat neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="49ab1-204">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="49ab1-205">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="49ab1-205">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="49ab1-206">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="49ab1-206">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="49ab1-207">Propojené služby typu [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="49ab1-207">A linked service of type [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="49ab1-208">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="49ab1-208">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="49ab1-209">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="49ab1-209">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="49ab1-210">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="49ab1-210">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="49ab1-211">[Kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="49ab1-211">The [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="49ab1-212">Ukázka zkopíruje data z výsledku dotazu v databázi PostgreSQL do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="49ab1-212">The sample copies data from a query result in PostgreSQL database to a blob every hour.</span></span> <span data-ttu-id="49ab1-213">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="49ab1-213">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="49ab1-214">Jako první krok nastavte Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="49ab1-214">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="49ab1-215">Tyto pokyny jsou v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="49ab1-215">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="49ab1-216">**PostgreSQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="49ab1-216">**PostgreSQL linked service:**</span></span>

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
<span data-ttu-id="49ab1-217">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="49ab1-217">**Azure Blob storage linked service:**</span></span>

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
    }
    }
}
```
<span data-ttu-id="49ab1-218">**PostgreSQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="49ab1-218">**PostgreSQL input dataset:**</span></span>

<span data-ttu-id="49ab1-219">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v PostgreSQL a obsahuje sloupec s názvem "časové razítko" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="49ab1-219">The sample assumes you have created a table “MyTable” in PostgreSQL and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="49ab1-220">Nastavení `"external": true` služba Data Factory informuje, že datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="49ab1-220">Setting `"external": true` informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

<span data-ttu-id="49ab1-221">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="49ab1-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="49ab1-222">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="49ab1-222">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="49ab1-223">Název složky a cesta k souboru pro tento objekt blob se vyhodnocují dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="49ab1-223">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="49ab1-224">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="49ab1-224">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="49ab1-225">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="49ab1-225">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="49ab1-226">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="49ab1-226">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="49ab1-227">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="49ab1-227">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="49ab1-228">Zadané pro dotaz SQL **dotazu** vlastnost vybere data z public.usstates tabulky v databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="49ab1-228">The SQL query specified for the **query** property selects the data from the public.usstates table in the PostgreSQL database.</span></span>

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"public\".\"usstates\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a><span data-ttu-id="49ab1-229">Mapování typu pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="49ab1-229">Type mapping for PostgreSQL</span></span>
<span data-ttu-id="49ab1-230">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup krok 2:</span><span class="sxs-lookup"><span data-stu-id="49ab1-230">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="49ab1-231">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="49ab1-231">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="49ab1-232">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="49ab1-232">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="49ab1-233">Při přesunu dat na PostgreSQL, se používají následující mapování z typu PostgreSQL na typ .NET.</span><span class="sxs-lookup"><span data-stu-id="49ab1-233">When moving data to PostgreSQL, the following mappings are used from PostgreSQL type to .NET type.</span></span>

| <span data-ttu-id="49ab1-234">Typ databáze PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="49ab1-234">PostgreSQL Database type</span></span> | <span data-ttu-id="49ab1-235">PostgresSQL aliasy</span><span class="sxs-lookup"><span data-stu-id="49ab1-235">PostgresSQL aliases</span></span> | <span data-ttu-id="49ab1-236">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="49ab1-236">.NET Framework type</span></span> |
| --- | --- | --- |
| <span data-ttu-id="49ab1-237">abstime</span><span class="sxs-lookup"><span data-stu-id="49ab1-237">abstime</span></span> | |<span data-ttu-id="49ab1-238">Data a času</span><span class="sxs-lookup"><span data-stu-id="49ab1-238">Datetime</span></span> | &nbsp;
| <span data-ttu-id="49ab1-239">bigint</span><span class="sxs-lookup"><span data-stu-id="49ab1-239">bigint</span></span> |<span data-ttu-id="49ab1-240">int8</span><span class="sxs-lookup"><span data-stu-id="49ab1-240">int8</span></span> |<span data-ttu-id="49ab1-241">Int64</span><span class="sxs-lookup"><span data-stu-id="49ab1-241">Int64</span></span> |
| <span data-ttu-id="49ab1-242">bigserial</span><span class="sxs-lookup"><span data-stu-id="49ab1-242">bigserial</span></span> |<span data-ttu-id="49ab1-243">serial8</span><span class="sxs-lookup"><span data-stu-id="49ab1-243">serial8</span></span> |<span data-ttu-id="49ab1-244">Int64</span><span class="sxs-lookup"><span data-stu-id="49ab1-244">Int64</span></span> |
| <span data-ttu-id="49ab1-245">bit [(ne)]</span><span class="sxs-lookup"><span data-stu-id="49ab1-245">bit [ (n) ]</span></span> | |<span data-ttu-id="49ab1-246">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-246">Byte[], String</span></span> | &nbsp;
| <span data-ttu-id="49ab1-247">bit různých [(ne)]</span><span class="sxs-lookup"><span data-stu-id="49ab1-247">bit varying [ (n) ]</span></span> |<span data-ttu-id="49ab1-248">varbit</span><span class="sxs-lookup"><span data-stu-id="49ab1-248">varbit</span></span> |<span data-ttu-id="49ab1-249">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-249">Byte[], String</span></span> |
| <span data-ttu-id="49ab1-250">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="49ab1-250">boolean</span></span> |<span data-ttu-id="49ab1-251">BOOL</span><span class="sxs-lookup"><span data-stu-id="49ab1-251">bool</span></span> |<span data-ttu-id="49ab1-252">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="49ab1-252">Boolean</span></span> |
| <span data-ttu-id="49ab1-253">Pole</span><span class="sxs-lookup"><span data-stu-id="49ab1-253">box</span></span> | |<span data-ttu-id="49ab1-254">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-254">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-255">bytea</span><span class="sxs-lookup"><span data-stu-id="49ab1-255">bytea</span></span> | |<span data-ttu-id="49ab1-256">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-256">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-257">znak [(ne)]</span><span class="sxs-lookup"><span data-stu-id="49ab1-257">character [ (n) ]</span></span> |<span data-ttu-id="49ab1-258">char [(ne)]</span><span class="sxs-lookup"><span data-stu-id="49ab1-258">char [ (n) ]</span></span> |<span data-ttu-id="49ab1-259">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-259">String</span></span> |
| <span data-ttu-id="49ab1-260">znak různých [(ne)]</span><span class="sxs-lookup"><span data-stu-id="49ab1-260">character varying [ (n) ]</span></span> |<span data-ttu-id="49ab1-261">varchar [(ne)]</span><span class="sxs-lookup"><span data-stu-id="49ab1-261">varchar [ (n) ]</span></span> |<span data-ttu-id="49ab1-262">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-262">String</span></span> |
| <span data-ttu-id="49ab1-263">CID</span><span class="sxs-lookup"><span data-stu-id="49ab1-263">cid</span></span> | |<span data-ttu-id="49ab1-264">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-264">String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-265">CIDR</span><span class="sxs-lookup"><span data-stu-id="49ab1-265">cidr</span></span> | |<span data-ttu-id="49ab1-266">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-266">String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-267">kruhu.</span><span class="sxs-lookup"><span data-stu-id="49ab1-267">circle</span></span> | |<span data-ttu-id="49ab1-268">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-268">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-269">Datum</span><span class="sxs-lookup"><span data-stu-id="49ab1-269">date</span></span> | |<span data-ttu-id="49ab1-270">Data a času</span><span class="sxs-lookup"><span data-stu-id="49ab1-270">Datetime</span></span> |&nbsp;
| <span data-ttu-id="49ab1-271">DateRange</span><span class="sxs-lookup"><span data-stu-id="49ab1-271">daterange</span></span> | |<span data-ttu-id="49ab1-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-272">String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-273">Dvojitá přesnost</span><span class="sxs-lookup"><span data-stu-id="49ab1-273">double precision</span></span> |<span data-ttu-id="49ab1-274">FLOAT8</span><span class="sxs-lookup"><span data-stu-id="49ab1-274">float8</span></span> |<span data-ttu-id="49ab1-275">Double</span><span class="sxs-lookup"><span data-stu-id="49ab1-275">Double</span></span> |
| <span data-ttu-id="49ab1-276">inet</span><span class="sxs-lookup"><span data-stu-id="49ab1-276">inet</span></span> | |<span data-ttu-id="49ab1-277">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-277">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-278">intarry</span><span class="sxs-lookup"><span data-stu-id="49ab1-278">intarry</span></span> | |<span data-ttu-id="49ab1-279">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-279">String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-280">int4range</span><span class="sxs-lookup"><span data-stu-id="49ab1-280">int4range</span></span> | |<span data-ttu-id="49ab1-281">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-281">String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-282">int8range</span><span class="sxs-lookup"><span data-stu-id="49ab1-282">int8range</span></span> | |<span data-ttu-id="49ab1-283">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-283">String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-284">celé číslo</span><span class="sxs-lookup"><span data-stu-id="49ab1-284">integer</span></span> |<span data-ttu-id="49ab1-285">int, int4</span><span class="sxs-lookup"><span data-stu-id="49ab1-285">int, int4</span></span> |<span data-ttu-id="49ab1-286">Int32</span><span class="sxs-lookup"><span data-stu-id="49ab1-286">Int32</span></span> |
| <span data-ttu-id="49ab1-287">Interval [pole] [(p)]</span><span class="sxs-lookup"><span data-stu-id="49ab1-287">interval [ fields ] [ (p) ]</span></span> | |<span data-ttu-id="49ab1-288">Časový interval</span><span class="sxs-lookup"><span data-stu-id="49ab1-288">Timespan</span></span> |&nbsp;
| <span data-ttu-id="49ab1-289">JSON</span><span class="sxs-lookup"><span data-stu-id="49ab1-289">json</span></span> | |<span data-ttu-id="49ab1-290">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-290">String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-291">jsonb</span><span class="sxs-lookup"><span data-stu-id="49ab1-291">jsonb</span></span> | |<span data-ttu-id="49ab1-292">Byte]</span><span class="sxs-lookup"><span data-stu-id="49ab1-292">Byte[]</span></span> |&nbsp;
| <span data-ttu-id="49ab1-293">řádek</span><span class="sxs-lookup"><span data-stu-id="49ab1-293">line</span></span> | |<span data-ttu-id="49ab1-294">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-294">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-295">lseg</span><span class="sxs-lookup"><span data-stu-id="49ab1-295">lseg</span></span> | |<span data-ttu-id="49ab1-296">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-296">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-297">macaddr</span><span class="sxs-lookup"><span data-stu-id="49ab1-297">macaddr</span></span> | |<span data-ttu-id="49ab1-298">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-298">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-299">peníze</span><span class="sxs-lookup"><span data-stu-id="49ab1-299">money</span></span> | |<span data-ttu-id="49ab1-300">Decimal</span><span class="sxs-lookup"><span data-stu-id="49ab1-300">Decimal</span></span> |&nbsp;
| <span data-ttu-id="49ab1-301">číselný [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="49ab1-301">numeric [ (p, s) ]</span></span> |<span data-ttu-id="49ab1-302">Decimal [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="49ab1-302">decimal [ (p, s) ]</span></span> |<span data-ttu-id="49ab1-303">Decimal</span><span class="sxs-lookup"><span data-stu-id="49ab1-303">Decimal</span></span> |
| <span data-ttu-id="49ab1-304">numrange</span><span class="sxs-lookup"><span data-stu-id="49ab1-304">numrange</span></span> | |<span data-ttu-id="49ab1-305">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-305">String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-306">OID</span><span class="sxs-lookup"><span data-stu-id="49ab1-306">oid</span></span> | |<span data-ttu-id="49ab1-307">Int32</span><span class="sxs-lookup"><span data-stu-id="49ab1-307">Int32</span></span> |&nbsp;
| <span data-ttu-id="49ab1-308">Cesta</span><span class="sxs-lookup"><span data-stu-id="49ab1-308">path</span></span> | |<span data-ttu-id="49ab1-309">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-309">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-310">pg_lsn</span><span class="sxs-lookup"><span data-stu-id="49ab1-310">pg_lsn</span></span> | |<span data-ttu-id="49ab1-311">Int64</span><span class="sxs-lookup"><span data-stu-id="49ab1-311">Int64</span></span> |&nbsp;
| <span data-ttu-id="49ab1-312">bod</span><span class="sxs-lookup"><span data-stu-id="49ab1-312">point</span></span> | |<span data-ttu-id="49ab1-313">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-313">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-314">mnohoúhelníku</span><span class="sxs-lookup"><span data-stu-id="49ab1-314">polygon</span></span> | |<span data-ttu-id="49ab1-315">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-315">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="49ab1-316">skutečné</span><span class="sxs-lookup"><span data-stu-id="49ab1-316">real</span></span> |<span data-ttu-id="49ab1-317">FLOAT4</span><span class="sxs-lookup"><span data-stu-id="49ab1-317">float4</span></span> |<span data-ttu-id="49ab1-318">Jeden</span><span class="sxs-lookup"><span data-stu-id="49ab1-318">Single</span></span> |
| <span data-ttu-id="49ab1-319">smallint</span><span class="sxs-lookup"><span data-stu-id="49ab1-319">smallint</span></span> |<span data-ttu-id="49ab1-320">int2</span><span class="sxs-lookup"><span data-stu-id="49ab1-320">int2</span></span> |<span data-ttu-id="49ab1-321">Int16</span><span class="sxs-lookup"><span data-stu-id="49ab1-321">Int16</span></span> |
| <span data-ttu-id="49ab1-322">smallserial</span><span class="sxs-lookup"><span data-stu-id="49ab1-322">smallserial</span></span> |<span data-ttu-id="49ab1-323">serial2</span><span class="sxs-lookup"><span data-stu-id="49ab1-323">serial2</span></span> |<span data-ttu-id="49ab1-324">Int16</span><span class="sxs-lookup"><span data-stu-id="49ab1-324">Int16</span></span> |
| <span data-ttu-id="49ab1-325">Sériového portu</span><span class="sxs-lookup"><span data-stu-id="49ab1-325">serial</span></span> |<span data-ttu-id="49ab1-326">serial4</span><span class="sxs-lookup"><span data-stu-id="49ab1-326">serial4</span></span> |<span data-ttu-id="49ab1-327">Int32</span><span class="sxs-lookup"><span data-stu-id="49ab1-327">Int32</span></span> |
| <span data-ttu-id="49ab1-328">Text</span><span class="sxs-lookup"><span data-stu-id="49ab1-328">text</span></span> | |<span data-ttu-id="49ab1-329">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab1-329">String</span></span> |&nbsp;

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="49ab1-330">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="49ab1-330">Map source to sink columns</span></span>
<span data-ttu-id="49ab1-331">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="49ab1-331">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="49ab1-332">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="49ab1-332">Repeatable read from relational sources</span></span>
<span data-ttu-id="49ab1-333">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="49ab1-333">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="49ab1-334">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="49ab1-334">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="49ab1-335">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="49ab1-335">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="49ab1-336">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="49ab1-336">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="49ab1-337">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="49ab1-337">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="49ab1-338">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="49ab1-338">Performance and Tuning</span></span>
<span data-ttu-id="49ab1-339">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="49ab1-339">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
