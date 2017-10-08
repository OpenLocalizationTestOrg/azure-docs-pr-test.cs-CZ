---
title: "aaaCreate virtuálního počítače systému SQL Server v prostředí Azure PowerShell (Resource Manager) | Microsoft Docs"
description: "Obsahuje kroky a skriptů prostředí PowerShell pro vytvoření virtuálního počítače Azure s obrázky Galerie virtuálního počítače systému SQL Server."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/17/2017
ms.author: jroth
ms.openlocfilehash: 2b8cb8f69ff9894a95eab617816a60c8674eeefa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell (Resource Manager)
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak toocreate jednoho virtuálního počítače Azure pomocí hello **Azure Resource Manager** modelu nasazení pomocí rutin prostředí Azure PowerShell. V tomto kurzu vytvoříme jednoho virtuálního počítače pomocí jednoho disku z bitové kopie v hello SQL galerie. Vytvoříme nové zprostředkovatele pro hello úložiště, síť a výpočetní prostředky, které se použijí hello virtuálního počítače. Pokud máte existující zprostředkovatele pro kterýkoli z těchto prostředků, můžete místo toho použít těchto zprostředkovatelů.

Pokud třeba hello classic verzi tohoto tématu, přečtěte si téma [zřízení virtuálního počítače s SQL serverem pomocí Azure PowerShell Classic](../classic/ps-sql-create.md).

## <a name="prerequisites"></a>Požadavky
Pro účely tohoto kurzu budete potřebovat:

* Účet Azure a předplatné před zahájením. Pokud nemáte, si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* [Azure PowerShell)](/powershell/azure/overview), minimální verze 1.4.0 nebo novější (Tento kurz vytvořené pomocí verze 1.5.0).
  * tooretrieve verze, typ **Azure Get-Module - ListAvailable**.

## <a name="configure-your-subscription"></a>Konfigurovat předplatné
Otevřete prostředí Windows PowerShell a spuštěním následující rutiny hello vytvořit tooyour přístup k účtu Azure. Zobrazí se přihlašovací obrazovka tooenter přihlašovacích údajů. Použití hello stejné e-mailu a heslo použít toosign v toohello portálu Azure.

    Add-AzureRmAccount

Po úspěšném přihlášení se zobrazí některé informace na obrazovce, která zahrnuje hello ID předplatného, se kterým jste přihlášení. Toto je hello předplatné, ve kterém se vytvoří hello prostředky pro tento kurz nezměníte tooa jiné předplatné. Pokud máte více SubscriptionIds, spusťte následující rutinu tooreturn hello seznam všech vaší SubscriptionIds:

    Get-AzureRmSubscription

tooanother toochange SubscriptionID, spusťte následující rutinu s vaší požadované ID předplatného hello.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Definování proměnné bitovou kopii
toosimplify použitelnost a vysvětlení skriptu hello dokončení tohoto kurzu, spustíme definováním počet proměnných. Hodnoty parametru hello změnit podle svých potřeb, ale pozor pojmenování délky související tooname omezení a speciální znaky, při úpravě hello hodnoty poskytnuté.

### <a name="location-and-resource-group"></a>Umístění a skupiny prostředků
Použití dvou proměnných toodefine hello data oblast a hello skupiny prostředků do které chcete vytvořit hello další prostředky pro virtuální počítač hello.

Podle potřeby a potom spusťte následující rutiny tooinitialize hello tyto proměnné.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Úložiště vlastností
Použijte následující proměnné toodefine hello úložiště účet a hello typ úložiště toobe používané virtuálním počítačem hello hello.

Podle potřeby a potom spusťte následující rutinu tooinitialize hello tyto proměnné. Všimněte si, že v tomto příkladu používáme [Storage úrovně Premium](../../../storage/common/storage-premium-storage.md), který se doporučuje pro úlohy v produkčním prostředí. Podrobnosti na tyto pokyny a dalších doporučeních najdete v tématu [osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Vlastnosti sítě
Použijte následující proměnné toodefine hello síťové rozhraní, metoda přidělení hello TCP/IP, hello název virtuální sítě, název hello virtuální podsítě, hello rozsah IP adres pro virtuální síť hello, hello rozsah IP adres pro podsíť hello a hello hello Popisek toobe název veřejné domény používané hello sítí v hello virtuálního počítače.

Podle potřeby a potom spusťte následující rutinu tooinitialize hello tyto proměnné.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"

### <a name="virtual-machine-properties"></a>Vlastnosti virtuálního počítače
Použijte název proměnné toodefine hello virtuálního počítače, název počítače hello, hello velikost virtuálního počítače a hello název disku operačního systému pro virtuální počítač hello hello.

Podle potřeby a potom spusťte následující rutinu tooinitialize hello tyto proměnné.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Vlastnosti bitové kopie
Použijte následující proměnné toodefine hello image toouse pro virtuální počítač hello hello. V tomto příkladu se používá SQL Server 2016 Enterprise hello bitové kopie.

Podle potřeby a potom spusťte následující rutinu tooinitialize hello tyto proměnné.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Všimněte si, že můžete získat úplný seznam nabídky bitovou kopii systému SQL Server pomocí příkazu Get-AzureRmVMImageOffer hello:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

A uvidíte hello SKU, které jsou k dispozici pro nabídky pomocí příkazu Get-AzureRmVMImageSku hello. Hello následující příkaz ukazuje všech skladových položek k dispozici pro hello **SQL2014SP1 WS2012R2** nabízejí.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Pomocí modelu nasazení Resource Manager hello je hello první objekt, který vytvoříte skupinu prostředků hello. Budeme používat hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) toocreate rutiny skupinu prostředků Azure a jeho prostředků s prostředky hello skupina název a umístění definované hello proměnné, které jste dříve inicializován.

