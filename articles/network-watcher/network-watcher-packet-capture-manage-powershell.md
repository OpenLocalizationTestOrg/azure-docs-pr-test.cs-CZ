---
title: "Spravovat zachycení paketů s sledovací proces sítě Azure – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak spravovat funkci zachycení paketu sledovací proces sítě pomocí prostředí PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 04d82085-c9ea-4ea1-b050-a3dd4960f3aa
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: abd3b3641da80ee835fac85b4bde68594449e451
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="e0f79-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e0f79-103">Manage packet captures with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="e0f79-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e0f79-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="e0f79-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e0f79-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="e0f79-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e0f79-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="e0f79-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e0f79-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="e0f79-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="e0f79-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="e0f79-109">Zachytáváním paketů sledovací proces sítě vám umožní vytvořit relace zachytávání sledovat provoz do a z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e0f79-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="e0f79-110">Filtry jsou k dispozici pro relaci zachytávání zajistit, že zaznamenáte pouze provoz, který chcete.</span><span class="sxs-lookup"><span data-stu-id="e0f79-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="e0f79-111">Při diagnostice sítě anomálií reaktivně a proaktivně pomáhá zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="e0f79-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="e0f79-112">Jiné účely zahrnují shromažďování statistiku sítě, získá informace o síti vniknutí, k ladění komunikaci klienta se serverem a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="e0f79-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="e0f79-113">Díky vzdáleně aktivovat paketu zachycení, tato funkce snižuje zátěž spuštěných zachytáváním paketů ručně a na požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="e0f79-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="e0f79-114">Tento článek vás provede úloh jiný správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="e0f79-114">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="e0f79-115">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="e0f79-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="e0f79-116">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="e0f79-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="e0f79-117">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="e0f79-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="e0f79-118">**Stáhnout zachytáváním paketů**</span><span class="sxs-lookup"><span data-stu-id="e0f79-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="e0f79-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="e0f79-119">Before you begin</span></span>

<span data-ttu-id="e0f79-120">Tento článek předpokládá, že máte v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="e0f79-120">This article assumes you have the following resources:</span></span>

* <span data-ttu-id="e0f79-121">Instance sledovací proces sítě v oblasti, kterou chcete vytvořit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="e0f79-121">An instance of Network Watcher in the region you want to create a packet capture</span></span>

* <span data-ttu-id="e0f79-122">Virtuální počítač s příponou zachytávání paketů povoleno.</span><span class="sxs-lookup"><span data-stu-id="e0f79-122">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e0f79-123">Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="e0f79-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="e0f79-124">Instalaci rozšíření na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="e0f79-124">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="e0f79-125">Instalace rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e0f79-125">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="e0f79-126">Krok 1</span><span class="sxs-lookup"><span data-stu-id="e0f79-126">Step 1</span></span>

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a><span data-ttu-id="e0f79-127">Krok 2</span><span class="sxs-lookup"><span data-stu-id="e0f79-127">Step 2</span></span>

<span data-ttu-id="e0f79-128">Následující příklad načte informace o rozšíření, které jsou potřebné ke spuštění `Set-AzureRmVMExtension` rutiny.</span><span class="sxs-lookup"><span data-stu-id="e0f79-128">The following example retrieves the extension information needed to run the `Set-AzureRmVMExtension` cmdlet.</span></span> <span data-ttu-id="e0f79-129">Tato rutina nainstaluje agenta zachytávání paketů na virtuálním počítači hosta.</span><span class="sxs-lookup"><span data-stu-id="e0f79-129">This cmdlet installs the packet capture agent on the guest virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="e0f79-130">`Set-AzureRmVMExtension` Rutiny může trvat několik minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="e0f79-130">The `Set-AzureRmVMExtension` cmdlet may take several minutes to complete.</span></span>

<span data-ttu-id="e0f79-131">Pro virtuální počítače s Windows:</span><span class="sxs-lookup"><span data-stu-id="e0f79-131">For Windows virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

<span data-ttu-id="e0f79-132">Pro virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="e0f79-132">For Linux virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

<span data-ttu-id="e0f79-133">Následující příklad je úspěšné odpovědi po spuštění `Set-AzureRmVMExtension` rutiny.</span><span class="sxs-lookup"><span data-stu-id="e0f79-133">The following example is a successful response after running the `Set-AzureRmVMExtension` cmdlet.</span></span>

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a><span data-ttu-id="e0f79-134">Krok 3</span><span class="sxs-lookup"><span data-stu-id="e0f79-134">Step 3</span></span>

