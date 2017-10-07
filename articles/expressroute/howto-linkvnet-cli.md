---
title: "Propojení virtuální sítě tooan okruh ExpressRoute: rozhraní příkazového řádku: Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak toolink virtuální sítě (virtuální sítě) tooExpressRoute okruhy pomocí modelu nasazení Resource Manager hello a rozhraní příkazového řádku."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlit
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: 1251f016d9b94d3fee81de1df164cb085cbe9d78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-cli"></a><span data-ttu-id="9423a-103">Připojit okruh ExpressRoute tooan virtuální sítě pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9423a-103">Connect a virtual network tooan ExpressRoute circuit using CLI</span></span>

<span data-ttu-id="9423a-104">Tento článek pomáhá odkaz okruhy ExpressRoute tooAzure virtuální sítě (virtuální sítě) pomocí rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="9423a-104">This article helps you link virtual networks (VNets) tooAzure ExpressRoute circuits using CLI.</span></span> <span data-ttu-id="9423a-105">použití Azure CLI, toolink hello virtuální sítě musí být vytvořen pomocí modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="9423a-105">toolink using Azure CLI, hello virtual networks must be created using hello Resource Manager deployment model.</span></span> <span data-ttu-id="9423a-106">Může být buď v hello stejného předplatného, nebo její část jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="9423a-106">They can either be in hello same subscription, or part of another subscription.</span></span> <span data-ttu-id="9423a-107">Pokud chcete toouse jinou metodu tooconnect vaší virtuální sítě tooan okruh ExpressRoute, můžete vybrat článek ze hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="9423a-107">If you want toouse a different method tooconnect your VNet tooan ExpressRoute circuit, you can select an article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9423a-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9423a-108">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="9423a-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9423a-109">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="9423a-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9423a-110">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="9423a-111">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="9423a-111">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="9423a-112">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="9423a-112">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="9423a-113">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="9423a-113">Configuration prerequisites</span></span>

