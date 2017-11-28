---
title: "aaaCheck připojení s sledovací proces sítě Azure - portálu Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak hello toocheck připojení s sledovací proces sítě v portálu Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: gwallace
ms.openlocfilehash: 8560011906fcce46d31556fc52cbfa671e8e653a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-hello-azure-portal"></a><span data-ttu-id="509fe-103">Zkontrolujte připojení s sledovací proces sítě Azure pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="509fe-103">Check connectivity with Azure Network Watcher using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="509fe-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="509fe-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="509fe-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="509fe-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="509fe-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="509fe-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="509fe-107">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="509fe-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="509fe-108">Zjistěte, jak by bylo možné navázat připojení tooverify toouse Pokud přímé připojení TCP z virtuálního počítače tooa, zadaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="509fe-108">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="509fe-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="509fe-109">Before you begin</span></span>

<span data-ttu-id="509fe-110">Tento článek předpokládá, že máte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="509fe-110">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="509fe-111">Instance sledovací proces sítě v hello oblasti, kterou chcete toocheck připojení.</span><span class="sxs-lookup"><span data-stu-id="509fe-111">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="509fe-112">Virtuální počítače připojení toocheck s.</span><span class="sxs-lookup"><span data-stu-id="509fe-112">Virtual machines toocheck connectivity with.</span></span>

<span data-ttu-id="509fe-113">ARMclient je použité toocall hello REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="509fe-113">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="509fe-114">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient).</span><span class="sxs-lookup"><span data-stu-id="509fe-114">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient).</span></span>

<span data-ttu-id="509fe-115">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="509fe-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="509fe-116">Kontrola připojení vyžaduje rozšíření virtuálního počítače `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="509fe-116">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="509fe-117">Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="509fe-117">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-hello-preview-capability"></a><span data-ttu-id="509fe-118">Zaregistrovat hello funkce preview</span><span class="sxs-lookup"><span data-stu-id="509fe-118">Register hello preview capability</span></span>

<span data-ttu-id="509fe-119">Zkontrolujte připojení je aktuálně ve verzi public preview, toouse tato funkce je nutné toobe zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="509fe-119">Connectivity check is currently in public preview, toouse this feature it needs toobe registered.</span></span> <span data-ttu-id="509fe-120">toodo se spuštění hello následující ukázka prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="509fe-120">toodo this, run hello following PowerShell sample:</span></span>

```powershell
Register-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

<span data-ttu-id="509fe-121">tooverify hello registrace byla úspěšná, spusťte hello následující ukázka prostředí Powershell:</span><span class="sxs-lookup"><span data-stu-id="509fe-121">tooverify hello registration was successful, run hello following Powershell sample:</span></span>

```powershell
Get-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace  Microsoft.Network
```

<span data-ttu-id="509fe-122">Pokud funkce hello byla správně zaregistrovány, hello výstup by měl odpovídat hello následující:</span><span class="sxs-lookup"><span data-stu-id="509fe-122">If hello feature was properly registered, hello output should match hello following:</span></span>

```
FeatureName                             ProviderName      RegistrationState
-----------                             ------------      -----------------
AllowNetworkWatcherConnectivityCheck    Microsoft.Network Registered
```

## <a name="log-in-with-armclient"></a><span data-ttu-id="509fe-123">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="509fe-123">Log in with ARMClient</span></span>

<span data-ttu-id="509fe-124">Přihlaste se pomocí svých přihlašovacích údajů Azure tooarmclient.</span><span class="sxs-lookup"><span data-stu-id="509fe-124">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="509fe-125">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="509fe-125">Retrieve a virtual machine</span></span>

<span data-ttu-id="509fe-126">Spusťte následující skript tooreturn hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="509fe-126">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="509fe-127">Tyto informace budete potřebovat pro spuštění připojení.</span><span class="sxs-lookup"><span data-stu-id="509fe-127">This information is needed for running connectivity.</span></span> 

<span data-ttu-id="509fe-128">Následující kód Hello potřebuje hodnoty pro hello následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="509fe-128">hello following code needs values for hello following variables:</span></span>

- <span data-ttu-id="509fe-129">**ID předplatného** -hello toouse ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="509fe-129">**subscriptionId** - hello subscription ID toouse.</span></span>
- <span data-ttu-id="509fe-130">**Název skupiny prostředků** – hello název skupiny prostředků, která obsahuje virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="509fe-130">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="509fe-131">ID hello hello virtuálního počítače hello následující výstup, se používá v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="509fe-131">From hello following output, hello ID of hello virtual machine is used in hello following example:</span></span>

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="509fe-132">Zkontrolujte připojení k tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="509fe-132">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="509fe-133">Tento příklad zkontroluje připojení tooa cílového virtuálního počítače přes port 80.</span><span class="sxs-lookup"><span data-stu-id="509fe-133">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="509fe-134">Příklad</span><span class="sxs-lookup"><span data-stu-id="509fe-134">Example</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$sourceResourceId = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/MultiTierApp0"
$destinationAddress = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/Database0"
$destinationPort = "0"
$requestBody = @"
{
  'source': {
    'resourceId': '${sourceResourceId}',
    'port': 0
  },
  'destination': {
    'resourceId': '${destinationAddress}',
    'port': ${destinationPort}
  }
}
"@

$response = armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/connectivityCheck?api-version=2017-03-01" $requestBody
```

