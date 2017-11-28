---
title: "Přesun dat ze služby Salesforce pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data ze služby Salesforce pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 9390b992bce2dede750c3fc55b7783a6b0db678f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a><span data-ttu-id="c80d5-103">Přesun dat ze služby Salesforce pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="c80d5-103">Move data from Salesforce by using Azure Data Factory</span></span>
<span data-ttu-id="c80d5-104">Tento článek popisuje, jak pomocí aktivity kopírování v objektu pro vytváření dat Azure ke kopírování dat ze služby Salesforce do jakékoli úložiště dat, která je uvedena ve sloupci jímka v [podporované zdroje a jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="c80d5-104">This article outlines how you can use Copy Activity in an Azure data factory to copy data from Salesforce to any data store that is listed under the Sink column in the [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="c80d5-105">Tento článek vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který uvádí obecný přehled přesun dat s aktivitou kopírování a kombinace podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="c80d5-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

<span data-ttu-id="c80d5-106">Azure Data Factory aktuálně podporuje pouze přesunutí dat ze služby Salesforce k [podporovanými úložišti dat podřízený](data-factory-data-movement-activities.md#supported-data-stores-and-formats), ale nemá nepodporuje přesun dat z jiných dat ukládá do služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="c80d5-106">Azure Data Factory currently supports only moving data from Salesforce to [supported sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats), but does not support moving data from other data stores to Salesforce.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="c80d5-107">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="c80d5-107">Supported versions</span></span>
<span data-ttu-id="c80d5-108">Tento konektor podporuje následujících edice Salesforce: Developer Edition, edice Professional, Enterprise Edition nebo neomezená Edition.</span><span class="sxs-lookup"><span data-stu-id="c80d5-108">This connector supports the following editions of Salesforce: Developer Edition, Professional Edition, Enterprise Edition, or Unlimited Edition.</span></span> <span data-ttu-id="c80d5-109">A podporuje kopírování z výroby, izolovaný prostor a vlastní doménu služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="c80d5-109">And it supports copying from Salesforce production, sandbox and custom domain.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c80d5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c80d5-110">Prerequisites</span></span>
* <span data-ttu-id="c80d5-111">Musí být povoleno oprávnění rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c80d5-111">API permission must be enabled.</span></span> <span data-ttu-id="c80d5-112">V tématu [povolení přístup pomocí rozhraní API v Salesforce sadou oprávnění?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span><span class="sxs-lookup"><span data-stu-id="c80d5-112">See [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span></span>
* <span data-ttu-id="c80d5-113">Kopírování dat ze služby Salesforce do místního úložiště dat, pokud nemáte alespoň 2.0 brány správy dat ve vašem prostředí místní instalace.</span><span class="sxs-lookup"><span data-stu-id="c80d5-113">To copy data from Salesforce to on-premises data stores, you must have at least Data Management Gateway 2.0 installed in your on-premises environment.</span></span>

## <a name="salesforce-request-limits"></a><span data-ttu-id="c80d5-114">Omezení žádostí o služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="c80d5-114">Salesforce request limits</span></span>
<span data-ttu-id="c80d5-115">Služba Salesforce má limity pro celkový počet žádostí o rozhraní API a souběžných žádostí o rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c80d5-115">Salesforce has limits for both total API requests and concurrent API requests.</span></span> <span data-ttu-id="c80d5-116">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="c80d5-116">Note the following points:</span></span>

- <span data-ttu-id="c80d5-117">Pokud počet souběžných požadavků překročí limit, dojde k omezení šířky pásma a zobrazí se náhodné chyby.</span><span class="sxs-lookup"><span data-stu-id="c80d5-117">If the number of concurrent requests exceeds the limit, throttling occurs and you will see random failures.</span></span>
- <span data-ttu-id="c80d5-118">Pokud celkový počet požadavků překračuje limit, účtu Salesforce se zablokuje 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="c80d5-118">If the total number of requests exceeds the limit, the Salesforce account will be blocked for 24 hours.</span></span>

<span data-ttu-id="c80d5-119">V obou případech se může také zobrazí chyba "REQUEST_LIMIT_EXCEEDED".</span><span class="sxs-lookup"><span data-stu-id="c80d5-119">You might also receive the “REQUEST_LIMIT_EXCEEDED“ error in both scenarios.</span></span> <span data-ttu-id="c80d5-120">Najdete v části "API omezení počtu požadavků" v [Salesforce vývojáře omezení](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) článku.</span><span class="sxs-lookup"><span data-stu-id="c80d5-120">See the "API Request Limits" section in the [Salesforce Developer Limits](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) article for details.</span></span>

## <a name="getting-started"></a><span data-ttu-id="c80d5-121">Začínáme</span><span class="sxs-lookup"><span data-stu-id="c80d5-121">Getting started</span></span>
<span data-ttu-id="c80d5-122">Vytvoření kanálu s aktivitou kopírování, který přesouvá data ze služby Salesforce pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c80d5-122">You can create a pipeline with a copy activity that moves data from Salesforce by using different tools/APIs.</span></span>

<span data-ttu-id="c80d5-123">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="c80d5-123">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="c80d5-124">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="c80d5-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="c80d5-125">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="c80d5-125">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="c80d5-126">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="c80d5-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="c80d5-127">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="c80d5-127">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="c80d5-128">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="c80d5-128">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="c80d5-129">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="c80d5-129">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="c80d5-130">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="c80d5-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="c80d5-131">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="c80d5-131">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="c80d5-132">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="c80d5-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="c80d5-133">Příklad s definicemi JSON entit služby Data Factory, které slouží ke kopírování dat ze služby Salesforce, naleznete v tématu [JSON příklad: kopírování dat ze služby Salesforce do objektu Blob Azure](#json-example-copy-data-from-salesforce-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="c80d5-133">For a sample with JSON definitions for Data Factory entities that are used to copy data from Salesforce, see [JSON example: Copy data from Salesforce to Azure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="c80d5-134">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení entit služby Data Factory konkrétní do služby Salesforce:</span><span class="sxs-lookup"><span data-stu-id="c80d5-134">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Salesforce:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="c80d5-135">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="c80d5-135">Linked service properties</span></span>
<span data-ttu-id="c80d5-136">Následující tabulka obsahuje popis elementy JSON, které jsou specifické pro službu propojené služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="c80d5-136">The following table provides descriptions for JSON elements that are specific to the Salesforce linked service.</span></span>

| <span data-ttu-id="c80d5-137">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c80d5-137">Property</span></span> | <span data-ttu-id="c80d5-138">Popis</span><span class="sxs-lookup"><span data-stu-id="c80d5-138">Description</span></span> | <span data-ttu-id="c80d5-139">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c80d5-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c80d5-140">type</span><span class="sxs-lookup"><span data-stu-id="c80d5-140">type</span></span> |<span data-ttu-id="c80d5-141">Vlastnost typu musí být nastavena na: **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="c80d5-141">The type property must be set to: **Salesforce**.</span></span> |<span data-ttu-id="c80d5-142">Ano</span><span class="sxs-lookup"><span data-stu-id="c80d5-142">Yes</span></span> |
| <span data-ttu-id="c80d5-143">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="c80d5-143">environmentUrl</span></span> | <span data-ttu-id="c80d5-144">Zadejte adresu URL služby Salesforce instanci.</span><span class="sxs-lookup"><span data-stu-id="c80d5-144">Specify the URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="c80d5-145">-Výchozí hodnota je "https://login.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="c80d5-145">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="c80d5-146">-Ke zkopírování dat z izolovaného prostoru, zadejte "https://test.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="c80d5-146">- To copy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="c80d5-147">-Ke zkopírování dat z vlastní domény, zadejte, například "https://[domain].my.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="c80d5-147">- To copy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="c80d5-148">Ne</span><span class="sxs-lookup"><span data-stu-id="c80d5-148">No</span></span> |
| <span data-ttu-id="c80d5-149">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="c80d5-149">username</span></span> |<span data-ttu-id="c80d5-150">Zadejte uživatelské jméno pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="c80d5-150">Specify a user name for the user account.</span></span> |<span data-ttu-id="c80d5-151">Ano</span><span class="sxs-lookup"><span data-stu-id="c80d5-151">Yes</span></span> |
| <span data-ttu-id="c80d5-152">heslo</span><span class="sxs-lookup"><span data-stu-id="c80d5-152">password</span></span> |<span data-ttu-id="c80d5-153">Zadejte heslo pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="c80d5-153">Specify a password for the user account.</span></span> |<span data-ttu-id="c80d5-154">Ano</span><span class="sxs-lookup"><span data-stu-id="c80d5-154">Yes</span></span> |
| <span data-ttu-id="c80d5-155">securityToken</span><span class="sxs-lookup"><span data-stu-id="c80d5-155">securityToken</span></span> |<span data-ttu-id="c80d5-156">Zadejte token zabezpečení pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="c80d5-156">Specify a security token for the user account.</span></span> <span data-ttu-id="c80d5-157">V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny o tom, jak resetování nebo získat token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c80d5-157">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how to reset/get a security token.</span></span> <span data-ttu-id="c80d5-158">Obecné informace o tokeny zabezpečení najdete v tématu [zabezpečení a rozhraní API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="c80d5-158">To learn about security tokens in general, see [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="c80d5-159">Ano</span><span class="sxs-lookup"><span data-stu-id="c80d5-159">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="c80d5-160">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="c80d5-160">Dataset properties</span></span>
<span data-ttu-id="c80d5-161">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c80d5-161">For a full list of sections and properties that are available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="c80d5-162">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL, objektů blob v Azure, Azure table atd.).</span><span class="sxs-lookup"><span data-stu-id="c80d5-162">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, and so on).</span></span>

<span data-ttu-id="c80d5-163">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="c80d5-163">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="c80d5-164">Rámci typeProperties část datové sady typu **RelationalTable** má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c80d5-164">The typeProperties section for a dataset of the type **RelationalTable** has the following properties:</span></span>

| <span data-ttu-id="c80d5-165">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c80d5-165">Property</span></span> | <span data-ttu-id="c80d5-166">Popis</span><span class="sxs-lookup"><span data-stu-id="c80d5-166">Description</span></span> | <span data-ttu-id="c80d5-167">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c80d5-167">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c80d5-168">tableName</span><span class="sxs-lookup"><span data-stu-id="c80d5-168">tableName</span></span> |<span data-ttu-id="c80d5-169">Název tabulky v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="c80d5-169">Name of the table in Salesforce.</span></span> |<span data-ttu-id="c80d5-170">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="c80d5-170">No (if a **query** of **RelationalSource** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="c80d5-171">Část "__c" název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="c80d5-171">The "__c" part of the API Name is needed for any custom object.</span></span>

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a><span data-ttu-id="c80d5-173">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="c80d5-173">Copy activity properties</span></span>
<span data-ttu-id="c80d5-174">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c80d5-174">For a full list of sections and properties that are available for defining activities, see the [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="c80d5-175">Vlastnosti, jako je název, popis, vstupní a výstupní tabulky a jsou dostupné pro všechny typy aktivit různé zásady.</span><span class="sxs-lookup"><span data-stu-id="c80d5-175">Properties like name, description, input and output tables, and various policies are available for all types of activities.</span></span>

<span data-ttu-id="c80d5-176">Vlastnosti, které jsou k dispozici v rámci typeProperties části aktivity, se liší na druhé straně, každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="c80d5-176">The properties that are available in the typeProperties section of the activity, on the other hand, vary with each activity type.</span></span> <span data-ttu-id="c80d5-177">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="c80d5-177">For Copy Activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="c80d5-178">Při aktivitě kopírování, pokud je zdroj typu **RelationalSource** (která zahrnuje Salesforce), v rámci typeProperties části jsou k dispozici následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c80d5-178">In copy activity, when the source is of the type **RelationalSource** (which includes Salesforce), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="c80d5-179">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c80d5-179">Property</span></span> | <span data-ttu-id="c80d5-180">Popis</span><span class="sxs-lookup"><span data-stu-id="c80d5-180">Description</span></span> | <span data-ttu-id="c80d5-181">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="c80d5-181">Allowed values</span></span> | <span data-ttu-id="c80d5-182">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c80d5-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c80d5-183">query</span><span class="sxs-lookup"><span data-stu-id="c80d5-183">query</span></span> |<span data-ttu-id="c80d5-184">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="c80d5-184">Use the custom query to read data.</span></span> |<span data-ttu-id="c80d5-185">Dotaz SQL 92 nebo [Salesforce objektu dotazu jazyka (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) dotazu.</span><span class="sxs-lookup"><span data-stu-id="c80d5-185">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="c80d5-186">Například `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="c80d5-186">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="c80d5-187">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="c80d5-187">No (if the **tableName** of the **dataset** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="c80d5-188">Část "__c" název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="c80d5-188">The "__c" part of the API Name is needed for any custom object.</span></span>

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a><span data-ttu-id="c80d5-190">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="c80d5-190">Query tips</span></span>
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a><span data-ttu-id="c80d5-191">Načítání dat pomocí where klauzule ve sloupci data a času</span><span class="sxs-lookup"><span data-stu-id="c80d5-191">Retrieving data using where clause on DateTime column</span></span>
<span data-ttu-id="c80d5-192">Při zadejte SOQL nebo SQL dotaz, věnujte pozornost rozdíl formátu data a času.</span><span class="sxs-lookup"><span data-stu-id="c80d5-192">When specify the SOQL or SQL query, pay attention to the DateTime format difference.</span></span> <span data-ttu-id="c80d5-193">Například:</span><span class="sxs-lookup"><span data-stu-id="c80d5-193">For example:</span></span>

* <span data-ttu-id="c80d5-194">**Ukázka SOQL**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="c80d5-194">**SOQL sample**: `$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span></span>
* <span data-ttu-id="c80d5-195">**Ukázka SQL**:</span><span class="sxs-lookup"><span data-stu-id="c80d5-195">**SQL sample**:</span></span>
    * <span data-ttu-id="c80d5-196">**Pomocí Průvodce kopírováním vytvoříte dotaz:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="c80d5-196">**Using copy wizard to specify the query:** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span></span>
    * <span data-ttu-id="c80d5-197">**Pomocí úpravy zadat dotaz JSON (char vyhnuli správně):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="c80d5-197">**Using JSON editing to specify the query (escape char properly):** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span></span>

### <a name="retrieving-data-from-salesforce-report"></a><span data-ttu-id="c80d5-198">Načítání dat ze sestavy služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="c80d5-198">Retrieving data from Salesforce Report</span></span>
<span data-ttu-id="c80d5-199">Ze sestavy služby Salesforce můžete data načíst zadáním dotazu jako `{call "<report name>"}`, např.</span><span class="sxs-lookup"><span data-stu-id="c80d5-199">You can retrieve data from Salesforce reports by specifying query as `{call "<report name>"}`,for example,.</span></span> <span data-ttu-id="c80d5-200">`"query": "{call \"TestReport\"}"`.</span><span class="sxs-lookup"><span data-stu-id="c80d5-200">`"query": "{call \"TestReport\"}"`.</span></span>

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a><span data-ttu-id="c80d5-201">Načítání odstranit záznamy z funkce Koš služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="c80d5-201">Retrieving deleted records from Salesforce Recycle Bin</span></span>
<span data-ttu-id="c80d5-202">Chcete-li prohledávat logicky odstraněné záznamy z funkce Koš služby Salesforce, můžete zadat **"IsDeleted = 1"** v dotazu.</span><span class="sxs-lookup"><span data-stu-id="c80d5-202">To query the soft deleted records from Salesforce Recycle Bin, you can specify **"IsDeleted = 1"** in your query.</span></span> <span data-ttu-id="c80d5-203">Například:</span><span class="sxs-lookup"><span data-stu-id="c80d5-203">For example,</span></span>

* <span data-ttu-id="c80d5-204">Chcete-li prohledávat pouze odstraněné záznamy, zadejte "vybrat * z MyTable__c **kde IsDeleted = 1**"</span><span class="sxs-lookup"><span data-stu-id="c80d5-204">To query only the deleted records, specify "select * from MyTable__c **where IsDeleted= 1**"</span></span>
* <span data-ttu-id="c80d5-205">Chcete-li prohledávat všechny záznamy, včetně existující a Odstraněná, zadejte "vybrat * z MyTable__c **kde IsDeleted = 0 nebo IsDeleted = 1**"</span><span class="sxs-lookup"><span data-stu-id="c80d5-205">To query all the records including the existing and the deleted, specify "select * from MyTable__c **where IsDeleted = 0 or IsDeleted = 1**"</span></span>

## <a name="json-example-copy-data-from-salesforce-to-azure-blob"></a><span data-ttu-id="c80d5-206">Příklad JSON: kopírování dat ze služby Salesforce do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="c80d5-206">JSON example: Copy data from Salesforce to Azure Blob</span></span>
<span data-ttu-id="c80d5-207">Následující příklad uvádí ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c80d5-207">The following example provides sample JSON definitions that you can use to create a pipeline by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="c80d5-208">Se ukazují, jak zkopírovat data ze služby Salesforce do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c80d5-208">They show how to copy data from Salesforce to Azure Blob Storage.</span></span> <span data-ttu-id="c80d5-209">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c80d5-209">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="c80d5-210">Tady jsou artefaktů služby Data Factory, které budete muset vytvořit implementovat scénář.</span><span class="sxs-lookup"><span data-stu-id="c80d5-210">Here are the Data Factory artifacts that you'll need to create to implement the scenario.</span></span> <span data-ttu-id="c80d5-211">Části seznamu obsahují podrobnosti o těchto krocích.</span><span class="sxs-lookup"><span data-stu-id="c80d5-211">The sections that follow the list provide details about these steps.</span></span>

* <span data-ttu-id="c80d5-212">Propojené služby typu [Salesforce](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="c80d5-212">A linked service of the type [Salesforce](#linked-service-properties)</span></span>
* <span data-ttu-id="c80d5-213">Propojené služby typu [azurestorage.](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="c80d5-213">A linked service of the type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="c80d5-214">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="c80d5-214">An input [dataset](data-factory-create-datasets.md) of the type [RelationalTable](#dataset-properties)</span></span>
* <span data-ttu-id="c80d5-215">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="c80d5-215">An output [dataset](data-factory-create-datasets.md) of the type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="c80d5-216">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="c80d5-216">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="c80d5-217">**Salesforce propojené služby**</span><span class="sxs-lookup"><span data-stu-id="c80d5-217">**Salesforce linked service**</span></span>

<span data-ttu-id="c80d5-218">Tento příklad používá **Salesforce** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="c80d5-218">This example uses the **Salesforce** linked service.</span></span> <span data-ttu-id="c80d5-219">Najdete v článku [Salesforce propojená služba](#linked-service-properties) části pro vlastnosti, které podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="c80d5-219">See the [Salesforce linked service](#linked-service-properties) section for the properties that are supported by this linked service.</span></span>  <span data-ttu-id="c80d5-220">V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny o tom, jak resetování nebo získat token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c80d5-220">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how to reset/get the security token.</span></span>

```json
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```
<span data-ttu-id="c80d5-221">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="c80d5-221">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="c80d5-222">**Vstupní datové sady služby Salesforce**</span><span class="sxs-lookup"><span data-stu-id="c80d5-222">**Salesforce input dataset**</span></span>

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"  
        },
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

<span data-ttu-id="c80d5-223">Nastavení **externí** k **true** služba Data Factory informuje, že datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="c80d5-223">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c80d5-224">Část "__c" název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="c80d5-224">The "__c" part of the API Name is needed for any custom object.</span></span>

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

<span data-ttu-id="c80d5-226">**Výstupní datová sada Azure Blob**</span><span class="sxs-lookup"><span data-stu-id="c80d5-226">**Azure blob output dataset**</span></span>

<span data-ttu-id="c80d5-227">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="c80d5-227">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/alltypes_c"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="c80d5-228">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="c80d5-228">**Pipeline with Copy Activity**</span></span>

<span data-ttu-id="c80d5-229">Kanál obsahuje aktivitu kopírování, která je konfigurovaná pro používání vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="c80d5-229">The pipeline contains Copy Activity, which is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="c80d5-230">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource**a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="c80d5-230">In the pipeline JSON definition, the **source** type is set to **RelationalSource**, and the **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="c80d5-231">V tématu [vlastnosti typu RelationalSource](#copy-activity-properties) pro seznam vlastností, které jsou podporovány RelationalSource.</span><span class="sxs-lookup"><span data-stu-id="c80d5-231">See [RelationalSource type properties](#copy-activity-properties) for the list of properties that are supported by the RelationalSource.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"                
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
> [!IMPORTANT]
> <span data-ttu-id="c80d5-232">Část "__c" název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="c80d5-232">The "__c" part of the API Name is needed for any custom object.</span></span>

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a><span data-ttu-id="c80d5-234">Mapování typu pro Salesforce</span><span class="sxs-lookup"><span data-stu-id="c80d5-234">Type mapping for Salesforce</span></span>
| <span data-ttu-id="c80d5-235">Typ služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="c80d5-235">Salesforce type</span></span> | <span data-ttu-id="c80d5-236">. Na základě NET typu</span><span class="sxs-lookup"><span data-stu-id="c80d5-236">.NET-based type</span></span> |
| --- | --- |
| <span data-ttu-id="c80d5-237">Automatické číslování</span><span class="sxs-lookup"><span data-stu-id="c80d5-237">Auto Number</span></span> |<span data-ttu-id="c80d5-238">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-238">String</span></span> |
| <span data-ttu-id="c80d5-239">Zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="c80d5-239">Checkbox</span></span> |<span data-ttu-id="c80d5-240">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="c80d5-240">Boolean</span></span> |
| <span data-ttu-id="c80d5-241">Měna</span><span class="sxs-lookup"><span data-stu-id="c80d5-241">Currency</span></span> |<span data-ttu-id="c80d5-242">Double</span><span class="sxs-lookup"><span data-stu-id="c80d5-242">Double</span></span> |
| <span data-ttu-id="c80d5-243">Datum</span><span class="sxs-lookup"><span data-stu-id="c80d5-243">Date</span></span> |<span data-ttu-id="c80d5-244">Data a času</span><span class="sxs-lookup"><span data-stu-id="c80d5-244">DateTime</span></span> |
| <span data-ttu-id="c80d5-245">Datum a čas</span><span class="sxs-lookup"><span data-stu-id="c80d5-245">Date/Time</span></span> |<span data-ttu-id="c80d5-246">Data a času</span><span class="sxs-lookup"><span data-stu-id="c80d5-246">DateTime</span></span> |
| <span data-ttu-id="c80d5-247">E-mail</span><span class="sxs-lookup"><span data-stu-id="c80d5-247">Email</span></span> |<span data-ttu-id="c80d5-248">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-248">String</span></span> |
| <span data-ttu-id="c80d5-249">ID</span><span class="sxs-lookup"><span data-stu-id="c80d5-249">Id</span></span> |<span data-ttu-id="c80d5-250">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-250">String</span></span> |
| <span data-ttu-id="c80d5-251">Relace hledání</span><span class="sxs-lookup"><span data-stu-id="c80d5-251">Lookup Relationship</span></span> |<span data-ttu-id="c80d5-252">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-252">String</span></span> |
| <span data-ttu-id="c80d5-253">Vybrat víc rozevíracího seznamu</span><span class="sxs-lookup"><span data-stu-id="c80d5-253">Multi-Select Picklist</span></span> |<span data-ttu-id="c80d5-254">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-254">String</span></span> |
| <span data-ttu-id="c80d5-255">Číslo</span><span class="sxs-lookup"><span data-stu-id="c80d5-255">Number</span></span> |<span data-ttu-id="c80d5-256">Double</span><span class="sxs-lookup"><span data-stu-id="c80d5-256">Double</span></span> |
| <span data-ttu-id="c80d5-257">Procento</span><span class="sxs-lookup"><span data-stu-id="c80d5-257">Percent</span></span> |<span data-ttu-id="c80d5-258">Double</span><span class="sxs-lookup"><span data-stu-id="c80d5-258">Double</span></span> |
| <span data-ttu-id="c80d5-259">Telefon</span><span class="sxs-lookup"><span data-stu-id="c80d5-259">Phone</span></span> |<span data-ttu-id="c80d5-260">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-260">String</span></span> |
| <span data-ttu-id="c80d5-261">Rozevírací seznam</span><span class="sxs-lookup"><span data-stu-id="c80d5-261">Picklist</span></span> |<span data-ttu-id="c80d5-262">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-262">String</span></span> |
| <span data-ttu-id="c80d5-263">Text</span><span class="sxs-lookup"><span data-stu-id="c80d5-263">Text</span></span> |<span data-ttu-id="c80d5-264">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-264">String</span></span> |
| <span data-ttu-id="c80d5-265">Textová oblast</span><span class="sxs-lookup"><span data-stu-id="c80d5-265">Text Area</span></span> |<span data-ttu-id="c80d5-266">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-266">String</span></span> |
| <span data-ttu-id="c80d5-267">Textová oblast (Long)</span><span class="sxs-lookup"><span data-stu-id="c80d5-267">Text Area (Long)</span></span> |<span data-ttu-id="c80d5-268">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-268">String</span></span> |
| <span data-ttu-id="c80d5-269">(Rich) textová oblast</span><span class="sxs-lookup"><span data-stu-id="c80d5-269">Text Area (Rich)</span></span> |<span data-ttu-id="c80d5-270">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-270">String</span></span> |
| <span data-ttu-id="c80d5-271">Text (šifrované)</span><span class="sxs-lookup"><span data-stu-id="c80d5-271">Text (Encrypted)</span></span> |<span data-ttu-id="c80d5-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-272">String</span></span> |
| <span data-ttu-id="c80d5-273">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="c80d5-273">URL</span></span> |<span data-ttu-id="c80d5-274">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c80d5-274">String</span></span> |

> [!NOTE]
> <span data-ttu-id="c80d5-275">Mapování sloupců z datové sady zdroje na sloupce ze sady jímku dat naleznete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="c80d5-275">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a><span data-ttu-id="c80d5-276">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="c80d5-276">Performance and tuning</span></span>
<span data-ttu-id="c80d5-277">Najdete v článku [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="c80d5-277">See the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
