---
title: "Ukázky konfigurace směrovače zákazníka aaaExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje ukázky konfigurace směrovače pro směrovače Cisco a Juniper."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 564826bc-017a-4683-a385-37c9fa814948
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: 5c91f24e6082e01c3e8df91b4fcfda46a6c29fa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="router-configuration-samples-tooset-up-and-manage-routing"></a>Konfigurace směrovače ukázky tooset nahoru a spravovat směrování
Tato stránka obsahuje rozhraní a směrování ukázky konfigurace pro Cisco IOS-XE a Juniper MX směrovače řady. Tyto jsou ukázky určený toobe pouze pokyny a nesmí se používat, protože je. Můžete pracovat s vaší toocome dodavatele s odpovídající konfigurací pro vaši síť. 

> [!IMPORTANT]
> Ukázky na této stránce jsou toobe určený výhradně pro pokyny. Musíte pracovat se tým prodeje / technické od dodavatele a vaší sítě team toocome až s odpovídající konfigurací toomeet vašim potřebám. Microsoft nebude podporovat problémy související s tooconfigurations uvedené na této stránce. Pro problémy podpory, bude nutné kontaktovat dodavatele zařízení.
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a>Nastavení jednotek MTU a MSS protokolu TCP na rozhraní směrovače
* Hello MTU pro rozhraní ExpressRoute hello je 1500, což je typický výchozí hello MTU pro rozhraní sítě Ethernet na směrovač. Pokud ve výchozím nastavení má směrovač jiné MTU, je bez nutnosti toospecify hodnotu na rozhraní směrovače hello.
* Na rozdíl od služby Azure VPN Gateway hello MSS protokolu TCP pro okruh ExpressRoute není nutné toobe zadán.

Následující ukázky konfigurace směrovače použít tooall partnerských vztahů. Zkontrolujte [partnerských vztahů ExpressRoute](expressroute-circuit-peerings.md) a [požadavky na směrování služby ExpressRoute](expressroute-routing.md) Další informace o směrování.


## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE na základě směrovače
Ukázky Hello v této části se vztahují na všechny směrovač se systémem IOS XE operačních systémů hello.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Konfigurace rozhraní a dílčí rozhraní
Budete potřebovat dílčí rozhraní na partnerský vztah v každé směrovače můžete připojit tooMicrosoft. Sub – rozhraní možné identifikovat pomocí ID sítě VLAN nebo pár skládaný ID sítě VLAN a IP adresu.

**Definice rozhraní Dot1Q**

Tato ukázka poskytuje hello Definice dílčí rozhraní pro dílčí rozhraní s jeden ID sítě VLAN. Hello ID sítě VLAN je jedinečný pro každého partnerského vztahu. poslední oktet Hello vaší adresy IPv4, bude vždy lichý počet.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

**Definice rozhraní QinQ**

Tato ukázka obsahuje hello Definice dílčí rozhraní pro dílčí rozhraní se dva identifikátory ID sítě VLAN. Dobrý den, který zůstane vnější ID sítě VLAN (s-tag), pokud se používá stejné hello napříč všech partnerských vztahů hello. vnitřní Hello ID sítě VLAN (c-tag) je jedinečný pro každého partnerského vztahu. poslední oktet Hello vaší adresy IPv4, bude vždy lichý počet.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a>2. Nastavení relace eBGP
Musíte nastavit relace protokolu BGP se společností Microsoft pro každý partnerský vztah. Následující ukázka Hello umožňuje toosetup relaci protokolu BGP se společností Microsoft. Pokud hello IPv4 adresu, která jste použili pro vaše rozhraní dílčí a.b.c.d, hello IP adresu BGP sousedním hello (Microsoft) bude a.b.c.d+1. poslední oktet Hello sousedním BGP hello adresy IPv4, bude vždy sudé číslo.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a>3. Nastavení toobe předpon inzerovaných během relace protokolu BGP hello
Můžete nakonfigurovat tooMicrosoft vyberte předpony tooadvertise vašeho směrovače. Můžete toho dosáhnout pomocí hello následující ukázka.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. Mapuje trasy
Můžete použít mapy trasy a předponu uvádí toofilter předpony rozšíří do vaší sítě. Můžete použít ukázkové hello níže tooaccomplish hello úloh. Ujistěte se, že máte příslušné předpony zobrazí instalační program.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Směrovače Juniper MX řady
Ukázky Hello v této části se vztahují na veškeré směrovače Juniper MX řady.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Konfigurace rozhraní a dílčí rozhraní

**Definice rozhraní Dot1Q**

Tato ukázka poskytuje hello Definice dílčí rozhraní pro dílčí rozhraní s jeden ID sítě VLAN. Hello ID sítě VLAN je jedinečný pro každého partnerského vztahu. poslední oktet Hello vaší adresy IPv4, bude vždy lichý počet.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


**Definice rozhraní QinQ**

Tato ukázka obsahuje hello Definice dílčí rozhraní pro dílčí rozhraní se dva identifikátory ID sítě VLAN. Dobrý den, který zůstane vnější ID sítě VLAN (s-tag), pokud se používá stejné hello napříč všech partnerských vztahů hello. vnitřní Hello ID sítě VLAN (c-tag) je jedinečný pro každého partnerského vztahu. poslední oktet Hello vaší adresy IPv4, bude vždy lichý počet.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. Nastavení relace eBGP
Musíte nastavit relace protokolu BGP se společností Microsoft pro každý partnerský vztah. Následující ukázka Hello umožňuje toosetup relaci protokolu BGP se společností Microsoft. Pokud hello IPv4 adresu, která jste použili pro vaše rozhraní dílčí a.b.c.d, hello IP adresu BGP sousedním hello (Microsoft) bude a.b.c.d+1. poslední oktet Hello sousedním BGP hello adresy IPv4, bude vždy sudé číslo.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a>3. Nastavení toobe předpon inzerovaných během relace protokolu BGP hello
Můžete nakonfigurovat tooMicrosoft vyberte předpony tooadvertise vašeho směrovače. Můžete toho dosáhnout pomocí hello následující ukázka.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. Mapuje trasy
Můžete použít mapy trasy a předponu uvádí toofilter předpony rozšíří do vaší sítě. Můžete použít ukázkové hello níže tooaccomplish hello úloh. Ujistěte se, že máte příslušné předpony zobrazí instalační program.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Další kroky
V tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) další podrobnosti.

