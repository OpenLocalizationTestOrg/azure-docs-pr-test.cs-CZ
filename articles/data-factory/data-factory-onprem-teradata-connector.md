---
title: "Přesun dat z Teradata pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o Teradata konektor pro službu Data Factory, který umožňuje přesun dat z databáze Teradata"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 98eb76d8-5f3d-4667-b76e-e59ed3eea3ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: 01edb32cd9e20d4199feac5b98a73aa06b74fec2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-teradata-using-azure-data-factory"></a><span data-ttu-id="21f2a-103">Přesun dat z Teradata pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="21f2a-103">Move data from Teradata using Azure Data Factory</span></span>
<span data-ttu-id="21f2a-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z databáze Teradata místně.</span><span class="sxs-lookup"><span data-stu-id="21f2a-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises Teradata database.</span></span> <span data-ttu-id="21f2a-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="21f2a-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="21f2a-106">Z úložiště dat Teradata místní může kopírovat data do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="21f2a-106">You can copy data from an on-premises Teradata data store to any supported sink data store.</span></span> <span data-ttu-id="21f2a-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="21f2a-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="21f2a-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z jiného úložiště dat Teradata k jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat do úložiště dat Teradata.</span><span class="sxs-lookup"><span data-stu-id="21f2a-108">Data factory currently supports only moving data from a Teradata data store to other data stores, but not for moving data from other data stores to a Teradata data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="21f2a-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="21f2a-109">Prerequisites</span></span>
<span data-ttu-id="21f2a-110">Objekt pro vytváření dat podporuje připojení k místním zdrojům Teradata prostřednictvím brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="21f2a-110">Data factory supports connecting to on-premises Teradata sources via the Data Management Gateway.</span></span> <span data-ttu-id="21f2a-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku se dozvíte o Brána pro správu dat a podrobné pokyny o nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="21f2a-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="21f2a-112">Vyžaduje se brána, i když Teradata je hostovaná ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="21f2a-112">Gateway is required even if the Teradata is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="21f2a-113">Bránu můžete nainstalovat na stejný virtuální počítač IaaS jako úložiště dat nebo na jiný virtuální počítač, dokud brána se může připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="21f2a-113">You can install the gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="21f2a-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="21f2a-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="21f2a-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="21f2a-115">Supported versions and installation</span></span>
<span data-ttu-id="21f2a-116">Pro bránu pro správu dat pro připojení k databázi Teradata, je potřeba nainstalovat [zprostředkovatel dat .NET pro Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) verzi 14 nebo vyšší ve stejném systému jako brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="21f2a-116">For Data Management Gateway to connect to the Teradata Database, you need to install the [.NET Data Provider for Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) version 14 or above on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="21f2a-117">Teradata verze 12 a vyšší je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="21f2a-117">Teradata version 12 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="21f2a-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="21f2a-118">Getting started</span></span>
<span data-ttu-id="21f2a-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="21f2a-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="21f2a-120">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="21f2a-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="21f2a-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="21f2a-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="21f2a-122">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="21f2a-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="21f2a-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="21f2a-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="21f2a-124">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="21f2a-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="21f2a-125">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="21f2a-125">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="21f2a-126">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="21f2a-126">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="21f2a-127">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="21f2a-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="21f2a-128">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="21f2a-128">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="21f2a-129">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="21f2a-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="21f2a-130">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat Teradata místní, naleznete v tématu [JSON příklad: kopírování dat z Teradata do objektu Blob Azure](#json-example-copy-data-from-teradata-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="21f2a-130">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises Teradata data store, see [JSON example: Copy data from Teradata to Azure Blob](#json-example-copy-data-from-teradata-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="21f2a-131">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k úložišti dat Teradata:</span><span class="sxs-lookup"><span data-stu-id="21f2a-131">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Teradata data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="21f2a-132">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="21f2a-132">Linked service properties</span></span>
<span data-ttu-id="21f2a-133">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro službu Teradata propojené služby.</span><span class="sxs-lookup"><span data-stu-id="21f2a-133">The following table provides description for JSON elements specific to Teradata linked service.</span></span>

