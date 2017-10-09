---
title: "příkazy pro virtuální počítače Azure aaaCommon prostředí PowerShell | Microsoft Docs"
description: "Běžné tooget příkazy prostředí PowerShell jste spustili vytváření a správu virtuálních počítačů Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: 3de9bd4d20259ced2c0aa8ef7a3f7d9520a071d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Běžné příkazy prostředí PowerShell pro vytváření a správu virtuálních počítačů Azure

Tento článek se zabývá některé příkazy Azure PowerShell, můžete použít toocreate a spravovat virtuální počítače ve vašem předplatném Azure hello.  Další pomoc s konkrétní přepínače příkazového řádku a možnosti, můžete použít **Get-Help** *příkaz*.

V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooyour účtu.

Tyto proměnné můžou být užitečné pro vás, pokud používá více než jeden z hello příkazy v tomto článku:

- $location - hello umístění hello virtuálního počítače. Můžete použít [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) toofind [geografické oblasti](https://azure.microsoft.com/regions/) který vám vyhovuje.
- $myResourceGroup - hello název skupiny prostředků hello, který obsahuje hello virtuálního počítače.
- $myVM - hello název hello virtuálního počítače.

## <a name="create-a-vm"></a>Vytvoření virtuálního počítače

| Úkol | Příkaz |
| ---- | ------- |
| Vytvoření konfigurace virtuálního počítače |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) - VMName $myVM - VMSize "Standard_D1_v1"<BR></BR><BR></BR>Konfigurace virtuálního počítače Hello je používané toodefine nebo aktualizace nastavení hello virtuálních počítačů. Konfigurace Hello je inicializován s názvem hello hello virtuálního počítače a jeho [velikost](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Přidat nastavení konfigurace |$vm = [set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-pověření $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Nastavení operačního systému, včetně [pověření](https://technet.microsoft.com/library/hh849815.aspx) přidají toohello objekt konfigurace, který jste dříve vytvořili pomocí New-AzureRmVMConfig. |
| Přidání rozhraní sítě |$vm = [přidat AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) - VM $vm-Id $ síťový adaptér. ID<BR></BR><BR></BR>Virtuální počítač musí mít [síťové rozhraní](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toocommunicate ve virtuální síti. Můžete také použít [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) tooretrieve existujícího objektu rozhraní sítě. |
| Zadat image platformy |$vm = [set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName "publisher_name"-nabízejí "publisher_offer" - SKU "product_sku"-"nejnovější" verze<BR></BR><BR></BR>[Informace o obrazu](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) se přidá toohello objekt konfigurace, který jste dříve vytvořili pomocí New-AzureRmVMConfig. Hello objekt vrácený tento příkaz slouží pouze při nastavení hello operačního systému disku toouse image platformy. |
| Nastavit image platformy toouse disku operačního systému |$vm = [set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk) - VM $vm-http://mystore1.blob.core.windows.net/vhds/myOSDisk.vhd"název"myOSDisk"- VhdUri" - CreateOption FromImage<BR></BR><BR></BR>Název Hello hello disk operačního systému a jeho umístění v [úložiště](../../storage/common/storage-powershell-guide-full.md) je přidána toohello objekt konfigurace, kterou jste vytvořili. |
| Zobecněný obrázek sady toouse disku operačního systému |$vm = set AzureRmVMOSDisk - VM $vm-https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk"název"myOSDisk"- SourceImageUri. {guid} VHD"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>Název Hello hello disk operačního systému, umístění hello hello zdroj bitové kopie a disku hello umístění v [úložiště](../../storage/common/storage-powershell-guide-full.md) je přidána toohello objekt konfigurace. |
| Specializované obrázek sady toouse disku operačního systému |$vm = set AzureRmVMOSDisk - VM $vm-http://mystore1.blob.core.windows.net/vhds/"název"myOSDisk"- VhdUri" Attach - CreateOption - Windows |
| Vytvoření virtuálního počítače |[Nový-AzureRmVM]() - ResourceGroupName $myResourceGroup-umístění $location - VM $vm<BR></BR><BR></BR>Všechny prostředky, které jsou vytvořené v [skupiny prostředků](../../azure-resource-manager/powershell-azure-resource-manager.md). Před spuštěním tohoto příkazu, spusťte nový AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, AzureRmVMNetworkInterface přidat a Set-AzureRmVMOSDisk. |

## <a name="get-information-about-vms"></a>Získat informace o virtuální počítače

| Úkol | Příkaz |
| ---- | ------- |
| Seznam virtuálních počítačů v předplatném. |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
| Seznam virtuálních počítačů ve skupině prostředků |Get-AzureRmVM - ResourceGroupName $myResourceGroup<BR></BR><BR></BR>tooget seznam prostředků skupiny v rámci vašeho předplatného, použijte [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup). |
| Získání informací o virtuálním počítači |Get-AzureRmVM - ResourceGroupName $myResourceGroup-název $myVM |

## <a name="manage-vms"></a>Správa virtuálních počítačů
| Úkol | Příkaz |
| --- | --- |
| Spuštění virtuálního počítače |[Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) - ResourceGroupName $myResourceGroup-název $myVM |
| Zastavení virtuálního počítače |[Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) - ResourceGroupName $myResourceGroup-název $myVM |
| Restartujte spuštění virtuálního počítače |[Restart-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) - ResourceGroupName $myResourceGroup-název $myVM |
| Odstranění virtuálního počítače |[Remove-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) - ResourceGroupName $myResourceGroup-název $myVM |
| Generalize virtuálního počítače |[Set-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvm) - ResourceGroupName $myResourceGroup-název $myVM-zobecněn<BR></BR><BR></BR>Tento příkaz spustíte před spuštěním AzureRmVMImage uložit. |
| Zachycení virtuálního počítače |[Uložit AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) - ResourceGroupName $myResourceGroup - VMName $myVM - DestinationContainerName "myImageContainer" - VHDNamePrefix "myImagePrefix"-"C:\filepath\filename.json" cesta<BR></BR><BR></BR>Virtuální počítač musí být [připravený, vypněte a zobecněn](prepare-for-upload-vhd-image.md) toocreate toobe použít bitovou kopii. Než spustíte tento příkaz, spusťte sadu-AzureRmVm. |
| Aktualizace virtuálního počítače |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) - ResourceGroupName $myResourceGroup - VM $vm<BR></BR><BR></BR>Získat aktuální konfigurace virtuálního počítače hello pomocí Get-AzureRmVM, změňte nastavení konfigurace na objekt virtuálního počítače hello a poté spusťte tento příkaz. |
| Přidání disku tooa data virtuálního počítače |[Přidat AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk) - VM $vm-https://mystore1.blob.core.windows.net/vhds/myDataDisk.vhd"název"myDataDisk"- VhdUri" - LUN #-ukládání do mezipaměti v režimu ReadWrite - DiskSizeinGB # - CreateOption prázdný<BR></BR><BR></BR>Použijte objekt Get-AzureRmVM tooget hello virtuálních počítačů. Zadejte počet LUN hello a hello velikost disku hello. Spuštění aktualizace-AzureRmVM tooapply hello konfigurace změny toohello virtuálních počítačů. Hello disk, který přidáte není inicializován. |
| Odebrání datového disku z virtuálního počítače |[Odebrat AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmdatadisk) - VM $vm-Name "myDataDisk"<BR></BR><BR></BR>Použijte objekt Get-AzureRmVM tooget hello virtuálních počítačů. Spuštění aktualizace-AzureRmVM tooapply hello konfigurace změny toohello virtuálních počítačů. |
| Přidat tooa rozšíření virtuálního počítače |[Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) - ResourceGroupName $myResourceGroup-umístění $location - VMName $myVM-název "Název_rozšíření"-vydavatele "publisherName"-typu "extensionType" - TypeHandlerVersion "#. #" – nastavení $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Spusťte tento příkaz s hello odpovídající [informace o konfiguraci](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) hello rozšíření, které chcete tooinstall. |
| Odebrání rozšíření virtuálního počítače |[Odebrat AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmextension) - ResourceGroupName $myResourceGroup-Name "Název_rozšíření" - VMName $myVM |

## <a name="next-steps"></a>Další kroky
* Informace o hello základní kroky pro vytvoření virtuálního počítače v [vytvořit virtuální počítač s Windows pomocí Resource Manageru a prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

