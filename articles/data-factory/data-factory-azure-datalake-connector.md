---
title: aaaCopy tooand dat z Azure Data Lake Store | Microsoft Docs
description: "Zjistěte, jak toocopy tooand dat z Data Lake Store pomocí Azure Data Factory"
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
ms.openlocfilehash: 2e78f75f3821738332dacf70f6bf2c16f0136408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-data-lake-store-by-using-data-factory"></a><span data-ttu-id="c17d1-103">Zkopírujte tooand dat z Data Lake Store pomocí objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="c17d1-103">Copy data tooand from Data Lake Store by using Data Factory</span></span>
<span data-ttu-id="c17d1-104">Tento článek vysvětluje, jak toouse aktivitu kopírování v Azure Data Factory toomove data tooand z Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c17d1-104">This article explains how toouse Copy Activity in Azure Data Factory toomove data tooand from Azure Data Lake Store.</span></span> <span data-ttu-id="c17d1-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku přehled o přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="c17d1-105">It builds on hello [Data movement activities](data-factory-data-movement-activities.md) article, an overview of data movement with Copy Activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="c17d1-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="c17d1-106">Supported scenarios</span></span>
<span data-ttu-id="c17d1-107">Může kopírovat data **z Azure Data Lake Store** toohello následující úložišť dat:</span><span class="sxs-lookup"><span data-stu-id="c17d1-107">You can copy data **from Azure Data Lake Store** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="c17d1-108">Data můžete zkopírovat z hello následující úložišť dat **tooAzure Data Lake Store**:</span><span class="sxs-lookup"><span data-stu-id="c17d1-108">You can copy data from hello following data stores **tooAzure Data Lake Store**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="c17d1-109">Před vytvořením kanálu s aktivitou kopírování, vytvoření účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c17d1-109">Create a Data Lake Store account before creating a pipeline with Copy Activity.</span></span> <span data-ttu-id="c17d1-110">Další informace najdete v tématu [Začínáme s Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c17d1-110">For more information, see [Get started with Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="c17d1-111">Typy podporované ověřování</span><span class="sxs-lookup"><span data-stu-id="c17d1-111">Supported authentication types</span></span>
<span data-ttu-id="c17d1-112">konektor Hello Data Lake Store podporuje tyto typy ověřování:</span><span class="sxs-lookup"><span data-stu-id="c17d1-112">hello Data Lake Store connector supports these authentication types:</span></span>
* <span data-ttu-id="c17d1-113">Ověřování instančních objektů</span><span class="sxs-lookup"><span data-stu-id="c17d1-113">Service principal authentication</span></span>
* <span data-ttu-id="c17d1-114">Ověření přihlašovacích údajů (OAuth) uživatele</span><span class="sxs-lookup"><span data-stu-id="c17d1-114">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="c17d1-115">Doporučujeme použít objekt zabezpečení ověřování služby, zejména pro naplánované dat kopie.</span><span class="sxs-lookup"><span data-stu-id="c17d1-115">We recommend that you use service principal authentication, especially for a scheduled data copy.</span></span> <span data-ttu-id="c17d1-116">Vypršení platnosti tokenu chování mohou nastat u ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="c17d1-116">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="c17d1-117">Podrobnosti konfigurace najdete v tématu hello [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="c17d1-117">For configuration details, see hello [Linked service properties](#linked-service-properties) section.</span></span>

## <a name="get-started"></a><span data-ttu-id="c17d1-118">Začínáme</span><span class="sxs-lookup"><span data-stu-id="c17d1-118">Get started</span></span>
<span data-ttu-id="c17d1-119">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Data Lake Store pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c17d1-119">You can create a pipeline with a copy activity that moves data to/from an Azure Data Lake Store by using different tools/APIs.</span></span>

<span data-ttu-id="c17d1-120">Nejjednodušší způsob, jak toocreate Hello toocopy datový kanál je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="c17d1-120">hello easiest way toocreate a pipeline toocopy data is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="c17d1-121">Kurz týkající se vytváření kanálu pomocí Průvodce kopírováním hello, najdete v části [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="c17d1-121">For a tutorial on creating a pipeline by using hello Copy Wizard, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="c17d1-122">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="c17d1-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="c17d1-123">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="c17d1-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="c17d1-124">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="c17d1-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="c17d1-125">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="c17d1-125">Create a **data factory**.</span></span> <span data-ttu-id="c17d1-126">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="c17d1-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="c17d1-127">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="c17d1-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="c17d1-128">Například pokud kopírujete data ze Azure blob storage tooan Azure Data Lake Store, vytvoříte dvě propojené služby toolink vašeho účtu úložiště Azure a Azure Data Lake store tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="c17d1-128">For example, if you are copying data from an Azure blob storage tooan Azure Data Lake Store, you create two linked services toolink your Azure storage account and Azure Data Lake store tooyour data factory.</span></span> <span data-ttu-id="c17d1-129">Vlastnosti propojené služby, které jsou specifické tooAzure Data Lake Store, naleznete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="c17d1-129">For linked service properties that are specific tooAzure Data Lake Store, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="c17d1-130">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="c17d1-130">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="c17d1-131">V příkladu hello uvedených v posledním kroku hello vytvořte kontejner objektů blob hello toospecify datovou sadu a složky, která obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-131">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="c17d1-132">A vytvořte jinou datovou sadu toospecify hello složky a cestu k souboru v úložišti Data Lake hello, který obsahuje hello daty zkopírovanými z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-132">And, you create another dataset toospecify hello folder and file path in hello Data Lake store that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="c17d1-133">Vlastnosti datové sady, které jsou specifické tooAzure Data Lake Store, naleznete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="c17d1-133">For dataset properties that are specific tooAzure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="c17d1-134">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="c17d1-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="c17d1-135">V příkladu hello již bylo zmíněno dříve použijete BlobSource jako zdroj a AzureDataLakeStoreSink jako jímku pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-135">In hello example mentioned earlier, you use BlobSource as a source and AzureDataLakeStoreSink as a sink for hello copy activity.</span></span> <span data-ttu-id="c17d1-136">Podobně pokud zkopírujete z Azure Data Lake Store tooAzure úložiště objektů Blob, použijte AzureDataLakeStoreSource a BlobSink v aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-136">Similarly, if you are copying from Azure Data Lake Store tooAzure Blob Storage, you use AzureDataLakeStoreSource  and BlobSink in hello copy activity.</span></span> <span data-ttu-id="c17d1-137">Vlastnosti aktivity kopírování, které jsou specifické tooAzure Data Lake Store, naleznete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="c17d1-137">For copy activity properties that are specific tooAzure Data Lake Store, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="c17d1-138">Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.</span><span class="sxs-lookup"><span data-stu-id="c17d1-138">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>  

<span data-ttu-id="c17d1-139">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="c17d1-139">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="c17d1-140">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="c17d1-141">Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo ze Azure Data Lake Store naleznete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-data-lake-store) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="c17d1-141">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure Data Lake Store, see [JSON examples](#json-examples-for-copying-data-to-and-from-data-lake-store) section of this article.</span></span>

<span data-ttu-id="c17d1-142">Hello následující části obsahují podrobné informace o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c17d1-142">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooData Lake Store.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="c17d1-143">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="c17d1-143">Linked service properties</span></span>
<span data-ttu-id="c17d1-144">Propojená služba odkazuje data store tooa objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="c17d1-144">A linked service links a data store tooa data factory.</span></span> <span data-ttu-id="c17d1-145">Vytvoření propojené služby typu **AzureDataLakeStore** toolink Data Lake Store data tooyour datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="c17d1-145">You create a linked service of type **AzureDataLakeStore** toolink your Data Lake Store data tooyour data factory.</span></span> <span data-ttu-id="c17d1-146">Hello následující tabulka popisuje JSON elementy konkrétní tooData Lake Store propojené služby.</span><span class="sxs-lookup"><span data-stu-id="c17d1-146">hello following table describes JSON elements specific tooData Lake Store linked services.</span></span> <span data-ttu-id="c17d1-147">Můžete zvolit instanční objekt a ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="c17d1-147">You can choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="c17d1-148">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c17d1-148">Property</span></span> | <span data-ttu-id="c17d1-149">Popis</span><span class="sxs-lookup"><span data-stu-id="c17d1-149">Description</span></span> | <span data-ttu-id="c17d1-150">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c17d1-150">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="c17d1-151">**Typ**</span><span class="sxs-lookup"><span data-stu-id="c17d1-151">**type**</span></span> | <span data-ttu-id="c17d1-152">musí být nastavena vlastnost typu Hello příliš**AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="c17d1-152">hello type property must be set too**AzureDataLakeStore**.</span></span> | <span data-ttu-id="c17d1-153">Ano</span><span class="sxs-lookup"><span data-stu-id="c17d1-153">Yes</span></span> |
| <span data-ttu-id="c17d1-154">**dataLakeStoreUri**</span><span class="sxs-lookup"><span data-stu-id="c17d1-154">**dataLakeStoreUri**</span></span> | <span data-ttu-id="c17d1-155">Informace o hello účtu Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c17d1-155">Information about hello Azure Data Lake Store account.</span></span> <span data-ttu-id="c17d1-156">Tyto informace má jednu z následujících formátů hello: `https://[accountname].azuredatalakestore.net/webhdfs/v1` nebo `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="c17d1-156">This information takes one of hello following formats: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="c17d1-157">Ano</span><span class="sxs-lookup"><span data-stu-id="c17d1-157">Yes</span></span> |
| <span data-ttu-id="c17d1-158">**ID předplatného**</span><span class="sxs-lookup"><span data-stu-id="c17d1-158">**subscriptionId**</span></span> | <span data-ttu-id="c17d1-159">Předplatné Azure ID toowhich hello účtu Data Lake Store patří.</span><span class="sxs-lookup"><span data-stu-id="c17d1-159">Azure subscription ID toowhich hello Data Lake Store account belongs.</span></span> | <span data-ttu-id="c17d1-160">Vyžaduje se pro sink</span><span class="sxs-lookup"><span data-stu-id="c17d1-160">Required for sink</span></span> |
| <span data-ttu-id="c17d1-161">**Název skupiny prostředků**</span><span class="sxs-lookup"><span data-stu-id="c17d1-161">**resourceGroupName**</span></span> | <span data-ttu-id="c17d1-162">Patří prostředků Azure skupiny název toowhich hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c17d1-162">Azure resource group name toowhich hello Data Lake Store account belongs.</span></span> | <span data-ttu-id="c17d1-163">Vyžaduje se pro sink</span><span class="sxs-lookup"><span data-stu-id="c17d1-163">Required for sink</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="c17d1-164">Objekt zabezpečení ověřování služby (doporučeno)</span><span class="sxs-lookup"><span data-stu-id="c17d1-164">Service principal authentication (recommended)</span></span>
<span data-ttu-id="c17d1-165">objekt zabezpečení ověřování služby toouse registrace entity aplikace v Azure Active Directory (Azure AD) a udělte ho hello přístup tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c17d1-165">toouse service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it hello access tooData Lake Store.</span></span> <span data-ttu-id="c17d1-166">Podrobné pokyny najdete v tématu [Service-to-service ověřování](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="c17d1-166">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="c17d1-167">Poznamenejte si hello následující hodnoty, které používáte toodefine hello propojené služby:</span><span class="sxs-lookup"><span data-stu-id="c17d1-167">Make note of hello following values, which you use toodefine hello linked service:</span></span>
* <span data-ttu-id="c17d1-168">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="c17d1-168">Application ID</span></span>
* <span data-ttu-id="c17d1-169">Klíč aplikace</span><span class="sxs-lookup"><span data-stu-id="c17d1-169">Application key</span></span> 
* <span data-ttu-id="c17d1-170">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="c17d1-170">Tenant ID</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c17d1-171">Pokud používáte hello Průvodce kopírováním tooauthor datových kanálů, ujistěte se, že udělíte hello instanční objekt alespoň **čtečky** role v řízení přístupu (správu identit a přístupu) pro účet Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-171">If you are using hello Copy Wizard tooauthor data pipelines, make sure that you grant hello service principal at least a **Reader** role in access control (identity and access management) for hello Data Lake Store account.</span></span> <span data-ttu-id="c17d1-172">Navíc alespoň udělit hello instanční objekt **čtení + Execute** oprávnění tooyour Data Lake Store kořenové ("/") a její podřízené položky.</span><span class="sxs-lookup"><span data-stu-id="c17d1-172">Also, grant hello service principal at least **Read + Execute** permission tooyour Data Lake Store root ("/") and its children.</span></span> <span data-ttu-id="c17d1-173">V opačném případě může zobrazit uvítací zprávu "hello, poskytnuté přihlašovací údaje jsou neplatné."</span><span class="sxs-lookup"><span data-stu-id="c17d1-173">Otherwise you might see hello message "hello credentials provided are invalid."</span></span><br/><br/>
<span data-ttu-id="c17d1-174">Po vytvoření nebo aktualizaci objektu služby ve službě Azure AD, může trvat několik minut, než platnost tootake změny hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-174">After you create or update a service principal in Azure AD, it can take a few minutes for hello changes tootake effect.</span></span> <span data-ttu-id="c17d1-175">Zkontrolujte hello instanční objekt a Data Lake Store přístup ovládacího prvku seznam (ACL) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c17d1-175">Check hello service principal and Data Lake Store access control list (ACL) configurations.</span></span> <span data-ttu-id="c17d1-176">Pokud stále vidět uvítací zprávu "hello, poskytnuté přihlašovací údaje jsou neplatné", Vyčkejte chvíli a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="c17d1-176">If you still see hello message "hello credentials provided are invalid," wait a while and try again.</span></span>

<span data-ttu-id="c17d1-177">Objekt zabezpečení ověřování služby použijte zadáním hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c17d1-177">Use service principal authentication by specifying hello following properties:</span></span>

| <span data-ttu-id="c17d1-178">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c17d1-178">Property</span></span> | <span data-ttu-id="c17d1-179">Popis</span><span class="sxs-lookup"><span data-stu-id="c17d1-179">Description</span></span> | <span data-ttu-id="c17d1-180">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c17d1-180">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="c17d1-181">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="c17d1-181">**servicePrincipalId**</span></span> | <span data-ttu-id="c17d1-182">Zadejte ID aplikace hello klienta.</span><span class="sxs-lookup"><span data-stu-id="c17d1-182">Specify hello application's client ID.</span></span> | <span data-ttu-id="c17d1-183">Ano</span><span class="sxs-lookup"><span data-stu-id="c17d1-183">Yes</span></span> |
| <span data-ttu-id="c17d1-184">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="c17d1-184">**servicePrincipalKey**</span></span> | <span data-ttu-id="c17d1-185">Zadejte klíč aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-185">Specify hello application's key.</span></span> | <span data-ttu-id="c17d1-186">Ano</span><span class="sxs-lookup"><span data-stu-id="c17d1-186">Yes</span></span> |
| <span data-ttu-id="c17d1-187">**klienta**</span><span class="sxs-lookup"><span data-stu-id="c17d1-187">**tenant**</span></span> | <span data-ttu-id="c17d1-188">Zadejte informace klienta hello (název nebo klienta domény ID) v rámci které se nachází aplikace.</span><span class="sxs-lookup"><span data-stu-id="c17d1-188">Specify hello tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="c17d1-189">Můžete jej načíst po výběru ukázáním hello myši v pravém horním rohu hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c17d1-189">You can retrieve it by hovering hello mouse in hello upper-right corner of hello Azure portal.</span></span> | <span data-ttu-id="c17d1-190">Ano</span><span class="sxs-lookup"><span data-stu-id="c17d1-190">Yes</span></span> |

<span data-ttu-id="c17d1-191">**Příkladu: Ověření objektu službu**</span><span class="sxs-lookup"><span data-stu-id="c17d1-191">**Example: Service principal authentication**</span></span>
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

### <a name="user-credential-authentication"></a><span data-ttu-id="c17d1-192">Ověření pověření uživatele</span><span class="sxs-lookup"><span data-stu-id="c17d1-192">User credential authentication</span></span>
<span data-ttu-id="c17d1-193">Alternativně můžete použít toocopy ověřování přihlašovacích údajů uživatele z nebo tooData Lake Store zadáním hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c17d1-193">Alternatively, you can use user credential authentication toocopy from or tooData Lake Store by specifying hello following properties:</span></span>

| <span data-ttu-id="c17d1-194">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c17d1-194">Property</span></span> | <span data-ttu-id="c17d1-195">Popis</span><span class="sxs-lookup"><span data-stu-id="c17d1-195">Description</span></span> | <span data-ttu-id="c17d1-196">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c17d1-196">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="c17d1-197">**autorizace**</span><span class="sxs-lookup"><span data-stu-id="c17d1-197">**authorization**</span></span> | <span data-ttu-id="c17d1-198">Klikněte na tlačítko hello **Autorizovat** tlačítka na hello editoru služby Data Factory a zadejte svoje přihlašovací údaje, který přiřazuje hello automaticky vygenerovanou autorizace URL toothis vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c17d1-198">Click hello **Authorize** button in hello Data Factory Editor and enter your credential that assigns hello autogenerated authorization URL toothis property.</span></span> | <span data-ttu-id="c17d1-199">Ano</span><span class="sxs-lookup"><span data-stu-id="c17d1-199">Yes</span></span> |
| <span data-ttu-id="c17d1-200">**ID relace**</span><span class="sxs-lookup"><span data-stu-id="c17d1-200">**sessionId**</span></span> | <span data-ttu-id="c17d1-201">ID relace OAuth z autorizační relace, hello OAuth.</span><span class="sxs-lookup"><span data-stu-id="c17d1-201">OAuth session ID from hello OAuth authorization session.</span></span> <span data-ttu-id="c17d1-202">Každé ID relace je jedinečné a může být použit pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="c17d1-202">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="c17d1-203">Toto nastavení se automaticky generuje při použití hello editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c17d1-203">This setting is automatically generated when you use hello Data Factory Editor.</span></span> | <span data-ttu-id="c17d1-204">Ano</span><span class="sxs-lookup"><span data-stu-id="c17d1-204">Yes</span></span> |

<span data-ttu-id="c17d1-205">**Příklad: Ověření pověření uživatele**</span><span class="sxs-lookup"><span data-stu-id="c17d1-205">**Example: User credential authentication**</span></span>
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

#### <a name="token-expiration"></a><span data-ttu-id="c17d1-206">Vypršení platnosti tokenu</span><span class="sxs-lookup"><span data-stu-id="c17d1-206">Token expiration</span></span>
<span data-ttu-id="c17d1-207">Hello autorizační kód, který můžete vygenerovat pomocí hello **Autorizovat** tlačítko vyprší po určité době.</span><span class="sxs-lookup"><span data-stu-id="c17d1-207">hello authorization code that you generate by using hello **Authorize** button expires after a certain amount of time.</span></span> <span data-ttu-id="c17d1-208">Hello následující zpráva znamená, že vypršela platnost tohoto hello ověřovací token:</span><span class="sxs-lookup"><span data-stu-id="c17d1-208">hello following message means that hello authentication token has expired:</span></span>

<span data-ttu-id="c17d1-209">Přihlašovací údaje chyby operace: invalid_grant - AADSTS70002: Chyba při ověřování přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="c17d1-209">Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="c17d1-210">AADSTS70008: hello údaje udělení přístupu vypršela platnost nebo odvolat.</span><span class="sxs-lookup"><span data-stu-id="c17d1-210">AADSTS70008: hello provided access grant is expired or revoked.</span></span> <span data-ttu-id="c17d1-211">ID trasování: ID korelace d18629e8-af88-43c5-88e3-d8419eb1fca1: časové razítko fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21-09-31Z.</span><span class="sxs-lookup"><span data-stu-id="c17d1-211">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21-09-31Z.</span></span>

<span data-ttu-id="c17d1-212">Hello následující tabulka uvádí dobu vypršení platnosti hello různé typy uživatelských účtů:</span><span class="sxs-lookup"><span data-stu-id="c17d1-212">hello following table shows hello expiration times of different types of user accounts:</span></span>


| <span data-ttu-id="c17d1-213">Typ uživatele</span><span class="sxs-lookup"><span data-stu-id="c17d1-213">User type</span></span> | <span data-ttu-id="c17d1-214">Platnost vyprší po</span><span class="sxs-lookup"><span data-stu-id="c17d1-214">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="c17d1-215">Uživatelské účty *není* spravované službou Azure Active Directory (například @hotmail.com nebo @live.com)</span><span class="sxs-lookup"><span data-stu-id="c17d1-215">User accounts *not* managed by Azure Active Directory (for example, @hotmail.com or @live.com)</span></span> |<span data-ttu-id="c17d1-216">12 hodin</span><span class="sxs-lookup"><span data-stu-id="c17d1-216">12 hours</span></span> |
| <span data-ttu-id="c17d1-217">Účty uživatelů spravované službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c17d1-217">Users accounts managed by Azure Active Directory</span></span> |<span data-ttu-id="c17d1-218">14 dnů po poslední řez hello spustit</span><span class="sxs-lookup"><span data-stu-id="c17d1-218">14 days after hello last slice run</span></span> <br/><br/><span data-ttu-id="c17d1-219">90 dnů, pokud řezu založeného na základě OAuth propojené služby používá alespoň jednou za 14 dní</span><span class="sxs-lookup"><span data-stu-id="c17d1-219">90 days, if a slice based on an OAuth-based linked service runs at least once every 14 days</span></span> |

<span data-ttu-id="c17d1-220">Pokud změníte heslo před hello dobu vypršení platnosti tokenu, platnost tokenu hello okamžitě vyprší.</span><span class="sxs-lookup"><span data-stu-id="c17d1-220">If you change your password before hello token expiration time, hello token expires immediately.</span></span> <span data-ttu-id="c17d1-221">Zobrazí se zpráva hello dříve popsaných v této části.</span><span class="sxs-lookup"><span data-stu-id="c17d1-221">You will see hello message mentioned earlier in this section.</span></span>

<span data-ttu-id="c17d1-222">Můžete opětovné pověření účtu hello pomocí hello **Autorizovat** tlačítko když hello tokenu vyprší platnost tooredeploy hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="c17d1-222">You can reauthorize hello account by using hello **Authorize** button when hello token expires tooredeploy hello linked service.</span></span> <span data-ttu-id="c17d1-223">Můžete také vygenerovat hodnoty pro hello **sessionId** a **autorizace** vlastnosti programově pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="c17d1-223">You can also generate values for hello **sessionId** and **authorization** properties programmatically by using hello following code:</span></span>


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
<span data-ttu-id="c17d1-224">Podrobnosti o třídy hello Data Factory používané v hello kódu najdete v tématu hello [azuredatalakestorelinkedservice třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), a [ Třída AuthorizationSessionGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témata.</span><span class="sxs-lookup"><span data-stu-id="c17d1-224">For details about hello Data Factory classes used in hello code, see hello [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics.</span></span> <span data-ttu-id="c17d1-225">Přidat odkaz na tooversion `2.9.10826.1824` z `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` pro hello `WindowsFormsWebAuthenticationDialog` třída používaná v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-225">Add a reference tooversion `2.9.10826.1824` of `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` for hello `WindowsFormsWebAuthenticationDialog` class used in hello code.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="c17d1-226">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="c17d1-226">Dataset properties</span></span>
<span data-ttu-id="c17d1-227">toospecify datovou sadu toorepresent vstupních dat v Data Lake Store, nastavíte hello **typ** vlastnosti datové sady hello příliš**AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="c17d1-227">toospecify a dataset toorepresent input data in a Data Lake Store, you set hello **type** property of hello dataset too**AzureDataLakeStore**.</span></span> <span data-ttu-id="c17d1-228">Sada hello **linkedServiceName** vlastnost hello datovou sadu toohello název hello Data Lake Store propojené služby.</span><span class="sxs-lookup"><span data-stu-id="c17d1-228">Set hello **linkedServiceName** property of hello dataset toohello name of hello Data Lake Store linked service.</span></span> <span data-ttu-id="c17d1-229">Úplný seznam části JSON a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c17d1-229">For a full list of JSON sections and properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="c17d1-230">Části datové sady ve formátu JSON, jako například **struktura**, **dostupnosti**, a **zásad**, jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, databáze a tabulky Azure, např.).</span><span class="sxs-lookup"><span data-stu-id="c17d1-230">Sections of a dataset in JSON, such as **structure**, **availability**, and **policy**, are similar for all dataset types (Azure SQL database, Azure blob, and Azure table, for example).</span></span> <span data-ttu-id="c17d1-231">Hello **rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace, jako je například umístění a formát hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-231">hello **typeProperties** section is different for each type of dataset and provides information such as location and format of hello data in hello data store.</span></span> 

<span data-ttu-id="c17d1-232">Hello **rámci typeProperties** části datové sady typu **AzureDataLakeStore** obsahuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c17d1-232">hello **typeProperties** section for a dataset of type **AzureDataLakeStore** contains hello following properties:</span></span>

| <span data-ttu-id="c17d1-233">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c17d1-233">Property</span></span> | <span data-ttu-id="c17d1-234">Popis</span><span class="sxs-lookup"><span data-stu-id="c17d1-234">Description</span></span> | <span data-ttu-id="c17d1-235">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c17d1-235">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="c17d1-236">**folderPath**</span><span class="sxs-lookup"><span data-stu-id="c17d1-236">**folderPath**</span></span> |<span data-ttu-id="c17d1-237">Cesta toohello kontejneru a složce v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c17d1-237">Path toohello container and folder in Data Lake Store.</span></span> |<span data-ttu-id="c17d1-238">Ano</span><span class="sxs-lookup"><span data-stu-id="c17d1-238">Yes</span></span> |
| <span data-ttu-id="c17d1-239">**Název souboru**</span><span class="sxs-lookup"><span data-stu-id="c17d1-239">**fileName**</span></span> |<span data-ttu-id="c17d1-240">Název souboru hello v Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c17d1-240">Name of hello file in Azure Data Lake Store.</span></span> <span data-ttu-id="c17d1-241">Hello **fileName** vlastnost je volitelná a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="c17d1-241">hello **fileName** property is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="c17d1-242">Pokud zadáte **fileName**, na konkrétní soubor hello funguje hello aktivitu (včetně kopie).</span><span class="sxs-lookup"><span data-stu-id="c17d1-242">If you specify **fileName**, hello activity (including Copy) works on hello specific file.</span></span><br/><br/><span data-ttu-id="c17d1-243">Když **fileName** není zadán, zahrnuje kopírování všech souborů v **folderPath** v hello vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="c17d1-243">When **fileName** is not specified, Copy includes all files in **folderPath** in hello input dataset.</span></span><br/><br/><span data-ttu-id="c17d1-244">Když **fileName** pro datovou sadu výstupů není zadána a **preserveHierarchy** není zadané v aktivity podřízený hello název souboru hello generované je ve formátu hello Data. _Identifikátor GUID_.txt'.</span><span class="sxs-lookup"><span data-stu-id="c17d1-244">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, hello name of hello generated file is in hello format Data._Guid_.txt\`.</span></span> <span data-ttu-id="c17d1-245">Například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span><span class="sxs-lookup"><span data-stu-id="c17d1-245">For example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span></span> |<span data-ttu-id="c17d1-246">Ne</span><span class="sxs-lookup"><span data-stu-id="c17d1-246">No</span></span> |
| <span data-ttu-id="c17d1-247">**partitionedBy**</span><span class="sxs-lookup"><span data-stu-id="c17d1-247">**partitionedBy**</span></span> |<span data-ttu-id="c17d1-248">Hello **partitionedBy** vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="c17d1-248">hello **partitionedBy** property is optional.</span></span> <span data-ttu-id="c17d1-249">Můžete ho toospecify dynamické cestu a název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="c17d1-249">You can use it toospecify a dynamic path and file name for time-series data.</span></span> <span data-ttu-id="c17d1-250">Například **folderPath** lze nastavit parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="c17d1-250">For example, **folderPath** can be parameterized for every hour of data.</span></span> <span data-ttu-id="c17d1-251">Podrobnosti a příklady naleznete v tématu [hello vlastnost partitionedBy](#using-partitionedby-property).</span><span class="sxs-lookup"><span data-stu-id="c17d1-251">For details and examples, see [hello partitionedBy property](#using-partitionedby-property).</span></span> |<span data-ttu-id="c17d1-252">Ne</span><span class="sxs-lookup"><span data-stu-id="c17d1-252">No</span></span> |
| <span data-ttu-id="c17d1-253">**Formát**</span><span class="sxs-lookup"><span data-stu-id="c17d1-253">**format**</span></span> | <span data-ttu-id="c17d1-254">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, a  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="c17d1-254">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, and **ParquetFormat**.</span></span> <span data-ttu-id="c17d1-255">Sada hello **typ** vlastnost pod **formátu** tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="c17d1-255">Set hello **type** property under **format** tooone of these values.</span></span> <span data-ttu-id="c17d1-256">Další informace najdete v tématu hello [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu JSON](data-factory-supported-file-and-compression-formats.md#json-format), [formát Avro](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formátu ](data-factory-supported-file-and-compression-formats.md#parquet-format) části hello [formáty souborů a komprese podporovaných službou Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c17d1-256">For more information, see hello [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections in hello [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span> <br><br> <span data-ttu-id="c17d1-257">Pokud chcete soubory toocopy "jako-je" mezi souborové úložiště (binární kopie), přeskočte hello `format` část v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="c17d1-257">If you want toocopy files "as-is" between file-based stores (binary copy), skip hello `format` section in both input and output dataset definitions.</span></span> |<span data-ttu-id="c17d1-258">Ne</span><span class="sxs-lookup"><span data-stu-id="c17d1-258">No</span></span> |
| <span data-ttu-id="c17d1-259">**komprese**</span><span class="sxs-lookup"><span data-stu-id="c17d1-259">**compression**</span></span> | <span data-ttu-id="c17d1-260">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-260">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="c17d1-261">Podporované typy jsou **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="c17d1-261">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="c17d1-262">Jsou podporované úrovně **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="c17d1-262">Supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="c17d1-263">Další informace najdete v tématu [formáty souborů a komprese podporovaných službou Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="c17d1-263">For more information, see [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="c17d1-264">Ne</span><span class="sxs-lookup"><span data-stu-id="c17d1-264">No</span></span> |

### <a name="hello-partitionedby-property"></a><span data-ttu-id="c17d1-265">Vlastnost partitionedBy Hello</span><span class="sxs-lookup"><span data-stu-id="c17d1-265">hello partitionedBy property</span></span>
<span data-ttu-id="c17d1-266">Můžete zadat dynamické **folderPath** a **fileName** vlastnosti pro data časové řady s hello **partitionedBy** vlastnost, funkce pro vytváření dat a systému proměnné.</span><span class="sxs-lookup"><span data-stu-id="c17d1-266">You can specify dynamic **folderPath** and **fileName** properties for time-series data with hello **partitionedBy** property, Data Factory functions, and system variables.</span></span> <span data-ttu-id="c17d1-267">Podrobnosti najdete v tématu hello [Azure Data Factory – funkce a systémové proměnné](data-factory-functions-variables.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c17d1-267">For details, see hello [Azure Data Factory - functions and system variables](data-factory-functions-variables.md) article.</span></span>


<span data-ttu-id="c17d1-268">V následujícím příkladu hello `{Slice}` se nahradí hello hodnoty proměnné systému Data Factory hello `SliceStart` v zadaný formát hello (`yyyyMMddHH`).</span><span class="sxs-lookup"><span data-stu-id="c17d1-268">In hello following example, `{Slice}` is replaced with hello value of hello Data Factory system variable `SliceStart` in hello format specified (`yyyyMMddHH`).</span></span> <span data-ttu-id="c17d1-269">Název Hello `SliceStart` odkazuje čas zahájení toohello hello řez.</span><span class="sxs-lookup"><span data-stu-id="c17d1-269">hello name `SliceStart` refers toohello start time of hello slice.</span></span> <span data-ttu-id="c17d1-270">Hello `folderPath` vlastnost je jiný pro každý řez, jako v `wikidatagateway/wikisampledataout/2014100103` nebo `wikidatagateway/wikisampledataout/2014100104`.</span><span class="sxs-lookup"><span data-stu-id="c17d1-270">hello `folderPath` property is different for each slice, as in `wikidatagateway/wikisampledataout/2014100103` or `wikidatagateway/wikisampledataout/2014100104`.</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="c17d1-271">V následujícím příkladu, hello rok, měsíc, den a čas hello `SliceStart` extrahují do samostatné proměnné, které jsou používány hello `folderPath` a `fileName` vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c17d1-271">In hello following example, hello year, month, day, and time of `SliceStart` are extracted into separate variables that are used by hello `folderPath` and `fileName` properties:</span></span>
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
<span data-ttu-id="c17d1-272">Další informace o datové sady času series, plánování a řezů, najdete v části hello [datové sady v Azure Data Factory](data-factory-create-datasets.md) a [Data Factory plánování a provádění](data-factory-scheduling-and-execution.md) články.</span><span class="sxs-lookup"><span data-stu-id="c17d1-272">For more details on time-series datasets, scheduling, and slices, see hello [Datasets in Azure Data Factory](data-factory-create-datasets.md) and [Data Factory scheduling and execution](data-factory-scheduling-and-execution.md) articles.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="c17d1-273">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="c17d1-273">Copy activity properties</span></span>
<span data-ttu-id="c17d1-274">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c17d1-274">For a full list of sections and properties available for defining activities, see hello [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="c17d1-275">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="c17d1-275">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="c17d1-276">Hello vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="c17d1-276">hello properties available in hello **typeProperties** section of an activity vary with each activity type.</span></span> <span data-ttu-id="c17d1-277">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="c17d1-277">For a copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="c17d1-278">**AzureDataLakeStoreSource** podporuje následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="c17d1-278">**AzureDataLakeStoreSource** supports hello following property in hello **typeProperties** section:</span></span>

| <span data-ttu-id="c17d1-279">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c17d1-279">Property</span></span> | <span data-ttu-id="c17d1-280">Popis</span><span class="sxs-lookup"><span data-stu-id="c17d1-280">Description</span></span> | <span data-ttu-id="c17d1-281">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="c17d1-281">Allowed values</span></span> | <span data-ttu-id="c17d1-282">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c17d1-282">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c17d1-283">**rekurzivní**</span><span class="sxs-lookup"><span data-stu-id="c17d1-283">**recursive**</span></span> |<span data-ttu-id="c17d1-284">Určuje, zda text hello je číst data rekurzivně z podsložky hello nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="c17d1-284">Indicates whether hello data is read recursively from hello subfolders or only from hello specified folder.</span></span> |<span data-ttu-id="c17d1-285">True (výchozí hodnota), False.</span><span class="sxs-lookup"><span data-stu-id="c17d1-285">True (default value), False</span></span> |<span data-ttu-id="c17d1-286">Ne</span><span class="sxs-lookup"><span data-stu-id="c17d1-286">No</span></span> |


<span data-ttu-id="c17d1-287">**AzureDataLakeStoreSink** podporuje následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="c17d1-287">**AzureDataLakeStoreSink** supports hello following properties in hello **typeProperties** section:</span></span>

| <span data-ttu-id="c17d1-288">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c17d1-288">Property</span></span> | <span data-ttu-id="c17d1-289">Popis</span><span class="sxs-lookup"><span data-stu-id="c17d1-289">Description</span></span> | <span data-ttu-id="c17d1-290">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="c17d1-290">Allowed values</span></span> | <span data-ttu-id="c17d1-291">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c17d1-291">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c17d1-292">**copyBehavior**</span><span class="sxs-lookup"><span data-stu-id="c17d1-292">**copyBehavior**</span></span> |<span data-ttu-id="c17d1-293">Určuje chování kopie hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-293">Specifies hello copy behavior.</span></span> |<span data-ttu-id="c17d1-294"><b>PreserveHierarchy</b>: zachovává hello hierarchií souborů v cílové složce hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-294"><b>PreserveHierarchy</b>: Preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="c17d1-295">relativní cesta Hello zdrojové složky toosource souboru je identické toohello relativní cestu složky tootarget cílového souboru.</span><span class="sxs-lookup"><span data-stu-id="c17d1-295">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="c17d1-296"><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky hello jsou vytvořené v první úroveň hello hello cílové složce.</span><span class="sxs-lookup"><span data-stu-id="c17d1-296"><b>FlattenHierarchy</b>: All files from hello source folder are created in hello first level of hello target folder.</span></span> <span data-ttu-id="c17d1-297">Hello zaměřením jsou vytvořeny s názvy generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="c17d1-297">hello target files are created with autogenerated names.</span></span><br/><br/><span data-ttu-id="c17d1-298"><b>MergeFiles</b>: sloučí všechny soubory ze hello zdrojové složky tooone souboru.</span><span class="sxs-lookup"><span data-stu-id="c17d1-298"><b>MergeFiles</b>: Merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="c17d1-299">Pokud hello soubor nebo objekt blob je zadán název, název sloučené souboru hello je zadaný název hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-299">If hello file or blob name is specified, hello merged file name is hello specified name.</span></span> <span data-ttu-id="c17d1-300">Název souboru hello jinak, je generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="c17d1-300">Otherwise, hello file name is autogenerated.</span></span> |<span data-ttu-id="c17d1-301">Ne</span><span class="sxs-lookup"><span data-stu-id="c17d1-301">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="c17d1-302">Příklady rekurzivní a copyBehavior</span><span class="sxs-lookup"><span data-stu-id="c17d1-302">recursive and copyBehavior examples</span></span>
<span data-ttu-id="c17d1-303">Tato část popisuje hello výsledné chování hello kopírování pro různé kombinace hodnot rekurzivní a copyBehavior.</span><span class="sxs-lookup"><span data-stu-id="c17d1-303">This section describes hello resulting behavior of hello Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="c17d1-304">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="c17d1-304">recursive</span></span> | <span data-ttu-id="c17d1-305">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="c17d1-305">copyBehavior</span></span> | <span data-ttu-id="c17d1-306">Výsledné chování</span><span class="sxs-lookup"><span data-stu-id="c17d1-306">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c17d1-307">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="c17d1-307">true</span></span> |<span data-ttu-id="c17d1-308">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="c17d1-308">preserveHierarchy</span></span> |<span data-ttu-id="c17d1-309">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="c17d1-309">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="c17d1-310">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-310">Folder1</span></span><br/><span data-ttu-id="c17d1-311">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-311">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c17d1-312">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-312">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c17d1-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c17d1-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c17d1-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="c17d1-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c17d1-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c17d1-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c17d1-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c17d1-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c17d1-317">Hello cílové složce složku1 je vytvořena s hello stejné struktury jako zdroj hello</span><span class="sxs-lookup"><span data-stu-id="c17d1-317">hello target folder Folder1 is created with hello same structure as hello source</span></span><br/><br/><span data-ttu-id="c17d1-318">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-318">Folder1</span></span><br/><span data-ttu-id="c17d1-319">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-319">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c17d1-320">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-320">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c17d1-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c17d1-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c17d1-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="c17d1-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c17d1-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c17d1-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c17d1-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="c17d1-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="c17d1-325">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="c17d1-325">true</span></span> |<span data-ttu-id="c17d1-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="c17d1-326">flattenHierarchy</span></span> |<span data-ttu-id="c17d1-327">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="c17d1-327">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="c17d1-328">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-328">Folder1</span></span><br/><span data-ttu-id="c17d1-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c17d1-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c17d1-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c17d1-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c17d1-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="c17d1-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c17d1-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c17d1-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c17d1-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c17d1-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c17d1-335">Vytvoření cíle Hello složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="c17d1-335">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="c17d1-336">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-336">Folder1</span></span><br/><span data-ttu-id="c17d1-337">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="c17d1-338">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="c17d1-339">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3</span><span class="sxs-lookup"><span data-stu-id="c17d1-339">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="c17d1-340">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4</span><span class="sxs-lookup"><span data-stu-id="c17d1-340">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="c17d1-341">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5</span><span class="sxs-lookup"><span data-stu-id="c17d1-341">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="c17d1-342">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="c17d1-342">true</span></span> |<span data-ttu-id="c17d1-343">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="c17d1-343">mergeFiles</span></span> |<span data-ttu-id="c17d1-344">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="c17d1-344">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="c17d1-345">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-345">Folder1</span></span><br/><span data-ttu-id="c17d1-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c17d1-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c17d1-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c17d1-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c17d1-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="c17d1-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c17d1-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c17d1-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c17d1-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c17d1-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c17d1-352">Vytvoření cíle Hello složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="c17d1-352">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="c17d1-353">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-353">Folder1</span></span><br/><span data-ttu-id="c17d1-354">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor</span><span class="sxs-lookup"><span data-stu-id="c17d1-354">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="c17d1-355">False</span><span class="sxs-lookup"><span data-stu-id="c17d1-355">false</span></span> |<span data-ttu-id="c17d1-356">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="c17d1-356">preserveHierarchy</span></span> |<span data-ttu-id="c17d1-357">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="c17d1-357">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="c17d1-358">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-358">Folder1</span></span><br/><span data-ttu-id="c17d1-359">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-359">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c17d1-360">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-360">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c17d1-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c17d1-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c17d1-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="c17d1-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c17d1-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c17d1-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c17d1-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c17d1-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c17d1-365">Hello cílové složce složku1 je vytvořena s hello strukturu</span><span class="sxs-lookup"><span data-stu-id="c17d1-365">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="c17d1-366">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-366">Folder1</span></span><br/><span data-ttu-id="c17d1-367">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-367">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c17d1-368">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-368">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="c17d1-369">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="c17d1-369">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="c17d1-370">False</span><span class="sxs-lookup"><span data-stu-id="c17d1-370">false</span></span> |<span data-ttu-id="c17d1-371">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="c17d1-371">flattenHierarchy</span></span> |<span data-ttu-id="c17d1-372">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="c17d1-372">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="c17d1-373">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-373">Folder1</span></span><br/><span data-ttu-id="c17d1-374">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-374">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c17d1-375">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-375">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c17d1-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c17d1-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c17d1-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="c17d1-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c17d1-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c17d1-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c17d1-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c17d1-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c17d1-380">Hello cílové složce složku1 je vytvořena s hello strukturu</span><span class="sxs-lookup"><span data-stu-id="c17d1-380">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="c17d1-381">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-381">Folder1</span></span><br/><span data-ttu-id="c17d1-382">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-382">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="c17d1-383">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-383">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="c17d1-384">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="c17d1-384">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="c17d1-385">False</span><span class="sxs-lookup"><span data-stu-id="c17d1-385">false</span></span> |<span data-ttu-id="c17d1-386">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="c17d1-386">mergeFiles</span></span> |<span data-ttu-id="c17d1-387">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="c17d1-387">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="c17d1-388">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-388">Folder1</span></span><br/><span data-ttu-id="c17d1-389">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-389">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c17d1-390">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c17d1-390">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c17d1-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c17d1-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c17d1-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="c17d1-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c17d1-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c17d1-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c17d1-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c17d1-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c17d1-395">Hello cílové složce složku1 je vytvořena s hello strukturu</span><span class="sxs-lookup"><span data-stu-id="c17d1-395">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="c17d1-396">Složku1</span><span class="sxs-lookup"><span data-stu-id="c17d1-396">Folder1</span></span><br/><span data-ttu-id="c17d1-397">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="c17d1-397">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="c17d1-398">automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="c17d1-398">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="c17d1-399">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="c17d1-399">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="c17d1-400">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="c17d1-400">Supported file and compression formats</span></span>
<span data-ttu-id="c17d1-401">Podrobnosti najdete v tématu hello [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c17d1-401">For details, see hello [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span>

## <a name="json-examples-for-copying-data-tooand-from-data-lake-store"></a><span data-ttu-id="c17d1-402">Příklady JSON kopírování tooand dat z Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c17d1-402">JSON examples for copying data tooand from Data Lake Store</span></span>
<span data-ttu-id="c17d1-403">Následující příklady Hello poskytují definice JSON ukázka.</span><span class="sxs-lookup"><span data-stu-id="c17d1-403">hello following examples provide sample JSON definitions.</span></span> <span data-ttu-id="c17d1-404">Můžete použít tyto ukázkové definice toocreate kanálu pomocí hello [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c17d1-404">You can use these sample definitions toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="c17d1-405">Zobrazit příklady jak Hello toocopy tooand dat z Data Lake Store a Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="c17d1-405">hello examples show how toocopy data tooand from Data Lake Store and Azure Blob storage.</span></span> <span data-ttu-id="c17d1-406">Nicméně je možné zkopírovat data _přímo_ z jakéhokoli z tooany zdroje hello z jímky hello podporována.</span><span class="sxs-lookup"><span data-stu-id="c17d1-406">However, data can be copied _directly_ from any of hello sources tooany of hello supported sinks.</span></span> <span data-ttu-id="c17d1-407">Další informace najdete v tématu hello části "podporované úložiště dat a formáty" v hello [přesun dat pomocí aktivity kopírování](data-factory-data-movement-activities.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c17d1-407">For more information, see hello section "Supported data stores and formats" in hello [Move data by using Copy Activity](data-factory-data-movement-activities.md) article.</span></span>  

### <a name="example-copy-data-from-azure-blob-storage-tooazure-data-lake-store"></a><span data-ttu-id="c17d1-408">Příklad: Kopírování dat z Azure Blob Storage tooAzure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c17d1-408">Example: Copy data from Azure Blob Storage tooAzure Data Lake Store</span></span>
<span data-ttu-id="c17d1-409">Hello ukázkový kód v této části uvádí:</span><span class="sxs-lookup"><span data-stu-id="c17d1-409">hello example code in this section shows:</span></span>

* <span data-ttu-id="c17d1-410">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-410">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="c17d1-411">Propojené služby typu [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-411">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="c17d1-412">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-412">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="c17d1-413">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-413">An output [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="c17d1-414">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [AzureDataLakeStoreSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-414">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [AzureDataLakeStoreSink](#copy-activity-properties).</span></span>

<span data-ttu-id="c17d1-415">Hello příklady ukazují, jak časové řady dat z Azure Blob Storage je zkopírovat tooData Lake Store každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="c17d1-415">hello examples show how time-series data from Azure Blob Storage is copied tooData Lake Store every hour.</span></span> 

<span data-ttu-id="c17d1-416">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="c17d1-416">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="c17d1-417">**Propojená služba Azure Data Lake Store**</span><span class="sxs-lookup"><span data-stu-id="c17d1-417">**Azure Data Lake Store linked service**</span></span>

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
> <span data-ttu-id="c17d1-418">Podrobnosti konfigurace najdete v tématu hello [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="c17d1-418">For configuration details, see hello [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="c17d1-419">**Vstupní datová sada Azure Blob**</span><span class="sxs-lookup"><span data-stu-id="c17d1-419">**Azure blob input dataset**</span></span>

<span data-ttu-id="c17d1-420">V následujícím příkladu hello, data je převzata z nového objektu blob každou hodinu (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="c17d1-420">In hello following example, data is picked up from a new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="c17d1-421">název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="c17d1-421">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="c17d1-422">Cesta ke složce Hello používá hello rok, měsíc a den část hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="c17d1-422">hello folder path uses hello year, month, and day portion of hello start time.</span></span> <span data-ttu-id="c17d1-423">Název souboru Hello používá hodinu hello část hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="c17d1-423">hello file name uses hello hour portion of hello start time.</span></span> <span data-ttu-id="c17d1-424">Hello `"external": true` nastavení informuje služba Data Factory hello Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-424">hello `"external": true` setting informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="c17d1-425">**Azure Data Lake Store výstupní datovou sadu**</span><span class="sxs-lookup"><span data-stu-id="c17d1-425">**Azure Data Lake Store output dataset**</span></span>

<span data-ttu-id="c17d1-426">Následující příklad kopie dat tooData Lake Store Hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-426">hello following example copies data tooData Lake Store.</span></span> <span data-ttu-id="c17d1-427">Zkopíruje nová data Lake Store tooData každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="c17d1-427">New data is copied tooData Lake Store every hour.</span></span>

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


<span data-ttu-id="c17d1-428">**Aktivita kopírování v kanálu s blob zdroj a jímka Data Lake Store**</span><span class="sxs-lookup"><span data-stu-id="c17d1-428">**Copy activity in a pipeline with a blob source and a Data Lake Store sink**</span></span>

<span data-ttu-id="c17d1-429">V následujícím příkladu hello, hello kanál obsahuje aktivitu kopírování, který je nakonfigurovaný toouse hello vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="c17d1-429">In hello following example, hello pipeline contains a copy activity that is configured toouse hello input and output datasets.</span></span> <span data-ttu-id="c17d1-430">Aktivita kopírování Hello je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="c17d1-430">hello copy activity is scheduled toorun every hour.</span></span> <span data-ttu-id="c17d1-431">V kanálu hello definici JSON, hello `source` je typ nastaven příliš`BlobSource`a hello `sink` je typ nastaven příliš`AzureDataLakeStoreSink`.</span><span class="sxs-lookup"><span data-stu-id="c17d1-431">In hello pipeline JSON definition, hello `source` type is set too`BlobSource`, and hello `sink` type is set too`AzureDataLakeStoreSink`.</span></span>

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

### <a name="example-copy-data-from-azure-data-lake-store-tooan-azure-blob"></a><span data-ttu-id="c17d1-432">Příklad: Kopírování dat z Azure Data Lake Store tooan objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="c17d1-432">Example: Copy data from Azure Data Lake Store tooan Azure blob</span></span>
<span data-ttu-id="c17d1-433">Hello ukázkový kód v této části uvádí:</span><span class="sxs-lookup"><span data-stu-id="c17d1-433">hello example code in this section shows:</span></span>

* <span data-ttu-id="c17d1-434">Propojené služby typu [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-434">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="c17d1-435">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-435">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="c17d1-436">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-436">An input [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="c17d1-437">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-437">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="c17d1-438">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [AzureDataLakeStoreSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="c17d1-438">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [AzureDataLakeStoreSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="c17d1-439">Kód Hello kopíruje data časové řady z Data Lake Store tooan objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="c17d1-439">hello code copies time-series data from Data Lake Store tooan Azure blob every hour.</span></span> 

<span data-ttu-id="c17d1-440">**Propojená služba Azure Data Lake Store**</span><span class="sxs-lookup"><span data-stu-id="c17d1-440">**Azure Data Lake Store linked service**</span></span>

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
> <span data-ttu-id="c17d1-441">Podrobnosti konfigurace najdete v tématu hello [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="c17d1-441">For configuration details, see hello [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="c17d1-442">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="c17d1-442">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="c17d1-443">**Vstupní datové sady Azure Data Lake**</span><span class="sxs-lookup"><span data-stu-id="c17d1-443">**Azure Data Lake input dataset**</span></span>

<span data-ttu-id="c17d1-444">V tomto příkladu nastavení `"external"` příliš`true` informuje služba Data Factory hello Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-444">In this example, setting `"external"` too`true` informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="c17d1-445">**Výstupní datová sada Azure Blob**</span><span class="sxs-lookup"><span data-stu-id="c17d1-445">**Azure blob output dataset**</span></span>

<span data-ttu-id="c17d1-446">V následujícím příkladu hello, data se zapisují nový objekt blob tooa každou hodinu (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="c17d1-446">In hello following example, data is written tooa new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="c17d1-447">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="c17d1-447">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="c17d1-448">Cesta ke složce Hello používá hello rok, měsíc, den a čas část hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="c17d1-448">hello folder path uses hello year, month, day, and hours portion of hello start time.</span></span>

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

<span data-ttu-id="c17d1-449">**Aktivita kopírování v kanálu s na Azure Data Lake Store zdroj a jímka objektů blob**</span><span class="sxs-lookup"><span data-stu-id="c17d1-449">**A copy activity in a pipeline with an Azure Data Lake Store source and a blob sink**</span></span>

<span data-ttu-id="c17d1-450">V následujícím příkladu hello, hello kanál obsahuje aktivitu kopírování, který je nakonfigurovaný toouse hello vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="c17d1-450">In hello following example, hello pipeline contains a copy activity that is configured toouse hello input and output datasets.</span></span> <span data-ttu-id="c17d1-451">Aktivita kopírování Hello je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="c17d1-451">hello copy activity is scheduled toorun every hour.</span></span> <span data-ttu-id="c17d1-452">V kanálu hello definici JSON, hello `source` je typ nastaven příliš`AzureDataLakeStoreSource`a hello `sink` je typ nastaven příliš`BlobSink`.</span><span class="sxs-lookup"><span data-stu-id="c17d1-452">In hello pipeline JSON definition, hello `source` type is set too`AzureDataLakeStoreSource`, and hello `sink` type is set too`BlobSink`.</span></span>

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

<span data-ttu-id="c17d1-453">V definici aktivity kopírování hello můžete namapovat sloupce z toocolumns datové sady zdroje hello v datové sadě podřízený hello.</span><span class="sxs-lookup"><span data-stu-id="c17d1-453">In hello copy activity definition, you can also map columns from hello source dataset toocolumns in hello sink dataset.</span></span> <span data-ttu-id="c17d1-454">Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="c17d1-454">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="c17d1-455">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="c17d1-455">Performance and tuning</span></span>
<span data-ttu-id="c17d1-456">toolearn hello faktory, které ovlivňují výkon aktivity kopírování a jak toooptimize, najdete v části hello [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c17d1-456">toolearn about hello factors that affect Copy Activity performance and how toooptimize it, see hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) article.</span></span>
