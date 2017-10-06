---
title: "Nastavit filtry směrování pro partnerský vztah Azure ExpressRoute Microsoft: portál | Microsoft Docs"
description: "Tento článek popisuje, jak hello tooconfigure filtry tras pro Microsoft Peering pomocí portálu Azure"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 2a47d465ec5f175d9510cef94606f70f036f0862
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a>Konfigurace filtrů směrování pro partnerský vztah Microsoftu

Filtry tras jsou způsob tooconsume podmnožinu podporované služby prostřednictvím partnerského vztahu Microsoftu. Hello kroky v této nápovědě článku můžete nakonfigurovat a spravovat filtry tras pro okruhy ExpressRoute.

Dynamics 365 služeb a služeb Office 365 například Exchange Online, SharePoint Online a Skype pro firmy, jsou přístupné prostřednictvím partnerského vztahu Microsoftu hello. Pokud partnerský vztah Microsoftu je nakonfigurován v okruhu ExpressRoute, jsou všechny služby související toothese předpony inzerované prostřednictvím hello relací protokolu BGP, které jsou vytvořeny. Hodnota komunity protokolu BGP je připojený tooevery předponu tooidentify hello služba, která je nabízeným přes hello předponu. Seznam hodnot komunity protokolu BGP hello a hello služeb jsou mapovány najdete v tématu [komunit protokolu BGP](expressroute-routing.md#bgp).

Pokud budete potřebovat připojení tooall služeb, jsou velký počet předpon inzerovaných prostřednictvím protokolu BGP. Tím se výrazně zvyšuje velikost hello hello směrovací tabulky udržuje pomocí směrovačů v rámci vaší sítě. Pokud máte v plánu tooconsume pouze podmnožinu službám nabízeným přes partnerské vztahy Microsoft, můžete snížit velikost hello vaší směrovací tabulky dvěma způsoby. Můžete:

- Filtrovat nežádoucí předpony použitím filtry tras na komunit protokolu BGP. Tento postup standardní sítě a běžně používá v rámci mnoho sítí.

- Definovat filtry tras a použít tooyour okruh ExpressRoute. Filtr trasy je nový prostředek, který vám umožní vybrat hello seznam služeb můžete naplánovat tooconsume prostřednictvím partnerského vztahu Microsoftu. ExpressRoute směrovače odeslat pouze hello seznam předpon, které patří služby toohello stanovených ve filtru hello trasy.

### <a name="about"></a>O filtrech směrování

Pokud partnerský vztah Microsoftu je nakonfigurován na váš okruh ExpressRoute, hello Microsoft hraniční směrovače vytvořte dvojici relací protokolu BGP s hello hraniční směrovače (váš nebo váš poskytovatel připojení). Trasy se inzerovaný tooyour sítě. tooenable trasy oznámení o inzerovaném programu tooyour sítě, je nutné přidružit filtr trasy.

Filtr trasy umožňuje identifikovat služby, které mají tooconsume prostřednictvím partnerského vztahu Microsoftu okruhu ExpressRoute. Je to v podstatě prázdný seznam všech hodnot komunity protokolu BGP hello. Prostředek trasy filtru je definovaný a připojené tooan okruh ExpressRoute, budete všech předpon, které mapují hodnoty komunity protokolu BGP toohello inzerovaný tooyour sítě.

filtry tras toobe možné tooattach se službami Office 365 na ně, musí mít služby Office 365 tooconsume autorizace prostřednictvím ExpressRoute. Pokud si nejste oprávnění tooconsume Office 365 služby prostřednictvím ExpressRoute, filtry tras tooattach hello operace selže. Další informace o procesu autorizačního hello najdete v tématu [Azure ExpressRoute pro Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd). 365 služeb připojení k tooDynamics nevyžaduje žádné předchozí autorizace.

> [!IMPORTANT]
> Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím partnerského vztahu Microsoftu, i když nejsou definovány filtry tras. Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh.
> 
> 

### <a name="workflow"></a>Pracovní postup

možnost toosuccessfully toobe připojit tooservices prostřednictvím partnerského vztahu Microsoftu, je třeba provést následující kroky konfigurace hello:

- Musí mít aktivní okruh ExpressRoute, který má Microsoft partnerský vztah zřízené. Následující pokyny tooaccomplish hello můžete použít tyto úlohy:
  - [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat. Hello okruh ExpressRoute musí být ve stavu zřízený a povolený.
  - [Vytvoření partnerského vztahu Microsoftu](expressroute-howto-routing-portal-resource-manager.md) Pokud spravujete hello přímo v relaci protokolu BGP. Nebo váš poskytovatel připojení zřídit partnerský vztah Microsoftu pro váš okruh.

-  Musíte vytvořit a konfigurovat filtr trasy.
    - Identifikovat hello služby services s tooconsume prostřednictvím partnerského vztahu Microsoftu
    - Identifikovat hello seznam hodnot komunity protokolu BGP souvisejících se službou hello
    - Vytvoření pravidlo tooallow hello předponu seznamu odpovídající hello hodnotami komunity protokolu BGP

-  Je nutné připojit okruh ExpressRoute toohello filtr trasy hello.

## <a name="before-you-begin"></a>Než začnete

Před zahájením konfigurace, ujistěte se, že splňujete hello následující kritéria:

 - Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.

 - Musí mít aktivní okruh ExpressRoute. Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat. Hello okruh ExpressRoute musí být ve stavu zřízený a povolený.

 - Musíte mít active partnerský vztah Microsoftu. Postupujte podle pokynů v [vytvoření a úprava konfigurace partnerského vztahu](expressroute-howto-routing-portal-resource-manager.md)


## <a name="prefixes"></a>Krok 1. Získat seznam předpon a hodnotami komunity protokolu BGP

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Získat seznam hodnot komunity protokolu BGP

Hodnoty komunity protokolu BGP souvisejících se službou přístupné prostřednictvím partnerského vztahu Microsoftu je k dispozici v hello [požadavky na směrování služby ExpressRoute](expressroute-routing.md) stránky.

### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a>2. Zkontrolujte seznam hello hodnot, které chcete toouse

Zkontrolujte seznam hodnotami komunity protokolu BGP, které chcete toouse ve filtru hello trasy. Jako příklad je hello hodnota komunity protokolu BGP pro služby Dynamics 365 12076:5040.

## <a name="filter"></a>Krok 2. Vytvořte filtr trasy a pravidlo filtru

Filtr trasy může mít pouze jedno pravidlo a hello pravidlo musí být typu 'Povolit'. Toto pravidlo může mít seznam hodnot komunity protokolu BGP s ním spojená.

### <a name="1-create-a-route-filter"></a>1. Vytvoření filtru trasy
Filtr trasy můžete vytvořit tak, že vyberete možnost toocreate hello nový prostředek. Klikněte na tlačítko **nový** > **sítě** > **RouteFilter**, jak ukazuje následující obrázek hello:

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\CreateRouteFilter1.png)

Je nutné umístit hello trasy filtru ve skupině prostředků. 

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a>2. Vytvořit pravidlo filtru

Můžete přidat a spravovat pravidla aktualizací výběrem hello karta pravidlo filtru trasy.

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\ManageRouteFilter.png)


