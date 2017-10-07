---
title: "aaaAzure ExpressRoute pro poskytovatele cloudových řešení | Microsoft Docs"
description: "Tento článek obsahuje informace pro poskytovatele cloudových služeb, že chcete tooincorporate Azure služeb a ExpressRoute do svých nabídek."
documentationcenter: na
services: expressroute
author: richcar
manager: carmonm
editor: 
ms.assetid: f6c5f8ee-40ba-41a1-ae31-67669ca419a6
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: richcar
ms.openlocfilehash: 062ecbb5e461e4384b01c4ac478cab696b7ed398
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-for-cloud-solution-providers-csp"></a>Azure ExpressRoute pro poskytovatele Cloud Solution Provider
Společnost Microsoft poskytuje služby pro tradiční prodejce a distributory (CSP) toobe toorapidly možné zřídit nové služby a řešení pro zákazníky bez hello zapotřebí tooinvest do vývoje těchto nových služeb. tooallow hello Cloud Solution Provider (CSP) hello možnost toodirectly spravovat tyto nové služby, společnost Microsoft poskytuje programy a rozhraní API umožňující hello CSP toomanage prostředky Microsoft Azure za své zákazníky. Jeden z těchto prostředků je ExpressRoute. ExpressRoute umožňuje hello CSP tooconnect existující prostředky tooAzure zákaznické. ExpressRoute je vysokorychlostní zajišťuje privátní komunikaci odkaz tooservices v Azure. 

ExpresRoute sestává z dvojice okruhů pro zajištění vysoké dostupnosti, které jsou připojené tooa jednoho zákazníka odběry a nesmí se sdílet více zákazníků. Každý okruh by měl být ukončen v s vysokou dostupností různých směrovačích toomaintain hello.

> [!NOTE]
> V ExpressRoute se uplatňují omezení šířky pásma a počtu připojení, což znamená, že velké nebo složité implementace budou vyžadovat více okruhů ExpressRoute pro jednoho zákazníka.
> 
> 

Microsoft Azure poskytuje rostoucí počet služeb, že můžete nabídnout zákazníkům tooyour.  toobest využít výhody těchto služeb, bude vyžadovat použití hello ExpressRoute připojení tooprovide vysokou rychlostí s nízkou latencí přístup toohello prostředí Microsoft Azure.

