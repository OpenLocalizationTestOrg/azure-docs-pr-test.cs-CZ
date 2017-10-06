---
title: "paket aaaManage zachytávali sledovací proces sítě Azure - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toomanage hello funkce zachytávání paketů sledovací proces sítě pomocí Azure CLI 2.0"
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
ms.openlocfilehash: d19cb7d0ca3b7a9bc0546859e07ef6d4df4f42d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="ea9e6-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ea9e6-103">Manage packet captures with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="ea9e6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ea9e6-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="ea9e6-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ea9e6-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="ea9e6-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ea9e6-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="ea9e6-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ea9e6-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="ea9e6-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="ea9e6-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="ea9e6-109">Zachytáváním paketů sledovací proces sítě vám umožní toocreate zaznamenání relace tootrack provoz tooand z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="ea9e6-110">Filtry jsou podle hello zaznamenání relace tooensure že zaznamenáte jenom provoz hello, které chcete.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="ea9e6-111">Zachytáváním paketů pomáhá toodiagnose sítě anomálií reaktivně a proaktivně.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="ea9e6-112">Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="ea9e6-113">Tím, že je schopný tooremotely aktivační událost paketu zachycení, tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně a hello požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="ea9e6-114">Tento článek používá naší nové generace rozhraní příkazového řádku pro model nasazení hello prostředků management, Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-114">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="ea9e6-115">tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="ea9e6-115">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

<span data-ttu-id="ea9e6-116">Tento článek vás provede hello úlohy různých správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-116">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="ea9e6-117">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="ea9e6-117">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="ea9e6-118">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="ea9e6-118">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="ea9e6-119">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="ea9e6-119">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="ea9e6-120">**Stáhnout zachytáváním paketů**</span><span class="sxs-lookup"><span data-stu-id="ea9e6-120">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="ea9e6-121">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ea9e6-121">Before you begin</span></span>

<span data-ttu-id="ea9e6-122">Tento článek předpokládá, že máte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="ea9e6-122">This article assumes you have hello following resources:</span></span>