<span data-ttu-id="509fe-135">Vzhledem k tomu, že tato operace je dlouhá spuštěna, hello identifikátor URI pro výsledek hello je vrácený v hlavičku odpovědi hello jak je znázorněno v hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="509fe-135">Since this operation is long running, hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="509fe-136">**Důležité hodnoty**</span><span class="sxs-lookup"><span data-stu-id="509fe-136">**Important Values**</span></span>

* <span data-ttu-id="509fe-137">**Umístění** -tato vlastnost obsahuje hello URI kde hello výsledky jsou při hello bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="509fe-137">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: f09b55fe-1d3a-4df7-817f-bceb8d2a94c8
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/f09b55fe-1d3a-4df7-817f-bceb8d2a94c8?api-version=2017-03-01
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 367a91aa-7142-436a-867d-d3a36f80bc54
x-ms-routing-request-id: WESTUS2:20170602T202117Z:367a91aa-7142-436a-867d-d3a36f80bc54
Date: Fri, 02 Jun 2017 20:21:16 GMT

null
```

### <a name="response"></a><span data-ttu-id="509fe-138">Odpověď</span><span class="sxs-lookup"><span data-stu-id="509fe-138">Response</span></span>

<span data-ttu-id="509fe-139">Hello následující odpověď je z předchozího příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="509fe-139">hello following response is from hello previous example.</span></span>  <span data-ttu-id="509fe-140">V této odpovědi hello `ConnectionStatus` je **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="509fe-140">In this response, hello `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="509fe-141">Uvidíte, že všechny hello sondy odeslání se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="509fe-141">You can see that all hello probes sent failed.</span></span> <span data-ttu-id="509fe-142">Hello připojení se nezdařilo u virtuálního zařízení hello kvůli tooa uživatelem nakonfigurovaného `NetworkSecurityRule` s názvem **UserRule_Port80**, nakonfigurované tooblock příchozí přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="509fe-142">hello connectivity failed at hello virtual appliance due tooa user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured tooblock incoming traffic on port 80.</span></span> <span data-ttu-id="509fe-143">Tato informace může být použité tooresearch problémů s připojením.</span><span class="sxs-lookup"><span data-stu-id="509fe-143">This information can be used tooresearch connection issues.</span></span>

