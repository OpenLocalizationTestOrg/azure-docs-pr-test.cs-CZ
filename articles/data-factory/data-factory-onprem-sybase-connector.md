---
title: "Přesun dat z databáze Sybase pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z databáze Sybase pomocí Azure Data Factory."
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
ms.openlocfilehash: 617e604b220b8bc1c452e67da83f733448e16c0b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-sybase-using-azure-data-factory"></a><span data-ttu-id="4826e-103">Přesun dat z databáze Sybase pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="4826e-103">Move data from Sybase using Azure Data Factory</span></span>
<span data-ttu-id="4826e-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z databáze Sybase na místě.</span><span class="sxs-lookup"><span data-stu-id="4826e-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises Sybase database.</span></span> <span data-ttu-id="4826e-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="4826e-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="4826e-106">Do úložiště dat žádné podporované podřízený může kopírovat data z úložiště dat místní databázi Sybase.</span><span class="sxs-lookup"><span data-stu-id="4826e-106">You can copy data from an on-premises Sybase data store to any supported sink data store.</span></span> <span data-ttu-id="4826e-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="4826e-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="4826e-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z jiného úložiště dat Sybase jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat do úložiště dat Sybase.</span><span class="sxs-lookup"><span data-stu-id="4826e-108">Data factory currently supports only moving data from a Sybase data store to other data stores, but not for moving data from other data stores to a Sybase data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4826e-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4826e-109">Prerequisites</span></span>
<span data-ttu-id="4826e-110">Služba data Factory podporuje připojení k místní databázi Sybase zdroje pomocí Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="4826e-110">Data Factory service supports connecting to on-premises Sybase sources using the Data Management Gateway.</span></span> <span data-ttu-id="4826e-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku se dozvíte o Brána pro správu dat a podrobné pokyny o nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="4826e-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="4826e-112">Vyžaduje se brána, i když je databáze Sybase hostované ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="4826e-112">Gateway is required even if the Sybase database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="4826e-113">Bránu můžete nainstalovat na stejný virtuální počítač IaaS jako úložiště dat nebo na jiný virtuální počítač, dokud brána se může připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="4826e-113">You can install the gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="4826e-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="4826e-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="4826e-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="4826e-115">Supported versions and installation</span></span>
<span data-ttu-id="4826e-116">Pro bránu pro správu dat pro připojení k databázi Sybase, je potřeba nainstalovat [zprostředkovatele dat pro databázi Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 nebo vyšší ve stejném systému jako brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="4826e-116">For Data Management Gateway to connect to the Sybase Database, you need to install the [data provider for Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 or above on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="4826e-117">Sybase verze 16 a vyšší je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="4826e-117">Sybase version 16 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="4826e-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="4826e-118">Getting started</span></span>
<span data-ttu-id="4826e-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4826e-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="4826e-120">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="4826e-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="4826e-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="4826e-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="4826e-122">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="4826e-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="4826e-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="4826e-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="4826e-124">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="4826e-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="4826e-125">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="4826e-125">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="4826e-126">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="4826e-126">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="4826e-127">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="4826e-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="4826e-128">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="4826e-128">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="4826e-129">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="4826e-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="4826e-130">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat Sybase na místě, naleznete v tématu [JSON příklad: kopírování dat z databáze Sybase do objektu Blob Azure](#json-example-copy-data-from-sybase-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4826e-130">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises Sybase data store, see [JSON example: Copy data from Sybase to Azure Blob](#json-example-copy-data-from-sybase-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="4826e-131">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k úložišti dat Sybase:</span><span class="sxs-lookup"><span data-stu-id="4826e-131">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Sybase data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="4826e-132">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="4826e-132">Linked service properties</span></span>
<span data-ttu-id="4826e-133">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro Sybase propojené služby.</span><span class="sxs-lookup"><span data-stu-id="4826e-133">The following table provides description for JSON elements specific to Sybase linked service.</span></span>

| <span data-ttu-id="4826e-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4826e-134">Property</span></span> | <span data-ttu-id="4826e-135">Popis</span><span class="sxs-lookup"><span data-stu-id="4826e-135">Description</span></span> | <span data-ttu-id="4826e-136">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4826e-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4826e-137">type</span><span class="sxs-lookup"><span data-stu-id="4826e-137">type</span></span> |<span data-ttu-id="4826e-138">Vlastnost typu musí být nastavena na: **OnPremisesSybase**</span><span class="sxs-lookup"><span data-stu-id="4826e-138">The type property must be set to: **OnPremisesSybase**</span></span> |<span data-ttu-id="4826e-139">Ano</span><span class="sxs-lookup"><span data-stu-id="4826e-139">Yes</span></span> |
| <span data-ttu-id="4826e-140">server</span><span class="sxs-lookup"><span data-stu-id="4826e-140">server</span></span> |<span data-ttu-id="4826e-141">Název serveru databáze Sybase.</span><span class="sxs-lookup"><span data-stu-id="4826e-141">Name of the Sybase server.</span></span> |<span data-ttu-id="4826e-142">Ano</span><span class="sxs-lookup"><span data-stu-id="4826e-142">Yes</span></span> |
| <span data-ttu-id="4826e-143">Databáze</span><span class="sxs-lookup"><span data-stu-id="4826e-143">database</span></span> |<span data-ttu-id="4826e-144">Název databáze Sybase.</span><span class="sxs-lookup"><span data-stu-id="4826e-144">Name of the Sybase database.</span></span> |<span data-ttu-id="4826e-145">Ano</span><span class="sxs-lookup"><span data-stu-id="4826e-145">Yes</span></span> |
| <span data-ttu-id="4826e-146">Schéma</span><span class="sxs-lookup"><span data-stu-id="4826e-146">schema</span></span> |<span data-ttu-id="4826e-147">Název schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="4826e-147">Name of the schema in the database.</span></span> |<span data-ttu-id="4826e-148">Ne</span><span class="sxs-lookup"><span data-stu-id="4826e-148">No</span></span> |
| <span data-ttu-id="4826e-149">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="4826e-149">authenticationType</span></span> |<span data-ttu-id="4826e-150">Typ ověřování používaný pro připojení k databázi Sybase.</span><span class="sxs-lookup"><span data-stu-id="4826e-150">Type of authentication used to connect to the Sybase database.</span></span> <span data-ttu-id="4826e-151">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4826e-151">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="4826e-152">Ano</span><span class="sxs-lookup"><span data-stu-id="4826e-152">Yes</span></span> |
| <span data-ttu-id="4826e-153">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="4826e-153">username</span></span> |<span data-ttu-id="4826e-154">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="4826e-154">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="4826e-155">Ne</span><span class="sxs-lookup"><span data-stu-id="4826e-155">No</span></span> |
| <span data-ttu-id="4826e-156">heslo</span><span class="sxs-lookup"><span data-stu-id="4826e-156">password</span></span> |<span data-ttu-id="4826e-157">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="4826e-157">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="4826e-158">Ne</span><span class="sxs-lookup"><span data-stu-id="4826e-158">No</span></span> |
| <span data-ttu-id="4826e-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="4826e-159">gatewayName</span></span> |<span data-ttu-id="4826e-160">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi Sybase.</span><span class="sxs-lookup"><span data-stu-id="4826e-160">Name of the gateway that the Data Factory service should use to connect to the on-premises Sybase database.</span></span> |<span data-ttu-id="4826e-161">Ano</span><span class="sxs-lookup"><span data-stu-id="4826e-161">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="4826e-162">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="4826e-162">Dataset properties</span></span>
<span data-ttu-id="4826e-163">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4826e-163">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="4826e-164">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="4826e-164">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="4826e-165">V rámci typeProperties části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="4826e-165">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="4826e-166">**Rámci typeProperties** části pro datovou sadu typu **RelationalTable** (což zahrnuje datová sada Sybase) má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="4826e-166">The **typeProperties** section for dataset of type **RelationalTable** (which includes Sybase dataset) has the following properties:</span></span>

| <span data-ttu-id="4826e-167">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4826e-167">Property</span></span> | <span data-ttu-id="4826e-168">Popis</span><span class="sxs-lookup"><span data-stu-id="4826e-168">Description</span></span> | <span data-ttu-id="4826e-169">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4826e-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4826e-170">tableName</span><span class="sxs-lookup"><span data-stu-id="4826e-170">tableName</span></span> |<span data-ttu-id="4826e-171">Název tabulky instance databáze Sybase na kterou odkazuje propojená služba.</span><span class="sxs-lookup"><span data-stu-id="4826e-171">Name of the table in the Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="4826e-172">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="4826e-172">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="4826e-173">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="4826e-173">Copy activity properties</span></span>
<span data-ttu-id="4826e-174">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4826e-174">For a full list of sections & properties available for defining activities, see [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="4826e-175">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="4826e-175">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="4826e-176">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="4826e-176">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="4826e-177">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="4826e-177">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="4826e-178">Pokud je zdroj typu **RelationalSource** (která zahrnuje Sybase), následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="4826e-178">When the source is of type **RelationalSource** (which includes Sybase), the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="4826e-179">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4826e-179">Property</span></span> | <span data-ttu-id="4826e-180">Popis</span><span class="sxs-lookup"><span data-stu-id="4826e-180">Description</span></span> | <span data-ttu-id="4826e-181">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="4826e-181">Allowed values</span></span> | <span data-ttu-id="4826e-182">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4826e-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4826e-183">query</span><span class="sxs-lookup"><span data-stu-id="4826e-183">query</span></span> |<span data-ttu-id="4826e-184">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="4826e-184">Use the custom query to read data.</span></span> |<span data-ttu-id="4826e-185">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="4826e-185">SQL query string.</span></span> <span data-ttu-id="4826e-186">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="4826e-186">For example: select * from MyTable.</span></span> |<span data-ttu-id="4826e-187">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="4826e-187">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-sybase-to-azure-blob"></a><span data-ttu-id="4826e-188">Příklad JSON: kopírování dat z databáze Sybase do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="4826e-188">JSON example: Copy data from Sybase to Azure Blob</span></span>
<span data-ttu-id="4826e-189">Následující příklad uvádí ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="4826e-189">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="4826e-190">Se ukazují, jak zkopírovat data z databáze Sybase do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="4826e-190">They show how to copy data from Sybase database to Azure Blob Storage.</span></span> <span data-ttu-id="4826e-191">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4826e-191">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="4826e-192">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="4826e-192">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="4826e-193">Propojené služby typu [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4826e-193">A linked service of type [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="4826e-194">Liked službu typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4826e-194">A liked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="4826e-195">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4826e-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="4826e-196">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4826e-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="4826e-197">[Kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="4826e-197">The [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="4826e-198">Ukázka zkopíruje data z výsledku dotazu v databázi Sybase do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4826e-198">The sample copies data from a query result in Sybase database to a blob every hour.</span></span> <span data-ttu-id="4826e-199">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="4826e-199">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="4826e-200">Jako první krok nastavte Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="4826e-200">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="4826e-201">Tyto pokyny jsou v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4826e-201">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="4826e-202">**Sybase propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="4826e-202">**Sybase linked service:**</span></span>

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

<span data-ttu-id="4826e-203">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="4826e-203">**Azure Blob storage linked service:**</span></span>

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

<span data-ttu-id="4826e-204">**Sybase vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="4826e-204">**Sybase input dataset:**</span></span>

<span data-ttu-id="4826e-205">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v Sybase a obsahuje sloupec s názvem "časové razítko" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="4826e-205">The sample assumes you have created a table “MyTable” in Sybase and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="4826e-206">Nastavení "externí": true informuje služba Data Factory, že tato datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="4826e-206">Setting “external”: true informs the Data Factory service that this dataset is external to the data factory and is not produced by an activity in the data factory.</span></span> <span data-ttu-id="4826e-207">Všimněte si, že **typ** propojené služby je nastavena na: **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="4826e-207">Notice that the **type** of the linked service is set to: **RelationalTable**.</span></span>

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

<span data-ttu-id="4826e-208">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="4826e-208">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="4826e-209">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="4826e-209">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="4826e-210">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="4826e-210">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="4826e-211">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="4826e-211">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="4826e-212">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="4826e-212">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="4826e-213">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4826e-213">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="4826e-214">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="4826e-214">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="4826e-215">Zadané pro dotaz SQL **dotazu** vlastnost vybere data ze správce databáze. Objednávky tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="4826e-215">The SQL query specified for the **query** property selects the data from the DBA.Orders table in the database.</span></span>

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

## <a name="type-mapping-for-sybase"></a><span data-ttu-id="4826e-216">Mapování typu pro databázi Sybase</span><span class="sxs-lookup"><span data-stu-id="4826e-216">Type mapping for Sybase</span></span>
<span data-ttu-id="4826e-217">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup krok 2:</span><span class="sxs-lookup"><span data-stu-id="4826e-217">As mentioned in the [Data Movement Activities](data-factory-data-movement-activities.md) article, the Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="4826e-218">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="4826e-218">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="4826e-219">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="4826e-219">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="4826e-220">Sybase podporuje T-SQL a typy T-SQL.</span><span class="sxs-lookup"><span data-stu-id="4826e-220">Sybase supports T-SQL and T-SQL types.</span></span> <span data-ttu-id="4826e-221">Tabulku mapování z typů sql pro typ formátu .NET, naleznete v části [konektor služby Azure SQL](data-factory-azure-sql-connector.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4826e-221">For a mapping table from sql types to .NET type, see [Azure SQL Connector](data-factory-azure-sql-connector.md) article.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="4826e-222">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="4826e-222">Map source to sink columns</span></span>
<span data-ttu-id="4826e-223">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="4826e-223">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="4826e-224">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="4826e-224">Repeatable read from relational sources</span></span>
<span data-ttu-id="4826e-225">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="4826e-225">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="4826e-226">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="4826e-226">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="4826e-227">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="4826e-227">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="4826e-228">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="4826e-228">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="4826e-229">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="4826e-229">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="4826e-230">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="4826e-230">Performance and Tuning</span></span>
<span data-ttu-id="4826e-231">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="4826e-231">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
