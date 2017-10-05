---
title: "Přehled sítě Azure a virtuální počítač s Linuxem | Microsoft Docs"
description: "Přehled sítě Azure a virtuální počítač s Linuxem."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: b5420e35-0463-4456-9803-6142db150f2e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/25/2016
ms.author: v-livech
ms.openlocfilehash: 1ff4a0482d6dc6ec0eceaa89ca4b87ba1e2f89a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-and-linux-vm-network-overview"></a>Přehled sítě virtuálního počítače s Linuxem a Azure
## <a name="virtual-networks"></a>Virtuální sítě
Virtuální síť Azure (VNet) je reprezentace vaší vlastní sítě v cloudu. Je to logická izolace cloudu Azure vyhrazeného pro vaše předplatné. V rámci této sítě máte plnou kontrolu nad bloky IP adres, nastavením DNS, zásadami zabezpečení a směrovacími tabulkami. Můžete také dál segmentovat do podsítí virtuální sítě a spouštět virtuální počítače Azure IaaS (VM) nebo cloudové služby (instance rolí PaaS). Virtuální síť můžete navíc připojit k místní síti pomocí jedné z možností připojení k dispozici v Azure. V podstatě můžete svoji síť rozšířit do Azure s úplnou kontrolou nad bloky IP adres a s výhodou poskytovatelů Azure celopodnikového rozsahu.

* [Přehled virtuálních sítí](../../virtual-network/virtual-networks-overview.md)

Chcete-li vytvořit síť VNet pomocí rozhraní příkazového řádku Azure, přejděte sem k procházení projít.

* [Postup vytvoření sítě VNet pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)

## <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě)
Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel seznamu řízení přístupu (ACL), která instancím virtuálních počítačů ve službě Virtual Network povolují nebo odpírají síťový provoz. Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti. Pokud je skupina NSG přidružená k podsíti, pravidla seznamu ACL platí pro všechny instance virtuálních počítačů v této podsíti. Provoz směřující do konkrétního virtuálního počítače se navíc dá dál omezit tím, že se přímo k tomuto virtuálnímu počítači přidruží skupina NSG.

* [Co je skupina zabezpečení sítě (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Jak vytvořit skupiny NSG v Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)

## <a name="user-defined-routes"></a>Trasy definované uživatelem
Když přidáte virtuální počítače do virtuální sítě v Azure, uvidíte, že tyto virtuální počítače automaticky umí vzájemně komunikovat prostřednictvím sítě. Není nutné určit bránu, ani když jsou virtuální počítače v různých podsítích. Totéž platí pro komunikaci z virtuálních počítačů do veřejného internetu, a dokonce i do vaší místní sítě, pokud je dostupné hybridní připojení z Azure do vlastního datacentra.

* [Co jsou trasy definované uživatelem a předávání IP?](../../virtual-network/virtual-networks-udr-overview.md)
* [Vytvoření UDR v Azure CLI](../../virtual-network/virtual-network-create-udr-arm-cli.md)

## <a name="associating-a-fqdn-to-your-linux-vm"></a>Přidružení plně kvalifikovaný název domény k virtuálním počítačům s Linuxem
Když vytvoříte virtuální počítač (VM) na portálu Azure pomocí modelu nasazení Resource Manager, se automaticky vytvoří prostředek veřejné IP pro virtuální počítač. Tuto IP adresu použijete vzdálený přístup k virtuálnímu počítači. I když na portál nevytvoří plně kvalifikovaný název domény, nebo plně kvalifikovaný název domény, ve výchozím nastavení, můžete přidat po vytvoření virtuálního počítače.

* [Vytvořit plně kvalifikovaný název domény na portálu Azure](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="network-interfaces"></a>Síťová rozhraní
Síťové rozhraní (NIC) je propojení mezi virtuálním počítačem a základní softwarovou sítí. Tento článek vysvětluje, co je síťové rozhraní a jak se používá v modelu nasazení Azure Resource Manager.

* [Rozhraní virtuální sítě](../../virtual-network/virtual-network-network-interface.md)

## <a name="virtual-nics-and-dns-labeling"></a>Virtuální síťové adaptéry a označování DNS
Pokud máte server, který je potřeba mít trvalé, ale server se považuje za zvířat a odpojí a nasadí často, bude chcete použijte DNS označování na svoji síťovou kartu k uchování název virtuální sítě.  Následující návod bude instalační program trvale s názvem síťový adaptér se statickou IP adresu.

* [Vytvoření kompletní prostředí Linux pomocí rozhraní příkazového řádku Azure](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="virtual-network-gateways"></a>Brány virtuálních sítí
Brána virtuální sítě slouží ke směrování síťového provozu mezi virtuálními sítěmi Azure a místními umístěními, stejně jako mezi virtuálními sítěmi v rámci Azure (VNet-to-VNet). Při konfiguraci brány sítě VPN je třeba vytvořit a nakonfigurovat bránu virtuální sítě a připojení brány virtuální sítě.

* [Informace o službě VPN Gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

## <a name="internal-load-balancing"></a>Interní Vyrovnávání zatížení
Azure Load Balancer je nástroj pro vyrovnávání zatížení úrovně 4 (TCP, UDP). Nástroj pro vyrovnávání zatížení poskytuje vysokou dostupnost díky distribuci příchozích přenosů mezi instance služeb, které jsou v pořádku, v cloudových službách nebo virtuálních počítačích v sadě nástroje pro vyrovnávání zatížení. Azure Load Balancer můžete také tyto služby prezentovat na více portech, více IP adresách nebo obojím.

* [Vytvoření interní nástroj pomocí rozhraní příkazového řádku Azure](../../load-balancer/load-balancer-get-started-internet-arm-cli.md)

