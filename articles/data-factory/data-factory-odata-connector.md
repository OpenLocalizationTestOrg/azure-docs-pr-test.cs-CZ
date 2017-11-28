---
title: aaaMove data ze zdroje OData | Microsoft Docs
description: "Informace o tom, jak zdroje dat toomove z OData pomocí Azure Data Factory."
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
ms.openlocfilehash: 328efe4fd274fb3e54c1d2f209e4614c77c1ff37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a><span data-ttu-id="ffa85-103">Přesun dat z OData zdroje pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="ffa85-103">Move data From a OData source using Azure Data Factory</span></span>
<span data-ttu-id="ffa85-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data ze zdroje OData.</span><span class="sxs-lookup"><span data-stu-id="ffa85-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an OData source.</span></span> <span data-ttu-id="ffa85-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="ffa85-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="ffa85-106">Může kopírovat data z úložiště dat podřízený tooany podporované zdroj OData.</span><span class="sxs-lookup"><span data-stu-id="ffa85-106">You can copy data from an OData source tooany supported sink data store.</span></span> <span data-ttu-id="ffa85-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="ffa85-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="ffa85-108">Objekt pro vytváření dat v současné době podporuje pouze přesouvání dat od OData zdroj tooother uloží, ale ne pro přesun dat z jiných dat ukládá tooan zdroj OData.</span><span class="sxs-lookup"><span data-stu-id="ffa85-108">Data factory currently supports only moving data from an OData source tooother data stores, but not for moving data from other data stores tooan OData source.</span></span> 

## <a name="supported-versions-and-authentication-types"></a><span data-ttu-id="ffa85-109">Podporované verze a typy ověřování</span><span class="sxs-lookup"><span data-stu-id="ffa85-109">Supported versions and authentication types</span></span>
<span data-ttu-id="ffa85-110">Tento konektor OData podporu OData verze 3.0 a 4.0 a můžete kopírovat data z obou cloudu OData a místním zdrojům OData.</span><span class="sxs-lookup"><span data-stu-id="ffa85-110">This OData connector support OData version 3.0 and 4.0, and you can copy data from both cloud OData and on-premises OData sources.</span></span> <span data-ttu-id="ffa85-111">Pro pozdější hello budete potřebovat tooinstall hello Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="ffa85-111">For hello latter, you need tooinstall hello Data Management Gateway.</span></span> <span data-ttu-id="ffa85-112">V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) podrobnosti o Brána pro správu dat najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="ffa85-112">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

<span data-ttu-id="ffa85-113">Níže ověřování jsou podporované typy:</span><span class="sxs-lookup"><span data-stu-id="ffa85-113">Below authentication types are supported:</span></span>

* <span data-ttu-id="ffa85-114">tooaccess **cloudu** datového kanálu OData, můžete použít anonymní, základní (uživatelské jméno a heslo) nebo Azure Active Directory na základě ověřování OAuth.</span><span class="sxs-lookup"><span data-stu-id="ffa85-114">tooaccess **cloud** OData feed, you can use anonymous, basic (user name and password), or Azure Active Directory based OAuth authentication.</span></span>
* <span data-ttu-id="ffa85-115">tooaccess **místní** datového kanálu OData, můžete použít anonymní, základní (uživatelské jméno a heslo) nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="ffa85-115">tooaccess **on-premises** OData feed, you can use anonymous, basic (user name and password), or Windows authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="ffa85-116">Začínáme</span><span class="sxs-lookup"><span data-stu-id="ffa85-116">Getting started</span></span>
<span data-ttu-id="ffa85-117">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje OData pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ffa85-117">You can create a pipeline with a copy activity that moves data from an OData source by using different tools/APIs.</span></span>

