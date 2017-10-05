---
title: "Azure síťové infrastruktury – pokyny pro Windows | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro nasazení virtuálních sítí ve službách infrastruktury Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e2d45973-5eba-4904-8ba0-1821f64feed7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 95303c8e393b759b03d3f64996510a830193ba46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-networking-infrastructure-guidelines-for-windows-vms"></a>Pokyny infrastruktury sítě pro virtuální počítače Windows Azure 

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na pochopení požadované kroky plánování pro virtuální sítě v rámci Azure a připojení mezi existující místní prostředí.

## <a name="implementation-guidelines-for-virtual-networks"></a>Postup implementace pro virtuální sítě
Rozhodnutí:

* Jaký typ virtuální sítě potřebujete k hostování zatížení IT nebo infrastrukturu (jenom pro cloud nebo mezi různými místy)?
* Pro virtuální sítě mezi různými místy kolik místa na adresu potřebujete hostovat podsítí a virtuálních počítačů teď a v budoucnu přiměřené rozšíření?
* Chystáte se vytvořit centralizované virtuální sítě nebo jednotlivé virtuální sítě pro každou skupinu prostředků?

Úlohy:

* Definujte adresní prostor virtuálních sítí, který se má vytvořit.
* Zadejte pro každou sadu podsítě a adresní prostor.
* Pro virtuální sítě mezi různými místy definujte sadu místní sítě adresních prostorů pro místní umístění, které je třeba virtuální počítače ve virtuální síti připojit.
* Práce s místním sítě týmu a zkontrolujte příslušné směrování se nakonfiguruje při vytvoření mezi různými místy virtuální sítě.
* Vytvoření virtuální sítě pomocí zásady vytváření názvů.

## <a name="virtual-networks"></a>Virtuální sítě
Virtuální sítě jsou nezbytné pro podporu komunikace mezi virtuálními počítači (VM). Můžete definovat podsítě, vlastní IP adresu, nastavení DNS, zabezpečení, filtrování a načíst vyrovnávání, stejně jako u fyzických sítí. Pomocí [brány VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) nebo [okruhu Express Route](../../expressroute/expressroute-introduction.md), virtuálních sítí Azure můžete připojit k místní sítě. Další informace o [virtuální sítě a jejich součástí](../../virtual-network/virtual-networks-overview.md).

Pomocí skupin prostředků, budete mít flexibilitu v tom, jak navrhnete součásti vaší virtuální sítě. Virtuální počítače můžete připojit k virtuálním sítím mimo své vlastní skupiny prostředků. Běžný postup návrhu by k vytváření skupin centralizované prostředků, které obsahují vaše základní síťová infrastruktura, která je možné spravovat pomocí běžných tým a potom virtuální počítače a jejich aplikace nasazené do samostatné skupiny prostředků. Tento přístup umožňuje přístup k aplikaci vlastníky do skupiny prostředků, který obsahuje jejich virtuální počítače bez otevřením konfiguraci širší virtuálních síťových prostředků.

## <a name="site-connectivity"></a>Připojení k webu
### <a name="cloud-only-virtual-networks"></a>Jenom pro cloud virtuálních sítí
Pokud místní uživatelé a počítače nevyžadují probíhající připojení k virtuálním počítačům v virtuální síť Azure, návrh vaší virtuální sítě je přímo dál:

![Diagram základní čistě cloudové virtuální sítě](./media/infrastructure-networking-guidelines/vnet01.png)

Tento přístup je obvykle pro internetový úloh, jako jsou internetového na webovém serveru. Můžete spravovat těchto virtuálních počítačů pomocí protokolu RDP nebo připojení k síti VPN point-to-site.

Vzhledem k tomu, že se nepřipojí k síti na pracovišti, jen Azure virtuální sítě můžete použít libovolnou část prostor privátní IP adresy, i když je stejný privátní místa v použít místní.

### <a name="cross-premises-virtual-networks"></a>Mezi různými místy virtuální sítě
Pokud místní uživatelé a počítače vyžadovat probíhající připojení k virtuální počítače ve virtuální síť Azure, vytvořte virtuální síť mezi různými místy.  Připojí k vaší místní sítí s ExpressRoute nebo připojení site-to-site VPN.

![Mezi různými místy virtuální síťový diagram](./media/infrastructure-networking-guidelines/vnet02.png)

V této konfiguraci je virtuální síť Azure v podstatě založená na cloudu rozšířením místní sítě.

