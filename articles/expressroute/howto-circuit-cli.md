---
title: "Vytvoření a úprava okruhu Azure ExpressRoute: rozhraní příkazového řádku | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate, zřizovat, ověřte, aktualizovat, odstranit a zrušit jejich zřízení okruhu ExpressRoute pomocí rozhraní příkazového řádku."
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
ms.date: 07/25/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 396e325658a59afadb209bb525cbb59ac775ae6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a>Vytvoření a úprava okruhu ExpressRoute pomocí rozhraní příkazového řádku


Tento článek popisuje, jak hello toocreate okruhu Azure ExpressRoute pomocí rozhraní příkazového řádku (CLI). Tento článek také ukazuje, jak toocheck hello stav, aktualizovat, nebo odstranit a zrušit jejich zřízení okruh. Pokud chcete toouse toowork jinou metodu s okruhy ExpressRoute, můžete vybrat hello článek z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a>Než začnete

* Než začnete, nainstalujte nejnovější verzi hello hello rozhraní příkazového řádku (2.0 nebo novější). Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli) a [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).
* Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.

## <a name="create-and-provision-an-expressroute-circuit"></a>Vytvořit a zřídit okruhu ExpressRoute

### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a>1. Přihlaste se tooyour účet Azure a vybrat své předplatné

toobegin konfiguraci přihlášení tooyour účet Azure. Následující příklady toohelp, ke kterým se připojujete pomocí hello:

```azurecli
az login
```

Zkontrolujte předplatná hello pro účet hello.

```azurecli
az account list
```

Vyberte hello předplatné, pro které chcete toocreate okruhu ExpressRoute.

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a>2. Získat hello seznam podporovaných zprostředkovatelů, umístění a šířek pásma

Než vytvoříte okruh ExpressRoute, musíte hello seznam poskytovatelů podporovaných připojení, umístění a možnosti šířky pásma. Hello rozhraní příkazového řádku příkaz 'az sítě express route seznamu, kteří, vrátí tato informace, které budete používat v dalších krocích:

```azurecli
az network express-route list-service-providers
```

odpověď Hello je podobné toohello následující ukázka:

```azurecli
[
  {
    "bandwidthsOffered": [
      {
        "offerName": "50Mbps",
        "valueInMbps": 50
      },
      {
        "offerName": "100Mbps",
        "valueInMbps": 100
      },
      {
        "offerName": "200Mbps",
        "valueInMbps": 200
      },
      {
        "offerName": "500Mbps",
        "valueInMbps": 500
      },
      {
        "offerName": "1Gbps",
        "valueInMbps": 1000
      },
      {
        "offerName": "2Gbps",
        "valueInMbps": 2000
      },
      {
        "offerName": "5Gbps",
        "valueInMbps": 5000
      },
      {
        "offerName": "10Gbps",
        "valueInMbps": 10000
      }
    ],
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/expressRouteServiceProviders/",
    "location": null,
    "name": "AARNet",
    "peeringLocations": [
      "Melbourne",
      "Sydney"
    ],
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "tags": null,
    "type": "Microsoft.Network/expressRouteServiceProviders"
  },
```

Hello odpovědi toosee zkontrolujte, zda je uvedený svého poskytovatele připojení. Poznamenejte si následující informace, které budete potřebovat při vytvoření okruhu hello:

* Name (Název)
* PeeringLocations
* BandwidthsOffered

Nyní jste připravené toocreate okruhu ExpressRoute.

### <a name="3-create-an-expressroute-circuit"></a>3. Vytvoření okruhu ExpressRoute

> [!IMPORTANT]
> Váš okruh ExpressRoute je účtován z hello okamžiku, kdy se objeví klíč služby. Tuto operaci proveďte, pokud poskytovatel připojení hello je připraven tooprovision hello okruh.
> 
> 

Pokud ještě nemáte skupinu prostředků, je musíte vytvořit před vytvořením váš okruh ExpressRoute. Můžete vytvořit skupinu prostředků spuštěním hello následující příkaz:

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

Hello následující příklad ukazuje, jak toocreate 200 MB/s ExpressRoute okruhu prostřednictvím Equinix ze Silicon Valley. Pokud používáte k jinému zprostředkovateli a různá nastavení, dosaďte tyto informace při zkontrolujte vaši žádost. 

Ujistěte se, že jste zadali správné úrovně SKU hello a řada SKU:

* Úroveň SKU Určuje, zda je povolen ExpressRoute standard nebo doplněk ExpressRoute premium. Můžete zadat "Standard" hello tooget standardní SKU nebo 'Premium' pro doplněk premium hello.
* Řada SKU Určuje typ fakturace hello. Pro plán neomezená data na úrovni můžete zadat 'Metereddata' pro plán měření podle objemu dat a 'Unlimiteddata'. Můžete změnit hello fakturace typu z 'Metereddata too'Unlimiteddata, ale nelze změnit typ hello z too'Metereddata 'Unlimiteddata' '.


Váš okruh ExpressRoute je účtován z hello okamžiku, kdy se objeví klíč služby. Následující ukázka Hello je žádost o nový klíč služby:

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

Hello odpovědi obsahuje klíč služby hello.

### <a name="4-list-all-expressroute-circuits"></a>4. Zobrazí seznam všech okruhy ExpressRoute

tooget seznam všech hello okruhy ExpressRoute, které jste vytvořili, spusťte příkaz "az sítě express route seznam" hello. Tyto informace kdykoli můžete načíst pomocí tohoto příkazu. toolist všechny okruhů, zkontrolujte hello volání bez parametrů.

```azurecli
az network express-route list
```

Klíč služby, je uvedena ve hello *klíč ServiceKey* pole hello odpovědi.

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
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

Podrobný popis všech parametrů hello získáte tak, že spuštěný příkaz hello pomocí hello '-h "parametr.