<span data-ttu-id="e0f79-135">Chcete-li zajistit, že je agent nainstalovaný, spusťte `Get-AzureRmVMExtension` rutiny a předejte ji název virtuálního počítače a název rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e0f79-135">To ensure that the agent is installed, run the `Get-AzureRmVMExtension` cmdlet and pass it the virtual machine name and the extension name.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

<span data-ttu-id="e0f79-136">Následující příklad je příklad odpovědi spuštění`Get-AzureRmVMExtension`</span><span class="sxs-lookup"><span data-stu-id="e0f79-136">The following sample is an example of the response from running `Get-AzureRmVMExtension`</span></span>

```
ResourceGroupName       : testrg
VMName                  : testvm1
Name                    : AzureNetworkWatcherExtension
Location                : westcentralus
Etag                    : null
Publisher               : Microsoft.Azure.NetworkWatcher
ExtensionType           : NetworkWatcherAgentWindows
TypeHandlerVersion      : 1.4
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1/
                          extensions/AzureNetworkWatcherExtension
PublicSettings          : 
ProtectedSettings       : 
ProvisioningState       : Succeeded
Statuses                : 
SubStatuses             : 
AutoUpgradeMinorVersion : True
ForceUpdateTag          : 
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="e0f79-137">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="e0f79-137">Start a packet capture</span></span>

<span data-ttu-id="e0f79-138">Po dokončení předchozích kroků se agent zachytávání paketů je nainstalován na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e0f79-138">Once the preceding steps are complete, the packet capture agent is installed on the virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="e0f79-139">Krok 1</span><span class="sxs-lookup"><span data-stu-id="e0f79-139">Step 1</span></span>

<span data-ttu-id="e0f79-140">Dalším krokem je pro získání instance sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="e0f79-140">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="e0f79-141">Tato proměnná je předána `New-AzureRmNetworkWatcherPacketCapture` rutiny v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="e0f79-141">This variable is passed to the `New-AzureRmNetworkWatcherPacketCapture` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a><span data-ttu-id="e0f79-142">Krok 2</span><span class="sxs-lookup"><span data-stu-id="e0f79-142">Step 2</span></span>

<span data-ttu-id="e0f79-143">Načtěte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0f79-143">Retrieve a storage account.</span></span> <span data-ttu-id="e0f79-144">Tento účet úložiště se používá k uložení souboru zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="e0f79-144">This storage account is used to store the packet capture file.</span></span>

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a><span data-ttu-id="e0f79-145">Krok 3</span><span class="sxs-lookup"><span data-stu-id="e0f79-145">Step 3</span></span>

<span data-ttu-id="e0f79-146">Chcete-li omezit data, která je uložená ve zachytáváním paketů použít filtry.</span><span class="sxs-lookup"><span data-stu-id="e0f79-146">Filters can be used to limit the data that is stored by the packet capture.</span></span> <span data-ttu-id="e0f79-147">Následující příklad nastaví dva filtry.</span><span class="sxs-lookup"><span data-stu-id="e0f79-147">The following example sets up two filters.</span></span>  <span data-ttu-id="e0f79-148">Jeden filtr shromažďuje odchozí přenosy TCP pouze z místní IP 10.0.0.3 do cílové porty 20, 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="e0f79-148">One filter collects outgoing TCP traffic only from local IP 10.0.0.3 to destination ports 20, 80 and 443.</span></span>  <span data-ttu-id="e0f79-149">Druhý filtr shromažďuje jenom provoz UDP.</span><span class="sxs-lookup"><span data-stu-id="e0f79-149">The second filter collects only UDP traffic.</span></span>

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> <span data-ttu-id="e0f79-150">Více filtrů lze definovat pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="e0f79-150">Multiple filters can be defined for a packet capture.</span></span>

### <a name="step-4"></a><span data-ttu-id="e0f79-151">Krok 4</span><span class="sxs-lookup"><span data-stu-id="e0f79-151">Step 4</span></span>

<span data-ttu-id="e0f79-152">Spustit `New-AzureRmNetworkWatcherPacketCapture` rutiny spustit proces zachycení paketu předávání požadované hodnoty načíst v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="e0f79-152">Run the `New-AzureRmNetworkWatcherPacketCapture` cmdlet to start the packet capture process, passing the required values retrieved in the preceding steps.</span></span>
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filters $filter1, $filter2
```

