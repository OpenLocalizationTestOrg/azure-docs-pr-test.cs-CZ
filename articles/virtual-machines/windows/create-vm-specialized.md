---
title: "aaaCreate virtuální počítač s Windows z specializované virtuálního pevného disku v Azure | Microsoft Docs"
description: "Vytvořte nový virtuální počítač Windows připojením specializované spravované disk jako disk s operačním systémem hello použití v modelu nasazení Resource Manager hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cynthn
ms.openlocfilehash: c72c6f4fb650e2664e87cf38ec9be62f0b2209fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a>Vytvoření virtuálního počítače s Windows z specializované disku

Vytvoření nového virtuálního počítače připojením specializované spravované disk jako disk s operačním systémem hello pomocí prostředí Powershell. Specializované disk je kopie virtuálního pevného disku (VHD) ze stávajícího virtuálního počítače, který udržuje hello uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače. 

Pokud používáte specializované toocreate virtuální pevný disk nového virtuálního počítače, hello nový virtuální počítač bude mít název počítače hello hello původní virtuální počítač. Další informace specifické pro počítač se také nacházet a v některých případech tyto duplicitní informace může způsobit problémy. Být vědět, jaké typy informace specifické pro počítač aplikace se spoléhají na při kopírování virtuálního počítače.

