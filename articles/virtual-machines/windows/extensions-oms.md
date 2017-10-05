---
title: "Rozšíření virtuálního počítače OMS Azure pro Windows | Microsoft Docs"
description: "Nasaďte agenta OMS na systému Windows virtuálního počítače pomocí rozšíření virtuálního počítače."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: nepeters
ms.openlocfilehash: d933f488fdda0c1d37892be65f2712cf0eb5694e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="oms-virtual-machine-extension-for-windows"></a>Rozšíření virtuálního počítače OMS pro Windows

Operations Management Suite (OMS) poskytuje možnosti nápravy monitorování, výstrahy a výstrahy v cloudové a místní prostředky. Rozšíření virtuálního počítače OMS Agent pro systém Windows je publikována a společnost Microsoft podporuje. Rozšíření nainstaluje agenta OMS na virtuálních počítačích Azure a zaregistruje virtuální počítače do existující pracovní prostor OMS. Tento dokument podrobně popisuje podporované platformy, konfigurace a možnosti nasazení pro rozšíření virtuálního počítače OMS pro systém Windows.

## <a name="prerequisites"></a>Požadavky

### <a name="operating-system"></a>Operační systém
Rozšíření agenta OMS pro pro Windows Server 2008 R2, můžete spouštět Windows 2012, 2012 R2 a 2016 uvolní.

### <a name="internet-connectivity"></a>Připojení k internetu
OMS Agent rozšíření pro Windows vyžaduje, aby cílový virtuální počítač je připojený k Internetu. 

## <a name="extension-schema"></a>Rozšíření schématu

Následujícím kódu JSON znázorňuje schéma pro rozšíření agenta OMS. Rozšíření vyžaduje pracovní prostor Id a klíč pracovního prostoru z je cílový pracovní prostor OMS, ty lze najít na portálu OMS. Vzhledem k tomu, že klíč pracovního prostoru by měl být považován za citlivá data, by měly být uložené v chráněném nastavení konfigurace. Data Azure nastavení rozšíření chráněný virtuální počítač je zašifrovaná a dešifrovat jenom na cílový virtuální počítač. Všimněte si, že **workspaceId** a **workspaceKey** malých a velkých písmen.

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a>Hodnoty vlastností

| Name (Název) | Hodnota nebo příklad |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Vydavatele | Microsoft.EnterpriseCloud.Monitoring |
| type | MicrosoftMonitoringAgent |
| typeHandlerVersion | 1.0 |
| ID pracovního prostoru (např.) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (např.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ == |

## <a name="template-deployment"></a>Nasazení šablon

Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager ke spuštění rozšíření agenta OMS při nasazení šablony Azure Resource Manager. Ukázka šablony, která obsahuje rozšíření virtuálního počítače agenta OMS naleznete na [Azure rychlý Start Galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm). 

JSON pro rozšíření virtuálního počítače můžete vnořit prostředek virtuálního počítače nebo umístěn na kořenový server WSUS nebo nejvyšší úrovně šablony JSON Resource Manager. Umístění formátu JSON, ovlivňuje hodnota název prostředku a typem. Další informace najdete v tématu [nastavte název a typ pro podřízené prostředky](../../azure-resource-manager/resource-manager-template-child-resource.md). 

V následujícím příkladu se předpokládá, že je rozšíření OMS vnořit prostředek virtuálního počítače. Při vnoření rozšíření prostředků, JSON je umístěn v `"resources": []` objektu virtuálního počítače.


```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

Při vkládání rozšíření JSON v kořenovém adresáři šablony, názvu prostředku obsahuje odkaz na nadřazený virtuální počítač a typ odráží vnořené konfigurace. 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a>Nasazení prostředí PowerShell

`Set-AzureRmVMExtension` Příkaz lze použít k nasazení rozšíření virtuálního počítače agenta OMS na existující virtuální počítač. Před spuštěním příkazu, veřejné a privátní konfigurace muset být uložena v tabulce hash prostředí PowerShell. 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzureRmVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS ` 
```

## <a name="troubleshoot-and-support"></a>Řešení potíží a podpora

### <a name="troubleshoot"></a>Řešení potíží

Data o stavu nasazení rozšíření mohou být načteny z portálu Azure a pomocí modulu Azure PowerShell. Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz modulu Azure PowerShell.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Výstupu spuštění rozšíření se zaznamenává soubory, které jsou v následujícím adresáři:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
