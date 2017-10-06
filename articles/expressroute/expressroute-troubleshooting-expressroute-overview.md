---
title: "Ověřování připojení: Průvodce odstraňováním potíží s Azure ExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje pokyny na řešení problémů a ověření připojení tooend end okruhu ExpressRoute."
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 713c39c7eafd77a4380b2a91902a9686f2ce1d85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="verifying-expressroute-connectivity"></a>Ověření připojení ExpressRoute
ExpressRoute, které zasahuje do místní sítě do hello cloudu Microsoftu přes privátní připojení, které usnadňují poskytovatele připojení, zahrnuje následující tři odlišné sítě zóny hello:

-   Své sítě.
-   Síti poskytovatele
-   Microsoft Datacenter

Hello účelem tohoto dokumentu je toohelp uživatele tooidentify kde (nebo i v případě) existuje problém s připojením a v rámci které zónu, a tím tooseek nápovědy odpovídající team tooresolve hello problém. Pokud podporu společnosti Microsoft je potřebné tooresolve problém, otevřete lístek podpory s [Microsoft Support][Support].

> [!IMPORTANT]
> Tento dokument je určený toohelp diagnostikovat a odstraňovat problémy jednoduché. Není určený toobe náhradní server pro podporu společnosti Microsoft. Otevřete lístek podpory s [Microsoft Support] [ Support] Pokud jste problém hello nelze toosolve pomocí pokynů hello.
>
>

## <a name="overview"></a>Přehled
Hello následující diagram znázorňuje hello připojení logické sítě tooMicrosoft sítě zákazníka pomocí ExpressRoute.
[![1]][1]

V předchozím diagramu hello hello čísla označují bodů klíče sítě. Hello bodů sítě jsou odkazovány často prostřednictvím tohoto článku podle jejich přidružené číslo.

V závislosti na připojení ExpressRoute hello modelu (cloudu Exchange společné umístění, připojení Ethernet typu Point-to-Point nebo Any-to-any (IPVPN)) hello bodů sítě 3 a 4 může být přepínače (vrstvy 2 zařízení). Ilustrovaný bodů Hello klíče sítě jsou následující:

1.  Zákazník výpočetní zařízení (například server nebo počítače)
2.  Webovou službu zápis certifikátů: Hraniční směrovače zákazníka 
3.  PEs (CE přístupem): zprostředkovatele hraniční směrovače nebo přepínače, které stojí hraniční směrovače zákazníka. Označuje tooas PE CEs v tomto dokumentu.
4.  PEs (MSEE přístupem): zprostředkovatele hraniční směrovače nebo přepínače, které stojí Msee. Označuje tooas PE Msee v tomto dokumentu.
5.  Msee: Microsoft Edge Enterprise (MSEE) ExpressRoute směrovači
6.  Brána virtuální sítě (VNet)
7.  Výpočetní zařízení na hello virtuální síť Azure

Pokud se používají modelů připojení cloudu Exchange společné umístění nebo připojení k síti Ethernet typu Point-to-Point hello, by vytvořit hraniční směrovač zákazníka hello (2) s Msee (5) partnerského vztahu protokolu BGP. Bodů sítě 3 a 4 by stále existují, ale poněkud transparentní jako zařízení vrstvy 2.

Pokud se používá model připojení hello Any-to-any (IPVPN), hello PEs (MSEE přístupem) (4) by navázat s Msee (5) partnerského vztahu protokolu BGP. Směrování by pak rozšíří back toohello zákazníka sítě prostřednictvím síti poskytovatele služeb IPVPN hello.

>[!NOTE]
>Pro zajištění vysoké dostupnosti ExpressRoute vyžaduje Microsoft redundantní dvojici relací protokolu BGP mezi Msee (5) a PE-Msee (4). Redundantní dvojici síťových cest je také podporována mezi síti zákazníka a PE CEs. Však v modelu připojení Any-to-any (IPVPN), jeden CE (2) může být zařízení připojená tooone nebo další PEs (3).
>
>

