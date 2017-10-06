---
title: "Jak tooconfigure směrování pro okruh ExpressRoute (partnerský vztah): Resource Manager: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek vás provede kroky hello pro vytváření a zřizování hello soukromého a veřejného a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak stav hello toocheck, aktualizace nebo odstranění partnerských vztahů pro váš okruh."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0a036d51-77ae-4fee-9ddb-35f040fbdcdf
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: eb3ddf5c05a086ac3e22c64417e51381ef465921
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a>Vytvářet a upravovat partnerský vztah pro okruhu ExpressRoute pomocí prostředí PowerShell

Tento článek pomůže při vytváření a správě konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager hello pomocí prostředí PowerShell. Můžete také zkontrolovat stav hello, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute. Pokud chcete toouse toowork jinou metodu s váš okruh, vyberte článek z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - soukromého partnerského vztahu](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - veřejného partnerského vztahu](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - partnerského vztahu Microsoftu](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a>Předpoklady konfigurace

* Budete potřebovat nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). 
* Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md) stránce hello [požadavky na směrování](expressroute-routing.md) stránku a hello [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Musí mít aktivní okruh ExpressRoute. Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat. Hello okruh ExpressRoute musí být ve stavu zřízený a povolený pro vás toobe možné toorun hello rutin v tomto článku.

Tyto pokyny platí pouze toocircuits vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.

> [!IMPORTANT]
> Jsme aktuálně neprovádějí ohlášení partnerské vztahy nakonfigurované poskytovateli služeb prostřednictvím portálu pro správu služby hello. Pracujeme na tom, aby tato možnost brzy fungovala. Před konfigurací partnerských vztahů protokolu BGP, zkontrolujte u vašeho poskytovatele služeb.
> 
> 

Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute. Partnerské vztahy můžete konfigurovat v libovolném pořadí. Přesvědčte se, že dokončení hello konfiguraci každého partnerského vztahu jeden po druhém. 

## <a name="azure-private-peering"></a>Soukromý partnerský vztah Azure

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello privátní konfigurace partnerského vztahu Azure pro okruh ExpressRoute.

### <a name="toocreate-azure-private-peering"></a>toocreate soukromého partnerského vztahu Azure

1. Importujte modul PowerShell hello pro ExpressRoute.

  Je nutné nainstalovat hello nejnovější verzi instalačního programu Powershellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello. Budete potřebovat toorun prostředí PowerShell jako správce.

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  Importujte všechny moduly AzureRM.* hello hello známé sémantické verze. rozsah.

  ```powershell
  Import-AzureRM
  ```

  Můžete můžete také naimportovat jenom modul select v hello známé sémantické verze. rozsah.

  ```powershell
  Import-Module AzureRM.Network 
  ```

  Přihlaste se tooyour účtu.

  ```powershell
  Login-AzureRmAccount
  ```

  Vyberte předplatné hello chcete toocreate okruh ExpressRoute.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. Vytvořte okruh ExpressRoute.

  Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení hello.

  Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás. V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello. Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.
3. Zkontrolujte, zda je zřízený a také povolený toomake okruh ExpressRoute hello. Použijte hello následující ukázka:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  odpověď Hello je podobné toohello následující ukázka:

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. Nakonfigurujte soukromý partnerský vztah Azure pro okruh hello. Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:

  * / 30 pro primární propojení hello podsítě. Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
  * / 30 pro sekundární propojení hello podsítě. Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
  * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS. Pro tento partnerský vztah můžete použít soukromé číslo AS. Zkontrolujte, že nepoužíváte 65515.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.

  Použijte následující příklad tooconfigure privátní partnerský vztah pro váš okruh Azure hello:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  Pokud si zvolíte toouse hodnotu hash MD5, použijte následující ukázka hello:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.
  > 
  >

### <a name="tooview-azure-private-peering-details"></a>tooview Azure privátní partnerský vztah podrobnosti

Podrobnosti o konfiguraci můžete získat pomocí hello následující ukázka:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
```

### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate konfigurace soukromého partnerského vztahu Azure

Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat. V tomto příkladu je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 100 too500.

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-private-peering"></a>toodelete soukromého partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:

> [!WARNING]
> Je nutné zajistit, že všechny virtuální sítě jsou před spuštěním tohoto příkladu odpojit od hello okruh ExpressRoute. 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-public-peering"></a>Veřejný partnerský vztah Azure

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello veřejné konfigurace partnerského vztahu Azure pro okruh ExpressRoute.

### <a name="toocreate-azure-public-peering"></a>toocreate veřejného partnerského vztahu Azure

1. Importujte modul PowerShell hello pro ExpressRoute.

  Je nutné nainstalovat hello nejnovější verzi instalačního programu Powershellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello. Budete potřebovat toorun prostředí PowerShell jako správce.

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  Importujte všechny moduly AzureRM.* hello hello známé sémantické verze. rozsah.

  ```powershell
  Import-AzureRM
  ```

  Můžete můžete také naimportovat jenom modul select v hello známé sémantické verze. rozsah.

  ```powershell
  Import-Module AzureRM.Network
```

  Přihlaste se tooyour účtu.

  ```powershell
  Login-AzureRmAccount
  ```

  Vyberte předplatné hello chcete toocreate okruh ExpressRoute.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. Vytvořte okruh ExpressRoute.

  Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení hello.

  Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás. V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello. Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.
3. Zkontrolujte tooensure okruh ExpressRoute hello je zřízený a také povolený. Použijte hello následující ukázka:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  odpověď Hello je podobné toohello následující ukázka:

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                      "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. Nakonfigurujte veřejný partnerský vztah Azure pro okruh hello. Ujistěte se, že máte hello následující informace, než budete pokračovat dál.

  * / 30 pro primární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy.
  * / 30 pro sekundární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy.
  * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.

  Spustit hello následující příklad tooconfigure Azure veřejný partnerský vztah pro váš okruh.

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  Pokud si zvolíte toouse hodnotu hash MD5, použijte následující ukázka hello:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.
  > 
  >

### <a name="tooview-azure-public-peering-details"></a>tooview Azure veřejného partnerského vztahu podrobnosti

Můžete získat podrobnosti o konfiguraci pomocí následující rutiny hello:

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="tooupdate-azure-public-peering-configuration"></a>tooupdate konfigurace veřejného partnerského vztahu Azure

Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat. V tomto příkladu je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 200 too600.

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-public-peering"></a>toodelete veřejného partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="microsoft-peering"></a>Partnerský vztah Microsoftu

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu hello Microsoftu pro okruh ExpressRoute.

> [!IMPORTANT]
> Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím hello partnerský vztah Microsoftu, i když nejsou definovány filtry tras. Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh. Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).
> 
> 

### <a name="toocreate-microsoft-peering"></a>partnerský vztah Microsoftu toocreate

1. Importujte modul PowerShell hello pro ExpressRoute.

  Je nutné nainstalovat hello nejnovější verzi instalačního programu Powershellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello. Budete potřebovat toorun prostředí PowerShell jako správce.

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  Importujte všechny moduly AzureRM.* hello hello známé sémantické verze. rozsah.

  ```powershell
  Import-AzureRM
  ```

  Můžete můžete také naimportovat jenom modul select v hello známé sémantické verze. rozsah.

  ```powershell
  Import-Module AzureRM.Network
  ```

  Přihlaste se tooyour účtu.

  ```powershell
  Login-AzureRmAccount
  ```

  Vyberte předplatné hello chcete toocreate okruh ExpressRoute.

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. Vytvořte okruh ExpressRoute.

  Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení hello.

  Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás. V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello. Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.
3. Zkontrolujte, zda je zřízený a také povolený toomake okruh ExpressRoute hello. Použijte hello následující ukázka:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  odpověď Hello je podobné toohello následující ukázka:

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. Nakonfigurujte partnerský vztah Microsoftu pro okruh hello. Ujistěte se, že máte hello následující informace, než budete pokračovat.

  * / 30 pro primární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
  * / 30 pro sekundární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
  * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
  * Inzerované předpony: musíte poskytnout seznam všech předpon plánování tooadvertise přes relaci protokolu BGP hello. Přijímají se jenom předpony veřejných IP adres. Pokud máte v plánu toosend sadu předpon, můžete odeslat seznam oddělený čárkami. Tyto předpony musí být registrovaný tooyou u rir / IRR.
  * **Volitelné -** zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované toohello číslo AS partnerského vztahu, můžete zadat hello jako číslo toowhich jsou registrované.
  * Název registru směrování: Můžete zadat hello RIR / IRR, u které hello jako číslo a předpony jsou registrované.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.

   Použijte následující příklad tooconfigure partnerský vztah Microsoftu pro váš okruh hello:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="tooget-microsoft-peering-details"></a>tooget podrobností partnerského vztahu Microsoftu

Můžete získat podrobnosti o konfiguraci pomocí hello následující ukázka:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-microsoft-peering-configuration"></a>konfiguraci partnerského vztahu Microsoftu tooupdate

Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat:

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-microsoft-peering"></a>partnerský vztah Microsoftu toodelete

Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny hello:

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a>Další kroky

Dalším krokem je [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-arm.md).

* Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).
* Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).
* Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).
