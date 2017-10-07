---
title: "aaaAzure rozšíření virtuálního počítače sítě sledovacích procesů agenta pro Linux | Microsoft Docs"
description: "Nasaďte hello sítě sledovacích procesů agenta na virtuální počítač s Linuxem pomocí rozšíření virtuálního počítače."
services: virtual-machines-linux
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 84bed132cbda83d0917be490f9a50914578952a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Sítě rozšíření virtuálního počítače sledovacích procesů agenta pro Linux

## <a name="overview"></a>Přehled

[Azure sledovací proces sítě](https://review.docs.microsoft.com/en-us/azure/network-watcher/) je sítě výkonu monitorování, diagnostiku a analýzy služba, která umožňuje monitorování pro sítě Azure. Hello rozšíření sítě sledovacích procesů agenta virtuálního počítače není pro některé funkce hello sledovací proces sítě na virtuálních počítačích Azure. To zahrnuje Zachytávání síťových přenosů na vyžádání a další pokročilé funkce.

Tento dokument podrobnosti hello podporované platformy a možnosti nasazení pro hello rozšíření virtuálního počítače sítě sledovacích procesů agenta pro Linux.

## <a name="prerequisites"></a>Požadavky

### <a name="operating-system"></a>Operační systém

pro tyto Linuxových distribucích můžete spouštět Hello sítě sledovacích procesů agenta rozšíření:

| Distribuce | Verze |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS a 12.04 LTS |
| Debian | 7 a 8 |
| RedHat | 6.x a 7.x |
| Oracle Linux | 7 x |
| SuSE | 11 a 12 |
| OpenSuse | 7.0 |
| CentOS | 7.0 |

Všimněte si, že se v tuto chvíli nepodporuje CoreOS.

### <a name="internet-connectivity"></a>Připojení k internetu

Některé hello funkce sítě sledovacích procesů agenta vyžaduje, aby hello cílový virtuální počítač připojené toohello Internetu. Bez hello možnost tooestablish odchozí připojení některé funkce sítě sledovacích procesů agenta hello nebude fungovat správně nebo nedostupná. Další podrobnosti najdete v tématu hello [sledovací proces sítě dokumentaci](https://review.docs.microsoft.com/en-us/azure/network-watcher/).

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
        "type": "NetworkWatcherAgentLinux",
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
| type | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Nasazení šablon

Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. popsané v předchozí části hello schématu JSON Hello lze použít v toorun hello šablony Azure Resource Manager sítě sledovacích procesů agenta rozšíření během nasazení šablony Azure Resource Manager.

## <a name="azure-cli-deployment"></a>Nasazení Azure CLI

Hello rozhraní příkazového řádku Azure se dá použít toodeploy hello sítě sledovacích procesů agenta virtuálního počítače rozšíření tooan existující virtuální počítač.

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a>Řešení potíží a podpora

### <a name="troubleshooting"></a>Řešení potíží

Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí hello rozhraní příkazového řádku Azure. Stav nasazení hello toosee rozšíření pro daný virtuální počítač, spusťte následující příkaz pomocí hello hello rozhraní příkazového řádku Azure.

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

Rozšíření spuštění výstup je zaznamenané toofiles najít v hello následující adresář:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, můžete naleznete v dokumentaci toohello sledovací proces sítě nebo se obraťte hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
