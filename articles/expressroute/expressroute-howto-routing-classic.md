---
title: "Jak tooconfigure směrování pro okruh ExpressRoute (partnerský vztah): Azure: classic | Microsoft Docs"
description: "Tento článek vás provede kroky hello pro vytváření a zřizování hello soukromého a veřejného a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak stav hello toocheck, aktualizace nebo odstranění partnerských vztahů pro váš okruh."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: a4bd39d2-373a-467a-8b06-36cfcc1027d2
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: dc5bcc4b86c3bc965a88281b6c2a660f3bc58eb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a>Vytvářet a upravovat partnerský vztah pro okruh ExpressRoute (klasické)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - soukromého partnerského vztahu](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - veřejného partnerského vztahu](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - partnerského vztahu Microsoftu](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-routing-classic.md)
> 

Tento článek vás provede kroky toocreate hello a správě konfigurace směrování pro okruhu ExpressRoute pomocí prostředí PowerShell a hello modelu nasazení classic. Hello kroky se také zobrazí jak toocheck hello stav, aktualizovat, nebo odstranit a zrušit jejich zřízení partnerských vztahů pro okruh ExpressRoute.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a>Předpoklady konfigurace
* Budete potřebovat nejnovější verzi rutin prostředí PowerShell Azure Service Management (SM) hello hello. Další informace najdete v tématu [Začínáme s rutinami prostředí Azure PowerShell](/powershell/azure/overview).  
* Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md) stránce hello [požadavky na směrování](expressroute-routing.md) stránku a hello [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Musí mít aktivní okruh ExpressRoute. Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat. Hello okruh ExpressRoute musí být ve stavu zřízený a povolený pro vás toobe možné toorun hello rutiny popsané dál.

> [!IMPORTANT]
> Tyto pokyny platí pouze toocircuits vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2. Pokud používáte poskytovatele služeb nabízejícího spravované služby vrstvy 3 (obvykle IPVPN, např. MPLS), poskytovatel připojení provede konfiguraci a správu směrování za vás.
> 
> 

Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute. Partnerské vztahy můžete konfigurovat v libovolném pořadí. Přesvědčte se, že dokončení hello konfiguraci každého partnerského vztahu jeden po druhém.


### <a name="log-in-tooyour-azure-account-and-select-a-subscription"></a>Přihlaste se tooyour účet Azure a vybrat odběr
1. Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu. Použijte následující příklad toohelp, ke kterým se připojujete hello:

        Login-AzureRmAccount

2. Zkontrolujte předplatná hello pro účet hello.

        Get-AzureRmSubscription

3. Pokud máte více než jedno předplatné, vyberte hello předplatné, které chcete toouse.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. Pak pomocí následující rutiny tooadd hello tooPowerShell vaše předplatné Azure pro model nasazení classic hello.

        Add-AzureAccount


## <a name="azure-private-peering"></a>Soukromý partnerský vztah Azure
Tato část obsahuje pokyny jak toocreate, získat, aktualizovat a odstranit hello privátní konfigurace partnerského vztahu Azure pro okruh ExpressRoute. 

### <a name="toocreate-azure-private-peering"></a>toocreate soukromého partnerského vztahu Azure
1. **Importujte modul PowerShell hello pro ExpressRoute.**
   
    Je nutné naimportovat moduly Azure a ExpressRoute hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello. Hello spusťte následující příkazy tooimport hello Azure a ExpressRoute modulů do relace prostředí PowerShell hello.  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **Vytvoření okruhu ExpressRoute.**
   
    Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho zřízený poskytovatelem připojení hello. Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás. V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello. Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů hello. 
3. **Zkontrolujte tooensure okruh ExpressRoute hello, které je zřízený.**
   
    Nejdřív musíte zkontrolovat toosee Pokud hello okruh ExpressRoute je zřízený a také povolený. Viz následující příklad hello.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Ujistěte se, že hello okruhu zobrazuje jako zajištěno a povoleno. Pokud tomu tak není, pracujete s vaší tooget zprostředkovatele připojení k okruhu toohello vyžaduje stav a stav.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Nakonfigurujte soukromý partnerský vztah Azure pro okruh hello.**
   
    Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:
   
   * / 30 pro primární propojení hello podsítě. Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
   * / 30 pro sekundární propojení hello podsítě. Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
   * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
   * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS. Pro tento partnerský vztah můžete použít soukromé číslo AS. Zkontrolujte, že nepoužíváte 65515.
   * Hash MD5, pokud se rozhodnete toouse jeden. **Tato položka je nepovinná.**
     
    Spuštěním následující rutiny tooconfigure soukromý partnerský vztah Azure pro váš okruh hello.
     
        Nové AzureBGPPeering - AccessType privátní - klíč ServiceKey "***" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100
     
    Pokud se rozhodnete toouse hodnotu hash MD5, můžete použít následující rutinu hello.
     
        Nové AzureBGPPeering - AccessType privátní - klíč ServiceKey "***" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100 - SharedKey "A1B2C3D4"
     
     > [!IMPORTANT]
     > Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.
     > 
     > 

### <a name="tooview-azure-private-peering-details"></a>tooview Azure privátní partnerský vztah podrobnosti
Můžete získat podrobnosti o konfiguraci pomocí následující rutiny hello

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate konfigurace soukromého partnerského vztahu Azure
Libovolnou část konfigurace hello pomocí hello následující rutiny můžete aktualizovat. V příkladu hello níže je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 100 too500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="toodelete-azure-private-peering"></a>toodelete soukromého partnerského vztahu Azure
Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny hello.

> [!WARNING]
> Je nutné zajistit, že všechny virtuální sítě se odpojit od hello okruh ExpressRoute před spuštěním této rutiny. 
> 
> 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Veřejný partnerský vztah Azure
Tato část obsahuje pokyny jak toocreate, získat, aktualizovat a odstranit hello veřejné konfigurace partnerského vztahu Azure pro okruh ExpressRoute.

### <a name="toocreate-azure-public-peering"></a>toocreate veřejného partnerského vztahu Azure
1. **Importujte modul PowerShell hello pro ExpressRoute.**
   
    Je nutné naimportovat moduly Azure a ExpressRoute hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello. Hello spusťte následující příkazy tooimport hello Azure a ExpressRoute modulů do relace prostředí PowerShell hello. 
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **Vytvoření okruhu ExpressRoute**
   
    Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho zřízený poskytovatelem připojení hello. Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure veřejný partnerský vztah za vás. V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello. Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů hello.
3. **Zkontrolujte tooensure okruh ExpressRoute, které je zřízený**
   
    Nejdřív musíte zkontrolovat toosee Pokud hello okruh ExpressRoute je zřízený a také povolený. Viz následující příklad hello.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Ujistěte se, že hello okruhu zobrazuje jako zajištěno a povoleno. Pokud tomu tak není, pracujete s vaší tooget zprostředkovatele připojení k okruhu toohello vyžaduje stav a stav.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Nakonfigurujte veřejný partnerský vztah Azure pro okruh hello**
   
    Ujistěte se, že máte následující informace před pokračováním hello.
   
   * / 30 pro primární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy.
   * / 30 pro sekundární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy.
   * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
   * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
   * Hash MD5, pokud se rozhodnete toouse jeden. **Tato položka je nepovinná.**
     
    Spuštěním následující rutiny tooconfigure veřejný partnerský vztah Azure pro váš okruh hello
     
        Nové AzureBGPPeering - AccessType veřejný - klíč ServiceKey "***" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200
     
    Pokud zvolíte toouse hodnotu hash MD5, můžete použít následující rutinu hello
     
        Nové AzureBGPPeering - AccessType veřejný - klíč ServiceKey "***" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200 - SharedKey "A1B2C3D4"
     
     > [!IMPORTANT]
     > Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, a ne zákaznické číslo ASN.
     > 
     > 

### <a name="tooview-azure-public-peering-details"></a>tooview Azure veřejného partnerského vztahu podrobnosti
Můžete získat podrobnosti o konfiguraci pomocí následující rutiny hello

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="tooupdate-azure-public-peering-configuration"></a>tooupdate konfigurace veřejného partnerského vztahu Azure
Libovolnou část konfigurace hello pomocí hello následující rutiny můžete aktualizovat

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

Hello ID sítě VLAN okruhu hello je aktualizováno z hodnoty 200 too600 v hello výše příklad.

### <a name="toodelete-azure-public-peering"></a>toodelete veřejného partnerského vztahu Azure
Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny hello

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Partnerský vztah Microsoftu
Tato část obsahuje pokyny jak toocreate, získat, aktualizovat a odstranit konfiguraci partnerského vztahu hello Microsoftu pro okruh ExpressRoute. 

### <a name="toocreate-microsoft-peering"></a>partnerský vztah Microsoftu toocreate
1. **Importujte modul PowerShell hello pro ExpressRoute.**
   
    Je nutné naimportovat moduly Azure a ExpressRoute hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello. Hello spusťte následující příkazy tooimport hello Azure a ExpressRoute modulů do relace prostředí PowerShell hello.  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **Vytvoření okruhu ExpressRoute**
   
    Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho zřízený poskytovatelem připojení hello. Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás. V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello. Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů hello.
3. **Zkontrolujte tooensure okruh ExpressRoute, které je zřízený**
   
    Nejdřív musíte zkontrolovat toosee Pokud hello okruh ExpressRoute je ve stavu zajištěno a povoleno.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Ujistěte se, že hello okruhu zobrazuje jako zajištěno a povoleno. Pokud tomu tak není, pracujete s vaší tooget zprostředkovatele připojení k okruhu toohello vyžaduje stav a stav.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Nakonfigurujte partnerský vztah Microsoftu pro okruh hello**
   
    Ujistěte se, že máte hello následující informace, než budete pokračovat.
   
   * / 30 pro primární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
   * / 30 pro sekundární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
   * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
   * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
   * Inzerované předpony: musíte poskytnout seznam všech předpon plánování tooadvertise přes relaci protokolu BGP hello. Přijímají se jenom předpony veřejných IP adres. Pokud máte v plánu toosend sadu předpon, můžete odeslat seznam oddělený čárkami. Tyto předpony musí být registrovaný tooyou u rir / IRR.
   * Zákaznické číslo ASN: Pokud jsou inzerování předpon, které nejsou registrované toohello číslo AS partnerského vztahu, můžete hello zadat jako číslo toowhich, které jsou registrované. **Tato položka je nepovinná.**
   * Název registru směrování: Můžete zadat hello RIR / IRR, u které hello jako číslo a předpony jsou registrované.
   * Hodnota hash MD5, pokud se rozhodnete toouse jeden. **Tato položka je nepovinná.**
     
    Spuštěním následující rutiny tooconfigure Microsoft pering pro váš okruh hello
     
        Nové AzureBGPPeering - AccessType Microsoft - klíč ServiceKey "***" "131.107.0.0/30" - PrimaryPeerSubnet - SecondaryPeerSubnet "131.107.0.4/30" - VlanId 300 - PeerAsn 1234 - CustomerAsn. 2245 - AdvertisedPublicPrefixes " 123.0.0.0/30 "- RoutingRegistryName"ARIN"- SharedKey"A1B2C3D4"

### <a name="tooview-microsoft-peering-details"></a>tooview podrobností partnerského vztahu Microsoftu
Můžete získat podrobnosti o konfiguraci pomocí následující rutiny hello.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="tooupdate-microsoft-peering-configuration"></a>konfiguraci partnerského vztahu Microsoftu tooupdate
Libovolnou část konfigurace hello pomocí hello následující rutiny můžete aktualizovat.

    Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="toodelete-microsoft-peering"></a>partnerský vztah Microsoftu toodelete
Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny hello.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Další kroky
Dále [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-classic.md).

* Další informace o pracovních postupech najdete v tématu [pracovních postupech](expressroute-workflows.md).
* Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).

