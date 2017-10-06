---
title: "Propojení virtuální sítě tooan okruh ExpressRoute: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak toolink virtuální sítě (virtuální sítě) tooExpressRoute okruhy pomocí modelu nasazení Resource Manager hello a prostředí PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: daacb6e5-705a-456f-9a03-c4fc3f8c1f7e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: ganesr
ms.openlocfilehash: e75a9f6b42fa8e1a579e4f19882ec99b277b545f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="08eec-103">Připojit virtuální síť tooan okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="08eec-103">Connect a virtual network tooan ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="08eec-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="08eec-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="08eec-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="08eec-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="08eec-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="08eec-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="08eec-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="08eec-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="08eec-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="08eec-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="08eec-109">Tento článek pomáhá propojení virtuální sítě (virtuální sítě) okruhy ExpressRoute tooAzure pomocí modelu nasazení Resource Manager hello a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08eec-109">This article helps you link virtual networks (VNets) tooAzure ExpressRoute circuits by using hello Resource Manager deployment model and PowerShell.</span></span> <span data-ttu-id="08eec-110">Virtuální sítě může být buď v hello stejné předplatné nebo součástí jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="08eec-110">Virtual networks can either be in hello same subscription or part of another subscription.</span></span> <span data-ttu-id="08eec-111">Tento článek také ukazuje, jak propojit tooupdate virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="08eec-111">This article also shows you how tooupdate a virtual network link.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="08eec-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="08eec-112">Before you begin</span></span>
* <span data-ttu-id="08eec-113">Nainstalujte nejnovější verzi modulů prostředí Azure PowerShell hello hello.</span><span class="sxs-lookup"><span data-stu-id="08eec-113">Install hello latest version of hello Azure PowerShell modules.</span></span> <span data-ttu-id="08eec-114">Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="08eec-114">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="08eec-115">Zkontrolujte hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="08eec-115">Review hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="08eec-116">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-116">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="08eec-117">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mít hello ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="08eec-117">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="08eec-118">Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="08eec-118">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="08eec-119">V tématu hello [konfigurace směrování](expressroute-howto-routing-arm.md) směrování pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="08eec-119">See hello [configure routing](expressroute-howto-routing-arm.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="08eec-120">Zajistěte, aby soukromý partnerský vztah Azure je nakonfigurován a hello partnerského vztahu protokolu BGP mezi vaší sítí a Microsoftem zapnutý tak, že povolíte připojení klient server.</span><span class="sxs-lookup"><span data-stu-id="08eec-120">Ensure that Azure private peering is configured and hello BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="08eec-121">Ujistěte se, že máte virtuální sítě a brány virtuální sítě vytvoří a plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="08eec-121">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="08eec-122">Postupujte podle pokynů hello příliš[vytvořit bránu virtuální sítě pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="08eec-122">Follow hello instructions too[create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="08eec-123">Brána virtuální sítě pro ExpressRoute používá hello GatewayType 'ExpressRoute' není síť VPN.</span><span class="sxs-lookup"><span data-stu-id="08eec-123">A virtual network gateway for ExpressRoute uses hello GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="08eec-124">Můžete propojit až too10 virtuální sítě tooa standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-124">You can link up too10 virtual networks tooa standard ExpressRoute circuit.</span></span> <span data-ttu-id="08eec-125">Všechny virtuální sítě musí být v hello stejné geopolitické oblasti při použití standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-125">All virtual networks must be in hello same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="08eec-126">Můžete propojit virtuální sítě mimo hello geopolitické oblasti hello okruh ExpressRoute nebo připojit větší počet virtuálních sítí tooyour okruh ExpressRoute, pokud jste povolili hello doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="08eec-126">You can link a virtual networks outside of hello geopolitical region of hello ExpressRoute circuit, or connect a larger number of virtual networks tooyour ExpressRoute circuit if you enabled hello ExpressRoute premium add-on.</span></span> <span data-ttu-id="08eec-127">Zkontrolujte hello [– nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium hello.</span><span class="sxs-lookup"><span data-stu-id="08eec-127">Check hello [FAQ](expressroute-faqs.md) for more details on hello premium add-on.</span></span>


## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="08eec-128">Připojit virtuální síť v hello stejnému okruhu tooa předplatného</span><span class="sxs-lookup"><span data-stu-id="08eec-128">Connect a virtual network in hello same subscription tooa circuit</span></span>
<span data-ttu-id="08eec-129">Pomocí následující rutiny hello můžete připojit virtuální síti brány tooan okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-129">You can connect a virtual network gateway tooan ExpressRoute circuit by using hello following cmdlet.</span></span> <span data-ttu-id="08eec-130">Ujistěte se, že hello brány virtuální sítě se vytvoří a je připravený k propojení před spuštěním rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="08eec-130">Make sure that hello virtual network gateway is created and is ready for linking before you run hello cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="08eec-131">Připojit virtuální síť v okruhu tooa jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="08eec-131">Connect a virtual network in a different subscription tooa circuit</span></span>
<span data-ttu-id="08eec-132">Okruh ExpressRoute můžete sdílet mezi více předplatných.</span><span class="sxs-lookup"><span data-stu-id="08eec-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="08eec-133">Hello následující obrázek znázorňuje jednoduchý schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.</span><span class="sxs-lookup"><span data-stu-id="08eec-133">hello following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="08eec-134">Každý hello menší cloudy v rámci cloudu velké hello je použité toorepresent odběry, které patří toodifferent oddělení v organizaci.</span><span class="sxs-lookup"><span data-stu-id="08eec-134">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span> <span data-ttu-id="08eec-135">Každé oddělení hello v rámci organizace hello můžete použít vlastní předplatné pro nasazení můžete sdílet své služby--ale jedné back tooyour místní sítě tooconnect okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-135">Each of hello departments within hello organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span> <span data-ttu-id="08eec-136">Jednoho oddělení (v tomto příkladu: IT) můžete vlastní hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-136">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="08eec-137">Hello okruh ExpressRoute můžete použít jiné odběry v rámci organizace hello.</span><span class="sxs-lookup"><span data-stu-id="08eec-137">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="08eec-138">Připojení a šířku pásma poplatky za hello okruh ExpressRoute bude použité toohello vlastník předplatného.</span><span class="sxs-lookup"><span data-stu-id="08eec-138">Connectivity and bandwidth charges for hello ExpressRoute circuit will be applied toohello subscription owner.</span></span> <span data-ttu-id="08eec-139">Všechny virtuální sítě sdílejí hello stejné šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="08eec-139">All virtual networks share hello same bandwidth.</span></span>
> 
> 

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="08eec-141">Správa - vlastníky okruhu a okruh uživatelů</span><span class="sxs-lookup"><span data-stu-id="08eec-141">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="08eec-142">Hello 'vlastníka okruhu, je oprávněný uživatel napájení z hello prostředků okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-142">hello 'circuit owner' is an authorized Power User of hello ExpressRoute circuit resource.</span></span> <span data-ttu-id="08eec-143">vlastníka okruhu Hello můžete vytvořit autorizací, které lze uplatnit podle 'okruh uživatele'.</span><span class="sxs-lookup"><span data-stu-id="08eec-143">hello circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="08eec-144">Vlastníci virtuální sítě jsou uživatelé okruh brány, které nejsou uvnitř hello stejného předplatného jako hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-144">Circuit users are owners of virtual network gateways that are not within hello same subscription as hello ExpressRoute circuit.</span></span> <span data-ttu-id="08eec-145">Uživatelé okruhu můžete uplatnit autorizací (jedním autorizačním na jednu virtuální síť).</span><span class="sxs-lookup"><span data-stu-id="08eec-145">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="08eec-146">vlastníka okruhu Hello má hello power toomodify a odvolat oprávnění kdykoli.</span><span class="sxs-lookup"><span data-stu-id="08eec-146">hello circuit owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="08eec-147">Odvolání autorizaci má za následek všechna připojení odkaz odstraňuje z předplatného hello jehož přístup byl odvolán.</span><span class="sxs-lookup"><span data-stu-id="08eec-147">Revoking an authorization results in all link connections being deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="08eec-148">Operace vlastníka okruhu</span><span class="sxs-lookup"><span data-stu-id="08eec-148">Circuit owner operations</span></span>

<span data-ttu-id="08eec-149">**toocreate povolení**</span><span class="sxs-lookup"><span data-stu-id="08eec-149">**toocreate an authorization**</span></span>

<span data-ttu-id="08eec-150">vlastníka okruhu Hello vytvoří povolení.</span><span class="sxs-lookup"><span data-stu-id="08eec-150">hello circuit owner creates an authorization.</span></span> <span data-ttu-id="08eec-151">To vede k vytváření hello autorizační klíč, který může být používán tooconnect uživatele okruhu jejich toohello brány virtuální sítě okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-151">This results in hello creation of an authorization key that can be used by a circuit user tooconnect their virtual network gateways toohello ExpressRoute circuit.</span></span> <span data-ttu-id="08eec-152">Povolení je platný pro jenom jedno připojení.</span><span class="sxs-lookup"><span data-stu-id="08eec-152">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="08eec-153">následující rutina fragment kódu ukazuje, jak Hello toocreate povolení:</span><span class="sxs-lookup"><span data-stu-id="08eec-153">hello following cmdlet snippet shows how toocreate an authorization:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


<span data-ttu-id="08eec-154">Hello odpovědi toothis bude obsahovat autorizační klíč hello a stav:</span><span class="sxs-lookup"><span data-stu-id="08eec-154">hello response toothis will contain hello authorization key and status:</span></span>

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



