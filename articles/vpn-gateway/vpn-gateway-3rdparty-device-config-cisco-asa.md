---
title: "Konfigurace aaaSample – zařízení Cisco ASA připojení brány VPN tooAzure | Microsoft Docs"
description: "Tento článek obsahuje ukázková konfigurace pro zařízení Cisco ASA připojení brány VPN tooAzure."
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
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: dad13e02afe8dad2379db750eb09602e08e8ea99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Ukázková konfigurace: Cisco ASA zařízení (BGP IKEv2/ne)
Tento článek obsahuje ukázkové konfigurace pro zařízení Cisco ASA připojení brány VPN tooAzure.

## <a name="device-at-a-glance"></a>Zařízení na první pohled

|                        |                                   |
| ---                    | ---                               |
| Výrobce zařízení          | Cisco                             |
| Model zařízení           | ASA (adaptivní zabezpečení zařízení) |
| Cílová verze         | 8.4+                              |
| Otestované modelu           | ASA 5505                          |
| Otestované verze         | 9.2                               |
| Verze IKE            | **IKEv2**                         |
| Protokol BGP                    | **Ne**                            |
| Typ brány Azure VPN | **Založené na trasách** brána sítě VPN       |
|                        |                                   |

> [!NOTE]
> 1. Hello konfigurace níže se připojuje tooan zařízení Cisco ASA Azure **založené na trasách** VPN gateway pomocí vlastních zásad protokolu IPsec/IKE s možností "UserPolicyBasedTrafficSelectors", jak je popsáno v [v tomto článku](vpn-gateway-connect-multiple-policybased-rm-ps.md).
> 2. Vyžaduje ASA zařízení toouse **IKEv2** s konfigurací na základě přístup k seznamu není na základě VTI.
> 3. Přečtěte si vaše specifikace dodavatele zařízení VPN, které zásady tooensure hello je podporované na vaše místní zařízení VPN.

## <a name="vpn-device-requirements"></a>Požadavky na zařízení VPN
Azure VPN Gateway pomocí standardního protokolu IPsec/IKE protokol sady tooestablish S2S VPN tunely. Odkazovat příliš[informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md) hello podrobné parametry protokolu IPsec/IKE a výchozí kryptografické algoritmy pro Azure VPN Gateway. Volitelně můžete zadat přesný kombinace hello kryptografické algoritmy a klíče síly pro konkrétní připojení jak je popsáno v [o požadavcích na kryptografických](vpn-gateway-about-compliance-crypto.md). Pokud vyberete konkrétní kombinaci kryptografické algoritmy a klíče síly, Zkontrolujte prosím, že používáte odpovídající specifikace hello na vaše zařízení VPN.

## <a name="single-vpn-tunnel"></a>Jedno tunelové propojení sítě VPN
Tato topologie se skládá z jednoho tunelu S2S VPN mezi bránu VPN Azure VPN a vaše místní zařízení VPN. Volitelně můžete nakonfigurovat protokol BGP mezi hello tunelového připojení sítě VPN.

