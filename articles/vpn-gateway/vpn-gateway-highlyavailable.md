---
title: "aaaOverview vysoce dostupné konfigurace službou Azure VPN Gateways | Microsoft Docs"
description: "Tento článek obsahuje přehled konfigurací s vysokou dostupností se službami Azure VPN Gateway."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2016
ms.author: yushwang
ms.openlocfilehash: 316293b9ac79645bf9bb9e89fbc4aa8f3eacd209
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Připojení s vysokou dostupností mezi jednotlivými místy a VNet-to-VNet
Tento článek obsahuje přehled konfigurací s vysokou dostupností pro vaše propojení mezi jednotlivými místy a propojení VNet-to-VNet se službami Azure VPN Gateway.

## <a name = "activestandby"></a>O Azure VPN gateway redundance
Každá Azure VPN Gateway se skládá ze dvou instancí v konfiguraci aktivní-pohotovostní. Pro všechny plánované údržby nebo neplánovaném přerušení, který se stane aktivní instance toohello by pohotovostní instancí hello automaticky převzít (převzetí služeb při selhání) a obnovit hello S2S VPN nebo připojení VNet-to-VNet. Přepněte Hello způsobí, že stručný přerušení. O plánované údržbě se má obnovit v rámci 10 sekund too15 hello připojení. Neplánované problémů obnovení připojení hello bude delší o 1 minutu too1 a půl minut v nejhorším případě hello. Pro P2S VPN gateway toohello připojení klienta bude odpojen připojení P2S hello a hello uživatelé budou potřebovat tooreconnect z hello klientských počítačů.

