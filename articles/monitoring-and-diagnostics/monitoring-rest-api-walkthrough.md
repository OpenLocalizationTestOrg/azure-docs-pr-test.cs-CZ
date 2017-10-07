---
title: "aaaAzure návod monitorování REST API | Microsoft Docs"
description: "Jak tooauthenticate požadavků tooand použití hello monitorování REST API služby Azure."
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
ms.openlocfilehash: b8ae3a03fd21af872f1dc5fed40a101a24ca1652
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitoring-rest-api-walkthrough"></a><span data-ttu-id="e959c-103">Monitorování návod rozhraní API REST Azure</span><span class="sxs-lookup"><span data-stu-id="e959c-103">Azure Monitoring REST API Walkthrough</span></span>
<span data-ttu-id="e959c-104">Tento článek ukazuje, jak tooperform ověřování, aby váš kód můžete použít hello [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="e959c-104">This article shows you how tooperform authentication so your code can use hello [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>         

<span data-ttu-id="e959c-105">Hello rozhraní API služby Azure monitorování je možné tooprogrammatically načtení hello dostupná výchozí definice metrik (hello Typ Metrika například čas procesoru, požadavků, atd.), členitosti a metriky hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e959c-105">hello Azure Monitor API makes it possible tooprogrammatically retrieve hello available default metric definitions (hello type of metric such as CPU Time, Requests, etc.), granularity, and metric values.</span></span> <span data-ttu-id="e959c-106">Jakmile načíst, mohou být hello data uložena v úložišti dat samostatné například Azure SQL Database, Azure Cosmos DB nebo Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="e959c-106">Once retrieved, hello data can be saved in a separate data store such as Azure SQL Database, Azure Cosmos DB, or Azure Data Lake.</span></span> <span data-ttu-id="e959c-107">Odtud můžete provést další analýzu podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="e959c-107">From there additional analysis can be performed as needed.</span></span>

<span data-ttu-id="e959c-108">Kromě práce s různými metriky datové body, jako tento článek ukazuje, hello monitorování API je možné toolist pravidla výstrah, zobrazení protokoly aktivity a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="e959c-108">Besides working with various metric data points, as this article demonstrates, hello Monitor API makes it possible toolist alert rules, view activity logs, and much more.</span></span> <span data-ttu-id="e959c-109">Úplný seznam dostupné operace, najdete v části hello [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="e959c-109">For a full list of available operations, see hello [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>

## <a name="authenticating-azure-monitor-requests"></a><span data-ttu-id="e959c-110">Ověřování Azure sledování požadavků</span><span class="sxs-lookup"><span data-stu-id="e959c-110">Authenticating Azure Monitor Requests</span></span>
<span data-ttu-id="e959c-111">prvním krokem Hello je tooauthenticate hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="e959c-111">hello first step is tooauthenticate hello request.</span></span>

<span data-ttu-id="e959c-112">Všechny úlohy hello prováděné vůči hello rozhraní API služby Azure monitorování použití hello Azure Resource Manager ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="e959c-112">All hello tasks executed against hello Azure Monitor API use hello Azure Resource Manager authentication model.</span></span> <span data-ttu-id="e959c-113">Proto musí být ověřeny všechny požadavky s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e959c-113">Therefore, all requests must be authenticated with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="e959c-114">Jeden způsob tooauthenticate hello klientská aplikace je toocreate objektu služby Azure AD a získat token hello ověřování (JWT).</span><span class="sxs-lookup"><span data-stu-id="e959c-114">One approach tooauthenticate hello client application is toocreate an Azure AD service principal and retrieve hello authentication (JWT) token.</span></span> <span data-ttu-id="e959c-115">Hello následující ukázkový skript ukazuje vytvoření objektu služby pomocí prostředí PowerShell Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e959c-115">hello following sample script demonstrates creating an Azure AD service principal via PowerShell.</span></span> <span data-ttu-id="e959c-116">Pro podrobnější návod, naleznete v dokumentaci toohello na [pomocí prostředí Azure PowerShell toocreate prostředky služby hlavní tooaccess](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span><span class="sxs-lookup"><span data-stu-id="e959c-116">For a more detailed walk-through, refer toohello documentation on [using Azure PowerShell toocreate a service principal tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span></span> <span data-ttu-id="e959c-117">Je také možné příliš[vytvořit objekt služby prostřednictvím portálu Azure hello](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e959c-117">It is also possible too[create a service principal via hello Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate tooa specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for hello service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with hello designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role toohello newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

<span data-ttu-id="e959c-118">hello tooquery rozhraní API Azure monitorování, klientská aplikace hello měli používat hello vytvořili tooauthenticate hlavní služby.</span><span class="sxs-lookup"><span data-stu-id="e959c-118">tooquery hello Azure Monitor API, hello client application should use hello previously created service principal tooauthenticate.</span></span> <span data-ttu-id="e959c-119">Hello následující skript prostředí PowerShell příklad ukazuje jeden ze způsobů, pomocí hello [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) toohelp získání tokenu ověřování tokenů JWT hello.</span><span class="sxs-lookup"><span data-stu-id="e959c-119">hello following example PowerShell script shows one approach, using hello [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) toohelp get hello JWT authentication token.</span></span> <span data-ttu-id="e959c-120">Hello JWT token se předal v rámci parametru HTTP autorizace v toohello požadavky REST API služby Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="e959c-120">hello JWT token is passed as part of an HTTP Authorization parameter in requests toohello Azure Monitor REST API.</span></span>

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

<span data-ttu-id="e959c-121">Po dokončení kroku hello ověřování dotazy mohou být pak provedeny proti hello REST API služby Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="e959c-121">Once hello authentication setup step is complete, queries can then be executed against hello Azure Monitor REST API.</span></span> <span data-ttu-id="e959c-122">Existují dva užitečné dotazy:</span><span class="sxs-lookup"><span data-stu-id="e959c-122">There are two helpful queries:</span></span>

1. <span data-ttu-id="e959c-123">Seznam hello Definice metrik pro prostředek</span><span class="sxs-lookup"><span data-stu-id="e959c-123">List hello metric definitions for a resource</span></span>
2. <span data-ttu-id="e959c-124">Načíst hodnoty metriky hello</span><span class="sxs-lookup"><span data-stu-id="e959c-124">Retrieve hello metric values</span></span>

## <a name="retrieve-metric-definitions"></a><span data-ttu-id="e959c-125">Načtení definice metrik</span><span class="sxs-lookup"><span data-stu-id="e959c-125">Retrieve Metric Definitions</span></span>
> [!NOTE]
> <span data-ttu-id="e959c-126">definice metrik tooretrieve pomocí hello REST API služby Azure monitorování, použijte "2016-03-01" hello verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e959c-126">tooretrieve metric definitions using hello Azure Monitor REST API, use "2016-03-01" as hello API version.</span></span>
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
<span data-ttu-id="e959c-127">Pro aplikace logiky Azure objeví definice metrik hello podobné toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e959c-127">For an Azure Logic App, hello metric definitions would appear similar toohello following screenshot:</span></span>

![ALT "JSON zobrazení metriky definice odpovědi."](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

<span data-ttu-id="e959c-129">Další informace najdete v tématu hello [seznamu hello Definice metrik pro prostředek v rozhraní REST API Azure monitorování](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e959c-129">For more information, see hello [List hello metric definitions for a resource in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) documentation.</span></span>

## <a name="retrieve-metric-values"></a><span data-ttu-id="e959c-130">Načtení metriky hodnot</span><span class="sxs-lookup"><span data-stu-id="e959c-130">Retrieve Metric Values</span></span>
<span data-ttu-id="e959c-131">K dispozici definice metrik hello známé, je možné tooretrieve hello souvisejících hodnot metriky.</span><span class="sxs-lookup"><span data-stu-id="e959c-131">Once hello available metric definitions are known, it is then possible tooretrieve hello related metric values.</span></span> <span data-ttu-id="e959c-132">Použijte název hello metrika 'Hodnota' (ne hello 'localizedValue') pro všechny filtrování požadavků (například načtení hello 'CpuTime' a 'Požadavky' metriky datové body).</span><span class="sxs-lookup"><span data-stu-id="e959c-132">Use hello metric’s name ‘value’ (not hello ‘localizedValue’) for any filtering requests (for example, retrieve hello ‘CpuTime’ and ‘Requests’ metric data points).</span></span> <span data-ttu-id="e959c-133">Pokud nejsou zadány žádné filtry, vrátí se hello výchozí metriku.</span><span class="sxs-lookup"><span data-stu-id="e959c-133">If no filters are specified, hello default metric is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="e959c-134">hodnoty metriky tooretrieve pomocí hello REST API služby Azure monitorování, použijte "2016-06-01" hello verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e959c-134">tooretrieve metric values using hello Azure Monitor REST API, use "2016-06-01" as hello API version.</span></span>
>
>

<span data-ttu-id="e959c-135">**Metoda**: získání</span><span class="sxs-lookup"><span data-stu-id="e959c-135">**Method**: GET</span></span>

<span data-ttu-id="e959c-136">**Identifikátor URI požadavku je**: https://management.azure.com/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/*{-– obor názvů zprostředkovatele prostředků}*/*{typ prostředku}*/*{název prostředku}*/providers/microsoft.insights/metrics?$filter=*{filtru}*& verze api-version =*{apiVersion}*</span><span class="sxs-lookup"><span data-stu-id="e959c-136">**Request URI**: https://management.azure.com/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?$filter=*{filter}*&api-version=*{apiVersion}*</span></span>

<span data-ttu-id="e959c-137">Například tooretrieve hello RunsSucceeded metriky datových bodů pro hello zadaný časový rozsah a časovým intervalem 1 hodina, žádost hello vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="e959c-137">For example, tooretrieve hello RunsSucceeded metric data points for hello given time range and for a time grain of 1 hour, hello request would be as follows:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

<span data-ttu-id="e959c-138">výsledek Hello vypadat podobně jako příklad toohello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e959c-138">hello result would appear similar toohello example following screenshot:</span></span>

![ALT "Odpověď JSON zobrazuje průměrný čas odezvy metriky hodnota"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

<span data-ttu-id="e959c-140">tooretrieve více dat nebo agregace body, přidejte hello metriky definice názvy a agregace typy toohello filtr, jak je vidět v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="e959c-140">tooretrieve multiple data or aggregation points, add hello metric definition names and aggregation types toohello filter, as seen in hello following example:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a><span data-ttu-id="e959c-141">Použití ARMClient</span><span class="sxs-lookup"><span data-stu-id="e959c-141">Use ARMClient</span></span>
<span data-ttu-id="e959c-142">Alternativní toousing prostředí PowerShell (jak je uvedeno výše), je toouse [ARMClient](https://github.com/projectkudu/ARMClient) na počítač se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="e959c-142">An alternative toousing PowerShell (as shown above), is toouse [ARMClient](https://github.com/projectkudu/ARMClient) on your Windows machine.</span></span> <span data-ttu-id="e959c-143">Automaticky ARMClient zpracovává ověřování hello Azure AD (a výsledný token JWT).</span><span class="sxs-lookup"><span data-stu-id="e959c-143">ARMClient handles hello Azure AD authentication (and resulting JWT token) automatically.</span></span> <span data-ttu-id="e959c-144">Hello následující kroky popisují použití ARMClient pro načítání metriky dat:</span><span class="sxs-lookup"><span data-stu-id="e959c-144">hello following steps outline use of ARMClient for retrieving metric data:</span></span>

1. <span data-ttu-id="e959c-145">Nainstalujte [Chocolatey](https://chocolatey.org/) a [ARMClient](https://github.com/projectkudu/ARMClient).</span><span class="sxs-lookup"><span data-stu-id="e959c-145">Install [Chocolatey](https://chocolatey.org/) and [ARMClient](https://github.com/projectkudu/ARMClient).</span></span>
2. <span data-ttu-id="e959c-146">Okno terminálu, zadejte *armclient.exe přihlášení*.</span><span class="sxs-lookup"><span data-stu-id="e959c-146">In a terminal window, type *armclient.exe login*.</span></span> <span data-ttu-id="e959c-147">To vás vyzve k toolog v tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e959c-147">This prompts you toolog in tooAzure.</span></span>
3. <span data-ttu-id="e959c-148">Typ *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span><span class="sxs-lookup"><span data-stu-id="e959c-148">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span></span>
4. <span data-ttu-id="e959c-149">Typ *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span><span class="sxs-lookup"><span data-stu-id="e959c-149">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span></span>

![ALT "Pomocí ARMClient toowork s hello rozhraní API REST Azure monitorování"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-hello-resource-id"></a><span data-ttu-id="e959c-151">Načtení hello ID prostředku</span><span class="sxs-lookup"><span data-stu-id="e959c-151">Retrieve hello Resource ID</span></span>
<span data-ttu-id="e959c-152">Použití hello REST API může pomoci skutečně toounderstand hello k dispozici definice metrik, členitosti a souvisejících hodnot.</span><span class="sxs-lookup"><span data-stu-id="e959c-152">Using hello REST API can really help toounderstand hello available metric definitions, granularity, and related values.</span></span> <span data-ttu-id="e959c-153">Tyto informace jsou užitečné při použití hello [Knihovna správy Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="e959c-153">That information is helpful when using hello [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>

<span data-ttu-id="e959c-154">Pro hello předcházející kódu toouse ID prostředku hello je úplná cesta toohello hello požadovaných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e959c-154">For hello preceding code, hello resource ID toouse is hello full path toohello desired Azure resource.</span></span> <span data-ttu-id="e959c-155">Například tooquery proti webové aplikace Azure, ID prostředku hello by být:</span><span class="sxs-lookup"><span data-stu-id="e959c-155">For example, tooquery against an Azure Web App, hello resource ID would be:</span></span>

<span data-ttu-id="e959c-156">*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-Group-Name}/providers/Microsoft.Web/Sites/{Site-Name}/*</span><span class="sxs-lookup"><span data-stu-id="e959c-156">*/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{site-name}/*</span></span>

<span data-ttu-id="e959c-157">Hello následující seznam obsahuje několik příkladů formáty ID prostředku pro různé prostředky Azure:</span><span class="sxs-lookup"><span data-stu-id="e959c-157">hello following list contains a few examples of resource ID formats for various Azure resources:</span></span>

* <span data-ttu-id="e959c-158">**IoT Hub** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*</span><span class="sxs-lookup"><span data-stu-id="e959c-158">**IoT Hub** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*</span></span>
* <span data-ttu-id="e959c-159">**Elastický fond SQL** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Sql/servers/*{fondu db}*/elasticpools/*{sql název fondu}*</span><span class="sxs-lookup"><span data-stu-id="e959c-159">**Elastic SQL Pool** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*</span></span>
* <span data-ttu-id="e959c-160">**Databáze SQL (v12)** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Sql/servers/*{název serveru}*/databases/*{název databáze}*</span><span class="sxs-lookup"><span data-stu-id="e959c-160">**SQL Database (v12)** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{server-name}*/databases/*{database-name}*</span></span>
* <span data-ttu-id="e959c-161">**Service Bus** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.ServiceBus/*{obor názvů}*/*{servicebus-name}*</span><span class="sxs-lookup"><span data-stu-id="e959c-161">**Service Bus** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.ServiceBus/*{namespace}*/*{servicebus-name}*</span></span>
* <span data-ttu-id="e959c-162">**Škálovatelné sady virtuálních počítačů** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{název virtuálního počítače}*</span><span class="sxs-lookup"><span data-stu-id="e959c-162">**VM Scale Sets** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{vm-name}*</span></span>
* <span data-ttu-id="e959c-163">**Virtuální počítače** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Compute/virtualMachines/*{název virtuálního počítače}*</span><span class="sxs-lookup"><span data-stu-id="e959c-163">**VMs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*</span></span>
* <span data-ttu-id="e959c-164">**Služba Event Hubs** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*</span><span class="sxs-lookup"><span data-stu-id="e959c-164">**Event Hubs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*</span></span>

<span data-ttu-id="e959c-165">Nejsou k dispozici alternativní přístupy tooretrieving hello ID prostředku, včetně použití Průzkumníka prostředků Azure, zobrazení hello požadovaného prostředku v hello portál Azure a pomocí prostředí PowerShell nebo hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e959c-165">There are alternative approaches tooretrieving hello resource ID, including using Azure Resource Explorer, viewing hello desired resource in hello Azure portal, and via PowerShell or hello Azure CLI.</span></span>

### <a name="azure-resource-explorer"></a><span data-ttu-id="e959c-166">Průzkumník prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="e959c-166">Azure Resource Explorer</span></span>
<span data-ttu-id="e959c-167">ID prostředku hello toofind pro požadovaný prostředek, jeden ze způsobů užitečné je toouse hello [Průzkumníka prostředků Azure](https://resources.azure.com) nástroj.</span><span class="sxs-lookup"><span data-stu-id="e959c-167">toofind hello resource ID for a desired resource, one helpful approach is toouse hello [Azure Resource Explorer](https://resources.azure.com) tool.</span></span> <span data-ttu-id="e959c-168">Přejděte toohello požadovaných prostředků a podívejte se na ID hello zobrazí jako hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e959c-168">Navigate toohello desired resource and then look at hello ID shown, as in hello following screenshot:</span></span>

![ALT "Průzkumníka prostředků Azure"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a><span data-ttu-id="e959c-170">portál Azure</span><span class="sxs-lookup"><span data-stu-id="e959c-170">Azure portal</span></span>
<span data-ttu-id="e959c-171">ID prostředku Hello můžete získat také hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e959c-171">hello resource ID can also be obtained from hello Azure portal.</span></span> <span data-ttu-id="e959c-172">toodo tedy přejděte toohello požadovaných prostředků a vyberte možnost Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e959c-172">toodo so, navigate toohello desired resource and then select Properties.</span></span> <span data-ttu-id="e959c-173">Hello ID prostředku se zobrazí v okně Vlastnosti hello, jak je vidět v hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e959c-173">hello Resource ID is displayed in hello Properties blade, as seen in hello following screenshot:</span></span>

![ALT "ID prostředku zobrazí v okně Vlastnosti hello v hello portál Azure"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a><span data-ttu-id="e959c-175">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e959c-175">Azure PowerShell</span></span>
<span data-ttu-id="e959c-176">ID prostředku Hello můžete načíst pomocí rutin prostředí Azure PowerShell také.</span><span class="sxs-lookup"><span data-stu-id="e959c-176">hello resource ID can be retrieved using Azure PowerShell cmdlets as well.</span></span> <span data-ttu-id="e959c-177">Například ID prostředku hello tooobtain pro webové aplikace Azure, spusťte rutinu Get-AzureRmWebApp hello jako hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e959c-177">For example, tooobtain hello resource ID for an Azure Web App, execute hello Get-AzureRmWebApp cmdlet, as in hello following screenshot:</span></span>

![ALT "ID prostředku získat pomocí prostředí PowerShell"](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a><span data-ttu-id="e959c-179">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e959c-179">Azure CLI</span></span>
<span data-ttu-id="e959c-180">tooretrieve hello ID prostředku pomocí hello příkazového řádku Azure CLI, spusťte příkaz "azure webapp zobrazit" hello, zadání hello ' – json, možnost, jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="e959c-180">tooretrieve hello resource ID using hello Azure CLI, execute hello 'azure webapp show' command, specifying hello '--json' option, as shown in hello following screenshot:</span></span>

![ALT "ID prostředku získat pomocí prostředí PowerShell"](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a><span data-ttu-id="e959c-182">Načíst Data protokolu aktivit</span><span class="sxs-lookup"><span data-stu-id="e959c-182">Retrieve Activity Log Data</span></span>
<span data-ttu-id="e959c-183">V přidání tooworking s definice metrik a souvisejících hodnot je také možné tooretrieve další zajímavé přehledy související tooAzure prostředky.</span><span class="sxs-lookup"><span data-stu-id="e959c-183">In addition tooworking with metric definitions and related values, it is also possible tooretrieve additional interesting insights related tooAzure resources.</span></span> <span data-ttu-id="e959c-184">Jako příklad, je možné tooquery [protokol aktivit](https://msdn.microsoft.com/library/azure/dn931934.aspx) data.</span><span class="sxs-lookup"><span data-stu-id="e959c-184">As an example, it is possible tooquery [activity log](https://msdn.microsoft.com/library/azure/dn931934.aspx) data.</span></span> <span data-ttu-id="e959c-185">Hello následující příklad ukazuje použití data protokolu hello REST API služby Azure monitorování tooquery aktivit v určité datum rozsahu pro předplatné Azure:</span><span class="sxs-lookup"><span data-stu-id="e959c-185">hello following sample demonstrates using hello Azure Monitor REST API tooquery activity log data within a specific date range for an Azure subscription:</span></span>

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a><span data-ttu-id="e959c-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e959c-186">Next steps</span></span>
* <span data-ttu-id="e959c-187">Zkontrolujte hello [Přehled monitorování](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e959c-187">Review hello [Overview of Monitoring](monitoring-overview.md).</span></span>
* <span data-ttu-id="e959c-188">Zobrazení hello [podporované metriky s Azure monitorování](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="e959c-188">View hello [Supported metrics with Azure Monitor](monitoring-supported-metrics.md).</span></span>
* <span data-ttu-id="e959c-189">Zkontrolujte hello [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="e959c-189">Review hello [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>
* <span data-ttu-id="e959c-190">Zkontrolujte hello [Knihovna správy Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="e959c-190">Review hello [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>
