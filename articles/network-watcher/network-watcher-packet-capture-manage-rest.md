---
title: "paket aaaManage zachytávali sledovací proces sítě Azure - REST API | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toomanage hello funkce zachytávání paketů sledovací proces sítě pomocí rozhraní REST API Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 53fe0324-835f-4005-afc8-145eeb314aeb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 7a531fbe796e85e94961bd192d171defb299be05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-rest-api"></a><span data-ttu-id="a8f36-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí rozhraní REST API Azure</span><span class="sxs-lookup"><span data-stu-id="a8f36-103">Manage packet captures with Azure Network Watcher using Azure REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="a8f36-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a8f36-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="a8f36-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8f36-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="a8f36-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a8f36-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="a8f36-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a8f36-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="a8f36-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="a8f36-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="a8f36-109">Zachytáváním paketů sledovací proces sítě vám umožní toocreate zaznamenání relace tootrack provoz tooand z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a8f36-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="a8f36-110">Filtry jsou podle hello zaznamenání relace tooensure že zaznamenáte jenom provoz hello, které chcete.</span><span class="sxs-lookup"><span data-stu-id="a8f36-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="a8f36-111">Zachytáváním paketů pomáhá toodiagnose sítě anomálií reaktivně a proaktivně.</span><span class="sxs-lookup"><span data-stu-id="a8f36-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="a8f36-112">Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="a8f36-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="a8f36-113">Tím, že je schopný tooremotely aktivační událost paketu zachycení, tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně a hello požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="a8f36-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="a8f36-114">Tento článek vás provede hello úlohy různých správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="a8f36-114">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="a8f36-115">**Získat zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="a8f36-115">**Get a packet capture**</span></span>](#get-a-packet-capture)
- [<span data-ttu-id="a8f36-116">**Zobrazí seznam všech zachycení paketu**</span><span class="sxs-lookup"><span data-stu-id="a8f36-116">**List all packet captures**</span></span>](#list-all-packet-captures)
- [<span data-ttu-id="a8f36-117">**Dotaz na stav hello zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="a8f36-117">**Query hello status of a packet capture**</span></span>](#query-packet-capture-status)
- [<span data-ttu-id="a8f36-118">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="a8f36-118">**Start a packet capture**</span></span>](#start-packet-capture)
- [<span data-ttu-id="a8f36-119">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="a8f36-119">**Stop a packet capture**</span></span>](#stop-packet-capture)
- [<span data-ttu-id="a8f36-120">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="a8f36-120">**Delete a packet capture**</span></span>](#delete-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="a8f36-121">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a8f36-121">Before you begin</span></span>

<span data-ttu-id="a8f36-122">V tomto scénáři volání hello toorun rozhraní API Rest sledovací proces sítě IP tok ověření.</span><span class="sxs-lookup"><span data-stu-id="a8f36-122">In this scenario, you call hello Network Watcher Rest API toorun IP Flow Verify.</span></span> <span data-ttu-id="a8f36-123">ARMclient je použité toocall hello REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a8f36-123">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="a8f36-124">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="a8f36-124">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="a8f36-125">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="a8f36-125">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

> <span data-ttu-id="a8f36-126">Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="a8f36-126">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="a8f36-127">Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="a8f36-127">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="a8f36-128">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="a8f36-128">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="a8f36-129">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="a8f36-129">Retrieve a virtual machine</span></span>

<span data-ttu-id="a8f36-130">Spusťte následující skript tooreturn hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a8f36-130">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="a8f36-131">Tyto informace budete potřebovat pro spuštění zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="a8f36-131">This information is needed for starting a packet capture.</span></span>

<span data-ttu-id="a8f36-132">Hello následující kód potřebuje proměnné:</span><span class="sxs-lookup"><span data-stu-id="a8f36-132">hello following code needs variables:</span></span>

- <span data-ttu-id="a8f36-133">**ID předplatného** -pomocí hello můžete také načíst id předplatného hello **Get-AzureRMSubscription** rutiny.</span><span class="sxs-lookup"><span data-stu-id="a8f36-133">**subscriptionId** - hello subscription id can also be retrieved with hello **Get-AzureRMSubscription** cmdlet.</span></span>
- <span data-ttu-id="a8f36-134">**Název skupiny prostředků** – hello název skupiny prostředků, která obsahuje virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="a8f36-134">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="a8f36-135">Z hello následující výstup, se v dalším příkladu hello používá id hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a8f36-135">From hello following output, hello id of hello virtual machine is used in hello next example.</span></span>

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


## <a name="get-a-packet-capture"></a><span data-ttu-id="a8f36-136">Získat zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="a8f36-136">Get a packet capture</span></span>

<span data-ttu-id="a8f36-137">Hello následující příklad načte hello stavu zaznamenání jednoho paketu</span><span class="sxs-lookup"><span data-stu-id="a8f36-137">hello following example gets hello status of a single packet capture</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

<span data-ttu-id="a8f36-138">Hello následující odpovědi jsou příklady typických odpověď vrácená při dotazování na stav hello zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="a8f36-138">hello following responses are examples of a typical response returned when querying hello status of a packet capture.</span></span>

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Running",
  "packetCaptureError": []
}
```

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Stopped",
  "stopReason": "TimeExceeded",
  "packetCaptureError": []
}
```

## <a name="list-all-packet-captures"></a><span data-ttu-id="a8f36-139">Zobrazí seznam všech zachycení paketu</span><span class="sxs-lookup"><span data-stu-id="a8f36-139">List all packet captures</span></span>

<span data-ttu-id="a8f36-140">Následující ukázka Hello získá všechny relace zachytávání paketů v oblasti.</span><span class="sxs-lookup"><span data-stu-id="a8f36-140">hello following example gets all packet capture sessions in a region.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures?api-version=2016-12-01"
```

<span data-ttu-id="a8f36-141">Hello následující odpověď je zaznamená příklad typické odpověď vrácená při získávání všechny paketu</span><span class="sxs-lookup"><span data-stu-id="a8f36-141">hello following response is an example of a typical response returned when getting all packet captures</span></span>

```json
{
  "value": [
    {
      "name": "TestPacketCapture6",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Succeeded",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_19_53_056.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    },
    {
      "name": "TestPacketCapture7",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture7",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Failed",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_23_15_364.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    }
  ]
}
```

## <a name="query-packet-capture-status"></a><span data-ttu-id="a8f36-142">Dotaz na stav zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="a8f36-142">Query packet capture status</span></span>

<span data-ttu-id="a8f36-143">Následující ukázka Hello získá všechny relace zachytávání paketů v oblasti.</span><span class="sxs-lookup"><span data-stu-id="a8f36-143">hello following example gets all packet capture sessions in a region.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

<span data-ttu-id="a8f36-144">Hello následující odpověď je příklad typické odpověď vrácená při dotazování na stav hello zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="a8f36-144">hello following response is an example of a typical response returned when querying hello status of a packet capture.</span></span>

```json
{
    "name": "vm1PacketCapture",     "id": "/subscriptions/{guid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkWatchers/{networkWatche rName}/packetCaptures/{packetCaptureName}",
   "captureStartTime" : "9/7/2016 12:35:24PM",
   "packetCaptureStatus" : "Stopped",
   "stopReason" : "TimeExceeded"
   "packetCaptureError" : [ ]
}
```

## <a name="start-packet-capture"></a><span data-ttu-id="a8f36-145">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="a8f36-145">Start packet capture</span></span>

<span data-ttu-id="a8f36-146">Hello následující ukázka vytvoří zachytáváním paketů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a8f36-146">hello following example creates a packet capture on a virtual machine.</span></span>  <span data-ttu-id="a8f36-147">je třeba Hello parametrizované tooallow flexibilitu při vytváření příklad.</span><span class="sxs-lookup"><span data-stu-id="a8f36-147">hello example is parameterized tooallow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
$storageaccountname = "contosoexamplergdiag374"
$vmName = "ContosoVM"
$bytestoCaptureperPacket = "0"
$bytesPerSession = "1073741824"
$captureTimeinSeconds = "60"
$localIP = ""
$localPort = "" # Examples are: 80, or 80-120
$remoteIP = ""
$remotePort = "" # Examples are: 80, or 80-120
$protocol = "" # Valid values are TCP, UDP and Any.
$targetUri = "" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName
$storageId = "" # Example: "https://mytestaccountname.blob.core.windows.net/capture/vm1Capture.cap"
$storagePath = ""
$localFilePath = "c:\\temp\\packetcapture.cap" # Example: "d:\capture\vm1Capture.cap"

$requestBody = @"
{
    'properties':  {
                       'target':  '/${targetUri}',
                       'bytesToCapturePerPacket':  '${bytestoCaptureperPacket}',
                       'totalBytesPerSession':  '${bytesPerSession}',
                       'timeLimitinSeconds':  '${captureTimeinSeconds}',
                       'storageLocation':  {
                                               'storageId':  '${storageId}',
                                               'storagePath':  '${storagePath}',
                                               'filePath':  '${localFilePath}'
                                           },
                       'filters':  [
                                       {
                                           'protocol':  '${protocol}',
                                           'localIPAddress':  '${localIP}',
                                           'localPort':  '${localPort}',
                                           'remoteIPAddress':  '${remoteIP}',
                                           'remotePort':  '${remotePort}'
                                       }
                                   ]
                   }
}
"@

armclient PUT "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-07-01" $requestbody
```

## <a name="stop-packet-capture"></a><span data-ttu-id="a8f36-148">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="a8f36-148">Stop packet capture</span></span>

<span data-ttu-id="a8f36-149">Následující ukázka Hello zastaví zachytáváním paketů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a8f36-149">hello following example stops a packet capture on a virtual machine.</span></span>  <span data-ttu-id="a8f36-150">je třeba Hello parametrizované tooallow flexibilitu při vytváření příklad.</span><span class="sxs-lookup"><span data-stu-id="a8f36-150">hello example is parameterized tooallow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/stop?api-version=2016-12-01"
```

## <a name="delete-packet-capture"></a><span data-ttu-id="a8f36-151">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="a8f36-151">Delete packet capture</span></span>

<span data-ttu-id="a8f36-152">Následující ukázka Hello odstraní zachytáváním paketů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a8f36-152">hello following example deletes a packet capture on a virtual machine.</span></span>  <span data-ttu-id="a8f36-153">je třeba Hello parametrizované tooallow flexibilitu při vytváření příklad.</span><span class="sxs-lookup"><span data-stu-id="a8f36-153">hello example is parameterized tooallow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"

armclient delete "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-12-01"
```

> [!NOTE]
> <span data-ttu-id="a8f36-154">Odstraňuje se zachytáváním paketů neodstraní hello souboru v účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="a8f36-154">Deleting a packet capture does not delete hello file in hello storage account</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8f36-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a8f36-155">Next steps</span></span>

<span data-ttu-id="a8f36-156">Pokyny ke stahování souborů z účty azure storage, najdete v části příliš[Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="a8f36-156">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="a8f36-157">Jiný nástroj, který je možné je Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="a8f36-157">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="a8f36-158">Další informace o Storage Explorer naleznete zde na hello následující odkaz: [Storage Explorer](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="a8f36-158">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

<span data-ttu-id="a8f36-159">Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="a8f36-159">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>













