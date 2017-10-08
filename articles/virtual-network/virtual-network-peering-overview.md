---
title: "partnerský vztah aaaAzure virtuální sítě | Microsoft Docs"
description: "Seznamte se s partnerskými vztahy virtuálních sítí v Azure."
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: narayan
ms.openlocfilehash: 46a14b416a7d4389f79a3cd7c55e388b5d312577
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-network-peering"></a>Partnerské vztahy virtuálních sítí
Virtuální sítě můžete tooconnect dvou virtuálních sítí v hello stejné oblasti prostřednictvím partnerského vztahu umožňuje hello Azure páteřní síti. Po peered, hello dvě virtuální sítě se zobrazí jako jedna pro účely připojení. Hello dvě virtuální sítě se pořád spravují jako samostatné prostředky, ale virtuální počítače v hello peered virtuální sítě může komunikovat s navzájem přímo, pomocí privátních IP adres.

Hello provoz mezi virtuálními počítači v hello peered virtuální sítě směrován přes hello infrastrukturu Azure, jako je směrovat provoz mezi virtuálními počítači v hello stejné virtuální síti. Mezi výhody hello použití partnerský vztah virtuální sítě, patří:

* Nízká latence a velká šířka pásma při propojení prostředků v různých virtuálních sítích.
* možnost Hello toouse prostředkům, například síťových zařízení a bran VPN jako body přenosu v peered virtuální sítě.
* Hello možnost toopeer dvě virtuální sítě vytvořené pomocí modelu nasazení Azure Resource Manager hello nebo toopeer jednu virtuální síť vytvořili pomocí Správce prostředků tooa virtuální sítě vytvořené pomocí modelu nasazení classic hello. Čtení hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku toolearn více informací o hello rozdíly mezi hello dvou modelech nasazení Azure.

## <a name="requirements-constraints"></a>Požadavky a omezení