<span data-ttu-id="ffa85-118">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="ffa85-118">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="ffa85-119">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="ffa85-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="ffa85-120">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru **, **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="ffa85-120">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="ffa85-121">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="ffa85-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="ffa85-122">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="ffa85-122">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="ffa85-123">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="ffa85-123">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="ffa85-124">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="ffa85-124">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="ffa85-125">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="ffa85-125">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="ffa85-126">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="ffa85-126">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="ffa85-127">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="ffa85-127">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="ffa85-128">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data ze zdroje OData, naleznete v tématu [JSON příklad: kopírování dat z tooAzure OData zdroje Blob](#json-example-copy-data-from-odata-source-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="ffa85-128">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an OData source, see [JSON example: Copy data from OData source tooAzure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="ffa85-129">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooOData zdroje:</span><span class="sxs-lookup"><span data-stu-id="ffa85-129">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooOData source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="ffa85-130">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="ffa85-130">Linked Service properties</span></span>
<span data-ttu-id="ffa85-131">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooOData propojené služby.</span><span class="sxs-lookup"><span data-stu-id="ffa85-131">hello following table provides description for JSON elements specific tooOData linked service.</span></span>

| <span data-ttu-id="ffa85-132">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ffa85-132">Property</span></span> | <span data-ttu-id="ffa85-133">Popis</span><span class="sxs-lookup"><span data-stu-id="ffa85-133">Description</span></span> | <span data-ttu-id="ffa85-134">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ffa85-134">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ffa85-135">type</span><span class="sxs-lookup"><span data-stu-id="ffa85-135">type</span></span> |<span data-ttu-id="ffa85-136">vlastnost typu Hello musí být nastavena na: **OData**</span><span class="sxs-lookup"><span data-stu-id="ffa85-136">hello type property must be set to: **OData**</span></span> |<span data-ttu-id="ffa85-137">Ano</span><span class="sxs-lookup"><span data-stu-id="ffa85-137">Yes</span></span> |
| <span data-ttu-id="ffa85-138">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="ffa85-138">url</span></span> |<span data-ttu-id="ffa85-139">Adresa URL služby OData hello.</span><span class="sxs-lookup"><span data-stu-id="ffa85-139">Url of hello OData service.</span></span> |<span data-ttu-id="ffa85-140">Ano</span><span class="sxs-lookup"><span data-stu-id="ffa85-140">Yes</span></span> |
| <span data-ttu-id="ffa85-141">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="ffa85-141">authenticationType</span></span> |<span data-ttu-id="ffa85-142">Typ ověřování používá tooconnect toohello OData zdroje.</span><span class="sxs-lookup"><span data-stu-id="ffa85-142">Type of authentication used tooconnect toohello OData source.</span></span> <br/><br/> <span data-ttu-id="ffa85-143">Možné hodnoty pro cloudové prostředí OData, jsou anonymní, základní a OAuth (Upozorňujeme, že Azure Active Directory na základě OAuth aktuálně jedinou podpory Azure Data Factory).</span><span class="sxs-lookup"><span data-stu-id="ffa85-143">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="ffa85-144">Pro místní OData možné hodnoty jsou anonymní, Basic a Windows.</span><span class="sxs-lookup"><span data-stu-id="ffa85-144">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="ffa85-145">Ano</span><span class="sxs-lookup"><span data-stu-id="ffa85-145">Yes</span></span> |
| <span data-ttu-id="ffa85-146">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="ffa85-146">username</span></span> |<span data-ttu-id="ffa85-147">Pokud používáte základní ověřování, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="ffa85-147">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="ffa85-148">Ano (jenom Pokud používáte základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="ffa85-148">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="ffa85-149">heslo</span><span class="sxs-lookup"><span data-stu-id="ffa85-149">password</span></span> |<span data-ttu-id="ffa85-150">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="ffa85-150">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="ffa85-151">Ano (jenom Pokud používáte základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="ffa85-151">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="ffa85-152">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="ffa85-152">authorizedCredential</span></span> |<span data-ttu-id="ffa85-153">Pokud používáte OAuth, klikněte na tlačítko **Autorizovat** tlačítko v hello Průvodce kopírováním služby Data Factory editoru nebo editoru a zadejte svoje přihlašovací údaje, pak bude automaticky generovaný hello hodnota této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ffa85-153">If you are using OAuth, click **Authorize** button in hello Data Factory Copy Wizard or Editor and enter your credential, then hello value of this property will be auto-generated.</span></span> |<span data-ttu-id="ffa85-154">Ano (jenom Pokud používáte ověřování OAuth)</span><span class="sxs-lookup"><span data-stu-id="ffa85-154">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="ffa85-155">gatewayName</span><span class="sxs-lookup"><span data-stu-id="ffa85-155">gatewayName</span></span> |<span data-ttu-id="ffa85-156">Název hello brány, kterou hello služba Data Factory měli používat službu tooconnect toohello místní OData.</span><span class="sxs-lookup"><span data-stu-id="ffa85-156">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises OData service.</span></span> <span data-ttu-id="ffa85-157">Zadejte, pokud jsou kopírování dat z místní zdroj OData.</span><span class="sxs-lookup"><span data-stu-id="ffa85-157">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="ffa85-158">Ne</span><span class="sxs-lookup"><span data-stu-id="ffa85-158">No</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="ffa85-159">Použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="ffa85-159">Using Basic authentication</span></span>
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

### <a name="using-anonymous-authentication"></a><span data-ttu-id="ffa85-160">Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="ffa85-160">Using Anonymous authentication</span></span>
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

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="ffa85-161">Pomocí ověřování systému Windows přístup k místnímu zdroji OData</span><span class="sxs-lookup"><span data-stu-id="ffa85-161">Using Windows authentication accessing on-premises OData source</span></span>
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

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="ffa85-162">Pomocí OAuth ověřování přístupu k cloudu zdroj OData</span><span class="sxs-lookup"><span data-stu-id="ffa85-162">Using OAuth authentication accessing cloud OData source</span></span>
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
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="ffa85-163">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="ffa85-163">Dataset properties</span></span>
<span data-ttu-id="ffa85-164">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ffa85-164">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="ffa85-165">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="ffa85-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="ffa85-166">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="ffa85-166">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="ffa85-167">rámci typeProperties Hello část datové sady typ **ODataResource** (což zahrnuje datová sada OData) má následující vlastnosti hello</span><span class="sxs-lookup"><span data-stu-id="ffa85-167">hello typeProperties section for dataset of type **ODataResource** (which includes OData dataset) has hello following properties</span></span>

| <span data-ttu-id="ffa85-168">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ffa85-168">Property</span></span> | <span data-ttu-id="ffa85-169">Popis</span><span class="sxs-lookup"><span data-stu-id="ffa85-169">Description</span></span> | <span data-ttu-id="ffa85-170">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ffa85-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ffa85-171">Cesta</span><span class="sxs-lookup"><span data-stu-id="ffa85-171">path</span></span> |<span data-ttu-id="ffa85-172">Toohello cestu OData prostředků</span><span class="sxs-lookup"><span data-stu-id="ffa85-172">Path toohello OData resource</span></span> |<span data-ttu-id="ffa85-173">Ne</span><span class="sxs-lookup"><span data-stu-id="ffa85-173">No</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="ffa85-174">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="ffa85-174">Copy activity properties</span></span>
<span data-ttu-id="ffa85-175">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ffa85-175">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="ffa85-176">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="ffa85-176">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="ffa85-177">Vlastnosti dostupné v rámci typeProperties části hello hello aktivit na hello druhé straně lišit každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="ffa85-177">Properties available in hello typeProperties section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="ffa85-178">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="ffa85-178">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="ffa85-179">Pokud je zdroj typu **RelationalSource** (která zahrnuje OData) hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="ffa85-179">When source is of type **RelationalSource** (which includes OData) hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="ffa85-180">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ffa85-180">Property</span></span> | <span data-ttu-id="ffa85-181">Popis</span><span class="sxs-lookup"><span data-stu-id="ffa85-181">Description</span></span> | <span data-ttu-id="ffa85-182">Příklad</span><span class="sxs-lookup"><span data-stu-id="ffa85-182">Example</span></span> | <span data-ttu-id="ffa85-183">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ffa85-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ffa85-184">query</span><span class="sxs-lookup"><span data-stu-id="ffa85-184">query</span></span> |<span data-ttu-id="ffa85-185">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="ffa85-185">Use hello custom query tooread data.</span></span> |<span data-ttu-id="ffa85-186">"? $select = název, popis a $top = 5"</span><span class="sxs-lookup"><span data-stu-id="ffa85-186">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="ffa85-187">Ne</span><span class="sxs-lookup"><span data-stu-id="ffa85-187">No</span></span> |

## <a name="type-mapping-for-odata"></a><span data-ttu-id="ffa85-188">Mapování typu pro OData</span><span class="sxs-lookup"><span data-stu-id="ffa85-188">Type Mapping for OData</span></span>
<span data-ttu-id="ffa85-189">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující dvoustupňový přístup.</span><span class="sxs-lookup"><span data-stu-id="ffa85-189">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach.</span></span>

1. <span data-ttu-id="ffa85-190">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="ffa85-190">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="ffa85-191">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ffa85-191">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="ffa85-192">Při přesouvání dat od OData, hello následující mapování se používají z typu too.NET typy OData.</span><span class="sxs-lookup"><span data-stu-id="ffa85-192">When moving data from OData, hello following mappings are used from OData types too.NET type.</span></span>

| <span data-ttu-id="ffa85-193">Typ dat OData</span><span class="sxs-lookup"><span data-stu-id="ffa85-193">OData Data Type</span></span> | <span data-ttu-id="ffa85-194">Typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="ffa85-194">.NET Type</span></span> |
| --- | --- |
| <span data-ttu-id="ffa85-195">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="ffa85-195">Edm.Binary</span></span> |<span data-ttu-id="ffa85-196">Byte]</span><span class="sxs-lookup"><span data-stu-id="ffa85-196">Byte[]</span></span> |
| <span data-ttu-id="ffa85-197">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="ffa85-197">Edm.Boolean</span></span> |<span data-ttu-id="ffa85-198">BOOL</span><span class="sxs-lookup"><span data-stu-id="ffa85-198">Bool</span></span> |
| <span data-ttu-id="ffa85-199">Edm.Byte</span><span class="sxs-lookup"><span data-stu-id="ffa85-199">Edm.Byte</span></span> |<span data-ttu-id="ffa85-200">Byte]</span><span class="sxs-lookup"><span data-stu-id="ffa85-200">Byte[]</span></span> |
| <span data-ttu-id="ffa85-201">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="ffa85-201">Edm.DateTime</span></span> |<span data-ttu-id="ffa85-202">Data a času</span><span class="sxs-lookup"><span data-stu-id="ffa85-202">DateTime</span></span> |
| <span data-ttu-id="ffa85-203">Edm.Decimal</span><span class="sxs-lookup"><span data-stu-id="ffa85-203">Edm.Decimal</span></span> |<span data-ttu-id="ffa85-204">Decimal</span><span class="sxs-lookup"><span data-stu-id="ffa85-204">Decimal</span></span> |
| <span data-ttu-id="ffa85-205">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="ffa85-205">Edm.Double</span></span> |<span data-ttu-id="ffa85-206">Double</span><span class="sxs-lookup"><span data-stu-id="ffa85-206">Double</span></span> |
| <span data-ttu-id="ffa85-207">Edm.Single</span><span class="sxs-lookup"><span data-stu-id="ffa85-207">Edm.Single</span></span> |<span data-ttu-id="ffa85-208">Jeden</span><span class="sxs-lookup"><span data-stu-id="ffa85-208">Single</span></span> |
| <span data-ttu-id="ffa85-209">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="ffa85-209">Edm.Guid</span></span> |<span data-ttu-id="ffa85-210">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="ffa85-210">Guid</span></span> |
| <span data-ttu-id="ffa85-211">Edm.Int16</span><span class="sxs-lookup"><span data-stu-id="ffa85-211">Edm.Int16</span></span> |<span data-ttu-id="ffa85-212">Int16</span><span class="sxs-lookup"><span data-stu-id="ffa85-212">Int16</span></span> |
| <span data-ttu-id="ffa85-213">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="ffa85-213">Edm.Int32</span></span> |<span data-ttu-id="ffa85-214">Int32</span><span class="sxs-lookup"><span data-stu-id="ffa85-214">Int32</span></span> |
| <span data-ttu-id="ffa85-215">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="ffa85-215">Edm.Int64</span></span> |<span data-ttu-id="ffa85-216">Int64</span><span class="sxs-lookup"><span data-stu-id="ffa85-216">Int64</span></span> |
| <span data-ttu-id="ffa85-217">Edm.SByte</span><span class="sxs-lookup"><span data-stu-id="ffa85-217">Edm.SByte</span></span> |<span data-ttu-id="ffa85-218">Int16</span><span class="sxs-lookup"><span data-stu-id="ffa85-218">Int16</span></span> |
| <span data-ttu-id="ffa85-219">Edm.String</span><span class="sxs-lookup"><span data-stu-id="ffa85-219">Edm.String</span></span> |<span data-ttu-id="ffa85-220">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ffa85-220">String</span></span> |
| <span data-ttu-id="ffa85-221">Edm.Time</span><span class="sxs-lookup"><span data-stu-id="ffa85-221">Edm.Time</span></span> |<span data-ttu-id="ffa85-222">Časový interval</span><span class="sxs-lookup"><span data-stu-id="ffa85-222">TimeSpan</span></span> |
| <span data-ttu-id="ffa85-223">Edm.DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ffa85-223">Edm.DateTimeOffset</span></span> |<span data-ttu-id="ffa85-224">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ffa85-224">DateTimeOffset</span></span> |

> [!Note]
> <span data-ttu-id="ffa85-225">Komplexní data OData typy například objekt nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="ffa85-225">OData complex data types e.g. Object are not supported.</span></span>

## <a name="json-example-copy-data-from-odata-source-tooazure-blob"></a><span data-ttu-id="ffa85-226">Příklad JSON: kopírování dat z tooAzure zdroj OData objektů Blob</span><span class="sxs-lookup"><span data-stu-id="ffa85-226">JSON example: Copy data from OData source tooAzure Blob</span></span>
<span data-ttu-id="ffa85-227">Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ffa85-227">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="ffa85-228">Ukazují jak zdroje dat toocopy z OData tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ffa85-228">They show how toocopy data from an OData source tooan Azure Blob Storage.</span></span> <span data-ttu-id="ffa85-229">Data však mohou být zkopírovaný tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ffa85-229">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span> <span data-ttu-id="ffa85-230">Ukázka Hello má hello následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="ffa85-230">hello sample has hello following Data Factory entities:</span></span>

1. <span data-ttu-id="ffa85-231">Propojené služby typu [OData](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ffa85-231">A linked service of type [OData](#linked-service-properties).</span></span>
2. <span data-ttu-id="ffa85-232">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ffa85-232">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="ffa85-233">Vstup [datovou sadu](data-factory-create-datasets.md) typu [ODataResource](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ffa85-233">An input [dataset](data-factory-create-datasets.md) of type [ODataResource](#dataset-properties).</span></span>
4. <span data-ttu-id="ffa85-234">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ffa85-234">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="ffa85-235">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="ffa85-235">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="ffa85-236">Ukázka Hello zkopíruje data z dotazování na zdroj tooan OData objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="ffa85-236">hello sample copies data from querying against an OData source tooan Azure blob every hour.</span></span> <span data-ttu-id="ffa85-237">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="ffa85-237">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="ffa85-238">**Propojená služba OData:** tento příklad používá hello anonymní ověřování.</span><span class="sxs-lookup"><span data-stu-id="ffa85-238">**OData linked service:** This example uses hello Anonymous authentication.</span></span> <span data-ttu-id="ffa85-239">V tématu [OData propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="ffa85-239">See [OData linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="ffa85-240">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="ffa85-240">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="ffa85-241">**Vstupní datové sady OData:**</span><span class="sxs-lookup"><span data-stu-id="ffa85-241">**OData input dataset:**</span></span>

<span data-ttu-id="ffa85-242">Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="ffa85-242">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="ffa85-243">Určení **cesta** v datové sadě hello definice je volitelné.</span><span class="sxs-lookup"><span data-stu-id="ffa85-243">Specifying **path** in hello dataset definition is optional.</span></span>

<span data-ttu-id="ffa85-244">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="ffa85-244">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="ffa85-245">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="ffa85-245">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="ffa85-246">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="ffa85-246">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="ffa85-247">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="ffa85-247">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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


<span data-ttu-id="ffa85-248">**Aktivita kopírování v kanálu s OData zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="ffa85-248">**Copy activity in a pipeline with OData source and Blob sink:**</span></span>

<span data-ttu-id="ffa85-249">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="ffa85-249">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="ffa85-250">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**RelationalSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="ffa85-250">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="ffa85-251">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello nejnovější (nejnovější) data ze zdroje hello OData.</span><span class="sxs-lookup"><span data-stu-id="ffa85-251">hello SQL query specified for hello **query** property selects hello latest (newest) data from hello OData source.</span></span>

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

<span data-ttu-id="ffa85-252">Určení **dotazu** v kanálu hello definice je volitelné.</span><span class="sxs-lookup"><span data-stu-id="ffa85-252">Specifying **query** in hello pipeline definition is optional.</span></span> <span data-ttu-id="ffa85-253">Hello **URL** je, že služba Data Factory hello používá tooretrieve data: adresa URL zadaná v hello propojené služby (povinné) + cesta zadaná v datové sadě hello (volitelné) + dotazu v kanálu hello (volitelné).</span><span class="sxs-lookup"><span data-stu-id="ffa85-253">hello **URL** that hello Data Factory service uses tooretrieve data is: URL specified in hello linked service (required) + path specified in hello dataset (optional) + query in hello pipeline (optional).</span></span>


### <a name="type-mapping-for-odata"></a><span data-ttu-id="ffa85-254">Mapování typu pro OData</span><span class="sxs-lookup"><span data-stu-id="ffa85-254">Type mapping for OData</span></span>
<span data-ttu-id="ffa85-255">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:</span><span class="sxs-lookup"><span data-stu-id="ffa85-255">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="ffa85-256">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="ffa85-256">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="ffa85-257">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ffa85-257">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="ffa85-258">Při přesouvání dat z úložiště dat OData, OData datové typy jsou typy namapované too.NET.</span><span class="sxs-lookup"><span data-stu-id="ffa85-258">When moving data from OData data stores, OData data types are mapped too.NET types.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="ffa85-259">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="ffa85-259">Map source toosink columns</span></span>
<span data-ttu-id="ffa85-260">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="ffa85-260">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="ffa85-261">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="ffa85-261">Repeatable read from relational sources</span></span>
<span data-ttu-id="ffa85-262">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="ffa85-262">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="ffa85-263">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="ffa85-263">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="ffa85-264">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="ffa85-264">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="ffa85-265">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="ffa85-265">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="ffa85-266">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="ffa85-266">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="ffa85-267">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="ffa85-267">Performance and Tuning</span></span>
<span data-ttu-id="ffa85-268">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="ffa85-268">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