toovalidate okruh ExpressRoute, hello následující kroky jsou popsané (s bodu sítě hello indikován hello související číslo):
1. [Ověření zřizování okruhů a stavu (5)](#validate-circuit-provisioning-and-state)
2. [Ověření alespoň jeden ExpressRoute partnerského vztahu je nakonfigurovaný (5)](#validate-peering-configuration)
3. [Ověření protokolu ARP mezi poskytovatele služeb společnosti Microsoft a hello (propojení mezi 4 a 5)](#validate-arp-between-microsoft-and-the-service-provider)
4. [Ověření protokolu BGP a trasy na hello MSEE (BGP mezi 4 too5 a 5 too6, pokud je připojený virtuální sítě)](#validate-bgp-and-routes-on-the-msee)
5. [Zkontrolujte hello statistiku provozu (provoz procházející 5)](#check-the-traffic-statistics)

Další ověření a kontroly přidá v hello zpět budoucí, zkontrolujte měsíčně!

##<a name="validate-circuit-provisioning-and-state"></a>Zřizování okruhů a stavu ověření
Bez ohledu na to hello modelu připojení, okruh ExpressRoute má toobe vytvořili a proto služby klíč vygenerovaný pro zřizování okruhů. Zřizování okruh ExpressRoute vytváří redundantní připojení vrstvy 2 mezi PE-Msee (4) a Msee (5). Další informace o tom, jak toocreate, upravit, poskytnout a ověřit okruh ExpressRoute najdete v článku hello [vytvoření a úprava okruhu ExpressRoute][CreateCircuit].

>[!TIP]
>Klíč služby jednoznačně identifikuje okruhu ExpressRoute. Tento klíč je požadován pro většinu příkazů prostředí powershell hello uvedených v tomto dokumentu. Taky byste potřebovali pomoc od Microsoftu nebo od tootroubleshoot partnera ExpressRoute problém ExpressRoute poskytovat službu hello klíče tooreadily identifikovat hello okruh.
>
>

###<a name="verification-via-hello-azure-portal"></a>Ověření prostřednictvím hello portálu Azure
V hello portálu Azure, hello stav okruhu ExpressRoute můžete zkontrolovat výběrem ![2][2] na hello nabídce vlevo. straně panelu a potom vyberete hello okruh ExpressRoute. Výběr ExpressRoute okruhu uvedené v části "Všechny prostředky" otevře se okno okruh ExpressRoute hello. V hello ![3][3] části hello okně hello ExpressRoute essentials jsou uvedeny, jak ukazuje následující snímek obrazovky hello:

![4][4]    

V hello ExpressRoute Essentials *okruhu stav* označuje hello stav okruhu hello na hello straně společnosti Microsoft. *Stav zprostředkovatele* označuje, zda došlo ke hello okruh *zajištěno nebo není zřízený* na straně hello poskytovatele služeb. 

ExpressRoute okruhu toobe provozní, hello *okruhu stav* musí být *povoleno* a hello *stav zprostředkovatele* musí být *zajištěno*.

>[!NOTE]
>Pokud hello *okruhu stav* není povoleno, obraťte se na [Microsoft Support][Support]. Pokud hello *stav zprostředkovatele* není zajišťováno, obraťte se na svého poskytovatele služeb.
>
>

###<a name="verification-via-powershell"></a>Ověření pomocí prostředí PowerShell
toolist všechny hello okruhy ExpressRoute ve skupině prostředků, použijte následující příkaz hello:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
>Název skupiny prostředků můžete získat prostřednictvím hello portálu Azure. Zobrazit hello předchozí část tohoto dokumentu a Všimněte si, že tento název skupiny prostředků hello je uvedena ve snímku obrazovky příklad hello.
>
>

tooselect konkrétní okruh ExpressRoute ve skupině prostředků, hello použijte následující příkaz:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

Ukázková odpověď je:

    Name                             : Test-ER-Ckt
    ResourceGroupName                : Test-ER-RG
    Location                         : westus2
    Id                               : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                        "Name": "Standard_UnlimitedData",
                                        "Tier": "Standard",
                                        "Family": "UnlimitedData"
                                        }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             : 
    ServiceProviderProperties        : {
                                        "ServiceProviderName": "****",
                                        "PeeringLocation": "******",
                                        "BandwidthInMbps": 100
                                        }
    ServiceKey                       : **************************************
    Peerings                         : []
    Authorizations                   : []

tooconfirm Pokud okruh ExpressRoute je funkční, věnovat zvláštní pozornost toohello následující pole:

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

>[!NOTE]
>Pokud hello *CircuitProvisioningState* není povoleno, obraťte se na [Microsoft Support][Support]. Pokud hello *ServiceProviderProvisioningState* není zajišťováno, obraťte se na svého poskytovatele služeb.
>
>

###<a name="verification-via-powershell-classic"></a>Ověření pomocí prostředí PowerShell (klasické)
toolist všechny hello okruhy ExpressRoute v rámci předplatného, použijte následující příkaz hello:

    Get-AzureDedicatedCircuit

tooselect konkrétní okruh ExpressRoute, hello použijte následující příkaz:

    Get-AzureDedicatedCircuit -ServiceKey **************************************

Ukázková odpověď je:

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

tooconfirm Pokud okruh ExpressRoute je funkční, věnovat zvláštní pozornost toohello následující pole: ServiceProviderProvisioningState: Stav zřízení: povoleno

>[!NOTE]
>Pokud hello *stav* není povoleno, obraťte se na [Microsoft Support][Support]. Pokud hello *ServiceProviderProvisioningState* není zajišťováno, obraťte se na svého poskytovatele služeb.
>
>

##<a name="validate-peering-configuration"></a>Ověření konfigurace partnerského vztahu
Po má poskytovatel služeb hello dokončené hello zřizování hello okruh ExpressRoute, mohou být vytvořeny konfigurace směrování přes hello okruh ExpressRoute mezi MSEE-PRs (4) a Msee (5). Každý okruh ExpressRoute může mít jednu, dvě nebo tři směrování kontexty povoleno: soukromý partnerský vztah Azure (provoz tooprivate virtuální sítě v Azure), veřejný partnerský vztah Azure (provoz toopublic IP adresy v Azure) a (provoz tooOffice 365 partnerského vztahu Microsoftu a Dynamics 365). Další informace o tom, toocreate a upravit konfigurace směrování, najdete v článku hello [vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering].

###<a name="verification-via-hello-azure-portal"></a>Ověření prostřednictvím hello portálu Azure
>[!IMPORTANT]
>V portálu Azure, ve kterém jsou partnerských vztahů ExpressRoute hello je známého problému *není* uvedené na portálu hello Pokud nakonfigurovat pomocí hello poskytovatele služeb. Přidání partnerských vztahů ExpressRoute prostřednictvím portálu hello nebo prostředí PowerShell *přepíše nastavení poskytovatele služby hello*. Tato akce naruší hello směrování v okruhu ExpressRoute hello a vyžaduje podporu hello hello služby zprostředkovatele toorestore hello nastavení a obnovit normální směrování. Partnerské vztahy ExpressRoute hello změnit, jenom Pokud je jisté, že tohoto zprostředkovatele služby hello poskytuje služby vrstvy 2 pouze!
>
>

<p/>
>[!NOTE]
>Když vrstvy 3 je zadané ve hello jsou prázdné hello portálu služby zprostředkovatele a hello partnerských vztahů, může být prostředí PowerShell používané toosee hello služby zprostředkovatele nakonfigurovaná nastavení.
>
>

V hello portálu Azure, můžete zkontrolovat stav okruhu ExpressRoute výběrem ![2][2] na hello nabídce vlevo. straně panelu a potom vyberete hello okruh ExpressRoute. Výběr ExpressRoute okruhu uvedené v části "Všechny prostředky" by otevřete okno okruh ExpressRoute hello. V hello ![3][3] části hello okně hello ExpressRoute, které by byly uvedeny essentials, jak ukazuje následující snímek obrazovky hello:

![5][5]

V předchozím příkladu hello jako uvedené Azure soukromého partnerského vztahu směrování kontextu je povoleno, zatímco veřejný Azure a kontexty směrování partnerského vztahu Microsoftu nejsou povolené. Úspěšně povolilo partnerského vztahu kontextu by měla mít také hello primární a sekundární point-to-point (povinné pro protokol BGP) podsítě uvedené. Hello /30 podsítě se používají pro hello rozhraní IP adresu hello Msee a PE Msee. 

>[!NOTE]
>Pokud partnerský vztah není povolena, zkontrolujte, je-li primární a sekundární podsítě hello přiřazené odpovídat konfiguraci hello v PE Msee. Pokud ne, toochange na směrovači MSEE konfigurace hello, podívejte se příliš[vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering]
>
>

###<a name="verification-via-powershell"></a>Ověření pomocí prostředí PowerShell
tooget hello Azure privátní partnerský vztah podrobnosti o konfiguraci, použijte hello následující příkazy:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt

Ukázková odpověď, úspěšně nakonfigurovaný soukromého partnerského vztahu, je:

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : ####
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 300
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 Úspěšně povolilo partnerského vztahu kontextu by měla mít předpony adres primární a sekundární hello uvedené. Hello /30 podsítě se používají pro hello rozhraní IP adresu hello Msee a PE Msee.

tooget hello Azure veřejného partnerského vztahu podrobnosti o konfiguraci, použijte hello následující příkazy:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt

tooget hello podrobností partnerského vztahu Microsoftu konfigurace, použijte hello následující příkazy:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

Pokud není nakonfigurováno partnerský vztah, by chybovou zprávu. Ukázková odpověď, když hello uvádí partnerského vztahu (Azure veřejný partnerský vztah v tomto příkladu) není nakonfigurované v rámci okruhu hello:

    Get-AzureRmExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzureRmExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
>Pokud partnerský vztah není povolena, zkontrolujte, pokud hello primární a sekundární podsítě přiřadit shodu hello konfiguraci na hello propojené PE MSEE. Také zkontrolujte, jestli hello opravit *VlanId*, *AzureASN*, a *PeerASN* se používají na Msee a pokud se tyto hodnoty mapuje toohello jsou použity na hello propojené PE MSEE. Pokud zvolíte použití algoritmu hash MD5, by měla být stejná na dvojice MSEE a PE MSEE hello sdílený klíč. Konfigurace hello toochange na směrovači MSEE hello, najdete v příliš [vytvoření a úprava směrování pro okruh ExpressRoute] [CreatePeering].  
>
>

### <a name="verification-via-powershell-classic"></a>Ověření pomocí prostředí PowerShell (klasické)
tooget hello Azure privátní partnerský vztah podrobnosti o konfiguraci, použijte hello následující příkaz:

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

Ukázková odpověď, úspěšně nakonfigurovaný soukromého partnerského vztahu je:

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : ####
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100

Úspěšně, povolená A partnerského vztahu kontextu by měla mít podsítě hello primárního a sekundárního partnera uvedené. Hello /30 podsítě se používají pro hello rozhraní IP adresu hello Msee a PE Msee.

tooget hello Azure veřejného partnerského vztahu podrobnosti o konfiguraci, použijte hello následující příkazy:

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

tooget hello podrobností partnerského vztahu Microsoftu konfigurace, použijte hello následující příkazy:

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
>Pokud vrstvy 3 partnerských vztahů byly nastavené zásadami hello poskytovatele služeb, nastavení hello partnerských vztahů ExpressRoute prostřednictvím portálu hello nebo prostředí PowerShell přepíše nastavení poskytovatele služby hello. Resetování partnerského vztahu nastavení straně hello zprostředkovatele vyžaduje podporu hello hello poskytovatele služeb. Partnerské vztahy ExpressRoute hello změnit, jenom Pokud je jisté, že tohoto zprostředkovatele služby hello poskytuje služby vrstvy 2 pouze!
>
>

<p/>
>[!NOTE]
>Pokud partnerský vztah není povolena, zkontrolujte, pokud konfigurace hello hello primárního a sekundárního partnera přiřazené podsítě shodu na hello propojené PE MSEE. Také zkontrolujte, jestli hello opravit *VlanId*, *AzureAsn*, a *PeerAsn* se používají na Msee a pokud se tyto hodnoty mapuje toohello jsou použity na hello propojené PE MSEE. Konfigurace hello toochange na směrovači MSEE hello, najdete v příliš [vytvoření a úprava směrování pro okruh ExpressRoute] [CreatePeering].
>
>

## <a name="validate-arp-between-microsoft-and-hello-service-provider"></a>Ověření protokolu ARP mezi poskytovatele služeb společnosti Microsoft a hello
Tato část používá příkazy prostředí PowerShell (klasické). Pokud používáte příkazy prostředí PowerShell Azure Resource Manager, ujistěte se, zda máte přístup správce nebo spolusprávce předplatného toohello prostřednictvím [portál Azure classic][OldPortal]. Řešení potíží s pomocí Azure Resource Manager příkazy naleznete toohello [získávání ARP tabulky v modelu nasazení Resource Manager hello] [ ARP] dokumentu.

>[!NOTE]
>tooget protokolu ARP, hello portál Azure a příkazy prostředí PowerShell Azure Resource Manager můžete použít. Pokud k chybám hello Azure Resource Manager příkazy prostředí PowerShell, classic příkazy prostředí PowerShell by měly fungovat jako Classic PowerShell příkazy spolupracovat taky s nástrojem okruhy ExpressRoute Azure Resource Manager.
>
>

tooget hello tabulky ARP z hello primární MSEE směrovač hello soukromého partnerského vztahu, použijte následující příkaz hello:

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Odpověď příklad pro příkaz hello, v případě úspěšné hello:

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

Podobně můžete zkontrolovat hello tabulky ARP z hello MSEE v hello *primární*/*sekundární* cestu, pro *privátní* /  *Veřejné*/*Microsoft* partnerských vztahů.

Hello následující příklad, že zobrazuje hello odpovědi hello příkazu pro partnerský vztah neexistuje.

    ARP Info:
       
>[!NOTE]
>Pokud nemá hello tabulky ARP IP adresy rozhraní hello mapované tooMAC adresy, hello zkontrolujte následující informace:
>1. Pokud přiřazená hello první IP adresa podsítě hello /30 pro hello propojení mezi hello MSEE PR a MSEE se používá na hello rozhraní MSEE PR. Azure vždy používá hello druhou IP adresu pro Msee.
>2. Ověřte, pokud zákazník hello (C-Tag) a značky VLAN služby (S-Tag) odpovídají na dvojice MSEE PR a MSEE.
>
>

## <a name="validate-bgp-and-routes-on-hello-msee"></a>Ověření protokolu BGP a trasy na hello MSEE
Tato část používá příkazy prostředí PowerShell (klasické). Pokud používáte příkazy prostředí PowerShell Azure Resource Manager, ujistěte se, zda máte přístup správce nebo spolusprávce předplatného toohello prostřednictvím [portál Azure classic][OldPortal]

>[!NOTE]
>tooget BGP informace, jak hello lze použít portál Azure a příkazy prostředí PowerShell Azure Resource Manager. Pokud k chybám hello Azure Resource Manager příkazy prostředí PowerShell, classic příkazy prostředí PowerShell by měly fungovat jako classic PowerShell příkazy spolupracovat taky s nástrojem okruhy ExpressRoute Azure Resource Manager.
>
>

tooget hello směrovací tabulky (BGP sousedním) souhrnu pro konkrétní směrování kontext, použijte následující příkaz hello:

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

Odpověď příklad je:

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

Jak je znázorněno v předchozím příkladu hello, příkaz hello je užitečné toodetermine pro jak dlouho je vytvořený hello směrování kontextu. Označuje také počet předpony trasy inzerované partnerského vztahu směrovačem hello.

>[!NOTE]
>Pokud je stav hello v aktivní nebo nečinnosti, zkontrolujte, pokud konfigurace hello hello primárního a sekundárního partnera přiřazené podsítě shodu na hello propojené PE MSEE. Také zkontrolujte, jestli hello opravit *VlanId*, *AzureAsn*, a *PeerAsn* se používají na Msee a pokud se tyto hodnoty mapuje toohello jsou použity na hello propojené PE MSEE. Pokud zvolíte použití algoritmu hash MD5, by měla být stejná na dvojice MSEE a PE MSEE hello sdílený klíč. Konfigurace hello toochange na směrovači MSEE hello, najdete v příliš[vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering].
>
>

<p/>
>[!NOTE]
>Pokud některá místa určení není dostupný přes konkrétní partnerský vztah, zkontrolujte hello směrovací tabulku z hello Msee patřící toohello konkrétní partnerského vztahu kontextu. Pokud se nachází ve směrovací tabulce hello odpovídající předpony (můžou být NATed IP), potom zkontrolujte, zda jsou brány firewall nebo nebo seznamy ACL skupiny NSG na cestu hello a v případě, že povolují provoz hello.
>
>

tooget hello úplné směrovací tabulky z MSEE na hello *primární* cestu pro konkrétní hello *privátní* směrování kontextu, hello použijte následující příkaz:

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Je úspěšnému výsledku příklad pro příkaz hello:

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

Podobně můžete zkontrolovat hello směrovací tabulky z hello MSEE v hello *primární*/*sekundární* cestu, pro *privátní* / *Veřejné*/*Microsoft* partnerského vztahu kontextu.

Hello následující příklad, že zobrazuje hello odpovědi hello příkazu pro partnerský vztah neexistuje:

    Route Table Info:

##<a name="check-hello-traffic-statistics"></a>Zkontrolujte hello statistiky provozu
primární a sekundární cesta statistiku provozu – bajtů tooget hello a odhlašování – kombinaci partnerského vztahu kontextu, hello použijte následující příkaz:

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

Ukázkový výstup hello příkazu je:

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

Ukázkový výstup hello příkazu pro neexistující partnerský vztah je:

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a>Další kroky
Další informace a nápovědu najdete na naší hello následující odkazy:

- [Podporu společnosti Microsoft][Support]
- [Vytvoření a úprava okruhu ExpressRoute][CreateCircuit]
- [Vytvoření a úprava směrování pro okruh ExpressRoute][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "Připojení k logické Express Route"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Ikona všechny prostředky"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Ikona – přehled"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "Snímek obrazovky ukázkové ExpressRoute Essentials"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "Snímek obrazovky ukázkové ExpressRoute Essentials"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[OldPortal]: https://manage.windowsazure.com
[ARP]: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






