---
title: "aaaTransform data pomocí skriptu U-SQL - Azure | Microsoft Docs"
description: "Zjistěte, jak tooprocess nebo transformace dat pomocí spouštění skriptů U-SQL v Azure Data Lake Analytics výpočetní služby."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: 51fdb40334d0c131720f65c3a96b4c5045a98b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a><span data-ttu-id="7fe38-103">Transformace dat pomocí spouštění skriptů U-SQL v Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="7fe38-103">Transform data by running U-SQL scripts on Azure Data Lake Analytics</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="7fe38-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="7fe38-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="7fe38-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="7fe38-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="7fe38-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="7fe38-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="7fe38-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="7fe38-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="7fe38-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="7fe38-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="7fe38-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="7fe38-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="7fe38-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="7fe38-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="7fe38-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="7fe38-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="7fe38-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="7fe38-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="7fe38-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7fe38-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="7fe38-114">Kanál v objektu pro vytváření dat Azure zpracovává data v služby propojené úložiště pomocí propojené výpočetní služby.</span><span class="sxs-lookup"><span data-stu-id="7fe38-114">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="7fe38-115">Obsahuje posloupnost aktivit, kde každá aktivita provede konkrétní zpracování operace.</span><span class="sxs-lookup"><span data-stu-id="7fe38-115">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="7fe38-116">Tento článek popisuje hello **Data Lake Analytics U-SQL aktivity** , která se spouští **U-SQL** skript na **Azure Data Lake Analytics** výpočetní propojené služby.</span><span class="sxs-lookup"><span data-stu-id="7fe38-116">This article describes hello **Data Lake Analytics U-SQL Activity** that runs a **U-SQL** script on an **Azure Data Lake Analytics** compute linked service.</span></span> 

