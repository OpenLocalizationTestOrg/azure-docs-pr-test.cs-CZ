---
title: "aaaCheck připojení s sledovací proces sítě Azure - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak zkontrolovat připojení toouse s sledovací proces sítě pomocí Azure CLI 2.0"
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
ms.date: 07/11/2017
ms.author: gwallace
ms.openlocfilehash: e94e0fad03fd36ebf4e1fdf9e3cfee934b289deb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="1bacd-103">Zkontrolujte připojení s sledovací proces sítě Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1bacd-103">Check connectivity with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="1bacd-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1bacd-104">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="1bacd-105">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1bacd-105">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="1bacd-106">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="1bacd-106">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="1bacd-107">Zjistěte, jak by bylo možné navázat připojení tooverify toouse Pokud přímé připojení TCP z virtuálního počítače tooa, zadaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="1bacd-107">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1bacd-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1bacd-108">Before you begin</span></span>

<span data-ttu-id="1bacd-109">Tento článek předpokládá, že máte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="1bacd-109">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="1bacd-110">Instance sledovací proces sítě v hello oblasti, kterou chcete toocheck připojení.</span><span class="sxs-lookup"><span data-stu-id="1bacd-110">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="1bacd-111">Virtuální počítače připojení toocheck s.</span><span class="sxs-lookup"><span data-stu-id="1bacd-111">Virtual machines toocheck connectivity with.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="1bacd-112">Kontrola připojení vyžaduje rozšíření virtuálního počítače `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="1bacd-112">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="1bacd-113">Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="1bacd-113">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-hello-preview-capability"></a><span data-ttu-id="1bacd-114">Zaregistrovat hello funkce preview</span><span class="sxs-lookup"><span data-stu-id="1bacd-114">Register hello preview capability</span></span> 

<span data-ttu-id="1bacd-115">Zkontrolujte připojení je aktuálně ve verzi public preview, toouse tato funkce je nutné toobe zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="1bacd-115">Connectivity check is currently in public preview, toouse this feature it needs toobe registered.</span></span> <span data-ttu-id="1bacd-116">toodo se spuštění hello následující ukázka rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="1bacd-116">toodo this, run hello following CLI sample</span></span>

```azurecli 
az feature register --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck

