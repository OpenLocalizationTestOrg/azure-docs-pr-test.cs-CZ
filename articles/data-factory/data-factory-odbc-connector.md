---
title: "Přesun dat z úložiště dat rozhraní ODBC | Microsoft Docs"
description: "Další informace o tom, jak přesun dat z úložiště dat rozhraní ODBC pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ad70a598-c031-4339-a883-c6125403cb76
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: 269d9802ca4a6a16dbf9021929fe21104cb431f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a><span data-ttu-id="862bf-103">Přesun dat z rozhraní ODBC datová úložiště pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="862bf-103">Move data From ODBC data stores using Azure Data Factory</span></span>
<span data-ttu-id="862bf-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z úložiště dat rozhraní ODBC místně.</span><span class="sxs-lookup"><span data-stu-id="862bf-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises ODBC data store.</span></span> <span data-ttu-id="862bf-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="862bf-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="862bf-106">Můžete zkopírovat data z úložiště dat rozhraní ODBC do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="862bf-106">You can copy data from an ODBC data store to any supported sink data store.</span></span> <span data-ttu-id="862bf-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="862bf-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="862bf-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z úložiště dat rozhraní ODBC do jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat k úložišti dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="862bf-108">Data factory currently supports only moving data from an ODBC data store to other data stores, but not for moving data from other data stores to an ODBC data store.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="862bf-109">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="862bf-109">Enabling connectivity</span></span>
<span data-ttu-id="862bf-110">Služba data Factory podporuje připojení k místní zdroje ODBC pomocí Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="862bf-110">Data Factory service supports connecting to on-premises ODBC sources using the Data Management Gateway.</span></span> <span data-ttu-id="862bf-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku se dozvíte o Brána pro správu dat a podrobné pokyny o nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="862bf-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span> <span data-ttu-id="862bf-112">Použijte bránu pro připojení k úložišti dat rozhraní ODBC i v případě, že je hostovaná ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="862bf-112">Use the gateway to connect to an ODBC data store even if it is hosted in an Azure IaaS VM.</span></span>

<span data-ttu-id="862bf-113">Bránu můžete nainstalovat na stejný místní počítač nebo virtuální počítač Azure jako úložiště dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="862bf-113">You can install the gateway on the same on-premises machine or the Azure VM as the ODBC data store.</span></span> <span data-ttu-id="862bf-114">Nicméně doporučujeme nainstalovat bránu na samostatný počítač nebo Azure IaaS virtuální počítač, abyste předešli sporu prostředků a pro dosažení vyššího výkonu.</span><span class="sxs-lookup"><span data-stu-id="862bf-114">However, we recommend that you install the gateway on a separate machine/Azure IaaS VM to avoid resource contention and for better performance.</span></span> <span data-ttu-id="862bf-115">Při instalaci brány na samostatný počítač, na počítač byste měli mít přístup k počítači s úložištěm dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="862bf-115">When you install the gateway on a separate machine, the machine should be able to access the machine with the ODBC data store.</span></span>

<span data-ttu-id="862bf-116">Kromě Brána pro správu dat budete také muset nainstalovat ovladač ODBC pro úložiště dat na počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="862bf-116">Apart from the Data Management Gateway, you also need to install the ODBC driver for the data store on the gateway machine.</span></span>

> [!NOTE]
> <span data-ttu-id="862bf-117">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="862bf-117">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="862bf-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="862bf-118">Getting started</span></span>
<span data-ttu-id="862bf-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště dat rozhraní ODBC pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="862bf-119">You can create a pipeline with a copy activity that moves data from an ODBC data store by using different tools/APIs.</span></span>

<span data-ttu-id="862bf-120">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="862bf-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="862bf-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="862bf-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="862bf-122">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="862bf-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="862bf-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="862bf-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="862bf-124">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="862bf-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="862bf-125">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="862bf-125">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="862bf-126">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="862bf-126">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="862bf-127">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="862bf-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="862bf-128">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="862bf-128">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="862bf-129">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="862bf-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="862bf-130">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat rozhraní ODBC, naleznete v tématu [JSON příklad: kopírování dat z dat ODBC uložit do objektu Blob Azure](#json-example-copy-data-from-odbc-data-store-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="862bf-130">For a sample with JSON definitions for Data Factory entities that are used to copy data from an ODBC data store, see [JSON example: Copy data from ODBC data store to Azure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="862bf-131">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k úložišti dat rozhraní ODBC:</span><span class="sxs-lookup"><span data-stu-id="862bf-131">The following sections provide details about JSON properties that are used to define Data Factory entities specific to ODBC data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="862bf-132">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="862bf-132">Linked service properties</span></span>
<span data-ttu-id="862bf-133">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro ODBC propojené služby.</span><span class="sxs-lookup"><span data-stu-id="862bf-133">The following table provides description for JSON elements specific to ODBC linked service.</span></span>

