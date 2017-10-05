---
title: "Řešení potíží s brány virtuální sítě a připojení pomocí sledovací proces sítě Azure - REST | Microsoft Docs"
description: "Tato stránka vysvětluje postup řešení potíží s brány virtuální sítě a připojení s sledovací proces sítě Azure pomocí REST"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e4d5f195-b839-4394-94ef-a04192766e55
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: bc61be74d85a309c158716460b918baaf4fa94dc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a><span data-ttu-id="1742f-103">Řešení potíží s Brána virtuální sítě a připojení pomocí sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="1742f-103">Troubleshoot Virtual Network gateway and Connections using Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="1742f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1742f-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="1742f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1742f-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="1742f-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1742f-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="1742f-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1742f-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="1742f-108">REST API</span><span class="sxs-lookup"><span data-stu-id="1742f-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="1742f-109">Sledovací proces sítě obsahuje řadu funkcí ve vztahu k pochopení síťovým prostředkům v Azure.</span><span class="sxs-lookup"><span data-stu-id="1742f-109">Network Watcher provides many capabilities as it relates to understanding your network resources in Azure.</span></span> <span data-ttu-id="1742f-110">Jeden z těchto funkcí je prostředek řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="1742f-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="1742f-111">Řešení potíží s prostředků je možné volat prostřednictvím portálu, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="1742f-111">Resource troubleshooting can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="1742f-112">Při volání, sledovací proces sítě kontroluje stav bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.</span><span class="sxs-lookup"><span data-stu-id="1742f-112">When called, Network Watcher inspects the health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="1742f-113">Tento článek vás provede úloh jiný správy, které jsou aktuálně dostupné pro řešení potíží s prostředků.</span><span class="sxs-lookup"><span data-stu-id="1742f-113">This article takes you through the different management tasks that are currently available for resource troubleshooting.</span></span>