```json
{
  "hops": [
    {
      "type": "Source",
      "id": "0cb75c91-7ebf-4df8-8424-15594d6fb51c",
      "address": "10.1.1.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "06dee00a-9c4a-4fb1-b2ea-fa0a539ca684"
      ],
      "issues": []
    },
    {
      "type": "VirtualAppliance",
      "id": "06dee00a-9c4a-4fb1-b2ea-fa0a539ca684",
      "address": "10.1.2.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/fwNic/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "75e0cfa5-f9d2-48d8-b705-2c7016f81570"
      ],
      "issues": []
    },
    {
      "type": "VirtualAppliance",
      "id": "75e0cfa5-f9d2-48d8-b705-2c7016f81570",
      "address": "10.1.3.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/auNic/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "86caf6aa-33b0-48a1-b4da-f3c9ce785072"
      ],
      "issues": [
        {
          "origin": "Outbound",
          "severity": "Error",
          "type": "NetworkSecurityRule",
          "context": [
            {
              "key": "RuleName",
              "value": "UserRule_Port80"
            }
          ]
        }
      ]
    },
    {
      "type": "VnetLocal",
      "id": "86caf6aa-33b0-48a1-b4da-f3c9ce785072",
      "address": "10.1.4.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/dbNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [],
      "issues": []
    }
  ],
  "connectionStatus": "Unreachable",
  "probesSent": 100,
  "probesFailed": 100
}
```

## <a name="validate-routing-issues"></a><span data-ttu-id="509fe-144">Ověření směrování problémy</span><span class="sxs-lookup"><span data-stu-id="509fe-144">Validate routing issues</span></span>

<span data-ttu-id="509fe-145">Příklad Hello ověří připojení mezi virtuálním počítačem a vzdálený koncový bod.</span><span class="sxs-lookup"><span data-stu-id="509fe-145">hello example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="509fe-146">Příklad</span><span class="sxs-lookup"><span data-stu-id="509fe-146">Example</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$sourceResourceId = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/MultiTierApp0"
$destinationAddress = "13.107.21.200"
$destinationPort = "80"
$requestBody = @"
{
  'source': {
    'resourceId': '${sourceResourceId}',
    'port': 0
  },
  'destination': {
    'address': '${destinationAddress}',
    'port': ${destinationPort}
  }
}
"@

$response = armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/connectivityCheck?api-version=2017-03-01" $requestBody
```

<span data-ttu-id="509fe-147">Vzhledem k tomu, že tato operace je dlouhá spuštěna, hello identifikátor URI pro výsledek hello je vrácený v hlavičku odpovědi hello jak je znázorněno v hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="509fe-147">Since this operation is long running, hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="509fe-148">**Důležité hodnoty**</span><span class="sxs-lookup"><span data-stu-id="509fe-148">**Important Values**</span></span>

* <span data-ttu-id="509fe-149">**Umístění** -tato vlastnost obsahuje hello URI kde hello výsledky jsou při hello bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="509fe-149">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 15eeeb69-fcef-41db-bc4a-e2adcf2658e0
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/15eeeb69-fcef-41db-bc4a-e2adcf2658e0?api-version=2017-03-01
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4370b798-cd8b-4d3e-ba28-22232bc81dc5
x-ms-routing-request-id: WESTUS:20170602T202606Z:4370b798-cd8b-4d3e-ba28-22232bc81dc5
Date: Fri, 02 Jun 2017 20:26:05 GMT

null
```

### <a name="response"></a><span data-ttu-id="509fe-150">Odpověď</span><span class="sxs-lookup"><span data-stu-id="509fe-150">Response</span></span>

<span data-ttu-id="509fe-151">V následujícím příkladu hello, hello `connectionStatus` se zobrazí jako **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="509fe-151">In hello following example, hello `connectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="509fe-152">V hello `hops` podrobnosti, můžete zobrazit v části `issues` že hello provoz byl zablokován kvůli tooa `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="509fe-152">In hello `hops` details, you can see under `issues` that hello traffic was blocked due tooa `UserDefinedRoute`.</span></span>

```json
{
  "hops": [
    {
      "type": "Source",
      "id": "5528055a-b393-4751-97bc-353d8c0aaeff",
      "address": "10.1.1.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "66eefa79-5bfe-48b2-b6ca-eec8247457a3"
      ],
      "issues": [
        {
          "origin": "Outbound",
          "severity": "Error",
          "type": "UserDefinedRoute",
          "context": [
            {
              "key": "RouteType",
              "value": "User"
            }
          ]
        }
      ]
    },
    {
      "type": "Destination",
      "id": "66eefa79-5bfe-48b2-b6ca-eec8247457a3",
      "address": "13.107.21.200",
      "resourceId": "Unknown",
      "nextHopIds": [],
      "issues": []
    }
  ],
  "connectionStatus": "Unreachable",
  "probesSent": 100,
  "probesFailed": 100
}
```

