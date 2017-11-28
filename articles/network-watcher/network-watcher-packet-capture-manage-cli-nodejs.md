---
title: "paket aaaManage zachytávali sledovací proces sítě Azure - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toomanage hello funkce zachytávání paketů sledovací proces sítě pomocí Azure CLI 1.0"
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
ms.openlocfilehash: c4b710a8d82ccaaf65876a8c2ef845aa97b5f831
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="bd75c-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="bd75c-103">Manage packet captures with Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="bd75c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bd75c-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="bd75c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd75c-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="bd75c-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="bd75c-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="bd75c-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bd75c-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="bd75c-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="bd75c-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="bd75c-109">Zachytáváním paketů sledovací proces sítě vám umožní toocreate zaznamenání relace tootrack provoz tooand z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bd75c-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="bd75c-110">Filtry jsou podle hello zaznamenání relace tooensure že zaznamenáte jenom provoz hello, které chcete.</span><span class="sxs-lookup"><span data-stu-id="bd75c-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="bd75c-111">Zachytáváním paketů pomáhá toodiagnose sítě anomálií reaktivně a proaktivně.</span><span class="sxs-lookup"><span data-stu-id="bd75c-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="bd75c-112">Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="bd75c-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="bd75c-113">Tím, že je schopný tooremotely aktivační událost paketu zachycení, tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně a hello požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="bd75c-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="bd75c-114">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="bd75c-114">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="bd75c-115">Tento článek vás provede hello úlohy různých správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="bd75c-115">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="bd75c-116">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="bd75c-116">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="bd75c-117">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="bd75c-117">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="bd75c-118">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="bd75c-118">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="bd75c-119">**Stáhnout zachytáváním paketů**</span><span class="sxs-lookup"><span data-stu-id="bd75c-119">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="bd75c-120">Než začnete</span><span class="sxs-lookup"><span data-stu-id="bd75c-120">Before you begin</span></span>

<span data-ttu-id="bd75c-121">Tento článek předpokládá, že máte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="bd75c-121">This article assumes you have hello following resources:</span></span>