Můžete vybrat hello služby, které mají tooconnect toofrom hello rozevíracím seznamu a uložíte pravidlo hello po dokončení.

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\AddRouteFilterRule.png)


## <a name="attach"></a>Krok 3. Připojte okruh ExpressRoute tooan filtr trasy hello

Vyberte tlačítko "Přidat okruh" hello a tlačítko hello okruh ExpressRoute z hello rozevíracího seznamu můžete připojit hello trasy filtru tooa okruh.

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\AddCktToRouteFilter.png)

## <a name="getproperties"></a>Vlastnosti hello tooget filtru trasy

Při otevření prostředku hello hello portálu, můžete zobrazit vlastnosti filtr trasy.

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\ViewRouteFilter.png)


## <a name="updateproperties"></a>Vlastnosti hello tooupdate filtru trasy

Výběrem tlačítka hello "Správa pravidlo" můžete aktualizovat seznam hello okruhu připojené tooa hodnoty komunity protokolu BGP.


![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\ManageRouteFilter.png)

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\AddRouteFilterRule.png) 


## <a name="detach"></a>toodetach filtr trasy z okruhu ExpressRoute

toodetach okruhu z hello trasy filtru, klikněte pravým tlačítkem na okruh hello a klikněte na "zrušit přidružení".

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\DetachRouteFilter.png) 


## <a name="delete"></a>toodelete filtr trasy

Výběrem hello tlačítko Odstranit odstraníte filtr trasy. 

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\DeleteRouteFilter.png) 

## <a name="next-steps"></a>Další kroky

Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
