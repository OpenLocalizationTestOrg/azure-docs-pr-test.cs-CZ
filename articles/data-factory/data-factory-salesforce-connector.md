---
title: "aaaMove data ze služby Salesforce pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data ze služby Salesforce pomocí Azure Data Factory."
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
ms.openlocfilehash: c1bde2a333f5a3c0a995eb8c13ecf585132888b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a><span data-ttu-id="7b75b-103">Přesun dat ze služby Salesforce pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="7b75b-103">Move data from Salesforce by using Azure Data Factory</span></span>
<span data-ttu-id="7b75b-104">Tento článek popisuje, jak můžete použít aktivitu kopírování v Azure data factory toocopy data ze služby Salesforce tooany datové úložiště, které se má zobrazit pod hello podřízený sloupec v hello [podporované zdroje a jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="7b75b-104">This article outlines how you can use Copy Activity in an Azure data factory toocopy data from Salesforce tooany data store that is listed under hello Sink column in hello [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="7b75b-105">Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který uvádí obecný přehled přesun dat s aktivitou kopírování a kombinace podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="7b75b-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

<span data-ttu-id="7b75b-106">Azure Data Factory v současné době podporuje pouze přesun dat ze služby Salesforce příliš[podporovanými úložišti dat podřízený](data-factory-data-movement-activities.md#supported-data-stores-and-formats), ale nepodporuje přesunutí dat z jiných tooSalesforce ukládá data.</span><span class="sxs-lookup"><span data-stu-id="7b75b-106">Azure Data Factory currently supports only moving data from Salesforce too[supported sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats), but does not support moving data from other data stores tooSalesforce.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="7b75b-107">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="7b75b-107">Supported versions</span></span>
<span data-ttu-id="7b75b-108">Tento konektor podporuje následujících edice Salesforce hello: Developer Edition, edice Professional, Enterprise Edition nebo neomezená Edition.</span><span class="sxs-lookup"><span data-stu-id="7b75b-108">This connector supports hello following editions of Salesforce: Developer Edition, Professional Edition, Enterprise Edition, or Unlimited Edition.</span></span> <span data-ttu-id="7b75b-109">A podporuje kopírování z výroby, izolovaný prostor a vlastní doménu služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="7b75b-109">And it supports copying from Salesforce production, sandbox and custom domain.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b75b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7b75b-110">Prerequisites</span></span>
* <span data-ttu-id="7b75b-111">Musí být povoleno oprávnění rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7b75b-111">API permission must be enabled.</span></span> <span data-ttu-id="7b75b-112">V tématu [povolení přístup pomocí rozhraní API v Salesforce sadou oprávnění?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span><span class="sxs-lookup"><span data-stu-id="7b75b-112">See [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span></span>
* <span data-ttu-id="7b75b-113">toocopy data ze služby Salesforce tooon místní datová úložiště, musíte mít alespoň 2.0 brány správy dat ve vašem prostředí místní instalace.</span><span class="sxs-lookup"><span data-stu-id="7b75b-113">toocopy data from Salesforce tooon-premises data stores, you must have at least Data Management Gateway 2.0 installed in your on-premises environment.</span></span>

## <a name="salesforce-request-limits"></a><span data-ttu-id="7b75b-114">Omezení žádostí o služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="7b75b-114">Salesforce request limits</span></span>
<span data-ttu-id="7b75b-115">Služba Salesforce má limity pro celkový počet žádostí o rozhraní API a souběžných žádostí o rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7b75b-115">Salesforce has limits for both total API requests and concurrent API requests.</span></span> <span data-ttu-id="7b75b-116">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="7b75b-116">Note hello following points:</span></span>

- <span data-ttu-id="7b75b-117">Pokud hello počet souběžných požadavků překročí hello limit, dojde k omezení šířky pásma a zobrazí se náhodné chyby.</span><span class="sxs-lookup"><span data-stu-id="7b75b-117">If hello number of concurrent requests exceeds hello limit, throttling occurs and you will see random failures.</span></span>
- <span data-ttu-id="7b75b-118">Pokud hello celkový počet požadavků překračuje hello limit, hello účtu Salesforce se zablokuje 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="7b75b-118">If hello total number of requests exceeds hello limit, hello Salesforce account will be blocked for 24 hours.</span></span>

<span data-ttu-id="7b75b-119">V obou případech se může zobrazit i chyba "REQUEST_LIMIT_EXCEEDED" hello.</span><span class="sxs-lookup"><span data-stu-id="7b75b-119">You might also receive hello “REQUEST_LIMIT_EXCEEDED“ error in both scenarios.</span></span> <span data-ttu-id="7b75b-120">Najdete v části hello "API omezení požadavků" v hello [Salesforce vývojáře omezení](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) článku.</span><span class="sxs-lookup"><span data-stu-id="7b75b-120">See hello "API Request Limits" section in hello [Salesforce Developer Limits](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) article for details.</span></span>

## <a name="getting-started"></a><span data-ttu-id="7b75b-121">Začínáme</span><span class="sxs-lookup"><span data-stu-id="7b75b-121">Getting started</span></span>
<span data-ttu-id="7b75b-122">Vytvoření kanálu s aktivitou kopírování, který přesouvá data ze služby Salesforce pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7b75b-122">You can create a pipeline with a copy activity that moves data from Salesforce by using different tools/APIs.</span></span>

<span data-ttu-id="7b75b-123">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="7b75b-123">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="7b75b-124">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="7b75b-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="7b75b-125">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="7b75b-125">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="7b75b-126">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="7b75b-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="7b75b-127">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="7b75b-127">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="7b75b-128">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="7b75b-128">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="7b75b-129">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="7b75b-129">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="7b75b-130">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="7b75b-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="7b75b-131">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="7b75b-131">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="7b75b-132">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="7b75b-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="7b75b-133">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data ze služby Salesforce, naleznete v tématu [JSON příklad: kopírování dat ze služby Salesforce tooAzure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="7b75b-133">For a sample with JSON definitions for Data Factory entities that are used toocopy data from Salesforce, see [JSON example: Copy data from Salesforce tooAzure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="7b75b-134">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooSalesforce:</span><span class="sxs-lookup"><span data-stu-id="7b75b-134">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooSalesforce:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="7b75b-135">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="7b75b-135">Linked service properties</span></span>
<span data-ttu-id="7b75b-136">Hello následující tabulka obsahuje popis JSON prvky, které jsou specifické toohello Salesforce propojené služby.</span><span class="sxs-lookup"><span data-stu-id="7b75b-136">hello following table provides descriptions for JSON elements that are specific toohello Salesforce linked service.</span></span>

| <span data-ttu-id="7b75b-137">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7b75b-137">Property</span></span> | <span data-ttu-id="7b75b-138">Popis</span><span class="sxs-lookup"><span data-stu-id="7b75b-138">Description</span></span> | <span data-ttu-id="7b75b-139">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7b75b-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7b75b-140">type</span><span class="sxs-lookup"><span data-stu-id="7b75b-140">type</span></span> |<span data-ttu-id="7b75b-141">vlastnost typu Hello musí být nastavena na: **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="7b75b-141">hello type property must be set to: **Salesforce**.</span></span> |<span data-ttu-id="7b75b-142">Ano</span><span class="sxs-lookup"><span data-stu-id="7b75b-142">Yes</span></span> |
| <span data-ttu-id="7b75b-143">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="7b75b-143">environmentUrl</span></span> | <span data-ttu-id="7b75b-144">Zadejte adresu URL služby Salesforce instanci hello.</span><span class="sxs-lookup"><span data-stu-id="7b75b-144">Specify hello URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="7b75b-145">-Výchozí hodnota je "https://login.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="7b75b-145">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="7b75b-146">-toocopy data z izolovaného prostoru, zadejte "https://test.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="7b75b-146">- toocopy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="7b75b-147">-toocopy data z vlastní doménu, zadejte například "https://[domain].my.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="7b75b-147">- toocopy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="7b75b-148">Ne</span><span class="sxs-lookup"><span data-stu-id="7b75b-148">No</span></span> |
| <span data-ttu-id="7b75b-149">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="7b75b-149">username</span></span> |<span data-ttu-id="7b75b-150">Zadejte uživatelské jméno pro hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="7b75b-150">Specify a user name for hello user account.</span></span> |<span data-ttu-id="7b75b-151">Ano</span><span class="sxs-lookup"><span data-stu-id="7b75b-151">Yes</span></span> |
| <span data-ttu-id="7b75b-152">heslo</span><span class="sxs-lookup"><span data-stu-id="7b75b-152">password</span></span> |<span data-ttu-id="7b75b-153">Zadejte heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="7b75b-153">Specify a password for hello user account.</span></span> |<span data-ttu-id="7b75b-154">Ano</span><span class="sxs-lookup"><span data-stu-id="7b75b-154">Yes</span></span> |
| <span data-ttu-id="7b75b-155">securityToken</span><span class="sxs-lookup"><span data-stu-id="7b75b-155">securityToken</span></span> |<span data-ttu-id="7b75b-156">Zadejte token zabezpečení pro hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="7b75b-156">Specify a security token for hello user account.</span></span> <span data-ttu-id="7b75b-157">V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny tooreset nebo získat token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7b75b-157">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how tooreset/get a security token.</span></span> <span data-ttu-id="7b75b-158">Obecně platí, najdete v části toolearn o tokeny zabezpečení [zabezpečení a hello rozhraní API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="7b75b-158">toolearn about security tokens in general, see [Security and hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="7b75b-159">Ano</span><span class="sxs-lookup"><span data-stu-id="7b75b-159">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="7b75b-160">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="7b75b-160">Dataset properties</span></span>
<span data-ttu-id="7b75b-161">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="7b75b-161">For a full list of sections and properties that are available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="7b75b-162">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL, objektů blob v Azure, Azure table atd.).</span><span class="sxs-lookup"><span data-stu-id="7b75b-162">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, and so on).</span></span>

<span data-ttu-id="7b75b-163">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="7b75b-163">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="7b75b-164">rámci typeProperties Hello část datové sady hello typu **RelationalTable** má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="7b75b-164">hello typeProperties section for a dataset of hello type **RelationalTable** has hello following properties:</span></span>

| <span data-ttu-id="7b75b-165">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7b75b-165">Property</span></span> | <span data-ttu-id="7b75b-166">Popis</span><span class="sxs-lookup"><span data-stu-id="7b75b-166">Description</span></span> | <span data-ttu-id="7b75b-167">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7b75b-167">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7b75b-168">tableName</span><span class="sxs-lookup"><span data-stu-id="7b75b-168">tableName</span></span> |<span data-ttu-id="7b75b-169">Název tabulky hello v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="7b75b-169">Name of hello table in Salesforce.</span></span> |<span data-ttu-id="7b75b-170">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="7b75b-170">No (if a **query** of **RelationalSource** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="7b75b-171">Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="7b75b-171">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a><span data-ttu-id="7b75b-173">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="7b75b-173">Copy activity properties</span></span>
<span data-ttu-id="7b75b-174">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="7b75b-174">For a full list of sections and properties that are available for defining activities, see hello [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="7b75b-175">Vlastnosti, jako je název, popis, vstupní a výstupní tabulky a jsou dostupné pro všechny typy aktivit různé zásady.</span><span class="sxs-lookup"><span data-stu-id="7b75b-175">Properties like name, description, input and output tables, and various policies are available for all types of activities.</span></span>

<span data-ttu-id="7b75b-176">Hello vlastnosti, které jsou k dispozici v rámci typeProperties části hello aktivit na hello hello druhé straně, pomocí jednotlivých typů aktivity se liší.</span><span class="sxs-lookup"><span data-stu-id="7b75b-176">hello properties that are available in hello typeProperties section of hello activity, on hello other hand, vary with each activity type.</span></span> <span data-ttu-id="7b75b-177">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="7b75b-177">For Copy Activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="7b75b-178">Při aktivitě kopírování, pokud je zdroj hello hello typu **RelationalSource** (která zahrnuje Salesforce), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="7b75b-178">In copy activity, when hello source is of hello type **RelationalSource** (which includes Salesforce), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="7b75b-179">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7b75b-179">Property</span></span> | <span data-ttu-id="7b75b-180">Popis</span><span class="sxs-lookup"><span data-stu-id="7b75b-180">Description</span></span> | <span data-ttu-id="7b75b-181">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="7b75b-181">Allowed values</span></span> | <span data-ttu-id="7b75b-182">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7b75b-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7b75b-183">query</span><span class="sxs-lookup"><span data-stu-id="7b75b-183">query</span></span> |<span data-ttu-id="7b75b-184">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="7b75b-184">Use hello custom query tooread data.</span></span> |<span data-ttu-id="7b75b-185">Dotaz SQL 92 nebo [Salesforce objektu dotazu jazyka (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) dotazu.</span><span class="sxs-lookup"><span data-stu-id="7b75b-185">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="7b75b-186">Například `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="7b75b-186">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="7b75b-187">Ne (Pokud hello **tableName** z hello **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="7b75b-187">No (if hello **tableName** of hello **dataset** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="7b75b-188">Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="7b75b-188">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a><span data-ttu-id="7b75b-190">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="7b75b-190">Query tips</span></span>
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a><span data-ttu-id="7b75b-191">Načítání dat pomocí where klauzule ve sloupci data a času</span><span class="sxs-lookup"><span data-stu-id="7b75b-191">Retrieving data using where clause on DateTime column</span></span>
<span data-ttu-id="7b75b-192">Při zadejte hello SOQL nebo SQL dotaz, platím pozornost toohello rozdíl formátu data a času.</span><span class="sxs-lookup"><span data-stu-id="7b75b-192">When specify hello SOQL or SQL query, pay attention toohello DateTime format difference.</span></span> <span data-ttu-id="7b75b-193">Například:</span><span class="sxs-lookup"><span data-stu-id="7b75b-193">For example:</span></span>

* <span data-ttu-id="7b75b-194">**Ukázka SOQL**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="7b75b-194">**SOQL sample**: `$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span></span>
* <span data-ttu-id="7b75b-195">**Ukázka SQL**:</span><span class="sxs-lookup"><span data-stu-id="7b75b-195">**SQL sample**:</span></span>
    * <span data-ttu-id="7b75b-196">**Pomocí kopírování Průvodce toospecify hello dotazu:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="7b75b-196">**Using copy wizard toospecify hello query:** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span></span>
    * <span data-ttu-id="7b75b-197">**Pomocí JSON úpravou dotazu hello toospecify (char vyhnuli správně):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="7b75b-197">**Using JSON editing toospecify hello query (escape char properly):** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span></span>

### <a name="retrieving-data-from-salesforce-report"></a><span data-ttu-id="7b75b-198">Načítání dat ze sestavy služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="7b75b-198">Retrieving data from Salesforce Report</span></span>
<span data-ttu-id="7b75b-199">Ze sestavy služby Salesforce můžete data načíst zadáním dotazu jako `{call "<report name>"}`, např.</span><span class="sxs-lookup"><span data-stu-id="7b75b-199">You can retrieve data from Salesforce reports by specifying query as `{call "<report name>"}`,for example,.</span></span> <span data-ttu-id="7b75b-200">`"query": "{call \"TestReport\"}"`.</span><span class="sxs-lookup"><span data-stu-id="7b75b-200">`"query": "{call \"TestReport\"}"`.</span></span>

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a><span data-ttu-id="7b75b-201">Načítání odstranit záznamy z funkce Koš služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="7b75b-201">Retrieving deleted records from Salesforce Recycle Bin</span></span>
<span data-ttu-id="7b75b-202">tooquery hello logicky odstranit záznamy z funkce Koš služby Salesforce, můžete zadat **"IsDeleted = 1"** v dotazu.</span><span class="sxs-lookup"><span data-stu-id="7b75b-202">tooquery hello soft deleted records from Salesforce Recycle Bin, you can specify **"IsDeleted = 1"** in your query.</span></span> <span data-ttu-id="7b75b-203">Například:</span><span class="sxs-lookup"><span data-stu-id="7b75b-203">For example,</span></span>

* <span data-ttu-id="7b75b-204">Zadejte tooquery pouze hello odstranit záznamy "vybrat * z MyTable__c **kde IsDeleted = 1**"</span><span class="sxs-lookup"><span data-stu-id="7b75b-204">tooquery only hello deleted records, specify "select * from MyTable__c **where IsDeleted= 1**"</span></span>
* <span data-ttu-id="7b75b-205">tooquery všechny hello záznamů včetně existující hello a hello odstranit, zadejte "vybrat * z MyTable__c **kde IsDeleted = 0 nebo IsDeleted = 1**"</span><span class="sxs-lookup"><span data-stu-id="7b75b-205">tooquery all hello records including hello existing and hello deleted, specify "select * from MyTable__c **where IsDeleted = 0 or IsDeleted = 1**"</span></span>

## <a name="json-example-copy-data-from-salesforce-tooazure-blob"></a><span data-ttu-id="7b75b-206">Příklad JSON: kopírování dat ze služby Salesforce tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="7b75b-206">JSON example: Copy data from Salesforce tooAzure Blob</span></span>
<span data-ttu-id="7b75b-207">Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí hello [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7b75b-207">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="7b75b-208">Ukazují jak toocopy data ze služby Salesforce tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="7b75b-208">They show how toocopy data from Salesforce tooAzure Blob Storage.</span></span> <span data-ttu-id="7b75b-209">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7b75b-209">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="7b75b-210">Tady jsou hello artefaktů služby Data Factory, budete potřebovat toocreate tooimplement hello scénář.</span><span class="sxs-lookup"><span data-stu-id="7b75b-210">Here are hello Data Factory artifacts that you'll need toocreate tooimplement hello scenario.</span></span> <span data-ttu-id="7b75b-211">části Hello hello seznamu obsahují podrobnosti o těchto krocích.</span><span class="sxs-lookup"><span data-stu-id="7b75b-211">hello sections that follow hello list provide details about these steps.</span></span>

* <span data-ttu-id="7b75b-212">Propojené služby typu hello [Salesforce](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="7b75b-212">A linked service of hello type [Salesforce](#linked-service-properties)</span></span>
* <span data-ttu-id="7b75b-213">Propojené služby typu hello [azurestorage.](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="7b75b-213">A linked service of hello type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="7b75b-214">Vstup [datovou sadu](data-factory-create-datasets.md) typu hello [RelationalTable](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="7b75b-214">An input [dataset](data-factory-create-datasets.md) of hello type [RelationalTable](#dataset-properties)</span></span>
* <span data-ttu-id="7b75b-215">Výstup [datovou sadu](data-factory-create-datasets.md) typu hello [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="7b75b-215">An output [dataset](data-factory-create-datasets.md) of hello type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="7b75b-216">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="7b75b-216">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="7b75b-217">**Salesforce propojené služby**</span><span class="sxs-lookup"><span data-stu-id="7b75b-217">**Salesforce linked service**</span></span>

<span data-ttu-id="7b75b-218">Tento příklad používá hello **Salesforce** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="7b75b-218">This example uses hello **Salesforce** linked service.</span></span> <span data-ttu-id="7b75b-219">V tématu hello [Salesforce propojená služba](#linked-service-properties) části hello vlastnosti, které podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="7b75b-219">See hello [Salesforce linked service](#linked-service-properties) section for hello properties that are supported by this linked service.</span></span>  <span data-ttu-id="7b75b-220">V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny, jak hello tooreset nebo získat token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7b75b-220">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how tooreset/get hello security token.</span></span>

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
<span data-ttu-id="7b75b-221">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="7b75b-221">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="7b75b-222">**Vstupní datové sady služby Salesforce**</span><span class="sxs-lookup"><span data-stu-id="7b75b-222">**Salesforce input dataset**</span></span>

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

<span data-ttu-id="7b75b-223">Nastavení **externí** příliš**true** informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="7b75b-223">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7b75b-224">Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="7b75b-224">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

<span data-ttu-id="7b75b-226">**Výstupní datová sada Azure Blob**</span><span class="sxs-lookup"><span data-stu-id="7b75b-226">**Azure blob output dataset**</span></span>

<span data-ttu-id="7b75b-227">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="7b75b-227">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

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

<span data-ttu-id="7b75b-228">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="7b75b-228">**Pipeline with Copy Activity**</span></span>

<span data-ttu-id="7b75b-229">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurována toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="7b75b-229">hello pipeline contains Copy Activity, which is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="7b75b-230">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource**a hello **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="7b75b-230">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource**, and hello **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="7b75b-231">V tématu [vlastnosti typu RelationalSource](#copy-activity-properties) hello seznam vlastností, které jsou podporovány hello RelationalSource.</span><span class="sxs-lookup"><span data-stu-id="7b75b-231">See [RelationalSource type properties](#copy-activity-properties) for hello list of properties that are supported by hello RelationalSource.</span></span>

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
            "description": "Copy from Salesforce tooan Azure blob",
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
> <span data-ttu-id="7b75b-232">Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="7b75b-232">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a><span data-ttu-id="7b75b-234">Mapování typu pro Salesforce</span><span class="sxs-lookup"><span data-stu-id="7b75b-234">Type mapping for Salesforce</span></span>
| <span data-ttu-id="7b75b-235">Typ služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="7b75b-235">Salesforce type</span></span> | <span data-ttu-id="7b75b-236">. Na základě NET typu</span><span class="sxs-lookup"><span data-stu-id="7b75b-236">.NET-based type</span></span> |
| --- | --- |
| <span data-ttu-id="7b75b-237">Automatické číslování</span><span class="sxs-lookup"><span data-stu-id="7b75b-237">Auto Number</span></span> |<span data-ttu-id="7b75b-238">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-238">String</span></span> |
| <span data-ttu-id="7b75b-239">Zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="7b75b-239">Checkbox</span></span> |<span data-ttu-id="7b75b-240">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="7b75b-240">Boolean</span></span> |
| <span data-ttu-id="7b75b-241">Měna</span><span class="sxs-lookup"><span data-stu-id="7b75b-241">Currency</span></span> |<span data-ttu-id="7b75b-242">Double</span><span class="sxs-lookup"><span data-stu-id="7b75b-242">Double</span></span> |
| <span data-ttu-id="7b75b-243">Datum</span><span class="sxs-lookup"><span data-stu-id="7b75b-243">Date</span></span> |<span data-ttu-id="7b75b-244">Data a času</span><span class="sxs-lookup"><span data-stu-id="7b75b-244">DateTime</span></span> |
| <span data-ttu-id="7b75b-245">Datum a čas</span><span class="sxs-lookup"><span data-stu-id="7b75b-245">Date/Time</span></span> |<span data-ttu-id="7b75b-246">Data a času</span><span class="sxs-lookup"><span data-stu-id="7b75b-246">DateTime</span></span> |
| <span data-ttu-id="7b75b-247">E-mail</span><span class="sxs-lookup"><span data-stu-id="7b75b-247">Email</span></span> |<span data-ttu-id="7b75b-248">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-248">String</span></span> |
| <span data-ttu-id="7b75b-249">ID</span><span class="sxs-lookup"><span data-stu-id="7b75b-249">Id</span></span> |<span data-ttu-id="7b75b-250">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-250">String</span></span> |
| <span data-ttu-id="7b75b-251">Relace hledání</span><span class="sxs-lookup"><span data-stu-id="7b75b-251">Lookup Relationship</span></span> |<span data-ttu-id="7b75b-252">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-252">String</span></span> |
| <span data-ttu-id="7b75b-253">Vybrat víc rozevíracího seznamu</span><span class="sxs-lookup"><span data-stu-id="7b75b-253">Multi-Select Picklist</span></span> |<span data-ttu-id="7b75b-254">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-254">String</span></span> |
| <span data-ttu-id="7b75b-255">Číslo</span><span class="sxs-lookup"><span data-stu-id="7b75b-255">Number</span></span> |<span data-ttu-id="7b75b-256">Double</span><span class="sxs-lookup"><span data-stu-id="7b75b-256">Double</span></span> |
| <span data-ttu-id="7b75b-257">Procento</span><span class="sxs-lookup"><span data-stu-id="7b75b-257">Percent</span></span> |<span data-ttu-id="7b75b-258">Double</span><span class="sxs-lookup"><span data-stu-id="7b75b-258">Double</span></span> |
| <span data-ttu-id="7b75b-259">Telefon</span><span class="sxs-lookup"><span data-stu-id="7b75b-259">Phone</span></span> |<span data-ttu-id="7b75b-260">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-260">String</span></span> |
| <span data-ttu-id="7b75b-261">Rozevírací seznam</span><span class="sxs-lookup"><span data-stu-id="7b75b-261">Picklist</span></span> |<span data-ttu-id="7b75b-262">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-262">String</span></span> |
| <span data-ttu-id="7b75b-263">Text</span><span class="sxs-lookup"><span data-stu-id="7b75b-263">Text</span></span> |<span data-ttu-id="7b75b-264">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-264">String</span></span> |
| <span data-ttu-id="7b75b-265">Textová oblast</span><span class="sxs-lookup"><span data-stu-id="7b75b-265">Text Area</span></span> |<span data-ttu-id="7b75b-266">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-266">String</span></span> |
| <span data-ttu-id="7b75b-267">Textová oblast (Long)</span><span class="sxs-lookup"><span data-stu-id="7b75b-267">Text Area (Long)</span></span> |<span data-ttu-id="7b75b-268">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-268">String</span></span> |
| <span data-ttu-id="7b75b-269">(Rich) textová oblast</span><span class="sxs-lookup"><span data-stu-id="7b75b-269">Text Area (Rich)</span></span> |<span data-ttu-id="7b75b-270">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-270">String</span></span> |
| <span data-ttu-id="7b75b-271">Text (šifrované)</span><span class="sxs-lookup"><span data-stu-id="7b75b-271">Text (Encrypted)</span></span> |<span data-ttu-id="7b75b-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-272">String</span></span> |
| <span data-ttu-id="7b75b-273">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="7b75b-273">URL</span></span> |<span data-ttu-id="7b75b-274">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b75b-274">String</span></span> |

> [!NOTE]
> <span data-ttu-id="7b75b-275">toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="7b75b-275">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a><span data-ttu-id="7b75b-276">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="7b75b-276">Performance and tuning</span></span>
<span data-ttu-id="7b75b-277">V tématu hello [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="7b75b-277">See hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
