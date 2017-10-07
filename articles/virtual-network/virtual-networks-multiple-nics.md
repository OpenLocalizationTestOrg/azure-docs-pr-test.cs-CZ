---
title: "aaaCreate virtuální počítač (klasický) s více síťovými kartami pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate a konfigurace virtuálních počítačů s více síťovými kartami pomocí prostředí PowerShell."
services: virtual-network, virtual-machines
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a1a3952c-2dcc-4977-bd7a-52d623c1fb07
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 8ef35bd4cfd7e6a527080f1cfc541275ca86f5e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a>Vytvoření virtuálního počítače (klasické) s více síťovými kartami
Můžete vytvořit virtuální počítače (VM) v Azure a připojit více síťových rozhraní (NIC) tooeach vaše virtuálních počítačů. Několik síťových adaptérů jsou nutné pro mnoho síťových virtuálních zařízení, například doručení aplikace a řešení optimalizace sítě WAN. Několik síťových adaptérů také poskytují izolaci provozu mezi síťové adaptéry.

![Síťový adaptér více virtuálních počítačů](./media/virtual-networks-multiple-nics/IC757773.png)

Zobrazí obrázek Hello virtuálního počítače se síťovými adaptéry, tři, každý z nich připojený tooa jiné podsíti.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala Resource Manager.

* Internetový VIP (nasazení classic) je podporován pouze na hello "Výchozí" síťový adaptér. Existuje jenom jedna IP adresa VIP toohello z hello výchozí síťový adaptér.
* V tomto okamžiku nejsou podporovány adresy Instance úroveň veřejné IP (LPIP) (nasazení classic) pro více virtuálních počítačů síťovou kartu.
* Hello pořadí síťových karet hello z uvnitř hello virtuálního počítače budou náhodné a také můžete změnit napříč aktualizace infrastruktury Azure. Ale hello IP adresy a hello odpovídající ethernetová adresa MAC adresy zůstane stejný hello. Předpokládejme například, **Eth1** 10.1.0.100 IP adresu a adresu MAC 00-0D-3A-B0-39-0D; po aktualizaci infrastruktury Azure a restartování, se může změnit příliš**Eth2**, ale hello IP a MAC párování bude zůstat stejné hello. Když je restartování spouštěná zákazníka, hello seskupování pořadí zůstane stejný hello.
* Hello adresa pro každý síťový adaptér na každý virtuální počítač se musí nacházet v podsíti, několik síťových adaptérů v rámci jednoho virtuálního počítače lze každou přiřadit hello adresy, které jsou ve stejné podsíti.
* Hello velikost virtuálního počítače určuje hello počet síťových ADAPTÉRŮ, který můžete vytvořit pro virtuální počítač. Referenční dokumentace hello [systému Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) virtuálních počítačů velikostí články toodetermine kolik síťových karet podporuje každý velikost virtuálního počítače. 

## <a name="network-security-groups-nsgs"></a>Skupiny zabezpečení sítě (Nsg)
V nasazení Resource Manager může být všechny síťové adaptéry na virtuálním počítači související s skupina zabezpečení sítě (NSG), včetně všech síťových adaptérů na virtuálním počítači, který má několik síťových adaptérů povoleno. Pokud síťový adaptér je přiřazena adresa v podsíti, kde je skupina NSG přidružená hello podsíť, pak hello pravidla v podsíti hello NSG, budou platit toothat síťový adaptér. V přidání podsítě tooassociating pomocí skupin Nsg lze přiřadit síťový adaptér s skupinu NSG.

Pokud podsíť je přidružen skupinu NSG a síťovou kartu v této podsíti jednotlivě souvisí s skupinu NSG, pravidla NSG hello spojené se použijí v **toku pořadí** podle toohello směr provozu hello předávány do nebo z Hello síťovou kartu:

* **Příchozí provoz** jehož cílem je hello síťový adaptér v otázku nejprve prochází hello podsíť, aktivuje pravidla NSG hello podsíť, před předávání do hello síťový adaptér a potom aktivuje pravidla NSG hello seskupování.
* **Odchozí přenosy** jejichž zdrojem je hello seskupování dotyčném toků první ze z hello síťového adaptéru, spouštění pravidla NSG hello seskupování, před prošla hello podsítě a potom aktivuje pravidla NSG hello podsítě.

Další informace o [skupin zabezpečení sítě](virtual-networks-nsg.md) a jak se používají na základě přidružení toosubnets, virtuálních počítačů a síťových karet...

## <a name="how-tooconfigure-a-multi-nic-vm-in-a-classic-deployment"></a>Jak tooConfigure více virtuálních počítačů síťovou kartu v nasazení classic
níže uvedené pokyny Hello vám pomůže vytvořit více virtuálních počítačů síťovou kartu obsahující 3 síťové adaptéry: výchozí síťový adaptér a další dva síťové adaptéry. Postup konfigurace Hello vytvoří virtuální počítač, který bude nakonfigurován podle toohello služby konfigurační soubor fragment níže:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over hello remainder section …
    </VirtualNetworkSite>


Budete potřebovat následující požadavky a teprve potom zkusili toorun hello příkazy prostředí PowerShell v příkladu hello hello.

* Předplatné Azure.
* Nakonfigurované virtuální sítě. V tématu [Přehled virtuálních sítí](virtual-networks-overview.md) Další informace o virtuálních sítí.
* nejnovější verzi prostředí Azure PowerShell Hello stáhnout a nainstalovat. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

toocreate virtuálního počítače s více síťovými kartami, dokončení hello zadáním každý příkaz v rámci jedné relace prostředí PowerShell následující kroky:

