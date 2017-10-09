---
title: "aaaAzure síťové infrastruktury – pokyny pro Linux | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro nasazení virtuálních sítí ve službách infrastruktury Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7ebd5da-3c62-45e8-ad90-6c73a37ebeb1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3f49b135b1f9bca3fd463e9ff27299fb97908c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking-infrastructure-guidelines-for-linux-vms"></a>Azure sítě infrastruktury pokyny pro virtuální počítače s Linuxem

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na pochopení hello požadované plánování kroků pro virtuální sítě v rámci Azure a připojení mezi existující místní prostředí.

## <a name="implementation-guidelines-for-virtual-networks"></a>Postup implementace pro virtuální sítě
Rozhodnutí:

* Jaký typ virtuální sítě potřebujete toohost IT zatížení nebo infrastrukturu (jenom pro cloud nebo mezi různými místy)?
* Pro virtuální sítě mezi různými místy, kolik adresní prostor potřebujete toohost hello podsítí a virtuálních počítačů teď a přiměřené rozšíření v hello budoucí?
* Jsou můžete přechodem toocreate centralizované virtuální sítě nebo vytvořit jednotlivé virtuální sítě pro každou skupinu prostředků?

Úlohy:

* Definujte hello adresního prostoru toobe hello virtuální sítě vytvořené.
* Definujte hello sadu podsítě a adresní prostor hello adres pro každou.
* Pro virtuální sítě mezi různými místy definujte hello sadu místní sítě adresních prostorů pro hello místní umístění, která hello virtuálních počítačů v tooreach nutné hello virtuální sítě.
* Práce s místní sítí team tooensure hello odpovídající směrování, lze nastavit při vytváření mezi různými místy virtuální sítě.
* Vytvoření virtuální sítě hello pomocí zásady vytváření názvů.

## <a name="virtual-networks"></a>Virtuální sítě
Virtuální sítě jsou nezbytné toosupport komunikace mezi virtuálními počítači (VM). Můžete definovat podsítě, vlastní IP adresu, nastavení DNS, zabezpečení, filtrování a načíst vyrovnávání, stejně jako u fyzických sítí. Pomocí [brány VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) nebo [okruhu Express Route](../../expressroute/expressroute-introduction.md), se můžete připojit virtuální sítě Azure tooyour místní sítě. Další informace o [virtuální sítě a jejich součástí](../../virtual-network/virtual-networks-overview.md).

Pomocí skupin prostředků, budete mít flexibilitu v tom, jak navrhnete součásti vaší virtuální sítě. Virtuální počítače můžete připojit toovirtual sítě mimo své vlastní skupiny prostředků. Běžný postup návrhu by toocreate centralizované skupiny prostředků, které obsahují vaše základní síťová infrastruktura, která je možné spravovat pomocí běžných tým. Virtuální počítače a jejich aplikace nasadit tooseparate skupiny prostředků. Tento přístup umožňuje vlastníci aplikace přístup toohello skupinu prostředků, která obsahuje jejich virtuální počítače bez otevření až konfigurace toohello přístupu prostředků hello širší virtuální sítě.

## <a name="site-connectivity"></a>Připojení k webu
### <a name="cloud-only-virtual-networks"></a>Jenom pro cloud virtuálních sítí
Pokud místní uživatelé a počítače nevyžadují připojení probíhající tooVMs v virtuální síť Azure, návrh vaší virtuální sítě je přímo dál:

![Diagram základní čistě cloudové virtuální sítě](./media/infrastructure-networking-guidelines/vnet01.png)

Tento přístup je obvykle pro internetový úloh, jako jsou internetového na webovém serveru. Můžete spravovat tyto virtuální počítače pomocí SSH nebo připojení k síti VPN point-to-site.

Vzhledem k tomu, aby se nepřipojovali tooyour do místní sítě, jen Azure virtuální sítě můžete použít libovolnou část prostor hello privátní IP adresy. Hello adresní prostor může být hello použít stejnou privátní prostor, který je v místní.

### <a name="cross-premises-virtual-networks"></a>Mezi různými místy virtuální sítě
Pokud místní uživatelé a počítače vyžadují tooVMs probíhající připojení v virtuální síť Azure, vytvořte virtuální síť mezi různými místy. Připojte hello virtuální sítě tooyour místní sítí s ExpressRoute nebo připojení site-to-site VPN.

![Mezi různými místy virtuální síťový diagram](./media/infrastructure-networking-guidelines/vnet02.png)

V této konfiguraci hello virtuální síť Azure je v podstatě založená na cloudu rozšíření místní sítě.

