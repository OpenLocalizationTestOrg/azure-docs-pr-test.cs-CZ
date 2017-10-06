---
title: "aaaTroubleshoot brány virtuální sítě a připojení pomocí sledovací proces sítě Azure - REST | Microsoft Docs"
description: "Tato stránka vysvětluje, jak REST tootroubleshoot brány virtuální sítě a připojení s použitím sledovací proces sítě Azure"
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
ms.openlocfilehash: cc89b46643fdbfefe53727b45d6b7d06914b58a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a><span data-ttu-id="23893-103">Řešení potíží s Brána virtuální sítě a připojení pomocí sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="23893-103">Troubleshoot Virtual Network gateway and Connections using Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="23893-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="23893-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="23893-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="23893-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="23893-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="23893-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="23893-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="23893-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="23893-108">REST API</span><span class="sxs-lookup"><span data-stu-id="23893-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="23893-109">Sledovací proces sítě obsahuje řadu funkcí, protože se týká toounderstanding síťovým prostředkům v Azure.</span><span class="sxs-lookup"><span data-stu-id="23893-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="23893-110">Jeden z těchto funkcí je prostředek řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="23893-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="23893-111">Řešení potíží s prostředků je možné volat prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="23893-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="23893-112">Při volání, sledovací proces sítě kontroluje stav hello bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.</span><span class="sxs-lookup"><span data-stu-id="23893-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="23893-113">Tento článek vás provede úlohy hello různých správy, které jsou aktuálně dostupné pro řešení potíží s prostředků.</span><span class="sxs-lookup"><span data-stu-id="23893-113">This article takes you through hello different management tasks that are currently available for resource troubleshooting.</span></span>

