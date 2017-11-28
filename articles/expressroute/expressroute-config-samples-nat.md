---
title: "Ukázky konfigurace směrovače zákazníka aaaExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje ukázky konfigurace směrovače pro směrovače Cisco a Juniper."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: d6ea716f-d5ee-4a61-92b0-640d6e7d6974
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: b5faca0666bda6173e54abb0b6560d5f8bf8bfc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="router-configuration-samples-tooset-up-and-manage-nat"></a><span data-ttu-id="83a6e-103">Konfigurace směrovače ukázky tooset nahoru a spravovat NAT</span><span class="sxs-lookup"><span data-stu-id="83a6e-103">Router configuration samples tooset up and manage NAT</span></span>
<span data-ttu-id="83a6e-104">Tato stránka obsahuje ukázky konfigurace NAT pro směrovače Cisco ASA a Juniper SRX řady.</span><span class="sxs-lookup"><span data-stu-id="83a6e-104">This page provides NAT configuration samples for Cisco ASA and Juniper SRX series routers.</span></span> <span data-ttu-id="83a6e-105">Tyto jsou ukázky určený toobe pouze pokyny a nesmí se používat, protože je.</span><span class="sxs-lookup"><span data-stu-id="83a6e-105">These are intended toobe samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="83a6e-106">Můžete pracovat s vaší toocome dodavatele s odpovídající konfigurací pro vaši síť.</span><span class="sxs-lookup"><span data-stu-id="83a6e-106">You can work with your vendor toocome up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="83a6e-107">Ukázky na této stránce jsou toobe určený výhradně pro pokyny.</span><span class="sxs-lookup"><span data-stu-id="83a6e-107">Samples in this page are intended toobe purely for guidance.</span></span> <span data-ttu-id="83a6e-108">Musíte pracovat se tým prodeje / technické od dodavatele a vaší sítě team toocome až s odpovídající konfigurací toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="83a6e-108">You must work with your vendor's sales / technical team and your networking team toocome up with appropriate configurations toomeet your needs.</span></span> <span data-ttu-id="83a6e-109">Microsoft nebude podporovat problémy související s tooconfigurations uvedené na této stránce.</span><span class="sxs-lookup"><span data-stu-id="83a6e-109">Microsoft will not support issues related tooconfigurations listed in this page.</span></span> <span data-ttu-id="83a6e-110">Pro problémy podpory, bude nutné kontaktovat dodavatele zařízení.</span><span class="sxs-lookup"><span data-stu-id="83a6e-110">You must contact your device vendor for support issues.</span></span>
> 
> 

* <span data-ttu-id="83a6e-111">Následující ukázky konfigurace směrovače použít partnerských vztahů tooAzure veřejné a společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="83a6e-111">Router configuration samples below apply tooAzure Public and Microsoft peerings.</span></span> <span data-ttu-id="83a6e-112">Nakonfigurujete nesmí NAT pro soukromý partnerský vztah Azure.</span><span class="sxs-lookup"><span data-stu-id="83a6e-112">You must not configure NAT for Azure private peering.</span></span> <span data-ttu-id="83a6e-113">Zkontrolujte [partnerských vztahů ExpressRoute](expressroute-circuit-peerings.md) a [požadavky ExpressRoute NAT](expressroute-nat.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="83a6e-113">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute NAT requirements](expressroute-nat.md) for more details.</span></span>

* <span data-ttu-id="83a6e-114">Je nutné použít samostatné fondy IP adres NAT pro toohello připojení k Internetu a ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="83a6e-114">You MUST use separate NAT IP pools for connectivity toohello internet and ExpressRoute.</span></span> <span data-ttu-id="83a6e-115">Pomocí stejné IP adres NAT fond napříč hello hello internet a ExpressRoute způsobí asymetrické směrování a ztráty připojení.</span><span class="sxs-lookup"><span data-stu-id="83a6e-115">Using hello same NAT IP pool across hello internet and ExpressRoute will result in asymmetric routing and loss of connectivity.</span></span>


## <a name="cisco-asa-firewalls"></a><span data-ttu-id="83a6e-116">Brány firewall Cisco ASA</span><span class="sxs-lookup"><span data-stu-id="83a6e-116">Cisco ASA firewalls</span></span>
### <a name="pat-configuration-for-traffic-from-customer-network-toomicrosoft"></a><span data-ttu-id="83a6e-117">Jan konfigurace pro provoz z tooMicrosoft sítě zákazníka</span><span class="sxs-lookup"><span data-stu-id="83a6e-117">PAT configuration for traffic from customer network tooMicrosoft</span></span>
    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>


    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>

    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>

    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>

    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2

    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-toocustomer-network"></a><span data-ttu-id="83a6e-118">Jan konfigurace pro data z Microsoft toocustomer sítě</span><span class="sxs-lookup"><span data-stu-id="83a6e-118">PAT configuration for traffic from Microsoft toocustomer network</span></span>

<span data-ttu-id="83a6e-119">**Rozhraní a směr:**</span><span class="sxs-lookup"><span data-stu-id="83a6e-119">**Interfaces and Direction:**</span></span>

    Source Interface (where hello traffic enters hello ASA): inside
    Destination Interface (where hello traffic exits hello ASA): outside

<span data-ttu-id="83a6e-120">**Konfigurace:**</span><span class="sxs-lookup"><span data-stu-id="83a6e-120">**Configuration:**</span></span>

<span data-ttu-id="83a6e-121">Fond NAT:</span><span class="sxs-lookup"><span data-stu-id="83a6e-121">NAT Pool:</span></span>

    object network outbound-PAT
        host <NAT-IP>

<span data-ttu-id="83a6e-122">Cílový Server:</span><span class="sxs-lookup"><span data-stu-id="83a6e-122">Target Server:</span></span>

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

