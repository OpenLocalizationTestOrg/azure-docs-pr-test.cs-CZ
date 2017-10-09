---
title: okruhy aaaMoving ExpressRoute z classic tooResource Manager | Microsoft Docs
description: "Tato stránka obsahuje přehled toho, co budete potřebovat tooknow o přemostění hello classic a modelu nasazení Resource Manager hello."
documentationcenter: na
services: expressroute
author: ganesr
manager: carmonm
editor: 
ms.assetid: bdf01217-1a98-4ec0-a08e-d84fd37f78af
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: ganesr
ms.openlocfilehash: c901d2cda01aec409b528d29fc937ac6afaea511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="moving-expressroute-circuits-from-hello-classic-toohello-resource-manager-deployment-model"></a>Přesun okruhů ExpressRoute z hello classic toohello modelu nasazení Resource Manager
Tento článek obsahuje přehled toho, co znamená toomove okruhu Azure ExpressRoute z hello classic toohello modelu nasazení Azure Resource Manager.

Můžete použít jedné ExpressRoute okruhu tooconnect toovirtual sítě, které jsou nasazené jak v modelu nasazení Resource Manager hello i hello classic. Okruh ExpressRoute, bez ohledu na to, jak je vytvořený, se nyní může propojit toovirtual sítí v obou modelech nasazení.

![Okruh ExpressRoute, který odkazuje toovirtual sítí v obou modelech nasazení](./media/expressroute-move/expressroute-move-1.png)

## <a name="expressroute-circuits-that-are-created-in-hello-classic-deployment-model"></a>Okruhy ExpressRoute, které jsou vytvořené v modelu nasazení classic hello
Okruhy ExpressRoute, které jsou vytvořené v modelu nasazení classic hello potřebovat přesunout toobe toohello nasazení modelu první tooenable připojení tooboth hello classic a hello Resource Manager modelech nasazení Resource Manager. Během přesunu připojení nedojde ke ztrátě nebo přerušení připojení. Všechna propojení okruhu do virtuální sítě v modelu nasazení classic hello (uvnitř stejné hello předplatného i mezi předplatnými) se zachovají.

Po přesunutí hello je úspěšně dokončit, hello okruh ExpressRoute vypadá, provede a pracuje úplně stejně jako okruh ExpressRoute, který byl vytvořen v modelu nasazení Resource Manager hello. Nyní můžete vytvořit připojení toovirtual sítí v modelu nasazení Resource Manager hello.

Po okruh ExpressRoute byl přesunutý toohello modelu nasazení Resource Manager, můžete spravovat životní cyklus hello hello okruhu ExpressRoute jenom pomocí modelu nasazení Resource Manager hello. To znamená, že můžete provádět operace, jako je přidání, aktualizace nebo odstranění partnerských vztahů, aktualizace vlastností okruhu (například šířky pásma, SKU a typu fakturace) a odstranění okruhů, pouze v modelu nasazení Resource Manager hello. Najdete v části toohello níže o okruzích, které jsou vytvořené v modelu nasazení Resource Manager hello další podrobnosti o tom, jak můžete spravovat přístup tooboth nasazení modelů.

Nemáte tooinvolve vaše připojení poskytovatele tooperform hello přesunout.

## <a name="expressroute-circuits-that-are-created-in-hello-resource-manager-deployment-model"></a>Okruhy ExpressRoute, které jsou vytvořené v modelu nasazení Resource Manager hello
Můžete povolit okruhy ExpressRoute, které jsou vytvořené v toobe modelu nasazení Resource Manager hello přístupné z obou modelů nasazení. Jakýkoli okruh ExpressRoute v rámci vašeho předplatného, může být povoleno toobe k němu přistupovat z obou modelů nasazení.

