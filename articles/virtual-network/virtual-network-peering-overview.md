---
title: "Partnerské vztahy virtuálních sítí v Azure | Dokumentace Microsoftu"
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
ms.openlocfilehash: 393557074db2ddbeb53ca20873a33d06874c4dc8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="virtual-network-peering"></a>Partnerské vztahy virtuálních sítí
Partnerský vztah virtuálních sít umožňuje propojit dvě virtuální sítě ve stejné oblasti prostřednictvím páteřní sítě Azure. Po navázání partnerského vztahu se tyto dvě virtuální sítě pro účely připojení jeví jako jedna síť. Jsou i nadále spravované jako samostatné prostředky, ale virtuální počítače v partnerských virtuálních sítích spolu můžou komunikovat přímo pomocí privátních IP adres.

Přenos dat mezi virtuálními počítači ve virtuálních sítích spojených v partnerském vztahu je směrován přes infrastrukturu Azure v zásadě stejně jako mezi virtuálními počítači ve stejné virtuální síti. Mezi výhody použití partnerských vztahů virtuálních sítí patří:

* Nízká latence a velká šířka pásma při propojení prostředků v různých virtuálních sítích.
* Možnost používat prostředky, například síťová zařízení a VPN Gateway, jako přenosové body ve virtuální síti s navázaným partnerským vztahem.
* Možnost navázání partnerského vztahu mezi dvěma virtuálními sítěmi vytvořenými prostřednictvím modelu nasazení Azure Resource Manager, nebo mezi jednou virtuální sítí vytvořenou prostřednictvím modelu nasazení Resource Manager a druhou vytvořenou prostřednictvím modelu nasazení Classic. Další informace o rozdílech mezi těmito dvěma modely nasazení Azure najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="requirements-constraints"></a>Požadavky a omezení