## <a name="check-website-latency"></a><span data-ttu-id="509fe-153">Zkontrolujte latence webu</span><span class="sxs-lookup"><span data-stu-id="509fe-153">Check website latency</span></span>

<span data-ttu-id="509fe-154">Hello následující příklad zkontroluje hello připojení tooa webu.</span><span class="sxs-lookup"><span data-stu-id="509fe-154">hello following example checks hello connectivity tooa website.</span></span>

### <a name="example"></a><span data-ttu-id="509fe-155">Příklad</span><span class="sxs-lookup"><span data-stu-id="509fe-155">Example</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$sourceResourceId = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/MultiTierApp0"
$destinationAddress = "http://bing.com"
$destinationPort = "0"
$requestBody = @"
{
  'source': {
    'resourceId': '${sourceResourceId}',
    'port': 0
  },
  'destination': {
    'address': '${destinationAddress}',
    'port': ${destinationPort}
  }
}
"@

$response = armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/connectivityCheck?api-version=2017-03-01" $requestBody
```

<span data-ttu-id="509fe-156">Vzhledem k tomu, že tato operace je dlouhá spuštěna, hello identifikátor URI pro výsledek hello je vrácený v hlavičku odpovědi hello jak je znázorněno v hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="509fe-156">Since this operation is long running, hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="509fe-157">**Důležité hodnoty**</span><span class="sxs-lookup"><span data-stu-id="509fe-157">**Important Values**</span></span>

* <span data-ttu-id="509fe-158">**Umístění** -tato vlastnost obsahuje hello URI kde hello výsledky jsou při hello bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="509fe-158">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: e49b12c7-c232-472c-b6d2-6c257ce80fa5
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/e49b12c7-c232-472c-b6d2-6c257ce80fa5?api-version=2017-03-01
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: c3d9744f-5683-427d-bdd1-636b68ab01b6
x-ms-routing-request-id: WESTUS:20170602T203101Z:c3d9744f-5683-427d-bdd1-636b68ab01b6
Date: Fri, 02 Jun 2017 20:31:00 GMT

null
```

### <a name="response"></a><span data-ttu-id="509fe-159">Odpověď</span><span class="sxs-lookup"><span data-stu-id="509fe-159">Response</span></span>

<span data-ttu-id="509fe-160">V následující odpověď hello, uvidíte hello `connectionStatus` zobrazuje jako **dostupné**.</span><span class="sxs-lookup"><span data-stu-id="509fe-160">In hello following response, you can see hello `connectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="509fe-161">Pokud je připojení úspěšné, jsou uvedeny hodnoty latence.</span><span class="sxs-lookup"><span data-stu-id="509fe-161">When a connection is successful, latency values are provided.</span></span>

```json
{
  "hops": [
    {
      "type": "Source",
      "id": "6adc0fe1-e384-4220-b1b1-f0d181220072",
      "address": "10.1.1.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "b50b7076-9ff2-4782-b40e-0b89cf758f74"
      ],
      "issues": []
    },
    {
      "type": "Internet",
      "id": "b50b7076-9ff2-4782-b40e-0b89cf758f74",
      "address": "204.79.197.200",
      "resourceId": "Internet",
      "nextHopIds": [],
      "issues": []
    }
  ],
  "connectionStatus": "Reachable",
  "avgLatencyInMs": 1,
  "minLatencyInMs": 0,
  "maxLatencyInMs": 7,
  "probesSent": 100,
  "probesFailed": 0
}
```

## <a name="check-connectivity-tooa-storage-endpoint"></a><span data-ttu-id="509fe-162">Zkontrolujte připojení koncový bod úložiště tooa</span><span class="sxs-lookup"><span data-stu-id="509fe-162">Check connectivity tooa storage endpoint</span></span>

<span data-ttu-id="509fe-163">Hello následující příklad zkontroluje hello připojení z účtu úložiště virtuálního počítače tooa blogu.</span><span class="sxs-lookup"><span data-stu-id="509fe-163">hello following example checks hello connectivity from a virtual machine tooa blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="509fe-164">Příklad</span><span class="sxs-lookup"><span data-stu-id="509fe-164">Example</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$sourceResourceId = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/MultiTierApp0"
$destinationAddress = "https://build2017nwdiag360.blob.core.windows.net/"
$destinationPort = "0"
$requestBody = @"
{
  'source': {
    'resourceId': '${sourceResourceId}',
    'port': 0
  },
  'destination': {
    'address': '${destinationAddress}',
    'port': ${destinationPort}
  }
}
"@

$response = armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/connectivityCheck?api-version=2017-03-01" $requestBody
```

