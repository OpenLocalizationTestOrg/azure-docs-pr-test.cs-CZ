---
title: "Spravovat zachycení paketů s sledovací proces sítě Azure - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak spravovat funkci zachycení paketu sledovací proces sítě pomocí Azure CLI 2.0"
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
ms.openlocfilehash: c94eb46f31f2f19b843ccd7bf77b8a39943a07d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="8e26d-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8e26d-103">Manage packet captures with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="8e26d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8e26d-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="8e26d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e26d-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="8e26d-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8e26d-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="8e26d-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8e26d-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="8e26d-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="8e26d-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="8e26d-109">Zachytáváním paketů sledovací proces sítě vám umožní vytvořit relace zachytávání sledovat provoz do a z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8e26d-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="8e26d-110">Filtry jsou k dispozici pro relaci zachytávání zajistit, že zaznamenáte pouze provoz, který chcete.</span><span class="sxs-lookup"><span data-stu-id="8e26d-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="8e26d-111">Při diagnostice sítě anomálií reaktivně a proaktivně pomáhá zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="8e26d-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="8e26d-112">Jiné účely zahrnují shromažďování statistiku sítě, získá informace o síti vniknutí, k ladění komunikaci klienta se serverem a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="8e26d-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="8e26d-113">Díky vzdáleně aktivovat paketu zachycení, tato funkce snižuje zátěž spuštěných zachytáváním paketů ručně a na požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="8e26d-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="8e26d-114">Tento článek používá naší nové generace rozhraní příkazového řádku pro model nasazení prostředků management, Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="8e26d-114">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="8e26d-115">Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="8e26d-115">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

<span data-ttu-id="8e26d-116">Tento článek vás provede úloh jiný správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="8e26d-116">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="8e26d-117">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="8e26d-117">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="8e26d-118">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="8e26d-118">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="8e26d-119">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="8e26d-119">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="8e26d-120">**Stáhnout zachytáváním paketů**</span><span class="sxs-lookup"><span data-stu-id="8e26d-120">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="8e26d-121">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8e26d-121">Before you begin</span></span>

<span data-ttu-id="8e26d-122">Tento článek předpokládá, že máte v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="8e26d-122">This article assumes you have the following resources:</span></span>

- <span data-ttu-id="8e26d-123">Instance sledovací proces sítě v oblasti, kterou chcete vytvořit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="8e26d-123">An instance of Network Watcher in the region you want to create a packet capture</span></span>
- <span data-ttu-id="8e26d-124">Virtuální počítač s příponou zachytávání paketů povoleno.</span><span class="sxs-lookup"><span data-stu-id="8e26d-124">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8e26d-125">Zachytáváním paketů vyžaduje agenta, aby byl spuštěn na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8e26d-125">Packet capture requires an agent to be running on the virtual machine.</span></span> <span data-ttu-id="8e26d-126">Agent byl nainstalován jako rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8e26d-126">The Agent is installed as an extension.</span></span> <span data-ttu-id="8e26d-127">Pokyny k rozšíření virtuálního počítače, navštivte [rozšíření virtuálního počítače a funkce](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="8e26d-127">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="8e26d-128">Instalace rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8e26d-128">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="8e26d-129">Krok 1</span><span class="sxs-lookup"><span data-stu-id="8e26d-129">Step 1</span></span>

<span data-ttu-id="8e26d-130">Spustit `az vm extension set` rutiny pro instalaci agenta zachytávání paketů na virtuálním počítači hosta.</span><span class="sxs-lookup"><span data-stu-id="8e26d-130">Run the `az vm extension set` cmdlet to install the packet capture agent on the guest virtual machine.</span></span>

<span data-ttu-id="8e26d-131">Pro virtuální počítače s Windows:</span><span class="sxs-lookup"><span data-stu-id="8e26d-131">For Windows virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

<span data-ttu-id="8e26d-132">Pro virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="8e26d-132">For Linux virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a><span data-ttu-id="8e26d-133">Krok 2</span><span class="sxs-lookup"><span data-stu-id="8e26d-133">Step 2</span></span>

<span data-ttu-id="8e26d-134">Chcete-li zajistit, že je agent nainstalovaný, spusťte `vm extension get` rutiny a předejte ji názvu prostředku skupiny a virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8e26d-134">To ensure that the agent is installed, run the `vm extension get` cmdlet and pass it the resource group and virtual machine name.</span></span> <span data-ttu-id="8e26d-135">Zkontrolujte výsledného seznamu ujistěte se, že je agent nainstalován.</span><span class="sxs-lookup"><span data-stu-id="8e26d-135">Check the resulting list to ensure the agent is installed.</span></span>

```azurecli
az vm extension show -resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

<span data-ttu-id="8e26d-136">Následující příklad je příklad odpovědi spuštění`az vm extension show`</span><span class="sxs-lookup"><span data-stu-id="8e26d-136">The following sample is an example of the response from running `az vm extension show`</span></span>

```json
{
  "autoUpgradeMinorVersion": true,
  "forceUpdateTag": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/extensions/NetworkWatcherAgentWindows",
  "instanceView": null,
  "location": "westcentralus",
  "name": "NetworkWatcherAgentWindows",
  "protectedSettings": null,
  "provisioningState": "Succeeded",
  "publisher": "Microsoft.Azure.NetworkWatcher",
  "resourceGroup": "{resourceGroupName}",
  "settings": null,
  "tags": null,
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "typeHandlerVersion": "1.4",
  "virtualMachineExtensionType": "NetworkWatcherAgentWindows"
}
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="8e26d-137">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="8e26d-137">Start a packet capture</span></span>

