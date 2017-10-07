---
title: "aaaUsing požadovaného stavu konfigurace s sady škálování virtuálního počítače | Microsoft Docs"
description: "Sady škálování virtuálního počítače pomocí hello rozšíření DSC Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 04/05/2017
ms.author: zachal
ms.openlocfilehash: a35f1ca6700aa4889978032aa512882db50d6573
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-virtual-machine-scale-sets-with-hello-azure-dsc-extension"></a>Sady škálování virtuálního počítače pomocí hello rozšíření DSC Azure
[Sady škálování virtuálního počítače](virtual-machine-scale-sets-overview.md) lze použít s hello [Azure požadovaného stavu konfigurace (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) rozšíření obslužné rutiny. Sady škálování virtuálního počítače zadejte způsob toodeploy a správu velkého počtu virtuálních počítačů a můžete Elasticky škálovat a odhlašování v tooload odpovědi. DSC je použité tooconfigure hello virtuálních počítačů, protože tak používají hello produkční softwaru režimu online.

## <a name="differences-between-deploying-toovirtual-machines-and-virtual-machine-scale-sets"></a>Rozdíly mezi nasazení tooVirtual počítačů a sady škálování virtuálního počítače
Hello podkladová struktura šablony pro sadu škálování virtuálního počítače se mírně liší od jeden virtuální počítač. Konkrétně jeden virtuální počítač nasadí rozšíření pod uzlem "virtualMachines" hello. Existuje položka typu "rozšíření", kde je DSC přidané toohello šablony

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Oddíl "vlastnosti" hello "VirtualMachineProfile", "extensionProfile" atribut má uzel sady škálování virtuálního počítače. V části "rozšíření" je přidána DSC

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-a-virtual-machine-scale-set"></a>Chování pro Škálovací sadu virtuálních počítačů
Hello chování pro sadu škálování virtuálního počítače je identické toohello chování pro jeden virtuální počítač. Když je vytvořen nový virtuální počítač, je automaticky zřizovat s hello rozšíření DSC. Pokud na novější verzi WMF je požadován pro rozšíření hello hello hello virtuální počítač restartuje před uveden do režimu online. Jakmile je online, stáhne hello DSC konfigurace .zip a přidělení na hello virtuálních počítačů. Další podrobnosti naleznete v [hello přehled rozšíření DSC Azure](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Další kroky
Zkontrolujte hello [šablony Azure Resource Manageru pro rozšíření hello DSC](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zjistěte, jak hello [rozšíření DSC bezpečně zpracovává pověření](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Další informace o hello Azure DSC rozšíření obslužné rutiny, najdete v části [Úvod toohello konfigurace požadovaného stavu Azure rozšíření obslužné rutiny](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Další informace o DSC Powershellu [navštivte centru dokumentace prostředí PowerShell hello](https://msdn.microsoft.com/powershell/dsc/overview). 

