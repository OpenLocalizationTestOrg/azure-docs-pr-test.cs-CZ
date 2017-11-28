---
title: "aaaMove data z MongoDB pomocí služby Data Factory | Microsoft Docs"
description: "Informace o tom, jak toomove data z MongoDB databáze pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 154e85712f27b978976c7499c43dde9429f124c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a><span data-ttu-id="9f2b3-103">Přesun dat z MongoDB pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="9f2b3-103">Move data From MongoDB using Azure Data Factory</span></span>
<span data-ttu-id="9f2b3-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove datech z místní databáze MongoDB.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises MongoDB database.</span></span> <span data-ttu-id="9f2b3-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="9f2b3-106">Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní MongoDB data.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-106">You can copy data from an on-premises MongoDB data store tooany supported sink data store.</span></span> <span data-ttu-id="9f2b3-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="9f2b3-108">Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat tooother uložit data z datové MongoDB, ale ne pro přesun dat z jiných dat úložiště tooan MongoDB úložiště.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-108">Data factory currently supports only moving data from a MongoDB data store tooother data stores, but not for moving data from other data stores tooan MongoDB datastore.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9f2b3-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f2b3-109">Prerequisites</span></span>
<span data-ttu-id="9f2b3-110">Pro hello Azure Data Factory služby toobe možné tooconnect tooyour místní databázi MongoDB je třeba nainstalovat hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="9f2b3-110">For hello Azure Data Factory service toobe able tooconnect tooyour on-premises MongoDB database, you must install hello following components:</span></span>