Máte dvě možnosti:
* [Nahrání virtuálního pevného disku](#option-1-upload-a-specialized-vhd)
* [Zkopírujte existující virtuální počítač Azure](#option-2-copy-an-existing-azure-vm)

Toto téma ukazuje, jak spravovat toouse disky. Pokud máte starší verzi nasazení, vyžaduje použití účtu úložiště najdete v tématu [vytvoření virtuálního počítače z specializované virtuálního pevného disku v účtu úložiště](sa-create-vm-specialized.md)

## <a name="before-you-begin"></a>Než začnete
Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello. 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).


## <a name="option-1-upload-a-specialized-vhd"></a>Možnost 1: Nahrát specializované virtuálního pevného disku

Můžete nahrát hello virtuální pevný disk z virtuálního počítače se specializovanou vytvořené nástrojem místní virtualizace, jako je technologie Hyper-V nebo na virtuální počítač exportovat z jiného cloudu.

### <a name="prepare-hello-vm"></a>Příprava hello virtuálních počítačů
Pokud máte v úmyslu toouse hello virtuálního pevného disku jako-je toocreate nový virtuální počítač, je doplnit hello následující kroky. 
  
  * [Příprava virtuální pevný disk Windows tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Nechcete** generalize hello virtuálního počítače pomocí nástroje Sysprep.
  * Odeberte všechny hosta virtualizačních nástrojů a agentů, které jsou nainstalované na hello virtuálního počítače (například nástroje VMware).
  * Ujistěte se, hello virtuální počítač je nakonfigurovaný toopull jeho IP adresu a nastavení DNS pomocí protokolu DHCP. Tím se zajistí, že tento server hello získá IP adresu v rámci hello virtuální síť při spuštění. 


### <a name="get-hello-storage-account"></a>Získat účet úložiště hello
Je třeba úložiště účtu v Azure toostore hello nahrát virtuální pevný disk. Můžete použít existující účet úložiště, nebo vytvořte novou. 

účty úložiště k dispozici hello tooshow, zadejte:

```powershell
Get-AzureRmStorageAccount
```

Pokud chcete toouse stávající účet úložiště, pokračovat toohello [hello nahrání virtuálního pevného disku](#upload-the-vhd-to-your-storage-account) části.

Pokud potřebujete toocreate účet úložiště, postupujte takto:

1. Je nutné hello název skupiny prostředků hello, kde se vytvořit účet úložiště hello. toofind se všechny skupiny zdrojů hello, které jsou v rámci vašeho předplatného, typ:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    skupinu prostředků s názvem toocreate *myResourceGroup* v hello *západní USA* oblast, zadejte:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Vytvořit účet úložiště s názvem *můj_účet_úložiště* v této skupině prostředků s použitím hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-hello-vhd-tooyour-storage-account"></a>Nahrát účet úložiště tooyour hello virtuálního pevného disku 
Použití hello [přidat AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) rutiny tooupload hello virtuálního pevného disku tooa kontejneru v účtu úložiště. Tento příklad nahrávání hello souboru *myVHD.vhd* z `"C:\Users\Public\Documents\Virtual hard disks\"` tooa účet úložiště s názvem *můj_účet_úložiště* v hello *myResourceGroup* skupinu prostředků. Hello soubor je uložen v hello kontejner s názvem *můj_kontejner* a bude nový název souboru hello *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
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

V závislosti na vašem síťovém připojení a hello velikost vašeho souboru virtuálního pevného disku, tento příkaz může chvíli trvat toocomplete

### <a name="create-a-managed-disk-from-hello-vhd"></a>Vytvoření spravovaného disku ze hello virtuálního pevného disku

Vytvoření spravovaného disku ze hello specializuje virtuálního pevného disku v účtu úložiště pomocí [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Tento příklad používá **myOSDisk1** pro název disku hello, PUT hello disk v *StandardLRS* úložiště a používá *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* jako hello identifikátor URI pro hello zdrojového virtuálního pevného disku.

Vytvořit novou skupinu prostředků pro hello nového virtuálního počítače.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

Vytvořit nový disk operačního systému hello z hello nahrát virtuální pevný disk. 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a>Možnost 2: Zkopírujte existující virtuální počítač Azure

Můžete vytvořit kopii virtuálního počítače, že používá spravuje disky pořízení snímku hello virtuálních počítačů, a potom pomocí této toocreate snímku nové spravovaného disku a vytvoření nového virtuálního počítače.


### <a name="take-a-snapshot-of-hello-os-disk"></a>Pořízení snímku hello disk operačního systému

Můžete provést z snímek a celý virtuální počítač (včetně všech disků) nebo právě jeden disk. Hello následující kroky vám ukážou, jak tootake snímek právě hello operačního systému disku virtuální počítač pomocí hello [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) rutiny. 

Nastavte některé parametry. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

Získejte objekt virtuálního počítače hello.

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
Získáte název disku hello operačního systému.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

Vytvoření snímku konfigurace hello. 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

Pořízení snímku hello.

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


Pokud máte v plánu toouse hello snímku toocreate virtuální počítač, který potřebuje toobe vysoké provádění, použijte parametr hello `-AccountType Premium_LRS` pomocí příkazu New-AzureRmSnapshot hello. Parametr Hello vytvoří snímek hello tak, aby je uložena jako spravovaný Disk úrovně Premium. Premium spravované disky jsou dražší než Standard. Proto ujistěte se, že je skutečně potřebujete Premium před použitím parametru hello.

### <a name="create-a-new-disk-from-hello-snapshot"></a>Vytvoření nového disku ze snímku hello

Vytvoření spravovaného disku ze snímku pomocí hello [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Tento příklad používá *myOSDisk* pro název disku hello.

Vytvořit novou skupinu prostředků pro hello nového virtuálního počítače.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

Nastavte název disku hello operačního systému. 

```powershell
$osDiskName = 'myOsDisk'
```

Vytvoření hello spravovaného disku.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-hello-new-vm"></a>Vytvoření nového virtuálního počítače hello 

Vytvoření sítě a jiných toobe prostředky virtuálních počítačů používané hello nového virtuálního počítače.

### <a name="create-hello-subnet-and-vnet"></a>Vytvoření podsítě hello a virtuální sítě

Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).

Vytvořte podsíť hello. Tento příklad vytvoří podsíť s názvem **mySubNet**, ve skupině prostředků hello **myDestinationResourceGroup**, a nastaví hello předpona adresy podsítě příliš**10.0.0.0/24**.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

Vytvoření sítě vNet hello. Tento příklad sady hello toobe název virtuální sítě **myVnetName**, hello umístění příliš**západní USA**, a příliš hello předpona adresy pro virtuální síť hello**10.0.0.0/16**. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP
možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389. Protože hello virtuálního pevného disku pro nový virtuální počítač byl vytvořen z existující virtuální počítač specializované hello, můžete použít účet z hello zdrojového virtuálního počítače pro protokol RDP.

Tento příklad sady hello NSG název příliš**myNsg** a hello RDP název pravidla příliš**myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Další informace o koncových bodů a pravidla NSG najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Vytvoření veřejné IP adresy a síťové karty
tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.

Vytvoření veřejné IP adresy hello. V tomto příkladu je název hello veřejné IP adresy nastavit příliš**myIP**.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

Vytvoření hello síťový adaptér. V tomto příkladu název hello síťový adaptér se nastaví příliš**myNicName**.
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-hello-vm-name-and-size"></a>Nastavte název virtuálního počítače hello a velikosti

Tento příklad sady hello název virtuálního počítače příliš*Můjvp* a velikost hello virtuálního počítače příliš*Standard_A2*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a>Přidat hello síťový adaptér
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-hello-os-disk"></a>Přidání disku hello operačního systému 

Přidat hello operačního systému disku toohello konfigurace pomocí [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). Tento příklad nastaví hello velikost disku hello příliš*128 GB* a připojí hello spravovaných disků jako *Windows* disk operačního systému.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-hello-vm"></a>Dokončení hello virtuálních počítačů 

Vytvořit virtuální počítač pomocí hello [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)hello konfigurace, které jsme právě vytvořili.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Pokud tento příkaz byl úspěšný, zobrazí se výstup takto:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-hello-vm-was-created"></a>Ověřte, že hello, ke které byl virtuální počítač vytvořen
Měli byste vidět hello nově vytvořený virtuální počítač buď v hello [portál Azure](https://portal.azure.com)v části **Procházet** > **virtuální počítače**, nebo pomocí prostředí PowerShell následující hello příkazy:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Další kroky
toosign v tooyour nový virtuální počítač, procházet toohello virtuálních počítačů v hello [portál](https://portal.azure.com), klikněte na tlačítko **Connect**a soubor Remote Desktop RDP otevřete hello. Pomocí přihlašovacích údajů účtu hello z vašeho původního toosign virtuálního počítače v tooyour nového virtuálního počítače. Další informace najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md).

