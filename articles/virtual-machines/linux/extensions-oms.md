---
title: "aaaOMS rozšíření virtuálního počítače Azure pro Linux | Microsoft Docs"
description: "Nasaďte agenta OMS hello na virtuální počítač s Linuxem pomocí rozšíření virtuálního počítače."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: 0fc8003d1fae6c043eef18ae78d12f9304b70832
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a>OMS rozšíření virtuálního počítače pro Linux

## <a name="overview"></a>Přehled

Operations Management Suite (OMS) poskytuje možnosti nápravy monitorování, výstrahy a výstrahy v cloudové a místní prostředky. Hello rozšíření virtuálního počítače OMS agenta pro Linux je publikována a společnost Microsoft podporuje. rozšíření Hello nainstaluje agenta OMS hello na virtuálních počítačích Azure a zaregistruje virtuální počítače do existující pracovní prostor OMS. Tento dokument podrobnosti hello podporované platformy, konfigurace a možnosti nasazení pro hello OMS rozšíření virtuálního počítače pro Linux.

## <a name="prerequisites"></a>Požadavky

### <a name="operating-system"></a>Operační systém

Hello rozšíření agenta OMS je možné spustit proti tyto Linuxových distribucích.

| Distribuce | Verze |
|---|---|
| CentOS Linux | 5, 6 a 7 |
| Oracle Linux | 5, 6 a 7 |
| Red Hat Enterprise Linux Server | 5, 6 a 7 |
| Debian GNU/Linux | 6, 7 a 8 |
| Ubuntu | 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS |
| SUSE Linux Enterprise Server | 11 a 12 |

### <a name="internet-connectivity"></a>Připojení k internetu

Hello rozšíření OMS agenta pro Linux vyžaduje, aby hello cílový virtuální počítač je připojený toohello Internetu. 

## <a name="extension-schema"></a>Rozšíření schématu

Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření agenta OMS. rozšíření Hello vyžaduje klíč ID a pracovního prostoru pracovní prostor hello z pracovní prostor OMS cíl hello; Tyto hodnoty můžete najít na portálu OMS hello. Protože hello klíč pracovního prostoru by měl být považován za citlivá data, by měly být uložené v chráněném nastavení konfigurace. Azure data virtuálního počítače chráněný rozšíření nastavení je zašifrovaná a dešifrovat jenom na hello cílového virtuálního počítače. Všimněte si, že **workspaceId** a **workspaceKey** malých a velkých písmen.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a>Hodnoty vlastností

| Name (Název) | Hodnota nebo příklad |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Vydavatele | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.4 |
| ID pracovního prostoru (např.) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (např.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ == |


## <a name="template-deployment"></a>Nasazení šablon

Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. Šablony jsou ideální, pokud nasazujete jednu nebo více virtuálních počítačů, které vyžadují konfiguraci nasazení post například tooOMS registrace. Ukázka šablonu Resource Manager, která obsahuje rozšíření virtuálního počítače agenta OMS hello naleznete na hello [Azure rychlý Start Galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

Konfigurace Hello JSON pro rozšíření virtuálního počítače můžete vnořit hello prostředek virtuálního počítače nebo umístěny na nejvyšší úrovni šablony Resource Manageru JSON nebo hello kořenové. umístění Hello hello JSON konfigurace ovlivní hodnotu hello hello název prostředku a typem. Další informace najdete v tématu [nastavte název a typ pro podřízené prostředky](../../azure-resource-manager/resource-manager-template-child-resource.md). 

Hello následující příklad předpokládá, že rozšíření OMS hello je vnořit hello prostředek virtuálního počítače. Při vnoření hello rozšíření prostředků, hello JSON je umístěn v hello `"resources": []` objekt hello virtuálního počítače.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

Při vkládání hello rozšíření JSON v kořenu hello hello šablony, název prostředku hello zahrnuje odkaz toohello nadřazený virtuální počítač a typ hello odráží hello vnořené konfigurace.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Nasazení Azure CLI

Hello rozhraní příkazového řádku Azure se dá použít toodeploy hello VM agenta OMS rozšíření tooan existující virtuální počítač. Nahraďte klíč OMS hello a OMS ID s těmi, která z pracovního prostoru OMS. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Řešení potíží a podpora

### <a name="troubleshoot"></a>Řešení potíží

Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí hello rozhraní příkazového řádku Azure. Stav nasazení hello toosee rozšíření pro daný virtuální počítač, spusťte následující příkaz pomocí hello hello rozhraní příkazového řádku Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Rozšíření spuštění výstup protokolu toohello následující soubor:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Kódy chyb a jejich významů

| Kód chyby | Význam | Možné akce |
| :---: | --- | --- |
| 10 | Virtuální počítač je již připojené tooan pracovním prostorem OMS | tooconnect hello virtuálních počítačů toohello prostoru zadaný v hello rozšíření schématu, nastavte stopOnMultipleConnections toofalse v nastavení veřejných nebo odeberte tuto vlastnost. Tento virtuální počítač získá účtován pro každý pracovní prostor po připojení. |
| 11 | Neplatná konfigurace poskytuje toohello rozšíření | Postupujte podle hello předcházející příklady tooset všechny hodnoty vlastností nezbytné pro nasazení. |
| 12 | Správce balíčků dpkg Hello je uzamčena. | Zajistěte, aby všechny dpkg operace aktualizace na počítači hello dokončení a akci opakujte. |
| 20 | Povolit názvem předčasně | [Aktualizace hello Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) toohello nejnovější dostupnou verzi. |
| 51 | Toto rozšíření není podporována v systému operace hello Virtuálního počítače | |
| 55 | Nelze se připojit službu Microsoft Operations Management Suite toohello | Zkontrolujte, zda text hello systém má přístup k Internetu nebo že bylo zadáno platný proxy server HTTP. Kromě toho zkontrolujte správnost hello hello ID pracovního prostoru. |

Další informace o odstraňování potíží naleznete na hello [Průvodce odstraňováním potíží OMS agenta pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