az provider register --namespace Microsoft.Network 
``` 

<span data-ttu-id="1bacd-117">tooverify hello registrace byla úspěšná, spusťte následující příkaz rozhraní příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="1bacd-117">tooverify hello registration was successful, run hello following CLI command:</span></span>

```azurecli
az feature show --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck 
```

<span data-ttu-id="1bacd-118">Pokud funkce hello byla správně zaregistrovány, hello výstup by měl odpovídat hello následující:</span><span class="sxs-lookup"><span data-stu-id="1bacd-118">If hello feature was properly registered, hello output should match hello following:</span></span> 

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowNetworkWatcherConnectivityCheck",
  "name": "Microsoft.Network/AllowNetworkWatcherConnectivityCheck",
  "properties": {
    "state": "Registered"
  },
  "type": "Microsoft.Features/providers/features"
}
``` 

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="1bacd-119">Zkontrolujte připojení k tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1bacd-119">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="1bacd-120">Tento příklad zkontroluje připojení tooa cílového virtuálního počítače přes port 80.</span><span class="sxs-lookup"><span data-stu-id="1bacd-120">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="1bacd-121">Příklad</span><span class="sxs-lookup"><span data-stu-id="1bacd-121">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-resource Database0 --dest-port 80
```

### <a name="response"></a><span data-ttu-id="1bacd-122">Odpověď</span><span class="sxs-lookup"><span data-stu-id="1bacd-122">Response</span></span>

<span data-ttu-id="1bacd-123">Hello následující odpověď je z předchozího příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="1bacd-123">hello following response is from hello previous example.</span></span>  <span data-ttu-id="1bacd-124">V této odpovědi hello `ConnectionStatus` je **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="1bacd-124">In this response, hello `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="1bacd-125">Uvidíte, že všechny hello sondy odeslání se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="1bacd-125">You can see that all hello probes sent failed.</span></span> <span data-ttu-id="1bacd-126">Hello připojení se nezdařilo u virtuálního zařízení hello kvůli tooa uživatelem nakonfigurovaného `NetworkSecurityRule` s názvem **UserRule_Port80**, nakonfigurované tooblock příchozí přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="1bacd-126">hello connectivity failed at hello virtual appliance due tooa user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured tooblock incoming traffic on port 80.</span></span> <span data-ttu-id="1bacd-127">Tato informace může být použité tooresearch problémů s připojením.</span><span class="sxs-lookup"><span data-stu-id="1bacd-127">This information can be used tooresearch connection issues.</span></span>

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "bb01d336-d881-4808-9fbc-72f091974d68",
      "issues": [],
      "nextHopIds": [
        "f8b074e9-9980-496b-a35e-619f9bcbf648"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "10.1.2.4",
      "id": "f8b074e9-9980-496b-a35e-619f9bcbf648",
      "issues": [],
      "nextHopIds": [
        "8a5857f3-6ab8-4b11-b9bf-a046d66b8696"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/fw
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.3.4",
      "id": "8a5857f3-6ab8-4b11-b9bf-a046d66b8696",
      "issues": [
        {
          "context": [
            {
              "key": "RuleName",
              "value": "UserRule_Port80"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "NetworkSecurityRule"
        }
      ],
      "nextHopIds": [
        "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/au
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.4.4",
      "id": "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/db
Nic0/ipConfigurations/ipconfig1",
      "type": "VnetLocal"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="validate-routing-issues"></a><span data-ttu-id="1bacd-128">Ověření směrování problémy</span><span class="sxs-lookup"><span data-stu-id="1bacd-128">Validate routing issues</span></span>

<span data-ttu-id="1bacd-129">Příklad Hello ověří připojení mezi virtuálním počítačem a vzdálený koncový bod.</span><span class="sxs-lookup"><span data-stu-id="1bacd-129">hello example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="1bacd-130">Příklad</span><span class="sxs-lookup"><span data-stu-id="1bacd-130">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address 13.107.21.200 --dest-port 80
```

### <a name="response"></a><span data-ttu-id="1bacd-131">Odpověď</span><span class="sxs-lookup"><span data-stu-id="1bacd-131">Response</span></span>

<span data-ttu-id="1bacd-132">V následujícím příkladu hello, hello `connectionStatus` se zobrazí jako **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="1bacd-132">In hello following example, hello `connectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="1bacd-133">V hello `hops` podrobnosti, můžete zobrazit v části `issues` že hello provoz byl zablokován kvůli tooa `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="1bacd-133">In hello `hops` details, you can see under `issues` that hello traffic was blocked due tooa `UserDefinedRoute`.</span></span>

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "f2cb1868-2049-4839-b8ed-57a480d06f95",
      "issues": [
        {
          "context": [
            {
              "key": "RouteType",
              "value": "User"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "UserDefinedRoute"
        }
      ],
      "nextHopIds": [
        "da4022db-0ab0-48c4-a507-dd4c03561ca5"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "13.107.21.200",
      "id": "da4022db-0ab0-48c4-a507-dd4c03561ca5",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Unknown",
      "type": "Destination"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="check-website-latency"></a><span data-ttu-id="1bacd-134">Zkontrolujte latence webu</span><span class="sxs-lookup"><span data-stu-id="1bacd-134">Check website latency</span></span>

<span data-ttu-id="1bacd-135">Hello následující příklad zkontroluje hello připojení tooa webu.</span><span class="sxs-lookup"><span data-stu-id="1bacd-135">hello following example checks hello connectivity tooa website.</span></span>

### <a name="example"></a><span data-ttu-id="1bacd-136">Příklad</span><span class="sxs-lookup"><span data-stu-id="1bacd-136">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address http://bing.com --dest-port 80
```

### <a name="response"></a><span data-ttu-id="1bacd-137">Odpověď</span><span class="sxs-lookup"><span data-stu-id="1bacd-137">Response</span></span>

<span data-ttu-id="1bacd-138">V následující odpověď hello, uvidíte hello `connectionStatus` zobrazuje jako **dostupné**.</span><span class="sxs-lookup"><span data-stu-id="1bacd-138">In hello following response, you can see hello `connectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="1bacd-139">Pokud je připojení úspěšné, jsou uvedeny hodnoty latence.</span><span class="sxs-lookup"><span data-stu-id="1bacd-139">When a connection is successful, latency values are provided.</span></span>

```json
{
  "avgLatencyInMs": 2,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "639c2d19-e163-4dfd-8737-5018dd1168ae",
      "issues": [],
      "nextHopIds": [
        "fd43a6e7-c758-4f48-90aa-8db99105a4a3"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "204.79.197.200",
      "id": "fd43a6e7-c758-4f48-90aa-8db99105a4a3",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="check-connectivity-tooa-storage-endpoint"></a><span data-ttu-id="1bacd-140">Zkontrolujte připojení koncový bod úložiště tooa</span><span class="sxs-lookup"><span data-stu-id="1bacd-140">Check connectivity tooa storage endpoint</span></span>

<span data-ttu-id="1bacd-141">Hello následující příklad zkontroluje hello připojení z účtu úložiště virtuálního počítače tooa blogu.</span><span class="sxs-lookup"><span data-stu-id="1bacd-141">hello following example checks hello connectivity from a virtual machine tooa blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="1bacd-142">Příklad</span><span class="sxs-lookup"><span data-stu-id="1bacd-142">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address https://contosoexamplesa.blob.core.windows.net/
```

### <a name="response"></a><span data-ttu-id="1bacd-143">Odpověď</span><span class="sxs-lookup"><span data-stu-id="1bacd-143">Response</span></span>

<span data-ttu-id="1bacd-144">Hello následujícím kódu json je hello příklad odpověď z spuštění rutiny předchozí hello.</span><span class="sxs-lookup"><span data-stu-id="1bacd-144">hello following json is hello example response from running hello previous cmdlet.</span></span> <span data-ttu-id="1bacd-145">Kontrola hello je úspěšné, hello `connectionStatus` vlastnost zobrazuje jako **dostupné**.</span><span class="sxs-lookup"><span data-stu-id="1bacd-145">As hello check is successful, hello `connectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="1bacd-146">Jsou k dispozici hello podrobnosti týkající se hello počet segmentů směrování požadované tooreach hello úložiště objektů blob a latenci.</span><span class="sxs-lookup"><span data-stu-id="1bacd-146">You are provided hello details regarding hello number of hops required tooreach hello storage blob and latency.</span></span>

```json
{
  "avgLatencyInMs": 1,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "5136acff-bf26-4c93-9966-4edb7dd40353",
      "issues": [],
      "nextHopIds": [
        "f8d958b7-3636-4d63-9441-602c1eb2fd56"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "1.2.3.4",
      "id": "f8d958b7-3636-4d63-9441-602c1eb2fd56",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="next-steps"></a><span data-ttu-id="1bacd-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1bacd-147">Next steps</span></span>

<span data-ttu-id="1bacd-148">Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="1bacd-148">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="1bacd-149">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="1bacd-149">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>