1. Vyberte bitovou kopii virtuálního počítače z Galerie obrázků virtuálního počítače Azure. Všimněte si, že Image často mění a jsou dostupné podle oblasti. Hello image zadanou v následujícím příkladu hello nesmí změnit nebo může být ve vaší oblasti, takže je nutné toospecify hello bitové kopie je nutné.

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. Vytvoření konfigurace virtuálního počítače.

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. Vytvořte přihlašovací údaje správce výchozí hello.

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. Přidejte další konfigurace virtuálního počítače toohello síťových karet.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. Zadejte hello podsíť a IP adresu pro hello výchozí síťový adaptér.

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. Vytvořte hello virtuálního počítače ve virtuální síti.

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > Hello virtuální sítě, který zde určíte již musí existovat (jak je uvedeno v hello požadavky). Následující příklad Hello Určuje virtuální síť s názvem **MultiNIC-VNet**.
    >

## <a name="limitations"></a>Omezení
Při použití více síťových adaptérů platí Hello následující omezení:

* Virtuální počítače s více síťovými kartami, musí být vytvořený v Azure virtuální sítě (virtuální sítě). Virtuální počítače non-VNet se nedá nakonfigurovat se několik síťových adaptérů.
* Všechny virtuální počítače ve skupině dostupnosti nastavena toouse nutné několik síťových adaptérů nebo jeden síťový adaptér. Nemůže mít směs více virtuální síťový adaptér počítače a jeden síťový adaptér virtuálních počítačů v rámci skupiny dostupnosti. Stejná pravidla použít pro virtuální počítače v cloudové službě. Pro víc virtuálních počítačů síťovou kartu, nejsou požadované toohave hello stejný počet síťových adaptérů, tak dlouho, dokud každá má alespoň dvě.
* Virtuální počítač s jednu síťovou kartu nelze konfigurovat pomocí více síťových adaptérů (a naopak) po jejím nasazení, bez odstranit a znovu ji vytvořit.

## <a name="secondary-nics-access-tooother-subnets"></a>Sekundární síťové adaptéry přístup tooother podsítě
Ve výchozím nastavení sekundární síťové adaptéry se nenakonfigurují s výchozí bránou, z důvodu toowhich hello přenosový tok v hello sekundární síťové adaptéry bude omezený toobe v rámci hello stejné podsíti. Pokud uživatel hello tooenable sekundární síťové adaptéry tootalk mimo vlastní podsíti, potřebují tooadd položku v hello směrovací tabulky tooconfigure hello brány jako popsané dole.

> [!NOTE]
> Virtuální počítače vytvořené před července 2015 může mít výchozí brána konfigurována pro všechny síťové adaptéry. Hello výchozí brána pro sekundární síťové adaptéry budou odebrány až tyto virtuální počítače se restartují. V operačních systémech, které používají model, směrování hello slabé hostitele například Linux můžete přerušení připojení k Internetu, pokud hello příchozí a odchozí provoz používat různé síťové adaptéry.
> 

### <a name="configure-windows-vms"></a>Konfigurace virtuálních počítačů Windows
Předpokládejme, že máte virtuální počítač s Windows se dvěma síťovými adaptéry, následujícím způsobem:

* Primární síťovou kartu IP adresa: 192.168.1.4
* Sekundární síťový adaptér IP adresa: 192.168.2.5

Tabulka směrování IPv4 Hello pro tento virtuální počítač bude vypadat takto:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Všimněte si, že tento hello výchozí trasa (0.0.0.0) je pouze k dispozici toohello primární síťový adaptér. Nebudete moct tooaccess prostředky mimo hello podsíť pro hello sekundární síťový adaptér, jak vidíte níže:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

výchozí směrování na tooadd hello sekundární síťový adaptér, postupujte podle následujících kroků hello:

1. Z příkazového řádku, spusťte příkaz hello níže číslo indexu hello tooidentify hello sekundární síťovou kartu:
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. Všimněte si hello druhý záznam v tabulce hello, s indexem 27 (v tomto příkladu).
3. Z příkazového řádku hello, spusťte hello **přidat trasy** příkaz, jak je uvedeno níže. V tomto příkladu jsou určení 192.168.2.1 jako hello výchozí brána pro hello sekundární síťovou kartu:
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. tootest připojení, přejděte zpět toohello příkazového řádku a zkuste to tooping z jiné podsítě hello sekundární síťový adaptér jako uvedené int eh příkladu níže:
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. Můžete také zkontrolovat, že vaše trasy tabulky toocheck hello nově přidaná postup, jak je uvedeno níže:
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Konfigurovat virtuální počítače s Linuxem
Pro virtuální počítače s Linuxem, protože výchozí chování hello používá slabé hostitele směrování, doporučujeme, abyste tento hello sekundární síťové adaptéry jsou toky s omezeným přístupem tootraffic pouze v rámci hello stejné podsíti. Ale pokud určité scénáře potřebují připojení mimo hello podsíť, uživatelé měli povolit tooensure směrování na základě zásad, která hello příchozí a odchozí provoz používá hello stejné síťový adaptér.

## <a name="next-steps"></a>Další kroky
* Nasazení [MultiNIC virtuálních počítačů v aplikaci na vrstvě 2 scénář v nasazení Resource Manager](virtual-network-deploy-multinic-arm-template.md).
* Nasazení [MultiNIC virtuálních počítačů v aplikaci na vrstvě 2 scénář v nasazení classic](virtual-network-deploy-multinic-classic-ps.md).

