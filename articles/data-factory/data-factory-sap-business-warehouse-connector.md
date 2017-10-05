---
title: "Přesun dat z SAP Business Warehouse pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z SAP Business Warehouse pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: 220ccc8b94797880d335385046001c5f3b17c862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a><span data-ttu-id="9eae8-103">Přesun dat z SAP Business Warehouse pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="9eae8-103">Move data From SAP Business Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="9eae8-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z místní SAP Business Warehouse (BW).</span><span class="sxs-lookup"><span data-stu-id="9eae8-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises SAP Business Warehouse (BW).</span></span> <span data-ttu-id="9eae8-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="9eae8-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="9eae8-106">Z místního úložiště dat SAP Business Warehouse může kopírovat data do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="9eae8-106">You can copy data from an on-premises SAP Business Warehouse data store to any supported sink data store.</span></span> <span data-ttu-id="9eae8-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="9eae8-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="9eae8-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z SAP Business Warehouse jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat do SAP Business Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9eae8-108">Data factory currently supports only moving data from an SAP Business Warehouse to other data stores, but not for moving data from other data stores to an SAP Business Warehouse.</span></span> 

## <a name="supported-versions-and-installation"></a><span data-ttu-id="9eae8-109">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="9eae8-109">Supported versions and installation</span></span>
<span data-ttu-id="9eae8-110">Tento konektor podporuje verze SAP Business Warehouse 7.x.</span><span class="sxs-lookup"><span data-stu-id="9eae8-110">This connector supports SAP Business Warehouse version 7.x.</span></span> <span data-ttu-id="9eae8-111">Podporuje kopírování dat z InfoCubes a QueryCubes (včetně BEx dotazy) pomocí dotazů MDX.</span><span class="sxs-lookup"><span data-stu-id="9eae8-111">It supports copying data from InfoCubes and QueryCubes (including BEx queries) using MDX queries.</span></span>

