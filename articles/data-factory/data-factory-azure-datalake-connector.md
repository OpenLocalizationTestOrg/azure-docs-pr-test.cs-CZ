---
title: "Kopírování dat do a z Azure Data Lake Store | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data do a z Data Lake Store pomocí Azure Data Factory"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 11629fbc83f0554e2097eb4322701654c0bc2028
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="copy-data-to-and-from-data-lake-store-by-using-data-factory"></a><span data-ttu-id="57461-103">Kopírování dat do a z Data Lake Store pomocí objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="57461-103">Copy data to and from Data Lake Store by using Data Factory</span></span>
<span data-ttu-id="57461-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat do a z Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-104">This article explains how to use Copy Activity in Azure Data Factory to move data to and from Azure Data Lake Store.</span></span> <span data-ttu-id="57461-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článku přehled o přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="57461-105">It builds on the [Data movement activities](data-factory-data-movement-activities.md) article, an overview of data movement with Copy Activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="57461-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="57461-106">Supported scenarios</span></span>
<span data-ttu-id="57461-107">Může kopírovat data **z Azure Data Lake Store** ukládá do následující data:</span><span class="sxs-lookup"><span data-stu-id="57461-107">You can copy data **from Azure Data Lake Store** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="57461-108">Může kopírovat data z následujících datových úložišť **do Azure Data Lake Store**:</span><span class="sxs-lookup"><span data-stu-id="57461-108">You can copy data from the following data stores **to Azure Data Lake Store**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="57461-109">Před vytvořením kanálu s aktivitou kopírování, vytvoření účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-109">Create a Data Lake Store account before creating a pipeline with Copy Activity.</span></span> <span data-ttu-id="57461-110">Další informace najdete v tématu [Začínáme s Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="57461-110">For more information, see [Get started with Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="57461-111">Typy podporované ověřování</span><span class="sxs-lookup"><span data-stu-id="57461-111">Supported authentication types</span></span>
<span data-ttu-id="57461-112">Konektor služby Data Lake Store podporuje tyto typy ověřování:</span><span class="sxs-lookup"><span data-stu-id="57461-112">The Data Lake Store connector supports these authentication types:</span></span>
* <span data-ttu-id="57461-113">Ověřování instančních objektů</span><span class="sxs-lookup"><span data-stu-id="57461-113">Service principal authentication</span></span>
* <span data-ttu-id="57461-114">Ověření přihlašovacích údajů (OAuth) uživatele</span><span class="sxs-lookup"><span data-stu-id="57461-114">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="57461-115">Doporučujeme použít objekt zabezpečení ověřování služby, zejména pro naplánované dat kopie.</span><span class="sxs-lookup"><span data-stu-id="57461-115">We recommend that you use service principal authentication, especially for a scheduled data copy.</span></span> <span data-ttu-id="57461-116">Vypršení platnosti tokenu chování mohou nastat u ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="57461-116">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="57461-117">Podrobnosti konfigurace najdete v tématu [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="57461-117">For configuration details, see the [Linked service properties](#linked-service-properties) section.</span></span>

## <a name="get-started"></a><span data-ttu-id="57461-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="57461-118">Get started</span></span>
<span data-ttu-id="57461-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Data Lake Store pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="57461-119">You can create a pipeline with a copy activity that moves data to/from an Azure Data Lake Store by using different tools/APIs.</span></span>

<span data-ttu-id="57461-120">Nejjednodušší způsob, jak vytvořit kanál ke zkopírování dat je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="57461-120">The easiest way to create a pipeline to copy data is to use the **Copy Wizard**.</span></span> <span data-ttu-id="57461-121">Kurz týkající se vytváření kanálu pomocí Průvodce kopírováním, najdete v části [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="57461-121">For a tutorial on creating a pipeline by using the Copy Wizard, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="57461-122">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="57461-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="57461-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="57461-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="57461-124">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="57461-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="57461-125">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="57461-125">Create a **data factory**.</span></span> <span data-ttu-id="57461-126">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="57461-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="57461-127">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="57461-127">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="57461-128">Pokud jsou kopírování dat z Azure blob storage do Azure Data Lake Store, například vytvoříte dvě propojené služby k propojení pro vytváření dat svůj účet úložiště Azure a Azure Data Lake store.</span><span class="sxs-lookup"><span data-stu-id="57461-128">For example, if you are copying data from an Azure blob storage to an Azure Data Lake Store, you create two linked services to link your Azure storage account and Azure Data Lake store to your data factory.</span></span> <span data-ttu-id="57461-129">Vlastnosti propojené služby, které jsou specifické pro Azure Data Lake Store, naleznete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="57461-129">For linked service properties that are specific to Azure Data Lake Store, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="57461-130">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="57461-130">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="57461-131">V příkladu uvedených v posledním kroku vytvoříte datové sady a zadat kontejner objektů blob a složky, která obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="57461-131">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="57461-132">A vytvořte jinou datovou sadu, která určete složku a cesta k souboru v úložišti Data Lake, který obsahuje data zkopírovat z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="57461-132">And, you create another dataset to specify the folder and file path in the Data Lake store that holds the data copied from the blob storage.</span></span> <span data-ttu-id="57461-133">Vlastnosti datové sady, které jsou specifické pro Azure Data Lake Store, naleznete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="57461-133">For dataset properties that are specific to Azure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="57461-134">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="57461-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="57461-135">V příkladu již bylo zmíněno dříve použijete BlobSource jako zdroj a AzureDataLakeStoreSink jako jímku pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="57461-135">In the example mentioned earlier, you use BlobSource as a source and AzureDataLakeStoreSink as a sink for the copy activity.</span></span> <span data-ttu-id="57461-136">Podobně pokud kopírujete z Azure Data Lake Store do úložiště objektů Blob Azure, můžete použít AzureDataLakeStoreSource a BlobSink v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="57461-136">Similarly, if you are copying from Azure Data Lake Store to Azure Blob Storage, you use AzureDataLakeStoreSource  and BlobSink in the copy activity.</span></span> <span data-ttu-id="57461-137">Kopírovat vlastnosti aktivity, které jsou specifické pro Azure Data Lake Store, naleznete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="57461-137">For copy activity properties that are specific to Azure Data Lake Store, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="57461-138">Podrobnosti o tom, jak používat úložiště dat jako zdroj nebo jímka klikněte na odkaz v předchozí části pro data store.</span><span class="sxs-lookup"><span data-stu-id="57461-138">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>  

<span data-ttu-id="57461-139">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="57461-139">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="57461-140">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="57461-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="57461-141">Ukázky s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat do nebo ze Azure Data Lake Store naleznete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-data-lake-store) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="57461-141">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an Azure Data Lake Store, see [JSON examples](#json-examples-for-copying-data-to-and-from-data-lake-store) section of this article.</span></span>

<span data-ttu-id="57461-142">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k definování entit služby Data Factory, které jsou specifické pro Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-142">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Data Lake Store.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="57461-143">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="57461-143">Linked service properties</span></span>
<span data-ttu-id="57461-144">Propojená služba odkazuje na objekt pro vytváření dat úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="57461-144">A linked service links a data store to a data factory.</span></span> <span data-ttu-id="57461-145">Vytvoření propojené služby typu **AzureDataLakeStore** propojení Data Lake Store dat do data factory.</span><span class="sxs-lookup"><span data-stu-id="57461-145">You create a linked service of type **AzureDataLakeStore** to link your Data Lake Store data to your data factory.</span></span> <span data-ttu-id="57461-146">Následující tabulka popisuje elementy JSON, které jsou specifické pro Data Lake Store propojené služby.</span><span class="sxs-lookup"><span data-stu-id="57461-146">The following table describes JSON elements specific to Data Lake Store linked services.</span></span> <span data-ttu-id="57461-147">Můžete zvolit instanční objekt a ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="57461-147">You can choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="57461-148">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="57461-148">Property</span></span> | <span data-ttu-id="57461-149">Popis</span><span class="sxs-lookup"><span data-stu-id="57461-149">Description</span></span> | <span data-ttu-id="57461-150">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="57461-150">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="57461-151">**Typ**</span><span class="sxs-lookup"><span data-stu-id="57461-151">**type**</span></span> | <span data-ttu-id="57461-152">Vlastnost typu musí být nastavená na **AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="57461-152">The type property must be set to **AzureDataLakeStore**.</span></span> | <span data-ttu-id="57461-153">Ano</span><span class="sxs-lookup"><span data-stu-id="57461-153">Yes</span></span> |
| <span data-ttu-id="57461-154">**dataLakeStoreUri**</span><span class="sxs-lookup"><span data-stu-id="57461-154">**dataLakeStoreUri**</span></span> | <span data-ttu-id="57461-155">Informace o účtu Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-155">Information about the Azure Data Lake Store account.</span></span> <span data-ttu-id="57461-156">Tyto informace má jednu z následujících formátů: `https://[accountname].azuredatalakestore.net/webhdfs/v1` nebo `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="57461-156">This information takes one of the following formats: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="57461-157">Ano</span><span class="sxs-lookup"><span data-stu-id="57461-157">Yes</span></span> |
| <span data-ttu-id="57461-158">**ID předplatného**</span><span class="sxs-lookup"><span data-stu-id="57461-158">**subscriptionId**</span></span> | <span data-ttu-id="57461-159">ID předplatného Azure, ke kterému patří účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-159">Azure subscription ID to which the Data Lake Store account belongs.</span></span> | <span data-ttu-id="57461-160">Vyžaduje se pro sink</span><span class="sxs-lookup"><span data-stu-id="57461-160">Required for sink</span></span> |
| <span data-ttu-id="57461-161">**Název skupiny prostředků**</span><span class="sxs-lookup"><span data-stu-id="57461-161">**resourceGroupName**</span></span> | <span data-ttu-id="57461-162">Název skupiny prostředků Azure, ke kterému patří účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-162">Azure resource group name to which the Data Lake Store account belongs.</span></span> | <span data-ttu-id="57461-163">Vyžaduje se pro sink</span><span class="sxs-lookup"><span data-stu-id="57461-163">Required for sink</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="57461-164">Objekt zabezpečení ověřování služby (doporučeno)</span><span class="sxs-lookup"><span data-stu-id="57461-164">Service principal authentication (recommended)</span></span>
<span data-ttu-id="57461-165">Pokud chcete použít ověřování hlavní služby, zaregistrujte entitu aplikace v Azure Active Directory (Azure AD) a jí udělit přístup k Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-165">To use service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it the access to Data Lake Store.</span></span> <span data-ttu-id="57461-166">Podrobné pokyny najdete v tématu [Service-to-service ověřování](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="57461-166">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="57461-167">Poznamenejte si následující hodnoty, které můžete použít k definování propojené služby:</span><span class="sxs-lookup"><span data-stu-id="57461-167">Make note of the following values, which you use to define the linked service:</span></span>
* <span data-ttu-id="57461-168">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="57461-168">Application ID</span></span>
* <span data-ttu-id="57461-169">Klíč aplikace</span><span class="sxs-lookup"><span data-stu-id="57461-169">Application key</span></span> 
* <span data-ttu-id="57461-170">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="57461-170">Tenant ID</span></span>

> [!IMPORTANT]
> <span data-ttu-id="57461-171">Pokud použijete Průvodce kopírováním k vytváření datových kanálů, ujistěte se, že udělíte instanční objekt alespoň **čtečky** role v řízení přístupu (správu identit a přístupu) pro účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-171">If you are using the Copy Wizard to author data pipelines, make sure that you grant the service principal at least a **Reader** role in access control (identity and access management) for the Data Lake Store account.</span></span> <span data-ttu-id="57461-172">Navíc alespoň udělit instanční objekt **čtení + Execute** oprávnění kořenového adresáře Data Lake Store ("/") a její podřízené položky.</span><span class="sxs-lookup"><span data-stu-id="57461-172">Also, grant the service principal at least **Read + Execute** permission to your Data Lake Store root ("/") and its children.</span></span> <span data-ttu-id="57461-173">V opačném případě může zobrazí se zpráva "zadané přihlašovací údaje jsou neplatné."</span><span class="sxs-lookup"><span data-stu-id="57461-173">Otherwise you might see the message "The credentials provided are invalid."</span></span><br/><br/>
<span data-ttu-id="57461-174">Po vytvoření nebo aktualizaci objektu služby ve službě Azure AD, může trvat několik minut, než změny vstoupí v platnost.</span><span class="sxs-lookup"><span data-stu-id="57461-174">After you create or update a service principal in Azure AD, it can take a few minutes for the changes to take effect.</span></span> <span data-ttu-id="57461-175">Zkontrolujte instanční objekt a Data Lake Store přístup ovládacího prvku seznam (ACL) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="57461-175">Check the service principal and Data Lake Store access control list (ACL) configurations.</span></span> <span data-ttu-id="57461-176">Pokud stále zobrazí zpráva "zadané přihlašovací údaje jsou neplatné," Vyčkejte chvíli a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="57461-176">If you still see the message "The credentials provided are invalid," wait a while and try again.</span></span>

<span data-ttu-id="57461-177">Použijte objekt zabezpečení ověřování služby tak, že zadáte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="57461-177">Use service principal authentication by specifying the following properties:</span></span>

| <span data-ttu-id="57461-178">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="57461-178">Property</span></span> | <span data-ttu-id="57461-179">Popis</span><span class="sxs-lookup"><span data-stu-id="57461-179">Description</span></span> | <span data-ttu-id="57461-180">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="57461-180">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="57461-181">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="57461-181">**servicePrincipalId**</span></span> | <span data-ttu-id="57461-182">Zadejte ID aplikace klienta.</span><span class="sxs-lookup"><span data-stu-id="57461-182">Specify the application's client ID.</span></span> | <span data-ttu-id="57461-183">Ano</span><span class="sxs-lookup"><span data-stu-id="57461-183">Yes</span></span> |
| <span data-ttu-id="57461-184">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="57461-184">**servicePrincipalKey**</span></span> | <span data-ttu-id="57461-185">Zadejte klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="57461-185">Specify the application's key.</span></span> | <span data-ttu-id="57461-186">Ano</span><span class="sxs-lookup"><span data-stu-id="57461-186">Yes</span></span> |
| <span data-ttu-id="57461-187">**klienta**</span><span class="sxs-lookup"><span data-stu-id="57461-187">**tenant**</span></span> | <span data-ttu-id="57461-188">Zadejte informace o klienta (název nebo klienta domény ID) v rámci které se nachází aplikace.</span><span class="sxs-lookup"><span data-stu-id="57461-188">Specify the tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="57461-189">Můžete ji načíst podržením ukazatele myši v pravém horním rohu portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="57461-189">You can retrieve it by hovering the mouse in the upper-right corner of the Azure portal.</span></span> | <span data-ttu-id="57461-190">Ano</span><span class="sxs-lookup"><span data-stu-id="57461-190">Yes</span></span> |

<span data-ttu-id="57461-191">**Příkladu: Ověření objektu službu**</span><span class="sxs-lookup"><span data-stu-id="57461-191">**Example: Service principal authentication**</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="user-credential-authentication"></a><span data-ttu-id="57461-192">Ověření pověření uživatele</span><span class="sxs-lookup"><span data-stu-id="57461-192">User credential authentication</span></span>
<span data-ttu-id="57461-193">Alternativně můžete ověření přihlašovacích údajů uživatele ke kopírování z nebo do Data Lake Store tak, že zadáte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="57461-193">Alternatively, you can use user credential authentication to copy from or to Data Lake Store by specifying the following properties:</span></span>

| <span data-ttu-id="57461-194">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="57461-194">Property</span></span> | <span data-ttu-id="57461-195">Popis</span><span class="sxs-lookup"><span data-stu-id="57461-195">Description</span></span> | <span data-ttu-id="57461-196">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="57461-196">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="57461-197">**autorizace**</span><span class="sxs-lookup"><span data-stu-id="57461-197">**authorization**</span></span> | <span data-ttu-id="57461-198">Klikněte **Autorizovat** tlačítko v editoru služby Data Factory a zadejte svoje přihlašovací údaje, který přiřazuje URL pro autorizaci automaticky generovaný této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="57461-198">Click the **Authorize** button in the Data Factory Editor and enter your credential that assigns the autogenerated authorization URL to this property.</span></span> | <span data-ttu-id="57461-199">Ano</span><span class="sxs-lookup"><span data-stu-id="57461-199">Yes</span></span> |
| <span data-ttu-id="57461-200">**ID relace**</span><span class="sxs-lookup"><span data-stu-id="57461-200">**sessionId**</span></span> | <span data-ttu-id="57461-201">ID relace OAuth z autorizační relace OAuth.</span><span class="sxs-lookup"><span data-stu-id="57461-201">OAuth session ID from the OAuth authorization session.</span></span> <span data-ttu-id="57461-202">Každé ID relace je jedinečné a může být použit pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="57461-202">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="57461-203">Toto nastavení se automaticky generuje při pomocí editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="57461-203">This setting is automatically generated when you use the Data Factory Editor.</span></span> | <span data-ttu-id="57461-204">Ano</span><span class="sxs-lookup"><span data-stu-id="57461-204">Yes</span></span> |

<span data-ttu-id="57461-205">**Příklad: Ověření pověření uživatele**</span><span class="sxs-lookup"><span data-stu-id="57461-205">**Example: User credential authentication**</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a><span data-ttu-id="57461-206">Vypršení platnosti tokenu</span><span class="sxs-lookup"><span data-stu-id="57461-206">Token expiration</span></span>
<span data-ttu-id="57461-207">Autorizační kód, který můžete vygenerovat pomocí **Autorizovat** tlačítko vyprší po určité době.</span><span class="sxs-lookup"><span data-stu-id="57461-207">The authorization code that you generate by using the **Authorize** button expires after a certain amount of time.</span></span> <span data-ttu-id="57461-208">Tato zpráva znamená, že vypršela platnost tokenu ověřování:</span><span class="sxs-lookup"><span data-stu-id="57461-208">The following message means that the authentication token has expired:</span></span>

<span data-ttu-id="57461-209">Přihlašovací údaje chyby operace: invalid_grant - AADSTS70002: Chyba při ověřování přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="57461-209">Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="57461-210">AADSTS70008: Předloženému udělení přístupu je prošlý nebo odvolat.</span><span class="sxs-lookup"><span data-stu-id="57461-210">AADSTS70008: The provided access grant is expired or revoked.</span></span> <span data-ttu-id="57461-211">ID trasování: ID korelace d18629e8-af88-43c5-88e3-d8419eb1fca1: časové razítko fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21-09-31Z.</span><span class="sxs-lookup"><span data-stu-id="57461-211">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21-09-31Z.</span></span>

<span data-ttu-id="57461-212">V následující tabulce jsou uvedeny doby vypršení platnosti, různé typy uživatelských účtů:</span><span class="sxs-lookup"><span data-stu-id="57461-212">The following table shows the expiration times of different types of user accounts:</span></span>


| <span data-ttu-id="57461-213">Typ uživatele</span><span class="sxs-lookup"><span data-stu-id="57461-213">User type</span></span> | <span data-ttu-id="57461-214">Platnost vyprší po</span><span class="sxs-lookup"><span data-stu-id="57461-214">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="57461-215">Uživatelské účty *není* spravované službou Azure Active Directory (například @hotmail.com nebo @live.com)</span><span class="sxs-lookup"><span data-stu-id="57461-215">User accounts *not* managed by Azure Active Directory (for example, @hotmail.com or @live.com)</span></span> |<span data-ttu-id="57461-216">12 hodin</span><span class="sxs-lookup"><span data-stu-id="57461-216">12 hours</span></span> |
| <span data-ttu-id="57461-217">Účty uživatelů spravované službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="57461-217">Users accounts managed by Azure Active Directory</span></span> |<span data-ttu-id="57461-218">14 dnů po poslední řez spustit</span><span class="sxs-lookup"><span data-stu-id="57461-218">14 days after the last slice run</span></span> <br/><br/><span data-ttu-id="57461-219">90 dnů, pokud řezu založeného na základě OAuth propojené služby používá alespoň jednou za 14 dní</span><span class="sxs-lookup"><span data-stu-id="57461-219">90 days, if a slice based on an OAuth-based linked service runs at least once every 14 days</span></span> |

<span data-ttu-id="57461-220">Pokud změníte heslo před časem vypršení platnosti tokenu, platnost tokenu vyprší okamžitě.</span><span class="sxs-lookup"><span data-stu-id="57461-220">If you change your password before the token expiration time, the token expires immediately.</span></span> <span data-ttu-id="57461-221">Zobrazí se zpráva již bylo zmíněno dříve v této části.</span><span class="sxs-lookup"><span data-stu-id="57461-221">You will see the message mentioned earlier in this section.</span></span>

<span data-ttu-id="57461-222">Můžete opětovné pověření k účtu pomocí **Autorizovat** tlačítko když vyprší platnost tokenu pro propojenou službu znovu nasaďte.</span><span class="sxs-lookup"><span data-stu-id="57461-222">You can reauthorize the account by using the **Authorize** button when the token expires to redeploy the linked service.</span></span> <span data-ttu-id="57461-223">Můžete také vygenerovat hodnoty **sessionId** a **autorizace** vlastnosti programově pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="57461-223">You can also generate values for the **sessionId** and **authorization** properties programmatically by using the following code:</span></span>


```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```
<span data-ttu-id="57461-224">Podrobnosti o třídách objekt pro vytváření dat používá v kódu najdete v tématu [azuredatalakestorelinkedservice třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), a [AuthorizationSessionGetResponse třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témata.</span><span class="sxs-lookup"><span data-stu-id="57461-224">For details about the Data Factory classes used in the code, see the [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics.</span></span> <span data-ttu-id="57461-225">Přidat odkaz na verzi `2.9.10826.1824` z `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` pro `WindowsFormsWebAuthenticationDialog` třída používaná v kódu.</span><span class="sxs-lookup"><span data-stu-id="57461-225">Add a reference to version `2.9.10826.1824` of `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` for the `WindowsFormsWebAuthenticationDialog` class used in the code.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="57461-226">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="57461-226">Dataset properties</span></span>
<span data-ttu-id="57461-227">Pokud chcete zadat datové sady představují vstupní data v Data Lake Store, nastavíte **typ** vlastnosti datové sady, která **AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="57461-227">To specify a dataset to represent input data in a Data Lake Store, you set the **type** property of the dataset to **AzureDataLakeStore**.</span></span> <span data-ttu-id="57461-228">Nastavte **linkedServiceName** vlastnosti datové sady, která název Data Lake Store propojené služby.</span><span class="sxs-lookup"><span data-stu-id="57461-228">Set the **linkedServiceName** property of the dataset to the name of the Data Lake Store linked service.</span></span> <span data-ttu-id="57461-229">Úplný seznam části JSON a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="57461-229">For a full list of JSON sections and properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="57461-230">Části datové sady ve formátu JSON, jako například **struktura**, **dostupnosti**, a **zásad**, jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, databáze a tabulky Azure, např.).</span><span class="sxs-lookup"><span data-stu-id="57461-230">Sections of a dataset in JSON, such as **structure**, **availability**, and **policy**, are similar for all dataset types (Azure SQL database, Azure blob, and Azure table, for example).</span></span> <span data-ttu-id="57461-231">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace, jako je například umístění a formát dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="57461-231">The **typeProperties** section is different for each type of dataset and provides information such as location and format of the data in the data store.</span></span> 

<span data-ttu-id="57461-232">**Rámci typeProperties** části datové sady typu **AzureDataLakeStore** obsahuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="57461-232">The **typeProperties** section for a dataset of type **AzureDataLakeStore** contains the following properties:</span></span>

| <span data-ttu-id="57461-233">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="57461-233">Property</span></span> | <span data-ttu-id="57461-234">Popis</span><span class="sxs-lookup"><span data-stu-id="57461-234">Description</span></span> | <span data-ttu-id="57461-235">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="57461-235">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="57461-236">**folderPath**</span><span class="sxs-lookup"><span data-stu-id="57461-236">**folderPath**</span></span> |<span data-ttu-id="57461-237">Cesta ke kontejneru a složce v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-237">Path to the container and folder in Data Lake Store.</span></span> |<span data-ttu-id="57461-238">Ano</span><span class="sxs-lookup"><span data-stu-id="57461-238">Yes</span></span> |
| <span data-ttu-id="57461-239">**Název souboru**</span><span class="sxs-lookup"><span data-stu-id="57461-239">**fileName**</span></span> |<span data-ttu-id="57461-240">Název souboru v Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-240">Name of the file in Azure Data Lake Store.</span></span> <span data-ttu-id="57461-241">**FileName** vlastnost je volitelná a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="57461-241">The **fileName** property is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="57461-242">Pokud zadáte **fileName**, aktivitu (včetně kopie) funguje na konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="57461-242">If you specify **fileName**, the activity (including Copy) works on the specific file.</span></span><br/><br/><span data-ttu-id="57461-243">Když **fileName** není zadán, zahrnuje kopírování všech souborů v **folderPath** ve vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="57461-243">When **fileName** is not specified, Copy includes all files in **folderPath** in the input dataset.</span></span><br/><br/><span data-ttu-id="57461-244">Když **fileName** pro datovou sadu výstupů není zadána a **preserveHierarchy** není zadané v podřízený aktivity, je název vygenerovaný soubor ve formátu Data. _Identifikátor GUID_.txt'.</span><span class="sxs-lookup"><span data-stu-id="57461-244">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, the name of the generated file is in the format Data._Guid_.txt\`.</span></span> <span data-ttu-id="57461-245">Například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span><span class="sxs-lookup"><span data-stu-id="57461-245">For example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span></span> |<span data-ttu-id="57461-246">Ne</span><span class="sxs-lookup"><span data-stu-id="57461-246">No</span></span> |
| <span data-ttu-id="57461-247">**partitionedBy**</span><span class="sxs-lookup"><span data-stu-id="57461-247">**partitionedBy**</span></span> |<span data-ttu-id="57461-248">**PartitionedBy** vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="57461-248">The **partitionedBy** property is optional.</span></span> <span data-ttu-id="57461-249">Můžete ji zadat dynamické cestu a název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="57461-249">You can use it to specify a dynamic path and file name for time-series data.</span></span> <span data-ttu-id="57461-250">Například **folderPath** lze nastavit parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="57461-250">For example, **folderPath** can be parameterized for every hour of data.</span></span> <span data-ttu-id="57461-251">Podrobnosti a příklady naleznete v tématu [vlastnost partitionedBy](#using-partitionedby-property).</span><span class="sxs-lookup"><span data-stu-id="57461-251">For details and examples, see [The partitionedBy property](#using-partitionedby-property).</span></span> |<span data-ttu-id="57461-252">Ne</span><span class="sxs-lookup"><span data-stu-id="57461-252">No</span></span> |
| <span data-ttu-id="57461-253">**Formát**</span><span class="sxs-lookup"><span data-stu-id="57461-253">**format**</span></span> | <span data-ttu-id="57461-254">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, a **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="57461-254">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, and **ParquetFormat**.</span></span> <span data-ttu-id="57461-255">Nastavte **typ** vlastnost pod **formát** na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="57461-255">Set the **type** property under **format** to one of these values.</span></span> <span data-ttu-id="57461-256">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu JSON](data-factory-supported-file-and-compression-formats.md#json-format), [formát Avro](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formátu](data-factory-supported-file-and-compression-formats.md#parquet-format) v částech [formáty souborů a komprese podporovaných službou Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článku.</span><span class="sxs-lookup"><span data-stu-id="57461-256">For more information, see the [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections in the [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span> <br><br> <span data-ttu-id="57461-257">Pokud chcete zkopírovat soubory "jako-je" mezi souborové úložiště (binární kopie), přejděte `format` část v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="57461-257">If you want to copy files "as-is" between file-based stores (binary copy), skip the `format` section in both input and output dataset definitions.</span></span> |<span data-ttu-id="57461-258">Ne</span><span class="sxs-lookup"><span data-stu-id="57461-258">No</span></span> |
| <span data-ttu-id="57461-259">**komprese**</span><span class="sxs-lookup"><span data-stu-id="57461-259">**compression**</span></span> | <span data-ttu-id="57461-260">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="57461-260">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="57461-261">Podporované typy jsou **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="57461-261">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="57461-262">Jsou podporované úrovně **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="57461-262">Supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="57461-263">Další informace najdete v tématu [formáty souborů a komprese podporovaných službou Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="57461-263">For more information, see [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="57461-264">Ne</span><span class="sxs-lookup"><span data-stu-id="57461-264">No</span></span> |

### <a name="the-partitionedby-property"></a><span data-ttu-id="57461-265">Vlastnost partitionedBy</span><span class="sxs-lookup"><span data-stu-id="57461-265">The partitionedBy property</span></span>
<span data-ttu-id="57461-266">Můžete zadat dynamické **folderPath** a **fileName** vlastnosti pro data časové řady s **partitionedBy** vlastnost, funkce pro vytváření dat a systémové proměnné.</span><span class="sxs-lookup"><span data-stu-id="57461-266">You can specify dynamic **folderPath** and **fileName** properties for time-series data with the **partitionedBy** property, Data Factory functions, and system variables.</span></span> <span data-ttu-id="57461-267">Podrobnosti najdete v tématu [Azure Data Factory – funkce a systémové proměnné](data-factory-functions-variables.md) článku.</span><span class="sxs-lookup"><span data-stu-id="57461-267">For details, see the [Azure Data Factory - functions and system variables](data-factory-functions-variables.md) article.</span></span>


<span data-ttu-id="57461-268">V následujícím příkladu `{Slice}` se nahradí hodnota proměnné objektu pro vytváření dat systému `SliceStart` ve formátu určeném (`yyyyMMddHH`).</span><span class="sxs-lookup"><span data-stu-id="57461-268">In the following example, `{Slice}` is replaced with the value of the Data Factory system variable `SliceStart` in the format specified (`yyyyMMddHH`).</span></span> <span data-ttu-id="57461-269">Název `SliceStart` odkazuje na čas spuštění řezu.</span><span class="sxs-lookup"><span data-stu-id="57461-269">The name `SliceStart` refers to the start time of the slice.</span></span> <span data-ttu-id="57461-270">`folderPath` Vlastnost je jiný pro každý řez, jako v `wikidatagateway/wikisampledataout/2014100103` nebo `wikidatagateway/wikisampledataout/2014100104`.</span><span class="sxs-lookup"><span data-stu-id="57461-270">The `folderPath` property is different for each slice, as in `wikidatagateway/wikisampledataout/2014100103` or `wikidatagateway/wikisampledataout/2014100104`.</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="57461-271">V následujícím příkladu, rok, měsíc, den a čas `SliceStart` extrahují do samostatné proměnné, které jsou používány `folderPath` a `fileName` vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="57461-271">In the following example, the year, month, day, and time of `SliceStart` are extracted into separate variables that are used by the `folderPath` and `fileName` properties:</span></span>
```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
<span data-ttu-id="57461-272">Další informace o datových sad časové řady, plánování a řezů, najdete v článku [datové sady v Azure Data Factory](data-factory-create-datasets.md) a [Data Factory plánování a provádění](data-factory-scheduling-and-execution.md) články.</span><span class="sxs-lookup"><span data-stu-id="57461-272">For more details on time-series datasets, scheduling, and slices, see the [Datasets in Azure Data Factory](data-factory-create-datasets.md) and [Data Factory scheduling and execution](data-factory-scheduling-and-execution.md) articles.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="57461-273">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="57461-273">Copy activity properties</span></span>
<span data-ttu-id="57461-274">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="57461-274">For a full list of sections and properties available for defining activities, see the [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="57461-275">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="57461-275">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="57461-276">Vlastnosti dostupné ve **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="57461-276">The properties available in the **typeProperties** section of an activity vary with each activity type.</span></span> <span data-ttu-id="57461-277">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="57461-277">For a copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="57461-278">**AzureDataLakeStoreSource** podporuje následující vlastnost v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="57461-278">**AzureDataLakeStoreSource** supports the following property in the **typeProperties** section:</span></span>

| <span data-ttu-id="57461-279">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="57461-279">Property</span></span> | <span data-ttu-id="57461-280">Popis</span><span class="sxs-lookup"><span data-stu-id="57461-280">Description</span></span> | <span data-ttu-id="57461-281">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="57461-281">Allowed values</span></span> | <span data-ttu-id="57461-282">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="57461-282">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="57461-283">**rekurzivní**</span><span class="sxs-lookup"><span data-stu-id="57461-283">**recursive**</span></span> |<span data-ttu-id="57461-284">Označuje, zda je data načíst rekurzivně z podsložky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="57461-284">Indicates whether the data is read recursively from the subfolders or only from the specified folder.</span></span> |<span data-ttu-id="57461-285">True (výchozí hodnota), False.</span><span class="sxs-lookup"><span data-stu-id="57461-285">True (default value), False</span></span> |<span data-ttu-id="57461-286">Ne</span><span class="sxs-lookup"><span data-stu-id="57461-286">No</span></span> |


<span data-ttu-id="57461-287">**AzureDataLakeStoreSink** podporuje následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="57461-287">**AzureDataLakeStoreSink** supports the following properties in the **typeProperties** section:</span></span>

| <span data-ttu-id="57461-288">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="57461-288">Property</span></span> | <span data-ttu-id="57461-289">Popis</span><span class="sxs-lookup"><span data-stu-id="57461-289">Description</span></span> | <span data-ttu-id="57461-290">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="57461-290">Allowed values</span></span> | <span data-ttu-id="57461-291">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="57461-291">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="57461-292">**copyBehavior**</span><span class="sxs-lookup"><span data-stu-id="57461-292">**copyBehavior**</span></span> |<span data-ttu-id="57461-293">Určuje chování kopírování.</span><span class="sxs-lookup"><span data-stu-id="57461-293">Specifies the copy behavior.</span></span> |<span data-ttu-id="57461-294"><b>PreserveHierarchy</b>: zachovává hierarchii souborů v cílové složce.</span><span class="sxs-lookup"><span data-stu-id="57461-294"><b>PreserveHierarchy</b>: Preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="57461-295">Relativní cesta zdrojového souboru do zdrojové složky je stejný jako relativní cestu k souboru cíl k cílové složce.</span><span class="sxs-lookup"><span data-stu-id="57461-295">The relative path of source file to source folder is identical to the relative path of target file to target folder.</span></span><br/><br/><span data-ttu-id="57461-296"><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky jsou vytvořené v první úroveň cílové složky.</span><span class="sxs-lookup"><span data-stu-id="57461-296"><b>FlattenHierarchy</b>: All files from the source folder are created in the first level of the target folder.</span></span> <span data-ttu-id="57461-297">Cílové soubory se vytvoří s automaticky vygenerovanou názvy.</span><span class="sxs-lookup"><span data-stu-id="57461-297">The target files are created with autogenerated names.</span></span><br/><br/><span data-ttu-id="57461-298"><b>MergeFiles</b>: sloučí všechny soubory ze zdrojové složky pro jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="57461-298"><b>MergeFiles</b>: Merges all files from the source folder to one file.</span></span> <span data-ttu-id="57461-299">Pokud je zadaný název souboru nebo objekt blob, název souboru sloučené je zadaný název.</span><span class="sxs-lookup"><span data-stu-id="57461-299">If the file or blob name is specified, the merged file name is the specified name.</span></span> <span data-ttu-id="57461-300">Název souboru, jinak je generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="57461-300">Otherwise, the file name is autogenerated.</span></span> |<span data-ttu-id="57461-301">Ne</span><span class="sxs-lookup"><span data-stu-id="57461-301">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="57461-302">Příklady rekurzivní a copyBehavior</span><span class="sxs-lookup"><span data-stu-id="57461-302">recursive and copyBehavior examples</span></span>
<span data-ttu-id="57461-303">Tato část popisuje jejich výsledné chování pro různé kombinace hodnot rekurzivní a copyBehavior operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="57461-303">This section describes the resulting behavior of the Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="57461-304">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="57461-304">recursive</span></span> | <span data-ttu-id="57461-305">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="57461-305">copyBehavior</span></span> | <span data-ttu-id="57461-306">Výsledné chování</span><span class="sxs-lookup"><span data-stu-id="57461-306">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="57461-307">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="57461-307">true</span></span> |<span data-ttu-id="57461-308">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="57461-308">preserveHierarchy</span></span> |<span data-ttu-id="57461-309">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="57461-309">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="57461-310">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-310">Folder1</span></span><br/><span data-ttu-id="57461-311">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="57461-311">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="57461-312">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="57461-312">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="57461-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="57461-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="57461-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="57461-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="57461-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="57461-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="57461-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="57461-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="57461-317">cílové složky složku1 je vytvořena s stejná struktura jako zdroj</span><span class="sxs-lookup"><span data-stu-id="57461-317">the target folder Folder1 is created with the same structure as the source</span></span><br/><br/><span data-ttu-id="57461-318">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-318">Folder1</span></span><br/><span data-ttu-id="57461-319">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="57461-319">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="57461-320">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="57461-320">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="57461-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="57461-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="57461-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="57461-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="57461-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="57461-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="57461-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="57461-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="57461-325">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="57461-325">true</span></span> |<span data-ttu-id="57461-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="57461-326">flattenHierarchy</span></span> |<span data-ttu-id="57461-327">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="57461-327">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="57461-328">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-328">Folder1</span></span><br/><span data-ttu-id="57461-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="57461-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="57461-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="57461-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="57461-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="57461-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="57461-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="57461-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="57461-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="57461-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="57461-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="57461-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="57461-335">cíl složku1 je vytvořen s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="57461-335">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="57461-336">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-336">Folder1</span></span><br/><span data-ttu-id="57461-337">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="57461-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="57461-338">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="57461-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="57461-339">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3</span><span class="sxs-lookup"><span data-stu-id="57461-339">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="57461-340">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4</span><span class="sxs-lookup"><span data-stu-id="57461-340">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="57461-341">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5</span><span class="sxs-lookup"><span data-stu-id="57461-341">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="57461-342">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="57461-342">true</span></span> |<span data-ttu-id="57461-343">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="57461-343">mergeFiles</span></span> |<span data-ttu-id="57461-344">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="57461-344">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="57461-345">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-345">Folder1</span></span><br/><span data-ttu-id="57461-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="57461-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="57461-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="57461-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="57461-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="57461-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="57461-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="57461-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="57461-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="57461-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="57461-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="57461-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="57461-352">cíl složku1 je vytvořen s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="57461-352">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="57461-353">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-353">Folder1</span></span><br/><span data-ttu-id="57461-354">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor</span><span class="sxs-lookup"><span data-stu-id="57461-354">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="57461-355">False</span><span class="sxs-lookup"><span data-stu-id="57461-355">false</span></span> |<span data-ttu-id="57461-356">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="57461-356">preserveHierarchy</span></span> |<span data-ttu-id="57461-357">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="57461-357">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="57461-358">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-358">Folder1</span></span><br/><span data-ttu-id="57461-359">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="57461-359">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="57461-360">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="57461-360">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="57461-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="57461-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="57461-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="57461-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="57461-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="57461-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="57461-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="57461-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="57461-365">Vytvoření cílové složky složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="57461-365">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="57461-366">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-366">Folder1</span></span><br/><span data-ttu-id="57461-367">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="57461-367">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="57461-368">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="57461-368">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="57461-369">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="57461-369">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="57461-370">False</span><span class="sxs-lookup"><span data-stu-id="57461-370">false</span></span> |<span data-ttu-id="57461-371">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="57461-371">flattenHierarchy</span></span> |<span data-ttu-id="57461-372">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="57461-372">For a source folder Folder1 with the following structure:</span></span><br/><br/><span data-ttu-id="57461-373">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-373">Folder1</span></span><br/><span data-ttu-id="57461-374">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="57461-374">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="57461-375">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="57461-375">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="57461-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="57461-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="57461-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="57461-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="57461-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="57461-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="57461-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="57461-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="57461-380">Vytvoření cílové složky složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="57461-380">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="57461-381">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-381">Folder1</span></span><br/><span data-ttu-id="57461-382">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="57461-382">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="57461-383">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="57461-383">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="57461-384">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="57461-384">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="57461-385">False</span><span class="sxs-lookup"><span data-stu-id="57461-385">false</span></span> |<span data-ttu-id="57461-386">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="57461-386">mergeFiles</span></span> |<span data-ttu-id="57461-387">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="57461-387">For a source folder Folder1 with the following structure:</span></span><br/><br/><span data-ttu-id="57461-388">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-388">Folder1</span></span><br/><span data-ttu-id="57461-389">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="57461-389">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="57461-390">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="57461-390">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="57461-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="57461-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="57461-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="57461-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="57461-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="57461-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="57461-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="57461-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="57461-395">Vytvoření cílové složky složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="57461-395">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="57461-396">Složku1</span><span class="sxs-lookup"><span data-stu-id="57461-396">Folder1</span></span><br/><span data-ttu-id="57461-397">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="57461-397">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="57461-398">automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="57461-398">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="57461-399">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="57461-399">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="57461-400">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="57461-400">Supported file and compression formats</span></span>
<span data-ttu-id="57461-401">Podrobnosti najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článku.</span><span class="sxs-lookup"><span data-stu-id="57461-401">For details, see the [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span>

## <a name="json-examples-for-copying-data-to-and-from-data-lake-store"></a><span data-ttu-id="57461-402">Příklady JSON pro kopírování dat do a z Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="57461-402">JSON examples for copying data to and from Data Lake Store</span></span>
<span data-ttu-id="57461-403">Následující příklady poskytují definice JSON ukázka.</span><span class="sxs-lookup"><span data-stu-id="57461-403">The following examples provide sample JSON definitions.</span></span> <span data-ttu-id="57461-404">Tyto ukázkové definice můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="57461-404">You can use these sample definitions to create a pipeline by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="57461-405">Příklady ukazují, jak ke zkopírování dat z Data Lake Store a Azure Blob storage a do.</span><span class="sxs-lookup"><span data-stu-id="57461-405">The examples show how to copy data to and from Data Lake Store and Azure Blob storage.</span></span> <span data-ttu-id="57461-406">Nicméně je možné zkopírovat data _přímo_ ze všech zdrojů do jakéhokoli z podporovaném jímky.</span><span class="sxs-lookup"><span data-stu-id="57461-406">However, data can be copied _directly_ from any of the sources to any of the supported sinks.</span></span> <span data-ttu-id="57461-407">Další informace najdete v části "podporované úložiště dat a formáty" v [přesun dat pomocí aktivity kopírování](data-factory-data-movement-activities.md) článku.</span><span class="sxs-lookup"><span data-stu-id="57461-407">For more information, see the section "Supported data stores and formats" in the [Move data by using Copy Activity](data-factory-data-movement-activities.md) article.</span></span>  

### <a name="example-copy-data-from-azure-blob-storage-to-azure-data-lake-store"></a><span data-ttu-id="57461-408">Příklad: Kopírování dat z Azure Blob Storage do Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="57461-408">Example: Copy data from Azure Blob Storage to Azure Data Lake Store</span></span>
<span data-ttu-id="57461-409">Ukázkový kód v této části uvádí:</span><span class="sxs-lookup"><span data-stu-id="57461-409">The example code in this section shows:</span></span>

* <span data-ttu-id="57461-410">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-410">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="57461-411">Propojené služby typu [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-411">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="57461-412">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-412">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="57461-413">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-413">An output [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="57461-414">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [AzureDataLakeStoreSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-414">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [AzureDataLakeStoreSink](#copy-activity-properties).</span></span>

<span data-ttu-id="57461-415">Příklady ukazují, jak časové řady dat z Azure Blob Storage je zkopírován do Data Lake Store každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="57461-415">The examples show how time-series data from Azure Blob Storage is copied to Data Lake Store every hour.</span></span> 

<span data-ttu-id="57461-416">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="57461-416">**Azure Storage linked service**</span></span>

```JSON
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

<span data-ttu-id="57461-417">**Propojená služba Azure Data Lake Store**</span><span class="sxs-lookup"><span data-stu-id="57461-417">**Azure Data Lake Store linked service**</span></span>

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="57461-418">Podrobnosti konfigurace najdete v tématu [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="57461-418">For configuration details, see the [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="57461-419">**Vstupní datová sada Azure Blob**</span><span class="sxs-lookup"><span data-stu-id="57461-419">**Azure blob input dataset**</span></span>

<span data-ttu-id="57461-420">V následujícím příkladu dat je převzata z nového objektu blob každou hodinu (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="57461-420">In the following example, data is picked up from a new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="57461-421">Název složky a cesta k souboru pro tento objekt blob se vyhodnocují dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="57461-421">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="57461-422">Cesta ke složce používá rok, měsíc a den část čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="57461-422">The folder path uses the year, month, and day portion of the start time.</span></span> <span data-ttu-id="57461-423">Název souboru používá hodinu část čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="57461-423">The file name uses the hour portion of the start time.</span></span> <span data-ttu-id="57461-424">`"external": true` Nastavení služba Data Factory informuje, že v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="57461-424">The `"external": true` setting informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

<span data-ttu-id="57461-425">**Azure Data Lake Store výstupní datovou sadu**</span><span class="sxs-lookup"><span data-stu-id="57461-425">**Azure Data Lake Store output dataset**</span></span>

<span data-ttu-id="57461-426">Následující příklad zkopíruje data do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="57461-426">The following example copies data to Data Lake Store.</span></span> <span data-ttu-id="57461-427">Nová data se zkopírují do Data Lake Store každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="57461-427">New data is copied to Data Lake Store every hour.</span></span>

```JSON
{
    "name": "AzureDataLakeStoreOutput",
      "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
              "frequency": "Hour",
              "interval": 1
        }
      }
}
```


<span data-ttu-id="57461-428">**Aktivita kopírování v kanálu s blob zdroj a jímka Data Lake Store**</span><span class="sxs-lookup"><span data-stu-id="57461-428">**Copy activity in a pipeline with a blob source and a Data Lake Store sink**</span></span>

<span data-ttu-id="57461-429">V následujícím příkladu kanál obsahuje aktivitu kopírování, která je nakonfigurována pro používání vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="57461-429">In the following example, the pipeline contains a copy activity that is configured to use the input and output datasets.</span></span> <span data-ttu-id="57461-430">Aktivita kopírování je naplánována na každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="57461-430">The copy activity is scheduled to run every hour.</span></span> <span data-ttu-id="57461-431">V definici JSON kanálu `source` je typ nastaven na `BlobSource`a `sink` je typ nastaven na `AzureDataLakeStoreSink`.</span><span class="sxs-lookup"><span data-stu-id="57461-431">In the pipeline JSON definition, the `source` type is set to `BlobSource`, and the `sink` type is set to `AzureDataLakeStoreSink`.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":
    {  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [  
              {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureBlobInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureDataLakeStoreOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                      },
                      "sink": {
                        "type": "AzureDataLakeStoreSink"
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

### <a name="example-copy-data-from-azure-data-lake-store-to-an-azure-blob"></a><span data-ttu-id="57461-432">Příklad: Kopírování dat z Azure Data Lake Store do objektu blob Azure</span><span class="sxs-lookup"><span data-stu-id="57461-432">Example: Copy data from Azure Data Lake Store to an Azure blob</span></span>
<span data-ttu-id="57461-433">Ukázkový kód v této části uvádí:</span><span class="sxs-lookup"><span data-stu-id="57461-433">The example code in this section shows:</span></span>

* <span data-ttu-id="57461-434">Propojené služby typu [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-434">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="57461-435">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-435">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="57461-436">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-436">An input [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="57461-437">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-437">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="57461-438">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [AzureDataLakeStoreSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="57461-438">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [AzureDataLakeStoreSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="57461-439">Kód kopíruje data časové řady z Data Lake Store do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="57461-439">The code copies time-series data from Data Lake Store to an Azure blob every hour.</span></span> 

<span data-ttu-id="57461-440">**Propojená služba Azure Data Lake Store**</span><span class="sxs-lookup"><span data-stu-id="57461-440">**Azure Data Lake Store linked service**</span></span>

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="57461-441">Podrobnosti konfigurace najdete v tématu [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="57461-441">For configuration details, see the [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="57461-442">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="57461-442">**Azure Storage linked service**</span></span>

```JSON
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
<span data-ttu-id="57461-443">**Vstupní datové sady Azure Data Lake**</span><span class="sxs-lookup"><span data-stu-id="57461-443">**Azure Data Lake input dataset**</span></span>

<span data-ttu-id="57461-444">V tomto příkladu nastavení `"external"` k `true` služba Data Factory informuje, že v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="57461-444">In this example, setting `"external"` to `true` informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "AzureDataLakeStoreInput",
      "properties":
    {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
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
<span data-ttu-id="57461-445">**Výstupní datová sada Azure Blob**</span><span class="sxs-lookup"><span data-stu-id="57461-445">**Azure blob output dataset**</span></span>

<span data-ttu-id="57461-446">V následujícím příkladu, data se zapisují do nového objektu blob každou hodinu (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="57461-446">In the following example, data is written to a new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="57461-447">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="57461-447">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="57461-448">Cesta ke složce používá rok, měsíc, den a čas část čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="57461-448">The folder path uses the year, month, day, and hours portion of the start time.</span></span>

```JSON
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

<span data-ttu-id="57461-449">**Aktivita kopírování v kanálu s na Azure Data Lake Store zdroj a jímka objektů blob**</span><span class="sxs-lookup"><span data-stu-id="57461-449">**A copy activity in a pipeline with an Azure Data Lake Store source and a blob sink**</span></span>

<span data-ttu-id="57461-450">V následujícím příkladu kanál obsahuje aktivitu kopírování, která je nakonfigurována pro používání vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="57461-450">In the following example, the pipeline contains a copy activity that is configured to use the input and output datasets.</span></span> <span data-ttu-id="57461-451">Aktivita kopírování je naplánována na každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="57461-451">The copy activity is scheduled to run every hour.</span></span> <span data-ttu-id="57461-452">V definici JSON kanálu `source` je typ nastaven na `AzureDataLakeStoreSource`a `sink` je typ nastaven na `BlobSink`.</span><span class="sxs-lookup"><span data-stu-id="57461-452">In the pipeline JSON definition, the `source` type is set to `AzureDataLakeStoreSource`, and the `sink` type is set to `BlobSink`.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureDataLakeStoreInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
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

<span data-ttu-id="57461-453">V definici aktivity kopírování můžete namapovat sloupců z datové sady zdroje sloupců v datové sadě jímky.</span><span class="sxs-lookup"><span data-stu-id="57461-453">In the copy activity definition, you can also map columns from the source dataset to columns in the sink dataset.</span></span> <span data-ttu-id="57461-454">Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="57461-454">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="57461-455">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="57461-455">Performance and tuning</span></span>
<span data-ttu-id="57461-456">Další informace o faktory, které ovlivňují výkon aktivity kopírování a jak ji optimalizovat najdete v tématu [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md) článku.</span><span class="sxs-lookup"><span data-stu-id="57461-456">To learn about the factors that affect Copy Activity performance and how to optimize it, see the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) article.</span></span>
