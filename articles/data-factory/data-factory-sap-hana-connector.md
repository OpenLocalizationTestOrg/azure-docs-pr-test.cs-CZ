---
title: "aaaMove data z SAP HANA pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data z SAP HANA pomocí Azure Data Factory."
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
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 5cefe4c8ed01ea4e86e02496b2f8a9083d0b949c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a><span data-ttu-id="88aed-103">Přesun dat z SAP HANA pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="88aed-103">Move data From SAP HANA using Azure Data Factory</span></span>
<span data-ttu-id="88aed-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data ze místní SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="88aed-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises SAP HANA.</span></span> <span data-ttu-id="88aed-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="88aed-106">Data můžete zkopírovat z místní SAP HANA dat úložiště tooany podporované jímku dat úložiště.</span><span class="sxs-lookup"><span data-stu-id="88aed-106">You can copy data from an on-premises SAP HANA data store tooany supported sink data store.</span></span> <span data-ttu-id="88aed-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="88aed-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="88aed-108">Objekt pro vytváření dat v současné době podporuje pouze přesouvání dat od SAP HANA tooother uloží, ale ne pro přesun dat z jiných dat ukládá tooan SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="88aed-108">Data factory currently supports only moving data from an SAP HANA tooother data stores, but not for moving data from other data stores tooan SAP HANA.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="88aed-109">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="88aed-109">Supported versions and installation</span></span>
<span data-ttu-id="88aed-110">Tento konektor podporuje všechny verze databáze SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="88aed-110">This connector supports any version of SAP HANA database.</span></span> <span data-ttu-id="88aed-111">Podporuje kopírování dat z modelů HANA informace (například analýzy a výpočet zobrazení) a řádek nebo sloupce tabulky pomocí dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="88aed-111">It supports copying data from HANA information models (such as Analytic and Calculation views) and Row/Column tables using SQL queries.</span></span>

