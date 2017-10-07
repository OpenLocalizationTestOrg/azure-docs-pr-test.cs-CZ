---
title: "Brána Classic tooResource aaaVPN při migraci správce | Microsoft Docs"
description: "Tato stránka obsahuje přehled hello Classic brány VPN tooResource při migraci správce."
documentationcenter: na
services: vpn-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: caa8eb19-825a-4031-8b49-18fbf3ebc04e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: amsriva
ms.openlocfilehash: c1d84bf5315224c5c8faac53d7957b496ed205ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vpn-gateway-classic-tooresource-manager-migration"></a>Brána sítě VPN classic tooResource při migraci správce
Brány sítě VPN můžete nyní migraci z modelu nasazení classic tooResource Manager. Další informace o službě Správce prostředků Azure [funkce a výhody](../azure-resource-manager/resource-group-overview.md). V tomto článku jsme podrobnosti, jak model založený na toomigrate z toonewer klasického nasazení Resource Manager. 

Brány sítě VPN se migrují jako součást migrace virtuální sítě z classic tooResource správce. Tato migrace provádí jednu virtuální síť v čase. Neexistuje žádný další požadavek z hlediska toomigration nástroje nebo požadavky. Kroky migrace jsou identické tooexisting virtuální síť migrace a jsou popsány v [stránka migrace prostředky IaaS](../virtual-machines/windows/migration-classic-resource-manager-ps.md). Neexistuje žádné výpadky cesta dat během migrace, a proto by existující úlohy pokračovat toofunction bez ztráty připojení místní během migrace. během procesu migrace hello nezmění Hello veřejnou IP adresu přidruženou hello brány VPN. To znamená, že nebudete potřebovat tooreconfigure místní směrovač po dokončení migrace hello.  

model Hello ve službě Správce prostředků se liší od klasického modelu a se skládá z brány virtuální sítě, brány místní sítě a prostředky připojení. Tyto představují samotné brány VPN hello, hello místní site představující na místní adresní prostor a připojení mezi hello, dva v uvedeném pořadí. Po dokončení migrace vašich bran nebude k dispozici v klasického modelu a všechny operace správy v brány virtuální sítě, brány místní sítě a objektů připojení musí být provedeno pomocí modelu Resource Manager.

## <a name="supported-scenarios"></a>Podporované scénáře
Obvyklé scénáře, připojení VPN jsou předmětem classic tooResource při migraci správce. Hello Podporované scénáře zahrnují-

* Připojení k bodu toosite
* Lokality toosite připojení ke službě VPN Gateway připojené tooon místní umístění
* Virtuální síť tooVNet připojení mezi oběma virtuálními sítěmi pomocí brány sítě VPN
* Více virtuálních sítí připojené toosame na místní umístění
* Připojení více lokalit
* Povolit vynucené tunelování virtuální sítě

Scénáře, které nejsou podporovány zahrnují-  

* Virtuální síť, bránu ExpressRoute i bránu VPN není aktuálně podporován.
* Přenosu scénáře, kde rozšíření virtuálního počítače jsou připojené tooon místní servery. Omezení připojení VPN přenosu jsou podrobně popsány níže.

> [!NOTE]
> Ověření CIDR v modelu Resource Manager je striktní více než hello, jeden v klasického modelu. Před migrací Ujistěte se, že zadané rozsahy classic adres odpovídat formátu CIDR toovalid před zahájením migrace hello. CIDR může být ověřen pomocí všechny běžné validátory CIDR. Virtuální síť nebo místní lokality s neplatnou rozsahy CIDR při migraci by způsobilo stavu selhání.
> 
> 

## <a name="vnet-toovnet-connectivity-migration"></a>Virtuální síť tooVNet připojení migrace
Bylo dosaženo připojení tooVNet virtuální síť v klasickém tak, že vytvoříte místní lokalita reprezentace hello připojené virtuální sítě. Zákazníci se vyžaduje toocreate dva místní weby, které reprezentované hello dvě virtuální sítě, které potřeba toobe propojeny. Ty se pak připojené toohello odpovídající pomocí protokolu IPsec tunel tooestablish připojení mezi virtuálními sítěmi hello dvě virtuální sítě. Tento model má problémy spravovatelnosti, protože změny rozsahu adres ve virtuální síti jeden musí lze udržovat také v hello odpovídající reprezentace místního webu. V modelu Resource Manager se už nepotřebuje toto řešení. Hello připojení mezi hello dvě virtuální sítě může být přímo opírá typ připojení: Vnet2Vnet"v prostředku připojení. 

![Migrace tooVNet snímek virtuální sítě.](./media/vpn-gateway-migration/migration1.png)

Migrace virtuální sítě, které je zjištěno, hello připojené entity toocurrent virtuální síť VPN gateway je jiné virtuální síti a ujistěte se, že po dokončení migrace obě virtuální sítě, by se již nezobrazují dva místní weby představující hello jiné virtuální sítě. Hello klasického modelu dvě brány sítě VPN, dva místní weby a dvě připojení mezi nimi je transformovaných tooResource Manager model, který obsahuje dvě brány sítě VPN a dvě připojení typu Vnet2Vnet.

## <a name="transit-vpn-connectivity"></a>Připojení k síti VPN přenosu
Brány sítě VPN můžete konfigurovat v topologii tak, aby místní připojení pro virtuální síť se dosahuje tooanother virtuální síť, která je přímo připojený tooon místní připojení. Toto je přenosu připojení VPN, kde nejsou připojené tooon místní prostředky prostřednictvím brány VPN toohello přenosu v připojené virtuální sítě, který je přímo připojený tooon místní instance v první sítě VNet. tooachieve tuto konfiguraci v modelu nasazení classic, je nutné toocreate místní lokality, který má agregovat předpony představující obou hello připojené virtuální sítě a místní adresní prostor. Tento representational místní web je pak toohello připojené virtuální sítě tooachieve přenosu připojení. Vzhledem k tomu, že všechny změny v místní rozsah adres musí být zachovaná také na místní lokality hello představující hello agregace virtuální sítě a místní, tento klasického modelu má také podobné možnosti správy problémů. Zavedení podpory protokolu BGP v Resource Manager podporován brány zjednodušuje možnosti správy, protože hello připojené brány další trasy z místní bez tooprefixes ruční úpravy.

![Snímek obrazovky scénář směrování přenosu.](./media/vpn-gateway-migration/migration2.png)

Vzhledem k tomu, že jsme transformace tooVNet připojení virtuální sítě bez nutnosti místní weby, ztratí hello přenosu scénář místní připojení pro virtuální síť, která je nepřímo připojené tooon místní. Hello ztráty připojení lze zmírnit v následujících dvou způsobů, hello po dokončení migrace - 

* Povolte protokol BGP na brány sítě VPN, které jsou propojeny a tooon místní. Povolení protokolu BGP obnoví připojení bez další změny konfigurace, protože trasy se naučili a ohlášené mezi brány virtuální sítě. Všimněte si, že protokol BGP možnost je dostupná jenom na standardní a vyšší SKU.
* Vytvořte explicitní spojení z ovlivněné virtuální sítě toohello místní síťové brány reprezentující místní umístění. To by také vyžadovat změnu konfigurace na hello místní směrovač toocreate a nakonfigurujte tunelu IPsec hello.

## <a name="next-steps"></a>Další kroky
Po získání informací o podpora migrace brány sítě VPN, přejděte příliš[platformy podporované migrace prostředky infrastruktury z classic tooResource Manager](../virtual-machines/windows/migration-classic-resource-manager-ps.md) tooget spuštěna.

