---
title: "Postup konfigurace směrování pro okruh Azure ExpressRoute: rozhraní příkazového řádku | Microsoft Docs"
description: "Tento článek vám pomůže vytvořit a zřizování soukromého a veřejného a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak kontrolovat stav partnerských vztahů pro váš okruh, aktualizovat je nebo je odstranit."
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
ms.openlocfilehash: fbf0bd9a139c22bbd63755f6df445f6596aaccc5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-routing-for-an-expressroute-circuit-using-cli"></a><span data-ttu-id="2be03-104">Vytvoření a úprava směrování pro okruh ExpressRoute pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="2be03-104">Create and modify routing for an ExpressRoute circuit using CLI</span></span>

<span data-ttu-id="2be03-105">Tento článek vám pomůže vytvořit a spravovat konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager pomocí rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2be03-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in the Resource Manager deployment model using CLI.</span></span> <span data-ttu-id="2be03-106">Můžete také zkontrolovat stav, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-106">You can also check the status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="2be03-107">Pokud chcete použít jinou metodu pro práci se váš okruh, vyberte článek z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="2be03-107">If you want to use a different method to work with your circuit, select an article from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2be03-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2be03-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="2be03-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2be03-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="2be03-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2be03-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="2be03-111">Video - soukromého partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="2be03-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="2be03-112">Video - veřejného partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="2be03-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="2be03-113">Video - partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="2be03-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="2be03-114">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="2be03-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="2be03-115">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="2be03-115">Configuration prerequisites</span></span>

