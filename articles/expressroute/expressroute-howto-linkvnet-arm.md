---
title: "Propojení virtuální sítě k okruhu ExpressRoute: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak propojit virtuální sítě (virtuální sítě) pro okruhy ExpressRoute pomocí modelu nasazení Resource Manager a prostředí PowerShell."
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
ms.openlocfilehash: 8c2f3036f754a98090ab860f95900416690ebf83
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="4516c-103">Připojit virtuální sítě k okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="4516c-103">Connect a virtual network to an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4516c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4516c-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="4516c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4516c-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="4516c-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4516c-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="4516c-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="4516c-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="4516c-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="4516c-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="4516c-109">Tento článek pomáhá propojení virtuálních sítí (virtuální sítě) pro okruhy Azure ExpressRoute pomocí modelu nasazení Resource Manager a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4516c-109">This article helps you link virtual networks (VNets) to Azure ExpressRoute circuits by using the Resource Manager deployment model and PowerShell.</span></span> <span data-ttu-id="4516c-110">Virtuální sítě může být buď v rámci stejného předplatného nebo součástí jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="4516c-110">Virtual networks can either be in the same subscription or part of another subscription.</span></span> <span data-ttu-id="4516c-111">Tento článek také ukazuje, jak aktualizovat propojení virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4516c-111">This article also shows you how to update a virtual network link.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="4516c-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4516c-112">Before you begin</span></span>
* <span data-ttu-id="4516c-113">Nainstalujte nejnovější verzi modulů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4516c-113">Install the latest version of the Azure PowerShell modules.</span></span> <span data-ttu-id="4516c-114">Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4516c-114">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="4516c-115">Zkontrolujte [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4516c-115">Review the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="4516c-116">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4516c-116">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="4516c-117">Postupujte podle pokynů a [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="4516c-117">Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="4516c-118">Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="4516c-118">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="4516c-119">Najdete v článku [konfigurace směrování](expressroute-howto-routing-arm.md) směrování pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="4516c-119">See the [configure routing](expressroute-howto-routing-arm.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="4516c-120">Zajistěte, aby soukromý partnerský vztah Azure je nakonfigurován a partnerském vztahu protokolu BGP mezi vaší sítí a Microsoftem zapnutý tak, že povolíte připojení klient server.</span><span class="sxs-lookup"><span data-stu-id="4516c-120">Ensure that Azure private peering is configured and the BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="4516c-121">Ujistěte se, že máte virtuální sítě a brány virtuální sítě vytvoří a plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="4516c-121">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="4516c-122">Postupujte podle pokynů a [vytvořit bránu virtuální sítě pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4516c-122">Follow the instructions to [create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="4516c-123">Brána virtuální sítě pro ExpressRoute používá GatewayType 'ExpressRoute' není síť VPN.</span><span class="sxs-lookup"><span data-stu-id="4516c-123">A virtual network gateway for ExpressRoute uses the GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="4516c-124">Až 10 virtuálních sítí můžete propojit standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4516c-124">You can link up to 10 virtual networks to a standard ExpressRoute circuit.</span></span> <span data-ttu-id="4516c-125">Všechny virtuální sítě musí být ve stejné geopolitické oblasti, při použití standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4516c-125">All virtual networks must be in the same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="4516c-126">Můžete propojit virtuální sítě mimo geopolitické oblasti okruhu ExpressRoute nebo větší počet virtuálních sítí připojit k okruhu ExpressRoute, pokud jste povolili doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="4516c-126">You can link a virtual networks outside of the geopolitical region of the ExpressRoute circuit, or connect a larger number of virtual networks to your ExpressRoute circuit if you enabled the ExpressRoute premium add-on.</span></span> <span data-ttu-id="4516c-127">Zkontrolujte [– nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium.</span><span class="sxs-lookup"><span data-stu-id="4516c-127">Check the [FAQ](expressroute-faqs.md) for more details on the premium add-on.</span></span>


## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a><span data-ttu-id="4516c-128">Připojit k okruhu virtuální sítě v rámci stejného předplatného</span><span class="sxs-lookup"><span data-stu-id="4516c-128">Connect a virtual network in the same subscription to a circuit</span></span>
<span data-ttu-id="4516c-129">Pomocí následující rutiny můžete připojit bránu virtuální sítě k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4516c-129">You can connect a virtual network gateway to an ExpressRoute circuit by using the following cmdlet.</span></span> <span data-ttu-id="4516c-130">Ujistěte se, že bránu virtuální sítě se vytvoří a je připravený pro propojování předtím, než spustíte rutinu:</span><span class="sxs-lookup"><span data-stu-id="4516c-130">Make sure that the virtual network gateway is created and is ready for linking before you run the cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a><span data-ttu-id="4516c-131">Připojení virtuální sítě jiného předplatného k okruhu</span><span class="sxs-lookup"><span data-stu-id="4516c-131">Connect a virtual network in a different subscription to a circuit</span></span>
<span data-ttu-id="4516c-132">Okruh ExpressRoute můžete sdílet mezi více předplatných.</span><span class="sxs-lookup"><span data-stu-id="4516c-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="4516c-133">Následující obrázek znázorňuje jednoduchý schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.</span><span class="sxs-lookup"><span data-stu-id="4516c-133">The following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="4516c-134">Každý z menší cloudy v rámci velké cloudu se používá k reprezentování odběry, které patří k různým oblastem v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="4516c-134">Each of the smaller clouds within the large cloud is used to represent subscriptions that belong to different departments within an organization.</span></span> <span data-ttu-id="4516c-135">Každé oddělení v organizaci můžete použít vlastní předplatné nasazování jejich služeb – ale můžete sdílet jeden okruh ExpressRoute zpětné připojení k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="4516c-135">Each of the departments within the organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit to connect back to your on-premises network.</span></span> <span data-ttu-id="4516c-136">Jednoho oddělení (v tomto příkladu: IT) můžete vlastní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4516c-136">A single department (in this example: IT) can own the ExpressRoute circuit.</span></span> <span data-ttu-id="4516c-137">Okruh ExpressRoute můžete použít jiné odběry v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="4516c-137">Other subscriptions within the organization can use the ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="4516c-138">Připojení a šířku pásma poplatky za okruh ExpressRoute se použijí k vlastník předplatného.</span><span class="sxs-lookup"><span data-stu-id="4516c-138">Connectivity and bandwidth charges for the ExpressRoute circuit will be applied to the subscription owner.</span></span> <span data-ttu-id="4516c-139">Všechny virtuální sítě sdílejí stejné šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="4516c-139">All virtual networks share the same bandwidth.</span></span>
> 
> 

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="4516c-141">Správa - vlastníky okruhu a okruh uživatelů</span><span class="sxs-lookup"><span data-stu-id="4516c-141">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="4516c-142">Vlastníka okruhu je oprávněný uživatel Power prostředku okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4516c-142">The 'circuit owner' is an authorized Power User of the ExpressRoute circuit resource.</span></span> <span data-ttu-id="4516c-143">Vlastníka okruhu můžete vytvořit autorizací, které lze uplatnit podle 'okruh uživatele'.</span><span class="sxs-lookup"><span data-stu-id="4516c-143">The circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="4516c-144">Okruh uživatelů se vlastníci brány virtuální sítě, které nejsou v rámci stejného předplatného jako okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4516c-144">Circuit users are owners of virtual network gateways that are not within the same subscription as the ExpressRoute circuit.</span></span> <span data-ttu-id="4516c-145">Uživatelé okruhu můžete uplatnit autorizací (jedním autorizačním na jednu virtuální síť).</span><span class="sxs-lookup"><span data-stu-id="4516c-145">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="4516c-146">Vlastníka okruhu má právo upravit a kdykoli odvolat oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4516c-146">The circuit owner has the power to modify and revoke authorizations at any time.</span></span> <span data-ttu-id="4516c-147">Odvolání autorizaci má za následek všechna připojení odkaz odstraňuje z předplatného, jejichž přístup byl odvolán.</span><span class="sxs-lookup"><span data-stu-id="4516c-147">Revoking an authorization results in all link connections being deleted from the subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="4516c-148">Operace vlastníka okruhu</span><span class="sxs-lookup"><span data-stu-id="4516c-148">Circuit owner operations</span></span>

<span data-ttu-id="4516c-149">**Chcete-li vytvořit povolení**</span><span class="sxs-lookup"><span data-stu-id="4516c-149">**To create an authorization**</span></span>

<span data-ttu-id="4516c-150">Vlastníka okruhu vytvoří povolení.</span><span class="sxs-lookup"><span data-stu-id="4516c-150">The circuit owner creates an authorization.</span></span> <span data-ttu-id="4516c-151">Výsledkem je vytvoření autorizační klíč, které je možné se připojit své brány virtuální sítě k okruhu ExpressRoute uživatelem okruh.</span><span class="sxs-lookup"><span data-stu-id="4516c-151">This results in the creation of an authorization key that can be used by a circuit user to connect their virtual network gateways to the ExpressRoute circuit.</span></span> <span data-ttu-id="4516c-152">Povolení je platný pro jenom jedno připojení.</span><span class="sxs-lookup"><span data-stu-id="4516c-152">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="4516c-153">Následující rutiny fragment kódu ukazuje vytvoření povolení:</span><span class="sxs-lookup"><span data-stu-id="4516c-153">The following cmdlet snippet shows how to create an authorization:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


<span data-ttu-id="4516c-154">Odpověď na to bude obsahovat autorizační klíč a stav:</span><span class="sxs-lookup"><span data-stu-id="4516c-154">The response to this will contain the authorization key and status:</span></span>

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



<span data-ttu-id="4516c-155">**Chcete-li zkontrolovat autorizací**</span><span class="sxs-lookup"><span data-stu-id="4516c-155">**To review authorizations**</span></span>

<span data-ttu-id="4516c-156">Vlastníka okruhu můžete zkontrolovat všechny autorizací, které jsou vydány na konkrétní okruhu spuštěním následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="4516c-156">The circuit owner can review all authorizations that are issued on a particular circuit by running the following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="4516c-157">**Chcete-li přidat autorizací**</span><span class="sxs-lookup"><span data-stu-id="4516c-157">**To add authorizations**</span></span>

<span data-ttu-id="4516c-158">Vlastníka okruhu můžete přidat oprávnění pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="4516c-158">The circuit owner can add authorizations by using the following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="4516c-159">**K odstranění autorizací**</span><span class="sxs-lookup"><span data-stu-id="4516c-159">**To delete authorizations**</span></span>

<span data-ttu-id="4516c-160">Vlastníka okruhu můžete odvolání nebo odstranění autorizací pro uživatele tak, že spustíte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="4516c-160">The circuit owner can revoke/delete authorizations to the user by running the following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a><span data-ttu-id="4516c-161">Operace okruh uživatele</span><span class="sxs-lookup"><span data-stu-id="4516c-161">Circuit user operations</span></span>

<span data-ttu-id="4516c-162">ID partnera a autorizační klíč z vlastníka okruhu, musí uživatel okruh.</span><span class="sxs-lookup"><span data-stu-id="4516c-162">The circuit user needs the peer ID and an authorization key from the circuit owner.</span></span> <span data-ttu-id="4516c-163">Autorizační klíč je identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="4516c-163">The authorization key is a GUID.</span></span>

<span data-ttu-id="4516c-164">ID partnera můžete zkontrolovat z následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4516c-164">Peer ID can be checked from the following command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="4516c-165">**Pokud chcete uplatnit autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="4516c-165">**To redeem a connection authorization**</span></span>

<span data-ttu-id="4516c-166">Okruh může spouštět následující rutiny můžete uplatnit odkaz autorizace:</span><span class="sxs-lookup"><span data-stu-id="4516c-166">The circuit user can run the following cmdlet to redeem a link authorization:</span></span>

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="4516c-167">**Chcete-li uvolnit autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="4516c-167">**To release a connection authorization**</span></span>

<span data-ttu-id="4516c-168">Povolení můžete vydat odstraněním připojení, který odkazuje okruhu ExpressRoute do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4516c-168">You can release an authorization by deleting the connection that links the ExpressRoute circuit to the virtual network.</span></span>

## <a name="modify-a-virtual-network-connection"></a><span data-ttu-id="4516c-169">Upravit připojení virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="4516c-169">Modify a virtual network connection</span></span>
<span data-ttu-id="4516c-170">Můžete aktualizovat některé vlastnosti připojení k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="4516c-170">You can update certain properties of a virtual network connection.</span></span> 

<span data-ttu-id="4516c-171">**Chcete-li aktualizovat váhy připojení**</span><span class="sxs-lookup"><span data-stu-id="4516c-171">**To update the connection weight**</span></span>

<span data-ttu-id="4516c-172">Virtuální sítě může být připojen k více okruhů ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4516c-172">Your virtual network can be connected to multiple ExpressRoute circuits.</span></span> <span data-ttu-id="4516c-173">Zobrazí se stejnou předponou z více než jeden okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4516c-173">You may receive the same prefix from more than one ExpressRoute circuit.</span></span> <span data-ttu-id="4516c-174">Chcete-li zvolit, které připojení odesílat provoz určený pro tuto předponu, můžete změnit *RoutingWeight* připojení.</span><span class="sxs-lookup"><span data-stu-id="4516c-174">To choose which connection to send traffic destined for this prefix, you can change *RoutingWeight* of a connection.</span></span> <span data-ttu-id="4516c-175">Přenosy se odesílají na připojení s nejvyšší *RoutingWeight*.</span><span class="sxs-lookup"><span data-stu-id="4516c-175">Traffic will be sent on the connection with the highest *RoutingWeight*.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

<span data-ttu-id="4516c-176">Rozsah *RoutingWeight* je od 0 do 32 000.</span><span class="sxs-lookup"><span data-stu-id="4516c-176">The range of *RoutingWeight* is 0 to 32000.</span></span> <span data-ttu-id="4516c-177">Výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="4516c-177">The default value is 0.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4516c-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4516c-178">Next steps</span></span>
<span data-ttu-id="4516c-179">Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="4516c-179">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
