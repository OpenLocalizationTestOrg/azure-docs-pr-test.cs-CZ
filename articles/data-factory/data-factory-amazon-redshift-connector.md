---
title: "aaaMove data z Amazonu Redshift pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data z Amazonu Redshift pomocí Azure Data Factory."
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
ms.openlocfilehash: 2a097320734ebdd57282d250f7fdba35741777f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a><span data-ttu-id="1a435-103">Přesun dat z Amazon Redshift pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="1a435-103">Move data From Amazon Redshift using Azure Data Factory</span></span>
<span data-ttu-id="1a435-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="1a435-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from Amazon Redshift.</span></span> <span data-ttu-id="1a435-105">Hello článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-105">hello article builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

<span data-ttu-id="1a435-106">Může kopírovat data z úložiště dat podřízený Amazon Redshift tooany podporována.</span><span class="sxs-lookup"><span data-stu-id="1a435-106">You can copy data from Amazon Redshift tooany supported sink data store.</span></span> <span data-ttu-id="1a435-107">Seznam úložišť dat jako jímky nepodporuje hello aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="1a435-107">For a list of data stores supported as sinks by hello copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="1a435-108">Objekt pro vytváření dat aktuálně podporuje přesunutí dat z úložiště dat tooother Amazon Redshift, ale ne pro přesun dat z jiných dat úložiště tooAmazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="1a435-108">Data factory currently supports moving data from Amazon Redshift tooother data stores, but not for moving data from other data stores tooAmazon Redshift.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a435-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1a435-109">Prerequisites</span></span>
* <span data-ttu-id="1a435-110">Pokud přesouváte data tooan místní data store, nainstalujte [Brána pro správu dat](data-factory-data-management-gateway.md) na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="1a435-110">If you are moving data tooan on-premises data store, install [Data Management Gateway](data-factory-data-management-gateway.md) on an on-premises machine.</span></span> <span data-ttu-id="1a435-111">Potom Grant Brána pro správu dat (použijte IP adresu počítače hello) hello tooAmazon Redshift clusteru přístupu.</span><span class="sxs-lookup"><span data-stu-id="1a435-111">Then, Grant Data Management Gateway (use IP address of hello machine) hello access tooAmazon Redshift cluster.</span></span> <span data-ttu-id="1a435-112">V tématu [autorizovat přístup toohello clusteru](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) pokyny.</span><span class="sxs-lookup"><span data-stu-id="1a435-112">See [Authorize access toohello cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) for instructions.</span></span>
* <span data-ttu-id="1a435-113">Pokud přesouváte data tooan Azure úložišti, přečtěte si téma [rozsahy IP Center dat Azure](https://www.microsoft.com/download/details.aspx?id=41653) pro hello výpočetní IP adresy a rozsahy SQL používané hello datových center Azure.</span><span class="sxs-lookup"><span data-stu-id="1a435-113">If you are moving data tooan Azure data store, see [Azure Data Center IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) for hello Compute IP address and SQL ranges used by hello Azure data centers.</span></span>

## <a name="getting-started"></a><span data-ttu-id="1a435-114">Začínáme</span><span class="sxs-lookup"><span data-stu-id="1a435-114">Getting started</span></span>
<span data-ttu-id="1a435-115">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Amazonu Redshift zdroje pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1a435-115">You can create a pipeline with a copy activity that moves data from an Amazon Redshift source by using different tools/APIs.</span></span>

<span data-ttu-id="1a435-116">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="1a435-116">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="1a435-117">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-117">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="1a435-118">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="1a435-118">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="1a435-119">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="1a435-119">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="1a435-120">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="1a435-120">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="1a435-121">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="1a435-121">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="1a435-122">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="1a435-122">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="1a435-123">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="1a435-123">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="1a435-124">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="1a435-124">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="1a435-125">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-125">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="1a435-126">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat Amazon Redshift, naleznete v tématu [JSON příklad: kopírování dat z Amazon Redshift tooAzure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="1a435-126">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an Amazon Redshift data store, see [JSON example: Copy data from Amazon Redshift tooAzure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="1a435-127">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAmazon Redshift:</span><span class="sxs-lookup"><span data-stu-id="1a435-127">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAmazon Redshift:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="1a435-128">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="1a435-128">Linked service properties</span></span>
<span data-ttu-id="1a435-129">Hello následující tabulka obsahuje popis pro konkrétní tooAmazon elementy JSON Redshift propojené služby.</span><span class="sxs-lookup"><span data-stu-id="1a435-129">hello following table provides description for JSON elements specific tooAmazon Redshift linked service.</span></span>

| <span data-ttu-id="1a435-130">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1a435-130">Property</span></span> | <span data-ttu-id="1a435-131">Popis</span><span class="sxs-lookup"><span data-stu-id="1a435-131">Description</span></span> | <span data-ttu-id="1a435-132">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="1a435-132">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1a435-133">type</span><span class="sxs-lookup"><span data-stu-id="1a435-133">type</span></span> |<span data-ttu-id="1a435-134">vlastnost typu Hello musí být nastavena na: **AmazonRedshift**.</span><span class="sxs-lookup"><span data-stu-id="1a435-134">hello type property must be set to: **AmazonRedshift**.</span></span> |<span data-ttu-id="1a435-135">Ano</span><span class="sxs-lookup"><span data-stu-id="1a435-135">Yes</span></span> |
| <span data-ttu-id="1a435-136">server</span><span class="sxs-lookup"><span data-stu-id="1a435-136">server</span></span> |<span data-ttu-id="1a435-137">IP adresa nebo název hostitele serveru Amazon Redshift hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-137">IP address or host name of hello Amazon Redshift server.</span></span> |<span data-ttu-id="1a435-138">Ano</span><span class="sxs-lookup"><span data-stu-id="1a435-138">Yes</span></span> |
| <span data-ttu-id="1a435-139">port</span><span class="sxs-lookup"><span data-stu-id="1a435-139">port</span></span> |<span data-ttu-id="1a435-140">Hello počet hello port TCP, který hello Amazon Redshift server používá toolisten pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="1a435-140">hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.</span></span> |<span data-ttu-id="1a435-141">Ne, výchozí hodnota: 5439</span><span class="sxs-lookup"><span data-stu-id="1a435-141">No, default value: 5439</span></span> |
| <span data-ttu-id="1a435-142">Databáze</span><span class="sxs-lookup"><span data-stu-id="1a435-142">database</span></span> |<span data-ttu-id="1a435-143">Název databáze Amazon Redshift hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-143">Name of hello Amazon Redshift database.</span></span> |<span data-ttu-id="1a435-144">Ano</span><span class="sxs-lookup"><span data-stu-id="1a435-144">Yes</span></span> |
| <span data-ttu-id="1a435-145">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="1a435-145">username</span></span> |<span data-ttu-id="1a435-146">Jméno uživatele, který má přístup toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="1a435-146">Name of user who has access toohello database.</span></span> |<span data-ttu-id="1a435-147">Ano</span><span class="sxs-lookup"><span data-stu-id="1a435-147">Yes</span></span> |
| <span data-ttu-id="1a435-148">heslo</span><span class="sxs-lookup"><span data-stu-id="1a435-148">password</span></span> |<span data-ttu-id="1a435-149">Heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-149">Password for hello user account.</span></span> |<span data-ttu-id="1a435-150">Ano</span><span class="sxs-lookup"><span data-stu-id="1a435-150">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="1a435-151">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="1a435-151">Dataset properties</span></span>
<span data-ttu-id="1a435-152">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1a435-152">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="1a435-153">Oddíly, jako je například struktura, dostupnost a zásady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="1a435-153">Sections such as structure, availability, and policy are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="1a435-154">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="1a435-154">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="1a435-155">Poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-155">It provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="1a435-156">rámci typeProperties Hello část datové sady typ **RelationalTable** (což zahrnuje datová sada Amazon Redshift) má následující vlastnosti hello</span><span class="sxs-lookup"><span data-stu-id="1a435-156">hello typeProperties section for dataset of type **RelationalTable** (which includes Amazon Redshift dataset) has hello following properties</span></span>

| <span data-ttu-id="1a435-157">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1a435-157">Property</span></span> | <span data-ttu-id="1a435-158">Popis</span><span class="sxs-lookup"><span data-stu-id="1a435-158">Description</span></span> | <span data-ttu-id="1a435-159">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="1a435-159">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1a435-160">tableName</span><span class="sxs-lookup"><span data-stu-id="1a435-160">tableName</span></span> |<span data-ttu-id="1a435-161">Název hello tabulky v databázi hello Amazon Redshift, která propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="1a435-161">Name of hello table in hello Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="1a435-162">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="1a435-162">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="1a435-163">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="1a435-163">Copy activity properties</span></span>
<span data-ttu-id="1a435-164">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1a435-164">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="1a435-165">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="1a435-165">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="1a435-166">Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="1a435-166">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="1a435-167">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="1a435-167">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="1a435-168">Když je zdrojem kopie aktivity typu **RelationalSource** (která zahrnuje Amazon Redshift), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="1a435-168">When source of copy activity is of type **RelationalSource** (which includes Amazon Redshift), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="1a435-169">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1a435-169">Property</span></span> | <span data-ttu-id="1a435-170">Popis</span><span class="sxs-lookup"><span data-stu-id="1a435-170">Description</span></span> | <span data-ttu-id="1a435-171">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="1a435-171">Allowed values</span></span> | <span data-ttu-id="1a435-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="1a435-172">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1a435-173">query</span><span class="sxs-lookup"><span data-stu-id="1a435-173">query</span></span> |<span data-ttu-id="1a435-174">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="1a435-174">Use hello custom query tooread data.</span></span> |<span data-ttu-id="1a435-175">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="1a435-175">SQL query string.</span></span> <span data-ttu-id="1a435-176">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="1a435-176">For example: select * from MyTable.</span></span> |<span data-ttu-id="1a435-177">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="1a435-177">No (if **tableName** of **dataset** is specified)</span></span> |

## <a name="json-example-copy-data-from-amazon-redshift-tooazure-blob"></a><span data-ttu-id="1a435-178">Příklad JSON: kopírování dat z Amazon Redshift tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="1a435-178">JSON example: Copy data from Amazon Redshift tooAzure Blob</span></span>
<span data-ttu-id="1a435-179">Tento příklad ukazuje, jak toocopy data ze Amazon Redshift databáze tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="1a435-179">This sample shows how toocopy data from an Amazon Redshift database tooan Azure Blob Storage.</span></span> <span data-ttu-id="1a435-180">Však můžete zkopírovat data **přímo** tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1a435-180">However, data can be copied **directly** tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="1a435-181">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="1a435-181">hello sample has hello following data factory entities:</span></span>

* <span data-ttu-id="1a435-182">Propojené služby typu [AmazonRedshift](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1a435-182">A linked service of type [AmazonRedshift](#linked-service-properties).</span></span>
* <span data-ttu-id="1a435-183">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1a435-183">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="1a435-184">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="1a435-184">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
* <span data-ttu-id="1a435-185">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="1a435-185">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="1a435-186">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="1a435-186">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span></span>

<span data-ttu-id="1a435-187">Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob tooa Amazon Redshift každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="1a435-187">hello sample copies data from a query result in Amazon Redshift tooa blob every hour.</span></span> <span data-ttu-id="1a435-188">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-188">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="1a435-189">**Amazon Redshift propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="1a435-189">**Amazon Redshift linked service:**</span></span>

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< hello IP address or host name of hello Amazon Redshift server >",
            "port": <hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.>,
            "database": "<hello database name of hello Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="1a435-190">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="1a435-190">**Azure Storage linked service:**</span></span>

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
<span data-ttu-id="1a435-191">**Amazon Redshift vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="1a435-191">**Amazon Redshift input dataset:**</span></span>

<span data-ttu-id="1a435-192">Nastavení `"external": true` informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-192">Setting `"external": true` informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span> <span data-ttu-id="1a435-193">Nastavte tuto vlastnost tootrue vstupní datovou sadu, která není vyprodukované aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="1a435-193">Set this property tootrue on an input dataset that is not produced by an activity in hello pipeline.</span></span>

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

<span data-ttu-id="1a435-194">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="1a435-194">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="1a435-195">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="1a435-195">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="1a435-196">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="1a435-196">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="1a435-197">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="1a435-197">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="1a435-198">**Aktivita kopírování v kanálu se zdrojem Azure Redshift (RelationalSource) a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="1a435-198">**Copy activity in a pipeline with Azure Redshift source (RelationalSource) and Blob sink:**</span></span>

<span data-ttu-id="1a435-199">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="1a435-199">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="1a435-200">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="1a435-200">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="1a435-201">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="1a435-201">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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
### <a name="type-mapping-for-amazon-redshift"></a><span data-ttu-id="1a435-202">Mapování typu pro Amazon Redshift</span><span class="sxs-lookup"><span data-stu-id="1a435-202">Type mapping for Amazon Redshift</span></span>
<span data-ttu-id="1a435-203">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello dvoustupňový přístup následující:</span><span class="sxs-lookup"><span data-stu-id="1a435-203">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="1a435-204">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="1a435-204">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="1a435-205">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1a435-205">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="1a435-206">Při přesunu dat tooAmazon Redshift, se používají hello následující mapování z Amazon Redshift typy too.NET typů.</span><span class="sxs-lookup"><span data-stu-id="1a435-206">When moving data tooAmazon Redshift, hello following mappings are used from Amazon Redshift types too.NET types.</span></span>

| <span data-ttu-id="1a435-207">Typ Redshift Amazon</span><span class="sxs-lookup"><span data-stu-id="1a435-207">Amazon Redshift Type</span></span> | <span data-ttu-id="1a435-208">.NET na základě typu</span><span class="sxs-lookup"><span data-stu-id="1a435-208">.NET Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="1a435-209">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="1a435-209">SMALLINT</span></span> |<span data-ttu-id="1a435-210">Int16</span><span class="sxs-lookup"><span data-stu-id="1a435-210">Int16</span></span> |
| <span data-ttu-id="1a435-211">CELÉ ČÍSLO</span><span class="sxs-lookup"><span data-stu-id="1a435-211">INTEGER</span></span> |<span data-ttu-id="1a435-212">Int32</span><span class="sxs-lookup"><span data-stu-id="1a435-212">Int32</span></span> |
| <span data-ttu-id="1a435-213">BIGINT</span><span class="sxs-lookup"><span data-stu-id="1a435-213">BIGINT</span></span> |<span data-ttu-id="1a435-214">Int64</span><span class="sxs-lookup"><span data-stu-id="1a435-214">Int64</span></span> |
| <span data-ttu-id="1a435-215">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="1a435-215">DECIMAL</span></span> |<span data-ttu-id="1a435-216">Decimal</span><span class="sxs-lookup"><span data-stu-id="1a435-216">Decimal</span></span> |
| <span data-ttu-id="1a435-217">SKUTEČNÉ</span><span class="sxs-lookup"><span data-stu-id="1a435-217">REAL</span></span> |<span data-ttu-id="1a435-218">Jeden</span><span class="sxs-lookup"><span data-stu-id="1a435-218">Single</span></span> |
| <span data-ttu-id="1a435-219">DVOJITÁ PŘESNOST</span><span class="sxs-lookup"><span data-stu-id="1a435-219">DOUBLE PRECISION</span></span> |<span data-ttu-id="1a435-220">Double</span><span class="sxs-lookup"><span data-stu-id="1a435-220">Double</span></span> |
| <span data-ttu-id="1a435-221">LOGICKÁ HODNOTA</span><span class="sxs-lookup"><span data-stu-id="1a435-221">BOOLEAN</span></span> |<span data-ttu-id="1a435-222">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1a435-222">String</span></span> |
| <span data-ttu-id="1a435-223">CHAR –</span><span class="sxs-lookup"><span data-stu-id="1a435-223">CHAR</span></span> |<span data-ttu-id="1a435-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1a435-224">String</span></span> |
| <span data-ttu-id="1a435-225">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="1a435-225">VARCHAR</span></span> |<span data-ttu-id="1a435-226">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1a435-226">String</span></span> |
| <span data-ttu-id="1a435-227">DATUM</span><span class="sxs-lookup"><span data-stu-id="1a435-227">DATE</span></span> |<span data-ttu-id="1a435-228">Data a času</span><span class="sxs-lookup"><span data-stu-id="1a435-228">DateTime</span></span> |
| <span data-ttu-id="1a435-229">ČASOVÉ RAZÍTKO</span><span class="sxs-lookup"><span data-stu-id="1a435-229">TIMESTAMP</span></span> |<span data-ttu-id="1a435-230">Data a času</span><span class="sxs-lookup"><span data-stu-id="1a435-230">DateTime</span></span> |
| <span data-ttu-id="1a435-231">TEXT</span><span class="sxs-lookup"><span data-stu-id="1a435-231">TEXT</span></span> |<span data-ttu-id="1a435-232">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1a435-232">String</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="1a435-233">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="1a435-233">Map source toosink columns</span></span>
<span data-ttu-id="1a435-234">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="1a435-234">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="1a435-235">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="1a435-235">Repeatable read from relational sources</span></span>
<span data-ttu-id="1a435-236">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="1a435-236">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="1a435-237">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="1a435-237">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="1a435-238">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="1a435-238">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="1a435-239">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="1a435-239">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="1a435-240">V tématu [Repeatable číst z relační zdrojů](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="1a435-240">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="1a435-241">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="1a435-241">Performance and Tuning</span></span>
<span data-ttu-id="1a435-242">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="1a435-242">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a435-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a435-243">Next Steps</span></span>
<span data-ttu-id="1a435-244">V tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="1a435-244">See hello following articles:</span></span>

* <span data-ttu-id="1a435-245">[Kopie aktivity kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny pro vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="1a435-245">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
