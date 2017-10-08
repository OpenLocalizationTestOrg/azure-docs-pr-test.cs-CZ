---
title: "aaaCreate nespravované image zobecněný virtuální počítač v Azure | Microsoft Docs"
description: "Vytvořte image unmanged zobecněný toocreate toouse virtuální počítač s Windows více kopií virtuálního počítače v Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: b8a044d20313efa6dd9b9757e61154f922134476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-unmanaged-vm-image-from-an-azure-vm"></a>Jak obrázek toocreate nespravované virtuálního počítače z virtuálního počítače Azure

Tento článek popisuje používání účtů úložiště. Doporučujeme místo účtu úložiště používat spravované disky a spravované bitové kopie. Další informace najdete v tématu [zaznamenat bitovou kopii spravované zobecněný virtuálního počítače v Azure](capture-image-resource.md).

Tento článek ukazuje, jak toouse prostředí Azure PowerShell toocreate image zobecněný virtuálního počítače Azure pomocí účtu úložiště. Potom můžete hello image toocreate jiným virtuálním Počítačem. Hello image obsahuje hello disk operačního systému a hello datových disků, které jsou připojené toohello virtuálního počítače. Obrázek Hello neobsahuje hello virtuální síťové prostředky, proto musíte tooset si tyto prostředky při vytváření nového virtuálního počítače hello. 

## <a name="prerequisites"></a>Požadavky
Je třeba toohave prostředí Azure PowerShell verze 1.0.x nebo novější. Pokud jste ještě nenainstalovali prostředí PowerShell, přečtěte si [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) kroky instalace.

## <a name="generalize-hello-vm"></a>Generalize hello virtuálních počítačů 
V této části se dozvíte, jak toogeneralize virtuálního počítače Windows pro použití jako obrázek. Generalizací virtuálního počítače odebere všechny osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek. Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).