| <span data-ttu-id="862bf-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="862bf-134">Property</span></span> | <span data-ttu-id="862bf-135">Popis</span><span class="sxs-lookup"><span data-stu-id="862bf-135">Description</span></span> | <span data-ttu-id="862bf-136">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="862bf-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="862bf-137">type</span><span class="sxs-lookup"><span data-stu-id="862bf-137">type</span></span> |<span data-ttu-id="862bf-138">Vlastnost typu musí být nastavena na: **OnPremisesOdbc**</span><span class="sxs-lookup"><span data-stu-id="862bf-138">The type property must be set to: **OnPremisesOdbc**</span></span> |<span data-ttu-id="862bf-139">Ano</span><span class="sxs-lookup"><span data-stu-id="862bf-139">Yes</span></span> |
| <span data-ttu-id="862bf-140">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="862bf-140">connectionString</span></span> |<span data-ttu-id="862bf-141">Přístup k pověření část připojovací řetězec a volitelné šifrovat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="862bf-141">The non-access credential portion of the connection string and an optional encrypted credential.</span></span> <span data-ttu-id="862bf-142">Příklady v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="862bf-142">See examples in the following sections.</span></span> |<span data-ttu-id="862bf-143">Ano</span><span class="sxs-lookup"><span data-stu-id="862bf-143">Yes</span></span> |
| <span data-ttu-id="862bf-144">přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="862bf-144">credential</span></span> |<span data-ttu-id="862bf-145">Část přístup přihlašovacích údajů z připojovacího řetězce zadaného ve formátu ovladačem vlastnost hodnota.</span><span class="sxs-lookup"><span data-stu-id="862bf-145">The access credential portion of the connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="862bf-146">Příklad: "Uid =<user ID>; PWD =<password>; RefreshToken =<secret refresh token>; ".</span><span class="sxs-lookup"><span data-stu-id="862bf-146">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="862bf-147">Ne</span><span class="sxs-lookup"><span data-stu-id="862bf-147">No</span></span> |
| <span data-ttu-id="862bf-148">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="862bf-148">authenticationType</span></span> |<span data-ttu-id="862bf-149">Typ ověřování používaný pro připojení k úložišti dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="862bf-149">Type of authentication used to connect to the ODBC data store.</span></span> <span data-ttu-id="862bf-150">Možné hodnoty jsou: anonymní a Basic.</span><span class="sxs-lookup"><span data-stu-id="862bf-150">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="862bf-151">Ano</span><span class="sxs-lookup"><span data-stu-id="862bf-151">Yes</span></span> |
| <span data-ttu-id="862bf-152">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="862bf-152">username</span></span> |<span data-ttu-id="862bf-153">Pokud používáte základní ověřování, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="862bf-153">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="862bf-154">Ne</span><span class="sxs-lookup"><span data-stu-id="862bf-154">No</span></span> |
| <span data-ttu-id="862bf-155">heslo</span><span class="sxs-lookup"><span data-stu-id="862bf-155">password</span></span> |<span data-ttu-id="862bf-156">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="862bf-156">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="862bf-157">Ne</span><span class="sxs-lookup"><span data-stu-id="862bf-157">No</span></span> |
| <span data-ttu-id="862bf-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="862bf-158">gatewayName</span></span> |<span data-ttu-id="862bf-159">Název brány, kterou služba Data Factory měla použít pro připojení k úložišti dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="862bf-159">Name of the gateway that the Data Factory service should use to connect to the ODBC data store.</span></span> |<span data-ttu-id="862bf-160">Ano</span><span class="sxs-lookup"><span data-stu-id="862bf-160">Yes</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="862bf-161">Použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="862bf-161">Using Basic authentication</span></span>

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```
### <a name="using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="862bf-162">Základní ověřování pomocí zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="862bf-162">Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="862bf-163">Můžete šifrovat přihlašovací údaje pomocí [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny (1.0 verzi prostředí Azure PowerShell) nebo [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 nebo starší verzi prostředí Azure PowerShell).</span><span class="sxs-lookup"><span data-stu-id="862bf-163">You can encrypt the credentials using the [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of the Azure PowerShell).</span></span>  

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a><span data-ttu-id="862bf-164">Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="862bf-164">Using Anonymous authentication</span></span>

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "mygateway"
        }
    }
}
```


