---
title: "Přesun dat do a z SQL serveru | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z databáze serveru SQL Server, který je místně nebo v virtuálního počítače Azure pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jingwang
ms.openlocfilehash: 9cd2077d897631457925cda5ef5e6df3c0c33177
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-to-and-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a><span data-ttu-id="78bc6-103">Přesun dat do a z místní SQL Server nebo na IaaS (virtuální počítač Azure) pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="78bc6-103">Move data to and from SQL Server on-premises or on IaaS (Azure VM) using Azure Data Factory</span></span>
<span data-ttu-id="78bc6-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z databáze SQL Server na místě.</span><span class="sxs-lookup"><span data-stu-id="78bc6-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from an on-premises SQL Server database.</span></span> <span data-ttu-id="78bc6-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="78bc6-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="78bc6-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="78bc6-106">Supported scenarios</span></span>
<span data-ttu-id="78bc6-107">Může kopírovat data **z databáze systému SQL Server** ukládá do následující data:</span><span class="sxs-lookup"><span data-stu-id="78bc6-107">You can copy data **from a SQL Server database** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="78bc6-108">Může kopírovat data z následujících datových úložišť **k databázi systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="78bc6-108">You can copy data from the following data stores **to a SQL Server database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a><span data-ttu-id="78bc6-109">Podporované verze systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="78bc6-109">Supported SQL Server versions</span></span>
<span data-ttu-id="78bc6-110">Tato podpora konektoru systému SQL Server kopírování dat z/do následující verze instance hostovaná místně nebo v Azure IaaS pomocí ověřování SQL a ověřování systému Windows: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span><span class="sxs-lookup"><span data-stu-id="78bc6-110">This SQL Server connector support copying data from/to the following versions of instance hosted on-premises or in Azure IaaS using both SQL authentication and Windows authentication: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="78bc6-111">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="78bc6-111">Enabling connectivity</span></span>
<span data-ttu-id="78bc6-112">Koncepty a kroky potřebné pro připojení s SQL serveru hostované místně nebo ve virtuálních počítačích Azure IaaS (infrastruktura jako služba) jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="78bc6-112">The concepts and steps needed for connecting with SQL Server hosted on-premises or in Azure IaaS (Infrastructure-as-a-Service) VMs are the same.</span></span> <span data-ttu-id="78bc6-113">V obou případech budete muset použít Brána pro správu dat pro připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="78bc6-113">In both cases, you need to use Data Management Gateway for connectivity.</span></span>

<span data-ttu-id="78bc6-114">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku se dozvíte o Brána pro správu dat a podrobné pokyny o nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="78bc6-114">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span> <span data-ttu-id="78bc6-115">Nastavení instance brány je nezbytný předpoklad pro připojení k systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="78bc6-115">Setting up a gateway instance is a pre-requisite for connecting with SQL Server.</span></span>

<span data-ttu-id="78bc6-116">Při bránu nainstalovat na stejný na místním počítači nebo instanci cloudu virtuálního počítače jako Server SQL pro lepší výkon, doporučujeme vám nainstalovat na samostatné počítače.</span><span class="sxs-lookup"><span data-stu-id="78bc6-116">While you can install gateway on the same on-premises machine or cloud VM instance as the SQL Server for better performance, we recommended that you install them on separate machines.</span></span> <span data-ttu-id="78bc6-117">S brány a SQL Server na samostatné počítače snižuje kolize prostředků.</span><span class="sxs-lookup"><span data-stu-id="78bc6-117">Having the gateway and SQL Server on separate machines reduces resource contention.</span></span>

## <a name="getting-started"></a><span data-ttu-id="78bc6-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="78bc6-118">Getting started</span></span>
<span data-ttu-id="78bc6-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z databáze SQL serveru místní pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="78bc6-119">You can create a pipeline with a copy activity that moves data to/from an on-premises SQL Server database by using different tools/APIs.</span></span>

<span data-ttu-id="78bc6-120">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="78bc6-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="78bc6-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="78bc6-122">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="78bc6-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="78bc6-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="78bc6-124">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="78bc6-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="78bc6-125">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-125">Create a **data factory**.</span></span> <span data-ttu-id="78bc6-126">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="78bc6-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="78bc6-127">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="78bc6-127">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="78bc6-128">Pokud jsou kopírování dat z databáze SQL serveru do Azure blob storage, například vytvoříte dvě propojené služby propojení databáze systému SQL Server a účet úložiště Azure pro vytváření dat..</span><span class="sxs-lookup"><span data-stu-id="78bc6-128">For example, if you are copying data from a SQL Server database to an Azure blob storage, you create two linked services to link your SQL Server database and Azure storage account to your data factory.</span></span> <span data-ttu-id="78bc6-129">Vlastnosti propojené služby, které jsou specifické pro databázi systému SQL Server, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="78bc6-129">For linked service properties that are specific to SQL Server database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="78bc6-130">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="78bc6-130">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="78bc6-131">V příkladu uvedených v posledním kroku vytvoříte datové sady určete tabulku SQL v databázi systému SQL Server, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="78bc6-131">In the example mentioned in the last step, you create a dataset to specify the SQL table in your SQL Server database that contains the input data.</span></span> <span data-ttu-id="78bc6-132">A vytvořte jinou datovou sadu, která zadejte kontejner objektů blob a složky, která obsahuje data zkopírovat z databáze serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="78bc6-132">And, you create another dataset to specify the blob container and the folder that holds the data copied from the SQL Server database.</span></span> <span data-ttu-id="78bc6-133">Vlastnosti datové sady, které jsou specifické pro databázi systému SQL Server, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="78bc6-133">For dataset properties that are specific to SQL Server database, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="78bc6-134">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="78bc6-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="78bc6-135">V příkladu již bylo zmíněno dříve použijete SqlSource jako zdroj a BlobSink jako jímku pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="78bc6-135">In the example mentioned earlier, you use SqlSource as a source and BlobSink as a sink for the copy activity.</span></span> <span data-ttu-id="78bc6-136">Podobně pokud kopírujete z Azure Blob Storage do databáze serveru SQL, použijte BlobSource a SqlSink v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="78bc6-136">Similarly, if you are copying from Azure Blob Storage to SQL Server Database, you use BlobSource and SqlSink in the copy activity.</span></span> <span data-ttu-id="78bc6-137">Kopírovat vlastnosti aktivity, které jsou specifické pro databáze serveru SQL, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="78bc6-137">For copy activity properties that are specific to SQL Server Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="78bc6-138">Podrobnosti o tom, jak používat úložiště dat jako zdroj nebo jímka klikněte na odkaz v předchozí části pro data store.</span><span class="sxs-lookup"><span data-stu-id="78bc6-138">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span> 

