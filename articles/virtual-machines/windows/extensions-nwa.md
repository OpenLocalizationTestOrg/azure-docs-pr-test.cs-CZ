---
title: "aaaAzure rozšíření virtuálního počítače sítě sledovacích procesů agenta pro Windows | Microsoft Docs"
description: "Nasaďte hello sítě sledovacích procesů agenta v systému Windows virtuálního počítače pomocí rozšíření virtuálního počítače."
services: virtual-machines-windows
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 21298706e462ff32c4d314f9a1ad127074ddf481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Sítě rozšíření virtuálního počítače sledovacích procesů agenta pro Windows

## <a name="overview"></a>Přehled

[Azure sledovací proces sítě](https://review.docs.microsoft.com/en-us/azure/network-watcher/) je sítě výkonu monitorování, diagnostiku a analýzy služba, která umožňuje monitorování pro sítě Azure. Hello rozšíření sítě sledovacích procesů agenta virtuálního počítače není pro některé funkce hello sledovací proces sítě na virtuálních počítačích Azure. To zahrnuje Zachytávání síťových přenosů na vyžádání a další pokročilé funkce.

Tento dokument podrobnosti hello podporované platformy a možnosti nasazení pro hello rozšíření virtuálního počítače sítě sledovacích procesů agenta pro Windows.

## <a name="prerequisites"></a>Požadavky

### <a name="operating-system"></a>Operační systém

Hello rozšíření sítě sledovacích procesů agenta pro Windows můžete spustit na Windows Server 2008 R2, 2012, 2012 R2 a 2016 uvolní. Všimněte si, že hello Nano Server není v současné době podporovaný.

### <a name="internet-connectivity"></a>Připojení k internetu

Některé hello funkce sítě sledovacích procesů agenta vyžaduje, aby hello cílový virtuální počítač připojené toohello Internetu. Bez hello možnost tooestablish odchozí připojení některé funkce sítě sledovacích procesů agenta hello nebude fungovat správně nebo nedostupná. Další podrobnosti najdete v tématu hello [sledovací proces sítě dokumentaci](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Rozšíření schématu

Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření sítě sledovacích procesů agenta. rozšíření Hello ani jeden z nich vyžaduje ani podporuje nastavení uživatelem zadané v tuto chvíli a spoléhá na jeho výchozí konfigurace.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Hodnoty vlastností

| Name (Název) | Hodnota nebo příklad |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Vydavatele | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Nasazení šablon

Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. popsané v předchozí části hello schématu JSON Hello lze použít v toorun hello šablony Azure Resource Manager sítě sledovacích procesů agenta rozšíření během nasazení šablony Azure Resource Manager.

## <a name="powershell-deployment"></a>Nasazení prostředí PowerShell

Hello `Set-AzureRmVMExtension` příkaz lze použít toodeploy hello sítě sledovacích procesů agenta virtuálního počítače rozšíření tooan existující virtuální počítač.

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Řešení potíží a podpora

### <a name="troubleshooting"></a>Řešení potíží

Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí modulu Azure PowerShell hello. Stav nasazení hello toosee rozšíření pro daný virtuální počítač, spusťte hello následující pomocí příkazu hello modul Azure PowerShell.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Rozšíření spuštění výstup je zaznamenané toofiles najít v hello následující adresář:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, můžete naleznete v dokumentaci toohello Příručka uživatele sledovací proces sítě nebo se obraťte hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