## <a name="dataset-properties"></a><span data-ttu-id="862bf-165">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="862bf-165">Dataset properties</span></span>
<span data-ttu-id="862bf-166">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="862bf-166">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="862bf-167">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="862bf-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="862bf-168">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="862bf-168">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="862bf-169">Rámci typeProperties část datové sady typ **RelationalTable** (která zahrnuje datovou sadu ODBC) má následující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="862bf-169">The typeProperties section for dataset of type **RelationalTable** (which includes ODBC dataset) has the following properties</span></span>

| <span data-ttu-id="862bf-170">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="862bf-170">Property</span></span> | <span data-ttu-id="862bf-171">Popis</span><span class="sxs-lookup"><span data-stu-id="862bf-171">Description</span></span> | <span data-ttu-id="862bf-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="862bf-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="862bf-173">tableName</span><span class="sxs-lookup"><span data-stu-id="862bf-173">tableName</span></span> |<span data-ttu-id="862bf-174">Název tabulky v úložišti dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="862bf-174">Name of the table in the ODBC data store.</span></span> |<span data-ttu-id="862bf-175">Ano</span><span class="sxs-lookup"><span data-stu-id="862bf-175">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="862bf-176">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="862bf-176">Copy activity properties</span></span>
<span data-ttu-id="862bf-177">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="862bf-177">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="862bf-178">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="862bf-178">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="862bf-179">Vlastnosti, které jsou k dispozici v **rámci typeProperties** části aktivity na druhé straně lišit každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="862bf-179">Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="862bf-180">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="862bf-180">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="862bf-181">Při aktivitě kopírování, pokud je zdroj typu **RelationalSource** (která zahrnuje rozhraní ODBC), v rámci typeProperties části jsou k dispozici následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="862bf-181">In copy activity, when source is of type **RelationalSource** (which includes ODBC), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="862bf-182">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="862bf-182">Property</span></span> | <span data-ttu-id="862bf-183">Popis</span><span class="sxs-lookup"><span data-stu-id="862bf-183">Description</span></span> | <span data-ttu-id="862bf-184">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="862bf-184">Allowed values</span></span> | <span data-ttu-id="862bf-185">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="862bf-185">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="862bf-186">query</span><span class="sxs-lookup"><span data-stu-id="862bf-186">query</span></span> |<span data-ttu-id="862bf-187">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="862bf-187">Use the custom query to read data.</span></span> |<span data-ttu-id="862bf-188">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="862bf-188">SQL query string.</span></span> <span data-ttu-id="862bf-189">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="862bf-189">For example: select * from MyTable.</span></span> |<span data-ttu-id="862bf-190">Ano</span><span class="sxs-lookup"><span data-stu-id="862bf-190">Yes</span></span> |


