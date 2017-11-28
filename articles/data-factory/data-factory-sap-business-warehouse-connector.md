---
title: "aaaMove data ze serveru SAP Business Warehouse pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data ze serveru SAP Business Warehouse pomocí Azure Data Factory."
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
ms.openlocfilehash: 85df16f4759a846f578cad301e3cf918179143d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a><span data-ttu-id="0af1b-103">Přesun dat z SAP Business Warehouse pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="0af1b-103">Move data From SAP Business Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="0af1b-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z místní SAP Business Warehouse (BW).</span><span class="sxs-lookup"><span data-stu-id="0af1b-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises SAP Business Warehouse (BW).</span></span> <span data-ttu-id="0af1b-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="0af1b-106">Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní SAP Business Warehouse data.</span><span class="sxs-lookup"><span data-stu-id="0af1b-106">You can copy data from an on-premises SAP Business Warehouse data store tooany supported sink data store.</span></span> <span data-ttu-id="0af1b-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="0af1b-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="0af1b-108">Objekt pro vytváření dat v současné době podporuje pouze přesouvání dat od tooother SAP Business Warehouse ukládá, ale ne pro přesun dat z jiných dat ukládá tooan SAP Business Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0af1b-108">Data factory currently supports only moving data from an SAP Business Warehouse tooother data stores, but not for moving data from other data stores tooan SAP Business Warehouse.</span></span> 

## <a name="supported-versions-and-installation"></a><span data-ttu-id="0af1b-109">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="0af1b-109">Supported versions and installation</span></span>
<span data-ttu-id="0af1b-110">Tento konektor podporuje verze SAP Business Warehouse 7.x.</span><span class="sxs-lookup"><span data-stu-id="0af1b-110">This connector supports SAP Business Warehouse version 7.x.</span></span> <span data-ttu-id="0af1b-111">Podporuje kopírování dat z InfoCubes a QueryCubes (včetně BEx dotazy) pomocí dotazů MDX.</span><span class="sxs-lookup"><span data-stu-id="0af1b-111">It supports copying data from InfoCubes and QueryCubes (including BEx queries) using MDX queries.</span></span>

