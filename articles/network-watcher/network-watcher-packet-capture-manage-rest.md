---
title: "Spravovat zachycení paketů s sledovací proces sítě Azure - REST API | Microsoft Docs"
description: "Tato stránka vysvětluje, jak spravovat funkci zachycení paketu sledovací proces sítě pomocí rozhraní REST API Azure"
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
ms.openlocfilehash: 49ec20802a252258d8493eb26510270b925e851a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-rest-api"></a><span data-ttu-id="0a9a6-103">Spravovat zachycení paketů s sledovací proces sítě Azure pomocí rozhraní REST API Azure</span><span class="sxs-lookup"><span data-stu-id="0a9a6-103">Manage packet captures with Azure Network Watcher using Azure REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="0a9a6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0a9a6-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="0a9a6-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0a9a6-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="0a9a6-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="0a9a6-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="0a9a6-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0a9a6-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="0a9a6-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="0a9a6-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="0a9a6-109">Zachytáváním paketů sledovací proces sítě vám umožní vytvořit relace zachytávání sledovat provoz do a z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="0a9a6-110">Filtry jsou k dispozici pro relaci zachytávání zajistit, že zaznamenáte pouze provoz, který chcete.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="0a9a6-111">Při diagnostice sítě anomálií reaktivně a proaktivně pomáhá zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="0a9a6-112">Jiné účely zahrnují shromažďování statistiku sítě, získá informace o síti vniknutí, k ladění komunikaci klienta se serverem a mnoho dalšího.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="0a9a6-113">Díky vzdáleně aktivovat paketu zachycení, tato funkce snižuje zátěž spuštěných zachytáváním paketů ručně a na požadované počítače, který úspora času.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="0a9a6-114">Tento článek vás provede úloh jiný správy, které jsou aktuálně dostupné pro zachytávání paketů.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-114">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="0a9a6-115">**Získat zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="0a9a6-115">**Get a packet capture**</span></span>](#get-a-packet-capture)
- [<span data-ttu-id="0a9a6-116">**Zobrazí seznam všech zachycení paketu**</span><span class="sxs-lookup"><span data-stu-id="0a9a6-116">**List all packet captures**</span></span>](#list-all-packet-captures)
- [<span data-ttu-id="0a9a6-117">**Dotaz na stav zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="0a9a6-117">**Query the status of a packet capture**</span></span>](#query-packet-capture-status)
- [<span data-ttu-id="0a9a6-118">**Spustit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="0a9a6-118">**Start a packet capture**</span></span>](#start-packet-capture)
- [<span data-ttu-id="0a9a6-119">**Zastavit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="0a9a6-119">**Stop a packet capture**</span></span>](#stop-packet-capture)
- [<span data-ttu-id="0a9a6-120">**Odstranit zachytávání paketů**</span><span class="sxs-lookup"><span data-stu-id="0a9a6-120">**Delete a packet capture**</span></span>](#delete-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="0a9a6-121">Než začnete</span><span class="sxs-lookup"><span data-stu-id="0a9a6-121">Before you begin</span></span>

<span data-ttu-id="0a9a6-122">V tomto scénáři volání rozhraní Rest API sítě sledovacích procesů ke spuštění tok ověření IP.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-122">In this scenario, you call the Network Watcher Rest API to run IP Flow Verify.</span></span> <span data-ttu-id="0a9a6-123">ARMclient se používá k volání rozhraní REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-123">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="0a9a6-124">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="0a9a6-124">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="0a9a6-125">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-125">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

> <span data-ttu-id="0a9a6-126">Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-126">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="0a9a6-127">Instalaci rozšíření na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="0a9a6-127">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="0a9a6-128">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="0a9a6-128">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="0a9a6-129">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="0a9a6-129">Retrieve a virtual machine</span></span>

<span data-ttu-id="0a9a6-130">Spusťte následující skript pro vrácení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-130">Run the following script to return a virtual machine.</span></span> <span data-ttu-id="0a9a6-131">Tyto informace budete potřebovat pro spuštění zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-131">This information is needed for starting a packet capture.</span></span>

<span data-ttu-id="0a9a6-132">Následující kód potřebuje proměnné:</span><span class="sxs-lookup"><span data-stu-id="0a9a6-132">The following code needs variables:</span></span>

- <span data-ttu-id="0a9a6-133">**ID předplatného** -pomocí můžete také načíst id předplatného **Get-AzureRMSubscription** rutiny.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-133">**subscriptionId** - The subscription id can also be retrieved with the **Get-AzureRMSubscription** cmdlet.</span></span>
- <span data-ttu-id="0a9a6-134">**Název skupiny prostředků** -název skupiny prostředků, která obsahuje virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-134">**resourceGroupName** - The name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="0a9a6-135">Z výstupu v následujícím je id virtuálního počítače použít v dalším příkladu.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-135">From the following output, the id of the virtual machine is used in the next example.</span></span>

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


## <a name="get-a-packet-capture"></a><span data-ttu-id="0a9a6-136">Získat zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="0a9a6-136">Get a packet capture</span></span>

<span data-ttu-id="0a9a6-137">Následující příklad získá stavu zaznamenání jednoho paketu</span><span class="sxs-lookup"><span data-stu-id="0a9a6-137">The following example gets the status of a single packet capture</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

<span data-ttu-id="0a9a6-138">Příklady typických odpověď vrácená při dotazování na stav zachytáváním paketů, jsou tyto odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-138">The following responses are examples of a typical response returned when querying the status of a packet capture.</span></span>

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

## <a name="list-all-packet-captures"></a><span data-ttu-id="0a9a6-139">Zobrazí seznam všech zachycení paketu</span><span class="sxs-lookup"><span data-stu-id="0a9a6-139">List all packet captures</span></span>

<span data-ttu-id="0a9a6-140">Následující příklad načte všechny relace zachytávání paketů v oblasti.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-140">The following example gets all packet capture sessions in a region.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures?api-version=2016-12-01"
```

<span data-ttu-id="0a9a6-141">Je odpověď na následující příklad typické odpověď vrácená při získávání všechny paketu zaznamená</span><span class="sxs-lookup"><span data-stu-id="0a9a6-141">The following response is an example of a typical response returned when getting all packet captures</span></span>

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

## <a name="query-packet-capture-status"></a><span data-ttu-id="0a9a6-142">Dotaz na stav zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="0a9a6-142">Query packet capture status</span></span>

<span data-ttu-id="0a9a6-143">Následující příklad načte všechny relace zachytávání paketů v oblasti.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-143">The following example gets all packet capture sessions in a region.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

<span data-ttu-id="0a9a6-144">Je odpověď na následující příklad typické odpověď vrácená při dotazování na stav zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-144">The following response is an example of a typical response returned when querying the status of a packet capture.</span></span>

```json
{
    "name": "vm1PacketCapture",     "id": "/subscriptions/{guid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkWatchers/{networkWatche rName}/packetCaptures/{packetCaptureName}",
   "captureStartTime" : "9/7/2016 12:35:24PM",
   "packetCaptureStatus" : "Stopped",
   "stopReason" : "TimeExceeded"
   "packetCaptureError" : [ ]
}
```

## <a name="start-packet-capture"></a><span data-ttu-id="0a9a6-145">Spustit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="0a9a6-145">Start packet capture</span></span>

<span data-ttu-id="0a9a6-146">Následující příklad vytvoří zachytáváním paketů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-146">The following example creates a packet capture on a virtual machine.</span></span>  <span data-ttu-id="0a9a6-147">V příkladu je parametry umožňující flexibilitu při vytváření příklad.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-147">The example is parameterized to allow for flexibility in creating an example.</span></span>

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

## <a name="stop-packet-capture"></a><span data-ttu-id="0a9a6-148">Zastavit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="0a9a6-148">Stop packet capture</span></span>

<span data-ttu-id="0a9a6-149">Následující příklad zastaví zachytáváním paketů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-149">The following example stops a packet capture on a virtual machine.</span></span>  <span data-ttu-id="0a9a6-150">V příkladu je parametry umožňující flexibilitu při vytváření příklad.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-150">The example is parameterized to allow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/stop?api-version=2016-12-01"
```

## <a name="delete-packet-capture"></a><span data-ttu-id="0a9a6-151">Odstranit zachytávání paketů</span><span class="sxs-lookup"><span data-stu-id="0a9a6-151">Delete packet capture</span></span>

<span data-ttu-id="0a9a6-152">Následující příklad odstraní zachytáváním paketů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-152">The following example deletes a packet capture on a virtual machine.</span></span>  <span data-ttu-id="0a9a6-153">V příkladu je parametry umožňující flexibilitu při vytváření příklad.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-153">The example is parameterized to allow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"

armclient delete "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-12-01"
```

> [!NOTE]
> <span data-ttu-id="0a9a6-154">Odstraňuje se zachytáváním paketů nedojde k odstranění souboru v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="0a9a6-154">Deleting a packet capture does not delete the file in the storage account</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a9a6-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0a9a6-155">Next steps</span></span>

<span data-ttu-id="0a9a6-156">Pokyny ke stahování souborů z účty azure storage, najdete v části [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="0a9a6-156">For instructions on downloading files from azure storage accounts, refer to [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="0a9a6-157">Jiný nástroj, který je možné je Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="0a9a6-157">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="0a9a6-158">Další informace o Storage Explorer naleznete zde na následující odkaz: [Storage Explorer](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="0a9a6-158">More information about Storage Explorer can be found here at the following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

<span data-ttu-id="0a9a6-159">Informace o automatizaci paketu zachytává se virtuální počítač výstrahy zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="0a9a6-159">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>