<span data-ttu-id="78bc6-139">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="78bc6-139">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="78bc6-140">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="78bc6-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="78bc6-141">Ukázky s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat do nebo z místní databáze systému SQL Server naleznete v části [JSON příklady](#json-examples-for-copying-data-from-and-to-sql-server) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="78bc6-141">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an on-premises SQL Server database, see [JSON examples](#json-examples-for-copying-data-from-and-to-sql-server) section of this article.</span></span> 

<span data-ttu-id="78bc6-142">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k systému SQL Server:</span><span class="sxs-lookup"><span data-stu-id="78bc6-142">The following sections provide details about JSON properties that are used to define Data Factory entities specific to SQL Server:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="78bc6-143">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="78bc6-143">Linked service properties</span></span>
<span data-ttu-id="78bc6-144">Vytvoření propojené služby typu **onpremisessqlserver** propojit místní databázi systému SQL Server do služby data factory.</span><span class="sxs-lookup"><span data-stu-id="78bc6-144">You create a linked service of type **OnPremisesSqlServer** to link an on-premises SQL Server database to a data factory.</span></span> <span data-ttu-id="78bc6-145">Následující tabulka obsahuje popis specifické pro službu SQL serveru propojená místní elementy JSON.</span><span class="sxs-lookup"><span data-stu-id="78bc6-145">The following table provides description for JSON elements specific to on-premises SQL Server linked service.</span></span>

<span data-ttu-id="78bc6-146">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro SQL Server propojené služby.</span><span class="sxs-lookup"><span data-stu-id="78bc6-146">The following table provides description for JSON elements specific to SQL Server linked service.</span></span>

| <span data-ttu-id="78bc6-147">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="78bc6-147">Property</span></span> | <span data-ttu-id="78bc6-148">Popis</span><span class="sxs-lookup"><span data-stu-id="78bc6-148">Description</span></span> | <span data-ttu-id="78bc6-149">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="78bc6-149">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="78bc6-150">type</span><span class="sxs-lookup"><span data-stu-id="78bc6-150">type</span></span> |<span data-ttu-id="78bc6-151">Vlastnost typu musí být nastavená na: **onpremisessqlserver**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-151">The type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="78bc6-152">Ano</span><span class="sxs-lookup"><span data-stu-id="78bc6-152">Yes</span></span> |
| <span data-ttu-id="78bc6-153">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="78bc6-153">connectionString</span></span> |<span data-ttu-id="78bc6-154">Zadejte připojovací řetězec informace potřebné pro připojení k místní databázi systému SQL Server pomocí ověřování SQL nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="78bc6-154">Specify connectionString information needed to connect to the on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="78bc6-155">Ano</span><span class="sxs-lookup"><span data-stu-id="78bc6-155">Yes</span></span> |
| <span data-ttu-id="78bc6-156">gatewayName</span><span class="sxs-lookup"><span data-stu-id="78bc6-156">gatewayName</span></span> |<span data-ttu-id="78bc6-157">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="78bc6-157">Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database.</span></span> |<span data-ttu-id="78bc6-158">Ano</span><span class="sxs-lookup"><span data-stu-id="78bc6-158">Yes</span></span> |
| <span data-ttu-id="78bc6-159">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="78bc6-159">username</span></span> |<span data-ttu-id="78bc6-160">Zadejte uživatelské jméno, pokud používáte ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="78bc6-160">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="78bc6-161">Příklad: **domainname\\uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-161">Example: **domainname\\username**.</span></span> |<span data-ttu-id="78bc6-162">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-162">No</span></span> |
| <span data-ttu-id="78bc6-163">heslo</span><span class="sxs-lookup"><span data-stu-id="78bc6-163">password</span></span> |<span data-ttu-id="78bc6-164">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="78bc6-164">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="78bc6-165">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-165">No</span></span> |

<span data-ttu-id="78bc6-166">Můžete šifrovat přihlašovací údaje pomocí **New-AzureRmDataFactoryEncryptValue** rutiny a jak je znázorněno v následujícím příkladu je využít v připojovacím řetězci (**EncryptedCredential** vlastnost):</span><span class="sxs-lookup"><span data-stu-id="78bc6-166">You can encrypt credentials using the **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in the connection string as shown in the following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a><span data-ttu-id="78bc6-167">Ukázky</span><span class="sxs-lookup"><span data-stu-id="78bc6-167">Samples</span></span>
<span data-ttu-id="78bc6-168">**JSON pro pomocí ověřování SQL.**</span><span class="sxs-lookup"><span data-stu-id="78bc6-168">**JSON for using SQL Authentication**</span></span>

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
<span data-ttu-id="78bc6-169">**JSON pro použití ověřování systému Windows**</span><span class="sxs-lookup"><span data-stu-id="78bc6-169">**JSON for using Windows Authentication**</span></span>

<span data-ttu-id="78bc6-170">Brána pro správu dat se zosobnit zadaný uživatelský účet pro připojení k místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="78bc6-170">Data Management Gateway will impersonate the specified user account to connect to the on-premises SQL Server database.</span></span> 