* Hello peered virtuální sítě, musí existovat v hello stejné oblasti Azure. Pomocí [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) můžete propojit virtuální sítě v různých oblastech Azure.
* Hello peered virtuální sítě musí mít-překrývající se adresní prostory IP adres.
* Adresní prostory nelze do virtuálních sítí přidávat ani je z nich odstraňovat, jakmile je mezi dvěma virtuálními sítěmi vytvořen partnerský vztah.
* Partnerský vztah virtuálních sítí se navazuje mezi dvěma virtuálními sítěmi. Nevzniká žádný odvozený tranzitivní vztah přes partnerské vztahy. Například pokud je s virtualNetworkB peered virtualNetworkA a virtualNetworkB je peered s virtualNetworkC, virtualNetworkA je *není* peered toovirtualNetworkC.
* Mohou párově virtuální sítě, které existují ve dvou různých předplatných, jako dlouho privilegovaných uživatelů (v tématu [konkrétní oprávnění](create-peering-different-deployment-models-subscriptions.md#permissions)) i odběry autorizuje hello partnerský vztah a hello odběry jsou přidružené toohello stejné Klienta Azure Active Directory. Můžete použít [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect virtuální sítě v rámci předplatných přidružené toodifferent klientech služby Active Directory.
* Virtuální sítě můžete peered, pokud obě jsou vytvořeny pomocí modelu nasazení Resource Manager hello nebo pokud je vytvořena jedna virtuální síť pomocí modelu nasazení Resource Manager hello a hello jiných je vytvořen pomocí modelu nasazení classic hello. Dvě virtuální sítě vytvořené pomocí modelu nasazení classic hello nemůže být peered tooeach jiných, ale. Můžete použít [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect dvě virtuální sítě vytvořené pomocí modelu nasazení classic hello.
* I když hello komunikace mezi virtuálními počítači ve virtuálních sítích, peered neexistují žádná omezení. Další šířka pásma, je maximální šířku pásma sítě, v závislosti na velikosti hello virtuální počítač, který stále platí. Další informace o maximální šířku pásma sítě pro jiný virtuální počítač velikostí, přečtěte si hello toolearn [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálního počítače.
* Interní překlad názvů DNS služby Azure pro virtuální počítače nebude fungovat mezi partnerskými virtuálními sítěmi fungovat. Virtuální počítače mají interní názvy DNS, které jsou přeložit pouze v rámci hello místní virtuální síť. Ale můžete konfigurovat virtuální sítě virtuálních počítačů připojený toopeered jako servery DNS pro virtuální síť. Další informace najdete v hello [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) článku.

![Základní partnerské vztahy virtuálních sítí](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Připojení
Po kterými mají partnerský dvě virtuální sítě, prostředky ve buď virtuální síti mohou připojovat přímo s prostředky v hello peered virtuální sítě. Hello dvě virtuální sítě mít úplná IP-úroveň připojení.

latence sítě Hello pro výměnu zpráv mezi dvěma virtuálními počítači ve virtuálních sítích, peered je hello stejné jako u dobu odezvy v rámci jedné virtuální sítě. propustnost sítě Hello je založena na hello šířky pásma, který je povolen pro hello virtuální počítač, hlavně tooits velikost. Není k dispozici žádné další omezení šířky pásma v rámci hello partnerský vztah.

Hello provoz mezi virtuálními počítači v peered virtuální sítě se směruje přímo přes hello Azure back endovou infrastrukturu, nejsou prostřednictvím brány.

Virtuální počítače připojené tooa virtuální síti mohly přistupovat k hello interní Vyrovnávání zatížení sítě koncových bodů v hello peered virtuální sítě. Skupiny zabezpečení sítě může být pro virtuální síť tooblock přístup tooother virtuální sítě nebo podsítě, v případě potřeby.

Při konfiguraci partnerského vztahu virtuální sítě, můžete otevřít nebo zavřít pravidel skupiny zabezpečení sítě hello mezi virtuálními sítěmi hello. Pokud otevřete úplné připojení mezi peered virtuální sítě (což je výchozí možnost hello), můžete použít sítě zabezpečení skupiny toospecific podsítě nebo virtuální počítače tooblock nebo odepření přístupu konkrétním. Další informace o toolearn sítě skupin zabezpečení, přečtěte si hello [přehled skupin zabezpečení sítě](virtual-networks-nsg.md) článku.

## <a name="service-chaining"></a>Řetězení služeb
Můžete nakonfigurovat trasy definované uživatelem tohoto počítače toovirtual bodu ve virtuálních sítích, peered jako hello "dalšího směrování" IP adresu tooenable služby řetězení. Služba řetězení umožňuje vám toodirect provoz z jedné virtuální sítě tooa virtuální zařízení v peered virtuální sítě prostřednictvím trasy definované uživatelem.

Můžete vytvořit také efektivně střed a paprsek typu prostředí, kde hello hub může hostovat komponent infrastruktury, jako je například virtuální síťové zařízení. Všechny hello ramenem virtuální sítě můžete pak partnerský vztah hello rozbočovače virtuální sítě. Můžete přenos přes síť virtuální zařízení, které běží ve virtuální síti hello rozbočovače. Stručně řečeno partnerský vztah virtuální sítě umožňuje hello IP adresa dalšího směrování na hello uživatelem definované trasy toobe hello IP adresu virtuálního počítače ve virtuální síti hello peered. Další informace o trasy definované uživatelem, přečtěte si hello toolearn [trasy definované uživatelem přehled](virtual-networks-udr-overview.md) článku.

## <a name="gateways-and-on-premises-connectivity"></a>Brány a místní připojení
Každá virtuální síť, bez ohledu na to, jestli je peered s jinou virtuální sítí, můžete stále mít svůj vlastní bránu a použít ho tooconnect tooan do místní sítě. Můžete také nakonfigurovat [připojení virtuální sítě pro virtuální sítě](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) pomocí brány, i když kterými mají partnerský hello virtuální sítě.

Pokud jsou obě možnosti pro propojení virtuální sítě nakonfigurována, hello provoz mezi virtuálními sítěmi hello toků prostřednictvím konfigurace partnerského vztahu hello (tedy až hello Azure páteřní).

Když virtuální sítě se kterými mají partnerský, lze také nakonfigurovat hello brány v hello peered virtuální sítě jako místní síť tooan bodu přenosu. V takovém případě hello virtuální síť, která používá Brána vzdálené nemůže mít vlastní brány. Virtuální síť může mít pouze jednu bránu. Hello brány může být místní nebo vzdálené brány (v hello peered virtuální sítě), jak je znázorněno v následujícím obrázku hello:

![Přenos dat při partnerských vztazích virtuálních sítí](./media/virtual-networks-peering-overview/figure02.png)

Přenosu brány není podporována v relaci partnerského vztahu hello mezi virtuální sítě vytvořené pomocí různé modely nasazení. Obě virtuální sítě v relaci partnerského vztahu hello musí být vytvořen pomocí Resource Manageru pro toowork přenosu brány.

Při kterými mají partnerský hello virtuální sítě, které sdílejí jedno připojení k Azure ExpressRoute, hello provoz mezi nimi prochází hello partnerského vztahu relace (tedy až hello Azure páteřní síti). V každé virtuální sítě tooconnect toohello místní okruhu můžete dál používat místní brány. Alternativním postupem je použití sdílené brány a konfigurace průchodu pro místní připojení.

## <a name="provisioning"></a>Zřizování
Navázání partnerského vztahu virtuální sítě je privilegovaná operace. Je samostatný funkce pod oborem názvů VirtualNetworks hello. Uživatel může dostat partnerský vztah tooauthorize konkrétní práva. Uživatel, který má přístup pro čtení a zápis toohello virtuální sítě automaticky zdědí tato práva.

Uživatel, který je buď správce nebo privilegované uživateli možnost hello partnerského vztahu můžete zahájit partnerského vztahu operace s jinou virtuální sítí. Pokud existuje odpovídající požadavku pro partnerský vztah na druhé straně hello, a pokud jsou splněny další požadavky, se naváže partnerský vztah hello.

## <a name="limits"></a>Omezení
Existují omezení počtu hello partnerských vztahů, které jsou povoleny pro jedné virtuální sítě. Další informace najdete v tématu hello [Azure síťové omezení](../azure-subscription-service-limits.md#networking-limits).

## <a name="pricing"></a>Ceny
Za příchozí a výchozí přenos dat využívající partnerský vztah virtuálních sítí se účtuje nominální poplatek. Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Další kroky

* Dokončete kurz o partnerském vztahu virtuálních sítí. Partnerský vztah virtuální sítě se vytvoří mezi virtuální sítě vytvořené pomocí hello stejné nebo jiné nasazení modely, které existují v hello stejné nebo různých předplatných. Dokončení kurzu pro jednu hello následující scénáře:
 
    |Model nasazení Azure  | Předplatné  |
    |---------|---------|
    |Obě Resource Manager |[Stejné](virtual-network-create-peering.md)|
    | |[Různé](create-peering-different-subscriptions.md)|
    |Jedna Resource Manager, druhá Classic     |[Stejné](create-peering-different-deployment-models.md)|
    | |[Různé](create-peering-different-deployment-models-subscriptions.md)|

* Zjistěte, jak toocreate [hvězdicové topologie sítě](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) 
* Další informace o všech [partnerského vztahu nastavení virtuální sítě a jak toochange je](virtual-network-manage-peering.md)
