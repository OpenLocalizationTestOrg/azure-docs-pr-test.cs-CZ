---
title: "aaaCheck připojení s sledovací proces sítě Azure – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak tootest připojení s sledovací proces sítě pomocí prostředí PowerShell"
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
ms.openlocfilehash: 4bcb90a72f178445c38b7bd7fc5054c5d0c200bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="7ff7e-103">Zkontrolujte připojení s sledovací proces sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ff7e-103">Check connectivity with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="7ff7e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7ff7e-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="7ff7e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ff7e-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="7ff7e-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="7ff7e-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="7ff7e-107">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="7ff7e-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="7ff7e-108">Zjistěte, jak by bylo možné navázat připojení tooverify toouse Pokud přímé připojení TCP z virtuálního počítače tooa, zadaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-108">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7ff7e-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="7ff7e-109">Before you begin</span></span>

<span data-ttu-id="7ff7e-110">Tento článek předpokládá, že máte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="7ff7e-110">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="7ff7e-111">Instance sledovací proces sítě v hello oblasti, kterou chcete toocheck připojení.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-111">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="7ff7e-112">Virtuální počítače připojení toocheck s.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-112">Virtual machines toocheck connectivity with.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="7ff7e-113">Kontrola připojení vyžaduje rozšíření virtuálního počítače `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-113">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="7ff7e-114">Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="7ff7e-114">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-hello-preview-capability"></a><span data-ttu-id="7ff7e-115">Zaregistrovat hello funkce preview</span><span class="sxs-lookup"><span data-stu-id="7ff7e-115">Register hello preview capability</span></span>

<span data-ttu-id="7ff7e-116">Připojení je aktuálně ve verzi public preview, toouse tato funkce je nutné toobe zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-116">Connectivity is currently in public preview, toouse this feature it needs toobe registered.</span></span> <span data-ttu-id="7ff7e-117">toodo se spuštění hello následující ukázka prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="7ff7e-117">toodo this, run hello following PowerShell sample:</span></span>

```powershell
Register-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

<span data-ttu-id="7ff7e-118">tooverify hello registrace byla úspěšná, spusťte hello následující ukázka prostředí Powershell:</span><span class="sxs-lookup"><span data-stu-id="7ff7e-118">tooverify hello registration was successful, run hello following Powershell sample:</span></span>

```powershell
Get-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace  Microsoft.Network
```

<span data-ttu-id="7ff7e-119">Pokud funkce hello byla správně zaregistrovány, hello výstup by měl odpovídat hello následující:</span><span class="sxs-lookup"><span data-stu-id="7ff7e-119">If hello feature was properly registered, hello output should match hello following:</span></span>

```
FeatureName         ProviderName      RegistrationState
-----------         ------------      -----------------
AllowNetworkWatcherConnectivityCheck  Microsoft.Network Registered
```

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="7ff7e-120">Zkontrolujte připojení k tooa virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7ff7e-120">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="7ff7e-121">Tento příklad zkontroluje připojení tooa cílového virtuálního počítače přes port 80.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-121">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="7ff7e-122">Příklad</span><span class="sxs-lookup"><span data-stu-id="7ff7e-122">Example</span></span>

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

### <a name="response"></a><span data-ttu-id="7ff7e-123">Odpověď</span><span class="sxs-lookup"><span data-stu-id="7ff7e-123">Response</span></span>

<span data-ttu-id="7ff7e-124">Hello následující odpověď je z předchozího příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-124">hello following response is from hello previous example.</span></span>  <span data-ttu-id="7ff7e-125">V této odpovědi hello `ConnectionStatus` je **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-125">In this response, hello `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="7ff7e-126">Uvidíte, že všechny hello sondy odeslání se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-126">You can see that all hello probes sent failed.</span></span> <span data-ttu-id="7ff7e-127">Hello připojení se nezdařilo u virtuálního zařízení hello kvůli tooa uživatelem nakonfigurovaného `NetworkSecurityRule` s názvem **UserRule_Port80**, nakonfigurované tooblock příchozí přenosy na portu 80.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-127">hello connectivity failed at hello virtual appliance due tooa user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured tooblock incoming traffic on port 80.</span></span> <span data-ttu-id="7ff7e-128">Tato informace může být použité tooresearch problémů s připojením.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-128">This information can be used tooresearch connection issues.</span></span>

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

## <a name="validate-routing-issues"></a><span data-ttu-id="7ff7e-129">Ověření směrování problémy</span><span class="sxs-lookup"><span data-stu-id="7ff7e-129">Validate routing issues</span></span>

<span data-ttu-id="7ff7e-130">Příklad Hello ověří připojení mezi virtuálním počítačem a vzdálený koncový bod.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-130">hello example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="7ff7e-131">Příklad</span><span class="sxs-lookup"><span data-stu-id="7ff7e-131">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress 13.107.21.200 -DestinationPort 80
```

