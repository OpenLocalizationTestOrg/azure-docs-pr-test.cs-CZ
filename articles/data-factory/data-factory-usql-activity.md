---
title: "Transformace dat pomocí skriptu U-SQL - Azure | Microsoft Docs"
description: "Informace o zpracování nebo transformace dat pomocí spouštění skriptů U-SQL na výpočetní služba Azure Data Lake Analytics."
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
ms.openlocfilehash: 49a809af92ed1bc6664fbdd3bf1aabf36afb8180
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a><span data-ttu-id="883d3-103">Transformace dat pomocí spouštění skriptů U-SQL v Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="883d3-103">Transform data by running U-SQL scripts on Azure Data Lake Analytics</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="883d3-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="883d3-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="883d3-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="883d3-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="883d3-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="883d3-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="883d3-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="883d3-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="883d3-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="883d3-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="883d3-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="883d3-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="883d3-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="883d3-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="883d3-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="883d3-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="883d3-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="883d3-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="883d3-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="883d3-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="883d3-114">Kanál v objektu pro vytváření dat Azure zpracovává data v služby propojené úložiště pomocí propojené výpočetní služby.</span><span class="sxs-lookup"><span data-stu-id="883d3-114">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="883d3-115">Obsahuje posloupnost aktivit, kde každá aktivita provede konkrétní zpracování operace.</span><span class="sxs-lookup"><span data-stu-id="883d3-115">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="883d3-116">Tento článek popisuje **Data Lake Analytics U-SQL aktivity** , která se spouští **U-SQL** skript na **Azure Data Lake Analytics** výpočetní propojené služby.</span><span class="sxs-lookup"><span data-stu-id="883d3-116">This article describes the **Data Lake Analytics U-SQL Activity** that runs a **U-SQL** script on an **Azure Data Lake Analytics** compute linked service.</span></span> 