- <span data-ttu-id="bd75c-122">Instance sledovací proces sítě v hello oblasti, kterou chcete toocreate zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="bd75c-122">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>
- <span data-ttu-id="bd75c-123">Virtuální počítač s hello paketu zachytit rozšíření povolené.</span><span class="sxs-lookup"><span data-stu-id="bd75c-123">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bd75c-124">Zachytáváním paketů vyžaduje toobe agenta spuštěny na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="bd75c-124">Packet capture requires an agent toobe running on hello virtual machine.</span></span> <span data-ttu-id="bd75c-125">Hello agenta je nainstalován jako rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bd75c-125">hello Agent is installed as an extension.</span></span> <span data-ttu-id="bd75c-126">Pokyny k rozšíření virtuálního počítače, navštivte [rozšíření virtuálního počítače a funkce](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="bd75c-126">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="bd75c-127">Instalace rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bd75c-127">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="bd75c-128">Krok 1</span><span class="sxs-lookup"><span data-stu-id="bd75c-128">Step 1</span></span>

<span data-ttu-id="bd75c-129">Spustit hello `azure vm extension set` rutiny tooinstall hello paketu zachycení agenta na virtuálním počítači hosta hello.</span><span class="sxs-lookup"><span data-stu-id="bd75c-129">Run hello `azure vm extension set` cmdlet tooinstall hello packet capture agent on hello guest virtual machine.</span></span>

<span data-ttu-id="bd75c-130">Pro virtuální počítače s Windows:</span><span class="sxs-lookup"><span data-stu-id="bd75c-130">For Windows virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentWindows -o 1.4
```

<span data-ttu-id="bd75c-131">Pro virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="bd75c-131">For Linux virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentLinux -o 1.4
````

### <a name="step-2"></a><span data-ttu-id="bd75c-132">Krok 2</span><span class="sxs-lookup"><span data-stu-id="bd75c-132">Step 2</span></span>

<span data-ttu-id="bd75c-133">tooensure, který hello agenta nainstalovat, spustit hello `vm extension get` rutiny a předejte ji hello skupinu prostředků a název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bd75c-133">tooensure that hello agent is installed, run hello `vm extension get` cmdlet and pass it hello resource group and virtual machine name.</span></span> <span data-ttu-id="bd75c-134">Zkontrolujte, zda je nainstalován agent hello tooensure výsledný seznam hello.</span><span class="sxs-lookup"><span data-stu-id="bd75c-134">Check hello resulting list tooensure hello agent is installed.</span></span>

```azurecli
azure vm extension get -g resourceGroupName -m virtualMachineName
```

<span data-ttu-id="bd75c-135">Následující ukázka Hello je příkladem hello odpověď od spuštění`azure vm extension get`</span><span class="sxs-lookup"><span data-stu-id="bd75c-135">hello following sample is an example of hello response from running `azure vm extension get`</span></span>

```
info:    Executing command vm extension get
+ Looking up hello VM "virtualMachineName"
data:    Publisher                       Name                        Version  State
data:    ------------------------------  -----------------------     -------  ---------
data:    Microsoft.Azure.NetworkWatcher  NetworkWatcherAgentWindows  1.4      Succeeded
info:    vm extension get command OK
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="bd75c-136">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="bd75c-136">Start a packet capture</span></span>

<span data-ttu-id="bd75c-137">Jakmile hello předchozí kroky jsou dokončeny, hello paketů zachycení agent je nainstalován na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="bd75c-137">Once hello preceding steps are complete, hello packet capture agent is installed on hello virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="bd75c-138">Krok 1</span><span class="sxs-lookup"><span data-stu-id="bd75c-138">Step 1</span></span>

<span data-ttu-id="bd75c-139">dalším krokem Hello je tooretrieve hello sledovací proces sítě instance.</span><span class="sxs-lookup"><span data-stu-id="bd75c-139">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="bd75c-140">Tato proměnná je předán toohello `network watcher show` rutiny v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="bd75c-140">This variable is passed toohello `network watcher show` cmdlet in step 4.</span></span>

```azurecli
azure network watcher show -g resourceGroup -n networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="bd75c-141">Krok 2</span><span class="sxs-lookup"><span data-stu-id="bd75c-141">Step 2</span></span>

<span data-ttu-id="bd75c-142">Načtěte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="bd75c-142">Retrieve a storage account.</span></span> <span data-ttu-id="bd75c-143">Tento účet úložiště je použité toostore hello paketu zachycení soubor.</span><span class="sxs-lookup"><span data-stu-id="bd75c-143">This storage account is used toostore hello packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="bd75c-144">Krok 3</span><span class="sxs-lookup"><span data-stu-id="bd75c-144">Step 3</span></span>

<span data-ttu-id="bd75c-145">Filtry lze použít toolimit hello data uložená ve zachytáváním paketů hello.</span><span class="sxs-lookup"><span data-stu-id="bd75c-145">Filters can be used toolimit hello data that is stored by hello packet capture.</span></span> <span data-ttu-id="bd75c-146">Hello následující příklad nastaví zachytáváním paketů s několika filtry.</span><span class="sxs-lookup"><span data-stu-id="bd75c-146">hello following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="bd75c-147">Hello první tři filtry shromažďovat odchozí přenosy TCP pouze z místní IP 10.0.0.3 toodestination porty 20, 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="bd75c-147">hello first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 toodestination ports 20, 80 and 443.</span></span>  <span data-ttu-id="bd75c-148">poslední filtru Hello shromažďuje jenom provoz UDP.</span><span class="sxs-lookup"><span data-stu-id="bd75c-148">hello last filter collects only UDP traffic.</span></span>

```azurecli
azure network watcher packet-capture create -g resourceGroupName -w networkWatcherName -n packetCaptureName -t targetResourceId -o storageAccountResourceId -f "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="bd75c-149">Více filtrů lze definovat pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="bd75c-149">Multiple filters can be defined for a packet capture.</span></span> <span data-ttu-id="bd75c-150">Pokud používáte struktura složitějších filtrů, je lepší toouse filtry jako chyby syntaxe tooavoid souboru json.</span><span class="sxs-lookup"><span data-stu-id="bd75c-150">If you are using a complex filter structure, it is better toouse filters as a json file tooavoid syntax errors.</span></span> <span data-ttu-id="bd75c-151">Například použijte příznak hello "-r" (místo "-f") a předejte hello umístění souboru json, který obsahuje hello následující filtry:</span><span class="sxs-lookup"><span data-stu-id="bd75c-151">For example, use hello flag "-r" (instead of "-f") and pass hello location of a json file containing hello following filters:</span></span>

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


<span data-ttu-id="bd75c-152">Hello následující příklad je hello očekávaný výstup z systémem hello `network watcher packet-capture create` rutiny.</span><span class="sxs-lookup"><span data-stu-id="bd75c-152">hello following example is hello expected output from running hello `network watcher packet-capture create` cmdlet.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes tooCapture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture create command OK
```

## <a name="get-a-packet-capture"></a><span data-ttu-id="bd75c-153">Získat zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="bd75c-153">Get a packet capture</span></span>

<span data-ttu-id="bd75c-154">Spuštění hello `network watcher packet-capture show` rutina, načte stav hello paketu zachycení aktuálně spuštěné, nebo pokud byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="bd75c-154">Running hello `network watcher packet-capture show` cmdlet, retrieves hello status of a currently running, or completed packet capture.</span></span>

```azurecli
azure network watcher packet-capture show -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

<span data-ttu-id="bd75c-155">Hello následující příklad je hello výstup hello `network watcher packet-capture show` rutiny.</span><span class="sxs-lookup"><span data-stu-id="bd75c-155">hello following example is hello output from hello `network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="bd75c-156">Hello následující příklad je po dokončení hello zachycení.</span><span class="sxs-lookup"><span data-stu-id="bd75c-156">hello following example is after hello capture is complete.</span></span> <span data-ttu-id="bd75c-157">StopReason TimeExceeded je zastavena Hello PacketCaptureStatus hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bd75c-157">hello PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="bd75c-158">Tato hodnota ukazuje, že zachytáváním paketů hello byla úspěšná a jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="bd75c-158">This value shows that hello packet capture was successful and ran its time.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes tooCapture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture show command OK
```

## <a name="stop-a-packet-capture"></a><span data-ttu-id="bd75c-159">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="bd75c-159">Stop a packet capture</span></span>

<span data-ttu-id="bd75c-160">Spuštěním hello `network watcher packet-capture stop` rutiny, pokud zachycení relace je v průběhu je zastavena.</span><span class="sxs-lookup"><span data-stu-id="bd75c-160">By running hello `network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
azure network watcher packet-capture stop -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="bd75c-161">žádná odpověď vrátí rutina Hello při spuštěné v relaci aktuálně spuštěné zachycení nebo existující relaci, která již byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="bd75c-161">hello cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="bd75c-162">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="bd75c-162">Delete a packet capture</span></span>

```azurecli
azure network watcher packet-capture delete -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="bd75c-163">Odstraňuje se zachytáváním paketů nedojde k odstranění hello souboru v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="bd75c-163">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="bd75c-164">Stáhnout zachytáváním paketů</span><span class="sxs-lookup"><span data-stu-id="bd75c-164">Download a packet capture</span></span>

<span data-ttu-id="bd75c-165">Po dokončení relace zachytávání paketů hello zachycení soubor může být nahrané tooblob úložiště nebo tooa místního souboru na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bd75c-165">Once your packet capture session has completed, hello capture file can be uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="bd75c-166">umístění úložiště Hello zachytáváním paketů hello se definuje při vytvoření relace hello.</span><span class="sxs-lookup"><span data-stu-id="bd75c-166">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="bd75c-167">Vhodné nástroje tooaccess tyto zaznamenat soubory uložené tooa účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="bd75c-167">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="bd75c-168">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají tooa účet úložiště v hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="bd75c-168">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="bd75c-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd75c-169">Next steps</span></span>

<span data-ttu-id="bd75c-170">Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="bd75c-170">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="bd75c-171">Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="bd75c-171">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