```json
{
     "Name": " MyOnPremisesSQLDB",
     "Properties":
     {
         "type": "OnPremisesSqlServer",
         "typeProperties": {
             "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
             "username": "<domain\\username>",
             "password": "<password>",
             "gatewayName": "<gateway name>"
        }
     }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="78bc6-171">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="78bc6-171">Dataset properties</span></span>
<span data-ttu-id="78bc6-172">V ukázky, můžete použít datovou sadu typu **SqlServerTable** představují tabulky v databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="78bc6-172">In the samples, you have used a dataset of type **SqlServerTable** to represent a table in a SQL Server database.</span></span>  

<span data-ttu-id="78bc6-173">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="78bc6-173">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="78bc6-174">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (SQL Server, objektů blob v Azure, Azure table atd.).</span><span class="sxs-lookup"><span data-stu-id="78bc6-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (SQL Server, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="78bc6-175">V rámci typeProperties části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="78bc6-175">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="78bc6-176">**Rámci typeProperties** části datové sady typu **SqlServerTable** má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="78bc6-176">The **typeProperties** section for the dataset of type **SqlServerTable** has the following properties:</span></span>

| <span data-ttu-id="78bc6-177">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="78bc6-177">Property</span></span> | <span data-ttu-id="78bc6-178">Popis</span><span class="sxs-lookup"><span data-stu-id="78bc6-178">Description</span></span> | <span data-ttu-id="78bc6-179">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="78bc6-179">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="78bc6-180">tableName</span><span class="sxs-lookup"><span data-stu-id="78bc6-180">tableName</span></span> |<span data-ttu-id="78bc6-181">Název tabulky nebo zobrazení instance databáze serveru SQL, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="78bc6-181">Name of the table or view in the SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="78bc6-182">Ano</span><span class="sxs-lookup"><span data-stu-id="78bc6-182">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="78bc6-183">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="78bc6-183">Copy activity properties</span></span>
<span data-ttu-id="78bc6-184">Pokud přesouváte data z databáze systému SQL Server, nastavíte typ zdroje v aktivitě kopírování do **SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-184">If you are moving data from a SQL Server database, you set the source type in the copy activity to **SqlSource**.</span></span> <span data-ttu-id="78bc6-185">Podobně pokud přesouváte data do databáze serveru SQL, nastavíte typ jímky v aktivitě kopírování do **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-185">Similarly, if you are moving data to a SQL Server database, you set the sink type in the copy activity to **SqlSink**.</span></span> <span data-ttu-id="78bc6-186">Tato část obsahuje seznam vlastností, které jsou podporované SqlSource a SqlSink.</span><span class="sxs-lookup"><span data-stu-id="78bc6-186">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

<span data-ttu-id="78bc6-187">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="78bc6-187">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="78bc6-188">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="78bc6-188">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="78bc6-189">Aktivita kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="78bc6-189">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="78bc6-190">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="78bc6-190">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="78bc6-191">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-191">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="78bc6-192">SqlSource</span><span class="sxs-lookup"><span data-stu-id="78bc6-192">SqlSource</span></span>
<span data-ttu-id="78bc6-193">Pokud zdroj v aktivitě kopírování je typu **SqlSource**, následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="78bc6-193">When source in a copy activity is of type **SqlSource**, the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="78bc6-194">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="78bc6-194">Property</span></span> | <span data-ttu-id="78bc6-195">Popis</span><span class="sxs-lookup"><span data-stu-id="78bc6-195">Description</span></span> | <span data-ttu-id="78bc6-196">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="78bc6-196">Allowed values</span></span> | <span data-ttu-id="78bc6-197">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="78bc6-197">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="78bc6-198">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="78bc6-198">sqlReaderQuery</span></span> |<span data-ttu-id="78bc6-199">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-199">Use the custom query to read data.</span></span> |<span data-ttu-id="78bc6-200">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="78bc6-200">SQL query string.</span></span> <span data-ttu-id="78bc6-201">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="78bc6-201">For example: select * from MyTable.</span></span> <span data-ttu-id="78bc6-202">Může odkazovat více tabulek z databáze odkazuje vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="78bc6-202">May reference multiple tables from the database referenced by the input dataset.</span></span> <span data-ttu-id="78bc6-203">Pokud není zadaný příkaz jazyka SQL, která se provedla: Vyberte možnost z MyTable.</span><span class="sxs-lookup"><span data-stu-id="78bc6-203">If not specified, the SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="78bc6-204">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-204">No</span></span> |
| <span data-ttu-id="78bc6-205">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="78bc6-205">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="78bc6-206">Název uložené procedury, který čte data ze zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-206">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="78bc6-207">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="78bc6-207">Name of the stored procedure.</span></span> <span data-ttu-id="78bc6-208">Poslední příkaz jazyka SQL musí být příkaz SELECT v uložené proceduře.</span><span class="sxs-lookup"><span data-stu-id="78bc6-208">The last SQL statement must be a SELECT statement in the stored procedure.</span></span> |<span data-ttu-id="78bc6-209">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-209">No</span></span> |
| <span data-ttu-id="78bc6-210">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="78bc6-210">storedProcedureParameters</span></span> |<span data-ttu-id="78bc6-211">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="78bc6-211">Parameters for the stored procedure.</span></span> |<span data-ttu-id="78bc6-212">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="78bc6-212">Name/value pairs.</span></span> <span data-ttu-id="78bc6-213">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="78bc6-213">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="78bc6-214">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-214">No</span></span> |

<span data-ttu-id="78bc6-215">Pokud **sqlReaderQuery** je zadán pro SqlSource, aktivitě kopírování spustí tento dotaz na zdroji databáze systému SQL Server získat data.</span><span class="sxs-lookup"><span data-stu-id="78bc6-215">If the **sqlReaderQuery** is specified for the SqlSource, the Copy Activity runs this query against the SQL Server Database source to get the data.</span></span>

<span data-ttu-id="78bc6-216">Alternativně můžete zadat uložené procedury zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura přebírá parametry).</span><span class="sxs-lookup"><span data-stu-id="78bc6-216">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="78bc6-217">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, sloupce definované v části struktura slouží k vytvoření dotazu vyberte možnost spustit v databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="78bc6-217">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="78bc6-218">Pokud definice datové sady nemá strukturu, jsou vybrány všechny sloupce z tabulky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-218">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

> [!NOTE]
> <span data-ttu-id="78bc6-219">Při použití **sqlReaderStoredProcedureName**, stále je třeba zadat hodnotu pro **tableName** vlastnost v datové sadě JSON.</span><span class="sxs-lookup"><span data-stu-id="78bc6-219">When you use **sqlReaderStoredProcedureName**, you still need to specify a value for the **tableName** property in the dataset JSON.</span></span> <span data-ttu-id="78bc6-220">Neexistují žádné ověření, ale adresovat této tabulky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-220">There are no validations performed against this table though.</span></span>

### <a name="sqlsink"></a><span data-ttu-id="78bc6-221">SqlSink</span><span class="sxs-lookup"><span data-stu-id="78bc6-221">SqlSink</span></span>
<span data-ttu-id="78bc6-222">**SqlSink** podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="78bc6-222">**SqlSink** supports the following properties:</span></span>

| <span data-ttu-id="78bc6-223">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="78bc6-223">Property</span></span> | <span data-ttu-id="78bc6-224">Popis</span><span class="sxs-lookup"><span data-stu-id="78bc6-224">Description</span></span> | <span data-ttu-id="78bc6-225">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="78bc6-225">Allowed values</span></span> | <span data-ttu-id="78bc6-226">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="78bc6-226">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="78bc6-227">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="78bc6-227">writeBatchTimeout</span></span> |<span data-ttu-id="78bc6-228">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.</span><span class="sxs-lookup"><span data-stu-id="78bc6-228">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="78bc6-229">Časový interval</span><span class="sxs-lookup"><span data-stu-id="78bc6-229">timespan</span></span><br/><br/> <span data-ttu-id="78bc6-230">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="78bc6-230">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="78bc6-231">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-231">No</span></span> |
| <span data-ttu-id="78bc6-232">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="78bc6-232">writeBatchSize</span></span> |<span data-ttu-id="78bc6-233">Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="78bc6-233">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="78bc6-234">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="78bc6-234">Integer (number of rows)</span></span> |<span data-ttu-id="78bc6-235">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="78bc6-235">No (default: 10000)</span></span> |
| <span data-ttu-id="78bc6-236">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="78bc6-236">sqlWriterCleanupScript</span></span> |<span data-ttu-id="78bc6-237">Zadejte dotaz aktivity kopírování provést tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="78bc6-237">Specify query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="78bc6-238">Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy) části.</span><span class="sxs-lookup"><span data-stu-id="78bc6-238">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="78bc6-239">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-239">A query statement.</span></span> |<span data-ttu-id="78bc6-240">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-240">No</span></span> |
| <span data-ttu-id="78bc6-241">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="78bc6-241">sliceIdentifierColumnName</span></span> |<span data-ttu-id="78bc6-242">Zadejte název sloupce pro aktivitu kopírování vyplníte identifikátor automaticky generovány řez, který se používá k vyčištění dat určitý řez při spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-242">Specify column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> <span data-ttu-id="78bc6-243">Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy) části.</span><span class="sxs-lookup"><span data-stu-id="78bc6-243">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="78bc6-244">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="78bc6-244">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="78bc6-245">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-245">No</span></span> |
| <span data-ttu-id="78bc6-246">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="78bc6-246">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="78bc6-247">Název uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-247">Name of the stored procedure that upserts (updates/inserts) data into the target table.</span></span> |<span data-ttu-id="78bc6-248">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="78bc6-248">Name of the stored procedure.</span></span> |<span data-ttu-id="78bc6-249">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-249">No</span></span> |
| <span data-ttu-id="78bc6-250">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="78bc6-250">storedProcedureParameters</span></span> |<span data-ttu-id="78bc6-251">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="78bc6-251">Parameters for the stored procedure.</span></span> |<span data-ttu-id="78bc6-252">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="78bc6-252">Name/value pairs.</span></span> <span data-ttu-id="78bc6-253">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="78bc6-253">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="78bc6-254">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-254">No</span></span> |
| <span data-ttu-id="78bc6-255">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="78bc6-255">sqlWriterTableType</span></span> |<span data-ttu-id="78bc6-256">Zadejte název typu tabulky má být použit v uložené proceduře.</span><span class="sxs-lookup"><span data-stu-id="78bc6-256">Specify table type name to be used in the stored procedure.</span></span> <span data-ttu-id="78bc6-257">Aktivita kopírování zpřístupní přesouvání dat v dočasné tabulce s tímto typem tabulky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-257">Copy activity makes the data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="78bc6-258">Uložená procedura kód pak sloučit data kopírovány s existujícími daty.</span><span class="sxs-lookup"><span data-stu-id="78bc6-258">Stored procedure code can then merge the data being copied with existing data.</span></span> |<span data-ttu-id="78bc6-259">Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-259">A table type name.</span></span> |<span data-ttu-id="78bc6-260">Ne</span><span class="sxs-lookup"><span data-stu-id="78bc6-260">No</span></span> |


## <a name="json-examples-for-copying-data-from-and-to-sql-server"></a><span data-ttu-id="78bc6-261">Příklady JSON pro kopírování dat z a do systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="78bc6-261">JSON examples for copying data from and to SQL Server</span></span>
<span data-ttu-id="78bc6-262">Následující příklady poskytují ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="78bc6-262">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="78bc6-263">Následující ukázky ukazují, jak ke zkopírování dat do a z SQL serveru a Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="78bc6-263">The following samples show how to copy data to and from SQL Server and Azure Blob Storage.</span></span> <span data-ttu-id="78bc6-264">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="78bc6-264">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>     

## <a name="example-copy-data-from-sql-server-to-azure-blob"></a><span data-ttu-id="78bc6-265">Příklad: Kopírování dat z SQL serveru do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="78bc6-265">Example: Copy data from SQL Server to Azure Blob</span></span>
<span data-ttu-id="78bc6-266">Následující příklad ukazuje:</span><span class="sxs-lookup"><span data-stu-id="78bc6-266">The following sample shows:</span></span>

1. <span data-ttu-id="78bc6-267">Propojené služby typu [onpremisessqlserver](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-267">A linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="78bc6-268">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-268">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="78bc6-269">Vstup [datovou sadu](data-factory-create-datasets.md) typu [SqlServerTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-269">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](#dataset-properties).</span></span>
4. <span data-ttu-id="78bc6-270">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-270">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="78bc6-271">[Kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-271">The [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="78bc6-272">Ukázka zkopíruje data časové řady z tabulky serveru SQL Server do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-272">The sample copies time-series data from a SQL Server table to an Azure blob every hour.</span></span> <span data-ttu-id="78bc6-273">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-273">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="78bc6-274">Jako první krok nastavte Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="78bc6-274">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="78bc6-275">Tyto pokyny jsou v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="78bc6-275">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="78bc6-276">**Služba SQL Server propojené**</span><span class="sxs-lookup"><span data-stu-id="78bc6-276">**SQL Server linked service**</span></span>
```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
<span data-ttu-id="78bc6-277">**Objekt Blob propojená služba Azure storage**</span><span class="sxs-lookup"><span data-stu-id="78bc6-277">**Azure Blob storage linked service**</span></span>

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="78bc6-278">**Vstupní datové sady SQL Server**</span><span class="sxs-lookup"><span data-stu-id="78bc6-278">**SQL Server input dataset**</span></span>

