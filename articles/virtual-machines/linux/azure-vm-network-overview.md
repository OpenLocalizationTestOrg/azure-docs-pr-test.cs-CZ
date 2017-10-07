---
title: "aaaAzure a přehled sítě virtuálních počítačů Linux | Microsoft Docs"
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
ms.openlocfilehash: c3de2dc583c62160e10c0e97e96fef49b9eaffbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-and-linux-vm-network-overview"></a>Přehled sítě virtuálního počítače s Linuxem a Azure
## <a name="virtual-networks"></a>Virtuální sítě
Virtuální sítě Azure (VNet) je reprezentace vlastní sítě v cloudu hello. Je to logická izolace cloudu Azure vyhrazeného tooyour předplatné hello. Můžete plně řídit hello bloky IP adres, nastavení DNS, zásady zabezpečení a směrovací tabulky v rámci této sítě. Můžete také dál segmentovat do podsítí virtuální sítě a spouštět virtuální počítače Azure IaaS (VM) nebo cloudové služby (instance rolí PaaS). Kromě toho se můžete připojit hello virtuální sítě tooyour místní síti pomocí jedné z možností připojení hello v Azure k dispozici. V zásadě můžete rozšířit vaše síť tooAzure, s úplnou kontrolou nad bloky IP adres s výhodou hello měřítka enterprise, které poskytuje Azure.

* [Přehled virtuálních sítí](../../virtual-network/virtual-networks-overview.md)

hello toocreate virtuální sítě pomocí rozhraní příkazového řádku Azure, přejděte sem toofollow hello ukázce.

* [Jak hello toocreate virtuální sítě pomocí rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)

## <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě)
Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel seznamu řízení přístupu (ACL), která povolují nebo odpírají síťový provoz tooyour instance virtuálních počítačů ve virtuální síti. Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti. Pokud je skupina NSG přidružená k podsíti, pravidla seznamu ACL hello platí tooall hello instance virtuálních počítačů v této podsíti. Kromě toho tooan provoz jednotlivých virtuálních počítačů, je možné omezit tím, že přidružíte skupina NSG další přímo toothat virtuálních počítačů.

* [Co je skupina zabezpečení sítě (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Jak toocreate skupin Nsg v hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)

## <a name="user-defined-routes"></a>Trasy definované uživatelem
Když přidáte virtuální počítače (VM) tooa virtuální síť (VNet) v Azure, si všimnete hello virtuální počítače automaticky se možné toocommunicate mezi sebou přes síť hello. Není nutné toospecify bránu, i když hello virtuální počítače jsou v různých podsítích. Hello totéž platí pro komunikaci z virtuálních počítačů toohello hello veřejného Internetu a dokonce tooyour do místní sítě při hybridní připojení z Azure tooyour vlastní datacenter je k dispozici.

* [Co jsou trasy definované uživatelem a předávání IP?](../../virtual-network/virtual-networks-udr-overview.md)
* [Vytvoření UDR v hello rozhraní příkazového řádku Azure](../../virtual-network/virtual-network-create-udr-arm-cli.md)

## <a name="associating-a-fqdn-tooyour-linux-vm"></a>Přidružení tooyour plně kvalifikovaný název domény virtuálního počítače s Linuxem
Při vytváření virtuálního počítače (VM) v hello portálu Azure pomocí modelu nasazení Resource Manager hello, veřejnou IP adresu prostředku pro hello virtuální počítač se automaticky vytvoří. Můžete použít tuto IP adresu tooremotely přístup hello virtuálních počítačů. Přestože portál hello nevytvoří plně kvalifikovaný název domény, nebo plně kvalifikovaný název domény, ve výchozím nastavení, můžete přidat po vytvoření hello virtuálních počítačů.

* [Vytvoření plně kvalifikovaný název domény v hello portálu Azure](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="network-interfaces"></a>Síťová rozhraní
Síťové rozhraní (NIC) je hello propojení mezi virtuální počítač (VM) a hello základní sítě softwaru. Tento článek vysvětluje, jaké síťové rozhraní a jeho použití v modelu nasazení Azure Resource Manager hello.

* [Rozhraní virtuální sítě](../../virtual-network/virtual-network-network-interface.md)

## <a name="virtual-nics-and-dns-labeling"></a>Virtuální síťové adaptéry a označování DNS
Pokud máte server musíte toobe trvalé, že tento server se považuje za zvířat a odpojí a nasadí často, budete chtít toouse DNS označování na název vaší seskupování toopersist hello na hello virtuální sítě.  Hello následující návodu se instalační program trvale s názvem síťový adaptér se statickou IP adresu.

* [Vytvoření kompletní prostředí Linux pomocí hello rozhraní příkazového řádku Azure](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="virtual-network-gateways"></a>Brány virtuálních sítí
Brána virtuální sítě se používá toosend síťový provoz mezi virtuálních sítí Azure a místní umístění a také mezi virtuálními sítěmi v rámci Azure (VNet-to-VNet). Při konfiguraci brány sítě VPN je třeba vytvořit a nakonfigurovat bránu virtuální sítě a připojení brány virtuální sítě.

* [Informace o službě VPN Gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

## <a name="internal-load-balancing"></a>Interní Vyrovnávání zatížení
Azure Load Balancer je nástroj pro vyrovnávání zatížení úrovně 4 (TCP, UDP). Nástroj pro vyrovnávání zatížení Hello poskytuje vysokou dostupnost distribucí příchozí komunikaci mezi instance pořádku služby ve cloudových službách nebo virtuálních počítačů v sadě nástroje pro vyrovnávání zatížení. Azure Load Balancer můžete také tyto služby prezentovat na více portech, více IP adresách nebo obojím.

* [Vytvoření interní nástroj používá hello rozhraní příkazového řádku Azure](../../load-balancer/load-balancer-get-started-internet-arm-cli.md)

