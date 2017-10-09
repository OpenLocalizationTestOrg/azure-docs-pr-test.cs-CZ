---
title: "aaaCreate spravovaných virtuálních počítačů Azure z disku VHD zobecněný místní | Microsoft Docs"
description: "Nahrát zobecněný virtuální pevný disk tooAzure a použít ho toocreate nové virtuální počítače, v modelu nasazení Resource Manager hello."
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
ms.date: 05/19/2017
ms.author: cynthn
ms.openlocfilehash: 2fd0c0eec922e6ca8af4e712c1bceb1f9466105c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-and-use-it-toocreate-new-vms-in-azure"></a>Nahrát zobecněný virtuální pevný disk a použít ho toocreate nové virtuální počítače v Azure

Toto téma vás provede pomocí prostředí PowerShell tooupload virtuální pevný disk zobecněný tooAzure virtuálního počítače, vytvořte bitovou kopii z hello virtuálního pevného disku a vytvoření nového virtuálního počítače z této bitové kopie. Můžete nahrát virtuální pevný disk exportovat z nástroj virtualizace na místní nebo z jiného cloudu. Pomocí [spravované disky](managed-disks-overview.md) hello nový virtuální počítač zjednodušuje správu hello virtuálních počítačů a poskytuje lepší dostupnost hello virtuálních počítačů při umístění do nastavení dostupnosti. 

Pokud chcete toouse ukázkový skript, najdete v části [ukázkový skript tooupload tooAzure virtuální pevný disk a vytvořte nový virtuální počítač](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Než začnete

- Před nahráním žádné tooAzure virtuálního pevného disku, postupujte podle [připravit tooAzure tooupload Windows VHD nebo VHDX](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Zkontrolujte [plánování migrace hello disky tooManaged](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) před zahájením migrace příliš[spravované disky](managed-disks-overview.md).
- Ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello. Spustit hello následující příkaz tooinstall.

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).


## <a name="generalize-hello-windows-vm-using-sysprep"></a>Generalize hello virtuální počítač s Windows pomocí nástroje Sysprep

Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek. Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).

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
6. Po dokončení nástroj Sysprep vypne hello virtuálního počítače. Nerestartovat hello virtuálních počítačů.



