---
title: "index tooSearch aaaPush dat pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom toopush data tooAzure indexu vyhledávání pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: f2d973d0a2c24d6448e2d59e37e24503aa433018
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="push-data-tooan-azure-search-index-by-using-azure-data-factory"></a><span data-ttu-id="4e78a-103">Push indexu Azure Search tooan dat pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="4e78a-103">Push data tooan Azure Search index by using Azure Data Factory</span></span>
<span data-ttu-id="4e78a-104">Tento článek popisuje, jak úložiště toouse hello aktivity kopírování toopush dat z podporovaných zdrojových dat tooAzure indexu vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4e78a-104">This article describes how toouse hello Copy Activity toopush data from a supported source data store tooAzure Search index.</span></span> <span data-ttu-id="4e78a-105">Podporované zdrojové úložiště dat jsou uvedené v hello zdrojový sloupec %{sourceproperty/ hello [podporované zdroje a jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e78a-105">Supported source data stores are listed in hello Source column of hello [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="4e78a-106">Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který uvádí obecný přehled přesun dat s aktivitou kopírování a kombinace podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="4e78a-106">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="4e78a-107">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="4e78a-107">Enabling connectivity</span></span>
<span data-ttu-id="4e78a-108">tooallow služba Data Factory připojení tooan místní data store, nainstalujte Brána pro správu dat ve vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e78a-108">tooallow Data Factory service connect tooan on-premises data store, you install Data Management Gateway in your on-premises environment.</span></span> <span data-ttu-id="4e78a-109">Bránu můžete nainstalovat na hello stejný počítač, který je hostitelem hello zdrojová data uložit nebo na samostatný počítač tooavoid, neslučitelných pro prostředky s hello data uložit.</span><span class="sxs-lookup"><span data-stu-id="4e78a-109">You can install gateway on hello same machine that hosts hello source data store or on a separate machine tooavoid competing for resources with hello data store.</span></span>

<span data-ttu-id="4e78a-110">Brána pro správu dat se připojuje místní datové zdroje toocloud služby způsobem, zabezpečení a správě.</span><span class="sxs-lookup"><span data-stu-id="4e78a-110">Data Management Gateway connects on-premises data sources toocloud services in a secure and managed way.</span></span> <span data-ttu-id="4e78a-111">V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) podrobnosti o Brána pro správu dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="4e78a-111">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="4e78a-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="4e78a-112">Getting started</span></span>
<span data-ttu-id="4e78a-113">Vytvoření kanálu s aktivitou kopírování, který by vložil data ze zdroje dat úložiště tooAzure vyhledávání indexu pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e78a-113">You can create a pipeline with a copy activity that pushes data from a source data store tooAzure Search index by using different tools/APIs.</span></span>

