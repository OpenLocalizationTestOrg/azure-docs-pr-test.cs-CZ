---
title: "Přesun dat z databáze MySQL pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z databáze MySQL pomocí Azure Data Factory."
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
ms.openlocfilehash: 05159bfd98977d0b57b43fbc02e4579439f7ce4c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a><span data-ttu-id="00321-103">Přesun dat pomocí Azure Data Factory z databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="00321-103">Move data From MySQL using Azure Data Factory</span></span>
<span data-ttu-id="00321-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z databáze MySQL na místě.</span><span class="sxs-lookup"><span data-stu-id="00321-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises MySQL database.</span></span> <span data-ttu-id="00321-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="00321-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="00321-106">Z úložiště dat MySQL místní může kopírovat data do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="00321-106">You can copy data from an on-premises MySQL data store to any supported sink data store.</span></span> <span data-ttu-id="00321-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="00321-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="00321-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z jiného úložiště dat MySQL jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat k úložišti dat MySQL.</span><span class="sxs-lookup"><span data-stu-id="00321-108">Data factory currently supports only moving data from a MySQL data store to other data stores, but not for moving data from other data stores to an MySQL data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="00321-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="00321-109">Prerequisites</span></span>
<span data-ttu-id="00321-110">Služba data Factory podporuje připojení k místním zdrojům MySQL pomocí Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="00321-110">Data Factory service supports connecting to on-premises MySQL sources using the Data Management Gateway.</span></span> <span data-ttu-id="00321-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku se dozvíte o Brána pro správu dat a podrobné pokyny o nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="00321-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="00321-112">Vyžaduje se brána, i když je databáze MySQL hostované ve virtuálním počítači Azure IaaS (VM).</span><span class="sxs-lookup"><span data-stu-id="00321-112">Gateway is required even if the MySQL database is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="00321-113">Bránu můžete nainstalovat na stejný virtuální počítač jako úložiště dat nebo na jiný virtuální počítač, dokud brána se může připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="00321-113">You can install the gateway on the same VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="00321-114">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="00321-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="00321-115">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="00321-115">Supported versions and installation</span></span>
<span data-ttu-id="00321-116">Pro bránu pro správu dat pro připojení k databázi MySQL, je potřeba nainstalovat [MySQL Connector/Net pro Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (verze 6.6.5 nebo vyšší) ve stejném systému jako brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="00321-116">For Data Management Gateway to connect to the MySQL Database, you need to install the [MySQL Connector/Net for Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (version 6.6.5 or above) on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="00321-117">MySQL verze 5.1 a vyšší je podporovaná.</span><span class="sxs-lookup"><span data-stu-id="00321-117">MySQL version 5.1 and above is supported.</span></span>

> [!TIP]
> <span data-ttu-id="00321-118">Jestli jste nedosáhli chyba "Ověřování se nezdařilo, protože je uzavřený vzdálené strany přenosu datového proudu.", zvažte MySQL Connector/Net upgradu na vyšší verzi.</span><span class="sxs-lookup"><span data-stu-id="00321-118">If you hit error on "Authentication failed because the remote party has closed the transport stream.", consider to upgrade the MySQL Connector/Net to higher version.</span></span>

## <a name="getting-started"></a><span data-ttu-id="00321-119">Začínáme</span><span class="sxs-lookup"><span data-stu-id="00321-119">Getting started</span></span>
<span data-ttu-id="00321-120">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="00321-120">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="00321-121">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="00321-121">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="00321-122">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="00321-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="00321-123">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="00321-123">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="00321-124">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="00321-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="00321-125">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="00321-125">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="00321-126">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="00321-126">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="00321-127">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="00321-127">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="00321-128">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="00321-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="00321-129">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="00321-129">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="00321-130">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="00321-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="00321-131">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat místní MySQL, naleznete v tématu [JSON příklad: kopírování dat z databáze MySQL do Azure Blob](#json-example-copy-data-from-mysql-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="00321-131">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises MySQL data store, see [JSON example: Copy data from MySQL to Azure Blob](#json-example-copy-data-from-mysql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="00321-132">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k úložišti dat MySQL:</span><span class="sxs-lookup"><span data-stu-id="00321-132">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a MySQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="00321-133">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="00321-133">Linked service properties</span></span>
<span data-ttu-id="00321-134">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro službu MySQL propojený.</span><span class="sxs-lookup"><span data-stu-id="00321-134">The following table provides description for JSON elements specific to MySQL linked service.</span></span>

| <span data-ttu-id="00321-135">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="00321-135">Property</span></span> | <span data-ttu-id="00321-136">Popis</span><span class="sxs-lookup"><span data-stu-id="00321-136">Description</span></span> | <span data-ttu-id="00321-137">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="00321-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="00321-138">type</span><span class="sxs-lookup"><span data-stu-id="00321-138">type</span></span> |<span data-ttu-id="00321-139">Vlastnost typu musí být nastavena na: **OnPremisesMySql**</span><span class="sxs-lookup"><span data-stu-id="00321-139">The type property must be set to: **OnPremisesMySql**</span></span> |<span data-ttu-id="00321-140">Ano</span><span class="sxs-lookup"><span data-stu-id="00321-140">Yes</span></span> |
| <span data-ttu-id="00321-141">server</span><span class="sxs-lookup"><span data-stu-id="00321-141">server</span></span> |<span data-ttu-id="00321-142">Název serveru databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="00321-142">Name of the MySQL server.</span></span> |<span data-ttu-id="00321-143">Ano</span><span class="sxs-lookup"><span data-stu-id="00321-143">Yes</span></span> |
| <span data-ttu-id="00321-144">Databáze</span><span class="sxs-lookup"><span data-stu-id="00321-144">database</span></span> |<span data-ttu-id="00321-145">Název databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="00321-145">Name of the MySQL database.</span></span> |<span data-ttu-id="00321-146">Ano</span><span class="sxs-lookup"><span data-stu-id="00321-146">Yes</span></span> |
| <span data-ttu-id="00321-147">Schéma</span><span class="sxs-lookup"><span data-stu-id="00321-147">schema</span></span> |<span data-ttu-id="00321-148">Název schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="00321-148">Name of the schema in the database.</span></span> |<span data-ttu-id="00321-149">Ne</span><span class="sxs-lookup"><span data-stu-id="00321-149">No</span></span> |
| <span data-ttu-id="00321-150">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="00321-150">authenticationType</span></span> |<span data-ttu-id="00321-151">Typ ověřování používaný pro připojení k databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="00321-151">Type of authentication used to connect to the MySQL database.</span></span> <span data-ttu-id="00321-152">Možné hodnoty jsou: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="00321-152">Possible values are: `Basic`.</span></span> |<span data-ttu-id="00321-153">Ano</span><span class="sxs-lookup"><span data-stu-id="00321-153">Yes</span></span> |
| <span data-ttu-id="00321-154">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="00321-154">username</span></span> |<span data-ttu-id="00321-155">Zadejte uživatelské jméno pro připojení k databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="00321-155">Specify user name to connect to the MySQL database.</span></span> |<span data-ttu-id="00321-156">Ano</span><span class="sxs-lookup"><span data-stu-id="00321-156">Yes</span></span> |
| <span data-ttu-id="00321-157">heslo</span><span class="sxs-lookup"><span data-stu-id="00321-157">password</span></span> |<span data-ttu-id="00321-158">Zadejte heslo pro uživatelský účet, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="00321-158">Specify password for the user account you specified.</span></span> |<span data-ttu-id="00321-159">Ano</span><span class="sxs-lookup"><span data-stu-id="00321-159">Yes</span></span> |
| <span data-ttu-id="00321-160">gatewayName</span><span class="sxs-lookup"><span data-stu-id="00321-160">gatewayName</span></span> |<span data-ttu-id="00321-161">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="00321-161">Name of the gateway that the Data Factory service should use to connect to the on-premises MySQL database.</span></span> |<span data-ttu-id="00321-162">Ano</span><span class="sxs-lookup"><span data-stu-id="00321-162">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="00321-163">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="00321-163">Dataset properties</span></span>
<span data-ttu-id="00321-164">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="00321-164">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="00321-165">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="00321-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="00321-166">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="00321-166">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="00321-167">Rámci typeProperties část datové sady typ **RelationalTable** (což zahrnuje datová sada MySQL) má následující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="00321-167">The typeProperties section for dataset of type **RelationalTable** (which includes MySQL dataset) has the following properties</span></span>

| <span data-ttu-id="00321-168">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="00321-168">Property</span></span> | <span data-ttu-id="00321-169">Popis</span><span class="sxs-lookup"><span data-stu-id="00321-169">Description</span></span> | <span data-ttu-id="00321-170">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="00321-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="00321-171">tableName</span><span class="sxs-lookup"><span data-stu-id="00321-171">tableName</span></span> |<span data-ttu-id="00321-172">Název tabulky instance databáze MySQL na kterou odkazuje propojená služba.</span><span class="sxs-lookup"><span data-stu-id="00321-172">Name of the table in the MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="00321-173">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="00321-173">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="00321-174">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="00321-174">Copy activity properties</span></span>
<span data-ttu-id="00321-175">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="00321-175">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="00321-176">Vlastnosti, například název, popis, vstupní a výstupní tabulky, jsou zásady jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="00321-176">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="00321-177">Vzhledem k tomu, vlastnosti dostupné ve **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="00321-177">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="00321-178">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="00321-178">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="00321-179">Pokud je zdroj v aktivitě kopírování typu **RelationalSource** (která zahrnuje MySQL), následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="00321-179">When source in copy activity is of type **RelationalSource** (which includes MySQL), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="00321-180">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="00321-180">Property</span></span> | <span data-ttu-id="00321-181">Popis</span><span class="sxs-lookup"><span data-stu-id="00321-181">Description</span></span> | <span data-ttu-id="00321-182">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="00321-182">Allowed values</span></span> | <span data-ttu-id="00321-183">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="00321-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="00321-184">query</span><span class="sxs-lookup"><span data-stu-id="00321-184">query</span></span> |<span data-ttu-id="00321-185">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="00321-185">Use the custom query to read data.</span></span> |<span data-ttu-id="00321-186">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="00321-186">SQL query string.</span></span> <span data-ttu-id="00321-187">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="00321-187">For example: select * from MyTable.</span></span> |<span data-ttu-id="00321-188">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="00321-188">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-mysql-to-azure-blob"></a><span data-ttu-id="00321-189">Příklad JSON: kopírování dat z databáze MySQL do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="00321-189">JSON example: Copy data from MySQL to Azure Blob</span></span>
<span data-ttu-id="00321-190">Tento příklad obsahuje ukázkové JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="00321-190">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="00321-191">Ukazuje, jak zkopírovat data z místní databáze MySQL do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="00321-191">It shows how to copy data from an on-premises MySQL database to an Azure Blob Storage.</span></span> <span data-ttu-id="00321-192">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="00321-192">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00321-193">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="00321-193">This sample provides JSON snippets.</span></span> <span data-ttu-id="00321-194">Podrobné pokyny pro vytvoření objektu pro vytváření dat neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="00321-194">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="00321-195">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="00321-195">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="00321-196">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="00321-196">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="00321-197">Propojené služby typu [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="00321-197">A linked service of type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="00321-198">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="00321-198">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="00321-199">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="00321-199">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="00321-200">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="00321-200">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="00321-201">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="00321-201">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="00321-202">Ukázka zkopíruje data z výsledku dotazu v databázi MySQL do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="00321-202">The sample copies data from a query result in MySQL database to a blob hourly.</span></span> <span data-ttu-id="00321-203">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="00321-203">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="00321-204">Jako první krok nastavte Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="00321-204">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="00321-205">Tyto pokyny jsou v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="00321-205">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="00321-206">**MySQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="00321-206">**MySQL linked service:**</span></span>

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

<span data-ttu-id="00321-207">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="00321-207">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="00321-208">**MySQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="00321-208">**MySQL input dataset:**</span></span>

<span data-ttu-id="00321-209">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v MySQL a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="00321-209">The sample assumes you have created a table “MyTable” in MySQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="00321-210">Nastavení "externí": "PRAVDA" informuje služba Data Factory, v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="00321-210">Setting “external”: ”true” informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="00321-211">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="00321-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="00321-212">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="00321-212">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="00321-213">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="00321-213">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="00321-214">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="00321-214">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="00321-215">**Kanál s aktivitou kopírování:**</span><span class="sxs-lookup"><span data-stu-id="00321-215">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="00321-216">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="00321-216">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="00321-217">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="00321-217">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="00321-218">Zadané pro dotaz SQL **dotazu** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="00321-218">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

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


### <a name="type-mapping-for-mysql"></a><span data-ttu-id="00321-219">Mapování typu pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="00321-219">Type mapping for MySQL</span></span>
<span data-ttu-id="00321-220">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup ve dvou krocích:</span><span class="sxs-lookup"><span data-stu-id="00321-220">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="00321-221">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="00321-221">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="00321-222">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="00321-222">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="00321-223">Při přesunu dat do databáze MySQL, se používají následující mapování z typů MySQL na typy .NET.</span><span class="sxs-lookup"><span data-stu-id="00321-223">When moving data to MySQL, the following mappings are used from MySQL types to .NET types.</span></span>

| <span data-ttu-id="00321-224">Typ databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="00321-224">MySQL Database type</span></span> | <span data-ttu-id="00321-225">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="00321-225">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="00321-226">bigint bez znaménka</span><span class="sxs-lookup"><span data-stu-id="00321-226">bigint unsigned</span></span> |<span data-ttu-id="00321-227">Decimal</span><span class="sxs-lookup"><span data-stu-id="00321-227">Decimal</span></span> |
| <span data-ttu-id="00321-228">bigint</span><span class="sxs-lookup"><span data-stu-id="00321-228">bigint</span></span> |<span data-ttu-id="00321-229">Int64</span><span class="sxs-lookup"><span data-stu-id="00321-229">Int64</span></span> |
| <span data-ttu-id="00321-230">Bit</span><span class="sxs-lookup"><span data-stu-id="00321-230">bit</span></span> |<span data-ttu-id="00321-231">Decimal</span><span class="sxs-lookup"><span data-stu-id="00321-231">Decimal</span></span> |
| <span data-ttu-id="00321-232">Objekt BLOB</span><span class="sxs-lookup"><span data-stu-id="00321-232">blob</span></span> |<span data-ttu-id="00321-233">Byte]</span><span class="sxs-lookup"><span data-stu-id="00321-233">Byte[]</span></span> |
| <span data-ttu-id="00321-234">BOOL</span><span class="sxs-lookup"><span data-stu-id="00321-234">bool</span></span> |<span data-ttu-id="00321-235">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="00321-235">Boolean</span></span> |
| <span data-ttu-id="00321-236">Char</span><span class="sxs-lookup"><span data-stu-id="00321-236">char</span></span> |<span data-ttu-id="00321-237">Řetězec</span><span class="sxs-lookup"><span data-stu-id="00321-237">String</span></span> |
| <span data-ttu-id="00321-238">Datum</span><span class="sxs-lookup"><span data-stu-id="00321-238">date</span></span> |<span data-ttu-id="00321-239">Data a času</span><span class="sxs-lookup"><span data-stu-id="00321-239">Datetime</span></span> |
| <span data-ttu-id="00321-240">Data a času</span><span class="sxs-lookup"><span data-stu-id="00321-240">datetime</span></span> |<span data-ttu-id="00321-241">Data a času</span><span class="sxs-lookup"><span data-stu-id="00321-241">Datetime</span></span> |
| <span data-ttu-id="00321-242">Decimal</span><span class="sxs-lookup"><span data-stu-id="00321-242">decimal</span></span> |<span data-ttu-id="00321-243">Decimal</span><span class="sxs-lookup"><span data-stu-id="00321-243">Decimal</span></span> |
| <span data-ttu-id="00321-244">Dvojitá přesnost</span><span class="sxs-lookup"><span data-stu-id="00321-244">double precision</span></span> |<span data-ttu-id="00321-245">Double</span><span class="sxs-lookup"><span data-stu-id="00321-245">Double</span></span> |
| <span data-ttu-id="00321-246">Double</span><span class="sxs-lookup"><span data-stu-id="00321-246">double</span></span> |<span data-ttu-id="00321-247">Double</span><span class="sxs-lookup"><span data-stu-id="00321-247">Double</span></span> |
| <span data-ttu-id="00321-248">výčet</span><span class="sxs-lookup"><span data-stu-id="00321-248">enum</span></span> |<span data-ttu-id="00321-249">Řetězec</span><span class="sxs-lookup"><span data-stu-id="00321-249">String</span></span> |
| <span data-ttu-id="00321-250">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="00321-250">float</span></span> |<span data-ttu-id="00321-251">Jeden</span><span class="sxs-lookup"><span data-stu-id="00321-251">Single</span></span> |
| <span data-ttu-id="00321-252">int bez znaménka</span><span class="sxs-lookup"><span data-stu-id="00321-252">int unsigned</span></span> |<span data-ttu-id="00321-253">Int64</span><span class="sxs-lookup"><span data-stu-id="00321-253">Int64</span></span> |
| <span data-ttu-id="00321-254">celá čísla</span><span class="sxs-lookup"><span data-stu-id="00321-254">int</span></span> |<span data-ttu-id="00321-255">Int32</span><span class="sxs-lookup"><span data-stu-id="00321-255">Int32</span></span> |
| <span data-ttu-id="00321-256">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="00321-256">integer unsigned</span></span> |<span data-ttu-id="00321-257">Int64</span><span class="sxs-lookup"><span data-stu-id="00321-257">Int64</span></span> |
| <span data-ttu-id="00321-258">celé číslo</span><span class="sxs-lookup"><span data-stu-id="00321-258">integer</span></span> |<span data-ttu-id="00321-259">Int32</span><span class="sxs-lookup"><span data-stu-id="00321-259">Int32</span></span> |
| <span data-ttu-id="00321-260">dlouhé varbinary</span><span class="sxs-lookup"><span data-stu-id="00321-260">long varbinary</span></span> |<span data-ttu-id="00321-261">Byte]</span><span class="sxs-lookup"><span data-stu-id="00321-261">Byte[]</span></span> |
| <span data-ttu-id="00321-262">dlouhé varchar</span><span class="sxs-lookup"><span data-stu-id="00321-262">long varchar</span></span> |<span data-ttu-id="00321-263">Řetězec</span><span class="sxs-lookup"><span data-stu-id="00321-263">String</span></span> |
| <span data-ttu-id="00321-264">longblob</span><span class="sxs-lookup"><span data-stu-id="00321-264">longblob</span></span> |<span data-ttu-id="00321-265">Byte]</span><span class="sxs-lookup"><span data-stu-id="00321-265">Byte[]</span></span> |
| <span data-ttu-id="00321-266">LONGTEXT</span><span class="sxs-lookup"><span data-stu-id="00321-266">longtext</span></span> |<span data-ttu-id="00321-267">Řetězec</span><span class="sxs-lookup"><span data-stu-id="00321-267">String</span></span> |
| <span data-ttu-id="00321-268">mediumblob</span><span class="sxs-lookup"><span data-stu-id="00321-268">mediumblob</span></span> |<span data-ttu-id="00321-269">Byte]</span><span class="sxs-lookup"><span data-stu-id="00321-269">Byte[]</span></span> |
| <span data-ttu-id="00321-270">mediumint bez znaménka</span><span class="sxs-lookup"><span data-stu-id="00321-270">mediumint unsigned</span></span> |<span data-ttu-id="00321-271">Int64</span><span class="sxs-lookup"><span data-stu-id="00321-271">Int64</span></span> |
| <span data-ttu-id="00321-272">mediumint</span><span class="sxs-lookup"><span data-stu-id="00321-272">mediumint</span></span> |<span data-ttu-id="00321-273">Int32</span><span class="sxs-lookup"><span data-stu-id="00321-273">Int32</span></span> |
| <span data-ttu-id="00321-274">mediumtext</span><span class="sxs-lookup"><span data-stu-id="00321-274">mediumtext</span></span> |<span data-ttu-id="00321-275">Řetězec</span><span class="sxs-lookup"><span data-stu-id="00321-275">String</span></span> |
| <span data-ttu-id="00321-276">číselné</span><span class="sxs-lookup"><span data-stu-id="00321-276">numeric</span></span> |<span data-ttu-id="00321-277">Decimal</span><span class="sxs-lookup"><span data-stu-id="00321-277">Decimal</span></span> |
| <span data-ttu-id="00321-278">skutečné</span><span class="sxs-lookup"><span data-stu-id="00321-278">real</span></span> |<span data-ttu-id="00321-279">Double</span><span class="sxs-lookup"><span data-stu-id="00321-279">Double</span></span> |
| <span data-ttu-id="00321-280">nastavení</span><span class="sxs-lookup"><span data-stu-id="00321-280">set</span></span> |<span data-ttu-id="00321-281">Řetězec</span><span class="sxs-lookup"><span data-stu-id="00321-281">String</span></span> |
| <span data-ttu-id="00321-282">smallint bez znaménka</span><span class="sxs-lookup"><span data-stu-id="00321-282">smallint unsigned</span></span> |<span data-ttu-id="00321-283">Int32</span><span class="sxs-lookup"><span data-stu-id="00321-283">Int32</span></span> |
| <span data-ttu-id="00321-284">smallint</span><span class="sxs-lookup"><span data-stu-id="00321-284">smallint</span></span> |<span data-ttu-id="00321-285">Int16</span><span class="sxs-lookup"><span data-stu-id="00321-285">Int16</span></span> |
| <span data-ttu-id="00321-286">Text</span><span class="sxs-lookup"><span data-stu-id="00321-286">text</span></span> |<span data-ttu-id="00321-287">Řetězec</span><span class="sxs-lookup"><span data-stu-id="00321-287">String</span></span> |
| <span data-ttu-id="00321-288">time</span><span class="sxs-lookup"><span data-stu-id="00321-288">time</span></span> |<span data-ttu-id="00321-289">Časový interval</span><span class="sxs-lookup"><span data-stu-id="00321-289">TimeSpan</span></span> |
| <span data-ttu-id="00321-290">časové razítko</span><span class="sxs-lookup"><span data-stu-id="00321-290">timestamp</span></span> |<span data-ttu-id="00321-291">Data a času</span><span class="sxs-lookup"><span data-stu-id="00321-291">Datetime</span></span> |
| <span data-ttu-id="00321-292">tinyblob</span><span class="sxs-lookup"><span data-stu-id="00321-292">tinyblob</span></span> |<span data-ttu-id="00321-293">Byte]</span><span class="sxs-lookup"><span data-stu-id="00321-293">Byte[]</span></span> |
| <span data-ttu-id="00321-294">tinyint bez znaménka</span><span class="sxs-lookup"><span data-stu-id="00321-294">tinyint unsigned</span></span> |<span data-ttu-id="00321-295">Int16</span><span class="sxs-lookup"><span data-stu-id="00321-295">Int16</span></span> |
| <span data-ttu-id="00321-296">tinyint</span><span class="sxs-lookup"><span data-stu-id="00321-296">tinyint</span></span> |<span data-ttu-id="00321-297">Int16</span><span class="sxs-lookup"><span data-stu-id="00321-297">Int16</span></span> |
| <span data-ttu-id="00321-298">tinytext</span><span class="sxs-lookup"><span data-stu-id="00321-298">tinytext</span></span> |<span data-ttu-id="00321-299">Řetězec</span><span class="sxs-lookup"><span data-stu-id="00321-299">String</span></span> |
| <span data-ttu-id="00321-300">varchar</span><span class="sxs-lookup"><span data-stu-id="00321-300">varchar</span></span> |<span data-ttu-id="00321-301">Řetězec</span><span class="sxs-lookup"><span data-stu-id="00321-301">String</span></span> |
| <span data-ttu-id="00321-302">Rok</span><span class="sxs-lookup"><span data-stu-id="00321-302">year</span></span> |<span data-ttu-id="00321-303">celá čísla</span><span class="sxs-lookup"><span data-stu-id="00321-303">Int</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="00321-304">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="00321-304">Map source to sink columns</span></span>
<span data-ttu-id="00321-305">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="00321-305">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="00321-306">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="00321-306">Repeatable read from relational sources</span></span>
<span data-ttu-id="00321-307">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="00321-307">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="00321-308">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="00321-308">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="00321-309">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="00321-309">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="00321-310">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="00321-310">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="00321-311">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="00321-311">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="00321-312">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="00321-312">Performance and Tuning</span></span>
<span data-ttu-id="00321-313">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="00321-313">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