Spusťte následující rutinu toocreate hello nové skupiny prostředků.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
Hello virtuální počítač vyžaduje prostředků úložiště pro disk operačního systému hello a hello dat systému SQL Server a soubory protokolu. Pro jednoduchost vytvoříme jediný disk pro obojí. Můžete připojit další disky později pomocí hello [disku Azure přidat](/powershell/module/azure/add-azuredisk) rutinu v pořadí tooplace data systému SQL Server a soubory protokolu na vyhrazených disků. Budeme používat hello [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny toocreate standardní úložiště účtů v nové skupiny prostředků a s názvem účtu úložiště hello, název Sku úložiště a umístění definovat pomocí hello proměnné, které dříve inicializován.

Spusťte následující rutinu toocreate hello váš nový účet úložiště.

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Vytvoření síťové prostředky
Hello virtuální počítač vyžaduje pro připojení k síti číslo síťových prostředků.

* Každý virtuální počítač vyžaduje virtuální síť.
* Virtuální síť musí mít alespoň jednu podsíť definované.
* Síťové rozhraní musí být definovány se veřejné nebo privátní IP adresy.

### <a name="create-a-virtual-network-subnet-configuration"></a>Vytvoření konfigurace podsítě virtuální sítě
Spustíme vytvořením konfigurace pro naše virtuální síť s podsítí. V našem kurzu vytvoříme výchozí podsíť pomocí hello [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) rutiny. Pomocí názvu a adresy předpona serveru hello podsítě definovat pomocí hello proměnné, které jste dříve inicializovat vytvoříme naše konfigurace podsítě virtuální sítě.

> [!NOTE]
> Můžete definovat další vlastnosti konfigurace podsítě virtuální sítě hello použití této rutiny, ale který je nad rámec tohoto návodu hello.
>
>

Spusťte následující rutinu toocreate hello konfiguraci virtuální podsítě.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě
V dalším kroku vytvoříme naše virtuální sítě pomocí hello [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) rutiny. V nové skupiny prostředků, hello název, umístění a předpona adresy definované pomocí hello proměnné, které jste dříve inicializovat a použití konfigurace hello podsítě, který jste zadali v předchozím kroku hello vytvoříme naše virtuální sítě.

Spusťte následující rutiny toocreate hello vaší virtuální sítě.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-hello-public-ip-address"></a>Vytvoření veřejné IP adresy hello
Teď, když máme naše virtuální sítě definované, potřebujeme tooconfigure IP adresu pro připojení k toohello virtuální počítač. V tomto kurzu vytvoříme veřejnou IP adresu pomocí dynamických IP adresování toosupport připojení k Internetu. Budeme používat hello [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) rutiny toocreate hello veřejnou IP adresu v hello prostředků skupina vytvořená prevously a hello název, umístění, metoda přidělení a popisek názvu domény DNS je definovat pomocí hello proměnné, které jste dříve inicializován.

> [!NOTE]
> Můžete definovat další vlastnosti hello veřejné IP adresy pomocí této rutiny, ale který je mimo obor hello počáteční kurzu. Můžete také vytvořit privátní adresu nebo adresu se statickou adresou, ale který je také nad rámec tohoto návodu hello.
>
>

Spusťte následující rutinu toocreate hello veřejnou IP adresu.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-hello-network-interface"></a>Vytvoření hello síťové rozhraní
Snažíme se teď připravena toocreate hello síťové rozhraní, které budou používat naše virtuálního počítače. Budeme používat hello [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) rutiny toocreate naše rozhraní sítě ve skupině prostředků hello dříve vytvořili a hello název, umístění, podsíť a veřejnou IP adresu předtím definovaný.

Spusťte následující rutinu toocreate hello síťové rozhraní.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Konfigurace objektu virtuálního počítače
Teď, když máme úložiště a síťové prostředky, které jsou definované, jsme připravené toodefine výpočetní prostředky pro virtuální počítač hello. V našem kurzu jsme se zadejte hello velikost virtuálního počítače a různé vlastnosti operačního systému, zadejte hello síťové rozhraní, které jsme dříve vytvořili, definovat úložiště objektů blob a pak zadejte hello disk operačního systému.

### <a name="create-hello-vm-object"></a>Vytvořit objekt virtuálního počítače hello
Spustíme zadáním hello velikost virtuálního počítače. V tomto kurzu zadáváme DS13. Budeme používat hello [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) rutiny toocreate objekt konfigurovat virtuální počítač s hello názvem a velikostí definovat pomocí hello proměnné, které jste dříve inicializován.

Spusťte následující rutiny toocreate hello virtuálního počítače objekt hello.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-toohold-hello-name-and-password-for-hello-local-administrator-credentials"></a>Vytvořte přihlašovací údaje objektu toohold hello jméno a heslo pro přihlašovací údaje místního správce hello
Než budeme moct nastavit hello vlastnosti operačního systému pro virtuální počítač hello, potřebujeme toosupply hello pověření pro účet místního správce hello jako zabezpečený řetězec. tooaccomplish, budeme používat hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) rutiny.