<span data-ttu-id="83a6e-123">Objekt skupiny pro IP adresy zákazníka</span><span class="sxs-lookup"><span data-stu-id="83a6e-123">Object Group for Customer IP Addresses</span></span>

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>

    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

<span data-ttu-id="83a6e-124">NAT příkazy:</span><span class="sxs-lookup"><span data-stu-id="83a6e-124">NAT Commands:</span></span>

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a><span data-ttu-id="83a6e-125">Juniper SRX řady směrovače</span><span class="sxs-lookup"><span data-stu-id="83a6e-125">Juniper SRX series routers</span></span>
### <a name="1-create-redundant-ethernet-interfaces-for-hello-cluster"></a><span data-ttu-id="83a6e-126">1. Vytvoření redundantní rozhraní sítě Ethernet pro hello cluster</span><span class="sxs-lookup"><span data-stu-id="83a6e-126">1. Create redundant Ethernet interfaces for hello cluster</span></span>
    interfaces {
        reth0 {
            description "tooInternal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "tooMicrosoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "tooMicrosoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a><span data-ttu-id="83a6e-127">2. Vytvořte dvě zóny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="83a6e-127">2. Create two security zones</span></span>
* <span data-ttu-id="83a6e-128">Vztah důvěryhodnosti zóny pro interní sítě a Untrust zóny pro externí síť směřující hraniční směrovače</span><span class="sxs-lookup"><span data-stu-id="83a6e-128">Trust Zone for internal network and Untrust Zone for external network facing Edge Routers</span></span>
* <span data-ttu-id="83a6e-129">Přiřaďte odpovídající rozhraní toohello zóny</span><span class="sxs-lookup"><span data-stu-id="83a6e-129">Assign appropriate interfaces toohello zones</span></span>
* <span data-ttu-id="83a6e-130">Povolit službám na rozhraní hello</span><span class="sxs-lookup"><span data-stu-id="83a6e-130">Allow services on hello interfaces</span></span>

    <span data-ttu-id="83a6e-131">zabezpečení {zón {zóny zabezpečení důvěryhodnosti {-příchozí-přenosů dat hostitelského {systému services {ping;                   } protokoly {bgp;                   rozhraní}} {reth0.100;               }} Untrust zóny zabezpečení {-příchozí-přenosů dat hostitelského {systému services {ping;                   } protokoly {bgp;                   rozhraní}} {reth1.100;               }           }       }   }</span><span class="sxs-lookup"><span data-stu-id="83a6e-131">security {       zones {           security-zone Trust {               host-inbound-traffic {                   system-services {                       ping;                   }                   protocols {                       bgp;                   }               }               interfaces {                   reth0.100;               }           }           security-zone Untrust {               host-inbound-traffic {                   system-services {                       ping;                   }                   protocols {                       bgp;                   }               }               interfaces {                   reth1.100;               }           }       }   }</span></span>


### <a name="3-create-security-policies-between-zones"></a><span data-ttu-id="83a6e-132">3. Vytvoření zásad zabezpečení mezi zóny</span><span class="sxs-lookup"><span data-stu-id="83a6e-132">3. Create security policies between zones</span></span>
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a><span data-ttu-id="83a6e-133">4. Nakonfigurovat zásady NAT</span><span class="sxs-lookup"><span data-stu-id="83a6e-133">4. Configure NAT policies</span></span>
* <span data-ttu-id="83a6e-134">Vytvořte dva NAT fondy.</span><span class="sxs-lookup"><span data-stu-id="83a6e-134">Create two NAT pools.</span></span> <span data-ttu-id="83a6e-135">Jeden z toohello zákazníků společnosti Microsoft bude tooMicrosoft odchozí provoz použité tooNAT a další.</span><span class="sxs-lookup"><span data-stu-id="83a6e-135">One will be used tooNAT traffic outbound tooMicrosoft and other from Microsoft toohello customer.</span></span>
* <span data-ttu-id="83a6e-136">Vytvoření pravidel tooNAT hello odpovídající provoz</span><span class="sxs-lookup"><span data-stu-id="83a6e-136">Create rules tooNAT hello respective traffic</span></span>
  
       security {
           nat {
               source {
                   pool SNAT-To-ExpressRoute {
                       routing-instance {
                           External-ExpressRoute;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   pool SNAT-From-ExpressRoute {
                       routing-instance {
                           Internal;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   rule-set Outbound_NAT {
                       from routing-instance Internal;
                       toorouting-instance External-ExpressRoute;
                       rule SNAT-Out {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-To-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
                   rule-set Inbound-NAT {
                       from routing-instance External-ExpressRoute;
                       toorouting-instance Internal;
                       rule SNAT-In {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-From-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
               }
           }
       }

### <a name="5-configure-bgp-tooadvertise-selective-prefixes-in-each-direction"></a><span data-ttu-id="83a6e-137">5. Konfigurace protokolu BGP tooadvertise selektivní předpony v každém směru</span><span class="sxs-lookup"><span data-stu-id="83a6e-137">5. Configure BGP tooadvertise selective prefixes in each direction</span></span>
<span data-ttu-id="83a6e-138">Odkazovat toosamples v [ukázky konfigurace směrování ](expressroute-config-samples-routing.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="83a6e-138">Refer toosamples in [Routing configuration samples ](expressroute-config-samples-routing.md) page.</span></span>

### <a name="6-create-policies"></a><span data-ttu-id="83a6e-139">6. Vytvoření zásad</span><span class="sxs-lookup"><span data-stu-id="83a6e-139">6. Create policies</span></span>
    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a><span data-ttu-id="83a6e-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83a6e-140">Next steps</span></span>
<span data-ttu-id="83a6e-141">V tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="83a6e-141">See hello [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>