<span data-ttu-id="78bc6-279">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v systému SQL Server a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="78bc6-279">The sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="78bc6-280">Můžete dotazovat přes více tabulek v rámci stejné databáze pomocí jednu datovou sadu, ale musí používat jednu tabulku pro typeProperty tableName datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-280">You can query over multiple tables within the same database using a single dataset, but a single table must be used for the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="78bc6-281">Nastavení "externí": "PRAVDA" informuje služba Data Factory, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="78bc6-281">Setting “external”: ”true” informs Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
  "name": "SqlServerInput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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
<span data-ttu-id="78bc6-282">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="78bc6-282">**Azure Blob output dataset**</span></span>

<span data-ttu-id="78bc6-283">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="78bc6-283">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="78bc6-284">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="78bc6-284">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="78bc6-285">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="78bc6-285">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="78bc6-286">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="78bc6-286">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="78bc6-287">Kanál obsahuje aktivitu kopírování, která je konfigurovaná pro používání těchto vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-287">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="78bc6-288">V definici JSON kanálu **zdroj** je typ nastaven na **SqlSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-288">In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="78bc6-289">Zadané pro dotaz SQL **SqlReaderQuery** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="78bc6-289">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
<span data-ttu-id="78bc6-290">V tomto příkladu **sqlReaderQuery** je zadán pro SqlSource.</span><span class="sxs-lookup"><span data-stu-id="78bc6-290">In this example, **sqlReaderQuery** is specified for the SqlSource.</span></span> <span data-ttu-id="78bc6-291">Aktivita kopírování spustí tento dotaz na zdroji databáze systému SQL Server získat data.</span><span class="sxs-lookup"><span data-stu-id="78bc6-291">The Copy Activity runs this query against the SQL Server Database source to get the data.</span></span> <span data-ttu-id="78bc6-292">Alternativně můžete zadat uložené procedury zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura přebírá parametry).</span><span class="sxs-lookup"><span data-stu-id="78bc6-292">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span> <span data-ttu-id="78bc6-293">SqlReaderQuery může odkazovat více tabulek v databázi odkazuje vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="78bc6-293">The sqlReaderQuery can reference multiple tables within the database referenced by the input dataset.</span></span> <span data-ttu-id="78bc6-294">Se neomezuje jenom do tabulky, nastavte jako typeProperty tableName datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-294">It is not limited to only the table set as the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="78bc6-295">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, sloupce definované v části struktura slouží k vytvoření dotazu vyberte možnost spustit v databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="78bc6-295">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="78bc6-296">Pokud definice datové sady nemá strukturu, jsou vybrány všechny sloupce z tabulky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-296">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