<span data-ttu-id="88aed-112">tooenable hello připojení toohello instance SAP HANA nainstalovat hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="88aed-112">tooenable hello connectivity toohello SAP HANA instance, install hello following components:</span></span>
- <span data-ttu-id="88aed-113">**Brána pro správu dat**: podporuje služby Data Factory připojení tooon místní datové úložiště (včetně SAP HANA) pomocí součásti názvem Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="88aed-113">**Data Management Gateway**: Data Factory service supports connecting tooon-premises data stores (including SAP HANA) using a component called Data Management Gateway.</span></span> <span data-ttu-id="88aed-114">v tématu toolearn o Brána pro správu dat a podrobné pokyny pro nastavení brány hello [přesouvání dat mezi místní data úložiště dat toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="88aed-114">toolearn about Data Management Gateway and step-by-step instructions for setting up hello gateway, see [Moving data between on-premises data store toocloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="88aed-115">Vyžaduje se brána, i když hello SAP HANA je hostovaná ve virtuálním počítači Azure IaaS (VM).</span><span class="sxs-lookup"><span data-stu-id="88aed-115">Gateway is required even if hello SAP HANA is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="88aed-116">Hello brány můžete nainstalovat na hello stejného virtuálního počítače jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="88aed-116">You can install hello gateway on hello same VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>
- <span data-ttu-id="88aed-117">**Ovladač SAP HANA ODBC** na počítači brány hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-117">**SAP HANA ODBC driver** on hello gateway machine.</span></span> <span data-ttu-id="88aed-118">Ovladač SAP HANA ODBC hello si můžete stáhnout z hello [SAP služby Stažení softwaru](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="88aed-118">You can download hello SAP HANA ODBC driver from hello [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="88aed-119">Hledání se hello – klíčové slovo **SAP HANA klienta pro systém Windows**.</span><span class="sxs-lookup"><span data-stu-id="88aed-119">Search with hello keyword **SAP HANA CLIENT for Windows**.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="88aed-120">Začínáme</span><span class="sxs-lookup"><span data-stu-id="88aed-120">Getting started</span></span>
<span data-ttu-id="88aed-121">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="88aed-121">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="88aed-122">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="88aed-122">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="88aed-123">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="88aed-124">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="88aed-124">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="88aed-125">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="88aed-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="88aed-126">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="88aed-126">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="88aed-127">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="88aed-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="88aed-128">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="88aed-128">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="88aed-129">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="88aed-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="88aed-130">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="88aed-130">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="88aed-131">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="88aed-132">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z SAP HANA místní, naleznete v tématu [JSON příklad: kopírování dat z SAP HANA tooAzure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="88aed-132">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises SAP HANA, see [JSON example: Copy data from SAP HANA tooAzure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="88aed-133">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooan SAP HANA úložiště dat:</span><span class="sxs-lookup"><span data-stu-id="88aed-133">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooan SAP HANA data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="88aed-134">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="88aed-134">Linked service properties</span></span>
<span data-ttu-id="88aed-135">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooSAP HANA propojené služby.</span><span class="sxs-lookup"><span data-stu-id="88aed-135">hello following table provides description for JSON elements specific tooSAP HANA linked service.</span></span>

<span data-ttu-id="88aed-136">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="88aed-136">Property</span></span> | <span data-ttu-id="88aed-137">Popis</span><span class="sxs-lookup"><span data-stu-id="88aed-137">Description</span></span> | <span data-ttu-id="88aed-138">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="88aed-138">Allowed values</span></span> | <span data-ttu-id="88aed-139">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="88aed-139">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="88aed-140">server</span><span class="sxs-lookup"><span data-stu-id="88aed-140">server</span></span> | <span data-ttu-id="88aed-141">Název hello serveru, na které hello SAP HANA nachází instance.</span><span class="sxs-lookup"><span data-stu-id="88aed-141">Name of hello server on which hello SAP HANA instance resides.</span></span> <span data-ttu-id="88aed-142">Pokud váš server používá vlastní port, zadejte `server:port`.</span><span class="sxs-lookup"><span data-stu-id="88aed-142">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="88aed-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88aed-143">string</span></span> | <span data-ttu-id="88aed-144">Ano</span><span class="sxs-lookup"><span data-stu-id="88aed-144">Yes</span></span>
<span data-ttu-id="88aed-145">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="88aed-145">authenticationType</span></span> | <span data-ttu-id="88aed-146">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="88aed-146">Type of authentication.</span></span> | <span data-ttu-id="88aed-147">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="88aed-147">string.</span></span> <span data-ttu-id="88aed-148">"Základní" nebo "Systém Windows"</span><span class="sxs-lookup"><span data-stu-id="88aed-148">"Basic" or "Windows"</span></span> | <span data-ttu-id="88aed-149">Ano</span><span class="sxs-lookup"><span data-stu-id="88aed-149">Yes</span></span> 
<span data-ttu-id="88aed-150">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="88aed-150">username</span></span> | <span data-ttu-id="88aed-151">Název hello uživatele, který má přístup k serveru SAP toohello</span><span class="sxs-lookup"><span data-stu-id="88aed-151">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="88aed-152">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88aed-152">string</span></span> | <span data-ttu-id="88aed-153">Ano</span><span class="sxs-lookup"><span data-stu-id="88aed-153">Yes</span></span>
<span data-ttu-id="88aed-154">heslo</span><span class="sxs-lookup"><span data-stu-id="88aed-154">password</span></span> | <span data-ttu-id="88aed-155">Heslo pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-155">Password for hello user.</span></span> | <span data-ttu-id="88aed-156">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88aed-156">string</span></span> | <span data-ttu-id="88aed-157">Ano</span><span class="sxs-lookup"><span data-stu-id="88aed-157">Yes</span></span>
<span data-ttu-id="88aed-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="88aed-158">gatewayName</span></span> | <span data-ttu-id="88aed-159">Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní SAP HANA instance.</span><span class="sxs-lookup"><span data-stu-id="88aed-159">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP HANA instance.</span></span> | <span data-ttu-id="88aed-160">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88aed-160">string</span></span> | <span data-ttu-id="88aed-161">Ano</span><span class="sxs-lookup"><span data-stu-id="88aed-161">Yes</span></span>
<span data-ttu-id="88aed-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="88aed-162">encryptedCredential</span></span> | <span data-ttu-id="88aed-163">Hello šifrovaný řetězec přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="88aed-163">hello encrypted credential string.</span></span> | <span data-ttu-id="88aed-164">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88aed-164">string</span></span> | <span data-ttu-id="88aed-165">Ne</span><span class="sxs-lookup"><span data-stu-id="88aed-165">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="88aed-166">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="88aed-166">Dataset properties</span></span>
<span data-ttu-id="88aed-167">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="88aed-167">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="88aed-168">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="88aed-168">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="88aed-169">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-169">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="88aed-170">Nejsou k dispozici žádné vlastnosti specifické pro typ podporované pro datovou sadu SAP HANA hello typu **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="88aed-170">There are no type-specific properties supported for hello SAP HANA dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="88aed-171">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="88aed-171">Copy activity properties</span></span>
<span data-ttu-id="88aed-172">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="88aed-172">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="88aed-173">Vlastnosti, například název, popis, vstupní a výstupní tabulky, jsou zásady jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="88aed-173">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="88aed-174">Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="88aed-174">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="88aed-175">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="88aed-175">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="88aed-176">Pokud je zdroj v aktivitě kopírování typu **RelationalSource** (která zahrnuje SAP HANA), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="88aed-176">When source in copy activity is of type **RelationalSource** (which includes SAP HANA), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="88aed-177">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="88aed-177">Property</span></span> | <span data-ttu-id="88aed-178">Popis</span><span class="sxs-lookup"><span data-stu-id="88aed-178">Description</span></span> | <span data-ttu-id="88aed-179">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="88aed-179">Allowed values</span></span> | <span data-ttu-id="88aed-180">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="88aed-180">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="88aed-181">query</span><span class="sxs-lookup"><span data-stu-id="88aed-181">query</span></span> | <span data-ttu-id="88aed-182">Určuje hello SQL dotaz tooread data z instance SAP HANA hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-182">Specifies hello SQL query tooread data from hello SAP HANA instance.</span></span> | <span data-ttu-id="88aed-183">Dotaz SQL.</span><span class="sxs-lookup"><span data-stu-id="88aed-183">SQL query.</span></span> | <span data-ttu-id="88aed-184">Ano</span><span class="sxs-lookup"><span data-stu-id="88aed-184">Yes</span></span> |

## <a name="json-example-copy-data-from-sap-hana-tooazure-blob"></a><span data-ttu-id="88aed-185">Příklad JSON: kopírování dat z SAP HANA tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="88aed-185">JSON example: Copy data from SAP HANA tooAzure Blob</span></span>
<span data-ttu-id="88aed-186">Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="88aed-186">hello following sample provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="88aed-187">Tento příklad ukazuje, jak toocopy data ze tooan SAP HANA místní úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="88aed-187">This sample shows how toocopy data from an on-premises SAP HANA tooan Azure Blob Storage.</span></span> <span data-ttu-id="88aed-188">Však můžete zkopírovat data **přímo** tooany hello jímky uvedené [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="88aed-188">However, data can be copied **directly** tooany of hello sinks listed [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="88aed-189">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="88aed-189">This sample provides JSON snippets.</span></span> <span data-ttu-id="88aed-190">Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-190">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="88aed-191">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="88aed-191">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="88aed-192">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="88aed-192">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="88aed-193">Propojené služby typu [SapHana](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="88aed-193">A linked service of type [SapHana](#linked-service-properties).</span></span>
2. <span data-ttu-id="88aed-194">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="88aed-194">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="88aed-195">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="88aed-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="88aed-196">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="88aed-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="88aed-197">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="88aed-197">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="88aed-198">Ukázka Hello zkopíruje data z tooan SAP HANA instance objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="88aed-198">hello sample copies data from an SAP HANA instance tooan Azure blob hourly.</span></span> <span data-ttu-id="88aed-199">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-199">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="88aed-200">Jako první krok nastavte Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-200">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="88aed-201">Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="88aed-201">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-hana-linked-service"></a><span data-ttu-id="88aed-202">SAP HANA propojené služby</span><span class="sxs-lookup"><span data-stu-id="88aed-202">SAP HANA linked service</span></span>
<span data-ttu-id="88aed-203">Tato propojená služba propojuje SAP HANA instance toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="88aed-203">This linked service links your SAP HANA instance toohello data factory.</span></span> <span data-ttu-id="88aed-204">Hello vlastnost Typ nastavena příliš**SapHana**.</span><span class="sxs-lookup"><span data-stu-id="88aed-204">hello type property is set too**SapHana**.</span></span> <span data-ttu-id="88aed-205">Hello rámci typeProperties část obsahuje informace o připojení pro instance SAP HANA hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-205">hello typeProperties section provides connection information for hello SAP HANA instance.</span></span>

```json
{
    "name": "SapHanaLinkedService",
    "properties":
    {
        "type": "SapHana",
        "typeProperties":
        {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="88aed-206">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="88aed-206">Azure Storage linked service</span></span>
<span data-ttu-id="88aed-207">Tato propojená služba propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="88aed-207">This linked service links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="88aed-208">Hello vlastnost Typ nastavena příliš**azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="88aed-208">hello type property is set too**AzureStorage**.</span></span> <span data-ttu-id="88aed-209">Hello rámci typeProperties část obsahuje informace o připojení pro hello účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="88aed-209">hello typeProperties section provides connection information for hello Azure Storage account.</span></span>

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

### <a name="sap-hana-input-dataset"></a><span data-ttu-id="88aed-210">Vstupní datové sady SAP HANA</span><span class="sxs-lookup"><span data-stu-id="88aed-210">SAP HANA input dataset</span></span>

<span data-ttu-id="88aed-211">Tato datová sada definuje datovou sadu hello SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="88aed-211">This dataset defines hello SAP HANA dataset.</span></span> <span data-ttu-id="88aed-212">Nastavte typ hello datovou sadu služby Data Factory hello příliš**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="88aed-212">You set hello type of hello Data Factory dataset too**RelationalTable**.</span></span> <span data-ttu-id="88aed-213">V současné době nezadáte jakékoli vlastnosti specifické pro typ pro datové sadě služby SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="88aed-213">Currently, you do not specify any type-specific properties for an SAP HANA dataset.</span></span> <span data-ttu-id="88aed-214">Hello dotaz hello definici aktivity kopírování Určuje, jaké tooread data z instance SAP HANA hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-214">hello query in hello Copy Activity definition specifies what data tooread from hello SAP HANA instance.</span></span> 

<span data-ttu-id="88aed-215">Služba Data Factory hello nastavení tootrue vlastnost external informuje, že tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-215">Setting external property tootrue informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

<span data-ttu-id="88aed-216">Frekvence a intervalu vlastnosti definuje hello plán.</span><span class="sxs-lookup"><span data-stu-id="88aed-216">Frequency and interval properties defines hello schedule.</span></span> <span data-ttu-id="88aed-217">V takovém případě hello je číst data z instance SAP HANA hello každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="88aed-217">In this case, hello data is read from hello SAP HANA instance hourly.</span></span> 

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="88aed-218">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="88aed-218">Azure Blob output dataset</span></span>
<span data-ttu-id="88aed-219">Tato datová sada definuje datovou sadu objektu Blob Azure výstup hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-219">This dataset defines hello output Azure Blob dataset.</span></span> <span data-ttu-id="88aed-220">Hello vlastnost Typ nastavena tooAzureBlob.</span><span class="sxs-lookup"><span data-stu-id="88aed-220">hello type property is set tooAzureBlob.</span></span> <span data-ttu-id="88aed-221">Hello rámci typeProperties část obsahuje, které jsou uložená data hello zkopírovány z instance SAP HANA hello.</span><span class="sxs-lookup"><span data-stu-id="88aed-221">hello typeProperties section provides where hello data copied from hello SAP HANA instance is stored.</span></span> <span data-ttu-id="88aed-222">Hello data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="88aed-222">hello data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="88aed-223">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="88aed-223">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="88aed-224">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="88aed-224">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/saphana/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="88aed-225">Kanál s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="88aed-225">Pipeline with Copy activity</span></span>

<span data-ttu-id="88aed-226">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="88aed-226">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="88aed-227">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** (pro SAP HANA zdroj) a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="88aed-227">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** (for SAP HANA source) and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="88aed-228">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="88aed-228">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<SQL Query for HANA>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapHanaDataset"
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
                "name": "SapHanaToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```


### <a name="type-mapping-for-sap-hana"></a><span data-ttu-id="88aed-229">Mapování typu pro SAP HANA</span><span class="sxs-lookup"><span data-stu-id="88aed-229">Type mapping for SAP HANA</span></span>
<span data-ttu-id="88aed-230">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello dvoustupňový přístup následující:</span><span class="sxs-lookup"><span data-stu-id="88aed-230">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="88aed-231">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="88aed-231">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="88aed-232">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="88aed-232">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="88aed-233">Při přesouvání dat od SAP HANA, se používají následující mapování hello z typů too.NET typy SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="88aed-233">When moving data from SAP HANA, hello following mappings are used from SAP HANA types too.NET types.</span></span>

<span data-ttu-id="88aed-234">Typ SAP HANA</span><span class="sxs-lookup"><span data-stu-id="88aed-234">SAP HANA Type</span></span> | <span data-ttu-id="88aed-235">.NET na základě typu</span><span class="sxs-lookup"><span data-stu-id="88aed-235">.NET Based Type</span></span>
------------- | ---------------
<span data-ttu-id="88aed-236">TINYINT</span><span class="sxs-lookup"><span data-stu-id="88aed-236">TINYINT</span></span> | <span data-ttu-id="88aed-237">Bajtů</span><span class="sxs-lookup"><span data-stu-id="88aed-237">Byte</span></span>
<span data-ttu-id="88aed-238">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="88aed-238">SMALLINT</span></span> | <span data-ttu-id="88aed-239">Int16</span><span class="sxs-lookup"><span data-stu-id="88aed-239">Int16</span></span>
<span data-ttu-id="88aed-240">INT</span><span class="sxs-lookup"><span data-stu-id="88aed-240">INT</span></span> | <span data-ttu-id="88aed-241">Int32</span><span class="sxs-lookup"><span data-stu-id="88aed-241">Int32</span></span>
<span data-ttu-id="88aed-242">BIGINT</span><span class="sxs-lookup"><span data-stu-id="88aed-242">BIGINT</span></span> | <span data-ttu-id="88aed-243">Int64</span><span class="sxs-lookup"><span data-stu-id="88aed-243">Int64</span></span>
<span data-ttu-id="88aed-244">SKUTEČNÉ</span><span class="sxs-lookup"><span data-stu-id="88aed-244">REAL</span></span> | <span data-ttu-id="88aed-245">Jeden</span><span class="sxs-lookup"><span data-stu-id="88aed-245">Single</span></span>
<span data-ttu-id="88aed-246">DOUBLE</span><span class="sxs-lookup"><span data-stu-id="88aed-246">DOUBLE</span></span> | <span data-ttu-id="88aed-247">Jeden</span><span class="sxs-lookup"><span data-stu-id="88aed-247">Single</span></span>
<span data-ttu-id="88aed-248">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="88aed-248">DECIMAL</span></span> | <span data-ttu-id="88aed-249">Decimal</span><span class="sxs-lookup"><span data-stu-id="88aed-249">Decimal</span></span>
<span data-ttu-id="88aed-250">LOGICKÁ HODNOTA</span><span class="sxs-lookup"><span data-stu-id="88aed-250">BOOLEAN</span></span> | <span data-ttu-id="88aed-251">Bajtů</span><span class="sxs-lookup"><span data-stu-id="88aed-251">Byte</span></span>
<span data-ttu-id="88aed-252">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="88aed-252">VARCHAR</span></span> | <span data-ttu-id="88aed-253">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88aed-253">String</span></span>
<span data-ttu-id="88aed-254">NVARCHAR</span><span class="sxs-lookup"><span data-stu-id="88aed-254">NVARCHAR</span></span> | <span data-ttu-id="88aed-255">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88aed-255">String</span></span>
<span data-ttu-id="88aed-256">DATOVÝ TYP CLOB</span><span class="sxs-lookup"><span data-stu-id="88aed-256">CLOB</span></span> | <span data-ttu-id="88aed-257">Byte]</span><span class="sxs-lookup"><span data-stu-id="88aed-257">Byte[]</span></span>
<span data-ttu-id="88aed-258">ALPHANUM</span><span class="sxs-lookup"><span data-stu-id="88aed-258">ALPHANUM</span></span> | <span data-ttu-id="88aed-259">Řetězec</span><span class="sxs-lookup"><span data-stu-id="88aed-259">String</span></span>
<span data-ttu-id="88aed-260">OBJEKT BLOB</span><span class="sxs-lookup"><span data-stu-id="88aed-260">BLOB</span></span> | <span data-ttu-id="88aed-261">Byte]</span><span class="sxs-lookup"><span data-stu-id="88aed-261">Byte[]</span></span>
<span data-ttu-id="88aed-262">DATUM</span><span class="sxs-lookup"><span data-stu-id="88aed-262">DATE</span></span> | <span data-ttu-id="88aed-263">Data a času</span><span class="sxs-lookup"><span data-stu-id="88aed-263">DateTime</span></span>
<span data-ttu-id="88aed-264">ČAS</span><span class="sxs-lookup"><span data-stu-id="88aed-264">TIME</span></span> | <span data-ttu-id="88aed-265">Časový interval</span><span class="sxs-lookup"><span data-stu-id="88aed-265">TimeSpan</span></span>
<span data-ttu-id="88aed-266">ČASOVÉ RAZÍTKO</span><span class="sxs-lookup"><span data-stu-id="88aed-266">TIMESTAMP</span></span> | <span data-ttu-id="88aed-267">Data a času</span><span class="sxs-lookup"><span data-stu-id="88aed-267">DateTime</span></span>
<span data-ttu-id="88aed-268">SECONDDATE</span><span class="sxs-lookup"><span data-stu-id="88aed-268">SECONDDATE</span></span> | <span data-ttu-id="88aed-269">Data a času</span><span class="sxs-lookup"><span data-stu-id="88aed-269">DateTime</span></span>

## <a name="known-limitations"></a><span data-ttu-id="88aed-270">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="88aed-270">Known limitations</span></span>
<span data-ttu-id="88aed-271">Při kopírování dat z SAP HANA existuje několik známá omezení:</span><span class="sxs-lookup"><span data-stu-id="88aed-271">There are a few known limitations when copying data from SAP HANA:</span></span>

- <span data-ttu-id="88aed-272">NVARCHAR řetězce jsou zkrácená toomaximum délce 4000 znaků Unicode</span><span class="sxs-lookup"><span data-stu-id="88aed-272">NVARCHAR strings are truncated toomaximum length of 4000 Unicode characters</span></span>
- <span data-ttu-id="88aed-273">SMALLDECIMAL není podporován.</span><span class="sxs-lookup"><span data-stu-id="88aed-273">SMALLDECIMAL is not supported</span></span>
- <span data-ttu-id="88aed-274">VARBINARY není podporován.</span><span class="sxs-lookup"><span data-stu-id="88aed-274">VARBINARY is not supported</span></span>
- <span data-ttu-id="88aed-275">Platná data jsou mezi 1899/12/30 a 9999/12/31</span><span class="sxs-lookup"><span data-stu-id="88aed-275">Valid Dates are between 1899/12/30 and 9999/12/31</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="88aed-276">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="88aed-276">Map source toosink columns</span></span>
<span data-ttu-id="88aed-277">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="88aed-277">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="88aed-278">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="88aed-278">Repeatable read from relational sources</span></span>
<span data-ttu-id="88aed-279">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="88aed-279">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="88aed-280">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="88aed-280">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="88aed-281">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="88aed-281">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="88aed-282">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="88aed-282">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="88aed-283">V tématu [Repeatable číst z relační zdrojů](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="88aed-283">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="88aed-284">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="88aed-284">Performance and Tuning</span></span>
<span data-ttu-id="88aed-285">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="88aed-285">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
