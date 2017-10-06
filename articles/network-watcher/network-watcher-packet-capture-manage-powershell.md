---
title: "paket aaaManage zachytávali sledovací proces sítě Azure – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak pro zachytávání paketů hello toomanage funkce sledovací proces sítě pomocí prostředí PowerShell"
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
ms.openlocfilehash: 77a522a1b05e020a73ba7140c1410615eb8761da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="4b5c1-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b5c1-103">Manage packet captures with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="4b5c1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4b5c1-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="4b5c1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b5c1-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="4b5c1-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4b5c1-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="4b5c1-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4b5c1-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="4b5c1-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="4b5c1-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="4b5c1-109">Zachytáváním paketů sledovací proces sítě vám umožní toocreate zaznamenání relace tootrack provoz tooand z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="4b5c1-110">Filtry jsou podle hello zaznamenání relace tooensure že zaznamenáte jenom provoz hello, které chcete.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="4b5c1-111">Zachytáváním paketů pomáhá toodiagnose sítě anomálií reaktivně a proaktivně.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="4b5c1-112">Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="4b5c1-113">Tím, že je schopný tooremotely aktivační událost paketu zachycení, tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně a hello požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="4b5c1-114">Tento článek vás provede hello úlohy různých správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-114">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="4b5c1-115">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="4b5c1-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="4b5c1-116">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="4b5c1-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="4b5c1-117">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="4b5c1-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="4b5c1-118">**Stáhnout zachytáváním paketů**</span><span class="sxs-lookup"><span data-stu-id="4b5c1-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="4b5c1-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4b5c1-119">Before you begin</span></span>

<span data-ttu-id="4b5c1-120">Tento článek předpokládá, že máte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="4b5c1-120">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="4b5c1-121">Instance sledovací proces sítě v hello oblasti, kterou chcete toocreate zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="4b5c1-121">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>

* <span data-ttu-id="4b5c1-122">Virtuální počítač s hello paketu zachytit rozšíření povolené.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-122">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4b5c1-123">Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="4b5c1-124">Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="4b5c1-124">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="4b5c1-125">Instalace rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4b5c1-125">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="4b5c1-126">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4b5c1-126">Step 1</span></span>

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a><span data-ttu-id="4b5c1-127">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4b5c1-127">Step 2</span></span>

<span data-ttu-id="4b5c1-128">Hello následující příklad načte hello informace o rozšíření potřeby toorun hello `Set-AzureRmVMExtension` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-128">hello following example retrieves hello extension information needed toorun hello `Set-AzureRmVMExtension` cmdlet.</span></span> <span data-ttu-id="4b5c1-129">Tato rutina nainstaluje agenta zachytávání paketů hello na hello hostovaného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-129">This cmdlet installs hello packet capture agent on hello guest virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="4b5c1-130">Hello `Set-AzureRmVMExtension` rutiny může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-130">hello `Set-AzureRmVMExtension` cmdlet may take several minutes toocomplete.</span></span>

<span data-ttu-id="4b5c1-131">Pro virtuální počítače s Windows:</span><span class="sxs-lookup"><span data-stu-id="4b5c1-131">For Windows virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

<span data-ttu-id="4b5c1-132">Pro virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="4b5c1-132">For Linux virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

<span data-ttu-id="4b5c1-133">Hello následující příklad je úspěšné odpovědi po spuštění hello `Set-AzureRmVMExtension` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-133">hello following example is a successful response after running hello `Set-AzureRmVMExtension` cmdlet.</span></span>

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a><span data-ttu-id="4b5c1-134">Krok 3</span><span class="sxs-lookup"><span data-stu-id="4b5c1-134">Step 3</span></span>

<span data-ttu-id="4b5c1-135">tooensure, který hello agenta nainstalovat, spustit hello `Get-AzureRmVMExtension` rutiny a předejte ji hello název virtuálního počítače a název rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-135">tooensure that hello agent is installed, run hello `Get-AzureRmVMExtension` cmdlet and pass it hello virtual machine name and hello extension name.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

<span data-ttu-id="4b5c1-136">Následující ukázka Hello je příkladem hello odpověď od spuštění`Get-AzureRmVMExtension`</span><span class="sxs-lookup"><span data-stu-id="4b5c1-136">hello following sample is an example of hello response from running `Get-AzureRmVMExtension`</span></span>

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

## <a name="start-a-packet-capture"></a><span data-ttu-id="4b5c1-137">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="4b5c1-137">Start a packet capture</span></span>

<span data-ttu-id="4b5c1-138">Jakmile hello předchozí kroky jsou dokončeny, hello paketů zachycení agent je nainstalován na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-138">Once hello preceding steps are complete, hello packet capture agent is installed on hello virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="4b5c1-139">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4b5c1-139">Step 1</span></span>

<span data-ttu-id="4b5c1-140">dalším krokem Hello je tooretrieve hello sledovací proces sítě instance.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-140">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="4b5c1-141">Tato proměnná je předán toohello `New-AzureRmNetworkWatcherPacketCapture` rutiny v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-141">This variable is passed toohello `New-AzureRmNetworkWatcherPacketCapture` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a><span data-ttu-id="4b5c1-142">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4b5c1-142">Step 2</span></span>

