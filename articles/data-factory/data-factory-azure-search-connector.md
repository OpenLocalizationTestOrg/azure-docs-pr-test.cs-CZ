---
title: "Nabízet data do indexu vyhledávání pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom, jak nabízet data do indexu Azure Search pomocí Azure Data Factory."
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
ms.openlocfilehash: 5c617c7a2f2eb4da2164ce5f802354a64dfd1fa1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="push-data-to-an-azure-search-index-by-using-azure-data-factory"></a><span data-ttu-id="89503-103">Zápis dat do indexu Azure Search pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="89503-103">Push data to an Azure Search index by using Azure Data Factory</span></span>
<span data-ttu-id="89503-104">Tento článek popisuje, jak pomocí aktivity kopírování a nabízí data z podporované zdrojové úložiště dat do indexu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="89503-104">This article describes how to use the Copy Activity to push data from a supported source data store to Azure Search index.</span></span> <span data-ttu-id="89503-105">Podporované zdrojové úložiště dat jsou uvedena ve sloupci zdroj z [podporované zdroje a jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="89503-105">Supported source data stores are listed in the Source column of the [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="89503-106">Tento článek vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který uvádí obecný přehled přesun dat s aktivitou kopírování a kombinace podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="89503-106">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="89503-107">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="89503-107">Enabling connectivity</span></span>
<span data-ttu-id="89503-108">Povolit služby připojit k úložišti dat místní služby Data Factory, nainstalujete Brána pro správu dat ve vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="89503-108">To allow Data Factory service connect to an on-premises data store, you install Data Management Gateway in your on-premises environment.</span></span> <span data-ttu-id="89503-109">Můžete bránu nainstalovat na stejný počítač, který hostitelů zdrojová data uložit nebo na samostatný počítač, aby se zabránilo neslučitelných pro prostředky s úložištěm dat.</span><span class="sxs-lookup"><span data-stu-id="89503-109">You can install gateway on the same machine that hosts the source data store or on a separate machine to avoid competing for resources with the data store.</span></span>

<span data-ttu-id="89503-110">Brána pro správu dat připojuje místní zdroje dat do cloudové služby v zabezpečený a spravovaný.</span><span class="sxs-lookup"><span data-stu-id="89503-110">Data Management Gateway connects on-premises data sources to cloud services in a secure and managed way.</span></span> <span data-ttu-id="89503-111">V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) podrobnosti o Brána pro správu dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="89503-111">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="89503-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="89503-112">Getting started</span></span>
<span data-ttu-id="89503-113">Vytvoření kanálu s aktivitou kopírování, který by vložil data ze zdrojového úložiště dat do indexu Azure Search pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="89503-113">You can create a pipeline with a copy activity that pushes data from a source data store to Azure Search index by using different tools/APIs.</span></span>

<span data-ttu-id="89503-114">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="89503-114">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="89503-115">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="89503-115">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="89503-116">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="89503-116">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="89503-117">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="89503-117">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="89503-118">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="89503-118">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="89503-119">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="89503-119">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="89503-120">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="89503-120">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="89503-121">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="89503-121">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="89503-122">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="89503-122">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="89503-123">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="89503-123">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="89503-124">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat do indexu Azure Search, naleznete v tématu [JSON příklad: kopírování dat z místního SQL serveru do indexu Azure Search](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="89503-124">For a sample with JSON definitions for Data Factory entities that are used to copy data to Azure Search index, see [JSON example: Copy data from on-premises SQL Server to Azure Search index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) section of this article.</span></span> 

<span data-ttu-id="89503-125">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení entit služby Data Factory konkrétní do indexu Azure Search:</span><span class="sxs-lookup"><span data-stu-id="89503-125">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Azure Search Index:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="89503-126">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="89503-126">Linked service properties</span></span>

<span data-ttu-id="89503-127">Následující tabulka obsahuje popis elementy JSON, které jsou specifické pro službu Azure Search propojený.</span><span class="sxs-lookup"><span data-stu-id="89503-127">The following table provides descriptions for JSON elements that are specific to the Azure Search linked service.</span></span>

| <span data-ttu-id="89503-128">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="89503-128">Property</span></span> | <span data-ttu-id="89503-129">Popis</span><span class="sxs-lookup"><span data-stu-id="89503-129">Description</span></span> | <span data-ttu-id="89503-130">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="89503-130">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="89503-131">type</span><span class="sxs-lookup"><span data-stu-id="89503-131">type</span></span> | <span data-ttu-id="89503-132">Vlastnost typu musí být nastavena na: **AzureSearch**.</span><span class="sxs-lookup"><span data-stu-id="89503-132">The type property must be set to: **AzureSearch**.</span></span> | <span data-ttu-id="89503-133">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-133">Yes</span></span> |
| <span data-ttu-id="89503-134">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="89503-134">url</span></span> | <span data-ttu-id="89503-135">Adresa URL pro službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="89503-135">URL for the Azure Search service.</span></span> | <span data-ttu-id="89503-136">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-136">Yes</span></span> |
| <span data-ttu-id="89503-137">key</span><span class="sxs-lookup"><span data-stu-id="89503-137">key</span></span> | <span data-ttu-id="89503-138">Klíč správce pro službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="89503-138">Admin key for the Azure Search service.</span></span> | <span data-ttu-id="89503-139">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-139">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="89503-140">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="89503-140">Dataset properties</span></span>

<span data-ttu-id="89503-141">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="89503-141">For a full list of sections and properties that are available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="89503-142">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="89503-142">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span> <span data-ttu-id="89503-143">**Rámci typeProperties** části se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="89503-143">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="89503-144">Rámci typeProperties část datové sady typu **AzureSearchIndex** má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="89503-144">The typeProperties section for a dataset of the type **AzureSearchIndex** has the following properties:</span></span>

| <span data-ttu-id="89503-145">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="89503-145">Property</span></span> | <span data-ttu-id="89503-146">Popis</span><span class="sxs-lookup"><span data-stu-id="89503-146">Description</span></span> | <span data-ttu-id="89503-147">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="89503-147">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="89503-148">type</span><span class="sxs-lookup"><span data-stu-id="89503-148">type</span></span> | <span data-ttu-id="89503-149">Vlastnost typu musí být nastavená na **AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="89503-149">The type property must be set to **AzureSearchIndex**.</span></span>| <span data-ttu-id="89503-150">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-150">Yes</span></span> |
| <span data-ttu-id="89503-151">indexName</span><span class="sxs-lookup"><span data-stu-id="89503-151">indexName</span></span> | <span data-ttu-id="89503-152">Název indexu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="89503-152">Name of the Azure Search index.</span></span> <span data-ttu-id="89503-153">Objekt pro vytváření dat vytvořit index.</span><span class="sxs-lookup"><span data-stu-id="89503-153">Data Factory does not create the index.</span></span> <span data-ttu-id="89503-154">Index musí existovat ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="89503-154">The index must exist in Azure Search.</span></span> | <span data-ttu-id="89503-155">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-155">Yes</span></span> |


## <a name="copy-activity-properties"></a><span data-ttu-id="89503-156">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="89503-156">Copy activity properties</span></span>
<span data-ttu-id="89503-157">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="89503-157">For a full list of sections and properties that are available for defining activities, see the [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="89503-158">Vlastnosti, například název, popis, vstupní a výstupní tabulky a různé zásady jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="89503-158">Properties such as name, description, input and output tables, and various policies are available for all types of activities.</span></span> <span data-ttu-id="89503-159">Zatímco se každý typ aktivity lišit podle vlastnosti dostupné v rámci typeProperties oddílu.</span><span class="sxs-lookup"><span data-stu-id="89503-159">Whereas, properties available in the typeProperties section vary with each activity type.</span></span> <span data-ttu-id="89503-160">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="89503-160">For Copy Activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="89503-161">Pro aktivitu kopírování, když je typ jímky **AzureSearchIndexSink**, následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="89503-161">For Copy Activity, when the sink is of the type **AzureSearchIndexSink**, the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="89503-162">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="89503-162">Property</span></span> | <span data-ttu-id="89503-163">Popis</span><span class="sxs-lookup"><span data-stu-id="89503-163">Description</span></span> | <span data-ttu-id="89503-164">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="89503-164">Allowed values</span></span> | <span data-ttu-id="89503-165">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="89503-165">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="89503-166">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="89503-166">WriteBehavior</span></span> | <span data-ttu-id="89503-167">Určuje, jestli se má sloučit nebo nahradit, pokud již dokument v indexu existuje.</span><span class="sxs-lookup"><span data-stu-id="89503-167">Specifies whether to merge or replace when a document already exists in the index.</span></span> <span data-ttu-id="89503-168">Najdete v článku [WriteBehavior vlastnost](#writebehavior-property).</span><span class="sxs-lookup"><span data-stu-id="89503-168">See the [WriteBehavior property](#writebehavior-property).</span></span>| <span data-ttu-id="89503-169">Merge (výchozí)</span><span class="sxs-lookup"><span data-stu-id="89503-169">Merge (default)</span></span><br/><span data-ttu-id="89503-170">Odeslat</span><span class="sxs-lookup"><span data-stu-id="89503-170">Upload</span></span>| <span data-ttu-id="89503-171">Ne</span><span class="sxs-lookup"><span data-stu-id="89503-171">No</span></span> |
| <span data-ttu-id="89503-172">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="89503-172">WriteBatchSize</span></span> | <span data-ttu-id="89503-173">Ukládání dat do indexu Azure Search, když velikost vyrovnávací paměti dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="89503-173">Uploads data into the Azure Search index when the buffer size reaches writeBatchSize.</span></span> <span data-ttu-id="89503-174">Najdete v článku [WriteBatchSize vlastnost](#writebatchsize-property) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="89503-174">See the [WriteBatchSize property](#writebatchsize-property) for details.</span></span> | <span data-ttu-id="89503-175">1 do 1000.</span><span class="sxs-lookup"><span data-stu-id="89503-175">1 to 1,000.</span></span> <span data-ttu-id="89503-176">Výchozí hodnota je 1 000.</span><span class="sxs-lookup"><span data-stu-id="89503-176">Default value is 1000.</span></span> | <span data-ttu-id="89503-177">Ne</span><span class="sxs-lookup"><span data-stu-id="89503-177">No</span></span> |

### <a name="writebehavior-property"></a><span data-ttu-id="89503-178">Vlastnost WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="89503-178">WriteBehavior property</span></span>
<span data-ttu-id="89503-179">Při zápisu dat AzureSearchSink upserts.</span><span class="sxs-lookup"><span data-stu-id="89503-179">AzureSearchSink upserts when writing data.</span></span> <span data-ttu-id="89503-180">Jinými slovy při zápisu dokumentu, pokud klíč dokumentu již existuje v indexu Azure Search, Azure Search aktualizuje stávající dokument místo vyvolání k výjimce konflikt.</span><span class="sxs-lookup"><span data-stu-id="89503-180">In other words, when writing a document, if the document key already exists in the Azure Search index, Azure Search updates the existing document rather than throwing a conflict exception.</span></span>

<span data-ttu-id="89503-181">AzureSearchSink poskytuje následujících dvou upsert chování (pomocí AzureSearch SDK):</span><span class="sxs-lookup"><span data-stu-id="89503-181">The AzureSearchSink provides the following two upsert behaviors (by using AzureSearch SDK):</span></span>

- <span data-ttu-id="89503-182">**Sloučení**: kombinovat všechny sloupce v nový dokument s existujícím.</span><span class="sxs-lookup"><span data-stu-id="89503-182">**Merge**: combine all the columns in the new document with the existing one.</span></span> <span data-ttu-id="89503-183">Pro sloupce s hodnotou null v novém dokumentu je hodnota v existujícím zachovaná.</span><span class="sxs-lookup"><span data-stu-id="89503-183">For columns with null value in the new document, the value in the existing one is preserved.</span></span>
- <span data-ttu-id="89503-184">**Nahrát**: nový dokument nahrazuje stávající.</span><span class="sxs-lookup"><span data-stu-id="89503-184">**Upload**: The new document replaces the existing one.</span></span> <span data-ttu-id="89503-185">Pro sloupce, které nebyly zadány v nový dokument je hodnota nastavena na hodnotu null, zda je jinou hodnotu než null v existujícího dokumentu nebo ne.</span><span class="sxs-lookup"><span data-stu-id="89503-185">For columns not specified in the new document, the value is set to null whether there is a non-null value in the existing document or not.</span></span>

<span data-ttu-id="89503-186">Výchozí chování je **sloučení**.</span><span class="sxs-lookup"><span data-stu-id="89503-186">The default behavior is **Merge**.</span></span>

### <a name="writebatchsize-property"></a><span data-ttu-id="89503-187">Vlastnost WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="89503-187">WriteBatchSize Property</span></span>
<span data-ttu-id="89503-188">Služba Azure Search podporuje zápis dokumentů jako dávku.</span><span class="sxs-lookup"><span data-stu-id="89503-188">Azure Search service supports writing documents as a batch.</span></span> <span data-ttu-id="89503-189">Batch může obsahovat akce 1 do 1000.</span><span class="sxs-lookup"><span data-stu-id="89503-189">A batch can contain 1 to 1,000 Actions.</span></span> <span data-ttu-id="89503-190">Akce zpracovává jeden dokument k provedení operace nahrávání či sloučení.</span><span class="sxs-lookup"><span data-stu-id="89503-190">An action handles one document to perform the upload/merge operation.</span></span>

### <a name="data-type-support"></a><span data-ttu-id="89503-191">Podpora datového typu</span><span class="sxs-lookup"><span data-stu-id="89503-191">Data type support</span></span>
<span data-ttu-id="89503-192">Následující tabulka určuje, zda datového typu Azure Search je podporováno, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="89503-192">The following table specifies whether an Azure Search data type is supported or not.</span></span>

| <span data-ttu-id="89503-193">Azure Search datový typ</span><span class="sxs-lookup"><span data-stu-id="89503-193">Azure Search data type</span></span> | <span data-ttu-id="89503-194">Podporované ve službě Azure Search jímka</span><span class="sxs-lookup"><span data-stu-id="89503-194">Supported in Azure Search Sink</span></span> |
| ---------------------- | ------------------------------ |
| <span data-ttu-id="89503-195">Řetězec</span><span class="sxs-lookup"><span data-stu-id="89503-195">String</span></span> | <span data-ttu-id="89503-196">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-196">Y</span></span> |
| <span data-ttu-id="89503-197">Int32</span><span class="sxs-lookup"><span data-stu-id="89503-197">Int32</span></span> | <span data-ttu-id="89503-198">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-198">Y</span></span> |
| <span data-ttu-id="89503-199">Int64</span><span class="sxs-lookup"><span data-stu-id="89503-199">Int64</span></span> | <span data-ttu-id="89503-200">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-200">Y</span></span> |
| <span data-ttu-id="89503-201">Double</span><span class="sxs-lookup"><span data-stu-id="89503-201">Double</span></span> | <span data-ttu-id="89503-202">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-202">Y</span></span> |
| <span data-ttu-id="89503-203">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="89503-203">Boolean</span></span> | <span data-ttu-id="89503-204">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-204">Y</span></span> |
| <span data-ttu-id="89503-205">DataTimeOffset</span><span class="sxs-lookup"><span data-stu-id="89503-205">DataTimeOffset</span></span> | <span data-ttu-id="89503-206">Ano</span><span class="sxs-lookup"><span data-stu-id="89503-206">Y</span></span> |
| <span data-ttu-id="89503-207">Pole řetězců</span><span class="sxs-lookup"><span data-stu-id="89503-207">String Array</span></span> | <span data-ttu-id="89503-208">N</span><span class="sxs-lookup"><span data-stu-id="89503-208">N</span></span> |
| <span data-ttu-id="89503-209">GeographyPoint</span><span class="sxs-lookup"><span data-stu-id="89503-209">GeographyPoint</span></span> | <span data-ttu-id="89503-210">N</span><span class="sxs-lookup"><span data-stu-id="89503-210">N</span></span> |

## <a name="json-example-copy-data-from-on-premises-sql-server-to-azure-search-index"></a><span data-ttu-id="89503-211">Příklad JSON: kopírování dat z místního SQL serveru do indexu Azure Search</span><span class="sxs-lookup"><span data-stu-id="89503-211">JSON example: Copy data from on-premises SQL Server to Azure Search index</span></span>

<span data-ttu-id="89503-212">Následující příklad ukazuje:</span><span class="sxs-lookup"><span data-stu-id="89503-212">The following sample shows:</span></span>

1.  <span data-ttu-id="89503-213">Propojené služby typu [AzureSearch](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="89503-213">A linked service of type [AzureSearch](#linked-service-properties).</span></span>
2.  <span data-ttu-id="89503-214">Propojené služby typu [onpremisessqlserver](data-factory-sqlserver-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="89503-214">A linked service of type [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).</span></span>
3.  <span data-ttu-id="89503-215">Vstup [datovou sadu](data-factory-create-datasets.md) typu [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="89503-215">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
4.  <span data-ttu-id="89503-216">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSearchIndex](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="89503-216">An output [dataset](data-factory-create-datasets.md) of type [AzureSearchIndex](#dataset-properties).</span></span>
4.  <span data-ttu-id="89503-217">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) a [AzureSearchIndexSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="89503-217">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) and [AzureSearchIndexSink](#copy-activity-properties).</span></span>

<span data-ttu-id="89503-218">Ukázka zkopíruje data časové řady z místní databáze systému SQL Server do indexu Azure Search každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="89503-218">The sample copies time-series data from an on-premises SQL Server database to an Azure Search index hourly.</span></span> <span data-ttu-id="89503-219">Vlastnostech JSON použitých v této ukázce jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="89503-219">The JSON properties used in this sample are described in sections following the samples.</span></span>

<span data-ttu-id="89503-220">Jako první krok nastavte Brána pro správu dat na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="89503-220">As a first step, setup the data management gateway on your on-premises machine.</span></span> <span data-ttu-id="89503-221">Tyto pokyny jsou v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="89503-221">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="89503-222">**Azure Search propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="89503-222">**Azure Search linked service:**</span></span>

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

<span data-ttu-id="89503-223">**Služba SQL Server propojené**</span><span class="sxs-lookup"><span data-stu-id="89503-223">**SQL Server linked service**</span></span>

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

<span data-ttu-id="89503-224">**Vstupní datové sady SQL Server**</span><span class="sxs-lookup"><span data-stu-id="89503-224">**SQL Server input dataset**</span></span>

<span data-ttu-id="89503-225">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v systému SQL Server a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="89503-225">The sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="89503-226">Můžete dotazovat přes více tabulek v rámci stejné databáze pomocí jednu datovou sadu, ale musí používat jednu tabulku pro typeProperty tableName datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="89503-226">You can query over multiple tables within the same database using a single dataset, but a single table must be used for the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="89503-227">Nastavení "externí": "PRAVDA" informuje služba Data Factory, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="89503-227">Setting “external”: ”true” informs Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="89503-228">**Služba Azure Search výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="89503-228">**Azure Search output dataset:**</span></span>

<span data-ttu-id="89503-229">Ukázka zkopíruje data do indexu Azure Search s názvem **produkty**.</span><span class="sxs-lookup"><span data-stu-id="89503-229">The sample copies data to an Azure Search index named **products**.</span></span> <span data-ttu-id="89503-230">Objekt pro vytváření dat vytvořit index.</span><span class="sxs-lookup"><span data-stu-id="89503-230">Data Factory does not create the index.</span></span> <span data-ttu-id="89503-231">Pokud chcete otestovat vzorovou, vytvořte index s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="89503-231">To test the sample, create an index with this name.</span></span> <span data-ttu-id="89503-232">Vytvoření indexu Azure Search s stejný počet sloupců jako vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="89503-232">Create the Azure Search index with the same number of columns as in the input dataset.</span></span> <span data-ttu-id="89503-233">Nové položky se přidají do indexu Azure Search každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="89503-233">New entries are added to the Azure Search index every hour.</span></span>

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

<span data-ttu-id="89503-234">**Aktivita kopírování v kanálu s SQL zdroj a jímka indexu Azure Search:**</span><span class="sxs-lookup"><span data-stu-id="89503-234">**Copy activity in a pipeline with SQL source and Azure Search Index sink:**</span></span>

<span data-ttu-id="89503-235">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="89503-235">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="89503-236">V definici JSON kanálu **zdroj** je typ nastaven na **SqlSource** a **podřízený** je typ nastaven na **AzureSearchIndexSink**.</span><span class="sxs-lookup"><span data-stu-id="89503-236">In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **AzureSearchIndexSink**.</span></span> <span data-ttu-id="89503-237">Zadané pro dotaz SQL **SqlReaderQuery** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="89503-237">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

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

<span data-ttu-id="89503-238">Pokud kopírujete data z jiného úložiště dat cloudu Azure Search, `executionLocation` vlastnost je povinná.</span><span class="sxs-lookup"><span data-stu-id="89503-238">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="89503-239">Následující fragment kódu JSON ukazuje změnu potřeba v rámci aktivity kopírování `typeProperties` jako příklad.</span><span class="sxs-lookup"><span data-stu-id="89503-239">The following JSON snippet shows the change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="89503-240">Zkontrolujte [kopírování dat mezi úložišti dat cloudu](data-factory-data-movement-activities.md#global) části Podporované hodnoty a další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="89503-240">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

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


## <a name="copy-from-a-cloud-source"></a><span data-ttu-id="89503-241">Kopírování ze zdrojového cloudu</span><span class="sxs-lookup"><span data-stu-id="89503-241">Copy from a cloud source</span></span>
<span data-ttu-id="89503-242">Pokud kopírujete data z jiného úložiště dat cloudu Azure Search, `executionLocation` vlastnost je povinná.</span><span class="sxs-lookup"><span data-stu-id="89503-242">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="89503-243">Následující fragment kódu JSON ukazuje změnu potřeba v rámci aktivity kopírování `typeProperties` jako příklad.</span><span class="sxs-lookup"><span data-stu-id="89503-243">The following JSON snippet shows the change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="89503-244">Zkontrolujte [kopírování dat mezi úložišti dat cloudu](data-factory-data-movement-activities.md#global) části Podporované hodnoty a další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="89503-244">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

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

<span data-ttu-id="89503-245">Můžete také mapovat sloupců z datové sady zdroje na sloupce ze sady jímku dat v definici aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="89503-245">You can also map columns from source dataset to columns from sink dataset in the copy activity definition.</span></span> <span data-ttu-id="89503-246">Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="89503-246">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="89503-247">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="89503-247">Performance and tuning</span></span>  
<span data-ttu-id="89503-248">Najdete v článku [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkonu přesun dat (aktivita kopírování) a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="89503-248">See the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="89503-249">Další kroky</span><span class="sxs-lookup"><span data-stu-id="89503-249">Next steps</span></span>
<span data-ttu-id="89503-250">Viz následující články:</span><span class="sxs-lookup"><span data-stu-id="89503-250">See the following articles:</span></span>

* <span data-ttu-id="89503-251">[Kopie aktivity kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny pro vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="89503-251">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>