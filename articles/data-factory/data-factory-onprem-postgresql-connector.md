---
title: "aaaMove data z PostgreSQL pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data z databáze PostgreSQL pomocí Azure Data Factory."
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
ms.openlocfilehash: ea384f4e06f7d7bedae2949e4ea727c8f8806614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a><span data-ttu-id="4e0f5-103">Přesun dat z PostgreSQL pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="4e0f5-103">Move data from PostgreSQL using Azure Data Factory</span></span>
<span data-ttu-id="4e0f5-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze PostgreSQL místně.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises PostgreSQL database.</span></span> <span data-ttu-id="4e0f5-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="4e0f5-106">Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní PostgreSQL data.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-106">You can copy data from an on-premises PostgreSQL data store tooany supported sink data store.</span></span> <span data-ttu-id="4e0f5-107">Seznam úložišť dat jako jímky nepodporuje hello aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-107">For a list of data stores supported as sinks by hello copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="4e0f5-108">Objekt pro vytváření dat aktuálně podporuje přesun dat z PostgreSQL databáze tooother úložiště dat, ale není pro přesun dat z jiných dat ukládá tooan databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-108">Data factory currently supports moving data from a PostgreSQL database tooother data stores, but not for moving data from other data stores tooan PostgreSQL database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4e0f5-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4e0f5-109">prerequisites</span></span>

