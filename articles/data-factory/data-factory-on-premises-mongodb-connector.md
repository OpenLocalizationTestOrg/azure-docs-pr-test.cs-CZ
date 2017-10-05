---
title: "Přesun dat z MongoDB pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z databáze MongoDB pomocí Azure Data Factory."
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
ms.openlocfilehash: ac4ff55c765a5b874b81714c3d0063a5b4765a05
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a><span data-ttu-id="8f0a2-103">Přesun dat z MongoDB pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="8f0a2-103">Move data From MongoDB using Azure Data Factory</span></span>
<span data-ttu-id="8f0a2-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z místní databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises MongoDB database.</span></span> <span data-ttu-id="8f0a2-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="8f0a2-106">Do úložiště dat žádné podporované podřízený může kopírovat data z místního úložiště dat MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-106">You can copy data from an on-premises MongoDB data store to any supported sink data store.</span></span> <span data-ttu-id="8f0a2-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="8f0a2-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z jiného úložiště dat MongoDB k jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat do úložiště dat MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-108">Data factory currently supports only moving data from a MongoDB data store to other data stores, but not for moving data from other data stores to an MongoDB datastore.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="8f0a2-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8f0a2-109">Prerequisites</span></span>
<span data-ttu-id="8f0a2-110">Pro službu Azure Data Factory být schopni připojit k vaší databázi MongoDB místní musíte nainstalovat následující součásti:</span><span class="sxs-lookup"><span data-stu-id="8f0a2-110">For the Azure Data Factory service to be able to connect to your on-premises MongoDB database, you must install the following components:</span></span>

