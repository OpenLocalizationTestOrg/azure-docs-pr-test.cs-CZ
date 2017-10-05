---
title: "Přesun dat z OData zdroje | Microsoft Docs"
description: "Další informace o tom, jak přesunout data ze zdroje OData pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: de28fa56-3204-4546-a4df-21a21de43ed7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 624b6c8f317477d83539392c6c2f15c2dc69d401
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a><span data-ttu-id="b0677-103">Přesun dat z OData zdroje pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b0677-103">Move data From a OData source using Azure Data Factory</span></span>
<span data-ttu-id="b0677-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z na zdroj OData.</span><span class="sxs-lookup"><span data-stu-id="b0677-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an OData source.</span></span> <span data-ttu-id="b0677-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="b0677-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="b0677-106">Z OData zdroje můžete zkopírovat data do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="b0677-106">You can copy data from an OData source to any supported sink data store.</span></span> <span data-ttu-id="b0677-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="b0677-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="b0677-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z zdroje OData k jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat do zdroje OData.</span><span class="sxs-lookup"><span data-stu-id="b0677-108">Data factory currently supports only moving data from an OData source to other data stores, but not for moving data from other data stores to an OData source.</span></span> 

## <a name="supported-versions-and-authentication-types"></a><span data-ttu-id="b0677-109">Podporované verze a typy ověřování</span><span class="sxs-lookup"><span data-stu-id="b0677-109">Supported versions and authentication types</span></span>
<span data-ttu-id="b0677-110">Tento konektor OData podporu OData verze 3.0 a 4.0 a můžete kopírovat data z obou cloudu OData a místním zdrojům OData.</span><span class="sxs-lookup"><span data-stu-id="b0677-110">This OData connector support OData version 3.0 and 4.0, and you can copy data from both cloud OData and on-premises OData sources.</span></span> <span data-ttu-id="b0677-111">K tomu musíte nainstalovat bránu dat správy.</span><span class="sxs-lookup"><span data-stu-id="b0677-111">For the latter, you need to install the Data Management Gateway.</span></span> <span data-ttu-id="b0677-112">V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) podrobnosti o Brána pro správu dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="b0677-112">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

<span data-ttu-id="b0677-113">Níže ověřování jsou podporované typy:</span><span class="sxs-lookup"><span data-stu-id="b0677-113">Below authentication types are supported:</span></span>

* <span data-ttu-id="b0677-114">Pro přístup k **cloudu** datového kanálu OData, můžete použít anonymní, základní (uživatelské jméno a heslo) nebo Azure Active Directory na základě ověřování OAuth.</span><span class="sxs-lookup"><span data-stu-id="b0677-114">To access **cloud** OData feed, you can use anonymous, basic (user name and password), or Azure Active Directory based OAuth authentication.</span></span>
* <span data-ttu-id="b0677-115">Pro přístup k **místní** datového kanálu OData, můžete použít anonymní, základní (uživatelské jméno a heslo) nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="b0677-115">To access **on-premises** OData feed, you can use anonymous, basic (user name and password), or Windows authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="b0677-116">Začínáme</span><span class="sxs-lookup"><span data-stu-id="b0677-116">Getting started</span></span>
<span data-ttu-id="b0677-117">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje OData pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b0677-117">You can create a pipeline with a copy activity that moves data from an OData source by using different tools/APIs.</span></span>

<span data-ttu-id="b0677-118">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="b0677-118">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="b0677-119">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="b0677-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="b0677-120">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="b0677-120">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="b0677-121">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="b0677-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="b0677-122">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="b0677-122">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="b0677-123">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="b0677-123">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="b0677-124">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="b0677-124">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="b0677-125">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="b0677-125">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="b0677-126">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="b0677-126">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="b0677-127">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b0677-127">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="b0677-128">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z zdroje OData, naleznete v tématu [JSON příklad: kopírování dat ze zdroje OData do objektu Blob Azure](#json-example-copy-data-from-odata-source-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="b0677-128">For a sample with JSON definitions for Data Factory entities that are used to copy data from an OData source, see [JSON example: Copy data from OData source to Azure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="b0677-129">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory na zdroj OData:</span><span class="sxs-lookup"><span data-stu-id="b0677-129">The following sections provide details about JSON properties that are used to define Data Factory entities specific to OData source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="b0677-130">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="b0677-130">Linked Service properties</span></span>
<span data-ttu-id="b0677-131">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro OData propojené služby.</span><span class="sxs-lookup"><span data-stu-id="b0677-131">The following table provides description for JSON elements specific to OData linked service.</span></span>

