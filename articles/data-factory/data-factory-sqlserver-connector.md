---
title: aaaMove tooand dat z SQL serveru | Microsoft Docs
description: "Další informace o způsobu toomove data do nebo ze systému SQL Server databáze tedy místní nebo virtuální počítač Azure pomocí Azure Data Factory."
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
ms.openlocfilehash: f0cccf56a670e62ec893d75052a81eb26d562050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a><span data-ttu-id="e3cf0-103">Přesunutí dat tooand z místní SQL Server nebo na IaaS (virtuální počítač Azure) pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="e3cf0-103">Move data tooand from SQL Server on-premises or on IaaS (Azure VM) using Azure Data Factory</span></span>
<span data-ttu-id="e3cf0-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z databáze SQL Server na místě.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from an on-premises SQL Server database.</span></span> <span data-ttu-id="e3cf0-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="e3cf0-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="e3cf0-106">Supported scenarios</span></span>
<span data-ttu-id="e3cf0-107">Může kopírovat data **z databáze systému SQL Server** toohello následující úložišť dat:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-107">You can copy data **from a SQL Server database** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="e3cf0-108">Data můžete zkopírovat z hello následující úložišť dat **databáze systému SQL Server tooa**:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-108">You can copy data from hello following data stores **tooa SQL Server database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a><span data-ttu-id="e3cf0-109">Podporované verze systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="e3cf0-109">Supported SQL Server versions</span></span>
<span data-ttu-id="e3cf0-110">Tato podpora konektoru systému SQL Server kopírování dat z / toohello následující verze instance hostovaná místně nebo v Azure IaaS pomocí ověřování SQL a ověřování systému Windows: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span><span class="sxs-lookup"><span data-stu-id="e3cf0-110">This SQL Server connector support copying data from/toohello following versions of instance hosted on-premises or in Azure IaaS using both SQL authentication and Windows authentication: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="e3cf0-111">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="e3cf0-111">Enabling connectivity</span></span>
<span data-ttu-id="e3cf0-112">Koncepty Hello a kroky potřebné pro připojení s SQL serveru hostované místně nebo v Azure IaaS (infrastruktura jako služba) virtuálních počítačů jsou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-112">hello concepts and steps needed for connecting with SQL Server hosted on-premises or in Azure IaaS (Infrastructure-as-a-Service) VMs are hello same.</span></span> <span data-ttu-id="e3cf0-113">V obou případech musíte toouse Brána pro správu dat pro připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-113">In both cases, you need toouse Data Management Gateway for connectivity.</span></span>

<span data-ttu-id="e3cf0-114">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-114">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span> <span data-ttu-id="e3cf0-115">Nastavení instance brány je nezbytný předpoklad pro připojení k systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-115">Setting up a gateway instance is a pre-requisite for connecting with SQL Server.</span></span>

<span data-ttu-id="e3cf0-116">Zatímco můžete bránu nainstalovat hello stejné na místní počítač nebo cloudové instance virtuálního počítače jako hello systému SQL Server pro lepší výkon, doporučujeme vám nainstalovat na samostatné počítače.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-116">While you can install gateway on hello same on-premises machine or cloud VM instance as hello SQL Server for better performance, we recommended that you install them on separate machines.</span></span> <span data-ttu-id="e3cf0-117">S hello brány a SQL Server na samostatné počítače snižuje kolize prostředků.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-117">Having hello gateway and SQL Server on separate machines reduces resource contention.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e3cf0-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="e3cf0-118">Getting started</span></span>
<span data-ttu-id="e3cf0-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z databáze SQL serveru místní pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-119">You can create a pipeline with a copy activity that moves data to/from an on-premises SQL Server database by using different tools/APIs.</span></span>

<span data-ttu-id="e3cf0-120">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="e3cf0-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="e3cf0-122">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="e3cf0-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="e3cf0-124">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="e3cf0-125">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-125">Create a **data factory**.</span></span> <span data-ttu-id="e3cf0-126">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="e3cf0-127">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="e3cf0-128">Například pokud data kopírujete tooan databáze systému SQL Server úložiště objektů blob v Azure, vytvoříte dvě propojené služby toolink databáze systému SQL Server a účet úložiště Azure tooyour služby data factory.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-128">For example, if you are copying data from a SQL Server database tooan Azure blob storage, you create two linked services toolink your SQL Server database and Azure storage account tooyour data factory.</span></span> <span data-ttu-id="e3cf0-129">Vlastnosti propojené služby, které jsou specifické tooSQL databáze serveru, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-129">For linked service properties that are specific tooSQL Server database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="e3cf0-130">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-130">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="e3cf0-131">V příkladu hello uvedených v posledním kroku hello vytvořit tabulku datovou sadu toospecify hello SQL v databázi systému SQL Server, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-131">In hello example mentioned in hello last step, you create a dataset toospecify hello SQL table in your SQL Server database that contains hello input data.</span></span> <span data-ttu-id="e3cf0-132">Vytvoření kontejneru objektů blob hello toospecify s jinou datovou sadu a hello složky, která obsahuje hello data zkopírovaných z hello databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-132">And, you create another dataset toospecify hello blob container and hello folder that holds hello data copied from hello SQL Server database.</span></span> <span data-ttu-id="e3cf0-133">Vlastnosti datové sady, které jsou konkrétní tooSQL databáze serveru, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-133">For dataset properties that are specific tooSQL Server database, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="e3cf0-134">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="e3cf0-135">V příkladu hello již bylo zmíněno dříve použijete SqlSource jako zdroj a BlobSink jako jímku pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-135">In hello example mentioned earlier, you use SqlSource as a source and BlobSink as a sink for hello copy activity.</span></span> <span data-ttu-id="e3cf0-136">Podobně pokud zkopírujete z Azure Blob Storage tooSQL databáze serveru, použijte BlobSource a SqlSink v aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-136">Similarly, if you are copying from Azure Blob Storage tooSQL Server Database, you use BlobSource and SqlSink in hello copy activity.</span></span> <span data-ttu-id="e3cf0-137">Vlastnosti aktivity kopírování, které jsou specifické tooSQL databáze serveru, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-137">For copy activity properties that are specific tooSQL Server Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="e3cf0-138">Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-138">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span> 