<span data-ttu-id="8e26d-138">Po dokončení předchozích kroků se agent zachytávání paketů je nainstalován na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8e26d-138">Once the preceding steps are complete, the packet capture agent is installed on the virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="8e26d-139">Krok 1</span><span class="sxs-lookup"><span data-stu-id="8e26d-139">Step 1</span></span>

<span data-ttu-id="8e26d-140">Dalším krokem je pro získání instance sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="8e26d-140">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="8e26d-141">Předaný název Tnelze sledovací proces sítě `az network watcher show` rutiny v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="8e26d-141">TThe name of the Network Watcher is passed to the `az network watcher show` cmdlet in step 4.</span></span>

```azurecli
az network watcher show -resource-group resourceGroup -name networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="8e26d-142">Krok 2</span><span class="sxs-lookup"><span data-stu-id="8e26d-142">Step 2</span></span>

<span data-ttu-id="8e26d-143">Načtěte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="8e26d-143">Retrieve a storage account.</span></span> <span data-ttu-id="8e26d-144">Tento účet úložiště se používá k uložení souboru zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="8e26d-144">This storage account is used to store the packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="8e26d-145">Krok 3</span><span class="sxs-lookup"><span data-stu-id="8e26d-145">Step 3</span></span>

<span data-ttu-id="8e26d-146">Chcete-li omezit data, která je uložená ve zachytáváním paketů použít filtry.</span><span class="sxs-lookup"><span data-stu-id="8e26d-146">Filters can be used to limit the data that is stored by the packet capture.</span></span> <span data-ttu-id="8e26d-147">Následující příklad nastaví zachytáváním paketů s několika filtry.</span><span class="sxs-lookup"><span data-stu-id="8e26d-147">The following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="8e26d-148">První tři filtry shromažďovat odchozí přenosy TCP pouze z místní IP 10.0.0.3 do cílové porty 20, 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="8e26d-148">The first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 to destination ports 20, 80 and 443.</span></span>  <span data-ttu-id="8e26d-149">Poslední filtr shromažďuje jenom provoz UDP.</span><span class="sxs-lookup"><span data-stu-id="8e26d-149">The last filter collects only UDP traffic.</span></span>

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="8e26d-150">V následujícím příkladu je očekávaný výstup spuštění `az network watcher packet-capture create` rutiny.</span><span class="sxs-lookup"><span data-stu-id="8e26d-150">The following example is the expected output from running the `az network watcher packet-capture create` cmdlet.</span></span>

```json
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/p
roviders/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapture_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="get-a-packet-capture"></a><span data-ttu-id="8e26d-151">Získat zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="8e26d-151">Get a packet capture</span></span>

<span data-ttu-id="8e26d-152">Spuštění `az network watcher packet-capture show` rutina načte stav paketu zachycení aktuálně spuštěné, nebo pokud byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="8e26d-152">Running the `az network watcher packet-capture show` cmdlet, retrieves the status of a currently running, or completed packet capture.</span></span>

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

<span data-ttu-id="8e26d-153">V následujícím příkladu je výstup z `az network watcher packet-capture show` rutiny.</span><span class="sxs-lookup"><span data-stu-id="8e26d-153">The following example is the output from the `az network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="8e26d-154">V následujícím příkladu je po dokončení zachytávání.</span><span class="sxs-lookup"><span data-stu-id="8e26d-154">The following example is after the capture is complete.</span></span> <span data-ttu-id="8e26d-155">Hodnota PacketCaptureStatus je zastavena, s StopReason TimeExceeded.</span><span class="sxs-lookup"><span data-stu-id="8e26d-155">The PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="8e26d-156">Tato hodnota ukazuje, že zachytáváním paketů byla úspěšná a jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="8e26d-156">This value shows that the packet capture was successful and ran its time.</span></span>

```
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/providers/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapt
ure_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="stop-a-packet-capture"></a><span data-ttu-id="8e26d-157">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="8e26d-157">Stop a packet capture</span></span>

<span data-ttu-id="8e26d-158">Spuštěním `az network watcher packet-capture stop` rutiny, pokud zachycení relace je v průběhu je zastavena.</span><span class="sxs-lookup"><span data-stu-id="8e26d-158">By running the `az network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="8e26d-159">Žádná odpověď vrátí rutina při spuštěné v relaci aktuálně spuštěné zachycení nebo existující relaci, která již byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="8e26d-159">The cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="8e26d-160">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="8e26d-160">Delete a packet capture</span></span>

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="8e26d-161">Odstraňuje se zachytáváním paketů nedojde k odstranění souboru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8e26d-161">Deleting a packet capture does not delete the file in the storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="8e26d-162">Stáhnout zachytáváním paketů</span><span class="sxs-lookup"><span data-stu-id="8e26d-162">Download a packet capture</span></span>

<span data-ttu-id="8e26d-163">Po dokončení relace zachytávání paketů můžete zaznamenat soubor odeslat do úložiště objektů blob nebo do místního souboru virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8e26d-163">Once your packet capture session has completed, the capture file can be uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="8e26d-164">Umístění úložiště pro zachytávání paketů se definuje při vytvoření relace.</span><span class="sxs-lookup"><span data-stu-id="8e26d-164">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="8e26d-165">Nástroj vhodné pro přístup k těmto zachycení soubory uložené na účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="8e26d-165">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="8e26d-166">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají na účet úložiště v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="8e26d-166">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="8e26d-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e26d-167">Next steps</span></span>

<span data-ttu-id="8e26d-168">Informace o automatizaci paketu zachytává se virtuální počítač výstrahy zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="8e26d-168">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="8e26d-169">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="8e26d-169">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
