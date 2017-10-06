---
title: "Propojení virtuální sítě tooan okruh ExpressRoute: portálu Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak toolink virtuální sítě okruhů tooExpressRoute (virtuální sítě)."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8bedcb11df7e30281fd439afdfb76cc67626a8f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a>Připojit virtuální síť tooan okruh ExpressRoute
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-linkvnet-classic.md)
> 

Tento článek vám pomůže propojení virtuální sítě (virtuální sítě) okruhy ExpressRoute tooAzure pomocí modelu nasazení Resource Manager hello a hello portálu Azure. Virtuální sítě může být buď v hello stejného předplatného, nebo může být součást jiného předplatného.

## <a name="before-you-begin"></a>Než začnete
* Zkontrolujte hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Musí mít aktivní okruh ExpressRoute.
  
  * Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mít hello ho povolený vaším poskytovatelem připojení.
  * Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh. V tématu hello [konfigurace směrování](expressroute-howto-routing-portal-resource-manager.md) směrování pokyny najdete v článku.
  * Zajistěte, aby soukromý partnerský vztah Azure je nakonfigurován a hello partnerského vztahu protokolu BGP mezi vaší sítí a Microsoftem zapnutý tak, že povolíte připojení klient server.
  * Ujistěte se, že máte virtuální sítě a brány virtuální sítě vytvoří a plně zřízený. Postupujte podle pokynů hello příliš[vytvořit bránu virtuální sítě pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Brána virtuální sítě pro ExpressRoute používá hello GatewayType 'ExpressRoute' není síť VPN.

* Můžete propojit až too10 virtuální sítě tooa standardní okruh ExpressRoute. Všechny virtuální sítě musí být v hello stejné geopolitické oblasti při použití standardní okruh ExpressRoute. 
* Můžete propojit virtuální sítě mimo hello geopolitické oblasti hello okruh ExpressRoute nebo připojit větší počet virtuálních sítí tooyour okruh ExpressRoute, pokud jste povolili hello doplněk ExpressRoute premium. Zkontrolujte hello [– nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium hello.
* Můžete [zobrazit video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) před začátku toobetter pochopit hello kroky.

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>Připojit virtuální síť v hello stejnému okruhu tooa předplatného

### <a name="toocreate-a-connection"></a>toocreate připojení

> [!NOTE]
> Informace o konfiguraci protokolu BGP nezobrazí, pokud zprostředkovatel vrstvy 3 hello nakonfigurovaný vaší partnerských vztahů. Pokud váš okruh je ve stavu, zřízené, musí být schopný toocreate připojení.
>

1. Zkontrolujte, že stav okruhu ExpressRoute a soukromý partnerský vztah Azure úspěšně nakonfigurovaný. Postupujte podle pokynů hello v [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a [konfigurace směrování](expressroute-howto-routing-arm.md). Váš okruh ExpressRoute by měl vypadat podobně jako hello následující bitové kopie:

    ![Snímek obrazovky okruhu ExpressRoute](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. Nyní můžete spustit zřizování toolink připojení vaší virtuální síti Brána tooyour okruh ExpressRoute. Klikněte na tlačítko **připojení** > **přidat** tooopen hello **přidat připojení** okno a pak nakonfigurujte hello hodnoty.

    ![Přidat připojení – snímek obrazovky](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. Po úspěšném dokončení konfigurace připojení k vaší objekt připojení zobrazí hello informace pro připojení k hello.

     ![Snímek obrazovky připojení objektu](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="toodelete-a-connection"></a>toodelete připojení
Připojení můžete odstranit výběrem hello **odstranit** ikonu na hello okno pro připojení.

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Připojit virtuální síť v okruhu tooa jiném předplatném.
Okruh ExpressRoute můžete sdílet mezi více předplatných. Hello na následujícím obrázku jednoduchou schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Každý hello menší cloudy v rámci cloudu velké hello je použité toorepresent odběry, které patří toodifferent oddělení v organizaci.
- Každé oddělení hello v rámci organizace hello můžete použít vlastní předplatné nasazování služeb, ale můžou sdílet jeden back tooyour místní síť tooconnect okruhu ExpressRoute.
- Jednoho oddělení (v tomto příkladu: IT) můžete vlastní hello okruh ExpressRoute. Hello okruh ExpressRoute můžete použít jiné odběry v rámci organizace hello.

    > [!NOTE]
    > Připojení a šířku pásma poplatky za hello vyhrazené okruhu bude použité toohello vlastníka okruhu ExpressRoute. Všechny virtuální sítě sdílejí hello stejné šířky pásma.
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a>Správa - vlastníky okruhu a okruh uživatelů

Hello 'vlastníka okruhu, je oprávněný uživatel napájení z hello prostředků okruhu ExpressRoute. vlastníka okruhu Hello můžete vytvořit autorizací, které lze uplatnit podle 'okruh uživatele'. Vlastníci virtuální sítě jsou uživatelé okruh brány, které nejsou uvnitř hello stejného předplatného jako hello okruh ExpressRoute. Uživatelé okruhu můžete uplatnit autorizací (jedním autorizačním na jednu virtuální síť).

vlastníka okruhu Hello má hello power toomodify a odvolat oprávnění kdykoli. Odvolání autorizaci má za následek všechna připojení odkaz odstraňuje z předplatného hello jehož přístup byl odvolán.

### <a name="circuit-owner-operations"></a>Operace vlastníka okruhu

**toocreate autorizace připojení**

vlastníka okruhu Hello vytvoří povolení. To vede k vytváření hello autorizační klíč, který může být používán tooconnect uživatele okruhu jejich toohello brány virtuální sítě okruh ExpressRoute. Povolení je platný pro jenom jedno připojení.

1. V okně hello ExpressRoute, klikněte na tlačítko **autorizací** a potom zadejte **název** hello autorizace a klikněte na **Uložit**.

    ![Povolení](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. Po uložení konfigurace hello zkopírujte hello **ID prostředku** a hello **autorizační klíč**.

    ![Autorizační klíč](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**toodelete autorizace připojení**

Připojení můžete odstranit výběrem hello **odstranit** ikonu na hello okno pro připojení.

### <a name="circuit-user-operations"></a>Operace okruh uživatele

Hello okruh uživatel potřebuje ID prostředku hello a autorizační klíč z vlastníka okruhu hello. 

**tooredeem autorizace připojení**

1.  Klikněte na tlačítko hello **+ nový** tlačítko.

    ![Klikněte na tlačítko Nový](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  Vyhledejte **"Připojení"** v hello Marketplace, vyberte ho a klikněte na tlačítko **vytvořit**.

    ![Hledání připojení](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  Ujistěte se, zda text hello **typ připojení** je nastaven příliš "ExpressRoute".


4.  Zadejte podrobnosti hello a pak klikněte na **OK** v okně základy hello.

    ![Okno Základy](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  V hello **nastavení** okně, vyberte hello **Brána virtuální sítě** a zkontrolujte hello **uplatnit autorizace** zaškrtávací políčko.

6.  Zadejte hello **autorizační klíč** a hello **Peer okruh URI** a pojmenujte hello připojení. Klikněte na **OK**.

    ![Okno Nastavení](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. Zkontrolujte informace hello v hello **Souhrn** a klikněte na **OK**.


**toorelease autorizace připojení**

Povolení můžete vydat odstraněním hello připojení, který odkazuje hello ExpressRoute okruhu toohello virtuální sítě.

## <a name="next-steps"></a>Další kroky
Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