<span data-ttu-id="e0f79-153">V následujícím příkladu je očekávaný výstup spuštění `New-AzureRmNetworkWatcherPacketCapture` rutiny.</span><span class="sxs-lookup"><span data-stu-id="e0f79-153">The following example is the expected output from running the `New-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span>

```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"3bf27278-8251-4651-9546-c7f369855e4e"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]


```

## <a name="get-a-packet-capture"></a><span data-ttu-id="e0f79-154">Získat zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="e0f79-154">Get a packet capture</span></span>

<span data-ttu-id="e0f79-155">Spuštění `Get-AzureRmNetworkWatcherPacketCapture` rutina načte stav paketu zachycení aktuálně spuštěné, nebo pokud byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="e0f79-155">Running the `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, retrieves the status of a currently running, or completed packet capture.</span></span>

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

<span data-ttu-id="e0f79-156">V následujícím příkladu je výstup z `Get-AzureRmNetworkWatcherPacketCapture` rutiny.</span><span class="sxs-lookup"><span data-stu-id="e0f79-156">The following example is the output from the `Get-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span> <span data-ttu-id="e0f79-157">V následujícím příkladu je po dokončení zachytávání.</span><span class="sxs-lookup"><span data-stu-id="e0f79-157">The following example is after the capture is complete.</span></span> <span data-ttu-id="e0f79-158">Hodnota PacketCaptureStatus je zastavena, s StopReason TimeExceeded.</span><span class="sxs-lookup"><span data-stu-id="e0f79-158">The PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="e0f79-159">Tato hodnota ukazuje, že zachytáváním paketů byla úspěšná a jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="e0f79-159">This value shows that the packet capture was successful and ran its time.</span></span>
```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"4b9a81ed-dc63-472e-869e-96d7166ccb9b"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]
CaptureStartTime        : 2/1/2017 10:43:01 PM
PacketCaptureStatus     : Stopped
StopReason              : TimeExceeded
PacketCaptureError      : []
```

## <a name="stop-a-packet-capture"></a><span data-ttu-id="e0f79-160">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="e0f79-160">Stop a packet capture</span></span>

<span data-ttu-id="e0f79-161">Spuštěním `Stop-AzureRmNetworkWatcherPacketCapture` rutiny, pokud zachycení relace je v průběhu je zastavena.</span><span class="sxs-lookup"><span data-stu-id="e0f79-161">By running the `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, if a capture session is in progress it is stopped.</span></span>

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="e0f79-162">Žádná odpověď vrátí rutina při spuštěné v relaci aktuálně spuštěné zachycení nebo existující relaci, která již byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="e0f79-162">The cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="e0f79-163">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="e0f79-163">Delete a packet capture</span></span>

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="e0f79-164">Odstraňuje se zachytáváním paketů nedojde k odstranění souboru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0f79-164">Deleting a packet capture does not delete the file in the storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="e0f79-165">Stáhnout zachytáváním paketů</span><span class="sxs-lookup"><span data-stu-id="e0f79-165">Download a packet capture</span></span>

<span data-ttu-id="e0f79-166">Po dokončení relace zachytávání paketů můžete zaznamenat soubor odeslat do úložiště objektů blob nebo do místního souboru virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e0f79-166">Once your packet capture session has completed, the capture file can be uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="e0f79-167">Umístění úložiště pro zachytávání paketů se definuje při vytvoření relace.</span><span class="sxs-lookup"><span data-stu-id="e0f79-167">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="e0f79-168">Nástroj vhodné pro přístup k těmto zachycení soubory uložené na účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="e0f79-168">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="e0f79-169">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají na účet úložiště v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="e0f79-169">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="e0f79-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0f79-170">Next steps</span></span>

<span data-ttu-id="e0f79-171">Informace o automatizaci paketu zachytává se virtuální počítač výstrahy zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="e0f79-171">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="e0f79-172">Najít, pokud určité provoz je povolený v orr mimo váš virtuální počítač navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="e0f79-172">Find if certain traffic is allowed in orr out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->














