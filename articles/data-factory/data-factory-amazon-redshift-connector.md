---
title: "Přesun dat z Amazon Redshift pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z Amazonu Redshift pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: bccb941363952bb2251629240a88148a6527d62e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a><span data-ttu-id="dd4c3-103">Přesun dat z Amazon Redshift pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="dd4c3-103">Move data From Amazon Redshift using Azure Data Factory</span></span>
<span data-ttu-id="dd4c3-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from Amazon Redshift.</span></span> <span data-ttu-id="dd4c3-105">Článek vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-105">The article builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

<span data-ttu-id="dd4c3-106">Data můžete zkopírovat z Amazon Redshift do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-106">You can copy data from Amazon Redshift to any supported sink data store.</span></span> <span data-ttu-id="dd4c3-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-107">For a list of data stores supported as sinks by the copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="dd4c3-108">Objekt pro vytváření dat aktuálně podporuje přesunutí dat z Amazon Redshift k jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat na Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-108">Data factory currently supports moving data from Amazon Redshift to other data stores, but not for moving data from other data stores to Amazon Redshift.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd4c3-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dd4c3-109">Prerequisites</span></span>
* <span data-ttu-id="dd4c3-110">Pokud přesouváte data k úložišti dat na místě, nainstalujte [Brána pro správu dat](data-factory-data-management-gateway.md) na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-110">If you are moving data to an on-premises data store, install [Data Management Gateway](data-factory-data-management-gateway.md) on an on-premises machine.</span></span> <span data-ttu-id="dd4c3-111">Brána pro správu dat (použijte IP adresu počítače), pak zajistit přístup do clusteru Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-111">Then, Grant Data Management Gateway (use IP address of the machine) the access to Amazon Redshift cluster.</span></span> <span data-ttu-id="dd4c3-112">V tématu [autorizovat přístup ke clusteru](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) pokyny.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-112">See [Authorize access to the cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) for instructions.</span></span>
* <span data-ttu-id="dd4c3-113">Pokud přesouváte data k úložišti dat Azure, najdete v části [rozsahy IP Center dat Azure](https://www.microsoft.com/download/details.aspx?id=41653) pro výpočetní IP adresy a rozsahy SQL používaných dat Azure centra.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-113">If you are moving data to an Azure data store, see [Azure Data Center IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) for the Compute IP address and SQL ranges used by the Azure data centers.</span></span>

## <a name="getting-started"></a><span data-ttu-id="dd4c3-114">Začínáme</span><span class="sxs-lookup"><span data-stu-id="dd4c3-114">Getting started</span></span>
<span data-ttu-id="dd4c3-115">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Amazonu Redshift zdroje pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-115">You can create a pipeline with a copy activity that moves data from an Amazon Redshift source by using different tools/APIs.</span></span>

<span data-ttu-id="dd4c3-116">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-116">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="dd4c3-117">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-117">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="dd4c3-118">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-118">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="dd4c3-119">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-119">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="dd4c3-120">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="dd4c3-120">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="dd4c3-121">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-121">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="dd4c3-122">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-122">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="dd4c3-123">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-123">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="dd4c3-124">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-124">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="dd4c3-125">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-125">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="dd4c3-126">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat Amazon Redshift, naleznete v tématu [JSON příklad: kopírování dat z Amazon Redshift do objektu Blob Azure](#json-example-copy-data-from-amazon-redshift-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-126">For a sample with JSON definitions for Data Factory entities that are used to copy data from an Amazon Redshift data store, see [JSON example: Copy data from Amazon Redshift to Azure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="dd4c3-127">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k definování entit služby Data Factory, které jsou specifické pro Amazon Redshift:</span><span class="sxs-lookup"><span data-stu-id="dd4c3-127">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Amazon Redshift:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="dd4c3-128">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="dd4c3-128">Linked service properties</span></span>
<span data-ttu-id="dd4c3-129">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro službu Amazon Redshift propojené služby.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-129">The following table provides description for JSON elements specific to Amazon Redshift linked service.</span></span>

| <span data-ttu-id="dd4c3-130">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="dd4c3-130">Property</span></span> | <span data-ttu-id="dd4c3-131">Popis</span><span class="sxs-lookup"><span data-stu-id="dd4c3-131">Description</span></span> | <span data-ttu-id="dd4c3-132">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dd4c3-132">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dd4c3-133">type</span><span class="sxs-lookup"><span data-stu-id="dd4c3-133">type</span></span> |<span data-ttu-id="dd4c3-134">Vlastnost typu musí být nastavena na: **AmazonRedshift**.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-134">The type property must be set to: **AmazonRedshift**.</span></span> |<span data-ttu-id="dd4c3-135">Ano</span><span class="sxs-lookup"><span data-stu-id="dd4c3-135">Yes</span></span> |
| <span data-ttu-id="dd4c3-136">server</span><span class="sxs-lookup"><span data-stu-id="dd4c3-136">server</span></span> |<span data-ttu-id="dd4c3-137">IP adresa nebo název hostitele serveru Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-137">IP address or host name of the Amazon Redshift server.</span></span> |<span data-ttu-id="dd4c3-138">Ano</span><span class="sxs-lookup"><span data-stu-id="dd4c3-138">Yes</span></span> |
| <span data-ttu-id="dd4c3-139">port</span><span class="sxs-lookup"><span data-stu-id="dd4c3-139">port</span></span> |<span data-ttu-id="dd4c3-140">Číslo portu TCP, který používá server Amazon Redshift naslouchat pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-140">The number of the TCP port that the Amazon Redshift server uses to listen for client connections.</span></span> |<span data-ttu-id="dd4c3-141">Ne, výchozí hodnota: 5439</span><span class="sxs-lookup"><span data-stu-id="dd4c3-141">No, default value: 5439</span></span> |
| <span data-ttu-id="dd4c3-142">Databáze</span><span class="sxs-lookup"><span data-stu-id="dd4c3-142">database</span></span> |<span data-ttu-id="dd4c3-143">Název databáze Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-143">Name of the Amazon Redshift database.</span></span> |<span data-ttu-id="dd4c3-144">Ano</span><span class="sxs-lookup"><span data-stu-id="dd4c3-144">Yes</span></span> |
| <span data-ttu-id="dd4c3-145">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="dd4c3-145">username</span></span> |<span data-ttu-id="dd4c3-146">Jméno uživatele, který má přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-146">Name of user who has access to the database.</span></span> |<span data-ttu-id="dd4c3-147">Ano</span><span class="sxs-lookup"><span data-stu-id="dd4c3-147">Yes</span></span> |
| <span data-ttu-id="dd4c3-148">heslo</span><span class="sxs-lookup"><span data-stu-id="dd4c3-148">password</span></span> |<span data-ttu-id="dd4c3-149">Heslo pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-149">Password for the user account.</span></span> |<span data-ttu-id="dd4c3-150">Ano</span><span class="sxs-lookup"><span data-stu-id="dd4c3-150">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="dd4c3-151">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="dd4c3-151">Dataset properties</span></span>
<span data-ttu-id="dd4c3-152">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-152">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="dd4c3-153">Oddíly, jako je například struktura, dostupnost a zásady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-153">Sections such as structure, availability, and policy are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="dd4c3-154">**Rámci typeProperties** části se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-154">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="dd4c3-155">Poskytuje informace o umístění dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-155">It provides information about the location of the data in the data store.</span></span> <span data-ttu-id="dd4c3-156">Rámci typeProperties část datové sady typ **RelationalTable** (což zahrnuje datová sada Amazon Redshift) má následující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="dd4c3-156">The typeProperties section for dataset of type **RelationalTable** (which includes Amazon Redshift dataset) has the following properties</span></span>

| <span data-ttu-id="dd4c3-157">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="dd4c3-157">Property</span></span> | <span data-ttu-id="dd4c3-158">Popis</span><span class="sxs-lookup"><span data-stu-id="dd4c3-158">Description</span></span> | <span data-ttu-id="dd4c3-159">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dd4c3-159">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dd4c3-160">tableName</span><span class="sxs-lookup"><span data-stu-id="dd4c3-160">tableName</span></span> |<span data-ttu-id="dd4c3-161">Název tabulky v databázi Amazon Redshift, propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-161">Name of the table in the Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="dd4c3-162">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="dd4c3-162">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="dd4c3-163">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="dd4c3-163">Copy activity properties</span></span>
<span data-ttu-id="dd4c3-164">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-164">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="dd4c3-165">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-165">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="dd4c3-166">Vzhledem k tomu, vlastnosti dostupné ve **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-166">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="dd4c3-167">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-167">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="dd4c3-168">Když je zdrojem kopie aktivity typu **RelationalSource** (která zahrnuje Amazon Redshift), v rámci typeProperties části jsou k dispozici následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="dd4c3-168">When source of copy activity is of type **RelationalSource** (which includes Amazon Redshift), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="dd4c3-169">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="dd4c3-169">Property</span></span> | <span data-ttu-id="dd4c3-170">Popis</span><span class="sxs-lookup"><span data-stu-id="dd4c3-170">Description</span></span> | <span data-ttu-id="dd4c3-171">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="dd4c3-171">Allowed values</span></span> | <span data-ttu-id="dd4c3-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dd4c3-172">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="dd4c3-173">query</span><span class="sxs-lookup"><span data-stu-id="dd4c3-173">query</span></span> |<span data-ttu-id="dd4c3-174">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-174">Use the custom query to read data.</span></span> |<span data-ttu-id="dd4c3-175">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-175">SQL query string.</span></span> <span data-ttu-id="dd4c3-176">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-176">For example: select * from MyTable.</span></span> |<span data-ttu-id="dd4c3-177">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="dd4c3-177">No (if **tableName** of **dataset** is specified)</span></span> |

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob"></a><span data-ttu-id="dd4c3-178">Příklad JSON: kopírování dat z Amazon Redshift do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="dd4c3-178">JSON example: Copy data from Amazon Redshift to Azure Blob</span></span>
<span data-ttu-id="dd4c3-179">Tento příklad ukazuje, jak zkopírovat data z databáze Amazon Redshift do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-179">This sample shows how to copy data from an Amazon Redshift database to an Azure Blob Storage.</span></span> <span data-ttu-id="dd4c3-180">Však můžete zkopírovat data **přímo** žádnému jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-180">However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="dd4c3-181">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="dd4c3-181">The sample has the following data factory entities:</span></span>

* <span data-ttu-id="dd4c3-182">Propojené služby typu [AmazonRedshift](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-182">A linked service of type [AmazonRedshift](#linked-service-properties).</span></span>
* <span data-ttu-id="dd4c3-183">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-183">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="dd4c3-184">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-184">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
* <span data-ttu-id="dd4c3-185">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-185">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="dd4c3-186">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-186">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span></span>

<span data-ttu-id="dd4c3-187">Ukázka zkopíruje data z výsledku dotazu v Amazon Redshift do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-187">The sample copies data from a query result in Amazon Redshift to a blob every hour.</span></span> <span data-ttu-id="dd4c3-188">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-188">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="dd4c3-189">**Amazon Redshift propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="dd4c3-189">**Amazon Redshift linked service:**</span></span>

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< The IP address or host name of the Amazon Redshift server >",
            "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="dd4c3-190">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="dd4c3-190">**Azure Storage linked service:**</span></span>

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="dd4c3-191">**Amazon Redshift vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="dd4c3-191">**Amazon Redshift input dataset:**</span></span>

<span data-ttu-id="dd4c3-192">Nastavení `"external": true` služba Data Factory informuje, že datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-192">Setting `"external": true` informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span> <span data-ttu-id="dd4c3-193">Nastavte tuto vlastnost na hodnotu true vstupní datovou sadu, která není vyprodukované aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-193">Set this property to true on an input dataset that is not produced by an activity in the pipeline.</span></span>

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

<span data-ttu-id="dd4c3-194">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="dd4c3-194">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="dd4c3-195">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-195">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="dd4c3-196">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-196">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="dd4c3-197">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-197">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="dd4c3-198">**Aktivita kopírování v kanálu se zdrojem Azure Redshift (RelationalSource) a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="dd4c3-198">**Copy activity in a pipeline with Azure Redshift source (RelationalSource) and Blob sink:**</span></span>

<span data-ttu-id="dd4c3-199">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-199">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="dd4c3-200">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-200">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="dd4c3-201">Zadané pro dotaz SQL **dotazu** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-201">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonRedshiftInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
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
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a><span data-ttu-id="dd4c3-202">Mapování typu pro Amazon Redshift</span><span class="sxs-lookup"><span data-stu-id="dd4c3-202">Type mapping for Amazon Redshift</span></span>
<span data-ttu-id="dd4c3-203">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup ve dvou krocích:</span><span class="sxs-lookup"><span data-stu-id="dd4c3-203">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="dd4c3-204">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="dd4c3-204">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="dd4c3-205">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="dd4c3-205">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="dd4c3-206">Při přesunu dat na Amazon Redshift, se používají následující mapování z typů Amazon Redshift na typy .NET.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-206">When moving data to Amazon Redshift, the following mappings are used from Amazon Redshift types to .NET types.</span></span>

| <span data-ttu-id="dd4c3-207">Typ Redshift Amazon</span><span class="sxs-lookup"><span data-stu-id="dd4c3-207">Amazon Redshift Type</span></span> | <span data-ttu-id="dd4c3-208">.NET na základě typu</span><span class="sxs-lookup"><span data-stu-id="dd4c3-208">.NET Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="dd4c3-209">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="dd4c3-209">SMALLINT</span></span> |<span data-ttu-id="dd4c3-210">Int16</span><span class="sxs-lookup"><span data-stu-id="dd4c3-210">Int16</span></span> |
| <span data-ttu-id="dd4c3-211">CELÉ ČÍSLO</span><span class="sxs-lookup"><span data-stu-id="dd4c3-211">INTEGER</span></span> |<span data-ttu-id="dd4c3-212">Int32</span><span class="sxs-lookup"><span data-stu-id="dd4c3-212">Int32</span></span> |
| <span data-ttu-id="dd4c3-213">BIGINT</span><span class="sxs-lookup"><span data-stu-id="dd4c3-213">BIGINT</span></span> |<span data-ttu-id="dd4c3-214">Int64</span><span class="sxs-lookup"><span data-stu-id="dd4c3-214">Int64</span></span> |
| <span data-ttu-id="dd4c3-215">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="dd4c3-215">DECIMAL</span></span> |<span data-ttu-id="dd4c3-216">Decimal</span><span class="sxs-lookup"><span data-stu-id="dd4c3-216">Decimal</span></span> |
| <span data-ttu-id="dd4c3-217">SKUTEČNÉ</span><span class="sxs-lookup"><span data-stu-id="dd4c3-217">REAL</span></span> |<span data-ttu-id="dd4c3-218">Jeden</span><span class="sxs-lookup"><span data-stu-id="dd4c3-218">Single</span></span> |
| <span data-ttu-id="dd4c3-219">DVOJITÁ PŘESNOST</span><span class="sxs-lookup"><span data-stu-id="dd4c3-219">DOUBLE PRECISION</span></span> |<span data-ttu-id="dd4c3-220">Double</span><span class="sxs-lookup"><span data-stu-id="dd4c3-220">Double</span></span> |
| <span data-ttu-id="dd4c3-221">LOGICKÁ HODNOTA</span><span class="sxs-lookup"><span data-stu-id="dd4c3-221">BOOLEAN</span></span> |<span data-ttu-id="dd4c3-222">Řetězec</span><span class="sxs-lookup"><span data-stu-id="dd4c3-222">String</span></span> |
| <span data-ttu-id="dd4c3-223">CHAR –</span><span class="sxs-lookup"><span data-stu-id="dd4c3-223">CHAR</span></span> |<span data-ttu-id="dd4c3-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="dd4c3-224">String</span></span> |
| <span data-ttu-id="dd4c3-225">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="dd4c3-225">VARCHAR</span></span> |<span data-ttu-id="dd4c3-226">Řetězec</span><span class="sxs-lookup"><span data-stu-id="dd4c3-226">String</span></span> |
| <span data-ttu-id="dd4c3-227">DATUM</span><span class="sxs-lookup"><span data-stu-id="dd4c3-227">DATE</span></span> |<span data-ttu-id="dd4c3-228">Data a času</span><span class="sxs-lookup"><span data-stu-id="dd4c3-228">DateTime</span></span> |
| <span data-ttu-id="dd4c3-229">ČASOVÉ RAZÍTKO</span><span class="sxs-lookup"><span data-stu-id="dd4c3-229">TIMESTAMP</span></span> |<span data-ttu-id="dd4c3-230">Data a času</span><span class="sxs-lookup"><span data-stu-id="dd4c3-230">DateTime</span></span> |
| <span data-ttu-id="dd4c3-231">TEXT</span><span class="sxs-lookup"><span data-stu-id="dd4c3-231">TEXT</span></span> |<span data-ttu-id="dd4c3-232">Řetězec</span><span class="sxs-lookup"><span data-stu-id="dd4c3-232">String</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="dd4c3-233">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="dd4c3-233">Map source to sink columns</span></span>
<span data-ttu-id="dd4c3-234">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="dd4c3-234">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="dd4c3-235">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="dd4c3-235">Repeatable read from relational sources</span></span>
<span data-ttu-id="dd4c3-236">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-236">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="dd4c3-237">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-237">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="dd4c3-238">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-238">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="dd4c3-239">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-239">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="dd4c3-240">V tématu [Repeatable číst z relační zdrojů](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="dd4c3-240">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="dd4c3-241">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="dd4c3-241">Performance and Tuning</span></span>
<span data-ttu-id="dd4c3-242">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-242">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd4c3-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd4c3-243">Next Steps</span></span>
<span data-ttu-id="dd4c3-244">Viz následující články:</span><span class="sxs-lookup"><span data-stu-id="dd4c3-244">See the following articles:</span></span>

* <span data-ttu-id="dd4c3-245">[Kopie aktivity kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny pro vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="dd4c3-245">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