Protože se připojují tooyour místní sítě, mezi různými místy, že virtuální sítě musí používat část hello adresní prostor ve své organizaci, které jsou jedinečné. V hello stejný způsobem této různých sídlech společnosti jsou přiřazeny určité podsítě IP, Azure stane jiného umístění, jak rozšířit na síti.

tooallow tootravel pakety z místní sítě tooyour virtuální sítě mezi různými místy, je nutné nakonfigurovat hello sadu předpon adres relevantní místní jako součást definice hello místní sítě pro virtuální síť hello. V závislosti na adresu hello místo sady hello virtuální sítě a hello relevantní místní umístění, může být velký počet předpon adres místní sítě hello.

Můžete převést na virtuální síť mezi různými místy tooa čistě cloudové virtuální sítě, ale pravděpodobně vyžaduje toore IP adresního prostoru virtuální sítě a prostředky Azure. Proto pečlivě zvažte, pokud virtuální síť musí toobe připojené tooyour do místní sítě při přiřazení podsítě protokolu IP.

## <a name="subnets"></a>Podsítě
Podsítě umožňují tooorganize prostředky, které se vztahují, buď logicky (například jednu podsíť pro virtuální počítače přidružené toohello stejná aplikace), nebo fyzicky (například jednu podsíť skupinu prostředků). Můžete také použít techniky izolace podsítě pro zvýšení zabezpečení.

Pro virtuální sítě mezi různými místy, měli byste navrhnout podsítě s hello stejnými názvů, které používáte pro místní prostředky. **Azure vždy používá hello první tři IP adresy hello adresního prostoru pro každou podsíť**. číslo hello toodetermine adres potřebné pro hello podsíť, začněte tím, že počítání hello počet virtuálních počítačů, které nyní potřebujete. Odhad pro budoucí růst a potom pomocí následující tabulky toodetermine hello velikost podsítě hello hello.

| Počet virtuálních počítačů, které jsou potřeba | Počet bitů hostitele potřeby | Velikost podsítě hello |
| --- | --- | --- |
| 1–3 |3 |/29 |
| 4–11 |4 |/28 |
| 12–27 |5 |/27 |
| 28–59 |6 |/26 |
| 60–123 |7 |/25 |

> [!NOTE]
> Pro běžné místní podsítě hello maximální počet hostitelské adresy pro podsíť s bity n hostitele je 2<sup> n </sup> – 2. Pro podsíť Azure hello maximální počet hostitelské adresy pro podsíť s bity n hostitele je 2<sup> n </sup> – 5 (2 a 3 pro hello adresy, které Azure používá v každé podsíti).
> 
> 

Pokud si zvolíte velikost podsítě, která je příliš malá, máte toore IP a znovu nasaďte hello virtuální počítače v podsíti hello.

## <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě)
Můžete použít filtrování provozu toohello pravidla, která se zobrazí prostřednictvím virtuálních sítí pomocí skupin zabezpečení sítě. Můžete vytvořit podrobné filtrování toosecure pravidla vaše virtuální síťové prostředí, řízení příchozí a odchozí přenosy, zdroj a cíl rozsazích IP adres, povolené porty atd. Skupiny zabezpečení sítě může být použité toosubnets v rámci virtuální sítě nebo přímo tooa zadané rozhraní virtuální sítě. Je doporučeno toohave určité úrovně skupinu zabezpečení sítě filtrování provozu na virtuálních sítí. Další informace o [skupin zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md).

## <a name="additional-network-components"></a>Další síťové součásti
Stejně jako u místní fyzické síťové infrastruktury, virtuálních sítí Azure může obsahovat více než jen podsítě a IP adresy. Při navrhování infrastruktury aplikace, může být vhodné tooincorporate některé z těchto dalších součástí:

* [Brány sítě VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) -připojení virtuální sítě Azure tooother Azure virtuální sítě, nebo se připojit tooon místní sítě prostřednictvím připojení Site-to-Site VPN. Implementace připojení Express Route pro vyhrazené a zabezpečené připojení. Přímý přístup uživatelů můžete poskytnout i připojení VPN typu Point-to-Site.
* [Nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-overview.md) -poskytuje Vyrovnávání zatížení provozu pro externí i interní provoz podle potřeby.
* [Aplikační brána](../../application-gateway/application-gateway-introduction.md) – HTTP Vyrovnávání zatížení v hello aplikační vrstvu, poskytuje některé další výhody pro webové aplikace místo nasazení služby Vyrovnávání zatížení Azure hello.
* [Správce provozu](../../traffic-manager/traffic-manager-overview.md) – na základě DNS provozu distribuční toodirect koncoví uživatelé toohello nejbližší dostupný koncový bod aplikace, povolení toohost aplikace mimo datových centrech Azure v různých oblastech.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