<span data-ttu-id="78bc6-297">Najdete v článku [zdroje Sql](#sqlsource) části a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) pro seznam vlastností, které jsou podporované SqlSource a BlobSink.</span><span class="sxs-lookup"><span data-stu-id="78bc6-297">See the [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for the list of properties supported by SqlSource and BlobSink.</span></span>

## <a name="example-copy-data-from-azure-blob-to-sql-server"></a><span data-ttu-id="78bc6-298">Příklad: Kopírování dat z objektu Blob Azure do SQL serveru</span><span class="sxs-lookup"><span data-stu-id="78bc6-298">Example: Copy data from Azure Blob to SQL Server</span></span>
<span data-ttu-id="78bc6-299">Následující příklad ukazuje:</span><span class="sxs-lookup"><span data-stu-id="78bc6-299">The following sample shows:</span></span>

1. <span data-ttu-id="78bc6-300">Propojené služby typu [onpremisessqlserver](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-300">The linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="78bc6-301">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-301">The linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="78bc6-302">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-302">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="78bc6-303">Výstup [datovou sadu](data-factory-create-datasets.md) typu [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-303">An output [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="78bc6-304">[Kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [SqlSink](#sql-server-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="78bc6-304">The [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#sql-server-copy-activity-type-properties).</span></span>

<span data-ttu-id="78bc6-305">Kopie ukázka časové řady dat z Azure blob do systému SQL Server tabulky každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-305">The sample copies time-series data from an Azure blob to a SQL Server table every hour.</span></span> <span data-ttu-id="78bc6-306">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="78bc6-306">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="78bc6-307">**Služba SQL Server propojené**</span><span class="sxs-lookup"><span data-stu-id="78bc6-307">**SQL Server linked service**</span></span>

```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
<span data-ttu-id="78bc6-308">**Objekt Blob propojená služba Azure storage**</span><span class="sxs-lookup"><span data-stu-id="78bc6-308">**Azure Blob storage linked service**</span></span>

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="78bc6-309">**Azure vstupní datovou sadu objektu Blob**</span><span class="sxs-lookup"><span data-stu-id="78bc6-309">**Azure Blob input dataset**</span></span>

<span data-ttu-id="78bc6-310">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="78bc6-310">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="78bc6-311">Název složky a cesta k souboru pro tento objekt blob se vyhodnocují dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="78bc6-311">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="78bc6-312">Cesta ke složce používá rok, měsíc a den součástí čas spuštění a název souboru používá hodinu součástí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="78bc6-312">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="78bc6-313">"externí": "PRAVDA" nastavení informuje služba Data Factory, že datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="78bc6-313">“external”: “true” setting informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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
<span data-ttu-id="78bc6-314">**Výstupní datové sady SQL Server**</span><span class="sxs-lookup"><span data-stu-id="78bc6-314">**SQL Server output dataset**</span></span>

<span data-ttu-id="78bc6-315">Ukázka zkopíruje data na tabulku s názvem "MyTable" v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="78bc6-315">The sample copies data to a table named “MyTable” in SQL Server.</span></span> <span data-ttu-id="78bc6-316">Podle očekávání souboru CSV objektů Blob tak, aby obsahovala, vytvořte v systému SQL Server s stejný počet sloupců v tabulce.</span><span class="sxs-lookup"><span data-stu-id="78bc6-316">Create the table in SQL Server with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="78bc6-317">Nové záznamy se přidají do tabulky každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-317">New rows are added to the table every hour.</span></span>

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="78bc6-318">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="78bc6-318">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="78bc6-319">Kanál obsahuje aktivitu kopírování, která je konfigurovaná pro používání těchto vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-319">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="78bc6-320">V definici JSON kanálu **zdroj** je typ nastaven na **BlobSource** a **podřízený** je typ nastaven na **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-320">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlSink**.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": " SqlServerOutput "
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
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

## <a name="troubleshooting-connection-issues"></a><span data-ttu-id="78bc6-321">Odstraňování potíží s připojením</span><span class="sxs-lookup"><span data-stu-id="78bc6-321">Troubleshooting connection issues</span></span>
1. <span data-ttu-id="78bc6-322">Konfigurace systému SQL Server tak, aby přijímal vzdálená připojení.</span><span class="sxs-lookup"><span data-stu-id="78bc6-322">Configure your SQL Server to accept remote connections.</span></span> <span data-ttu-id="78bc6-323">Spusťte **SQL Server Management Studio**, klikněte pravým tlačítkem na **server**a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-323">Launch **SQL Server Management Studio**, right-click **server**, and click **Properties**.</span></span> <span data-ttu-id="78bc6-324">Vyberte **připojení** ze seznamu a zkontrolujte **povolit vzdálená připojení k serveru**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-324">Select **Connections** from the list and check **Allow remote connections to the server**.</span></span>

    ![Povolit vzdálená připojení](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    <span data-ttu-id="78bc6-326">V tématu [konfigurovat možnosti konfigurace serveru pro vzdálený přístup](https://msdn.microsoft.com/library/ms191464.aspx) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="78bc6-326">See [Configure the remote access Server Configuration Option](https://msdn.microsoft.com/library/ms191464.aspx) for detailed steps.</span></span>
2. <span data-ttu-id="78bc6-327">Spusťte **Správce konfigurace systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-327">Launch **SQL Server Configuration Manager**.</span></span> <span data-ttu-id="78bc6-328">Rozbalte položku **konfigurace sítě serveru SQL Server** pro instanci a vyberte **protokoly pro MSSQLSERVER**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-328">Expand **SQL Server Network Configuration** for the instance you want, and select **Protocols for MSSQLSERVER**.</span></span> <span data-ttu-id="78bc6-329">Měli byste vidět protokolů v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="78bc6-329">You should see protocols in the right-pane.</span></span> <span data-ttu-id="78bc6-330">Povolte protokol TCP/IP kliknutím pravým tlačítkem na **TCP/IP** a kliknutím na **povolit**.</span><span class="sxs-lookup"><span data-stu-id="78bc6-330">Enable TCP/IP by right-clicking **TCP/IP** and clicking **Enable**.</span></span>

    ![Povolte protokol TCP/IP](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    <span data-ttu-id="78bc6-332">V tématu [povolit nebo zakázat síťový protokol serveru](https://msdn.microsoft.com/library/ms191294.aspx) podrobnosti a alternativní způsoby povolení protokolu TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="78bc6-332">See [Enable or Disable a Server Network Protocol](https://msdn.microsoft.com/library/ms191294.aspx) for details and alternate ways of enabling TCP/IP protocol.</span></span>
3. <span data-ttu-id="78bc6-333">V rámci stejného časového období, klikněte dvakrát na **TCP/IP** spustíte **vlastností protokolu TCP/IP** okno.</span><span class="sxs-lookup"><span data-stu-id="78bc6-333">In the same window, double-click **TCP/IP** to launch **TCP/IP Properties** window.</span></span>
4. <span data-ttu-id="78bc6-334">Přepnout **IP adresy** kartě.</span><span class="sxs-lookup"><span data-stu-id="78bc6-334">Switch to the **IP Addresses** tab.</span></span> <span data-ttu-id="78bc6-335">Posuňte se dolů a najdete **IPAll** části.</span><span class="sxs-lookup"><span data-stu-id="78bc6-335">Scroll down to see **IPAll** section.</span></span> <span data-ttu-id="78bc6-336">Poznamenejte si ** TCP Port ** (výchozí hodnota je **1433**).</span><span class="sxs-lookup"><span data-stu-id="78bc6-336">Note down the **TCP Port **(default is **1433**).</span></span>
5. <span data-ttu-id="78bc6-337">Vytvoření **pravidlo pro bránu Windows Firewall** na počítači, aby povolit příchozí přenosy pomocí tohoto portu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-337">Create a **rule for the Windows Firewall** on the machine to allow incoming traffic through this port.</span></span>  
6. <span data-ttu-id="78bc6-338">**Ověření připojení**: pro připojení k serveru SQL pomocí plně kvalifikovaný název, použijte SQL Server Management Studio z jiný počítač.</span><span class="sxs-lookup"><span data-stu-id="78bc6-338">**Verify connection**: To connect to the SQL Server using fully qualified name, use SQL Server Management Studio from a different machine.</span></span> <span data-ttu-id="78bc6-339">Například: "<machine>.<domain>. Corp.<company>.com, 1433. "</span><span class="sxs-lookup"><span data-stu-id="78bc6-339">For example: "<machine>.<domain>.corp.<company>.com,1433."</span></span>

   > [!IMPORTANT]

   > <span data-ttu-id="78bc6-340">V tématu [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md) podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="78bc6-340">See [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for detailed information.</span></span>
   >
   > <span data-ttu-id="78bc6-341">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="78bc6-341">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>
   >
   >


## <a name="identity-columns-in-the-target-database"></a><span data-ttu-id="78bc6-342">Sloupce identity v cílové databázi</span><span class="sxs-lookup"><span data-stu-id="78bc6-342">Identity columns in the target database</span></span>
<span data-ttu-id="78bc6-343">Tato část poskytuje příklad, který kopíruje data ze zdrojové tabulky s žádný sloupec identity do cílové tabulky se sloupcem identity.</span><span class="sxs-lookup"><span data-stu-id="78bc6-343">This section provides an example that copies data from a source table with no identity column to a destination table with an identity column.</span></span>

<span data-ttu-id="78bc6-344">**Zdrojová tabulka:**</span><span class="sxs-lookup"><span data-stu-id="78bc6-344">**Source table:**</span></span>

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="78bc6-345">**Cílové tabulky:**</span><span class="sxs-lookup"><span data-stu-id="78bc6-345">**Destination table:**</span></span>

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

<span data-ttu-id="78bc6-346">Všimněte si, že cílová tabulka obsahuje sloupec identity.</span><span class="sxs-lookup"><span data-stu-id="78bc6-346">Notice that the target table has an identity column.</span></span>

<span data-ttu-id="78bc6-347">**Definice JSON datové sady zdroje**</span><span class="sxs-lookup"><span data-stu-id="78bc6-347">**Source dataset JSON definition**</span></span>

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
<span data-ttu-id="78bc6-348">**Cílový definici JSON datové sady**</span><span class="sxs-lookup"><span data-stu-id="78bc6-348">**Destination dataset JSON definition**</span></span>

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

<span data-ttu-id="78bc6-349">Všimněte si, že jako zdrojové a cílové tabulky jiné schéma (cíl má sloupec s identitou).</span><span class="sxs-lookup"><span data-stu-id="78bc6-349">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="78bc6-350">V tomto scénáři budete muset zadat **struktura** vlastnost v definici datové sady cíl, který neobsahuje sloupec identity.</span><span class="sxs-lookup"><span data-stu-id="78bc6-350">In this scenario, you need to specify **structure** property in the target dataset definition, which doesn’t include the identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="78bc6-351">Volání uložené procedury jímku SQL</span><span class="sxs-lookup"><span data-stu-id="78bc6-351">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="78bc6-352">V tématu [vyvolat uloženou proceduru SQL jímka v aktivitě kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md) článku příklad volání uložené procedury z jímku SQL při aktivitě kopírování kanálu.</span><span class="sxs-lookup"><span data-stu-id="78bc6-352">See [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article for an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline.</span></span>

## <a name="type-mapping-for-sql-server"></a><span data-ttu-id="78bc6-353">Mapování typu pro SQL server</span><span class="sxs-lookup"><span data-stu-id="78bc6-353">Type mapping for SQL server</span></span>
<span data-ttu-id="78bc6-354">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup krok 2:</span><span class="sxs-lookup"><span data-stu-id="78bc6-354">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, the Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="78bc6-355">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="78bc6-355">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="78bc6-356">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="78bc6-356">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="78bc6-357">Při přesunu dat do a z SQL serveru, se používají následující mapování z typu SQL na typ .NET a naopak.</span><span class="sxs-lookup"><span data-stu-id="78bc6-357">When moving data to & from SQL server, the following mappings are used from SQL type to .NET type and vice versa.</span></span>

<span data-ttu-id="78bc6-358">Mapování je stejný jako mapování SQL Server datového typu pro technologii ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="78bc6-358">The mapping is same as the SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="78bc6-359">Typ databázového stroje SQL Server</span><span class="sxs-lookup"><span data-stu-id="78bc6-359">SQL Server Database Engine type</span></span> | <span data-ttu-id="78bc6-360">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="78bc6-360">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="78bc6-361">bigint</span><span class="sxs-lookup"><span data-stu-id="78bc6-361">bigint</span></span> |<span data-ttu-id="78bc6-362">Int64</span><span class="sxs-lookup"><span data-stu-id="78bc6-362">Int64</span></span> |
| <span data-ttu-id="78bc6-363">Binární</span><span class="sxs-lookup"><span data-stu-id="78bc6-363">binary</span></span> |<span data-ttu-id="78bc6-364">Byte]</span><span class="sxs-lookup"><span data-stu-id="78bc6-364">Byte[]</span></span> |
| <span data-ttu-id="78bc6-365">Bit</span><span class="sxs-lookup"><span data-stu-id="78bc6-365">bit</span></span> |<span data-ttu-id="78bc6-366">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="78bc6-366">Boolean</span></span> |
| <span data-ttu-id="78bc6-367">Char</span><span class="sxs-lookup"><span data-stu-id="78bc6-367">char</span></span> |<span data-ttu-id="78bc6-368">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="78bc6-368">String, Char[]</span></span> |
| <span data-ttu-id="78bc6-369">Datum</span><span class="sxs-lookup"><span data-stu-id="78bc6-369">date</span></span> |<span data-ttu-id="78bc6-370">Data a času</span><span class="sxs-lookup"><span data-stu-id="78bc6-370">DateTime</span></span> |
| <span data-ttu-id="78bc6-371">Data a času</span><span class="sxs-lookup"><span data-stu-id="78bc6-371">Datetime</span></span> |<span data-ttu-id="78bc6-372">Data a času</span><span class="sxs-lookup"><span data-stu-id="78bc6-372">DateTime</span></span> |
| <span data-ttu-id="78bc6-373">datetime2</span><span class="sxs-lookup"><span data-stu-id="78bc6-373">datetime2</span></span> |<span data-ttu-id="78bc6-374">Data a času</span><span class="sxs-lookup"><span data-stu-id="78bc6-374">DateTime</span></span> |
| <span data-ttu-id="78bc6-375">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="78bc6-375">Datetimeoffset</span></span> |<span data-ttu-id="78bc6-376">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="78bc6-376">DateTimeOffset</span></span> |
| <span data-ttu-id="78bc6-377">Decimal</span><span class="sxs-lookup"><span data-stu-id="78bc6-377">Decimal</span></span> |<span data-ttu-id="78bc6-378">Decimal</span><span class="sxs-lookup"><span data-stu-id="78bc6-378">Decimal</span></span> |
| <span data-ttu-id="78bc6-379">Atribut FILESTREAM (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="78bc6-379">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="78bc6-380">Byte]</span><span class="sxs-lookup"><span data-stu-id="78bc6-380">Byte[]</span></span> |
| <span data-ttu-id="78bc6-381">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="78bc6-381">Float</span></span> |<span data-ttu-id="78bc6-382">Double</span><span class="sxs-lookup"><span data-stu-id="78bc6-382">Double</span></span> |
| <span data-ttu-id="78bc6-383">Bitové kopie</span><span class="sxs-lookup"><span data-stu-id="78bc6-383">image</span></span> |<span data-ttu-id="78bc6-384">Byte]</span><span class="sxs-lookup"><span data-stu-id="78bc6-384">Byte[]</span></span> |
| <span data-ttu-id="78bc6-385">celá čísla</span><span class="sxs-lookup"><span data-stu-id="78bc6-385">int</span></span> |<span data-ttu-id="78bc6-386">Int32</span><span class="sxs-lookup"><span data-stu-id="78bc6-386">Int32</span></span> |
| <span data-ttu-id="78bc6-387">peníze</span><span class="sxs-lookup"><span data-stu-id="78bc6-387">money</span></span> |<span data-ttu-id="78bc6-388">Decimal</span><span class="sxs-lookup"><span data-stu-id="78bc6-388">Decimal</span></span> |
| <span data-ttu-id="78bc6-389">nchar</span><span class="sxs-lookup"><span data-stu-id="78bc6-389">nchar</span></span> |<span data-ttu-id="78bc6-390">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="78bc6-390">String, Char[]</span></span> |
| <span data-ttu-id="78bc6-391">ntext</span><span class="sxs-lookup"><span data-stu-id="78bc6-391">ntext</span></span> |<span data-ttu-id="78bc6-392">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="78bc6-392">String, Char[]</span></span> |
| <span data-ttu-id="78bc6-393">číselné</span><span class="sxs-lookup"><span data-stu-id="78bc6-393">numeric</span></span> |<span data-ttu-id="78bc6-394">Decimal</span><span class="sxs-lookup"><span data-stu-id="78bc6-394">Decimal</span></span> |
| <span data-ttu-id="78bc6-395">nvarchar</span><span class="sxs-lookup"><span data-stu-id="78bc6-395">nvarchar</span></span> |<span data-ttu-id="78bc6-396">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="78bc6-396">String, Char[]</span></span> |
| <span data-ttu-id="78bc6-397">skutečné</span><span class="sxs-lookup"><span data-stu-id="78bc6-397">real</span></span> |<span data-ttu-id="78bc6-398">Jeden</span><span class="sxs-lookup"><span data-stu-id="78bc6-398">Single</span></span> |
| <span data-ttu-id="78bc6-399">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="78bc6-399">rowversion</span></span> |<span data-ttu-id="78bc6-400">Byte]</span><span class="sxs-lookup"><span data-stu-id="78bc6-400">Byte[]</span></span> |
| <span data-ttu-id="78bc6-401">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="78bc6-401">smalldatetime</span></span> |<span data-ttu-id="78bc6-402">Data a času</span><span class="sxs-lookup"><span data-stu-id="78bc6-402">DateTime</span></span> |
| <span data-ttu-id="78bc6-403">smallint</span><span class="sxs-lookup"><span data-stu-id="78bc6-403">smallint</span></span> |<span data-ttu-id="78bc6-404">Int16</span><span class="sxs-lookup"><span data-stu-id="78bc6-404">Int16</span></span> |
| <span data-ttu-id="78bc6-405">Smallmoney</span><span class="sxs-lookup"><span data-stu-id="78bc6-405">smallmoney</span></span> |<span data-ttu-id="78bc6-406">Decimal</span><span class="sxs-lookup"><span data-stu-id="78bc6-406">Decimal</span></span> |
| <span data-ttu-id="78bc6-407">SQL_VARIANT</span><span class="sxs-lookup"><span data-stu-id="78bc6-407">sql_variant</span></span> |<span data-ttu-id="78bc6-408">Objekt *</span><span class="sxs-lookup"><span data-stu-id="78bc6-408">Object *</span></span> |
| <span data-ttu-id="78bc6-409">Text</span><span class="sxs-lookup"><span data-stu-id="78bc6-409">text</span></span> |<span data-ttu-id="78bc6-410">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="78bc6-410">String, Char[]</span></span> |
| <span data-ttu-id="78bc6-411">time</span><span class="sxs-lookup"><span data-stu-id="78bc6-411">time</span></span> |<span data-ttu-id="78bc6-412">Časový interval</span><span class="sxs-lookup"><span data-stu-id="78bc6-412">TimeSpan</span></span> |
| <span data-ttu-id="78bc6-413">časové razítko</span><span class="sxs-lookup"><span data-stu-id="78bc6-413">timestamp</span></span> |<span data-ttu-id="78bc6-414">Byte]</span><span class="sxs-lookup"><span data-stu-id="78bc6-414">Byte[]</span></span> |
| <span data-ttu-id="78bc6-415">tinyint</span><span class="sxs-lookup"><span data-stu-id="78bc6-415">tinyint</span></span> |<span data-ttu-id="78bc6-416">Bajtů</span><span class="sxs-lookup"><span data-stu-id="78bc6-416">Byte</span></span> |
| <span data-ttu-id="78bc6-417">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="78bc6-417">uniqueidentifier</span></span> |<span data-ttu-id="78bc6-418">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="78bc6-418">Guid</span></span> |
| <span data-ttu-id="78bc6-419">varbinary</span><span class="sxs-lookup"><span data-stu-id="78bc6-419">varbinary</span></span> |<span data-ttu-id="78bc6-420">Byte]</span><span class="sxs-lookup"><span data-stu-id="78bc6-420">Byte[]</span></span> |
| <span data-ttu-id="78bc6-421">varchar</span><span class="sxs-lookup"><span data-stu-id="78bc6-421">varchar</span></span> |<span data-ttu-id="78bc6-422">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="78bc6-422">String, Char[]</span></span> |
| <span data-ttu-id="78bc6-423">xml</span><span class="sxs-lookup"><span data-stu-id="78bc6-423">xml</span></span> |<span data-ttu-id="78bc6-424">XML</span><span class="sxs-lookup"><span data-stu-id="78bc6-424">Xml</span></span> |

## <a name="mapping-source-to-sink-columns"></a><span data-ttu-id="78bc6-425">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="78bc6-425">Mapping source to sink columns</span></span>
<span data-ttu-id="78bc6-426">Mapování sloupců z datové sady zdroje na sloupce ze sady jímku dat naleznete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="78bc6-426">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="78bc6-427">Opakovatelných kopie</span><span class="sxs-lookup"><span data-stu-id="78bc6-427">Repeatable copy</span></span>
<span data-ttu-id="78bc6-428">Při kopírování dat do databáze serveru SQL, připojí aktivitě kopírování dat do tabulky jímky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="78bc6-428">When copying data to SQL Server Database, the copy activity appends data to the sink table by default.</span></span> <span data-ttu-id="78bc6-429">Místo toho provést UPSERT, najdete v tématu [Repeatable zapisovat do SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) článku.</span><span class="sxs-lookup"><span data-stu-id="78bc6-429">To perform an UPSERT instead,  See [Repeatable write to SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="78bc6-430">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="78bc6-430">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="78bc6-431">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="78bc6-431">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="78bc6-432">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="78bc6-432">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="78bc6-433">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="78bc6-433">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="78bc6-434">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="78bc6-434">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="78bc6-435">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="78bc6-435">Performance and Tuning</span></span>
<span data-ttu-id="78bc6-436">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="78bc6-436">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