<span data-ttu-id="9eae8-112">Pokud chcete povolit připojení k instanci SAP BW, nainstalujte následující součásti:</span><span class="sxs-lookup"><span data-stu-id="9eae8-112">To enable the connectivity to the SAP BW instance, install the following components:</span></span>
- <span data-ttu-id="9eae8-113">**Brána pro správu dat**: podporuje služby Data Factory připojení k datům místní úložiště (včetně SAP Business Warehouse) pomocí součásti názvem Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="9eae8-113">**Data Management Gateway**: Data Factory service supports connecting to on-premises data stores (including SAP Business Warehouse) using a component called Data Management Gateway.</span></span> <span data-ttu-id="9eae8-114">Další informace o Brána pro správu dat a podrobné pokyny pro nastavení brány najdete v tématu [přesouvání dat mezi místní data uložit do cloudu úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9eae8-114">To learn about Data Management Gateway and step-by-step instructions for setting up the gateway, see [Moving data between on-premises data store to cloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="9eae8-115">Vyžaduje se brána, i když SAP Business Warehouse je hostovaná ve virtuálním počítači Azure IaaS (VM).</span><span class="sxs-lookup"><span data-stu-id="9eae8-115">Gateway is required even if the SAP Business Warehouse is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="9eae8-116">Bránu můžete nainstalovat na stejný virtuální počítač jako úložiště dat nebo na jiný virtuální počítač, dokud brána se může připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="9eae8-116">You can install the gateway on the same VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>
- <span data-ttu-id="9eae8-117">**Knihovna SAP NetWeaver** na počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="9eae8-117">**SAP NetWeaver library** on the gateway machine.</span></span> <span data-ttu-id="9eae8-118">Knihovna SAP Netweaver získáte od správce SAP, nebo přímo z [SAP služby Stažení softwaru](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="9eae8-118">You can get the SAP Netweaver library from your SAP administrator, or directly from the [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="9eae8-119">Vyhledejte **&#1025361; Poznámka SAP** získat umístění stahování na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="9eae8-119">Search for the **SAP Note #1025361** to get the download location for the most recent version.</span></span> <span data-ttu-id="9eae8-120">Ujistěte se, že architektura pro knihovnu SAP NetWeaver (32bitová nebo 64bitová verze) odpovídá vaší instalace brány.</span><span class="sxs-lookup"><span data-stu-id="9eae8-120">Make sure that the architecture for the SAP NetWeaver library (32-bit or 64-bit) matches your gateway installation.</span></span> <span data-ttu-id="9eae8-121">Nainstalujte všechny soubory, které jsou součástí sady SDK SAP NetWeaver RFC podle Poznámka SAP.</span><span class="sxs-lookup"><span data-stu-id="9eae8-121">Then install all files included in the SAP NetWeaver RFC SDK according to the SAP Note.</span></span> <span data-ttu-id="9eae8-122">Knihovna SAP NetWeaver jsou také obsaženy v nástrojích klienta SAP instalace.</span><span class="sxs-lookup"><span data-stu-id="9eae8-122">The SAP NetWeaver library is also included in the SAP Client Tools installation.</span></span>

> [!TIP]
> <span data-ttu-id="9eae8-123">Uveďte knihovny DLL do složky system32 extrahována ze sady SDK NetWeaver RFC.</span><span class="sxs-lookup"><span data-stu-id="9eae8-123">Put the dlls extracted from the NetWeaver RFC SDK into system32 folder.</span></span>

## <a name="getting-started"></a><span data-ttu-id="9eae8-124">Začínáme</span><span class="sxs-lookup"><span data-stu-id="9eae8-124">Getting started</span></span>
<span data-ttu-id="9eae8-125">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9eae8-125">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="9eae8-126">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="9eae8-126">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="9eae8-127">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="9eae8-127">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="9eae8-128">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="9eae8-128">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="9eae8-129">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="9eae8-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="9eae8-130">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="9eae8-130">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="9eae8-131">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="9eae8-131">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="9eae8-132">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="9eae8-132">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="9eae8-133">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="9eae8-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="9eae8-134">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="9eae8-134">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="9eae8-135">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="9eae8-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="9eae8-136">Ukázka s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z místní SAP Business Warehouse, najdete v části [JSON příklad: kopírování dat z SAP Business Warehouse do Azure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9eae8-136">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises SAP Business Warehouse, see [JSON example: Copy data from SAP Business Warehouse to Azure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="9eae8-137">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k úložišti dat SAP BW:</span><span class="sxs-lookup"><span data-stu-id="9eae8-137">The following sections provide details about JSON properties that are used to define Data Factory entities specific to an SAP BW data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="9eae8-138">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="9eae8-138">Linked service properties</span></span>
<span data-ttu-id="9eae8-139">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro SAP Business Warehouse (BW) propojené služby.</span><span class="sxs-lookup"><span data-stu-id="9eae8-139">The following table provides description for JSON elements specific to SAP Business Warehouse (BW) linked service.</span></span>

<span data-ttu-id="9eae8-140">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9eae8-140">Property</span></span> | <span data-ttu-id="9eae8-141">Popis</span><span class="sxs-lookup"><span data-stu-id="9eae8-141">Description</span></span> | <span data-ttu-id="9eae8-142">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="9eae8-142">Allowed values</span></span> | <span data-ttu-id="9eae8-143">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9eae8-143">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="9eae8-144">server</span><span class="sxs-lookup"><span data-stu-id="9eae8-144">server</span></span> | <span data-ttu-id="9eae8-145">Název serveru, na kterém se nachází instance SAP BW.</span><span class="sxs-lookup"><span data-stu-id="9eae8-145">Name of the server on which the SAP BW instance resides.</span></span> | <span data-ttu-id="9eae8-146">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-146">string</span></span> | <span data-ttu-id="9eae8-147">Ano</span><span class="sxs-lookup"><span data-stu-id="9eae8-147">Yes</span></span>
<span data-ttu-id="9eae8-148">systemNumber</span><span class="sxs-lookup"><span data-stu-id="9eae8-148">systemNumber</span></span> | <span data-ttu-id="9eae8-149">Číslo systému SAP BW systému.</span><span class="sxs-lookup"><span data-stu-id="9eae8-149">System number of the SAP BW system.</span></span> | <span data-ttu-id="9eae8-150">Desetinné číslo letopočty řetězec.</span><span class="sxs-lookup"><span data-stu-id="9eae8-150">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="9eae8-151">Ano</span><span class="sxs-lookup"><span data-stu-id="9eae8-151">Yes</span></span>
<span data-ttu-id="9eae8-152">clientId</span><span class="sxs-lookup"><span data-stu-id="9eae8-152">clientId</span></span> | <span data-ttu-id="9eae8-153">ID klienta v systému SAP W klienta.</span><span class="sxs-lookup"><span data-stu-id="9eae8-153">Client ID of the client in the SAP W system.</span></span> | <span data-ttu-id="9eae8-154">Tři číslice desítkové číslo řetězec.</span><span class="sxs-lookup"><span data-stu-id="9eae8-154">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="9eae8-155">Ano</span><span class="sxs-lookup"><span data-stu-id="9eae8-155">Yes</span></span>
<span data-ttu-id="9eae8-156">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="9eae8-156">username</span></span> | <span data-ttu-id="9eae8-157">Jméno uživatele, který má přístup k serveru SAP</span><span class="sxs-lookup"><span data-stu-id="9eae8-157">Name of the user who has access to the SAP server</span></span> | <span data-ttu-id="9eae8-158">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-158">string</span></span> | <span data-ttu-id="9eae8-159">Ano</span><span class="sxs-lookup"><span data-stu-id="9eae8-159">Yes</span></span>
<span data-ttu-id="9eae8-160">heslo</span><span class="sxs-lookup"><span data-stu-id="9eae8-160">password</span></span> | <span data-ttu-id="9eae8-161">Heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="9eae8-161">Password for the user.</span></span> | <span data-ttu-id="9eae8-162">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-162">string</span></span> | <span data-ttu-id="9eae8-163">Ano</span><span class="sxs-lookup"><span data-stu-id="9eae8-163">Yes</span></span>
<span data-ttu-id="9eae8-164">gatewayName</span><span class="sxs-lookup"><span data-stu-id="9eae8-164">gatewayName</span></span> | <span data-ttu-id="9eae8-165">Název brány, kterou služba Data Factory měla použít pro připojení k místní instanci SAP BW.</span><span class="sxs-lookup"><span data-stu-id="9eae8-165">Name of the gateway that the Data Factory service should use to connect to the on-premises SAP BW instance.</span></span> | <span data-ttu-id="9eae8-166">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-166">string</span></span> | <span data-ttu-id="9eae8-167">Ano</span><span class="sxs-lookup"><span data-stu-id="9eae8-167">Yes</span></span>
<span data-ttu-id="9eae8-168">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="9eae8-168">encryptedCredential</span></span> | <span data-ttu-id="9eae8-169">Řetězec šifrovaný přihlašovací údaj.</span><span class="sxs-lookup"><span data-stu-id="9eae8-169">The encrypted credential string.</span></span> | <span data-ttu-id="9eae8-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-170">string</span></span> | <span data-ttu-id="9eae8-171">Ne</span><span class="sxs-lookup"><span data-stu-id="9eae8-171">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="9eae8-172">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="9eae8-172">Dataset properties</span></span>
<span data-ttu-id="9eae8-173">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9eae8-173">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="9eae8-174">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="9eae8-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="9eae8-175">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="9eae8-175">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="9eae8-176">Nejsou žádné vlastnosti specifické pro typ podporované pro SAP BW datovou sadu typu **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="9eae8-176">There are no type-specific properties supported for the SAP BW dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="9eae8-177">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="9eae8-177">Copy activity properties</span></span>
<span data-ttu-id="9eae8-178">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9eae8-178">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="9eae8-179">Vlastnosti, například název, popis, vstupní a výstupní tabulky, jsou zásady jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="9eae8-179">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="9eae8-180">Vzhledem k tomu, vlastnosti dostupné ve **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="9eae8-180">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="9eae8-181">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="9eae8-181">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="9eae8-182">Pokud je zdroj v aktivitě kopírování typu **RelationalSource** (která zahrnuje SAP BW), následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="9eae8-182">When source in copy activity is of type **RelationalSource** (which includes SAP BW), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="9eae8-183">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9eae8-183">Property</span></span> | <span data-ttu-id="9eae8-184">Popis</span><span class="sxs-lookup"><span data-stu-id="9eae8-184">Description</span></span> | <span data-ttu-id="9eae8-185">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="9eae8-185">Allowed values</span></span> | <span data-ttu-id="9eae8-186">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9eae8-186">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9eae8-187">query</span><span class="sxs-lookup"><span data-stu-id="9eae8-187">query</span></span> | <span data-ttu-id="9eae8-188">Určuje dotaz MDX číst data z instance SAP BW.</span><span class="sxs-lookup"><span data-stu-id="9eae8-188">Specifies the MDX query to read data from the SAP BW instance.</span></span> | <span data-ttu-id="9eae8-189">Dotaz MDX.</span><span class="sxs-lookup"><span data-stu-id="9eae8-189">MDX query.</span></span> | <span data-ttu-id="9eae8-190">Ano</span><span class="sxs-lookup"><span data-stu-id="9eae8-190">Yes</span></span> |


## <a name="json-example-copy-data-from-sap-business-warehouse-to-azure-blob"></a><span data-ttu-id="9eae8-191">Příklad JSON: kopírování dat z SAP Business Warehouse do objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="9eae8-191">JSON example: Copy data from SAP Business Warehouse to Azure Blob</span></span>
<span data-ttu-id="9eae8-192">Následující příklad uvádí ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9eae8-192">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="9eae8-193">Tento příklad ukazuje, jak ke zkopírování dat z místní SAP Business Warehouse do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="9eae8-193">This sample shows how to copy data from an on-premises SAP Business Warehouse to an Azure Blob Storage.</span></span> <span data-ttu-id="9eae8-194">Však můžete zkopírovat data **přímo** žádnému jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="9eae8-194">However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="9eae8-195">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="9eae8-195">This sample provides JSON snippets.</span></span> <span data-ttu-id="9eae8-196">Podrobné pokyny pro vytvoření objektu pro vytváření dat neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="9eae8-196">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="9eae8-197">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="9eae8-197">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="9eae8-198">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="9eae8-198">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="9eae8-199">Propojené služby typu [SapBw](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9eae8-199">A linked service of type [SapBw](#linked-service-properties).</span></span>
2. <span data-ttu-id="9eae8-200">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9eae8-200">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="9eae8-201">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9eae8-201">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="9eae8-202">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9eae8-202">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="9eae8-203">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="9eae8-203">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="9eae8-204">Ukázka zkopíruje data z instance SAP Business Warehouse do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="9eae8-204">The sample copies data from an SAP Business Warehouse instance to an Azure blob hourly.</span></span> <span data-ttu-id="9eae8-205">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="9eae8-205">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="9eae8-206">Jako první krok nastavte Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="9eae8-206">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="9eae8-207">Tyto pokyny jsou v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9eae8-207">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-business-warehouse-linked-service"></a><span data-ttu-id="9eae8-208">SAP Business Warehouse propojená služba</span><span class="sxs-lookup"><span data-stu-id="9eae8-208">SAP Business Warehouse linked service</span></span>
<span data-ttu-id="9eae8-209">Tato propojená služba propojuje SAP BW instanci objektu pro vytváření dat..</span><span class="sxs-lookup"><span data-stu-id="9eae8-209">This linked service links your SAP BW instance to the data factory.</span></span> <span data-ttu-id="9eae8-210">Vlastnost typ nastavena na **SapBw**.</span><span class="sxs-lookup"><span data-stu-id="9eae8-210">The type property is set to **SapBw**.</span></span> <span data-ttu-id="9eae8-211">Rámci typeProperties část obsahuje informace o připojení pro SAP BW instanci.</span><span class="sxs-lookup"><span data-stu-id="9eae8-211">The typeProperties section provides connection information for the SAP BW instance.</span></span> 

```json
{
    "name": "SapBwLinkedService",
    "properties":
    {
        "type": "SapBw",
        "typeProperties":
        {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="9eae8-212">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="9eae8-212">Azure Storage linked service</span></span>
<span data-ttu-id="9eae8-213">Tato propojená služba propojuje účet úložiště Azure pro vytváření dat..</span><span class="sxs-lookup"><span data-stu-id="9eae8-213">This linked service links your Azure Storage account to the data factory.</span></span> <span data-ttu-id="9eae8-214">Vlastnost typ nastavena na **azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="9eae8-214">The type property is set to **AzureStorage**.</span></span> <span data-ttu-id="9eae8-215">Rámci typeProperties část obsahuje informace o připojení pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9eae8-215">The typeProperties section provides connection information for the Azure Storage account.</span></span>

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

### <a name="sap-bw-input-dataset"></a><span data-ttu-id="9eae8-216">Vstupní datové sady SAP BW</span><span class="sxs-lookup"><span data-stu-id="9eae8-216">SAP BW input dataset</span></span>
<span data-ttu-id="9eae8-217">Tato datová sada definuje datovou sadu SAP Business Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9eae8-217">This dataset defines the SAP Business Warehouse dataset.</span></span> <span data-ttu-id="9eae8-218">Nastavte typ objektu pro vytváření dat datové sady, která **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="9eae8-218">You set the type of the Data Factory dataset to **RelationalTable**.</span></span> <span data-ttu-id="9eae8-219">V současné době nezadáte jakékoli vlastnosti specifické pro typ pro datové sadě služby SAP BW.</span><span class="sxs-lookup"><span data-stu-id="9eae8-219">Currently, you do not specify any type-specific properties for an SAP BW dataset.</span></span> <span data-ttu-id="9eae8-220">Dotaz v definici aktivity kopírování Určuje, jaká data načíst z instance SAP BW.</span><span class="sxs-lookup"><span data-stu-id="9eae8-220">The query in the Copy Activity definition specifies what data to read from the SAP BW instance.</span></span> 

<span data-ttu-id="9eae8-221">Služba Data Factory nastavení externí vlastnost na hodnotu true informuje, že v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="9eae8-221">Setting external property to true informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

<span data-ttu-id="9eae8-222">Frekvence a intervalu vlastnosti definuje plán.</span><span class="sxs-lookup"><span data-stu-id="9eae8-222">Frequency and interval properties defines the schedule.</span></span> <span data-ttu-id="9eae8-223">V takovém případě dat je pro čtení z instance SAP BW každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="9eae8-223">In this case, the data is read from the SAP BW instance hourly.</span></span> 

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```



### <a name="azure-blob-output-dataset"></a><span data-ttu-id="9eae8-224">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="9eae8-224">Azure Blob output dataset</span></span>
<span data-ttu-id="9eae8-225">Tato datová sada definuje výstupní datovou sadu objektu Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="9eae8-225">This dataset defines the output Azure Blob dataset.</span></span> <span data-ttu-id="9eae8-226">Vlastnost typ nastavena na hodnotu AzureBlob.</span><span class="sxs-lookup"><span data-stu-id="9eae8-226">The type property is set to AzureBlob.</span></span> <span data-ttu-id="9eae8-227">V rámci typeProperties části poskytuje, které jsou uložená data zkopírovány z instance SAP BW.</span><span class="sxs-lookup"><span data-stu-id="9eae8-227">The typeProperties section provides where the data copied from the SAP BW instance is stored.</span></span> <span data-ttu-id="9eae8-228">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="9eae8-228">The data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="9eae8-229">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="9eae8-229">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="9eae8-230">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="9eae8-230">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sapbw/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="9eae8-231">Kanál s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="9eae8-231">Pipeline with Copy activity</span></span>
<span data-ttu-id="9eae8-232">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="9eae8-232">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="9eae8-233">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource** (pro SAP BW zdroj) a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="9eae8-233">In the pipeline JSON definition, the **source** type is set to **RelationalSource** (for SAP BW source) and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="9eae8-234">Dotaz zadaný pro **dotazu** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="9eae8-234">The query specified for the **query** property selects the data in the past hour to copy.</span></span>

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<MDX query for SAP BW>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapBwDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
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
                "name": "SapBwToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```



### <a name="type-mapping-for-sap-bw"></a><span data-ttu-id="9eae8-235">Mapování typu pro SAP BW</span><span class="sxs-lookup"><span data-stu-id="9eae8-235">Type mapping for SAP BW</span></span>
<span data-ttu-id="9eae8-236">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup ve dvou krocích:</span><span class="sxs-lookup"><span data-stu-id="9eae8-236">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="9eae8-237">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="9eae8-237">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="9eae8-238">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="9eae8-238">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="9eae8-239">Při přesouvání dat od SAP BW, se používají následující mapování z typů SAP BW na typy .NET.</span><span class="sxs-lookup"><span data-stu-id="9eae8-239">When moving data from SAP BW, the following mappings are used from SAP BW types to .NET types.</span></span>

<span data-ttu-id="9eae8-240">Typ dat ve slovníku ABAP</span><span class="sxs-lookup"><span data-stu-id="9eae8-240">Data type in the ABAP Dictionary</span></span> | <span data-ttu-id="9eae8-241">Datový typ rozhraní .net</span><span class="sxs-lookup"><span data-stu-id="9eae8-241">.Net Data Type</span></span>
-------------------------------- | --------------
<span data-ttu-id="9eae8-242">ACCP</span><span class="sxs-lookup"><span data-stu-id="9eae8-242">ACCP</span></span> |  <span data-ttu-id="9eae8-243">celá čísla</span><span class="sxs-lookup"><span data-stu-id="9eae8-243">Int</span></span>
<span data-ttu-id="9eae8-244">CHAR –</span><span class="sxs-lookup"><span data-stu-id="9eae8-244">CHAR</span></span> | <span data-ttu-id="9eae8-245">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-245">String</span></span>
<span data-ttu-id="9eae8-246">CLNT</span><span class="sxs-lookup"><span data-stu-id="9eae8-246">CLNT</span></span> | <span data-ttu-id="9eae8-247">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-247">String</span></span>
<span data-ttu-id="9eae8-248">AKTUÁLNÍ</span><span class="sxs-lookup"><span data-stu-id="9eae8-248">CURR</span></span> | <span data-ttu-id="9eae8-249">Decimal</span><span class="sxs-lookup"><span data-stu-id="9eae8-249">Decimal</span></span>
<span data-ttu-id="9eae8-250">CUKY</span><span class="sxs-lookup"><span data-stu-id="9eae8-250">CUKY</span></span> | <span data-ttu-id="9eae8-251">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-251">String</span></span>
<span data-ttu-id="9eae8-252">DEC</span><span class="sxs-lookup"><span data-stu-id="9eae8-252">DEC</span></span> | <span data-ttu-id="9eae8-253">Decimal</span><span class="sxs-lookup"><span data-stu-id="9eae8-253">Decimal</span></span>
<span data-ttu-id="9eae8-254">FLTP</span><span class="sxs-lookup"><span data-stu-id="9eae8-254">FLTP</span></span> | <span data-ttu-id="9eae8-255">Double</span><span class="sxs-lookup"><span data-stu-id="9eae8-255">Double</span></span>
<span data-ttu-id="9eae8-256">INT1</span><span class="sxs-lookup"><span data-stu-id="9eae8-256">INT1</span></span> | <span data-ttu-id="9eae8-257">Bajtů</span><span class="sxs-lookup"><span data-stu-id="9eae8-257">Byte</span></span>
<span data-ttu-id="9eae8-258">INT2</span><span class="sxs-lookup"><span data-stu-id="9eae8-258">INT2</span></span> | <span data-ttu-id="9eae8-259">Int16</span><span class="sxs-lookup"><span data-stu-id="9eae8-259">Int16</span></span>
<span data-ttu-id="9eae8-260">INT4</span><span class="sxs-lookup"><span data-stu-id="9eae8-260">INT4</span></span> | <span data-ttu-id="9eae8-261">celá čísla</span><span class="sxs-lookup"><span data-stu-id="9eae8-261">Int</span></span>
<span data-ttu-id="9eae8-262">JAZYK</span><span class="sxs-lookup"><span data-stu-id="9eae8-262">LANG</span></span> | <span data-ttu-id="9eae8-263">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-263">String</span></span>
<span data-ttu-id="9eae8-264">LCHR</span><span class="sxs-lookup"><span data-stu-id="9eae8-264">LCHR</span></span> | <span data-ttu-id="9eae8-265">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-265">String</span></span>
<span data-ttu-id="9eae8-266">LRAW</span><span class="sxs-lookup"><span data-stu-id="9eae8-266">LRAW</span></span> | <span data-ttu-id="9eae8-267">Byte]</span><span class="sxs-lookup"><span data-stu-id="9eae8-267">Byte[]</span></span>
<span data-ttu-id="9eae8-268">PREC</span><span class="sxs-lookup"><span data-stu-id="9eae8-268">PREC</span></span> | <span data-ttu-id="9eae8-269">Int16</span><span class="sxs-lookup"><span data-stu-id="9eae8-269">Int16</span></span>
<span data-ttu-id="9eae8-270">QUAN</span><span class="sxs-lookup"><span data-stu-id="9eae8-270">QUAN</span></span> | <span data-ttu-id="9eae8-271">Decimal</span><span class="sxs-lookup"><span data-stu-id="9eae8-271">Decimal</span></span>
<span data-ttu-id="9eae8-272">NEZPRACOVANÁ</span><span class="sxs-lookup"><span data-stu-id="9eae8-272">RAW</span></span> | <span data-ttu-id="9eae8-273">Byte]</span><span class="sxs-lookup"><span data-stu-id="9eae8-273">Byte[]</span></span>
<span data-ttu-id="9eae8-274">RAWSTRING</span><span class="sxs-lookup"><span data-stu-id="9eae8-274">RAWSTRING</span></span> | <span data-ttu-id="9eae8-275">Byte]</span><span class="sxs-lookup"><span data-stu-id="9eae8-275">Byte[]</span></span>
<span data-ttu-id="9eae8-276">ŘETĚZEC</span><span class="sxs-lookup"><span data-stu-id="9eae8-276">STRING</span></span> | <span data-ttu-id="9eae8-277">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-277">String</span></span>
<span data-ttu-id="9eae8-278">JEDNOTKA</span><span class="sxs-lookup"><span data-stu-id="9eae8-278">UNIT</span></span> | <span data-ttu-id="9eae8-279">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-279">String</span></span>
<span data-ttu-id="9eae8-280">SOUBORY DAT</span><span class="sxs-lookup"><span data-stu-id="9eae8-280">DATS</span></span> | <span data-ttu-id="9eae8-281">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-281">String</span></span>
<span data-ttu-id="9eae8-282">NUMC</span><span class="sxs-lookup"><span data-stu-id="9eae8-282">NUMC</span></span> | <span data-ttu-id="9eae8-283">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-283">String</span></span>
<span data-ttu-id="9eae8-284">TIMS</span><span class="sxs-lookup"><span data-stu-id="9eae8-284">TIMS</span></span> | <span data-ttu-id="9eae8-285">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9eae8-285">String</span></span>

> [!NOTE]
> <span data-ttu-id="9eae8-286">Mapování sloupců z datové sady zdroje na sloupce ze sady jímku dat naleznete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="9eae8-286">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="map-source-to-sink-columns"></a><span data-ttu-id="9eae8-287">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="9eae8-287">Map source to sink columns</span></span>
<span data-ttu-id="9eae8-288">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="9eae8-288">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="9eae8-289">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="9eae8-289">Repeatable read from relational sources</span></span>
<span data-ttu-id="9eae8-290">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="9eae8-290">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="9eae8-291">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="9eae8-291">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="9eae8-292">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="9eae8-292">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="9eae8-293">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="9eae8-293">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="9eae8-294">V tématu [Repeatable číst z relační zdrojů](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="9eae8-294">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="9eae8-295">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="9eae8-295">Performance and Tuning</span></span>
<span data-ttu-id="9eae8-296">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="9eae8-296">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
