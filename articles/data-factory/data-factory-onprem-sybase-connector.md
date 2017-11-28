---
title: "aaaMove data z databáze Sybase pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data z databáze Sybase pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: b379ee10-0ff5-4974-8c87-c95f82f1c5c6
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: ad003ec502028d56db9570fe08af329eb5b71817
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sybase-using-azure-data-factory"></a><span data-ttu-id="e1a90-103">Přesun dat z databáze Sybase pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="e1a90-103">Move data from Sybase using Azure Data Factory</span></span>
<span data-ttu-id="e1a90-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze Sybase na místě.</span><span class="sxs-lookup"><span data-stu-id="e1a90-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises Sybase database.</span></span> <span data-ttu-id="e1a90-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="e1a90-106">Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní Sybase data.</span><span class="sxs-lookup"><span data-stu-id="e1a90-106">You can copy data from an on-premises Sybase data store tooany supported sink data store.</span></span> <span data-ttu-id="e1a90-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="e1a90-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="e1a90-108">Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat tooother uložit data z databáze Sybase data, ale ne pro přesun dat z jiného úložiště dat úložiště tooa Sybase data.</span><span class="sxs-lookup"><span data-stu-id="e1a90-108">Data factory currently supports only moving data from a Sybase data store tooother data stores, but not for moving data from other data stores tooa Sybase data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e1a90-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e1a90-109">Prerequisites</span></span>
<span data-ttu-id="e1a90-110">Služba data Factory podporuje připojování tooon místní databázi Sybase zdroje pomocí hello Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="e1a90-110">Data Factory service supports connecting tooon-premises Sybase sources using hello Data Management Gateway.</span></span> <span data-ttu-id="e1a90-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.</span><span class="sxs-lookup"><span data-stu-id="e1a90-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="e1a90-112">Vyžaduje se brána, i když je databáze Sybase hello hostované ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="e1a90-112">Gateway is required even if hello Sybase database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="e1a90-113">Hello brány můžete nainstalovat na stejný virtuální počítač IaaS jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit databázi toohello hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-113">You can install hello gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="e1a90-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="e1a90-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="e1a90-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="e1a90-115">Supported versions and installation</span></span>
<span data-ttu-id="e1a90-116">Brána pro správu dat tooconnect toohello databáze Sybase, musíte tooinstall hello [zprostředkovatele dat pro databázi Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 nebo výše na hello stejné systému jako brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-116">For Data Management Gateway tooconnect toohello Sybase Database, you need tooinstall hello [data provider for Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 or above on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="e1a90-117">Sybase verze 16 a vyšší je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="e1a90-117">Sybase version 16 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e1a90-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="e1a90-118">Getting started</span></span>
<span data-ttu-id="e1a90-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e1a90-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="e1a90-120">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="e1a90-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="e1a90-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="e1a90-122">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="e1a90-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="e1a90-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="e1a90-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="e1a90-124">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="e1a90-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="e1a90-125">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="e1a90-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="e1a90-126">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="e1a90-126">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="e1a90-127">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="e1a90-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="e1a90-128">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="e1a90-128">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="e1a90-129">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="e1a90-130">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat Sybase na místě, naleznete v tématu [JSON příklad: kopírování dat z databáze Sybase tooAzure Blob](#json-example-copy-data-from-sybase-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="e1a90-130">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises Sybase data store, see [JSON example: Copy data from Sybase tooAzure Blob](#json-example-copy-data-from-sybase-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="e1a90-131">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa Sybase úložiště dat:</span><span class="sxs-lookup"><span data-stu-id="e1a90-131">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Sybase data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="e1a90-132">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="e1a90-132">Linked service properties</span></span>
<span data-ttu-id="e1a90-133">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooSybase propojené služby.</span><span class="sxs-lookup"><span data-stu-id="e1a90-133">hello following table provides description for JSON elements specific tooSybase linked service.</span></span>

| <span data-ttu-id="e1a90-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="e1a90-134">Property</span></span> | <span data-ttu-id="e1a90-135">Popis</span><span class="sxs-lookup"><span data-stu-id="e1a90-135">Description</span></span> | <span data-ttu-id="e1a90-136">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e1a90-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e1a90-137">type</span><span class="sxs-lookup"><span data-stu-id="e1a90-137">type</span></span> |<span data-ttu-id="e1a90-138">vlastnost typu Hello musí být nastavena na: **OnPremisesSybase**</span><span class="sxs-lookup"><span data-stu-id="e1a90-138">hello type property must be set to: **OnPremisesSybase**</span></span> |<span data-ttu-id="e1a90-139">Ano</span><span class="sxs-lookup"><span data-stu-id="e1a90-139">Yes</span></span> |
| <span data-ttu-id="e1a90-140">server</span><span class="sxs-lookup"><span data-stu-id="e1a90-140">server</span></span> |<span data-ttu-id="e1a90-141">Název serveru Sybase hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-141">Name of hello Sybase server.</span></span> |<span data-ttu-id="e1a90-142">Ano</span><span class="sxs-lookup"><span data-stu-id="e1a90-142">Yes</span></span> |
| <span data-ttu-id="e1a90-143">Databáze</span><span class="sxs-lookup"><span data-stu-id="e1a90-143">database</span></span> |<span data-ttu-id="e1a90-144">Název databáze Sybase hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-144">Name of hello Sybase database.</span></span> |<span data-ttu-id="e1a90-145">Ano</span><span class="sxs-lookup"><span data-stu-id="e1a90-145">Yes</span></span> |
| <span data-ttu-id="e1a90-146">Schéma</span><span class="sxs-lookup"><span data-stu-id="e1a90-146">schema</span></span> |<span data-ttu-id="e1a90-147">Název schématu hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-147">Name of hello schema in hello database.</span></span> |<span data-ttu-id="e1a90-148">Ne</span><span class="sxs-lookup"><span data-stu-id="e1a90-148">No</span></span> |
| <span data-ttu-id="e1a90-149">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="e1a90-149">authenticationType</span></span> |<span data-ttu-id="e1a90-150">Typ ověřování používá databázi Sybase toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="e1a90-150">Type of authentication used tooconnect toohello Sybase database.</span></span> <span data-ttu-id="e1a90-151">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e1a90-151">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="e1a90-152">Ano</span><span class="sxs-lookup"><span data-stu-id="e1a90-152">Yes</span></span> |
| <span data-ttu-id="e1a90-153">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="e1a90-153">username</span></span> |<span data-ttu-id="e1a90-154">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="e1a90-154">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="e1a90-155">Ne</span><span class="sxs-lookup"><span data-stu-id="e1a90-155">No</span></span> |
| <span data-ttu-id="e1a90-156">heslo</span><span class="sxs-lookup"><span data-stu-id="e1a90-156">password</span></span> |<span data-ttu-id="e1a90-157">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-157">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="e1a90-158">Ne</span><span class="sxs-lookup"><span data-stu-id="e1a90-158">No</span></span> |
| <span data-ttu-id="e1a90-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="e1a90-159">gatewayName</span></span> |<span data-ttu-id="e1a90-160">Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi Sybase.</span><span class="sxs-lookup"><span data-stu-id="e1a90-160">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Sybase database.</span></span> |<span data-ttu-id="e1a90-161">Ano</span><span class="sxs-lookup"><span data-stu-id="e1a90-161">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="e1a90-162">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="e1a90-162">Dataset properties</span></span>
<span data-ttu-id="e1a90-163">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="e1a90-163">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="e1a90-164">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="e1a90-164">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="e1a90-165">část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-165">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="e1a90-166">Hello **rámci typeProperties** části pro datovou sadu typu **RelationalTable** (což zahrnuje datová sada Sybase) má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="e1a90-166">hello **typeProperties** section for dataset of type **RelationalTable** (which includes Sybase dataset) has hello following properties:</span></span>

| <span data-ttu-id="e1a90-167">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="e1a90-167">Property</span></span> | <span data-ttu-id="e1a90-168">Popis</span><span class="sxs-lookup"><span data-stu-id="e1a90-168">Description</span></span> | <span data-ttu-id="e1a90-169">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e1a90-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e1a90-170">tableName</span><span class="sxs-lookup"><span data-stu-id="e1a90-170">tableName</span></span> |<span data-ttu-id="e1a90-171">Název tabulky hello v hello instance databáze Sybase, která je propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="e1a90-171">Name of hello table in hello Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="e1a90-172">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="e1a90-172">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="e1a90-173">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="e1a90-173">Copy activity properties</span></span>
<span data-ttu-id="e1a90-174">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="e1a90-174">For a full list of sections & properties available for defining activities, see [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="e1a90-175">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="e1a90-175">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="e1a90-176">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="e1a90-176">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="e1a90-177">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="e1a90-177">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="e1a90-178">Pokud zdroj hello je typu **RelationalSource** (která zahrnuje Sybase), hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="e1a90-178">When hello source is of type **RelationalSource** (which includes Sybase), hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="e1a90-179">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="e1a90-179">Property</span></span> | <span data-ttu-id="e1a90-180">Popis</span><span class="sxs-lookup"><span data-stu-id="e1a90-180">Description</span></span> | <span data-ttu-id="e1a90-181">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="e1a90-181">Allowed values</span></span> | <span data-ttu-id="e1a90-182">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e1a90-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e1a90-183">query</span><span class="sxs-lookup"><span data-stu-id="e1a90-183">query</span></span> |<span data-ttu-id="e1a90-184">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="e1a90-184">Use hello custom query tooread data.</span></span> |<span data-ttu-id="e1a90-185">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="e1a90-185">SQL query string.</span></span> <span data-ttu-id="e1a90-186">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="e1a90-186">For example: select * from MyTable.</span></span> |<span data-ttu-id="e1a90-187">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="e1a90-187">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-sybase-tooazure-blob"></a><span data-ttu-id="e1a90-188">Příklad JSON: kopírování dat z databáze Sybase tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="e1a90-188">JSON example: Copy data from Sybase tooAzure Blob</span></span>
<span data-ttu-id="e1a90-189">Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e1a90-189">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="e1a90-190">Ukazují jak toocopy data z databáze Sybase databáze tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="e1a90-190">They show how toocopy data from Sybase database tooAzure Blob Storage.</span></span> <span data-ttu-id="e1a90-191">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="e1a90-191">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="e1a90-192">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="e1a90-192">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="e1a90-193">Propojené služby typu [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e1a90-193">A linked service of type [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="e1a90-194">Liked službu typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e1a90-194">A liked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="e1a90-195">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e1a90-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="e1a90-196">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e1a90-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="e1a90-197">Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="e1a90-197">hello [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="e1a90-198">Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob tooa databáze Sybase každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="e1a90-198">hello sample copies data from a query result in Sybase database tooa blob every hour.</span></span> <span data-ttu-id="e1a90-199">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-199">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="e1a90-200">Jako první krok nastavte Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-200">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="e1a90-201">Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="e1a90-201">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="e1a90-202">**Sybase propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="e1a90-202">**Sybase linked service:**</span></span>

```JSON
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
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

<span data-ttu-id="e1a90-203">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="e1a90-203">**Azure Blob storage linked service:**</span></span>

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

<span data-ttu-id="e1a90-204">**Sybase vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="e1a90-204">**Sybase input dataset:**</span></span>

<span data-ttu-id="e1a90-205">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v Sybase a obsahuje sloupec s názvem "časové razítko" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="e1a90-205">hello sample assumes you have created a table “MyTable” in Sybase and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="e1a90-206">Nastavení "externí": true informuje hello služba Data Factory, tato datová sada je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-206">Setting “external”: true informs hello Data Factory service that this dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span> <span data-ttu-id="e1a90-207">Všimněte si, že hello **typ** z hello propojené služby je nastavena na: **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="e1a90-207">Notice that hello **type** of hello linked service is set to: **RelationalTable**.</span></span>

```JSON
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
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

<span data-ttu-id="e1a90-208">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="e1a90-208">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="e1a90-209">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="e1a90-209">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="e1a90-210">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="e1a90-210">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="e1a90-211">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="e1a90-211">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```JSON
{
    "name": "AzureBlobSybaseDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="e1a90-212">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="e1a90-212">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="e1a90-213">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="e1a90-213">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="e1a90-214">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="e1a90-214">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="e1a90-215">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data z hello správce databáze. Objednávky tabulky v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="e1a90-215">hello SQL query specified for hello **query** property selects hello data from hello DBA.Orders table in hello database.</span></span>

```JSON
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from DBA.Orders"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "SybaseDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobSybaseDataSet"
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
                "name": "SybaseToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-sybase"></a><span data-ttu-id="e1a90-216">Mapování typu pro databázi Sybase</span><span class="sxs-lookup"><span data-stu-id="e1a90-216">Type mapping for Sybase</span></span>
<span data-ttu-id="e1a90-217">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku hello aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:</span><span class="sxs-lookup"><span data-stu-id="e1a90-217">As mentioned in hello [Data Movement Activities](data-factory-data-movement-activities.md) article, hello Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="e1a90-218">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="e1a90-218">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="e1a90-219">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="e1a90-219">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="e1a90-220">Sybase podporuje T-SQL a typy T-SQL.</span><span class="sxs-lookup"><span data-stu-id="e1a90-220">Sybase supports T-SQL and T-SQL types.</span></span> <span data-ttu-id="e1a90-221">Pro tabulku mapování z typu too.NET typy sql, najdete v části [konektor služby Azure SQL](data-factory-azure-sql-connector.md) článku.</span><span class="sxs-lookup"><span data-stu-id="e1a90-221">For a mapping table from sql types too.NET type, see [Azure SQL Connector](data-factory-azure-sql-connector.md) article.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="e1a90-222">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="e1a90-222">Map source toosink columns</span></span>
<span data-ttu-id="e1a90-223">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="e1a90-223">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="e1a90-224">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="e1a90-224">Repeatable read from relational sources</span></span>
<span data-ttu-id="e1a90-225">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="e1a90-225">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="e1a90-226">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="e1a90-226">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="e1a90-227">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="e1a90-227">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="e1a90-228">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="e1a90-228">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="e1a90-229">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="e1a90-229">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="e1a90-230">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="e1a90-230">Performance and Tuning</span></span>
<span data-ttu-id="e1a90-231">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="e1a90-231">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