## <a name="log-in-tooazure"></a>Přihlaste se tooAzure
Pokud ještě nemáte prostředí PowerShell verze 1.4 nebo vyšší nainstalovaná, přečtěte si [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

1. Otevřete prostředí Azure PowerShell a přihlaste se tooyour účet Azure. Automaticky otevírané okno otevře pro tooenter jste přihlašovací údaje účtu Azure.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Pořiďte si předplatné hello ID pro dostupných předplatných.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Nastavit správné předplatné hello pomocí ID hello předplatného. Nahraďte  *<subscriptionID>*  s hello ID hello opravte předplatného.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-hello-storage-account"></a>Získat účet úložiště hello
Je nutné účet úložiště na image virtuálního počítače Azure toostore hello nahrát. Můžete použít existující účet úložiště, nebo vytvořte novou. 

Pokud budete používat hello virtuálního pevného disku toocreate spravovaných disků pro virtuální počítač, musí být umístění účtu úložiště hello stejné hello umístění, kde bude vytvářet hello virtuálních počítačů.

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

    skupinu prostředků s názvem toocreate **myResourceGroup** v hello **východní USA** oblast, zadejte:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. Vytvořit účet úložiště s názvem **můj_účet_úložiště** v této skupině prostředků s použitím hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    Platné hodnoty - SkuName jsou:
   
   * **Standard_LRS** -místně redundantní úložiště. 
   * **Standard_ZRS** -zónu redundantní úložiště.
   * **Standard_GRS** -geograficky redundantní úložiště. 
   * **Standard_RAGRS** -geograficky redundantní úložiště s přístupem pro čtení. 
   * **Premium_LRS** -místně redundantního úložiště Premium. 

## <a name="upload-hello-vhd-tooyour-storage-account"></a>Nahrát účet úložiště tooyour hello virtuálního pevného disku

Použití hello [přidat AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) rutiny tooupload hello virtuálního pevného disku tooa kontejneru v účtu úložiště. Tento příklad nahrávání hello souboru *myVHD.vhd* z *"C:\Users\Public\Documents\Virtual pevné disky\"*  tooa účet úložiště s názvem *můj_účet_úložiště*v hello *myResourceGroup* skupinu prostředků. soubor Hello budou umístěny do hello kontejner s názvem *můj_kontejner* a bude nový název souboru hello *myUploadedVHD.vhd*.

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

V závislosti na vašem síťovém připojení a hello velikost vašeho souboru virtuálního pevného disku, tento příkaz může chvíli trvat toocomplete

Uložit hello **identifikátor URI pro cíl** cesta toouse novější, pokud budete toocreate se spravovaným diskem nebo nového virtuálního počítače pomocí hello nahrát virtuální pevný disk.

### <a name="other-options-for-uploading-a-vhd"></a>Další možnosti pro odesílání virtuálního pevného disku
 
 
Můžete také nahrát účtu úložiště tooyour virtuální pevný disk pomocí jedné z následujících hello:

- [AzCopy](http://aka.ms/downloadazcopy)
- [Objekt Blob služby Azure Storage kopie rozhraní API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Objektů BLOB Azure Storage Explorer odesílání](https://azurestorageexplorer.codeplex.com/)
- [Referenční dokumentace rozhraní API úložiště importu/exportu služby REST](https://msdn.microsoft.com/library/dn529096.aspx)
-   Doporučujeme používat službu Import/Export, pokud je předpokládaná doba odesílání doba je delší než 7 dní. Můžete použít [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) čas hello tooestimate z jednotky velikost a přenos dat. 
    Import a Export lze použít toocopy tooa standardní účet úložiště. Budete potřebovat toocopy z účtu úložiště toopremium standardní úložiště pomocí nástroje, například AzCopy.


## <a name="create-a-managed-image-from-hello-uploaded-vhd"></a>Vytvoření spravované bitovou kopii z hello nahrát virtuální pevný disk 

Vytvoření spravované image pomocí vaší zobecněný virtuální pevný disk operačního systému. Hello hodnoty nahraďte svými vlastními informacemi.


1.  Nastavte nejprve, hello společné parametry:

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  Vytvoření image hello pomocí vaší zobecněný virtuální pevný disk operačního systému.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě
Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).

1. Vytvořte podsíť hello. Tento příklad vytvoří podsíť s názvem *mySubnet* s předponou adresy hello z *10.0.0.0/24*.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Vytvoření virtuální sítě hello. Tento příklad vytvoří virtuální síť s názvem *myVnet* s předponou adresy hello z *10.0.0.0/16*.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Vytvoření veřejné IP adresy a síťového rozhraní

tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.

1. Vytvoření veřejné IP adresy. Tento příklad vytvoří veřejnou IP adresu s názvem *myPip*. 
   
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

Tento příklad vytvoří skupinu NSG s názvem *myNsg* obsahující pravidlo názvem *myRdpRule* přes port 3389 umožňuje provoz protokolu RDP. Další informace o skupinách Nsg najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

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

## <a name="add-hello-vm-name-and-size-toohello-vm-configuration"></a>Přidejte hello název virtuálního počítače a konfigurace virtuálního počítače toohello velikost.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a>Sada hello image virtuálního počítače jako zdroj bitové kopie pro hello nového virtuálního počítače

Nastavit hello zdrojové bitové kopie pomocí ID hello hello spravované image virtuálního počítače.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a>Nastavte hello konfigurace operačního systému a přidejte hello síťový adaptér.

Zadejte typ úložiště hello (PremiumLRS nebo StandardLRS) a velikost hello hello disk operačního systému. Tento příklad nastaví typ účtu hello příliš*PremiumLRS*, hello velikost disku příliš*128 GB* a disku ukládání do mezipaměti příliš*ReadWrite*.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a>Vytvoření hello virtuálních počítačů

Vytvoření nového virtuálního počítače pomocí hello konfigurace uložené v hello hello **$vm** proměnné.

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

toosign v tooyour nový virtuální počítač, procházet toohello virtuálních počítačů v hello [portál](https://portal.azure.com), klikněte na tlačítko **Connect**a soubor Remote Desktop RDP otevřete hello. Pomocí přihlašovacích údajů účtu hello z vašeho původního toosign virtuálního počítače v tooyour nového virtuálního počítače. Další informace najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

