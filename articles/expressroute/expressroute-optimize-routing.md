---
title: "Optimalizace směrování ExpressRoute: Azure | Dokumentace Microsoftu"
description: "Tato stránka poskytuje podrobnosti o tom, jak toooptimize směrování, pokud máte více než jeden ExpressRoute okruhy, připojení mezi Microsoftem a podnikovou sítí."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
ms.assetid: fca53249-d9c3-4cff-8916-f8749386a4dd
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/06/2017
ms.author: charwen
ms.openlocfilehash: ebcfb638f67a9ac78c3e476668bfd0bb0ffb9985
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-expressroute-routing"></a>Optimalizace směrování ExpressRoute
Pokud máte víc okruhů ExpressRoute, máte více než jeden tooMicrosoft tooconnect cesta. V důsledku toho může dojít, neoptimální směrování – tedy provozu může trvat delší cestu tooreach společnosti Microsoft a tooyour sítě společnosti Microsoft. Hello delší hello síťovou cestu, vyšší latence hello hello. Latence má přímý vliv na výkon aplikací a činnost koncového uživatele. Tento článek popíše tento problém, který se popisují, jak hello toooptimize směrování pomocí standardních technologií směrování.

## <a name="suboptimal-routing-from-customer-toomicrosoft"></a>Neoptimální směrování z tooMicrosoft zákazníka
Podívejme se na příkladu zblízka na problém směrování hello. Představte si, že máte dvě pobočky v hello USA, jednu v Los Angeles a jednu v New Yorku. Vaše pobočky jsou připojené k síti WAN, což může být buď vaše páteřní síti, nebo virtuální privátní síť IP poskytovatele služeb. Máte dva okruhy ExpressRoute, jeden v oblasti USA – západ a druhý v oblasti USA – východ, které jsou také připojené hello sítě WAN. Zjevně máte dvě cesty tooconnect toohello Microsoft sítě. Nyní si představte, že máte nasazení Azure (například Azure App Service) v oblasti USA – západ i USA – východ. Vaším záměrem je tooconnect vaši uživatelé v Los Angeles tooAzure USA – západ a uživatele z New Yorku tooAzure USA – východ, protože správce služby inzeruje, že každý office přístupu uživatelům z hello nedaleko Azure services pro činnost optimální. Bohužel hello plán funguje dobře pro uživatele na východním pobřeží hello, ale ne pro uživatele na západním pobřeží hello. Hello příčinu problému hello je následující hello. V každém okruhu ExpressRoute inzerujeme tooyou hello předponu v Azure USA – východ (23.100.0.0/16) i hello předponu v Azure USA – západ (13.100.0.0/16). Pokud si nejste jisti, která předpona je z které oblasti, nejste možné tootreat ho jinak. Vaše síť WAN může vezměte v úvahu obě hello předpony jsou blíž tooUS – východ než USA – západ a proto směruje uživatele toohello i office okruh ExpressRoute v oblasti USA – východ. V hello end budete mít v hello office Los Angeles mnoho nespokojených uživatelů.

![Problém ExpressRoute případ 1 - neoptimální směrování z tooMicrosoft zákazníka](./media/expressroute-optimize-routing/expressroute-case1-problem.png)

### <a name="solution-use-bgp-communities"></a>Řešení: Použití komunit protokolu BGP
toooptimize směrování pro uživatele obou poboček, budete potřebovat tooknow, která předpona je z Azure USA – západ a která z Azure USA – východ. Tyto informace kódujeme pomocí [hodnot komunity protokolu BGP](expressroute-routing.md). Přiřadili jsme tooeach jedinečnou hodnotu komunity protokolu BGP oblast Azure, například 12076:51004 pro USA – východ, 12076:51006 pro USA – západ. Teď, když už víte, které předpona je z které oblasti Azure, můžete nakonfigurovat, který okruh ExpressRoute se bude upřednostňovat. Vzhledem k tomu, že používáme hello informací o tooexchange směrování protokolu BGP, můžete použít směrování tooinfluence hodnotu Local Preference protokolu BGP. V našem příkladu můžete přiřadit too13.100.0.0/16 hodnotu vyšší hodnotu local preference v USA – západ než v USA – východ a obdobně vyšší too23.100.0.0/16 hodnota hodnotu local preference v oblasti USA – východ než USA – západ. Tato konfigurace zajistí, že když jsou k dispozici obě cesty tooMicrosoft, uživatelé v Los Angeles učiní hello okruh ExpressRoute v oblasti USA – západ tooconnect tooAzure USA – západ zatímco uživatelé v New Yorku trvat hello ExpressRoute v oblasti USA – východ tooAzure USA – východ . Směrování je optimalizované na obou stranách. 

