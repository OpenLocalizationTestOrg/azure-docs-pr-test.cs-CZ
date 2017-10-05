---
title: "Spravovat zachycení paketů s sledovací proces sítě Azure - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak spravovat funkci zachycení paketu sledovací proces sítě pomocí Azure CLI 1.0"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 91588910334859c1ea77186674d5bfb31b311b36
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="9c8d6-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9c8d6-103">Manage packet captures with Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="9c8d6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9c8d6-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="9c8d6-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9c8d6-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="9c8d6-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9c8d6-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="9c8d6-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9c8d6-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="9c8d6-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="9c8d6-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="9c8d6-109">Zachytáváním paketů sledovací proces sítě vám umožní vytvořit relace zachytávání sledovat provoz do a z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="9c8d6-110">Filtry jsou k dispozici pro relaci zachytávání zajistit, že zaznamenáte pouze provoz, který chcete.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="9c8d6-111">Při diagnostice sítě anomálií reaktivně a proaktivně pomáhá zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="9c8d6-112">Jiné účely zahrnují shromažďování statistiku sítě, získá informace o síti vniknutí, k ladění komunikaci klienta se serverem a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="9c8d6-113">Díky vzdáleně aktivovat paketu zachycení, tato funkce snižuje zátěž spuštěných zachytáváním paketů ručně a na požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="9c8d6-114">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-114">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="9c8d6-115">Tento článek vás provede úloh jiný správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-115">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="9c8d6-116">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="9c8d6-116">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="9c8d6-117">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="9c8d6-117">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="9c8d6-118">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="9c8d6-118">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="9c8d6-119">**Stáhnout zachytáváním paketů**</span><span class="sxs-lookup"><span data-stu-id="9c8d6-119">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="9c8d6-120">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9c8d6-120">Before you begin</span></span>

<span data-ttu-id="9c8d6-121">Tento článek předpokládá, že máte v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="9c8d6-121">This article assumes you have the following resources:</span></span>