* Okruhy ExpressRoute, které byly vytvořeny v modelu nasazení Resource Manager hello nemají přístup k modelu nasazení classic toohello ve výchozím nastavení.
* Okruhy ExpressRoute, které byly přesunuté z modelu nasazení classic hello, toohello model nasazení Správce prostředků jsou přístupné z obou modelů nasazení ve výchozím nastavení.
* Okruh ExpressRoute má vždy přístup toohello modelu nasazení Resource Manager, bez ohledu na to, jestli byl vytvořený v modelu nasazení classic nebo hello Resource Manager. To znamená, které můžete vytvořit připojení toovirtual sítě vytvořené v hello modelu nasazení Resource Manager podle pokynů na [jak virtuální sítě toolink](expressroute-howto-linkvnet-arm.md).
* Řídí přístup k modelu nasazení classic toohello hello **allowClassicOperations** parametr v hello okruh ExpressRoute.

> [!IMPORTANT]
> Všechny kvóty, které jsou popsané na hello [omezení služby](../azure-subscription-service-limits.md) platí. Jako příklad jsou standardní okruh může mít maximálně 10 připojení virtuální sítě odkazy/napříč hello classic a modelu nasazení Resource Manager hello.
> 
> 

## <a name="controlling-access-toohello-classic-deployment-model"></a>Řízení přístupu k modelu nasazení classic toohello
Můžete povolit jednom ExpressRoute okruhu toolink toovirtual sítí v obou modelech nasazení nastavení hello **allowClassicOperations** parametr hello okruh ExpressRoute.

Nastavení **allowClassicOperations** tooTRUE vám umožní toolink virtuální sítě z obou toohello modely nasazení okruh ExpressRoute. Můžete se propojit toovirtual sítí v modelu nasazení classic hello podle pokynů v [jak hello toolink virtuálních sítí v modelu nasazení classic](expressroute-howto-linkvnet-classic.md). Můžete se propojit toovirtual sítí v modelu nasazení Resource Manager hello podle pokynů v [jak hello toolink virtuálních sítí v modelu nasazení Resource Manager](expressroute-howto-linkvnet-arm.md).

Nastavení **allowClassicOperations** tooFALSE zablokujete přístup toohello obvodu z modelu nasazení classic hello. Nicméně všechna propojení virtuálních sítí v modelu nasazení classic hello zachovány. V takovém případě není zobrazená v modelu nasazení classic hello hello okruh ExpressRoute.

## <a name="supported-operations-in-hello-classic-deployment-model"></a>Podporované operace v modelu nasazení classic hello
Hello následující klasické operace jsou podporovány v ExpressRoute okruhu při **allowClassicOperations** nastavena tooTRUE:

* Získání informací o okruhu ExpressRoute
* Vytvoření nebo aktualizace, získání nebo odstranění virtuální sítě odkazy tooclassic virtuální sítě
* Vytvoření, aktualizace, získání nebo odstranění autorizací propojení virtuální sítě pro připojení mezi předplatnými

Nelze provést hello následující klasické operace při **allowClassicOperations** nastavena tooTRUE:

* Vytvoření, aktualizace, získání nebo odstranění partnerských vztahů protokolu BGP pro soukromé partnerské vztahy Azure, veřejné partnerské vztahy Azure a partnerské vztahy Microsoftu
* Odstranění okruhů ExpressRoute

## <a name="communication-between-hello-classic-and-hello-resource-manager-deployment-models"></a>Komunikace mezi hello classic a modelu nasazení Resource Manager hello
Hello okruh ExpressRoute pracuje jako most mezi hello classic a modelu nasazení Resource Manager hello. Přenosy dat mezi virtuálními ve virtuálních sítích v modelu nasazení classic hello a těmi ve virtuálních sítích v hello Resource Manager modelu nasazení prochází ExpressRoute, pokud jsou obě virtuální sítě propojené toohello stejnému okruhu ExpressRoute.

Agregační propustnost je omezená kapacitou propustnosti hello hello brány virtuální sítě. Provoz nevstupuje sítí poskytovatele připojení hello nebo do vašich sítí v takových případech. Tok přenosů mezi virtuálními sítěmi hello je plně obsažen v rámci sítě Microsoft hello.

