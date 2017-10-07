---
title: "aaaMove data z Teradata pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o Teradata konektor pro hello služba Data Factory, který umožňuje přesun dat z databáze Teradata"
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
ms.openlocfilehash: 79153476157666463b499edaa7585adaf8ad3bee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-teradata-using-azure-data-factory"></a><span data-ttu-id="bcb8b-103">Přesun dat z Teradata pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="bcb8b-103">Move data from Teradata using Azure Data Factory</span></span>
<span data-ttu-id="bcb8b-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze Teradata místně.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises Teradata database.</span></span> <span data-ttu-id="bcb8b-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="bcb8b-106">Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní Teradata data.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-106">You can copy data from an on-premises Teradata data store tooany supported sink data store.</span></span> <span data-ttu-id="bcb8b-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="bcb8b-108">Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat z datové Teradata tooother datová úložiště, ale ne pro přesun dat z jiného úložiště dat úložiště tooa Teradata data.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-108">Data factory currently supports only moving data from a Teradata data store tooother data stores, but not for moving data from other data stores tooa Teradata data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="bcb8b-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bcb8b-109">Prerequisites</span></span>
<span data-ttu-id="bcb8b-110">Objekt pro vytváření dat podporuje připojování zdroje Teradata tooon místní prostřednictvím hello Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-110">Data factory supports connecting tooon-premises Teradata sources via hello Data Management Gateway.</span></span> <span data-ttu-id="bcb8b-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="bcb8b-112">Vyžaduje se brána, i když hello Teradata je hostovaná ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-112">Gateway is required even if hello Teradata is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="bcb8b-113">Hello brány můžete nainstalovat na stejný virtuální počítač IaaS jako hello data uložit nebo na jiný virtuální počítač stejně dlouho jako hello brány může připojit databázi toohello hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-113">You can install hello gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="bcb8b-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="bcb8b-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="bcb8b-115">Supported versions and installation</span></span>
<span data-ttu-id="bcb8b-116">Brána pro správu dat tooconnect toohello Teradata databáze, musíte tooinstall hello [zprostředkovatel dat .NET pro Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) verzi 14 nebo výše na hello stejné systému jako brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-116">For Data Management Gateway tooconnect toohello Teradata Database, you need tooinstall hello [.NET Data Provider for Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) version 14 or above on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="bcb8b-117">Teradata verze 12 a vyšší je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-117">Teradata version 12 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="bcb8b-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="bcb8b-118">Getting started</span></span>
<span data-ttu-id="bcb8b-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="bcb8b-120">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="bcb8b-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="bcb8b-122">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="bcb8b-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="bcb8b-124">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="bcb8b-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="bcb8b-125">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="bcb8b-126">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-126">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="bcb8b-127">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="bcb8b-128">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-128">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="bcb8b-129">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="bcb8b-130">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat Teradata místní, naleznete v tématu [JSON příklad: kopírování dat z Teradata tooAzure Blob](#json-example-copy-data-from-teradata-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-130">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises Teradata data store, see [JSON example: Copy data from Teradata tooAzure Blob](#json-example-copy-data-from-teradata-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="bcb8b-131">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa Teradata úložiště dat:</span><span class="sxs-lookup"><span data-stu-id="bcb8b-131">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Teradata data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="bcb8b-132">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="bcb8b-132">Linked service properties</span></span>
<span data-ttu-id="bcb8b-133">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooTeradata propojené služby.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-133">hello following table provides description for JSON elements specific tooTeradata linked service.</span></span>

| <span data-ttu-id="bcb8b-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bcb8b-134">Property</span></span> | <span data-ttu-id="bcb8b-135">Popis</span><span class="sxs-lookup"><span data-stu-id="bcb8b-135">Description</span></span> | <span data-ttu-id="bcb8b-136">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="bcb8b-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bcb8b-137">type</span><span class="sxs-lookup"><span data-stu-id="bcb8b-137">type</span></span> |<span data-ttu-id="bcb8b-138">vlastnost typu Hello musí být nastavena na: **OnPremisesTeradata**</span><span class="sxs-lookup"><span data-stu-id="bcb8b-138">hello type property must be set to: **OnPremisesTeradata**</span></span> |<span data-ttu-id="bcb8b-139">Ano</span><span class="sxs-lookup"><span data-stu-id="bcb8b-139">Yes</span></span> |
| <span data-ttu-id="bcb8b-140">server</span><span class="sxs-lookup"><span data-stu-id="bcb8b-140">server</span></span> |<span data-ttu-id="bcb8b-141">Název serveru Teradata hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-141">Name of hello Teradata server.</span></span> |<span data-ttu-id="bcb8b-142">Ano</span><span class="sxs-lookup"><span data-stu-id="bcb8b-142">Yes</span></span> |
| <span data-ttu-id="bcb8b-143">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-143">authenticationType</span></span> |<span data-ttu-id="bcb8b-144">Typ ověřování používá tooconnect toohello Teradata databáze.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-144">Type of authentication used tooconnect toohello Teradata database.</span></span> <span data-ttu-id="bcb8b-145">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-145">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="bcb8b-146">Ano</span><span class="sxs-lookup"><span data-stu-id="bcb8b-146">Yes</span></span> |
| <span data-ttu-id="bcb8b-147">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="bcb8b-147">username</span></span> |<span data-ttu-id="bcb8b-148">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-148">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="bcb8b-149">Ne</span><span class="sxs-lookup"><span data-stu-id="bcb8b-149">No</span></span> |
| <span data-ttu-id="bcb8b-150">heslo</span><span class="sxs-lookup"><span data-stu-id="bcb8b-150">password</span></span> |<span data-ttu-id="bcb8b-151">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-151">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="bcb8b-152">Ne</span><span class="sxs-lookup"><span data-stu-id="bcb8b-152">No</span></span> |
| <span data-ttu-id="bcb8b-153">gatewayName</span><span class="sxs-lookup"><span data-stu-id="bcb8b-153">gatewayName</span></span> |<span data-ttu-id="bcb8b-154">Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní Teradata databázi.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-154">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Teradata database.</span></span> |<span data-ttu-id="bcb8b-155">Ano</span><span class="sxs-lookup"><span data-stu-id="bcb8b-155">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="bcb8b-156">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="bcb8b-156">Dataset properties</span></span>
<span data-ttu-id="bcb8b-157">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-157">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="bcb8b-158">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-158">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="bcb8b-159">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-159">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="bcb8b-160">Momentálně nejsou k dispozici žádné vlastnosti typu podporované pro datovou sadu hello Teradata.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-160">Currently, there are no type properties supported for hello Teradata dataset.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="bcb8b-161">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="bcb8b-161">Copy activity properties</span></span>
<span data-ttu-id="bcb8b-162">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-162">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="bcb8b-163">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-163">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="bcb8b-164">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-164">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="bcb8b-165">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-165">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="bcb8b-166">Pokud zdroj hello je typu **RelationalSource** (která zahrnuje Teradata), hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="bcb8b-166">When hello source is of type **RelationalSource** (which includes Teradata), hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="bcb8b-167">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bcb8b-167">Property</span></span> | <span data-ttu-id="bcb8b-168">Popis</span><span class="sxs-lookup"><span data-stu-id="bcb8b-168">Description</span></span> | <span data-ttu-id="bcb8b-169">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="bcb8b-169">Allowed values</span></span> | <span data-ttu-id="bcb8b-170">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="bcb8b-170">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bcb8b-171">query</span><span class="sxs-lookup"><span data-stu-id="bcb8b-171">query</span></span> |<span data-ttu-id="bcb8b-172">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-172">Use hello custom query tooread data.</span></span> |<span data-ttu-id="bcb8b-173">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-173">SQL query string.</span></span> <span data-ttu-id="bcb8b-174">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-174">For example: select * from MyTable.</span></span> |<span data-ttu-id="bcb8b-175">Ano</span><span class="sxs-lookup"><span data-stu-id="bcb8b-175">Yes</span></span> |

### <a name="json-example-copy-data-from-teradata-tooazure-blob"></a><span data-ttu-id="bcb8b-176">Příklad JSON: kopírování dat z Teradata tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="bcb8b-176">JSON example: Copy data from Teradata tooAzure Blob</span></span>
<span data-ttu-id="bcb8b-177">Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-177">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="bcb8b-178">Ukazují jak toocopy data z Teradata tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-178">They show how toocopy data from Teradata tooAzure Blob Storage.</span></span> <span data-ttu-id="bcb8b-179">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-179">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="bcb8b-180">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="bcb8b-180">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="bcb8b-181">Propojené služby typu [OnPremisesTeradata](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-181">A linked service of type [OnPremisesTeradata](#linked-service-properties).</span></span>
2. <span data-ttu-id="bcb8b-182">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-182">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="bcb8b-183">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-183">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="bcb8b-184">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-184">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="bcb8b-185">Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-185">hello [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="bcb8b-186">Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob tooa databáze Teradata každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-186">hello sample copies data from a query result in Teradata database tooa blob every hour.</span></span> <span data-ttu-id="bcb8b-187">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-187">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="bcb8b-188">Jako první krok nastavte Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-188">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="bcb8b-189">Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-189">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="bcb8b-190">**Teradata propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="bcb8b-190">**Teradata linked service:**</span></span>

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

<span data-ttu-id="bcb8b-191">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="bcb8b-191">**Azure Blob storage linked service:**</span></span>

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

<span data-ttu-id="bcb8b-192">**Teradata vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="bcb8b-192">**Teradata input dataset:**</span></span>

<span data-ttu-id="bcb8b-193">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v Teradata a obsahuje sloupec s názvem "časové razítko" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-193">hello sample assumes you have created a table “MyTable” in Teradata and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="bcb8b-194">Nastavení "externí": true informuje služba Data Factory hello Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-194">Setting “external”: true informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="bcb8b-195">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="bcb8b-195">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="bcb8b-196">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-196">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="bcb8b-197">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-197">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="bcb8b-198">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-198">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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
<span data-ttu-id="bcb8b-199">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="bcb8b-199">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="bcb8b-200">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-200">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="bcb8b-201">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-201">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="bcb8b-202">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-202">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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
## <a name="type-mapping-for-teradata"></a><span data-ttu-id="bcb8b-203">Mapování typu pro Teradata</span><span class="sxs-lookup"><span data-stu-id="bcb8b-203">Type mapping for Teradata</span></span>
<span data-ttu-id="bcb8b-204">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku hello aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:</span><span class="sxs-lookup"><span data-stu-id="bcb8b-204">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, hello Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="bcb8b-205">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="bcb8b-205">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="bcb8b-206">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="bcb8b-206">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="bcb8b-207">Při přesunu dat tooTeradata, hello následující mapování se používají z typu too.NET typu Teradata.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-207">When moving data tooTeradata, hello following mappings are used from Teradata type too.NET type.</span></span>

| <span data-ttu-id="bcb8b-208">Typ databáze Teradata</span><span class="sxs-lookup"><span data-stu-id="bcb8b-208">Teradata Database type</span></span> | <span data-ttu-id="bcb8b-209">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="bcb8b-209">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="bcb8b-210">Char</span><span class="sxs-lookup"><span data-stu-id="bcb8b-210">Char</span></span> |<span data-ttu-id="bcb8b-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-211">String</span></span> |
| <span data-ttu-id="bcb8b-212">Datový typ CLOB</span><span class="sxs-lookup"><span data-stu-id="bcb8b-212">Clob</span></span> |<span data-ttu-id="bcb8b-213">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-213">String</span></span> |
| <span data-ttu-id="bcb8b-214">Obrázek</span><span class="sxs-lookup"><span data-stu-id="bcb8b-214">Graphic</span></span> |<span data-ttu-id="bcb8b-215">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-215">String</span></span> |
| <span data-ttu-id="bcb8b-216">VarChar</span><span class="sxs-lookup"><span data-stu-id="bcb8b-216">VarChar</span></span> |<span data-ttu-id="bcb8b-217">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-217">String</span></span> |
| <span data-ttu-id="bcb8b-218">VarGraphic</span><span class="sxs-lookup"><span data-stu-id="bcb8b-218">VarGraphic</span></span> |<span data-ttu-id="bcb8b-219">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-219">String</span></span> |
| <span data-ttu-id="bcb8b-220">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="bcb8b-220">Blob</span></span> |<span data-ttu-id="bcb8b-221">Byte]</span><span class="sxs-lookup"><span data-stu-id="bcb8b-221">Byte[]</span></span> |
| <span data-ttu-id="bcb8b-222">Bajtů</span><span class="sxs-lookup"><span data-stu-id="bcb8b-222">Byte</span></span> |<span data-ttu-id="bcb8b-223">Byte]</span><span class="sxs-lookup"><span data-stu-id="bcb8b-223">Byte[]</span></span> |
| <span data-ttu-id="bcb8b-224">VarByte</span><span class="sxs-lookup"><span data-stu-id="bcb8b-224">VarByte</span></span> |<span data-ttu-id="bcb8b-225">Byte]</span><span class="sxs-lookup"><span data-stu-id="bcb8b-225">Byte[]</span></span> |
| <span data-ttu-id="bcb8b-226">BigInt</span><span class="sxs-lookup"><span data-stu-id="bcb8b-226">BigInt</span></span> |<span data-ttu-id="bcb8b-227">Int64</span><span class="sxs-lookup"><span data-stu-id="bcb8b-227">Int64</span></span> |
| <span data-ttu-id="bcb8b-228">ByteInt</span><span class="sxs-lookup"><span data-stu-id="bcb8b-228">ByteInt</span></span> |<span data-ttu-id="bcb8b-229">Int16</span><span class="sxs-lookup"><span data-stu-id="bcb8b-229">Int16</span></span> |
| <span data-ttu-id="bcb8b-230">Decimal</span><span class="sxs-lookup"><span data-stu-id="bcb8b-230">Decimal</span></span> |<span data-ttu-id="bcb8b-231">Decimal</span><span class="sxs-lookup"><span data-stu-id="bcb8b-231">Decimal</span></span> |
| <span data-ttu-id="bcb8b-232">Double</span><span class="sxs-lookup"><span data-stu-id="bcb8b-232">Double</span></span> |<span data-ttu-id="bcb8b-233">Double</span><span class="sxs-lookup"><span data-stu-id="bcb8b-233">Double</span></span> |
| <span data-ttu-id="bcb8b-234">Integer</span><span class="sxs-lookup"><span data-stu-id="bcb8b-234">Integer</span></span> |<span data-ttu-id="bcb8b-235">Int32</span><span class="sxs-lookup"><span data-stu-id="bcb8b-235">Int32</span></span> |
| <span data-ttu-id="bcb8b-236">Číslo</span><span class="sxs-lookup"><span data-stu-id="bcb8b-236">Number</span></span> |<span data-ttu-id="bcb8b-237">Double</span><span class="sxs-lookup"><span data-stu-id="bcb8b-237">Double</span></span> |
| <span data-ttu-id="bcb8b-238">SmallInt</span><span class="sxs-lookup"><span data-stu-id="bcb8b-238">SmallInt</span></span> |<span data-ttu-id="bcb8b-239">Int16</span><span class="sxs-lookup"><span data-stu-id="bcb8b-239">Int16</span></span> |
| <span data-ttu-id="bcb8b-240">Datum</span><span class="sxs-lookup"><span data-stu-id="bcb8b-240">Date</span></span> |<span data-ttu-id="bcb8b-241">Data a času</span><span class="sxs-lookup"><span data-stu-id="bcb8b-241">DateTime</span></span> |
| <span data-ttu-id="bcb8b-242">Čas</span><span class="sxs-lookup"><span data-stu-id="bcb8b-242">Time</span></span> |<span data-ttu-id="bcb8b-243">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-243">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-244">Čas s časovým pásmem</span><span class="sxs-lookup"><span data-stu-id="bcb8b-244">Time With Time Zone</span></span> |<span data-ttu-id="bcb8b-245">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-245">String</span></span> |
| <span data-ttu-id="bcb8b-246">časové razítko</span><span class="sxs-lookup"><span data-stu-id="bcb8b-246">Timestamp</span></span> |<span data-ttu-id="bcb8b-247">Data a času</span><span class="sxs-lookup"><span data-stu-id="bcb8b-247">DateTime</span></span> |
| <span data-ttu-id="bcb8b-248">Časové razítko s časovým pásmem</span><span class="sxs-lookup"><span data-stu-id="bcb8b-248">Timestamp With Time Zone</span></span> |<span data-ttu-id="bcb8b-249">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="bcb8b-249">DateTimeOffset</span></span> |
| <span data-ttu-id="bcb8b-250">Interval den</span><span class="sxs-lookup"><span data-stu-id="bcb8b-250">Interval Day</span></span> |<span data-ttu-id="bcb8b-251">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-251">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-252">Interval den tooHour</span><span class="sxs-lookup"><span data-stu-id="bcb8b-252">Interval Day tooHour</span></span> |<span data-ttu-id="bcb8b-253">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-253">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-254">Interval den tooMinute</span><span class="sxs-lookup"><span data-stu-id="bcb8b-254">Interval Day tooMinute</span></span> |<span data-ttu-id="bcb8b-255">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-255">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-256">Interval den tooSecond</span><span class="sxs-lookup"><span data-stu-id="bcb8b-256">Interval Day tooSecond</span></span> |<span data-ttu-id="bcb8b-257">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-257">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-258">Interval hodinu</span><span class="sxs-lookup"><span data-stu-id="bcb8b-258">Interval Hour</span></span> |<span data-ttu-id="bcb8b-259">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-259">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-260">Interval hodinu tooMinute</span><span class="sxs-lookup"><span data-stu-id="bcb8b-260">Interval Hour tooMinute</span></span> |<span data-ttu-id="bcb8b-261">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-261">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-262">Interval hodinu tooSecond</span><span class="sxs-lookup"><span data-stu-id="bcb8b-262">Interval Hour tooSecond</span></span> |<span data-ttu-id="bcb8b-263">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-263">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-264">Interval minutu</span><span class="sxs-lookup"><span data-stu-id="bcb8b-264">Interval Minute</span></span> |<span data-ttu-id="bcb8b-265">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-265">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-266">Interval minut tooSecond</span><span class="sxs-lookup"><span data-stu-id="bcb8b-266">Interval Minute tooSecond</span></span> |<span data-ttu-id="bcb8b-267">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-267">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-268">Interval druhý</span><span class="sxs-lookup"><span data-stu-id="bcb8b-268">Interval Second</span></span> |<span data-ttu-id="bcb8b-269">Časový interval</span><span class="sxs-lookup"><span data-stu-id="bcb8b-269">TimeSpan</span></span> |
| <span data-ttu-id="bcb8b-270">Interval roku</span><span class="sxs-lookup"><span data-stu-id="bcb8b-270">Interval Year</span></span> |<span data-ttu-id="bcb8b-271">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-271">String</span></span> |
| <span data-ttu-id="bcb8b-272">Interval tooMonth roku</span><span class="sxs-lookup"><span data-stu-id="bcb8b-272">Interval Year tooMonth</span></span> |<span data-ttu-id="bcb8b-273">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-273">String</span></span> |
| <span data-ttu-id="bcb8b-274">Interval měsíc</span><span class="sxs-lookup"><span data-stu-id="bcb8b-274">Interval Month</span></span> |<span data-ttu-id="bcb8b-275">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-275">String</span></span> |
| <span data-ttu-id="bcb8b-276">Period(Date)</span><span class="sxs-lookup"><span data-stu-id="bcb8b-276">Period(Date)</span></span> |<span data-ttu-id="bcb8b-277">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-277">String</span></span> |
| <span data-ttu-id="bcb8b-278">Period(Time)</span><span class="sxs-lookup"><span data-stu-id="bcb8b-278">Period(Time)</span></span> |<span data-ttu-id="bcb8b-279">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-279">String</span></span> |
| <span data-ttu-id="bcb8b-280">Období (čas s časovým pásmem)</span><span class="sxs-lookup"><span data-stu-id="bcb8b-280">Period(Time With Time Zone)</span></span> |<span data-ttu-id="bcb8b-281">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-281">String</span></span> |
| <span data-ttu-id="bcb8b-282">Period(Timestamp)</span><span class="sxs-lookup"><span data-stu-id="bcb8b-282">Period(Timestamp)</span></span> |<span data-ttu-id="bcb8b-283">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-283">String</span></span> |
| <span data-ttu-id="bcb8b-284">Období (časové razítko s časovým pásmem)</span><span class="sxs-lookup"><span data-stu-id="bcb8b-284">Period(Timestamp With Time Zone)</span></span> |<span data-ttu-id="bcb8b-285">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-285">String</span></span> |
| <span data-ttu-id="bcb8b-286">XML</span><span class="sxs-lookup"><span data-stu-id="bcb8b-286">Xml</span></span> |<span data-ttu-id="bcb8b-287">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bcb8b-287">String</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="bcb8b-288">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="bcb8b-288">Map source toosink columns</span></span>
<span data-ttu-id="bcb8b-289">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-289">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="bcb8b-290">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="bcb8b-290">Repeatable read from relational sources</span></span>
<span data-ttu-id="bcb8b-291">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-291">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="bcb8b-292">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-292">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="bcb8b-293">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-293">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="bcb8b-294">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-294">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="bcb8b-295">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="bcb8b-295">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="bcb8b-296">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="bcb8b-296">Performance and Tuning</span></span>
<span data-ttu-id="bcb8b-297">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="bcb8b-297">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