## <a name="json-example-copy-data-from-odbc-data-store-to-azure-blob"></a><span data-ttu-id="862bf-191">Příklad JSON: kopírování dat z dat ODBC uložit do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="862bf-191">JSON example: Copy data from ODBC data store to Azure Blob</span></span>
<span data-ttu-id="862bf-192">Tento příklad obsahuje definice JSON, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="862bf-192">This example provides JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="862bf-193">Ukazuje, jak zkopírovat data ze zdroje ODBC do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="862bf-193">It shows how to copy data from an ODBC source to an Azure Blob Storage.</span></span> <span data-ttu-id="862bf-194">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="862bf-194">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="862bf-195">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="862bf-195">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="862bf-196">Propojené služby typu [OnPremisesOdbc](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="862bf-196">A linked service of type [OnPremisesOdbc](#linked-service-properties).</span></span>
2. <span data-ttu-id="862bf-197">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="862bf-197">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="862bf-198">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="862bf-198">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="862bf-199">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="862bf-199">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="862bf-200">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="862bf-200">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="862bf-201">Ukázka zkopíruje data z výsledku dotazu v úložišti dat rozhraní ODBC do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="862bf-201">The sample copies data from a query result in an ODBC data store to a blob every hour.</span></span> <span data-ttu-id="862bf-202">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="862bf-202">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="862bf-203">Jako první krok nastavte Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="862bf-203">As a first step, set up the data management gateway.</span></span> <span data-ttu-id="862bf-204">Tyto pokyny jsou v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="862bf-204">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="862bf-205">**ODBC propojená služba** tento příklad používá základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="862bf-205">**ODBC linked service** This example uses the Basic authentication.</span></span> <span data-ttu-id="862bf-206">V tématu [ODBC propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="862bf-206">See [ODBC linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```json
{
    "name": "OnPremOdbcLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

<span data-ttu-id="862bf-207">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="862bf-207">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="862bf-208">**Vstupní datové sady rozhraní ODBC**</span><span class="sxs-lookup"><span data-stu-id="862bf-208">**ODBC input dataset**</span></span>

<span data-ttu-id="862bf-209">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v databázi ODBC a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="862bf-209">The sample assumes you have created a table “MyTable” in an ODBC database and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="862bf-210">Nastavení "externí": "PRAVDA" informuje služba Data Factory, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="862bf-210">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremOdbcLinkedService",
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

<span data-ttu-id="862bf-211">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="862bf-211">**Azure Blob output dataset**</span></span>

<span data-ttu-id="862bf-212">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="862bf-212">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="862bf-213">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="862bf-213">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="862bf-214">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="862bf-214">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobOdbcDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odbc/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


<span data-ttu-id="862bf-215">**Aktivita kopírování v kanálu s zdroje ODBC (RelationalSource) a podřízený objekt Blob (BlobSink)**</span><span class="sxs-lookup"><span data-stu-id="862bf-215">**Copy activity in a pipeline with ODBC source (RelationalSource) and Blob sink (BlobSink)**</span></span>

<span data-ttu-id="862bf-216">Kanál obsahuje aktivitu kopírování, která je konfigurovaná pro používání těchto vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="862bf-216">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="862bf-217">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="862bf-217">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="862bf-218">Zadané pro dotaz SQL **dotazu** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="862bf-218">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```json
{
    "name": "CopyODBCToBlob",
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
                        "name": "OdbcDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOdbcDataSet"
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
                "name": "OdbcToBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-odbc"></a><span data-ttu-id="862bf-219">Mapování typu pro rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="862bf-219">Type mapping for ODBC</span></span>
<span data-ttu-id="862bf-220">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup ve dvou krocích:</span><span class="sxs-lookup"><span data-stu-id="862bf-220">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="862bf-221">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="862bf-221">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="862bf-222">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="862bf-222">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="862bf-223">Při přesouvání dat z úložiště dat rozhraní ODBC, typy dat rozhraní ODBC jsou namapované na typy .NET, jak je uvedeno v [mapování datového typu ODBC](https://msdn.microsoft.com/library/cc668763.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="862bf-223">When moving data from ODBC data stores, ODBC data types are mapped to .NET types as mentioned in the [ODBC Data Type Mappings](https://msdn.microsoft.com/library/cc668763.aspx) topic.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="862bf-224">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="862bf-224">Map source to sink columns</span></span>
<span data-ttu-id="862bf-225">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="862bf-225">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="862bf-226">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="862bf-226">Repeatable read from relational sources</span></span>
<span data-ttu-id="862bf-227">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="862bf-227">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="862bf-228">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="862bf-228">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="862bf-229">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="862bf-229">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="862bf-230">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="862bf-230">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="862bf-231">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="862bf-231">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="ge-historian-store"></a><span data-ttu-id="862bf-232">GE Historian úložiště</span><span class="sxs-lookup"><span data-stu-id="862bf-232">GE Historian store</span></span>
<span data-ttu-id="862bf-233">Vytvoření služby ODBC propojené propojení [GE Proficy Historian (teď GE Historian)](http://www.geautomation.com/products/proficy-historian) úložiště dat do služby Azure data factory, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="862bf-233">You create an ODBC linked service to link a [GE Proficy Historian (now GE Historian)](http://www.geautomation.com/products/proficy-historian) data store to an Azure data factory as shown in the following example:</span></span>

```json
{
    "name": "HistorianLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "connectionString": "DSN=<name of the GE Historian store>",
            "gatewayName": "<gateway name>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="862bf-234">Nainstalujte Brána pro správu dat na místním počítači a zaregistrujte bránu pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="862bf-234">Install Data Management Gateway on an on-premises machine and register the gateway with the portal.</span></span> <span data-ttu-id="862bf-235">Brány nainstalované na místním počítači používá k připojení k úložišti dat GE Historian ovladač ODBC pro GE Historian.</span><span class="sxs-lookup"><span data-stu-id="862bf-235">The gateway installed on your on-premises computer uses the ODBC driver for GE Historian to connect to the GE Historian data store.</span></span> <span data-ttu-id="862bf-236">Nainstalujte ovladač, proto, pokud ještě není nainstalovaná na počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="862bf-236">Therefore, install the driver if it is not already installed on the gateway machine.</span></span> <span data-ttu-id="862bf-237">V tématu [povolení připojení](#enabling-connectivity) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="862bf-237">See [Enabling connectivity](#enabling-connectivity) section for details.</span></span>

<span data-ttu-id="862bf-238">Než použijete úložišti GE Historian v řešení pro vytváření dat, ověřte, zda brána se může připojit k úložišti dat pomocí pokynů v další části.</span><span class="sxs-lookup"><span data-stu-id="862bf-238">Before you use the GE Historian store in a Data Factory solution, verify whether the gateway can connect to the data store using instructions in the next section.</span></span>

<span data-ttu-id="862bf-239">Čtení článku od začátku podrobnější přehled použití dat ODBC ukládá jako zdrojové úložiště dat v operaci kopírování.</span><span class="sxs-lookup"><span data-stu-id="862bf-239">Read the article from the beginning for a detailed overview of using ODBC data stores as source data stores in a copy operation.</span></span>  

## <a name="troubleshoot-connectivity-issues"></a><span data-ttu-id="862bf-240">Řešení potíží s problémy s připojením</span><span class="sxs-lookup"><span data-stu-id="862bf-240">Troubleshoot connectivity issues</span></span>
<span data-ttu-id="862bf-241">K odstraňování problémů s připojením, použijte **diagnostiky** kartě **Správce konfigurace brány pro správu dat**.</span><span class="sxs-lookup"><span data-stu-id="862bf-241">To troubleshoot connection issues, use the **Diagnostics** tab of **Data Management Gateway Configuration Manager**.</span></span>

1. <span data-ttu-id="862bf-242">Spusťte **Správce konfigurace brány pro správu dat**.</span><span class="sxs-lookup"><span data-stu-id="862bf-242">Launch **Data Management Gateway Configuration Manager**.</span></span> <span data-ttu-id="862bf-243">Buď můžete spustit "C:\Program Files\Microsoft Data správy Gateway\1.0\Shared\ConfigManager.exe" přímo (nebo) vyhledávání pro **brány** najít odkaz na **Brána pro správu dat** aplikace, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="862bf-243">You can either run "C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe" directly (or) search for **Gateway** to find a link to **Microsoft Data Management Gateway** application as shown in the following image.</span></span>

    ![Hledání brány](./media/data-factory-odbc-connector/search-gateway.png)
2. <span data-ttu-id="862bf-245">Přepnout **diagnostiky** kartě.</span><span class="sxs-lookup"><span data-stu-id="862bf-245">Switch to the **Diagnostics** tab.</span></span>

    ![Diagnostiku brány](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. <span data-ttu-id="862bf-247">Vyberte **typ** dat uložit (propojené služby).</span><span class="sxs-lookup"><span data-stu-id="862bf-247">Select the **type** of data store (linked service).</span></span>
4. <span data-ttu-id="862bf-248">Zadejte **ověřování** a zadejte **pověření** (nebo) zadejte **připojovací řetězec** používané pro připojení k úložišti.</span><span class="sxs-lookup"><span data-stu-id="862bf-248">Specify **authentication** and enter **credentials** (or) enter **connection string** that is used to connect to the data store.</span></span>
5. <span data-ttu-id="862bf-249">Klikněte na tlačítko **testovací připojení** k testování připojení k úložišti.</span><span class="sxs-lookup"><span data-stu-id="862bf-249">Click **Test connection** to test the connection to the data store.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="862bf-250">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="862bf-250">Performance and Tuning</span></span>
<span data-ttu-id="862bf-251">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="862bf-251">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
