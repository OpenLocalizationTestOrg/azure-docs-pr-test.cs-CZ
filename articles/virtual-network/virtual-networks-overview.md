---
title: "aaaAzure virtuální sítě | Microsoft Docs"
description: "Další informace o Azure Virtual Network konceptů a funkcí."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2017
ms.author: jdial
ms.openlocfilehash: 55ae6a131d882ad893aeffcaa4127bc47beda552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

Hello Azure Virtual Network service umožňuje toosecurely můžete připojit prostředky Azure tooeach jiné s virtuálními sítěmi (virtuální sítě). Virtuální síť je reprezentace vlastní sítě v cloudu hello. Virtuální síť je to logická izolace cloudu Azure vyhrazeného tooyour předplatné hello. Můžete také připojit virtuální sítě tooyour do místní sítě. Hello následující obrázek ukazuje některé z možností hello hello Azure Virtual Network service:

![Síťový diagram](./media/virtual-networks-overview/virtual-network-overview.png)

toolearn Další informace o hello následující funkce Azure Virtual Network, klikněte na možnost hello:
- **[Izolace:](#isolation)**  virtuální sítě jsou izolované od sebe navzájem. Můžete vytvořit samostatné virtuální sítě pro vývoj, testování a produkci této hello použijte stejné bloky adres CIDR. Naopak můžete vytvořit více virtuálních sítí, použít jiné bloky adres CIDR a společně připojení sítě. Virtuální síť můžete rozdělit do několika podsítí. Azure poskytuje interní překlad adres pro virtuální počítače a instance rolí cloudové služby připojený tooa virtuální sítě. Volitelně můžete nakonfigurovat virtuální síť toouse vlastní servery DNS, místo použití Azure interní překlad adres.
- **[Připojení k Internetu:](#internet)**  cloudových služeb pro všechny virtuální počítače Azure (VM) a instancí rolí tooa připojené virtuální sítě mají přístup k Internetu, toohello ve výchozím nastavení. Můžete také povolit příchozí přístup k prostředkům toospecific, podle potřeby.
- **[Připojení prostředků Azure:](#within-vnet)**  prostředky Azure, jako je například cloudových služeb a virtuálních počítačů může být připojené toohello stejnou virtuální síť. Hello prostředky můžete připojit tooeach jiných použití privátních IP adres, i když jsou v různých podsítích. Azure poskytuje výchozí směrování mezi podsítěmi, virtuálními sítěmi a místními sítěmi, takže nemáte tooconfigure a spravovat trasy.
- **[Připojení virtuální sítě:](#connect-vnets)**  virtuální sítě může být připojené tooeach jiné, povolení prostředky připojené virtuální sítě toocommunicate tooany s jakýmikoli prostředky na ostatní virtuální sítě.
- **[Místní připojení:](#connect-on-premises)**  virtuální sítě můžou být připojené tooon místní sítě prostřednictvím privátní sítě připojení mezi vaší sítí a Azure, nebo připojení site-to-site VPN přes hello Internet.
- **[Filtrování přenosu:](#filtering)**  virtuálního počítače a cloudové služby role instance síťového provozu je možné filtrovat příchozí a odchozí zdrojové IP adresy a portu, cílové IP adresy a portu a protokolu.
- **[Směrování:](#routing)**  Volitelně můžete přepsat výchozí Azure směrování konfiguraci vlastních tras, nebo pomocí trasy protokolu BGP prostřednictvím brány sítě.

## <a name = "isolation"></a>Izolace sítě a segmentace

Můžete implementovat více virtuálních sítí v rámci každé Azure [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) a Azure [oblast](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Každý virtuální sítě je izolovaná od jiných virtuálních sítí. Pro každý virtuální síť můžete:
- Zadejte vlastní prostor privátní IP adresy pomocí veřejné a privátní adresy (RFC 1918). Azure přiřadí připojené toohello prostředky virtuální sítě privátní IP adresy z hello adresního prostoru, který přiřadíte.
- Segment hello virtuální sítě do jedné nebo více podsítí a přidělovat část podsíť tooeach hello virtuální síť adres místa.
- Můžete použít Azure překlad nebo zadejte že vlastní server DNS za účelem použití prostředků připojený tooa virtuální sítě. Další informace o překlad názvů ve virtuálních sítí, přečtěte si hello toolearn [překlad názvů pro virtuální počítače a cloudové služby](virtual-networks-name-resolution-for-vms-and-role-instances.md) článku.

## <a name = "internet"></a>Připojit toohello Internetu
Všechny prostředky připojené tooa virtuální síť mít toohello odchozí připojení k Internetu ve výchozím nastavení. Hello privátní IP adresu prostředku hello je, že zdrojová síťová adresa přeložen hello infrastrukturu Azure (překládat pomocí SNAT) tooa veřejnou IP adresu. Další informace o odchozí připojení k Internetu, přečtěte si hello toolearn [pochopení odchozí připojení v Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) článku. Implementací vlastní směrování a filtrování provozu, můžete změnit výchozí připojení hello.

toocommunicate příchozí tooAzure prostředky z hello Internet nebo toocommunicate odchozí toohello, které Internetu bez překládat pomocí SNAT, prostředek musí být přiřazen veřejnou IP adresu. Další informace o veřejné IP adresy, přečtěte si hello toolearn [veřejné IP adresy](virtual-network-public-ip-address.md) článku.

## <a name="within-vnet"></a>Připojení prostředků Azure
Několik prostředků Azure tooa virtuální sítě, jako jsou virtuální počítače (VM), cloudové služby, prostředí App Service a sady škálování virtuálního počítače se můžete připojit. Virtuální počítače připojení tooa podsítí v rámci virtuální sítě přes síťové rozhraní (NIC). Další informace o síťových adaptérů, přečtěte si hello toolearn [síťových rozhraní](virtual-network-network-interface.md) článku.

## <a name="connect-vnets"></a>Připojit virtuální sítě

Virtuální sítě tooeach další se můžete připojit, povolení prostředky toocommunicate tooeither virtuální síť připojení mezi sebou mezi virtuálními sítěmi. Můžete použít jednu nebo obě následující možnosti tooconnect virtuálních sítí tooeach jiných hello:
- **Partnerský vztah:** umožňuje prostředky připojené toodifferent sítě Azure Vnet v rámci hello stejné umístění Azure toocommunicate mezi sebou. Hello šířky pásma a latence napříč hello virtuální sítě je hello stejné jako v případě, že hello prostředky byly připojené toohello stejnou virtuální síť. toolearn Další informace o vytvoření partnerského vztahu, přečtěte si hello [partnerský vztah virtuální sítě](virtual-network-peering-overview.md) článku.
- **Připojení VNet-to-VNet:** umožňuje prostředky připojené toodifferent virtuální síť Azure v rámci hello stejný nebo jiný Azure umístění. Na rozdíl od partnerského vztahu, šířka pásma je omezená mezi virtuálními sítěmi, protože provoz musí procházet skrz bránu VPN Azure. Další informace o propojení virtuálních sítí s připojení VNet-to-VNet, přečtěte si hello toolearn [konfigurace připojení typu VNet-to-VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.

## <a name="connect-on-premises"></a>Připojit tooan do místní sítě

Můžete připojit vaše místní síť tooa sítě VNet pomocí libovolné kombinace hello následující možnosti:
- **Point-to-site virtuální privátní sítě (VPN):** mezi jeden počítač připojený tooyour sítě a hello virtuální sítě. Tento typ připojení je skvělé, pokud jste se právě Začínáme s Azure, nebo pro vývojáře, protože vyžaduje žádné nebo téměř žádné změny tooyour stávající síť. připojení Hello používá hello SSTP protokol tooprovide zašifrovaná komunikace přes Internet hello mezi hello počítači a hello virtuální sítě. Hello latence pro síť VPN point-to-site nepředvídatelným, protože hello provoz prochází hello Internetu.
- **Site-to-site VPN:** mezi zařízení VPN a služby Azure VPN Gateway. Tento typ připojení umožňuje jakémukoli prostředku místní autorizujete tooaccess virtuální sítě. je Hello připojení VPN pomocí protokolu IPSec/IKE, která poskytuje šifrovanou komunikaci přes Internet hello mezi místní zařízení a hello Azure VPN gateway. Hello latence pro připojení site-to-site nepředvídatelným, protože hello provoz prochází hello Internetu.
- **Azure ExpressRoute:** navázat mezi vaší sítí a Azure prostřednictvím partnerem ExpressRoute. Toto připojení je soukromé. Provoz neprochází hello Internetu. Hello latence pro připojení typu ExpressRoute je předvídatelný, vzhledem k tomu, že provoz není procházení hello Internetu.

Další informace o všech hello předchozí možnosti připojení, přečtěte si hello toolearn [diagramy topologie připojení](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams) článku.

## <a name="filtering"></a>Filtrování provozu sítě přenosů
Můžete filtrovat síťový provoz mezi podsítěmi pomocí jedné nebo obou hello následující možnosti:
- **Skupin zabezpečení (NSG) sítě:** každou NSG může obsahovat více zabezpečení příchozí a odchozí pravidla, která umožňují toofilter provoz podle zdrojové a cílové IP adresy, portu a protokolu. Můžete použít NSG tooeach síťový adaptér ve virtuálním počítači. Můžete taky použít podsíť toohello NSG síťový adaptér nebo jiných prostředků Azure, je připojen k. Další informace o skupin Nsg, přečtěte si hello toolearn [skupin zabezpečení sítě](virtual-networks-nsg.md) článku.
- **Síťových virtuálních zařízení (hodnocení chyb zabezpečení):** hodnocení chyb zabezpečení sítě je virtuální počítač spuštěný software, který provádí síťové funkce, jako je například Brána firewall. Zobrazit seznam dostupných NVAs v hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). NVAs jsou také k dispozici, které poskytují optimalizace sítě WAN a jiných síťových přenosů funkce. NVAs jsou obvykle používány s uživatelem nebo trasy protokolu BGP. Můžete také použít hodnocení chyb zabezpečení toofilter provoz mezi virtuálními sítěmi.

## <a name="routing"></a>Směrovat síťový provoz

Azure vytvoří směrovací tabulky, které umožňují prostředkům připojených tooany podsíť v toocommunicate žádné virtuální sítě s sebou ve výchozím nastavení. Můžete implementovat buď nebo obě následující možnosti toooverride hello hello výchozích tras, které vytvoří Azure:
- **Trasy definované uživatelem:** můžete vytvořit vlastní směrovací tabulky s trasami, které řídí, kde je provoz směrovaný toofor každou podsíť. Další informace o trasy definované uživatelem, přečtěte si hello toolearn [trasy definované uživatelem](virtual-networks-udr-overview.md) článku.
- **Trasy protokolu BGP:** Pokud připojíte vaší virtuální sítě tooyour místní sítí pomocí připojení k Azure VPN Gateway nebo ExpressRoute, můžete rozšířit tooyour trasy protokolu BGP virtuální sítě.

## <a name="pricing"></a>Ceny

Skupiny zabezpečení není nijak zpoplatněn pro virtuální sítě, podsítě, směrovací tabulky nebo sítě. Odchozí šířky pásma Internetu, veřejné IP adresy, partnerský vztah virtuální sítě, brány sítě VPN a ExpressRoute každý mají svůj vlastní ceny struktury. Zobrazení hello [virtuální síť](https://azure.microsoft.com/pricing/details/virtual-network), [brány VPN](https://azure.microsoft.com/pricing/details/vpn-gateway), a [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) ceny stránky pro další informace.

## <a name="faq"></a>Nejčastější dotazy

tooreview nejčastější dotazy k virtuální síti, najdete v části hello [virtuální sítě – nejčastější dotazy](virtual-networks-faq.md) článku.


## <a name="next-steps"></a>Další kroky

- Vytvoření vaší první virtuální sítě a připojení tooit několik virtuálních počítačů, pomocí kroků hello v hello [vytvoření vaší první virtuální síť](virtual-network-get-started-vnet-subnet.md) článku.
- Vytvoření připojení point-to-site tooa virtuální sítě pomocí kroků hello v hello [konfigurace připojení typu point-to-site](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.
- Další informace o některých hello Další klíč [sítě možnosti](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.