## <a name="access-tooazure-public-and-microsoft-peering-resources"></a>Přístup k tooAzure veřejného partnerského vztahu a partnerského vztahu zdrojů společnosti Microsoft
Můžete pokračovat v tooaccess prostředky, které jsou obvykle přístupné prostřednictvím veřejného partnerského vztahu Azure a partnerský vztah Microsoftu bez přerušení.  

## <a name="whats-supported"></a>Co je podporováno
Tato část popisuje, co je podporováno pro okruhy ExpressRoute:

* Můžete použít jedné ExpressRoute okruhu tooaccess virtuální sítě, které jsou nasazeny v modelu nasazení Resource Manager hello i hello classic.
* Z klasického toohello hello modelu nasazení Resource Manager můžete přesunout okruh ExpressRoute. Po přesunutí, hello okruh ExpressRoute vypadá, funguje a pracuje stejně jako kterýkoli jiný okruh ExpressRoute, který je vytvořen v modelu nasazení Resource Manager hello.
* Můžete přesunout jenom okruh ExpressRoute hello. Pomocí této operace nejde přesunout propojení okruhu, virtuální sítě a brány VPN.
* Po okruh ExpressRoute byl přesunutý toohello modelu nasazení Resource Manager, můžete spravovat životní cyklus hello hello okruhu ExpressRoute jenom pomocí modelu nasazení Resource Manager hello. To znamená, že můžete provádět operace, jako je přidání, aktualizace nebo odstranění partnerských vztahů, aktualizace vlastností okruhu (například šířky pásma, SKU a typu fakturace) a odstranění okruhů, pouze v modelu nasazení Resource Manager hello.
* Hello okruh ExpressRoute pracuje jako most mezi hello classic a modelu nasazení Resource Manager hello. Přenosy dat mezi virtuálními ve virtuálních sítích v modelu nasazení classic hello a těmi ve virtuálních sítích v hello Resource Manager modelu nasazení prochází ExpressRoute, pokud jsou obě virtuální sítě propojené toohello stejnému okruhu ExpressRoute.
* Připojení mezi předplatnými je podporováno v hello classic i hello modelech nasazení Resource Manager.
* Po přesunutí okruhu ExpressRoute z modelu Azure Resource Manager toohello hello klasického modelu, můžete [migrovat hello virtuální sítě propojené toohello okruh ExpressRoute](expressroute-migration-classic-resource-manager.md).

## <a name="whats-not-supported"></a>Co není podporováno
Tato část popisuje, co není podporováno pro okruhy ExpressRoute:

* Správa hello životní cyklus okruhu ExpressRoute z modelu nasazení classic hello.
* Na základě rolí podpora řízení přístupu (RBAC) pro model nasazení classic hello. Nelze provést RBAC ovládací prvky tooa okruh v modelu nasazení classic hello. Libovolný správce nebo spolusprávce předplatného hello můžete propojit nebo zrušit propojení okruhu toohello virtuální sítě.

## <a name="configuration"></a>Konfigurace
Postupujte podle pokynů hello, které jsou popsány v [přesun okruhu ExpressRoute z modelu nasazení Resource Manager classic toohello hello](expressroute-howto-move-arm.md).

## <a name="next-steps"></a>Další kroky
* [Migrovat hello virtuální sítě propojené toohello okruh ExpressRoute z modelu hello klasického modelu toohello Azure Resource Manager](expressroute-migration-classic-resource-manager.md)
* Informace o pracovním postupu najdete v tématu [Pracovní postupy zřizování okruhů ExpressRoute a stavy okruhu](expressroute-workflows.md).
* tooconfigure připojení ExpressRoute:
  
  * [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md)
  * [Konfigurace směrování](expressroute-howto-routing-arm.md)
  * [Propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-arm.md)