| <span data-ttu-id="b0677-132">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b0677-132">Property</span></span> | <span data-ttu-id="b0677-133">Popis</span><span class="sxs-lookup"><span data-stu-id="b0677-133">Description</span></span> | <span data-ttu-id="b0677-134">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0677-134">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b0677-135">type</span><span class="sxs-lookup"><span data-stu-id="b0677-135">type</span></span> |<span data-ttu-id="b0677-136">Vlastnost typu musí být nastavena na: **OData**</span><span class="sxs-lookup"><span data-stu-id="b0677-136">The type property must be set to: **OData**</span></span> |<span data-ttu-id="b0677-137">Ano</span><span class="sxs-lookup"><span data-stu-id="b0677-137">Yes</span></span> |
| <span data-ttu-id="b0677-138">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="b0677-138">url</span></span> |<span data-ttu-id="b0677-139">Adresa URL služby OData.</span><span class="sxs-lookup"><span data-stu-id="b0677-139">Url of the OData service.</span></span> |<span data-ttu-id="b0677-140">Ano</span><span class="sxs-lookup"><span data-stu-id="b0677-140">Yes</span></span> |
| <span data-ttu-id="b0677-141">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="b0677-141">authenticationType</span></span> |<span data-ttu-id="b0677-142">Typ ověřování používaný pro připojení ke zdroji OData.</span><span class="sxs-lookup"><span data-stu-id="b0677-142">Type of authentication used to connect to the OData source.</span></span> <br/><br/> <span data-ttu-id="b0677-143">Možné hodnoty pro cloudové prostředí OData, jsou anonymní, základní a OAuth (Upozorňujeme, že Azure Active Directory na základě OAuth aktuálně jedinou podpory Azure Data Factory).</span><span class="sxs-lookup"><span data-stu-id="b0677-143">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="b0677-144">Pro místní OData možné hodnoty jsou anonymní, Basic a Windows.</span><span class="sxs-lookup"><span data-stu-id="b0677-144">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="b0677-145">Ano</span><span class="sxs-lookup"><span data-stu-id="b0677-145">Yes</span></span> |
| <span data-ttu-id="b0677-146">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="b0677-146">username</span></span> |<span data-ttu-id="b0677-147">Pokud používáte základní ověřování, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="b0677-147">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="b0677-148">Ano (jenom Pokud používáte základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="b0677-148">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="b0677-149">heslo</span><span class="sxs-lookup"><span data-stu-id="b0677-149">password</span></span> |<span data-ttu-id="b0677-150">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="b0677-150">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="b0677-151">Ano (jenom Pokud používáte základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="b0677-151">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="b0677-152">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="b0677-152">authorizedCredential</span></span> |<span data-ttu-id="b0677-153">Pokud používáte OAuth, klikněte na tlačítko **Autorizovat** tlačítko Průvodce kopírováním služby Data Factory nebo editoru a zadejte svoje přihlašovací údaje, pak bude automaticky generovaný hodnota této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b0677-153">If you are using OAuth, click **Authorize** button in the Data Factory Copy Wizard or Editor and enter your credential, then the value of this property will be auto-generated.</span></span> |<span data-ttu-id="b0677-154">Ano (jenom Pokud používáte ověřování OAuth)</span><span class="sxs-lookup"><span data-stu-id="b0677-154">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="b0677-155">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b0677-155">gatewayName</span></span> |<span data-ttu-id="b0677-156">Název brány, kterou služba Data Factory měla použít pro připojení ke službě OData na místě.</span><span class="sxs-lookup"><span data-stu-id="b0677-156">Name of the gateway that the Data Factory service should use to connect to the on-premises OData service.</span></span> <span data-ttu-id="b0677-157">Zadejte, pokud jsou kopírování dat z místní zdroj OData.</span><span class="sxs-lookup"><span data-stu-id="b0677-157">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="b0677-158">Ne</span><span class="sxs-lookup"><span data-stu-id="b0677-158">No</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="b0677-159">Použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="b0677-159">Using Basic authentication</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a><span data-ttu-id="b0677-160">Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="b0677-160">Using Anonymous authentication</span></span>
```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="b0677-161">Pomocí ověřování systému Windows přístup k místnímu zdroji OData</span><span class="sxs-lookup"><span data-stu-id="b0677-161">Using Windows authentication accessing on-premises OData source</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="b0677-162">Pomocí OAuth ověřování přístupu k cloudu zdroj OData</span><span class="sxs-lookup"><span data-stu-id="b0677-162">Using OAuth authentication accessing cloud OData source</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source e.g. https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="b0677-163">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="b0677-163">Dataset properties</span></span>
<span data-ttu-id="b0677-164">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b0677-164">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="b0677-165">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="b0677-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="b0677-166">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="b0677-166">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="b0677-167">Rámci typeProperties část datové sady typ **ODataResource** (což zahrnuje datová sada OData) má následující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="b0677-167">The typeProperties section for dataset of type **ODataResource** (which includes OData dataset) has the following properties</span></span>

| <span data-ttu-id="b0677-168">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b0677-168">Property</span></span> | <span data-ttu-id="b0677-169">Popis</span><span class="sxs-lookup"><span data-stu-id="b0677-169">Description</span></span> | <span data-ttu-id="b0677-170">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0677-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b0677-171">Cesta</span><span class="sxs-lookup"><span data-stu-id="b0677-171">path</span></span> |<span data-ttu-id="b0677-172">Cesta k prostředku OData</span><span class="sxs-lookup"><span data-stu-id="b0677-172">Path to the OData resource</span></span> |<span data-ttu-id="b0677-173">Ne</span><span class="sxs-lookup"><span data-stu-id="b0677-173">No</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="b0677-174">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="b0677-174">Copy activity properties</span></span>
<span data-ttu-id="b0677-175">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b0677-175">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="b0677-176">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="b0677-176">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="b0677-177">Vlastnosti dostupné v rámci typeProperties části aktivity se liší na druhé straně každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="b0677-177">Properties available in the typeProperties section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="b0677-178">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="b0677-178">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="b0677-179">Pokud je zdroj typu **RelationalSource** (která zahrnuje OData) následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="b0677-179">When source is of type **RelationalSource** (which includes OData) the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="b0677-180">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b0677-180">Property</span></span> | <span data-ttu-id="b0677-181">Popis</span><span class="sxs-lookup"><span data-stu-id="b0677-181">Description</span></span> | <span data-ttu-id="b0677-182">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0677-182">Example</span></span> | <span data-ttu-id="b0677-183">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0677-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b0677-184">query</span><span class="sxs-lookup"><span data-stu-id="b0677-184">query</span></span> |<span data-ttu-id="b0677-185">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="b0677-185">Use the custom query to read data.</span></span> |<span data-ttu-id="b0677-186">"? $select = název, popis a $top = 5"</span><span class="sxs-lookup"><span data-stu-id="b0677-186">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="b0677-187">Ne</span><span class="sxs-lookup"><span data-stu-id="b0677-187">No</span></span> |

## <a name="type-mapping-for-odata"></a><span data-ttu-id="b0677-188">Mapování typu pro OData</span><span class="sxs-lookup"><span data-stu-id="b0677-188">Type Mapping for OData</span></span>
<span data-ttu-id="b0677-189">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="b0677-189">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach.</span></span>

1. <span data-ttu-id="b0677-190">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="b0677-190">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="b0677-191">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="b0677-191">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="b0677-192">Při přesouvání dat od OData, se používají následující mapování z typů OData k typ formátu .NET.</span><span class="sxs-lookup"><span data-stu-id="b0677-192">When moving data from OData, the following mappings are used from OData types to .NET type.</span></span>

| <span data-ttu-id="b0677-193">Typ dat OData</span><span class="sxs-lookup"><span data-stu-id="b0677-193">OData Data Type</span></span> | <span data-ttu-id="b0677-194">Typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="b0677-194">.NET Type</span></span> |
| --- | --- |
| <span data-ttu-id="b0677-195">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="b0677-195">Edm.Binary</span></span> |<span data-ttu-id="b0677-196">Byte]</span><span class="sxs-lookup"><span data-stu-id="b0677-196">Byte[]</span></span> |
| <span data-ttu-id="b0677-197">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="b0677-197">Edm.Boolean</span></span> |<span data-ttu-id="b0677-198">BOOL</span><span class="sxs-lookup"><span data-stu-id="b0677-198">Bool</span></span> |
| <span data-ttu-id="b0677-199">Edm.Byte</span><span class="sxs-lookup"><span data-stu-id="b0677-199">Edm.Byte</span></span> |<span data-ttu-id="b0677-200">Byte]</span><span class="sxs-lookup"><span data-stu-id="b0677-200">Byte[]</span></span> |
| <span data-ttu-id="b0677-201">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="b0677-201">Edm.DateTime</span></span> |<span data-ttu-id="b0677-202">Data a času</span><span class="sxs-lookup"><span data-stu-id="b0677-202">DateTime</span></span> |
| <span data-ttu-id="b0677-203">Edm.Decimal</span><span class="sxs-lookup"><span data-stu-id="b0677-203">Edm.Decimal</span></span> |<span data-ttu-id="b0677-204">Decimal</span><span class="sxs-lookup"><span data-stu-id="b0677-204">Decimal</span></span> |
| <span data-ttu-id="b0677-205">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="b0677-205">Edm.Double</span></span> |<span data-ttu-id="b0677-206">Double</span><span class="sxs-lookup"><span data-stu-id="b0677-206">Double</span></span> |
| <span data-ttu-id="b0677-207">Edm.Single</span><span class="sxs-lookup"><span data-stu-id="b0677-207">Edm.Single</span></span> |<span data-ttu-id="b0677-208">Jeden</span><span class="sxs-lookup"><span data-stu-id="b0677-208">Single</span></span> |
| <span data-ttu-id="b0677-209">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="b0677-209">Edm.Guid</span></span> |<span data-ttu-id="b0677-210">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="b0677-210">Guid</span></span> |
| <span data-ttu-id="b0677-211">Edm.Int16</span><span class="sxs-lookup"><span data-stu-id="b0677-211">Edm.Int16</span></span> |<span data-ttu-id="b0677-212">Int16</span><span class="sxs-lookup"><span data-stu-id="b0677-212">Int16</span></span> |
| <span data-ttu-id="b0677-213">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="b0677-213">Edm.Int32</span></span> |<span data-ttu-id="b0677-214">Int32</span><span class="sxs-lookup"><span data-stu-id="b0677-214">Int32</span></span> |
| <span data-ttu-id="b0677-215">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="b0677-215">Edm.Int64</span></span> |<span data-ttu-id="b0677-216">Int64</span><span class="sxs-lookup"><span data-stu-id="b0677-216">Int64</span></span> |
| <span data-ttu-id="b0677-217">Edm.SByte</span><span class="sxs-lookup"><span data-stu-id="b0677-217">Edm.SByte</span></span> |<span data-ttu-id="b0677-218">Int16</span><span class="sxs-lookup"><span data-stu-id="b0677-218">Int16</span></span> |
| <span data-ttu-id="b0677-219">Edm.String</span><span class="sxs-lookup"><span data-stu-id="b0677-219">Edm.String</span></span> |<span data-ttu-id="b0677-220">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b0677-220">String</span></span> |
| <span data-ttu-id="b0677-221">Edm.Time</span><span class="sxs-lookup"><span data-stu-id="b0677-221">Edm.Time</span></span> |<span data-ttu-id="b0677-222">Časový interval</span><span class="sxs-lookup"><span data-stu-id="b0677-222">TimeSpan</span></span> |
| <span data-ttu-id="b0677-223">Edm.DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="b0677-223">Edm.DateTimeOffset</span></span> |<span data-ttu-id="b0677-224">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="b0677-224">DateTimeOffset</span></span> |

> [!Note]
> <span data-ttu-id="b0677-225">Komplexní data OData typy například objekt nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="b0677-225">OData complex data types e.g. Object are not supported.</span></span>

## <a name="json-example-copy-data-from-odata-source-to-azure-blob"></a><span data-ttu-id="b0677-226">Příklad JSON: kopírování dat ze zdroje OData do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="b0677-226">JSON example: Copy data from OData source to Azure Blob</span></span>
<span data-ttu-id="b0677-227">Tento příklad obsahuje ukázkové JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b0677-227">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="b0677-228">Se ukazují, jak zkopírovat data z zdroje OData do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="b0677-228">They show how to copy data from an OData source to an Azure Blob Storage.</span></span> <span data-ttu-id="b0677-229">Však lze zkopírovat data do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="b0677-229">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span> <span data-ttu-id="b0677-230">Ukázka má následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="b0677-230">The sample has the following Data Factory entities:</span></span>

1. <span data-ttu-id="b0677-231">Propojené služby typu [OData](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b0677-231">A linked service of type [OData](#linked-service-properties).</span></span>
2. <span data-ttu-id="b0677-232">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b0677-232">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="b0677-233">Vstup [datovou sadu](data-factory-create-datasets.md) typu [ODataResource](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b0677-233">An input [dataset](data-factory-create-datasets.md) of type [ODataResource](#dataset-properties).</span></span>
4. <span data-ttu-id="b0677-234">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b0677-234">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="b0677-235">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b0677-235">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="b0677-236">Ukázka zkopíruje data z dotazování na zdroj OData do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="b0677-236">The sample copies data from querying against an OData source to an Azure blob every hour.</span></span> <span data-ttu-id="b0677-237">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="b0677-237">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="b0677-238">**Propojená služba OData:** tento příklad používá anonymní ověřování.</span><span class="sxs-lookup"><span data-stu-id="b0677-238">**OData linked service:** This example uses the Anonymous authentication.</span></span> <span data-ttu-id="b0677-239">V tématu [OData propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="b0677-239">See [OData linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
            }
        }
}
```

<span data-ttu-id="b0677-240">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="b0677-240">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="b0677-241">**Vstupní datové sady OData:**</span><span class="sxs-lookup"><span data-stu-id="b0677-241">**OData input dataset:**</span></span>

<span data-ttu-id="b0677-242">Nastavení "externí": "PRAVDA" informuje služba Data Factory, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="b0677-242">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "typeProperties":
        {
                "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3                
        }
    }
}
```

<span data-ttu-id="b0677-243">Určení **cesta** v datové sadě definice je volitelné.</span><span class="sxs-lookup"><span data-stu-id="b0677-243">Specifying **path** in the dataset definition is optional.</span></span>

<span data-ttu-id="b0677-244">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="b0677-244">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="b0677-245">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="b0677-245">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b0677-246">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="b0677-246">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="b0677-247">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="b0677-247">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobODataDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


<span data-ttu-id="b0677-248">**Aktivita kopírování v kanálu s OData zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="b0677-248">**Copy activity in a pipeline with OData source and Blob sink:**</span></span>

<span data-ttu-id="b0677-249">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="b0677-249">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="b0677-250">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="b0677-250">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="b0677-251">Zadané pro dotaz SQL **dotazu** vlastnost vybere nejnovější (nejnovější) data ze zdroje OData.</span><span class="sxs-lookup"><span data-stu-id="b0677-251">The SQL query specified for the **query** property selects the latest (newest) data from the OData source.</span></span>

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "?$select=Name, Description&$top=5",
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "ODataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobODataDataSet"
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
                "name": "ODataToBlob"
            }
        ],
        "start": "2017-02-01T18:00:00Z",
        "end": "2017-02-03T19:00:00Z"
    }
}
```