- [<span data-ttu-id="1742f-114">**Řešení potíží s bránu virtuální sítě**</span><span class="sxs-lookup"><span data-stu-id="1742f-114">**Troubleshoot a Virtual Network gateway**</span></span>](#troubleshoot-a-virtual-network-gateway)
- [<span data-ttu-id="1742f-115">**Vyřešte potíže připojením**</span><span class="sxs-lookup"><span data-stu-id="1742f-115">**Troubleshoot a Connection**</span></span>](#troubleshoot-connections)

## <a name="before-you-begin"></a><span data-ttu-id="1742f-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1742f-116">Before you begin</span></span>

<span data-ttu-id="1742f-117">ARMclient se používá k volání rozhraní REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1742f-117">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="1742f-118">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="1742f-118">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="1742f-119">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="1742f-119">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

<span data-ttu-id="1742f-120">Seznam podporovaných brány typy návštěvě [typy podporované brány](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="1742f-120">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="1742f-121">Přehled</span><span class="sxs-lookup"><span data-stu-id="1742f-121">Overview</span></span>

<span data-ttu-id="1742f-122">Řešení potíží s sledovací proces sítě poskytuje možnost odstraňování problémů, které nastat u brány virtuální sítě a připojení.</span><span class="sxs-lookup"><span data-stu-id="1742f-122">Network Watcher troubleshooting provides the ability troubleshoot issues that arise with Virtual Network gateways and Connections.</span></span> <span data-ttu-id="1742f-123">Po odeslání žádosti k prostředku, řešení potíží s protokoly jsou dotazování a prověřovány.</span><span class="sxs-lookup"><span data-stu-id="1742f-123">When a request is made to the resource troubleshooting, logs are querying and inspected.</span></span> <span data-ttu-id="1742f-124">Po dokončení kontroly budou vráceny výsledky.</span><span class="sxs-lookup"><span data-stu-id="1742f-124">When inspection is complete, the results are returned.</span></span> <span data-ttu-id="1742f-125">Poradce při potížích rozhraní API žádosti, které jsou dlouhé systémem požadavků, které může trvat několik minut vrácení výsledku.</span><span class="sxs-lookup"><span data-stu-id="1742f-125">The troubleshoot API requests are long running requests, which could take multiple minutes to return a result.</span></span> <span data-ttu-id="1742f-126">Protokoly se ukládají v kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="1742f-126">Logs are stored in a container on a storage account.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="1742f-127">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="1742f-127">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a><span data-ttu-id="1742f-128">Řešení potíží s bránu virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1742f-128">Troubleshoot a Virtual Network gateway</span></span>


### <a name="post-the-troubleshoot-request"></a><span data-ttu-id="1742f-129">Odeslat požadavek Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="1742f-129">POST the troubleshoot request</span></span>

<span data-ttu-id="1742f-130">Následující příklad dotazuje se na stav brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1742f-130">The following example queries the status of a Virtual Network gateway.</span></span>

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$vnetGatewayName = "ContosoVNETGateway"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @"
{
'TargetResourceId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/virtualNetworkGateways/${vnetGatewayName}',
'Properties': {
'StorageId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}',
'StoragePath': 'https://${storageAccountName}.blob.core.windows.net/${containerName}'
}
}
"@

}
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 "
```

<span data-ttu-id="1742f-131">Vzhledem k tomu, že tato operace je dlouho spuštěný, identifikátor URI pro dotazování operaci a identifikátor URI pro výsledek je vrácená v hlavičce odpovědi, jak je znázorněno v následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="1742f-131">Since this operation is long running, the URI for querying the operation and the URI for the result is returned in the response header as shown in the following response:</span></span>

<span data-ttu-id="1742f-132">**Důležité hodnoty**</span><span class="sxs-lookup"><span data-stu-id="1742f-132">**Important Values**</span></span>

* <span data-ttu-id="1742f-133">**Azure AsyncOperation** -tato vlastnost obsahuje identifikátor URI pro dotaz Async řešení operace</span><span class="sxs-lookup"><span data-stu-id="1742f-133">**Azure-AsyncOperation** - This property contains the URI to query the Async troubleshoot operation</span></span>
* <span data-ttu-id="1742f-134">**Umístění** -tato vlastnost obsahuje identifikátor URI, kde jsou výsledky při dokončení operace</span><span class="sxs-lookup"><span data-stu-id="1742f-134">**Location** - This property contains the URI where the results are when the operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-the-async-operation-for-completion"></a><span data-ttu-id="1742f-135">Dotaz pro dokončení asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="1742f-135">Query the async operation for completion</span></span>

<span data-ttu-id="1742f-136">Použijte operace URI dotazu pro průběh operace, jak je vidět v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1742f-136">Use the operations URI to query for the progress of the operation as seen in the following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="1742f-137">Když probíhá operace, zobrazí odpověď **InProgress** jak je vidět v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1742f-137">While the operation is in progress, the response shows **InProgress** as seen in the following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="1742f-138">Po dokončení operace se stav změní na **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="1742f-138">When the operation is complete the status changes to **Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-the-results"></a><span data-ttu-id="1742f-139">Načíst výsledky</span><span class="sxs-lookup"><span data-stu-id="1742f-139">Retrieve the results</span></span>

<span data-ttu-id="1742f-140">Jakmile se stav vrátí je **úspěšné**, volání metody GET na výsledek URI načíst výsledky.</span><span class="sxs-lookup"><span data-stu-id="1742f-140">Once the status returned is **Succeeded**, call a GET Method on the operationResult URI to retrieve the results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="1742f-141">Příklady typických sníženou odpověď vrácená při dotazování na výsledky řešení potíží s bránu, jsou tyto odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1742f-141">The following responses are examples of a typical degraded response returned when querying the results of troubleshooting a gateway.</span></span> <span data-ttu-id="1742f-142">V tématu [seznámení s výsledky](#understanding-the-results) získat vysvětlení na významu vlastnosti v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1742f-142">See [Understanding the results](#understanding-the-results) to get clarification on what the properties in the response mean.</span></span>

```json
{
  "startTime": "2017-01-12T10:31:41.562646-08:00",
  "endTime": "2017-01-12T18:31:48.677Z",
  "code": "Degraded",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN Gateway"
        },
        {
          "actionText": "If your VPN gateway isn't up and running by the expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN gateway is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```


## <a name="troubleshoot-connections"></a><span data-ttu-id="1742f-143">Odstraňování potíží s připojením</span><span class="sxs-lookup"><span data-stu-id="1742f-143">Troubleshoot Connections</span></span>

<span data-ttu-id="1742f-144">V následujícím příkladu se dotazuje stav připojení.</span><span class="sxs-lookup"><span data-stu-id="1742f-144">The following example queries the status of a Connection.</span></span>

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$connectionName = "VNET2toVNET1Connection"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @{
"TargetResourceId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/connections/${connectionName}",
"Properties": {
"StorageId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}",
"StoragePath": "https://${storageAccountName}.blob.core.windows.net/${containerName}"
}

}
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 "
```

> [!NOTE]
> <span data-ttu-id="1742f-145">Poradce při potížích operaci nelze spustit souběžně na jeho odpovídající brány a připojení.</span><span class="sxs-lookup"><span data-stu-id="1742f-145">The troubleshoot operation cannot be run in parallel on a Connection and its corresponding gateways.</span></span> <span data-ttu-id="1742f-146">Operaci musíte dokončit před spuštěním na dřívější prostředek.</span><span class="sxs-lookup"><span data-stu-id="1742f-146">The operation must complete prior to running it on the previous resource.</span></span>

<span data-ttu-id="1742f-147">Vzhledem k tomu, že toto je dlouhotrvající transakci, v hlavičce odpověď se vrátí identifikátor URI pro dotazování operaci a identifikátor URI pro výsledek, jak je znázorněno v následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="1742f-147">Since this is a long running transaction, in the response header, the URI for querying the operation and the URI for the result is returned as shown in the following response:</span></span>

<span data-ttu-id="1742f-148">**Důležité hodnoty**</span><span class="sxs-lookup"><span data-stu-id="1742f-148">**Important Values**</span></span>

* <span data-ttu-id="1742f-149">**Azure AsyncOperation** -tato vlastnost obsahuje identifikátor URI pro dotaz Async řešení operace</span><span class="sxs-lookup"><span data-stu-id="1742f-149">**Azure-AsyncOperation** - This property contains the URI to query the Async troubleshoot operation</span></span>
* <span data-ttu-id="1742f-150">**Umístění** -tato vlastnost obsahuje identifikátor URI, kde jsou výsledky při dokončení operace</span><span class="sxs-lookup"><span data-stu-id="1742f-150">**Location** - This property contains the URI where the results are when the operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-the-async-operation-for-completion"></a><span data-ttu-id="1742f-151">Dotaz pro dokončení asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="1742f-151">Query the async operation for completion</span></span>

<span data-ttu-id="1742f-152">Použijte operace URI dotazu pro průběh operace, jak je vidět v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1742f-152">Use the operations URI to query for the progress of the operation as seen in the following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="1742f-153">Když probíhá operace, zobrazí odpověď **InProgress** jak je vidět v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1742f-153">While the operation is in progress, the response shows **InProgress** as seen in the following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="1742f-154">Po dokončení operace se stav změní na **úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="1742f-154">When the operation is complete, the status changes to **Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

<span data-ttu-id="1742f-155">Příklady typických odpověď vrácená při dotazování na výsledky řešení potíží s připojení, jsou tyto odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1742f-155">The following responses are examples of a typical response returned when querying the results of troubleshooting a Connection.</span></span>

### <a name="retrieve-the-results"></a><span data-ttu-id="1742f-156">Načíst výsledky</span><span class="sxs-lookup"><span data-stu-id="1742f-156">Retrieve the results</span></span>

<span data-ttu-id="1742f-157">Jakmile se stav vrátí je **úspěšné**, volání metody GET na výsledek URI načíst výsledky.</span><span class="sxs-lookup"><span data-stu-id="1742f-157">Once the status returned is **Succeeded**, call a GET Method on the operationResult URI to retrieve the results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="1742f-158">Příklady typických odpověď vrácená při dotazování na výsledky řešení potíží s připojení, jsou tyto odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1742f-158">The following responses are examples of a typical response returned when querying the results of troubleshooting a Connection.</span></span>

```json
{
  "startTime": "2017-01-12T14:09:19.1215346-08:00",
  "endTime": "2017-01-12T22:09:23.747Z",
  "code": "UnHealthy",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This 
is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN gateway"
        },
        {
          "actionText": "If your VPN Connection isn't up and running by the expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN Connection is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```

## <a name="understanding-the-results"></a><span data-ttu-id="1742f-159">Seznámení s výsledky</span><span class="sxs-lookup"><span data-stu-id="1742f-159">Understanding the results</span></span>

<span data-ttu-id="1742f-160">Text akce obsahuje obecné pokyny k vyřešení problému.</span><span class="sxs-lookup"><span data-stu-id="1742f-160">The action text provides general guidance on how to resolve the issue.</span></span> <span data-ttu-id="1742f-161">Pokud může být akce pro tento problém, je k dispozici odkaz s další pokyny.</span><span class="sxs-lookup"><span data-stu-id="1742f-161">If an action can be taken for the issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="1742f-162">V případě, že tam, kde není žádná další pokyny, odpověď obsahuje adresu url pro otevření případu podpory.</span><span class="sxs-lookup"><span data-stu-id="1742f-162">In the case where there is no additional guidance, the response provides the url to open a support case.</span></span>  <span data-ttu-id="1742f-163">Další informace o vlastnostech odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="1742f-163">For more information about the properties of the response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="1742f-164">Pokyny ke stahování souborů z účty azure storage, najdete v části [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="1742f-164">For instructions on downloading files from azure storage accounts, refer to [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="1742f-165">Jiný nástroj, který je možné je Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="1742f-165">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="1742f-166">Další informace o Storage Explorer naleznete zde na následující odkaz: [Storage Explorer](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="1742f-166">More information about Storage Explorer can be found here at the following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="1742f-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1742f-167">Next steps</span></span>

<span data-ttu-id="1742f-168">Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) sledovat pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.</span><span class="sxs-lookup"><span data-stu-id="1742f-168">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that may be in question.</span></span>