* <span data-ttu-id="2be03-116">Než začnete, nainstalujte si nejnovější verzi příkazů rozhraní příkazového řádku (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="2be03-116">Before beginning, install the latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="2be03-117">Informace o instalaci příkazů rozhraní příkazového řádku najdete v tématu [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2be03-117">For information about installing the CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="2be03-118">Ujistěte se, že jste si přečetli [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovního postupu](expressroute-workflows.md) stránky před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2be03-118">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflow](expressroute-workflows.md) pages before you begin configuration.</span></span>
* <span data-ttu-id="2be03-119">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="2be03-120">Než budete pokračovat, podle pokynů [vytvořte okruh ExpressRoute](howto-circuit-cli.md) a mějte ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-120">Follow the instructions to [Create an ExpressRoute circuit](howto-circuit-cli.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="2be03-121">Okruh ExpressRoute musí být ve stavu zřízený a povolený pro vás mohli ke spuštění příkazů v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="2be03-121">The ExpressRoute circuit must be in a provisioned and enabled state for you to be able to run the commands in this article.</span></span>

<span data-ttu-id="2be03-122">Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="2be03-122">These instructions only apply to circuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="2be03-123">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="2be03-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

<span data-ttu-id="2be03-124">Můžete nakonfigurovat jednu, dvě nebo všechny tři partnerské vztahy (Azure privátní, veřejný Azure a Microsoft) pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-124">You can configure one, two, or all three peerings (Azure private, Azure public, and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="2be03-125">Partnerské vztahy můžete konfigurovat v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2be03-125">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="2be03-126">Musíte se ale přesvědčit, že jste vždy konfiguraci každého partnerského vztahu dokončili.</span><span class="sxs-lookup"><span data-stu-id="2be03-126">However, you must make sure that you complete the configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="2be03-127">Soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-127">Azure private peering</span></span>

<span data-ttu-id="2be03-128">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit Azure konfigurace soukromého partnerského vztahu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-128">This section helps you create, get, update, and delete the Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-private-peering"></a><span data-ttu-id="2be03-129">Vytvoření soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-129">To create Azure private peering</span></span>

1. <span data-ttu-id="2be03-130">Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2be03-130">Install the latest version of Azure CLI.</span></span> <span data-ttu-id="2be03-131">Je nutné použít nejnovější verzi z rozhraní příkazového řádku Azure (CLI). * Zkontrolujte [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2be03-131">You must use the latest version of the Azure Command-line Interface (CLI).* Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="2be03-132">Vyberte předplatné, pro které chcete vytvořit okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-132">Select the subscription you want to create ExpressRoute circuit</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="2be03-133">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-133">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="2be03-134">Podle pokynů vytvořte [okruh ExpressRoute](howto-circuit-cli.md) a mějte ho zřízený poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-134">Follow the instructions to create an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="2be03-135">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil soukromý partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="2be03-135">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="2be03-136">V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="2be03-136">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="2be03-137">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí následující kroky.</span><span class="sxs-lookup"><span data-stu-id="2be03-137">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="2be03-138">Zkontrolujte okruh ExpressRoute a ujistěte se, že je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="2be03-138">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="2be03-139">Použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-139">Use the following example:</span></span>

  ```azurecli
  az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
  ```

  <span data-ttu-id="2be03-140">Odpověď je stejný jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2be03-140">The response is similar to the following example:</span></span>

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

4. <span data-ttu-id="2be03-141">Nakonfigurujte soukromý partnerský vztah Azure pro okruh.</span><span class="sxs-lookup"><span data-stu-id="2be03-141">Configure Azure private peering for the circuit.</span></span> <span data-ttu-id="2be03-142">Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:</span><span class="sxs-lookup"><span data-stu-id="2be03-142">Make sure that you have the following items before you proceed with the next steps:</span></span>

  * <span data-ttu-id="2be03-143">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-143">A /30 subnet for the primary link.</span></span> <span data-ttu-id="2be03-144">Podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2be03-144">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="2be03-145">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-145">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="2be03-146">Podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2be03-146">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="2be03-147">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="2be03-147">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="2be03-148">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="2be03-148">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="2be03-149">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="2be03-149">AS number for peering.</span></span> <span data-ttu-id="2be03-150">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="2be03-150">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="2be03-151">Pro tento partnerský vztah můžete použít soukromé číslo AS.</span><span class="sxs-lookup"><span data-stu-id="2be03-151">You can use a private AS number for this peering.</span></span> <span data-ttu-id="2be03-152">Zkontrolujte, že nepoužíváte 65515.</span><span class="sxs-lookup"><span data-stu-id="2be03-152">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="2be03-153">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="2be03-153">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="2be03-154">Následující příklad použijte ke konfiguraci soukromého partnerského vztahu Azure pro váš okruh:</span><span class="sxs-lookup"><span data-stu-id="2be03-154">Use the following example to configure Azure private peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
  ```

  <span data-ttu-id="2be03-155">Pokud se rozhodnete použít hodnotu hash MD5, použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-155">If you choose to use an MD5 hash, use the following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="2be03-156">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="2be03-156">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  > 

### <a name="to-view-azure-private-peering-details"></a><span data-ttu-id="2be03-157">Zobrazení podrobností soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-157">To view Azure private peering details</span></span>

<span data-ttu-id="2be03-158">Můžete získat podrobnosti o konfiguraci pomocí následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-158">You can get configuration details by using the following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

<span data-ttu-id="2be03-159">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="2be03-159">The output is similar to the following example:</span></span>

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

### <a name="to-update-azure-private-peering-configuration"></a><span data-ttu-id="2be03-160">Aktualizace konfigurace soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-160">To update Azure private peering configuration</span></span>

<span data-ttu-id="2be03-161">Libovolnou část konfigurace pomocí následujícího příkladu můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="2be03-161">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="2be03-162">V tomto příkladu je ID sítě VLAN okruhu aktualizováno z 100 na 500.</span><span class="sxs-lookup"><span data-stu-id="2be03-162">In this example, the VLAN ID of the circuit is being updated from 100 to 500.</span></span>

```azurecli
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="to-delete-azure-private-peering"></a><span data-ttu-id="2be03-163">Odstranění soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-163">To delete Azure private peering</span></span>

<span data-ttu-id="2be03-164">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-164">You can remove your peering configuration by running the following example:</span></span>

> [!WARNING]
> <span data-ttu-id="2be03-165">Je nutné zajistit, že všechny virtuální sítě jsou před spuštěním tohoto příkladu odpojit od okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-165">You must ensure that all virtual networks are unlinked from the ExpressRoute circuit before running this example.</span></span> 
> 
> 

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="azure-public-peering"></a><span data-ttu-id="2be03-166">Veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-166">Azure public peering</span></span>

<span data-ttu-id="2be03-167">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit veřejného partnerského vztahu konfiguraci Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-167">This section helps you create, get, update, and delete the Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-public-peering"></a><span data-ttu-id="2be03-168">Vytvoření veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-168">To create Azure public peering</span></span>

1. <span data-ttu-id="2be03-169">Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2be03-169">Install the latest version of Azure CLI.</span></span> <span data-ttu-id="2be03-170">Je nutné použít nejnovější verzi z rozhraní příkazového řádku Azure (CLI). * Zkontrolujte [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2be03-170">You must use the latest version of the Azure Command-line Interface (CLI).* Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="2be03-171">Vyberte předplatné, pro který chcete vytvořit okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-171">Select the subscription for which you want to create ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="2be03-172">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-172">Create an ExpressRoute circuit.</span></span>  <span data-ttu-id="2be03-173">Podle pokynů vytvořte [okruh ExpressRoute](howto-circuit-cli.md) a mějte ho zřízený poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-173">Follow the instructions to create an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="2be03-174">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat, že poskytovatel připojení umožňuje soukromý partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="2be03-174">If your connectivity provider offers managed Layer 3 services, you can request that your connectivity provider enables Azure private peering for you.</span></span> <span data-ttu-id="2be03-175">V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="2be03-175">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="2be03-176">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí následující kroky.</span><span class="sxs-lookup"><span data-stu-id="2be03-176">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="2be03-177">Zkontrolujte okruh ExpressRoute a ověřte je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="2be03-177">Check the ExpressRoute circuit to ensure it is provisioned and also enabled.</span></span> <span data-ttu-id="2be03-178">Použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-178">Use the following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="2be03-179">Odpověď je stejný jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2be03-179">The response is similar to the following example:</span></span>

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

4. <span data-ttu-id="2be03-180">Nakonfigurujte veřejný partnerský vztah Azure pro okruh.</span><span class="sxs-lookup"><span data-stu-id="2be03-180">Configure Azure public peering for the circuit.</span></span> <span data-ttu-id="2be03-181">Ujistěte se, že máte následující informace předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="2be03-181">Make sure that you have the following information before you proceed further.</span></span>

  * <span data-ttu-id="2be03-182">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-182">A /30 subnet for the primary link.</span></span> <span data-ttu-id="2be03-183">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="2be03-183">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="2be03-184">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-184">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="2be03-185">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="2be03-185">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="2be03-186">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="2be03-186">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="2be03-187">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="2be03-187">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="2be03-188">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="2be03-188">AS number for peering.</span></span> <span data-ttu-id="2be03-189">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="2be03-189">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="2be03-190">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="2be03-190">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="2be03-191">Spusťte následující příklad konfigurace veřejného partnerského vztahu Azure pro váš okruh:</span><span class="sxs-lookup"><span data-stu-id="2be03-191">Run the following example to configure Azure public peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
  ```

  <span data-ttu-id="2be03-192">Pokud se rozhodnete použít hodnotu hash MD5, použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-192">If you choose to use an MD5 hash, use the following example:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="2be03-193">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="2be03-193">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>

### <a name="to-view-azure-public-peering-details"></a><span data-ttu-id="2be03-194">Zobrazení podrobností veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-194">To view Azure public peering details</span></span>

<span data-ttu-id="2be03-195">Můžete získat podrobnosti o konfiguraci pomocí následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-195">You can get configuration details using the following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

<span data-ttu-id="2be03-196">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="2be03-196">The output is similar to the following example:</span></span>

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

### <a name="to-update-azure-public-peering-configuration"></a><span data-ttu-id="2be03-197">Aktualizace konfigurace veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-197">To update Azure public peering configuration</span></span>

<span data-ttu-id="2be03-198">Libovolnou část konfigurace pomocí následujícího příkladu můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="2be03-198">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="2be03-199">V tomto příkladu je ID sítě VLAN okruhu aktualizováno z hodnoty 200 na hodnotu 600.</span><span class="sxs-lookup"><span data-stu-id="2be03-199">In this example, the VLAN ID of the circuit is being updated from 200 to 600.</span></span>

```azurecli
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="to-delete-azure-public-peering"></a><span data-ttu-id="2be03-200">Odstranění veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="2be03-200">To delete Azure public peering</span></span>

<span data-ttu-id="2be03-201">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-201">You can remove your peering configuration by running the following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="microsoft-peering"></a><span data-ttu-id="2be03-202">Partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="2be03-202">Microsoft peering</span></span>

<span data-ttu-id="2be03-203">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu Microsoftu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-203">This section helps you create, get, update, and delete the Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2be03-204">Okruhy ExpressRoute, které byly nakonfigurovány před 1. srpna 2017 partnerského vztahu Microsoftu bude mít všechny služby předpony inzerované prostřednictvím Microsoft partnerský vztah, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="2be03-204">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through the Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="2be03-205">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované dokud trasy filtr je připojen k okruhu.</span><span class="sxs-lookup"><span data-stu-id="2be03-205">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span> <span data-ttu-id="2be03-206">Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2be03-206">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="to-create-microsoft-peering"></a><span data-ttu-id="2be03-207">Vytvoření partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="2be03-207">To create Microsoft peering</span></span>

1. <span data-ttu-id="2be03-208">Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2be03-208">Install the latest version of Azure CLI.</span></span> <span data-ttu-id="2be03-209">Použít nejnovější verzi z rozhraní příkazového řádku Azure (CLI). * Zkontrolujte [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2be03-209">Use the latest version of the Azure Command-line Interface (CLI).* Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

  ```azurecli
  az login
  ```

  <span data-ttu-id="2be03-210">Vyberte předplatné, pro který chcete vytvořit okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-210">Select the subscription for which you want to create ExpressRoute circuit.</span></span>

  ```azurecli
  az account set --subscription "<subscription ID>"
  ```
2. <span data-ttu-id="2be03-211">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2be03-211">Create an ExpressRoute circuit.</span></span> <span data-ttu-id="2be03-212">Podle pokynů vytvořte [okruh ExpressRoute](howto-circuit-cli.md) a mějte ho zřízený poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-212">Follow the instructions to create an [ExpressRoute circuit](howto-circuit-cli.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="2be03-213">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil soukromý partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="2be03-213">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="2be03-214">V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="2be03-214">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="2be03-215">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí následující kroky.</span><span class="sxs-lookup"><span data-stu-id="2be03-215">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>

3. <span data-ttu-id="2be03-216">Zkontrolujte okruh ExpressRoute a ujistěte se, že je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="2be03-216">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="2be03-217">Použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-217">Use the following example:</span></span>

  ```azurecli
  az network express-route list
  ```

  <span data-ttu-id="2be03-218">Odpověď je stejný jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2be03-218">The response is similar to the following example:</span></span>

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

4. <span data-ttu-id="2be03-219">Nakonfigurujte partnerský vztah Microsoftu pro okruh.</span><span class="sxs-lookup"><span data-stu-id="2be03-219">Configure Microsoft peering for the circuit.</span></span> <span data-ttu-id="2be03-220">Před pokračováním se ujistěte, že máte k dispozici následující informace.</span><span class="sxs-lookup"><span data-stu-id="2be03-220">Make sure that you have the following information before you proceed.</span></span>

  * <span data-ttu-id="2be03-221">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-221">A /30 subnet for the primary link.</span></span> <span data-ttu-id="2be03-222">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="2be03-222">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="2be03-223">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="2be03-223">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="2be03-224">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="2be03-224">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="2be03-225">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="2be03-225">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="2be03-226">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="2be03-226">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="2be03-227">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="2be03-227">AS number for peering.</span></span> <span data-ttu-id="2be03-228">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="2be03-228">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="2be03-229">Inzerované předpony: Musíte poskytnout seznam všech předpon, které plánujete inzerovat přes relaci protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="2be03-229">Advertised prefixes: You must provide a list of all prefixes you plan to advertise over the BGP session.</span></span> <span data-ttu-id="2be03-230">Přijímají se jenom předpony veřejných IP adres.</span><span class="sxs-lookup"><span data-stu-id="2be03-230">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="2be03-231">Pokud chcete odeslat sadu předpon, můžete odeslat seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="2be03-231">If you plan to send a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="2be03-232">Tyto předpony musí být v RIR/IRR zaregistrované na vás.</span><span class="sxs-lookup"><span data-stu-id="2be03-232">These prefixes must be registered to you in an RIR / IRR.</span></span>
  * <span data-ttu-id="2be03-233">**Volitelné -** zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované na číslo AS partnerského vztahu, můžete zadat číslo AS, do kterého jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="2be03-233">**Optional -** Customer ASN: If you are advertising prefixes that are not registered to the peering AS number, you can specify the AS number to which they are registered.</span></span>
  * <span data-ttu-id="2be03-234">Název registru směrování: Můžete zadat RIR/IRR, kde jsou předpony a číslo AS registrované.</span><span class="sxs-lookup"><span data-stu-id="2be03-234">Routing Registry Name: You can specify the RIR / IRR against which the AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="2be03-235">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="2be03-235">**Optional -** An MD5 hash if you choose to use one.</span></span>

   <span data-ttu-id="2be03-236">Spusťte podle následujícího příkladu lze nakonfigurovat partnerský vztah Microsoftu pro váš okruh:</span><span class="sxs-lookup"><span data-stu-id="2be03-236">Run the following example to configure Microsoft peering for your circuit:</span></span>

  ```azurecli
  az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
  ```

### <a name="to-get-microsoft-peering-details"></a><span data-ttu-id="2be03-237">Získání podrobností partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="2be03-237">To get Microsoft peering details</span></span>

<span data-ttu-id="2be03-238">Můžete získat podrobnosti o konfiguraci pomocí následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-238">You can get configuration details by using the following example:</span></span>

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```

<span data-ttu-id="2be03-239">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="2be03-239">The output is similar to the following example:</span></span>

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

### <a name="to-update-microsoft-peering-configuration"></a><span data-ttu-id="2be03-240">Aktualizace konfigurace partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="2be03-240">To update Microsoft peering configuration</span></span>

<span data-ttu-id="2be03-241">Libovolnou část konfigurace můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="2be03-241">You can update any part of the configuration.</span></span> <span data-ttu-id="2be03-242">Inzerované předpony okruhu jsou aktualizováno z hodnoty 123.1.0.0/24 na 124.1.0.0/24 v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2be03-242">The advertised prefixes of the circuit are being updated from 123.1.0.0/24 to 124.1.0.0/24 in the following example:</span></span>

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="to-delete-microsoft-peering"></a><span data-ttu-id="2be03-243">Odstranění partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="2be03-243">To delete Microsoft peering</span></span>

<span data-ttu-id="2be03-244">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující příklad:</span><span class="sxs-lookup"><span data-stu-id="2be03-244">You can remove your peering configuration by running the following example:</span></span>

```azurecli
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="next-steps"></a><span data-ttu-id="2be03-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2be03-245">Next steps</span></span>

<span data-ttu-id="2be03-246">Dalším krokem je [Propojení virtuální sítě s okruhem ExpressRoute](howto-linkvnet-cli.md).</span><span class="sxs-lookup"><span data-stu-id="2be03-246">Next step, [Link a VNet to an ExpressRoute circuit](howto-linkvnet-cli.md).</span></span>

* <span data-ttu-id="2be03-247">Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="2be03-247">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="2be03-248">Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="2be03-248">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="2be03-249">Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2be03-249">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>