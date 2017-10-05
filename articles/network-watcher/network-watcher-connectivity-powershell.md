---
title: "Zkontrolujte připojení s sledovací proces sítě Azure – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak k testování připojení s sledovací proces sítě pomocí prostředí PowerShell"
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
ms.openlocfilehash: a8f936cd23838759dc30b04688d3c6544e4895cc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="31aa2-103">Zkontrolujte připojení s sledovací proces sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="31aa2-103">Check connectivity with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="31aa2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31aa2-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="31aa2-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="31aa2-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="31aa2-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="31aa2-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="31aa2-107">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="31aa2-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="31aa2-108">Naučte se používat připojení k ověření, pokud lze navázat přímé připojení TCP z virtuálního počítače do daného koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="31aa2-108">Learn how to use connectivity to verify if a direct TCP connection from a virtual machine to a given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="31aa2-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="31aa2-109">Before you begin</span></span>

<span data-ttu-id="31aa2-110">Tento článek předpokládá, že máte v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="31aa2-110">This article assumes you have the following resources:</span></span>

* <span data-ttu-id="31aa2-111">Instance sledovací proces sítě v oblasti, které chcete zkontrolovat připojení.</span><span class="sxs-lookup"><span data-stu-id="31aa2-111">An instance of Network Watcher in the region you want to check connectivity.</span></span>

* <span data-ttu-id="31aa2-112">Zkontrolujte připojení k virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="31aa2-112">Virtual machines to check connectivity with.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="31aa2-113">Kontrola připojení vyžaduje rozšíření virtuálního počítače `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="31aa2-113">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="31aa2-114">Instalaci rozšíření na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="31aa2-114">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-the-preview-capability"></a><span data-ttu-id="31aa2-115">Registrace funkce preview</span><span class="sxs-lookup"><span data-stu-id="31aa2-115">Register the preview capability</span></span>

<span data-ttu-id="31aa2-116">Připojení je aktuálně ve verzi public preview k použití této funkce, které musí být registrováno.</span><span class="sxs-lookup"><span data-stu-id="31aa2-116">Connectivity is currently in public preview, to use this feature it needs to be registered.</span></span> <span data-ttu-id="31aa2-117">Chcete-li to provést, spusťte následující ukázku v prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="31aa2-117">To do this, run the following PowerShell sample:</span></span>

```powershell
Register-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

<span data-ttu-id="31aa2-118">Chcete-li ověřit, zda že byla registrace úspěšná, spusťte následující ukázku v prostředí Powershell:</span><span class="sxs-lookup"><span data-stu-id="31aa2-118">To verify the registration was successful, run the following Powershell sample:</span></span>

```powershell
Get-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace  Microsoft.Network
```

<span data-ttu-id="31aa2-119">Pokud funkci byla správně zaregistrovány, by měl odpovídat následující výstup:</span><span class="sxs-lookup"><span data-stu-id="31aa2-119">If the feature was properly registered, the output should match the following:</span></span>

```
FeatureName         ProviderName      RegistrationState
-----------         ------------      -----------------
AllowNetworkWatcherConnectivityCheck  Microsoft.Network Registered
```

## <a name="check-connectivity-to-a-virtual-machine"></a><span data-ttu-id="31aa2-120">Zkontrolujte připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="31aa2-120">Check connectivity to a virtual machine</span></span>

<span data-ttu-id="31aa2-121">Tento příklad zkontroluje připojení k cílovému virtuálnímu počítači přes port 80.</span><span class="sxs-lookup"><span data-stu-id="31aa2-121">This example checks connectivity to a destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="31aa2-122">Příklad</span><span class="sxs-lookup"><span data-stu-id="31aa2-122">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"
$destVMName = "Database0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName
$VM2 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $destVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationId $VM2.Id -DestinationPort 80
```

### <a name="response"></a><span data-ttu-id="31aa2-123">Odpověď</span><span class="sxs-lookup"><span data-stu-id="31aa2-123">Response</span></span>

<span data-ttu-id="31aa2-124">Následující odpověď je z předchozího příkladu.</span><span class="sxs-lookup"><span data-stu-id="31aa2-124">The following response is from the previous example.</span></span>  <span data-ttu-id="31aa2-125">V této odpovědi `ConnectionStatus` je **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="31aa2-125">In this response, the `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="31aa2-126">Uvidíte, že všechny sondy neodesílají se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="31aa2-126">You can see that all the probes sent failed.</span></span> <span data-ttu-id="31aa2-127">Připojení se nezdařilo u virtuálního zařízení z důvodu nakonfigurován uživatel `NetworkSecurityRule` s názvem **UserRule_Port80**, nakonfigurovaná tak, aby blokovala příchozí přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="31aa2-127">The connectivity failed at the virtual appliance due to a user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured to block incoming traffic on port 80.</span></span> <span data-ttu-id="31aa2-128">Tyto informace můžete použít pro zkoumání problémů s připojením.</span><span class="sxs-lookup"><span data-stu-id="31aa2-128">This information can be used to research connection issues.</span></span>