<span data-ttu-id="509fe-165">Vzhledem k tomu, že tato operace je dlouhá spuštěna, hello identifikátor URI pro výsledek hello je vrácený v hlavičku odpovědi hello jak je znázorněno v hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="509fe-165">Since this operation is long running, hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="509fe-166">**Důležité hodnoty**</span><span class="sxs-lookup"><span data-stu-id="509fe-166">**Important Values**</span></span>

* <span data-ttu-id="509fe-167">**Umístění** -tato vlastnost obsahuje hello URI kde hello výsledky jsou při hello bylo dokončeno</span><span class="sxs-lookup"><span data-stu-id="509fe-167">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: c4ed3806-61ea-4a6b-abc1-9d6f2afc79c2
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/c4ed3806-61ea-4a6b-abc1-9d6f2afc79c2?api-version=2017-03-01
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 93bf5af0-fef5-4b7a-bb9e-9976ba5cdb95
x-ms-routing-request-id: WESTUS2:20170602T200504Z:93bf5af0-fef5-4b7a-bb9e-9976ba5cdb95
Date: Fri, 02 Jun 2017 20:05:03 GMT

null
```

### <a name="response"></a><span data-ttu-id="509fe-168">Odpověď</span><span class="sxs-lookup"><span data-stu-id="509fe-168">Response</span></span>

<span data-ttu-id="509fe-169">Hello následující příklad je hello odpověď z systémem hello předchozí volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="509fe-169">hello following example is hello response from running hello previous API call.</span></span> <span data-ttu-id="509fe-170">Kontrola hello je úspěšné, hello `connectionStatus` vlastnost zobrazuje jako **dostupné**.</span><span class="sxs-lookup"><span data-stu-id="509fe-170">As hello check is successful, hello `connectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="509fe-171">Jsou k dispozici hello podrobnosti týkající se hello počet segmentů směrování požadované tooreach hello úložiště objektů blob a latenci.</span><span class="sxs-lookup"><span data-stu-id="509fe-171">You are provided hello details regarding hello number of hops required tooreach hello storage blob and latency.</span></span>

```json
{
  "hops": [
    {
      "type": "Source",
      "id": "6adc0fe1-e384-4220-b1b1-f0d181220072",
      "address": "10.1.1.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "b50b7076-9ff2-4782-b40e-0b89cf758f74"
      ],
      "issues": []
    },
    {
      "type": "Internet",
      "id": "b50b7076-9ff2-4782-b40e-0b89cf758f74",
      "address": "13.71.200.248",
      "resourceId": "Internet",
      "nextHopIds": [],
      "issues": []
    }
  ],
  "connectionStatus": "Reachable",
  "avgLatencyInMs": 1,
  "minLatencyInMs": 0,
  "maxLatencyInMs": 7,
  "probesSent": 100,
  "probesFailed": 0
}
```

## <a name="next-steps"></a><span data-ttu-id="509fe-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="509fe-172">Next steps</span></span>

<span data-ttu-id="509fe-173">Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="509fe-173">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="509fe-174">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="509fe-174">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->