<span data-ttu-id="4b5c1-143">Načtěte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-143">Retrieve a storage account.</span></span> <span data-ttu-id="4b5c1-144">Tento účet úložiště je použité toostore hello paketu zachycení soubor.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-144">This storage account is used toostore hello packet capture file.</span></span>

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a><span data-ttu-id="4b5c1-145">Krok 3</span><span class="sxs-lookup"><span data-stu-id="4b5c1-145">Step 3</span></span>

<span data-ttu-id="4b5c1-146">Filtry lze použít toolimit hello data uložená ve zachytáváním paketů hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-146">Filters can be used toolimit hello data that is stored by hello packet capture.</span></span> <span data-ttu-id="4b5c1-147">Hello následující příklad nastaví dva filtry.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-147">hello following example sets up two filters.</span></span>  <span data-ttu-id="4b5c1-148">Shromažďuje jeden filtr protokolu TCP odchozí provoz jenom z místní IP 10.0.0.3 toodestination porty 20, 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-148">One filter collects outgoing TCP traffic only from local IP 10.0.0.3 toodestination ports 20, 80 and 443.</span></span>  <span data-ttu-id="4b5c1-149">druhý filtr Hello shromažďuje jenom provoz UDP.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-149">hello second filter collects only UDP traffic.</span></span>

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> <span data-ttu-id="4b5c1-150">Více filtrů lze definovat pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-150">Multiple filters can be defined for a packet capture.</span></span>

### <a name="step-4"></a><span data-ttu-id="4b5c1-151">Krok 4</span><span class="sxs-lookup"><span data-stu-id="4b5c1-151">Step 4</span></span>

<span data-ttu-id="4b5c1-152">Spustit hello `New-AzureRmNetworkWatcherPacketCapture` rutiny toostart hello paketu proces zachycení, předávání hello požadované hodnoty získané v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-152">Run hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet toostart hello packet capture process, passing hello required values retrieved in hello preceding steps.</span></span>
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filters $filter1, $filter2
```

<span data-ttu-id="4b5c1-153">Hello následující příklad je hello očekávaný výstup z systémem hello `New-AzureRmNetworkWatcherPacketCapture` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-153">hello following example is hello expected output from running hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span>

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

## <a name="get-a-packet-capture"></a><span data-ttu-id="4b5c1-154">Získat zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="4b5c1-154">Get a packet capture</span></span>

<span data-ttu-id="4b5c1-155">Spuštění hello `Get-AzureRmNetworkWatcherPacketCapture` rutina, načte stav hello paketu zachycení aktuálně spuštěné, nebo pokud byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-155">Running hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, retrieves hello status of a currently running, or completed packet capture.</span></span>

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

<span data-ttu-id="4b5c1-156">Hello následující příklad je hello výstup hello `Get-AzureRmNetworkWatcherPacketCapture` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-156">hello following example is hello output from hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span> <span data-ttu-id="4b5c1-157">Hello následující příklad je po dokončení hello zachycení.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-157">hello following example is after hello capture is complete.</span></span> <span data-ttu-id="4b5c1-158">StopReason TimeExceeded je zastavena Hello PacketCaptureStatus hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-158">hello PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="4b5c1-159">Tato hodnota ukazuje, že zachytáváním paketů hello byla úspěšná a jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-159">This value shows that hello packet capture was successful and ran its time.</span></span>
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

## <a name="stop-a-packet-capture"></a><span data-ttu-id="4b5c1-160">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="4b5c1-160">Stop a packet capture</span></span>

<span data-ttu-id="4b5c1-161">Spuštěním hello `Stop-AzureRmNetworkWatcherPacketCapture` rutiny, pokud zachycení relace je v průběhu je zastavena.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-161">By running hello `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, if a capture session is in progress it is stopped.</span></span>

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="4b5c1-162">žádná odpověď vrátí rutina Hello při spuštěné v relaci aktuálně spuštěné zachycení nebo existující relaci, která již byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-162">hello cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="4b5c1-163">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="4b5c1-163">Delete a packet capture</span></span>

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="4b5c1-164">Odstraňuje se zachytáváním paketů nedojde k odstranění hello souboru v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-164">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="4b5c1-165">Stáhnout zachytáváním paketů</span><span class="sxs-lookup"><span data-stu-id="4b5c1-165">Download a packet capture</span></span>

<span data-ttu-id="4b5c1-166">Po dokončení relace zachytávání paketů hello zachycení soubor může být nahrané tooblob úložiště nebo tooa místního souboru na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-166">Once your packet capture session has completed, hello capture file can be uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="4b5c1-167">umístění úložiště Hello zachytáváním paketů hello se definuje při vytvoření relace hello.</span><span class="sxs-lookup"><span data-stu-id="4b5c1-167">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="4b5c1-168">Vhodné nástroje tooaccess tyto zaznamenat soubory uložené tooa účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="4b5c1-168">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="4b5c1-169">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají tooa účet úložiště v hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="4b5c1-169">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="4b5c1-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b5c1-170">Next steps</span></span>

<span data-ttu-id="4b5c1-171">Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="4b5c1-171">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="4b5c1-172">Najít, pokud určité provoz je povolený v orr mimo váš virtuální počítač navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="4b5c1-172">Find if certain traffic is allowed in orr out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->














