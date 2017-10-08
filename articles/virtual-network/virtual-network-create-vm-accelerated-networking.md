---
title: "aaaCreate virtuálního počítače Azure pomocí Accelerated sítě | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální počítač s Accelerated sítě."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 241362891bfe083ab32c2f558be54f63f87c0693
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a>Vytvoření virtuálního počítače pomocí Accelerated sítě

V tomto kurzu zjistíte, jak toocreate virtuálního počítače (virtuální počítač Azure) pomocí Accelerated sítě. Zrychlený sítě je GA pro systém Windows a ve veřejné verzi Preview pro konkrétní distribuce systému Linux. Zrychlený sítě umožňuje jeden kořenový vstupně-výstupních operací virtualizace (SR-IOV) tooa virtuálního počítače, výrazně zlepšit sítě. Tato cesta vysoce výkonné obchází hello hostitele z hello datapath snižuje latence, zpoždění a využití procesoru pro použití s hello nejnáročnější zatížení sítě na podporované typy virtuálních počítačů. Následující obrázek ukazuje komunikaci mezi dvěma virtuálními počítači (VM) s i bez Zrychlený sítě Hello:

![Porovnání](./media/virtual-network-create-vm-accelerated-networking/image1.png)

Bez Zrychlený sítě, musí všechny síťové přenosy a deaktivovat hello virtuálních počítačů procházení hello hostitele a virtuálního přepínače hello. Hello virtuální přepínač poskytuje všechny vynucení zásad, například jako skupin zabezpečení sítě přístup seznamy řízení, izolace a jiné síťové virtualizované služby toonetwork provoz. Další informace o virtuální přepínače, přečtěte si hello toolearn [virtualizace sítě Hyper-V a virtuálního přepínače](https://technet.microsoft.com/library/jj945275.aspx) článku.

Zrychlený sítě, síťové přenosy dorazí na rozhraní sítě hello Virtuálního počítače (NIC) a předá toohello virtuálních počítačů. Všechny zásady sítě, které hello virtuální přepínač platí bez Zrychlený sítě se sníženou zátěží a použít v hardwaru. Použít zásady v hardwaru umožňuje hello seskupování tooforward síťový provoz přímo toohello virtuálních počítačů, obcházení hello hostitele a virtuálního přepínače hello, při zachování všech hello zásady, které se použijí v hello hostitele.

Hello výhod Zrychlený sítě se projeví pouze toohello virtuální počítač, který je zapnutá. Nejlepších výsledků dosáhnete hello, je ideální tooenable tuto funkci na alespoň dva virtuální počítače připojené toohello stejné virtuální síti Azure (VNet). Při komunikaci mezi virtuálními sítěmi nebo připojování místní, tato funkce má minimální dopad toooverall latence.

> [!WARNING]
> Tento Linux verzi Public Preview nemusí mít stejnou úroveň dostupnost a spolehlivost, stejně jako funkce, které jsou hello obecné dostupnosti k verzi. Hello funkce nepodporuje, může mít omezené možnosti a nemusí být k dispozici ve všech Azure umístění. Hello nejaktuálnější upozornění na stav této funkce a dostupnost zkontrolujte hello Azure Virtual Network aktualizací stránky.

## <a name="benefits"></a>Výhody
* **Nižší latenci vyšší pakety za sekundu (pps):** odebrání hello virtuální přepínač z hello datapath odebere hello doby, po paketů v hello hostitele pro zpracování zásad a zvyšuje hello počet paketů, které lze zpracovat uvnitř hello virtuálních počítačů.
* **Snižuje zpoždění:** virtuální přepínač zpracování závisí na množství hello zásady, které musí použít toobe a hello zatížení procesoru Dobrý den, které provádí zpracování hello. Snižování zátěže hardwaru toohello vynucení zásad hello odstraní tento variabilita tím, že doručování paketů přímo přepne toohello virtuálních počítačů, odebere hello hostitele tooVM komunikace a všechna přerušení softwaru a kontext.
* **Snížení využití procesoru:** Bypassing hello virtuální přepínač na hostiteli hello vede využití procesoru tooless ke zpracování síťového provozu.

## <a name="Limitations"></a>Omezení
Při používání této funkce existují Hello následující omezení:

* **Vytvoření rozhraní sítě:** Accelerated sítě lze povolit pouze pro nový síťový adaptér. Nelze nastavit pro existující síťovou.
* **Vytvoření virtuálního počítače:** A síťovým Adaptérem s Zrychlený sítě povolené může obsahovat jenom být připojené tooa virtuálních počítačů při hello virtuální počítač je vytvořen. Hello síťový adaptér nesmí být připojené tooan existující virtuální počítač.
* **Oblasti:** virtuálních počítačů Windows s Zrychlený sítě jsou nabízena v oblastech nejvíce Azure. Virtuální počítače Linux s Zrychlený sítě jsou nabízena v několika oblastech. Hello oblastí, které tato funkce je dostupná v zvětšuje. Naleznete v blogu aktualizace virtuální sítě na Azure hello níže hello nejnovější informace.   
* **Podporované operační systémy:** Windows: Microsoft Windows Server 2012 R2 Datacenter a Windows Server 2016. Linux: Ubuntu Server 16.04 LTS s 4.4.0-77 jádra nebo vyšší, SLES 12 SP2, RHEL 7.3 a CentOS 7.3 (publikováno "Podvodný Wave software").
* **Velikost virtuálního počítače:** velikost optimalizované výpočetní instance s osmi nebo více jader a obecné účely. Další informace najdete v tématu hello [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů. Hello sadu podporované velikosti instance virtuálních počítačů bude rozšiřovat v budoucnu hello.
* **Nasazení pomocí Azure Resource Manager (ARM) pouze:** Accelerated síťové služby není k dispozici pro nasazení prostřednictvím ASM/RDFE.

Omezení toothese změny jsou oznámeno prostřednictvím hello [virtuální sítí Azure aktualizace](https://azure.microsoft.com/updates/accelerated-networking-in-preview) stránky.

## <a name="create-a-windows-vm"></a>Vytvoření virtuálního počítače s Windows
Můžete použít hello portál Azure nebo Azure [prostředí PowerShell](#windows-powershell) toocreate hello virtuálních počítačů.

### <a name="windows-portal"></a>Portál

1. Z internetového prohlížeče, otevřete hello Azure [portál](https://portal.azure.com) a přihlaste se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Hello portálu, klikněte na tlačítko **+ nový** > **výpočetní** > **Windows Server 2016 Datacenter**. 
3. V hello **Windows Server 2016 Datacenter** okno, které se zobrazí, ponechejte *Resource Manager* vybrané pod **vybrat model nasazení**a klikněte na tlačítko **vytvořit** .
4. V hello **Základy** okno, které se zobrazí, zadejte hello následující hodnoty, ponechejte zbývající výchozí možnosti hello nebo vyberte nebo zadejte vlastní hodnoty a klikněte na tlačítko hello **OK** tlačítko:

    |Nastavení|Hodnota|
    |---|---|
    |Name (Název)|Můjvp|
    |Skupina prostředků|Nechte **vytvořit nový** vybrané a zadejte *MyResourceGroup*|
    |Umístění|Západní USA 2|

    Pokud jste nový tooAzure, další informace o [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [odběry](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), a [umístění](https://azure.microsoft.com/regions) (která se také označují tooas oblasti).
5. V hello **zvolte velikost** okno, které se zobrazí, zadejte *8* v hello **minimální počet jader** pole a pak klikněte na **zobrazit všechny**.
6. Klikněte na tlačítko **DS4_V2 standardní**, nebo podporují se všechny virtuální počítač, pak klikněte na tlačítko hello **vyberte** tlačítko.
7. V hello **nastavení** okno, které se zobrazí, ponechejte všechna nastavení jako-je kromě klikněte na tlačítko **povoleno** v části **Accelerated sítě**, pak klikněte na tlačítko hello **OK** tlačítko. **Poznámka:** , pokud v předchozích krocích jste vybrali hodnoty pro velikost virtuálního počítače, operační systém nebo umístění, které nejsou uvedené v hello [omezení](#Limitations) tohoto článku **Accelerated sítě**nejsou viditelná.
8. V hello **Souhrn** okno, které se zobrazí, klikněte na tlačítko hello **OK** tlačítko. Azure začne vytvářet hello virtuálních počítačů. Vytvoření virtuálního počítače trvá několik minut.
9. tooinstall hello accelerated síťové ovladače pro Windows, dokončení hello kroky hello [konfigurace systému Windows](#configure-windows) tohoto článku.

### <a name="windows-powershell"></a>Prostředí PowerShell
1. Nainstalujte nejnovější verzi prostředí Azure PowerShell hello hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu. Pokud jste nový tooAzure prostředí PowerShell, přečtěte si hello [Přehled prostředí Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.
2. Spusťte relaci prostředí PowerShell kliknutím na tlačítko Spustit Windows hello zadáním **prostředí powershell**, pak levým na **prostředí PowerShell** z výsledků hledání hello.
3. V okně prostředí PowerShell, zadejte hello `login-azurermaccount` příkaz toosign se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
4. V prohlížeči zkopírujte následující skript hello:
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. V okně prostředí PowerShell klikněte pravým tlačítkem na toopaste hello skript a spusťte ho spustíte. Zobrazí se výzva k zadání uživatelského jména a hesla. Při připojování tooit v dalším kroku hello používejte tyto přihlašovací údaje toolog v toohello virtuálních počítačů. Pokud skript hello selže a změnit hodnoty ve skriptu hello před jeho provedením, potvrďte hello hodnoty, které jste použili pro velikost virtuálního počítače, operační systém, a umístění jsou uvedeny v hello [omezení](#Limitations) tohoto článku.
6. tooinstall hello accelerated síťové ovladače pro Windows, dokončení hello kroky hello [konfigurace systému Windows](#configure-windows) tohoto článku.

### <a name="configure-windows"></a>Konfigurace systému Windows
Jakmile vytvoříte hello virtuálních počítačů v Azure, je nutné nainstalovat hello Zrychlený síťové ovladače pro Windows. Před dokončením hello následující kroky, kterou jste vytvořili virtuální počítač s Windows v Zrychlený sítě pomocí buď hello [portál](#windows-portal) nebo [prostředí PowerShell](#windows-powershell) kroky v tomto článku. 

1. Z internetového prohlížeče, otevřete hello Azure [portál](https://portal.azure.com) a přihlaste se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *Můjvp*. Když **Můjvp** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **Můjvp** okno, které se zobrazí, klikněte na tlačítko hello **Connect** tlačítka na hello levém horním rohu okna hello. **Poznámka:** Pokud **vytváření** je viditelný v hello **Connect** tlačítko Azure ještě nebyla dokončena vytváření hello virtuálních počítačů. Klikněte na tlačítko **připojit** až poté, co se již nezobrazují **vytváření** pod hello **Connect** tlačítko.
4. Povolit váš prohlížeč toodownload hello **MyVm.rdp** souboru.  Po stažení, klikněte na soubor tooopen hello ho. 
5. Klikněte na tlačítko hello **připojit** tlačítka na hello **připojení ke vzdálené ploše** pole, který se zobrazí upozornění, že hello nelze identifikovat vydavatele hello vzdáleného připojení.
6. V hello **zabezpečení systému Windows** pole, které se zobrazí, klikněte na tlačítko **další možnosti**, pak klikněte na tlačítko **použít jiný účet**. Zadejte hello uživatelské jméno a heslo, které jste zadali v kroku 4, a pak klikněte na tlačítko hello **OK** tlačítko.
7. Klikněte na tlačítko hello **Ano** tlačítko pole hello připojení ke vzdálené ploše, který se zobrazí upozornění, že nelze ověřit identitu hello hello vzdáleného počítače.
8. Klikněte pravým tlačítkem na tlačítko Start systému Windows hello a klikněte na tlačítko **Správce zařízení**. Rozbalte hello **síťové adaptéry** uzlu. Potvrďte, že hello **Mellanox ConnectX 3 virtuální adaptér Ethernet funkce** se zobrazí, jak je znázorněno v následujícím obrázku hello:
   
    ![Správce zařízení](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. Pro virtuální počítač je nyní k dispozici Zrychlený sítě.

## <a name="create-a-linux-vm"></a>Vytvoření virtuálního počítače s Linuxem
Můžete použít hello portál Azure nebo Azure [prostředí PowerShell](#linux-powershell) toocreate Ubuntu nebo SLES virtuálních počítačů. RHEL a CentOS virtuální počítače se jiný pracovní postup.  Podrobnosti viz níže uvedené pokyny hello.

### <a name="linux-portal"></a>Portál
1. Registrace pro hello accelerated sítě pro Linux preview provedením kroků 1 až 5 hello [vytvoření virtuálního počítače s Linuxem - PowerShell](#linux-powershell) tohoto článku.  Nelze zaregistrovat hello preview hello portálu.
2. Dokončení kroků 1 – 8 v hello [vytvoření virtuálního počítače s Windows – portál](#windows-portal) tohoto článku. V kroku 2, klikněte na tlačítko **Ubuntu Server 16.04 LTS** místo **Windows Server 2016 Datacenter**. V tomto kurzu zvolte toouse heslo, nikoli klíč SSH, i když pro nasazení v produkčním prostředí, můžete použít buď. Pokud **Accelerated sítě** po dokončení kroku 7 hello není k dispozici [vytvoření virtuálního počítače s Windows – portál](#windows-portal) části tohoto článku je pravděpodobně pro jednu z následujících důvodů hello:
    - Nejsou ještě registrované pro hello preview. Potvrďte, že je váš stav registrace **registrovaná**, jak je popsáno v kroku 4 hello [vytvoření virtuálního počítače s Linuxem - Powershell](#linux-powershell) tohoto článku. **Poznámka:** Pokud jste se účastnili hello Accelerated sítě pro virtuální počítače Windows preview, (jeho žádné delší toouse nezbytné tooregister Accelerated sítě pro virtuální počítače Windows), které nejsou registrované automaticky pro hello Accelerated sítě pro Virtuální počítače s Linuxem preview. Je třeba zaregistrovat pro hello Accelerated sítě pro virtuální počítače s Linuxem náhled tooparticipate v ní.
    - Nevybrali jste velikost virtuálního počítače, operační systém nebo umístění, které jsou uvedené v hello [omezení](#limitations) tohoto článku.
3. tooinstall hello accelerated síťové ovladače pro Linux, dokončení hello kroky hello [konfigurace Linux](#configure-linux) tohoto článku.

### <a name="linux-powershell"></a>Prostředí PowerShell

>[!WARNING]
>Pokud chcete vytvořit virtuální počítače s Linuxem se Zrychlený sítěmi v odběru a pak pokus toocreate virtuální počítač s Windows se Zrychlený sítěmi v hello stejného předplatného, vytvoření virtuálního počítače Windows hello může selhat. Během této verzi preview se doporučuje otestovat Linux a virtuálních počítačů Windows s Zrychlený sítě v samostatných předplatných.
>

1. Nainstalujte nejnovější verzi prostředí Azure PowerShell hello hello [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modulu. Pokud jste nový tooAzure prostředí PowerShell, přečtěte si hello [Přehled prostředí Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.
2. Spusťte relaci prostředí PowerShell kliknutím na tlačítko Spustit Windows hello zadáním **prostředí powershell**, pak levým na **prostředí PowerShell** z výsledků hledání hello.
3. V okně prostředí PowerShell, zadejte hello `login-azurermaccount` příkaz toosign se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Registrace pro hello accelerated sítě pro Azure preview provedením hello následující kroky:
    - Odeslat e-mail příliš[ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) s ID předplatného Azure a zamýšlené použití. Počkejte na potvrzení e-mailu společnosti Microsoft o vaše předplatné povolený.
    - Zadejte následující příkaz tooconfirm, které jsou registrovány pro verzi preview hello hello:
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        Pokračujte krokem 5 až **registrovaná** se zobrazí v hello výstup po zadání hello předchozí příkaz. Hello následující výstup před pokračováním musí vypadat výstupu:
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      >Pokud jste se účastnili hello Accelerated sítě pro virtuální počítače Windows preview, (jeho žádné delší toouse nezbytné tooregister Accelerated sítě pro virtuální počítače Windows), které nejsou registrované automaticky pro hello Accelerated sítě pro virtuální počítače s Linuxem preview. Je třeba zaregistrovat pro hello Accelerated sítě pro virtuální počítače s Linuxem náhled tooparticipate v ní.
      >
5. V prohlížeči zkopírujte následující skript podle potřeby nahraďte Ubuntu nebo SLES hello.  Znovu Redhat a CentOS mít jiný pracovní postup uvedené níže:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define hello new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. V okně prostředí PowerShell klikněte pravým tlačítkem na toopaste hello skript a spusťte ho spustíte. Zobrazí se výzva k zadání uživatelského jména a hesla. Při připojování tooit v dalším kroku hello používejte tyto přihlašovací údaje toolog v toohello virtuálních počítačů. Pokud se skript hello nezdaří, zkontrolujte, že:
    - Jste zaregistrováni pro hello preview, jak je popsáno v kroku 4
    - Pokud jste změnili velikost, typ operačního systému nebo hodnoty umístění ve skriptu hello virtuálního počítače před jeho provedením, zda text hello hodnoty jsou uvedeny ve hello [omezení](#Limitations) tohoto článku.
7. tooinstall hello accelerated síťové ovladače pro Linux, dokončení hello kroky hello [konfigurace Linux](#configure-linux) tohoto článku.

### <a name="configure-linux"></a>Konfigurace systému Linux

Jakmile vytvoříte hello virtuálních počítačů v Azure, je nutné nainstalovat hello Zrychlený síťové ovladače pro Linux. Před dokončením hello následující kroky, kterou jste vytvořili virtuální počítač s Linuxem v Zrychlený sítě pomocí buď hello [portál](#linux-portal) nebo [prostředí PowerShell](#linux-powershell) kroky v tomto článku. 

1. Z internetového prohlížeče, otevřete hello Azure [portál](https://portal.azure.com) a přihlaste se pomocí vaší Azure [účet](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Hello horní části hello portálu toohello napravo od hello *vyhledávání prostředků* panel, klikněte na tlačítko hello **> _** ikonu toostart cloudové prostředí Bash (Preview). Hello Bash cloudové prostředí podokně se zobrazí v dolní části hello hello portálu a za několik sekund, uvede  **username@Azure:~ $** řádku. I když můžete SSH toohello virtuálního počítače z vašeho počítače, nikoli hello cloudové prostředí, hello pokyny v tomto kurzu se předpokládá, že používáte hello cloudové prostředí.
3. V horní části hello hello portálu, hello pole obsahující hello text *vyhledávání prostředků*, typ *Můjvp*. Když **Můjvp** se zobrazí ve výsledcích hledání hello, klikněte na něj.
4. V hello **Můjvp** okno, které se zobrazí, klikněte na tlačítko hello **Connect** tlačítka na hello levém horním rohu okna hello. **Poznámka:** Pokud **vytváření** je viditelný v hello **Connect** tlačítko Azure ještě nebyla dokončena vytváření hello virtuálních počítačů. Klikněte na tlačítko **připojit** až poté, co se již nezobrazují **vytváření** pod hello **Connect** tlačítko.
5. Azure otevře pole informující o tooenter hello `ssh adminuser@<ipaddress>`. Zadejte tento příkaz hello cloudové prostředí (nebo kopii z hello pole, která objevil v kroku 4 a vložte ji do prostředí cloudu toohello), stiskněte klávesu Enter.
6. Zadejte **Ano** toohello otázku s dotazem, můžete podle potřeby toocontinue připojení, stiskněte klávesu Enter.
7. Zadejte heslo hello, kterou jste zadali při vytváření hello virtuálních počítačů. Po úspěšném přihlášení toohello virtuálních počítačů, se zobrazí adminuser@MyVm:~ $ řádku. Nyní jste přihlášeni toohello virtuálních počítačů prostřednictvím relace prostředí cloudu hello. **Poznámka:** cloudové prostředí relace vyprší po 10 minutách nečinnosti.

Hello pokyny v tomto okamžiku záviset na hello distribuce, který používáte. 

#### <a name="ubuntusles"></a>Ubuntu/SLES

1. Zadejte na příkazovém řádku hello `uname -r` a potvrďte hello verze pro:

    * Ubuntu je "4.4.0-77-generic", nebo vyšší
    * SLES je "4.4.59-92.20-default" nebo vyšší

2. Vytvoření vazby mezi hello standardní síťového adaptéru vNIC a síťového adaptéru vNIC hello accelerated spuštěné hello příkazy, které následují. Provoz sítě používá hello vyšší provádění Zrychlený síťového adaptéru vNIC a při hello dokumentových zajistí, že síťové přenosy proces se nepřerušil napříč změny konfigurace.
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. Po spouštění skriptu hello hello virtuální počítač restartuje po pozastavení 60 sekund.
4. Jednou hello restartování virtuálního počítače znovu tooit podle kroků 5 až 7 znovu.
5. Spustit hello `ifconfig` příkazů a potvrdit, že bond0 zase a hello rozhraní se zobrazuje jako nahoru. 
 
 >[!NOTE]
      >Aplikace, které používají Zrychlený sítě musí komunikovat přes hello *bond0* rozhraní není *eth0*.  Název rozhraní Hello může změnit než Zrychlený sítě dosáhne obecné dostupnosti.

#### <a name="rhelcentos"></a>RHEL nebo CentOS

Vytváření Red Hat Enterprise Linux nebo CentOS 7.3 virtuálního počítače vyžaduje některé další kroky tooload hello nejnovější ovladače potřebné pro rozhraní SR-IOV a hello ovladač virtuální funkce (VF) pro hello síťové karty. Hello první fázi hello pokyny připraví obrázek, který lze použít toomake jeden nebo více virtuálních počítačů, které se mají ovladače hello předem načtená.

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a>Fáze 1: Příprava Red Hat Enterprise Linux nebo CentOS 7.3 základní image. 

1.  Zřídit jiný - SR-IOV CentOS 7.3 virtuálního počítače v Azure

2.  Instalovat 4.2.2:
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  Stáhnout konfigurační soubory
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  Zrušení zřízení tohoto virtuálního počítače

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  Z portálu Azure Zastavit tento virtuální počítač; a přejděte na tooVM "disky", zaznamenat hello OSDisk URI virtuálního pevného disku. Tento identifikátor URI obsahuje název hello základní image virtuálního pevného disku a jeho účet úložiště. 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a>Fáze 2: zřizovat nové virtuální počítače v Azure

1.  Zřídit nové virtuální počítače na základě pomocí New-AzureRMVMConfig pomocí hello základní image virtuálního pevného disku zachytit v první fázi, pomocí AcceleratedNetworking povolit na kartě vNIC hello:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify hello base image's VHD URI (from phase one step 5). 
    # Note: hello storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need tooreplace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for hello location from which hello new image binary large object (BLOB) is copied toostart hello virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create hello virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  Po spustit virtuální počítače, ověřte hello VF zařízení podle "lspci" a zkontrolujte položku Mellanox hello. Například bychom měli vidět této položky ve výstupu lspci hello:
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  Spusťte skript záruční hello:

    ```bash
    sudo bondvf.sh
    ```

4.  Restartovat hello nové virtuální počítače:

    ```bash
    sudo reboot
    ```

Hello virtuálního počítače by měla začínat znakem bond0 nakonfigurované a hello Accelerated sítě cesty povolené.  Spustit `ifconfig` tooconfirm.
