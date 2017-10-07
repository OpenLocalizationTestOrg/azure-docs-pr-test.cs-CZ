---
title: "připojení k místní aaaDiagnose prostřednictvím brány sítě VPN s sledovací proces sítě Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toodiagnose místní připojení prostřednictvím brány sítě VPN s odstraňováním problémů prostředků sledovací proces sítě Azure."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: aeffbf3d-fd19-4d61-831d-a7114f7534f9
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 9941c5d1b49bec29062210684dae8653cbdb84b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-on-premises-connectivity-via-vpn-gateways"></a>Diagnostika místní připojení prostřednictvím brány sítě VPN

Služba Azure VPN Gateway umožňuje toocreate hybridní řešení, které řeší hello potřebu zabezpečené připojení mezi místní sítí a virtuální sítě Azure. Podle požadavků jsou jedinečné, takže je hello volbou místní zařízení VPN. Azure aktuálně podporuje [několika zařízeními VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) , jsou neustále ověřit ve spolupráci s dodavateli zařízení hello. Zkontrolujte nastavení konfigurace specifických zařízení hello před konfigurací vaše místní zařízení VPN. Podobně je Azure VPN Gateway nakonfigurovat sadu [podporované parametry protokolu IPsec](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) , který se používá pro navazování připojení. Aktuálně neexistuje žádný způsob pro vás toospecify nebo vyberte konkrétní kombinaci parametrů IPsec z hello Azure VPN Gateway. Pro vytvoření úspěšné připojení mezi místními a Azure, hello místní nastavení zařízení VPN musí být v souladu s parametry protokolu IPsec hello předepsané Azure VPN Gateway. Pokud hello nastavení jsou ve správné, dojde ke ztrátě připojení a dosud řešení těchto potíží nebyla trivial a obvykle trvalo hodin tooidentify a opravte problém hello.

Řešení potíží s funkcí s hello sledovací proces sítě Azure, jsou možné toodiagnose všechny problémy s brány a připojení a během minut mít dostatek informací toomake problém hello toorectify informované rozhodnutí.

## <a name="scenario"></a>Scénář

Chcete tooconfigure site-to-site připojení mezi Azure a místními pomocí FortiGate jako hello místní brány VPN. tooachieve v tomto scénáři by vyžadovaly hello následující nastavení:

1. Brána virtuální sítě - hello brány VPN Azure
1. Brána místní sítě - hello [místní brány sítě VPN (FortiGate)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) reprezentace v cloudu Azure
1. Připojení Site-to-site (na základě zásad) - [připojení mezi hello brána sítě VPN a hello místní směrovač](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#createconnection)
1. [Konfigurace FortiGate](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/Site-to-Site_VPN_using_FortiGate.md)

Podrobné pokyny krok za krokem pro konfiguraci konfigurace Site-to-Site najdete navštivte stránky: [vytvoření virtuální sítě s připojením Site-to-Site pomocí portálu Azure hello](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Jeden z kroků konfigurace kritické hello je konfigurace parametry komunikace hello protokolu IPsec, všechny chybné konfigurace vede tooloss připojení mezi hello místní sítí a Azure. Aktuálně jsou brány sítě VPN Azure nakonfigurovaný toosupport hello následující parametry protokolu IPsec pro fázi 1. Všimněte si, jak je uvedeno výše, že toto nastavení nelze změnit.  Jak můžete vidět v hello tabulce, jsou hello šifrovacích algoritmů nepodporuje Azure VPN Gateway AES256, AES128 a 3DES.

### <a name="ike-phase-1-setup"></a>Nastavení protokolu IKE fáze 1

| **Vlastnost** | **PolicyBased** | **RouteBased a standardní nebo vysoce výkonné VPN gateway** |
| --- | --- | --- |
| Verze IKE |IKEv1 |IKEv2 |
| Skupina Diffie-Hellman |Skupina 2 (1 024 bitů) |Skupina 2 (1 024 bitů) |
| Metoda ověřování |Předsdílený klíč |Předsdílený klíč |
| Algoritmy šifrování |AES256 AES128 3DES |AES256 3DES |
| Algoritmus hash |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Životnost přidružení zabezpečení (SA) Fáze 1 (čas) |28 800 sekund |10 800 sekund |

Jako uživatel, bude požadované tooconfigure FortiGate, Ukázková konfigurace lze najít v [Githubu](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/fortigate_show%20full-configuration.txt). Vaše FortiGate toouse SHA-512 se nechtěně nakonfigurovaný jako hello algoritmus hash. Protože tento algoritmus není podporované algoritmus pro připojení na základě zásad, připojení k síti VPN funguje.

Tyto problémy jsou pevné tootroubleshoot a základní příčiny jsou často intuitivní. V takovém případě můžete otevřít pomáhá tooget lístek podpory v řešení problému hello. Ale s sledovací proces sítě Azure Poradce při potížích s rozhraní API, můžete identifikovat tyto problémy sami.

## <a name="troubleshooting-using-azure-network-watcher"></a>Řešení problémů pomocí sledovací proces sítě Azure

toodiagnose připojení, připojte tooAzure prostředí PowerShell a zahájit hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` rutiny. Můžete najít hello podrobnosti o použití této rutiny v [řešení brány virtuální sítě a připojení - PowerShell](network-watcher-troubleshoot-manage-powershell.md). Tato rutina může trvat až toocomplete toofew minut.

Po dokončení rutiny hello, můžete přejít umístění úložiště toohello zadaný v rutině hello tooget podrobné informace o problému hello a protokoly na. Azure sledovací proces sítě se vytvoří složky zip, který obsahuje hello následující soubory protokolů:

![1][1]

Otevřete hello souboru s názvem IKEErrors.txt a zobrazí hello následující chybu, která znamená problém s místními chybné konfigurace nastavení protokolu IKE.

```
Error: On-premises device rejected Quick Mode settings. Check values.
     based on log : Peer sent NO_PROPOSAL_CHOSEN notify
```

Podrobné informace můžete získat z hello Scrubbed wfpdiag.txt o hello chybě, protože v takovém případě uvádí, že došlo `ERROR_IPSEC_IKE_POLICY_MATCH` této tooconnection realizace nepracuje správně.

Jiné běžná chyba konfigurace souvisí hello zadání nesprávné sdílených klíčů. Pokud v předchozím příkladu byl zadán jiný sdílených klíčů hello, hello IKEErrors.txt ukazuje hello následující chyba: `Error: Authentication failed. Check shared key`.

Funkce řešení Azure sledovací proces sítě vám umožní toodiagnose a řešení potíží s vaše brána sítě VPN a připojení hello snadno jednoduché rutiny prostředí PowerShell. V současné době podporují diagnostikování hello následující podmínky a pracují směrem přidáním další podmínky.

### <a name="gateway"></a>brána

| Typ chyby | Důvod | Protokol|
|---|---|---|
| NoFault | Když je zjištěna žádná chyba. |Ano|
| GatewayNotFound | Nelze najít, že není zřízený brány nebo brána. |Ne|
| PlannedMaintenance |  Instance brány je v rámci údržby.  |Ne|
| UserDrivenUpdate | Pokud je aktualizace uživatele v průběhu. To může být operace změny velikosti. | Ne |
| VipUnResponsive | Nelze kontaktovat hello primární instance hello brány. To se stane, když selže test stavu hello. | Ne |
| PlatformInActive | Nastane problém s platformou hello. | Ne|
| ServiceNotRunning | Hello základní služba není spuštěna. | Ne|
| NoConnectionsFoundForGateway | Žádná připojení existuje v bráně hello. Toto je pouze upozornění.| Ne|
| ConnectionsNotConnected | Žádná z hello připojení jsou připojené. Toto je pouze upozornění.| Ano|
| GatewayCPUUsageExceeded | Hello aktuální využití brány využití procesoru je > 95 %. | Ano |

### <a name="connection"></a>Připojení

| Typ chyby | Důvod | Protokol|
|---|---|---|
| NoFault | Když je zjištěna žádná chyba. |Ano|
| GatewayNotFound | Nelze najít, že není zřízený brány nebo brána. |Ne|
| PlannedMaintenance | Instance brány je v rámci údržby.  |Ne|
| UserDrivenUpdate | Pokud je aktualizace uživatele v průběhu. To může být operace změny velikosti.  | Ne |
| VipUnResponsive | Nelze kontaktovat hello primární instance hello brány. Ho se stane, když selže test stavu hello. | Ne |
| ConnectionEntityNotFound | Konfigurace připojení nebyl nalezen. | Ne |
| ConnectionIsMarkedDisconnected | Hello připojení je označena "odpojen." |Ne|
| ConnectionNotConfiguredOnGateway | základní služby Hello nemá hello nakonfigurováno připojení. | Ano |
| ConnectionMarkedStandy | podkladová služba Hello je označena jako pohotovostní režim.| Ano|
| Authentication | Neshoda předsdílený klíč. | Ano|
| PeerReachability | Hello sdílené brány není dostupný. | Ano|
| IkePolicyMismatch | Hello sdílené brány má IKE zásady, které nejsou podporované službou Azure. | Ano|
| Chyba WfpParse | Došlo k chybě analýzy protokolů Ochrana souborů systému Windows hello. |Ano|

## <a name="next-steps"></a>Další kroky

Další toocheck připojení VPN Gateway pomocí prostředí PowerShell a automatizace Azure. navštivte stránky [monitorování VPN Gateway se řešení potíží s sledovací proces sítě Azure](network-watcher-monitor-with-azure-automation.md)

[1]: ./media/network-watcher-diagnose-on-premises-connectivity/figure1.png
