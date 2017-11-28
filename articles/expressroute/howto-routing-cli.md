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
# <a name="create-and-modify-routing-for-an-expressroute-circuit-using-cli"></a><span data-ttu-id="8c3e1-104">Vytvoření a úprava směrování pro okruh ExpressRoute pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="8c3e1-104">Create and modify routing for an ExpressRoute circuit using CLI</span></span>

<span data-ttu-id="8c3e1-105">Tento článek vám pomůže vytvořit a spravovat konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager hello pomocí rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in hello Resource Manager deployment model using CLI.</span></span> <span data-ttu-id="8c3e1-106">Můžete také zkontrolovat stav hello, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-106">You can also check hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="8c3e1-107">Pokud chcete toouse toowork jinou metodu s váš okruh, vyberte článek z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-107">If you want toouse a different method toowork with your circuit, select an article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8c3e1-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8c3e1-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="8c3e1-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c3e1-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="8c3e1-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8c3e1-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="8c3e1-111">Video - soukromého partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="8c3e1-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="8c3e1-112">Video - veřejného partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="8c3e1-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="8c3e1-113">Video - partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="8c3e1-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="8c3e1-114">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="8c3e1-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="8c3e1-115">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="8c3e1-115">Configuration prerequisites</span></span>

* <span data-ttu-id="8c3e1-116">Než začnete, nainstalujte nejnovější verzi hello hello rozhraní příkazového řádku (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="8c3e1-116">Before beginning, install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="8c3e1-117">Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8c3e1-117">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="8c3e1-118">Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovního postupu](expressroute-workflows.md) stránky před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-118">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflow](expressroute-workflows.md) pages before you begin configuration.</span></span>
* <span data-ttu-id="8c3e1-119">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="8c3e1-120">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](howto-circuit-cli.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-120">Follow hello instructions too[Create an ExpressRoute circuit](howto-circuit-cli.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="8c3e1-121">Hello okruh ExpressRoute musí být ve stavu zřízený a povolený pro vás toobe možné toorun hello příkazy v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-121">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello commands in this article.</span></span>

<span data-ttu-id="8c3e1-122">Tyto pokyny platí pouze toocircuits vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-122">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="8c3e1-123">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

<span data-ttu-id="8c3e1-124">Můžete nakonfigurovat jednu, dvě nebo všechny tři partnerské vztahy (Azure privátní, veřejný Azure a Microsoft) pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-124">You can configure one, two, or all three peerings (Azure private, Azure public, and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="8c3e1-125">Partnerské vztahy můžete konfigurovat v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-125">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="8c3e1-126">Přesvědčte se, že dokončení hello konfiguraci každého partnerského vztahu jeden po druhém.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-126">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="8c3e1-127">Soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="8c3e1-127">Azure private peering</span></span>

<span data-ttu-id="8c3e1-128">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello privátní konfigurace partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-128">This section helps you create, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="8c3e1-129">toocreate soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="8c3e1-129">toocreate Azure private peering</span></span>

1. <span data-ttu-id="8c3e1-130">Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-130">Install hello latest version of Azure CLI.</span></span> <span data-ttu-id="8c3e1-131">Musíte použít nejnovější verzi hello hello rozhraní příkazového řádku Azure (CLI). * Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-131">You must use hello latest version of hello Azure Command-line Interface (CLI).* Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="8c3e1-132">Vyberte předplatné hello chcete toocreate okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="8c3e1-132">Select hello subscription you want toocreate ExpressRoute circuit</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="8c3e1-133">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-133">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="8c3e1-134">Postupujte podle pokynů toocreate hello [okruh ExpressRoute](howto-circuit-cli.md) a mějte ho zřízený poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-134">Follow hello instructions toocreate an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="8c3e1-135">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-135">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="8c3e1-136">V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-136">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="8c3e1-137">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-137">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="8c3e1-138">Zkontrolujte, zda je zřízený a také povolený toomake okruh ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-138">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="8c3e1-139">Použijte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-139">Use hello following example:</span></span>

  ```azurecli
  az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
  ```

  <span data-ttu-id="8c3e1-140">odpověď Hello je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-140">hello response is similar toohello following example:</span></span>

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

4. <span data-ttu-id="8c3e1-141">Nakonfigurujte soukromý partnerský vztah Azure pro okruh hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-141">Configure Azure private peering for hello circuit.</span></span> <span data-ttu-id="8c3e1-142">Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-142">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="8c3e1-143">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-143">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="8c3e1-144">Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-144">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="8c3e1-145">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-145">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="8c3e1-146">Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-146">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="8c3e1-147">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-147">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="8c3e1-148">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-148">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="8c3e1-149">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-149">AS number for peering.</span></span> <span data-ttu-id="8c3e1-150">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-150">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="8c3e1-151">Pro tento partnerský vztah můžete použít soukromé číslo AS.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-151">You can use a private AS number for this peering.</span></span> <span data-ttu-id="8c3e1-152">Zkontrolujte, že nepoužíváte 65515.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-152">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="8c3e1-153">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-153">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="8c3e1-154">Použijte následující příklad tooconfigure privátní partnerský vztah pro váš okruh Azure hello:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-154">Use hello following example tooconfigure Azure private peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
  ```

  <span data-ttu-id="8c3e1-155">Pokud si zvolíte toouse hodnotu hash MD5, použijte následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-155">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="8c3e1-156">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-156">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  > 

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="8c3e1-157">tooview Azure privátní partnerský vztah podrobnosti</span><span class="sxs-lookup"><span data-stu-id="8c3e1-157">tooview Azure private peering details</span></span>

<span data-ttu-id="8c3e1-158">Podrobnosti o konfiguraci můžete získat pomocí hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-158">You can get configuration details by using hello following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

<span data-ttu-id="8c3e1-159">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-159">hello output is similar toohello following example:</span></span>

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

### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="8c3e1-160">tooupdate konfigurace soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="8c3e1-160">tooupdate Azure private peering configuration</span></span>

<span data-ttu-id="8c3e1-161">Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-161">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="8c3e1-162">V tomto příkladu je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 100 too500.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-162">In this example, hello VLAN ID of hello circuit is being updated from 100 too500.</span></span>

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="8c3e1-163">toodelete soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="8c3e1-163">toodelete Azure private peering</span></span>

<span data-ttu-id="8c3e1-164">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-164">You can remove your peering configuration by running hello following example:</span></span>

> [!WARNING]
> <span data-ttu-id="8c3e1-165">Je nutné zajistit, že všechny virtuální sítě jsou před spuštěním tohoto příkladu odpojit od hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-165">You must ensure that all virtual networks are unlinked from hello ExpressRoute circuit before running this example.</span></span> 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="azure-public-peering"></a><span data-ttu-id="8c3e1-166">Veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="8c3e1-166">Azure public peering</span></span>

<span data-ttu-id="8c3e1-167">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello veřejné konfigurace partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-167">This section helps you create, get, update, and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="8c3e1-168">toocreate veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="8c3e1-168">toocreate Azure public peering</span></span>

1. <span data-ttu-id="8c3e1-169">Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-169">Install hello latest version of Azure CLI.</span></span> <span data-ttu-id="8c3e1-170">Musíte použít nejnovější verzi hello hello rozhraní příkazového řádku Azure (CLI). * Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-170">You must use hello latest version of hello Azure Command-line Interface (CLI).* Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="8c3e1-171">Vyberte hello předplatné, pro které chcete toocreate okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-171">Select hello subscription for which you want toocreate ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="8c3e1-172">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-172">Create an ExpressRoute circuit.</span></span>  <span data-ttu-id="8c3e1-173">Postupujte podle pokynů toocreate hello [okruh ExpressRoute](howto-circuit-cli.md) a mějte ho zřízený poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-173">Follow hello instructions toocreate an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="8c3e1-174">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat, že poskytovatel připojení umožňuje soukromý partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-174">If your connectivity provider offers managed Layer 3 services, you can request that your connectivity provider enables Azure private peering for you.</span></span> <span data-ttu-id="8c3e1-175">V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-175">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="8c3e1-176">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-176">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="8c3e1-177">Zkontrolujte tooensure okruh ExpressRoute hello je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-177">Check hello ExpressRoute circuit tooensure it is provisioned and also enabled.</span></span> <span data-ttu-id="8c3e1-178">Použijte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-178">Use hello following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="8c3e1-179">odpověď Hello je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-179">hello response is similar toohello following example:</span></span>

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

4. <span data-ttu-id="8c3e1-180">Nakonfigurujte veřejný partnerský vztah Azure pro okruh hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-180">Configure Azure public peering for hello circuit.</span></span> <span data-ttu-id="8c3e1-181">Ujistěte se, že máte hello následující informace, než budete pokračovat dál.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-181">Make sure that you have hello following information before you proceed further.</span></span>

  * <span data-ttu-id="8c3e1-182">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-182">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="8c3e1-183">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-183">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="8c3e1-184">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-184">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="8c3e1-185">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-185">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="8c3e1-186">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-186">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="8c3e1-187">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-187">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="8c3e1-188">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-188">AS number for peering.</span></span> <span data-ttu-id="8c3e1-189">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-189">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="8c3e1-190">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-190">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="8c3e1-191">Spusťte následující příklad tooconfigure veřejný partnerský vztah pro váš okruh Azure hello:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-191">Run hello following example tooconfigure Azure public peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
  ```

  <span data-ttu-id="8c3e1-192">Pokud si zvolíte toouse hodnotu hash MD5, použijte následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-192">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="8c3e1-193">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-193">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="8c3e1-194">tooview Azure veřejného partnerského vztahu podrobnosti</span><span class="sxs-lookup"><span data-stu-id="8c3e1-194">tooview Azure public peering details</span></span>

<span data-ttu-id="8c3e1-195">Můžete získat podrobnosti o konfiguraci pomocí hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-195">You can get configuration details using hello following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

<span data-ttu-id="8c3e1-196">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-196">hello output is similar toohello following example:</span></span>

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

### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="8c3e1-197">tooupdate konfigurace veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="8c3e1-197">tooupdate Azure public peering configuration</span></span>

<span data-ttu-id="8c3e1-198">Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-198">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="8c3e1-199">V tomto příkladu je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 200 too600.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-199">In this example, hello VLAN ID of hello circuit is being updated from 200 too600.</span></span>

```azurecli
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="8c3e1-200">toodelete veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="8c3e1-200">toodelete Azure public peering</span></span>

<span data-ttu-id="8c3e1-201">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-201">You can remove your peering configuration by running hello following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="microsoft-peering"></a><span data-ttu-id="8c3e1-202">Partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="8c3e1-202">Microsoft peering</span></span>

<span data-ttu-id="8c3e1-203">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu hello Microsoftu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-203">This section helps you create, get, update, and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8c3e1-204">Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím hello partnerský vztah Microsoftu, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-204">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through hello Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="8c3e1-205">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-205">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span> <span data-ttu-id="8c3e1-206">Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8c3e1-206">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="8c3e1-207">partnerský vztah Microsoftu toocreate</span><span class="sxs-lookup"><span data-stu-id="8c3e1-207">toocreate Microsoft peering</span></span>

1. <span data-ttu-id="8c3e1-208">Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-208">Install hello latest version of Azure CLI.</span></span> <span data-ttu-id="8c3e1-209">Použití hello nejnovější verzi hello rozhraní příkazového řádku Azure (CLI). * Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-209">Use hello latest version of hello Azure Command-line Interface (CLI).* Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="8c3e1-210">Vyberte hello předplatné, pro které chcete toocreate okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-210">Select hello subscription for which you want toocreate ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="8c3e1-211">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-211">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="8c3e1-212">Postupujte podle pokynů toocreate hello [okruh ExpressRoute](howto-circuit-cli.md) a mějte ho zřízený poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-212">Follow hello instructions toocreate an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="8c3e1-213">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-213">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="8c3e1-214">V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-214">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="8c3e1-215">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-215">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>

3. <span data-ttu-id="8c3e1-216">Zkontrolujte, zda je zřízený a také povolený toomake okruh ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-216">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="8c3e1-217">Použijte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-217">Use hello following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="8c3e1-218">odpověď Hello je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-218">hello response is similar toohello following example:</span></span>

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

4. <span data-ttu-id="8c3e1-219">Nakonfigurujte partnerský vztah Microsoftu pro okruh hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-219">Configure Microsoft peering for hello circuit.</span></span> <span data-ttu-id="8c3e1-220">Ujistěte se, že máte hello následující informace, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-220">Make sure that you have hello following information before you proceed.</span></span>

  * <span data-ttu-id="8c3e1-221">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-221">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="8c3e1-222">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-222">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="8c3e1-223">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-223">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="8c3e1-224">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-224">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="8c3e1-225">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-225">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="8c3e1-226">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-226">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="8c3e1-227">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-227">AS number for peering.</span></span> <span data-ttu-id="8c3e1-228">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-228">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="8c3e1-229">Inzerované předpony: musíte poskytnout seznam všech předpon plánování tooadvertise přes relaci protokolu BGP hello.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-229">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="8c3e1-230">Přijímají se jenom předpony veřejných IP adres.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-230">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="8c3e1-231">Pokud máte v plánu toosend sadu předpon, můžete odeslat seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-231">If you plan toosend a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="8c3e1-232">Tyto předpony musí být registrovaný tooyou u rir / IRR.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-232">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
  * <span data-ttu-id="8c3e1-233">**Volitelné -** zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované toohello číslo AS partnerského vztahu, můžete zadat hello jako číslo toowhich jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-233">**Optional -** Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span>
  * <span data-ttu-id="8c3e1-234">Název registru směrování: Můžete zadat hello RIR / IRR, u které hello jako číslo a předpony jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-234">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="8c3e1-235">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-235">**Optional -** An MD5 hash if you choose toouse one.</span></span>

   <span data-ttu-id="8c3e1-236">Spusťte následující příklad tooconfigure partnerský vztah Microsoftu pro váš okruh hello:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-236">Run hello following example tooconfigure Microsoft peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
  ```

### <a name="tooget-microsoft-peering-details"></a><span data-ttu-id="8c3e1-237">tooget podrobností partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="8c3e1-237">tooget Microsoft peering details</span></span>

<span data-ttu-id="8c3e1-238">Podrobnosti o konfiguraci můžete získat pomocí hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-238">You can get configuration details by using hello following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```

<span data-ttu-id="8c3e1-239">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-239">hello output is similar toohello following example:</span></span>

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

### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="8c3e1-240">konfiguraci partnerského vztahu Microsoftu tooupdate</span><span class="sxs-lookup"><span data-stu-id="8c3e1-240">tooupdate Microsoft peering configuration</span></span>

<span data-ttu-id="8c3e1-241">Libovolnou část konfigurace hello můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="8c3e1-241">You can update any part of hello configuration.</span></span> <span data-ttu-id="8c3e1-242">Hello ohlášen jsou předpony hello okruhu aktualizováno z hodnoty 123.1.0.0/24 too124.1.0.0/24 v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-242">hello advertised prefixes of hello circuit are being updated from 123.1.0.0/24 too124.1.0.0/24 in hello following example:</span></span>

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="8c3e1-243">partnerský vztah Microsoftu toodelete</span><span class="sxs-lookup"><span data-stu-id="8c3e1-243">toodelete Microsoft peering</span></span>

<span data-ttu-id="8c3e1-244">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="8c3e1-244">You can remove your peering configuration by running hello following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="next-steps"></a><span data-ttu-id="8c3e1-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c3e1-245">Next steps</span></span>

<span data-ttu-id="8c3e1-246">Dalším krokem je [propojení virtuální sítě tooan okruh ExpressRoute](howto-linkvnet-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8c3e1-246">Next step, [Link a VNet tooan ExpressRoute circuit](howto-linkvnet-cli.md).</span></span>

* <span data-ttu-id="8c3e1-247">Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="8c3e1-247">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="8c3e1-248">Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="8c3e1-248">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="8c3e1-249">Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8c3e1-249">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>