---
title: "aaaCreate virtuální počítač z bitové kopie spravovaných virtuálních počítačů v Azure | Microsoft Docs"
description: "Vytvoření virtuálního počítače Windows z zobecněný spravované image virtuálního počítače pomocí prostředí Azure PowerShell v modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: 5036ef1533c144a9a328e94599b359e0166f337d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-managed-image"></a>Vytvoření virtuálního počítače ze spravovaných bitové kopie

Můžete vytvořit víc virtuálních počítačů ze spravovaných image virtuálního počítače v Azure. Spravované image virtuálního počítače obsahuje hello informace nezbytné toocreate virtuálního počítače, včetně hello operačního systému a datové disky. Hello virtuální pevné disky, které tvoří hello bitovou kopii, včetně disků hello operačního systému a všech datových disků, jsou uloženy jako spravované disky. 


## <a name="prerequisites"></a>Požadavky

Je třeba toohave již [vytvořit spravované image virtuálního počítače](capture-image-resource.md) hello toouse pro vytvoření nového virtuálního počítače. 

Ujistěte se, že máte nejnovější verzi modulů AzureRM.Compute a prostředí AzureRM.Network PowerShell hello hello. Otevřete příkazovém řádku prostředí PowerShell jako správce a spusťte následující příkaz tooinstall hello je.

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).



## <a name="collect-information-about-hello-image"></a>Shromažďovat informace o bitové kopii hello

Nejprve nutné toogather základní informace o bitových hello a vytvořte proměnnou pro bitovou kopii hello. Tento příklad používá spravované image virtuálního počítače s názvem **myImage** který je v hello **myResourceGroup** skupiny prostředků v hello **– Západ střední USA** umístění. 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě
Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).

1. Vytvořte podsíť hello. Tento příklad vytvoří podsíť s názvem **mySubnet** s předponou adresy hello z **10.0.0.0/24**.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Vytvoření virtuální sítě hello. Tento příklad vytvoří virtuální síť s názvem **myVnet** s předponou adresy hello z **10.0.0.0/16**.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Vytvoření veřejné IP adresy a síťového rozhraní

tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.

1. Vytvoření veřejné IP adresy. Tento příklad vytvoří veřejnou IP adresu s názvem **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Vytvoření hello síťový adaptér. Tento příklad vytvoří síťový adaptér s názvem **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP

možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidla zabezpečení sítě (NSG), který umožňuje přístup RDP na portu 3389. 

Tento příklad vytvoří skupinu NSG s názvem **myNsg** obsahující pravidlo názvem **myRdpRule** přes port 3389 umožňuje provoz protokolu RDP. Další informace o skupinách Nsg najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

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


## <a name="create-a-variable-for-hello-virtual-network"></a>Vytvořte proměnnou pro virtuální síť hello

Vytvořte proměnnou pro virtuální síť hello byla dokončena. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a>Získání přihlašovacích údajů hello pro hello virtuálních počítačů

Hello následující rutiny se otevře okno, kde bude zadat nové toouse jméno a heslo uživatele jako účet místního správce hello pro vzdálený přístup k hello virtuálních počítačů. 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-hello-vm-name-computer-name-and-hello-size-of-hello-vm"></a>Sada proměnných pro hello virtuálních počítačů název, název počítače a hello velikost hello virtuálních počítačů

1. Vytváření proměnných pro hello název virtuálního počítače a název počítače. Tento příklad nastaví hello název virtuálního počítače jako **Můjvp** a název počítače hello jako **tento počítač**.

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. Nastavení velikosti hello hello virtuálního počítače. Tento příklad vytvoří **Standard_DS1_v2** velikost virtuálního počítače. V tématu hello [velikosti virtuálních počítačů](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) Další informace naleznete v dokumentaci.

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. Přidejte hello název virtuálního počítače a konfigurace virtuálního počítače toohello velikost.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a>Sada hello image virtuálního počítače jako zdroj bitové kopie pro hello nového virtuálního počítače

Nastavit hello zdrojové bitové kopie pomocí ID hello hello spravované image virtuálního počítače.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a>Nastavte hello konfigurace operačního systému a přidejte hello síťový adaptér.

Zadejte typ úložiště hello (PremiumLRS nebo StandardLRS) a velikost hello hello disk operačního systému. Tento příklad nastaví typ účtu hello příliš**PremiumLRS**, hello velikost disku příliš**128 GB** a disku ukládání do mezipaměti příliš**ReadWrite**.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a>Vytvoření hello virtuálních počítačů

Vytvoření nového virtuálního počítače pomocí hello konfigurace, který jsme vytvořen a uložen v hello hello **$vm** proměnné.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a>Ověřte, že hello, ke které byl virtuální počítač vytvořen
Po dokončení byste měli vidět hello nově vytvořený virtuální počítač v hello [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících hello Příkazy prostředí PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Další kroky
toomanage nový virtuální počítač s prostředím Azure PowerShell najdete v části [vytvořit a spravovat virtuální počítače Windows hello modul Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