![jedno tunelové propojení](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Odkazovat příliš[jedním tunelem instalace](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) podrobné naleznete podrobné pokyny toobuild hello konfigurace Azure.

### <a name="network-and-vpn-gateway-information"></a>Informace o bráně sítě a sítě VPN
V této části seznamu hello parametry pro hello této ukázce.

| **Parametr**                | **Hodnota**                    |
| ---                          | ---                          |
| Předpony adres sítí VNet        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure IP adresa brány sítě VPN         | Azure_Gateway_Public_IP      |
| Předpony adres místní | 10.51.0.0/16<br>10.52.0.0/16 |
| IP adresa zařízení místní sítě VPN    | OnPrem_Device_Public_IP     |
| * Protokol BGP ASN virtuální sítě                | 65010                        |
| * Azure IP adresa partnera BGP           | 10.12.255.30                 |
| * Místní ASN protokolu BGP         | 65050                        |
| * IP adresa partnera BGP na místě     | 10.52.255.254                |
|                              |                              |

* (*) Volitelné parametry pro protokol BGP jenom.

### <a name="ipsecike-policy--parameters"></a>Zásady protokolu IPsec/IKE a parametry

Následující tabulka Hello uvádí algoritmy hello protokolu IPsec/IKE a parametry použité v ukázce hello. Přečtěte si vaše se, že všechny algoritmy, které jsou uvedené výše jsou podporována modely zařízení VPN a verzí firmwaru VPN toomake specifikace zařízení.

| **IPsec/IKEv2**  | **Hodnota**                            |
| ---              | ---                                  |
| Šifrování protokolem IKEv2 | AES256                               |
| Integrita protokolu IKEv2  | SHA384                               |
| Skupina DH         | DHGroup24                            |
| Šifrování protokolem IPsec | AES256                               |
| Integrita protokolu IPsec  | SHA1                                 |
| Skupina PFS        | PFS24                                |
| Doba života přidružení zabezpečení v rychlém režimu   | 7200 sekund                         |
| Selektor provozu | UsePolicyBasedTrafficSelectors $True |
| Předsdílený klíč   | PreSharedKey                         |
|                  |                                      |

- (*) Na některých zařízeních musí být integrity protokolu IPsec "null", pokud GCM AES se používá jako šifrovací algoritmus protokolu IPsec.

### <a name="device-notes"></a>Poznámky k zařízení

>[!NOTE]
>
> 1. Má podporu protokolu IKEv2 vyžaduje ASA verze 8.4 a vyšší.
> 2. Podpora skupiny DH vyšší a PFS (kromě skupiny 5) vyžaduje verzi ASA 9.x.
> 3. Šifrování pomocí protokolu IPsec s AES-GCM a protokolu IPsec integrita s SHA-256, SHA-384 a SHA-512 podporu vyžaduje verzi ASA 9.x na novější ASA hardwaru; 5505 ASA, 5510, 5520, 5540, 5550, jsou 5580 **není** podporována. (Zkontrolujte hello dodavatele specifikace tooconfirm.)
>


### <a name="sample-device-configurations"></a>Ukázka konfigurace zařízení
níže uvedený skript Hello poskytuje ukázkové konfiguraci podle hello topologie a parametry uvedené výše. Konfigurace tunel S2S VPN Hello se skládá z hello následující části:

1. Rozhraní & trasy
2. Seznamy přístupu
3. Zásady IKE a parametry (fáze 1 nebo hlavního režimu)
4. Parametry (fáze 2 nebo rychlého režimu) a zásad protokolu IPsec
5. Další parametry (upínací MSS protokolu TCP, atd.)

>[!IMPORTANT] 
>Zkontrolujte prosím, že dokončení hello níže uvedené další konfigurace a nahraďte zástupné symboly hello hello skutečnými hodnotami:
> 
> - Konfigurace rozhraní pro i mimo ně rozhraní
> - Trasy pro vaše uvnitř a privátního a mimo nebo veřejné sítě
> - Zajistěte, všechny názvy a zásad čísla jedinečná na zařízení hello
> - Ujistěte se, že hello kryptografické algoritmy jsou podporovány ve vašem zařízení
> - Nahraďte hello následující zástupného skutečnými hodnotami hello
>   - Mimo název rozhraní: "mimo"
>   - Azure_Gateway_Public_IP
>   - OnPrem_Device_Public_IP
>   - IKE Pre_Shared_Key
>   - Virtuální síť a názvy brány místní sítě (VNetName, LNGName)
>   - Virtuální síť a místní sítě předpony adres
>   - Správné masky sítě

#### <a name="sample-configuration"></a>Ukázková konfigurace

```
! Sample ASA configuration for connecting tooAzure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace hello following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - hello Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with hello actual nexthop IP address
!
! (*) Must be unique names in hello device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on hello outside interface or vlan
!     > <PrivateIPAddress> on hello inside interface or vlan; e.g., 10.51.0.1/24
!     > Route tooconnect too<Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between hello ASA and hello Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding toohello <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines hello on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify hello access-list between hello Azure VNet and your on-premises network.
!       This access list defines hello IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between hello on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure hello policy number is not used
!       - integrity and prf must be hello same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec protposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as hello crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned tooyour outside interface, you must use
!         hello same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses hello access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses hello proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS too1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a>Jednoduché ladění příkazy

Zde jsou některé příkazy ASA pro účely ladění:

1. Zobrazit hello IPsec a přidružení zabezpečení protokolu IKE
    - "Zobrazit kryptografických ipsec sa"
    - "Zobrazit šifrování sa protokolu ikev2"
2. Vstup režim ladění – to můžete získat v konzole hello velmi aktivní
    - "ladění kryptografických ikev2 platformy <level>"
    - "ladění protokolu ikev2 kryptografických <level>"
3. Aktuální konfigurace seznamu
    - "spuštění zobrazit" – ukazuje hello aktuální konfigurace na hello zařízení; můžete použít různé dílčí příkazy toolist konkrétní části hello konfigurace hello. Například "Zobrazit spustit kryptografických", "spustit zobrazit seznam přístupu", "Zobrazit spustit tunel skupina", atd.


## <a name="next-steps"></a>Další kroky
V tématu [konfiguraci brány VPN aktivní-aktivní pro mezi různými místy a připojení VNet-to-VNet](vpn-gateway-activeactive-rm-powershell.md) kroky tooconfigure aktivní aktivní mezi různými místy a připojení VNet-to-VNet.

