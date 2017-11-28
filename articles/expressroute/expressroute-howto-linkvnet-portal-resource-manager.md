---
title: "Propojení virtuální sítě tooan okruh ExpressRoute: portálu Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak toolink virtuální sítě okruhů tooExpressRoute (virtuální sítě)."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8bedcb11df7e30281fd439afdfb76cc67626a8f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="19d6b-103">Připojit virtuální síť tooan okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="19d6b-103">Connect a virtual network tooan ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="19d6b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="19d6b-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="19d6b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="19d6b-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="19d6b-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="19d6b-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="19d6b-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="19d6b-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="19d6b-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="19d6b-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

<span data-ttu-id="19d6b-109">Tento článek vám pomůže propojení virtuální sítě (virtuální sítě) okruhy ExpressRoute tooAzure pomocí modelu nasazení Resource Manager hello a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="19d6b-109">This article helps you link virtual networks (VNets) tooAzure ExpressRoute circuits by using hello Resource Manager deployment model and hello Azure portal.</span></span> <span data-ttu-id="19d6b-110">Virtuální sítě může být buď v hello stejného předplatného, nebo může být součást jiného předplatného.</span><span class="sxs-lookup"><span data-stu-id="19d6b-110">Virtual networks can either be in hello same subscription, or they can be part of another subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="19d6b-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="19d6b-111">Before you begin</span></span>
* <span data-ttu-id="19d6b-112">Zkontrolujte hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="19d6b-112">Review hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="19d6b-113">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-113">You must have an active ExpressRoute circuit.</span></span>
  
  * <span data-ttu-id="19d6b-114">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mít hello ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="19d6b-114">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider.</span></span>
  * <span data-ttu-id="19d6b-115">Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="19d6b-115">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="19d6b-116">V tématu hello [konfigurace směrování](expressroute-howto-routing-portal-resource-manager.md) směrování pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="19d6b-116">See hello [Configure routing](expressroute-howto-routing-portal-resource-manager.md) article for routing instructions.</span></span>
  * <span data-ttu-id="19d6b-117">Zajistěte, aby soukromý partnerský vztah Azure je nakonfigurován a hello partnerského vztahu protokolu BGP mezi vaší sítí a Microsoftem zapnutý tak, že povolíte připojení klient server.</span><span class="sxs-lookup"><span data-stu-id="19d6b-117">Ensure that Azure private peering is configured and hello BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="19d6b-118">Ujistěte se, že máte virtuální sítě a brány virtuální sítě vytvoří a plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="19d6b-118">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="19d6b-119">Postupujte podle pokynů hello příliš[vytvořit bránu virtuální sítě pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="19d6b-119">Follow hello instructions too[create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="19d6b-120">Brána virtuální sítě pro ExpressRoute používá hello GatewayType 'ExpressRoute' není síť VPN.</span><span class="sxs-lookup"><span data-stu-id="19d6b-120">A virtual network gateway for ExpressRoute uses hello GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="19d6b-121">Můžete propojit až too10 virtuální sítě tooa standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-121">You can link up too10 virtual networks tooa standard ExpressRoute circuit.</span></span> <span data-ttu-id="19d6b-122">Všechny virtuální sítě musí být v hello stejné geopolitické oblasti při použití standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-122">All virtual networks must be in hello same geopolitical region when using a standard ExpressRoute circuit.</span></span> 
* <span data-ttu-id="19d6b-123">Můžete propojit virtuální sítě mimo hello geopolitické oblasti hello okruh ExpressRoute nebo připojit větší počet virtuálních sítí tooyour okruh ExpressRoute, pokud jste povolili hello doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="19d6b-123">You can link a virtual network outside of hello geopolitical region of hello ExpressRoute circuit, or connect a larger number of virtual networks tooyour ExpressRoute circuit if you enabled hello ExpressRoute premium add-on.</span></span> <span data-ttu-id="19d6b-124">Zkontrolujte hello [– nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium hello.</span><span class="sxs-lookup"><span data-stu-id="19d6b-124">Check hello [FAQ](expressroute-faqs.md) for more details on hello premium add-on.</span></span>
* <span data-ttu-id="19d6b-125">Můžete [zobrazit video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) před začátku toobetter pochopit hello kroky.</span><span class="sxs-lookup"><span data-stu-id="19d6b-125">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) before beginning toobetter understand hello steps.</span></span>

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="19d6b-126">Připojit virtuální síť v hello stejnému okruhu tooa předplatného</span><span class="sxs-lookup"><span data-stu-id="19d6b-126">Connect a virtual network in hello same subscription tooa circuit</span></span>

### <a name="toocreate-a-connection"></a><span data-ttu-id="19d6b-127">toocreate připojení</span><span class="sxs-lookup"><span data-stu-id="19d6b-127">toocreate a connection</span></span>

> [!NOTE]
> <span data-ttu-id="19d6b-128">Informace o konfiguraci protokolu BGP nezobrazí, pokud zprostředkovatel vrstvy 3 hello nakonfigurovaný vaší partnerských vztahů.</span><span class="sxs-lookup"><span data-stu-id="19d6b-128">BGP configuration information will not show up if hello layer 3 provider configured your peerings.</span></span> <span data-ttu-id="19d6b-129">Pokud váš okruh je ve stavu, zřízené, musí být schopný toocreate připojení.</span><span class="sxs-lookup"><span data-stu-id="19d6b-129">If your circuit is in a provisioned state, you should be able toocreate connections.</span></span>
>

1. <span data-ttu-id="19d6b-130">Zkontrolujte, že stav okruhu ExpressRoute a soukromý partnerský vztah Azure úspěšně nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="19d6b-130">Ensure that your ExpressRoute circuit and Azure private peering have been configured successfully.</span></span> <span data-ttu-id="19d6b-131">Postupujte podle pokynů hello v [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a [konfigurace směrování](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="19d6b-131">Follow hello instructions in [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [Configure routing](expressroute-howto-routing-arm.md).</span></span> <span data-ttu-id="19d6b-132">Váš okruh ExpressRoute by měl vypadat podobně jako hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="19d6b-132">Your ExpressRoute circuit should look like hello following image:</span></span>

    ![Snímek obrazovky okruhu ExpressRoute](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. <span data-ttu-id="19d6b-134">Nyní můžete spustit zřizování toolink připojení vaší virtuální síti Brána tooyour okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-134">You can now start provisioning a connection toolink your virtual network gateway tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="19d6b-135">Klikněte na tlačítko **připojení** > **přidat** tooopen hello **přidat připojení** okno a pak nakonfigurujte hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="19d6b-135">Click **Connection** > **Add** tooopen hello **Add connection** blade, and then configure hello values.</span></span>

    ![Přidat připojení – snímek obrazovky](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. <span data-ttu-id="19d6b-137">Po úspěšném dokončení konfigurace připojení k vaší objekt připojení zobrazí hello informace pro připojení k hello.</span><span class="sxs-lookup"><span data-stu-id="19d6b-137">After your connection has been successfully configured, your connection object will show hello information for hello connection.</span></span>

     ![Snímek obrazovky připojení objektu](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="toodelete-a-connection"></a><span data-ttu-id="19d6b-139">toodelete připojení</span><span class="sxs-lookup"><span data-stu-id="19d6b-139">toodelete a connection</span></span>
<span data-ttu-id="19d6b-140">Připojení můžete odstranit výběrem hello **odstranit** ikonu na hello okno pro připojení.</span><span class="sxs-lookup"><span data-stu-id="19d6b-140">You can delete a connection by selecting hello **Delete** icon on hello blade for your connection.</span></span>

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="19d6b-141">Připojit virtuální síť v okruhu tooa jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="19d6b-141">Connect a virtual network in a different subscription tooa circuit</span></span>
<span data-ttu-id="19d6b-142">Okruh ExpressRoute můžete sdílet mezi více předplatných.</span><span class="sxs-lookup"><span data-stu-id="19d6b-142">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="19d6b-143">Hello na následujícím obrázku jednoduchou schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.</span><span class="sxs-lookup"><span data-stu-id="19d6b-143">hello figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- <span data-ttu-id="19d6b-145">Každý hello menší cloudy v rámci cloudu velké hello je použité toorepresent odběry, které patří toodifferent oddělení v organizaci.</span><span class="sxs-lookup"><span data-stu-id="19d6b-145">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span>
- <span data-ttu-id="19d6b-146">Každé oddělení hello v rámci organizace hello můžete použít vlastní předplatné nasazování služeb, ale můžou sdílet jeden back tooyour místní síť tooconnect okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-146">Each of hello departments within hello organization can use their own subscription for deploying their services, but they can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span>
- <span data-ttu-id="19d6b-147">Jednoho oddělení (v tomto příkladu: IT) můžete vlastní hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-147">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="19d6b-148">Hello okruh ExpressRoute můžete použít jiné odběry v rámci organizace hello.</span><span class="sxs-lookup"><span data-stu-id="19d6b-148">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="19d6b-149">Připojení a šířku pásma poplatky za hello vyhrazené okruhu bude použité toohello vlastníka okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-149">Connectivity and bandwidth charges for hello dedicated circuit will be applied toohello ExpressRoute circuit owner.</span></span> <span data-ttu-id="19d6b-150">Všechny virtuální sítě sdílejí hello stejné šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="19d6b-150">All virtual networks share hello same bandwidth.</span></span>
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="19d6b-151">Správa - vlastníky okruhu a okruh uživatelů</span><span class="sxs-lookup"><span data-stu-id="19d6b-151">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="19d6b-152">Hello 'vlastníka okruhu, je oprávněný uživatel napájení z hello prostředků okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-152">hello 'circuit owner' is an authorized Power User of hello ExpressRoute circuit resource.</span></span> <span data-ttu-id="19d6b-153">vlastníka okruhu Hello můžete vytvořit autorizací, které lze uplatnit podle 'okruh uživatele'.</span><span class="sxs-lookup"><span data-stu-id="19d6b-153">hello circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="19d6b-154">Vlastníci virtuální sítě jsou uživatelé okruh brány, které nejsou uvnitř hello stejného předplatného jako hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-154">Circuit users are owners of virtual network gateways that are not within hello same subscription as hello ExpressRoute circuit.</span></span> <span data-ttu-id="19d6b-155">Uživatelé okruhu můžete uplatnit autorizací (jedním autorizačním na jednu virtuální síť).</span><span class="sxs-lookup"><span data-stu-id="19d6b-155">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="19d6b-156">vlastníka okruhu Hello má hello power toomodify a odvolat oprávnění kdykoli.</span><span class="sxs-lookup"><span data-stu-id="19d6b-156">hello circuit owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="19d6b-157">Odvolání autorizaci má za následek všechna připojení odkaz odstraňuje z předplatného hello jehož přístup byl odvolán.</span><span class="sxs-lookup"><span data-stu-id="19d6b-157">Revoking an authorization results in all link connections being deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="19d6b-158">Operace vlastníka okruhu</span><span class="sxs-lookup"><span data-stu-id="19d6b-158">Circuit owner operations</span></span>

<span data-ttu-id="19d6b-159">**toocreate autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="19d6b-159">**toocreate a connection authorization**</span></span>

<span data-ttu-id="19d6b-160">vlastníka okruhu Hello vytvoří povolení.</span><span class="sxs-lookup"><span data-stu-id="19d6b-160">hello circuit owner creates an authorization.</span></span> <span data-ttu-id="19d6b-161">To vede k vytváření hello autorizační klíč, který může být používán tooconnect uživatele okruhu jejich toohello brány virtuální sítě okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19d6b-161">This results in hello creation of an authorization key that can be used by a circuit user tooconnect their virtual network gateways toohello ExpressRoute circuit.</span></span> <span data-ttu-id="19d6b-162">Povolení je platný pro jenom jedno připojení.</span><span class="sxs-lookup"><span data-stu-id="19d6b-162">An authorization is valid for only one connection.</span></span>

1. <span data-ttu-id="19d6b-163">V okně hello ExpressRoute, klikněte na tlačítko **autorizací** a potom zadejte **název** hello autorizace a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="19d6b-163">In hello ExpressRoute blade, Click **Authorizations** and then type a **name** for hello authorization and click **Save**.</span></span>

    ![Povolení](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. <span data-ttu-id="19d6b-165">Po uložení konfigurace hello zkopírujte hello **ID prostředku** a hello **autorizační klíč**.</span><span class="sxs-lookup"><span data-stu-id="19d6b-165">Once hello configuration is saved, copy hello **Resource ID** and hello **Authorization Key**.</span></span>

    ![Autorizační klíč](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

<span data-ttu-id="19d6b-167">**toodelete autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="19d6b-167">**toodelete a connection authorization**</span></span>

<span data-ttu-id="19d6b-168">Připojení můžete odstranit výběrem hello **odstranit** ikonu na hello okno pro připojení.</span><span class="sxs-lookup"><span data-stu-id="19d6b-168">You can delete a connection by selecting hello **Delete** icon on hello blade for your connection.</span></span>

### <a name="circuit-user-operations"></a><span data-ttu-id="19d6b-169">Operace okruh uživatele</span><span class="sxs-lookup"><span data-stu-id="19d6b-169">Circuit user operations</span></span>

<span data-ttu-id="19d6b-170">Hello okruh uživatel potřebuje ID prostředku hello a autorizační klíč z vlastníka okruhu hello.</span><span class="sxs-lookup"><span data-stu-id="19d6b-170">hello circuit user needs hello resource ID and an authorization key from hello circuit owner.</span></span> 

<span data-ttu-id="19d6b-171">**tooredeem autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="19d6b-171">**tooredeem a connection authorization**</span></span>

1.  <span data-ttu-id="19d6b-172">Klikněte na tlačítko hello **+ nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="19d6b-172">Click hello **+New** button.</span></span>

    ![Klikněte na tlačítko Nový](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  <span data-ttu-id="19d6b-174">Vyhledejte **"Připojení"** v hello Marketplace, vyberte ho a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="19d6b-174">Search for **"Connection"** in hello Marketplace, select it, and click **Create**.</span></span>

    ![Hledání připojení](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  <span data-ttu-id="19d6b-176">Ujistěte se, zda text hello **typ připojení** je nastaven příliš "ExpressRoute".</span><span class="sxs-lookup"><span data-stu-id="19d6b-176">Make sure hello **Connection type** is set too"ExpressRoute".</span></span>


4.  <span data-ttu-id="19d6b-177">Zadejte podrobnosti hello a pak klikněte na **OK** v okně základy hello.</span><span class="sxs-lookup"><span data-stu-id="19d6b-177">Fill in hello details, then click **OK** in hello Basics blade.</span></span>

    ![Okno Základy](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  <span data-ttu-id="19d6b-179">V hello **nastavení** okně, vyberte hello **Brána virtuální sítě** a zkontrolujte hello **uplatnit autorizace** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="19d6b-179">In hello **Settings** blade, Select hello **Virtual network gateway** and check hello **Redeem authorization** check box.</span></span>

6.  <span data-ttu-id="19d6b-180">Zadejte hello **autorizační klíč** a hello **Peer okruh URI** a pojmenujte hello připojení.</span><span class="sxs-lookup"><span data-stu-id="19d6b-180">Enter hello **Authorization key** and hello **Peer circuit URI** and give hello connection a name.</span></span> <span data-ttu-id="19d6b-181">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="19d6b-181">Click **OK**.</span></span>

    ![Okno Nastavení](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. <span data-ttu-id="19d6b-183">Zkontrolujte informace hello v hello **Souhrn** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="19d6b-183">Review hello information in hello **Summary** blade and click **OK**.</span></span>


<span data-ttu-id="19d6b-184">**toorelease autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="19d6b-184">**toorelease a connection authorization**</span></span>

<span data-ttu-id="19d6b-185">Povolení můžete vydat odstraněním hello připojení, který odkazuje hello ExpressRoute okruhu toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="19d6b-185">You can release an authorization by deleting hello connection that links hello ExpressRoute circuit toohello virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="19d6b-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19d6b-186">Next steps</span></span>
<span data-ttu-id="19d6b-187">Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="19d6b-187">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