> [!NOTE]
> <span data-ttu-id="883d3-117">Vytvoření účtu Azure Data Lake Analytics před vytvořením kanálu s aktivitou Data Lake Analytics U-SQL.</span><span class="sxs-lookup"><span data-stu-id="883d3-117">Create an Azure Data Lake Analytics account before creating a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="883d3-118">Další informace o Azure Data Lake Analytics najdete v tématu [Začínáme s Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="883d3-118">To learn about Azure Data Lake Analytics, see [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span>
> 
> <span data-ttu-id="883d3-119">Zkontrolujte [vytvoření vaší první kurz kanálu](data-factory-build-your-first-pipeline.md) podrobné pokyny pro vytváření dat, propojené služby, datových sad a kanálu.</span><span class="sxs-lookup"><span data-stu-id="883d3-119">Review the [Build your first pipeline tutorial](data-factory-build-your-first-pipeline.md) for detailed steps to create a data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="883d3-120">Použití JSON fragmenty pomocí editoru služby Data Factory nebo Visual Studio nebo Azure PowerShell k vytvoření entit služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="883d3-120">Use JSON snippets with Data Factory Editor or Visual Studio or Azure PowerShell to create Data Factory entities.</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="883d3-121">Typy podporované ověřování</span><span class="sxs-lookup"><span data-stu-id="883d3-121">Supported authentication types</span></span>
<span data-ttu-id="883d3-122">Aktivita U-SQL podporuje následující typy ověřování proti Data Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="883d3-122">U-SQL activity supports below authentication types against Data Lake Analytics:</span></span>
* <span data-ttu-id="883d3-123">Ověřování instančních objektů</span><span class="sxs-lookup"><span data-stu-id="883d3-123">Service principal authentication</span></span>
* <span data-ttu-id="883d3-124">Ověření přihlašovacích údajů (OAuth) uživatele</span><span class="sxs-lookup"><span data-stu-id="883d3-124">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="883d3-125">Doporučujeme použít objekt zabezpečení ověřování služby, zejména pro naplánovaného spuštění U-SQL.</span><span class="sxs-lookup"><span data-stu-id="883d3-125">We recommend that you use service principal authentication, especially for a scheduled U-SQL execution.</span></span> <span data-ttu-id="883d3-126">Vypršení platnosti tokenu chování mohou nastat u ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="883d3-126">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="883d3-127">Podrobnosti konfigurace najdete v tématu [propojené vlastnosti služby](#azure-data-lake-analytics-linked-service) části.</span><span class="sxs-lookup"><span data-stu-id="883d3-127">For configuration details, see the [Linked service properties](#azure-data-lake-analytics-linked-service) section.</span></span>

## <a name="azure-data-lake-analytics-linked-service"></a><span data-ttu-id="883d3-128">Propojená služba Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="883d3-128">Azure Data Lake Analytics Linked Service</span></span>
<span data-ttu-id="883d3-129">Vytvoříte **Azure Data Lake Analytics** propojená služba Azure Data Lake Analytics výpočetní služby s objektem pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="883d3-129">You create an **Azure Data Lake Analytics** linked service to link an Azure Data Lake Analytics compute service to an Azure data factory.</span></span> <span data-ttu-id="883d3-130">Data Lake Analytics U-SQL aktivitu v kanálu odkazuje na tato propojená služba.</span><span class="sxs-lookup"><span data-stu-id="883d3-130">The Data Lake Analytics U-SQL activity in the pipeline refers to this linked service.</span></span> 

<span data-ttu-id="883d3-131">Následující tabulka obsahuje popis obecné vlastnosti používané v definici JSON.</span><span class="sxs-lookup"><span data-stu-id="883d3-131">The following table provides descriptions for the generic properties used in the JSON definition.</span></span> <span data-ttu-id="883d3-132">Dále můžete mezi instanční objekt a ověření přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="883d3-132">You can further choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="883d3-133">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="883d3-133">Property</span></span> | <span data-ttu-id="883d3-134">Popis</span><span class="sxs-lookup"><span data-stu-id="883d3-134">Description</span></span> | <span data-ttu-id="883d3-135">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="883d3-135">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="883d3-136">**Typ**</span><span class="sxs-lookup"><span data-stu-id="883d3-136">**type**</span></span> |<span data-ttu-id="883d3-137">Vlastnost typu musí být nastavená na: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="883d3-137">The type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="883d3-138">Ano</span><span class="sxs-lookup"><span data-stu-id="883d3-138">Yes</span></span> |
| <span data-ttu-id="883d3-139">**název účtu**</span><span class="sxs-lookup"><span data-stu-id="883d3-139">**accountName**</span></span> |<span data-ttu-id="883d3-140">Název účtu Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="883d3-140">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="883d3-141">Ano</span><span class="sxs-lookup"><span data-stu-id="883d3-141">Yes</span></span> |
| <span data-ttu-id="883d3-142">**dataLakeAnalyticsUri**</span><span class="sxs-lookup"><span data-stu-id="883d3-142">**dataLakeAnalyticsUri**</span></span> |<span data-ttu-id="883d3-143">Identifikátor URI služby Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="883d3-143">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="883d3-144">Ne</span><span class="sxs-lookup"><span data-stu-id="883d3-144">No</span></span> |
| <span data-ttu-id="883d3-145">**ID předplatného**</span><span class="sxs-lookup"><span data-stu-id="883d3-145">**subscriptionId**</span></span> |<span data-ttu-id="883d3-146">Id předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="883d3-146">Azure subscription id</span></span> |<span data-ttu-id="883d3-147">Ne (když není určeno, předplatné objektu pro vytváření dat se používá).</span><span class="sxs-lookup"><span data-stu-id="883d3-147">No (If not specified, subscription of the data factory is used).</span></span> |
| <span data-ttu-id="883d3-148">**Název skupiny prostředků**</span><span class="sxs-lookup"><span data-stu-id="883d3-148">**resourceGroupName**</span></span> |<span data-ttu-id="883d3-149">Název skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="883d3-149">Azure resource group name</span></span> |<span data-ttu-id="883d3-150">Ne (když není určeno, skupinu prostředků objektu pro vytváření dat se používá).</span><span class="sxs-lookup"><span data-stu-id="883d3-150">No (If not specified, resource group of the data factory is used).</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="883d3-151">Objekt zabezpečení ověřování služby (doporučeno)</span><span class="sxs-lookup"><span data-stu-id="883d3-151">Service principal authentication (recommended)</span></span>
<span data-ttu-id="883d3-152">Pokud chcete použít ověřování hlavní služby, zaregistrujte entitu aplikace v Azure Active Directory (Azure AD) a jí udělit přístup k Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="883d3-152">To use service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it the access to Data Lake Store.</span></span> <span data-ttu-id="883d3-153">Podrobné pokyny najdete v tématu [Service-to-service ověřování](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="883d3-153">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="883d3-154">Poznamenejte si následující hodnoty, které můžete použít k definování propojené služby:</span><span class="sxs-lookup"><span data-stu-id="883d3-154">Make note of the following values, which you use to define the linked service:</span></span>
* <span data-ttu-id="883d3-155">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="883d3-155">Application ID</span></span>
* <span data-ttu-id="883d3-156">Klíč aplikace</span><span class="sxs-lookup"><span data-stu-id="883d3-156">Application key</span></span> 
* <span data-ttu-id="883d3-157">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="883d3-157">Tenant ID</span></span>

<span data-ttu-id="883d3-158">Použijte objekt zabezpečení ověřování služby tak, že zadáte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="883d3-158">Use service principal authentication by specifying the following properties:</span></span>

| <span data-ttu-id="883d3-159">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="883d3-159">Property</span></span> | <span data-ttu-id="883d3-160">Popis</span><span class="sxs-lookup"><span data-stu-id="883d3-160">Description</span></span> | <span data-ttu-id="883d3-161">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="883d3-161">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="883d3-162">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="883d3-162">**servicePrincipalId**</span></span> | <span data-ttu-id="883d3-163">Zadejte ID aplikace klienta.</span><span class="sxs-lookup"><span data-stu-id="883d3-163">Specify the application's client ID.</span></span> | <span data-ttu-id="883d3-164">Ano</span><span class="sxs-lookup"><span data-stu-id="883d3-164">Yes</span></span> |
| <span data-ttu-id="883d3-165">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="883d3-165">**servicePrincipalKey**</span></span> | <span data-ttu-id="883d3-166">Zadejte klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="883d3-166">Specify the application's key.</span></span> | <span data-ttu-id="883d3-167">Ano</span><span class="sxs-lookup"><span data-stu-id="883d3-167">Yes</span></span> |
| <span data-ttu-id="883d3-168">**klienta**</span><span class="sxs-lookup"><span data-stu-id="883d3-168">**tenant**</span></span> | <span data-ttu-id="883d3-169">Zadejte informace o klienta (název nebo klienta domény ID) v rámci které se nachází aplikace.</span><span class="sxs-lookup"><span data-stu-id="883d3-169">Specify the tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="883d3-170">Můžete ji načíst podržením ukazatele myši v pravém horním rohu portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="883d3-170">You can retrieve it by hovering the mouse in the upper-right corner of the Azure portal.</span></span> | <span data-ttu-id="883d3-171">Ano</span><span class="sxs-lookup"><span data-stu-id="883d3-171">Yes</span></span> |

<span data-ttu-id="883d3-172">**Příkladu: Ověření objektu službu**</span><span class="sxs-lookup"><span data-stu-id="883d3-172">**Example: Service principal authentication**</span></span>
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

### <a name="user-credential-authentication"></a><span data-ttu-id="883d3-173">Ověření pověření uživatele</span><span class="sxs-lookup"><span data-stu-id="883d3-173">User credential authentication</span></span>
<span data-ttu-id="883d3-174">Alternativně můžete použít ověřování pověření uživatele pro Data Lake Analytics zadáním následujících vlastností:</span><span class="sxs-lookup"><span data-stu-id="883d3-174">Alternatively, you can use user credential authentication for Data Lake Analytics by specifying the following properties:</span></span>

| <span data-ttu-id="883d3-175">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="883d3-175">Property</span></span> | <span data-ttu-id="883d3-176">Popis</span><span class="sxs-lookup"><span data-stu-id="883d3-176">Description</span></span> | <span data-ttu-id="883d3-177">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="883d3-177">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="883d3-178">**autorizace**</span><span class="sxs-lookup"><span data-stu-id="883d3-178">**authorization**</span></span> | <span data-ttu-id="883d3-179">Klikněte **Autorizovat** tlačítko v editoru služby Data Factory a zadejte svoje přihlašovací údaje, který přiřazuje URL pro autorizaci automaticky generovaný této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="883d3-179">Click the **Authorize** button in the Data Factory Editor and enter your credential that assigns the autogenerated authorization URL to this property.</span></span> | <span data-ttu-id="883d3-180">Ano</span><span class="sxs-lookup"><span data-stu-id="883d3-180">Yes</span></span> |
| <span data-ttu-id="883d3-181">**ID relace**</span><span class="sxs-lookup"><span data-stu-id="883d3-181">**sessionId**</span></span> | <span data-ttu-id="883d3-182">ID relace OAuth z autorizační relace OAuth.</span><span class="sxs-lookup"><span data-stu-id="883d3-182">OAuth session ID from the OAuth authorization session.</span></span> <span data-ttu-id="883d3-183">Každé ID relace je jedinečné a může být použit pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="883d3-183">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="883d3-184">Toto nastavení se automaticky generuje při pomocí editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="883d3-184">This setting is automatically generated when you use the Data Factory Editor.</span></span> | <span data-ttu-id="883d3-185">Ano</span><span class="sxs-lookup"><span data-stu-id="883d3-185">Yes</span></span> |

<span data-ttu-id="883d3-186">**Příklad: Ověření pověření uživatele**</span><span class="sxs-lookup"><span data-stu-id="883d3-186">**Example: User credential authentication**</span></span>
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

#### <a name="token-expiration"></a><span data-ttu-id="883d3-187">Vypršení platnosti tokenu</span><span class="sxs-lookup"><span data-stu-id="883d3-187">Token expiration</span></span>
<span data-ttu-id="883d3-188">Autorizační kód, který jste vygenerovali pomocí **Autorizovat** tlačítko vyprší po určité době.</span><span class="sxs-lookup"><span data-stu-id="883d3-188">The authorization code you generated by using the **Authorize** button expires after sometime.</span></span> <span data-ttu-id="883d3-189">Pro dobu vypršení platnosti pro různé typy uživatelských účtů najdete v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="883d3-189">See the following table for the expiration times for different types of user accounts.</span></span> <span data-ttu-id="883d3-190">Mohou se zobrazit následující chyby zprávy při ověřování **platnost tokenu vyprší**: přihlašovací údaje chyby operace: invalid_grant - AADSTS70002: Chyba při ověřování přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="883d3-190">You may see the following error message when the authentication **token expires**: Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="883d3-191">AADSTS70008: Předloženému udělení přístupu je prošlý nebo odvolat.</span><span class="sxs-lookup"><span data-stu-id="883d3-191">AADSTS70008: The provided access grant is expired or revoked.</span></span> <span data-ttu-id="883d3-192">ID trasování: ID korelace d18629e8-af88-43c5-88e3-d8419eb1fca1: časové razítko fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21:09:31Z</span><span class="sxs-lookup"><span data-stu-id="883d3-192">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21:09:31Z</span></span>

| <span data-ttu-id="883d3-193">Typ uživatele</span><span class="sxs-lookup"><span data-stu-id="883d3-193">User type</span></span> | <span data-ttu-id="883d3-194">Platnost vyprší po</span><span class="sxs-lookup"><span data-stu-id="883d3-194">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="883d3-195">Uživatelské účty, které nejsou spravované přes Azure Active Directory (@hotmail.com, @live.comatd.)</span><span class="sxs-lookup"><span data-stu-id="883d3-195">User accounts NOT managed by Azure Active Directory (@hotmail.com, @live.com, etc.)</span></span> |<span data-ttu-id="883d3-196">12 hodin</span><span class="sxs-lookup"><span data-stu-id="883d3-196">12 hours</span></span> |
| <span data-ttu-id="883d3-197">Účty uživatelů spravované pomocí Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="883d3-197">Users accounts managed by Azure Active Directory (AAD)</span></span> |<span data-ttu-id="883d3-198">14 dnů po poslední řez spustit.</span><span class="sxs-lookup"><span data-stu-id="883d3-198">14 days after the last slice run.</span></span> <br/><br/><span data-ttu-id="883d3-199">90 dnů, pokud řezu založeného na základě OAuth propojené služby používá alespoň jednou za 14 dní.</span><span class="sxs-lookup"><span data-stu-id="883d3-199">90 days, if a slice based on OAuth-based linked service runs at least once every 14 days.</span></span> |

<span data-ttu-id="883d3-200">Vyhněte se/vyřešit tuto chybu, opětovné pověření pomocí **Authorize** tlačítko při **platnost tokenu vyprší** a znovu nasaďte propojené služby.</span><span class="sxs-lookup"><span data-stu-id="883d3-200">To avoid/resolve this error, reauthorize using the **Authorize** button when the **token expires** and redeploy the linked service.</span></span> <span data-ttu-id="883d3-201">Můžete také vygenerovat hodnoty pro **sessionId** a **autorizace** vlastnosti programově pomocí kódu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="883d3-201">You can also generate values for **sessionId** and **authorization** properties programmatically using code as follows:</span></span>

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

<span data-ttu-id="883d3-202">V tématu [azuredatalakestorelinkedservice třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), a [AuthorizationSessionGetResponse třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témata podrobnosti o třídy objektu pro vytváření dat používá v kódu.</span><span class="sxs-lookup"><span data-stu-id="883d3-202">See [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics for details about the Data Factory classes used in the code.</span></span> <span data-ttu-id="883d3-203">Přidat odkaz na: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll pro třídu WindowsFormsWebAuthenticationDialog.</span><span class="sxs-lookup"><span data-stu-id="883d3-203">Add a reference to: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll for the WindowsFormsWebAuthenticationDialog class.</span></span> 

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="883d3-204">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="883d3-204">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="883d3-205">Následující fragment kódu JSON definuje kanál s aktivitou Data Lake Analytics U-SQL.</span><span class="sxs-lookup"><span data-stu-id="883d3-205">The following JSON snippet defines a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="883d3-206">Definici aktivity odkazuje na Azure Data Lake Analytics propojené služby, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="883d3-206">The activity definition has a reference to the Azure Data Lake Analytics linked service you created earlier.</span></span>   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
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

<span data-ttu-id="883d3-207">Následující tabulka popisuje názvy a popisy vlastností, které jsou specifické pro tuto aktivitu.</span><span class="sxs-lookup"><span data-stu-id="883d3-207">The following table describes names and descriptions of properties that are specific to this activity.</span></span> 

| <span data-ttu-id="883d3-208">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="883d3-208">Property</span></span> | <span data-ttu-id="883d3-209">Popis</span><span class="sxs-lookup"><span data-stu-id="883d3-209">Description</span></span> | <span data-ttu-id="883d3-210">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="883d3-210">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="883d3-211">type</span><span class="sxs-lookup"><span data-stu-id="883d3-211">type</span></span> |<span data-ttu-id="883d3-212">Vlastnost typu musí být nastavená na **DataLakeAnalyticsU SQL**.</span><span class="sxs-lookup"><span data-stu-id="883d3-212">The type property must be set to **DataLakeAnalyticsU-SQL**.</span></span> |<span data-ttu-id="883d3-213">Ano</span><span class="sxs-lookup"><span data-stu-id="883d3-213">Yes</span></span> |
| <span data-ttu-id="883d3-214">scriptPath</span><span class="sxs-lookup"><span data-stu-id="883d3-214">scriptPath</span></span> |<span data-ttu-id="883d3-215">Cesta ke složce, který obsahuje skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="883d3-215">Path to folder that contains the U-SQL script.</span></span> <span data-ttu-id="883d3-216">Název souboru je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="883d3-216">Name of the file is case-sensitive.</span></span> |<span data-ttu-id="883d3-217">Ne (když používáte skript)</span><span class="sxs-lookup"><span data-stu-id="883d3-217">No (if you use script)</span></span> |
| <span data-ttu-id="883d3-218">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="883d3-218">scriptLinkedService</span></span> |<span data-ttu-id="883d3-219">Propojené služby, který odkazuje úložiště, který obsahuje skript pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="883d3-219">Linked service that links the storage that contains the script to the data factory</span></span> |<span data-ttu-id="883d3-220">Ne (když používáte skript)</span><span class="sxs-lookup"><span data-stu-id="883d3-220">No (if you use script)</span></span> |
| <span data-ttu-id="883d3-221">Skript</span><span class="sxs-lookup"><span data-stu-id="883d3-221">script</span></span> |<span data-ttu-id="883d3-222">Zadejte místo zadání scriptPath a scriptLinkedService zpracování vloženého skriptu.</span><span class="sxs-lookup"><span data-stu-id="883d3-222">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="883d3-223">Například: `"script": "CREATE DATABASE test"`.</span><span class="sxs-lookup"><span data-stu-id="883d3-223">For example: `"script": "CREATE DATABASE test"`.</span></span> |<span data-ttu-id="883d3-224">Ne (když používáte scriptPath a scriptLinkedService)</span><span class="sxs-lookup"><span data-stu-id="883d3-224">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="883d3-225">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="883d3-225">degreeOfParallelism</span></span> |<span data-ttu-id="883d3-226">Maximální počet uzlů současně slouží ke spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="883d3-226">The maximum number of nodes simultaneously used to run the job.</span></span> |<span data-ttu-id="883d3-227">Ne</span><span class="sxs-lookup"><span data-stu-id="883d3-227">No</span></span> |
| <span data-ttu-id="883d3-228">Priorita</span><span class="sxs-lookup"><span data-stu-id="883d3-228">priority</span></span> |<span data-ttu-id="883d3-229">Určuje, jaké úlohy mimo všechny, které jsou zařazeny do fronty, měla by být vybrána má spustit jako první.</span><span class="sxs-lookup"><span data-stu-id="883d3-229">Determines which jobs out of all that are queued should be selected to run first.</span></span> <span data-ttu-id="883d3-230">Čím nižší je číslo, tím vyšší je priorita.</span><span class="sxs-lookup"><span data-stu-id="883d3-230">The lower the number, the higher the priority.</span></span> |<span data-ttu-id="883d3-231">Ne</span><span class="sxs-lookup"><span data-stu-id="883d3-231">No</span></span> |
| <span data-ttu-id="883d3-232">Parametry</span><span class="sxs-lookup"><span data-stu-id="883d3-232">parameters</span></span> |<span data-ttu-id="883d3-233">Parametry pro skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="883d3-233">Parameters for the U-SQL script</span></span> |<span data-ttu-id="883d3-234">Ne</span><span class="sxs-lookup"><span data-stu-id="883d3-234">No</span></span> |
| <span data-ttu-id="883d3-235">runtimeVersion</span><span class="sxs-lookup"><span data-stu-id="883d3-235">runtimeVersion</span></span> | <span data-ttu-id="883d3-236">Verze runtime – stroje U-SQL používat</span><span class="sxs-lookup"><span data-stu-id="883d3-236">Runtime version of the U-SQL engine to use</span></span> | <span data-ttu-id="883d3-237">Ne</span><span class="sxs-lookup"><span data-stu-id="883d3-237">No</span></span> | 
| <span data-ttu-id="883d3-238">compilationMode</span><span class="sxs-lookup"><span data-stu-id="883d3-238">compilationMode</span></span> | <p><span data-ttu-id="883d3-239">Režim kompilace U-SQL.</span><span class="sxs-lookup"><span data-stu-id="883d3-239">Compilation mode of U-SQL.</span></span> <span data-ttu-id="883d3-240">Musí být jedna z těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="883d3-240">Must be one of these values:</span></span></p> <ul><li><span data-ttu-id="883d3-241">**Sémantické:** provádět jenom sémantického kontroly a nezbytné správností kontroly.</span><span class="sxs-lookup"><span data-stu-id="883d3-241">**Semantic:** Only perform semantic checks and necessary sanity checks.</span></span></li><li><span data-ttu-id="883d3-242">**Úplné:** provést úplné kompilace, včetně kontrola syntaxe, optimalizace, generování kódu atd.</span><span class="sxs-lookup"><span data-stu-id="883d3-242">**Full:** Perform the full compilation, including syntax check, optimization, code generation, etc.</span></span></li><li><span data-ttu-id="883d3-243">**SingleBox:** provést úplné kompilace s TargetType nastavení SingleBox.</span><span class="sxs-lookup"><span data-stu-id="883d3-243">**SingleBox:** Perform the full compilation, with TargetType setting to SingleBox.</span></span></li></ul><p><span data-ttu-id="883d3-244">Pokud nezadáte hodnotu pro tuto vlastnost, server určí režim optimální kompilace.</span><span class="sxs-lookup"><span data-stu-id="883d3-244">If you don't specify a value for this property, the server determines the optimal compilation mode.</span></span> </p>| <span data-ttu-id="883d3-245">Ne</span><span class="sxs-lookup"><span data-stu-id="883d3-245">No</span></span> | 

<span data-ttu-id="883d3-246">V tématu [definice skriptu SearchLogProcessing.txt](#sample-u-sql-script) pro definici skriptu.</span><span class="sxs-lookup"><span data-stu-id="883d3-246">See [SearchLogProcessing.txt Script Definition](#sample-u-sql-script) for the script definition.</span></span> 

## <a name="sample-input-and-output-datasets"></a><span data-ttu-id="883d3-247">Ukázkové vstupní a výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="883d3-247">Sample input and output datasets</span></span>
### <a name="input-dataset"></a><span data-ttu-id="883d3-248">Vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="883d3-248">Input dataset</span></span>
<span data-ttu-id="883d3-249">V tomto příkladu vstupní data nachází v Azure Data Lake Store (soubor SearchLog.tsv soubor ve složce datalake/vstupu).</span><span class="sxs-lookup"><span data-stu-id="883d3-249">In this example, the input data resides in an Azure Data Lake Store (SearchLog.tsv file in the datalake/input folder).</span></span> 

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

### <a name="output-dataset"></a><span data-ttu-id="883d3-250">Výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="883d3-250">Output dataset</span></span>
<span data-ttu-id="883d3-251">V tomto příkladu výstupní data vytvořená skript U-SQL uložený v Azure Data Lake Store (datalake výstupní složka).</span><span class="sxs-lookup"><span data-stu-id="883d3-251">In this example, the output data produced by the U-SQL script is stored in an Azure Data Lake Store (datalake/output folder).</span></span> 

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

### <a name="sample-data-lake-store-linked-service"></a><span data-ttu-id="883d3-252">Ukázková Data Lake Store propojená služba</span><span class="sxs-lookup"><span data-stu-id="883d3-252">Sample Data Lake Store Linked Service</span></span>
<span data-ttu-id="883d3-253">Zde je definici vzorku Azure Data Lake Store propojená služba používá vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="883d3-253">Here is the definition of the sample Azure Data Lake Store linked service used by the input/output datasets.</span></span> 

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

<span data-ttu-id="883d3-254">V tématu [přesun dat do a z Azure Data Lake Store](data-factory-azure-datalake-connector.md) článku popisy vlastností JSON.</span><span class="sxs-lookup"><span data-stu-id="883d3-254">See [Move data to and from Azure Data Lake Store](data-factory-azure-datalake-connector.md) article for descriptions of JSON properties.</span></span> 

## <a name="sample-u-sql-script"></a><span data-ttu-id="883d3-255">Ukázkový skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="883d3-255">Sample U-SQL Script</span></span>

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
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

<span data-ttu-id="883d3-256">Hodnoty pro  **@in**  a  **@out**  parametry ve skriptu U-SQL jsou předaná dynamicky ADF pomocí části parametry.".</span><span class="sxs-lookup"><span data-stu-id="883d3-256">The values for **@in** and **@out** parameters in the U-SQL script are passed dynamically by ADF using the ‘parameters’ section.</span></span> <span data-ttu-id="883d3-257">Najdete v části 'parametry' v definici kanálu.</span><span class="sxs-lookup"><span data-stu-id="883d3-257">See the ‘parameters’ section in the pipeline definition.</span></span>

<span data-ttu-id="883d3-258">Také můžete zadat další vlastnosti, například degreeOfParallelism a priority v definici vaší kanálu pro úlohy, které běží na službu Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="883d3-258">You can specify other properties such as degreeOfParallelism and priority as well in your pipeline definition for the jobs that run on the Azure Data Lake Analytics service.</span></span>

## <a name="dynamic-parameters"></a><span data-ttu-id="883d3-259">Dynamické parametry</span><span class="sxs-lookup"><span data-stu-id="883d3-259">Dynamic parameters</span></span>
<span data-ttu-id="883d3-260">V definici ukázkový kanál a odhlašování parametry jsou přiřazeny pevně definovaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="883d3-260">In the sample pipeline definition, in and out parameters are assigned with hard-coded values.</span></span> 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

<span data-ttu-id="883d3-261">Je možné místo toho použít dynamické parametry.</span><span class="sxs-lookup"><span data-stu-id="883d3-261">It is possible to use dynamic parameters instead.</span></span> <span data-ttu-id="883d3-262">Například:</span><span class="sxs-lookup"><span data-stu-id="883d3-262">For example:</span></span> 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

<span data-ttu-id="883d3-263">V takovém případě vstupní soubory jsou stále zachyceny ze složky /datalake/input a výstupní soubory se generují ve složce /datalake/output.</span><span class="sxs-lookup"><span data-stu-id="883d3-263">In this case, input files are still picked up from the /datalake/input folder and output files are generated in the /datalake/output folder.</span></span> <span data-ttu-id="883d3-264">Názvy souborů jsou dynamické podle času zahájení řez.</span><span class="sxs-lookup"><span data-stu-id="883d3-264">The file names are dynamic based on the slice start time.</span></span>  

