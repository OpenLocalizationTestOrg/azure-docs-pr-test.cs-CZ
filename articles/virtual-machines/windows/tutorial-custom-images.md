---
title: "aaaCreate vlastní Image virtuálních počítačů s hello prostředí Azure PowerShell | Microsoft Docs"
description: "Kurz – vytvoření vlastní image virtuálního počítače pomocí hello prostředí Azure PowerShell."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 3a759fe1b7e7b72f531399b0f4a99e341713c6a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Vytvořit vlastní image virtuálního počítače Azure pomocí prostředí PowerShell

Vlastní Image jsou jako obrázky marketplace, ale můžete vytvořit sami. Vlastní Image může být použité toobootstrap konfigurace, jako je například předběžného načítání aplikace, konfigurace aplikace a další konfigurace operačního systému. V tomto kurzu vytvoříte svoji vlastní image virtuálního počítače v Azure. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Nástroj Sysprep a generalize virtuální počítače
> * Vytvoření vlastní image
> * Vytvoření virtuálního počítače z vlastní image
> * Zobrazí seznam všech hello bitové kopie v rámci vašeho předplatného
> * Odstranit bitovou kopii

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Než začnete

Následující postup Hello podrobnosti, jak tootake existující virtuální počítač a zapněte ji do opakovaně použitelné vlastní image, které můžete použít toocreate nové instance virtuálního počítače.

Příklad hello toocomplete v tomto kurzu, musí mít existující virtuální počítač. V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás. Při absolvování hello kurzu, nahraďte hello skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.

## <a name="prepare-vm"></a>Příprava virtuálního počítače

toocreate bitové kopie virtuálního počítače, musíte tooprepare hello virtuálních počítačů pomocí generalizací hello virtuálních počítačů, rušení přidělení a pak označí hello zdrojového virtuálního počítače jako zobecněn v Azure.

### <a name="generalize-hello-windows-vm-using-sysprep"></a>Generalize hello virtuální počítač s Windows pomocí nástroje Sysprep

Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek. Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).


1. Připojte toohello virtuálního počítače.
2. Otevřete okno příkazového řádku hello jako správce. Změnit adresář, hello příliš*%windir%\system32\sysprep*a poté spusťte *sysprep.exe*.
3. V hello **nástroj pro přípravu systému** dialogové okno, vyberte *prostředí Out-of-Box zadejte systému (při prvním zapnutí)*a ujistěte se, že hello *generalizace* je zaškrtnuté políčko.
4. V **možnosti vypnutí**, vyberte *vypnutí* a pak klikněte na **OK**.
5. Po dokončení nástroj Sysprep vypne hello virtuálního počítače. **Nerestartovat hello virtuálních počítačů**.

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a>Deallocate a označit hello jako zobecněný virtuální počítač

toocreate bitovou kopii, hello virtuálních počítačů musí toobe navrácena a označené jako zobecněn v Azure.

Virtuální počítač pomocí deallocated hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

Nastavte stav hello hello virtuálního počítače příliš`-Generalized` pomocí [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-hello-image"></a>Vytvoření bitové kopie hello

Nyní můžete vytvořit image hello virtuálních počítačů pomocí [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) a [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). Hello následující příklad vytvoří bitovou kopii s názvem *myImage* z virtuálního počítače s názvem *Můjvp*.

Získáte hello virtuální počítač. 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

Vytvořte konfiguraci hello bitové kopie.

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Vytvoření bitové kopie hello.

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
``` 

 
## <a name="create-vms-from-hello-image"></a>Vytvořit virtuální počítače z hello image

Teď, když máte image, můžete vytvořit jeden nebo více nové virtuální počítače z hello image. Vytvoření virtuálního počítače z vlastní image je velmi podobné toocreating virtuálního počítače pomocí image pořízenou prostřednictvím Marketplace. Pokud používáte image pořízenou prostřednictvím Marketplace, máte tooinformation o hello image, image poskytovatele, nabídky, SKU a verzi. Vlastní Image bude třeba jen tooprovide hello ID hello vlastní prostředek obrázku. 

V následující skript hello, vytvoříme proměnnou *$image* toostore informace o používání vlastní image hello [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) a pak nám pomocí [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage)a zadejte ID hello pomocí hello *$image* proměnné jsme právě vytvořili. 

skript Hello vytvoří virtuální počítač s názvem *myVMfromImage* z našich vlastní image ve novou skupinu prostředků s názvem *myResourceGroupFromImage* v hello *západní USA* umístění.


```powershell
$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

  $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

  $nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable toostore information about hello image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want toocreate hello VM from and image and provide hello image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a>Správy bitových kopií 

Zde jsou některé příklady běžné úlohy správy bitové kopie a jak toocomplete je pomocí prostředí PowerShell.

Zobrazí seznam všech Image podle názvu.

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Odstraňte bitovou kopii. Tento příklad odstranění hello image s názvem *myOldImage* z hello *myResourceGroup*.

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili vlastní image virtuálního počítače. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Nástroj Sysprep a generalize virtuální počítače
> * Vytvoření vlastní image
> * Vytvoření virtuálního počítače z vlastní image
> * Zobrazí seznam všech hello bitové kopie v rámci vašeho předplatného
> * Odstranit bitovou kopii

Posunutí další kurz toolearn toohello o tom, jak vysoce dostupných virtuálních počítačů.

> [!div class="nextstepaction"]
> [Vytvoření vysoce dostupných virtuálních počítačů](tutorial-availability-sets.md)