![Případ 1 ExpressRoute – Řešení: Použití komunit protokolu BGP](./media/expressroute-optimize-routing/expressroute-case1-solution.png)

> [!NOTE]
> Dobrý den stejným způsobem, pomocí hodnotu Local Preference, může být použité toorouting z zákazníka tooAzure virtuální sítě. Jsme nemáte označit komunity protokolu BGP hodnota toohello předpon inzerovaných z Azure tooyour sítě. Ale vzhledem k tomu, že znáte, což při nasazení virtuální sítě je zavřít toowhich sady Office, můžete nakonfigurovat směrovače odpovídajícím způsobem tooprefer jeden tooanother typu okruhu ExpressRoute.
>
>

## <a name="suboptimal-routing-from-microsoft-toocustomer"></a>Neoptimální směrování z Microsoft toocustomer
Zde je další příklad kde připojení z Microsoftu trvat delší cestu tooreach vaší sítě. V tomto případě používáte místní servery Exchange a Exchange Online v [hybridním prostředí](https://technet.microsoft.com/library/jj200581%28v=exchg.150%29.aspx). Vaše pobočky jsou připojené tooa sítě WAN. Inzerujete předpony hello místní serverů v obou vaší pobočky tooMicrosoft prostřednictvím hello dva okruhy ExpressRoute. Exchange Online bude inicializovat místními servery toohello připojení v případech, jako je například přenesení poštovní schránky. Bohužel hello připojení tooyour office Los Angeles se směruje toohello okruh ExpressRoute v oblasti USA – východ před procházení hello celý obsah přenášel back toohello západním pobřeží. Hello příčinou problému hello je podobné toohello první z nich. Bez jakékoli pomoci namůže síť Microsoftu hello zjistit, která předpona zákazníka je zavřít tooUS – východ a které z nich je, zavřete tooUS – západ. Odehrává se toopick hello cesty je chybný tooyour office v Los Angeles.

![ExpressRoute případ 2 - neoptimální směrování z Microsoft toocustomer](./media/expressroute-optimize-routing/expressroute-case2-problem.png)

### <a name="solution-use-as-path-prepending"></a>Řešení: Použití předřazení AS PATH
Existují dvě řešení toohello problém. Hello nejdřív jednu je, že budete jednoduše inzerovat vaši místní předponu pro office Los Angeles, 177.2.0.0/31, v okruhu ExpressRoute hello v oblasti USA – západ a místní předponu pro vaše v New Yorku, 177.2.0.2/31, v hello okruh ExpressRoute v oblasti USA – východ. V důsledku toho není k dispozici jenom jednu cestu pro Microsoft tooconnect tooeach vašich pobočkách. Nedochází k nejednoznačnosti a směrování je optimalizované. Tento návrh je nutné toothink o strategie převzetí služeb při selhání. V hello je porušený událost, která hello cesta tooMicrosoft přes ExpressRoute, musíte toomake jistotu, že Exchange Online se pořád připojit tooyour na místní servery. 

Hello druhým řešením je, že pokračovat tooadvertise předpon hello v obou okruzích ExpressRoute a kromě toho nám dáte pokyn, která předpona je zavřít toowhich jeden z poboček. Protože podporujeme předřazení protokolu BGP AS Path, můžete nakonfigurovat hello AS Path pro směrování tooinfluence předponu. V tomto příkladu lze prodloužit hello AS PATH pro 172.2.0.0/31 v oblasti USA – východ, takže jsme se raději hello okruh ExpressRoute v oblasti USA – západ pro přenos dat určený pro tuto předponu (protože naše síť si bude myslet, že předpona toothis cesta hello je kratší in – západ hello). Obdobně lze prodloužit hello AS PATH pro 172.2.0.2/31 v oblasti USA – Západ, abychom preferovali okruh ExpressRoute hello v oblasti USA – východ. Směrování je optimalizované pro obě pobočky. Pokud v tomto návrhu jeden okruh ExpressRoute není funkční, Exchange Online se s vámi pořád může spojit prostřednictvím jiného okruhu ExpressRoute a vaší sítě WAN. 

> [!IMPORTANT]
> Odebereme soukromá jako čísla v hello AS PATH pro předpony hello přijaté na Peering společnosti Microsoft. Je třeba tooappend veřejná čísla AS v hello směrování tooinfluence AS PATH pro Peering společnosti Microsoft.
> 
> 

![Případ 2 ExpressRoute – Řešení: Použití předřazení AS PATH](./media/expressroute-optimize-routing/expressroute-case2-solution.png)

> [!NOTE]
> Zde uvedené příklady hello jsou pro společnost Microsoft a veřejných partnerských vztahů, jsme podporují hello stejné funkce pro hello soukromého partnerského vztahu. Navíc předřazení AS Path hello funguje v rámci jednoho jeden okruh ExpressRoute, tooinfluence hello výběr hello primární a sekundární cesty.
> 
> 

## <a name="suboptimal-routing-between-virtual-networks"></a>Neoptimální směrování mezi virtuálními sítěmi
Pomocí ExpressRoute, můžete povolit virtuální sítě tooVirtual sítě (která je také označována jako "Virtuální sítě") komunikace jejich propojováním tooan okruh ExpressRoute. Při propojení je toomultiple okruhy ExpressRoute, neoptimálnímu směrování může dojít mezi hello virtuální sítě. Zvažte následující příklad. Máte dva okruhy ExpressRoute, jeden v oblasti USA – západ a druhý v oblasti USA – východ. V každé oblasti máte dvě virtuální sítě. V jedné virtuální sítě nasazených webových serverů a aplikačních serverů v jiných hello. Pro redundanci můžete odkaz hello dvou virtuálních sítí v každé oblasti tooboth hello místní okruh ExpressRoute a hello vzdálené okruh ExpressRoute. Jak je vidět níže, v každé virtuální sítě existují dvě cesty toohello jiné virtuální sítě. Hello virtuálních sítí neznáte, který okruh ExpressRoute je místní a která je vzdálený. V důsledku stejně jako rovná náklady více cesta (ECMP) směrování tooload vyrovnávání přenosů mezi virtuálními, některé tok přenosů dat bude trvat delší cestu hello a získat směruje na okruh ExpressRoute vzdálené hello.

![Případ 3 ExpressRoute – Neoptimální směrování mezi virtuálními sítěmi](./media/expressroute-optimize-routing/expressroute-case3-problem.png)

### <a name="solution-assign-a-high-weight-toolocal-connection"></a>Řešení: přiřaďte vysokou váhu toolocal připojení
Hello řešení je jednoduchá. Vzhledem k tomu, že znáte, kde jsou hello virtuální sítě a hello okruhů, můžete nám říct které cesty každý virtuální síť by měla přednost. Speciálně pro tento příklad přiřadit vyšší váhou toohello místní připojení než toohello vzdáleného připojení (viz příklad konfigurace hello [sem](expressroute-howto-linkvnet-arm.md#modify-a-virtual-network-connection)). Když virtuální síť, která obdrží hello předpona hello jiné virtuální sítě na víc připojení ho s hello nejvyšší váhy toosend provoz určený pro tuto předponu preferovali hello připojení.

![Řešení ExpressRoute případ 3 - přiřazení vysokou váhu toolocal připojení](./media/expressroute-optimize-routing/expressroute-case3-solution.png)

> [!NOTE]
> Můžete také ovlivnit směrování z virtuální sítě tooyour místní sítě, pokud máte víc okruhů ExpressRoute, tím, že nakonfigurujete váhy hello připojení místo použití AS PATH předřazení techniku popsanou v hello druhý scénář výše. Pro každou předponu vždy Podíváme se na váhy připojení hello před hello AS Path délka při rozhodování o tom, jak toosend provoz.
>
>
