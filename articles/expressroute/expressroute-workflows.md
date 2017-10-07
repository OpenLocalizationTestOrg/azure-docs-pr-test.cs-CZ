---
title: aaaWorkflows pro konfiguraci okruh ExpressRoute | Microsoft Docs
description: "Tato stránka vás provede hello pracovní postupy pro konfiguraci okruhu ExpressRoute a partnerských vztahů"
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 55e0418c-e0bf-44a7-9aa1-720076df9297
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8e1dfc137401e0d6d53608ae6c8de0085e182eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>Pracovní postupy ExpressRoute pro zřizování a stavy okruhů
Tato stránka vás provede procesem hello zřizování služby a pracovní postupy směrování configuration na vysoké úrovni.

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

Hello následující obrázek a příslušné postupy ukazují hello úlohy, že je třeba provést v pořadí toohave ExpressRoute okruh zřízený end až do konce. 

1. Pomocí prostředí PowerShell tooconfigure okruhu ExpressRoute. Postupujte podle pokynů hello v hello [okruhy ExpressRoute vytvořit](expressroute-howto-circuit-classic.md) další podrobnosti najdete v článku.
2. Pořadí připojení od poskytovatele služeb hello. Tento proces se liší. Další podrobnosti o tom, obraťte se na svého poskytovatele připojení tooorder připojení.
3. Ujistěte se, že hello okruh má úspěšně zřízený kontrolou okruh ExpressRoute hello zřizování stavu pomocí prostředí PowerShell. 
4. Konfigurace domény směrování. Pokud poskytovatel připojení vrstvy 3 spravuje za vás, nakonfiguruje směrování pro váš okruh. Pokud poskytovatel připojení nabízí pouze služby vrstvy 2, je nutné nakonfigurovat směrování podle pokynů popsaných v hello [požadavky na směrování](expressroute-routing.md) a [konfigurace směrování](expressroute-howto-routing-classic.md) stránky.
   
   * Povolení soukromého partnerského vztahu Azure – musí povolit tohoto partnerského vztahu tooVMs tooconnect / cloudových služeb nasadit v rámci virtuální sítě.
   * Povolit veřejný partnerský vztah Azure – je nutné povolit veřejný partnerský vztah Azure chcete tooconnect tooAzure služeb hostovaných na veřejné IP adresy. Toto je požadavek tooaccess prostředky Azure, pokud jste vybrali tooenable výchozí směrování pro soukromý partnerský vztah Azure.
   * Povolit partnerského vztahu Microsoftu - je nutné povolit tuto tooaccess Office 365 a Dynamics 365. 
     
     > [!IMPORTANT]
     > Ujistěte se, že používáte samostatné proxy server / edge tooconnect tooMicrosoft než text hello, který používáte pro hello Internetu. Pomocí hello stejné hraniční pro ExpressRoute a hello Internetu bude způsobit asymetrické směrování a způsobit výpadky připojení pro vaši síť.
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. Propojení virtuální sítě okruhů tooExpressRoute – můžete propojit okruh ExpressRoute tooyour virtuální sítě. Postupujte podle pokynů [toolink virtuálních sítí](expressroute-howto-linkvnet-arm.md) tooyour okruh. Tyto virtuální sítě může být buď v hello stejné předplatné jako hello okruh ExpressRoute, nebo může být v jiném předplatném.

## <a name="expressroute-circuit-provisioning-states"></a>Stavy zřizování okruh ExpressRoute
Každý okruh ExpressRoute má dva stavy:

* Stav zřizování poskytovatele služby
* Status

Stav představuje stav zřizování společnosti Microsoft. Tato vlastnost nastavena tooEnabled při vytvoření okruhu Expressroute

Stav zřizování zprostředkovatele připojení k Hello představuje hello stav na straně poskytovatele připojení hello. Může to být *NotProvisioned*, *zřizování*, nebo *zajištěno*. Hello okruh ExpressRoute musí být v zajištěno stavu můžete toobe možné toouse ho.

### <a name="possible-states-of-an-expressroute-circuit"></a>Možné stavy okruhu ExpressRoute
Tato část uvádí out hello možné stavy pro okruh ExpressRoute.

**V okamžiku vytvoření**

Zobrazí se hello okruh ExpressRoute v hello následující stavu co nejdříve můžete spustit rutiny prostředí PowerShell text hello, toocreate okruh ExpressRoute hello.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**Pokud poskytovatel připojení je v hello proces zřizování okruh hello**

Zobrazí se hello okruh ExpressRoute v hello následující stavu co nejdříve můžete předat poskytovatele připojení klíče toohello hello služby a zahájil hello procesu zřizování.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**Po dokončení procesu zřizování hello poskytovatele připojení**

Zobrazí se hello okruh ExpressRoute v hello následující stavu, jakmile poskytovatel připojení hello dokončil hello procesu zřizování.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

Zřízený a povolený je hello stavu hello okruh může být pouze v pro jste toobe možné toouse ho. Pokud používáte poskytovatele vrstvy 2, můžete nakonfigurovat směrování pro váš okruh, pouze pokud je v tomto stavu.

**Když je poskytovatel připojení zrušení zřízení okruhu hello**

Pokud jste si vyžádali hello služby zprostředkovatele toodeprovision hello okruh ExpressRoute, zobrazí se hello okruh nastavit toohello následující stav po dokončení procesu zrušení zřízení hello hello poskytovatele služeb.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Můžete povolit toore je-li potřeba nebo se spustit rutiny prostředí PowerShell toodelete hello okruh.  

> [!IMPORTANT]
> Pokud spustíte hello prostředí PowerShell rutinu toodelete hello okruh hello ServiceProviderProvisioningState při zřizování nebo zajištěno hello operace selže. Spojte se s okruh ExpressRoute toodeprovision hello vaše připojení poskytovatele nejprve a pak odstraňte hello okruh. Microsoft bude pokračovat toobill hello okruh, dokud nespustíte hello okruh hello toodelete rutiny prostředí PowerShell.
> 
> 

## <a name="routing-session-configuration-state"></a>Stav konfigurace směrování relace
Hello Stav zřizování BGP umožňuje zjistit, zda byla relace protokolu BGP hello zapnuta hello Microsoft edge. Hello stav musí být povolen pro vás hello toobe možné toouse partnerský vztah.

Je stav relace protokolu BGP důležité toocheck hello hlavně pro partnerský vztah Microsoftu. V přidání toohello BGP zřizování stavu, je jiný stav názvem *inzerované předpony veřejných stavu*. Hello inzerované předpony veřejných stavu musí být v *nakonfigurované* stavu, jak pro toobe relace protokolu BGP hello nahoru a vaše směrování toowork začátku do konce. 

Pokud hello inzerované předponu veřejné stav je nastavený tooa *ověření potřeby* stavu relace protokolu BGP hello není povolena, protože hello inzerované předpony neodpovídá hello jako číslo v některém z registrech směrování hello. 

> [!IMPORTANT]
> Pokud hello inzerované předpony veřejných stavu je v *ruční ověřování* stavu, musíte otevřít lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a prokázání, že vlastníte hello IP adresy inzerované podél s hello související číslo autonomního systému.
> 
> 

## <a name="next-steps"></a>Další kroky
* Nakonfigurujte připojení ExpressRoute.
  
  * [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md)
  * [Konfigurace směrování](expressroute-howto-routing-arm.md)
  * [Propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-arm.md)

