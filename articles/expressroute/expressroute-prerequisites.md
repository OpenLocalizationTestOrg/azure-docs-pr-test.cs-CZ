---
title: "aaaPrerequisites pro přijetí Azure ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje seznam toobe požadavky splněny, než můžete objednat okruh Azure ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: f872d25e-acfd-405d-9d1b-dcb9f323a2ff
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/30/2017
ms.author: cherylmc
ms.openlocfilehash: 524c86f6570dc6e6505fe55323b8508e8eeff791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-prerequisites--checklist"></a>Požadavky ExpressRoute a kontrolní seznam
tooconnect tooMicrosoft cloudové služby pomocí služby ExpressRoute, musíte tooverify této hello následující požadavky uvedené v následující části hello byly splněny.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Účet Azure
* Platný a aktivní účet Microsoft Azure. Tento účet se vyžaduje tooset až hello okruh ExpressRoute. Okruhy ExpressRoute jsou prostředky v rámci předplatných Azure. Předplatné Azure je požadavkem i v případě, že připojení je omezené toonon Azure cloudových služeb Microsoftu, například služby Office 365 a Dynamics 365.
* Aktivní předplatné Office 365 (pokud používáte služby Office 365). Další informace najdete v tématu hello [specifické požadavky Office 365](#office-365-specific-requirements) tohoto článku.

## <a name="connectivity-provider"></a>Poskytovatel připojení

* Můžete pracovat [partnerem připojení ExpressRoute](expressroute-locations.md#partners) tooconnect toohello cloudu Microsoftu. Připojení mezi místní sítí a Microsoftem můžete vytvořit [třemi způsoby](expressroute-introduction.md)
* Pokud váš poskytovatel není partnerem připojení ExpressRoute, můžete se pořád připojit toohello cloudu Microsoftu prostřednictvím [poskytovatele cloudové výměny](expressroute-locations.md#connectivity-through-exchange-providers).

## <a name="network-requirements"></a>Síťové požadavky
* **Redundantní připojení:** Neexistuje požadavek na redundanci fyzického připojení mezi vámi a poskytovatelem. Microsoft nevyžaduje redundantní toobe relací protokolu BGP mezi směrovači Microsoftu a směrovači partnerského vztahu hello, nastavit i v případě, že máte právě [jeden fyzické připojení tooa cloudové výměně](expressroute-faqs.md#onep2plink).
* **Směrování**: v závislosti na tom, jak připojit toohello cloudu Microsoftu, vy nebo váš poskytovatel potřebovat tooset nahoru a spravovat relace BGP hello pro [domény směrování](expressroute-circuit-peerings.md). Někteří poskytovatelé ethernetového připojení nebo poskytovatelé cloudové výměny můžou nabízet správu protokolu BGP jako službu s přidanou hodnotou.
* **NAT:** Microsoft prostřednictvím partnerského vztahu Microsoftu přijímá jenom veřejné IP adresy. Pokud používáte soukromé IP adresy v síti na pracovišti, můžete nebo zprostředkovatele nutné tootranslate hello privátní IP adresy toohello veřejné IP adresy [pomocí hello NAT](expressroute-nat.md).
* **QoS:** Skype pro firmy má různé služby (třeba hlasové, textové nebo videoslužby), které vyžadují diferencovaný přístup. Vy a váš poskytovatel postupujte podle hello [požadavky na technologii QoS](expressroute-qos.md).
* **Zabezpečení sítě**: zvažte [zabezpečení sítě](../best-practices-network-security.md) při připojování toohello cloudu Microsoftu pomocí ExpressRoute.

## <a name="office-365"></a>Office 365
Pokud máte v plánu tooenable Office 365 v ExpressRoute, přečtěte si následující dokumenty, kde najdete další informace o požadavcích Office 365 hello.

* [Přehled ExpressRoute pro Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Směrování s ExpressRoute pro Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
* [Adresy URL a rozsahy IP adres pro Office 365](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Plánování sítě a optimalizace výkonu pro Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Nástroje a kalkulačky šířky pásma sítě](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
* [Integrace Office 365 s místními prostředími](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)
* [Školicí videa ExpressRoute v Office 365 pro pokročilé](https://channel9.msdn.com/series/aer/)

## <a name="dynamics-365"></a>Dynamics 365
Pokud máte v plánu tooenable Dynamics 365 v ExpressRoute, přečtěte si následující dokumenty, kde najdete další informace o Dynamics 365 hello

* [Dokument white paper pro Dynamics 365 a ExpressRoute](http://download.microsoft.com/download/B/2/8/B2896B38-9832-417B-9836-9EF240C0A212/Microsoft%20Dynamics%20365%20and%20ExpressRoute.pdf)
* [Adresy URL](https://support.microsoft.com/kb/2655102) a [rozsahy IP adres](https://support.microsoft.com/kb/2728473) pro Dynamics 365

## <a name="next-steps"></a>Další kroky
* Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
* Vyhledejte poskytovatele připojení ExpressRoute. Viz [Partneři ExpressRoute a umístění partnerského vztahu](expressroute-locations.md).
* Odkazovat toorequirements pro [směrování](expressroute-routing.md), [NAT](expressroute-nat.md), a [QoS](expressroute-qos.md).
* Nakonfigurujte připojení ExpressRoute.
  * [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md)
  * [Konfigurace směrování](expressroute-howto-routing-classic.md)
  * [Propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-classic.md)
