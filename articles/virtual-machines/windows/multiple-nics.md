---
title: "aaaCreate a spravovat virtuální počítače Windows v Azure, použít několik síťových adaptérů | Microsoft Docs"
description: "Zjistěte, jak toocreate a spravovat virtuální počítač Windows, který má více síťových adaptérů připojených tooit pomocí šablony Azure PowerShell nebo správce prostředků."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: c3c7d7569aca6f047238146d84b2ffccf05d4079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Vytvoření a Správa virtuálního počítače s Windows, který má několik síťových adaptérů
Virtuální počítače (VM) v Azure může mít několik toothem karty (NIC) připojené rozhraní virtuální sítě. Obvyklým scénářem je toohave různé podsítě pro připojení front-end a back-end nebo síť vyhrazený tooa monitorování nebo řešení zálohování. Tento článek podrobně popisuje, jak toocreate virtuální počítač, který má několik síťových adaptérů připojen tooit. Také zjistíte, jak tooadd nebo odeberte síťové adaptéry ze stávajícího virtuálního počítače. Různé [velikosti virtuálních počítačů](sizes.md) podporu různých počet síťových adaptérů, takže odpovídajícím způsobem upravit velikost virtuálního počítače.

Podrobné informace, včetně jak toocreate několik síťových adaptérů v rámci své vlastní skripty prostředí PowerShell najdete v části [nasazení virtuálních počítačů více Síťových](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="prerequisites"></a>Požadavky
Ujistěte se, že máte hello [nejnovější verzi prostředí Azure PowerShell nainstalovaný a nakonfigurovaný](/powershell/azure/overview).

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.


## <a name="create-a-vm-with-multiple-nics"></a>Vytvoření virtuálního počítače s několika síťovými kartami
Nejprve vytvořte skupinu prostředků. Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *EastUs* umístění:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Vytvoření virtuální sítě a podsítě
Obvyklým scénářem je pro virtuální síť toohave dvě nebo více podsítí. Jedna podsíť může být pro front-endu provoz, hello jiné pro provoz back-end. tooconnect tooboth podsítě, můžete pak používat několik síťových adaptérů na vašem virtuálním počítači.

1. Definovat dvě podsítě virtuální sítě s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Hello následující příklad definuje hello podsítě pro *mySubnetFrontEnd* a *mySubnetBackEnd*:

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. Vytvoření virtuální sítě a podsítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Hello následující příklad vytvoří virtuální síť s názvem *myVnet*:

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Vytvořit několik síťových adaptérů
Vytvořte dva síťové adaptéry se [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Připojte jednu podsíť front-end toohello síťový adaptér a jeden síťový adaptér toohello back-end podsíť. Hello následující příklad vytvoří síťové adaptéry s názvem *myNic1* a *myNic2*:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

Obvykle můžete také vytvořit [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) nebo [nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-overview.md) toohelp spravovat a distribuovat provoz do virtuálních počítačů. Hello [podrobnější virtuální počítač více Síťových](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) článek vás provede vytvoření skupiny zabezpečení sítě a přiřazení síťových karet.

### <a name="create-hello-virtual-machine"></a>Vytvoření virtuálního počítače hello
Nyní spusťte toobuild konfiguraci virtuálního počítače. Limit velikost každého virtuálního počítače je hello celkový počet síťových adaptérů, můžete přidat tooa virtuálních počítačů. Další informace najdete v tématu [velikosti virtuálních počítačů Windows](sizes.md).

1. Nastavte přihlašovací údaje toohello váš virtuální počítač `$cred` proměnná následujícím způsobem:

    ```powershell
    $cred = Get-Credential
    ```

2. Zadejte virtuální počítač s [nové AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Hello následující příklad definuje virtuální počítač s názvem *Můjvp* a používá velikost virtuálního počítače, který podporuje více než dva síťové adaptéry (*Standard_DS3_v2*):

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. Vytvoření hello zbytek konfiguraci virtuálních počítačů s [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) a [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). Hello následující ukázka vytvoří virtuální počítač s Windows Server 2016:

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. Připojte hello dvěma síťovými adaptéry, které jste předtím vytvořili pomocí [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. Nakonec vytvořte virtuální počítač s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-tooan-existing-vm"></a>Přidat existující virtuální počítač síťový adaptér tooan
Přidat virtuální síťový adaptér tooan existující virtuální počítač, navrácení hello virtuálních počítačů, tooadd hello virtuální síťovou kartu a pak spusťte hello virtuálních počítačů.

1. Zrušit přidělení virtuálního počítače s hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Získání hello existující konfigurace hello virtuálních počítačů s [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Hello následující příklad načte informace o hello virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Hello následující příklad vytvoří virtuální síťový adaptér s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) s názvem *myNic3* připojená příliš*mySubnetBackEnd*. připojit virtuální síťový adaptér je pak Hello toohello virtuálního počítače s názvem *Můjvp* v *myResourceGroup* s [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    # Get info for hello back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get hello ID of hello new virtual NIC and add tooVM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>Primární virtuální síťové karty
    Jeden z hello síťové adaptéry na virtuálním počítači více síťovými Kartami, musí toobe primární. Pokud jeden z hello existující virtuální síťové karty na hello virtuální počítač je již nastavena jako primární, můžete tento krok přeskočit. Hello následující příklad předpokládá, že dva virtuální síťové karty jsou teď k dispozici na virtuálním počítači a chcete tooadd hello první síťový adaptér (`[0]`) jako primární hello:
        
    ```powershell
    # List existing NICs on hello VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 toobe primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update hello VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. Spustit virtuální počítač s hello [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a>Odeberte síťový adaptér ze stávajícího virtuálního počítače
tooremove virtuální síťový adaptér ze stávajícího virtuálního počítače, můžete zrušit přidělení hello virtuálních počítačů, odeberte hello virtuální síťovou kartu a potom hello spuštění virtuálního počítače.

1. Zrušit přidělení virtuálního počítače s hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Získání hello existující konfigurace hello virtuálních počítačů s [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Hello následující příklad načte informace o hello virtuálního počítače s názvem *Můjvp* v *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Získání informací o hello odeberte síťovou kartu s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface). Hello následující příklad načte informace *myNic3*:

    ```powershell
    # List existing NICs on hello VM if you need toodetermine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. Odebrat hello síťový adaptér s [odebrat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) a potom aktualizovat hello virtuálního počítače s [aktualizace-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm). Hello následující příklad odebere *myNic3* získány `$nicId` v předchozím kroku hello:

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. Spustit virtuální počítač s hello [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Vytvořit několik síťových adaptérů se šablonami
Šablony Azure Resource Manageru poskytují způsob, jak toocreate více instancí prostředku během nasazení, jako je například vytváření několik síťových adaptérů. Šablony Resource Manageru pomocí deklarativní toodefine soubory JSON prostředí. Další informace najdete v tématu [přehled nástroje Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Můžete použít *kopie* toospecify hello počet instancí toocreate:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

Další informace najdete v tématu [vytvoření více instancí pomocí *kopie*](../../resource-group-create-multiple.md). 

Můžete také použít `copyIndex()` tooappend číslo tooa název prostředku. Potom můžete vytvořit *myNic1*, *MyNic2* a tak dále. Hello následující kód ukazuje příklad připojování hodnotu indexu hello:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Kompletní příklad, jak si můžete přečíst [vytvoření několik síťových adaptérů pomocí šablony Resource Manageru](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Další kroky
Zkontrolujte [velikosti virtuálních počítačů Windows](sizes.md) když se snažíte toocreate virtuální počítač, který má několik síťových adaptérů. Věnujte pozornost toohello maximální počet síťových adaptérů, které podporuje každý velikost virtuálního počítače. 


