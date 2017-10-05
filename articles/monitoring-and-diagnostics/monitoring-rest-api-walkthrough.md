---
title: "Monitorování návod rozhraní API REST Azure | Microsoft Docs"
description: "Postup žádosti o ověření a pomocí monitorování REST API služby Azure."
author: mcollier
manager: 
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 565e6a88-3131-4a48-8b82-3effc9a3d5c6
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: mcollier
ms.openlocfilehash: 454a85c4752ec9c7522ef147d5ce594ef5992c32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-monitoring-rest-api-walkthrough"></a><span data-ttu-id="58f5c-103">Monitorování návod rozhraní API REST Azure</span><span class="sxs-lookup"><span data-stu-id="58f5c-103">Azure Monitoring REST API Walkthrough</span></span>
<span data-ttu-id="58f5c-104">V tomto článku se dozvíte, jak provést ověření, abyste mohli používat kódu [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="58f5c-104">This article shows you how to perform authentication so your code can use the [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>         

<span data-ttu-id="58f5c-105">Rozhraní API služby Azure monitorování umožňuje načítání prostřednictvím kódu programu dostupná výchozí definice metrik (Typ Metrika například čas procesoru, požadavků, atd.), členitosti a metriky hodnoty.</span><span class="sxs-lookup"><span data-stu-id="58f5c-105">The Azure Monitor API makes it possible to programmatically retrieve the available default metric definitions (the type of metric such as CPU Time, Requests, etc.), granularity, and metric values.</span></span> <span data-ttu-id="58f5c-106">Po načíst, můžete data uložit v samostatných data store například Azure SQL Database, Azure Cosmos DB nebo Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="58f5c-106">Once retrieved, the data can be saved in a separate data store such as Azure SQL Database, Azure Cosmos DB, or Azure Data Lake.</span></span> <span data-ttu-id="58f5c-107">Odtud můžete provést další analýzu podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="58f5c-107">From there additional analysis can be performed as needed.</span></span>

<span data-ttu-id="58f5c-108">Kromě práce s různými metriky datové body, jako tento článek ukazuje, rozhraní API monitorování umožňuje k zobrazení seznamu pravidla výstrah, zobrazení protokoly aktivity a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="58f5c-108">Besides working with various metric data points, as this article demonstrates, the Monitor API makes it possible to list alert rules, view activity logs, and much more.</span></span> <span data-ttu-id="58f5c-109">Úplný seznam dostupné operace, najdete v článku [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="58f5c-109">For a full list of available operations, see the [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>

## <a name="authenticating-azure-monitor-requests"></a><span data-ttu-id="58f5c-110">Ověřování Azure sledování požadavků</span><span class="sxs-lookup"><span data-stu-id="58f5c-110">Authenticating Azure Monitor Requests</span></span>
<span data-ttu-id="58f5c-111">Prvním krokem je ověřit žádost.</span><span class="sxs-lookup"><span data-stu-id="58f5c-111">The first step is to authenticate the request.</span></span>

<span data-ttu-id="58f5c-112">Všechny úlohy prováděné vůči monitorování rozhraní API služby Azure pomocí Azure Resource Manager ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="58f5c-112">All the tasks executed against the Azure Monitor API use the Azure Resource Manager authentication model.</span></span> <span data-ttu-id="58f5c-113">Proto musí být ověřeny všechny požadavky s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="58f5c-113">Therefore, all requests must be authenticated with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="58f5c-114">Jeden ze způsobů ověření klientská aplikace je vytvořit objekt služby Azure AD a načtení tokenu ověřování (JWT).</span><span class="sxs-lookup"><span data-stu-id="58f5c-114">One approach to authenticate the client application is to create an Azure AD service principal and retrieve the authentication (JWT) token.</span></span> <span data-ttu-id="58f5c-115">Následující ukázkový skript ukazuje vytvoření objektu zabezpečení pomocí prostředí PowerShell služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="58f5c-115">The following sample script demonstrates creating an Azure AD service principal via PowerShell.</span></span> <span data-ttu-id="58f5c-116">Pro podrobnější návod, naleznete v dokumentaci na [pomocí Azure PowerShell k vytvoření objektu služby pro přístup k prostředkům](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span><span class="sxs-lookup"><span data-stu-id="58f5c-116">For a more detailed walk-through, refer to the documentation on [using Azure PowerShell to create a service principal to access resources](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span></span> <span data-ttu-id="58f5c-117">Je také možné [vytvořit objekt služby prostřednictvím portálu Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="58f5c-117">It is also possible to [create a service principal via the Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

<span data-ttu-id="58f5c-118">Zpracovat dotaz rozhraní API Azure monitorování, měli klientské aplikace k ověření použít dříve vytvořenou instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="58f5c-118">To query the Azure Monitor API, the client application should use the previously created service principal to authenticate.</span></span> <span data-ttu-id="58f5c-119">Skript prostředí PowerShell následující příklad ukazuje jeden přístupu, pomocí [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) ke získání tokenu ověřování tokenů JWT.</span><span class="sxs-lookup"><span data-stu-id="58f5c-119">The following example PowerShell script shows one approach, using the [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) to help get the JWT authentication token.</span></span> <span data-ttu-id="58f5c-120">JWT token je předán jako součást parametru HTTP autorizace v žádostech o monitorování REST API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="58f5c-120">The JWT token is passed as part of an HTTP Authorization parameter in requests to the Azure Monitor REST API.</span></span>

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

<span data-ttu-id="58f5c-121">Po dokončení instalace krok ověřování dotazy mohou být pak provedeny na REST API Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="58f5c-121">Once the authentication setup step is complete, queries can then be executed against the Azure Monitor REST API.</span></span> <span data-ttu-id="58f5c-122">Existují dva užitečné dotazy:</span><span class="sxs-lookup"><span data-stu-id="58f5c-122">There are two helpful queries:</span></span>

1. <span data-ttu-id="58f5c-123">Seznam definice metrik pro prostředek</span><span class="sxs-lookup"><span data-stu-id="58f5c-123">List the metric definitions for a resource</span></span>
2. <span data-ttu-id="58f5c-124">Načíst metriky hodnoty</span><span class="sxs-lookup"><span data-stu-id="58f5c-124">Retrieve the metric values</span></span>

## <a name="retrieve-metric-definitions"></a><span data-ttu-id="58f5c-125">Načtení definice metrik</span><span class="sxs-lookup"><span data-stu-id="58f5c-125">Retrieve Metric Definitions</span></span>
> [!NOTE]
> <span data-ttu-id="58f5c-126">Pokud chcete načíst pomocí rozhraní REST API Azure monitorování definice metrik, použijte jako verze rozhraní API "2016-03-01".</span><span class="sxs-lookup"><span data-stu-id="58f5c-126">To retrieve metric definitions using the Azure Monitor REST API, use "2016-03-01" as the API version.</span></span>
>
>

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
<span data-ttu-id="58f5c-127">Pro aplikace logiky Azure objeví definice metrik podobně jako na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="58f5c-127">For an Azure Logic App, the metric definitions would appear similar to the following screenshot:</span></span>

![ALT "JSON zobrazení metriky definice odpovědi."](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

<span data-ttu-id="58f5c-129">Další informace najdete v tématu [seznamu definice metrik pro prostředek v rozhraní REST API Azure monitorování](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="58f5c-129">For more information, see the [List the metric definitions for a resource in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) documentation.</span></span>

## <a name="retrieve-metric-values"></a><span data-ttu-id="58f5c-130">Načtení metriky hodnot</span><span class="sxs-lookup"><span data-stu-id="58f5c-130">Retrieve Metric Values</span></span>
<span data-ttu-id="58f5c-131">Jakmile se ví, že k dispozici definice metrik, je pak možné načíst související hodnoty metriky.</span><span class="sxs-lookup"><span data-stu-id="58f5c-131">Once the available metric definitions are known, it is then possible to retrieve the related metric values.</span></span> <span data-ttu-id="58f5c-132">Použít název v metrice 'Hodnota' (ne ' localizedValue') pro všechny požadavky na filtrování (například získat metriky datových bodů 'CpuTime' a 'Požadavky').</span><span class="sxs-lookup"><span data-stu-id="58f5c-132">Use the metric’s name ‘value’ (not the ‘localizedValue’) for any filtering requests (for example, retrieve the ‘CpuTime’ and ‘Requests’ metric data points).</span></span> <span data-ttu-id="58f5c-133">Pokud nejsou zadány žádné filtry, vrátí se výchozí metriku.</span><span class="sxs-lookup"><span data-stu-id="58f5c-133">If no filters are specified, the default metric is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="58f5c-134">Pro načtení metriky hodnoty pomocí rozhraní REST API Azure monitorování, použijte "2016-06-01" jako verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="58f5c-134">To retrieve metric values using the Azure Monitor REST API, use "2016-06-01" as the API version.</span></span>
>
>

<span data-ttu-id="58f5c-135">**Metoda**: získání</span><span class="sxs-lookup"><span data-stu-id="58f5c-135">**Method**: GET</span></span>

<span data-ttu-id="58f5c-136">**Identifikátor URI požadavku je**: https://management.azure.com/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/*{-– obor názvů zprostředkovatele prostředků}*/*{typ prostředku}*/*{název prostředku}*/providers/microsoft.insights/metrics?$filter=*{filtru}*& verze api-version =*{apiVersion}*</span><span class="sxs-lookup"><span data-stu-id="58f5c-136">**Request URI**: https://management.azure.com/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?$filter=*{filter}*&api-version=*{apiVersion}*</span></span>

<span data-ttu-id="58f5c-137">Například pokud chcete načíst RunsSucceeded metriky datových bodů pro dané časové rozmezí a časovým intervalem 1 hodina, žádost vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="58f5c-137">For example, to retrieve the RunsSucceeded metric data points for the given time range and for a time grain of 1 hour, the request would be as follows:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

<span data-ttu-id="58f5c-138">Výsledek vypadat podobně jako v příkladu následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="58f5c-138">The result would appear similar to the example following screenshot:</span></span>

![ALT "Odpověď JSON zobrazuje průměrný čas odezvy metriky hodnota"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

<span data-ttu-id="58f5c-140">Načtení více bodů data nebo agregace, přidejte metriky definice názvy a typy agregace filtru, jak je vidět v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="58f5c-140">To retrieve multiple data or aggregation points, add the metric definition names and aggregation types to the filter, as seen in the following example:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a><span data-ttu-id="58f5c-141">Použití ARMClient</span><span class="sxs-lookup"><span data-stu-id="58f5c-141">Use ARMClient</span></span>
<span data-ttu-id="58f5c-142">Alternativu k použití prostředí PowerShell (jak je uvedeno výše), je použití [ARMClient](https://github.com/projectkudu/ARMClient) na počítač se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="58f5c-142">An alternative to using PowerShell (as shown above), is to use [ARMClient](https://github.com/projectkudu/ARMClient) on your Windows machine.</span></span> <span data-ttu-id="58f5c-143">Automaticky ARMClient zpracovává ověřování Azure AD (a výsledný token JWT).</span><span class="sxs-lookup"><span data-stu-id="58f5c-143">ARMClient handles the Azure AD authentication (and resulting JWT token) automatically.</span></span> <span data-ttu-id="58f5c-144">Následující kroky popisují použití ARMClient pro načítání metriky dat:</span><span class="sxs-lookup"><span data-stu-id="58f5c-144">The following steps outline use of ARMClient for retrieving metric data:</span></span>

1. <span data-ttu-id="58f5c-145">Nainstalujte [Chocolatey](https://chocolatey.org/) a [ARMClient](https://github.com/projectkudu/ARMClient).</span><span class="sxs-lookup"><span data-stu-id="58f5c-145">Install [Chocolatey](https://chocolatey.org/) and [ARMClient](https://github.com/projectkudu/ARMClient).</span></span>
2. <span data-ttu-id="58f5c-146">Okno terminálu, zadejte *armclient.exe přihlášení*.</span><span class="sxs-lookup"><span data-stu-id="58f5c-146">In a terminal window, type *armclient.exe login*.</span></span> <span data-ttu-id="58f5c-147">Výzva k přihlášení k Azure.</span><span class="sxs-lookup"><span data-stu-id="58f5c-147">This prompts you to log in to Azure.</span></span>
3. <span data-ttu-id="58f5c-148">Typ *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span><span class="sxs-lookup"><span data-stu-id="58f5c-148">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span></span>
4. <span data-ttu-id="58f5c-149">Typ *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span><span class="sxs-lookup"><span data-stu-id="58f5c-149">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span></span>

![ALT "Pomocí ARMClient pro práci s Azure monitorování REST API"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-the-resource-id"></a><span data-ttu-id="58f5c-151">Načtení ID prostředku</span><span class="sxs-lookup"><span data-stu-id="58f5c-151">Retrieve the Resource ID</span></span>
<span data-ttu-id="58f5c-152">Pomocí rozhraní REST API skutečně pomáhá pochopit dostupné definice metrik, členitosti a souvisejících hodnot.</span><span class="sxs-lookup"><span data-stu-id="58f5c-152">Using the REST API can really help to understand the available metric definitions, granularity, and related values.</span></span> <span data-ttu-id="58f5c-153">Informace jsou užitečné při použití [Knihovna správy Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="58f5c-153">That information is helpful when using the [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>

<span data-ttu-id="58f5c-154">ID prostředku, který má používat pro předchozí kód je úplná cesta k požadované prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="58f5c-154">For the preceding code, the resource ID to use is the full path to the desired Azure resource.</span></span> <span data-ttu-id="58f5c-155">Například k dotazování proti webové aplikace Azure, bude ID prostředku:</span><span class="sxs-lookup"><span data-stu-id="58f5c-155">For example, to query against an Azure Web App, the resource ID would be:</span></span>

<span data-ttu-id="58f5c-156">*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-Group-Name}/providers/Microsoft.Web/Sites/{Site-Name}/*</span><span class="sxs-lookup"><span data-stu-id="58f5c-156">*/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{site-name}/*</span></span>

<span data-ttu-id="58f5c-157">Následující seznam obsahuje několik příkladů formáty ID prostředku pro různé prostředky Azure:</span><span class="sxs-lookup"><span data-stu-id="58f5c-157">The following list contains a few examples of resource ID formats for various Azure resources:</span></span>

* <span data-ttu-id="58f5c-158">**IoT Hub** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*</span><span class="sxs-lookup"><span data-stu-id="58f5c-158">**IoT Hub** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*</span></span>
* <span data-ttu-id="58f5c-159">**Elastický fond SQL** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Sql/servers/*{fondu db}*/elasticpools/*{sql název fondu}*</span><span class="sxs-lookup"><span data-stu-id="58f5c-159">**Elastic SQL Pool** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*</span></span>
* <span data-ttu-id="58f5c-160">**Databáze SQL (v12)** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Sql/servers/*{název serveru}*/databases/*{název databáze}*</span><span class="sxs-lookup"><span data-stu-id="58f5c-160">**SQL Database (v12)** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{server-name}*/databases/*{database-name}*</span></span>
* <span data-ttu-id="58f5c-161">**Service Bus** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.ServiceBus/*{obor názvů}*/*{servicebus-name}*</span><span class="sxs-lookup"><span data-stu-id="58f5c-161">**Service Bus** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.ServiceBus/*{namespace}*/*{servicebus-name}*</span></span>
* <span data-ttu-id="58f5c-162">**Škálovatelné sady virtuálních počítačů** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{název virtuálního počítače}*</span><span class="sxs-lookup"><span data-stu-id="58f5c-162">**VM Scale Sets** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{vm-name}*</span></span>
* <span data-ttu-id="58f5c-163">**Virtuální počítače** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Compute/virtualMachines/*{název virtuálního počítače}*</span><span class="sxs-lookup"><span data-stu-id="58f5c-163">**VMs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*</span></span>
* <span data-ttu-id="58f5c-164">**Služba Event Hubs** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*</span><span class="sxs-lookup"><span data-stu-id="58f5c-164">**Event Hubs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*</span></span>

<span data-ttu-id="58f5c-165">Načítání ID prostředku, včetně použití Průzkumníka prostředků Azure, zobrazení požadovaný prostředek na portálu Azure a pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure alternativní způsoby.</span><span class="sxs-lookup"><span data-stu-id="58f5c-165">There are alternative approaches to retrieving the resource ID, including using Azure Resource Explorer, viewing the desired resource in the Azure portal, and via PowerShell or the Azure CLI.</span></span>

### <a name="azure-resource-explorer"></a><span data-ttu-id="58f5c-166">Průzkumník prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="58f5c-166">Azure Resource Explorer</span></span>
<span data-ttu-id="58f5c-167">K vyhledání ID prostředku pro požadovaný prostředek, je užitečné jeden ze způsobů použití [Průzkumníka prostředků Azure](https://resources.azure.com) nástroj.</span><span class="sxs-lookup"><span data-stu-id="58f5c-167">To find the resource ID for a desired resource, one helpful approach is to use the [Azure Resource Explorer](https://resources.azure.com) tool.</span></span> <span data-ttu-id="58f5c-168">Přejděte na požadovaný prostředek a podívejte se na zobrazené ID, stejně jako na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="58f5c-168">Navigate to the desired resource and then look at the ID shown, as in the following screenshot:</span></span>

![ALT "Průzkumníka prostředků Azure"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a><span data-ttu-id="58f5c-170">portál Azure</span><span class="sxs-lookup"><span data-stu-id="58f5c-170">Azure portal</span></span>
<span data-ttu-id="58f5c-171">ID prostředku můžete získat taky z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="58f5c-171">The resource ID can also be obtained from the Azure portal.</span></span> <span data-ttu-id="58f5c-172">Uděláte to tak, přejděte na požadovaný prostředek a potom vyberte možnost Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="58f5c-172">To do so, navigate to the desired resource and then select Properties.</span></span> <span data-ttu-id="58f5c-173">ID prostředku se zobrazí v okně vlastností, jak je vidět na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="58f5c-173">The Resource ID is displayed in the Properties blade, as seen in the following screenshot:</span></span>

![ALT "ID prostředku zobrazí v okně vlastností na portálu Azure"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a><span data-ttu-id="58f5c-175">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="58f5c-175">Azure PowerShell</span></span>
<span data-ttu-id="58f5c-176">ID prostředku můžete načíst pomocí rutin prostředí Azure PowerShell také.</span><span class="sxs-lookup"><span data-stu-id="58f5c-176">The resource ID can be retrieved using Azure PowerShell cmdlets as well.</span></span> <span data-ttu-id="58f5c-177">Například pokud chcete získat ID prostředku pro webové aplikace Azure, spusťte rutinu Get-AzureRmWebApp, stejně jako na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="58f5c-177">For example, to obtain the resource ID for an Azure Web App, execute the Get-AzureRmWebApp cmdlet, as in the following screenshot:</span></span>

![ALT "ID prostředku získat pomocí prostředí PowerShell"](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a><span data-ttu-id="58f5c-179">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="58f5c-179">Azure CLI</span></span>
<span data-ttu-id="58f5c-180">Načíst ID prostředků pomocí Azure CLI, spusťte příkaz 'azure webapp zobrazit', určení ' – json, možnost, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="58f5c-180">To retrieve the resource ID using the Azure CLI, execute the 'azure webapp show' command, specifying the '--json' option, as shown in the following screenshot:</span></span>

![ALT "ID prostředku získat pomocí prostředí PowerShell"](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a><span data-ttu-id="58f5c-182">Načíst Data protokolu aktivit</span><span class="sxs-lookup"><span data-stu-id="58f5c-182">Retrieve Activity Log Data</span></span>
<span data-ttu-id="58f5c-183">Kromě práci s definice metrik a souvisejících hodnot, je také možné načíst další zajímavé přehledy související s prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="58f5c-183">In addition to working with metric definitions and related values, it is also possible to retrieve additional interesting insights related to Azure resources.</span></span> <span data-ttu-id="58f5c-184">Jako příklad, je možné dotazu [protokol aktivit](https://msdn.microsoft.com/library/azure/dn931934.aspx) data.</span><span class="sxs-lookup"><span data-stu-id="58f5c-184">As an example, it is possible to query [activity log](https://msdn.microsoft.com/library/azure/dn931934.aspx) data.</span></span> <span data-ttu-id="58f5c-185">Následující příklad ukazuje, pomocí REST API pro monitorování Azure pro data protokolu aktivit dotazu v určité datum rozsahu pro předplatné Azure:</span><span class="sxs-lookup"><span data-stu-id="58f5c-185">The following sample demonstrates using the Azure Monitor REST API to query activity log data within a specific date range for an Azure subscription:</span></span>

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a><span data-ttu-id="58f5c-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="58f5c-186">Next steps</span></span>
* <span data-ttu-id="58f5c-187">Zkontrolujte [Přehled monitorování](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="58f5c-187">Review the [Overview of Monitoring](monitoring-overview.md).</span></span>
* <span data-ttu-id="58f5c-188">Zobrazení [podporované metriky s Azure monitorování](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="58f5c-188">View the [Supported metrics with Azure Monitor](monitoring-supported-metrics.md).</span></span>
* <span data-ttu-id="58f5c-189">Zkontrolujte [Microsoft Azure monitorovat referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="58f5c-189">Review the [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>
* <span data-ttu-id="58f5c-190">Zkontrolujte [knihovna Azure správy](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="58f5c-190">Review the [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>
