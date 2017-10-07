---
title: "aaaMove data z databáze MySQL pomocí Azure Data Factory | Microsoft Docs"
description: "Informace o tom, jak toomove data z databáze MySQL databáze pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 452f4fce-9eb5-40a0-92f8-1e98691bea4c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jingwang
ms.openlocfilehash: 3ffe969e42ce1a54b265c4739df43fdc594ea891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a><span data-ttu-id="c8dc1-103">Přesun dat pomocí Azure Data Factory z databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="c8dc1-103">Move data From MySQL using Azure Data Factory</span></span>
<span data-ttu-id="c8dc1-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze MySQL na místě.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises MySQL database.</span></span> <span data-ttu-id="c8dc1-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="c8dc1-106">Z úložiště úložiště tooany podporované jímku dat místní MySQL dat může kopírovat data.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-106">You can copy data from an on-premises MySQL data store tooany supported sink data store.</span></span> <span data-ttu-id="c8dc1-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="c8dc1-108">Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat tooother uložit data z databáze MySQL data, ale ne pro přesun dat z jiného úložiště dat úložiště tooan MySQL data.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-108">Data factory currently supports only moving data from a MySQL data store tooother data stores, but not for moving data from other data stores tooan MySQL data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c8dc1-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c8dc1-109">Prerequisites</span></span>
<span data-ttu-id="c8dc1-110">Služba data Factory podporuje připojování zdroje MySQL tooon místní pomocí hello Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-110">Data Factory service supports connecting tooon-premises MySQL sources using hello Data Management Gateway.</span></span> <span data-ttu-id="c8dc1-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="c8dc1-112">Vyžaduje se brána, i když je databáze MySQL hello hostované ve virtuálním počítači Azure IaaS (VM).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-112">Gateway is required even if hello MySQL database is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="c8dc1-113">Hello brány můžete nainstalovat na hello stejného virtuálního počítače jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-113">You can install hello gateway on hello same VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="c8dc1-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="c8dc1-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="c8dc1-115">Supported versions and installation</span></span>
<span data-ttu-id="c8dc1-116">Brána pro správu dat tooconnect toohello databáze MySQL, musíte tooinstall hello [MySQL Connector/Net pro Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (verze 6.6.5 nebo vyšší) na stejné hello systému jako brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-116">For Data Management Gateway tooconnect toohello MySQL Database, you need tooinstall hello [MySQL Connector/Net for Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (version 6.6.5 or above) on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="c8dc1-117">MySQL verze 5.1 a vyšší je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-117">MySQL version 5.1 and above is supported.</span></span>

> [!TIP]
> <span data-ttu-id="c8dc1-118">Chyba při dosažení "Ověřování se nezdařilo, protože je uzavřený vzdálené strany hello hello přenosu datového proudu.", zvažte tooupgrade hello MySQL Connector/Net toohigher verze.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-118">If you hit error on "Authentication failed because hello remote party has closed hello transport stream.", consider tooupgrade hello MySQL Connector/Net toohigher version.</span></span>

## <a name="getting-started"></a><span data-ttu-id="c8dc1-119">Začínáme</span><span class="sxs-lookup"><span data-stu-id="c8dc1-119">Getting started</span></span>
<span data-ttu-id="c8dc1-120">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-120">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="c8dc1-121">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-121">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="c8dc1-122">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="c8dc1-123">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-123">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="c8dc1-124">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="c8dc1-125">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="c8dc1-125">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="c8dc1-126">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-126">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="c8dc1-127">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-127">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="c8dc1-128">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="c8dc1-129">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-129">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="c8dc1-130">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="c8dc1-131">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat místní MySQL, naleznete v tématu [JSON příklad: kopírování dat z databáze MySQL tooAzure Blob](#json-example-copy-data-from-mysql-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-131">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises MySQL data store, see [JSON example: Copy data from MySQL tooAzure Blob](#json-example-copy-data-from-mysql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="c8dc1-132">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa MySQL úložiště dat:</span><span class="sxs-lookup"><span data-stu-id="c8dc1-132">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa MySQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="c8dc1-133">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="c8dc1-133">Linked service properties</span></span>
<span data-ttu-id="c8dc1-134">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooMySQL propojené služby.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-134">hello following table provides description for JSON elements specific tooMySQL linked service.</span></span>

| <span data-ttu-id="c8dc1-135">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c8dc1-135">Property</span></span> | <span data-ttu-id="c8dc1-136">Popis</span><span class="sxs-lookup"><span data-stu-id="c8dc1-136">Description</span></span> | <span data-ttu-id="c8dc1-137">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c8dc1-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c8dc1-138">type</span><span class="sxs-lookup"><span data-stu-id="c8dc1-138">type</span></span> |<span data-ttu-id="c8dc1-139">vlastnost typu Hello musí být nastavena na: **OnPremisesMySql**</span><span class="sxs-lookup"><span data-stu-id="c8dc1-139">hello type property must be set to: **OnPremisesMySql**</span></span> |<span data-ttu-id="c8dc1-140">Ano</span><span class="sxs-lookup"><span data-stu-id="c8dc1-140">Yes</span></span> |
| <span data-ttu-id="c8dc1-141">server</span><span class="sxs-lookup"><span data-stu-id="c8dc1-141">server</span></span> |<span data-ttu-id="c8dc1-142">Název serveru MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-142">Name of hello MySQL server.</span></span> |<span data-ttu-id="c8dc1-143">Ano</span><span class="sxs-lookup"><span data-stu-id="c8dc1-143">Yes</span></span> |
| <span data-ttu-id="c8dc1-144">Databáze</span><span class="sxs-lookup"><span data-stu-id="c8dc1-144">database</span></span> |<span data-ttu-id="c8dc1-145">Název databáze MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-145">Name of hello MySQL database.</span></span> |<span data-ttu-id="c8dc1-146">Ano</span><span class="sxs-lookup"><span data-stu-id="c8dc1-146">Yes</span></span> |
| <span data-ttu-id="c8dc1-147">Schéma</span><span class="sxs-lookup"><span data-stu-id="c8dc1-147">schema</span></span> |<span data-ttu-id="c8dc1-148">Název schématu hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-148">Name of hello schema in hello database.</span></span> |<span data-ttu-id="c8dc1-149">Ne</span><span class="sxs-lookup"><span data-stu-id="c8dc1-149">No</span></span> |
| <span data-ttu-id="c8dc1-150">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-150">authenticationType</span></span> |<span data-ttu-id="c8dc1-151">Typ ověřování používá databázi MySQL toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-151">Type of authentication used tooconnect toohello MySQL database.</span></span> <span data-ttu-id="c8dc1-152">Možné hodnoty jsou: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-152">Possible values are: `Basic`.</span></span> |<span data-ttu-id="c8dc1-153">Ano</span><span class="sxs-lookup"><span data-stu-id="c8dc1-153">Yes</span></span> |
| <span data-ttu-id="c8dc1-154">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="c8dc1-154">username</span></span> |<span data-ttu-id="c8dc1-155">Zadejte uživatelské jméno tooconnect toohello MySQL databázi.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-155">Specify user name tooconnect toohello MySQL database.</span></span> |<span data-ttu-id="c8dc1-156">Ano</span><span class="sxs-lookup"><span data-stu-id="c8dc1-156">Yes</span></span> |
| <span data-ttu-id="c8dc1-157">heslo</span><span class="sxs-lookup"><span data-stu-id="c8dc1-157">password</span></span> |<span data-ttu-id="c8dc1-158">Zadejte heslo pro hello uživatelského účtu, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-158">Specify password for hello user account you specified.</span></span> |<span data-ttu-id="c8dc1-159">Ano</span><span class="sxs-lookup"><span data-stu-id="c8dc1-159">Yes</span></span> |
| <span data-ttu-id="c8dc1-160">gatewayName</span><span class="sxs-lookup"><span data-stu-id="c8dc1-160">gatewayName</span></span> |<span data-ttu-id="c8dc1-161">Název hello brány, kterou hello služba Data Factory by měl použít toohello tooconnect, místní databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-161">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises MySQL database.</span></span> |<span data-ttu-id="c8dc1-162">Ano</span><span class="sxs-lookup"><span data-stu-id="c8dc1-162">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="c8dc1-163">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="c8dc1-163">Dataset properties</span></span>
<span data-ttu-id="c8dc1-164">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-164">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="c8dc1-165">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="c8dc1-166">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-166">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="c8dc1-167">rámci typeProperties Hello část datové sady typ **RelationalTable** (což zahrnuje datová sada MySQL) má následující vlastnosti hello</span><span class="sxs-lookup"><span data-stu-id="c8dc1-167">hello typeProperties section for dataset of type **RelationalTable** (which includes MySQL dataset) has hello following properties</span></span>

| <span data-ttu-id="c8dc1-168">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c8dc1-168">Property</span></span> | <span data-ttu-id="c8dc1-169">Popis</span><span class="sxs-lookup"><span data-stu-id="c8dc1-169">Description</span></span> | <span data-ttu-id="c8dc1-170">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c8dc1-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c8dc1-171">tableName</span><span class="sxs-lookup"><span data-stu-id="c8dc1-171">tableName</span></span> |<span data-ttu-id="c8dc1-172">Název tabulky hello v hello instance databáze MySQL, která je propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-172">Name of hello table in hello MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="c8dc1-173">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="c8dc1-173">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="c8dc1-174">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="c8dc1-174">Copy activity properties</span></span>
<span data-ttu-id="c8dc1-175">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-175">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="c8dc1-176">Vlastnosti, například název, popis, vstupní a výstupní tabulky, jsou zásady jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-176">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="c8dc1-177">Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-177">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="c8dc1-178">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-178">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="c8dc1-179">Pokud je zdroj v aktivitě kopírování typu **RelationalSource** (která zahrnuje MySQL), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="c8dc1-179">When source in copy activity is of type **RelationalSource** (which includes MySQL), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="c8dc1-180">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c8dc1-180">Property</span></span> | <span data-ttu-id="c8dc1-181">Popis</span><span class="sxs-lookup"><span data-stu-id="c8dc1-181">Description</span></span> | <span data-ttu-id="c8dc1-182">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="c8dc1-182">Allowed values</span></span> | <span data-ttu-id="c8dc1-183">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c8dc1-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c8dc1-184">query</span><span class="sxs-lookup"><span data-stu-id="c8dc1-184">query</span></span> |<span data-ttu-id="c8dc1-185">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-185">Use hello custom query tooread data.</span></span> |<span data-ttu-id="c8dc1-186">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-186">SQL query string.</span></span> <span data-ttu-id="c8dc1-187">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-187">For example: select * from MyTable.</span></span> |<span data-ttu-id="c8dc1-188">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="c8dc1-188">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-mysql-tooazure-blob"></a><span data-ttu-id="c8dc1-189">Příklad JSON: kopírování dat z databáze MySQL tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="c8dc1-189">JSON example: Copy data from MySQL tooAzure Blob</span></span>
<span data-ttu-id="c8dc1-190">Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-190">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="c8dc1-191">Ukazuje, jak toocopy data z databáze MySQL místní databázi tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-191">It shows how toocopy data from an on-premises MySQL database tooan Azure Blob Storage.</span></span> <span data-ttu-id="c8dc1-192">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-192">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c8dc1-193">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-193">This sample provides JSON snippets.</span></span> <span data-ttu-id="c8dc1-194">Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-194">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="c8dc1-195">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-195">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="c8dc1-196">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="c8dc1-196">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="c8dc1-197">Propojené služby typu [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-197">A linked service of type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="c8dc1-198">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-198">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="c8dc1-199">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-199">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="c8dc1-200">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-200">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="c8dc1-201">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-201">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="c8dc1-202">Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob tooa databáze MySQL každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-202">hello sample copies data from a query result in MySQL database tooa blob hourly.</span></span> <span data-ttu-id="c8dc1-203">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-203">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="c8dc1-204">Jako první krok nastavte Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-204">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="c8dc1-205">Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-205">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="c8dc1-206">**MySQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="c8dc1-206">**MySQL linked service:**</span></span>

```JSON
    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }
```

<span data-ttu-id="c8dc1-207">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="c8dc1-207">**Azure Storage linked service:**</span></span>

```JSON
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

<span data-ttu-id="c8dc1-208">**MySQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="c8dc1-208">**MySQL input dataset:**</span></span>

<span data-ttu-id="c8dc1-209">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v MySQL a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-209">hello sample assumes you have created a table “MyTable” in MySQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="c8dc1-210">Nastavení "externí": "PRAVDA" informuje služba Data Factory hello Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-210">Setting “external”: ”true” informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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

<span data-ttu-id="c8dc1-211">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="c8dc1-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="c8dc1-212">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-212">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="c8dc1-213">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-213">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="c8dc1-214">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-214">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```JSON
    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="c8dc1-215">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="c8dc1-215">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="c8dc1-216">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-216">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="c8dc1-217">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-217">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="c8dc1-218">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-218">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```JSON
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }
```


### <a name="type-mapping-for-mysql"></a><span data-ttu-id="c8dc1-219">Mapování typu pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="c8dc1-219">Type mapping for MySQL</span></span>
<span data-ttu-id="c8dc1-220">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello dvoustupňový přístup následující:</span><span class="sxs-lookup"><span data-stu-id="c8dc1-220">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="c8dc1-221">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="c8dc1-221">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="c8dc1-222">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="c8dc1-222">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="c8dc1-223">Při přesunu dat tooMySQL, hello následující mapování se používají z databáze MySQL typy too.NET typů.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-223">When moving data tooMySQL, hello following mappings are used from MySQL types too.NET types.</span></span>

| <span data-ttu-id="c8dc1-224">Typ databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="c8dc1-224">MySQL Database type</span></span> | <span data-ttu-id="c8dc1-225">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="c8dc1-225">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="c8dc1-226">bigint bez znaménka</span><span class="sxs-lookup"><span data-stu-id="c8dc1-226">bigint unsigned</span></span> |<span data-ttu-id="c8dc1-227">Decimal</span><span class="sxs-lookup"><span data-stu-id="c8dc1-227">Decimal</span></span> |
| <span data-ttu-id="c8dc1-228">bigint</span><span class="sxs-lookup"><span data-stu-id="c8dc1-228">bigint</span></span> |<span data-ttu-id="c8dc1-229">Int64</span><span class="sxs-lookup"><span data-stu-id="c8dc1-229">Int64</span></span> |
| <span data-ttu-id="c8dc1-230">Bit</span><span class="sxs-lookup"><span data-stu-id="c8dc1-230">bit</span></span> |<span data-ttu-id="c8dc1-231">Decimal</span><span class="sxs-lookup"><span data-stu-id="c8dc1-231">Decimal</span></span> |
| <span data-ttu-id="c8dc1-232">Objekt BLOB</span><span class="sxs-lookup"><span data-stu-id="c8dc1-232">blob</span></span> |<span data-ttu-id="c8dc1-233">Byte]</span><span class="sxs-lookup"><span data-stu-id="c8dc1-233">Byte[]</span></span> |
| <span data-ttu-id="c8dc1-234">BOOL</span><span class="sxs-lookup"><span data-stu-id="c8dc1-234">bool</span></span> |<span data-ttu-id="c8dc1-235">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="c8dc1-235">Boolean</span></span> |
| <span data-ttu-id="c8dc1-236">Char</span><span class="sxs-lookup"><span data-stu-id="c8dc1-236">char</span></span> |<span data-ttu-id="c8dc1-237">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c8dc1-237">String</span></span> |
| <span data-ttu-id="c8dc1-238">Datum</span><span class="sxs-lookup"><span data-stu-id="c8dc1-238">date</span></span> |<span data-ttu-id="c8dc1-239">Data a času</span><span class="sxs-lookup"><span data-stu-id="c8dc1-239">Datetime</span></span> |
| <span data-ttu-id="c8dc1-240">Data a času</span><span class="sxs-lookup"><span data-stu-id="c8dc1-240">datetime</span></span> |<span data-ttu-id="c8dc1-241">Data a času</span><span class="sxs-lookup"><span data-stu-id="c8dc1-241">Datetime</span></span> |
| <span data-ttu-id="c8dc1-242">Decimal</span><span class="sxs-lookup"><span data-stu-id="c8dc1-242">decimal</span></span> |<span data-ttu-id="c8dc1-243">Decimal</span><span class="sxs-lookup"><span data-stu-id="c8dc1-243">Decimal</span></span> |
| <span data-ttu-id="c8dc1-244">Dvojitá přesnost</span><span class="sxs-lookup"><span data-stu-id="c8dc1-244">double precision</span></span> |<span data-ttu-id="c8dc1-245">Double</span><span class="sxs-lookup"><span data-stu-id="c8dc1-245">Double</span></span> |
| <span data-ttu-id="c8dc1-246">Double</span><span class="sxs-lookup"><span data-stu-id="c8dc1-246">double</span></span> |<span data-ttu-id="c8dc1-247">Double</span><span class="sxs-lookup"><span data-stu-id="c8dc1-247">Double</span></span> |
| <span data-ttu-id="c8dc1-248">výčet</span><span class="sxs-lookup"><span data-stu-id="c8dc1-248">enum</span></span> |<span data-ttu-id="c8dc1-249">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c8dc1-249">String</span></span> |
| <span data-ttu-id="c8dc1-250">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="c8dc1-250">float</span></span> |<span data-ttu-id="c8dc1-251">Jeden</span><span class="sxs-lookup"><span data-stu-id="c8dc1-251">Single</span></span> |
| <span data-ttu-id="c8dc1-252">int bez znaménka</span><span class="sxs-lookup"><span data-stu-id="c8dc1-252">int unsigned</span></span> |<span data-ttu-id="c8dc1-253">Int64</span><span class="sxs-lookup"><span data-stu-id="c8dc1-253">Int64</span></span> |
| <span data-ttu-id="c8dc1-254">celá čísla</span><span class="sxs-lookup"><span data-stu-id="c8dc1-254">int</span></span> |<span data-ttu-id="c8dc1-255">Int32</span><span class="sxs-lookup"><span data-stu-id="c8dc1-255">Int32</span></span> |
| <span data-ttu-id="c8dc1-256">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="c8dc1-256">integer unsigned</span></span> |<span data-ttu-id="c8dc1-257">Int64</span><span class="sxs-lookup"><span data-stu-id="c8dc1-257">Int64</span></span> |
| <span data-ttu-id="c8dc1-258">celé číslo</span><span class="sxs-lookup"><span data-stu-id="c8dc1-258">integer</span></span> |<span data-ttu-id="c8dc1-259">Int32</span><span class="sxs-lookup"><span data-stu-id="c8dc1-259">Int32</span></span> |
| <span data-ttu-id="c8dc1-260">dlouhé varbinary</span><span class="sxs-lookup"><span data-stu-id="c8dc1-260">long varbinary</span></span> |<span data-ttu-id="c8dc1-261">Byte]</span><span class="sxs-lookup"><span data-stu-id="c8dc1-261">Byte[]</span></span> |
| <span data-ttu-id="c8dc1-262">dlouhé varchar</span><span class="sxs-lookup"><span data-stu-id="c8dc1-262">long varchar</span></span> |<span data-ttu-id="c8dc1-263">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c8dc1-263">String</span></span> |
| <span data-ttu-id="c8dc1-264">longblob</span><span class="sxs-lookup"><span data-stu-id="c8dc1-264">longblob</span></span> |<span data-ttu-id="c8dc1-265">Byte]</span><span class="sxs-lookup"><span data-stu-id="c8dc1-265">Byte[]</span></span> |
| <span data-ttu-id="c8dc1-266">LONGTEXT</span><span class="sxs-lookup"><span data-stu-id="c8dc1-266">longtext</span></span> |<span data-ttu-id="c8dc1-267">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c8dc1-267">String</span></span> |
| <span data-ttu-id="c8dc1-268">mediumblob</span><span class="sxs-lookup"><span data-stu-id="c8dc1-268">mediumblob</span></span> |<span data-ttu-id="c8dc1-269">Byte]</span><span class="sxs-lookup"><span data-stu-id="c8dc1-269">Byte[]</span></span> |
| <span data-ttu-id="c8dc1-270">mediumint bez znaménka</span><span class="sxs-lookup"><span data-stu-id="c8dc1-270">mediumint unsigned</span></span> |<span data-ttu-id="c8dc1-271">Int64</span><span class="sxs-lookup"><span data-stu-id="c8dc1-271">Int64</span></span> |
| <span data-ttu-id="c8dc1-272">mediumint</span><span class="sxs-lookup"><span data-stu-id="c8dc1-272">mediumint</span></span> |<span data-ttu-id="c8dc1-273">Int32</span><span class="sxs-lookup"><span data-stu-id="c8dc1-273">Int32</span></span> |
| <span data-ttu-id="c8dc1-274">mediumtext</span><span class="sxs-lookup"><span data-stu-id="c8dc1-274">mediumtext</span></span> |<span data-ttu-id="c8dc1-275">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c8dc1-275">String</span></span> |
| <span data-ttu-id="c8dc1-276">číselné</span><span class="sxs-lookup"><span data-stu-id="c8dc1-276">numeric</span></span> |<span data-ttu-id="c8dc1-277">Decimal</span><span class="sxs-lookup"><span data-stu-id="c8dc1-277">Decimal</span></span> |
| <span data-ttu-id="c8dc1-278">skutečné</span><span class="sxs-lookup"><span data-stu-id="c8dc1-278">real</span></span> |<span data-ttu-id="c8dc1-279">Double</span><span class="sxs-lookup"><span data-stu-id="c8dc1-279">Double</span></span> |
| <span data-ttu-id="c8dc1-280">nastavení</span><span class="sxs-lookup"><span data-stu-id="c8dc1-280">set</span></span> |<span data-ttu-id="c8dc1-281">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c8dc1-281">String</span></span> |
| <span data-ttu-id="c8dc1-282">smallint bez znaménka</span><span class="sxs-lookup"><span data-stu-id="c8dc1-282">smallint unsigned</span></span> |<span data-ttu-id="c8dc1-283">Int32</span><span class="sxs-lookup"><span data-stu-id="c8dc1-283">Int32</span></span> |
| <span data-ttu-id="c8dc1-284">smallint</span><span class="sxs-lookup"><span data-stu-id="c8dc1-284">smallint</span></span> |<span data-ttu-id="c8dc1-285">Int16</span><span class="sxs-lookup"><span data-stu-id="c8dc1-285">Int16</span></span> |
| <span data-ttu-id="c8dc1-286">Text</span><span class="sxs-lookup"><span data-stu-id="c8dc1-286">text</span></span> |<span data-ttu-id="c8dc1-287">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c8dc1-287">String</span></span> |
| <span data-ttu-id="c8dc1-288">time</span><span class="sxs-lookup"><span data-stu-id="c8dc1-288">time</span></span> |<span data-ttu-id="c8dc1-289">Časový interval</span><span class="sxs-lookup"><span data-stu-id="c8dc1-289">TimeSpan</span></span> |
| <span data-ttu-id="c8dc1-290">časové razítko</span><span class="sxs-lookup"><span data-stu-id="c8dc1-290">timestamp</span></span> |<span data-ttu-id="c8dc1-291">Data a času</span><span class="sxs-lookup"><span data-stu-id="c8dc1-291">Datetime</span></span> |
| <span data-ttu-id="c8dc1-292">tinyblob</span><span class="sxs-lookup"><span data-stu-id="c8dc1-292">tinyblob</span></span> |<span data-ttu-id="c8dc1-293">Byte]</span><span class="sxs-lookup"><span data-stu-id="c8dc1-293">Byte[]</span></span> |
| <span data-ttu-id="c8dc1-294">tinyint bez znaménka</span><span class="sxs-lookup"><span data-stu-id="c8dc1-294">tinyint unsigned</span></span> |<span data-ttu-id="c8dc1-295">Int16</span><span class="sxs-lookup"><span data-stu-id="c8dc1-295">Int16</span></span> |
| <span data-ttu-id="c8dc1-296">tinyint</span><span class="sxs-lookup"><span data-stu-id="c8dc1-296">tinyint</span></span> |<span data-ttu-id="c8dc1-297">Int16</span><span class="sxs-lookup"><span data-stu-id="c8dc1-297">Int16</span></span> |
| <span data-ttu-id="c8dc1-298">tinytext</span><span class="sxs-lookup"><span data-stu-id="c8dc1-298">tinytext</span></span> |<span data-ttu-id="c8dc1-299">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c8dc1-299">String</span></span> |
| <span data-ttu-id="c8dc1-300">varchar</span><span class="sxs-lookup"><span data-stu-id="c8dc1-300">varchar</span></span> |<span data-ttu-id="c8dc1-301">Řetězec</span><span class="sxs-lookup"><span data-stu-id="c8dc1-301">String</span></span> |
| <span data-ttu-id="c8dc1-302">Rok</span><span class="sxs-lookup"><span data-stu-id="c8dc1-302">year</span></span> |<span data-ttu-id="c8dc1-303">celá čísla</span><span class="sxs-lookup"><span data-stu-id="c8dc1-303">Int</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="c8dc1-304">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="c8dc1-304">Map source toosink columns</span></span>
<span data-ttu-id="c8dc1-305">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-305">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="c8dc1-306">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="c8dc1-306">Repeatable read from relational sources</span></span>
<span data-ttu-id="c8dc1-307">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-307">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="c8dc1-308">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-308">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="c8dc1-309">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-309">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="c8dc1-310">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-310">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="c8dc1-311">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="c8dc1-311">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="c8dc1-312">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="c8dc1-312">Performance and Tuning</span></span>
<span data-ttu-id="c8dc1-313">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="c8dc1-313">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
