---
title: "aaaMove data z Cassandra pomocí služby Data Factory | Microsoft Docs"
description: "Informace o tom, jak toomove data z Cassandra místní databázi pomocí Azure Data Factory."
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
ms.openlocfilehash: 0e265d3a8439d0a2cb2a5c32e5ea8348a1617621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a><span data-ttu-id="58076-103">Přesun dat z databáze Cassandra místně pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="58076-103">Move data from an on-premises Cassandra database using Azure Data Factory</span></span>
<span data-ttu-id="58076-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze Cassandra místně.</span><span class="sxs-lookup"><span data-stu-id="58076-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises Cassandra database.</span></span> <span data-ttu-id="58076-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="58076-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="58076-106">Může kopírovat data z úložiště úložiště tooany podporované jímku dat místní Cassandra data.</span><span class="sxs-lookup"><span data-stu-id="58076-106">You can copy data from an on-premises Cassandra data store tooany supported sink data store.</span></span> <span data-ttu-id="58076-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="58076-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="58076-108">Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat z datové Cassandra tooother datová úložiště, ale ne pro přesun dat z jiného úložiště dat úložiště tooa Cassandra data.</span><span class="sxs-lookup"><span data-stu-id="58076-108">Data factory currently supports only moving data from a Cassandra data store tooother data stores, but not for moving data from other data stores tooa Cassandra data store.</span></span> 

## <a name="supported-versions"></a><span data-ttu-id="58076-109">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="58076-109">Supported versions</span></span>
<span data-ttu-id="58076-110">Hello Cassandra konektor podporuje následující verze Cassandra hello: 2.X.</span><span class="sxs-lookup"><span data-stu-id="58076-110">hello Cassandra connector supports hello following versions of Cassandra: 2.X.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58076-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="58076-111">Prerequisites</span></span>
<span data-ttu-id="58076-112">Hello Azure Data Factory toobe možné tooconnect tooyour místní Cassandra databáze služby, je nutné nainstalovat brána pro správu dat na hello stejný počítač databázi hello hostitele nebo na samostatný počítač tooavoid, neslučitelných pro prostředky s hello databáze.</span><span class="sxs-lookup"><span data-stu-id="58076-112">For hello Azure Data Factory service toobe able tooconnect tooyour on-premises Cassandra database, you must install a Data Management Gateway on hello same machine that hosts hello database or on a separate machine tooavoid competing for resources with hello database.</span></span> <span data-ttu-id="58076-113">Brána pro správu dat je komponenta, která připojuje místní datové zdroje toocloud služby v zabezpečený a spravovaný.</span><span class="sxs-lookup"><span data-stu-id="58076-113">Data Management Gateway is a component that connects on-premises data sources toocloud services in a secure and managed way.</span></span> <span data-ttu-id="58076-114">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o Brána pro správu dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="58076-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="58076-115">V tématu [přesun dat z místní toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k nastavení brány hello toomove data datového kanálu.</span><span class="sxs-lookup"><span data-stu-id="58076-115">See [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway a data pipeline toomove data.</span></span>

<span data-ttu-id="58076-116">Hello brány tooconnect tooa Cassandra databázi je nutné použít i v případě, že hello je databáze hostována v cloudu hello, například na virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="58076-116">You must use hello gateway tooconnect tooa Cassandra database even if hello database is hosted in hello cloud, for example, on an Azure IaaS VM.</span></span> <span data-ttu-id="58076-117">Y, které máte hello brány na hello databázi hello hostitelů stejného virtuálního počítače nebo na samostatný virtuální počítač stejně dlouho jako hello brány můžete připojit toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="58076-117">Y You can have hello gateway on hello same VM that hosts hello database or on a separate VM as long as hello gateway can connect toohello database.</span></span>  

<span data-ttu-id="58076-118">Při instalaci brány hello se automaticky nainstaluje Microsoft Cassandra ODBC ovladače použít tooconnect tooCassandra databáze.</span><span class="sxs-lookup"><span data-stu-id="58076-118">When you install hello gateway, it automatically installs a Microsoft Cassandra ODBC driver used tooconnect tooCassandra database.</span></span> <span data-ttu-id="58076-119">Proto není nutné toomanually všechny ovladače nainstalovat na počítač brány hello při kopírování dat z databáze Cassandra hello.</span><span class="sxs-lookup"><span data-stu-id="58076-119">Therefore, you don't need toomanually install any driver on hello gateway machine when copying data from hello Cassandra database.</span></span> 