- <span data-ttu-id="9f2b3-111">Podporované verze MongoDB jsou: 2.4, 2.6, 3.0 a 3.2.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-111">Supported MongoDB versions are:  2.4, 2.6, 3.0, and 3.2.</span></span>
- <span data-ttu-id="9f2b3-112">Brána pro správu dat v databázi hello hostitelů hello stejný počítač nebo na samostatný počítač tooavoid, neslučitelných pro prostředky s databází hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-112">Data Management Gateway on hello same machine that hosts hello database or on a separate machine tooavoid competing for resources with hello database.</span></span> <span data-ttu-id="9f2b3-113">Brána pro správu dat je software, který se připojuje místní datové zdroje toocloud služby způsobem, zabezpečení a správě.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-113">Data Management Gateway is a software that connects on-premises data sources toocloud services in a secure and managed way.</span></span> <span data-ttu-id="9f2b3-114">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o Brána pro správu dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="9f2b3-115">V tématu [přesun dat z místní toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k nastavení brány hello toomove data datového kanálu.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-115">See [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway a data pipeline toomove data.</span></span>

    <span data-ttu-id="9f2b3-116">Při instalaci brány hello se automaticky nainstaluje tooMongoDB tooconnect použít ovladače Microsoft MongoDB ODBC.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-116">When you install hello gateway, it automatically installs a Microsoft MongoDB ODBC driver used tooconnect tooMongoDB.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9f2b3-117">Je nutné toouse hello brány tooconnect tooMongoDB i v případě, že je hostován v virtuální počítače Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-117">You need toouse hello gateway tooconnect tooMongoDB even if it is hosted in Azure IaaS VMs.</span></span> <span data-ttu-id="9f2b3-118">Pokud se pokoušíte tooconnect tooan instanci MongoDB hostované v cloudu, můžete také nainstalovat instanci brány hello v hello virtuálních počítačů IaaS.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-118">If you are trying tooconnect tooan instance of MongoDB hosted in cloud, you can also install hello gateway instance in hello IaaS VM.</span></span>

## <a name="getting-started"></a><span data-ttu-id="9f2b3-119">Začínáme</span><span class="sxs-lookup"><span data-stu-id="9f2b3-119">Getting started</span></span>
<span data-ttu-id="9f2b3-120">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní MongoDB data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-120">You can create a pipeline with a copy activity that moves data from an on-premises MongoDB data store by using different tools/APIs.</span></span>

<span data-ttu-id="9f2b3-121">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-121">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="9f2b3-122">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="9f2b3-123">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru **, **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-123">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="9f2b3-124">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="9f2b3-125">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="9f2b3-125">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="9f2b3-126">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-126">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="9f2b3-127">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-127">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="9f2b3-128">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="9f2b3-129">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-129">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="9f2b3-130">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="9f2b3-131">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z místního úložiště dat MongoDB, naleznete v tématu [JSON příklad: kopírování dat z MongoDB tooAzure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-131">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises MongoDB data store, see [JSON example: Copy data from MongoDB tooAzure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="9f2b3-132">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooMongoDB zdroje:</span><span class="sxs-lookup"><span data-stu-id="9f2b3-132">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooMongoDB source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="9f2b3-133">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="9f2b3-133">Linked service properties</span></span>
<span data-ttu-id="9f2b3-134">Hello následující tabulka obsahuje popis pro konkrétní elementy JSON příliš**OnPremisesMongoDB** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-134">hello following table provides description for JSON elements specific too**OnPremisesMongoDB** linked service.</span></span>

| <span data-ttu-id="9f2b3-135">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9f2b3-135">Property</span></span> | <span data-ttu-id="9f2b3-136">Popis</span><span class="sxs-lookup"><span data-stu-id="9f2b3-136">Description</span></span> | <span data-ttu-id="9f2b3-137">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9f2b3-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9f2b3-138">type</span><span class="sxs-lookup"><span data-stu-id="9f2b3-138">type</span></span> |<span data-ttu-id="9f2b3-139">vlastnost typu Hello musí být nastavena na: **OnPremisesMongoDb**</span><span class="sxs-lookup"><span data-stu-id="9f2b3-139">hello type property must be set to: **OnPremisesMongoDb**</span></span> |<span data-ttu-id="9f2b3-140">Ano</span><span class="sxs-lookup"><span data-stu-id="9f2b3-140">Yes</span></span> |
| <span data-ttu-id="9f2b3-141">server</span><span class="sxs-lookup"><span data-stu-id="9f2b3-141">server</span></span> |<span data-ttu-id="9f2b3-142">IP adresa nebo název hostitele serveru MongoDB hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-142">IP address or host name of hello MongoDB server.</span></span> |<span data-ttu-id="9f2b3-143">Ano</span><span class="sxs-lookup"><span data-stu-id="9f2b3-143">Yes</span></span> |
| <span data-ttu-id="9f2b3-144">port</span><span class="sxs-lookup"><span data-stu-id="9f2b3-144">port</span></span> |<span data-ttu-id="9f2b3-145">Port TCP, který hello serveru MongoDB toolisten používá pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-145">TCP port that hello MongoDB server uses toolisten for client connections.</span></span> |<span data-ttu-id="9f2b3-146">Volitelné, výchozí hodnota: 27017</span><span class="sxs-lookup"><span data-stu-id="9f2b3-146">Optional, default value: 27017</span></span> |
| <span data-ttu-id="9f2b3-147">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-147">authenticationType</span></span> |<span data-ttu-id="9f2b3-148">Základní, nebo anonymní.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-148">Basic, or Anonymous.</span></span> |<span data-ttu-id="9f2b3-149">Ano</span><span class="sxs-lookup"><span data-stu-id="9f2b3-149">Yes</span></span> |
| <span data-ttu-id="9f2b3-150">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="9f2b3-150">username</span></span> |<span data-ttu-id="9f2b3-151">Uživatel účet tooaccess MongoDB.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-151">User account tooaccess MongoDB.</span></span> |<span data-ttu-id="9f2b3-152">Ano (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-152">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="9f2b3-153">heslo</span><span class="sxs-lookup"><span data-stu-id="9f2b3-153">password</span></span> |<span data-ttu-id="9f2b3-154">Heslo pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-154">Password for hello user.</span></span> |<span data-ttu-id="9f2b3-155">Ano (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-155">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="9f2b3-156">authSource</span><span class="sxs-lookup"><span data-stu-id="9f2b3-156">authSource</span></span> |<span data-ttu-id="9f2b3-157">Název databáze hello MongoDB, že chcete toouse toocheck přihlašovacích údajů pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-157">Name of hello MongoDB database that you want toouse toocheck your credentials for authentication.</span></span> |<span data-ttu-id="9f2b3-158">Volitelný parametr (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-158">Optional (if basic authentication is used).</span></span> <span data-ttu-id="9f2b3-159">Výchozí: používá účet správce hello a hello databáze zadat pomocí vlastnost databaseName.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-159">default: uses hello admin account and hello database specified using databaseName property.</span></span> |
| <span data-ttu-id="9f2b3-160">Název databáze</span><span class="sxs-lookup"><span data-stu-id="9f2b3-160">databaseName</span></span> |<span data-ttu-id="9f2b3-161">Název databáze hello MongoDB, které chcete tooaccess.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-161">Name of hello MongoDB database that you want tooaccess.</span></span> |<span data-ttu-id="9f2b3-162">Ano</span><span class="sxs-lookup"><span data-stu-id="9f2b3-162">Yes</span></span> |
| <span data-ttu-id="9f2b3-163">gatewayName</span><span class="sxs-lookup"><span data-stu-id="9f2b3-163">gatewayName</span></span> |<span data-ttu-id="9f2b3-164">Název brány hello, který přistupuje k úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-164">Name of hello gateway that accesses hello data store.</span></span> |<span data-ttu-id="9f2b3-165">Ano</span><span class="sxs-lookup"><span data-stu-id="9f2b3-165">Yes</span></span> |
| <span data-ttu-id="9f2b3-166">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="9f2b3-166">encryptedCredential</span></span> |<span data-ttu-id="9f2b3-167">Přihlašovací údaje zašifrovaná pomocí brány.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-167">Credential encrypted by gateway.</span></span> |<span data-ttu-id="9f2b3-168">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="9f2b3-168">Optional</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="9f2b3-169">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="9f2b3-169">Dataset properties</span></span>
<span data-ttu-id="9f2b3-170">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-170">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="9f2b3-171">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="9f2b3-172">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-172">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="9f2b3-173">rámci typeProperties Hello část datové sady typ **MongoDbCollection** má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="9f2b3-173">hello typeProperties section for dataset of type **MongoDbCollection** has hello following properties:</span></span>

| <span data-ttu-id="9f2b3-174">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9f2b3-174">Property</span></span> | <span data-ttu-id="9f2b3-175">Popis</span><span class="sxs-lookup"><span data-stu-id="9f2b3-175">Description</span></span> | <span data-ttu-id="9f2b3-176">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9f2b3-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9f2b3-177">Název_kolekce</span><span class="sxs-lookup"><span data-stu-id="9f2b3-177">collectionName</span></span> |<span data-ttu-id="9f2b3-178">Název kolekce hello v databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-178">Name of hello collection in MongoDB database.</span></span> |<span data-ttu-id="9f2b3-179">Ano</span><span class="sxs-lookup"><span data-stu-id="9f2b3-179">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="9f2b3-180">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="9f2b3-180">Copy activity properties</span></span>
<span data-ttu-id="9f2b3-181">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-181">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="9f2b3-182">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-182">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="9f2b3-183">Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivit na hello se každý typ aktivity lišit podle druhé straně.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-183">Properties available in hello **typeProperties** section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="9f2b3-184">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-184">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="9f2b3-185">Pokud zdroj hello je typu **MongoDbSource** hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="9f2b3-185">When hello source is of type **MongoDbSource** hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="9f2b3-186">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9f2b3-186">Property</span></span> | <span data-ttu-id="9f2b3-187">Popis</span><span class="sxs-lookup"><span data-stu-id="9f2b3-187">Description</span></span> | <span data-ttu-id="9f2b3-188">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="9f2b3-188">Allowed values</span></span> | <span data-ttu-id="9f2b3-189">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9f2b3-189">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9f2b3-190">query</span><span class="sxs-lookup"><span data-stu-id="9f2b3-190">query</span></span> |<span data-ttu-id="9f2b3-191">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-191">Use hello custom query tooread data.</span></span> |<span data-ttu-id="9f2b3-192">Řetězec dotazu SQL 92.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-192">SQL-92 query string.</span></span> <span data-ttu-id="9f2b3-193">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-193">For example: select * from MyTable.</span></span> |<span data-ttu-id="9f2b3-194">Ne (Pokud **Název_kolekce** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="9f2b3-194">No (if **collectionName** of **dataset** is specified)</span></span> |



## <a name="json-example-copy-data-from-mongodb-tooazure-blob"></a><span data-ttu-id="9f2b3-195">Příklad JSON: kopírování dat z MongoDB tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="9f2b3-195">JSON example: Copy data from MongoDB tooAzure Blob</span></span>
<span data-ttu-id="9f2b3-196">Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-196">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="9f2b3-197">Ukazuje, jak toocopy data ze tooan MongoDB místní úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-197">It shows how toocopy data from an on-premises MongoDB tooan Azure Blob Storage.</span></span> <span data-ttu-id="9f2b3-198">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-198">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="9f2b3-199">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="9f2b3-199">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="9f2b3-200">Propojené služby typu [OnPremisesMongoDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-200">A linked service of type [OnPremisesMongoDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="9f2b3-201">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-201">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="9f2b3-202">Vstup [datovou sadu](data-factory-create-datasets.md) typu [MongoDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-202">An input [dataset](data-factory-create-datasets.md) of type [MongoDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="9f2b3-203">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-203">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="9f2b3-204">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [MongoDbSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-204">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [MongoDbSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="9f2b3-205">Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob tooa databázi MongoDB každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-205">hello sample copies data from a query result in MongoDB database tooa blob every hour.</span></span> <span data-ttu-id="9f2b3-206">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-206">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="9f2b3-207">Jako první krok, instalační program hello Brána pro správu dat podle pokynů hello v hello [Brána pro správu dat](data-factory-data-management-gateway.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-207">As a first step, setup hello data management gateway as per hello instructions in hello [Data Management Gateway](data-factory-data-management-gateway.md) article.</span></span>

<span data-ttu-id="9f2b3-208">**MongoDB propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="9f2b3-208">**MongoDB linked service:**</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",  
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
        }
    }
}
```

<span data-ttu-id="9f2b3-209">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="9f2b3-209">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="9f2b3-210">**Vstupní datové sady MongoDB:** nastavení "externí": "PRAVDA" informuje služba Data Factory hello Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-210">**MongoDB input dataset:** Setting “external”: ”true” informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
     "name":  "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"    
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

<span data-ttu-id="9f2b3-211">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="9f2b3-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="9f2b3-212">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-212">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="9f2b3-213">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-213">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="9f2b3-214">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-214">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="9f2b3-215">**Aktivita kopírování v kanálu s MongoDB zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="9f2b3-215">**Copy activity in a pipeline with MongoDB source and Blob sink:**</span></span>

<span data-ttu-id="9f2b3-216">Hello kanál obsahuje aktivitu kopírování, nakonfigurované toouse hello výše vstupní a výstupní datové sady, který je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-216">hello pipeline contains a Copy Activity that is configured toouse hello above input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="9f2b3-217">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**MongoDbSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-217">In hello pipeline JSON definition, hello **source** type is set too**MongoDbSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="9f2b3-218">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-218">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
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
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```


## <a name="schema-by-data-factory"></a><span data-ttu-id="9f2b3-219">Schéma službou Data Factory</span><span class="sxs-lookup"><span data-stu-id="9f2b3-219">Schema by Data Factory</span></span>
<span data-ttu-id="9f2b3-220">Služba Azure Data Factory odvodí schématu z kolekce MongoDB pomocí hello nejnovější 100 dokumenty v kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-220">Azure Data Factory service infers schema from a MongoDB collection by using hello latest 100 documents in hello collection.</span></span> <span data-ttu-id="9f2b3-221">Pokud tyto dokumenty 100 neobsahují úplné schéma, může být během operace kopírování hello ignorován některé sloupce.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-221">If these 100 documents do not contain full schema, some columns may be ignored during hello copy operation.</span></span>

## <a name="type-mapping-for-mongodb"></a><span data-ttu-id="9f2b3-222">Mapování typu pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="9f2b3-222">Type mapping for MongoDB</span></span>
<span data-ttu-id="9f2b3-223">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:</span><span class="sxs-lookup"><span data-stu-id="9f2b3-223">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="9f2b3-224">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="9f2b3-224">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="9f2b3-225">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9f2b3-225">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="9f2b3-226">Při přesunu dat tooMongoDB hello následující mapování se používají z typů too.NET typy MongoDB.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-226">When moving data tooMongoDB hello following mappings are used from MongoDB types too.NET types.</span></span>

| <span data-ttu-id="9f2b3-227">Typ MongoDB</span><span class="sxs-lookup"><span data-stu-id="9f2b3-227">MongoDB type</span></span> | <span data-ttu-id="9f2b3-228">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="9f2b3-228">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="9f2b3-229">Binární</span><span class="sxs-lookup"><span data-stu-id="9f2b3-229">Binary</span></span> |<span data-ttu-id="9f2b3-230">Byte]</span><span class="sxs-lookup"><span data-stu-id="9f2b3-230">Byte[]</span></span> |
| <span data-ttu-id="9f2b3-231">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9f2b3-231">Boolean</span></span> |<span data-ttu-id="9f2b3-232">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9f2b3-232">Boolean</span></span> |
| <span data-ttu-id="9f2b3-233">Datum</span><span class="sxs-lookup"><span data-stu-id="9f2b3-233">Date</span></span> |<span data-ttu-id="9f2b3-234">Data a času</span><span class="sxs-lookup"><span data-stu-id="9f2b3-234">DateTime</span></span> |
| <span data-ttu-id="9f2b3-235">NumberDouble</span><span class="sxs-lookup"><span data-stu-id="9f2b3-235">NumberDouble</span></span> |<span data-ttu-id="9f2b3-236">Double</span><span class="sxs-lookup"><span data-stu-id="9f2b3-236">Double</span></span> |
| <span data-ttu-id="9f2b3-237">NumberInt</span><span class="sxs-lookup"><span data-stu-id="9f2b3-237">NumberInt</span></span> |<span data-ttu-id="9f2b3-238">Int32</span><span class="sxs-lookup"><span data-stu-id="9f2b3-238">Int32</span></span> |
| <span data-ttu-id="9f2b3-239">NumberLong</span><span class="sxs-lookup"><span data-stu-id="9f2b3-239">NumberLong</span></span> |<span data-ttu-id="9f2b3-240">Int64</span><span class="sxs-lookup"><span data-stu-id="9f2b3-240">Int64</span></span> |
| <span data-ttu-id="9f2b3-241">ObjectID</span><span class="sxs-lookup"><span data-stu-id="9f2b3-241">ObjectID</span></span> |<span data-ttu-id="9f2b3-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9f2b3-242">String</span></span> |
| <span data-ttu-id="9f2b3-243">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9f2b3-243">String</span></span> |<span data-ttu-id="9f2b3-244">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9f2b3-244">String</span></span> |
| <span data-ttu-id="9f2b3-245">UUID</span><span class="sxs-lookup"><span data-stu-id="9f2b3-245">UUID</span></span> |<span data-ttu-id="9f2b3-246">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="9f2b3-246">Guid</span></span> |
| <span data-ttu-id="9f2b3-247">Objekt</span><span class="sxs-lookup"><span data-stu-id="9f2b3-247">Object</span></span> |<span data-ttu-id="9f2b3-248">Renormalized do vyrovnání sloupce s "_" jako vnořené oddělovače</span><span class="sxs-lookup"><span data-stu-id="9f2b3-248">Renormalized into flatten columns with “_” as nested separator</span></span> |

> [!NOTE]
> <span data-ttu-id="9f2b3-249">toolearn o podpoře pro polí pomocí virtuální tabulky odkazovat příliš[podporu pro komplexní typy pomocí virtuální tabulky](#support-for-complex-types-using-virtual-tables) části níže.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-249">toolearn about support for arrays using virtual tables, refer too[Support for complex types using virtual tables](#support-for-complex-types-using-virtual-tables) section below.</span></span>

<span data-ttu-id="9f2b3-250">V současné době nejsou podporovány následující typy dat MongoDB hello: DBPointer, JavaScript, Max za minutu klíče, regulární výraz, Symbol, časové razítko, Undefined</span><span class="sxs-lookup"><span data-stu-id="9f2b3-250">Currently, hello following MongoDB data types are not supported: DBPointer, JavaScript, Max/Min key, Regular Expression, Symbol, Timestamp, Undefined</span></span>

## <a name="support-for-complex-types-using-virtual-tables"></a><span data-ttu-id="9f2b3-251">Podpora pro komplexní typy pomocí virtuální tabulky</span><span class="sxs-lookup"><span data-stu-id="9f2b3-251">Support for complex types using virtual tables</span></span>
<span data-ttu-id="9f2b3-252">Azure Data Factory používá integrované ODBC ovladač tooconnect tooand kopírování dat z databáze MongoDB.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-252">Azure Data Factory uses a built-in ODBC driver tooconnect tooand copy data from your MongoDB database.</span></span> <span data-ttu-id="9f2b3-253">Pro komplexní typy jako pole nebo objekty s různými typy na dokumentech hello hello ovladač znovu sjednotí data na odpovídající virtuální tabulky.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-253">For complex types such as arrays or objects with different types across hello documents, hello driver re-normalizes data into corresponding virtual tables.</span></span> <span data-ttu-id="9f2b3-254">Konkrétně Pokud tabulka obsahuje tyto sloupce, hello ovladač generuje hello následující virtuální tabulky:</span><span class="sxs-lookup"><span data-stu-id="9f2b3-254">Specifically, if a table contains such columns, hello driver generates hello following virtual tables:</span></span>

* <span data-ttu-id="9f2b3-255">A **základní tabulka**, který obsahuje hello stejná data jako hello skutečné tabulky s výjimkou hello komplexní typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-255">A **base table**, which contains hello same data as hello real table except for hello complex type columns.</span></span> <span data-ttu-id="9f2b3-256">Základní tabulka Hello používá hello stejný název jako hello skutečné tabulku, která reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-256">hello base table uses hello same name as hello real table that it represents.</span></span>
* <span data-ttu-id="9f2b3-257">A **virtuální tabulku** pro každý sloupec komplexní typ, který rozbalí hello vnořená data.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-257">A **virtual table** for each complex type column, which expands hello nested data.</span></span> <span data-ttu-id="9f2b3-258">virtuální tabulky Hello jsou pojmenované pomocí hello název hello skutečné tabulky, oddělovače "_" a název hello hello pole nebo objekt.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-258">hello virtual tables are named using hello name of hello real table, a separator “_” and hello name of hello array or object.</span></span>

<span data-ttu-id="9f2b3-259">Virtuální tabulky odkazovat toohello data v tabulce skutečné hello, povolení tooaccess ovladač hello hello nenormalizované data.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-259">Virtual tables refer toohello data in hello real table, enabling hello driver tooaccess hello denormalized data.</span></span> <span data-ttu-id="9f2b3-260">Viz část o příklad níže podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-260">See Example section below details.</span></span> <span data-ttu-id="9f2b3-261">Obsah hello MongoDB polí můžete přejít pomocí dotazování a spojování tabulek virtuálním hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-261">You can access hello content of MongoDB arrays by querying and joining hello virtual tables.</span></span>

<span data-ttu-id="9f2b3-262">Můžete použít hello [Průvodce kopírováním](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively zobrazení hello seznam tabulek v databázi MongoDB, včetně hello virtuální tabulky a náhled obsažená data hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-262">You can use hello [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively view hello list of tables in MongoDB database including hello virtual tables, and preview hello data inside.</span></span> <span data-ttu-id="9f2b3-263">Můžete také vytvořit dotaz v hello Průvodce kopírováním a ověřit toosee hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-263">You can also construct a query in hello Copy Wizard and validate toosee hello result.</span></span>

### <a name="example"></a><span data-ttu-id="9f2b3-264">Příklad</span><span class="sxs-lookup"><span data-stu-id="9f2b3-264">Example</span></span>
<span data-ttu-id="9f2b3-265">Například "ExampleTable" pod je MongoDB tabulku, která obsahuje jeden sloupec s pole objektů v každé buňce – faktury a jeden sloupec s pole Skalární typy – hodnocení.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-265">For example, “ExampleTable” below is a MongoDB table that has one column with an array of Objects in each cell – Invoices, and one column with an array of Scalar types – Ratings.</span></span>

| <span data-ttu-id="9f2b3-266">_id</span><span class="sxs-lookup"><span data-stu-id="9f2b3-266">_id</span></span> | <span data-ttu-id="9f2b3-267">Jméno zákazníka</span><span class="sxs-lookup"><span data-stu-id="9f2b3-267">Customer Name</span></span> | <span data-ttu-id="9f2b3-268">Faktury</span><span class="sxs-lookup"><span data-stu-id="9f2b3-268">Invoices</span></span> | <span data-ttu-id="9f2b3-269">Úrovně služeb</span><span class="sxs-lookup"><span data-stu-id="9f2b3-269">Service Level</span></span> | <span data-ttu-id="9f2b3-270">Hodnocení</span><span class="sxs-lookup"><span data-stu-id="9f2b3-270">Ratings</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="9f2b3-271">1111</span><span class="sxs-lookup"><span data-stu-id="9f2b3-271">1111</span></span> |<span data-ttu-id="9f2b3-272">ABC</span><span class="sxs-lookup"><span data-stu-id="9f2b3-272">ABC</span></span> |<span data-ttu-id="9f2b3-273">[{invoice_id: "123", položka: "Toaster byl", cena: "456", slevu: "0,2"}, {invoice_id: "124", položka: "sušárny", ceny: slevách "1235": "0,2"}]</span><span class="sxs-lookup"><span data-stu-id="9f2b3-273">[{invoice_id:”123”, item:”toaster”, price:”456”, discount:”0.2”}, {invoice_id:”124”, item:”oven”, price: ”1235”, discount: ”0.2”}]</span></span> |<span data-ttu-id="9f2b3-274">Stříbrná</span><span class="sxs-lookup"><span data-stu-id="9f2b3-274">Silver</span></span> |<span data-ttu-id="9f2b3-275">[5,6]</span><span class="sxs-lookup"><span data-stu-id="9f2b3-275">[5,6]</span></span> |
| <span data-ttu-id="9f2b3-276">2222</span><span class="sxs-lookup"><span data-stu-id="9f2b3-276">2222</span></span> |<span data-ttu-id="9f2b3-277">XYZ</span><span class="sxs-lookup"><span data-stu-id="9f2b3-277">XYZ</span></span> |<span data-ttu-id="9f2b3-278">[{invoice_id: "135", položka: "ledničky", cena: "12543", slevu: "0,0"}]</span><span class="sxs-lookup"><span data-stu-id="9f2b3-278">[{invoice_id:”135”, item:”fridge”, price: ”12543”, discount: ”0.0”}]</span></span> |<span data-ttu-id="9f2b3-279">Zlatý</span><span class="sxs-lookup"><span data-stu-id="9f2b3-279">Gold</span></span> |<span data-ttu-id="9f2b3-280">[1,2]</span><span class="sxs-lookup"><span data-stu-id="9f2b3-280">[1,2]</span></span> |

<span data-ttu-id="9f2b3-281">ovladač Hello by vygeneroval více toorepresent virtuální tabulky tento jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-281">hello driver would generate multiple virtual tables toorepresent this single table.</span></span> <span data-ttu-id="9f2b3-282">první virtuální tabulky Hello je hello základní tabulka s názvem "ExampleTable", viz následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-282">hello first virtual table is hello base table named “ExampleTable”, shown below.</span></span> <span data-ttu-id="9f2b3-283">Hello základní tabulka obsahuje všechna data hello hello původní tabulky, ale hello data z pole hello byla vynechána a je rozšířit hello virtuální tabulky.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-283">hello base table contains all hello data of hello original table, but hello data from hello arrays has been omitted and is expanded in hello virtual tables.</span></span>

| <span data-ttu-id="9f2b3-284">_id</span><span class="sxs-lookup"><span data-stu-id="9f2b3-284">_id</span></span> | <span data-ttu-id="9f2b3-285">Jméno zákazníka</span><span class="sxs-lookup"><span data-stu-id="9f2b3-285">Customer Name</span></span> | <span data-ttu-id="9f2b3-286">Úrovně služeb</span><span class="sxs-lookup"><span data-stu-id="9f2b3-286">Service Level</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9f2b3-287">1111</span><span class="sxs-lookup"><span data-stu-id="9f2b3-287">1111</span></span> |<span data-ttu-id="9f2b3-288">ABC</span><span class="sxs-lookup"><span data-stu-id="9f2b3-288">ABC</span></span> |<span data-ttu-id="9f2b3-289">Stříbrná</span><span class="sxs-lookup"><span data-stu-id="9f2b3-289">Silver</span></span> |
| <span data-ttu-id="9f2b3-290">2222</span><span class="sxs-lookup"><span data-stu-id="9f2b3-290">2222</span></span> |<span data-ttu-id="9f2b3-291">XYZ</span><span class="sxs-lookup"><span data-stu-id="9f2b3-291">XYZ</span></span> |<span data-ttu-id="9f2b3-292">Zlatý</span><span class="sxs-lookup"><span data-stu-id="9f2b3-292">Gold</span></span> |

<span data-ttu-id="9f2b3-293">Hello následující tabulky popisují hello virtuální tabulky, které představují hello původní pole v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-293">hello following tables show hello virtual tables that represent hello original arrays in hello example.</span></span> <span data-ttu-id="9f2b3-294">Tyto tabulky obsahují hello následující:</span><span class="sxs-lookup"><span data-stu-id="9f2b3-294">These tables contain hello following:</span></span>

* <span data-ttu-id="9f2b3-295">Odkaz zpět toohello původní sloupec primárního klíče odpovídající toohello řádek hello původní pole (přes sloupec _id hello)</span><span class="sxs-lookup"><span data-stu-id="9f2b3-295">A reference back toohello original primary key column corresponding toohello row of hello original array (via hello _id column)</span></span>
* <span data-ttu-id="9f2b3-296">Označení pozice hello hello dat v rámci pole původní hello</span><span class="sxs-lookup"><span data-stu-id="9f2b3-296">An indication of hello position of hello data within hello original array</span></span>
* <span data-ttu-id="9f2b3-297">Hello rozšířit dat pro každý prvek v rámci pole hello</span><span class="sxs-lookup"><span data-stu-id="9f2b3-297">hello expanded data for each element within hello array</span></span>

<span data-ttu-id="9f2b3-298">Tabulka "ExampleTable_Invoices":</span><span class="sxs-lookup"><span data-stu-id="9f2b3-298">Table “ExampleTable_Invoices”:</span></span>

| <span data-ttu-id="9f2b3-299">_id</span><span class="sxs-lookup"><span data-stu-id="9f2b3-299">_id</span></span> | <span data-ttu-id="9f2b3-300">ExampleTable_Invoices_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="9f2b3-300">ExampleTable_Invoices_dim1_idx</span></span> | <span data-ttu-id="9f2b3-301">invoice_id</span><span class="sxs-lookup"><span data-stu-id="9f2b3-301">invoice_id</span></span> | <span data-ttu-id="9f2b3-302">Položka</span><span class="sxs-lookup"><span data-stu-id="9f2b3-302">item</span></span> | <span data-ttu-id="9f2b3-303">price</span><span class="sxs-lookup"><span data-stu-id="9f2b3-303">price</span></span> | <span data-ttu-id="9f2b3-304">Sleva</span><span class="sxs-lookup"><span data-stu-id="9f2b3-304">Discount</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="9f2b3-305">1111</span><span class="sxs-lookup"><span data-stu-id="9f2b3-305">1111</span></span> |<span data-ttu-id="9f2b3-306">0</span><span class="sxs-lookup"><span data-stu-id="9f2b3-306">0</span></span> |<span data-ttu-id="9f2b3-307">123</span><span class="sxs-lookup"><span data-stu-id="9f2b3-307">123</span></span> |<span data-ttu-id="9f2b3-308">Toaster byl</span><span class="sxs-lookup"><span data-stu-id="9f2b3-308">toaster</span></span> |<span data-ttu-id="9f2b3-309">456</span><span class="sxs-lookup"><span data-stu-id="9f2b3-309">456</span></span> |<span data-ttu-id="9f2b3-310">0.2</span><span class="sxs-lookup"><span data-stu-id="9f2b3-310">0.2</span></span> |
| <span data-ttu-id="9f2b3-311">1111</span><span class="sxs-lookup"><span data-stu-id="9f2b3-311">1111</span></span> |<span data-ttu-id="9f2b3-312">1</span><span class="sxs-lookup"><span data-stu-id="9f2b3-312">1</span></span> |<span data-ttu-id="9f2b3-313">124</span><span class="sxs-lookup"><span data-stu-id="9f2b3-313">124</span></span> |<span data-ttu-id="9f2b3-314">sušárny</span><span class="sxs-lookup"><span data-stu-id="9f2b3-314">oven</span></span> |<span data-ttu-id="9f2b3-315">1235</span><span class="sxs-lookup"><span data-stu-id="9f2b3-315">1235</span></span> |<span data-ttu-id="9f2b3-316">0.2</span><span class="sxs-lookup"><span data-stu-id="9f2b3-316">0.2</span></span> |
| <span data-ttu-id="9f2b3-317">2222</span><span class="sxs-lookup"><span data-stu-id="9f2b3-317">2222</span></span> |<span data-ttu-id="9f2b3-318">0</span><span class="sxs-lookup"><span data-stu-id="9f2b3-318">0</span></span> |<span data-ttu-id="9f2b3-319">135</span><span class="sxs-lookup"><span data-stu-id="9f2b3-319">135</span></span> |<span data-ttu-id="9f2b3-320">ledničky</span><span class="sxs-lookup"><span data-stu-id="9f2b3-320">fridge</span></span> |<span data-ttu-id="9f2b3-321">12543</span><span class="sxs-lookup"><span data-stu-id="9f2b3-321">12543</span></span> |<span data-ttu-id="9f2b3-322">0.0</span><span class="sxs-lookup"><span data-stu-id="9f2b3-322">0.0</span></span> |

<span data-ttu-id="9f2b3-323">Tabulka "ExampleTable_Ratings":</span><span class="sxs-lookup"><span data-stu-id="9f2b3-323">Table “ExampleTable_Ratings”:</span></span>

| <span data-ttu-id="9f2b3-324">_id</span><span class="sxs-lookup"><span data-stu-id="9f2b3-324">_id</span></span> | <span data-ttu-id="9f2b3-325">ExampleTable_Ratings_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="9f2b3-325">ExampleTable_Ratings_dim1_idx</span></span> | <span data-ttu-id="9f2b3-326">ExampleTable_Ratings</span><span class="sxs-lookup"><span data-stu-id="9f2b3-326">ExampleTable_Ratings</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9f2b3-327">1111</span><span class="sxs-lookup"><span data-stu-id="9f2b3-327">1111</span></span> |<span data-ttu-id="9f2b3-328">0</span><span class="sxs-lookup"><span data-stu-id="9f2b3-328">0</span></span> |<span data-ttu-id="9f2b3-329">5</span><span class="sxs-lookup"><span data-stu-id="9f2b3-329">5</span></span> |
| <span data-ttu-id="9f2b3-330">1111</span><span class="sxs-lookup"><span data-stu-id="9f2b3-330">1111</span></span> |<span data-ttu-id="9f2b3-331">1</span><span class="sxs-lookup"><span data-stu-id="9f2b3-331">1</span></span> |<span data-ttu-id="9f2b3-332">6</span><span class="sxs-lookup"><span data-stu-id="9f2b3-332">6</span></span> |
| <span data-ttu-id="9f2b3-333">2222</span><span class="sxs-lookup"><span data-stu-id="9f2b3-333">2222</span></span> |<span data-ttu-id="9f2b3-334">0</span><span class="sxs-lookup"><span data-stu-id="9f2b3-334">0</span></span> |<span data-ttu-id="9f2b3-335">1</span><span class="sxs-lookup"><span data-stu-id="9f2b3-335">1</span></span> |
| <span data-ttu-id="9f2b3-336">2222</span><span class="sxs-lookup"><span data-stu-id="9f2b3-336">2222</span></span> |<span data-ttu-id="9f2b3-337">1</span><span class="sxs-lookup"><span data-stu-id="9f2b3-337">1</span></span> |<span data-ttu-id="9f2b3-338">2</span><span class="sxs-lookup"><span data-stu-id="9f2b3-338">2</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="9f2b3-339">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="9f2b3-339">Map source toosink columns</span></span>
<span data-ttu-id="9f2b3-340">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-340">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="9f2b3-341">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="9f2b3-341">Repeatable read from relational sources</span></span>
<span data-ttu-id="9f2b3-342">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-342">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="9f2b3-343">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-343">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="9f2b3-344">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-344">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="9f2b3-345">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-345">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="9f2b3-346">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="9f2b3-346">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="9f2b3-347">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="9f2b3-347">Performance and Tuning</span></span>
<span data-ttu-id="9f2b3-348">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-348">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f2b3-349">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f2b3-349">Next Steps</span></span>
<span data-ttu-id="9f2b3-350">V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) článek podrobné pokyny pro vytváření dat kanál, který přesouvá data z místních dat úložiště Azure data tooan.</span><span class="sxs-lookup"><span data-stu-id="9f2b3-350">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions for creating a data pipeline that moves data from an on-premises data store tooan Azure data store.</span></span>