<span data-ttu-id="4e78a-114">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="4e78a-114">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="4e78a-115">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="4e78a-115">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="4e78a-116">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="4e78a-116">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="4e78a-117">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="4e78a-117">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="4e78a-118">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="4e78a-118">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="4e78a-119">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="4e78a-119">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="4e78a-120">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="4e78a-120">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="4e78a-121">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="4e78a-121">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="4e78a-122">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="4e78a-122">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="4e78a-123">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="4e78a-123">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="4e78a-124">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data tooAzure vyhledávání index, naleznete v tématu [JSON příklad: kopírování dat z indexu vyhledávání tooAzure místní systém SQL Server](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4e78a-124">For a sample with JSON definitions for Data Factory entities that are used toocopy data tooAzure Search index, see [JSON example: Copy data from on-premises SQL Server tooAzure Search index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) section of this article.</span></span> 

<span data-ttu-id="4e78a-125">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAzure indexu vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="4e78a-125">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure Search Index:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="4e78a-126">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="4e78a-126">Linked service properties</span></span>

<span data-ttu-id="4e78a-127">Hello následující tabulka obsahuje popis JSON prvky, které jsou konkrétní toohello Azure Search propojené služby.</span><span class="sxs-lookup"><span data-stu-id="4e78a-127">hello following table provides descriptions for JSON elements that are specific toohello Azure Search linked service.</span></span>

| <span data-ttu-id="4e78a-128">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4e78a-128">Property</span></span> | <span data-ttu-id="4e78a-129">Popis</span><span class="sxs-lookup"><span data-stu-id="4e78a-129">Description</span></span> | <span data-ttu-id="4e78a-130">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4e78a-130">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="4e78a-131">type</span><span class="sxs-lookup"><span data-stu-id="4e78a-131">type</span></span> | <span data-ttu-id="4e78a-132">vlastnost typu Hello musí být nastavena na: **AzureSearch**.</span><span class="sxs-lookup"><span data-stu-id="4e78a-132">hello type property must be set to: **AzureSearch**.</span></span> | <span data-ttu-id="4e78a-133">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-133">Yes</span></span> |
| <span data-ttu-id="4e78a-134">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="4e78a-134">url</span></span> | <span data-ttu-id="4e78a-135">Adresa URL pro hello služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4e78a-135">URL for hello Azure Search service.</span></span> | <span data-ttu-id="4e78a-136">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-136">Yes</span></span> |
| <span data-ttu-id="4e78a-137">key</span><span class="sxs-lookup"><span data-stu-id="4e78a-137">key</span></span> | <span data-ttu-id="4e78a-138">Klíč správce pro hello služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4e78a-138">Admin key for hello Azure Search service.</span></span> | <span data-ttu-id="4e78a-139">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-139">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="4e78a-140">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="4e78a-140">Dataset properties</span></span>

<span data-ttu-id="4e78a-141">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4e78a-141">For a full list of sections and properties that are available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="4e78a-142">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="4e78a-142">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span> <span data-ttu-id="4e78a-143">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="4e78a-143">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="4e78a-144">rámci typeProperties Hello část datové sady hello typu **AzureSearchIndex** má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="4e78a-144">hello typeProperties section for a dataset of hello type **AzureSearchIndex** has hello following properties:</span></span>

| <span data-ttu-id="4e78a-145">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4e78a-145">Property</span></span> | <span data-ttu-id="4e78a-146">Popis</span><span class="sxs-lookup"><span data-stu-id="4e78a-146">Description</span></span> | <span data-ttu-id="4e78a-147">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4e78a-147">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="4e78a-148">type</span><span class="sxs-lookup"><span data-stu-id="4e78a-148">type</span></span> | <span data-ttu-id="4e78a-149">musí být nastavena vlastnost typu Hello příliš**AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="4e78a-149">hello type property must be set too**AzureSearchIndex**.</span></span>| <span data-ttu-id="4e78a-150">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-150">Yes</span></span> |
| <span data-ttu-id="4e78a-151">indexName</span><span class="sxs-lookup"><span data-stu-id="4e78a-151">indexName</span></span> | <span data-ttu-id="4e78a-152">Název indexu Azure Search hello.</span><span class="sxs-lookup"><span data-stu-id="4e78a-152">Name of hello Azure Search index.</span></span> <span data-ttu-id="4e78a-153">Objekt pro vytváření dat nevytváří hello index.</span><span class="sxs-lookup"><span data-stu-id="4e78a-153">Data Factory does not create hello index.</span></span> <span data-ttu-id="4e78a-154">Hello index musí existovat ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4e78a-154">hello index must exist in Azure Search.</span></span> | <span data-ttu-id="4e78a-155">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-155">Yes</span></span> |


## <a name="copy-activity-properties"></a><span data-ttu-id="4e78a-156">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="4e78a-156">Copy activity properties</span></span>
<span data-ttu-id="4e78a-157">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4e78a-157">For a full list of sections and properties that are available for defining activities, see hello [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="4e78a-158">Vlastnosti, například název, popis, vstupní a výstupní tabulky a různé zásady jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="4e78a-158">Properties such as name, description, input and output tables, and various policies are available for all types of activities.</span></span> <span data-ttu-id="4e78a-159">Zatímco se každý typ aktivity lišit podle vlastnosti dostupné v rámci typeProperties části hello.</span><span class="sxs-lookup"><span data-stu-id="4e78a-159">Whereas, properties available in hello typeProperties section vary with each activity type.</span></span> <span data-ttu-id="4e78a-160">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="4e78a-160">For Copy Activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="4e78a-161">Pro aktivitu kopírování, po hello podřízený typu hello **AzureSearchIndexSink**, hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="4e78a-161">For Copy Activity, when hello sink is of hello type **AzureSearchIndexSink**, hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="4e78a-162">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4e78a-162">Property</span></span> | <span data-ttu-id="4e78a-163">Popis</span><span class="sxs-lookup"><span data-stu-id="4e78a-163">Description</span></span> | <span data-ttu-id="4e78a-164">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="4e78a-164">Allowed values</span></span> | <span data-ttu-id="4e78a-165">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4e78a-165">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="4e78a-166">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="4e78a-166">WriteBehavior</span></span> | <span data-ttu-id="4e78a-167">Určuje, zda toomerge nebo nahradit, když dokumentu již existuje v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="4e78a-167">Specifies whether toomerge or replace when a document already exists in hello index.</span></span> <span data-ttu-id="4e78a-168">V tématu hello [WriteBehavior vlastnost](#writebehavior-property).</span><span class="sxs-lookup"><span data-stu-id="4e78a-168">See hello [WriteBehavior property](#writebehavior-property).</span></span>| <span data-ttu-id="4e78a-169">Merge (výchozí)</span><span class="sxs-lookup"><span data-stu-id="4e78a-169">Merge (default)</span></span><br/><span data-ttu-id="4e78a-170">Odeslat</span><span class="sxs-lookup"><span data-stu-id="4e78a-170">Upload</span></span>| <span data-ttu-id="4e78a-171">Ne</span><span class="sxs-lookup"><span data-stu-id="4e78a-171">No</span></span> |
| <span data-ttu-id="4e78a-172">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="4e78a-172">WriteBatchSize</span></span> | <span data-ttu-id="4e78a-173">Ukládání dat do indexu Azure Search hello, když velikost vyrovnávací paměti hello dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="4e78a-173">Uploads data into hello Azure Search index when hello buffer size reaches writeBatchSize.</span></span> <span data-ttu-id="4e78a-174">V tématu hello [WriteBatchSize vlastnost](#writebatchsize-property) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4e78a-174">See hello [WriteBatchSize property](#writebatchsize-property) for details.</span></span> | <span data-ttu-id="4e78a-175">1 too1 000.</span><span class="sxs-lookup"><span data-stu-id="4e78a-175">1 too1,000.</span></span> <span data-ttu-id="4e78a-176">Výchozí hodnota je 1 000.</span><span class="sxs-lookup"><span data-stu-id="4e78a-176">Default value is 1000.</span></span> | <span data-ttu-id="4e78a-177">Ne</span><span class="sxs-lookup"><span data-stu-id="4e78a-177">No</span></span> |

### <a name="writebehavior-property"></a><span data-ttu-id="4e78a-178">Vlastnost WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="4e78a-178">WriteBehavior property</span></span>
<span data-ttu-id="4e78a-179">Při zápisu dat AzureSearchSink upserts.</span><span class="sxs-lookup"><span data-stu-id="4e78a-179">AzureSearchSink upserts when writing data.</span></span> <span data-ttu-id="4e78a-180">Jinými slovy při zápisu dokumentu, pokud klíč dokumentu hello již existuje v indexu Azure Search hello, Azure Search aktualizuje stávající dokument hello místo vyvolání k výjimce konflikt.</span><span class="sxs-lookup"><span data-stu-id="4e78a-180">In other words, when writing a document, if hello document key already exists in hello Azure Search index, Azure Search updates hello existing document rather than throwing a conflict exception.</span></span>

<span data-ttu-id="4e78a-181">Hello AzureSearchSink poskytuje hello následující dvě upsert chování (pomocí AzureSearch SDK):</span><span class="sxs-lookup"><span data-stu-id="4e78a-181">hello AzureSearchSink provides hello following two upsert behaviors (by using AzureSearch SDK):</span></span>

- <span data-ttu-id="4e78a-182">**Sloučení**: kombinovat všechny sloupce hello v hello nový dokument s hello existující jeden.</span><span class="sxs-lookup"><span data-stu-id="4e78a-182">**Merge**: combine all hello columns in hello new document with hello existing one.</span></span> <span data-ttu-id="4e78a-183">Pro sloupce s hodnotou null v hello nový dokument je hodnota hello v hello existující jeden zachovaná.</span><span class="sxs-lookup"><span data-stu-id="4e78a-183">For columns with null value in hello new document, hello value in hello existing one is preserved.</span></span>
- <span data-ttu-id="4e78a-184">**Nahrát**: hello hello nový dokument nahradí existující.</span><span class="sxs-lookup"><span data-stu-id="4e78a-184">**Upload**: hello new document replaces hello existing one.</span></span> <span data-ttu-id="4e78a-185">Pro sloupce, které nebyly zadány v hello nový dokument hello hodnota je nastavena toonull, zda je jinou hodnotu než null v hello existujícího dokumentu nebo ne.</span><span class="sxs-lookup"><span data-stu-id="4e78a-185">For columns not specified in hello new document, hello value is set toonull whether there is a non-null value in hello existing document or not.</span></span>

<span data-ttu-id="4e78a-186">Hello výchozí chování je **sloučení**.</span><span class="sxs-lookup"><span data-stu-id="4e78a-186">hello default behavior is **Merge**.</span></span>

### <a name="writebatchsize-property"></a><span data-ttu-id="4e78a-187">Vlastnost WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="4e78a-187">WriteBatchSize Property</span></span>
<span data-ttu-id="4e78a-188">Služba Azure Search podporuje zápis dokumentů jako dávku.</span><span class="sxs-lookup"><span data-stu-id="4e78a-188">Azure Search service supports writing documents as a batch.</span></span> <span data-ttu-id="4e78a-189">Batch může obsahovat 1 too1 000 akce.</span><span class="sxs-lookup"><span data-stu-id="4e78a-189">A batch can contain 1 too1,000 Actions.</span></span> <span data-ttu-id="4e78a-190">Akce zpracovává jeden dokument tooperform hello nahrávání či sloučení operaci.</span><span class="sxs-lookup"><span data-stu-id="4e78a-190">An action handles one document tooperform hello upload/merge operation.</span></span>

### <a name="data-type-support"></a><span data-ttu-id="4e78a-191">Podpora datového typu</span><span class="sxs-lookup"><span data-stu-id="4e78a-191">Data type support</span></span>
<span data-ttu-id="4e78a-192">Hello následující tabulka určuje, zda typ dat. Azure Search je podporováno, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="4e78a-192">hello following table specifies whether an Azure Search data type is supported or not.</span></span>

| <span data-ttu-id="4e78a-193">Azure Search datový typ</span><span class="sxs-lookup"><span data-stu-id="4e78a-193">Azure Search data type</span></span> | <span data-ttu-id="4e78a-194">Podporované ve službě Azure Search jímka</span><span class="sxs-lookup"><span data-stu-id="4e78a-194">Supported in Azure Search Sink</span></span> |
| ---------------------- | ------------------------------ |
| <span data-ttu-id="4e78a-195">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e78a-195">String</span></span> | <span data-ttu-id="4e78a-196">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-196">Y</span></span> |
| <span data-ttu-id="4e78a-197">Int32</span><span class="sxs-lookup"><span data-stu-id="4e78a-197">Int32</span></span> | <span data-ttu-id="4e78a-198">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-198">Y</span></span> |
| <span data-ttu-id="4e78a-199">Int64</span><span class="sxs-lookup"><span data-stu-id="4e78a-199">Int64</span></span> | <span data-ttu-id="4e78a-200">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-200">Y</span></span> |
| <span data-ttu-id="4e78a-201">Double</span><span class="sxs-lookup"><span data-stu-id="4e78a-201">Double</span></span> | <span data-ttu-id="4e78a-202">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-202">Y</span></span> |
| <span data-ttu-id="4e78a-203">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4e78a-203">Boolean</span></span> | <span data-ttu-id="4e78a-204">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-204">Y</span></span> |
| <span data-ttu-id="4e78a-205">DataTimeOffset</span><span class="sxs-lookup"><span data-stu-id="4e78a-205">DataTimeOffset</span></span> | <span data-ttu-id="4e78a-206">Ano</span><span class="sxs-lookup"><span data-stu-id="4e78a-206">Y</span></span> |
| <span data-ttu-id="4e78a-207">Pole řetězců</span><span class="sxs-lookup"><span data-stu-id="4e78a-207">String Array</span></span> | <span data-ttu-id="4e78a-208">N</span><span class="sxs-lookup"><span data-stu-id="4e78a-208">N</span></span> |
| <span data-ttu-id="4e78a-209">GeographyPoint</span><span class="sxs-lookup"><span data-stu-id="4e78a-209">GeographyPoint</span></span> | <span data-ttu-id="4e78a-210">N</span><span class="sxs-lookup"><span data-stu-id="4e78a-210">N</span></span> |

## <a name="json-example-copy-data-from-on-premises-sql-server-tooazure-search-index"></a><span data-ttu-id="4e78a-211">Příklad JSON: kopírování dat z indexu vyhledávání tooAzure místní systém SQL Server</span><span class="sxs-lookup"><span data-stu-id="4e78a-211">JSON example: Copy data from on-premises SQL Server tooAzure Search index</span></span>

<span data-ttu-id="4e78a-212">Následující ukázka ukazuje Hello:</span><span class="sxs-lookup"><span data-stu-id="4e78a-212">hello following sample shows:</span></span>

1.  <span data-ttu-id="4e78a-213">Propojené služby typu [AzureSearch](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4e78a-213">A linked service of type [AzureSearch](#linked-service-properties).</span></span>
2.  <span data-ttu-id="4e78a-214">Propojené služby typu [onpremisessqlserver](data-factory-sqlserver-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4e78a-214">A linked service of type [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).</span></span>
3.  <span data-ttu-id="4e78a-215">Vstup [datovou sadu](data-factory-create-datasets.md) typu [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4e78a-215">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
4.  <span data-ttu-id="4e78a-216">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSearchIndex](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4e78a-216">An output [dataset](data-factory-create-datasets.md) of type [AzureSearchIndex](#dataset-properties).</span></span>
4.  <span data-ttu-id="4e78a-217">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) a [AzureSearchIndexSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="4e78a-217">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) and [AzureSearchIndexSink](#copy-activity-properties).</span></span>

<span data-ttu-id="4e78a-218">Ukázka Hello kopíruje data časové řady každou hodinu z indexu Azure Search tooan databáze místní systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4e78a-218">hello sample copies time-series data from an on-premises SQL Server database tooan Azure Search index hourly.</span></span> <span data-ttu-id="4e78a-219">Hello vlastnostech JSON použitých v této ukázce jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="4e78a-219">hello JSON properties used in this sample are described in sections following hello samples.</span></span>

<span data-ttu-id="4e78a-220">Jako první krok instalační program hello Brána pro správu dat na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="4e78a-220">As a first step, setup hello data management gateway on your on-premises machine.</span></span> <span data-ttu-id="4e78a-221">Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4e78a-221">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="4e78a-222">**Azure Search propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="4e78a-222">**Azure Search linked service:**</span></span>

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

<span data-ttu-id="4e78a-223">**Služba SQL Server propojené**</span><span class="sxs-lookup"><span data-stu-id="4e78a-223">**SQL Server linked service**</span></span>

```JSON
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```

<span data-ttu-id="4e78a-224">**Vstupní datové sady SQL Server**</span><span class="sxs-lookup"><span data-stu-id="4e78a-224">**SQL Server input dataset**</span></span>

<span data-ttu-id="4e78a-225">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v systému SQL Server a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="4e78a-225">hello sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="4e78a-226">Můžete dotazovat přes více tabulek v rámci hello stejnou databázi pomocí jednu datovou sadu, ale jedné tabulky musí být použita pro typeProperty tableName hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="4e78a-226">You can query over multiple tables within hello same database using a single dataset, but a single table must be used for hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="4e78a-227">Nastavení "externí": "PRAVDA" informuje služba Data Factory tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="4e78a-227">Setting “external”: ”true” informs Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "SqlServerDataset",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
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

<span data-ttu-id="4e78a-228">**Služba Azure Search výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="4e78a-228">**Azure Search output dataset:**</span></span>

<span data-ttu-id="4e78a-229">Hello ukázka kopie dat tooan indexu Azure Search s názvem **produkty**.</span><span class="sxs-lookup"><span data-stu-id="4e78a-229">hello sample copies data tooan Azure Search index named **products**.</span></span> <span data-ttu-id="4e78a-230">Objekt pro vytváření dat nevytváří hello index.</span><span class="sxs-lookup"><span data-stu-id="4e78a-230">Data Factory does not create hello index.</span></span> <span data-ttu-id="4e78a-231">tootest hello ukázkové, vytvořte index s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="4e78a-231">tootest hello sample, create an index with this name.</span></span> <span data-ttu-id="4e78a-232">Vytvoření indexu Azure Search hello s hello stejný počet sloupců jako vstupní datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="4e78a-232">Create hello Azure Search index with hello same number of columns as in hello input dataset.</span></span> <span data-ttu-id="4e78a-233">Nové položky se přidají indexu Azure Search toohello každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4e78a-233">New entries are added toohello Azure Search index every hour.</span></span>

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
   }
}
```

<span data-ttu-id="4e78a-234">**Aktivita kopírování v kanálu s SQL zdroj a jímka indexu Azure Search:**</span><span class="sxs-lookup"><span data-stu-id="4e78a-234">**Copy activity in a pipeline with SQL source and Azure Search Index sink:**</span></span>

<span data-ttu-id="4e78a-235">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4e78a-235">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="4e78a-236">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlSource** a **podřízený** je typ nastaven příliš**AzureSearchIndexSink**.</span><span class="sxs-lookup"><span data-stu-id="4e78a-236">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**AzureSearchIndexSink**.</span></span> <span data-ttu-id="4e78a-237">Dotaz SQL Hello zadaný pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="4e78a-237">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

<span data-ttu-id="4e78a-238">Pokud kopírujete data z jiného úložiště dat cloudu Azure Search, `executionLocation` vlastnost je povinná.</span><span class="sxs-lookup"><span data-stu-id="4e78a-238">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="4e78a-239">Hello následující fragment kódu JSON ukazuje změnu hello potřeba v rámci aktivity kopírování `typeProperties` jako příklad.</span><span class="sxs-lookup"><span data-stu-id="4e78a-239">hello following JSON snippet shows hello change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="4e78a-240">Zkontrolujte [kopírování dat mezi úložišti dat cloudu](data-factory-data-movement-activities.md#global) části Podporované hodnoty a další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4e78a-240">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a><span data-ttu-id="4e78a-241">Kopírování ze zdrojového cloudu</span><span class="sxs-lookup"><span data-stu-id="4e78a-241">Copy from a cloud source</span></span>
<span data-ttu-id="4e78a-242">Pokud kopírujete data z jiného úložiště dat cloudu Azure Search, `executionLocation` vlastnost je povinná.</span><span class="sxs-lookup"><span data-stu-id="4e78a-242">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="4e78a-243">Hello následující fragment kódu JSON ukazuje změnu hello potřeba v rámci aktivity kopírování `typeProperties` jako příklad.</span><span class="sxs-lookup"><span data-stu-id="4e78a-243">hello following JSON snippet shows hello change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="4e78a-244">Zkontrolujte [kopírování dat mezi úložišti dat cloudu](data-factory-data-movement-activities.md#global) části Podporované hodnoty a další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4e78a-244">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

<span data-ttu-id="4e78a-245">Můžete také mapovat sloupců z toocolumns datové sady zdroje z podřízený datovou sadu v definici aktivity kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="4e78a-245">You can also map columns from source dataset toocolumns from sink dataset in hello copy activity definition.</span></span> <span data-ttu-id="4e78a-246">Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="4e78a-246">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="4e78a-247">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="4e78a-247">Performance and tuning</span></span>  
<span data-ttu-id="4e78a-248">V tématu hello [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="4e78a-248">See hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e78a-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e78a-249">Next steps</span></span>
<span data-ttu-id="4e78a-250">V tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="4e78a-250">See hello following articles:</span></span>

* <span data-ttu-id="4e78a-251">[Kopie aktivity kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny pro vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="4e78a-251">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