- <span data-ttu-id="9c8d6-122">Instance sledovací proces sítě v oblasti, kterou chcete vytvořit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9c8d6-122">An instance of Network Watcher in the region you want to create a packet capture</span></span>
- <span data-ttu-id="9c8d6-123">Virtuální počítač s příponou zachytávání paketů povoleno.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-123">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c8d6-124">Zachytáváním paketů vyžaduje agenta, aby byl spuštěn na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-124">Packet capture requires an agent to be running on the virtual machine.</span></span> <span data-ttu-id="9c8d6-125">Agent byl nainstalován jako rozšíření.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-125">The Agent is installed as an extension.</span></span> <span data-ttu-id="9c8d6-126">Pokyny k rozšíření virtuálního počítače, navštivte [rozšíření virtuálního počítače a funkce](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="9c8d6-126">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="9c8d6-127">Instalace rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="9c8d6-127">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="9c8d6-128">Krok 1</span><span class="sxs-lookup"><span data-stu-id="9c8d6-128">Step 1</span></span>

<span data-ttu-id="9c8d6-129">Spustit `azure vm extension set` rutiny pro instalaci agenta zachytávání paketů na virtuálním počítači hosta.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-129">Run the `azure vm extension set` cmdlet to install the packet capture agent on the guest virtual machine.</span></span>

<span data-ttu-id="9c8d6-130">Pro virtuální počítače s Windows:</span><span class="sxs-lookup"><span data-stu-id="9c8d6-130">For Windows virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentWindows -o 1.4
```

<span data-ttu-id="9c8d6-131">Pro virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="9c8d6-131">For Linux virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentLinux -o 1.4
````

### <a name="step-2"></a><span data-ttu-id="9c8d6-132">Krok 2</span><span class="sxs-lookup"><span data-stu-id="9c8d6-132">Step 2</span></span>

<span data-ttu-id="9c8d6-133">Chcete-li zajistit, že je agent nainstalovaný, spusťte `vm extension get` rutiny a předejte ji názvu prostředku skupiny a virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-133">To ensure that the agent is installed, run the `vm extension get` cmdlet and pass it the resource group and virtual machine name.</span></span> <span data-ttu-id="9c8d6-134">Zkontrolujte výsledného seznamu ujistěte se, že je agent nainstalován.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-134">Check the resulting list to ensure the agent is installed.</span></span>

```azurecli
azure vm extension get -g resourceGroupName -m virtualMachineName
```

<span data-ttu-id="9c8d6-135">Následující příklad je příklad odpovědi spuštění`azure vm extension get`</span><span class="sxs-lookup"><span data-stu-id="9c8d6-135">The following sample is an example of the response from running `azure vm extension get`</span></span>

```
info:    Executing command vm extension get
+ Looking up the VM "virtualMachineName"
data:    Publisher                       Name                        Version  State
data:    ------------------------------  -----------------------     -------  ---------
data:    Microsoft.Azure.NetworkWatcher  NetworkWatcherAgentWindows  1.4      Succeeded
info:    vm extension get command OK
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="9c8d6-136">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9c8d6-136">Start a packet capture</span></span>

<span data-ttu-id="9c8d6-137">Po dokončení předchozích kroků se agent zachytávání paketů je nainstalován na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-137">Once the preceding steps are complete, the packet capture agent is installed on the virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="9c8d6-138">Krok 1</span><span class="sxs-lookup"><span data-stu-id="9c8d6-138">Step 1</span></span>

<span data-ttu-id="9c8d6-139">Dalším krokem je pro získání instance sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-139">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="9c8d6-140">Tato proměnná je předána `network watcher show` rutiny v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-140">This variable is passed to the `network watcher show` cmdlet in step 4.</span></span>

```azurecli
azure network watcher show -g resourceGroup -n networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="9c8d6-141">Krok 2</span><span class="sxs-lookup"><span data-stu-id="9c8d6-141">Step 2</span></span>

<span data-ttu-id="9c8d6-142">Načtěte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-142">Retrieve a storage account.</span></span> <span data-ttu-id="9c8d6-143">Tento účet úložiště se používá k uložení souboru zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-143">This storage account is used to store the packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="9c8d6-144">Krok 3</span><span class="sxs-lookup"><span data-stu-id="9c8d6-144">Step 3</span></span>

<span data-ttu-id="9c8d6-145">Chcete-li omezit data, která je uložená ve zachytáváním paketů použít filtry.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-145">Filters can be used to limit the data that is stored by the packet capture.</span></span> <span data-ttu-id="9c8d6-146">Následující příklad nastaví zachytáváním paketů s několika filtry.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-146">The following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="9c8d6-147">První tři filtry shromažďovat odchozí přenosy TCP pouze z místní IP 10.0.0.3 do cílové porty 20, 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-147">The first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 to destination ports 20, 80 and 443.</span></span>  <span data-ttu-id="9c8d6-148">Poslední filtr shromažďuje jenom provoz UDP.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-148">The last filter collects only UDP traffic.</span></span>

```azurecli
azure network watcher packet-capture create -g resourceGroupName -w networkWatcherName -n packetCaptureName -t targetResourceId -o storageAccountResourceId -f "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="9c8d6-149">Více filtrů lze definovat pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-149">Multiple filters can be defined for a packet capture.</span></span> <span data-ttu-id="9c8d6-150">Pokud používáte struktura složitějších filtrů, je vhodnější použít filtry jako soubor json, aby se zabránilo chyby syntaxe.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-150">If you are using a complex filter structure, it is better to use filters as a json file to avoid syntax errors.</span></span> <span data-ttu-id="9c8d6-151">Například použijte příznak "-r" (místo "-f") a předejte umístění souboru json, který obsahuje následující filtry:</span><span class="sxs-lookup"><span data-stu-id="9c8d6-151">For example, use the flag "-r" (instead of "-f") and pass the location of a json file containing the following filters:</span></span>

```json
[
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"20"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"80"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"443"
    },
    {
        "protocol":"UDP"
    }
]
```


<span data-ttu-id="9c8d6-152">V následujícím příkladu je očekávaný výstup spuštění `network watcher packet-capture create` rutiny.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-152">The following example is the expected output from running the `network watcher packet-capture create` cmdlet.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture create command OK
```

## <a name="get-a-packet-capture"></a><span data-ttu-id="9c8d6-153">Získat zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9c8d6-153">Get a packet capture</span></span>

<span data-ttu-id="9c8d6-154">Spuštění `network watcher packet-capture show` rutina načte stav paketu zachycení aktuálně spuštěné, nebo pokud byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-154">Running the `network watcher packet-capture show` cmdlet, retrieves the status of a currently running, or completed packet capture.</span></span>

```azurecli
azure network watcher packet-capture show -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

<span data-ttu-id="9c8d6-155">V následujícím příkladu je výstup z `network watcher packet-capture show` rutiny.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-155">The following example is the output from the `network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="9c8d6-156">V následujícím příkladu je po dokončení zachytávání.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-156">The following example is after the capture is complete.</span></span> <span data-ttu-id="9c8d6-157">Hodnota PacketCaptureStatus je zastavena, s StopReason TimeExceeded.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-157">The PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="9c8d6-158">Tato hodnota ukazuje, že zachytáváním paketů byla úspěšná a jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-158">This value shows that the packet capture was successful and ran its time.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture show command OK
```

## <a name="stop-a-packet-capture"></a><span data-ttu-id="9c8d6-159">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9c8d6-159">Stop a packet capture</span></span>

<span data-ttu-id="9c8d6-160">Spuštěním `network watcher packet-capture stop` rutiny, pokud zachycení relace je v průběhu je zastavena.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-160">By running the `network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
azure network watcher packet-capture stop -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="9c8d6-161">Žádná odpověď vrátí rutina při spuštěné v relaci aktuálně spuštěné zachycení nebo existující relaci, která již byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-161">The cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="9c8d6-162">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="9c8d6-162">Delete a packet capture</span></span>

```azurecli
azure network watcher packet-capture delete -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="9c8d6-163">Odstraňuje se zachytáváním paketů nedojde k odstranění souboru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-163">Deleting a packet capture does not delete the file in the storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="9c8d6-164">Stáhnout zachytáváním paketů</span><span class="sxs-lookup"><span data-stu-id="9c8d6-164">Download a packet capture</span></span>

<span data-ttu-id="9c8d6-165">Po dokončení relace zachytávání paketů můžete zaznamenat soubor odeslat do úložiště objektů blob nebo do místního souboru virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-165">Once your packet capture session has completed, the capture file can be uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="9c8d6-166">Umístění úložiště pro zachytávání paketů se definuje při vytvoření relace.</span><span class="sxs-lookup"><span data-stu-id="9c8d6-166">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="9c8d6-167">Nástroj vhodné pro přístup k těmto zachycení soubory uložené na účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="9c8d6-167">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="9c8d6-168">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají na účet úložiště v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="9c8d6-168">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="9c8d6-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c8d6-169">Next steps</span></span>

<span data-ttu-id="9c8d6-170">Informace o automatizaci paketu zachytává se virtuální počítač výstrahy zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="9c8d6-170">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="9c8d6-171">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="9c8d6-171">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
