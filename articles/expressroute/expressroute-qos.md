---
title: "aaaQoS požadavky pro ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje podrobné požadavky pro konfiguraci a správu technologie QoS pro okruhy ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: db1c1447-0283-4a09-907b-ae481adc40c7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: cherylmc
ms.openlocfilehash: 46cc81bd38ff50dd9e7a1bfdd0faa457ff7b2fa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-qos-requirements"></a>Požadavky na technologii QoS služby ExpressRoute
Skype pro firmy má úlohy, které vyžadují odlišné zacházení podle QoS. Pokud máte v plánu tooconsume hlasové služby prostřednictvím ExpressRoute, měli byste dodržovat toohello požadavky popsané dál.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> Požadavky na technologii QoS použít toohello Microsoft jenom partnerského vztahu. hodnoty DSCP Hello v provozu sítě byla přijata veřejný partnerský vztah Azure a soukromý partnerský vztah Azure budou too0 resetování. 
> 
> 

Hello následující tabulka obsahuje seznam označení DSCP používaných Skypem pro firmy. Odkazovat příliš[Správa technologie QoS pro Skype pro firmy](https://technet.microsoft.com/library/gg405409.aspx) Další informace.

| **Třída provozu** | **Zpracování (označení DSCP)** | **Úlohy Skypu pro firmy** |
| --- | --- | --- |
| **Hlas** |EF (46) |Skype/Lync – hlas |
| **Interaktivní** |AF41 (34) |Video, VBSS |
| AF21 (18) |Sdílení aplikací | |
| **Výchozí** |AF11 (10) |Přenos souborů |
| CS0 (0) |Cokoliv jiného | |

* Měli byste klasifikovat úlohy hello a označit správné hodnoty DSCP hello. Postupujte podle pokynů hello uvedených [sem](https://technet.microsoft.com/library/gg405409.aspx) o označení DSCP tooset ve vaší síti.
* Měli byste ve vaší síti nakonfigurovat a podporovat více front QoS. Hlas musí být samostatná třída a přijímat zpracování EF hello uvedené v dokumentu RFC 3246. 
* Můžete rozhodnout hello služby Řízení front mechanismus, zásadách detekce zahlcení a přidělení šířky pásma jednotlivé třídy provozu. Ale hello označení DSCP pro Skype pro firmy musí být zachováno. Pokud používáte označení DSCP není uvedené výš, například AF31 (26), je třeba tento too0 hodnotu DSCP přepsat před odesláním paketu tooMicrosoft hello. Microsoft odesílá jenom pakety označené hodnotami DSCP uvedenými v hello výše tabulky hello. 

## <a name="next-steps"></a>Další kroky
* Odkazovat toohello požadavky pro [směrování](expressroute-routing.md) a [NAT](expressroute-nat.md).
* Zjistit hello následující odkazy tooconfigure připojení ExpressRoute.
  
  * [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md)
  * [Konfigurace směrování](expressroute-howto-routing-classic.md)
  * [Propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-classic.md)

