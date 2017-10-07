---
title: "Jak tooconfigure směrování pro okruh Azure ExpressRoute: rozhraní příkazového řádku | Microsoft Docs"
description: "Tento článek vám pomůže vytvořit a zřídit hello soukromého a veřejného a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak stav hello toocheck, aktualizace nebo odstranění partnerských vztahů pro váš okruh."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: 33130af050045527cdb316e77821c6d101b6a82a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-routing-for-an-expressroute-circuit-using-cli"></a>Vytvoření a úprava směrování pro okruh ExpressRoute pomocí rozhraní příkazového řádku

Tento článek vám pomůže vytvořit a spravovat konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager hello pomocí rozhraní příkazového řádku. Můžete také zkontrolovat stav hello, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute. Pokud chcete toouse toowork jinou metodu s váš okruh, vyberte článek z hello následující seznamu:

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

* Než začnete, nainstalujte nejnovější verzi hello hello rozhraní příkazového řádku (2.0 nebo novější). Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli).
* Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovního postupu](expressroute-workflows.md) stránky před zahájením konfigurace.
* Musí mít aktivní okruh ExpressRoute. Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](howto-circuit-cli.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat. Hello okruh ExpressRoute musí být ve stavu zřízený a povolený pro vás toobe možné toorun hello příkazy v tomto článku.

Tyto pokyny platí pouze toocircuits vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.

Můžete nakonfigurovat jednu, dvě nebo všechny tři partnerské vztahy (Azure privátní, veřejný Azure a Microsoft) pro okruh ExpressRoute. Partnerské vztahy můžete konfigurovat v libovolném pořadí. Přesvědčte se, že dokončení hello konfiguraci každého partnerského vztahu jeden po druhém.

## <a name="azure-private-peering"></a>Soukromý partnerský vztah Azure

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello privátní konfigurace partnerského vztahu Azure pro okruh ExpressRoute.

### <a name="toocreate-azure-private-peering"></a>toocreate soukromého partnerského vztahu Azure

1. Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure hello. Musíte použít nejnovější verzi hello hello rozhraní příkazového řádku Azure (CLI). * Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.

  ```azurecli
  az login
  ```

  Vyberte předplatné hello chcete toocreate okruh ExpressRoute

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Vytvořte okruh ExpressRoute. Postupujte podle pokynů toocreate hello [okruh ExpressRoute](howto-circuit-cli.md) a mějte ho zřízený poskytovatelem připojení hello.

  Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás. V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello. Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.
3. Zkontrolujte, zda je zřízený a také povolený toomake okruh ExpressRoute hello. Použijte hello následující ukázka:

  ```azurecli
  az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
  ```

  odpověď Hello je podobné toohello následující ukázka:

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Nakonfigurujte soukromý partnerský vztah Azure pro okruh hello. Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:

  * / 30 pro primární propojení hello podsítě. Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
  * / 30 pro sekundární propojení hello podsítě. Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
  * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS. Pro tento partnerský vztah můžete použít soukromé číslo AS. Zkontrolujte, že nepoužíváte 65515.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.

  Použijte následující příklad tooconfigure privátní partnerský vztah pro váš okruh Azure hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
  ```

  Pokud si zvolíte toouse hodnotu hash MD5, použijte následující ukázka hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.
  > 
  > 

### <a name="tooview-azure-private-peering-details"></a>tooview Azure privátní partnerský vztah podrobnosti

Podrobnosti o konfiguraci můžete získat pomocí hello následující ukázka:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

Hello výstup je podobné toohello následující ukázka:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePrivatePeering",
  "ipv6PeeringConfig": null,
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePrivatePeering",
  "peerAsn": 7671,
  "peeringType": "AzurePrivatePeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate konfigurace soukromého partnerského vztahu Azure

Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat. V tomto příkladu je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 100 too500.

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="toodelete-azure-private-peering"></a>toodelete soukromého partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:

> [!WARNING]
> Je nutné zajistit, že všechny virtuální sítě jsou před spuštěním tohoto příkladu odpojit od hello okruh ExpressRoute. 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="azure-public-peering"></a>Veřejný partnerský vztah Azure

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello veřejné konfigurace partnerského vztahu Azure pro okruh ExpressRoute.

### <a name="toocreate-azure-public-peering"></a>toocreate veřejného partnerského vztahu Azure

1. Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure hello. Musíte použít nejnovější verzi hello hello rozhraní příkazového řádku Azure (CLI). * Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.

  ```azurecli
  az login
  ```

  Vyberte hello předplatné, pro které chcete toocreate okruh ExpressRoute.

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Vytvořte okruh ExpressRoute.  Postupujte podle pokynů toocreate hello [okruh ExpressRoute](howto-circuit-cli.md) a mějte ho zřízený poskytovatelem připojení hello.

  Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat, že poskytovatel připojení umožňuje soukromý partnerský vztah Azure za vás. V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello. Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.
3. Zkontrolujte tooensure okruh ExpressRoute hello je zřízený a také povolený. Použijte hello následující ukázka:

  ```azurecli
  az network express-route list
  ```

  odpověď Hello je podobné toohello následující ukázka:

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
    "bandwidthInMbps": 200,
    "peeringLocation": "Silicon Valley",
    "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
  ```

4. Nakonfigurujte veřejný partnerský vztah Azure pro okruh hello. Ujistěte se, že máte hello následující informace, než budete pokračovat dál.

  * / 30 pro primární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy.
  * / 30 pro sekundární propojení hello podsítě. Musí se jednat o platnou předponu veřejné IPv4 adresy.
  * Platné ID sítě VLAN tooestablish tohoto partnerského vztahu. Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.

  Spusťte následující příklad tooconfigure veřejný partnerský vztah pro váš okruh Azure hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
  ```

  Pokud si zvolíte toouse hodnotu hash MD5, použijte následující ukázka hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.

### <a name="tooview-azure-public-peering-details"></a>tooview Azure veřejného partnerského vztahu podrobnosti

Můžete získat podrobnosti o konfiguraci pomocí hello následující ukázka:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

Hello výstup je podobné toohello následující ukázka:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePublicPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePublicPeering",
  "peerAsn": 7671,
  "peeringType": "AzurePublicPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="tooupdate-azure-public-peering-configuration"></a>tooupdate konfigurace veřejného partnerského vztahu Azure

Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat. V tomto příkladu je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 200 too600.

```azurecli
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="toodelete-azure-public-peering"></a>toodelete veřejného partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="microsoft-peering"></a>Partnerský vztah Microsoftu

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu hello Microsoftu pro okruh ExpressRoute.

> [!IMPORTANT]
> Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím hello partnerský vztah Microsoftu, i když nejsou definovány filtry tras. Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh. Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).
> 
> 