> [!NOTE]
> <span data-ttu-id="7fe38-117">Vytvoření účtu Azure Data Lake Analytics před vytvořením kanálu s aktivitou Data Lake Analytics U-SQL.</span><span class="sxs-lookup"><span data-stu-id="7fe38-117">Create an Azure Data Lake Analytics account before creating a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="7fe38-118">toolearn o Azure Data Lake Analytics najdete v části [Začínáme s Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7fe38-118">toolearn about Azure Data Lake Analytics, see [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span>
> 
> <span data-ttu-id="7fe38-119">Zkontrolujte hello [vytvoření vaší první kurz kanálu](data-factory-build-your-first-pipeline.md) pro podrobné kroky toocreate objekt pro vytváření dat, propojené služby, datových sad a kanálu.</span><span class="sxs-lookup"><span data-stu-id="7fe38-119">Review hello [Build your first pipeline tutorial](data-factory-build-your-first-pipeline.md) for detailed steps toocreate a data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="7fe38-120">Fragmenty kódu JSON pomocí editoru služby Data Factory nebo entit služby Data Factory toocreate Visual Studio nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7fe38-120">Use JSON snippets with Data Factory Editor or Visual Studio or Azure PowerShell toocreate Data Factory entities.</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="7fe38-121">Typy podporované ověřování</span><span class="sxs-lookup"><span data-stu-id="7fe38-121">Supported authentication types</span></span>
<span data-ttu-id="7fe38-122">Aktivita U-SQL podporuje následující typy ověřování proti Data Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="7fe38-122">U-SQL activity supports below authentication types against Data Lake Analytics:</span></span>
* <span data-ttu-id="7fe38-123">Ověřování instančních objektů</span><span class="sxs-lookup"><span data-stu-id="7fe38-123">Service principal authentication</span></span>
* <span data-ttu-id="7fe38-124">Ověření přihlašovacích údajů (OAuth) uživatele</span><span class="sxs-lookup"><span data-stu-id="7fe38-124">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="7fe38-125">Doporučujeme použít objekt zabezpečení ověřování služby, zejména pro naplánovaného spuštění U-SQL.</span><span class="sxs-lookup"><span data-stu-id="7fe38-125">We recommend that you use service principal authentication, especially for a scheduled U-SQL execution.</span></span> <span data-ttu-id="7fe38-126">Vypršení platnosti tokenu chování mohou nastat u ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="7fe38-126">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="7fe38-127">Podrobnosti konfigurace najdete v tématu hello [propojené vlastnosti služby](#azure-data-lake-analytics-linked-service) části.</span><span class="sxs-lookup"><span data-stu-id="7fe38-127">For configuration details, see hello [Linked service properties](#azure-data-lake-analytics-linked-service) section.</span></span>

## <a name="azure-data-lake-analytics-linked-service"></a><span data-ttu-id="7fe38-128">Propojená služba Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="7fe38-128">Azure Data Lake Analytics Linked Service</span></span>
<span data-ttu-id="7fe38-129">Vytvoříte **Azure Data Lake Analytics** propojené služby toolink služby Azure Data Lake Analytics výpočetní služby tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="7fe38-129">You create an **Azure Data Lake Analytics** linked service toolink an Azure Data Lake Analytics compute service tooan Azure data factory.</span></span> <span data-ttu-id="7fe38-130">Hello Data Lake Analytics U-SQL aktivitu v kanálu hello odkazuje toothis propojené služby.</span><span class="sxs-lookup"><span data-stu-id="7fe38-130">hello Data Lake Analytics U-SQL activity in hello pipeline refers toothis linked service.</span></span> 

<span data-ttu-id="7fe38-131">Hello následující tabulka obsahuje popis hello obecné vlastnosti používané ve hello definici JSON.</span><span class="sxs-lookup"><span data-stu-id="7fe38-131">hello following table provides descriptions for hello generic properties used in hello JSON definition.</span></span> <span data-ttu-id="7fe38-132">Dále můžete mezi instanční objekt a ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="7fe38-132">You can further choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="7fe38-133">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7fe38-133">Property</span></span> | <span data-ttu-id="7fe38-134">Popis</span><span class="sxs-lookup"><span data-stu-id="7fe38-134">Description</span></span> | <span data-ttu-id="7fe38-135">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7fe38-135">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7fe38-136">**Typ**</span><span class="sxs-lookup"><span data-stu-id="7fe38-136">**type**</span></span> |<span data-ttu-id="7fe38-137">vlastnost typu Hello by měla být nastavena na: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="7fe38-137">hello type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="7fe38-138">Ano</span><span class="sxs-lookup"><span data-stu-id="7fe38-138">Yes</span></span> |
| <span data-ttu-id="7fe38-139">**název účtu**</span><span class="sxs-lookup"><span data-stu-id="7fe38-139">**accountName**</span></span> |<span data-ttu-id="7fe38-140">Název účtu Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="7fe38-140">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="7fe38-141">Ano</span><span class="sxs-lookup"><span data-stu-id="7fe38-141">Yes</span></span> |
| <span data-ttu-id="7fe38-142">**dataLakeAnalyticsUri**</span><span class="sxs-lookup"><span data-stu-id="7fe38-142">**dataLakeAnalyticsUri**</span></span> |<span data-ttu-id="7fe38-143">Identifikátor URI služby Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="7fe38-143">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="7fe38-144">Ne</span><span class="sxs-lookup"><span data-stu-id="7fe38-144">No</span></span> |
| <span data-ttu-id="7fe38-145">**ID předplatného**</span><span class="sxs-lookup"><span data-stu-id="7fe38-145">**subscriptionId**</span></span> |<span data-ttu-id="7fe38-146">Id předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="7fe38-146">Azure subscription id</span></span> |<span data-ttu-id="7fe38-147">Ne (když není určeno předplatné hello se používá pro vytváření dat).</span><span class="sxs-lookup"><span data-stu-id="7fe38-147">No (If not specified, subscription of hello data factory is used).</span></span> |
| <span data-ttu-id="7fe38-148">**Název skupiny prostředků**</span><span class="sxs-lookup"><span data-stu-id="7fe38-148">**resourceGroupName**</span></span> |<span data-ttu-id="7fe38-149">Název skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7fe38-149">Azure resource group name</span></span> |<span data-ttu-id="7fe38-150">Ne (když není určeno skupiny prostředků hello se používá pro vytváření dat).</span><span class="sxs-lookup"><span data-stu-id="7fe38-150">No (If not specified, resource group of hello data factory is used).</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="7fe38-151">Objekt zabezpečení ověřování služby (doporučeno)</span><span class="sxs-lookup"><span data-stu-id="7fe38-151">Service principal authentication (recommended)</span></span>
<span data-ttu-id="7fe38-152">objekt zabezpečení ověřování služby toouse registrace entity aplikace v Azure Active Directory (Azure AD) a udělte ho hello přístup tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="7fe38-152">toouse service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it hello access tooData Lake Store.</span></span> <span data-ttu-id="7fe38-153">Podrobné pokyny najdete v tématu [Service-to-service ověřování](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="7fe38-153">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="7fe38-154">Poznamenejte si hello následující hodnoty, které používáte toodefine hello propojené služby:</span><span class="sxs-lookup"><span data-stu-id="7fe38-154">Make note of hello following values, which you use toodefine hello linked service:</span></span>
* <span data-ttu-id="7fe38-155">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="7fe38-155">Application ID</span></span>
* <span data-ttu-id="7fe38-156">Klíč aplikace</span><span class="sxs-lookup"><span data-stu-id="7fe38-156">Application key</span></span> 
* <span data-ttu-id="7fe38-157">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="7fe38-157">Tenant ID</span></span>

<span data-ttu-id="7fe38-158">Objekt zabezpečení ověřování služby použijte zadáním hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="7fe38-158">Use service principal authentication by specifying hello following properties:</span></span>

| <span data-ttu-id="7fe38-159">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7fe38-159">Property</span></span> | <span data-ttu-id="7fe38-160">Popis</span><span class="sxs-lookup"><span data-stu-id="7fe38-160">Description</span></span> | <span data-ttu-id="7fe38-161">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7fe38-161">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7fe38-162">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="7fe38-162">**servicePrincipalId**</span></span> | <span data-ttu-id="7fe38-163">Zadejte ID aplikace hello klienta.</span><span class="sxs-lookup"><span data-stu-id="7fe38-163">Specify hello application's client ID.</span></span> | <span data-ttu-id="7fe38-164">Ano</span><span class="sxs-lookup"><span data-stu-id="7fe38-164">Yes</span></span> |
| <span data-ttu-id="7fe38-165">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="7fe38-165">**servicePrincipalKey**</span></span> | <span data-ttu-id="7fe38-166">Zadejte klíč aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-166">Specify hello application's key.</span></span> | <span data-ttu-id="7fe38-167">Ano</span><span class="sxs-lookup"><span data-stu-id="7fe38-167">Yes</span></span> |
| <span data-ttu-id="7fe38-168">**klienta**</span><span class="sxs-lookup"><span data-stu-id="7fe38-168">**tenant**</span></span> | <span data-ttu-id="7fe38-169">Zadejte informace klienta hello (název nebo klienta domény ID) v rámci které se nachází aplikace.</span><span class="sxs-lookup"><span data-stu-id="7fe38-169">Specify hello tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="7fe38-170">Můžete jej načíst po výběru ukázáním hello myši v pravém horním rohu hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7fe38-170">You can retrieve it by hovering hello mouse in hello upper-right corner of hello Azure portal.</span></span> | <span data-ttu-id="7fe38-171">Ano</span><span class="sxs-lookup"><span data-stu-id="7fe38-171">Yes</span></span> |

<span data-ttu-id="7fe38-172">**Příkladu: Ověření objektu službu**</span><span class="sxs-lookup"><span data-stu-id="7fe38-172">**Example: Service principal authentication**</span></span>
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a><span data-ttu-id="7fe38-173">Ověření pověření uživatele</span><span class="sxs-lookup"><span data-stu-id="7fe38-173">User credential authentication</span></span>
<span data-ttu-id="7fe38-174">Alternativně můžete použít ověřování pověření uživatele pro Data Lake Analytics zadáním hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="7fe38-174">Alternatively, you can use user credential authentication for Data Lake Analytics by specifying hello following properties:</span></span>

| <span data-ttu-id="7fe38-175">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7fe38-175">Property</span></span> | <span data-ttu-id="7fe38-176">Popis</span><span class="sxs-lookup"><span data-stu-id="7fe38-176">Description</span></span> | <span data-ttu-id="7fe38-177">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7fe38-177">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7fe38-178">**autorizace**</span><span class="sxs-lookup"><span data-stu-id="7fe38-178">**authorization**</span></span> | <span data-ttu-id="7fe38-179">Klikněte na tlačítko hello **Autorizovat** tlačítka na hello editoru služby Data Factory a zadejte svoje přihlašovací údaje, který přiřazuje hello automaticky vygenerovanou autorizace URL toothis vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7fe38-179">Click hello **Authorize** button in hello Data Factory Editor and enter your credential that assigns hello autogenerated authorization URL toothis property.</span></span> | <span data-ttu-id="7fe38-180">Ano</span><span class="sxs-lookup"><span data-stu-id="7fe38-180">Yes</span></span> |
| <span data-ttu-id="7fe38-181">**ID relace**</span><span class="sxs-lookup"><span data-stu-id="7fe38-181">**sessionId**</span></span> | <span data-ttu-id="7fe38-182">ID relace OAuth z autorizační relace, hello OAuth.</span><span class="sxs-lookup"><span data-stu-id="7fe38-182">OAuth session ID from hello OAuth authorization session.</span></span> <span data-ttu-id="7fe38-183">Každé ID relace je jedinečné a může být použit pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="7fe38-183">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="7fe38-184">Toto nastavení se automaticky generuje při použití hello editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7fe38-184">This setting is automatically generated when you use hello Data Factory Editor.</span></span> | <span data-ttu-id="7fe38-185">Ano</span><span class="sxs-lookup"><span data-stu-id="7fe38-185">Yes</span></span> |

<span data-ttu-id="7fe38-186">**Příklad: Ověření pověření uživatele**</span><span class="sxs-lookup"><span data-stu-id="7fe38-186">**Example: User credential authentication**</span></span>
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a><span data-ttu-id="7fe38-187">Vypršení platnosti tokenu</span><span class="sxs-lookup"><span data-stu-id="7fe38-187">Token expiration</span></span>
<span data-ttu-id="7fe38-188">Hello autorizační kód vygenerovaného s využitím hello **Autorizovat** tlačítko vyprší po určité době.</span><span class="sxs-lookup"><span data-stu-id="7fe38-188">hello authorization code you generated by using hello **Authorize** button expires after sometime.</span></span> <span data-ttu-id="7fe38-189">Viz následující tabulka pro hello vypršení platnosti časy pro různé typy uživatelských účtů hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-189">See hello following table for hello expiration times for different types of user accounts.</span></span> <span data-ttu-id="7fe38-190">Zobrazí následující chybová zpráva hello při hello ověřování **platnost tokenu vyprší**: přihlašovací údaje chyby operace: invalid_grant - AADSTS70002: Chyba při ověřování přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="7fe38-190">You may see hello following error message when hello authentication **token expires**: Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="7fe38-191">AADSTS70008: hello údaje udělení přístupu vypršela platnost nebo odvolat.</span><span class="sxs-lookup"><span data-stu-id="7fe38-191">AADSTS70008: hello provided access grant is expired or revoked.</span></span> <span data-ttu-id="7fe38-192">ID trasování: ID korelace d18629e8-af88-43c5-88e3-d8419eb1fca1: časové razítko fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21:09:31Z</span><span class="sxs-lookup"><span data-stu-id="7fe38-192">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21:09:31Z</span></span>

| <span data-ttu-id="7fe38-193">Typ uživatele</span><span class="sxs-lookup"><span data-stu-id="7fe38-193">User type</span></span> | <span data-ttu-id="7fe38-194">Platnost vyprší po</span><span class="sxs-lookup"><span data-stu-id="7fe38-194">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="7fe38-195">Uživatelské účty, které nejsou spravované přes Azure Active Directory (@hotmail.com, @live.comatd.)</span><span class="sxs-lookup"><span data-stu-id="7fe38-195">User accounts NOT managed by Azure Active Directory (@hotmail.com, @live.com, etc.)</span></span> |<span data-ttu-id="7fe38-196">12 hodin</span><span class="sxs-lookup"><span data-stu-id="7fe38-196">12 hours</span></span> |
| <span data-ttu-id="7fe38-197">Účty uživatelů spravované pomocí Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="7fe38-197">Users accounts managed by Azure Active Directory (AAD)</span></span> |<span data-ttu-id="7fe38-198">14 dnů po poslední řez hello spustit.</span><span class="sxs-lookup"><span data-stu-id="7fe38-198">14 days after hello last slice run.</span></span> <br/><br/><span data-ttu-id="7fe38-199">90 dnů, pokud řezu založeného na základě OAuth propojené služby používá alespoň jednou za 14 dní.</span><span class="sxs-lookup"><span data-stu-id="7fe38-199">90 days, if a slice based on OAuth-based linked service runs at least once every 14 days.</span></span> |

<span data-ttu-id="7fe38-200">tooavoid nebo vyřešit tuto chybu, opětovné pověření pomocí hello **Authorize** když hello **platnost tokenu vyprší** a znovu nasaďte hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="7fe38-200">tooavoid/resolve this error, reauthorize using hello **Authorize** button when hello **token expires** and redeploy hello linked service.</span></span> <span data-ttu-id="7fe38-201">Můžete také vygenerovat hodnoty pro **sessionId** a **autorizace** vlastnosti programově pomocí kódu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7fe38-201">You can also generate values for **sessionId** and **authorization** properties programmatically using code as follows:</span></span>

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

<span data-ttu-id="7fe38-202">V tématu [azuredatalakestorelinkedservice třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), a [AuthorizationSessionGetResponse třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témata podrobnosti o třídách Data Factory hello používá v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-202">See [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics for details about hello Data Factory classes used in hello code.</span></span> <span data-ttu-id="7fe38-203">Přidat odkaz na: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll pro hello WindowsFormsWebAuthenticationDialog třídy.</span><span class="sxs-lookup"><span data-stu-id="7fe38-203">Add a reference to: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll for hello WindowsFormsWebAuthenticationDialog class.</span></span> 

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="7fe38-204">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="7fe38-204">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="7fe38-205">Definuje Hello následujícím fragmentu kódu JSON kanálu s aktivitou Data Lake Analytics U-SQL.</span><span class="sxs-lookup"><span data-stu-id="7fe38-205">hello following JSON snippet defines a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="7fe38-206">definici aktivity Hello má toohello referenční dokumentace Azure Data Lake Analytics propojené služby, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="7fe38-206">hello activity definition has a reference toohello Azure Data Lake Analytics linked service you created earlier.</span></span>   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline toocompute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="7fe38-207">Hello následující tabulka popisuje názvy a popisy vlastností, které jsou specifické toothis aktivity.</span><span class="sxs-lookup"><span data-stu-id="7fe38-207">hello following table describes names and descriptions of properties that are specific toothis activity.</span></span> 

| <span data-ttu-id="7fe38-208">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7fe38-208">Property</span></span> | <span data-ttu-id="7fe38-209">Popis</span><span class="sxs-lookup"><span data-stu-id="7fe38-209">Description</span></span> | <span data-ttu-id="7fe38-210">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7fe38-210">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7fe38-211">type</span><span class="sxs-lookup"><span data-stu-id="7fe38-211">type</span></span> |<span data-ttu-id="7fe38-212">musí být nastavena vlastnost typu Hello příliš**DataLakeAnalyticsU SQL**.</span><span class="sxs-lookup"><span data-stu-id="7fe38-212">hello type property must be set too**DataLakeAnalyticsU-SQL**.</span></span> |<span data-ttu-id="7fe38-213">Ano</span><span class="sxs-lookup"><span data-stu-id="7fe38-213">Yes</span></span> |
| <span data-ttu-id="7fe38-214">scriptPath</span><span class="sxs-lookup"><span data-stu-id="7fe38-214">scriptPath</span></span> |<span data-ttu-id="7fe38-215">Cesta toofolder, který obsahuje skript hello U-SQL.</span><span class="sxs-lookup"><span data-stu-id="7fe38-215">Path toofolder that contains hello U-SQL script.</span></span> <span data-ttu-id="7fe38-216">Název souboru hello rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="7fe38-216">Name of hello file is case-sensitive.</span></span> |<span data-ttu-id="7fe38-217">Ne (když používáte skript)</span><span class="sxs-lookup"><span data-stu-id="7fe38-217">No (if you use script)</span></span> |
| <span data-ttu-id="7fe38-218">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="7fe38-218">scriptLinkedService</span></span> |<span data-ttu-id="7fe38-219">Propojené služby, který odkazuje hello úložiště, který obsahuje toohello hello skriptu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="7fe38-219">Linked service that links hello storage that contains hello script toohello data factory</span></span> |<span data-ttu-id="7fe38-220">Ne (když používáte skript)</span><span class="sxs-lookup"><span data-stu-id="7fe38-220">No (if you use script)</span></span> |
| <span data-ttu-id="7fe38-221">Skript</span><span class="sxs-lookup"><span data-stu-id="7fe38-221">script</span></span> |<span data-ttu-id="7fe38-222">Zadejte místo zadání scriptPath a scriptLinkedService zpracování vloženého skriptu.</span><span class="sxs-lookup"><span data-stu-id="7fe38-222">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="7fe38-223">Například: `"script": "CREATE DATABASE test"`.</span><span class="sxs-lookup"><span data-stu-id="7fe38-223">For example: `"script": "CREATE DATABASE test"`.</span></span> |<span data-ttu-id="7fe38-224">Ne (když používáte scriptPath a scriptLinkedService)</span><span class="sxs-lookup"><span data-stu-id="7fe38-224">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="7fe38-225">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="7fe38-225">degreeOfParallelism</span></span> |<span data-ttu-id="7fe38-226">maximální počet uzlů Hello současně použít toorun hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="7fe38-226">hello maximum number of nodes simultaneously used toorun hello job.</span></span> |<span data-ttu-id="7fe38-227">Ne</span><span class="sxs-lookup"><span data-stu-id="7fe38-227">No</span></span> |
| <span data-ttu-id="7fe38-228">Priorita</span><span class="sxs-lookup"><span data-stu-id="7fe38-228">priority</span></span> |<span data-ttu-id="7fe38-229">Určuje, které z uložených ve frontě úloh by měl být vybrané toorun nejdřív.</span><span class="sxs-lookup"><span data-stu-id="7fe38-229">Determines which jobs out of all that are queued should be selected toorun first.</span></span> <span data-ttu-id="7fe38-230">Hello nižší hello číslo, vyšší prioritu hello hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-230">hello lower hello number, hello higher hello priority.</span></span> |<span data-ttu-id="7fe38-231">Ne</span><span class="sxs-lookup"><span data-stu-id="7fe38-231">No</span></span> |
| <span data-ttu-id="7fe38-232">parameters</span><span class="sxs-lookup"><span data-stu-id="7fe38-232">parameters</span></span> |<span data-ttu-id="7fe38-233">Parametry pro skript hello U-SQL</span><span class="sxs-lookup"><span data-stu-id="7fe38-233">Parameters for hello U-SQL script</span></span> |<span data-ttu-id="7fe38-234">Ne</span><span class="sxs-lookup"><span data-stu-id="7fe38-234">No</span></span> |
| <span data-ttu-id="7fe38-235">runtimeVersion</span><span class="sxs-lookup"><span data-stu-id="7fe38-235">runtimeVersion</span></span> | <span data-ttu-id="7fe38-236">Verze runtime toouse modul hello U-SQL</span><span class="sxs-lookup"><span data-stu-id="7fe38-236">Runtime version of hello U-SQL engine toouse</span></span> | <span data-ttu-id="7fe38-237">Ne</span><span class="sxs-lookup"><span data-stu-id="7fe38-237">No</span></span> | 
| <span data-ttu-id="7fe38-238">compilationMode</span><span class="sxs-lookup"><span data-stu-id="7fe38-238">compilationMode</span></span> | <p><span data-ttu-id="7fe38-239">Režim kompilace U-SQL.</span><span class="sxs-lookup"><span data-stu-id="7fe38-239">Compilation mode of U-SQL.</span></span> <span data-ttu-id="7fe38-240">Musí být jedna z těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="7fe38-240">Must be one of these values:</span></span></p> <ul><li><span data-ttu-id="7fe38-241">**Sémantické:** provádět jenom sémantického kontroly a nezbytné správností kontroly.</span><span class="sxs-lookup"><span data-stu-id="7fe38-241">**Semantic:** Only perform semantic checks and necessary sanity checks.</span></span></li><li><span data-ttu-id="7fe38-242">**Úplné:** provést úplné kompilace hello, včetně kontrola syntaxe, optimalizace, generování kódu atd.</span><span class="sxs-lookup"><span data-stu-id="7fe38-242">**Full:** Perform hello full compilation, including syntax check, optimization, code generation, etc.</span></span></li><li><span data-ttu-id="7fe38-243">**SingleBox:** provést úplné kompilace hello s tooSingleBox TargetType nastavení.</span><span class="sxs-lookup"><span data-stu-id="7fe38-243">**SingleBox:** Perform hello full compilation, with TargetType setting tooSingleBox.</span></span></li></ul><p><span data-ttu-id="7fe38-244">Pokud nezadáte hodnotu pro tuto vlastnost, hello server určuje režim optimální kompilace hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-244">If you don't specify a value for this property, hello server determines hello optimal compilation mode.</span></span> </p>| <span data-ttu-id="7fe38-245">Ne</span><span class="sxs-lookup"><span data-stu-id="7fe38-245">No</span></span> | 

<span data-ttu-id="7fe38-246">V tématu [definice skriptu SearchLogProcessing.txt](#sample-u-sql-script) pro definici skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-246">See [SearchLogProcessing.txt Script Definition](#sample-u-sql-script) for hello script definition.</span></span> 

## <a name="sample-input-and-output-datasets"></a><span data-ttu-id="7fe38-247">Ukázkové vstupní a výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="7fe38-247">Sample input and output datasets</span></span>
### <a name="input-dataset"></a><span data-ttu-id="7fe38-248">Vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="7fe38-248">Input dataset</span></span>
<span data-ttu-id="7fe38-249">V tomto příkladu hello vstupní data nachází v Azure Data Lake Store (soubor SearchLog.tsv soubor ve složce datalake/vstup hello).</span><span class="sxs-lookup"><span data-stu-id="7fe38-249">In this example, hello input data resides in an Azure Data Lake Store (SearchLog.tsv file in hello datalake/input folder).</span></span> 

```json
{
    "name": "DataLakeTable",
    "properties": {
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
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

### <a name="output-dataset"></a><span data-ttu-id="7fe38-250">Výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="7fe38-250">Output dataset</span></span>
<span data-ttu-id="7fe38-251">V tomto příkladu hello výstupních dat vytvářených hello skript U-SQL uložený v Azure Data Lake Store (datalake výstupní složka).</span><span class="sxs-lookup"><span data-stu-id="7fe38-251">In this example, hello output data produced by hello U-SQL script is stored in an Azure Data Lake Store (datalake/output folder).</span></span> 

```json
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="sample-data-lake-store-linked-service"></a><span data-ttu-id="7fe38-252">Ukázková Data Lake Store propojená služba</span><span class="sxs-lookup"><span data-stu-id="7fe38-252">Sample Data Lake Store Linked Service</span></span>
<span data-ttu-id="7fe38-253">Zde je definice hello hello ukázky Azure Data Lake Store propojené služby používané hello vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="7fe38-253">Here is hello definition of hello sample Azure Data Lake Store linked service used by hello input/output datasets.</span></span> 

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
        }
    }
}
```

<span data-ttu-id="7fe38-254">V tématu [přesunout tooand dat z Azure Data Lake Store](data-factory-azure-datalake-connector.md) článku popisy vlastností JSON.</span><span class="sxs-lookup"><span data-stu-id="7fe38-254">See [Move data tooand from Azure Data Lake Store](data-factory-azure-datalake-connector.md) article for descriptions of JSON properties.</span></span> 

## <a name="sample-u-sql-script"></a><span data-ttu-id="7fe38-255">Ukázkový skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="7fe38-255">Sample U-SQL Script</span></span>

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    too@out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

<span data-ttu-id="7fe38-256">Hello hodnoty pro  **@in**  a  **@out**  parametry v hello U-SQL skriptů jsou předaná dynamicky ADF pomocí oddílu "parametry" hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-256">hello values for **@in** and **@out** parameters in hello U-SQL script are passed dynamically by ADF using hello ‘parameters’ section.</span></span> <span data-ttu-id="7fe38-257">Naleznete hello 'parametry' v definici kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-257">See hello ‘parameters’ section in hello pipeline definition.</span></span>

<span data-ttu-id="7fe38-258">Také můžete zadat další vlastnosti, například degreeOfParallelism a priority v definici vaší kanálu pro hello úlohy, které běží na hello služby Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="7fe38-258">You can specify other properties such as degreeOfParallelism and priority as well in your pipeline definition for hello jobs that run on hello Azure Data Lake Analytics service.</span></span>

## <a name="dynamic-parameters"></a><span data-ttu-id="7fe38-259">Dynamické parametry</span><span class="sxs-lookup"><span data-stu-id="7fe38-259">Dynamic parameters</span></span>
<span data-ttu-id="7fe38-260">V definici kanálu ukázka hello a odhlašování parametry jsou přiřazeny pevně definovaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="7fe38-260">In hello sample pipeline definition, in and out parameters are assigned with hard-coded values.</span></span> 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

<span data-ttu-id="7fe38-261">Místo toho je možné toouse dynamických parametrů.</span><span class="sxs-lookup"><span data-stu-id="7fe38-261">It is possible toouse dynamic parameters instead.</span></span> <span data-ttu-id="7fe38-262">Například:</span><span class="sxs-lookup"><span data-stu-id="7fe38-262">For example:</span></span> 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

<span data-ttu-id="7fe38-263">V takovém případě vstupní soubory jsou stále zachyceny ze složky /datalake/input hello a výstupní soubory se generují ve složce /datalake/output hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-263">In this case, input files are still picked up from hello /datalake/input folder and output files are generated in hello /datalake/output folder.</span></span> <span data-ttu-id="7fe38-264">názvy souborů Hello jsou dynamické podle času zahájení řez hello.</span><span class="sxs-lookup"><span data-stu-id="7fe38-264">hello file names are dynamic based on hello slice start time.</span></span>  

