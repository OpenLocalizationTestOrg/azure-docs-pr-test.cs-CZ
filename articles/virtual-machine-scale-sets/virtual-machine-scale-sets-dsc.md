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
# <a name="using-virtual-machine-scale-sets-with-hello-azure-dsc-extension"></a><span data-ttu-id="6a968-103">Sady škálování virtuálního počítače pomocí hello rozšíření DSC Azure</span><span class="sxs-lookup"><span data-stu-id="6a968-103">Using Virtual Machine Scale Sets with hello Azure DSC Extension</span></span>
<span data-ttu-id="6a968-104">[Sady škálování virtuálního počítače](virtual-machine-scale-sets-overview.md) lze použít s hello [Azure požadovaného stavu konfigurace (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) rozšíření obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6a968-104">[Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md) can be used with hello [Azure Desired State Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) extension handler.</span></span> <span data-ttu-id="6a968-105">Sady škálování virtuálního počítače zadejte způsob toodeploy a správu velkého počtu virtuálních počítačů a můžete Elasticky škálovat a odhlašování v tooload odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6a968-105">Virtual machine scale sets provide a way toodeploy and manage large numbers of virtual machines, and can elastically scale in and out in response tooload.</span></span> <span data-ttu-id="6a968-106">DSC je použité tooconfigure hello virtuálních počítačů, protože tak používají hello produkční softwaru režimu online.</span><span class="sxs-lookup"><span data-stu-id="6a968-106">DSC is used tooconfigure hello VMs as they come online so they are running hello production software.</span></span>

## <a name="differences-between-deploying-toovirtual-machines-and-virtual-machine-scale-sets"></a><span data-ttu-id="6a968-107">Rozdíly mezi nasazení tooVirtual počítačů a sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6a968-107">Differences between deploying tooVirtual Machines and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="6a968-108">Hello podkladová struktura šablony pro sadu škálování virtuálního počítače se mírně liší od jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6a968-108">hello underlying template structure for a virtual machine scale set is slightly different from a single VM.</span></span> <span data-ttu-id="6a968-109">Konkrétně jeden virtuální počítač nasadí rozšíření pod uzlem "virtualMachines" hello.</span><span class="sxs-lookup"><span data-stu-id="6a968-109">Specifically, a single VM deploys extensions under hello "virtualMachines" node.</span></span> <span data-ttu-id="6a968-110">Existuje položka typu "rozšíření", kde je DSC přidané toohello šablony</span><span class="sxs-lookup"><span data-stu-id="6a968-110">There is an entry of type "extensions" where DSC is added toohello template</span></span>

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

<span data-ttu-id="6a968-111">Oddíl "vlastnosti" hello "VirtualMachineProfile", "extensionProfile" atribut má uzel sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6a968-111">A virtual machine scale set node has a "properties" section with hello "VirtualMachineProfile", "extensionProfile" attribute.</span></span> <span data-ttu-id="6a968-112">V části "rozšíření" je přidána DSC</span><span class="sxs-lookup"><span data-stu-id="6a968-112">DSC is added under "extensions"</span></span>

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

## <a name="behavior-for-a-virtual-machine-scale-set"></a><span data-ttu-id="6a968-113">Chování pro Škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6a968-113">Behavior for a Virtual Machine Scale Set</span></span>
<span data-ttu-id="6a968-114">Hello chování pro sadu škálování virtuálního počítače je identické toohello chování pro jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6a968-114">hello behavior for a virtual machine scale set is identical toohello behavior for a single VM.</span></span> <span data-ttu-id="6a968-115">Když je vytvořen nový virtuální počítač, je automaticky zřizovat s hello rozšíření DSC.</span><span class="sxs-lookup"><span data-stu-id="6a968-115">When a new VM is created, it is automatically provisioned with hello DSC extension.</span></span> <span data-ttu-id="6a968-116">Pokud na novější verzi WMF je požadován pro rozšíření hello hello hello virtuální počítač restartuje před uveden do režimu online.</span><span class="sxs-lookup"><span data-stu-id="6a968-116">If a newer version of hello WMF is required by hello extension, hello VM reboots before coming online.</span></span> <span data-ttu-id="6a968-117">Jakmile je online, stáhne hello DSC konfigurace .zip a přidělení na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6a968-117">Once it is online, it downloads hello DSC configuration .zip and provision it on hello VM.</span></span> <span data-ttu-id="6a968-118">Další podrobnosti naleznete v [hello přehled rozšíření DSC Azure](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a968-118">More details can be found in [hello Azure DSC Extension Overview](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a968-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a968-119">Next steps</span></span>
<span data-ttu-id="6a968-120">Zkontrolujte hello [šablony Azure Resource Manageru pro rozšíření hello DSC](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a968-120">Examine hello [Azure Resource Manager template for hello DSC extension](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="6a968-121">Zjistěte, jak hello [rozšíření DSC bezpečně zpracovává pověření](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a968-121">Learn how hello [DSC extension securely handles credentials](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="6a968-122">Další informace o hello Azure DSC rozšíření obslužné rutiny, najdete v části [Úvod toohello konfigurace požadovaného stavu Azure rozšíření obslužné rutiny](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a968-122">For more information on hello Azure DSC extension handler, see [Introduction toohello Azure Desired State Configuration extension handler](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="6a968-123">Další informace o DSC Powershellu [navštivte centru dokumentace prostředí PowerShell hello](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="6a968-123">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

