---
title: "Ukázky konfigurace směrovače zákazníka ExpressRoute | Microsoft Docs"
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
ms.openlocfilehash: 032e584dc5abf59e9e3e8d80673b402f1fbf721b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="router-configuration-samples-to-set-up-and-manage-routing"></a><span data-ttu-id="0a261-103">Ukázky konfigurace směrovače nastavit a spravovat směrování</span><span class="sxs-lookup"><span data-stu-id="0a261-103">Router configuration samples to set up and manage routing</span></span>
<span data-ttu-id="0a261-104">Tato stránka obsahuje rozhraní a směrování ukázky konfigurace pro Cisco IOS-XE a Juniper MX směrovače řady.</span><span class="sxs-lookup"><span data-stu-id="0a261-104">This page provides interface and routing configuration samples for Cisco IOS-XE and Juniper MX series routers.</span></span> <span data-ttu-id="0a261-105">Tyto by měla být ukázky jenom pokyny a nesmí se používat, protože je.</span><span class="sxs-lookup"><span data-stu-id="0a261-105">These are intended to be samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="0a261-106">Můžete pracovat s vaším dodavatelem spolu s odpovídající konfigurací pro vaši síť.</span><span class="sxs-lookup"><span data-stu-id="0a261-106">You can work with your vendor to come up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="0a261-107">Ukázky na této stránce by měla být čistě pokyny.</span><span class="sxs-lookup"><span data-stu-id="0a261-107">Samples in this page are intended to be purely for guidance.</span></span> <span data-ttu-id="0a261-108">Musíte pracovat se od dodavatele prodeje / technické vaší síťových adaptérů a spolu s odpovídající konfigurací podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="0a261-108">You must work with your vendor's sales / technical team and your networking team to come up with appropriate configurations to meet your needs.</span></span> <span data-ttu-id="0a261-109">Problémy související s konfigurací, které jsou uvedené na této stránce nebudou podpory společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0a261-109">Microsoft will not support issues related to configurations listed in this page.</span></span> <span data-ttu-id="0a261-110">Pro problémy podpory, bude nutné kontaktovat dodavatele zařízení.</span><span class="sxs-lookup"><span data-stu-id="0a261-110">You must contact your device vendor for support issues.</span></span>
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a><span data-ttu-id="0a261-111">Nastavení jednotek MTU a MSS protokolu TCP na rozhraní směrovače</span><span class="sxs-lookup"><span data-stu-id="0a261-111">MTU and TCP MSS settings on router interfaces</span></span>
* <span data-ttu-id="0a261-112">MTU pro rozhraní ExpressRoute je 1500, což je typický výchozí MTU pro rozhraní sítě Ethernet na směrovač.</span><span class="sxs-lookup"><span data-stu-id="0a261-112">The MTU for the ExpressRoute interface is 1500, which is the typical default MTU for an Ethernet interface on a router.</span></span> <span data-ttu-id="0a261-113">Pokud ve výchozím nastavení má směrovač jiné MTU, není nutné zadat hodnotu na rozhraní směrovače.</span><span class="sxs-lookup"><span data-stu-id="0a261-113">Unless your router has a different MTU by default, there is no need to specify a value on the router interface.</span></span>
* <span data-ttu-id="0a261-114">Na rozdíl od služby Azure VPN Gateway MSS protokolu TCP pro okruh ExpressRoute nemusí být zadaný.</span><span class="sxs-lookup"><span data-stu-id="0a261-114">Unlike an Azure VPN Gateway, the TCP MSS for an ExpressRoute circuit does not need to be specified.</span></span>

<span data-ttu-id="0a261-115">Následující ukázky směrovač konfigurace platí pro všechny partnerské vztahy.</span><span class="sxs-lookup"><span data-stu-id="0a261-115">Router configuration samples below apply to all peerings.</span></span> <span data-ttu-id="0a261-116">Zkontrolujte [partnerských vztahů ExpressRoute](expressroute-circuit-peerings.md) a [požadavky na směrování služby ExpressRoute](expressroute-routing.md) Další informace o směrování.</span><span class="sxs-lookup"><span data-stu-id="0a261-116">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute routing requirements](expressroute-routing.md) for more details on routing.</span></span>