> [!NOTE]
> <span data-ttu-id="58076-120">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="58076-120">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="58076-121">Začínáme</span><span class="sxs-lookup"><span data-stu-id="58076-121">Getting started</span></span>
<span data-ttu-id="58076-122">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="58076-122">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="58076-123">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="58076-123">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="58076-124">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="58076-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="58076-125">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="58076-125">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="58076-126">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="58076-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="58076-127">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="58076-127">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="58076-128">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="58076-128">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="58076-129">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="58076-129">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="58076-130">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="58076-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="58076-131">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="58076-131">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="58076-132">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="58076-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="58076-133">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat Cassandra místní, naleznete v tématu [JSON příklad: kopírování dat z Cassandra tooAzure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="58076-133">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises Cassandra data store, see [JSON example: Copy data from Cassandra tooAzure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="58076-134">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa Cassandra úložiště dat:</span><span class="sxs-lookup"><span data-stu-id="58076-134">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Cassandra data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="58076-135">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="58076-135">Linked service properties</span></span>
<span data-ttu-id="58076-136">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooCassandra propojené služby.</span><span class="sxs-lookup"><span data-stu-id="58076-136">hello following table provides description for JSON elements specific tooCassandra linked service.</span></span>

| <span data-ttu-id="58076-137">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="58076-137">Property</span></span> | <span data-ttu-id="58076-138">Popis</span><span class="sxs-lookup"><span data-stu-id="58076-138">Description</span></span> | <span data-ttu-id="58076-139">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="58076-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="58076-140">type</span><span class="sxs-lookup"><span data-stu-id="58076-140">type</span></span> |<span data-ttu-id="58076-141">vlastnost typu Hello musí být nastavena na: **OnPremisesCassandra**</span><span class="sxs-lookup"><span data-stu-id="58076-141">hello type property must be set to: **OnPremisesCassandra**</span></span> |<span data-ttu-id="58076-142">Ano</span><span class="sxs-lookup"><span data-stu-id="58076-142">Yes</span></span> |
| <span data-ttu-id="58076-143">hostitele</span><span class="sxs-lookup"><span data-stu-id="58076-143">host</span></span> |<span data-ttu-id="58076-144">Jeden nebo více IP adres nebo názvů hostitelů Cassandra serverů.</span><span class="sxs-lookup"><span data-stu-id="58076-144">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="58076-145">Zadejte seznam IP adres nebo serverům tooall tooconnect názvy hostitele současně.</span><span class="sxs-lookup"><span data-stu-id="58076-145">Specify a comma-separated list of IP addresses or host names tooconnect tooall servers concurrently.</span></span> |<span data-ttu-id="58076-146">Ano</span><span class="sxs-lookup"><span data-stu-id="58076-146">Yes</span></span> |
| <span data-ttu-id="58076-147">port</span><span class="sxs-lookup"><span data-stu-id="58076-147">port</span></span> |<span data-ttu-id="58076-148">port TCP, který hello Cassandra server Hello používá toolisten pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="58076-148">hello TCP port that hello Cassandra server uses toolisten for client connections.</span></span> |<span data-ttu-id="58076-149">Ne, výchozí hodnota: 9042</span><span class="sxs-lookup"><span data-stu-id="58076-149">No, default value: 9042</span></span> |
| <span data-ttu-id="58076-150">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="58076-150">authenticationType</span></span> |<span data-ttu-id="58076-151">Basic nebo Anonymous</span><span class="sxs-lookup"><span data-stu-id="58076-151">Basic, or Anonymous</span></span> |<span data-ttu-id="58076-152">Ano</span><span class="sxs-lookup"><span data-stu-id="58076-152">Yes</span></span> |
| <span data-ttu-id="58076-153">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="58076-153">username</span></span> |<span data-ttu-id="58076-154">Zadejte uživatelské jméno pro hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="58076-154">Specify user name for hello user account.</span></span> |<span data-ttu-id="58076-155">Ano, pokud je nastavená authenticationType tooBasic.</span><span class="sxs-lookup"><span data-stu-id="58076-155">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="58076-156">heslo</span><span class="sxs-lookup"><span data-stu-id="58076-156">password</span></span> |<span data-ttu-id="58076-157">Zadejte heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="58076-157">Specify password for hello user account.</span></span> |<span data-ttu-id="58076-158">Ano, pokud je nastavená authenticationType tooBasic.</span><span class="sxs-lookup"><span data-stu-id="58076-158">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="58076-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="58076-159">gatewayName</span></span> |<span data-ttu-id="58076-160">Název Hello hello bránu, která je použité tooconnect toohello místní Cassandra databázi.</span><span class="sxs-lookup"><span data-stu-id="58076-160">hello name of hello gateway that is used tooconnect toohello on-premises Cassandra database.</span></span> |<span data-ttu-id="58076-161">Ano</span><span class="sxs-lookup"><span data-stu-id="58076-161">Yes</span></span> |
| <span data-ttu-id="58076-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="58076-162">encryptedCredential</span></span> |<span data-ttu-id="58076-163">Přihlašovací údaje šifrované bránou hello.</span><span class="sxs-lookup"><span data-stu-id="58076-163">Credential encrypted by hello gateway.</span></span> |<span data-ttu-id="58076-164">Ne</span><span class="sxs-lookup"><span data-stu-id="58076-164">No</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="58076-165">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="58076-165">Dataset properties</span></span>
<span data-ttu-id="58076-166">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="58076-166">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="58076-167">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="58076-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="58076-168">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="58076-168">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="58076-169">rámci typeProperties Hello část datové sady typ **CassandraTable** má následující vlastnosti hello</span><span class="sxs-lookup"><span data-stu-id="58076-169">hello typeProperties section for dataset of type **CassandraTable** has hello following properties</span></span>

| <span data-ttu-id="58076-170">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="58076-170">Property</span></span> | <span data-ttu-id="58076-171">Popis</span><span class="sxs-lookup"><span data-stu-id="58076-171">Description</span></span> | <span data-ttu-id="58076-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="58076-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="58076-173">keyspace</span><span class="sxs-lookup"><span data-stu-id="58076-173">keyspace</span></span> |<span data-ttu-id="58076-174">Název schématu v databázi Cassandra nebo hello keyspace.</span><span class="sxs-lookup"><span data-stu-id="58076-174">Name of hello keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="58076-175">Ano (Pokud **dotazu** pro **CassandraSource** není definován).</span><span class="sxs-lookup"><span data-stu-id="58076-175">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="58076-176">tableName</span><span class="sxs-lookup"><span data-stu-id="58076-176">tableName</span></span> |<span data-ttu-id="58076-177">Název hello tabulky v databázi Cassandra.</span><span class="sxs-lookup"><span data-stu-id="58076-177">Name of hello table in Cassandra database.</span></span> |<span data-ttu-id="58076-178">Ano (Pokud **dotazu** pro **CassandraSource** není definován).</span><span class="sxs-lookup"><span data-stu-id="58076-178">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="58076-179">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="58076-179">Copy activity properties</span></span>
<span data-ttu-id="58076-180">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="58076-180">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="58076-181">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="58076-181">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="58076-182">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="58076-182">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="58076-183">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="58076-183">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="58076-184">Pokud je zdroj typu **CassandraSource**, hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="58076-184">When source is of type **CassandraSource**, hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="58076-185">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="58076-185">Property</span></span> | <span data-ttu-id="58076-186">Popis</span><span class="sxs-lookup"><span data-stu-id="58076-186">Description</span></span> | <span data-ttu-id="58076-187">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="58076-187">Allowed values</span></span> | <span data-ttu-id="58076-188">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="58076-188">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="58076-189">query</span><span class="sxs-lookup"><span data-stu-id="58076-189">query</span></span> |<span data-ttu-id="58076-190">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="58076-190">Use hello custom query tooread data.</span></span> |<span data-ttu-id="58076-191">Dotaz SQL 92 nebo CQL dotazu.</span><span class="sxs-lookup"><span data-stu-id="58076-191">SQL-92 query or CQL query.</span></span> <span data-ttu-id="58076-192">V tématu [CQL odkaz](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="58076-192">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="58076-193">Při použití příkazu jazyka SQL, zadejte **keyspace name.table název** toorepresent hello tabulky, které mají být tooquery.</span><span class="sxs-lookup"><span data-stu-id="58076-193">When using SQL query, specify **keyspace name.table name** toorepresent hello table you want tooquery.</span></span> |<span data-ttu-id="58076-194">Ne (pokud jsou definovány tableName a keyspace v sadě dat).</span><span class="sxs-lookup"><span data-stu-id="58076-194">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="58076-195">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="58076-195">consistencyLevel</span></span> |<span data-ttu-id="58076-196">úroveň konzistence Hello Určuje, kolik repliky musí odpovídat požadavků na čtení tooa před vrácením dat toohello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="58076-196">hello consistency level specifies how many replicas must respond tooa read request before returning data toohello client application.</span></span> <span data-ttu-id="58076-197">Kontroly Cassandra hello zadaný počet replik pro data toosatisfy hello čtení požadavku.</span><span class="sxs-lookup"><span data-stu-id="58076-197">Cassandra checks hello specified number of replicas for data toosatisfy hello read request.</span></span> |<span data-ttu-id="58076-198">JEDEN, DVA, TŘI, KVORA, VŠE, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="58076-198">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="58076-199">V tématu [konfigurace konzistenci dat](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="58076-199">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="58076-200">Ne.</span><span class="sxs-lookup"><span data-stu-id="58076-200">No.</span></span> <span data-ttu-id="58076-201">Výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="58076-201">Default value is ONE.</span></span> |

## <a name="json-example-copy-data-from-cassandra-tooazure-blob"></a><span data-ttu-id="58076-202">Příklad JSON: kopírování dat z Cassandra tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="58076-202">JSON example: Copy data from Cassandra tooAzure Blob</span></span>
<span data-ttu-id="58076-203">Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="58076-203">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="58076-204">Ukazuje, jak toocopy data z Cassandra místní databázi tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="58076-204">It shows how toocopy data from an on-premises Cassandra database tooan Azure Blob Storage.</span></span> <span data-ttu-id="58076-205">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="58076-205">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58076-206">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="58076-206">This sample provides JSON snippets.</span></span> <span data-ttu-id="58076-207">Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="58076-207">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="58076-208">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="58076-208">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="58076-209">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="58076-209">hello sample has hello following data factory entities:</span></span>

* <span data-ttu-id="58076-210">Propojené služby typu [OnPremisesCassandra](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="58076-210">A linked service of type [OnPremisesCassandra](#linked-service-properties).</span></span>
* <span data-ttu-id="58076-211">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="58076-211">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="58076-212">Vstup [datovou sadu](data-factory-create-datasets.md) typu [CassandraTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="58076-212">An input [dataset](data-factory-create-datasets.md) of type [CassandraTable](#dataset-properties).</span></span>
* <span data-ttu-id="58076-213">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="58076-213">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="58076-214">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [CassandraSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="58076-214">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [CassandraSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="58076-215">**Cassandra propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="58076-215">**Cassandra linked service:**</span></span>

<span data-ttu-id="58076-216">Tento příklad používá hello **Cassandra** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="58076-216">This example uses hello **Cassandra** linked service.</span></span> <span data-ttu-id="58076-217">V tématu [Cassandra propojená služba](#linked-service-properties) části vlastností hello nepodporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="58076-217">See [Cassandra linked service](#linked-service-properties) section for hello properties supported by this linked service.</span></span>  

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

<span data-ttu-id="58076-218">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="58076-218">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="58076-219">**Cassandra vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="58076-219">**Cassandra input dataset:**</span></span>

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

<span data-ttu-id="58076-220">Nastavení **externí** příliš**true** informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="58076-220">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

<span data-ttu-id="58076-221">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="58076-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="58076-222">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="58076-222">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

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

<span data-ttu-id="58076-223">**Aktivita kopírování v kanálu s Cassandra zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="58076-223">**Copy activity in a pipeline with Cassandra source and Blob sink:**</span></span>

<span data-ttu-id="58076-224">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="58076-224">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="58076-225">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**CassandraSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="58076-225">In hello pipeline JSON definition, hello **source** type is set too**CassandraSource** and **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="58076-226">V tématu [vlastnosti typu RelationalSource](#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem hello RelationalSource.</span><span class="sxs-lookup"><span data-stu-id="58076-226">See [RelationalSource type properties](#copy-activity-properties) for hello list of properties supported by hello RelationalSource.</span></span>

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
            "description": "Copy from Cassandra tooan Azure blob",
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

### <a name="type-mapping-for-cassandra"></a><span data-ttu-id="58076-227">Mapování typu pro Cassandra</span><span class="sxs-lookup"><span data-stu-id="58076-227">Type mapping for Cassandra</span></span>
| <span data-ttu-id="58076-228">Typ Cassandra</span><span class="sxs-lookup"><span data-stu-id="58076-228">Cassandra Type</span></span> | <span data-ttu-id="58076-229">.NET na základě typu</span><span class="sxs-lookup"><span data-stu-id="58076-229">.Net Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="58076-230">ASCII</span><span class="sxs-lookup"><span data-stu-id="58076-230">ASCII</span></span> |<span data-ttu-id="58076-231">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58076-231">String</span></span> |
| <span data-ttu-id="58076-232">BIGINT</span><span class="sxs-lookup"><span data-stu-id="58076-232">BIGINT</span></span> |<span data-ttu-id="58076-233">Int64</span><span class="sxs-lookup"><span data-stu-id="58076-233">Int64</span></span> |
| <span data-ttu-id="58076-234">OBJEKT BLOB</span><span class="sxs-lookup"><span data-stu-id="58076-234">BLOB</span></span> |<span data-ttu-id="58076-235">Byte]</span><span class="sxs-lookup"><span data-stu-id="58076-235">Byte[]</span></span> |
| <span data-ttu-id="58076-236">LOGICKÁ HODNOTA</span><span class="sxs-lookup"><span data-stu-id="58076-236">BOOLEAN</span></span> |<span data-ttu-id="58076-237">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="58076-237">Boolean</span></span> |
| <span data-ttu-id="58076-238">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="58076-238">DECIMAL</span></span> |<span data-ttu-id="58076-239">Decimal</span><span class="sxs-lookup"><span data-stu-id="58076-239">Decimal</span></span> |
| <span data-ttu-id="58076-240">DOUBLE</span><span class="sxs-lookup"><span data-stu-id="58076-240">DOUBLE</span></span> |<span data-ttu-id="58076-241">Double</span><span class="sxs-lookup"><span data-stu-id="58076-241">Double</span></span> |
| <span data-ttu-id="58076-242">PLOVOUCÍ DESETINNÁ ČÁRKA</span><span class="sxs-lookup"><span data-stu-id="58076-242">FLOAT</span></span> |<span data-ttu-id="58076-243">Jeden</span><span class="sxs-lookup"><span data-stu-id="58076-243">Single</span></span> |
| <span data-ttu-id="58076-244">INET</span><span class="sxs-lookup"><span data-stu-id="58076-244">INET</span></span> |<span data-ttu-id="58076-245">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58076-245">String</span></span> |
| <span data-ttu-id="58076-246">INT</span><span class="sxs-lookup"><span data-stu-id="58076-246">INT</span></span> |<span data-ttu-id="58076-247">Int32</span><span class="sxs-lookup"><span data-stu-id="58076-247">Int32</span></span> |
| <span data-ttu-id="58076-248">TEXT</span><span class="sxs-lookup"><span data-stu-id="58076-248">TEXT</span></span> |<span data-ttu-id="58076-249">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58076-249">String</span></span> |
| <span data-ttu-id="58076-250">ČASOVÉ RAZÍTKO</span><span class="sxs-lookup"><span data-stu-id="58076-250">TIMESTAMP</span></span> |<span data-ttu-id="58076-251">Data a času</span><span class="sxs-lookup"><span data-stu-id="58076-251">DateTime</span></span> |
| <span data-ttu-id="58076-252">TIMEUUID</span><span class="sxs-lookup"><span data-stu-id="58076-252">TIMEUUID</span></span> |<span data-ttu-id="58076-253">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="58076-253">Guid</span></span> |
| <span data-ttu-id="58076-254">UUID</span><span class="sxs-lookup"><span data-stu-id="58076-254">UUID</span></span> |<span data-ttu-id="58076-255">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="58076-255">Guid</span></span> |
| <span data-ttu-id="58076-256">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="58076-256">VARCHAR</span></span> |<span data-ttu-id="58076-257">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58076-257">String</span></span> |
| <span data-ttu-id="58076-258">VARINT</span><span class="sxs-lookup"><span data-stu-id="58076-258">VARINT</span></span> |<span data-ttu-id="58076-259">Decimal</span><span class="sxs-lookup"><span data-stu-id="58076-259">Decimal</span></span> |

> [!NOTE]
> <span data-ttu-id="58076-260">Kolekce typů (mapy, sady, seznamu atd.), najdete v části příliš[pracovat s typy kolekcí Cassandra pomocí virtuální tabulky](#work-with-collections-using-virtual-table) části.</span><span class="sxs-lookup"><span data-stu-id="58076-260">For collection types (map, set, list, etc.), refer too[Work with Cassandra collection types using virtual table](#work-with-collections-using-virtual-table) section.</span></span>
>
> <span data-ttu-id="58076-261">Uživatelem definované typy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="58076-261">User-defined types are not supported.</span></span>
>
> <span data-ttu-id="58076-262">Hello délka binárního sloupce a sloupce, řetězec délky nemůže být větší než 4000.</span><span class="sxs-lookup"><span data-stu-id="58076-262">hello length of Binary Column and String Column lengths cannot be greater than 4000.</span></span>
>
>

## <a name="work-with-collections-using-virtual-table"></a><span data-ttu-id="58076-263">Práce s kolekcí pomocí virtuální tabulky</span><span class="sxs-lookup"><span data-stu-id="58076-263">Work with collections using virtual table</span></span>
<span data-ttu-id="58076-264">Azure Data Factory používá integrované ODBC ovladač tooconnect tooand kopírování dat z databáze Cassandra.</span><span class="sxs-lookup"><span data-stu-id="58076-264">Azure Data Factory uses a built-in ODBC driver tooconnect tooand copy data from your Cassandra database.</span></span> <span data-ttu-id="58076-265">Pro typy kolekcí včetně mapy, sady a seznamem hello ovladač renormalizes hello data do odpovídající virtuální tabulky.</span><span class="sxs-lookup"><span data-stu-id="58076-265">For collection types including map, set and list, hello driver renormalizes hello data into corresponding virtual tables.</span></span> <span data-ttu-id="58076-266">Konkrétně Pokud tabulka obsahuje všechny sloupce, kolekce, hello ovladač generuje hello následující virtuální tabulky:</span><span class="sxs-lookup"><span data-stu-id="58076-266">Specifically, if a table contains any collection columns, hello driver generates hello following virtual tables:</span></span>

* <span data-ttu-id="58076-267">A **základní tabulka**, který obsahuje hello stejná data jako hello skutečné tabulky s výjimkou hello kolekce sloupců.</span><span class="sxs-lookup"><span data-stu-id="58076-267">A **base table**, which contains hello same data as hello real table except for hello collection columns.</span></span> <span data-ttu-id="58076-268">Základní tabulka Hello používá hello stejný název jako hello skutečné tabulku, která reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="58076-268">hello base table uses hello same name as hello real table that it represents.</span></span>
* <span data-ttu-id="58076-269">A **virtuální tabulku** pro každý sloupec kolekce, které rozšíří hello vnořená data.</span><span class="sxs-lookup"><span data-stu-id="58076-269">A **virtual table** for each collection column, which expands hello nested data.</span></span> <span data-ttu-id="58076-270">Hello virtuální tabulky, které představují kolekce jsou pojmenované pomocí názvu hello hello skutečné tabulky, oddělovače "*vt*" a hello název sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="58076-270">hello virtual tables that represent collections are named using hello name of hello real table, a separator “*vt*” and hello name of hello column.</span></span>

<span data-ttu-id="58076-271">Virtuální tabulky odkazovat toohello data v tabulce skutečné hello, povolení tooaccess ovladač hello hello nenormalizované data.</span><span class="sxs-lookup"><span data-stu-id="58076-271">Virtual tables refer toohello data in hello real table, enabling hello driver tooaccess hello denormalized data.</span></span> <span data-ttu-id="58076-272">Příklad naleznete v části Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="58076-272">See Example section for details.</span></span> <span data-ttu-id="58076-273">Obsah hello Cassandra kolekcí můžete přejít pomocí dotazování a spojování tabulek virtuálním hello.</span><span class="sxs-lookup"><span data-stu-id="58076-273">You can access hello content of Cassandra collections by querying and joining hello virtual tables.</span></span>

<span data-ttu-id="58076-274">Můžete použít hello [Průvodce kopírováním](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively zobrazení hello seznam tabulek v databázi Cassandra včetně hello virtuální tabulky a zobrazení náhledu obsažená data hello.</span><span class="sxs-lookup"><span data-stu-id="58076-274">You can use hello [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively view hello list of tables in Cassandra database including hello virtual tables, and preview hello data inside.</span></span> <span data-ttu-id="58076-275">Můžete také vytvořit dotaz v hello Průvodce kopírováním a ověřit toosee hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="58076-275">You can also construct a query in hello Copy Wizard and validate toosee hello result.</span></span>

### <a name="example"></a><span data-ttu-id="58076-276">Příklad</span><span class="sxs-lookup"><span data-stu-id="58076-276">Example</span></span>
<span data-ttu-id="58076-277">Například hello následující "ExampleTable" je Cassandra tabulku databáze, která obsahuje celé číslo primární klíče sloupec s názvem "pk_int", o textový sloupec s názvem hodnotu, sloupce seznamu, mapy sloupec a sadu sloupec (s názvem "StringSet").</span><span class="sxs-lookup"><span data-stu-id="58076-277">For example, hello following “ExampleTable” is a Cassandra database table that contains an integer primary key column named “pk_int”, a text column named value, a list column, a map column, and a set column (named “StringSet”).</span></span>

| <span data-ttu-id="58076-278">pk_int</span><span class="sxs-lookup"><span data-stu-id="58076-278">pk_int</span></span> | <span data-ttu-id="58076-279">Hodnota</span><span class="sxs-lookup"><span data-stu-id="58076-279">Value</span></span> | <span data-ttu-id="58076-280">Seznam</span><span class="sxs-lookup"><span data-stu-id="58076-280">List</span></span> | <span data-ttu-id="58076-281">mapy</span><span class="sxs-lookup"><span data-stu-id="58076-281">Map</span></span> | <span data-ttu-id="58076-282">StringSet</span><span class="sxs-lookup"><span data-stu-id="58076-282">StringSet</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="58076-283">1</span><span class="sxs-lookup"><span data-stu-id="58076-283">1</span></span> |<span data-ttu-id="58076-284">"Ukázková hodnota 1"</span><span class="sxs-lookup"><span data-stu-id="58076-284">"sample value 1"</span></span> |<span data-ttu-id="58076-285">["1", "2", "3"]</span><span class="sxs-lookup"><span data-stu-id="58076-285">["1", "2", "3"]</span></span> |<span data-ttu-id="58076-286">{"S1": "a", "S2": "b"}</span><span class="sxs-lookup"><span data-stu-id="58076-286">{"S1": "a", "S2": "b"}</span></span> |<span data-ttu-id="58076-287">{"A", "B", "C"}</span><span class="sxs-lookup"><span data-stu-id="58076-287">{"A", "B", "C"}</span></span> |
| <span data-ttu-id="58076-288">3</span><span class="sxs-lookup"><span data-stu-id="58076-288">3</span></span> |<span data-ttu-id="58076-289">"Ukázková hodnota 3"</span><span class="sxs-lookup"><span data-stu-id="58076-289">"sample value 3"</span></span> |<span data-ttu-id="58076-290">["100", "101", "102", "105"]</span><span class="sxs-lookup"><span data-stu-id="58076-290">["100", "101", "102", "105"]</span></span> |<span data-ttu-id="58076-291">{"S1": "t"}</span><span class="sxs-lookup"><span data-stu-id="58076-291">{"S1": "t"}</span></span> |<span data-ttu-id="58076-292">{"A", "E"}</span><span class="sxs-lookup"><span data-stu-id="58076-292">{"A", "E"}</span></span> |

<span data-ttu-id="58076-293">ovladač Hello by vygeneroval více toorepresent virtuální tabulky tento jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="58076-293">hello driver would generate multiple virtual tables toorepresent this single table.</span></span> <span data-ttu-id="58076-294">Hello sloupce cizích klíčů v tabulkách virtuálním hello odkazovat hello primární klíčových sloupců v tabulce skutečné hello a indikují které reálné odpovídá řádku virtuální tabulku hello řádek tabulky.</span><span class="sxs-lookup"><span data-stu-id="58076-294">hello foreign key columns in hello virtual tables reference hello primary key columns in hello real table, and indicate which real table row hello virtual table row corresponds to.</span></span>

<span data-ttu-id="58076-295">první virtuální tabulky Hello je hello základní tabulka s názvem "ExampleTable" se zobrazí v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="58076-295">hello first virtual table is hello base table named “ExampleTable” is shown in hello following table.</span></span> <span data-ttu-id="58076-296">Hello základní tabulka obsahuje hello stejná data jako hello původní tabulky databáze s výjimkou hello kolekce, které jsou vynechání z této tabulky a rozšířit ostatní virtuální tabulky.</span><span class="sxs-lookup"><span data-stu-id="58076-296">hello base table contains hello same data as hello original database table except for hello collections, which are omitted from this table and expanded in other virtual tables.</span></span>

| <span data-ttu-id="58076-297">pk_int</span><span class="sxs-lookup"><span data-stu-id="58076-297">pk_int</span></span> | <span data-ttu-id="58076-298">Hodnota</span><span class="sxs-lookup"><span data-stu-id="58076-298">Value</span></span> |
| --- | --- |
| <span data-ttu-id="58076-299">1</span><span class="sxs-lookup"><span data-stu-id="58076-299">1</span></span> |<span data-ttu-id="58076-300">"Ukázková hodnota 1"</span><span class="sxs-lookup"><span data-stu-id="58076-300">"sample value 1"</span></span> |
| <span data-ttu-id="58076-301">3</span><span class="sxs-lookup"><span data-stu-id="58076-301">3</span></span> |<span data-ttu-id="58076-302">"Ukázková hodnota 3"</span><span class="sxs-lookup"><span data-stu-id="58076-302">"sample value 3"</span></span> |

<span data-ttu-id="58076-303">Hello následující tabulky popisují hello virtuální tabulky, které renormalize hello data ze seznamu a mapy, StringSet sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="58076-303">hello following tables show hello virtual tables that renormalize hello data from hello List, Map, and StringSet columns.</span></span> <span data-ttu-id="58076-304">Hello sloupce s názvy, které končí "_index" nebo "_key" označuje pozici hello hello dat v rámci původní seznam hello nebo mapy.</span><span class="sxs-lookup"><span data-stu-id="58076-304">hello columns with names that end with “_index” or “_key” indicate hello position of hello data within hello original list or map.</span></span> <span data-ttu-id="58076-305">Hello sloupce s názvy, které končí "_value" obsahují hello rozšířit data z kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="58076-305">hello columns with names that end with “_value” contain hello expanded data from hello collection.</span></span>

#### <a name="table-exampletablevtlist"></a><span data-ttu-id="58076-306">Tabulka "ExampleTable_vt_List":</span><span class="sxs-lookup"><span data-stu-id="58076-306">Table “ExampleTable_vt_List”:</span></span>
| <span data-ttu-id="58076-307">pk_int</span><span class="sxs-lookup"><span data-stu-id="58076-307">pk_int</span></span> | <span data-ttu-id="58076-308">List_index</span><span class="sxs-lookup"><span data-stu-id="58076-308">List_index</span></span> | <span data-ttu-id="58076-309">List_value</span><span class="sxs-lookup"><span data-stu-id="58076-309">List_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="58076-310">1</span><span class="sxs-lookup"><span data-stu-id="58076-310">1</span></span> |<span data-ttu-id="58076-311">0</span><span class="sxs-lookup"><span data-stu-id="58076-311">0</span></span> |<span data-ttu-id="58076-312">1</span><span class="sxs-lookup"><span data-stu-id="58076-312">1</span></span> |
| <span data-ttu-id="58076-313">1</span><span class="sxs-lookup"><span data-stu-id="58076-313">1</span></span> |<span data-ttu-id="58076-314">1</span><span class="sxs-lookup"><span data-stu-id="58076-314">1</span></span> |<span data-ttu-id="58076-315">2</span><span class="sxs-lookup"><span data-stu-id="58076-315">2</span></span> |
| <span data-ttu-id="58076-316">1</span><span class="sxs-lookup"><span data-stu-id="58076-316">1</span></span> |<span data-ttu-id="58076-317">2</span><span class="sxs-lookup"><span data-stu-id="58076-317">2</span></span> |<span data-ttu-id="58076-318">3</span><span class="sxs-lookup"><span data-stu-id="58076-318">3</span></span> |
| <span data-ttu-id="58076-319">3</span><span class="sxs-lookup"><span data-stu-id="58076-319">3</span></span> |<span data-ttu-id="58076-320">0</span><span class="sxs-lookup"><span data-stu-id="58076-320">0</span></span> |<span data-ttu-id="58076-321">100</span><span class="sxs-lookup"><span data-stu-id="58076-321">100</span></span> |
| <span data-ttu-id="58076-322">3</span><span class="sxs-lookup"><span data-stu-id="58076-322">3</span></span> |<span data-ttu-id="58076-323">1</span><span class="sxs-lookup"><span data-stu-id="58076-323">1</span></span> |<span data-ttu-id="58076-324">101</span><span class="sxs-lookup"><span data-stu-id="58076-324">101</span></span> |
| <span data-ttu-id="58076-325">3</span><span class="sxs-lookup"><span data-stu-id="58076-325">3</span></span> |<span data-ttu-id="58076-326">2</span><span class="sxs-lookup"><span data-stu-id="58076-326">2</span></span> |<span data-ttu-id="58076-327">102</span><span class="sxs-lookup"><span data-stu-id="58076-327">102</span></span> |
| <span data-ttu-id="58076-328">3</span><span class="sxs-lookup"><span data-stu-id="58076-328">3</span></span> |<span data-ttu-id="58076-329">3</span><span class="sxs-lookup"><span data-stu-id="58076-329">3</span></span> |<span data-ttu-id="58076-330">103</span><span class="sxs-lookup"><span data-stu-id="58076-330">103</span></span> |

#### <a name="table-exampletablevtmap"></a><span data-ttu-id="58076-331">Tabulka "ExampleTable_vt_Map":</span><span class="sxs-lookup"><span data-stu-id="58076-331">Table “ExampleTable_vt_Map”:</span></span>
| <span data-ttu-id="58076-332">pk_int</span><span class="sxs-lookup"><span data-stu-id="58076-332">pk_int</span></span> | <span data-ttu-id="58076-333">Map_key</span><span class="sxs-lookup"><span data-stu-id="58076-333">Map_key</span></span> | <span data-ttu-id="58076-334">Map_value</span><span class="sxs-lookup"><span data-stu-id="58076-334">Map_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="58076-335">1</span><span class="sxs-lookup"><span data-stu-id="58076-335">1</span></span> |<span data-ttu-id="58076-336">S1</span><span class="sxs-lookup"><span data-stu-id="58076-336">S1</span></span> |<span data-ttu-id="58076-337">A</span><span class="sxs-lookup"><span data-stu-id="58076-337">A</span></span> |
| <span data-ttu-id="58076-338">1</span><span class="sxs-lookup"><span data-stu-id="58076-338">1</span></span> |<span data-ttu-id="58076-339">S2</span><span class="sxs-lookup"><span data-stu-id="58076-339">S2</span></span> |<span data-ttu-id="58076-340">B</span><span class="sxs-lookup"><span data-stu-id="58076-340">b</span></span> |
| <span data-ttu-id="58076-341">3</span><span class="sxs-lookup"><span data-stu-id="58076-341">3</span></span> |<span data-ttu-id="58076-342">S1</span><span class="sxs-lookup"><span data-stu-id="58076-342">S1</span></span> |<span data-ttu-id="58076-343">T</span><span class="sxs-lookup"><span data-stu-id="58076-343">t</span></span> |

#### <a name="table-exampletablevtstringset"></a><span data-ttu-id="58076-344">Tabulka "ExampleTable_vt_StringSet":</span><span class="sxs-lookup"><span data-stu-id="58076-344">Table “ExampleTable_vt_StringSet”:</span></span>
| <span data-ttu-id="58076-345">pk_int</span><span class="sxs-lookup"><span data-stu-id="58076-345">pk_int</span></span> | <span data-ttu-id="58076-346">StringSet_value</span><span class="sxs-lookup"><span data-stu-id="58076-346">StringSet_value</span></span> |
| --- | --- |
| <span data-ttu-id="58076-347">1</span><span class="sxs-lookup"><span data-stu-id="58076-347">1</span></span> |<span data-ttu-id="58076-348">A</span><span class="sxs-lookup"><span data-stu-id="58076-348">A</span></span> |
| <span data-ttu-id="58076-349">1</span><span class="sxs-lookup"><span data-stu-id="58076-349">1</span></span> |<span data-ttu-id="58076-350">B</span><span class="sxs-lookup"><span data-stu-id="58076-350">B</span></span> |
| <span data-ttu-id="58076-351">1</span><span class="sxs-lookup"><span data-stu-id="58076-351">1</span></span> |<span data-ttu-id="58076-352">C</span><span class="sxs-lookup"><span data-stu-id="58076-352">C</span></span> |
| <span data-ttu-id="58076-353">3</span><span class="sxs-lookup"><span data-stu-id="58076-353">3</span></span> |<span data-ttu-id="58076-354">A</span><span class="sxs-lookup"><span data-stu-id="58076-354">A</span></span> |
| <span data-ttu-id="58076-355">3</span><span class="sxs-lookup"><span data-stu-id="58076-355">3</span></span> |<span data-ttu-id="58076-356">E</span><span class="sxs-lookup"><span data-stu-id="58076-356">E</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="58076-357">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="58076-357">Map source toosink columns</span></span>
<span data-ttu-id="58076-358">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="58076-358">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="58076-359">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="58076-359">Repeatable read from relational sources</span></span>
<span data-ttu-id="58076-360">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="58076-360">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="58076-361">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="58076-361">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="58076-362">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="58076-362">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="58076-363">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="58076-363">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="58076-364">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="58076-364">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="58076-365">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="58076-365">Performance and Tuning</span></span>
<span data-ttu-id="58076-366">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="58076-366">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