<span data-ttu-id="08eec-155">**povolení tooreview**</span><span class="sxs-lookup"><span data-stu-id="08eec-155">**tooreview authorizations**</span></span>

<span data-ttu-id="08eec-156">vlastníka okruhu Hello můžete zkontrolovat všechny autorizací, které jsou vydány na konkrétní okruhu spuštěním následující rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="08eec-156">hello circuit owner can review all authorizations that are issued on a particular circuit by running hello following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="08eec-157">**povolení tooadd**</span><span class="sxs-lookup"><span data-stu-id="08eec-157">**tooadd authorizations**</span></span>

<span data-ttu-id="08eec-158">vlastníka okruhu Hello můžete přidat oprávnění pomocí hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="08eec-158">hello circuit owner can add authorizations by using hello following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="08eec-159">**povolení toodelete**</span><span class="sxs-lookup"><span data-stu-id="08eec-159">**toodelete authorizations**</span></span>

<span data-ttu-id="08eec-160">vlastníka okruhu Hello můžete odvolání nebo odstranění autorizací toohello uživatele spuštěním následující rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="08eec-160">hello circuit owner can revoke/delete authorizations toohello user by running hello following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a><span data-ttu-id="08eec-161">Operace okruh uživatele</span><span class="sxs-lookup"><span data-stu-id="08eec-161">Circuit user operations</span></span>

<span data-ttu-id="08eec-162">Hello okruh uživatel potřebuje hello ID partnera a autorizační klíč z vlastníka okruhu hello.</span><span class="sxs-lookup"><span data-stu-id="08eec-162">hello circuit user needs hello peer ID and an authorization key from hello circuit owner.</span></span> <span data-ttu-id="08eec-163">Hello autorizační klíč je identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="08eec-163">hello authorization key is a GUID.</span></span>

<span data-ttu-id="08eec-164">ID partnera můžete zkontrolovat z hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="08eec-164">Peer ID can be checked from hello following command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="08eec-165">**tooredeem autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="08eec-165">**tooredeem a connection authorization**</span></span>

<span data-ttu-id="08eec-166">uživatele okruhu Hello můžete spustit následující rutiny tooredeem hello odkaz autorizace:</span><span class="sxs-lookup"><span data-stu-id="08eec-166">hello circuit user can run hello following cmdlet tooredeem a link authorization:</span></span>

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="08eec-167">**toorelease autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="08eec-167">**toorelease a connection authorization**</span></span>

<span data-ttu-id="08eec-168">Povolení můžete vydat odstraněním hello připojení, který odkazuje hello ExpressRoute okruhu toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="08eec-168">You can release an authorization by deleting hello connection that links hello ExpressRoute circuit toohello virtual network.</span></span>

## <a name="modify-a-virtual-network-connection"></a><span data-ttu-id="08eec-169">Upravit připojení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="08eec-169">Modify a virtual network connection</span></span>
<span data-ttu-id="08eec-170">Můžete aktualizovat některé vlastnosti připojení k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="08eec-170">You can update certain properties of a virtual network connection.</span></span> 

<span data-ttu-id="08eec-171">**Váha připojení tooupdate hello**</span><span class="sxs-lookup"><span data-stu-id="08eec-171">**tooupdate hello connection weight**</span></span>

<span data-ttu-id="08eec-172">Virtuální sítě může být připojené toomultiple okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-172">Your virtual network can be connected toomultiple ExpressRoute circuits.</span></span> <span data-ttu-id="08eec-173">Můžete obdržet hello stejné předpony z více než jeden okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="08eec-173">You may receive hello same prefix from more than one ExpressRoute circuit.</span></span> <span data-ttu-id="08eec-174">Jaký připojení toosend provoz určený pro tuto předponu toochoose, můžete změnit *RoutingWeight* připojení.</span><span class="sxs-lookup"><span data-stu-id="08eec-174">toochoose which connection toosend traffic destined for this prefix, you can change *RoutingWeight* of a connection.</span></span> <span data-ttu-id="08eec-175">Přenosy se odesílají na hello připojení s nejvyšší hello *RoutingWeight*.</span><span class="sxs-lookup"><span data-stu-id="08eec-175">Traffic will be sent on hello connection with hello highest *RoutingWeight*.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

<span data-ttu-id="08eec-176">Hello rozsah *RoutingWeight* je 0 too32000.</span><span class="sxs-lookup"><span data-stu-id="08eec-176">hello range of *RoutingWeight* is 0 too32000.</span></span> <span data-ttu-id="08eec-177">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="08eec-177">hello default value is 0.</span></span>

## <a name="next-steps"></a><span data-ttu-id="08eec-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08eec-178">Next steps</span></span>
<span data-ttu-id="08eec-179">Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="08eec-179">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