<span data-ttu-id="0af1b-112">tooenable hello připojení toohello instance SAP BW nainstalovat hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="0af1b-112">tooenable hello connectivity toohello SAP BW instance, install hello following components:</span></span>
- <span data-ttu-id="0af1b-113">**Brána pro správu dat**: podporuje služby Data Factory připojení tooon místní datové úložiště (včetně SAP Business Warehouse) pomocí součásti názvem Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="0af1b-113">**Data Management Gateway**: Data Factory service supports connecting tooon-premises data stores (including SAP Business Warehouse) using a component called Data Management Gateway.</span></span> <span data-ttu-id="0af1b-114">v tématu toolearn o Brána pro správu dat a podrobné pokyny pro nastavení brány hello [přesouvání dat mezi místní data úložiště dat toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0af1b-114">toolearn about Data Management Gateway and step-by-step instructions for setting up hello gateway, see [Moving data between on-premises data store toocloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="0af1b-115">Vyžaduje se brána, i když hello SAP Business Warehouse je hostovaná ve virtuálním počítači Azure IaaS (VM).</span><span class="sxs-lookup"><span data-stu-id="0af1b-115">Gateway is required even if hello SAP Business Warehouse is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="0af1b-116">Hello brány můžete nainstalovat na hello stejného virtuálního počítače jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="0af1b-116">You can install hello gateway on hello same VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>
- <span data-ttu-id="0af1b-117">**Knihovna SAP NetWeaver** na počítači brány hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-117">**SAP NetWeaver library** on hello gateway machine.</span></span> <span data-ttu-id="0af1b-118">Knihovna SAP Netweaver hello můžete získat od správce SAP, nebo přímo z hello [SAP služby Stažení softwaru](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="0af1b-118">You can get hello SAP Netweaver library from your SAP administrator, or directly from hello [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="0af1b-119">Vyhledejte hello **&#1025361; Poznámka SAP** umístění stahování hello tooget pro hello nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="0af1b-119">Search for hello **SAP Note #1025361** tooget hello download location for hello most recent version.</span></span> <span data-ttu-id="0af1b-120">Ujistěte se, že architektura hello hello SAP NetWeaver knihovny (32bitová nebo 64bitová verze) odpovídá vaší instalace brány.</span><span class="sxs-lookup"><span data-stu-id="0af1b-120">Make sure that hello architecture for hello SAP NetWeaver library (32-bit or 64-bit) matches your gateway installation.</span></span> <span data-ttu-id="0af1b-121">Nainstalujte všechny soubory, které jsou součástí hello SAP NetWeaver RFC SDK podle toohello Poznámka SAP.</span><span class="sxs-lookup"><span data-stu-id="0af1b-121">Then install all files included in hello SAP NetWeaver RFC SDK according toohello SAP Note.</span></span> <span data-ttu-id="0af1b-122">Knihovna SAP NetWeaver Hello je také součástí hello instalace SAP Client Tools.</span><span class="sxs-lookup"><span data-stu-id="0af1b-122">hello SAP NetWeaver library is also included in hello SAP Client Tools installation.</span></span>

> [!TIP]
> <span data-ttu-id="0af1b-123">Uveďte hello knihovny DLL z hello NetWeaver RFC SDK do složky system32 extrahovat.</span><span class="sxs-lookup"><span data-stu-id="0af1b-123">Put hello dlls extracted from hello NetWeaver RFC SDK into system32 folder.</span></span>

## <a name="getting-started"></a><span data-ttu-id="0af1b-124">Začínáme</span><span class="sxs-lookup"><span data-stu-id="0af1b-124">Getting started</span></span>
<span data-ttu-id="0af1b-125">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0af1b-125">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="0af1b-126">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-126">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="0af1b-127">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-127">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="0af1b-128">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-128">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="0af1b-129">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="0af1b-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="0af1b-130">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="0af1b-130">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="0af1b-131">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="0af1b-131">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="0af1b-132">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="0af1b-132">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="0af1b-133">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="0af1b-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="0af1b-134">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="0af1b-134">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="0af1b-135">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="0af1b-136">Ukázka s definicemi JSON entit služby Data Factory, které jsou používané toocopy data ze místní SAP Business Warehouse, najdete v části [JSON příklad: kopírování dat z SAP Business Warehouse tooAzure objektů Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="0af1b-136">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises SAP Business Warehouse, see [JSON example: Copy data from SAP Business Warehouse tooAzure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="0af1b-137">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooan SAP BW úložiště dat:</span><span class="sxs-lookup"><span data-stu-id="0af1b-137">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooan SAP BW data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="0af1b-138">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="0af1b-138">Linked service properties</span></span>
<span data-ttu-id="0af1b-139">Hello následující tabulka obsahuje popis pro konkrétní tooSAP elementy JSON skladu obchodní (BW) propojené služby.</span><span class="sxs-lookup"><span data-stu-id="0af1b-139">hello following table provides description for JSON elements specific tooSAP Business Warehouse (BW) linked service.</span></span>

<span data-ttu-id="0af1b-140">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0af1b-140">Property</span></span> | <span data-ttu-id="0af1b-141">Popis</span><span class="sxs-lookup"><span data-stu-id="0af1b-141">Description</span></span> | <span data-ttu-id="0af1b-142">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="0af1b-142">Allowed values</span></span> | <span data-ttu-id="0af1b-143">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0af1b-143">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="0af1b-144">server</span><span class="sxs-lookup"><span data-stu-id="0af1b-144">server</span></span> | <span data-ttu-id="0af1b-145">Název hello serveru, na které hello SAP BW nachází instance.</span><span class="sxs-lookup"><span data-stu-id="0af1b-145">Name of hello server on which hello SAP BW instance resides.</span></span> | <span data-ttu-id="0af1b-146">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-146">string</span></span> | <span data-ttu-id="0af1b-147">Ano</span><span class="sxs-lookup"><span data-stu-id="0af1b-147">Yes</span></span>
<span data-ttu-id="0af1b-148">systemNumber</span><span class="sxs-lookup"><span data-stu-id="0af1b-148">systemNumber</span></span> | <span data-ttu-id="0af1b-149">Systém počet hello systému SAP BW.</span><span class="sxs-lookup"><span data-stu-id="0af1b-149">System number of hello SAP BW system.</span></span> | <span data-ttu-id="0af1b-150">Desetinné číslo letopočty řetězec.</span><span class="sxs-lookup"><span data-stu-id="0af1b-150">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="0af1b-151">Ano</span><span class="sxs-lookup"><span data-stu-id="0af1b-151">Yes</span></span>
<span data-ttu-id="0af1b-152">clientId</span><span class="sxs-lookup"><span data-stu-id="0af1b-152">clientId</span></span> | <span data-ttu-id="0af1b-153">ID klienta hello klienta v hello SAP W systému.</span><span class="sxs-lookup"><span data-stu-id="0af1b-153">Client ID of hello client in hello SAP W system.</span></span> | <span data-ttu-id="0af1b-154">Tři číslice desítkové číslo řetězec.</span><span class="sxs-lookup"><span data-stu-id="0af1b-154">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="0af1b-155">Ano</span><span class="sxs-lookup"><span data-stu-id="0af1b-155">Yes</span></span>
<span data-ttu-id="0af1b-156">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="0af1b-156">username</span></span> | <span data-ttu-id="0af1b-157">Název hello uživatele, který má přístup k serveru SAP toohello</span><span class="sxs-lookup"><span data-stu-id="0af1b-157">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="0af1b-158">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-158">string</span></span> | <span data-ttu-id="0af1b-159">Ano</span><span class="sxs-lookup"><span data-stu-id="0af1b-159">Yes</span></span>
<span data-ttu-id="0af1b-160">heslo</span><span class="sxs-lookup"><span data-stu-id="0af1b-160">password</span></span> | <span data-ttu-id="0af1b-161">Heslo pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-161">Password for hello user.</span></span> | <span data-ttu-id="0af1b-162">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-162">string</span></span> | <span data-ttu-id="0af1b-163">Ano</span><span class="sxs-lookup"><span data-stu-id="0af1b-163">Yes</span></span>
<span data-ttu-id="0af1b-164">gatewayName</span><span class="sxs-lookup"><span data-stu-id="0af1b-164">gatewayName</span></span> | <span data-ttu-id="0af1b-165">Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní SAP BW instancí.</span><span class="sxs-lookup"><span data-stu-id="0af1b-165">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP BW instance.</span></span> | <span data-ttu-id="0af1b-166">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-166">string</span></span> | <span data-ttu-id="0af1b-167">Ano</span><span class="sxs-lookup"><span data-stu-id="0af1b-167">Yes</span></span>
<span data-ttu-id="0af1b-168">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="0af1b-168">encryptedCredential</span></span> | <span data-ttu-id="0af1b-169">Hello šifrovaný řetězec přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="0af1b-169">hello encrypted credential string.</span></span> | <span data-ttu-id="0af1b-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-170">string</span></span> | <span data-ttu-id="0af1b-171">Ne</span><span class="sxs-lookup"><span data-stu-id="0af1b-171">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="0af1b-172">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="0af1b-172">Dataset properties</span></span>
<span data-ttu-id="0af1b-173">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0af1b-173">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="0af1b-174">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="0af1b-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="0af1b-175">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-175">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="0af1b-176">Nejsou k dispozici žádné vlastnosti specifické pro typ podporované pro datovou sadu SAP BW hello typu **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-176">There are no type-specific properties supported for hello SAP BW dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="0af1b-177">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="0af1b-177">Copy activity properties</span></span>
<span data-ttu-id="0af1b-178">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0af1b-178">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="0af1b-179">Vlastnosti, například název, popis, vstupní a výstupní tabulky, jsou zásady jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="0af1b-179">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="0af1b-180">Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="0af1b-180">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="0af1b-181">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="0af1b-181">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="0af1b-182">Pokud je zdroj v aktivitě kopírování typu **RelationalSource** (která zahrnuje SAP BW), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="0af1b-182">When source in copy activity is of type **RelationalSource** (which includes SAP BW), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="0af1b-183">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0af1b-183">Property</span></span> | <span data-ttu-id="0af1b-184">Popis</span><span class="sxs-lookup"><span data-stu-id="0af1b-184">Description</span></span> | <span data-ttu-id="0af1b-185">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="0af1b-185">Allowed values</span></span> | <span data-ttu-id="0af1b-186">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0af1b-186">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0af1b-187">query</span><span class="sxs-lookup"><span data-stu-id="0af1b-187">query</span></span> | <span data-ttu-id="0af1b-188">Určuje hello MDX dotazu tooread data z instance SAP BW hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-188">Specifies hello MDX query tooread data from hello SAP BW instance.</span></span> | <span data-ttu-id="0af1b-189">Dotaz MDX.</span><span class="sxs-lookup"><span data-stu-id="0af1b-189">MDX query.</span></span> | <span data-ttu-id="0af1b-190">Ano</span><span class="sxs-lookup"><span data-stu-id="0af1b-190">Yes</span></span> |


## <a name="json-example-copy-data-from-sap-business-warehouse-tooazure-blob"></a><span data-ttu-id="0af1b-191">Příklad JSON: kopírování dat z SAP Business Warehouse tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="0af1b-191">JSON example: Copy data from SAP Business Warehouse tooAzure Blob</span></span>
<span data-ttu-id="0af1b-192">Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="0af1b-192">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="0af1b-193">Tento příklad ukazuje, jak toocopy data ze tooan SAP Business Warehouse místní úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="0af1b-193">This sample shows how toocopy data from an on-premises SAP Business Warehouse tooan Azure Blob Storage.</span></span> <span data-ttu-id="0af1b-194">Však můžete zkopírovat data **přímo** tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0af1b-194">However, data can be copied **directly** tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="0af1b-195">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="0af1b-195">This sample provides JSON snippets.</span></span> <span data-ttu-id="0af1b-196">Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-196">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="0af1b-197">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="0af1b-197">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="0af1b-198">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="0af1b-198">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="0af1b-199">Propojené služby typu [SapBw](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0af1b-199">A linked service of type [SapBw](#linked-service-properties).</span></span>
2. <span data-ttu-id="0af1b-200">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0af1b-200">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="0af1b-201">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0af1b-201">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="0af1b-202">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0af1b-202">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="0af1b-203">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="0af1b-203">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="0af1b-204">Ukázka Hello zkopíruje data z tooan instance SAP Business Warehouse objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="0af1b-204">hello sample copies data from an SAP Business Warehouse instance tooan Azure blob hourly.</span></span> <span data-ttu-id="0af1b-205">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-205">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="0af1b-206">Jako první krok nastavte Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-206">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="0af1b-207">Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0af1b-207">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-business-warehouse-linked-service"></a><span data-ttu-id="0af1b-208">SAP Business Warehouse propojená služba</span><span class="sxs-lookup"><span data-stu-id="0af1b-208">SAP Business Warehouse linked service</span></span>
<span data-ttu-id="0af1b-209">Tato propojená služba propojuje SAP BW instance toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="0af1b-209">This linked service links your SAP BW instance toohello data factory.</span></span> <span data-ttu-id="0af1b-210">Hello vlastnost Typ nastavena příliš**SapBw**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-210">hello type property is set too**SapBw**.</span></span> <span data-ttu-id="0af1b-211">Hello rámci typeProperties část obsahuje informace o připojení pro SAP BW instanci hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-211">hello typeProperties section provides connection information for hello SAP BW instance.</span></span> 

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

### <a name="azure-storage-linked-service"></a><span data-ttu-id="0af1b-212">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0af1b-212">Azure Storage linked service</span></span>
<span data-ttu-id="0af1b-213">Tato propojená služba propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="0af1b-213">This linked service links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="0af1b-214">Hello vlastnost Typ nastavena příliš**azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-214">hello type property is set too**AzureStorage**.</span></span> <span data-ttu-id="0af1b-215">Hello rámci typeProperties část obsahuje informace o připojení pro hello účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="0af1b-215">hello typeProperties section provides connection information for hello Azure Storage account.</span></span>

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

### <a name="sap-bw-input-dataset"></a><span data-ttu-id="0af1b-216">Vstupní datové sady SAP BW</span><span class="sxs-lookup"><span data-stu-id="0af1b-216">SAP BW input dataset</span></span>
<span data-ttu-id="0af1b-217">Tato datová sada definuje datovou sadu SAP Business Warehouse hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-217">This dataset defines hello SAP Business Warehouse dataset.</span></span> <span data-ttu-id="0af1b-218">Nastavte typ hello datovou sadu služby Data Factory hello příliš**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-218">You set hello type of hello Data Factory dataset too**RelationalTable**.</span></span> <span data-ttu-id="0af1b-219">V současné době nezadáte jakékoli vlastnosti specifické pro typ pro datové sadě služby SAP BW.</span><span class="sxs-lookup"><span data-stu-id="0af1b-219">Currently, you do not specify any type-specific properties for an SAP BW dataset.</span></span> <span data-ttu-id="0af1b-220">Hello dotaz hello definici aktivity kopírování Určuje, jaké tooread data z instance SAP BW hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-220">hello query in hello Copy Activity definition specifies what data tooread from hello SAP BW instance.</span></span> 

<span data-ttu-id="0af1b-221">Služba Data Factory hello nastavení tootrue vlastnost external informuje, že tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-221">Setting external property tootrue informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

<span data-ttu-id="0af1b-222">Frekvence a intervalu vlastnosti definuje hello plán.</span><span class="sxs-lookup"><span data-stu-id="0af1b-222">Frequency and interval properties defines hello schedule.</span></span> <span data-ttu-id="0af1b-223">V takovém případě hello je číst data z instance SAP BW hello každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="0af1b-223">In this case, hello data is read from hello SAP BW instance hourly.</span></span> 

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



### <a name="azure-blob-output-dataset"></a><span data-ttu-id="0af1b-224">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="0af1b-224">Azure Blob output dataset</span></span>
<span data-ttu-id="0af1b-225">Tato datová sada definuje datovou sadu objektu Blob Azure výstup hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-225">This dataset defines hello output Azure Blob dataset.</span></span> <span data-ttu-id="0af1b-226">Hello vlastnost Typ nastavena tooAzureBlob.</span><span class="sxs-lookup"><span data-stu-id="0af1b-226">hello type property is set tooAzureBlob.</span></span> <span data-ttu-id="0af1b-227">Hello rámci typeProperties část obsahuje, které jsou uložená data hello zkopírovány z instance SAP BW hello.</span><span class="sxs-lookup"><span data-stu-id="0af1b-227">hello typeProperties section provides where hello data copied from hello SAP BW instance is stored.</span></span> <span data-ttu-id="0af1b-228">Hello data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="0af1b-228">hello data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="0af1b-229">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="0af1b-229">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="0af1b-230">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="0af1b-230">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="0af1b-231">Kanál s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="0af1b-231">Pipeline with Copy activity</span></span>
<span data-ttu-id="0af1b-232">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="0af1b-232">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="0af1b-233">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** (pro SAP BW zdroj) a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-233">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** (for SAP BW source) and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="0af1b-234">Hello dotazu zadané pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="0af1b-234">hello query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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



### <a name="type-mapping-for-sap-bw"></a><span data-ttu-id="0af1b-235">Mapování typu pro SAP BW</span><span class="sxs-lookup"><span data-stu-id="0af1b-235">Type mapping for SAP BW</span></span>
<span data-ttu-id="0af1b-236">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello dvoustupňový přístup následující:</span><span class="sxs-lookup"><span data-stu-id="0af1b-236">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="0af1b-237">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="0af1b-237">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="0af1b-238">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="0af1b-238">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="0af1b-239">Při přesouvání dat od SAP BW, se používají následující mapování hello z typů too.NET typy SAP BW.</span><span class="sxs-lookup"><span data-stu-id="0af1b-239">When moving data from SAP BW, hello following mappings are used from SAP BW types too.NET types.</span></span>

<span data-ttu-id="0af1b-240">Typ dat ve slovníku ABAP hello</span><span class="sxs-lookup"><span data-stu-id="0af1b-240">Data type in hello ABAP Dictionary</span></span> | <span data-ttu-id="0af1b-241">Datový typ rozhraní .net</span><span class="sxs-lookup"><span data-stu-id="0af1b-241">.Net Data Type</span></span>
-------------------------------- | --------------
<span data-ttu-id="0af1b-242">ACCP</span><span class="sxs-lookup"><span data-stu-id="0af1b-242">ACCP</span></span> |  <span data-ttu-id="0af1b-243">celá čísla</span><span class="sxs-lookup"><span data-stu-id="0af1b-243">Int</span></span>
<span data-ttu-id="0af1b-244">CHAR –</span><span class="sxs-lookup"><span data-stu-id="0af1b-244">CHAR</span></span> | <span data-ttu-id="0af1b-245">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-245">String</span></span>
<span data-ttu-id="0af1b-246">CLNT</span><span class="sxs-lookup"><span data-stu-id="0af1b-246">CLNT</span></span> | <span data-ttu-id="0af1b-247">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-247">String</span></span>
<span data-ttu-id="0af1b-248">AKTUÁLNÍ</span><span class="sxs-lookup"><span data-stu-id="0af1b-248">CURR</span></span> | <span data-ttu-id="0af1b-249">Decimal</span><span class="sxs-lookup"><span data-stu-id="0af1b-249">Decimal</span></span>
<span data-ttu-id="0af1b-250">CUKY</span><span class="sxs-lookup"><span data-stu-id="0af1b-250">CUKY</span></span> | <span data-ttu-id="0af1b-251">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-251">String</span></span>
<span data-ttu-id="0af1b-252">DEC</span><span class="sxs-lookup"><span data-stu-id="0af1b-252">DEC</span></span> | <span data-ttu-id="0af1b-253">Decimal</span><span class="sxs-lookup"><span data-stu-id="0af1b-253">Decimal</span></span>
<span data-ttu-id="0af1b-254">FLTP</span><span class="sxs-lookup"><span data-stu-id="0af1b-254">FLTP</span></span> | <span data-ttu-id="0af1b-255">Double</span><span class="sxs-lookup"><span data-stu-id="0af1b-255">Double</span></span>
<span data-ttu-id="0af1b-256">INT1</span><span class="sxs-lookup"><span data-stu-id="0af1b-256">INT1</span></span> | <span data-ttu-id="0af1b-257">Bajtů</span><span class="sxs-lookup"><span data-stu-id="0af1b-257">Byte</span></span>
<span data-ttu-id="0af1b-258">INT2</span><span class="sxs-lookup"><span data-stu-id="0af1b-258">INT2</span></span> | <span data-ttu-id="0af1b-259">Int16</span><span class="sxs-lookup"><span data-stu-id="0af1b-259">Int16</span></span>
<span data-ttu-id="0af1b-260">INT4</span><span class="sxs-lookup"><span data-stu-id="0af1b-260">INT4</span></span> | <span data-ttu-id="0af1b-261">celá čísla</span><span class="sxs-lookup"><span data-stu-id="0af1b-261">Int</span></span>
<span data-ttu-id="0af1b-262">JAZYK</span><span class="sxs-lookup"><span data-stu-id="0af1b-262">LANG</span></span> | <span data-ttu-id="0af1b-263">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-263">String</span></span>
<span data-ttu-id="0af1b-264">LCHR</span><span class="sxs-lookup"><span data-stu-id="0af1b-264">LCHR</span></span> | <span data-ttu-id="0af1b-265">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-265">String</span></span>
<span data-ttu-id="0af1b-266">LRAW</span><span class="sxs-lookup"><span data-stu-id="0af1b-266">LRAW</span></span> | <span data-ttu-id="0af1b-267">Byte]</span><span class="sxs-lookup"><span data-stu-id="0af1b-267">Byte[]</span></span>
<span data-ttu-id="0af1b-268">PREC</span><span class="sxs-lookup"><span data-stu-id="0af1b-268">PREC</span></span> | <span data-ttu-id="0af1b-269">Int16</span><span class="sxs-lookup"><span data-stu-id="0af1b-269">Int16</span></span>
<span data-ttu-id="0af1b-270">QUAN</span><span class="sxs-lookup"><span data-stu-id="0af1b-270">QUAN</span></span> | <span data-ttu-id="0af1b-271">Decimal</span><span class="sxs-lookup"><span data-stu-id="0af1b-271">Decimal</span></span>
<span data-ttu-id="0af1b-272">NEZPRACOVANÁ</span><span class="sxs-lookup"><span data-stu-id="0af1b-272">RAW</span></span> | <span data-ttu-id="0af1b-273">Byte]</span><span class="sxs-lookup"><span data-stu-id="0af1b-273">Byte[]</span></span>
<span data-ttu-id="0af1b-274">RAWSTRING</span><span class="sxs-lookup"><span data-stu-id="0af1b-274">RAWSTRING</span></span> | <span data-ttu-id="0af1b-275">Byte]</span><span class="sxs-lookup"><span data-stu-id="0af1b-275">Byte[]</span></span>
<span data-ttu-id="0af1b-276">ŘETĚZEC</span><span class="sxs-lookup"><span data-stu-id="0af1b-276">STRING</span></span> | <span data-ttu-id="0af1b-277">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-277">String</span></span>
<span data-ttu-id="0af1b-278">JEDNOTKA</span><span class="sxs-lookup"><span data-stu-id="0af1b-278">UNIT</span></span> | <span data-ttu-id="0af1b-279">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-279">String</span></span>
<span data-ttu-id="0af1b-280">SOUBORY DAT</span><span class="sxs-lookup"><span data-stu-id="0af1b-280">DATS</span></span> | <span data-ttu-id="0af1b-281">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-281">String</span></span>
<span data-ttu-id="0af1b-282">NUMC</span><span class="sxs-lookup"><span data-stu-id="0af1b-282">NUMC</span></span> | <span data-ttu-id="0af1b-283">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-283">String</span></span>
<span data-ttu-id="0af1b-284">TIMS</span><span class="sxs-lookup"><span data-stu-id="0af1b-284">TIMS</span></span> | <span data-ttu-id="0af1b-285">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0af1b-285">String</span></span>

> [!NOTE]
> <span data-ttu-id="0af1b-286">toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="0af1b-286">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="map-source-toosink-columns"></a><span data-ttu-id="0af1b-287">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="0af1b-287">Map source toosink columns</span></span>
<span data-ttu-id="0af1b-288">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="0af1b-288">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="0af1b-289">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="0af1b-289">Repeatable read from relational sources</span></span>
<span data-ttu-id="0af1b-290">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="0af1b-290">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="0af1b-291">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="0af1b-291">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="0af1b-292">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="0af1b-292">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="0af1b-293">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="0af1b-293">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="0af1b-294">V tématu [Repeatable číst z relační zdrojů](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="0af1b-294">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="0af1b-295">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="0af1b-295">Performance and Tuning</span></span>
<span data-ttu-id="0af1b-296">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="0af1b-296">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