<span data-ttu-id="b0677-252">Určení **dotazu** v kanálu definice je volitelné.</span><span class="sxs-lookup"><span data-stu-id="b0677-252">Specifying **query** in the pipeline definition is optional.</span></span> <span data-ttu-id="b0677-253">**URL** , služba Data Factory používá k načtení dat je: adresa URL zadaná v propojené službě (povinné) + cesta zadaná v datové sadě (volitelné) + dotazu v kanálu (volitelné).</span><span class="sxs-lookup"><span data-stu-id="b0677-253">The **URL** that the Data Factory service uses to retrieve data is: URL specified in the linked service (required) + path specified in the dataset (optional) + query in the pipeline (optional).</span></span>


### <a name="type-mapping-for-odata"></a><span data-ttu-id="b0677-254">Mapování typu pro OData</span><span class="sxs-lookup"><span data-stu-id="b0677-254">Type mapping for OData</span></span>
<span data-ttu-id="b0677-255">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup krok 2:</span><span class="sxs-lookup"><span data-stu-id="b0677-255">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="b0677-256">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="b0677-256">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="b0677-257">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="b0677-257">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="b0677-258">Při přesunutí dat od OData ukládá, OData datové typy jsou namapované na typy .NET.</span><span class="sxs-lookup"><span data-stu-id="b0677-258">When moving data from OData data stores, OData data types are mapped to .NET types.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="b0677-259">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="b0677-259">Map source to sink columns</span></span>
<span data-ttu-id="b0677-260">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="b0677-260">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="b0677-261">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="b0677-261">Repeatable read from relational sources</span></span>
<span data-ttu-id="b0677-262">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="b0677-262">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="b0677-263">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="b0677-263">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="b0677-264">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="b0677-264">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="b0677-265">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="b0677-265">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="b0677-266">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="b0677-266">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="b0677-267">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="b0677-267">Performance and Tuning</span></span>
<span data-ttu-id="b0677-268">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="b0677-268">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