| <span data-ttu-id="21f2a-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="21f2a-134">Property</span></span> | <span data-ttu-id="21f2a-135">Popis</span><span class="sxs-lookup"><span data-stu-id="21f2a-135">Description</span></span> | <span data-ttu-id="21f2a-136">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="21f2a-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="21f2a-137">type</span><span class="sxs-lookup"><span data-stu-id="21f2a-137">type</span></span> |<span data-ttu-id="21f2a-138">Vlastnost typu musí být nastavena na: **OnPremisesTeradata**</span><span class="sxs-lookup"><span data-stu-id="21f2a-138">The type property must be set to: **OnPremisesTeradata**</span></span> |<span data-ttu-id="21f2a-139">Ano</span><span class="sxs-lookup"><span data-stu-id="21f2a-139">Yes</span></span> |
| <span data-ttu-id="21f2a-140">server</span><span class="sxs-lookup"><span data-stu-id="21f2a-140">server</span></span> |<span data-ttu-id="21f2a-141">Název serveru Teradata.</span><span class="sxs-lookup"><span data-stu-id="21f2a-141">Name of the Teradata server.</span></span> |<span data-ttu-id="21f2a-142">Ano</span><span class="sxs-lookup"><span data-stu-id="21f2a-142">Yes</span></span> |
| <span data-ttu-id="21f2a-143">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="21f2a-143">authenticationType</span></span> |<span data-ttu-id="21f2a-144">Typ ověřování používaný pro připojení k databázi Teradata.</span><span class="sxs-lookup"><span data-stu-id="21f2a-144">Type of authentication used to connect to the Teradata database.</span></span> <span data-ttu-id="21f2a-145">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="21f2a-145">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="21f2a-146">Ano</span><span class="sxs-lookup"><span data-stu-id="21f2a-146">Yes</span></span> |
| <span data-ttu-id="21f2a-147">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="21f2a-147">username</span></span> |<span data-ttu-id="21f2a-148">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="21f2a-148">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="21f2a-149">Ne</span><span class="sxs-lookup"><span data-stu-id="21f2a-149">No</span></span> |
| <span data-ttu-id="21f2a-150">heslo</span><span class="sxs-lookup"><span data-stu-id="21f2a-150">password</span></span> |<span data-ttu-id="21f2a-151">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="21f2a-151">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="21f2a-152">Ne</span><span class="sxs-lookup"><span data-stu-id="21f2a-152">No</span></span> |
| <span data-ttu-id="21f2a-153">gatewayName</span><span class="sxs-lookup"><span data-stu-id="21f2a-153">gatewayName</span></span> |<span data-ttu-id="21f2a-154">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi Teradata.</span><span class="sxs-lookup"><span data-stu-id="21f2a-154">Name of the gateway that the Data Factory service should use to connect to the on-premises Teradata database.</span></span> |<span data-ttu-id="21f2a-155">Ano</span><span class="sxs-lookup"><span data-stu-id="21f2a-155">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="21f2a-156">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="21f2a-156">Dataset properties</span></span>
<span data-ttu-id="21f2a-157">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="21f2a-157">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="21f2a-158">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="21f2a-158">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="21f2a-159">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="21f2a-159">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="21f2a-160">Momentálně nejsou k dispozici žádné vlastnosti typu podporované pro datovou sadu Teradata.</span><span class="sxs-lookup"><span data-stu-id="21f2a-160">Currently, there are no type properties supported for the Teradata dataset.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="21f2a-161">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="21f2a-161">Copy activity properties</span></span>
<span data-ttu-id="21f2a-162">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="21f2a-162">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="21f2a-163">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="21f2a-163">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="21f2a-164">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="21f2a-164">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="21f2a-165">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="21f2a-165">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="21f2a-166">Pokud je zdroj typu **RelationalSource** (která zahrnuje Teradata), následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="21f2a-166">When the source is of type **RelationalSource** (which includes Teradata), the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="21f2a-167">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="21f2a-167">Property</span></span> | <span data-ttu-id="21f2a-168">Popis</span><span class="sxs-lookup"><span data-stu-id="21f2a-168">Description</span></span> | <span data-ttu-id="21f2a-169">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="21f2a-169">Allowed values</span></span> | <span data-ttu-id="21f2a-170">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="21f2a-170">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="21f2a-171">query</span><span class="sxs-lookup"><span data-stu-id="21f2a-171">query</span></span> |<span data-ttu-id="21f2a-172">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="21f2a-172">Use the custom query to read data.</span></span> |<span data-ttu-id="21f2a-173">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="21f2a-173">SQL query string.</span></span> <span data-ttu-id="21f2a-174">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="21f2a-174">For example: select * from MyTable.</span></span> |<span data-ttu-id="21f2a-175">Ano</span><span class="sxs-lookup"><span data-stu-id="21f2a-175">Yes</span></span> |

