---
title: "aaaMove data z úložiště dat rozhraní ODBC | Microsoft Docs"
description: "Informace o tom, jak toomove dat od ODBC ukládá pomocí Azure Data Factory."
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
ms.openlocfilehash: bf96e71da449313b6144bb194205c572d2ca2030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a><span data-ttu-id="fa6c5-103">Přesun dat z rozhraní ODBC datová úložiště pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="fa6c5-103">Move data From ODBC data stores using Azure Data Factory</span></span>
<span data-ttu-id="fa6c5-104">Tento článek vysvětluje, jak uložit toouse hello aktivitu kopírování v Azure Data Factory toomove data z místními dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises ODBC data store.</span></span> <span data-ttu-id="fa6c5-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="fa6c5-106">Může kopírovat data z ODBC dat úložiště tooany podporované jímku dat úložiště.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-106">You can copy data from an ODBC data store tooany supported sink data store.</span></span> <span data-ttu-id="fa6c5-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="fa6c5-108">Objekt pro vytváření dat se aktuálně podporuje pouze přesun Klientova úložiště dat tooother uložit data z dat ODBC, ale ne pro přesun dat z jiného úložiště dat úložiště tooan ODBC data.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-108">Data factory currently supports only moving data from an ODBC data store tooother data stores, but not for moving data from other data stores tooan ODBC data store.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="fa6c5-109">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="fa6c5-109">Enabling connectivity</span></span>
<span data-ttu-id="fa6c5-110">Služba data Factory podporuje připojování zdroje ODBC tooon místní pomocí hello Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-110">Data Factory service supports connecting tooon-premises ODBC sources using hello Data Management Gateway.</span></span> <span data-ttu-id="fa6c5-111">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span> <span data-ttu-id="fa6c5-112">Použijte úložiště dat rozhraní ODBC tooconnect tooan aplikace hello brány i v případě, že je hostovaná ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-112">Use hello gateway tooconnect tooan ODBC data store even if it is hosted in an Azure IaaS VM.</span></span>

<span data-ttu-id="fa6c5-113">Hello brány můžete nainstalovat na stejný místní počítač nebo virtuální počítač Azure jako úložiště dat rozhraní ODBC hello hello hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-113">You can install hello gateway on hello same on-premises machine or hello Azure VM as hello ODBC data store.</span></span> <span data-ttu-id="fa6c5-114">Doporučujeme však nainstalovat hello brány na samostatný počítač nebo Azure IaaS virtuálních počítačů tooavoid sporu prostředků a pro dosažení vyššího výkonu.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-114">However, we recommend that you install hello gateway on a separate machine/Azure IaaS VM tooavoid resource contention and for better performance.</span></span> <span data-ttu-id="fa6c5-115">Když instalujete na samostatný počítač hello brány, hello počítač měl být schopný tooaccess hello počítače s úložištěm dat rozhraní ODBC hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-115">When you install hello gateway on a separate machine, hello machine should be able tooaccess hello machine with hello ODBC data store.</span></span>

<span data-ttu-id="fa6c5-116">Vedle hello Brána pro správu dat musíte také ovladače ODBC hello tooinstall pro úložiště dat hello na počítači brány hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-116">Apart from hello Data Management Gateway, you also need tooinstall hello ODBC driver for hello data store on hello gateway machine.</span></span>

> [!NOTE]
> <span data-ttu-id="fa6c5-117">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-117">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="fa6c5-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="fa6c5-118">Getting started</span></span>
<span data-ttu-id="fa6c5-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště dat rozhraní ODBC pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-119">You can create a pipeline with a copy activity that moves data from an ODBC data store by using different tools/APIs.</span></span>

