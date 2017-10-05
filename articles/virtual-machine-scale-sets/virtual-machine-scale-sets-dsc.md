---
title: "Pomocí konfigurace požadovaného stavu s sady škálování virtuálního počítače | Microsoft Docs"
description: "Použití škálování virtuálních počítačů sad s Azure rozšíření DSC"
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
ms.openlocfilehash: b61b0acf3072569ab733a13defb465c921d26187
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a><span data-ttu-id="086ad-103">Použití škálování virtuálních počítačů sad s Azure rozšíření DSC</span><span class="sxs-lookup"><span data-stu-id="086ad-103">Using Virtual Machine Scale Sets with the Azure DSC Extension</span></span>
<span data-ttu-id="086ad-104">[Sady škálování virtuálního počítače](virtual-machine-scale-sets-overview.md) lze použít s [Azure požadovaného stavu konfigurace (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) rozšíření obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="086ad-104">[Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md) can be used with the [Azure Desired State Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) extension handler.</span></span> <span data-ttu-id="086ad-105">Sady škálování virtuálního počítače zadejte způsob, jak nasadit a spravovat velké počty virtuálních počítačů a můžete Elasticky škálovat a odhlašování v reakci na zatížení.</span><span class="sxs-lookup"><span data-stu-id="086ad-105">Virtual machine scale sets provide a way to deploy and manage large numbers of virtual machines, and can elastically scale in and out in response to load.</span></span> <span data-ttu-id="086ad-106">DSC slouží ke konfiguraci virtuálních počítačů jako jejich uvést do režimu online, používají produkční softwaru.</span><span class="sxs-lookup"><span data-stu-id="086ad-106">DSC is used to configure the VMs as they come online so they are running the production software.</span></span>

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a><span data-ttu-id="086ad-107">Rozdíly mezi nasazením do virtuálních počítačů a sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="086ad-107">Differences between deploying to Virtual Machines and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="086ad-108">Podkladová struktura šablony pro sadu škálování virtuálního počítače se mírně liší od jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="086ad-108">The underlying template structure for a virtual machine scale set is slightly different from a single VM.</span></span> <span data-ttu-id="086ad-109">Konkrétně jeden virtuální počítač nasadí rozšíření pod uzlem "virtualMachines".</span><span class="sxs-lookup"><span data-stu-id="086ad-109">Specifically, a single VM deploys extensions under the "virtualMachines" node.</span></span> <span data-ttu-id="086ad-110">Existuje položka typu "rozšíření", kde je DSC přidat do šablony</span><span class="sxs-lookup"><span data-stu-id="086ad-110">There is an entry of type "extensions" where DSC is added to the template</span></span>

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

<span data-ttu-id="086ad-111">Oddíl "vlastnosti" "VirtualMachineProfile", "extensionProfile" atribut má uzel sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="086ad-111">A virtual machine scale set node has a "properties" section with the "VirtualMachineProfile", "extensionProfile" attribute.</span></span> <span data-ttu-id="086ad-112">V části "rozšíření" je přidána DSC</span><span class="sxs-lookup"><span data-stu-id="086ad-112">DSC is added under "extensions"</span></span>

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

## <a name="behavior-for-a-virtual-machine-scale-set"></a><span data-ttu-id="086ad-113">Chování pro Škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="086ad-113">Behavior for a Virtual Machine Scale Set</span></span>
<span data-ttu-id="086ad-114">Chování pro sadu škálování virtuálního počítače je stejný jako u chování pro jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="086ad-114">The behavior for a virtual machine scale set is identical to the behavior for a single VM.</span></span> <span data-ttu-id="086ad-115">Když je vytvořen nový virtuální počítač, je automaticky zajištěna s příponou DSC.</span><span class="sxs-lookup"><span data-stu-id="086ad-115">When a new VM is created, it is automatically provisioned with the DSC extension.</span></span> <span data-ttu-id="086ad-116">Pokud na novější verzi WMF je požadován pro rozšíření, virtuální počítač se restartuje před uveden do režimu online.</span><span class="sxs-lookup"><span data-stu-id="086ad-116">If a newer version of the WMF is required by the extension, the VM reboots before coming online.</span></span> <span data-ttu-id="086ad-117">Jakmile je online, stáhne .zip konfigurace DSC a zřídit ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="086ad-117">Once it is online, it downloads the DSC configuration .zip and provision it on the VM.</span></span> <span data-ttu-id="086ad-118">Další podrobnosti naleznete v [přehled rozšíření DSC Azure](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="086ad-118">More details can be found in [the Azure DSC Extension Overview](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="086ad-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="086ad-119">Next steps</span></span>
<span data-ttu-id="086ad-120">Zkontrolujte [šablony Azure Resource Manageru pro rozšíření DSC](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="086ad-120">Examine the [Azure Resource Manager template for the DSC extension](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="086ad-121">Zjistěte, jak [rozšíření DSC bezpečně zpracovává pověření](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="086ad-121">Learn how the [DSC extension securely handles credentials](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="086ad-122">Další informace o obslužná rutina rozšíření Azure DSC najdete v tématu [Úvod do rozšíření obslužné rutiny konfigurace požadovaného stavu Azure](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="086ad-122">For more information on the Azure DSC extension handler, see [Introduction to the Azure Desired State Configuration extension handler](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="086ad-123">Další informace o DSC Powershellu [přejděte do centra dokumentace k prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="086ad-123">For more information about PowerShell DSC, [visit the PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

