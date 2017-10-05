---
title: "Přesun dat z Cassandra pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z databáze Cassandra místně pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: f2b225bdbdf2880d26a6ab5f992301bf0a804b0d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a><span data-ttu-id="0aeb7-103">Přesun dat z databáze Cassandra místně pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="0aeb7-103">Move data from an on-premises Cassandra database using Azure Data Factory</span></span>
<span data-ttu-id="0aeb7-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z databáze Cassandra místně.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises Cassandra database.</span></span> <span data-ttu-id="0aeb7-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="0aeb7-106">Z úložiště dat Cassandra místní může kopírovat data do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-106">You can copy data from an on-premises Cassandra data store to any supported sink data store.</span></span> <span data-ttu-id="0aeb7-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="0aeb7-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z jiného úložiště dat Cassandra k jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat do úložiště dat Cassandra.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-108">Data factory currently supports only moving data from a Cassandra data store to other data stores, but not for moving data from other data stores to a Cassandra data store.</span></span> 

## <a name="supported-versions"></a><span data-ttu-id="0aeb7-109">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="0aeb7-109">Supported versions</span></span>
<span data-ttu-id="0aeb7-110">Cassandra konektor podporuje následující verze systému Cassandra: 2.X.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-110">The Cassandra connector supports the following versions of Cassandra: 2.X.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0aeb7-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0aeb7-111">Prerequisites</span></span>
<span data-ttu-id="0aeb7-112">Pro službu Azure Data Factory být schopni připojit k vaší databázi Cassandra místní musíte nainstalovat brána pro správu dat na stejném počítači, který je hostitelem databáze nebo na samostatný počítač, aby se zabránilo neslučitelných pro prostředky s databází.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-112">For the Azure Data Factory service to be able to connect to your on-premises Cassandra database, you must install a Data Management Gateway on the same machine that hosts the database or on a separate machine to avoid competing for resources with the database.</span></span> <span data-ttu-id="0aeb7-113">Brána pro správu dat je komponenta, která připojuje místní zdroje dat do cloudové služby v zabezpečený a spravovaný.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-113">Data Management Gateway is a component that connects on-premises data sources to cloud services in a secure and managed way.</span></span> <span data-ttu-id="0aeb7-114">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o Brána pro správu dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="0aeb7-115">V tématu [přesun dat z lokálního prostředí do cloudu](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny o nastavení brány datovém kanálu pro přesun dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-115">See [Move data from on-premises to cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway a data pipeline to move data.</span></span>

<span data-ttu-id="0aeb7-116">Musíte použít bránu pro připojení k databázi Cassandra i v případě, že databáze je hostovaná v cloudu, například na virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-116">You must use the gateway to connect to a Cassandra database even if the database is hosted in the cloud, for example, on an Azure IaaS VM.</span></span> <span data-ttu-id="0aeb7-117">Y můžete na stejný virtuální počítač, který je hostitelem databáze může mít bránu nebo na samostatný virtuální počítač stejně dlouho jako brány může připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-117">Y You can have the gateway on the same VM that hosts the database or on a separate VM as long as the gateway can connect to the database.</span></span>  

<span data-ttu-id="0aeb7-118">Při instalaci brány se automaticky nainstaluje ovladač Microsoft Cassandra ODBC použít pro připojení k databázi Cassandra.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-118">When you install the gateway, it automatically installs a Microsoft Cassandra ODBC driver used to connect to Cassandra database.</span></span> <span data-ttu-id="0aeb7-119">Proto nemusíte ručně nainstalovat všechny ovladače v počítači s bránou při kopírování dat z databáze Cassandra.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-119">Therefore, you don't need to manually install any driver on the gateway machine when copying data from the Cassandra database.</span></span> 

> [!NOTE]
> <span data-ttu-id="0aeb7-120">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-120">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="0aeb7-121">Začínáme</span><span class="sxs-lookup"><span data-stu-id="0aeb7-121">Getting started</span></span>
<span data-ttu-id="0aeb7-122">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-122">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="0aeb7-123">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-123">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="0aeb7-124">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="0aeb7-125">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-125">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="0aeb7-126">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="0aeb7-127">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="0aeb7-127">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="0aeb7-128">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-128">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="0aeb7-129">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-129">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="0aeb7-130">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="0aeb7-131">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-131">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="0aeb7-132">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="0aeb7-133">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat Cassandra místní, naleznete v tématu [JSON příklad: kopírování dat z Cassandra do objektu Blob Azure](#json-example-copy-data-from-cassandra-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-133">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises Cassandra data store, see [JSON example: Copy data from Cassandra to Azure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="0aeb7-134">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k úložišti dat Cassandra:</span><span class="sxs-lookup"><span data-stu-id="0aeb7-134">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Cassandra data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="0aeb7-135">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="0aeb7-135">Linked service properties</span></span>
<span data-ttu-id="0aeb7-136">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro Cassandra propojené služby.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-136">The following table provides description for JSON elements specific to Cassandra linked service.</span></span>

| <span data-ttu-id="0aeb7-137">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0aeb7-137">Property</span></span> | <span data-ttu-id="0aeb7-138">Popis</span><span class="sxs-lookup"><span data-stu-id="0aeb7-138">Description</span></span> | <span data-ttu-id="0aeb7-139">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0aeb7-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0aeb7-140">type</span><span class="sxs-lookup"><span data-stu-id="0aeb7-140">type</span></span> |<span data-ttu-id="0aeb7-141">Vlastnost typu musí být nastavena na: **OnPremisesCassandra**</span><span class="sxs-lookup"><span data-stu-id="0aeb7-141">The type property must be set to: **OnPremisesCassandra**</span></span> |<span data-ttu-id="0aeb7-142">Ano</span><span class="sxs-lookup"><span data-stu-id="0aeb7-142">Yes</span></span> |
| <span data-ttu-id="0aeb7-143">hostitele</span><span class="sxs-lookup"><span data-stu-id="0aeb7-143">host</span></span> |<span data-ttu-id="0aeb7-144">Jeden nebo více IP adres nebo názvů hostitelů Cassandra serverů.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-144">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="0aeb7-145">Zadejte seznam IP adres nebo názvů hostitelů se připojit na všechny servery současně.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-145">Specify a comma-separated list of IP addresses or host names to connect to all servers concurrently.</span></span> |<span data-ttu-id="0aeb7-146">Ano</span><span class="sxs-lookup"><span data-stu-id="0aeb7-146">Yes</span></span> |
| <span data-ttu-id="0aeb7-147">port</span><span class="sxs-lookup"><span data-stu-id="0aeb7-147">port</span></span> |<span data-ttu-id="0aeb7-148">Port TCP, který používá Cassandra server naslouchat pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-148">The TCP port that the Cassandra server uses to listen for client connections.</span></span> |<span data-ttu-id="0aeb7-149">Ne, výchozí hodnota: 9042</span><span class="sxs-lookup"><span data-stu-id="0aeb7-149">No, default value: 9042</span></span> |
| <span data-ttu-id="0aeb7-150">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-150">authenticationType</span></span> |<span data-ttu-id="0aeb7-151">Basic nebo Anonymous</span><span class="sxs-lookup"><span data-stu-id="0aeb7-151">Basic, or Anonymous</span></span> |<span data-ttu-id="0aeb7-152">Ano</span><span class="sxs-lookup"><span data-stu-id="0aeb7-152">Yes</span></span> |
| <span data-ttu-id="0aeb7-153">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="0aeb7-153">username</span></span> |<span data-ttu-id="0aeb7-154">Zadejte uživatelské jméno pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-154">Specify user name for the user account.</span></span> |<span data-ttu-id="0aeb7-155">Ano, pokud authenticationType je nastaven na Basic.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-155">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="0aeb7-156">heslo</span><span class="sxs-lookup"><span data-stu-id="0aeb7-156">password</span></span> |<span data-ttu-id="0aeb7-157">Zadejte heslo pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-157">Specify password for the user account.</span></span> |<span data-ttu-id="0aeb7-158">Ano, pokud authenticationType je nastaven na Basic.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-158">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="0aeb7-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="0aeb7-159">gatewayName</span></span> |<span data-ttu-id="0aeb7-160">Název brány, která se používá pro připojení k místní databázi Cassandra.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-160">The name of the gateway that is used to connect to the on-premises Cassandra database.</span></span> |<span data-ttu-id="0aeb7-161">Ano</span><span class="sxs-lookup"><span data-stu-id="0aeb7-161">Yes</span></span> |
| <span data-ttu-id="0aeb7-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="0aeb7-162">encryptedCredential</span></span> |<span data-ttu-id="0aeb7-163">Přihlašovací údaje zašifrovaná pomocí brány.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-163">Credential encrypted by the gateway.</span></span> |<span data-ttu-id="0aeb7-164">Ne</span><span class="sxs-lookup"><span data-stu-id="0aeb7-164">No</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="0aeb7-165">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="0aeb7-165">Dataset properties</span></span>
<span data-ttu-id="0aeb7-166">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-166">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="0aeb7-167">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="0aeb7-168">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-168">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="0aeb7-169">Rámci typeProperties část datové sady typ **CassandraTable** má následující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="0aeb7-169">The typeProperties section for dataset of type **CassandraTable** has the following properties</span></span>

| <span data-ttu-id="0aeb7-170">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0aeb7-170">Property</span></span> | <span data-ttu-id="0aeb7-171">Popis</span><span class="sxs-lookup"><span data-stu-id="0aeb7-171">Description</span></span> | <span data-ttu-id="0aeb7-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0aeb7-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0aeb7-173">keyspace</span><span class="sxs-lookup"><span data-stu-id="0aeb7-173">keyspace</span></span> |<span data-ttu-id="0aeb7-174">Název keyspace nebo schéma v Cassandra databáze.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-174">Name of the keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="0aeb7-175">Ano (Pokud **dotazu** pro **CassandraSource** není definován).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-175">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="0aeb7-176">tableName</span><span class="sxs-lookup"><span data-stu-id="0aeb7-176">tableName</span></span> |<span data-ttu-id="0aeb7-177">Název tabulky v databázi Cassandra.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-177">Name of the table in Cassandra database.</span></span> |<span data-ttu-id="0aeb7-178">Ano (Pokud **dotazu** pro **CassandraSource** není definován).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-178">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="0aeb7-179">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="0aeb7-179">Copy activity properties</span></span>
<span data-ttu-id="0aeb7-180">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-180">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="0aeb7-181">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-181">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="0aeb7-182">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-182">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="0aeb7-183">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-183">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="0aeb7-184">Pokud je zdroj typu **CassandraSource**, následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="0aeb7-184">When source is of type **CassandraSource**, the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="0aeb7-185">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0aeb7-185">Property</span></span> | <span data-ttu-id="0aeb7-186">Popis</span><span class="sxs-lookup"><span data-stu-id="0aeb7-186">Description</span></span> | <span data-ttu-id="0aeb7-187">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="0aeb7-187">Allowed values</span></span> | <span data-ttu-id="0aeb7-188">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="0aeb7-188">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0aeb7-189">query</span><span class="sxs-lookup"><span data-stu-id="0aeb7-189">query</span></span> |<span data-ttu-id="0aeb7-190">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-190">Use the custom query to read data.</span></span> |<span data-ttu-id="0aeb7-191">Dotaz SQL 92 nebo CQL dotazu.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-191">SQL-92 query or CQL query.</span></span> <span data-ttu-id="0aeb7-192">V tématu [CQL odkaz](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-192">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="0aeb7-193">Při použití příkazu jazyka SQL, zadejte **keyspace name.table název** představují tabulky, které mají být zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-193">When using SQL query, specify **keyspace name.table name** to represent the table you want to query.</span></span> |<span data-ttu-id="0aeb7-194">Ne (pokud jsou definovány tableName a keyspace v sadě dat).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-194">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="0aeb7-195">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="0aeb7-195">consistencyLevel</span></span> |<span data-ttu-id="0aeb7-196">Úroveň konzistence Určuje, kolik repliky musí odpovědět na požadavek čtení před vrácením dat do klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-196">The consistency level specifies how many replicas must respond to a read request before returning data to the client application.</span></span> <span data-ttu-id="0aeb7-197">Cassandra ověří zadaný počet replik pro data, aby pokryl požadavek na čtení.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-197">Cassandra checks the specified number of replicas for data to satisfy the read request.</span></span> |<span data-ttu-id="0aeb7-198">JEDEN, DVA, TŘI, KVORA, VŠE, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-198">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="0aeb7-199">V tématu [konfigurace konzistenci dat](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-199">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="0aeb7-200">Ne.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-200">No.</span></span> <span data-ttu-id="0aeb7-201">Výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-201">Default value is ONE.</span></span> |

## <a name="json-example-copy-data-from-cassandra-to-azure-blob"></a><span data-ttu-id="0aeb7-202">Příklad JSON: kopírování dat z Cassandra do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="0aeb7-202">JSON example: Copy data from Cassandra to Azure Blob</span></span>
<span data-ttu-id="0aeb7-203">Tento příklad obsahuje ukázkové JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-203">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="0aeb7-204">Ukazuje, jak zkopírovat data z databáze Cassandra místně do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-204">It shows how to copy data from an on-premises Cassandra database to an Azure Blob Storage.</span></span> <span data-ttu-id="0aeb7-205">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-205">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0aeb7-206">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-206">This sample provides JSON snippets.</span></span> <span data-ttu-id="0aeb7-207">Podrobné pokyny pro vytvoření objektu pro vytváření dat neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-207">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="0aeb7-208">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-208">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="0aeb7-209">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="0aeb7-209">The sample has the following data factory entities:</span></span>

* <span data-ttu-id="0aeb7-210">Propojené služby typu [OnPremisesCassandra](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-210">A linked service of type [OnPremisesCassandra](#linked-service-properties).</span></span>
* <span data-ttu-id="0aeb7-211">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-211">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="0aeb7-212">Vstup [datovou sadu](data-factory-create-datasets.md) typu [CassandraTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-212">An input [dataset](data-factory-create-datasets.md) of type [CassandraTable](#dataset-properties).</span></span>
* <span data-ttu-id="0aeb7-213">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-213">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="0aeb7-214">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [CassandraSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-214">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [CassandraSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="0aeb7-215">**Cassandra propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="0aeb7-215">**Cassandra linked service:**</span></span>

<span data-ttu-id="0aeb7-216">Tento příklad používá **Cassandra** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-216">This example uses the **Cassandra** linked service.</span></span> <span data-ttu-id="0aeb7-217">V tématu [Cassandra propojená služba](#linked-service-properties) části vlastnostech podporovaných zprostředkovatelem tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-217">See [Cassandra linked service](#linked-service-properties) section for the properties supported by this linked service.</span></span>  

```json
{
    "name": "CassandraLinkedService",
    "properties":
    {
        "type": "OnPremisesCassandra",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "host": "mycassandraserver",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

<span data-ttu-id="0aeb7-218">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="0aeb7-218">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="0aeb7-219">**Cassandra vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="0aeb7-219">**Cassandra input dataset:**</span></span>

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "mykeyspace"
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

<span data-ttu-id="0aeb7-220">Nastavení **externí** k **true** služba Data Factory informuje, že datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-220">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

<span data-ttu-id="0aeb7-221">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="0aeb7-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="0aeb7-222">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-222">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/fromcassandra"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="0aeb7-223">**Aktivita kopírování v kanálu s Cassandra zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="0aeb7-223">**Copy activity in a pipeline with Cassandra source and Blob sink:**</span></span>

<span data-ttu-id="0aeb7-224">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-224">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="0aeb7-225">V definici JSON kanálu **zdroj** je typ nastaven na **CassandraSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-225">In the pipeline JSON definition, the **source** type is set to **CassandraSource** and **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="0aeb7-226">V tématu [vlastnosti typu RelationalSource](#copy-activity-properties) pro seznam vlastností nepodporuje RelationalSource.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-226">See [RelationalSource type properties](#copy-activity-properties) for the list of properties supported by the RelationalSource.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "CassandraInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"

                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }
        ]    
    }
}
```

### <a name="type-mapping-for-cassandra"></a><span data-ttu-id="0aeb7-227">Mapování typu pro Cassandra</span><span class="sxs-lookup"><span data-stu-id="0aeb7-227">Type mapping for Cassandra</span></span>
| <span data-ttu-id="0aeb7-228">Typ Cassandra</span><span class="sxs-lookup"><span data-stu-id="0aeb7-228">Cassandra Type</span></span> | <span data-ttu-id="0aeb7-229">.NET na základě typu</span><span class="sxs-lookup"><span data-stu-id="0aeb7-229">.Net Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="0aeb7-230">ASCII</span><span class="sxs-lookup"><span data-stu-id="0aeb7-230">ASCII</span></span> |<span data-ttu-id="0aeb7-231">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0aeb7-231">String</span></span> |
| <span data-ttu-id="0aeb7-232">BIGINT</span><span class="sxs-lookup"><span data-stu-id="0aeb7-232">BIGINT</span></span> |<span data-ttu-id="0aeb7-233">Int64</span><span class="sxs-lookup"><span data-stu-id="0aeb7-233">Int64</span></span> |
| <span data-ttu-id="0aeb7-234">OBJEKT BLOB</span><span class="sxs-lookup"><span data-stu-id="0aeb7-234">BLOB</span></span> |<span data-ttu-id="0aeb7-235">Byte]</span><span class="sxs-lookup"><span data-stu-id="0aeb7-235">Byte[]</span></span> |
| <span data-ttu-id="0aeb7-236">LOGICKÁ HODNOTA</span><span class="sxs-lookup"><span data-stu-id="0aeb7-236">BOOLEAN</span></span> |<span data-ttu-id="0aeb7-237">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="0aeb7-237">Boolean</span></span> |
| <span data-ttu-id="0aeb7-238">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="0aeb7-238">DECIMAL</span></span> |<span data-ttu-id="0aeb7-239">Decimal</span><span class="sxs-lookup"><span data-stu-id="0aeb7-239">Decimal</span></span> |
| <span data-ttu-id="0aeb7-240">DOUBLE</span><span class="sxs-lookup"><span data-stu-id="0aeb7-240">DOUBLE</span></span> |<span data-ttu-id="0aeb7-241">Double</span><span class="sxs-lookup"><span data-stu-id="0aeb7-241">Double</span></span> |
| <span data-ttu-id="0aeb7-242">PLOVOUCÍ DESETINNÁ ČÁRKA</span><span class="sxs-lookup"><span data-stu-id="0aeb7-242">FLOAT</span></span> |<span data-ttu-id="0aeb7-243">Jeden</span><span class="sxs-lookup"><span data-stu-id="0aeb7-243">Single</span></span> |
| <span data-ttu-id="0aeb7-244">INET</span><span class="sxs-lookup"><span data-stu-id="0aeb7-244">INET</span></span> |<span data-ttu-id="0aeb7-245">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0aeb7-245">String</span></span> |
| <span data-ttu-id="0aeb7-246">INT</span><span class="sxs-lookup"><span data-stu-id="0aeb7-246">INT</span></span> |<span data-ttu-id="0aeb7-247">Int32</span><span class="sxs-lookup"><span data-stu-id="0aeb7-247">Int32</span></span> |
| <span data-ttu-id="0aeb7-248">TEXT</span><span class="sxs-lookup"><span data-stu-id="0aeb7-248">TEXT</span></span> |<span data-ttu-id="0aeb7-249">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0aeb7-249">String</span></span> |
| <span data-ttu-id="0aeb7-250">ČASOVÉ RAZÍTKO</span><span class="sxs-lookup"><span data-stu-id="0aeb7-250">TIMESTAMP</span></span> |<span data-ttu-id="0aeb7-251">Data a času</span><span class="sxs-lookup"><span data-stu-id="0aeb7-251">DateTime</span></span> |
| <span data-ttu-id="0aeb7-252">TIMEUUID</span><span class="sxs-lookup"><span data-stu-id="0aeb7-252">TIMEUUID</span></span> |<span data-ttu-id="0aeb7-253">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="0aeb7-253">Guid</span></span> |
| <span data-ttu-id="0aeb7-254">UUID</span><span class="sxs-lookup"><span data-stu-id="0aeb7-254">UUID</span></span> |<span data-ttu-id="0aeb7-255">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="0aeb7-255">Guid</span></span> |
| <span data-ttu-id="0aeb7-256">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="0aeb7-256">VARCHAR</span></span> |<span data-ttu-id="0aeb7-257">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0aeb7-257">String</span></span> |
| <span data-ttu-id="0aeb7-258">VARINT</span><span class="sxs-lookup"><span data-stu-id="0aeb7-258">VARINT</span></span> |<span data-ttu-id="0aeb7-259">Decimal</span><span class="sxs-lookup"><span data-stu-id="0aeb7-259">Decimal</span></span> |

> [!NOTE]
> <span data-ttu-id="0aeb7-260">Kolekce typů (mapy, sady, seznamu atd.), najdete v části [pracovat s typy kolekcí Cassandra pomocí virtuální tabulky](#work-with-collections-using-virtual-table) části.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-260">For collection types (map, set, list, etc.), refer to [Work with Cassandra collection types using virtual table](#work-with-collections-using-virtual-table) section.</span></span>
>
> <span data-ttu-id="0aeb7-261">Uživatelem definované typy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-261">User-defined types are not supported.</span></span>
>
> <span data-ttu-id="0aeb7-262">Délka binárního sloupce a sloupce, řetězec délky nemůže být větší než 4000.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-262">The length of Binary Column and String Column lengths cannot be greater than 4000.</span></span>
>
>

## <a name="work-with-collections-using-virtual-table"></a><span data-ttu-id="0aeb7-263">Práce s kolekcí pomocí virtuální tabulky</span><span class="sxs-lookup"><span data-stu-id="0aeb7-263">Work with collections using virtual table</span></span>
<span data-ttu-id="0aeb7-264">Azure Data Factory používá integrované ovladače ODBC pro připojení k a kopírování dat z databáze Cassandra.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-264">Azure Data Factory uses a built-in ODBC driver to connect to and copy data from your Cassandra database.</span></span> <span data-ttu-id="0aeb7-265">Ovladač pro typy kolekcí včetně mapy, sady a seznamem, renormalizes data do odpovídající virtuální tabulky.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-265">For collection types including map, set and list, the driver renormalizes the data into corresponding virtual tables.</span></span> <span data-ttu-id="0aeb7-266">Konkrétně Pokud tabulka obsahuje všechny sloupce, kolekce, ovladač generuje následující virtuální tabulky:</span><span class="sxs-lookup"><span data-stu-id="0aeb7-266">Specifically, if a table contains any collection columns, the driver generates the following virtual tables:</span></span>

* <span data-ttu-id="0aeb7-267">A **základní tabulka**, která obsahuje stejná data jako skutečné tabulky s výjimkou kolekce sloupců.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-267">A **base table**, which contains the same data as the real table except for the collection columns.</span></span> <span data-ttu-id="0aeb7-268">Základní tabulka používá stejný název jako skutečné tabulky, která reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-268">The base table uses the same name as the real table that it represents.</span></span>
* <span data-ttu-id="0aeb7-269">A **virtuální tabulku** pro každý sloupec kolekce, které rozšíří vnořená data.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-269">A **virtual table** for each collection column, which expands the nested data.</span></span> <span data-ttu-id="0aeb7-270">Virtuální tabulky, které představují kolekce jsou pojmenované pomocí názvu skutečné tabulky oddělovač "*vt*" a název sloupce.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-270">The virtual tables that represent collections are named using the name of the real table, a separator “*vt*” and the name of the column.</span></span>

<span data-ttu-id="0aeb7-271">Virtuální tabulky odkazovat na data v tabulce skutečné povolení ovladače pro přístup k datům nenormalizované.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-271">Virtual tables refer to the data in the real table, enabling the driver to access the denormalized data.</span></span> <span data-ttu-id="0aeb7-272">Příklad naleznete v části Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-272">See Example section for details.</span></span> <span data-ttu-id="0aeb7-273">Dotazování a připojení virtuální tabulky, můžete přístup k obsahu Cassandra kolekcí.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-273">You can access the content of Cassandra collections by querying and joining the virtual tables.</span></span>

<span data-ttu-id="0aeb7-274">Můžete použít [Průvodce kopírováním](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) intuitivně zobrazit seznam tabulek v databázi Cassandra včetně virtuální tabulky a zobrazte náhled dat, uvnitř.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-274">You can use the [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) to intuitively view the list of tables in Cassandra database including the virtual tables, and preview the data inside.</span></span> <span data-ttu-id="0aeb7-275">Můžete také vytvořit dotaz v Průvodci kopírováním a ověření zobrazíte výsledek.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-275">You can also construct a query in the Copy Wizard and validate to see the result.</span></span>

### <a name="example"></a><span data-ttu-id="0aeb7-276">Příklad</span><span class="sxs-lookup"><span data-stu-id="0aeb7-276">Example</span></span>
<span data-ttu-id="0aeb7-277">Například následující "ExampleTable" je Cassandra tabulku databáze, která obsahuje celé číslo primární klíče sloupec s názvem "pk_int", o textový sloupec s názvem hodnotu, sloupce seznamu, mapy sloupec a sadu sloupec (s názvem "StringSet").</span><span class="sxs-lookup"><span data-stu-id="0aeb7-277">For example, the following “ExampleTable” is a Cassandra database table that contains an integer primary key column named “pk_int”, a text column named value, a list column, a map column, and a set column (named “StringSet”).</span></span>

| <span data-ttu-id="0aeb7-278">pk_int</span><span class="sxs-lookup"><span data-stu-id="0aeb7-278">pk_int</span></span> | <span data-ttu-id="0aeb7-279">Hodnota</span><span class="sxs-lookup"><span data-stu-id="0aeb7-279">Value</span></span> | <span data-ttu-id="0aeb7-280">Seznam</span><span class="sxs-lookup"><span data-stu-id="0aeb7-280">List</span></span> | <span data-ttu-id="0aeb7-281">mapy</span><span class="sxs-lookup"><span data-stu-id="0aeb7-281">Map</span></span> | <span data-ttu-id="0aeb7-282">StringSet</span><span class="sxs-lookup"><span data-stu-id="0aeb7-282">StringSet</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="0aeb7-283">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-283">1</span></span> |<span data-ttu-id="0aeb7-284">"Ukázková hodnota 1"</span><span class="sxs-lookup"><span data-stu-id="0aeb7-284">"sample value 1"</span></span> |<span data-ttu-id="0aeb7-285">["1", "2", "3"]</span><span class="sxs-lookup"><span data-stu-id="0aeb7-285">["1", "2", "3"]</span></span> |<span data-ttu-id="0aeb7-286">{"S1": "a", "S2": "b"}</span><span class="sxs-lookup"><span data-stu-id="0aeb7-286">{"S1": "a", "S2": "b"}</span></span> |<span data-ttu-id="0aeb7-287">{"A", "B", "C"}</span><span class="sxs-lookup"><span data-stu-id="0aeb7-287">{"A", "B", "C"}</span></span> |
| <span data-ttu-id="0aeb7-288">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-288">3</span></span> |<span data-ttu-id="0aeb7-289">"Ukázková hodnota 3"</span><span class="sxs-lookup"><span data-stu-id="0aeb7-289">"sample value 3"</span></span> |<span data-ttu-id="0aeb7-290">["100", "101", "102", "105"]</span><span class="sxs-lookup"><span data-stu-id="0aeb7-290">["100", "101", "102", "105"]</span></span> |<span data-ttu-id="0aeb7-291">{"S1": "t"}</span><span class="sxs-lookup"><span data-stu-id="0aeb7-291">{"S1": "t"}</span></span> |<span data-ttu-id="0aeb7-292">{"A", "E"}</span><span class="sxs-lookup"><span data-stu-id="0aeb7-292">{"A", "E"}</span></span> |

<span data-ttu-id="0aeb7-293">Ovladač by vygeneroval více virtuální tabulky k reprezentaci této jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-293">The driver would generate multiple virtual tables to represent this single table.</span></span> <span data-ttu-id="0aeb7-294">Sloupce cizích klíčů v tabulkách virtuální odkazovat sloupců primárních klíčů v tabulce skutečné a označit skutečné tabulky řádek, který odpovídá řádku virtuální tabulky.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-294">The foreign key columns in the virtual tables reference the primary key columns in the real table, and indicate which real table row the virtual table row corresponds to.</span></span>

<span data-ttu-id="0aeb7-295">První virtuální tabulky je základní tabulka s názvem "ExampleTable" je uvedené v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-295">The first virtual table is the base table named “ExampleTable” is shown in the following table.</span></span> <span data-ttu-id="0aeb7-296">Základní tabulka obsahuje stejná data jako původní tabulky databáze s výjimkou kolekce, které jsou vynechání z této tabulky a rozšířit ostatní virtuální tabulky.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-296">The base table contains the same data as the original database table except for the collections, which are omitted from this table and expanded in other virtual tables.</span></span>

| <span data-ttu-id="0aeb7-297">pk_int</span><span class="sxs-lookup"><span data-stu-id="0aeb7-297">pk_int</span></span> | <span data-ttu-id="0aeb7-298">Hodnota</span><span class="sxs-lookup"><span data-stu-id="0aeb7-298">Value</span></span> |
| --- | --- |
| <span data-ttu-id="0aeb7-299">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-299">1</span></span> |<span data-ttu-id="0aeb7-300">"Ukázková hodnota 1"</span><span class="sxs-lookup"><span data-stu-id="0aeb7-300">"sample value 1"</span></span> |
| <span data-ttu-id="0aeb7-301">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-301">3</span></span> |<span data-ttu-id="0aeb7-302">"Ukázková hodnota 3"</span><span class="sxs-lookup"><span data-stu-id="0aeb7-302">"sample value 3"</span></span> |

<span data-ttu-id="0aeb7-303">Následující tabulky popisují virtuální tabulky, které renormalize data ze seznamu a mapy, StringSet sloupce.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-303">The following tables show the virtual tables that renormalize the data from the List, Map, and StringSet columns.</span></span> <span data-ttu-id="0aeb7-304">Sloupce s názvy, které končí "_index" nebo "_key" označuje pozici dat v rámci původního seznamu nebo mapy.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-304">The columns with names that end with “_index” or “_key” indicate the position of the data within the original list or map.</span></span> <span data-ttu-id="0aeb7-305">Sloupce s názvy, které končí "_value" obsahují Rozšířená data z kolekce.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-305">The columns with names that end with “_value” contain the expanded data from the collection.</span></span>

#### <a name="table-exampletablevtlist"></a><span data-ttu-id="0aeb7-306">Tabulka "ExampleTable_vt_List":</span><span class="sxs-lookup"><span data-stu-id="0aeb7-306">Table “ExampleTable_vt_List”:</span></span>
| <span data-ttu-id="0aeb7-307">pk_int</span><span class="sxs-lookup"><span data-stu-id="0aeb7-307">pk_int</span></span> | <span data-ttu-id="0aeb7-308">List_index</span><span class="sxs-lookup"><span data-stu-id="0aeb7-308">List_index</span></span> | <span data-ttu-id="0aeb7-309">List_value</span><span class="sxs-lookup"><span data-stu-id="0aeb7-309">List_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0aeb7-310">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-310">1</span></span> |<span data-ttu-id="0aeb7-311">0</span><span class="sxs-lookup"><span data-stu-id="0aeb7-311">0</span></span> |<span data-ttu-id="0aeb7-312">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-312">1</span></span> |
| <span data-ttu-id="0aeb7-313">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-313">1</span></span> |<span data-ttu-id="0aeb7-314">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-314">1</span></span> |<span data-ttu-id="0aeb7-315">2</span><span class="sxs-lookup"><span data-stu-id="0aeb7-315">2</span></span> |
| <span data-ttu-id="0aeb7-316">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-316">1</span></span> |<span data-ttu-id="0aeb7-317">2</span><span class="sxs-lookup"><span data-stu-id="0aeb7-317">2</span></span> |<span data-ttu-id="0aeb7-318">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-318">3</span></span> |
| <span data-ttu-id="0aeb7-319">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-319">3</span></span> |<span data-ttu-id="0aeb7-320">0</span><span class="sxs-lookup"><span data-stu-id="0aeb7-320">0</span></span> |<span data-ttu-id="0aeb7-321">100</span><span class="sxs-lookup"><span data-stu-id="0aeb7-321">100</span></span> |
| <span data-ttu-id="0aeb7-322">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-322">3</span></span> |<span data-ttu-id="0aeb7-323">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-323">1</span></span> |<span data-ttu-id="0aeb7-324">101</span><span class="sxs-lookup"><span data-stu-id="0aeb7-324">101</span></span> |
| <span data-ttu-id="0aeb7-325">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-325">3</span></span> |<span data-ttu-id="0aeb7-326">2</span><span class="sxs-lookup"><span data-stu-id="0aeb7-326">2</span></span> |<span data-ttu-id="0aeb7-327">102</span><span class="sxs-lookup"><span data-stu-id="0aeb7-327">102</span></span> |
| <span data-ttu-id="0aeb7-328">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-328">3</span></span> |<span data-ttu-id="0aeb7-329">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-329">3</span></span> |<span data-ttu-id="0aeb7-330">103</span><span class="sxs-lookup"><span data-stu-id="0aeb7-330">103</span></span> |

#### <a name="table-exampletablevtmap"></a><span data-ttu-id="0aeb7-331">Tabulka "ExampleTable_vt_Map":</span><span class="sxs-lookup"><span data-stu-id="0aeb7-331">Table “ExampleTable_vt_Map”:</span></span>
| <span data-ttu-id="0aeb7-332">pk_int</span><span class="sxs-lookup"><span data-stu-id="0aeb7-332">pk_int</span></span> | <span data-ttu-id="0aeb7-333">Map_key</span><span class="sxs-lookup"><span data-stu-id="0aeb7-333">Map_key</span></span> | <span data-ttu-id="0aeb7-334">Map_value</span><span class="sxs-lookup"><span data-stu-id="0aeb7-334">Map_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0aeb7-335">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-335">1</span></span> |<span data-ttu-id="0aeb7-336">S1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-336">S1</span></span> |<span data-ttu-id="0aeb7-337">A</span><span class="sxs-lookup"><span data-stu-id="0aeb7-337">A</span></span> |
| <span data-ttu-id="0aeb7-338">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-338">1</span></span> |<span data-ttu-id="0aeb7-339">S2</span><span class="sxs-lookup"><span data-stu-id="0aeb7-339">S2</span></span> |<span data-ttu-id="0aeb7-340">B</span><span class="sxs-lookup"><span data-stu-id="0aeb7-340">b</span></span> |
| <span data-ttu-id="0aeb7-341">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-341">3</span></span> |<span data-ttu-id="0aeb7-342">S1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-342">S1</span></span> |<span data-ttu-id="0aeb7-343">T</span><span class="sxs-lookup"><span data-stu-id="0aeb7-343">t</span></span> |

#### <a name="table-exampletablevtstringset"></a><span data-ttu-id="0aeb7-344">Tabulka "ExampleTable_vt_StringSet":</span><span class="sxs-lookup"><span data-stu-id="0aeb7-344">Table “ExampleTable_vt_StringSet”:</span></span>
| <span data-ttu-id="0aeb7-345">pk_int</span><span class="sxs-lookup"><span data-stu-id="0aeb7-345">pk_int</span></span> | <span data-ttu-id="0aeb7-346">StringSet_value</span><span class="sxs-lookup"><span data-stu-id="0aeb7-346">StringSet_value</span></span> |
| --- | --- |
| <span data-ttu-id="0aeb7-347">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-347">1</span></span> |<span data-ttu-id="0aeb7-348">A</span><span class="sxs-lookup"><span data-stu-id="0aeb7-348">A</span></span> |
| <span data-ttu-id="0aeb7-349">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-349">1</span></span> |<span data-ttu-id="0aeb7-350">B</span><span class="sxs-lookup"><span data-stu-id="0aeb7-350">B</span></span> |
| <span data-ttu-id="0aeb7-351">1</span><span class="sxs-lookup"><span data-stu-id="0aeb7-351">1</span></span> |<span data-ttu-id="0aeb7-352">C</span><span class="sxs-lookup"><span data-stu-id="0aeb7-352">C</span></span> |
| <span data-ttu-id="0aeb7-353">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-353">3</span></span> |<span data-ttu-id="0aeb7-354">A</span><span class="sxs-lookup"><span data-stu-id="0aeb7-354">A</span></span> |
| <span data-ttu-id="0aeb7-355">3</span><span class="sxs-lookup"><span data-stu-id="0aeb7-355">3</span></span> |<span data-ttu-id="0aeb7-356">E</span><span class="sxs-lookup"><span data-stu-id="0aeb7-356">E</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="0aeb7-357">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="0aeb7-357">Map source to sink columns</span></span>
<span data-ttu-id="0aeb7-358">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-358">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="0aeb7-359">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="0aeb7-359">Repeatable read from relational sources</span></span>
<span data-ttu-id="0aeb7-360">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-360">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="0aeb7-361">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-361">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="0aeb7-362">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-362">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="0aeb7-363">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-363">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="0aeb7-364">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="0aeb7-364">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="0aeb7-365">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="0aeb7-365">Performance and Tuning</span></span>
<span data-ttu-id="0aeb7-366">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="0aeb7-366">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