- [<span data-ttu-id="23893-114">**Řešení potíží s bránu virtuální sítě**</span><span class="sxs-lookup"><span data-stu-id="23893-114">**Troubleshoot a Virtual Network gateway**</span></span>](#troubleshoot-a-virtual-network-gateway)
- [<span data-ttu-id="23893-115">**Vyřešte potíže připojením**</span><span class="sxs-lookup"><span data-stu-id="23893-115">**Troubleshoot a Connection**</span></span>](#troubleshoot-connections)

## <a name="before-you-begin"></a><span data-ttu-id="23893-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="23893-116">Before you begin</span></span>

<span data-ttu-id="23893-117">ARMclient je použité toocall hello REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="23893-117">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="23893-118">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="23893-118">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="23893-119">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="23893-119">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="23893-120">Seznam podporovaných brány typy návštěvě [typy podporované brány](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="23893-120">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="23893-121">Přehled</span><span class="sxs-lookup"><span data-stu-id="23893-121">Overview</span></span>

<span data-ttu-id="23893-122">Řešení potíží s sledovací proces sítě poskytuje hello možnosti řešení potíží, které nastat u brány virtuální sítě a připojení.</span><span class="sxs-lookup"><span data-stu-id="23893-122">Network Watcher troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network gateways and Connections.</span></span> <span data-ttu-id="23893-123">Po odeslání žádosti se toohello prostředků řešení potíží, protokoly jsou dotazování a prověřovány.</span><span class="sxs-lookup"><span data-stu-id="23893-123">When a request is made toohello resource troubleshooting, logs are querying and inspected.</span></span> <span data-ttu-id="23893-124">Po dokončení kontroly hello výsledky se vrátí.</span><span class="sxs-lookup"><span data-stu-id="23893-124">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="23893-125">Hello řešení rozhraní API žádosti jsou dlouhé systémem požadavků, které může trvat několik minut tooreturn výsledku.</span><span class="sxs-lookup"><span data-stu-id="23893-125">hello troubleshoot API requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="23893-126">Protokoly se ukládají v kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="23893-126">Logs are stored in a container on a storage account.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="23893-127">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="23893-127">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a><span data-ttu-id="23893-128">Řešení potíží s bránu virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="23893-128">Troubleshoot a Virtual Network gateway</span></span>


### <a name="post-hello-troubleshoot-request"></a><span data-ttu-id="23893-129">Řešení požadavek POST hello</span><span class="sxs-lookup"><span data-stu-id="23893-129">POST hello troubleshoot request</span></span>

<span data-ttu-id="23893-130">Následující příklad dotazy hello stav brány virtuální sítě Hello.</span><span class="sxs-lookup"><span data-stu-id="23893-130">hello following example queries hello status of a Virtual Network gateway.</span></span>

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

<span data-ttu-id="23893-131">Vzhledem k tomu, že tato operace je dlouhá spuštěna, hello identifikátor URI pro dotazování hello operace a hello identifikátor URI pro výsledek hello je vrácený v hello hlavička odpovědi, jak je znázorněno v hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="23893-131">Since this operation is long running, hello URI for querying hello operation and hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="23893-132">**Důležité hodnoty**</span><span class="sxs-lookup"><span data-stu-id="23893-132">**Important Values**</span></span>

* <span data-ttu-id="23893-133">**Azure AsyncOperation** -tato vlastnost obsahuje hello URI tooquery řešení hello asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="23893-133">**Azure-AsyncOperation** - This property contains hello URI tooquery hello Async troubleshoot operation</span></span>
* <span data-ttu-id="23893-134">**Umístění** -tato vlastnost obsahuje hello URI kde hello výsledky jsou při hello bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="23893-134">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

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

### <a name="query-hello-async-operation-for-completion"></a><span data-ttu-id="23893-135">Dotaz hello asynchronní operace pro dokončení</span><span class="sxs-lookup"><span data-stu-id="23893-135">Query hello async operation for completion</span></span>

<span data-ttu-id="23893-136">Použijte hello operations URI tooquery pro hello průběh operace hello, jak je vidět v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="23893-136">Use hello operations URI tooquery for hello progress of hello operation as seen in hello following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="23893-137">Když probíhá operace hello hello odpovědi ukazuje **InProgress** jak je vidět v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="23893-137">While hello operation is in progress, hello response shows **InProgress** as seen in hello following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="23893-138">Pokud se operace hello je změny stavu dokončení hello příliš**úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="23893-138">When hello operation is complete hello status changes too**Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-hello-results"></a><span data-ttu-id="23893-139">Načíst výsledky hello</span><span class="sxs-lookup"><span data-stu-id="23893-139">Retrieve hello results</span></span>

<span data-ttu-id="23893-140">Jakmile vrácen stav hello je **úspěšné**, na hello výsledek URI tooretrieve hello výsledky volání metody GET.</span><span class="sxs-lookup"><span data-stu-id="23893-140">Once hello status returned is **Succeeded**, call a GET Method on hello operationResult URI tooretrieve hello results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="23893-141">Hello následující odpovědi jsou příklady typických sníženou odpověď vrácená při dotazování na výsledky hello řešení potíží s bránu.</span><span class="sxs-lookup"><span data-stu-id="23893-141">hello following responses are examples of a typical degraded response returned when querying hello results of troubleshooting a gateway.</span></span> <span data-ttu-id="23893-142">V tématu [pochopení hello výsledky](#understanding-the-results) tooget vysvětlení na jaké vlastnosti hello v odpovědi střední hello.</span><span class="sxs-lookup"><span data-stu-id="23893-142">See [Understanding hello results](#understanding-the-results) tooget clarification on what hello properties in hello response mean.</span></span>

```json
{
  "startTime": "2017-01-12T10:31:41.562646-08:00",
  "endTime": "2017-01-12T18:31:48.677Z",
  "code": "Degraded",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time hello gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This is a transient state while hello Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If hello condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting hello VPN Gateway"
        },
        {
          "actionText": "If your VPN gateway isn't up and running by hello expected resolution time, contact support",
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
          "actionText": "If you are still experience problems with hello VPN gateway, please try resetting hello VPN gateway.",
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


## <a name="troubleshoot-connections"></a><span data-ttu-id="23893-143">Odstraňování potíží s připojením</span><span class="sxs-lookup"><span data-stu-id="23893-143">Troubleshoot Connections</span></span>

<span data-ttu-id="23893-144">Následující příklad dotazy hello stav připojení Hello.</span><span class="sxs-lookup"><span data-stu-id="23893-144">hello following example queries hello status of a Connection.</span></span>

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
> <span data-ttu-id="23893-145">Hello řešení operaci nelze spustit souběžně na jeho odpovídající brány a připojení.</span><span class="sxs-lookup"><span data-stu-id="23893-145">hello troubleshoot operation cannot be run in parallel on a Connection and its corresponding gateways.</span></span> <span data-ttu-id="23893-146">Hello operaci musíte dokončit předchozí toorunning ho na dřívější prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="23893-146">hello operation must complete prior toorunning it on hello previous resource.</span></span>

<span data-ttu-id="23893-147">Vzhledem k tomu, že toto je dlouhotrvající transakci, v hello hlavička odpovědi, hello identifikátor URI pro dotazování hello operace a hello identifikátor URI pro výsledek hello se vrátí, jak je znázorněno v hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="23893-147">Since this is a long running transaction, in hello response header, hello URI for querying hello operation and hello URI for hello result is returned as shown in hello following response:</span></span>

<span data-ttu-id="23893-148">**Důležité hodnoty**</span><span class="sxs-lookup"><span data-stu-id="23893-148">**Important Values**</span></span>

* <span data-ttu-id="23893-149">**Azure AsyncOperation** -tato vlastnost obsahuje hello URI tooquery řešení hello asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="23893-149">**Azure-AsyncOperation** - This property contains hello URI tooquery hello Async troubleshoot operation</span></span>
* <span data-ttu-id="23893-150">**Umístění** -tato vlastnost obsahuje hello URI kde hello výsledky jsou při hello bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="23893-150">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

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

### <a name="query-hello-async-operation-for-completion"></a><span data-ttu-id="23893-151">Dotaz hello asynchronní operace pro dokončení</span><span class="sxs-lookup"><span data-stu-id="23893-151">Query hello async operation for completion</span></span>

<span data-ttu-id="23893-152">Použijte hello operations URI tooquery pro hello průběh operace hello, jak je vidět v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="23893-152">Use hello operations URI tooquery for hello progress of hello operation as seen in hello following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="23893-153">Když probíhá operace hello hello odpovědi ukazuje **InProgress** jak je vidět v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="23893-153">While hello operation is in progress, hello response shows **InProgress** as seen in hello following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="23893-154">Po dokončení operace hello hello stav změní příliš**úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="23893-154">When hello operation is complete, hello status changes too**Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

<span data-ttu-id="23893-155">Hello následující odpovědi jsou příklady typických odpověď vrácená při dotazování na výsledky hello řešení potíží s připojení.</span><span class="sxs-lookup"><span data-stu-id="23893-155">hello following responses are examples of a typical response returned when querying hello results of troubleshooting a Connection.</span></span>

### <a name="retrieve-hello-results"></a><span data-ttu-id="23893-156">Načíst výsledky hello</span><span class="sxs-lookup"><span data-stu-id="23893-156">Retrieve hello results</span></span>

<span data-ttu-id="23893-157">Jakmile vrácen stav hello je **úspěšné**, na hello výsledek URI tooretrieve hello výsledky volání metody GET.</span><span class="sxs-lookup"><span data-stu-id="23893-157">Once hello status returned is **Succeeded**, call a GET Method on hello operationResult URI tooretrieve hello results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="23893-158">Hello následující odpovědi jsou příklady typických odpověď vrácená při dotazování na výsledky hello řešení potíží s připojení.</span><span class="sxs-lookup"><span data-stu-id="23893-158">hello following responses are examples of a typical response returned when querying hello results of troubleshooting a Connection.</span></span>

```json
{
  "startTime": "2017-01-12T14:09:19.1215346-08:00",
  "endTime": "2017-01-12T22:09:23.747Z",
  "code": "UnHealthy",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time hello gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This 
is a transient state while hello Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If hello condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting hello VPN gateway"
        },
        {
          "actionText": "If your VPN Connection isn't up and running by hello expected resolution time, contact support",
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
          "actionText": "If you are still experience problems with hello VPN gateway, please try resetting hello VPN gateway.",
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

## <a name="understanding-hello-results"></a><span data-ttu-id="23893-159">Seznámení s výsledky hello</span><span class="sxs-lookup"><span data-stu-id="23893-159">Understanding hello results</span></span>

<span data-ttu-id="23893-160">text Hello akce obsahuje obecné pokyny k jak tooresolve hello problém.</span><span class="sxs-lookup"><span data-stu-id="23893-160">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="23893-161">Pokud může být akce hello problému, je k dispozici odkaz s další pokyny.</span><span class="sxs-lookup"><span data-stu-id="23893-161">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="23893-162">V případě hello tam, kde není žádná další pokyny, hello odpovědi poskytuje hello url tooopen případu podpory.</span><span class="sxs-lookup"><span data-stu-id="23893-162">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="23893-163">Další informace o vlastnostech hello hello odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="23893-163">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="23893-164">Pokyny ke stahování souborů z účty azure storage, najdete v části příliš[Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="23893-164">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="23893-165">Jiný nástroj, který je možné je Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="23893-165">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="23893-166">Další informace o Storage Explorer naleznete zde na hello následující odkaz: [Storage Explorer](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="23893-166">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="23893-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23893-167">Next steps</span></span>

<span data-ttu-id="23893-168">Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.</span><span class="sxs-lookup"><span data-stu-id="23893-168">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