```
ConnectionStatus : Unreachable
AvgLatencyInMs   : 
MinLatencyInMs   : 
MaxLatencyInMs   : 
ProbesSent       : 100
ProbesFailed     : 100
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "c5222ea0-3213-4f85-a642-cee63217c2f3",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurat
                   ions/ipconfig1",
                       "NextHopIds": [
                         "9283a9f0-cc5e-4239-8f5e-ae0f3c19fbaa"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "VirtualAppliance",
                       "Id": "9283a9f0-cc5e-4239-8f5e-ae0f3c19fbaa",
                       "Address": "10.1.2.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/fwNic/ipConfiguratio
                   ns/ipconfig1",
                       "NextHopIds": [
                         "0f1500cd-c512-4d43-b431-7267e4e67017"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "VirtualAppliance",
                       "Id": "0f1500cd-c512-4d43-b431-7267e4e67017",
                       "Address": "10.1.3.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/auNic/ipConfiguratio
                   ns/ipconfig1",
                       "NextHopIds": [
                         "a88940f8-5fbe-40da-8d99-1dee89240f64"
                       ],
                       "Issues": [
                         {
                           "Origin": "Outbound",
                           "Severity": "Error",
                           "Type": "NetworkSecurityRule",
                           "Context": [
                             {
                               "key": "RuleName",
                               "value": "UserRule_Port80"
                             }
                           ]
                         }
                       ]
                     },
                     {
                       "Type": "VnetLocal",
                       "Id": "a88940f8-5fbe-40da-8d99-1dee89240f64",
                       "Address": "10.1.4.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/dbNic0/ipConfigurati
                   ons/ipconfig1",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="validate-routing-issues"></a><span data-ttu-id="31aa2-129">Ověření směrování problémy</span><span class="sxs-lookup"><span data-stu-id="31aa2-129">Validate routing issues</span></span>

<span data-ttu-id="31aa2-130">V příkladu ověří připojení mezi virtuálním počítačem a vzdálený koncový bod.</span><span class="sxs-lookup"><span data-stu-id="31aa2-130">The example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="31aa2-131">Příklad</span><span class="sxs-lookup"><span data-stu-id="31aa2-131">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress 13.107.21.200 -DestinationPort 80
```

### <a name="response"></a><span data-ttu-id="31aa2-132">Odpověď</span><span class="sxs-lookup"><span data-stu-id="31aa2-132">Response</span></span>

<span data-ttu-id="31aa2-133">V následujícím příkladu `ConnectionStatus` se zobrazí jako **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="31aa2-133">In the following example, the `ConnectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="31aa2-134">V `Hops` podrobnosti, můžete zobrazit v části `Issues` blokovaného provoz z důvodu `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="31aa2-134">In the `Hops` details, you can see under `Issues` that the traffic was blocked due to a `UserDefinedRoute`.</span></span> 

```
ConnectionStatus : Unreachable
AvgLatencyInMs   : 
MinLatencyInMs   : 
MaxLatencyInMs   : 
ProbesSent       : 100
ProbesFailed     : 100
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "b4f7bceb-07a3-44ca-8bae-adec6628225f",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "3fee8adf-692f-4523-b742-f6fdf6da6584"
                       ],
                       "Issues": [
                         {
                           "Origin": "Outbound",
                           "Severity": "Error",
                           "Type": "UserDefinedRoute",
                           "Context": [
                             {
                               "key": "RouteType",
                               "value": "User"
                             }
                           ]
                         }
                       ]
                     },
                     {
                       "Type": "Destination",
                       "Id": "3fee8adf-692f-4523-b742-f6fdf6da6584",
                       "Address": "13.107.21.200",
                       "ResourceId": "Unknown",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="check-website-latency"></a><span data-ttu-id="31aa2-135">Zkontrolujte latence webu</span><span class="sxs-lookup"><span data-stu-id="31aa2-135">Check website latency</span></span>

<span data-ttu-id="31aa2-136">Následující příklad zkontroluje připojení k webu.</span><span class="sxs-lookup"><span data-stu-id="31aa2-136">The following example checks the connectivity to a website.</span></span>

### <a name="example"></a><span data-ttu-id="31aa2-137">Příklad</span><span class="sxs-lookup"><span data-stu-id="31aa2-137">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress http://bing.com/
```

