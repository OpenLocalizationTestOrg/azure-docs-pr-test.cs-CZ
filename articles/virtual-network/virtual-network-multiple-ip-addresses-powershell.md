---
title: "aaaMultiple IP adresy pro virtuální počítače Azure – prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak tooassign více IP adres tooa virtuálního počítače pomocí prostředí PowerShell | Správce prostředků."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/24/2017
ms.author: jdial;annahar
ms.openlocfilehash: df54c4386ce13521e660a3e7208c8c1ab1459bc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-powershell"></a>Přiřadit více IP adres toovirtual počítače pomocí prostředí PowerShell

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Tento článek vysvětluje, jak toocreate virtuální počítač (VM) prostřednictvím nasazení Azure Resource Manager hello modelu pomocí prostředí PowerShell. Tooresources vytvořené pomocí modelu nasazení classic hello nelze přiřadit více IP adres. Další informace o modelech nasazení Azure, přečtěte si hello toolearn [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Vytvoření virtuálního počítače s více IP adres

Hello kroky, které následují vysvětlují, jak toocreate příklad virtuálního počítače s více IP adres, jak je popsáno v hello scénáři. Změňte hodnoty proměnných podle potřeby týkající se vaší implementace.

1. Otevřete příkazový řádek prostředí PowerShell a dokončení hello zbývající kroky v této části v rámci jedné relace prostředí PowerShell. Pokud ještě nemáte prostředí PowerShell nainstalovaný a nakonfigurovaný, dokončení hello kroky v hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.
2. Účet pro přihlášení tooyour s hello `login-azurermaccount` příkaz.
3. Nahraďte *myResourceGroup* a *westus* s názvem a umístění dle vlastního výběru. Vytvořte skupinu prostředků. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

    ```powershell
    $RgName   = "MyResourceGroup"
    $Location = "westus"

    New-AzureRmResourceGroup `
    -Name $RgName `
    -Location $Location
    ```

4. Vytvořte virtuální síť (VNet) a podsítě v hello stejné umístění jako skupina prostředků hello:

    ```powershell
    
    # Create a subnet configuration
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name MySubnet `
    -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $VNet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyVNet `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $subnetConfig

    # Get hello subnet object
    $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
    ```

5. Vytvořte skupinu zabezpečení sítě (NSG) a pravidla. Hello NSG zabezpečuje hello virtuálních počítačů pomocí příchozí a odchozí pravidla. V tomto případě se vytvoří příchozí pravidlo pro port 3389, které umožní příchozí připojení ke vzdálené ploše.

    ```powershell
    
    # Create an inbound network security group rule for port 3389

    $NSGRule = New-AzureRmNetworkSecurityRuleConfig `
    -Name MyNsgRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow
    
    # Create a network security group
    $NSG = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyNetworkSecurityGroup `
    -SecurityRules $NSGRule
    ```

6. Definování hello primární konfiguraci IP adresy pro hello síťový adaptér. Změna 10.0.0.4 tooa platná adresa v podsíti hello jste vytvořili, pokud jste nepoužili dříve definovanou hodnotu hello. Před přiřazením statické IP adresy, doporučujeme nejdřív ověřit, zda že se již používá. Zadejte příkaz hello `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`. Pokud není k dispozici hello adresa, výstup hello vrátí *True*. Pokud není k dispozici, výstup hello vrátí *False* a seznam adres, které jsou k dispozici. 

    V následující příkazy, hello **< nahradit s vaše jedinečné name > nahraďte hello toouse název jedinečný DNS.** Název Hello musí být jedinečný mezi všechny veřejné IP adresy v rámci oblasti Azure. Toto je volitelný parametr. Může být odebrán, pokud chcete pouze tooconnect toohello virtuálních počítačů pomocí hello veřejnou IP adresu.

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign hello public IP ddress tooit
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    Přiřadíte-li více tooa konfigurace IP síťovou kartu, musí být přiřazena jednu konfiguraci jako hello *-primární*.

    > [!NOTE]
    > Veřejné IP adresy mají nominální poplatek. více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném. Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.

7. Definovat hello sekundární konfigurace IP pro hello síťový adaptér. Můžete přidávat nebo odebírat konfigurace podle potřeby. Každá konfigurace IP musí mít přiřazené privátní IP adresy. Každá konfigurace můžete volitelně může mít jednu veřejnou IP adresu přiřadit.

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign hello public IP ddress tooit
    $IpConfigName2 = "IPConfig-2"
    $IpConfig2     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName2 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.5 `
    -PublicIpAddress $PublicIP2
        
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IPConfigName3 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.6
    ```

8. Vytvořte hello síťový adaptér a přidružte hello tři IP konfigurace tooit:

    ```powershell
    
    $NIC = New-AzureRmNetworkInterface `
    -Name MyNIC `
    -ResourceGroupName $RgName `
    -Location $Location `
    -NetworkSecurityGroupId $NSG.Id `
    -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```

    >[!NOTE]
    >I když všechny konfigurace přiřazené tooone síťový adaptér v tomto článku, můžete přiřadit více IP konfigurace tooevery síťovou kartu připojenou toohello virtuálních počítačů. jak toocreate virtuálního počítače s více síťovými kartami, číst toolearn hello [vytvoření virtuálního počítače s více síťovými kartami](virtual-network-deploy-multinic-arm-ps.md) článku.

9. Vytvořte hello virtuálních počítačů zadáním hello následující příkazy:

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted tooenter a sername and password for hello VM you're reating.
    $cred = Get-Credential
    
    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
    -VMName MyVM `
    -VMSize Standard_DS1_v2 | `
    Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName MyVM `
    -Credential $cred | `
    Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
    Add-AzureRmVMNetworkInterface `
    -Id $NIC.Id
    
    # Create hello VM
    New-AzureRmVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. Přidat hello privátní IP adresy toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku. Nepřidávejte hello veřejné IP adresy toohello operačního systému.

## <a name="add"></a>Přidat tooa IP adresy virtuálních počítačů

Privátní a veřejné IP adresy tooa síťovou kartu můžete přidat pomocí hello kroků, které následují. Hello příklady v následující části hello předpokládají, že už máte virtuální počítač s konfigurací protokolu IP hello tři popsané v hello [scénář](#Scenario) v tomto článku, ale není to nutné, abyste provedli.

1. Otevřete příkazový řádek prostředí PowerShell a dokončení hello zbývající kroky v této části v rámci jedné relace prostředí PowerShell. Pokud ještě nemáte prostředí PowerShell nainstalovaný a nakonfigurovaný, dokončení hello kroky v hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.
2. Změna hello "hodnoty" hello následující název toohello $Variables hello chcete tooadd IP adresu tooand hello prostředků skupiny a umístění hello síťový adaptér existuje v síťovou kartu:

    ```powershell
    $NicName  = "MyNIC"
    $RgName   = "MyResourceGroup"
    $Location = "westus"
    ```

    Pokud neznáte název hello hello chcete toochange síťovou kartu, zadejte následující příkazy hello, pak změňte hodnoty hello předchozích proměnných hello:

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
3. Vytvoření proměnné a nastavte ji toohello stávající síťovou kartu zadáním hello následující příkaz:

    ```powershell
    $MyNIC = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
    ```
4. Hello následující příkazy, změňte *MyVNet* a *MySubnet* toohello názvy virtuálních sítí a podsítí hello hello síťový adaptér je připojen k. Zadejte hello příkazy tooretrieve hello virtuálních sítí a podsítí objekty hello síťový adaptér je připojen k:

    ```powershell
    $MyVNet = Get-AzureRMVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
    $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
    ```
    Pokud si nejste jisti hello virtuální síť nebo podsíť název hello síťový adaptér je připojen k, zadejte následující příkaz hello:
    ```powershell
    $MyNIC.IpConfigurations
    ```
    Ve výstupu hello hledejte text podobné toohello následující příklad výstupu:
    
    ```
    "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
    ```
    V tento výstup *MyVnet* je hello virtuální sítě a *MySubnet* je hello podsíť hello síťový adaptér je připojen k.

5. Proveďte kroky hello v jednom z hello následující části, podle požadavků:

    **Přidejte privátní IP adresy**

    tooadd tooa privátní adresy IP síťový adaptér, je nutné vytvořit konfiguraci IP adres. Hello následující příkaz vytvoří konfiguraci se statickou IP adresu 10.0.0.7. Při zadávání statickou IP adresu, musí být adresu nepoužívané hello podsítě. Doporučuje se nejdřív otestovat tooensure adresu hello je k dispozici zadáním hello `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` příkaz. Pokud není k dispozici hello IP adresa, výstup hello vrátí *True*. Pokud není k dispozici, výstup hello vrátí *False*a seznam adres, které jsou k dispozici.

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
    $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    Vytvořte tolik konfigurace, jak požadujete, pomocí konfigurace jedinečné názvy a privátní IP adresy (pro konfigurace s statické IP adresy).

    Přidat hello privátní IP adresu toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.

    **Přidejte veřejnou IP adresu**

    Veřejná IP adresa se přidá spojením veřejnou IP adresu prostředku tooeither na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres. Kroky hello v jedné z hello částí, které následují, potřebujete.

    > [!NOTE]
    > Veřejné IP adresy mají nominální poplatek. více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném. Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.
    >

    - **Přidružení hello veřejné IP adresy prostředků tooa nová konfigurace IP**
    
        Vždy, když přidáte veřejnou IP adresu v novou konfiguraci protokolu IP, musíte taky přidat privátní IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy. Můžete přidat existující prostředek veřejné IP adresy, nebo vytvořte novou. toocreate novou, zadejte následující příkaz hello:
    
        ```powershell
        $myPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "myPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location `
        -AllocationMethod Static
        ```

        toocreate na novou konfiguraci protokolu IP se statickou privátní IP adresou a hello přidružené *myPublicIp3* veřejných IP adres prostředků, zadejte následující příkaz hello:

        ```powershell
        Add-AzureRmNetworkInterfaceIpConfig `
        -Name IPConfig-4 `
        -NetworkInterface $myNIC `
        -Subnet $Subnet `
        -PrivateIpAddress 10.0.0.7 `
        -PublicIpAddress $myPublicIp3
        ```

    - **Přidružení hello veřejné konfiguraci IP adresy prostředků tooan stávající IP**

        Prostředek veřejné IP adresy lze pouze přidružené tooan konfigurace protokolu IP, který ho přidružené neobsahuje. Můžete určit, zda konfiguraci IP adres má přidružené veřejnou IP adresu zadáním hello následující příkaz:

        ```powershell
        $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
        ```

        Zobrazí výstup podobný toohello následující:

        ```     
        Name       PrivateIpAddress PublicIpAddress                                           Primary
        
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False
        ```

        Od hello **PublicIpAddress** sloupec pro *IpConfig 3* je prázdné, žádný prostředek veřejné IP adresy je aktuálně přidružené tooit. Můžete přidat existující veřejnou IP adresu prostředku tooIpConfig-3, nebo zadejte následující příkaz toocreate jeden hello:

        ```powershell
        $MyPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "MyPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location -AllocationMethod Static
        ```

        Zadejte následující příkaz tooassociate hello veřejných IP adres toohello stávající IP konfigurace prostředků s názvem hello *IpConfig 3*:
    
        ```powershell
        Set-AzureRmNetworkInterfaceIpConfig `
        -Name IpConfig-3 `
        -NetworkInterface $mynic `
        -Subnet $Subnet `
        -PublicIpAddress $myPublicIp3
        ```

6. Nastavte hello síťový adaptér s novou konfigurací IP adresy hello zadáním hello následující příkaz:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $MyNIC
    ```

7. Zobrazení hello privátních IP adres a hello veřejnou IP adresu prostředky přiřazené toohello hello síťový adaptér tak, že zadáte následující příkaz:

    ```powershell   
    $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
8. Přidat hello privátní IP adresu toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku. Nepřidávejte hello veřejnou IP adresu toohello operačního systému.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