## <a name="cisco-ios-xe-based-routers"></a><span data-ttu-id="0a261-117">Cisco IOS-XE na základě směrovače</span><span class="sxs-lookup"><span data-stu-id="0a261-117">Cisco IOS-XE based routers</span></span>
<span data-ttu-id="0a261-118">Ukázky v této části se vztahují na všechny směrovač se systémem IOS XE operačních systémů.</span><span class="sxs-lookup"><span data-stu-id="0a261-118">The samples in this section apply for any router running the IOS-XE OS family.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="0a261-119">1. Konfigurace rozhraní a dílčí rozhraní</span><span class="sxs-lookup"><span data-stu-id="0a261-119">1. Configuring interfaces and sub-interfaces</span></span>
<span data-ttu-id="0a261-120">Budete potřebovat dílčí rozhraní na partnerský vztah v každé směrovače se připojíte k Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="0a261-120">You will require a sub interface per peering in every router you connect to Microsoft.</span></span> <span data-ttu-id="0a261-121">Sub – rozhraní možné identifikovat pomocí ID sítě VLAN nebo pár skládaný ID sítě VLAN a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0a261-121">A sub interface can be identified with a VLAN ID or a stacked pair of VLAN IDs and an IP address.</span></span>

<span data-ttu-id="0a261-122">**Definice rozhraní Dot1Q**</span><span class="sxs-lookup"><span data-stu-id="0a261-122">**Dot1Q interface definition**</span></span>

<span data-ttu-id="0a261-123">Tato ukázka obsahuje definice dílčí rozhraní pro dílčí rozhraní s jeden ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="0a261-123">This sample provides the sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="0a261-124">ID sítě VLAN je jedinečný pro každého partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="0a261-124">The VLAN ID is unique per peering.</span></span> <span data-ttu-id="0a261-125">Poslední oktet vaši adresu IPv4, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="0a261-125">The last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

<span data-ttu-id="0a261-126">**Definice rozhraní QinQ**</span><span class="sxs-lookup"><span data-stu-id="0a261-126">**QinQ interface definition**</span></span>

<span data-ttu-id="0a261-127">Tato ukázka obsahuje definice dílčí rozhraní pro dílčí rozhraní se dva identifikátory ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="0a261-127">This sample provides the sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="0a261-128">Vnější ID sítě VLAN (s-tag), pokud se používá zůstává stejná napříč všech partnerských vztahů.</span><span class="sxs-lookup"><span data-stu-id="0a261-128">The outer VLAN ID (s-tag), if used remains the same across all the peerings.</span></span> <span data-ttu-id="0a261-129">Vnitřní ID sítě VLAN (c-tag) je jedinečný pro každého partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="0a261-129">The inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="0a261-130">Poslední oktet vaši adresu IPv4, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="0a261-130">The last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="0a261-131">2. Nastavení relace eBGP</span><span class="sxs-lookup"><span data-stu-id="0a261-131">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="0a261-132">Musíte nastavit relace protokolu BGP se společností Microsoft pro každý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="0a261-132">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="0a261-133">Následující ukázka umožňuje nastavit relace protokolu BGP se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0a261-133">The sample below enables you to setup a BGP session with Microsoft.</span></span> <span data-ttu-id="0a261-134">Pokud adresu IPv4, která jste použili pro vaše rozhraní dílčí a.b.c.d, IP adresu BGP sousedním (Microsoft) bude a.b.c.d+1.</span><span class="sxs-lookup"><span data-stu-id="0a261-134">If the IPv4 address you used for your sub interface was a.b.c.d, the IP address of the BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="0a261-135">Poslední oktet sousedním BGP adresy IPv4, bude vždy sudé číslo.</span><span class="sxs-lookup"><span data-stu-id="0a261-135">The last octet of the BGP neighbor's IPv4 address will always be an even number.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a><span data-ttu-id="0a261-136">3. Nastavení předpony chcete inzerovat přes relaci protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="0a261-136">3. Setting up prefixes to be advertised over the BGP session</span></span>
<span data-ttu-id="0a261-137">Můžete nakonfigurovat směrovači inzerovat vyberte předpony společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0a261-137">You can configure your router to advertise select prefixes to Microsoft.</span></span> <span data-ttu-id="0a261-138">Můžete tak učinit pomocí následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="0a261-138">You can do so using the sample below.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a><span data-ttu-id="0a261-139">4. Mapuje trasy</span><span class="sxs-lookup"><span data-stu-id="0a261-139">4. Route maps</span></span>
<span data-ttu-id="0a261-140">Můžete použít mapy trasy a předponu seznamu filtru předpony rozšíří do vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="0a261-140">You can use route-maps and prefix lists to filter prefixes propagated into your network.</span></span> <span data-ttu-id="0a261-141">Následující ukázky můžete použít k provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="0a261-141">You can use the sample below to accomplish the task.</span></span> <span data-ttu-id="0a261-142">Ujistěte se, že máte příslušné předpony zobrazí instalační program.</span><span class="sxs-lookup"><span data-stu-id="0a261-142">Ensure that you have appropriate prefix lists setup.</span></span>

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