<span data-ttu-id="e3cf0-139">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-139">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="e3cf0-140">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="e3cf0-141">Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo z místní databáze systému SQL Server naleznete v části [JSON příklady](#json-examples-for-copying-data-from-and-to-sql-server) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-141">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an on-premises SQL Server database, see [JSON examples](#json-examples-for-copying-data-from-and-to-sql-server) section of this article.</span></span> 

<span data-ttu-id="e3cf0-142">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooSQL serveru:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-142">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooSQL Server:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="e3cf0-143">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="e3cf0-143">Linked service properties</span></span>
<span data-ttu-id="e3cf0-144">Vytvoření propojené služby typu **onpremisessqlserver** toolink služby místní systém SQL Server databáze tooa data factory.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-144">You create a linked service of type **OnPremisesSqlServer** toolink an on-premises SQL Server database tooa data factory.</span></span> <span data-ttu-id="e3cf0-145">Hello následující tabulka obsahuje popis služby SQL serveru propojená konkrétní tooon místní elementy JSON.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-145">hello following table provides description for JSON elements specific tooon-premises SQL Server linked service.</span></span>

<span data-ttu-id="e3cf0-146">Hello následující tabulka obsahuje popis pro konkrétní tooSQL elementy JSON serveru propojené služby.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-146">hello following table provides description for JSON elements specific tooSQL Server linked service.</span></span>

| <span data-ttu-id="e3cf0-147">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="e3cf0-147">Property</span></span> | <span data-ttu-id="e3cf0-148">Popis</span><span class="sxs-lookup"><span data-stu-id="e3cf0-148">Description</span></span> | <span data-ttu-id="e3cf0-149">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3cf0-149">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e3cf0-150">type</span><span class="sxs-lookup"><span data-stu-id="e3cf0-150">type</span></span> |<span data-ttu-id="e3cf0-151">vlastnost typu Hello by měla být nastavena na: **onpremisessqlserver**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-151">hello type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="e3cf0-152">Ano</span><span class="sxs-lookup"><span data-stu-id="e3cf0-152">Yes</span></span> |
| <span data-ttu-id="e3cf0-153">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="e3cf0-153">connectionString</span></span> |<span data-ttu-id="e3cf0-154">Zadejte požadované informace connectionString tooconnect toohello místní databáze SQL serveru pomocí ověřování SQL nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-154">Specify connectionString information needed tooconnect toohello on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="e3cf0-155">Ano</span><span class="sxs-lookup"><span data-stu-id="e3cf0-155">Yes</span></span> |
| <span data-ttu-id="e3cf0-156">gatewayName</span><span class="sxs-lookup"><span data-stu-id="e3cf0-156">gatewayName</span></span> |<span data-ttu-id="e3cf0-157">Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-157">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SQL Server database.</span></span> |<span data-ttu-id="e3cf0-158">Ano</span><span class="sxs-lookup"><span data-stu-id="e3cf0-158">Yes</span></span> |
| <span data-ttu-id="e3cf0-159">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="e3cf0-159">username</span></span> |<span data-ttu-id="e3cf0-160">Zadejte uživatelské jméno, pokud používáte ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-160">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="e3cf0-161">Příklad: **domainname\\uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-161">Example: **domainname\\username**.</span></span> |<span data-ttu-id="e3cf0-162">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-162">No</span></span> |
| <span data-ttu-id="e3cf0-163">heslo</span><span class="sxs-lookup"><span data-stu-id="e3cf0-163">password</span></span> |<span data-ttu-id="e3cf0-164">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-164">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="e3cf0-165">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-165">No</span></span> |

<span data-ttu-id="e3cf0-166">Můžete šifrovat přihlašovací údaje pomocí hello **New-AzureRmDataFactoryEncryptValue** rutiny a použijte je v hello připojovací řetězec, jak ukazuje následující příklad hello (**EncryptedCredential** vlastnost):</span><span class="sxs-lookup"><span data-stu-id="e3cf0-166">You can encrypt credentials using hello **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in hello connection string as shown in hello following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a><span data-ttu-id="e3cf0-167">Ukázky</span><span class="sxs-lookup"><span data-stu-id="e3cf0-167">Samples</span></span>
<span data-ttu-id="e3cf0-168">**JSON pro pomocí ověřování SQL.**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-168">**JSON for using SQL Authentication**</span></span>

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
<span data-ttu-id="e3cf0-169">**JSON pro použití ověřování systému Windows**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-169">**JSON for using Windows Authentication**</span></span>

<span data-ttu-id="e3cf0-170">Brána pro správu dat se zosobnit hello zadaný uživatel účet tooconnect toohello místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-170">Data Management Gateway will impersonate hello specified user account tooconnect toohello on-premises SQL Server database.</span></span> 

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

## <a name="dataset-properties"></a><span data-ttu-id="e3cf0-171">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="e3cf0-171">Dataset properties</span></span>
<span data-ttu-id="e3cf0-172">V hello ukázky jste použili datovou sadu typu **SqlServerTable** toorepresent tabulky v databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-172">In hello samples, you have used a dataset of type **SqlServerTable** toorepresent a table in a SQL Server database.</span></span>  

<span data-ttu-id="e3cf0-173">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-173">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="e3cf0-174">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (SQL Server, objektů blob v Azure, Azure table atd.).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (SQL Server, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="e3cf0-175">část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-175">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="e3cf0-176">Hello **rámci typeProperties** části pro datovou sadu hello typu **SqlServerTable** má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-176">hello **typeProperties** section for hello dataset of type **SqlServerTable** has hello following properties:</span></span>

| <span data-ttu-id="e3cf0-177">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="e3cf0-177">Property</span></span> | <span data-ttu-id="e3cf0-178">Popis</span><span class="sxs-lookup"><span data-stu-id="e3cf0-178">Description</span></span> | <span data-ttu-id="e3cf0-179">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3cf0-179">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e3cf0-180">tableName</span><span class="sxs-lookup"><span data-stu-id="e3cf0-180">tableName</span></span> |<span data-ttu-id="e3cf0-181">Název hello tabulku nebo zobrazení v hello instance databáze SQL serveru, který propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-181">Name of hello table or view in hello SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="e3cf0-182">Ano</span><span class="sxs-lookup"><span data-stu-id="e3cf0-182">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="e3cf0-183">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="e3cf0-183">Copy activity properties</span></span>
<span data-ttu-id="e3cf0-184">Pokud přesouváte data z databáze systému SQL Server, nastavíte typ zdroje hello v aktivitě kopírování hello příliš**SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-184">If you are moving data from a SQL Server database, you set hello source type in hello copy activity too**SqlSource**.</span></span> <span data-ttu-id="e3cf0-185">Podobně pokud přesouváte databázi systému SQL Server tooa dat, nastavíte typ jímky hello v aktivitě kopírování hello příliš**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-185">Similarly, if you are moving data tooa SQL Server database, you set hello sink type in hello copy activity too**SqlSink**.</span></span> <span data-ttu-id="e3cf0-186">Tato část obsahuje seznam vlastností, které jsou podporované SqlSource a SqlSink.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-186">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

<span data-ttu-id="e3cf0-187">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-187">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="e3cf0-188">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-188">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="e3cf0-189">Hello aktivity kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-189">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="e3cf0-190">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-190">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="e3cf0-191">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-191">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="e3cf0-192">SqlSource</span><span class="sxs-lookup"><span data-stu-id="e3cf0-192">SqlSource</span></span>
<span data-ttu-id="e3cf0-193">Pokud zdroj v aktivitě kopírování je typu **SqlSource**, hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-193">When source in a copy activity is of type **SqlSource**, hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="e3cf0-194">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="e3cf0-194">Property</span></span> | <span data-ttu-id="e3cf0-195">Popis</span><span class="sxs-lookup"><span data-stu-id="e3cf0-195">Description</span></span> | <span data-ttu-id="e3cf0-196">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="e3cf0-196">Allowed values</span></span> | <span data-ttu-id="e3cf0-197">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3cf0-197">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e3cf0-198">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="e3cf0-198">sqlReaderQuery</span></span> |<span data-ttu-id="e3cf0-199">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-199">Use hello custom query tooread data.</span></span> |<span data-ttu-id="e3cf0-200">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-200">SQL query string.</span></span> <span data-ttu-id="e3cf0-201">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-201">For example: select * from MyTable.</span></span> <span data-ttu-id="e3cf0-202">Může odkazovat více tabulek z databáze hello odkazuje hello vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-202">May reference multiple tables from hello database referenced by hello input dataset.</span></span> <span data-ttu-id="e3cf0-203">Pokud není zadaný, hello příkaz jazyka SQL, která se provedla: Vyberte možnost z MyTable.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-203">If not specified, hello SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="e3cf0-204">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-204">No</span></span> |
| <span data-ttu-id="e3cf0-205">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="e3cf0-205">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="e3cf0-206">Název hello uložené procedury, která čte data z hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-206">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="e3cf0-207">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-207">Name of hello stored procedure.</span></span> <span data-ttu-id="e3cf0-208">Hello poslední příkaz jazyka SQL musí být příkaz SELECT v hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-208">hello last SQL statement must be a SELECT statement in hello stored procedure.</span></span> |<span data-ttu-id="e3cf0-209">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-209">No</span></span> |
| <span data-ttu-id="e3cf0-210">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="e3cf0-210">storedProcedureParameters</span></span> |<span data-ttu-id="e3cf0-211">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-211">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="e3cf0-212">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-212">Name/value pairs.</span></span> <span data-ttu-id="e3cf0-213">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-213">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="e3cf0-214">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-214">No</span></span> |

<span data-ttu-id="e3cf0-215">Pokud hello **sqlReaderQuery** je zadán pro hello SqlSource, hello aktivity kopírování spouští tento dotaz hello databáze systému SQL Server zdrojová tooget hello data.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-215">If hello **sqlReaderQuery** is specified for hello SqlSource, hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span>

<span data-ttu-id="e3cf0-216">Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-216">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="e3cf0-217">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello jsou použité toobuild dotaz select toorun proti hello databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-217">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="e3cf0-218">Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-218">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="e3cf0-219">Při použití **sqlReaderStoredProcedureName**, stále potřebujete toospecify hodnotu pro hello **tableName** vlastnost v datové sadě hello JSON.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-219">When you use **sqlReaderStoredProcedureName**, you still need toospecify a value for hello **tableName** property in hello dataset JSON.</span></span> <span data-ttu-id="e3cf0-220">Neexistují žádné ověření, ale adresovat této tabulky.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-220">There are no validations performed against this table though.</span></span>

### <a name="sqlsink"></a><span data-ttu-id="e3cf0-221">SqlSink</span><span class="sxs-lookup"><span data-stu-id="e3cf0-221">SqlSink</span></span>
<span data-ttu-id="e3cf0-222">**SqlSink** podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-222">**SqlSink** supports hello following properties:</span></span>

| <span data-ttu-id="e3cf0-223">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="e3cf0-223">Property</span></span> | <span data-ttu-id="e3cf0-224">Popis</span><span class="sxs-lookup"><span data-stu-id="e3cf0-224">Description</span></span> | <span data-ttu-id="e3cf0-225">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="e3cf0-225">Allowed values</span></span> | <span data-ttu-id="e3cf0-226">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e3cf0-226">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e3cf0-227">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="e3cf0-227">writeBatchTimeout</span></span> |<span data-ttu-id="e3cf0-228">Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-228">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="e3cf0-229">Časový interval</span><span class="sxs-lookup"><span data-stu-id="e3cf0-229">timespan</span></span><br/><br/> <span data-ttu-id="e3cf0-230">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-230">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="e3cf0-231">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-231">No</span></span> |
| <span data-ttu-id="e3cf0-232">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="e3cf0-232">writeBatchSize</span></span> |<span data-ttu-id="e3cf0-233">Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-233">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="e3cf0-234">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="e3cf0-234">Integer (number of rows)</span></span> |<span data-ttu-id="e3cf0-235">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="e3cf0-235">No (default: 10000)</span></span> |
| <span data-ttu-id="e3cf0-236">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="e3cf0-236">sqlWriterCleanupScript</span></span> |<span data-ttu-id="e3cf0-237">Zadejte dotaz pro aktivitu kopírování tooexecute tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-237">Specify query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="e3cf0-238">Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy) části.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-238">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="e3cf0-239">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-239">A query statement.</span></span> |<span data-ttu-id="e3cf0-240">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-240">No</span></span> |
| <span data-ttu-id="e3cf0-241">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="e3cf0-241">sliceIdentifierColumnName</span></span> |<span data-ttu-id="e3cf0-242">Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-242">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> <span data-ttu-id="e3cf0-243">Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy) části.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-243">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="e3cf0-244">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-244">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="e3cf0-245">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-245">No</span></span> |
| <span data-ttu-id="e3cf0-246">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="e3cf0-246">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="e3cf0-247">Název hello uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-247">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="e3cf0-248">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-248">Name of hello stored procedure.</span></span> |<span data-ttu-id="e3cf0-249">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-249">No</span></span> |
| <span data-ttu-id="e3cf0-250">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="e3cf0-250">storedProcedureParameters</span></span> |<span data-ttu-id="e3cf0-251">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-251">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="e3cf0-252">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-252">Name/value pairs.</span></span> <span data-ttu-id="e3cf0-253">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-253">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="e3cf0-254">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-254">No</span></span> |
| <span data-ttu-id="e3cf0-255">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="e3cf0-255">sqlWriterTableType</span></span> |<span data-ttu-id="e3cf0-256">Zadejte toobe název typu tabulky používán hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-256">Specify table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="e3cf0-257">Aktivita kopírování zpřístupní přesouvání dat hello v dočasné tabulce s tímto typem tabulky.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-257">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="e3cf0-258">Uložená procedura kódu můžete pak sloučit data hello kopírovány s existujícími daty.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-258">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="e3cf0-259">Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-259">A table type name.</span></span> |<span data-ttu-id="e3cf0-260">Ne</span><span class="sxs-lookup"><span data-stu-id="e3cf0-260">No</span></span> |


## <a name="json-examples-for-copying-data-from-and-toosql-server"></a><span data-ttu-id="e3cf0-261">Příklady JSON ke kopírování dat z a tooSQL serveru</span><span class="sxs-lookup"><span data-stu-id="e3cf0-261">JSON examples for copying data from and tooSQL Server</span></span>
<span data-ttu-id="e3cf0-262">Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-262">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="e3cf0-263">Dobrý den, jak následující ukázky zobrazit toocopy tooand dat z SQL serveru a Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-263">hello following samples show how toocopy data tooand from SQL Server and Azure Blob Storage.</span></span> <span data-ttu-id="e3cf0-264">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-264">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>     

## <a name="example-copy-data-from-sql-server-tooazure-blob"></a><span data-ttu-id="e3cf0-265">Příklad: Kopírování dat z SQL serveru tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="e3cf0-265">Example: Copy data from SQL Server tooAzure Blob</span></span>
<span data-ttu-id="e3cf0-266">Následující ukázka ukazuje Hello:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-266">hello following sample shows:</span></span>

1. <span data-ttu-id="e3cf0-267">Propojené služby typu [onpremisessqlserver](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-267">A linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="e3cf0-268">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-268">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="e3cf0-269">Vstup [datovou sadu](data-factory-create-datasets.md) typu [SqlServerTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-269">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](#dataset-properties).</span></span>
4. <span data-ttu-id="e3cf0-270">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-270">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="e3cf0-271">Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-271">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="e3cf0-272">Ukázka Hello zkopíruje data časové řady z tabulky tooan SQL Server objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-272">hello sample copies time-series data from a SQL Server table tooan Azure blob every hour.</span></span> <span data-ttu-id="e3cf0-273">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-273">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="e3cf0-274">Jako první krok nastavte Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-274">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="e3cf0-275">Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-275">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="e3cf0-276">**Služba SQL Server propojené**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-276">**SQL Server linked service**</span></span>
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
<span data-ttu-id="e3cf0-277">**Objekt Blob propojená služba Azure storage**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-277">**Azure Blob storage linked service**</span></span>

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
<span data-ttu-id="e3cf0-278">**Vstupní datové sady SQL Server**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-278">**SQL Server input dataset**</span></span>

<span data-ttu-id="e3cf0-279">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v systému SQL Server a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-279">hello sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="e3cf0-280">Můžete dotazovat přes více tabulek v rámci hello stejnou databázi pomocí jednu datovou sadu, ale jedné tabulky musí být použita pro typeProperty tableName hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-280">You can query over multiple tables within hello same database using a single dataset, but a single table must be used for hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="e3cf0-281">Nastavení "externí": "PRAVDA" informuje služba Data Factory tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-281">Setting “external”: ”true” informs Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="e3cf0-282">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-282">**Azure Blob output dataset**</span></span>

<span data-ttu-id="e3cf0-283">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-283">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="e3cf0-284">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-284">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="e3cf0-285">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-285">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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
<span data-ttu-id="e3cf0-286">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-286">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="e3cf0-287">Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-287">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="e3cf0-288">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-288">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="e3cf0-289">Dotaz SQL Hello zadaný pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-289">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

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
<span data-ttu-id="e3cf0-290">V tomto příkladu **sqlReaderQuery** pro hello SqlSource je zadána.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-290">In this example, **sqlReaderQuery** is specified for hello SqlSource.</span></span> <span data-ttu-id="e3cf0-291">Hello aktivity kopírování spouští tento dotaz hello data hello tooget zdrojové databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-291">hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span> <span data-ttu-id="e3cf0-292">Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-292">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span> <span data-ttu-id="e3cf0-293">Hello sqlReaderQuery může odkazovat více tabulek v rámci hello databáze odkazuje hello vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-293">hello sqlReaderQuery can reference multiple tables within hello database referenced by hello input dataset.</span></span> <span data-ttu-id="e3cf0-294">Není omezený tooonly hello tabulky nastavena jako hello typeProperty tableName datové sady.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-294">It is not limited tooonly hello table set as hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="e3cf0-295">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello jsou použité toobuild dotaz select toorun proti hello databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-295">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="e3cf0-296">Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-296">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

<span data-ttu-id="e3cf0-297">V tématu hello [zdroje Sql](#sqlsource) části a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem SqlSource a BlobSink.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-297">See hello [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for hello list of properties supported by SqlSource and BlobSink.</span></span>

## <a name="example-copy-data-from-azure-blob-toosql-server"></a><span data-ttu-id="e3cf0-298">Příklad: Kopírování dat z Azure Blob tooSQL serveru</span><span class="sxs-lookup"><span data-stu-id="e3cf0-298">Example: Copy data from Azure Blob tooSQL Server</span></span>
<span data-ttu-id="e3cf0-299">Následující ukázka ukazuje Hello:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-299">hello following sample shows:</span></span>

1. <span data-ttu-id="e3cf0-300">Hello propojené služby typu [onpremisessqlserver](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-300">hello linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="e3cf0-301">Hello propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-301">hello linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="e3cf0-302">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-302">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="e3cf0-303">Výstup [datovou sadu](data-factory-create-datasets.md) typu [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-303">An output [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="e3cf0-304">Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [SqlSink](#sql-server-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-304">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#sql-server-copy-activity-type-properties).</span></span>

<span data-ttu-id="e3cf0-305">Ukázka Hello zkopíruje data časové řady z tabulky serveru SQL Server tooa objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-305">hello sample copies time-series data from an Azure blob tooa SQL Server table every hour.</span></span> <span data-ttu-id="e3cf0-306">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-306">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="e3cf0-307">**Služba SQL Server propojené**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-307">**SQL Server linked service**</span></span>

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
<span data-ttu-id="e3cf0-308">**Objekt Blob propojená služba Azure storage**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-308">**Azure Blob storage linked service**</span></span>

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
<span data-ttu-id="e3cf0-309">**Azure vstupní datovou sadu objektu Blob**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-309">**Azure Blob input dataset**</span></span>

<span data-ttu-id="e3cf0-310">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-310">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="e3cf0-311">název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-311">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="e3cf0-312">Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-312">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="e3cf0-313">"externí": "PRAVDA" nastavení informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-313">“external”: “true” setting informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="e3cf0-314">**Výstupní datové sady SQL Server**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-314">**SQL Server output dataset**</span></span>

<span data-ttu-id="e3cf0-315">Ukázka Hello zkopíruje data tooa tabulku s názvem "MyTable" v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-315">hello sample copies data tooa table named “MyTable” in SQL Server.</span></span> <span data-ttu-id="e3cf0-316">Vytvoření tabulky hello v systému SQL Server s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-316">Create hello table in SQL Server with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="e3cf0-317">Přidávání řádků tabulky toohello každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-317">New rows are added toohello table every hour.</span></span>

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
<span data-ttu-id="e3cf0-318">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-318">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="e3cf0-319">Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-319">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="e3cf0-320">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-320">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

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

## <a name="troubleshooting-connection-issues"></a><span data-ttu-id="e3cf0-321">Odstraňování potíží s připojením</span><span class="sxs-lookup"><span data-stu-id="e3cf0-321">Troubleshooting connection issues</span></span>
1. <span data-ttu-id="e3cf0-322">Konfigurace vzdáleného připojení tooaccept systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-322">Configure your SQL Server tooaccept remote connections.</span></span> <span data-ttu-id="e3cf0-323">Spusťte **SQL Server Management Studio**, klikněte pravým tlačítkem na **server**a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-323">Launch **SQL Server Management Studio**, right-click **server**, and click **Properties**.</span></span> <span data-ttu-id="e3cf0-324">Vyberte **připojení** ze seznamu hello a zkontrolujte **povolit vzdálená připojení toohello server**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-324">Select **Connections** from hello list and check **Allow remote connections toohello server**.</span></span>

    ![Povolit vzdálená připojení](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    <span data-ttu-id="e3cf0-326">V tématu [konfigurace vzdáleného přístupu hello možnosti konfigurace serveru](https://msdn.microsoft.com/library/ms191464.aspx) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-326">See [Configure hello remote access Server Configuration Option](https://msdn.microsoft.com/library/ms191464.aspx) for detailed steps.</span></span>
2. <span data-ttu-id="e3cf0-327">Spusťte **Správce konfigurace systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-327">Launch **SQL Server Configuration Manager**.</span></span> <span data-ttu-id="e3cf0-328">Rozbalte položku **konfigurace sítě serveru SQL Server** pro hello instance je chcete a vyberte **protokoly pro MSSQLSERVER**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-328">Expand **SQL Server Network Configuration** for hello instance you want, and select **Protocols for MSSQLSERVER**.</span></span> <span data-ttu-id="e3cf0-329">Měli byste vidět protokolů v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-329">You should see protocols in hello right-pane.</span></span> <span data-ttu-id="e3cf0-330">Povolte protokol TCP/IP kliknutím pravým tlačítkem na **TCP/IP** a kliknutím na **povolit**.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-330">Enable TCP/IP by right-clicking **TCP/IP** and clicking **Enable**.</span></span>

    ![Povolte protokol TCP/IP](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    <span data-ttu-id="e3cf0-332">V tématu [povolit nebo zakázat síťový protokol serveru](https://msdn.microsoft.com/library/ms191294.aspx) podrobnosti a alternativní způsoby povolení protokolu TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-332">See [Enable or Disable a Server Network Protocol](https://msdn.microsoft.com/library/ms191294.aspx) for details and alternate ways of enabling TCP/IP protocol.</span></span>
3. <span data-ttu-id="e3cf0-333">Ve stejném okně hello, dvakrát klikněte na **TCP/IP** toolaunch **vlastností protokolu TCP/IP** okno.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-333">In hello same window, double-click **TCP/IP** toolaunch **TCP/IP Properties** window.</span></span>
4. <span data-ttu-id="e3cf0-334">Přepínač toohello **IP adresy** kartě. Projděte dolů toosee **IPAll** části.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-334">Switch toohello **IP Addresses** tab. Scroll down toosee **IPAll** section.</span></span> <span data-ttu-id="e3cf0-335">Zapište hello ** TCP Port ** (výchozí hodnota je **1433**).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-335">Note down hello **TCP Port **(default is **1433**).</span></span>
5. <span data-ttu-id="e3cf0-336">Vytvoření **pravidlo pro hello brány Windows Firewall** pro příchozí provoz tooallow hello počítače pomocí tohoto portu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-336">Create a **rule for hello Windows Firewall** on hello machine tooallow incoming traffic through this port.</span></span>  
6. <span data-ttu-id="e3cf0-337">**Ověření připojení**: tooconnect toohello pomocí plně kvalifikovaného názvu SQL serveru použít SQL Server Management Studio z jiný počítač.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-337">**Verify connection**: tooconnect toohello SQL Server using fully qualified name, use SQL Server Management Studio from a different machine.</span></span> <span data-ttu-id="e3cf0-338">Například: "<machine>.<domain>. Corp.<company>.com, 1433. "</span><span class="sxs-lookup"><span data-stu-id="e3cf0-338">For example: "<machine>.<domain>.corp.<company>.com,1433."</span></span>

   > [!IMPORTANT]

   > <span data-ttu-id="e3cf0-339">V tématu [přesun dat mezi místní zdroje a hello cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md) podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-339">See [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for detailed information.</span></span>
   >
   > <span data-ttu-id="e3cf0-340">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-340">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>
   >
   >


## <a name="identity-columns-in-hello-target-database"></a><span data-ttu-id="e3cf0-341">Sloupce identity v hello cílová databáze</span><span class="sxs-lookup"><span data-stu-id="e3cf0-341">Identity columns in hello target database</span></span>
<span data-ttu-id="e3cf0-342">Tato část poskytuje příklad, který kopíruje data ze zdrojové tabulky s žádné identity sloupec tooa cílové tabulky se sloupcem identity.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-342">This section provides an example that copies data from a source table with no identity column tooa destination table with an identity column.</span></span>

<span data-ttu-id="e3cf0-343">**Zdrojová tabulka:**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-343">**Source table:**</span></span>

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="e3cf0-344">**Cílové tabulky:**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-344">**Destination table:**</span></span>

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

<span data-ttu-id="e3cf0-345">Všimněte si, že hello cílová tabulka obsahuje sloupec identity.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-345">Notice that hello target table has an identity column.</span></span>

<span data-ttu-id="e3cf0-346">**Definice JSON datové sady zdroje**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-346">**Source dataset JSON definition**</span></span>

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
<span data-ttu-id="e3cf0-347">**Cílový definici JSON datové sady**</span><span class="sxs-lookup"><span data-stu-id="e3cf0-347">**Destination dataset JSON definition**</span></span>

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

<span data-ttu-id="e3cf0-348">Všimněte si, že jako zdrojové a cílové tabulky jiné schéma (cíl má sloupec s identitou).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-348">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="e3cf0-349">V tomto scénáři budete potřebovat toospecify **struktura** vlastnost v definici datové sady cíl hello, který neobsahuje sloupec identity hello.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-349">In this scenario, you need toospecify **structure** property in hello target dataset definition, which doesn’t include hello identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="e3cf0-350">Volání uložené procedury jímku SQL</span><span class="sxs-lookup"><span data-stu-id="e3cf0-350">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="e3cf0-351">V tématu [vyvolat uloženou proceduru SQL jímka v aktivitě kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md) článku příklad volání uložené procedury z jímku SQL při aktivitě kopírování kanálu.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-351">See [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article for an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline.</span></span>

## <a name="type-mapping-for-sql-server"></a><span data-ttu-id="e3cf0-352">Mapování typu pro SQL server</span><span class="sxs-lookup"><span data-stu-id="e3cf0-352">Type mapping for SQL server</span></span>
<span data-ttu-id="e3cf0-353">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku hello aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:</span><span class="sxs-lookup"><span data-stu-id="e3cf0-353">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, hello Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="e3cf0-354">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="e3cf0-354">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="e3cf0-355">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="e3cf0-355">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="e3cf0-356">Při přesunutí dat příliš & ze serveru SQL server hello se používají následující mapování z typu too.NET typ SQL a naopak.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-356">When moving data too& from SQL server, hello following mappings are used from SQL type too.NET type and vice versa.</span></span>

<span data-ttu-id="e3cf0-357">mapování Hello je stejný jako hello mapování datového typu aplikace SQL Server pro technologii ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-357">hello mapping is same as hello SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="e3cf0-358">Typ databázového stroje SQL Server</span><span class="sxs-lookup"><span data-stu-id="e3cf0-358">SQL Server Database Engine type</span></span> | <span data-ttu-id="e3cf0-359">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="e3cf0-359">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="e3cf0-360">bigint</span><span class="sxs-lookup"><span data-stu-id="e3cf0-360">bigint</span></span> |<span data-ttu-id="e3cf0-361">Int64</span><span class="sxs-lookup"><span data-stu-id="e3cf0-361">Int64</span></span> |
| <span data-ttu-id="e3cf0-362">Binární</span><span class="sxs-lookup"><span data-stu-id="e3cf0-362">binary</span></span> |<span data-ttu-id="e3cf0-363">Byte]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-363">Byte[]</span></span> |
| <span data-ttu-id="e3cf0-364">Bit</span><span class="sxs-lookup"><span data-stu-id="e3cf0-364">bit</span></span> |<span data-ttu-id="e3cf0-365">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="e3cf0-365">Boolean</span></span> |
| <span data-ttu-id="e3cf0-366">Char</span><span class="sxs-lookup"><span data-stu-id="e3cf0-366">char</span></span> |<span data-ttu-id="e3cf0-367">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-367">String, Char[]</span></span> |
| <span data-ttu-id="e3cf0-368">Datum</span><span class="sxs-lookup"><span data-stu-id="e3cf0-368">date</span></span> |<span data-ttu-id="e3cf0-369">Data a času</span><span class="sxs-lookup"><span data-stu-id="e3cf0-369">DateTime</span></span> |
| <span data-ttu-id="e3cf0-370">Data a času</span><span class="sxs-lookup"><span data-stu-id="e3cf0-370">Datetime</span></span> |<span data-ttu-id="e3cf0-371">Data a času</span><span class="sxs-lookup"><span data-stu-id="e3cf0-371">DateTime</span></span> |
| <span data-ttu-id="e3cf0-372">datetime2</span><span class="sxs-lookup"><span data-stu-id="e3cf0-372">datetime2</span></span> |<span data-ttu-id="e3cf0-373">Data a času</span><span class="sxs-lookup"><span data-stu-id="e3cf0-373">DateTime</span></span> |
| <span data-ttu-id="e3cf0-374">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="e3cf0-374">Datetimeoffset</span></span> |<span data-ttu-id="e3cf0-375">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="e3cf0-375">DateTimeOffset</span></span> |
| <span data-ttu-id="e3cf0-376">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3cf0-376">Decimal</span></span> |<span data-ttu-id="e3cf0-377">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3cf0-377">Decimal</span></span> |
| <span data-ttu-id="e3cf0-378">Atribut FILESTREAM (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="e3cf0-378">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="e3cf0-379">Byte]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-379">Byte[]</span></span> |
| <span data-ttu-id="e3cf0-380">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="e3cf0-380">Float</span></span> |<span data-ttu-id="e3cf0-381">Double</span><span class="sxs-lookup"><span data-stu-id="e3cf0-381">Double</span></span> |
| <span data-ttu-id="e3cf0-382">Bitové kopie</span><span class="sxs-lookup"><span data-stu-id="e3cf0-382">image</span></span> |<span data-ttu-id="e3cf0-383">Byte]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-383">Byte[]</span></span> |
| <span data-ttu-id="e3cf0-384">celá čísla</span><span class="sxs-lookup"><span data-stu-id="e3cf0-384">int</span></span> |<span data-ttu-id="e3cf0-385">Int32</span><span class="sxs-lookup"><span data-stu-id="e3cf0-385">Int32</span></span> |
| <span data-ttu-id="e3cf0-386">peníze</span><span class="sxs-lookup"><span data-stu-id="e3cf0-386">money</span></span> |<span data-ttu-id="e3cf0-387">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3cf0-387">Decimal</span></span> |
| <span data-ttu-id="e3cf0-388">nchar</span><span class="sxs-lookup"><span data-stu-id="e3cf0-388">nchar</span></span> |<span data-ttu-id="e3cf0-389">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-389">String, Char[]</span></span> |
| <span data-ttu-id="e3cf0-390">ntext</span><span class="sxs-lookup"><span data-stu-id="e3cf0-390">ntext</span></span> |<span data-ttu-id="e3cf0-391">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-391">String, Char[]</span></span> |
| <span data-ttu-id="e3cf0-392">číselné</span><span class="sxs-lookup"><span data-stu-id="e3cf0-392">numeric</span></span> |<span data-ttu-id="e3cf0-393">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3cf0-393">Decimal</span></span> |
| <span data-ttu-id="e3cf0-394">nvarchar</span><span class="sxs-lookup"><span data-stu-id="e3cf0-394">nvarchar</span></span> |<span data-ttu-id="e3cf0-395">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-395">String, Char[]</span></span> |
| <span data-ttu-id="e3cf0-396">skutečné</span><span class="sxs-lookup"><span data-stu-id="e3cf0-396">real</span></span> |<span data-ttu-id="e3cf0-397">Jeden</span><span class="sxs-lookup"><span data-stu-id="e3cf0-397">Single</span></span> |
| <span data-ttu-id="e3cf0-398">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="e3cf0-398">rowversion</span></span> |<span data-ttu-id="e3cf0-399">Byte]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-399">Byte[]</span></span> |
| <span data-ttu-id="e3cf0-400">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="e3cf0-400">smalldatetime</span></span> |<span data-ttu-id="e3cf0-401">Data a času</span><span class="sxs-lookup"><span data-stu-id="e3cf0-401">DateTime</span></span> |
| <span data-ttu-id="e3cf0-402">smallint</span><span class="sxs-lookup"><span data-stu-id="e3cf0-402">smallint</span></span> |<span data-ttu-id="e3cf0-403">Int16</span><span class="sxs-lookup"><span data-stu-id="e3cf0-403">Int16</span></span> |
| <span data-ttu-id="e3cf0-404">Smallmoney</span><span class="sxs-lookup"><span data-stu-id="e3cf0-404">smallmoney</span></span> |<span data-ttu-id="e3cf0-405">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3cf0-405">Decimal</span></span> |
| <span data-ttu-id="e3cf0-406">SQL_VARIANT</span><span class="sxs-lookup"><span data-stu-id="e3cf0-406">sql_variant</span></span> |<span data-ttu-id="e3cf0-407">Objekt *</span><span class="sxs-lookup"><span data-stu-id="e3cf0-407">Object *</span></span> |
| <span data-ttu-id="e3cf0-408">Text</span><span class="sxs-lookup"><span data-stu-id="e3cf0-408">text</span></span> |<span data-ttu-id="e3cf0-409">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-409">String, Char[]</span></span> |
| <span data-ttu-id="e3cf0-410">time</span><span class="sxs-lookup"><span data-stu-id="e3cf0-410">time</span></span> |<span data-ttu-id="e3cf0-411">Časový interval</span><span class="sxs-lookup"><span data-stu-id="e3cf0-411">TimeSpan</span></span> |
| <span data-ttu-id="e3cf0-412">časové razítko</span><span class="sxs-lookup"><span data-stu-id="e3cf0-412">timestamp</span></span> |<span data-ttu-id="e3cf0-413">Byte]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-413">Byte[]</span></span> |
| <span data-ttu-id="e3cf0-414">tinyint</span><span class="sxs-lookup"><span data-stu-id="e3cf0-414">tinyint</span></span> |<span data-ttu-id="e3cf0-415">Bajtů</span><span class="sxs-lookup"><span data-stu-id="e3cf0-415">Byte</span></span> |
| <span data-ttu-id="e3cf0-416">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="e3cf0-416">uniqueidentifier</span></span> |<span data-ttu-id="e3cf0-417">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="e3cf0-417">Guid</span></span> |
| <span data-ttu-id="e3cf0-418">varbinary</span><span class="sxs-lookup"><span data-stu-id="e3cf0-418">varbinary</span></span> |<span data-ttu-id="e3cf0-419">Byte]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-419">Byte[]</span></span> |
| <span data-ttu-id="e3cf0-420">varchar</span><span class="sxs-lookup"><span data-stu-id="e3cf0-420">varchar</span></span> |<span data-ttu-id="e3cf0-421">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="e3cf0-421">String, Char[]</span></span> |
| <span data-ttu-id="e3cf0-422">xml</span><span class="sxs-lookup"><span data-stu-id="e3cf0-422">xml</span></span> |<span data-ttu-id="e3cf0-423">XML</span><span class="sxs-lookup"><span data-stu-id="e3cf0-423">Xml</span></span> |

## <a name="mapping-source-toosink-columns"></a><span data-ttu-id="e3cf0-424">Mapování zdrojové toosink sloupce</span><span class="sxs-lookup"><span data-stu-id="e3cf0-424">Mapping source toosink columns</span></span>
<span data-ttu-id="e3cf0-425">toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-425">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="e3cf0-426">Opakovatelných kopie</span><span class="sxs-lookup"><span data-stu-id="e3cf0-426">Repeatable copy</span></span>
<span data-ttu-id="e3cf0-427">Při kopírování dat tooSQL databáze serveru, aktivity kopírování hello připojí tabulky jímky toohello data ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-427">When copying data tooSQL Server Database, hello copy activity appends data toohello sink table by default.</span></span> <span data-ttu-id="e3cf0-428">Místo toho najdete v části tooperform UPSERT [tooSqlSink opakovatelných zápisu](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) článku.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-428">tooperform an UPSERT instead,  See [Repeatable write tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="e3cf0-429">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-429">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="e3cf0-430">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-430">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="e3cf0-431">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-431">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="e3cf0-432">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-432">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="e3cf0-433">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="e3cf0-433">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="e3cf0-434">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="e3cf0-434">Performance and Tuning</span></span>
<span data-ttu-id="e3cf0-435">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="e3cf0-435">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