### <a name="response"></a><span data-ttu-id="31aa2-138">Odpověď</span><span class="sxs-lookup"><span data-stu-id="31aa2-138">Response</span></span>

<span data-ttu-id="31aa2-139">V následující odpověď, se zobrazí `ConnectionStatus` zobrazuje jako **dostupné**.</span><span class="sxs-lookup"><span data-stu-id="31aa2-139">In the following response, you can see the `ConnectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="31aa2-140">Pokud je připojení úspěšné, jsou uvedeny hodnoty latence.</span><span class="sxs-lookup"><span data-stu-id="31aa2-140">When a connection is successful, latency values are provided.</span></span>

```
ConnectionStatus : Reachable
AvgLatencyInMs   : 1
MinLatencyInMs   : 0
MaxLatencyInMs   : 7
ProbesSent       : 100
ProbesFailed     : 0
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "1f0e3415-27b0-4bf7-a59d-3e19fb854e3e",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "f99f2bd1-42e8-4bbf-85b6-5d21d00c84e0"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "Internet",
                       "Id": "f99f2bd1-42e8-4bbf-85b6-5d21d00c84e0",
                       "Address": "204.79.197.200",
                       "ResourceId": "Internet",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="check-connectivity-to-a-storage-endpoint"></a><span data-ttu-id="31aa2-141">Zkontrolujte připojení ke koncovému bodu úložiště</span><span class="sxs-lookup"><span data-stu-id="31aa2-141">Check connectivity to a storage endpoint</span></span>

<span data-ttu-id="31aa2-142">Následující příklad Otestuje připojení z virtuálního počítače na účet úložiště blogu.</span><span class="sxs-lookup"><span data-stu-id="31aa2-142">The following example tests the connectivity from a virtual machine to a blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="31aa2-143">Příklad</span><span class="sxs-lookup"><span data-stu-id="31aa2-143">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress https://contosostorageexample.blob.core.windows.net/ 
```

### <a name="response"></a><span data-ttu-id="31aa2-144">Odpověď</span><span class="sxs-lookup"><span data-stu-id="31aa2-144">Response</span></span>

<span data-ttu-id="31aa2-145">Následujícím kódu json je spustit rutinu předchozí příklad odpověď.</span><span class="sxs-lookup"><span data-stu-id="31aa2-145">The following json is the example response from running the previous cmdlet.</span></span> <span data-ttu-id="31aa2-146">Jako cílové místo je dostupná, `ConnectionStatus` vlastnost zobrazuje jako **dostupné**.</span><span class="sxs-lookup"><span data-stu-id="31aa2-146">As the destination is reachable, the `ConnectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="31aa2-147">Jsou k dispozici podrobnosti týkající se počet skoků potřebná k získání přístupu objektu blob storage a latenci.</span><span class="sxs-lookup"><span data-stu-id="31aa2-147">You are provided the details regarding the number of hops required to reach the storage blob and latency.</span></span>

```
ConnectionStatus : Reachable
AvgLatencyInMs   : 1
MinLatencyInMs   : 0
MaxLatencyInMs   : 8
ProbesSent       : 100
ProbesFailed     : 0
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "9e7f61d9-fb45-41db-83e2-c815a919b8ed",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "1e6d4b3c-7964-4afd-b959-aaa746ee0f15"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "Internet",
                       "Id": "1e6d4b3c-7964-4afd-b959-aaa746ee0f15",
                       "Address": "13.71.200.248",
                       "ResourceId": "Internet",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="next-steps"></a><span data-ttu-id="31aa2-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31aa2-148">Next steps</span></span>

<span data-ttu-id="31aa2-149">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="31aa2-149">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<span data-ttu-id="31aa2-150">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) sledovat pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="31aa2-150">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are defined.</span></span>

<!-- Image references -->