- <span data-ttu-id="8f0a2-111">Podporované verze MongoDB jsou: 2.4, 2.6, 3.0 a 3.2.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-111">Supported MongoDB versions are:  2.4, 2.6, 3.0, and 3.2.</span></span>
- <span data-ttu-id="8f0a2-112">Brána pro správu dat na stejném počítači, který je hostitelem databáze nebo na samostatném počítači, aby se zabránilo neslučitelných pro prostředky s databází.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-112">Data Management Gateway on the same machine that hosts the database or on a separate machine to avoid competing for resources with the database.</span></span> <span data-ttu-id="8f0a2-113">Brána pro správu dat je software, který se připojuje místní zdroje dat do cloudové služby v zabezpečený a spravovaný.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-113">Data Management Gateway is a software that connects on-premises data sources to cloud services in a secure and managed way.</span></span> <span data-ttu-id="8f0a2-114">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o Brána pro správu dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="8f0a2-115">V tématu [přesun dat z lokálního prostředí do cloudu](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny o nastavení brány datovém kanálu pro přesun dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-115">See [Move data from on-premises to cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway a data pipeline to move data.</span></span>

    <span data-ttu-id="8f0a2-116">Při instalaci brány se automaticky nainstaluje ovladač Microsoft MongoDB ODBC používaný pro připojení k MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-116">When you install the gateway, it automatically installs a Microsoft MongoDB ODBC driver used to connect to MongoDB.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f0a2-117">Musíte použít bránu pro připojení k MongoDB, i když je hostován v virtuální počítače Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-117">You need to use the gateway to connect to MongoDB even if it is hosted in Azure IaaS VMs.</span></span> <span data-ttu-id="8f0a2-118">Pokud se pokoušíte připojit k instanci MongoDB hostované v cloudu, můžete také nainstalovat instanci brány ve virtuálním počítači IaaS.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-118">If you are trying to connect to an instance of MongoDB hosted in cloud, you can also install the gateway instance in the IaaS VM.</span></span>

## <a name="getting-started"></a><span data-ttu-id="8f0a2-119">Začínáme</span><span class="sxs-lookup"><span data-stu-id="8f0a2-119">Getting started</span></span>
<span data-ttu-id="8f0a2-120">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní MongoDB data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-120">You can create a pipeline with a copy activity that moves data from an on-premises MongoDB data store by using different tools/APIs.</span></span>

<span data-ttu-id="8f0a2-121">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-121">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="8f0a2-122">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="8f0a2-123">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-123">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="8f0a2-124">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="8f0a2-125">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="8f0a2-125">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="8f0a2-126">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-126">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="8f0a2-127">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-127">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="8f0a2-128">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="8f0a2-129">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-129">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="8f0a2-130">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="8f0a2-131">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat místní MongoDB, naleznete v tématu [JSON příklad: kopírování dat z MongoDB do objektu Blob Azure](#json-example-copy-data-from-mongodb-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-131">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises MongoDB data store, see [JSON example: Copy data from MongoDB to Azure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="8f0a2-132">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení entit služby Data Factory konkrétní zdroj MongoDB:</span><span class="sxs-lookup"><span data-stu-id="8f0a2-132">The following sections provide details about JSON properties that are used to define Data Factory entities specific to MongoDB source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="8f0a2-133">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="8f0a2-133">Linked service properties</span></span>
<span data-ttu-id="8f0a2-134">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro **OnPremisesMongoDB** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-134">The following table provides description for JSON elements specific to **OnPremisesMongoDB** linked service.</span></span>

| <span data-ttu-id="8f0a2-135">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8f0a2-135">Property</span></span> | <span data-ttu-id="8f0a2-136">Popis</span><span class="sxs-lookup"><span data-stu-id="8f0a2-136">Description</span></span> | <span data-ttu-id="8f0a2-137">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8f0a2-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f0a2-138">type</span><span class="sxs-lookup"><span data-stu-id="8f0a2-138">type</span></span> |<span data-ttu-id="8f0a2-139">Vlastnost typu musí být nastavena na: **OnPremisesMongoDb**</span><span class="sxs-lookup"><span data-stu-id="8f0a2-139">The type property must be set to: **OnPremisesMongoDb**</span></span> |<span data-ttu-id="8f0a2-140">Ano</span><span class="sxs-lookup"><span data-stu-id="8f0a2-140">Yes</span></span> |
| <span data-ttu-id="8f0a2-141">server</span><span class="sxs-lookup"><span data-stu-id="8f0a2-141">server</span></span> |<span data-ttu-id="8f0a2-142">IP adresa nebo název hostitele serveru MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-142">IP address or host name of the MongoDB server.</span></span> |<span data-ttu-id="8f0a2-143">Ano</span><span class="sxs-lookup"><span data-stu-id="8f0a2-143">Yes</span></span> |
| <span data-ttu-id="8f0a2-144">port</span><span class="sxs-lookup"><span data-stu-id="8f0a2-144">port</span></span> |<span data-ttu-id="8f0a2-145">Port TCP, který používá MongoDB server naslouchat pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-145">TCP port that the MongoDB server uses to listen for client connections.</span></span> |<span data-ttu-id="8f0a2-146">Volitelné, výchozí hodnota: 27017</span><span class="sxs-lookup"><span data-stu-id="8f0a2-146">Optional, default value: 27017</span></span> |
| <span data-ttu-id="8f0a2-147">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-147">authenticationType</span></span> |<span data-ttu-id="8f0a2-148">Základní, nebo anonymní.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-148">Basic, or Anonymous.</span></span> |<span data-ttu-id="8f0a2-149">Ano</span><span class="sxs-lookup"><span data-stu-id="8f0a2-149">Yes</span></span> |
| <span data-ttu-id="8f0a2-150">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="8f0a2-150">username</span></span> |<span data-ttu-id="8f0a2-151">Uživatelský účet pro přístup k MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-151">User account to access MongoDB.</span></span> |<span data-ttu-id="8f0a2-152">Ano (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-152">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="8f0a2-153">heslo</span><span class="sxs-lookup"><span data-stu-id="8f0a2-153">password</span></span> |<span data-ttu-id="8f0a2-154">Heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-154">Password for the user.</span></span> |<span data-ttu-id="8f0a2-155">Ano (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-155">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="8f0a2-156">authSource</span><span class="sxs-lookup"><span data-stu-id="8f0a2-156">authSource</span></span> |<span data-ttu-id="8f0a2-157">Název databáze MongoDB, kterou chcete použít ke kontrole přihlašovacích údajů pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-157">Name of the MongoDB database that you want to use to check your credentials for authentication.</span></span> |<span data-ttu-id="8f0a2-158">Volitelný parametr (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-158">Optional (if basic authentication is used).</span></span> <span data-ttu-id="8f0a2-159">Výchozí: používá účet správce a do databáze určené pomocí vlastnost databaseName.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-159">default: uses the admin account and the database specified using databaseName property.</span></span> |
| <span data-ttu-id="8f0a2-160">Název databáze</span><span class="sxs-lookup"><span data-stu-id="8f0a2-160">databaseName</span></span> |<span data-ttu-id="8f0a2-161">Název databáze MongoDB, kterou chcete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-161">Name of the MongoDB database that you want to access.</span></span> |<span data-ttu-id="8f0a2-162">Ano</span><span class="sxs-lookup"><span data-stu-id="8f0a2-162">Yes</span></span> |
| <span data-ttu-id="8f0a2-163">gatewayName</span><span class="sxs-lookup"><span data-stu-id="8f0a2-163">gatewayName</span></span> |<span data-ttu-id="8f0a2-164">Název brány, který přistupuje k úložišti.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-164">Name of the gateway that accesses the data store.</span></span> |<span data-ttu-id="8f0a2-165">Ano</span><span class="sxs-lookup"><span data-stu-id="8f0a2-165">Yes</span></span> |
| <span data-ttu-id="8f0a2-166">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="8f0a2-166">encryptedCredential</span></span> |<span data-ttu-id="8f0a2-167">Přihlašovací údaje zašifrovaná pomocí brány.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-167">Credential encrypted by gateway.</span></span> |<span data-ttu-id="8f0a2-168">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="8f0a2-168">Optional</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="8f0a2-169">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="8f0a2-169">Dataset properties</span></span>
<span data-ttu-id="8f0a2-170">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-170">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="8f0a2-171">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="8f0a2-172">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-172">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="8f0a2-173">Rámci typeProperties část datové sady typ **MongoDbCollection** má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8f0a2-173">The typeProperties section for dataset of type **MongoDbCollection** has the following properties:</span></span>

| <span data-ttu-id="8f0a2-174">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8f0a2-174">Property</span></span> | <span data-ttu-id="8f0a2-175">Popis</span><span class="sxs-lookup"><span data-stu-id="8f0a2-175">Description</span></span> | <span data-ttu-id="8f0a2-176">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8f0a2-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f0a2-177">Název_kolekce</span><span class="sxs-lookup"><span data-stu-id="8f0a2-177">collectionName</span></span> |<span data-ttu-id="8f0a2-178">Název kolekce v databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-178">Name of the collection in MongoDB database.</span></span> |<span data-ttu-id="8f0a2-179">Ano</span><span class="sxs-lookup"><span data-stu-id="8f0a2-179">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="8f0a2-180">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="8f0a2-180">Copy activity properties</span></span>
<span data-ttu-id="8f0a2-181">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-181">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="8f0a2-182">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-182">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="8f0a2-183">Vlastnosti, které jsou k dispozici v **rámci typeProperties** části aktivity na druhé straně lišit každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-183">Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="8f0a2-184">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-184">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="8f0a2-185">Pokud je zdroj typu **MongoDbSource** následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="8f0a2-185">When the source is of type **MongoDbSource** the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="8f0a2-186">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8f0a2-186">Property</span></span> | <span data-ttu-id="8f0a2-187">Popis</span><span class="sxs-lookup"><span data-stu-id="8f0a2-187">Description</span></span> | <span data-ttu-id="8f0a2-188">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="8f0a2-188">Allowed values</span></span> | <span data-ttu-id="8f0a2-189">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8f0a2-189">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8f0a2-190">query</span><span class="sxs-lookup"><span data-stu-id="8f0a2-190">query</span></span> |<span data-ttu-id="8f0a2-191">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-191">Use the custom query to read data.</span></span> |<span data-ttu-id="8f0a2-192">Řetězec dotazu SQL 92.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-192">SQL-92 query string.</span></span> <span data-ttu-id="8f0a2-193">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-193">For example: select * from MyTable.</span></span> |<span data-ttu-id="8f0a2-194">Ne (Pokud **Název_kolekce** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="8f0a2-194">No (if **collectionName** of **dataset** is specified)</span></span> |



## <a name="json-example-copy-data-from-mongodb-to-azure-blob"></a><span data-ttu-id="8f0a2-195">Příklad JSON: kopírování dat z MongoDB do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="8f0a2-195">JSON example: Copy data from MongoDB to Azure Blob</span></span>
<span data-ttu-id="8f0a2-196">Tento příklad obsahuje ukázkové JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-196">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="8f0a2-197">Ukazuje, jak zkopírovat data z místní MongoDB do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-197">It shows how to copy data from an on-premises MongoDB to an Azure Blob Storage.</span></span> <span data-ttu-id="8f0a2-198">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-198">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="8f0a2-199">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="8f0a2-199">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="8f0a2-200">Propojené služby typu [OnPremisesMongoDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-200">A linked service of type [OnPremisesMongoDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="8f0a2-201">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-201">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="8f0a2-202">Vstup [datovou sadu](data-factory-create-datasets.md) typu [MongoDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-202">An input [dataset](data-factory-create-datasets.md) of type [MongoDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="8f0a2-203">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-203">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="8f0a2-204">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [MongoDbSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-204">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [MongoDbSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="8f0a2-205">Ukázka zkopíruje data z výsledku dotazu v databázi MongoDB do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-205">The sample copies data from a query result in MongoDB database to a blob every hour.</span></span> <span data-ttu-id="8f0a2-206">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-206">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="8f0a2-207">Jako první krok, nastavit Brána pro správu dat podle pokynů v [Brána pro správu dat](data-factory-data-management-gateway.md) článku.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-207">As a first step, setup the data management gateway as per the instructions in the [Data Management Gateway](data-factory-data-management-gateway.md) article.</span></span>

<span data-ttu-id="8f0a2-208">**MongoDB propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="8f0a2-208">**MongoDB linked service:**</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",  
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
        }
    }
}
```

<span data-ttu-id="8f0a2-209">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="8f0a2-209">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="8f0a2-210">**Vstupní datové sady MongoDB:** nastavení "externí": "PRAVDA" informuje služba Data Factory, v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-210">**MongoDB input dataset:** Setting “external”: ”true” informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="8f0a2-211">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="8f0a2-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="8f0a2-212">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-212">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="8f0a2-213">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-213">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="8f0a2-214">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-214">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="8f0a2-215">**Aktivita kopírování v kanálu s MongoDB zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="8f0a2-215">**Copy activity in a pipeline with MongoDB source and Blob sink:**</span></span>

<span data-ttu-id="8f0a2-216">Kanál obsahuje aktivitu kopírování, která je nakonfigurována pro používání výše vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-216">The pipeline contains a Copy Activity that is configured to use the above input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="8f0a2-217">V definici JSON kanálu **zdroj** je typ nastaven na **MongoDbSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-217">In the pipeline JSON definition, the **source** type is set to **MongoDbSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="8f0a2-218">Zadané pro dotaz SQL **dotazu** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-218">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

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


## <a name="schema-by-data-factory"></a><span data-ttu-id="8f0a2-219">Schéma službou Data Factory</span><span class="sxs-lookup"><span data-stu-id="8f0a2-219">Schema by Data Factory</span></span>
<span data-ttu-id="8f0a2-220">Služba Azure Data Factory odvodí schématu z kolekce MongoDB pomocí nejnovější 100 dokumenty v kolekci.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-220">Azure Data Factory service infers schema from a MongoDB collection by using the latest 100 documents in the collection.</span></span> <span data-ttu-id="8f0a2-221">Pokud tyto dokumenty 100 neobsahují úplné schéma, může být některé sloupce ignorován při kopírování.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-221">If these 100 documents do not contain full schema, some columns may be ignored during the copy operation.</span></span>

## <a name="type-mapping-for-mongodb"></a><span data-ttu-id="8f0a2-222">Mapování typu pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="8f0a2-222">Type mapping for MongoDB</span></span>
<span data-ttu-id="8f0a2-223">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup krok 2:</span><span class="sxs-lookup"><span data-stu-id="8f0a2-223">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="8f0a2-224">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="8f0a2-224">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="8f0a2-225">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="8f0a2-225">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="8f0a2-226">Při přesunu dat na MongoDB se používají následující mapování z typů MongoDB na typy .NET.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-226">When moving data to MongoDB the following mappings are used from MongoDB types to .NET types.</span></span>

| <span data-ttu-id="8f0a2-227">Typ MongoDB</span><span class="sxs-lookup"><span data-stu-id="8f0a2-227">MongoDB type</span></span> | <span data-ttu-id="8f0a2-228">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="8f0a2-228">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="8f0a2-229">Binární</span><span class="sxs-lookup"><span data-stu-id="8f0a2-229">Binary</span></span> |<span data-ttu-id="8f0a2-230">Byte]</span><span class="sxs-lookup"><span data-stu-id="8f0a2-230">Byte[]</span></span> |
| <span data-ttu-id="8f0a2-231">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="8f0a2-231">Boolean</span></span> |<span data-ttu-id="8f0a2-232">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="8f0a2-232">Boolean</span></span> |
| <span data-ttu-id="8f0a2-233">Datum</span><span class="sxs-lookup"><span data-stu-id="8f0a2-233">Date</span></span> |<span data-ttu-id="8f0a2-234">Data a času</span><span class="sxs-lookup"><span data-stu-id="8f0a2-234">DateTime</span></span> |
| <span data-ttu-id="8f0a2-235">NumberDouble</span><span class="sxs-lookup"><span data-stu-id="8f0a2-235">NumberDouble</span></span> |<span data-ttu-id="8f0a2-236">Double</span><span class="sxs-lookup"><span data-stu-id="8f0a2-236">Double</span></span> |
| <span data-ttu-id="8f0a2-237">NumberInt</span><span class="sxs-lookup"><span data-stu-id="8f0a2-237">NumberInt</span></span> |<span data-ttu-id="8f0a2-238">Int32</span><span class="sxs-lookup"><span data-stu-id="8f0a2-238">Int32</span></span> |
| <span data-ttu-id="8f0a2-239">NumberLong</span><span class="sxs-lookup"><span data-stu-id="8f0a2-239">NumberLong</span></span> |<span data-ttu-id="8f0a2-240">Int64</span><span class="sxs-lookup"><span data-stu-id="8f0a2-240">Int64</span></span> |
| <span data-ttu-id="8f0a2-241">ObjectID</span><span class="sxs-lookup"><span data-stu-id="8f0a2-241">ObjectID</span></span> |<span data-ttu-id="8f0a2-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="8f0a2-242">String</span></span> |
| <span data-ttu-id="8f0a2-243">Řetězec</span><span class="sxs-lookup"><span data-stu-id="8f0a2-243">String</span></span> |<span data-ttu-id="8f0a2-244">Řetězec</span><span class="sxs-lookup"><span data-stu-id="8f0a2-244">String</span></span> |
| <span data-ttu-id="8f0a2-245">UUID</span><span class="sxs-lookup"><span data-stu-id="8f0a2-245">UUID</span></span> |<span data-ttu-id="8f0a2-246">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="8f0a2-246">Guid</span></span> |
| <span data-ttu-id="8f0a2-247">Objekt</span><span class="sxs-lookup"><span data-stu-id="8f0a2-247">Object</span></span> |<span data-ttu-id="8f0a2-248">Renormalized do vyrovnání sloupce s "_" jako vnořené oddělovače</span><span class="sxs-lookup"><span data-stu-id="8f0a2-248">Renormalized into flatten columns with “_” as nested separator</span></span> |

> [!NOTE]
> <span data-ttu-id="8f0a2-249">Další informace o podpoře pro polí pomocí virtuální tabulky, najdete v tématu [podporu pro komplexní typy pomocí virtuální tabulky](#support-for-complex-types-using-virtual-tables) části níže.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-249">To learn about support for arrays using virtual tables, refer to [Support for complex types using virtual tables](#support-for-complex-types-using-virtual-tables) section below.</span></span>

<span data-ttu-id="8f0a2-250">V současné době nejsou podporovány následující typy dat MongoDB: DBPointer, JavaScript, Max za minutu klíče, regulární výraz, Symbol, časové razítko, Undefined</span><span class="sxs-lookup"><span data-stu-id="8f0a2-250">Currently, the following MongoDB data types are not supported: DBPointer, JavaScript, Max/Min key, Regular Expression, Symbol, Timestamp, Undefined</span></span>

## <a name="support-for-complex-types-using-virtual-tables"></a><span data-ttu-id="8f0a2-251">Podpora pro komplexní typy pomocí virtuální tabulky</span><span class="sxs-lookup"><span data-stu-id="8f0a2-251">Support for complex types using virtual tables</span></span>
<span data-ttu-id="8f0a2-252">Azure Data Factory používá integrované ovladače ODBC pro připojení k a kopírování dat z databáze MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-252">Azure Data Factory uses a built-in ODBC driver to connect to and copy data from your MongoDB database.</span></span> <span data-ttu-id="8f0a2-253">Pro komplexní typy jako pole nebo objekty s různé typy mezi dokumenty ovladač znovu sjednotí data na odpovídající virtuální tabulky.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-253">For complex types such as arrays or objects with different types across the documents, the driver re-normalizes data into corresponding virtual tables.</span></span> <span data-ttu-id="8f0a2-254">Konkrétně Pokud tabulka obsahuje tyto sloupce, ovladač generuje následující virtuální tabulky:</span><span class="sxs-lookup"><span data-stu-id="8f0a2-254">Specifically, if a table contains such columns, the driver generates the following virtual tables:</span></span>

* <span data-ttu-id="8f0a2-255">A **základní tabulka**, která obsahuje stejná data jako skutečné tabulky s výjimkou komplexní typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-255">A **base table**, which contains the same data as the real table except for the complex type columns.</span></span> <span data-ttu-id="8f0a2-256">Základní tabulka používá stejný název jako skutečné tabulky, která reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-256">The base table uses the same name as the real table that it represents.</span></span>
* <span data-ttu-id="8f0a2-257">A **virtuální tabulku** pro každý sloupec komplexní typ, který rozbalí vnořená data.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-257">A **virtual table** for each complex type column, which expands the nested data.</span></span> <span data-ttu-id="8f0a2-258">Virtuální tabulky jsou pojmenované pomocí názvu skutečné tabulky, oddělovač "_" a název pole nebo objekt.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-258">The virtual tables are named using the name of the real table, a separator “_” and the name of the array or object.</span></span>

<span data-ttu-id="8f0a2-259">Virtuální tabulky odkazovat na data v tabulce skutečné povolení ovladače pro přístup k datům nenormalizované.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-259">Virtual tables refer to the data in the real table, enabling the driver to access the denormalized data.</span></span> <span data-ttu-id="8f0a2-260">Viz část o příklad níže podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-260">See Example section below details.</span></span> <span data-ttu-id="8f0a2-261">Dotazování a připojení virtuální tabulky, můžete přístup k obsahu polí MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-261">You can access the content of MongoDB arrays by querying and joining the virtual tables.</span></span>

<span data-ttu-id="8f0a2-262">Můžete použít [Průvodce kopírováním](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) intuitivně zobrazit seznam tabulek v databázi MongoDB, včetně virtuální tabulky a zobrazte náhled dat, uvnitř.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-262">You can use the [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) to intuitively view the list of tables in MongoDB database including the virtual tables, and preview the data inside.</span></span> <span data-ttu-id="8f0a2-263">Můžete také vytvořit dotaz v Průvodci kopírováním a ověření zobrazíte výsledek.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-263">You can also construct a query in the Copy Wizard and validate to see the result.</span></span>

### <a name="example"></a><span data-ttu-id="8f0a2-264">Příklad</span><span class="sxs-lookup"><span data-stu-id="8f0a2-264">Example</span></span>
<span data-ttu-id="8f0a2-265">Například "ExampleTable" pod je MongoDB tabulku, která obsahuje jeden sloupec s pole objektů v každé buňce – faktury a jeden sloupec s pole Skalární typy – hodnocení.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-265">For example, “ExampleTable” below is a MongoDB table that has one column with an array of Objects in each cell – Invoices, and one column with an array of Scalar types – Ratings.</span></span>

| <span data-ttu-id="8f0a2-266">_id</span><span class="sxs-lookup"><span data-stu-id="8f0a2-266">_id</span></span> | <span data-ttu-id="8f0a2-267">Jméno zákazníka</span><span class="sxs-lookup"><span data-stu-id="8f0a2-267">Customer Name</span></span> | <span data-ttu-id="8f0a2-268">Faktury</span><span class="sxs-lookup"><span data-stu-id="8f0a2-268">Invoices</span></span> | <span data-ttu-id="8f0a2-269">Úrovně služeb</span><span class="sxs-lookup"><span data-stu-id="8f0a2-269">Service Level</span></span> | <span data-ttu-id="8f0a2-270">Hodnocení</span><span class="sxs-lookup"><span data-stu-id="8f0a2-270">Ratings</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="8f0a2-271">1111</span><span class="sxs-lookup"><span data-stu-id="8f0a2-271">1111</span></span> |<span data-ttu-id="8f0a2-272">ABC</span><span class="sxs-lookup"><span data-stu-id="8f0a2-272">ABC</span></span> |<span data-ttu-id="8f0a2-273">[{invoice_id: "123", položka: "Toaster byl", cena: "456", slevu: "0,2"}, {invoice_id: "124", položka: "sušárny", ceny: slevách "1235": "0,2"}]</span><span class="sxs-lookup"><span data-stu-id="8f0a2-273">[{invoice_id:”123”, item:”toaster”, price:”456”, discount:”0.2”}, {invoice_id:”124”, item:”oven”, price: ”1235”, discount: ”0.2”}]</span></span> |<span data-ttu-id="8f0a2-274">Stříbrná</span><span class="sxs-lookup"><span data-stu-id="8f0a2-274">Silver</span></span> |<span data-ttu-id="8f0a2-275">[5,6]</span><span class="sxs-lookup"><span data-stu-id="8f0a2-275">[5,6]</span></span> |
| <span data-ttu-id="8f0a2-276">2222</span><span class="sxs-lookup"><span data-stu-id="8f0a2-276">2222</span></span> |<span data-ttu-id="8f0a2-277">XYZ</span><span class="sxs-lookup"><span data-stu-id="8f0a2-277">XYZ</span></span> |<span data-ttu-id="8f0a2-278">[{invoice_id: "135", položka: "ledničky", cena: "12543", slevu: "0,0"}]</span><span class="sxs-lookup"><span data-stu-id="8f0a2-278">[{invoice_id:”135”, item:”fridge”, price: ”12543”, discount: ”0.0”}]</span></span> |<span data-ttu-id="8f0a2-279">Zlatý</span><span class="sxs-lookup"><span data-stu-id="8f0a2-279">Gold</span></span> |<span data-ttu-id="8f0a2-280">[1,2]</span><span class="sxs-lookup"><span data-stu-id="8f0a2-280">[1,2]</span></span> |

<span data-ttu-id="8f0a2-281">Ovladač by vygeneroval více virtuální tabulky k reprezentaci této jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-281">The driver would generate multiple virtual tables to represent this single table.</span></span> <span data-ttu-id="8f0a2-282">První virtuální tabulky je základní tabulka s názvem "ExampleTable", viz následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-282">The first virtual table is the base table named “ExampleTable”, shown below.</span></span> <span data-ttu-id="8f0a2-283">Základní tabulka obsahuje všechna data z původní tabulky, ale data z pole byla vynechána a je v tabulkách virtuální rozbalena.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-283">The base table contains all the data of the original table, but the data from the arrays has been omitted and is expanded in the virtual tables.</span></span>

| <span data-ttu-id="8f0a2-284">_id</span><span class="sxs-lookup"><span data-stu-id="8f0a2-284">_id</span></span> | <span data-ttu-id="8f0a2-285">Jméno zákazníka</span><span class="sxs-lookup"><span data-stu-id="8f0a2-285">Customer Name</span></span> | <span data-ttu-id="8f0a2-286">Úrovně služeb</span><span class="sxs-lookup"><span data-stu-id="8f0a2-286">Service Level</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f0a2-287">1111</span><span class="sxs-lookup"><span data-stu-id="8f0a2-287">1111</span></span> |<span data-ttu-id="8f0a2-288">ABC</span><span class="sxs-lookup"><span data-stu-id="8f0a2-288">ABC</span></span> |<span data-ttu-id="8f0a2-289">Stříbrná</span><span class="sxs-lookup"><span data-stu-id="8f0a2-289">Silver</span></span> |
| <span data-ttu-id="8f0a2-290">2222</span><span class="sxs-lookup"><span data-stu-id="8f0a2-290">2222</span></span> |<span data-ttu-id="8f0a2-291">XYZ</span><span class="sxs-lookup"><span data-stu-id="8f0a2-291">XYZ</span></span> |<span data-ttu-id="8f0a2-292">Zlatý</span><span class="sxs-lookup"><span data-stu-id="8f0a2-292">Gold</span></span> |

<span data-ttu-id="8f0a2-293">Virtuální tabulky, které představují původní pole v příkladu v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-293">The following tables show the virtual tables that represent the original arrays in the example.</span></span> <span data-ttu-id="8f0a2-294">Tyto tabulky obsahují následující:</span><span class="sxs-lookup"><span data-stu-id="8f0a2-294">These tables contain the following:</span></span>

* <span data-ttu-id="8f0a2-295">Odkaz zpět na původní sloupec primárního klíče odpovídající řádek původní pole (přes sloupec _id)</span><span class="sxs-lookup"><span data-stu-id="8f0a2-295">A reference back to the original primary key column corresponding to the row of the original array (via the _id column)</span></span>
* <span data-ttu-id="8f0a2-296">Označení pozice dat v rámci původní pole</span><span class="sxs-lookup"><span data-stu-id="8f0a2-296">An indication of the position of the data within the original array</span></span>
* <span data-ttu-id="8f0a2-297">Rozšířená data pro každý prvek v rámci pole</span><span class="sxs-lookup"><span data-stu-id="8f0a2-297">The expanded data for each element within the array</span></span>

<span data-ttu-id="8f0a2-298">Tabulka "ExampleTable_Invoices":</span><span class="sxs-lookup"><span data-stu-id="8f0a2-298">Table “ExampleTable_Invoices”:</span></span>

| <span data-ttu-id="8f0a2-299">_id</span><span class="sxs-lookup"><span data-stu-id="8f0a2-299">_id</span></span> | <span data-ttu-id="8f0a2-300">ExampleTable_Invoices_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="8f0a2-300">ExampleTable_Invoices_dim1_idx</span></span> | <span data-ttu-id="8f0a2-301">invoice_id</span><span class="sxs-lookup"><span data-stu-id="8f0a2-301">invoice_id</span></span> | <span data-ttu-id="8f0a2-302">Položka</span><span class="sxs-lookup"><span data-stu-id="8f0a2-302">item</span></span> | <span data-ttu-id="8f0a2-303">price</span><span class="sxs-lookup"><span data-stu-id="8f0a2-303">price</span></span> | <span data-ttu-id="8f0a2-304">Sleva</span><span class="sxs-lookup"><span data-stu-id="8f0a2-304">Discount</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="8f0a2-305">1111</span><span class="sxs-lookup"><span data-stu-id="8f0a2-305">1111</span></span> |<span data-ttu-id="8f0a2-306">0</span><span class="sxs-lookup"><span data-stu-id="8f0a2-306">0</span></span> |<span data-ttu-id="8f0a2-307">123</span><span class="sxs-lookup"><span data-stu-id="8f0a2-307">123</span></span> |<span data-ttu-id="8f0a2-308">Toaster byl</span><span class="sxs-lookup"><span data-stu-id="8f0a2-308">toaster</span></span> |<span data-ttu-id="8f0a2-309">456</span><span class="sxs-lookup"><span data-stu-id="8f0a2-309">456</span></span> |<span data-ttu-id="8f0a2-310">0.2</span><span class="sxs-lookup"><span data-stu-id="8f0a2-310">0.2</span></span> |
| <span data-ttu-id="8f0a2-311">1111</span><span class="sxs-lookup"><span data-stu-id="8f0a2-311">1111</span></span> |<span data-ttu-id="8f0a2-312">1</span><span class="sxs-lookup"><span data-stu-id="8f0a2-312">1</span></span> |<span data-ttu-id="8f0a2-313">124</span><span class="sxs-lookup"><span data-stu-id="8f0a2-313">124</span></span> |<span data-ttu-id="8f0a2-314">sušárny</span><span class="sxs-lookup"><span data-stu-id="8f0a2-314">oven</span></span> |<span data-ttu-id="8f0a2-315">1235</span><span class="sxs-lookup"><span data-stu-id="8f0a2-315">1235</span></span> |<span data-ttu-id="8f0a2-316">0.2</span><span class="sxs-lookup"><span data-stu-id="8f0a2-316">0.2</span></span> |
| <span data-ttu-id="8f0a2-317">2222</span><span class="sxs-lookup"><span data-stu-id="8f0a2-317">2222</span></span> |<span data-ttu-id="8f0a2-318">0</span><span class="sxs-lookup"><span data-stu-id="8f0a2-318">0</span></span> |<span data-ttu-id="8f0a2-319">135</span><span class="sxs-lookup"><span data-stu-id="8f0a2-319">135</span></span> |<span data-ttu-id="8f0a2-320">ledničky</span><span class="sxs-lookup"><span data-stu-id="8f0a2-320">fridge</span></span> |<span data-ttu-id="8f0a2-321">12543</span><span class="sxs-lookup"><span data-stu-id="8f0a2-321">12543</span></span> |<span data-ttu-id="8f0a2-322">0.0</span><span class="sxs-lookup"><span data-stu-id="8f0a2-322">0.0</span></span> |

<span data-ttu-id="8f0a2-323">Tabulka "ExampleTable_Ratings":</span><span class="sxs-lookup"><span data-stu-id="8f0a2-323">Table “ExampleTable_Ratings”:</span></span>

| <span data-ttu-id="8f0a2-324">_id</span><span class="sxs-lookup"><span data-stu-id="8f0a2-324">_id</span></span> | <span data-ttu-id="8f0a2-325">ExampleTable_Ratings_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="8f0a2-325">ExampleTable_Ratings_dim1_idx</span></span> | <span data-ttu-id="8f0a2-326">ExampleTable_Ratings</span><span class="sxs-lookup"><span data-stu-id="8f0a2-326">ExampleTable_Ratings</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f0a2-327">1111</span><span class="sxs-lookup"><span data-stu-id="8f0a2-327">1111</span></span> |<span data-ttu-id="8f0a2-328">0</span><span class="sxs-lookup"><span data-stu-id="8f0a2-328">0</span></span> |<span data-ttu-id="8f0a2-329">5</span><span class="sxs-lookup"><span data-stu-id="8f0a2-329">5</span></span> |
| <span data-ttu-id="8f0a2-330">1111</span><span class="sxs-lookup"><span data-stu-id="8f0a2-330">1111</span></span> |<span data-ttu-id="8f0a2-331">1</span><span class="sxs-lookup"><span data-stu-id="8f0a2-331">1</span></span> |<span data-ttu-id="8f0a2-332">6</span><span class="sxs-lookup"><span data-stu-id="8f0a2-332">6</span></span> |
| <span data-ttu-id="8f0a2-333">2222</span><span class="sxs-lookup"><span data-stu-id="8f0a2-333">2222</span></span> |<span data-ttu-id="8f0a2-334">0</span><span class="sxs-lookup"><span data-stu-id="8f0a2-334">0</span></span> |<span data-ttu-id="8f0a2-335">1</span><span class="sxs-lookup"><span data-stu-id="8f0a2-335">1</span></span> |
| <span data-ttu-id="8f0a2-336">2222</span><span class="sxs-lookup"><span data-stu-id="8f0a2-336">2222</span></span> |<span data-ttu-id="8f0a2-337">1</span><span class="sxs-lookup"><span data-stu-id="8f0a2-337">1</span></span> |<span data-ttu-id="8f0a2-338">2</span><span class="sxs-lookup"><span data-stu-id="8f0a2-338">2</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="8f0a2-339">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="8f0a2-339">Map source to sink columns</span></span>
<span data-ttu-id="8f0a2-340">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-340">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="8f0a2-341">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="8f0a2-341">Repeatable read from relational sources</span></span>
<span data-ttu-id="8f0a2-342">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-342">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="8f0a2-343">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-343">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="8f0a2-344">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-344">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="8f0a2-345">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-345">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="8f0a2-346">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="8f0a2-346">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="8f0a2-347">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="8f0a2-347">Performance and Tuning</span></span>
<span data-ttu-id="8f0a2-348">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-348">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f0a2-349">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f0a2-349">Next Steps</span></span>
<span data-ttu-id="8f0a2-350">V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) článek podrobné pokyny pro vytváření dat kanál, který přesouvá data z místního úložiště dat k úložišti dat Azure.</span><span class="sxs-lookup"><span data-stu-id="8f0a2-350">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions for creating a data pipeline that moves data from an on-premises data store to an Azure data store.</span></span>
