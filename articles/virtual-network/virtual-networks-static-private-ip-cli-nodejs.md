---
title: "aaaConfigure privátních IP adres pro virtuální počítače – 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6caae79c98c7bc5f246b7bb3bb8bd8f032eb2d8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-10"></a>Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí hello Azure CLI 1.0


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku 

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku: 

- [Azure CLI 1.0](#how-to-specify-a-static-private-ip-address-when-creating-a-vm) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](virtual-networks-static-private-ip-arm-cli.md) -naše generace CLI pro model nasazení správy prostředků hello 

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic hello](virtual-networks-static-private-ip-classic-cli.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořeny. Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-cli.md).

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a>Jak toospecify statickou privátní IP adresy při vytvoření virtuálního počítače
virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků hello:

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.
   
        azure config mode arm
   
    Očekávaný výstup:
   
        info:    New mode is arm
3. Spustit hello **vytvoření veřejné ip síť azure** toocreate veřejnou IP adresu pro hello virtuálních počítačů. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.
   
        azure network public-ip create -g TestRG -n TestPIP -l centralus
   
    Očekávaný výstup:
   
        info:    Executing command network public-ip create
        + Looking up hello public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up hello public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK
   
   * **-g (nebo --resource-group)**. Název hello prostředků skupiny hello veřejnou IP adresu budou vytvořeny v.
   * **-n (nebo --name)**. Název veřejné IP adresy hello.
   * **-l (nebo --location)**. Oblast Azure, kde bude vytvořen hello veřejnou IP adresu. V našem scénáři je to *centralus*.
4. Spustit hello **síťových adaptérů sítě azure vytvořit** příkaz toocreate síťový adaptér se statickou privátní IP. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.
   
        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd
   
    Očekávaný výstup:
   
        + Looking up hello network interface "TestNIC"
        + Looking up hello subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up hello network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
   
   * **-a (nebo--privátní. ip adresa)**. Statickou privátní IP adresou pro hello síťový adaptér.
   * **-m (nebo--název podsítě virtuální sítě)**. Název hello virtuální síť, kde bude vytvořen hello síťový adaptér.
   * **-k (nebo--název podsítě)**. Název podsítě hello, kde bude vytvořen hello síťový adaptér.
5. Spustit hello **vytvoření virtuálního počítače azure** hello toocreate příkaz virtuálního počítače pomocí hello veřejná IP adresa a síťovou kartu vytvořili výše. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.
   
        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd
   
    Očekávaný výstup:
   
        info:    Executing command vm create
        + Looking up hello VM "DNS01"
        info:    Using hello VM Size "Standard_A1"
        warn:    hello image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account vnetstorage
        + Looking up hello NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in hello NIC "TestNIC"
        info:    Found public ip parameters, trying toosetup PublicIP profile
        + Looking up hello public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up hello NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK
   
   * **-y (nebo typ operačního systému –)**. Typ operačního systému pro virtuální počítač, hello buď *Windows* nebo *Linux*.
   * **-f (nebo--seskupování-name)**. Použije se název hello hello síťový adaptér virtuálního počítače.
   * **-i (nebo--název veřejné IP adresy)**. Název veřejné IP hello hello virtuální počítač bude používat.
   * **-F (nebo--vnet-name)**. Název hello virtuální síť, kde bude vytvořen hello virtuálních počítačů.
   * **-j (nebo--vnet název podsítě)**. Název podsítě hello, kde bude vytvořen hello virtuálních počítačů.

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Jak tooretrieve statickou privátní IP adresu pro virtuální počítač
tooview hello statickou privátní IP adresu pro hello virtuální počítač vytvořen pomocí skriptu hello výše, spusťte následující příkaz rozhraní příkazového řádku Azure hello a sledovat hello hodnoty pro *alokační metody privátní IP* a *privátní IP adresa*:

    azure vm show -g TestRG -n DNS01

Očekávaný výstup:

    info:    Executing command vm show
    + Looking up hello VM "DNS01"
    + Looking up hello NIC "TestNIC"
    + Looking up hello public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Jak tooremove statickou privátní IP adresu z virtuálního počítače
Statickou privátní IP adresu nelze odebrat z síťový adaptér v Azure CLI pro Resource Manager. Musíte vytvořit novou síťovou kartu, která používá dynamický IP, odeberte hello předchozí síťový adaptér z hello virtuálních počítačů, a poté přidejte hello nové toohello síťový adaptér virtuálního počítače. toochange hello síťovou kartu pro hello virtuálních počítačů použít int eh příkazů výše, postupujte podle následujících kroků hello.

1. Spustit hello **síťových adaptérů sítě azure vytvořit** příkaz toocreate nový síťový adaptér pomocí dynamické přidělování IP adres. Všimněte si, jak není nutné toospecify hello IP adresu této doby.
   
        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd
   
    Očekávaný výstup:
   
        info:    Executing command network nic create
        + Looking up hello network interface "TestNIC2"
        + Looking up hello subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up hello network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
2. Spustit hello **sada virtuálních počítačů azure** příkaz toochange hello síťový adaptér používá hello virtuálních počítačů.
   
        azure vm set -g TestRG -n DNS01 -N TestNIC2
   
    Očekávaný výstup:
   
        info:    Executing command vm set
        + Looking up hello VM "DNS01"
        + Looking up hello NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK
3. Pokud je potřeba, spusťte hello **odstranit síťových adaptérů sítě azure** příkaz toodelete hello staré síťový adaptér.
   
        azure network nic delete -g TestRG -n TestNIC --quiet
   
    Očekávaný výstup:
   
        info:    Executing command network nic delete
        + Looking up hello network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a>Jak tooadd statickou privátní IP adresa tooan existující virtuální počítač
tooadd statickou privátní IP adresu toohello síťový adaptér používá virtuální počítač vytvořený skript hello výše, spusťte následující příkaz hello:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Očekávaný výstup:

    info:    Executing command network nic set
    + Looking up hello network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up hello network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Další kroky
* Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.
* Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.
* Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).