<span data-ttu-id="fa6c5-120">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="fa6c5-121">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="fa6c5-122">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="fa6c5-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="fa6c5-124">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="fa6c5-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="fa6c5-125">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="fa6c5-126">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-126">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="fa6c5-127">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="fa6c5-128">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-128">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="fa6c5-129">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="fa6c5-130">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat rozhraní ODBC, naleznete v tématu [JSON příklad: kopírování dat z dat ODBC ukládání objektů Blob tooAzure](#json-example-copy-data-from-odbc-data-store-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-130">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an ODBC data store, see [JSON example: Copy data from ODBC data store tooAzure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="fa6c5-131">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou úložiště dat konkrétní tooODBC entity služby Data Factory používané toodefine:</span><span class="sxs-lookup"><span data-stu-id="fa6c5-131">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooODBC data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="fa6c5-132">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="fa6c5-132">Linked service properties</span></span>
<span data-ttu-id="fa6c5-133">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooODBC propojené služby.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-133">hello following table provides description for JSON elements specific tooODBC linked service.</span></span>

| <span data-ttu-id="fa6c5-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="fa6c5-134">Property</span></span> | <span data-ttu-id="fa6c5-135">Popis</span><span class="sxs-lookup"><span data-stu-id="fa6c5-135">Description</span></span> | <span data-ttu-id="fa6c5-136">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fa6c5-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fa6c5-137">type</span><span class="sxs-lookup"><span data-stu-id="fa6c5-137">type</span></span> |<span data-ttu-id="fa6c5-138">vlastnost typu Hello musí být nastavena na: **OnPremisesOdbc**</span><span class="sxs-lookup"><span data-stu-id="fa6c5-138">hello type property must be set to: **OnPremisesOdbc**</span></span> |<span data-ttu-id="fa6c5-139">Ano</span><span class="sxs-lookup"><span data-stu-id="fa6c5-139">Yes</span></span> |
| <span data-ttu-id="fa6c5-140">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="fa6c5-140">connectionString</span></span> |<span data-ttu-id="fa6c5-141">šifrovat přihlašovací údaje bez přístupu část Hello hello připojovací řetězec a volitelné přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-141">hello non-access credential portion of hello connection string and an optional encrypted credential.</span></span> <span data-ttu-id="fa6c5-142">Příklady v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-142">See examples in hello following sections.</span></span> |<span data-ttu-id="fa6c5-143">Ano</span><span class="sxs-lookup"><span data-stu-id="fa6c5-143">Yes</span></span> |
| <span data-ttu-id="fa6c5-144">přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="fa6c5-144">credential</span></span> |<span data-ttu-id="fa6c5-145">Hello přístup pověření část hello připojovacího řetězce zadaného ve formátu ovladačem vlastnost hodnota.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-145">hello access credential portion of hello connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="fa6c5-146">Příklad: "Uid =<user ID>; PWD =<password>; RefreshToken =<secret refresh token>; ".</span><span class="sxs-lookup"><span data-stu-id="fa6c5-146">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="fa6c5-147">Ne</span><span class="sxs-lookup"><span data-stu-id="fa6c5-147">No</span></span> |
| <span data-ttu-id="fa6c5-148">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-148">authenticationType</span></span> |<span data-ttu-id="fa6c5-149">Typ ověřování používá úložiště dat rozhraní ODBC toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-149">Type of authentication used tooconnect toohello ODBC data store.</span></span> <span data-ttu-id="fa6c5-150">Možné hodnoty jsou: anonymní a Basic.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-150">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="fa6c5-151">Ano</span><span class="sxs-lookup"><span data-stu-id="fa6c5-151">Yes</span></span> |
| <span data-ttu-id="fa6c5-152">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="fa6c5-152">username</span></span> |<span data-ttu-id="fa6c5-153">Pokud používáte základní ověřování, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-153">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="fa6c5-154">Ne</span><span class="sxs-lookup"><span data-stu-id="fa6c5-154">No</span></span> |
| <span data-ttu-id="fa6c5-155">heslo</span><span class="sxs-lookup"><span data-stu-id="fa6c5-155">password</span></span> |<span data-ttu-id="fa6c5-156">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-156">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="fa6c5-157">Ne</span><span class="sxs-lookup"><span data-stu-id="fa6c5-157">No</span></span> |
| <span data-ttu-id="fa6c5-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="fa6c5-158">gatewayName</span></span> |<span data-ttu-id="fa6c5-159">Úložiště dat rozhraní ODBC toohello tooconnect měli použít název hello brány, kterou hello služba Data Factory.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-159">Name of hello gateway that hello Data Factory service should use tooconnect toohello ODBC data store.</span></span> |<span data-ttu-id="fa6c5-160">Ano</span><span class="sxs-lookup"><span data-stu-id="fa6c5-160">Yes</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="fa6c5-161">Použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="fa6c5-161">Using Basic authentication</span></span>

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
### <a name="using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="fa6c5-162">Základní ověřování pomocí zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="fa6c5-162">Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="fa6c5-163">Můžete šifrovat přihlašovací údaje hello pomocí hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny (1.0 verzi prostředí Azure PowerShell) nebo [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 nebo starší verzi hello Azure PowerShell).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-163">You can encrypt hello credentials using hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of hello Azure PowerShell).</span></span>  

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

### <a name="using-anonymous-authentication"></a><span data-ttu-id="fa6c5-164">Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="fa6c5-164">Using Anonymous authentication</span></span>

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


## <a name="dataset-properties"></a><span data-ttu-id="fa6c5-165">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="fa6c5-165">Dataset properties</span></span>
<span data-ttu-id="fa6c5-166">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-166">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="fa6c5-167">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="fa6c5-168">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-168">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="fa6c5-169">rámci typeProperties Hello část datové sady typ **RelationalTable** (která zahrnuje datovou sadu ODBC) má následující vlastnosti hello</span><span class="sxs-lookup"><span data-stu-id="fa6c5-169">hello typeProperties section for dataset of type **RelationalTable** (which includes ODBC dataset) has hello following properties</span></span>

| <span data-ttu-id="fa6c5-170">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="fa6c5-170">Property</span></span> | <span data-ttu-id="fa6c5-171">Popis</span><span class="sxs-lookup"><span data-stu-id="fa6c5-171">Description</span></span> | <span data-ttu-id="fa6c5-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fa6c5-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fa6c5-173">tableName</span><span class="sxs-lookup"><span data-stu-id="fa6c5-173">tableName</span></span> |<span data-ttu-id="fa6c5-174">Název tabulky hello v úložišti dat rozhraní ODBC hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-174">Name of hello table in hello ODBC data store.</span></span> |<span data-ttu-id="fa6c5-175">Ano</span><span class="sxs-lookup"><span data-stu-id="fa6c5-175">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="fa6c5-176">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="fa6c5-176">Copy activity properties</span></span>
<span data-ttu-id="fa6c5-177">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-177">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="fa6c5-178">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-178">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="fa6c5-179">Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivit na hello se každý typ aktivity lišit podle druhé straně.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-179">Properties available in hello **typeProperties** section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="fa6c5-180">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-180">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="fa6c5-181">Při aktivitě kopírování, pokud je zdroj typu **RelationalSource** (která zahrnuje rozhraní ODBC), hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="fa6c5-181">In copy activity, when source is of type **RelationalSource** (which includes ODBC), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="fa6c5-182">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="fa6c5-182">Property</span></span> | <span data-ttu-id="fa6c5-183">Popis</span><span class="sxs-lookup"><span data-stu-id="fa6c5-183">Description</span></span> | <span data-ttu-id="fa6c5-184">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="fa6c5-184">Allowed values</span></span> | <span data-ttu-id="fa6c5-185">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fa6c5-185">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="fa6c5-186">query</span><span class="sxs-lookup"><span data-stu-id="fa6c5-186">query</span></span> |<span data-ttu-id="fa6c5-187">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-187">Use hello custom query tooread data.</span></span> |<span data-ttu-id="fa6c5-188">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-188">SQL query string.</span></span> <span data-ttu-id="fa6c5-189">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-189">For example: select * from MyTable.</span></span> |<span data-ttu-id="fa6c5-190">Ano</span><span class="sxs-lookup"><span data-stu-id="fa6c5-190">Yes</span></span> |


## <a name="json-example-copy-data-from-odbc-data-store-tooazure-blob"></a><span data-ttu-id="fa6c5-191">Příklad JSON: kopírování dat z dat ODBC ukládání tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="fa6c5-191">JSON example: Copy data from ODBC data store tooAzure Blob</span></span>
<span data-ttu-id="fa6c5-192">Tento příklad obsahuje definice JSON, můžete použít toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-192">This example provides JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="fa6c5-193">Ukazuje, jak toocopy data z ODBC zdroje tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-193">It shows how toocopy data from an ODBC source tooan Azure Blob Storage.</span></span> <span data-ttu-id="fa6c5-194">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-194">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="fa6c5-195">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="fa6c5-195">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="fa6c5-196">Propojené služby typu [OnPremisesOdbc](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-196">A linked service of type [OnPremisesOdbc](#linked-service-properties).</span></span>
2. <span data-ttu-id="fa6c5-197">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-197">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="fa6c5-198">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-198">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="fa6c5-199">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-199">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="fa6c5-200">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-200">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="fa6c5-201">Ukázka Hello zkopíruje data z výsledku dotazu v objektu blob úložiště tooa ODBC dat každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-201">hello sample copies data from a query result in an ODBC data store tooa blob every hour.</span></span> <span data-ttu-id="fa6c5-202">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-202">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="fa6c5-203">Jako první krok nastavte Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-203">As a first step, set up hello data management gateway.</span></span> <span data-ttu-id="fa6c5-204">Hello pokyny jsou v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-204">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="fa6c5-205">**ODBC propojená služba** tento příklad používá hello základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-205">**ODBC linked service** This example uses hello Basic authentication.</span></span> <span data-ttu-id="fa6c5-206">V tématu [ODBC propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-206">See [ODBC linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="fa6c5-207">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="fa6c5-207">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="fa6c5-208">**Vstupní datové sady rozhraní ODBC**</span><span class="sxs-lookup"><span data-stu-id="fa6c5-208">**ODBC input dataset**</span></span>

<span data-ttu-id="fa6c5-209">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v databázi ODBC a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-209">hello sample assumes you have created a table “MyTable” in an ODBC database and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="fa6c5-210">Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-210">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="fa6c5-211">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="fa6c5-211">**Azure Blob output dataset**</span></span>

<span data-ttu-id="fa6c5-212">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-212">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="fa6c5-213">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-213">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="fa6c5-214">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-214">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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


<span data-ttu-id="fa6c5-215">**Aktivita kopírování v kanálu s zdroje ODBC (RelationalSource) a podřízený objekt Blob (BlobSink)**</span><span class="sxs-lookup"><span data-stu-id="fa6c5-215">**Copy activity in a pipeline with ODBC source (RelationalSource) and Blob sink (BlobSink)**</span></span>

<span data-ttu-id="fa6c5-216">Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-216">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="fa6c5-217">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-217">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="fa6c5-218">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-218">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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
### <a name="type-mapping-for-odbc"></a><span data-ttu-id="fa6c5-219">Mapování typu pro rozhraní ODBC</span><span class="sxs-lookup"><span data-stu-id="fa6c5-219">Type mapping for ODBC</span></span>
<span data-ttu-id="fa6c5-220">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello dvoustupňový přístup následující:</span><span class="sxs-lookup"><span data-stu-id="fa6c5-220">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="fa6c5-221">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="fa6c5-221">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="fa6c5-222">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="fa6c5-222">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="fa6c5-223">Při přesouvání dat z úložiště dat rozhraní ODBC, ODBC datové typy jsou typy namapované too.NET, jak je uvedeno v hello [mapování datového typu ODBC](https://msdn.microsoft.com/library/cc668763.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-223">When moving data from ODBC data stores, ODBC data types are mapped too.NET types as mentioned in hello [ODBC Data Type Mappings](https://msdn.microsoft.com/library/cc668763.aspx) topic.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="fa6c5-224">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="fa6c5-224">Map source toosink columns</span></span>
<span data-ttu-id="fa6c5-225">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-225">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="fa6c5-226">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="fa6c5-226">Repeatable read from relational sources</span></span>
<span data-ttu-id="fa6c5-227">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-227">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="fa6c5-228">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-228">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="fa6c5-229">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-229">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="fa6c5-230">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-230">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="fa6c5-231">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-231">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="ge-historian-store"></a><span data-ttu-id="fa6c5-232">GE Historian úložiště</span><span class="sxs-lookup"><span data-stu-id="fa6c5-232">GE Historian store</span></span>
<span data-ttu-id="fa6c5-233">Vytvoření toolink ODBC propojené služby [GE Proficy Historian (teď GE Historian)](http://www.geautomation.com/products/proficy-historian) úložiště dat tooan pro vytváření dat Azure, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="fa6c5-233">You create an ODBC linked service toolink a [GE Proficy Historian (now GE Historian)](http://www.geautomation.com/products/proficy-historian) data store tooan Azure data factory as shown in hello following example:</span></span>

```json
{
    "name": "HistorianLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "connectionString": "DSN=<name of hello GE Historian store>",
            "gatewayName": "<gateway name>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="fa6c5-234">Nainstalovat brána pro správu dat na místním počítači a zaregistrujte bránu hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-234">Install Data Management Gateway on an on-premises machine and register hello gateway with hello portal.</span></span> <span data-ttu-id="fa6c5-235">Hello brány je nainstalovaná v místním počítači používá hello ovladač ODBC pro GE Historian tooconnect toohello GE Historian úložišti.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-235">hello gateway installed on your on-premises computer uses hello ODBC driver for GE Historian tooconnect toohello GE Historian data store.</span></span> <span data-ttu-id="fa6c5-236">Proto nainstalujte ovladač hello, pokud ještě není nainstalovaná na počítači brány hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-236">Therefore, install hello driver if it is not already installed on hello gateway machine.</span></span> <span data-ttu-id="fa6c5-237">V tématu [povolení připojení](#enabling-connectivity) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-237">See [Enabling connectivity](#enabling-connectivity) section for details.</span></span>

<span data-ttu-id="fa6c5-238">Než použijete hello GE Historian ukládat v řešení pro vytváření dat, ověřte, zda hello brány může připojit toohello data store pomocí pokynů v další části hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-238">Before you use hello GE Historian store in a Data Factory solution, verify whether hello gateway can connect toohello data store using instructions in hello next section.</span></span>

<span data-ttu-id="fa6c5-239">Přečíst článek hello od začátku hello podrobnější přehled použití dat ODBC se ukládá jako zdrojové úložiště dat v operaci kopírování.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-239">Read hello article from hello beginning for a detailed overview of using ODBC data stores as source data stores in a copy operation.</span></span>  

## <a name="troubleshoot-connectivity-issues"></a><span data-ttu-id="fa6c5-240">Řešení potíží s problémy s připojením</span><span class="sxs-lookup"><span data-stu-id="fa6c5-240">Troubleshoot connectivity issues</span></span>
<span data-ttu-id="fa6c5-241">potíže s připojením tootroubleshoot, použijte hello **diagnostiky** kartě **Správce konfigurace brány pro správu dat**.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-241">tootroubleshoot connection issues, use hello **Diagnostics** tab of **Data Management Gateway Configuration Manager**.</span></span>

1. <span data-ttu-id="fa6c5-242">Spusťte **Správce konfigurace brány pro správu dat**.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-242">Launch **Data Management Gateway Configuration Manager**.</span></span> <span data-ttu-id="fa6c5-243">Buď můžete spustit "C:\Program Files\Microsoft Data správy Gateway\1.0\Shared\ConfigManager.exe" přímo (nebo) vyhledávání pro **brány** toofind odkaz příliš**Brána pro správu dat** aplikace, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-243">You can either run "C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe" directly (or) search for **Gateway** toofind a link too**Microsoft Data Management Gateway** application as shown in hello following image.</span></span>

    ![Hledání brány](./media/data-factory-odbc-connector/search-gateway.png)
2. <span data-ttu-id="fa6c5-245">Přepínač toohello **diagnostiky** kartě.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-245">Switch toohello **Diagnostics** tab.</span></span>

    ![Diagnostiku brány](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. <span data-ttu-id="fa6c5-247">Vyberte hello **typ** dat uložit (propojené služby).</span><span class="sxs-lookup"><span data-stu-id="fa6c5-247">Select hello **type** of data store (linked service).</span></span>
4. <span data-ttu-id="fa6c5-248">Zadejte **ověřování** a zadejte **pověření** (nebo) zadejte **připojovací řetězec** , je použít tooconnect toohello datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-248">Specify **authentication** and enter **credentials** (or) enter **connection string** that is used tooconnect toohello data store.</span></span>
5. <span data-ttu-id="fa6c5-249">Klikněte na tlačítko **testovací připojení** tootest hello připojení toohello úložišti.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-249">Click **Test connection** tootest hello connection toohello data store.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="fa6c5-250">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="fa6c5-250">Performance and Tuning</span></span>
<span data-ttu-id="fa6c5-251">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="fa6c5-251">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