Protože se připojit k síti na pracovišti, mezi různými místy, že virtuální sítě musí používat část adresní prostor ve své organizaci, které jsou jedinečné. Stejným způsobem, který různých sídlech společnosti jsou přiřazeny určité podsítě IP se stane Azure jako rozšířit na síti jiného umístění.

Povolit pakety do přecházíte z virtuální sítě mezi různými místy k síti na pracovišti, je nutné nakonfigurovat sadu předpon adres relevantní místní jako součást definice místní sítě pro virtuální síť. V závislosti na adresní prostor virtuální sítě a sadu relevantní místní umístění může být velký počet předpon adres místní sítě.

Můžete převést čistě cloudové virtuální síť k virtuální síti mezi různými místy, ale pravděpodobně se vyžaduje, abyste znovu IP adresního prostoru virtuální sítě a prostředky Azure. Proto pečlivě zvažte, pokud virtuální síť musí být připojen k síti na pracovišti, když přiřadíte podsíť protokolu IP.

## <a name="subnets"></a>Podsítě
Povolit podsítě, můžete k uspořádání prostředků, které se vztahují, buď logicky (například jednu podsíť pro virtuální počítače přidružené k stejnou aplikaci), nebo fyzicky (například jednu podsíť skupinu prostředků). Můžete také použít techniky izolace podsítě pro zvýšení zabezpečení.

Pro virtuální sítě mezi různými místy měli byste navrhnout podsítě se stejnými názvů, které používáte pro místní prostředky. **Azure vždy používá první tři IP adresy do adresního prostoru pro každou podsíť**. Můžete určit počet požadovaných pro podsíť adres, spusťte určovat počet virtuálních počítačů, které nyní potřebujete. Určení pro budoucí růst a potom pomocí následující tabulky můžete určit velikost podsítě.

| Počet virtuálních počítačů, které jsou potřeba | Počet bitů hostitele potřeby | Velikost podsítě |
| --- | --- | --- |
| 1–3 |3 |/29 |
| 4–11 |4 |/28 |
| 12–27 |5 |/27 |
| 28–59 |6 |/26 |
| 60–123 |7 |/25 |

> [!NOTE]
> Pro běžné místní podsítě, je maximální počet hostitelské adresy pro podsíť s n hostitele bity 2<sup> n </sup> – 2. Pro podsíť Azure je maximální počet hostitelské adresy pro podsíť s n hostitele bity 2<sup> n </sup> – 5 (2 a 3 pro adresy, které Azure používá v každé podsíti).
> 
> 

Pokud si zvolíte velikost podsítě, která je příliš malá, je nutné znovu IP a znovu nasadit virtuální počítače v podsíti.

## <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě)
Filtrování pravidla můžete použít k provozu přenášeného přes virtuální sítě pomocí skupin zabezpečení sítě. Podrobné pravidel filtrování zabezpečit vaše virtuální síťové prostředí, můžete vytvořit řízení příchozí a odchozí přenosy, zdroj a cíl rozsazích IP adres, povolené porty atd. Skupiny zabezpečení sítě můžete použít k podsítím v rámci virtuální sítě nebo přímo na dané virtuální síti rozhraní. Doporučuje se mít některé úroveň skupinu zabezpečení sítě filtrování provozu na virtuálních sítí. Další informace o [skupin zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md).

## <a name="additional-network-components"></a>Další síťové součásti
Stejně jako u místní fyzické síťové infrastruktury, může obsahovat více než podsítě a IP adresování virtuálních sítí Azure. Při navrhování infrastruktury aplikace, můžete začlenit některé z těchto dalších součástí:

* [Brány sítě VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) – připojení virtuální sítě Azure k jiné virtuální sítě Azure nebo připojení k místní sítě prostřednictvím připojení Site-to-Site VPN. Implementace připojení Express Route pro vyhrazené a zabezpečené připojení. Přímý přístup uživatelů můžete poskytnout i připojení VPN typu Point-to-Site.
* [Nástroj pro vyrovnávání zatížení](../../load-balancer/load-balancer-overview.md) -poskytuje Vyrovnávání zatížení provozu pro externí i interní provoz podle potřeby.
* [Aplikační brána](../../application-gateway/application-gateway-introduction.md) – HTTP Vyrovnávání zatížení na aplikační vrstvu, poskytuje některé další výhody pro webové aplikace místo nasazení služby Vyrovnávání zatížení Azure.
* [Správce provozu](../../traffic-manager/traffic-manager-overview.md) – na základě DNS provozu distribuční přímé koncovým uživatelům na nejbližší dostupné aplikace koncového bodu, což umožňuje hostování vaší aplikace z datových centrech Azure v různých oblastech.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

