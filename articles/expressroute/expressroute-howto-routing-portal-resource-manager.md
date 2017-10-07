---
title: "Jak tooconfigure směrování pro okruh ExpressRoute (partnerský vztah): Resource Manager: Azure | Microsoft Docs"
description: "Tento článek vás provede kroky hello pro vytváření a zřizování hello soukromého a veřejného a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak stav hello toocheck, aktualizace nebo odstranění partnerských vztahů pro váš okruh."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: a8ca25f488dde728cb3b06cd2c91873f3069feaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Vytvářet a upravovat partnerský vztah pro okruh ExpressRoute

Tento článek pomůže při vytváření a správě konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager hello pomocí hello portálu Azure. Můžete také zkontrolovat stav hello, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute. Pokud chcete toouse toowork jinou metodu s váš okruh, vyberte článek z hello následující seznamu:


## <a name="configuration-prerequisites"></a>Předpoklady konfigurace

* Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md) stránce hello [požadavky na směrování](expressroute-routing.md) stránku a hello [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Musí mít aktivní okruh ExpressRoute. Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat. Hello okruh ExpressRoute musí být ve stavu zřízený a povolený pro vás toobe možné toorun hello rutiny v dalších částech hello.
* Pokud máte v plánu toouse sdílené hodnota hash klíče s algoritmem MD5, být jisti toouse to na obou stranách hello tunelové propojení a limit hello počet znaků tooa maximálně 25.

Tyto pokyny platí pouze toocircuits vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení konfiguruje a spravuje směrování. 

> [!IMPORTANT]
> Jsme aktuálně neprovádějí ohlášení partnerské vztahy nakonfigurované poskytovateli služeb prostřednictvím portálu pro správu služby hello. Pracujeme na tom, aby tato možnost brzy fungovala. Před konfigurací partnerských vztahů protokolu BGP, zkontrolujte u vašeho poskytovatele služeb.
> 
> 

Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute. Partnerské vztahy můžete konfigurovat v libovolném pořadí. Přesvědčte se, že dokončení hello konfiguraci každého partnerského vztahu jeden po druhém.

## <a name="azure-private-peering"></a>Soukromý partnerský vztah Azure

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello privátní konfigurace partnerského vztahu Azure pro okruh ExpressRoute.

### <a name="toocreate-azure-private-peering"></a>toocreate soukromého partnerského vztahu Azure

1. Nakonfigurujte okruh ExpressRoute hello. Ujistěte se, že hello okruh plně zřízený poskytovatelem připojení hello než budete pokračovat.

  ![seznam](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Nakonfigurujte soukromý partnerský vztah Azure pro okruh hello. Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:

  * / 30 pro primární propojení hello podsítě. Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
  * / 30 pro sekundární propojení hello podsítě. Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
  * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS. Pro tento partnerský vztah můžete použít soukromé číslo AS. Zkontrolujte, že nepoužíváte 65515.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.
3. Vyberte řádek Azure privátní partnerský vztah hello, jak je znázorněno v hello následující ukázka:

  ![Privátní](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Nakonfigurujte soukromý partnerský vztah. Hello následující obrázek ukazuje příklad konfigurace:

  ![Nakonfigurujte soukromý partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Uložte konfiguraci hello po zadání všech parametrů. Po hello konfigurace úspěšně přijatá, zobrazí se něco podobné toohello následující ukázka:

  ![Uložit soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooview-azure-private-peering-details"></a>tooview Azure privátní partnerský vztah podrobnosti

Můžete zobrazit vlastnosti hello soukromého partnerského vztahu Azure výběrem partnerského vztahu hello.

![Zobrazit soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate konfigurace soukromého partnerského vztahu Azure

Můžete vybrat hello řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu hello.

![Aktualizovat soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="toodelete-azure-private-peering"></a>toodelete soukromého partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění hello, jak ukazuje následující obrázek hello:

![Odstranění soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a>Veřejný partnerský vztah Azure

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello veřejné konfigurace partnerského vztahu Azure pro okruh ExpressRoute.

### <a name="toocreate-azure-public-peering"></a>toocreate veřejného partnerského vztahu Azure

1. Nakonfigurujte okruh ExpressRoute. Zkontrolujte, zda text hello okruh plně zřízený poskytovatelem připojení hello než budete dál pokračovat.

  ![seznam veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Nakonfigurujte veřejný partnerský vztah Azure pro okruh hello. Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:

  * / 30 pro primární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy.
  * / 30 pro sekundární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy.
  * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.
3. Vyberte hello řádek Azure veřejný partnerský vztah, jak ukazuje následující obrázek hello:

  ![Vyberte řádek pro veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Nakonfigurujte veřejný partnerský vztah. Hello následující obrázek ukazuje příklad konfigurace:

  ![Nakonfigurujte veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Uložte konfiguraci hello po zadání všech parametrů. Po hello konfigurace úspěšně přijatá, zobrazí se něco podobné toohello následující ukázka:

  ![Uložte konfiguraci veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooview-azure-public-peering-details"></a>tooview Azure veřejného partnerského vztahu podrobnosti

Můžete zobrazit vlastnosti hello veřejného partnerského vztahu Azure výběrem partnerského vztahu hello.

![Zobrazit vlastnosti veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooupdate-azure-public-peering-configuration"></a>tooupdate konfigurace veřejného partnerského vztahu Azure

Můžete vybrat hello řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu hello.

![Vyberte řádek pro veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="toodelete-azure-public-peering"></a>toodelete veřejného partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění hello, jak je znázorněno v hello následující ukázka:

![odstranění veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a>Partnerský vztah Microsoftu

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu hello Microsoftu pro okruh ExpressRoute.

> [!IMPORTANT]
> Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím hello partnerský vztah Microsoftu, i když nejsou definovány filtry tras. Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh. Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).
> 
> 

### <a name="toocreate-microsoft-peering"></a>partnerský vztah Microsoftu toocreate

1. Nakonfigurujte okruh ExpressRoute. Zkontrolujte, zda text hello okruh plně zřízený poskytovatelem připojení hello než budete dál pokračovat.

  ![seznam partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Nakonfigurujte partnerský vztah Microsoftu pro okruh hello. Ujistěte se, že máte hello následující informace, než budete pokračovat.

  * / 30 pro primární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
  * / 30 pro sekundární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
  * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
  * Inzerované předpony: musíte poskytnout seznam všech předpon plánování tooadvertise přes relaci protokolu BGP hello. Přijímají se jenom předpony veřejných IP adres. Pokud máte v plánu toosend sadu předpon, můžete odeslat seznam oddělený čárkami. Tyto předpony musí být registrovaný tooyou u rir / IRR.
  * **Volitelné -** zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované toohello číslo AS partnerského vztahu, můžete zadat hello jako číslo toowhich jsou registrované.
  * Název registru směrování: Můžete zadat hello RIR / IRR, u které hello jako číslo a předpony jsou registrované.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.
3. Můžete vybrat hello partnerský vztah chcete tooconfigure, jak je znázorněno v hello následující příklad. Vyberte řádek partnerského vztahu Microsoftu hello.

  ![Vyberte řádek partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Nakonfigurujte partnerský vztah Microsoftu. Hello následující obrázek ukazuje příklad konfigurace:

  ![Nakonfigurujte partnerský vztah Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Uložte konfiguraci hello po zadání všech parametrů.

  Pokud váš okruh dostane tooa 'potřeby ověřování, stavu (jak je znázorněno v bitové kopii hello), musíte otevřít tooshow lístek podpory, která je doklad vlastnictví tým podpory tooour předpony hello.

  ![Uložte konfiguraci partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  Lístek podpory můžete otevřít přímo z portálu hello, jak je znázorněno v hello následující ukázka:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. Po hello konfigurace úspěšně přijatá, zobrazí se něco podobné toohello následující bitové kopie:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="tooview-microsoft-peering-details"></a>tooview podrobností partnerského vztahu Microsoftu

Můžete zobrazit vlastnosti hello veřejného partnerského vztahu Azure výběrem partnerského vztahu hello.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="tooupdate-microsoft-peering-configuration"></a>konfiguraci partnerského vztahu Microsoftu tooupdate

Můžete vybrat hello řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu hello.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="toodelete-microsoft-peering"></a>partnerský vztah Microsoftu toodelete

Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění hello, jak ukazuje následující obrázek hello:

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a>Další kroky

Dalším krokem je [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
* Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).
* Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).
* Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).
