---
title: "aaaUpload toocreate virtuálního pevného disku generalizace více virtuálních počítačů v Azure | Microsoft Docs"
description: "Nahrajte zobecněný virtuální pevný disk tooan úložiště Azure účet toocreate toouse virtuální počítač s Windows pomocí modelu nasazení Resource Manager hello."
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
ms.date: 05/18/2017
ms.author: cynthn
ms.openlocfilehash: aa1af2a0acf81685e62853de71afa51e819cb696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-tooazure-toocreate-a-new-vm"></a>Nahrát zobecněný toocreate tooAzure virtuální pevný disk nového virtuálního počítače

Toto téma popisuje odesílání účtu úložiště tooa zobecněný nespravované disk a pak vytvořit nový virtuální počítač pomocí disku hello nahrát. Bitovou kopii zobecněný virtuální pevný disk má měly všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep. 

Pokud chcete toocreate virtuálního počítače z specializované virtuálního pevného disku v účtu úložiště, najdete v části [vytvoření virtuálního počítače z disku VHD specializované](sa-create-vm-specialized.md).

Toto téma popisuje používání účtů úložiště, ale doporučujeme, aby zákazníci přesunout disky toousing spravované místo. Kompletní návod jak tooprepare, odesílání a vytvořit nový virtuální počítač pomocí správy disků najdete v části [vytvořit nový virtuální počítač z zobecněný virtuální pevný disk nahrán tooAzure pomocí disků spravované](upload-generalized-managed.md).



## <a name="prepare-hello-vm"></a>Příprava hello virtuálních počítačů

Zobecněný virtuální pevný disk má měly všechny vaše osobní účet informace odebrat pomocí nástroje Sysprep. Pokud máte v úmyslu toouse hello virtuálního pevného disku jako image toocreate nové virtuální počítače z, měli byste:
  
  * [Příprava virtuální pevný disk Windows tooupload tooAzure](prepare-for-upload-vhd-image.md). 
  * Generalize hello virtuálního počítače pomocí nástroje Sysprep

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Generalize virtuálního počítače s Windows pomocí nástroje Sysprep
V této části se dozvíte, jak toogeneralize virtuálního počítače Windows pro použití jako obrázek. Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek. Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).

Ujistěte se, že role serveru hello spuštěné na počítači hello jsou podporovány nástrojem Sysprep. Další informace najdete v tématu [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Pokud používáte nástroj Sysprep před nahráním vaší tooAzure virtuálního pevného disku pro hello poprvé, ujistěte se, máte [připravit virtuální počítač](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před spuštěním nástroje Sysprep. 
> 
> 

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


## <a name="upload-hello-vhd"></a>Nahrát hello virtuálního pevného disku

Nahrajte účtu úložiště Azure tooan hello virtuálního pevného disku.

### <a name="log-in-tooazure"></a>Přihlaste se tooAzure
Pokud ještě nemáte prostředí PowerShell verze 1.4 nebo vyšší nainstalovaná, přečtěte si [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

1. Otevřete prostředí Azure PowerShell a přihlaste se tooyour účet Azure. Automaticky otevírané okno otevře pro tooenter jste přihlašovací údaje účtu Azure.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Pořiďte si předplatné hello ID pro dostupných předplatných.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Nastavit správné předplatné hello pomocí ID hello předplatného. Nahraďte `<subscriptionID>` s hello ID hello opravte předplatného.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-hello-storage-account"></a>Získat účet úložiště hello
Je nutné účet úložiště na image virtuálního počítače Azure toostore hello nahrát. Můžete použít existující účet úložiště, nebo vytvořte novou. 

účty úložiště k dispozici hello tooshow, zadejte:

```powershell
Get-AzureRmStorageAccount
```

Pokud chcete toouse stávající účet úložiště, pokračovat toohello [image virtuálního počítače hello nahrávání](#upload-the-vm-vhd-to-your-storage-account) části.

Pokud potřebujete toocreate účet úložiště, postupujte takto:

1. Je nutné hello název skupiny prostředků hello, kde se vytvořit účet úložiště hello. toofind se všechny skupiny zdrojů hello, které jsou v rámci vašeho předplatného, typ:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    skupinu prostředků s názvem toocreate **myResourceGroup** v hello **západní USA** oblast, zadejte:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Vytvořit účet úložiště s názvem **můj_účet_úložiště** v této skupině prostředků s použitím hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-hello-upload"></a>Spustit odeslání hello 

Použití hello [přidat AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) rutiny tooupload hello image tooa kontejneru v účtu úložiště. Tento příklad nahrávání hello souboru **myVHD.vhd** z `"C:\Users\Public\Documents\Virtual hard disks\"` tooa účet úložiště s názvem **můj_účet_úložiště** v hello **myResourceGroup** skupinu prostředků. soubor Hello budou umístěny do hello kontejner s názvem **můj_kontejner** a bude nový název souboru hello **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Pokud bylo úspěšné, můžete získat odpovědi, která vypadá podobně jako toothis:

```powershell
MD5 hash is being calculated for hello file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for hello operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

V závislosti na vašem síťovém připojení a hello velikost vašeho souboru virtuálního pevného disku, tento příkaz může chvíli trvat toocomplete.


## <a name="create-a-new-vm"></a>Vytvoření nového virtuálního počítače 

Můžete se teď může použít hello nahrán toocreate virtuální pevný disk nového virtuálního počítače. 

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
Hello následující skript prostředí PowerShell ukazuje, jak tooset hello konfigurace virtuálních počítačů a použití hello odesílané image virtuálního počítače jako hello zdroj pro novou instalaci hello.



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

## <a name="verify-that-hello-vm-was-created"></a>Ověřte, že hello, ke které byl virtuální počítač vytvořen
Po dokončení byste měli vidět hello nově vytvořený virtuální počítač v hello [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících hello Příkazy prostředí PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Další kroky
toomanage nový virtuální počítač s prostředím Azure PowerShell najdete v části [Správa virtuálních počítačů pomocí Azure Resource Manageru a prostředí PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