### <a name="response"></a><span data-ttu-id="7ff7e-132">Odpověď</span><span class="sxs-lookup"><span data-stu-id="7ff7e-132">Response</span></span>

<span data-ttu-id="7ff7e-133">V následujícím příkladu hello, hello `ConnectionStatus` se zobrazí jako **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-133">In hello following example, hello `ConnectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="7ff7e-134">V hello `Hops` podrobnosti, můžete zobrazit v části `Issues` že hello provoz byl zablokován kvůli tooa `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-134">In hello `Hops` details, you can see under `Issues` that hello traffic was blocked due tooa `UserDefinedRoute`.</span></span> 

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

## <a name="check-website-latency"></a><span data-ttu-id="7ff7e-135">Zkontrolujte latence webu</span><span class="sxs-lookup"><span data-stu-id="7ff7e-135">Check website latency</span></span>

<span data-ttu-id="7ff7e-136">Hello následující příklad zkontroluje hello připojení tooa webu.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-136">hello following example checks hello connectivity tooa website.</span></span>

### <a name="example"></a><span data-ttu-id="7ff7e-137">Příklad</span><span class="sxs-lookup"><span data-stu-id="7ff7e-137">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress http://bing.com/
```

### <a name="response"></a><span data-ttu-id="7ff7e-138">Odpověď</span><span class="sxs-lookup"><span data-stu-id="7ff7e-138">Response</span></span>

<span data-ttu-id="7ff7e-139">V následující odpověď hello, uvidíte hello `ConnectionStatus` zobrazuje jako **dostupné**.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-139">In hello following response, you can see hello `ConnectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="7ff7e-140">Pokud je připojení úspěšné, jsou uvedeny hodnoty latence.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-140">When a connection is successful, latency values are provided.</span></span>

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

## <a name="check-connectivity-tooa-storage-endpoint"></a><span data-ttu-id="7ff7e-141">Zkontrolujte připojení koncový bod úložiště tooa</span><span class="sxs-lookup"><span data-stu-id="7ff7e-141">Check connectivity tooa storage endpoint</span></span>

<span data-ttu-id="7ff7e-142">Následující ukázka Hello Otestuje připojení hello z účtu úložiště virtuálního počítače tooa blogu.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-142">hello following example tests hello connectivity from a virtual machine tooa blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="7ff7e-143">Příklad</span><span class="sxs-lookup"><span data-stu-id="7ff7e-143">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress https://contosostorageexample.blob.core.windows.net/ 
```

### <a name="response"></a><span data-ttu-id="7ff7e-144">Odpověď</span><span class="sxs-lookup"><span data-stu-id="7ff7e-144">Response</span></span>

<span data-ttu-id="7ff7e-145">Hello následujícím kódu json je hello příklad odpověď z spuštění rutiny předchozí hello.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-145">hello following json is hello example response from running hello previous cmdlet.</span></span> <span data-ttu-id="7ff7e-146">Jako cíl hello je dostupný, hello `ConnectionStatus` vlastnost zobrazuje jako **dostupné**.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-146">As hello destination is reachable, hello `ConnectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="7ff7e-147">Jsou k dispozici hello podrobnosti týkající se hello počet segmentů směrování požadované tooreach hello úložiště objektů blob a latenci.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-147">You are provided hello details regarding hello number of hops required tooreach hello storage blob and latency.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7ff7e-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ff7e-148">Next steps</span></span>

<span data-ttu-id="7ff7e-149">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7ff7e-149">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<span data-ttu-id="7ff7e-150">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="7ff7e-150">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

<!-- Image references -->