## <a name="microsoft-azure-management"></a>Správa Microsoft Azure
Microsoft poskytuje CSP rozhraní API toomanage hello předplatných Azure zákazníků tím, že programovou integraci s vlastními systémy správy služeb. Podporované možnosti správy najdete [tady](https://msdn.microsoft.com/library/partnercenter/dn974944.aspx).

## <a name="microsoft-azure-resource-management"></a>Správa prostředků Microsoft Azure
V závislosti na hello smlouvě se zákazníkem určí, jak se budou spravovat předplatné hello. Hello CSP může přímo spravovat vytváření hello a údržby prostředků nebo hello zákazníka můžete zachovat kontrolu nad hello předplatné Microsoft Azure a vytvořit hello prostředky Azure podle potřeby. Pokud zákazník spravuje vytváření prostředků v svého předplatného Microsoft Azure hello bude používat jeden ze dvou modelů: "Connect-Through" model, nebo "Direct-To" modelu. Tyto modely jsou podrobně popsány v následující části hello.  

### <a name="connect-through-model"></a>Model s nepřímým připojením
![alternativní text](./media/expressroute-for-cloud-solution-providers/connect-through.png)  

V hello model s nepřímým připojením hello CSP vytvoří přímé připojení mezi vaším datovým centrem a předplatným Azure zákazníka. Hello přímé připojení se vytvoří pomocí ExpressRoute, které propojuje vaši síť s Azure. Pak se zákazník připojí tooyour sítě. Tento scénář vyžaduje, že tohoto zákazníka hello projdou hello CSP sítě tooaccess služby Azure. 

Pokud má zákazník i jiná předplatná Azure, nejsou spravované přes Dobrý den, můžete, používají by hello veřejný Internet nebo vlastní privátní připojení tooconnect toothose službám zřizovaným v rámci předplatného mimo příslušného poskytovatele CSP hello. 

Poskytovatele CSP, který spravuje služby Azure se předpokládá, že tento hello CSP má identitu administrate zákazníka úložiště, které se poté replikuje do služby Azure Active Directory pro správu příslušného předplatného poskytovatele CSP prostřednictvím vytvořil (AOBO). Mezi klíčové předpoklady pro tento scénář patří, kdy má příslušný partner nebo poskytovatel služeb vytvořen vztah se zákazníkem hello má, hello zákazník aktuálně spotřebovává služby poskytovatele nebo hello partner má desire tooprovide kombinaci poskytovatele a hostovaných a Azure hostovaná řešení tooprovide flexibilitu a adresa zákazníka výzvy, které poskytovatel CSP sám nedokáže splnit. Tento model je znázorněn na následujícím **obrázku**.

![alternativní text](./media/expressroute-for-cloud-solution-providers/connect-through-model.png)

### <a name="connect-toomodel"></a>Připojit toomodel
![alternativní text](./media/expressroute-for-cloud-solution-providers/connect-to.png)

V hello Connect-toomodel hello vytvoří poskytovatel služby přímé připojení mezi datovým centrem zákazníka a hello CSP zřídit předplatné použitím ExpressRoute prostřednictvím hello zákazníka (zákazníka) sítě.

> [!NOTE]
> Pro ExpressRoute zákazník hello by potřebovat toocreate a udržovat hello okruh ExpressRoute.  
> 
> 

Tento scénář připojení vyžaduje že tento hello zákazník připojil přímo prostřednictvím sítě tooaccess zákazník předplatnému Azure spravovanému poskytovatele CSP pomocí přímého síťového připojení, který je vytvořen, vlastní a spravuje hello zákazníka zcela nebo částečně. U těchto zákazníků se předpokládá, že zprostředkovatel hello aktuálně nemá vytvořeno úložiště identit zákazníka a zprostředkovatele hello by assist hello zákazníka při replikaci jeho aktuálního úložiště identit do služby Azure Active Directory pro správu jejich předplatná prostřednictvím funkce AOBO. Mezi klíčové předpoklady pro tento scénář patří, kdy má příslušný partner nebo poskytovatel služeb vytvořen vztah se zákazníkem hello má, hello zákazník aktuálně spotřebovává služby poskytovatele nebo hello partner má služby tooprovide přání, které jsou založené výhradně na Azure hostovaná řešení bez hello potřebovat pro existující datacenter zprostředkovatele nebo infrastruktury.

![alternativní text](./media/expressroute-for-cloud-solution-providers/connect-to-model.png)

Hello volba mezi těmito dvěma možnostmi jsou založená na potřebách zákazníka a vaše aktuální potřebovat tooprovide Azure services. Hello podrobnosti o těchto modelech a hello přidruženého řízení přístupu na základě rolí, sítě a vzorech návrhu identity jsou uvedeny v hello následující odkazy:

* **Řízení přístupu na základě role (RBAC)** – Funkce RBAC je založena na službě Azure Active Directory.  Další informace o funkci Azure RBAC najdete [tady](../active-directory/role-based-access-control-configure.md).
* **Sítě** – zahrnuje hello různým tématům týkající se sítí v Microsoft Azure.
* **Azure Active Directory (AAD)** – AAD poskytuje hello Správa identit pro Microsoft Azure a aplikace SaaS 3. stran. Další informace o Azure AD najdete [tady](https://azure.microsoft.com/documentation/services/active-directory/).  

## <a name="network-speeds"></a>Rychlost sítě
ExpressRoute podporuje rychlost sítě 50 Mb/s too10Gb/s. To umožňuje zákazníkům toopurchase hello šířku pásma sítě, které jsou potřeba pro konkrétní jedinečné prostředí.

> [!NOTE]
> Podle potřeby bez přerušení komunikace, můžete zvětšit šířku pásma sítě, ale rychlost tooreduce hello sítě vyžaduje zrušení okruhu hello a jeho opětné vytvoření hello nižší rychlostí sítě.  
> 
> 

ExpressRoute podporuje hello připojení více virtuálních sítí tooa jeden okruh ExpressRoute pro lepší využití hello vysokorychlostní připojení. Jeden okruh ExpressRoute může být sdílen více předplatnými Azure vlastníkem hello tentýž zákazník.

## <a name="configuring-expressroute"></a>Konfigurace ExpressRoute
ExpressRoute může být nakonfigurované toosupport tři typy přenosů ([domény směrování](#ExpressRoute-routing-domains)) přes jeden okruh ExpressRoute. Tento provoz je rozdělen na partnerský vztah Microsoftu, veřejný partnerský vztah Azure a privátní partnerský vztah. Můžete vybrat jednoho nebo všech typů přenosů toobe odešlou přes jeden okruh ExpressRoute nebo použít více okruhů ExpressRoute v závislosti na velikosti hello hello okruh ExpressRoute a izolaci požadované zákazníkem. Hello postavení zabezpečení vašeho zákazníka, nemusí umožňovat veřejné provozu a tootraverse privátní provoz přes hello stejnému okruhu.

### <a name="connect-through-model"></a>Model s nepřímým připojením
V hello konfigurace s nepřímým připojením budete odpovídat pro všechny hello sítě podchycení tooconnect odběrů zákazníkům prostředky datacenter toohello hostované v Azure. Každý z vašich zákazníků, který má být toouse možnosti Azure bude potřebovat vlastní připojení ExpressRoute, které bude spravovat hello je. Hello použijete hello tentýž zákazník hello metody využije okruh ExpressRoute tooprocure hello. Hello postupujte podle stejných kroků uvedených v článku hello hello [pracovních postupech](expressroute-workflows.md) ohledně zřizování okruhů a stavů okruhů. Hello pak nakonfigurujete hello protokol BGP (Border Gateway) trasy toocontrol hello provoz mezi hello místní sítí a virtuální sítí Azure.

### <a name="connect-toomodel"></a>Připojit toomodel
V connect-tooconfiguration zákazníkovi už má existující připojení tooAzure nebo zahájí poskytovatele služeb Internetu toohello připojení cílem propojit ExpressRoute z vašeho vlastního datového centra zákazníka přímo tooAzure, místo vašeho datového centra. toobegin hello procesu zřizování, zákazník bude podle kroků hello, jak je popsáno v hello model s nepřímým připojením, výše. Po vytvoření okruhu hello zákazníkovi potřebovat tooconfigure hello místní směrovače toobe možné tooaccess vaší sítí a virtuálních sítí Azure.

Může být užitečné nastavení hello připojení služby a konfiguraci hello směruje tooallow hello prostředky ve vašem datových centrech toocommunicate client hello prostředky ve vašem datovém centru nebo s hello prostředky hostovanými v Azure.

## <a name="expressroute-routing-domains"></a>Domény směrování ExpressRoute
ExpressRoute nabízí tři domény směrování: veřejnou, privátní partnerský vztah Microsoftu. Každé z domén směrování hello konfigurují se s použitím stejných směrovačů v konfiguraci aktivní aktivní pro zajištění vysoké dostupnosti. Podrobnější informace o doménách směrování ExpressRoute najdete [tady](expressroute-circuit-peerings.md).

Můžete definovat vlastní trasy filtry tooallow pouze hello trasy má tooallow nebo potřebujete. Další informace nebo toosee jak toomake těchto změn najdete v části článku: [vytvoření a úprava směrování pro okruhu ExpressRoute pomocí prostředí PowerShell](expressroute-howto-routing-classic.md) pro další informace o filtrech směrování.

> [!NOTE]
> Pro společnost Microsoft a veřejného partnerského vztahu připojení musí používat veřejnou IP adresu vlastníkem hello zákazník nebo poskytovatel CSP a musí splňovat pravidla tooall definované. Další informace najdete v tématu hello [požadavky služby ExpressRoute](expressroute-prerequisites.md) stránky.  
> 
> 

## <a name="routing"></a>Směrování
ExpressRoute se připojuje toohello Azure sítěmi přes hello brány virtuální sítě Azure. Brány sítě poskytují směrování pro virtuální sítě Azure.

Vytváření virtuálních sítí Azure vytvoří také výchozí směrovací tabulka pro provoz toodirect vNet hello z hello podsítě virtuální sítě hello. Pokud hello výchozí směrovací tabulka je nedostatečná pro řešení hello vlastní trasy je možné vytvořit tooroute odchozí provoz toocustom zařízení nebo tooblock směruje toospecific podsítí či externích sítí.

### <a name="default-routing"></a>Výchozí směrování
Hello výchozí směrovací tabulka obsahuje hello následující trasy:

* Směrování v rámci podsítě
* Podsítě pro podsíť v rámci virtuální sítě hello
* toohello Internet
* Mezi virtuálními sítěmi s použitím brány VPN
* Z virtuální sítě do místní sítě s použitím brány VPN nebo brány ExpressRoute

![alternativní text](./media/expressroute-for-cloud-solution-providers/default-routing.png)  

### <a name="user-defined-routing-udr"></a>Směrování definované uživatelem (UDR)
Trasy definované uživatelem umožňují řízení hello provozu odchozí z hello přiřazené podsítě tooother podsítí ve virtuální síti hello nebo prostřednictvím některé z hello ostatních předdefinovaných bran (ExpressRoute; internet nebo VPN). Hello výchozí systémovou tabulku směrování můžete nahradit uživatelem definované směrovací tabulku, která nahradí výchozí směrovací tabulka hello vlastní trasy. S uživatelem definované směrování, můžete zákazníci vytvářet konkrétní trasy tooappliances, jako jsou brány firewall nebo zařízení pro detekci narušení nebo blokovat přístup toospecific podsítím z podsítě hello hostování hello trasy definované uživatelem. Přehled tras definovaných uživatelem najdete [tady](../virtual-network/virtual-networks-udr-overview.md). 

## <a name="security"></a>Zabezpečení
V závislosti na tom, které model se používá, připojit tooor Connect-Through zákazník definuje zásady zabezpečení hello ve své virtuální síti nebo poskytuje požadavky zásad zabezpečení hello toohello CSP toodefine tootheir virtuální sítě. je možné definovat Hello následující kritéria zabezpečení:

1. **Izolace zákazníka** – hello platformy Azure poskytuje možnost izolace zákazníka a to uložením informace o ID zákazníka a virtuální sítě v zabezpečené databázi, která je použité tooencapsulate veškerého provozu zákazníka v tunelových propojeních GRE.
2. **Skupina zabezpečení (NSG) sítě** pravidel slouží k definování povoleného provoz do a z hello podsítí v rámci virtuálních sítí v Azure. Ve výchozím nastavení hello NSG obsahují bloku pravidla tooblock provoz z hello Internet toohello virtuální sítě a pravidla povolit pro provoz v rámci virtuální sítě. Další informace o skupinách zabezpečení sítě najdete [tady](https://azure.microsoft.com/blog/network-security-groups/).
3. **Vynucené tunelování** – jedná se možnost tooredirect internet vázaný provoz z Azure toobe přesměrován prostřednictvím toohello připojení ExpressRoute hello na lokálním datovém centru. Další informace o vynuceném tunelovém propojení najdete [tady](expressroute-routing.md#advertising-default-routes).  
4. **Šifrování** – Přestože jsou okruhy ExpressRoute hello vyhrazené tooa konkrétního zákazníka, je hello možnost, že hello poskytovatele sítě může být nedodržení, umožňuje útočníka tooexamine paketu provoz. tooaddress, které tento potenciál, zákazník nebo poskytovatel CSP můžete šifrovat přenosy přes připojení hello definováním zásad režimu tunelového propojení protokolu IPSec pro veškerý provoz mezi hello na místní prostředky a prostředky Azure (viz. toohello volitelného režimu tunelového propojení protokolu IPSec pro zákazníka 1 na obrázku 5: zabezpečení ExpressRoute výše). Druhá možnost Hello by toouse zařízení brány firewall na každý koncový bod hello hello okruh ExpressRoute. To bude vyžadovat další 3. stran brány firewall toobe virtuálních počítačů či zařízení nainstalované na obou koncích tooencrypt hello provoz přes okruh ExpressRoute hello.

![alternativní text](./media/expressroute-for-cloud-solution-providers/expressroute-security.png)  

## <a name="next-steps"></a>Další kroky
Hello služba poskytovatele Cloud Solution Provider poskytuje způsob, jak tooincrease, hodnota tooyour zákazníky bez hello nutnost nákladné infrastruktury a schopností nákupy, při zachování vaší pozice hello primárního poskytovatele outsourcingu. Bezproblémovou integraci s Microsoft Azure je možné zajistit prostřednictvím hello rozhraní API CSP, což vám toointegrate správy ve službě Microsoft Azure v rámci vašimi stávajícími strukturami správy.  

Další informace naleznete na následující odkazy hello:

[Program Microsoft Cloud Solution Provider](https://partner.microsoft.com/en-US/Solutions/cloud-reseller-overview).  
[Získat připravené tootransact jako poskytovatele cloudových řešení](https://partner.microsoft.com/en-us/solutions/cloud-reseller-pre-launch).  
[Prostředky pro poskytovatele Microsoft Cloud Solution Provider](https://partner.microsoft.com/en-us/solutions/cloud-reseller-resources).