* <span data-ttu-id="9423a-114">Je nutné hello nejnovější verzi hello rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="9423a-114">You need hello latest version of hello command-line interface (CLI).</span></span> <span data-ttu-id="9423a-115">Další informace najdete v tématu [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9423a-115">For more information, see [Install Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="9423a-116">Je třeba tooreview hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9423a-116">You need tooreview hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="9423a-117">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-117">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="9423a-118">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](howto-circuit-cli.md) a mít hello ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="9423a-118">Follow hello instructions too[create an ExpressRoute circuit](howto-circuit-cli.md) and have hello circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="9423a-119">Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="9423a-119">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="9423a-120">V tématu hello [konfigurace směrování](howto-routing-cli.md) směrování pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="9423a-120">See hello [configure routing](howto-routing-cli.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="9423a-121">Zkontrolujte, jestli je nakonfigurovaný soukromého partnerského vztahu Azure.</span><span class="sxs-lookup"><span data-stu-id="9423a-121">Ensure that Azure private peering is configured.</span></span> <span data-ttu-id="9423a-122">Hello partnerského vztahu protokolu BGP mezi vaší sítí a Microsoftem musí být si tak, že povolíte připojení klient server.</span><span class="sxs-lookup"><span data-stu-id="9423a-122">hello BGP peering between your network and Microsoft must be up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="9423a-123">Ujistěte se, že máte virtuální sítě a brány virtuální sítě vytvoří a plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="9423a-123">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="9423a-124">Postupujte podle pokynů hello příliš[nakonfigurovat bránu virtuální sítě pro ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span><span class="sxs-lookup"><span data-stu-id="9423a-124">Follow hello instructions too[Configure a virtual network gateway for ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span></span> <span data-ttu-id="9423a-125">Být jisti toouse `--gateway-type ExpressRoute`.</span><span class="sxs-lookup"><span data-stu-id="9423a-125">Be sure toouse `--gateway-type ExpressRoute`.</span></span>

* <span data-ttu-id="9423a-126">Můžete propojit až too10 virtuální sítě tooa standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-126">You can link up too10 virtual networks tooa standard ExpressRoute circuit.</span></span> <span data-ttu-id="9423a-127">Všechny virtuální sítě musí být v hello stejné geopolitické oblasti při použití standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-127">All virtual networks must be in hello same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="9423a-128">Pokud povolíte hello doplněk ExpressRoute premium, můžete propojení virtuální sítě mimo hello geopolitické oblasti hello okruh ExpressRoute nebo připojit větší počet virtuálních sítí tooyour okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-128">If you enable hello ExpressRoute premium add-on, you can link a virtual network outside of hello geopolitical region of hello ExpressRoute circuit, or connect a larger number of virtual networks tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="9423a-129">Další informace o rozšíření hello premium najdete v tématu hello [– nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="9423a-129">For more information about hello premium add-on, see hello [FAQ](expressroute-faqs.md).</span></span>

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="9423a-130">Připojit virtuální síť v hello stejnému okruhu tooa předplatného</span><span class="sxs-lookup"><span data-stu-id="9423a-130">Connect a virtual network in hello same subscription tooa circuit</span></span>

<span data-ttu-id="9423a-131">Okruh ExpressRoute tooan brány virtuální sítě můžete připojit pomocí příklad hello.</span><span class="sxs-lookup"><span data-stu-id="9423a-131">You can connect a virtual network gateway tooan ExpressRoute circuit by using hello example.</span></span> <span data-ttu-id="9423a-132">Ujistěte se, že hello brány virtuální sítě se vytvoří a je připravený k propojení před spuštěním příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="9423a-132">Make sure that hello virtual network gateway is created and is ready for linking before you run hello command.</span></span>

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="9423a-133">Připojit virtuální síť v okruhu tooa jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="9423a-133">Connect a virtual network in a different subscription tooa circuit</span></span>

<span data-ttu-id="9423a-134">Okruh ExpressRoute můžete sdílet mezi více předplatných.</span><span class="sxs-lookup"><span data-stu-id="9423a-134">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="9423a-135">Hello na následujícím obrázku jednoduchou schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.</span><span class="sxs-lookup"><span data-stu-id="9423a-135">hello figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="9423a-136">Každý hello menší cloudy v rámci cloudu velké hello je použité toorepresent odběry, které patří toodifferent oddělení v organizaci.</span><span class="sxs-lookup"><span data-stu-id="9423a-136">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span> <span data-ttu-id="9423a-137">Každé oddělení hello v rámci organizace hello můžete použít vlastní předplatné pro nasazení můžete sdílet své služby--ale jedné back tooyour místní sítě tooconnect okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-137">Each of hello departments within hello organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span> <span data-ttu-id="9423a-138">Jednoho oddělení (v tomto příkladu: IT) můžete vlastní hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-138">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="9423a-139">Hello okruh ExpressRoute můžete použít jiné odběry v rámci organizace hello.</span><span class="sxs-lookup"><span data-stu-id="9423a-139">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="9423a-140">Připojení a šířku pásma poplatky za hello vyhrazené okruhu bude použité toohello vlastníka okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-140">Connectivity and bandwidth charges for hello dedicated circuit will be applied toohello ExpressRoute Circuit Owner.</span></span> <span data-ttu-id="9423a-141">Všechny virtuální sítě sdílejí hello stejné šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="9423a-141">All virtual networks share hello same bandwidth.</span></span>
> 
> 

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="9423a-143">Správa - vlastníky okruhu a okruh uživatelů</span><span class="sxs-lookup"><span data-stu-id="9423a-143">Administration - Circuit Owners and Circuit Users</span></span>

<span data-ttu-id="9423a-144">Hello 'vlastníka okruhu, je oprávněný uživatel napájení z hello prostředků okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-144">hello 'Circuit Owner' is an authorized Power User of hello ExpressRoute circuit resource.</span></span> <span data-ttu-id="9423a-145">Hello vlastníka okruhu můžete vytvořit autorizací, které lze uplatnit podle 'Okruh uživatele'.</span><span class="sxs-lookup"><span data-stu-id="9423a-145">hello Circuit Owner can create authorizations that can be redeemed by 'Circuit Users'.</span></span> <span data-ttu-id="9423a-146">Vlastníci virtuální sítě jsou uživatelé okruh brány, které nejsou uvnitř hello stejného předplatného jako hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-146">Circuit Users are owners of virtual network gateways that are not within hello same subscription as hello ExpressRoute circuit.</span></span> <span data-ttu-id="9423a-147">Uživatelé okruhu můžete uplatnit autorizací (jedním autorizačním na jednu virtuální síť).</span><span class="sxs-lookup"><span data-stu-id="9423a-147">Circuit Users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="9423a-148">Hello vlastníka okruhu má hello power toomodify a odvolat oprávnění kdykoli.</span><span class="sxs-lookup"><span data-stu-id="9423a-148">hello Circuit Owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="9423a-149">Při povolení odvolán, všechna připojení odkaz se odstraní z předplatného hello jehož přístup byl odvolán.</span><span class="sxs-lookup"><span data-stu-id="9423a-149">When an authorization is revoked, all link connections are deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="9423a-150">Operace vlastníka okruhu</span><span class="sxs-lookup"><span data-stu-id="9423a-150">Circuit Owner operations</span></span>

<span data-ttu-id="9423a-151">**toocreate povolení**</span><span class="sxs-lookup"><span data-stu-id="9423a-151">**toocreate an authorization**</span></span>

<span data-ttu-id="9423a-152">Hello vlastníka okruhu vytvoří autorizaci, které vytvoří autorizační klíč, který může být používán tooconnect uživatele okruhu jejich toohello brány virtuální sítě okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9423a-152">hello Circuit Owner creates an authorization, which creates an authorization key that can be used by a Circuit User tooconnect their virtual network gateways toohello ExpressRoute circuit.</span></span> <span data-ttu-id="9423a-153">Povolení je platný pro jenom jedno připojení.</span><span class="sxs-lookup"><span data-stu-id="9423a-153">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="9423a-154">Následující příklad ukazuje, jak Hello toocreate povolení:</span><span class="sxs-lookup"><span data-stu-id="9423a-154">hello following example shows how toocreate an authorization:</span></span>

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

<span data-ttu-id="9423a-155">Hello odpovědi obsahuje hello autorizační klíč a stav:</span><span class="sxs-lookup"><span data-stu-id="9423a-155">hello response contains hello authorization key and status:</span></span>

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

<span data-ttu-id="9423a-156">**povolení tooreview**</span><span class="sxs-lookup"><span data-stu-id="9423a-156">**tooreview authorizations**</span></span>

<span data-ttu-id="9423a-157">Hello vlastníka okruhu můžete zkontrolovat všechny autorizací, které jsou vydány na konkrétní okruhu spuštěním hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9423a-157">hello Circuit Owner can review all authorizations that are issued on a particular circuit by running hello following example:</span></span>

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

<span data-ttu-id="9423a-158">**povolení tooadd**</span><span class="sxs-lookup"><span data-stu-id="9423a-158">**tooadd authorizations**</span></span>

<span data-ttu-id="9423a-159">Hello vlastníka okruhu můžete přidat oprávnění pomocí hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9423a-159">hello Circuit Owner can add authorizations by using hello following example:</span></span>

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

<span data-ttu-id="9423a-160">**povolení toodelete**</span><span class="sxs-lookup"><span data-stu-id="9423a-160">**toodelete authorizations**</span></span>

<span data-ttu-id="9423a-161">Hello vlastníka okruhu můžete odvolání nebo odstranění autorizací toohello uživatele spuštěním hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9423a-161">hello Circuit Owner can revoke/delete authorizations toohello user by running hello following example:</span></span>

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a><span data-ttu-id="9423a-162">Operace okruh uživatele</span><span class="sxs-lookup"><span data-stu-id="9423a-162">Circuit User operations</span></span>

<span data-ttu-id="9423a-163">Hello uživatele okruhu musí hello ID partnera a autorizační klíč z hello vlastníka okruhu.</span><span class="sxs-lookup"><span data-stu-id="9423a-163">hello Circuit User needs hello peer ID and an authorization key from hello Circuit Owner.</span></span> <span data-ttu-id="9423a-164">Hello autorizační klíč je identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="9423a-164">hello authorization key is a GUID.</span></span>

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="9423a-165">**tooredeem autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="9423a-165">**tooredeem a connection authorization**</span></span>

<span data-ttu-id="9423a-166">Hello uživatele okruhu můžete spustit následující příklad tooredeem hello odkaz autorizace:</span><span class="sxs-lookup"><span data-stu-id="9423a-166">hello Circuit User can run hello following example tooredeem a link authorization:</span></span>

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="9423a-167">**toorelease autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="9423a-167">**toorelease a connection authorization**</span></span>

<span data-ttu-id="9423a-168">Povolení můžete vydat odstraněním hello připojení, který odkazuje hello ExpressRoute okruhu toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9423a-168">You can release an authorization by deleting hello connection that links hello ExpressRoute circuit toohello virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9423a-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9423a-169">Next steps</span></span>

<span data-ttu-id="9423a-170">Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="9423a-170">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