```azurecli
az network express-route list -h
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>5. Odeslání poskytovatele připojení klíče tooyour hello služby pro zřizování

'ServiceProviderProvisioningState' poskytuje informace o hello aktuální stav zřizování na straně hello poskytovatele služeb. Hello stavu poskytuje stav hello hello straně společnosti Microsoft. Další informace najdete v tématu hello [pracovních článku](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Když vytvoříte nový okruh ExpressRoute, hello okruh je ve hello následující stav:

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

změny toohello okruhu Hello následující stavu, pokud je zprostředkovatel připojení k hello v procesu hello povolení pro vás:

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

Pro jste toobe možné toouse okruh ExpressRoute musí být v hello následující stav:

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>6. Pravidelně kontrolovat hello stav a stav hello hello okruh klíče

Kontrola stavu hello a stavu hello hello okruh klíče umožňuje vědět, pokud se váš poskytovatel povolil váš okruh. Po nakonfiguroval hello okruh, se zobrazí 'ServiceProviderProvisioningState' jako 'Zajištěno', jak ukazuje následující příklad hello:

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
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

### <a name="7-create-your-routing-configuration"></a>7. Vytvořte vlastní konfiguraci směrování

Podrobné pokyny najdete v tématu hello [konfigurace směrování pro okruh ExpressRoute](howto-routing-cli.md) článek toocreate a upravit partnerských vztahů pro okruh.

> [!IMPORTANT]
> Tyto pokyny platí pouze toocircuits, které jsou vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení konfiguruje a spravuje směrování.
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a>8. Propojení virtuální sítě tooan okruh ExpressRoute

V dalším kroku propojte virtuální sítě tooyour okruh ExpressRoute. Použití hello [propojení virtuální sítě okruhů tooExpressRoute](howto-linkvnet-cli.md) článku.

## <a name="modify"></a>Úprava okruhu ExpressRoute

Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute. Můžete provést následující změny bez výpadků hello:

* Můžete povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.
* Zadaný na portu hello je dostupná kapacita můžete zvýšit hello šířka pásma okruhu ExpressRoute. Však hello šířka pásma okruhu přechod na starší verzi není podporován. 
* Z tooUnlimited dat – měření podle objemu dat, můžete změnit plán měření hello. Změna však hello měření plán z tooMetered neomezená Data na úrovni, dat není podporováno.
* Můžete povolit nebo zakázat *povolit klasické operace*.

Další informace o omezení a omezení, najdete v části hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

### <a name="tooenable-hello-expressroute-premium-add-on"></a>doplněk ExpressRoute premium tooenable hello

Doplněk ExpressRoute premium hello u existujícího okruhu můžete povolit pomocí hello následující příkaz:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

okruh Hello má nyní hello ExpressRoute premium rozšíření funkce povolené. Můžeme začít hned, jak byl úspěšně spuštěn příkaz hello fakturace pro funkci rozšíření hello premium.

### <a name="toodisable-hello-expressroute-premium-add-on"></a>doplněk ExpressRoute premium toodisable hello

> [!IMPORTANT]
> Tato operace může selhat, pokud používáte prostředky, které jsou větší než co je povolené standardní okruh hello.
> 
> 

Před zakázáním hello doplněk ExpressRoute premium, pochopit hello následující kritéria:

* Předtím, než jste se downgradovat z úrovně premium toostandard, musí se ujistěte, že máte méně než 10 virtuální sítě propojené toohello okruh. Pokud máte více než 10, vaše žádost o aktualizaci nezdaří a budeme vám účtovat za zvýhodněné sazby.
* Je nutné zrušit všechny virtuální sítě v jiných geopolitické oblasti. Pokud nemáte propojení virtuálních sítí, vaše žádost o aktualizaci nezdaří a budeme vám účtovat za zvýhodněné sazby.
* Směrovací tabulka musí být menší než 4 000 tras pro soukromý partnerský vztah. Pokud vaše velikost tabulky trasy je větší než 4 000 tras, zahodí se relace protokolu BGP hello. opětovně Hello relace nebudou povolena, dokud hello počet předpon inzerovaných je menší než 4 000.

Doplněk ExpressRoute premium hello u existujícího okruhu hello můžete zakázat pomocí hello následující ukázka:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a>šířku pásma okruhu ExpressRoute tooupdate hello

Možnosti šířky pásma hello podporované pro zprostředkovatele, zkontrolujte hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md). Můžete vybrat libovolnou velikost, která je větší než velikost hello existujícím okruhem.

> [!IMPORTANT]
> Pokud je na existující port hello nedostatečné kapacity, můžete mít okruh ExpressRoute toorecreate hello. Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat hello okruh.
>
> Nelze zmenšit hello šířku pásma okruhu ExpressRoute bez přerušení. Přechod na starší verzi šířky pásma vyžaduje jste toodeprovision hello okruh ExpressRoute a pak znova nezajistíte nové okruh ExpressRoute.
>

Až se rozhodnete hello velikost, které potřebujete, použijte následující příkaz tooresize hello váš okruh:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

Váš okruh je velikost na straně Microsoft hello. Dále je třeba kontaktovat vaše připojení poskytovatele tooupdate konfigurace na jejich straně toomatch tuto změnu. Když provedete toto oznámení, začneme fakturace můžete pro možnost hello aktualizovat šířky pásma.

### <a name="toomove-hello-sku-from-metered-toounlimited"></a>toomove hello SKU z monitorovaných toounlimited

Hello SKU okruhu ExpressRoute můžete změnit pomocí hello následující ukázka:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a>toocontrol přístup toohello classic a Resource Manager prostředí

Přečtěte si pokyny hello v [okruhy ExpressRoute přesunout z modelu nasazení Resource Manager classic toohello hello](expressroute-howto-move-arm.md).

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Zrušení zřízení a odstraňování okruhu ExpressRoute

toodeprovision a odstranit okruh ExpressRoute, ujistěte se, že rozumíte hello následující kritéria:

* Je nutné zrušit všechny virtuální sítě od hello okruh ExpressRoute. Pokud se tato operace nezdaří, zkontrolujte, že toosee, pokud žádné virtuální sítě propojené toohello okruh.
* Pokud je hello ExpressRoute okruhu poskytovatele služeb Stav zřizování **zřizování** nebo **zajištěno**, na jejich straně, musíte pracovat se váš okruh hello toodeprovision zprostředkovatele služby. Pokračovat tooreserve prostředky jsme vám účtovat, dokud poskytovatele služeb hello dokončení zrušení zřízení hello okruhu a upozorní nás.
* Hello okruhu můžete odstranit, pokud má poskytovatel služeb hello zrušit hello okruh. Když je zrušit okruh, poskytovatele služeb hello Stav zřizování se nastaví příliš**není zajišťováno**. To zastaví fakturace hello okruh.

Váš okruh ExpressRoute můžete odstranit spuštěním hello následující příkaz:

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Další kroky

Po vytvoření okruhu, ujistěte se, zda text hello následující úlohy:

* [Vytvoření a úprava směrování pro okruhu ExpressRoute](howto-routing-cli.md)
* [Propojení vaší virtuální sítě tooyour okruh ExpressRoute](howto-linkvnet-cli.md)