### <a name="toocreate-microsoft-peering"></a>partnerský vztah Microsoftu toocreate

1. Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure hello. Použití hello nejnovější verzi hello rozhraní příkazového řádku Azure (CLI). * Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.

  ```azurecli
  az login
  ```

  Vyberte hello předplatné, pro které chcete toocreate okruh ExpressRoute.

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. Vytvořte okruh ExpressRoute. Postupujte podle pokynů toocreate hello [okruh ExpressRoute](howto-circuit-cli.md) a mějte ho zřízený poskytovatelem připojení hello.

  Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás. V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello. Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.

3. Zkontrolujte, zda je zřízený a také povolený toomake okruh ExpressRoute hello. Použijte hello následující ukázka:

  ```azurecli
  az network express-route list
  ```

  odpověď Hello je podobné toohello následující ukázka:

  ```azurecli
  "allowClassicOperations": false,
  "authorizations": [],
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
  "gatewayManagerEtag": null,
  "id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
  "location": "westus",
  "name": "MyCircuit",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
  "serviceProviderNotes": null,
  "serviceProviderProperties": {
    "bandwidthInMbps": 200,
    "peeringLocation": "Silicon Valley",
    "serviceProviderName": "Equinix"
  },
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits]
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

   Spusťte následující příklad tooconfigure partnerský vztah Microsoftu pro váš okruh hello:

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
  ```

### <a name="tooget-microsoft-peering-details"></a>tooget podrobností partnerského vztahu Microsoftu

Podrobnosti o konfiguraci můžete získat pomocí hello následující ukázka:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```

Hello výstup je podobné toohello následující ukázka:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzureMicrosoftPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": {
    "advertisedPublicPrefixes": [
        ""
      ],
     "advertisedPublicPrefixesState": "",
     "customerASN": ,
     "routingRegistryName": ""
  }
  "name": "AzureMicrosoftPeering",
  "peerAsn": ,
  "peeringType": "AzureMicrosoftPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="tooupdate-microsoft-peering-configuration"></a>konfiguraci partnerského vztahu Microsoftu tooupdate

Libovolnou část konfigurace hello můžete aktualizovat. Hello ohlášen jsou předpony hello okruhu aktualizováno z hodnoty 123.1.0.0/24 too124.1.0.0/24 v hello následující ukázka:

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="toodelete-microsoft-peering"></a>partnerský vztah Microsoftu toodelete

Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="next-steps"></a>Další kroky

Dalším krokem je [propojení virtuální sítě tooan okruh ExpressRoute](howto-linkvnet-cli.md).

* Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).
* Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).
* Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).