---
title: "aaaCreate virtuálních počítačů z specializované disku v Azure | Microsoft Docs"
description: "Vytvoření nového virtuálního počítače připojením specializované nespravované disku, v modelu nasazení Resource Manager hello."
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
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: c88f213b6629a6c1d6ff5845e76c2f7719672714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a>Vytvoření virtuálního počítače z specializované virtuálního pevného disku v účtu úložiště

Vytvoření nového virtuálního počítače připojením specializované nespravované disk jako disk s operačním systémem hello pomocí prostředí Powershell. Specializované disk je kopie virtuálního pevného disku z existující virtuální počítač, který udržuje hello uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače. 

Máte dvě možnosti:
* [Nahrání virtuálního pevného disku](create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [Zkopírujte hello virtuální pevný disk existující virtuální počítač Azure](create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a>Než začnete
Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello. Spustit hello následující příkaz tooinstall.

```powershell
Install-Module AzureRM.Compute 
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).


## <a name="option-1-upload-a-specialized-vhd"></a>Možnost 1: Nahrát specializované virtuálního pevného disku

Můžete nahrát hello virtuální pevný disk z virtuálního počítače se specializovanou vytvořené nástrojem místní virtualizace, jako je technologie Hyper-V nebo na virtuální počítač exportovat z jiného cloudu.

### <a name="prepare-hello-vm"></a>Příprava hello virtuálních počítačů
Můžete nahrát specializované VHD, který byl vytvořen pomocí místní počítač nebo virtuální pevný disk exportovaný z jiného cloudu. Specializované virtuálního pevného disku udržuje hello uživatelské účty, aplikace a další data o stavu z vašeho původního virtuálního počítače. Pokud máte v úmyslu toouse hello virtuálního pevného disku jako-je toocreate nový virtuální počítač, je doplnit hello následující kroky. 
  
  * [Příprava virtuální pevný disk Windows tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Nechcete** generalize hello virtuálního počítače pomocí nástroje Sysprep.
  * Odeberte všechny hosta virtualizačních nástrojů a agentů, které jsou nainstalované na hello virtuálního počítače (tj. nástroje VMware).
  * Ujistěte se, hello virtuální počítač je nakonfigurovaný toopull jeho IP adresu a nastavení DNS pomocí protokolu DHCP. Tím se zajistí, že tento server hello získá IP adresu v rámci hello virtuální síť při spuštění. 


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
   
### <a name="upload-hello-vhd-tooyour-storage-account"></a>Nahrát účet úložiště tooyour hello virtuálního pevného disku
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


## <a name="option-2-copy-hello-vhd-from-an-existing-azure-vm"></a>Možnost 2: Kopírování hello virtuálního pevného disku z existující virtuální počítač Azure

Při vytváření nového virtuálního počítače duplicitní můžete zkopírovat toouse účet úložiště tooanother virtuální pevný disk.

### <a name="before-you-begin"></a>Než začnete
Ujistěte se, že jste:

* Neobsahuje informace o hello **zdrojové a cílové účty úložiště**. Pro hello zdrojového virtuálního počítače budete potřebovat názvy toohave hello úložiště účtu a kontejneru. Obvykle bude název kontejneru hello **virtuální pevné disky**. Budete také potřebovat toohave cílový účet úložiště. Pokud jste již nemáte, můžete vytvořit pomocí portálu buď hello (**více služeb** > účty úložiště > Přidat) nebo pomocí hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny. 
* Stáhli a nainstalovali hello [nástroj AzCopy](../../storage/common/storage-use-azcopy.md). 

### <a name="deallocate-hello-vm"></a>Deallocate hello virtuálních počítačů
Deallocate hello virtuálních počítačů, což uvolní toobe hello virtuální pevný disk zkopírovali. 

* **Portál**: klikněte na tlačítko **virtuální počítače** > **Můjvp** > Zastavit
* **Prostředí PowerShell**: použití [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) toostop (zrušit přidělení) hello virtuálního počítače s názvem **Můjvp** ve skupině prostředků **myResourceGroup**.

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Hello **stav** pro hello virtuálních počítačů v hello Azure portal změní z **Zastaveno** příliš**zastaveném (nepřiřazeném)**.

### <a name="get-hello-storage-account-urls"></a>Získání adres URL účtu úložiště hello
Je nutné adresy URL hello hello zdrojové a cílové úložiště účtů. Hello adresy URL vypadat: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Pokud už znáte hello název účtu a kontejneru úložiště, můžete právě nahradit hello informací mezi hello závorky toocreate adresu URL. 

Můžete použít hello portál Azure nebo Azure Powershell tooget hello adresa URL:

* **Portál**: klikněte na tlačítko hello  **>**  pro **další služby** > **účty úložiště**  >   *účet úložiště* > **objekty BLOB** a váš zdrojový soubor virtuálního pevného disku je pravděpodobně v hello **virtuální pevné disky** kontejneru. Klikněte na tlačítko **vlastnosti** hello kontejneru a kopírovat text hello s názvem bez přípony **URL**. Budete potřebovat kontejnery obou hello zdrojové a cílové adresy URL hello. 
* **Prostředí PowerShell**: použití [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) tooget hello informace pro virtuální počítač s názvem **Můjvp** ve skupině prostředků hello **myResourceGroup**. Ve výsledcích hello Hledat v hello **profilu úložiště** části hello **Uri virtuálního pevného disku**. první část Hello hello identifikátor Uri je hello URL toohello kontejneru a poslední část hello hello název virtuálního pevného disku operačního systému pro hello virtuálních počítačů.

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-hello-storage-access-keys"></a>Získání přístupových klíčů k úložišti hello
Najít hello přístupové klíče pro hello zdrojové a cílové účty úložiště. Další informace o přístupových klíčů najdete v tématu [účty Azure storage](../../storage/common/storage-create-storage-account.md).

* **Portál**: klikněte na tlačítko **další služby** > **účty úložiště** > *účet úložiště*  >  **Přístupové klíče**. Kopírování hello klíč označený jako **key1**.
* **Prostředí PowerShell**: použití [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) tooget hello úložiště klíč pro účet úložiště hello **můj_účet_úložiště** ve skupině prostředků hello  **myResourceGroup**. Kopírování hello klíč s názvem bez přípony **key1**.

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-hello-vhd"></a>Zkopírujte hello virtuálního pevného disku
Můžete zkopírovat soubory mezi účty úložiště pomocí nástroje AzCopy. Pro cílový kontejner hello Pokud hello zadaný kontejner neexistuje, bude vytvořena pro vás. 

toouse AzCopy, otevřete příkazový řádek na místním počítači a přejděte toohello složku, kde je nainstalován nástroj AzCopy. Je podobné příliš*C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

toocopy všechny hello souborů v rámci kontejneru, použijte hello **/S** přepínače. To může být použité toocopy hello virtuálního pevného disku operačního systému a všechny hello datových disků, pokud jsou ve stejném kontejneru hello. Tento příklad ukazuje, jak toocopy všechny hello soubory v kontejneru hello **mysourcecontainer** v účtu úložiště **mysourcestorageaccount** toohello kontejneru **mydestinationcontainer**  v hello **mydestinationstorageaccount** účet úložiště. Nahraďte hello názvy účtů úložiště hello a kontejnery vlastními. Nahraďte `<sourceStorageAccountKey1>` a `<destinationStorageAccountKey1>` s vlastními klíči.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Pokud chcete pouze toocopy konkrétní virtuální pevný disk v kontejneru s více soubory, můžete také zadat název souboru hello pomocí přepínače /Pattern hello. V tomto příkladu pouze hello soubor s názvem **myFileName.vhd** budou zkopírovány.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


Po dokončení, zobrazí se zpráva, která vypadá podobně jako:

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### <a name="troubleshooting"></a>Řešení potíží
* Při použití nástroje AZCopy, pokud se zobrazí chyba hello "Serveru se nezdařilo požadavek tooauthenticate hello", zkontrolujte, zda je hodnota hello hello autorizační hlavičky formátu správně včetně hello podpis. Pokud používáte 2 klíč nebo klíč hello sekundární úložiště, zkuste použít klíč hello primární nebo 1. úložiště.

## <a name="create-hello-new-vm"></a>Vytvoření nového virtuálního počítače hello 

Budete potřebovat toocreate sítě a jiných toobe prostředky virtuálních počítačů používané hello nového virtuálního počítače.

### <a name="create-hello-subnet-and-vnet"></a>Vytvoření podsítě hello a virtuální sítě

Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).

1. Vytvořte podsíť hello. Tento příklad vytvoří podsíť s názvem **mySubNet**, ve skupině prostředků hello **myResourceGroup**, a nastaví hello předpona adresy podsítě příliš**10.0.0.0/24**.
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Vytvoření sítě vNet hello. Tento příklad sady hello toobe název virtuální sítě **myVnetName**, hello umístění příliš**západní USA**, a příliš hello předpona adresy pro virtuální síť hello**10.0.0.0/16**. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-nic"></a>Vytvoření veřejné IP adresy a síťové karty
tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.

1. Vytvoření veřejné IP adresy hello. V tomto příkladu je název hello veřejné IP adresy nastavit příliš**myIP**.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Vytvoření hello síťový adaptér. V tomto příkladu název hello síťový adaptér se nastaví příliš**myNicName**.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP
možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidlo zabezpečení, která umožňuje přístup RDP na portu 3389. Protože specializuje hello virtuálního pevného disku pro dobrý den, kdy byl vytvořen nový virtuální počítač ze stávajícího virtuálního počítače, po vytvoření hello virtuálního počítače je můžete použít existující účet z hello zdrojový virtuální počítač, který měl oprávnění toolog na použití protokolu RDP.
Tento příklad sady hello NSG název příliš**myNsg** a hello RDP název pravidla příliš**myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Další informace o koncových bodů a pravidla NSG najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="set-hello-vm-name-and-size"></a>Nastavte název virtuálního počítače hello a velikosti

Tento příklad sady hello název virtuálního počítače příliš "Můjvp" a hello virtuálních počítačů velikost příliš "Standard_A2".
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a>Přidat hello síťový adaptér
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-hello-os-disk"></a>Konfigurace disku hello operačního systému

1. Nastavte hello identifikátor URI pro hello virtuálního pevného disku, který jste nahráli nebo zkopírovat. V tomto příkladu hello soubor virtuálního pevného disku s názvem **myOsDisk.vhd** je uložen v účtu úložiště s názvem **Můj_účet_úložiště** v kontejneru nazvaném **Můj_kontejner**.

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. Přidání disku hello operačního systému. V tomto příkladu když je vytvořen hello disk operačního systému, hello termín "osDisk" je appened toohello virtuálních počítačů název toocreate hello operačního systému disku název. Tento příklad také určuje, že tento virtuální pevný disk systému Windows musí být připojené toohello virtuální počítač jako disk hello operačního systému.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

Volitelné: Pokud máte datových disků tohoto toobe nutné připojit toohello virtuálních počítačů, přidejte hello datových disků pomocí adres URL hello dat virtuální pevné disky a hello odpovídající logické jednotky (LUN).

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

Pokud používáte účet úložiště, hello data a adresy URL disku operačního systému vypadat přibližně takto: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. To můžete najít na portálu hello procházení toohello cílového úložiště kontejneru, kliknutím na tlačítko hello operačního systému nebo data virtuálního pevného disku, který jste zkopírovali, a pak kopírování hello obsah adresy URL hello.


### <a name="complete-hello-vm"></a>Dokončení hello virtuálních počítačů 

Vytvoření hello virtuálního počítače pomocí hello konfigurace, které jsme právě vytvořili.

```powershell
#Create hello new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
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
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Další kroky
toosign v tooyour nový virtuální počítač, procházet toohello virtuálních počítačů v hello [portál](https://portal.azure.com), klikněte na tlačítko **Connect**a soubor Remote Desktop RDP otevřete hello. Pomocí přihlašovacích údajů účtu hello z vašeho původního toosign virtuálního počítače v tooyour nového virtuálního počítače. Další informace najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md).