## <a name="juniper-mx-series-routers"></a><span data-ttu-id="0a261-143">Směrovače Juniper MX řady</span><span class="sxs-lookup"><span data-stu-id="0a261-143">Juniper MX series routers</span></span>
<span data-ttu-id="0a261-144">Ukázky v této části se vztahují na veškeré směrovače Juniper MX řady.</span><span class="sxs-lookup"><span data-stu-id="0a261-144">The samples in this section apply for any Juniper MX series routers.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="0a261-145">1. Konfigurace rozhraní a dílčí rozhraní</span><span class="sxs-lookup"><span data-stu-id="0a261-145">1. Configuring interfaces and sub-interfaces</span></span>

<span data-ttu-id="0a261-146">**Definice rozhraní Dot1Q**</span><span class="sxs-lookup"><span data-stu-id="0a261-146">**Dot1Q interface definition**</span></span>

<span data-ttu-id="0a261-147">Tato ukázka obsahuje definice dílčí rozhraní pro dílčí rozhraní s jeden ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="0a261-147">This sample provides the sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="0a261-148">ID sítě VLAN je jedinečný pro každého partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="0a261-148">The VLAN ID is unique per peering.</span></span> <span data-ttu-id="0a261-149">Poslední oktet vaši adresu IPv4, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="0a261-149">The last octet of your IPv4 address will always be an odd number.</span></span>

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


<span data-ttu-id="0a261-150">**Definice rozhraní QinQ**</span><span class="sxs-lookup"><span data-stu-id="0a261-150">**QinQ interface definition**</span></span>

<span data-ttu-id="0a261-151">Tato ukázka obsahuje definice dílčí rozhraní pro dílčí rozhraní se dva identifikátory ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="0a261-151">This sample provides the sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="0a261-152">Vnější ID sítě VLAN (s-tag), pokud se používá zůstává stejná napříč všech partnerských vztahů.</span><span class="sxs-lookup"><span data-stu-id="0a261-152">The outer VLAN ID (s-tag), if used remains the same across all the peerings.</span></span> <span data-ttu-id="0a261-153">Vnitřní ID sítě VLAN (c-tag) je jedinečný pro každého partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="0a261-153">The inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="0a261-154">Poslední oktet vaši adresu IPv4, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="0a261-154">The last octet of your IPv4 address will always be an odd number.</span></span>

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

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="0a261-155">2. Nastavení relace eBGP</span><span class="sxs-lookup"><span data-stu-id="0a261-155">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="0a261-156">Musíte nastavit relace protokolu BGP se společností Microsoft pro každý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="0a261-156">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="0a261-157">Následující ukázka umožňuje nastavit relace protokolu BGP se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0a261-157">The sample below enables you to setup a BGP session with Microsoft.</span></span> <span data-ttu-id="0a261-158">Pokud adresu IPv4, která jste použili pro vaše rozhraní dílčí a.b.c.d, IP adresu BGP sousedním (Microsoft) bude a.b.c.d+1.</span><span class="sxs-lookup"><span data-stu-id="0a261-158">If the IPv4 address you used for your sub interface was a.b.c.d, the IP address of the BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="0a261-159">Poslední oktet sousedním BGP adresy IPv4, bude vždy sudé číslo.</span><span class="sxs-lookup"><span data-stu-id="0a261-159">The last octet of the BGP neighbor's IPv4 address will always be an even number.</span></span>

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

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a><span data-ttu-id="0a261-160">3. Nastavení předpony chcete inzerovat přes relaci protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="0a261-160">3. Setting up prefixes to be advertised over the BGP session</span></span>
<span data-ttu-id="0a261-161">Můžete nakonfigurovat směrovači inzerovat vyberte předpony společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0a261-161">You can configure your router to advertise select prefixes to Microsoft.</span></span> <span data-ttu-id="0a261-162">Můžete tak učinit pomocí následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="0a261-162">You can do so using the sample below.</span></span>

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


### <a name="4-route-maps"></a><span data-ttu-id="0a261-163">4. Mapuje trasy</span><span class="sxs-lookup"><span data-stu-id="0a261-163">4. Route maps</span></span>
<span data-ttu-id="0a261-164">Můžete použít mapy trasy a předponu seznamu filtru předpony rozšíří do vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="0a261-164">You can use route-maps and prefix lists to filter prefixes propagated into your network.</span></span> <span data-ttu-id="0a261-165">Následující ukázky můžete použít k provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="0a261-165">You can use the sample below to accomplish the task.</span></span> <span data-ttu-id="0a261-166">Ujistěte se, že máte příslušné předpony zobrazí instalační program.</span><span class="sxs-lookup"><span data-stu-id="0a261-166">Ensure that you have appropriate prefix lists setup.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="0a261-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0a261-167">Next Steps</span></span>
<span data-ttu-id="0a261-168">Další podrobnosti najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="0a261-168">See the [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>

