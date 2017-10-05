---
title: "Vytvoření virtuálního počítače ze spravovaných image virtuálního počítače v Azure | Microsoft Docs"
description: "Vytvoření virtuálního počítače Windows z zobecněný spravované image virtuálního počítače pomocí prostředí Azure PowerShell v modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.openlocfilehash: 2bb2d66271178a64ec0f4642e46b23f5618a56d9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-vm-from-a-managed-image"></a>Vytvoření virtuálního počítače ze spravovaných bitové kopie

Můžete vytvořit víc virtuálních počítačů ze spravovaných image virtuálního počítače v Azure. Spravované image virtuálního počítače obsahuje informace potřebné k vytvoření virtuálního počítače, včetně disků operačního systému a data. Virtuální pevné disky, které tvoří bitovou kopii, včetně disků operačního systému a všech datových disků, jsou uloženy jako spravované disky. 


## <a name="prerequisites"></a>Požadavky

Musíte mít již [vytvořit spravované image virtuálního počítače](capture-image-resource.md) použít pro vytvoření nového virtuálního počítače. 

Ujistěte se, že máte nejnovější verzi modulů AzureRM.Compute a AzureRM.Network prostředí PowerShell. Otevřete příkazovém řádku prostředí PowerShell jako správce a spusťte následující příkaz k instalaci.

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).



## <a name="collect-information-about-the-image"></a>Shromažďovat informace o bitové kopii

Nejprve musíme shromažďovat základní informace o bitové kopii a vytvořte proměnnou pro bitovou kopii. Tento příklad používá spravované image virtuálního počítače s názvem **myImage** který se v **myResourceGroup** skupinu prostředků **– Západ střední USA** umístění. 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě
Vytvořit virtuální síť a podsíť [virtuální sítě](../../virtual-network/virtual-networks-overview.md).

1. Vytvořte podsíť. Tento příklad vytvoří podsíť s názvem **mySubnet** s předponou adresy z **10.0.0.0/24**.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Vytvořte virtuální síť. Tento příklad vytvoří virtuální síť s názvem **myVnet** s předponou adresy z **10.0.0.0/16**.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Vytvoření veřejné IP adresy a síťového rozhraní

Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.

1. Vytvoření veřejné IP adresy. Tento příklad vytvoří veřejnou IP adresu s názvem **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Vytvořit síťovou kartu. Tento příklad vytvoří síťový adaptér s názvem **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Vytvořte skupinu zabezpečení sítě a pravidlo protokolu RDP

Abyste mohli přihlásit k virtuálnímu počítači pomocí protokolu RDP, musíte mít pravidla zabezpečení sítě (NSG), který umožňuje přístup RDP na portu 3389. 

Tento příklad vytvoří skupinu NSG s názvem **myNsg** obsahující pravidlo názvem **myRdpRule** přes port 3389 umožňuje provoz protokolu RDP. Další informace o skupinách Nsg najdete v tématu [otevřít porty pro virtuální počítač v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Vytvořte proměnnou pro virtuální síť

Vytvořte proměnnou pro dokončené virtuální síť. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a>Získání přihlašovacích údajů pro virtuální počítač.

Následující rutiny otevře okno, kde bude zadejte nové uživatelské jméno a heslo pro použití jako účet místního správce pro vzdálený přístup k virtuálnímu počítači. 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a>Nastavení proměnných pro název virtuálního počítače, název počítače a velikosti virtuálního počítače

1. Vytváření proměnných pro název virtuálního počítače a název počítače. Tento příklad nastaví název virtuálního počítače jako **Můjvp** a název počítače jako **tento počítač**.

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. Nastavení velikosti virtuálního počítače. Tento příklad vytvoří **Standard_DS1_v2** velikost virtuálního počítače. Najdete v článku [velikosti virtuálních počítačů](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) Další informace naleznete v dokumentaci.

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. Přidejte název virtuálního počítače a velikost ke konfiguraci virtuálních počítačů.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>Nastavit image virtuálního počítače jako zdroj bitové kopie pro nový virtuální počítač

Nastavte zdrojové bitové kopie pomocí ID spravované image virtuálního počítače.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a>Nastavení konfigurace operačního systému a přidejte na síťový adaptér.

Zadejte typ úložiště (PremiumLRS nebo StandardLRS) a velikost disku operačního systému. Tento příklad nastaví typ účtu **PremiumLRS**, velikost disku na **128 GB** a ukládání do mezipaměti na disku ke **ReadWrite**.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a>Vytvořte virtuální počítač.

Vytvoření nového virtuálního počítače pomocí konfigurace, který jsme vytvořen a uložen v **$vm** proměnné.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a>Zkontrolujte, zda byl vytvořen virtuální počítač
Po dokončení byste měli vidět nově vytvořený virtuální počítač v [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících příkazů prostředí PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Další kroky
Ke správě nového virtuálního počítače v prostředí Azure PowerShell najdete [vytvořit a spravovat virtuální počítače Windows pomocí modulu Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