- <span data-ttu-id="ea9e6-123">Instance sledovací proces sítě v hello oblasti, kterou chcete toocreate zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="ea9e6-123">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>
- <span data-ttu-id="ea9e6-124">Virtuální počítač s hello paketu zachytit rozšíření povolené.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-124">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ea9e6-125">Zachytáváním paketů vyžaduje toobe agenta spuštěny na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-125">Packet capture requires an agent toobe running on hello virtual machine.</span></span> <span data-ttu-id="ea9e6-126">Hello agenta je nainstalován jako rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-126">hello Agent is installed as an extension.</span></span> <span data-ttu-id="ea9e6-127">Pokyny k rozšíření virtuálního počítače, navštivte [rozšíření virtuálního počítače a funkce](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="ea9e6-127">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="ea9e6-128">Instalace rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ea9e6-128">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="ea9e6-129">Krok 1</span><span class="sxs-lookup"><span data-stu-id="ea9e6-129">Step 1</span></span>

<span data-ttu-id="ea9e6-130">Spustit hello `az vm extension set` rutiny tooinstall hello paketu zachycení agenta na virtuálním počítači hosta hello.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-130">Run hello `az vm extension set` cmdlet tooinstall hello packet capture agent on hello guest virtual machine.</span></span>

<span data-ttu-id="ea9e6-131">Pro virtuální počítače s Windows:</span><span class="sxs-lookup"><span data-stu-id="ea9e6-131">For Windows virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

<span data-ttu-id="ea9e6-132">Pro virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="ea9e6-132">For Linux virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a><span data-ttu-id="ea9e6-133">Krok 2</span><span class="sxs-lookup"><span data-stu-id="ea9e6-133">Step 2</span></span>

<span data-ttu-id="ea9e6-134">tooensure, který hello agenta nainstalovat, spustit hello `vm extension get` rutiny a předejte ji hello skupinu prostředků a název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-134">tooensure that hello agent is installed, run hello `vm extension get` cmdlet and pass it hello resource group and virtual machine name.</span></span> <span data-ttu-id="ea9e6-135">Zkontrolujte, zda je nainstalován agent hello tooensure výsledný seznam hello.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-135">Check hello resulting list tooensure hello agent is installed.</span></span>

```azurecli
az vm extension show -resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

<span data-ttu-id="ea9e6-136">Následující ukázka Hello je příkladem hello odpověď od spuštění`az vm extension show`</span><span class="sxs-lookup"><span data-stu-id="ea9e6-136">hello following sample is an example of hello response from running `az vm extension show`</span></span>

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

## <a name="start-a-packet-capture"></a><span data-ttu-id="ea9e6-137">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="ea9e6-137">Start a packet capture</span></span>

<span data-ttu-id="ea9e6-138">Jakmile hello předchozí kroky jsou dokončeny, hello paketů zachycení agent je nainstalován na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-138">Once hello preceding steps are complete, hello packet capture agent is installed on hello virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="ea9e6-139">Krok 1</span><span class="sxs-lookup"><span data-stu-id="ea9e6-139">Step 1</span></span>

<span data-ttu-id="ea9e6-140">dalším krokem Hello je tooretrieve hello sledovací proces sítě instance.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-140">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="ea9e6-141">Název Tnelze hello sledovací proces sítě je předán toohello `az network watcher show` rutiny v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-141">TThe name of hello Network Watcher is passed toohello `az network watcher show` cmdlet in step 4.</span></span>

```azurecli
az network watcher show -resource-group resourceGroup -name networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="ea9e6-142">Krok 2</span><span class="sxs-lookup"><span data-stu-id="ea9e6-142">Step 2</span></span>

<span data-ttu-id="ea9e6-143">Načtěte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-143">Retrieve a storage account.</span></span> <span data-ttu-id="ea9e6-144">Tento účet úložiště je použité toostore hello paketu zachycení soubor.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-144">This storage account is used toostore hello packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="ea9e6-145">Krok 3</span><span class="sxs-lookup"><span data-stu-id="ea9e6-145">Step 3</span></span>

<span data-ttu-id="ea9e6-146">Filtry lze použít toolimit hello data uložená ve zachytáváním paketů hello.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-146">Filters can be used toolimit hello data that is stored by hello packet capture.</span></span> <span data-ttu-id="ea9e6-147">Hello následující příklad nastaví zachytáváním paketů s několika filtry.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-147">hello following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="ea9e6-148">Hello první tři filtry shromažďovat odchozí přenosy TCP pouze z místní IP 10.0.0.3 toodestination porty 20, 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-148">hello first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 toodestination ports 20, 80 and 443.</span></span>  <span data-ttu-id="ea9e6-149">poslední filtru Hello shromažďuje jenom provoz UDP.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-149">hello last filter collects only UDP traffic.</span></span>

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="ea9e6-150">Hello následující příklad je hello očekávaný výstup z systémem hello `az network watcher packet-capture create` rutiny.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-150">hello following example is hello expected output from running hello `az network watcher packet-capture create` cmdlet.</span></span>

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

## <a name="get-a-packet-capture"></a><span data-ttu-id="ea9e6-151">Získat zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="ea9e6-151">Get a packet capture</span></span>

<span data-ttu-id="ea9e6-152">Spuštění hello `az network watcher packet-capture show` rutina, načte stav hello paketu zachycení aktuálně spuštěné, nebo pokud byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-152">Running hello `az network watcher packet-capture show` cmdlet, retrieves hello status of a currently running, or completed packet capture.</span></span>

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

<span data-ttu-id="ea9e6-153">Hello následující příklad je hello výstup hello `az network watcher packet-capture show` rutiny.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-153">hello following example is hello output from hello `az network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="ea9e6-154">Hello následující příklad je po dokončení hello zachycení.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-154">hello following example is after hello capture is complete.</span></span> <span data-ttu-id="ea9e6-155">StopReason TimeExceeded je zastavena Hello PacketCaptureStatus hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-155">hello PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="ea9e6-156">Tato hodnota ukazuje, že zachytáváním paketů hello byla úspěšná a jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-156">This value shows that hello packet capture was successful and ran its time.</span></span>

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

## <a name="stop-a-packet-capture"></a><span data-ttu-id="ea9e6-157">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="ea9e6-157">Stop a packet capture</span></span>

<span data-ttu-id="ea9e6-158">Spuštěním hello `az network watcher packet-capture stop` rutiny, pokud zachycení relace je v průběhu je zastavena.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-158">By running hello `az network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="ea9e6-159">žádná odpověď vrátí rutina Hello při spuštěné v relaci aktuálně spuštěné zachycení nebo existující relaci, která již byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-159">hello cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="ea9e6-160">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="ea9e6-160">Delete a packet capture</span></span>

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="ea9e6-161">Odstraňuje se zachytáváním paketů nedojde k odstranění hello souboru v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-161">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="ea9e6-162">Stáhnout zachytáváním paketů</span><span class="sxs-lookup"><span data-stu-id="ea9e6-162">Download a packet capture</span></span>

<span data-ttu-id="ea9e6-163">Po dokončení relace zachytávání paketů hello zachycení soubor může být nahrané tooblob úložiště nebo tooa místního souboru na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-163">Once your packet capture session has completed, hello capture file can be uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="ea9e6-164">umístění úložiště Hello zachytáváním paketů hello se definuje při vytvoření relace hello.</span><span class="sxs-lookup"><span data-stu-id="ea9e6-164">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="ea9e6-165">Vhodné nástroje tooaccess tyto zaznamenat soubory uložené tooa účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="ea9e6-165">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="ea9e6-166">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají tooa účet úložiště v hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="ea9e6-166">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="ea9e6-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ea9e6-167">Next steps</span></span>

<span data-ttu-id="ea9e6-168">Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="ea9e6-168">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="ea9e6-169">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ea9e6-169">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