<span data-ttu-id="4e0f5-110">Služba data Factory podporuje připojování zdroje PostgreSQL tooon místní pomocí hello Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-110">Data Factory service supports connecting tooon-premises PostgreSQL sources using hello Data Management Gateway.</span></span> <span data-ttu-id="4e0f5-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="4e0f5-112">Vyžaduje se brána, i když je databáze PostgreSQL hello hostována v virtuálního počítače Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-112">Gateway is required even if hello PostgreSQL database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="4e0f5-113">Brány můžete nainstalovat na stejný virtuální počítač IaaS jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit databázi toohello hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-113">You can install gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="4e0f5-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="4e0f5-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="4e0f5-115">Supported versions and installation</span></span>
<span data-ttu-id="4e0f5-116">Brána pro správu dat tooconnect toohello databázi PostgreSQL, nainstalujte hello [Ngpsql zprostředkovatele dat pro PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 nebo výše na hello stejné systému jako brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-116">For Data Management Gateway tooconnect toohello PostgreSQL Database, install hello [Ngpsql data provider for PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 or above on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="4e0f5-117">PostgreSQL verze 7.4 a vyšší je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-117">PostgreSQL version 7.4 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="4e0f5-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="4e0f5-118">Getting started</span></span>
<span data-ttu-id="4e0f5-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní PostgreSQL data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-119">You can create a pipeline with a copy activity that moves data from an on-premises PostgreSQL data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="4e0f5-120">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="4e0f5-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="4e0f5-122">Také můžete použít následující nástroje toocreate kanálu hello:</span><span class="sxs-lookup"><span data-stu-id="4e0f5-122">You can also use hello following tools toocreate a pipeline:</span></span> 
    - <span data-ttu-id="4e0f5-123">portál Azure</span><span class="sxs-lookup"><span data-stu-id="4e0f5-123">Azure portal</span></span>
    - <span data-ttu-id="4e0f5-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e0f5-124">Visual Studio</span></span>
    - <span data-ttu-id="4e0f5-125">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e0f5-125">Azure PowerShell</span></span>
    - <span data-ttu-id="4e0f5-126">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4e0f5-126">Azure Resource Manager template</span></span>
    - <span data-ttu-id="4e0f5-127">.NET API</span><span class="sxs-lookup"><span data-stu-id="4e0f5-127">.NET API</span></span>
    - <span data-ttu-id="4e0f5-128">REST API</span><span class="sxs-lookup"><span data-stu-id="4e0f5-128">REST API</span></span>

     <span data-ttu-id="4e0f5-129">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="4e0f5-130">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="4e0f5-130">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="4e0f5-131">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-131">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="4e0f5-132">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-132">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="4e0f5-133">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="4e0f5-134">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-134">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="4e0f5-135">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="4e0f5-136">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z místního úložiště dat PostgreSQL, naleznete v tématu [JSON příklad: kopírování dat z PostgreSQL tooAzure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-136">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises PostgreSQL data store, see [JSON example: Copy data from PostgreSQL tooAzure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="4e0f5-137">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa PostgreSQL úložiště dat:</span><span class="sxs-lookup"><span data-stu-id="4e0f5-137">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa PostgreSQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="4e0f5-138">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="4e0f5-138">Linked service properties</span></span>
<span data-ttu-id="4e0f5-139">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooPostgreSQL propojené služby.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-139">hello following table provides description for JSON elements specific tooPostgreSQL linked service.</span></span>

| <span data-ttu-id="4e0f5-140">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4e0f5-140">Property</span></span> | <span data-ttu-id="4e0f5-141">Popis</span><span class="sxs-lookup"><span data-stu-id="4e0f5-141">Description</span></span> | <span data-ttu-id="4e0f5-142">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4e0f5-142">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4e0f5-143">type</span><span class="sxs-lookup"><span data-stu-id="4e0f5-143">type</span></span> |<span data-ttu-id="4e0f5-144">vlastnost typu Hello musí být nastavena na: **OnPremisesPostgreSql**</span><span class="sxs-lookup"><span data-stu-id="4e0f5-144">hello type property must be set to: **OnPremisesPostgreSql**</span></span> |<span data-ttu-id="4e0f5-145">Ano</span><span class="sxs-lookup"><span data-stu-id="4e0f5-145">Yes</span></span> |
| <span data-ttu-id="4e0f5-146">server</span><span class="sxs-lookup"><span data-stu-id="4e0f5-146">server</span></span> |<span data-ttu-id="4e0f5-147">Název serveru PostgreSQL hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-147">Name of hello PostgreSQL server.</span></span> |<span data-ttu-id="4e0f5-148">Ano</span><span class="sxs-lookup"><span data-stu-id="4e0f5-148">Yes</span></span> |
| <span data-ttu-id="4e0f5-149">Databáze</span><span class="sxs-lookup"><span data-stu-id="4e0f5-149">database</span></span> |<span data-ttu-id="4e0f5-150">Název databáze PostgreSQL hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-150">Name of hello PostgreSQL database.</span></span> |<span data-ttu-id="4e0f5-151">Ano</span><span class="sxs-lookup"><span data-stu-id="4e0f5-151">Yes</span></span> |
| <span data-ttu-id="4e0f5-152">Schéma</span><span class="sxs-lookup"><span data-stu-id="4e0f5-152">schema</span></span> |<span data-ttu-id="4e0f5-153">Název schématu hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-153">Name of hello schema in hello database.</span></span> <span data-ttu-id="4e0f5-154">název schématu Hello rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-154">hello schema name is case-sensitive.</span></span> |<span data-ttu-id="4e0f5-155">Ne</span><span class="sxs-lookup"><span data-stu-id="4e0f5-155">No</span></span> |
| <span data-ttu-id="4e0f5-156">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-156">authenticationType</span></span> |<span data-ttu-id="4e0f5-157">Typ ověřování používá databázi PostgreSQL toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-157">Type of authentication used tooconnect toohello PostgreSQL database.</span></span> <span data-ttu-id="4e0f5-158">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-158">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="4e0f5-159">Ano</span><span class="sxs-lookup"><span data-stu-id="4e0f5-159">Yes</span></span> |
| <span data-ttu-id="4e0f5-160">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="4e0f5-160">username</span></span> |<span data-ttu-id="4e0f5-161">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-161">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="4e0f5-162">Ne</span><span class="sxs-lookup"><span data-stu-id="4e0f5-162">No</span></span> |
| <span data-ttu-id="4e0f5-163">heslo</span><span class="sxs-lookup"><span data-stu-id="4e0f5-163">password</span></span> |<span data-ttu-id="4e0f5-164">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-164">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="4e0f5-165">Ne</span><span class="sxs-lookup"><span data-stu-id="4e0f5-165">No</span></span> |
| <span data-ttu-id="4e0f5-166">gatewayName</span><span class="sxs-lookup"><span data-stu-id="4e0f5-166">gatewayName</span></span> |<span data-ttu-id="4e0f5-167">Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-167">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises PostgreSQL database.</span></span> |<span data-ttu-id="4e0f5-168">Ano</span><span class="sxs-lookup"><span data-stu-id="4e0f5-168">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="4e0f5-169">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="4e0f5-169">Dataset properties</span></span>
<span data-ttu-id="4e0f5-170">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-170">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="4e0f5-171">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="4e0f5-172">část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-172">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="4e0f5-173">rámci typeProperties Hello část datové sady typ **RelationalTable** (což zahrnuje datová sada PostgreSQL) má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="4e0f5-173">hello typeProperties section for dataset of type **RelationalTable** (which includes PostgreSQL dataset) has hello following properties:</span></span>

| <span data-ttu-id="4e0f5-174">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4e0f5-174">Property</span></span> | <span data-ttu-id="4e0f5-175">Popis</span><span class="sxs-lookup"><span data-stu-id="4e0f5-175">Description</span></span> | <span data-ttu-id="4e0f5-176">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4e0f5-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4e0f5-177">tableName</span><span class="sxs-lookup"><span data-stu-id="4e0f5-177">tableName</span></span> |<span data-ttu-id="4e0f5-178">Název tabulky hello v hello instance databáze PostgreSQL, která je propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-178">Name of hello table in hello PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="4e0f5-179">Hello tableName rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-179">hello tableName is case-sensitive.</span></span> |<span data-ttu-id="4e0f5-180">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="4e0f5-180">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="4e0f5-181">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="4e0f5-181">Copy activity properties</span></span>
<span data-ttu-id="4e0f5-182">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-182">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="4e0f5-183">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-183">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="4e0f5-184">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-184">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="4e0f5-185">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-185">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="4e0f5-186">Pokud je zdroj typu **RelationalSource** (která zahrnuje PostgreSQL), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="4e0f5-186">When source is of type **RelationalSource** (which includes PostgreSQL), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="4e0f5-187">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4e0f5-187">Property</span></span> | <span data-ttu-id="4e0f5-188">Popis</span><span class="sxs-lookup"><span data-stu-id="4e0f5-188">Description</span></span> | <span data-ttu-id="4e0f5-189">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="4e0f5-189">Allowed values</span></span> | <span data-ttu-id="4e0f5-190">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4e0f5-190">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4e0f5-191">query</span><span class="sxs-lookup"><span data-stu-id="4e0f5-191">query</span></span> |<span data-ttu-id="4e0f5-192">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-192">Use hello custom query tooread data.</span></span> |<span data-ttu-id="4e0f5-193">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-193">SQL query string.</span></span> <span data-ttu-id="4e0f5-194">Například: "dotaz": "vybrat * z \"MySchema\".\" MyTable\"".</span><span class="sxs-lookup"><span data-stu-id="4e0f5-194">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="4e0f5-195">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="4e0f5-195">No (if **tableName** of **dataset** is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="4e0f5-196">Schéma a tabulku názvy rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-196">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="4e0f5-197">Uzavřete je do `""` (dvojité uvozovky) v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-197">Enclose them in `""` (double quotes) in hello query.</span></span>  

<span data-ttu-id="4e0f5-198">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="4e0f5-198">**Example:**</span></span>

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-tooazure-blob"></a><span data-ttu-id="4e0f5-199">Příklad JSON: kopírování dat z PostgreSQL tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="4e0f5-199">JSON example: Copy data from PostgreSQL tooAzure Blob</span></span>
<span data-ttu-id="4e0f5-200">Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-200">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="4e0f5-201">Ukazují jak toocopy data z PostgreSQL databáze tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-201">They show how toocopy data from PostgreSQL database tooAzure Blob Storage.</span></span> <span data-ttu-id="4e0f5-202">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-202">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="4e0f5-203">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-203">This sample provides JSON snippets.</span></span> <span data-ttu-id="4e0f5-204">Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-204">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="4e0f5-205">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-205">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="4e0f5-206">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="4e0f5-206">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="4e0f5-207">Propojené služby typu [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-207">A linked service of type [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="4e0f5-208">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-208">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="4e0f5-209">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-209">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="4e0f5-210">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-210">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="4e0f5-211">Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-211">hello [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="4e0f5-212">Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob tooa databáze PostgreSQL každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-212">hello sample copies data from a query result in PostgreSQL database tooa blob every hour.</span></span> <span data-ttu-id="4e0f5-213">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-213">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="4e0f5-214">Jako první krok nastavte Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-214">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="4e0f5-215">Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-215">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="4e0f5-216">**PostgreSQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="4e0f5-216">**PostgreSQL linked service:**</span></span>

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
<span data-ttu-id="4e0f5-217">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="4e0f5-217">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="4e0f5-218">**PostgreSQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="4e0f5-218">**PostgreSQL input dataset:**</span></span>

<span data-ttu-id="4e0f5-219">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v PostgreSQL a obsahuje sloupec s názvem "časové razítko" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-219">hello sample assumes you have created a table “MyTable” in PostgreSQL and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="4e0f5-220">Nastavení `"external": true` informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-220">Setting `"external": true` informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="4e0f5-221">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="4e0f5-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="4e0f5-222">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-222">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="4e0f5-223">název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-223">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="4e0f5-224">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-224">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="4e0f5-225">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="4e0f5-225">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="4e0f5-226">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-226">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="4e0f5-227">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-227">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="4e0f5-228">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data z hello public.usstates tabulky v databázi PostgreSQL hello.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-228">hello SQL query specified for hello **query** property selects hello data from hello public.usstates table in hello PostgreSQL database.</span></span>

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
## <a name="type-mapping-for-postgresql"></a><span data-ttu-id="4e0f5-229">Mapování typu pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4e0f5-229">Type mapping for PostgreSQL</span></span>
<span data-ttu-id="4e0f5-230">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:</span><span class="sxs-lookup"><span data-stu-id="4e0f5-230">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="4e0f5-231">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="4e0f5-231">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="4e0f5-232">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="4e0f5-232">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="4e0f5-233">Při přesunu dat tooPostgreSQL, hello následující mapování se používají z typu too.NET typu PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-233">When moving data tooPostgreSQL, hello following mappings are used from PostgreSQL type too.NET type.</span></span>

| <span data-ttu-id="4e0f5-234">Typ databáze PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4e0f5-234">PostgreSQL Database type</span></span> | <span data-ttu-id="4e0f5-235">PostgresSQL aliasy</span><span class="sxs-lookup"><span data-stu-id="4e0f5-235">PostgresSQL aliases</span></span> | <span data-ttu-id="4e0f5-236">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="4e0f5-236">.NET Framework type</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4e0f5-237">abstime</span><span class="sxs-lookup"><span data-stu-id="4e0f5-237">abstime</span></span> | |<span data-ttu-id="4e0f5-238">Data a času</span><span class="sxs-lookup"><span data-stu-id="4e0f5-238">Datetime</span></span> | &nbsp;
| <span data-ttu-id="4e0f5-239">bigint</span><span class="sxs-lookup"><span data-stu-id="4e0f5-239">bigint</span></span> |<span data-ttu-id="4e0f5-240">int8</span><span class="sxs-lookup"><span data-stu-id="4e0f5-240">int8</span></span> |<span data-ttu-id="4e0f5-241">Int64</span><span class="sxs-lookup"><span data-stu-id="4e0f5-241">Int64</span></span> |
| <span data-ttu-id="4e0f5-242">bigserial</span><span class="sxs-lookup"><span data-stu-id="4e0f5-242">bigserial</span></span> |<span data-ttu-id="4e0f5-243">serial8</span><span class="sxs-lookup"><span data-stu-id="4e0f5-243">serial8</span></span> |<span data-ttu-id="4e0f5-244">Int64</span><span class="sxs-lookup"><span data-stu-id="4e0f5-244">Int64</span></span> |
| <span data-ttu-id="4e0f5-245">bit [(ne)]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-245">bit [ (n) ]</span></span> | |<span data-ttu-id="4e0f5-246">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-246">Byte[], String</span></span> | &nbsp;
| <span data-ttu-id="4e0f5-247">bit různých [(ne)]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-247">bit varying [ (n) ]</span></span> |<span data-ttu-id="4e0f5-248">varbit</span><span class="sxs-lookup"><span data-stu-id="4e0f5-248">varbit</span></span> |<span data-ttu-id="4e0f5-249">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-249">Byte[], String</span></span> |
| <span data-ttu-id="4e0f5-250">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4e0f5-250">boolean</span></span> |<span data-ttu-id="4e0f5-251">BOOL</span><span class="sxs-lookup"><span data-stu-id="4e0f5-251">bool</span></span> |<span data-ttu-id="4e0f5-252">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4e0f5-252">Boolean</span></span> |
| <span data-ttu-id="4e0f5-253">Pole</span><span class="sxs-lookup"><span data-stu-id="4e0f5-253">box</span></span> | |<span data-ttu-id="4e0f5-254">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-254">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-255">bytea</span><span class="sxs-lookup"><span data-stu-id="4e0f5-255">bytea</span></span> | |<span data-ttu-id="4e0f5-256">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-256">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-257">znak [(ne)]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-257">character [ (n) ]</span></span> |<span data-ttu-id="4e0f5-258">char [(ne)]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-258">char [ (n) ]</span></span> |<span data-ttu-id="4e0f5-259">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-259">String</span></span> |
| <span data-ttu-id="4e0f5-260">znak různých [(ne)]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-260">character varying [ (n) ]</span></span> |<span data-ttu-id="4e0f5-261">varchar [(ne)]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-261">varchar [ (n) ]</span></span> |<span data-ttu-id="4e0f5-262">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-262">String</span></span> |
| <span data-ttu-id="4e0f5-263">CID</span><span class="sxs-lookup"><span data-stu-id="4e0f5-263">cid</span></span> | |<span data-ttu-id="4e0f5-264">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-264">String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-265">CIDR</span><span class="sxs-lookup"><span data-stu-id="4e0f5-265">cidr</span></span> | |<span data-ttu-id="4e0f5-266">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-266">String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-267">kruhu.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-267">circle</span></span> | |<span data-ttu-id="4e0f5-268">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-268">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-269">Datum</span><span class="sxs-lookup"><span data-stu-id="4e0f5-269">date</span></span> | |<span data-ttu-id="4e0f5-270">Data a času</span><span class="sxs-lookup"><span data-stu-id="4e0f5-270">Datetime</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-271">DateRange</span><span class="sxs-lookup"><span data-stu-id="4e0f5-271">daterange</span></span> | |<span data-ttu-id="4e0f5-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-272">String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-273">Dvojitá přesnost</span><span class="sxs-lookup"><span data-stu-id="4e0f5-273">double precision</span></span> |<span data-ttu-id="4e0f5-274">FLOAT8</span><span class="sxs-lookup"><span data-stu-id="4e0f5-274">float8</span></span> |<span data-ttu-id="4e0f5-275">Double</span><span class="sxs-lookup"><span data-stu-id="4e0f5-275">Double</span></span> |
| <span data-ttu-id="4e0f5-276">inet</span><span class="sxs-lookup"><span data-stu-id="4e0f5-276">inet</span></span> | |<span data-ttu-id="4e0f5-277">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-277">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-278">intarry</span><span class="sxs-lookup"><span data-stu-id="4e0f5-278">intarry</span></span> | |<span data-ttu-id="4e0f5-279">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-279">String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-280">int4range</span><span class="sxs-lookup"><span data-stu-id="4e0f5-280">int4range</span></span> | |<span data-ttu-id="4e0f5-281">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-281">String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-282">int8range</span><span class="sxs-lookup"><span data-stu-id="4e0f5-282">int8range</span></span> | |<span data-ttu-id="4e0f5-283">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-283">String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-284">celé číslo</span><span class="sxs-lookup"><span data-stu-id="4e0f5-284">integer</span></span> |<span data-ttu-id="4e0f5-285">int, int4</span><span class="sxs-lookup"><span data-stu-id="4e0f5-285">int, int4</span></span> |<span data-ttu-id="4e0f5-286">Int32</span><span class="sxs-lookup"><span data-stu-id="4e0f5-286">Int32</span></span> |
| <span data-ttu-id="4e0f5-287">Interval [pole] [(p)]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-287">interval [ fields ] [ (p) ]</span></span> | |<span data-ttu-id="4e0f5-288">Časový interval</span><span class="sxs-lookup"><span data-stu-id="4e0f5-288">Timespan</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-289">JSON</span><span class="sxs-lookup"><span data-stu-id="4e0f5-289">json</span></span> | |<span data-ttu-id="4e0f5-290">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-290">String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-291">jsonb</span><span class="sxs-lookup"><span data-stu-id="4e0f5-291">jsonb</span></span> | |<span data-ttu-id="4e0f5-292">Byte]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-292">Byte[]</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-293">řádek</span><span class="sxs-lookup"><span data-stu-id="4e0f5-293">line</span></span> | |<span data-ttu-id="4e0f5-294">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-294">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-295">lseg</span><span class="sxs-lookup"><span data-stu-id="4e0f5-295">lseg</span></span> | |<span data-ttu-id="4e0f5-296">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-296">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-297">macaddr</span><span class="sxs-lookup"><span data-stu-id="4e0f5-297">macaddr</span></span> | |<span data-ttu-id="4e0f5-298">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-298">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-299">peníze</span><span class="sxs-lookup"><span data-stu-id="4e0f5-299">money</span></span> | |<span data-ttu-id="4e0f5-300">Decimal</span><span class="sxs-lookup"><span data-stu-id="4e0f5-300">Decimal</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-301">číselný [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-301">numeric [ (p, s) ]</span></span> |<span data-ttu-id="4e0f5-302">Decimal [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="4e0f5-302">decimal [ (p, s) ]</span></span> |<span data-ttu-id="4e0f5-303">Decimal</span><span class="sxs-lookup"><span data-stu-id="4e0f5-303">Decimal</span></span> |
| <span data-ttu-id="4e0f5-304">numrange</span><span class="sxs-lookup"><span data-stu-id="4e0f5-304">numrange</span></span> | |<span data-ttu-id="4e0f5-305">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-305">String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-306">OID</span><span class="sxs-lookup"><span data-stu-id="4e0f5-306">oid</span></span> | |<span data-ttu-id="4e0f5-307">Int32</span><span class="sxs-lookup"><span data-stu-id="4e0f5-307">Int32</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-308">Cesta</span><span class="sxs-lookup"><span data-stu-id="4e0f5-308">path</span></span> | |<span data-ttu-id="4e0f5-309">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-309">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-310">pg_lsn</span><span class="sxs-lookup"><span data-stu-id="4e0f5-310">pg_lsn</span></span> | |<span data-ttu-id="4e0f5-311">Int64</span><span class="sxs-lookup"><span data-stu-id="4e0f5-311">Int64</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-312">bod</span><span class="sxs-lookup"><span data-stu-id="4e0f5-312">point</span></span> | |<span data-ttu-id="4e0f5-313">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-313">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-314">mnohoúhelníku</span><span class="sxs-lookup"><span data-stu-id="4e0f5-314">polygon</span></span> | |<span data-ttu-id="4e0f5-315">Byte [] řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-315">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="4e0f5-316">skutečné</span><span class="sxs-lookup"><span data-stu-id="4e0f5-316">real</span></span> |<span data-ttu-id="4e0f5-317">FLOAT4</span><span class="sxs-lookup"><span data-stu-id="4e0f5-317">float4</span></span> |<span data-ttu-id="4e0f5-318">Jeden</span><span class="sxs-lookup"><span data-stu-id="4e0f5-318">Single</span></span> |
| <span data-ttu-id="4e0f5-319">smallint</span><span class="sxs-lookup"><span data-stu-id="4e0f5-319">smallint</span></span> |<span data-ttu-id="4e0f5-320">int2</span><span class="sxs-lookup"><span data-stu-id="4e0f5-320">int2</span></span> |<span data-ttu-id="4e0f5-321">Int16</span><span class="sxs-lookup"><span data-stu-id="4e0f5-321">Int16</span></span> |
| <span data-ttu-id="4e0f5-322">smallserial</span><span class="sxs-lookup"><span data-stu-id="4e0f5-322">smallserial</span></span> |<span data-ttu-id="4e0f5-323">serial2</span><span class="sxs-lookup"><span data-stu-id="4e0f5-323">serial2</span></span> |<span data-ttu-id="4e0f5-324">Int16</span><span class="sxs-lookup"><span data-stu-id="4e0f5-324">Int16</span></span> |
| <span data-ttu-id="4e0f5-325">Sériového portu</span><span class="sxs-lookup"><span data-stu-id="4e0f5-325">serial</span></span> |<span data-ttu-id="4e0f5-326">serial4</span><span class="sxs-lookup"><span data-stu-id="4e0f5-326">serial4</span></span> |<span data-ttu-id="4e0f5-327">Int32</span><span class="sxs-lookup"><span data-stu-id="4e0f5-327">Int32</span></span> |
| <span data-ttu-id="4e0f5-328">Text</span><span class="sxs-lookup"><span data-stu-id="4e0f5-328">text</span></span> | |<span data-ttu-id="4e0f5-329">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e0f5-329">String</span></span> |&nbsp;

## <a name="map-source-toosink-columns"></a><span data-ttu-id="4e0f5-330">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="4e0f5-330">Map source toosink columns</span></span>
<span data-ttu-id="4e0f5-331">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-331">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="4e0f5-332">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="4e0f5-332">Repeatable read from relational sources</span></span>
<span data-ttu-id="4e0f5-333">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-333">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="4e0f5-334">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-334">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="4e0f5-335">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-335">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="4e0f5-336">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-336">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="4e0f5-337">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="4e0f5-337">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="4e0f5-338">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="4e0f5-338">Performance and Tuning</span></span>
<span data-ttu-id="4e0f5-339">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="4e0f5-339">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