Spuštění hello následující rutiny a v okně požadavku pověření hello prostředí Windows PowerShell, zadejte hello toouse jméno a heslo pro účet místního správce hello ve virtuálním počítači Windows hello.

    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."

### <a name="set-hello-operating-system-properties-for-hello-virtual-machine"></a>Nastavit vlastnosti hello operačního systému pro virtuální počítač hello
Nyní jsme vlastnosti operačního systému připravené tooset hello virtuálního počítače. Budeme používat hello [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) vyžadují rutiny tooset hello typ operačního systému jako Windows hello [agenta virtuálního počítače](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toobe nainstalovaná, zadejte tuto rutinu hello umožňuje automaticky Aktualizovat a nastavit hello název virtuálního počítače, název počítače hello a hello pověření pomocí hello proměnné, které jste dříve inicializován.

Spusťte hello následující rutiny tooset hello vlastnosti operačního systému pro virtuální počítač.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-hello-network-interface-toohello-virtual-machine"></a>Přidat hello síťové rozhraní toohello virtuální počítač
Potom přidáme hello síťové rozhraní, že jsme vytvořili dříve toohello virtuálního počítače. Budeme používat hello [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) rutiny tooadd hello síťových rozhraní pomocí hello síťové rozhraní proměnnou, kterou jste definovali dříve.

Spusťte hello následující rutiny tooset hello síťové rozhraní pro virtuální počítač.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-hello-blob-storage-location-for-hello-disk-toobe-used-by-hello-virtual-machine"></a>Nastavte umístění úložiště objektů blob hello toobe disku hello používá hello virtuálním počítačem
V dalším kroku se nastaví hello umístění úložiště objektů blob pro toobe disku hello používá hello virtuálního počítače pomocí hello proměnné, které jste definovali dříve.

Spusťte hello následující umístění úložiště objektů blob hello tooset rutiny.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-hello-operating-system-disk-properties-for-hello-virtual-machine"></a>Nastavit vlastnosti disku pro virtuální počítač hello hello operačního systému
V dalším kroku jsme nastaví hello operačního systému disku vlastnosti hello virtuálního počítače. Použijeme hello [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) toospecify rutiny, která hello operačního systému pro virtuální počítač hello bude pocházet z bitové kopie, tooset ukládání do mezipaměti pouze tooread (protože je během instalace systému SQL Server na hello stejný disk) a definovat název virtuálního počítače Hello a disk operačního systému hello definovat pomocí hello proměnné, které jsme definovali dříve.

Spusťte hello následující vlastnosti disku rutiny tooset hello operačního systému pro virtuální počítač.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-hello-platform-image-for-hello-virtual-machine"></a>Zadejte hello image platformy pro virtuální počítač hello
Naše poslední krok konfigurace je image platformy hello toospecify pro naše virtuální počítač. V našem kurzu používáme hello nejnovější bitovou kopii systému SQL Server 2016 CTP. Budeme používat hello [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) rutiny toouse tuto bitovou kopii podle definice hello proměnné, které jste definovali dříve.

Spusťte hello následující rutiny toospecify hello image platformy pro virtuální počítač.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-hello-sql-vm"></a>Vytvořit virtuální počítač SQL hello
Teď, když dokončíte kroky konfigurace hello, jste připravené toocreate hello virtuálního počítače. Budeme používat hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) rutiny toocreate hello virtuálního počítače pomocí hello proměnné, které jsme definovali.

Spusťte následující rutiny toocreate hello virtuálního počítače.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Při vytvoření Hello virtuálního počítače. Všimněte si, že standardní účet úložiště je pro Diagnostika spouštění vytvořit, protože hello zadaný účet úložiště pro disk hello virtuálního počítače je prémiový účet úložiště.

Tento počítač nyní můžete zobrazit v portálu Azure toosee hello [jeho veřejnou IP adresu a jeho plně kvalifikovaný název domény](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="example-script"></a>Ukázkový skript
Hello následující skript obsahuje hello dokončení skriptu prostředí PowerShell pro účely tohoto kurzu. Se předpokládá, že máte již instalace hello předplatného Azure toouse s hello **Add-AzureRmAccount** a **Select-AzureRmSubscription** příkazy.

    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create hello VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Další kroky
Po vytvoření virtuálního počítače hello jste připravené tooconnect toohello virtuálního počítače pomocí připojení RDP a instalační program. Další informace najdete v tématu [připojit tooa virtuálního počítače systému SQL Server na platformě Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).