![Aktivní–pohotovostní konfigurace](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Připojení s vysokou dostupností mezi jednotlivými místy
lepší dostupnost tooprovide pro vaše mezi místní připojení, nejsou k dispozici několik možností:

* Několik místních zařízení VPN
* Azure VPN Gateway v konfiguraci aktivní-aktivní
* Kombinace obojího

### <a name = "activeactiveonprem"></a>Více místní zařízení VPN
Více zařízení VPN můžete použít z vaší místní síti tooconnect tooyour brány Azure VPN, jak je znázorněno v následujícím diagramu hello:

![Několik místních zařízení VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Tato konfigurace poskytuje více tunelů active z hello stejné Azure VPN gateway tooyour místní zařízení v hello stejné umístění. Existuje několik požadavků a omezení:

1. Je nutné toocreate víceklientská připojení VPN typu S2S z vaší tooAzure zařízení VPN. Při připojení více zařízení VPN z hello stejnou místní sítě tooAzure, budete potřebovat toocreate jedna brána místní sítě pro jednotlivá zařízení VPN a jedno připojení z vaší brány Azure VPN gateway toohello místní sítě.
2. brány místní sítě Hello odpovídající tooyour zařízení VPN musí mít jedinečné veřejné IP adresy v hello vlastnost "GatewayIpAddress".
3. Pro tuto konfiguraci se vyžaduje BGP. Každý místní síťové brány reprezentující zařízení VPN musí mít jedinečnou BGP sdílené IP adresu zadaná ve vlastnosti "BgpPeerIpAddress" hello.
4. pole vlastnosti Hello AddressPrefix v každé brány místní sítě se nesmí překrývat. Musíte zadat hello "BgpPeerIpAddress" v /32 formátu CIDR hello AddressPrefix pole, například 10.200.200.254/32.
5. Měli byste použít protokol BGP tooadvertise hello stejné předpony z hello stejnou místní sítě předpony tooyour Azure VPN gateway a provoz hello se předají do těchto tunelových propojení současně.
6. Každé připojení se počítá proti hello maximální počet tunelových propojení pro bránu Azure VPN, 10 pro základní a standardní SKU a pro HighPerformance SKU 30. 

V této konfiguraci hello Azure VPN gateway je stále v režimu aktivní pohotovostní, takže hello stejné chování převzetí služeb při selhání a stručný přerušení bude stále nastat, jak je popsáno [výše](#activestandby). Tato konfigurace ale chrání proti selháním nebo přerušením, jejichž příčina je v místní síti nebo v zařízení VPN.

### <a name="active-active-azure-vpn-gateway"></a>Azure VPN Gateway v konfiguraci aktivní-aktivní
Nyní můžete vytvořit bránu VPN Azure VPN v konfiguraci aktivní aktivní, kde obou instancí hello brány virtuálních počítačů bude vytvoření že s2s VPN tunelových propojení tooyour místního zařízení VPN, jako uvedené hello následující diagram:

![Aktivní–aktivní](./media/vpn-gateway-highlyavailable/active-active.png)

V této konfiguraci každá instance brány Azure bude mít jedinečný veřejnou IP adresu a každý navázání protokolu IPsec/IKE S2S VPN tunel tooyour místní zařízení VPN zadanou v bráně místní sítě a připojení. Všimněte si, že oba tunelových propojení VPN jsou ve skutečnosti součástí hello stejné připojení. Pořád budete potřebovat tooconfigure vaší místní sítě VPN zařízení tooaccept nebo vytvořit dvě S2S VPN tunely toothose dva Azure VPN gateway veřejné IP adresy.

Protože instance brány Azure hello v konfiguraci aktivní aktivní, hello provoz z vaší virtuální síti Azure tooyour místního sítě, budou směrovány do obou tunelových propojení současně, i v případě, že vaše místní zařízení VPN může upřednostnit jeden tunel přes Hello jiné. Poznámka: když hello stejného toku TCP nebo UDP bude vždy procházení hello stejné tunelové propojení nebo cestu, pokud událost údržby se odehrává na jednu z instancí hello.

Při plánované údržby nebo neplánované události se stane instance brány tooone, hello tunelu IPsec z této instance tooyour místního zařízení VPN budou odpojeny. Hello odpovídající trasy na vaše zařízení VPN by měla být odebrán nebo odvolají automaticky tak, aby provoz hello bude přepnuta přes toohello jiných active tunelu IPsec. Na hello Azure straně proběhne hello přepněte automaticky z hello vliv instance toohello aktivní instance.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Dvojitá redundance: brány VPN v konfiguraci aktivní–aktivní na straně Azure i v místní síti
možnost nejspolehlivější Hello je toocombine hello aktivní aktivní brány na vaší sítí a Azure, jak je znázorněno v následujícím diagramu hello.

![Dvojitá redundance](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Zde vytvoření a nastavení brány Azure VPN hello v konfiguraci aktivní aktivní a vytvořit dvě brány místní sítě a dvě připojení pro vaše dva místní zařízení VPN jak bylo popsáno výše. Hello výsledkem je připojení Vícecestná 4 tunelových propojení IPsec mezi virtuální sítě Azure a v místní síti.

Všechny brány a tunely jsou aktivní z hello Azure straně, tak hello provoz bude šíří mezi všechny 4 tunely současně, i když každý TCP nebo UDP toku bude znovu postupujte podle hello stejné tunelové propojení nebo cestu z hello Azure straně. I když tak, že se provoz hello, může zobrazit něco vyšší propustnost přes tunely IPsec hello, hello primární cílem této konfigurace je pro vysokou dostupnost. A z důvodu toohello statistické povahy hello šíření, je obtížné tooprovide hello měření v provozu jak různých aplikací podmínky ovlivní agregovanou propustnost hello.

Tato topologie vyžaduje dvě brány místní sítě a dvě připojení toosupport hello pár místní zařízení VPN a BGP je požadovaná tooallow hello dvě připojení toohello stejné místní síti. Tyto požadavky jsou stejné jako hello hello [výše](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Připojení s vysokou dostupností VNet-to-VNet prostřednictvím služby Azure VPN Gateway
Hello stejnou konfiguraci aktivní aktivní můžete také použít připojení VNet-to-VNet tooAzure. Můžete vytvořit aktivní aktivní brány sítě VPN pro obě virtuální sítě a připojte je společně tooform hello stejné úplné OK sítě připojení 4 tunelová propojení mezi hello dvě virtuální sítě, jak je znázorněno v hello diagramu níže:

![VNet-to-VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Tím se zajistí, že jsou vždy pár tunelová propojení mezi hello dvě virtuální sítě pro všechny události plánované údržby tím ještě lepší dostupnost. I když hello stejné topologie pro připojení mezi různými místy vyžaduje dvě připojení, topologie sítě VNet-to-VNet hello výše uvedeném potřebovat pouze jedno připojení pro každou bránu. Kromě toho protokol BGP je nepovinný, pokud směrování přenosu přes hello připojení VNet-to-VNet je požadovaná.

## <a name="next-steps"></a>Další kroky
V tématu [konfiguraci brány VPN aktivní-aktivní pro mezi různými místy a připojení VNet-to-VNet](vpn-gateway-activeactive-rm-powershell.md) kroky tooconfigure aktivní aktivní mezi různými místy a připojení VNet-to-VNet.