### <a name="json-example-copy-data-from-teradata-to-azure-blob"></a><span data-ttu-id="21f2a-176">Příklad JSON: kopírování dat z Teradata do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="21f2a-176">JSON example: Copy data from Teradata to Azure Blob</span></span>
<span data-ttu-id="21f2a-177">Následující příklad uvádí ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="21f2a-177">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="21f2a-178">Se ukazují, jak zkopírovat data z Teradata do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="21f2a-178">They show how to copy data from Teradata to Azure Blob Storage.</span></span> <span data-ttu-id="21f2a-179">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="21f2a-179">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="21f2a-180">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="21f2a-180">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="21f2a-181">Propojené služby typu [OnPremisesTeradata](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="21f2a-181">A linked service of type [OnPremisesTeradata](#linked-service-properties).</span></span>
2. <span data-ttu-id="21f2a-182">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="21f2a-182">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="21f2a-183">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="21f2a-183">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="21f2a-184">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="21f2a-184">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="21f2a-185">[Kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="21f2a-185">The [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="21f2a-186">Ukázka zkopíruje data z výsledku dotazu v databázi Teradata do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="21f2a-186">The sample copies data from a query result in Teradata database to a blob every hour.</span></span> <span data-ttu-id="21f2a-187">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="21f2a-187">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="21f2a-188">Jako první krok nastavte Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="21f2a-188">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="21f2a-189">Tyto pokyny jsou v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="21f2a-189">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="21f2a-190">**Teradata propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="21f2a-190">**Teradata linked service:**</span></span>

```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="21f2a-191">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="21f2a-191">**Azure Blob storage linked service:**</span></span>

```json
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

<span data-ttu-id="21f2a-192">**Teradata vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="21f2a-192">**Teradata input dataset:**</span></span>

<span data-ttu-id="21f2a-193">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v Teradata a obsahuje sloupec s názvem "časové razítko" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="21f2a-193">The sample assumes you have created a table “MyTable” in Teradata and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="21f2a-194">Nastavení "externí": true informuje služba Data Factory, že v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="21f2a-194">Setting “external”: true informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "TeradataDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {
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

<span data-ttu-id="21f2a-195">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="21f2a-195">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="21f2a-196">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="21f2a-196">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="21f2a-197">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="21f2a-197">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="21f2a-198">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="21f2a-198">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobTeradataDataSet",
    "properties": {
        "published": false,
        "location": {
            "type": "AzureBlobLocation",
            "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "linkedServiceName": "AzureStorageLinkedService"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
<span data-ttu-id="21f2a-199">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="21f2a-199">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="21f2a-200">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="21f2a-200">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="21f2a-201">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="21f2a-201">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="21f2a-202">Zadané pro dotaz SQL **dotazu** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="21f2a-202">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "TeradataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobTeradataDataSet"
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
                "name": "TeradataToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z",
        "isPaused": false
    }
}
```
## <a name="type-mapping-for-teradata"></a><span data-ttu-id="21f2a-203">Mapování typu pro Teradata</span><span class="sxs-lookup"><span data-stu-id="21f2a-203">Type mapping for Teradata</span></span>
<span data-ttu-id="21f2a-204">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup krok 2:</span><span class="sxs-lookup"><span data-stu-id="21f2a-204">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, the Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="21f2a-205">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="21f2a-205">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="21f2a-206">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="21f2a-206">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="21f2a-207">Při přesunu dat na Teradata, se používají následující mapování z typu Teradata na typ .NET.</span><span class="sxs-lookup"><span data-stu-id="21f2a-207">When moving data to Teradata, the following mappings are used from Teradata type to .NET type.</span></span>

| <span data-ttu-id="21f2a-208">Typ databáze Teradata</span><span class="sxs-lookup"><span data-stu-id="21f2a-208">Teradata Database type</span></span> | <span data-ttu-id="21f2a-209">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="21f2a-209">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="21f2a-210">Char</span><span class="sxs-lookup"><span data-stu-id="21f2a-210">Char</span></span> |<span data-ttu-id="21f2a-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-211">String</span></span> |
| <span data-ttu-id="21f2a-212">Datový typ CLOB</span><span class="sxs-lookup"><span data-stu-id="21f2a-212">Clob</span></span> |<span data-ttu-id="21f2a-213">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-213">String</span></span> |
| <span data-ttu-id="21f2a-214">Obrázek</span><span class="sxs-lookup"><span data-stu-id="21f2a-214">Graphic</span></span> |<span data-ttu-id="21f2a-215">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-215">String</span></span> |
| <span data-ttu-id="21f2a-216">VarChar</span><span class="sxs-lookup"><span data-stu-id="21f2a-216">VarChar</span></span> |<span data-ttu-id="21f2a-217">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-217">String</span></span> |
| <span data-ttu-id="21f2a-218">VarGraphic</span><span class="sxs-lookup"><span data-stu-id="21f2a-218">VarGraphic</span></span> |<span data-ttu-id="21f2a-219">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-219">String</span></span> |
| <span data-ttu-id="21f2a-220">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="21f2a-220">Blob</span></span> |<span data-ttu-id="21f2a-221">Byte]</span><span class="sxs-lookup"><span data-stu-id="21f2a-221">Byte[]</span></span> |
| <span data-ttu-id="21f2a-222">Bajtů</span><span class="sxs-lookup"><span data-stu-id="21f2a-222">Byte</span></span> |<span data-ttu-id="21f2a-223">Byte]</span><span class="sxs-lookup"><span data-stu-id="21f2a-223">Byte[]</span></span> |
| <span data-ttu-id="21f2a-224">VarByte</span><span class="sxs-lookup"><span data-stu-id="21f2a-224">VarByte</span></span> |<span data-ttu-id="21f2a-225">Byte]</span><span class="sxs-lookup"><span data-stu-id="21f2a-225">Byte[]</span></span> |
| <span data-ttu-id="21f2a-226">BigInt</span><span class="sxs-lookup"><span data-stu-id="21f2a-226">BigInt</span></span> |<span data-ttu-id="21f2a-227">Int64</span><span class="sxs-lookup"><span data-stu-id="21f2a-227">Int64</span></span> |
| <span data-ttu-id="21f2a-228">ByteInt</span><span class="sxs-lookup"><span data-stu-id="21f2a-228">ByteInt</span></span> |<span data-ttu-id="21f2a-229">Int16</span><span class="sxs-lookup"><span data-stu-id="21f2a-229">Int16</span></span> |
| <span data-ttu-id="21f2a-230">Decimal</span><span class="sxs-lookup"><span data-stu-id="21f2a-230">Decimal</span></span> |<span data-ttu-id="21f2a-231">Decimal</span><span class="sxs-lookup"><span data-stu-id="21f2a-231">Decimal</span></span> |
| <span data-ttu-id="21f2a-232">Double</span><span class="sxs-lookup"><span data-stu-id="21f2a-232">Double</span></span> |<span data-ttu-id="21f2a-233">Double</span><span class="sxs-lookup"><span data-stu-id="21f2a-233">Double</span></span> |
| <span data-ttu-id="21f2a-234">Integer</span><span class="sxs-lookup"><span data-stu-id="21f2a-234">Integer</span></span> |<span data-ttu-id="21f2a-235">Int32</span><span class="sxs-lookup"><span data-stu-id="21f2a-235">Int32</span></span> |
| <span data-ttu-id="21f2a-236">Číslo</span><span class="sxs-lookup"><span data-stu-id="21f2a-236">Number</span></span> |<span data-ttu-id="21f2a-237">Double</span><span class="sxs-lookup"><span data-stu-id="21f2a-237">Double</span></span> |
| <span data-ttu-id="21f2a-238">SmallInt</span><span class="sxs-lookup"><span data-stu-id="21f2a-238">SmallInt</span></span> |<span data-ttu-id="21f2a-239">Int16</span><span class="sxs-lookup"><span data-stu-id="21f2a-239">Int16</span></span> |
| <span data-ttu-id="21f2a-240">Datum</span><span class="sxs-lookup"><span data-stu-id="21f2a-240">Date</span></span> |<span data-ttu-id="21f2a-241">Data a času</span><span class="sxs-lookup"><span data-stu-id="21f2a-241">DateTime</span></span> |
| <span data-ttu-id="21f2a-242">Čas</span><span class="sxs-lookup"><span data-stu-id="21f2a-242">Time</span></span> |<span data-ttu-id="21f2a-243">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-243">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-244">Čas s časovým pásmem</span><span class="sxs-lookup"><span data-stu-id="21f2a-244">Time With Time Zone</span></span> |<span data-ttu-id="21f2a-245">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-245">String</span></span> |
| <span data-ttu-id="21f2a-246">časové razítko</span><span class="sxs-lookup"><span data-stu-id="21f2a-246">Timestamp</span></span> |<span data-ttu-id="21f2a-247">Data a času</span><span class="sxs-lookup"><span data-stu-id="21f2a-247">DateTime</span></span> |
| <span data-ttu-id="21f2a-248">Časové razítko s časovým pásmem</span><span class="sxs-lookup"><span data-stu-id="21f2a-248">Timestamp With Time Zone</span></span> |<span data-ttu-id="21f2a-249">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="21f2a-249">DateTimeOffset</span></span> |
| <span data-ttu-id="21f2a-250">Interval den</span><span class="sxs-lookup"><span data-stu-id="21f2a-250">Interval Day</span></span> |<span data-ttu-id="21f2a-251">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-251">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-252">Interval den a hodina</span><span class="sxs-lookup"><span data-stu-id="21f2a-252">Interval Day To Hour</span></span> |<span data-ttu-id="21f2a-253">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-253">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-254">Denní interval minuty.</span><span class="sxs-lookup"><span data-stu-id="21f2a-254">Interval Day To Minute</span></span> |<span data-ttu-id="21f2a-255">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-255">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-256">Denní interval sekundy.</span><span class="sxs-lookup"><span data-stu-id="21f2a-256">Interval Day To Second</span></span> |<span data-ttu-id="21f2a-257">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-257">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-258">Interval hodinu</span><span class="sxs-lookup"><span data-stu-id="21f2a-258">Interval Hour</span></span> |<span data-ttu-id="21f2a-259">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-259">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-260">Interval hodiny, minuty.</span><span class="sxs-lookup"><span data-stu-id="21f2a-260">Interval Hour To Minute</span></span> |<span data-ttu-id="21f2a-261">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-261">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-262">Interval hodinu sekundu</span><span class="sxs-lookup"><span data-stu-id="21f2a-262">Interval Hour To Second</span></span> |<span data-ttu-id="21f2a-263">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-263">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-264">Interval minutu</span><span class="sxs-lookup"><span data-stu-id="21f2a-264">Interval Minute</span></span> |<span data-ttu-id="21f2a-265">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-265">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-266">Interval minuty, sekundy.</span><span class="sxs-lookup"><span data-stu-id="21f2a-266">Interval Minute To Second</span></span> |<span data-ttu-id="21f2a-267">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-267">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-268">Interval druhý</span><span class="sxs-lookup"><span data-stu-id="21f2a-268">Interval Second</span></span> |<span data-ttu-id="21f2a-269">Časový interval</span><span class="sxs-lookup"><span data-stu-id="21f2a-269">TimeSpan</span></span> |
| <span data-ttu-id="21f2a-270">Interval roku</span><span class="sxs-lookup"><span data-stu-id="21f2a-270">Interval Year</span></span> |<span data-ttu-id="21f2a-271">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-271">String</span></span> |
| <span data-ttu-id="21f2a-272">Interval rok, měsíc</span><span class="sxs-lookup"><span data-stu-id="21f2a-272">Interval Year To Month</span></span> |<span data-ttu-id="21f2a-273">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-273">String</span></span> |
| <span data-ttu-id="21f2a-274">Interval měsíc</span><span class="sxs-lookup"><span data-stu-id="21f2a-274">Interval Month</span></span> |<span data-ttu-id="21f2a-275">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-275">String</span></span> |
| <span data-ttu-id="21f2a-276">Period(Date)</span><span class="sxs-lookup"><span data-stu-id="21f2a-276">Period(Date)</span></span> |<span data-ttu-id="21f2a-277">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-277">String</span></span> |
| <span data-ttu-id="21f2a-278">Period(Time)</span><span class="sxs-lookup"><span data-stu-id="21f2a-278">Period(Time)</span></span> |<span data-ttu-id="21f2a-279">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-279">String</span></span> |
| <span data-ttu-id="21f2a-280">Období (čas s časovým pásmem)</span><span class="sxs-lookup"><span data-stu-id="21f2a-280">Period(Time With Time Zone)</span></span> |<span data-ttu-id="21f2a-281">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-281">String</span></span> |
| <span data-ttu-id="21f2a-282">Period(Timestamp)</span><span class="sxs-lookup"><span data-stu-id="21f2a-282">Period(Timestamp)</span></span> |<span data-ttu-id="21f2a-283">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-283">String</span></span> |
| <span data-ttu-id="21f2a-284">Období (časové razítko s časovým pásmem)</span><span class="sxs-lookup"><span data-stu-id="21f2a-284">Period(Timestamp With Time Zone)</span></span> |<span data-ttu-id="21f2a-285">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-285">String</span></span> |
| <span data-ttu-id="21f2a-286">XML</span><span class="sxs-lookup"><span data-stu-id="21f2a-286">Xml</span></span> |<span data-ttu-id="21f2a-287">Řetězec</span><span class="sxs-lookup"><span data-stu-id="21f2a-287">String</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="21f2a-288">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="21f2a-288">Map source to sink columns</span></span>
<span data-ttu-id="21f2a-289">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="21f2a-289">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="21f2a-290">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="21f2a-290">Repeatable read from relational sources</span></span>
<span data-ttu-id="21f2a-291">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="21f2a-291">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="21f2a-292">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="21f2a-292">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="21f2a-293">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="21f2a-293">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="21f2a-294">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="21f2a-294">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="21f2a-295">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="21f2a-295">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="21f2a-296">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="21f2a-296">Performance and Tuning</span></span>
<span data-ttu-id="21f2a-297">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="21f2a-297">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