* Partnerské virtuální sítě musí existovat ve stejné oblasti Azure. Pomocí [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) můžete propojit virtuální sítě v různých oblastech Azure.
* Adresní prostory IP adres partnerských virtuálních sítí se nesmí překrývat.
* Adresní prostory nelze do virtuálních sítí přidávat ani je z nich odstraňovat, jakmile je mezi dvěma virtuálními sítěmi vytvořen partnerský vztah.
* Partnerský vztah virtuálních sítí se navazuje mezi dvěma virtuálními sítěmi. Nevzniká žádný odvozený tranzitivní vztah přes partnerské vztahy. Pokud je například virtuální síť A v partnerském vztahu s virtuální sítí B a virtuální síť B v partnerském vztahu s virtuální sítí C, pak virtuální síť A *není* v partnerském vztahu s virtuální sítí C.
* Partnerský vztah lze navázat mezi virtuálními sítěmi ve dvou různých předplatných za předpokladu, že navázání tohoto vztahu u obou předplatných autorizuje uživatel s oprávněním správce (viz [konkrétní oprávnění](create-peering-different-deployment-models-subscriptions.md#permissions)) a že předplatná jsou přidružená ke stejnému tenantovi Azure Active Directory. K propojení virtuálních sítí v předplatných přidružených k různým tenantům Active Directory můžete použít [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
* Mezi virtuálními sítěmi lze navázat partnerský vztah, pokud jsou obě vytvořené prostřednictvím modelu nasazení Resource Manager, nebo pokud je jedna virtuální síť vytvořena prostřednictvím modelu nasazení Resource Manager a druhá prostřednictvím modelu nasazení Classic. Nejde ale navázat partnerský vztah mezi dvěma virtuálními sítěmi, které obě používají model nasazení Classic. K propojení dvou virtuálních sítí vytvořených prostřednictvím modelu nasazení Classic můžete použít službu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
* Ačkoli komunikace mezi virtuálními počítači ve virtuálních sítích propojených partnerským vztahem nemá žádná další omezení šířky pásma, stále platí omezení šířky pásma na základě velikosti virtuálního počítače. Další informace o maximální šířce pásma pro různé velikosti virtuálních počítačů najdete v článcích o velikosti virtuálních počítačů pro [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Interní překlad názvů DNS služby Azure pro virtuální počítače nebude fungovat mezi partnerskými virtuálními sítěmi fungovat. Virtuální počítače mají interní názvy DNS, které jsou přeložitelné pouze v rámci místní virtuální sítě. Virtuální počítače připojené k partnerským virtuálním sítím ale můžete nakonfigurovat jako servery DNS pro virtuální síť. Další podrobnosti najdete v článku s popisem [překladu názvů pomocí vlastního serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

![Základní partnerské vztahy virtuálních sítí](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Připojení
Po navázání partnerského vztahu mezi dvěma virtuálními sítěmi se prostředky v některé z virtuálních sítí můžou přímo připojit k prostředkům v partnerské virtuální síti. Mezi těmito dvěma virtuálními sítěmi existuje plné propojení na úrovni protokolu IP.

Síťová latence doby odezvy komunikace mezi dvěma virtuálními počítači ve virtuálních sítích s navázaným partnerským vztahem je stejná jako latence v rámci jedné virtuální sítě. Propustnost sítě závisí na šířce pásma, která je pro daný virtuální počítač povolená na základě jeho velikosti. S partnerským vztahem zde nejsou žádná další omezení týkající se šířky pásma.

Provoz mezi virtuálními počítači v partnerských virtuálních sítích je směrován přímo přes back-end infrastrukturu Azure, ne prostřednictvím brány.

Virtuální počítače připojené k virtuální síti mají přístup ke koncovým bodům s interním nástrojem na vyrovnávání zatížení v partnerské virtuální síti. Skupiny zabezpečení sítě je možné v obou virtuálních sítích v případě potřeby použít k blokování přístupu do jiných virtuálních sítí nebo podsítí.

Při konfiguraci partnerského vztahu virtuálních sítí můžete otevřít nebo zavřít pravidla skupiny zabezpečení sítě mezi virtuálními sítěmi. Pokud otevřete úplné propojení mezi partnerskými virtuálními sítěmi (výchozí volba), můžete pomocí skupin zabezpečení sítě u konkrétních podsítí nebo virtuálních počítačů blokovat nebo odepřít specifický přístup. Další informace o skupinách zabezpečení sítě najdete v článku [Přehled skupin zabezpečení sítě](virtual-networks-nsg.md).

## <a name="service-chaining"></a>Řetězení služeb
Můžete nakonfigurovat uživatelsky definované trasy, které jsou směrované na virtuální počítače v partnerských virtuálních sítích jako na IP adresy pro další směrování, a povolit tak řetězení služeb. Řetězení služeb vám umožní směrovat přenos dat z jedné virtuální sítě do virtuálního zařízení v partnerské virtuální síti s použitím uživatelsky definovaných tras.

Můžete také fakticky vytvářet prostředí hvězdicovitého typu, kde centrální síť může být hostitelem komponent infrastruktury, například síťového virtuálního zařízení. Všechny uvedené virtuální sítě pak můžou vytvořit partnerský vztah s centrální virtuální sítí. Provoz může probíhat přes virtuální síťová zařízení, která běží v centrální virtuální síti. Partnerské vztahy virtuálních sítí tedy umožňují, aby IP adresou dalšího směrování v uživatelsky definované trase byla IP adresa virtuálního počítače v partnerské virtuální síti. Další informace o trasách definovaných uživatelem najdete v článku s [přehledem tras definovaných uživatelem](virtual-networks-udr-overview.md).

## <a name="gateways-and-on-premises-connectivity"></a>Brány a místní připojení
Každá virtuální síť, bez ohledu na to, jestli má navázaný partnerský vztah s jinou virtuální sítí, může mít stále svou vlastní bránu a používat ji pro připojení k místní síti. Můžete také nakonfigurovat [připojení mezi virtuálními sítěmi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) pomocí bran, a to i v situaci, kdy jsou tyto virtuální sítě v partnerském vztahu.

Když jsou nakonfigurovány obě možnosti pro propojení virtuálních sítí, přenos dat mezi těmito sítěmi probíhá s použitím konfigurace partnerského vztahu (t.j. přes páteřní síť Azure).

Když je mezi virtuálními sítěmi navázán partnerský vztah, můžete také v partnerské virtuální síti nakonfigurovat bránu, která bude sloužit jako tranzitní bod pro místní síť. V tomto případě virtuální síť, která používá vzdálenou bránu, nemůže mít svou vlastní bránu. Virtuální síť může mít pouze jednu bránu. Může jí být buď místní brána, nebo vzdálená brána (v partnerské virtuální síti), jak ukazuje následující obrázek:

![Přenos dat při partnerských vztazích virtuálních sítí](./media/virtual-networks-peering-overview/figure02.png)

Průchod branou není podporován v případě, že jde o partnerský vztah mezi virtuálními sítěmi vytvořenými pomocí různých modelů nasazení. Pokud chcete používat průchod branou, je nutné, aby byly obě partnerské virtuální sítě vytvořené pomocí modelu nasazení Resource Manager.

Po nastavení partnerského vztahu mezi virtuálními sítěmi sdílejícími jedno připojení Azure ExpressRoute přenos dat prochází skrze partnerský vztah (tj. prostřednictvím páteřní sítě Azure). K připojení k místnímu okruhu lze v obou sítích nadále používat místní brány. Alternativním postupem je použití sdílené brány a konfigurace průchodu pro místní připojení.

## <a name="provisioning"></a>Zřizování
Navázání partnerského vztahu virtuální sítě je privilegovaná operace. Jde o samostatnou funkci v rámci oboru názvů VirtualNetworks. Uživatel může dostat specifická práva k autorizaci partnerských vztahů. Uživatel, který má přístup pro čtení a zápis do virtuální sítě, zdědí tato práva automaticky.

Uživatel, který je buď správcem nebo privilegovaným uživatelem pro navázání partnerských vztahů, může iniciovat operaci navázání partnerství s jinou virtuální sítí. Pokud i na druhé straně existuje odpovídající požadavek a jsou splněny i další podmínky, partnerský vztah bude navázán.

## <a name="limits"></a>Omezení
Existují omezení týkající se počtu partnerských vztahů, které lze vytvořit pro jednu virtuální síť. Další informace najdete v tématu [Omezení pro sítě v Azure](../azure-subscription-service-limits.md#networking-limits).

## <a name="pricing"></a>Ceny
Za příchozí a výchozí přenos dat využívající partnerský vztah virtuálních sítí se účtuje nominální poplatek. Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Další kroky

* Dokončete kurz o partnerském vztahu virtuálních sítí. Partnerský vztah virtuálních sítí se vytváří mezi virtuálními sítěmi vytvořenými prostřednictvím stejného modelu nebo různých modelů nasazení, které existují ve stejném předplatném nebo různých předplatných. Dokončete kurz pro jeden z následujících scénářů:
 
    |Model nasazení Azure  | Předplatné  |
    |---------|---------|
    |Obě Resource Manager |[Stejné](virtual-network-create-peering.md)|
    | |[Různé](create-peering-different-subscriptions.md)|
    |Jedna Resource Manager, druhá Classic     |[Stejné](create-peering-different-deployment-models.md)|
    | |[Různé](create-peering-different-deployment-models-subscriptions.md)|

* Zjistěte, jak vytvořit [rozbočovač a uvedenou síťovou topologii](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering). 
* Prostudujte si všechna [nastavení partnerského vztahu virtuálních sítí a jejich změny](virtual-network-manage-peering.md).
