---
title: "Vytvoření a úprava okruhu ExpressRoute: portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate, zřizovat, ověřte, aktualizovat, odstranit a zrušit jejich zřízení okruhu ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/07/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: 200418aed6bdebace43a2f57cf532d1c8d13cb83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a>Vytvoření a úprava okruhu ExpressRoute
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-circuit-classic.md)
>

Tento článek popisuje, jak hello toocreate okruhu Azure ExpressRoute pomocí Azure portal a hello modelu nasazení Azure Resource Manager. Hello také následující kroky ukazují, jak stav hello toocheck hello okruh, aktualizovat, nebo odstranit a zrušit jejich zřízení se.


## <a name="before-you-begin"></a>Než začnete
* Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Zajistěte, abyste měli přístup toohello [portál Azure](https://portal.azure.com).
* Ujistěte se, zda máte oprávnění toocreate nové síťové prostředky. Pokud nemáte hello správná oprávnění, obraťte se na správce účtu.
* Můžete [zobrazit video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) před zahájením v pořadí toobetter pochopit hello kroky.

## <a name="create-and-provision-an-expressroute-circuit"></a>Vytvořit a zřídit okruhu ExpressRoute
### <a name="1-sign-in-toohello-azure-portal"></a>1. Přihlaste se toohello portálu Azure
V prohlížeči přejděte toohello [portál Azure](http://portal.azure.com) a přihlaste se pomocí účtu Azure.

### <a name="2-create-a-new-expressroute-circuit"></a>2. Vytvořit nový okruh ExpressRoute.
> [!IMPORTANT]
> Váš okruh ExpressRoute bude účtován z hello okamžiku, kdy se objeví klíč služby. Ujistěte se, když hello poskytovatele připojení je připraven tooprovision hello okruh provedení této operace.
> 
> 

1. Okruh ExpressRoute můžete vytvořit tak, že vyberete možnost toocreate hello nový prostředek. Klikněte na tlačítko **nový** > **sítě** > **ExpressRoute**, jak ukazuje následující obrázek hello:
   
    ![Vytvoření okruhu ExpressRoute](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. Po kliknutí na tlačítko **ExpressRoute**, uvidíte hello **okruh ExpressRoute vytvořit** okno. Když jste vyplnění hello hodnoty v tomto okně, ujistěte se, že zadáváte správnou úroveň SKU hello a data měření.
   
   * **Úroveň** Určuje, zda je povoleno ExpressRoute standard nebo doplněk ExpressRoute premium. Můžete zadat **standardní** tooget hello standardní SKU nebo **Premium** pro doplněk premium hello.
   * **Měření dat** Určuje typ fakturace hello. Můžete zadat **Metered** pro plán měření podle objemu dat a **neomezený** pro plán neomezená data na úrovni. Můžete změnit hello fakturace typu z **Metered** příliš**neomezený**, ale nemůžete změnit typ hello z **neomezený** příliš**Metered**.
     
     ![Konfigurace hello SKU vrstvy a měření dat.](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> Upozorňujeme, že hello umístění v partnerském vztahu označuje hello [fyzické umístění](expressroute-locations.md) kde jste partnerského vztahu se společností Microsoft. Toto je **není** příliš propojená vlastnost "Umístění", který odkazuje toohello geography, kde se nachází hello poskytovatele síťových prostředků Azure. I když se nesouvisí, je vhodné toochoose geograficky zavřít toohello poskytovatele síťových prostředků umístění v partnerském vztahu okruhu hello. 
> 
> 

### <a name="3-view-hello-circuits-and-properties"></a>3. Zobrazení hello okruhy a vlastnosti
**Zobrazit všechny okruhy hello**

Můžete zobrazit všechny hello okruhů, které jste vytvořili výběrem **všechny prostředky** v levé nabídce hello.

![Okruhy zobrazení](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Zobrazit vlastnosti hello**

    You can view hello properties of hello circuit by selecting it. On this blade, note hello service key for hello circuit. You must copy hello circuit key for your circuit and pass it down toohello service provider toocomplete hello provisioning process. hello circuit key is specific tooyour circuit.

![Zobrazení vlastností](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>4. Odeslání poskytovatele připojení klíče tooyour hello služby pro zřizování
V tomto okně **stav zprostředkovatele** poskytuje informace o hello aktuální stav zřizování na straně hello poskytovatele služeb. **Stav okruhu** poskytuje hello stavu na straně Microsoft hello. Další informace o zřizování stavy okruhu najdete v tématu hello [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.

Když vytvoříte nový okruh ExpressRoute, bude mít okruh hello hello následující stav:

Stav zprostředkovatele: není zajišťováno<BR>
Stav okruhu: povoleno

![Zahájení procesu zřizování](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

okruh Hello změní toohello následující stavu, pokud je zprostředkovatel připojení k hello v procesu hello povolení pro vás:

Stav zprostředkovatele: zřizování<BR>
Stav okruhu: povoleno

Pro jste toobe možné toouse okruh ExpressRoute musí být v hello následující stav:

Stav zprostředkovatele: zřídit<BR>
Stav okruhu: povoleno

### <a name="5-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>5. Pravidelně kontrolovat hello stav a stav hello hello okruh klíče
Můžete zobrazit vlastnosti hello hello okruhu, který máte zájem výběrem. Zkontrolujte hello **stav zprostředkovatele** a ujistěte se, že se přesunul příliš**zajištěno** než budete pokračovat.

![Stav okruhu a zprostředkovatele](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. Vytvořte vlastní konfiguraci směrování
Podrobné pokyny najdete v části toohello [konfigurace směrování pro okruh ExpressRoute](expressroute-howto-routing-portal-resource-manager.md) článek toocreate a upravit partnerských vztahů pro okruh.

> [!IMPORTANT]
> Tyto pokyny platí pouze toocircuits, které jsou vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.
> 
> 

### <a name="7-link-a-virtual-network-tooan-expressroute-circuit"></a>7. Propojení virtuální sítě tooan okruh ExpressRoute
V dalším kroku propojte virtuální sítě tooyour okruh ExpressRoute. Použití hello [propojení virtuální sítě okruhů tooExpressRoute](expressroute-howto-linkvnet-arm.md) článek při práci s modelem nasazení Resource Manager hello.

## <a name="getting-hello-status-of-an-expressroute-circuit"></a>Získávání hello stav okruhu ExpressRoute
Hello stav okruhu můžete zobrazit výběrem. 

![Stav okruhu ExpressRoute](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a>Úprava okruhu ExpressRoute
Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.

Můžete provést následující bez výpadků hello:

* Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.
* Zvýšení hello šířka pásma okruhu ExpressRoute, pokud na portu hello je dostupné kapacity. Všimněte si, že hello šířka pásma okruhu přechod na starší verzi není podporován. 
* Změnit plán z tooUnlimited dat – měření podle objemu dat měření hello. Poznámka: Změna hello měření plán z tooMetered neomezená Data na úrovni, dat není podporováno.
* Můžete povolit nebo zakázat *povolit klasické operace*.

Další informace o omezení a omezení, najdete v části toohello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

toomodify okruh ExpressRoute, klikněte na hello **konfigurace** jak je znázorněno na následujícím obrázku hello.

![Úprava okruhu](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

Můžete upravit hello šířky pásma, SKU, model fakturace a povolit klasické operace v rámci okna konfigurace hello.

> [!IMPORTANT]
> Pokud je na existující port hello nedostatečné kapacity, mohou mít okruh ExpressRoute toorecreate hello. Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat hello okruh.
>
> Nelze zmenšit hello šířku pásma okruhu ExpressRoute bez přerušení. Přechod na starší verzi šířky pásma vyžaduje, abyste okruh ExpressRoute hello toodeprovision a pak znova nezajistíte nové okruh ExpressRoute.
> 
> Zakázat doplněk premium operace může selhat, pokud používáte prostředky, které jsou větší než co je povolené standardní okruh hello.
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Zrušení zřízení a odstraňování okruhu ExpressRoute
Váš okruh ExpressRoute můžete odstranit výběrem hello **odstranit** ikonu. Vezměte na vědomí následující hello:

* Je nutné zrušit všechny virtuální sítě od hello okruh ExpressRoute. Pokud se tato operace nezdaří, zkontrolujte, zda jsou všechny virtuální sítě propojené toohello okruh.
* Pokud je hello ExpressRoute okruhu poskytovatele služeb Stav zřizování **zřizování** nebo **zajištěno** na jejich straně, musíte pracovat se váš okruh hello toodeprovision zprostředkovatele služby. Jsme bude i nadále tooreserve prostředky a dokud poskytovatele služeb hello dokončení zrušení zřízení hello okruhu a upozorní nám vám účtovat.
* Pokud má poskytovatel služeb hello zrušit okruh hello (Stav zřizování poskytovatele služeb hello je nastaven příliš**není zajišťováno**) pak můžete odstranit okruh hello. Tato akce ukončí fakturace pro okruh hello

## <a name="next-steps"></a>Další kroky
Po vytvoření okruhu, ujistěte se, že hello následující:

* [Vytvoření a úprava směrování pro okruhu ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)
* [Propojení vaší virtuální sítě tooyour okruh ExpressRoute](expressroute-howto-linkvnet-arm.md)