Ujistěte se, že role serveru hello spuštěné na počítači hello jsou podporovány nástrojem Sysprep. Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Pokud nahráváte vaší tooAzure virtuálního pevného disku pro hello poprvé, zajistěte, aby byla [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep. 
> 
> 

Můžete také generalize virtuálního počítače s Linuxem pomocí `sudo waagent -deprovision+user` a potom pomocí prostředí PowerShell toocapture hello virtuálních počítačů. Informace o používání rozhraní příkazového řádku toocapture hello virtuálního počítače najdete v tématu [jak hello toogeneralize a zachycení virtuálního počítače Linux pomocí příkazového řádku Azure CLI ](../linux/capture-image.md).


1. Přihlaste se toohello virtuálního počítače Windows.
2. Otevřete okno příkazového řádku hello jako správce. Změnit adresář, hello příliš**%windir%\system32\sysprep**a poté spusťte `sysprep.exe`.
3. V hello **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že hello **generalizace** je zaškrtnuté políčko.
4. V **možnosti vypnutí**, vyberte **vypnutí**.
5. Klikněte na **OK**.
   
    ![Spusťte nástroj Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. Po dokončení nástroj Sysprep vypne hello virtuálního počítače. 

> [!IMPORTANT]
> Nerestartovat hello virtuálních počítačů, dokud jste done odesílání tooAzure hello virtuálního pevného disku nebo vytvoření bitové kopie z hello virtuálních počítačů. Pokud omylem získá restartována hello virtuálního počítače, spusťte nástroj Sysprep toogeneralize ho znovu.
> 
> 

## <a name="log-in-tooazure-powershell"></a>Přihlaste se tooAzure prostředí PowerShell
1. Otevřete prostředí Azure PowerShell a přihlaste se tooyour účet Azure.
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    Automaticky otevírané okno otevře pro tooenter jste přihlašovací údaje účtu Azure.
2. Pořiďte si předplatné hello ID pro dostupných předplatných.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Nastavit správné předplatné hello pomocí ID hello předplatného.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-hello-vm-and-set-hello-state-toogeneralized"></a>Deallocate hello virtuálních počítačů a nastavte toogeneralized stavu hello
1. Deallocate hello prostředky virtuálních počítačů.
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    Hello *stav* pro hello virtuálních počítačů v hello Azure portal změní z **Zastaveno** příliš**zastaveném (nepřiřazeném)**.
2. Nastavte stav hello hello virtuálního počítače příliš**zobecněno**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. Zkontrolujte stav hello hello virtuálních počítačů. Hello **OSState/zobecněn** části pro hello virtuální počítač by měl mít hello **DisplayStatus** nastavit příliš**zobecněný virtuální počítač**.  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-hello-image"></a>Vytvoření bitové kopie hello

Vytvoření image nespravované virtuální počítač v hello cílový kontejner úložiště použití tohoto příkazu. Hello bitové kopie je vytvořen v hello stejný účet úložiště jako hello původní virtuální počítač. Hello `-Path` parametr uloží kopii hello šablona JSON pro hello zdrojový virtuální počítač tooyour místní počítač. Hello `-DestinationContainerName` parametr je hello název kontejneru hello, které chcete toohold obrázků. Pokud hello kontejner neexistuje, vytvoří se pro vás.
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
Adresa URL hello vaší bitové kopie můžete získat z hello šablona souboru JSON. Přejděte toohello **prostředky** > **storageProfile** > **osDisk** > **image**  >  **uri** části hello kompletní cesta bitové kopie. vypadá Hello adresa URL obrázku hello: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
   
Můžete si taky ověřit hello URI hello portálu. Obrázek Hello je zkopírovaný tooa kontejner s názvem **systému** ve vašem účtu úložiště. 

## <a name="create-a-vm-from-hello-image"></a>Vytvoření virtuálního počítače z hello image

Nyní můžete vytvořit jeden nebo více virtuálních počítačů z nespravovaných image hello.

### <a name="set-hello-uri-of-hello-vhd"></a>Nastavit hello URI hello virtuálního pevného disku

Hello identifikátor URI pro toouse hello virtuální pevný disk je ve formátu hello: https://**můj_účet_úložiště**.blob.core.windows.net/**můj_kontejner**/**MyVhdName**VHD. V tomto příkladu hello virtuálního pevného disku s názvem **myVHD** v účtu úložiště hello **můj_účet_úložiště** v kontejneru hello **můj_kontejner**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě
Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).

1. Vytvořte podsíť hello. Hello následující ukázka vytvoří podsíť s názvem **mySubnet** ve skupině prostředků hello **myResourceGroup** s předponou adresy hello z **10.0.0.0/24**.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Vytvoření virtuální sítě hello. Hello následující ukázka vytvoří virtuální síť s názvem **myVnet** v hello **západní USA** umístění s předponu adresy hello **10.0.0.0/16**.  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Vytvoření veřejné IP adresy a síťového rozhraní
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

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP
možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389. 

Tento příklad vytvoří skupinu NSG s názvem **myNsg** obsahující pravidlo názvem **myRdpRule** přes port 3389 umožňuje provoz protokolu RDP. Další informace o skupinách Nsg najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a>Vytvořte proměnnou pro virtuální síť hello
Vytvořte proměnnou pro virtuální síť hello byla dokončena. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a>Vytvoření hello virtuálních počítačů
Hello následující PowerShell dokončí hello konfigurace virtuálních počítačů a nespravované image se používá jako hello zdroj pro novou instalaci hello.

</br>

```powershell
    # Enter a new user name and password toouse as hello local administrator account 
    # for remotely accessing hello VM.
    $cred = Get-Credential

    # Name of hello storage account where hello VHD is located. This example sets hello 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of hello virtual machine. This example sets hello VM name as "myVM".
    $vmName = "myVM"

    # Size of hello virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See hello VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for hello VM. This examples sets hello computer name as "myComputer".
    $computerName = "myComputer"

    # Name of hello disk that holds hello OS. This example sets hello 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets hello SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get hello storage account where hello uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set hello VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set hello Windows operating system configuration and add hello NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create hello OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure hello OS disk toobe created from hello existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create hello new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

### <a name="verify-that-hello-vm-was-created"></a>Ověřte, že hello, ke které byl virtuální počítač vytvořen
Po dokončení byste měli vidět hello nově vytvořený virtuální počítač v hello [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících hello Příkazy prostředí PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Další kroky
toomanage nový virtuální počítač s prostředím Azure PowerShell najdete v části [Správa virtuálních počítačů pomocí Azure Resource Manageru a prostředí PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


