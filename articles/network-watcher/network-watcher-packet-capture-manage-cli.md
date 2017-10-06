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
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a>Spravovat zachycení paketů s sledovací proces sítě Azure pomocí Azure CLI 2.0

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Rozhraní API Azure REST](network-watcher-packet-capture-manage-rest.md)

Zachytáváním paketů sledovací proces sítě vám umožní toocreate zaznamenání relace tootrack provoz tooand z virtuálního počítače. Filtry jsou podle hello zaznamenání relace tooensure že zaznamenáte jenom provoz hello, které chcete. Zachytáváním paketů pomáhá toodiagnose sítě anomálií reaktivně a proaktivně. Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího. Tím, že je schopný tooremotely aktivační událost paketu zachycení, tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně a hello požadované počítače, který úspora času.

Tento článek používá naší nové generace rozhraní příkazového řádku pro model nasazení hello prostředků management, Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.

tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

Tento článek vás provede hello úlohy různých správy, které jsou aktuálně dostupné pro zachytávání paketů.

- [**Spustit zachytávání paketů**](#start-a-packet-capture)
- [**Zastavit zachytávání paketů**](#stop-a-packet-capture)
- [**Odstranit zachytávání paketů**](#delete-a-packet-capture)
- [**Stáhnout zachytáváním paketů**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Než začnete

Tento článek předpokládá, že máte hello následující prostředky:

- Instance sledovací proces sítě v hello oblasti, kterou chcete toocreate zachytávání paketů
- Virtuální počítač s hello paketu zachytit rozšíření povolené.

> [!IMPORTANT]
> Zachytáváním paketů vyžaduje toobe agenta spuštěny na virtuálním počítači hello. Hello agenta je nainstalován jako rozšíření. Pokyny k rozšíření virtuálního počítače, navštivte [rozšíření virtuálního počítače a funkce](../virtual-machines/windows/extensions-features.md).

## <a name="install-vm-extension"></a>Instalace rozšíření virtuálního počítače

### <a name="step-1"></a>Krok 1

Spustit hello `az vm extension set` rutiny tooinstall hello paketu zachycení agenta na virtuálním počítači hosta hello.

Pro virtuální počítače s Windows:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

Pro virtuální počítače Linux:

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a>Krok 2

tooensure, který hello agenta nainstalovat, spustit hello `vm extension get` rutiny a předejte ji hello skupinu prostředků a název virtuálního počítače. Zkontrolujte, zda je nainstalován agent hello tooensure výsledný seznam hello.

```azurecli
az vm extension show -resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

Následující ukázka Hello je příkladem hello odpověď od spuštění`az vm extension show`

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

## <a name="start-a-packet-capture"></a>Spustit zachytávání paketů

Jakmile hello předchozí kroky jsou dokončeny, hello paketů zachycení agent je nainstalován na virtuálním počítači hello.

### <a name="step-1"></a>Krok 1

dalším krokem Hello je tooretrieve hello sledovací proces sítě instance. Název Tnelze hello sledovací proces sítě je předán toohello `az network watcher show` rutiny v kroku 4.

```azurecli
az network watcher show -resource-group resourceGroup -name networkWatcherName
```

### <a name="step-2"></a>Krok 2

Načtěte účet úložiště. Tento účet úložiště je použité toostore hello paketu zachycení soubor.

```azurecli
azure storage account list
```

### <a name="step-3"></a>Krok 3

Filtry lze použít toolimit hello data uložená ve zachytáváním paketů hello. Hello následující příklad nastaví zachytáváním paketů s několika filtry.  Hello první tři filtry shromažďovat odchozí přenosy TCP pouze z místní IP 10.0.0.3 toodestination porty 20, 80 a 443.  poslední filtru Hello shromažďuje jenom provoz UDP.

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

Hello následující příklad je hello očekávaný výstup z systémem hello `az network watcher packet-capture create` rutiny.

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

## <a name="get-a-packet-capture"></a>Získat zachytávání paketů

Spuštění hello `az network watcher packet-capture show` rutina, načte stav hello paketu zachycení aktuálně spuštěné, nebo pokud byla dokončena.

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

Hello následující příklad je hello výstup hello `az network watcher packet-capture show` rutiny. Hello následující příklad je po dokončení hello zachycení. StopReason TimeExceeded je zastavena Hello PacketCaptureStatus hodnotu. Tato hodnota ukazuje, že zachytáváním paketů hello byla úspěšná a jeho spuštění.

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

## <a name="stop-a-packet-capture"></a>Zastavit zachytávání paketů

Spuštěním hello `az network watcher packet-capture stop` rutiny, pokud zachycení relace je v průběhu je zastavena.

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> žádná odpověď vrátí rutina Hello při spuštěné v relaci aktuálně spuštěné zachycení nebo existující relaci, která již byla zastavena.

## <a name="delete-a-packet-capture"></a>Odstranit zachytávání paketů

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> Odstraňuje se zachytáváním paketů nedojde k odstranění hello souboru v účtu úložiště hello.

## <a name="download-a-packet-capture"></a>Stáhnout zachytáváním paketů

Po dokončení relace zachytávání paketů hello zachycení soubor může být nahrané tooblob úložiště nebo tooa místního souboru na hello virtuálních počítačů. umístění úložiště Hello zachytáváním paketů hello se definuje při vytvoření relace hello. Vhodné nástroje tooaccess tyto zaznamenat soubory uložené tooa účet úložiště je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/

Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají tooa účet úložiště v hello následující umístění:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)

Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
