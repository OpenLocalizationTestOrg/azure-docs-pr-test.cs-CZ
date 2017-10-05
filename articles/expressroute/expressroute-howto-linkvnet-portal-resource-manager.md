---
title: "Propojení virtuální sítě k okruhu ExpressRoute: portálu Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak propojit virtuální sítě (virtuální sítě) pro okruhy ExpressRoute."
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
ms.openlocfilehash: 595c30ab5d9adc6061ad753d952adf894ba80b2f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="8520f-103">Připojit virtuální sítě k okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="8520f-103">Connect a virtual network to an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8520f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8520f-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="8520f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8520f-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="8520f-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8520f-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="8520f-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="8520f-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="8520f-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="8520f-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

<span data-ttu-id="8520f-109">Tento článek pomáhá propojení virtuálních sítí (virtuální sítě) pro okruhy Azure ExpressRoute pomocí modelu nasazení Resource Manager a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8520f-109">This article helps you link virtual networks (VNets) to Azure ExpressRoute circuits by using the Resource Manager deployment model and the Azure portal.</span></span> <span data-ttu-id="8520f-110">Virtuální sítě může být v rámci stejného předplatného, nebo mohou být součástí jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="8520f-110">Virtual networks can either be in the same subscription, or they can be part of another subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8520f-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8520f-111">Before you begin</span></span>
* <span data-ttu-id="8520f-112">Zkontrolujte [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8520f-112">Review the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="8520f-113">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8520f-113">You must have an active ExpressRoute circuit.</span></span>
  
  * <span data-ttu-id="8520f-114">Postupujte podle pokynů a [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mějte ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="8520f-114">Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have the circuit enabled by your connectivity provider.</span></span>
  * <span data-ttu-id="8520f-115">Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="8520f-115">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="8520f-116">Najdete v článku [konfigurace směrování](expressroute-howto-routing-portal-resource-manager.md) směrování pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="8520f-116">See the [Configure routing](expressroute-howto-routing-portal-resource-manager.md) article for routing instructions.</span></span>
  * <span data-ttu-id="8520f-117">Zajistěte, aby soukromý partnerský vztah Azure je nakonfigurován a partnerském vztahu protokolu BGP mezi vaší sítí a Microsoftem zapnutý tak, že povolíte připojení klient server.</span><span class="sxs-lookup"><span data-stu-id="8520f-117">Ensure that Azure private peering is configured and the BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="8520f-118">Ujistěte se, že máte virtuální sítě a brány virtuální sítě vytvoří a plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="8520f-118">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="8520f-119">Postupujte podle pokynů a [vytvořit bránu virtuální sítě pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="8520f-119">Follow the instructions to [create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="8520f-120">Brána virtuální sítě pro ExpressRoute používá GatewayType 'ExpressRoute' není síť VPN.</span><span class="sxs-lookup"><span data-stu-id="8520f-120">A virtual network gateway for ExpressRoute uses the GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="8520f-121">Až 10 virtuálních sítí můžete propojit standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8520f-121">You can link up to 10 virtual networks to a standard ExpressRoute circuit.</span></span> <span data-ttu-id="8520f-122">Všechny virtuální sítě musí být ve stejné geopolitické oblasti, při použití standardní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8520f-122">All virtual networks must be in the same geopolitical region when using a standard ExpressRoute circuit.</span></span> 
* <span data-ttu-id="8520f-123">Můžete propojit virtuální sítě mimo geopolitické oblasti okruhu ExpressRoute nebo větší počet virtuálních sítí připojit k okruhu ExpressRoute, pokud jste povolili doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="8520f-123">You can link a virtual network outside of the geopolitical region of the ExpressRoute circuit, or connect a larger number of virtual networks to your ExpressRoute circuit if you enabled the ExpressRoute premium add-on.</span></span> <span data-ttu-id="8520f-124">Zkontrolujte [– nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium.</span><span class="sxs-lookup"><span data-stu-id="8520f-124">Check the [FAQ](expressroute-faqs.md) for more details on the premium add-on.</span></span>
* <span data-ttu-id="8520f-125">Můžete [zobrazit video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) před zahájením pro lepší pochopení kroků.</span><span class="sxs-lookup"><span data-stu-id="8520f-125">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) before beginning to better understand the steps.</span></span>

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a><span data-ttu-id="8520f-126">Připojit k okruhu virtuální sítě v rámci stejného předplatného</span><span class="sxs-lookup"><span data-stu-id="8520f-126">Connect a virtual network in the same subscription to a circuit</span></span>

### <a name="to-create-a-connection"></a><span data-ttu-id="8520f-127">Chcete-li vytvořit připojení</span><span class="sxs-lookup"><span data-stu-id="8520f-127">To create a connection</span></span>

> [!NOTE]
> <span data-ttu-id="8520f-128">Informace o konfiguraci protokolu BGP nezobrazí, pokud zprostředkovatel služby vrstvy 3 nakonfigurovaný vaší partnerských vztahů.</span><span class="sxs-lookup"><span data-stu-id="8520f-128">BGP configuration information will not show up if the layer 3 provider configured your peerings.</span></span> <span data-ttu-id="8520f-129">Pokud váš okruh je ve stavu, zřízené, byste měli k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="8520f-129">If your circuit is in a provisioned state, you should be able to create connections.</span></span>
>

1. <span data-ttu-id="8520f-130">Zkontrolujte, že stav okruhu ExpressRoute a soukromý partnerský vztah Azure úspěšně nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="8520f-130">Ensure that your ExpressRoute circuit and Azure private peering have been configured successfully.</span></span> <span data-ttu-id="8520f-131">Postupujte podle pokynů v [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a [konfigurace směrování](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="8520f-131">Follow the instructions in [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [Configure routing](expressroute-howto-routing-arm.md).</span></span> <span data-ttu-id="8520f-132">Váš okruh ExpressRoute by měl vypadat podobně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="8520f-132">Your ExpressRoute circuit should look like the following image:</span></span>

    ![Snímek obrazovky okruhu ExpressRoute](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. <span data-ttu-id="8520f-134">Nyní můžete spustit zřizování připojení k propojení vaší brány virtuální sítě k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8520f-134">You can now start provisioning a connection to link your virtual network gateway to your ExpressRoute circuit.</span></span> <span data-ttu-id="8520f-135">Klikněte na tlačítko **připojení** > **přidat** otevřete **přidat připojení** okno a pak proveďte konfiguraci hodnot.</span><span class="sxs-lookup"><span data-stu-id="8520f-135">Click **Connection** > **Add** to open the **Add connection** blade, and then configure the values.</span></span>

    ![Přidat připojení – snímek obrazovky](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. <span data-ttu-id="8520f-137">Po úspěšném dokončení konfigurace připojení k objektu připojení se zobrazí informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="8520f-137">After your connection has been successfully configured, your connection object will show the information for the connection.</span></span>

     ![Snímek obrazovky připojení objektu](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="to-delete-a-connection"></a><span data-ttu-id="8520f-139">Odstranění připojení</span><span class="sxs-lookup"><span data-stu-id="8520f-139">To delete a connection</span></span>
<span data-ttu-id="8520f-140">Připojení můžete odstranit výběrem **odstranit** ikona v okně pro připojení.</span><span class="sxs-lookup"><span data-stu-id="8520f-140">You can delete a connection by selecting the **Delete** icon on the blade for your connection.</span></span>

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a><span data-ttu-id="8520f-141">Připojení virtuální sítě jiného předplatného k okruhu</span><span class="sxs-lookup"><span data-stu-id="8520f-141">Connect a virtual network in a different subscription to a circuit</span></span>
<span data-ttu-id="8520f-142">Okruh ExpressRoute můžete sdílet mezi více předplatných.</span><span class="sxs-lookup"><span data-stu-id="8520f-142">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="8520f-143">Následující obrázek znázorňuje jednoduchý schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.</span><span class="sxs-lookup"><span data-stu-id="8520f-143">The figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- <span data-ttu-id="8520f-145">Každý z menší cloudy v rámci velké cloudu se používá k reprezentování odběry, které patří k různým oblastem v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="8520f-145">Each of the smaller clouds within the large cloud is used to represent subscriptions that belong to different departments within an organization.</span></span>
- <span data-ttu-id="8520f-146">Každé oddělení v organizaci můžete použít vlastní předplatné nasazování služeb, ale můžou sdílet jeden okruh ExpressRoute zpětné připojení k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="8520f-146">Each of the departments within the organization can use their own subscription for deploying their services, but they can share a single ExpressRoute circuit to connect back to your on-premises network.</span></span>
- <span data-ttu-id="8520f-147">Jednoho oddělení (v tomto příkladu: IT) můžete vlastní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8520f-147">A single department (in this example: IT) can own the ExpressRoute circuit.</span></span> <span data-ttu-id="8520f-148">Okruh ExpressRoute můžete použít jiné odběry v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="8520f-148">Other subscriptions within the organization can use the ExpressRoute circuit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8520f-149">Připojení a šířku pásma poplatky za vyhrazené okruh uplatní na vlastníka okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8520f-149">Connectivity and bandwidth charges for the dedicated circuit will be applied to the ExpressRoute circuit owner.</span></span> <span data-ttu-id="8520f-150">Všechny virtuální sítě sdílejí stejné šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="8520f-150">All virtual networks share the same bandwidth.</span></span>
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="8520f-151">Správa - vlastníky okruhu a okruh uživatelů</span><span class="sxs-lookup"><span data-stu-id="8520f-151">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="8520f-152">Vlastníka okruhu je oprávněný uživatel Power prostředku okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8520f-152">The 'circuit owner' is an authorized Power User of the ExpressRoute circuit resource.</span></span> <span data-ttu-id="8520f-153">Vlastníka okruhu můžete vytvořit autorizací, které lze uplatnit podle 'okruh uživatele'.</span><span class="sxs-lookup"><span data-stu-id="8520f-153">The circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="8520f-154">Okruh uživatelů se vlastníci brány virtuální sítě, které nejsou v rámci stejného předplatného jako okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8520f-154">Circuit users are owners of virtual network gateways that are not within the same subscription as the ExpressRoute circuit.</span></span> <span data-ttu-id="8520f-155">Uživatelé okruhu můžete uplatnit autorizací (jedním autorizačním na jednu virtuální síť).</span><span class="sxs-lookup"><span data-stu-id="8520f-155">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="8520f-156">Vlastníka okruhu má právo upravit a kdykoli odvolat oprávnění.</span><span class="sxs-lookup"><span data-stu-id="8520f-156">The circuit owner has the power to modify and revoke authorizations at any time.</span></span> <span data-ttu-id="8520f-157">Odvolání autorizaci má za následek všechna připojení odkaz odstraňuje z předplatného, jejichž přístup byl odvolán.</span><span class="sxs-lookup"><span data-stu-id="8520f-157">Revoking an authorization results in all link connections being deleted from the subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="8520f-158">Operace vlastníka okruhu</span><span class="sxs-lookup"><span data-stu-id="8520f-158">Circuit owner operations</span></span>

<span data-ttu-id="8520f-159">**Chcete-li vytvořit připojení autorizace**</span><span class="sxs-lookup"><span data-stu-id="8520f-159">**To create a connection authorization**</span></span>

<span data-ttu-id="8520f-160">Vlastníka okruhu vytvoří povolení.</span><span class="sxs-lookup"><span data-stu-id="8520f-160">The circuit owner creates an authorization.</span></span> <span data-ttu-id="8520f-161">Výsledkem je vytvoření autorizační klíč, které je možné se připojit své brány virtuální sítě k okruhu ExpressRoute uživatelem okruh.</span><span class="sxs-lookup"><span data-stu-id="8520f-161">This results in the creation of an authorization key that can be used by a circuit user to connect their virtual network gateways to the ExpressRoute circuit.</span></span> <span data-ttu-id="8520f-162">Povolení je platný pro jenom jedno připojení.</span><span class="sxs-lookup"><span data-stu-id="8520f-162">An authorization is valid for only one connection.</span></span>

1. <span data-ttu-id="8520f-163">V okně ExpressRoute, klikněte na tlačítko **autorizací** a potom zadejte **název** pro ověřování a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8520f-163">In the ExpressRoute blade, Click **Authorizations** and then type a **name** for the authorization and click **Save**.</span></span>

    ![Povolení](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. <span data-ttu-id="8520f-165">Po uložení konfigurace zkopírovat **ID prostředku** a **autorizační klíč**.</span><span class="sxs-lookup"><span data-stu-id="8520f-165">Once the configuration is saved, copy the **Resource ID** and the **Authorization Key**.</span></span>

    ![Autorizační klíč](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

<span data-ttu-id="8520f-167">**Chcete-li odstranit autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="8520f-167">**To delete a connection authorization**</span></span>

<span data-ttu-id="8520f-168">Připojení můžete odstranit výběrem **odstranit** ikona v okně pro připojení.</span><span class="sxs-lookup"><span data-stu-id="8520f-168">You can delete a connection by selecting the **Delete** icon on the blade for your connection.</span></span>

### <a name="circuit-user-operations"></a><span data-ttu-id="8520f-169">Operace okruh uživatele</span><span class="sxs-lookup"><span data-stu-id="8520f-169">Circuit user operations</span></span>

<span data-ttu-id="8520f-170">ID prostředku a autorizační klíč z vlastníka okruhu, musí uživatel okruh.</span><span class="sxs-lookup"><span data-stu-id="8520f-170">The circuit user needs the resource ID and an authorization key from the circuit owner.</span></span> 

<span data-ttu-id="8520f-171">**Pokud chcete uplatnit autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="8520f-171">**To redeem a connection authorization**</span></span>

1.  <span data-ttu-id="8520f-172">Klikněte **+ nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8520f-172">Click the **+New** button.</span></span>

    ![Klikněte na tlačítko Nový](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  <span data-ttu-id="8520f-174">Vyhledejte **"Připojení"** na webu Marketplace, vyberte ho a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8520f-174">Search for **"Connection"** in the Marketplace, select it, and click **Create**.</span></span>

    ![Hledání připojení](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  <span data-ttu-id="8520f-176">Zajistěte, aby **typ připojení** je nastaven na "ExpressRoute".</span><span class="sxs-lookup"><span data-stu-id="8520f-176">Make sure the **Connection type** is set to "ExpressRoute".</span></span>


4.  <span data-ttu-id="8520f-177">Zadejte podrobnosti a pak klikněte na **OK** v okně základy.</span><span class="sxs-lookup"><span data-stu-id="8520f-177">Fill in the details, then click **OK** in the Basics blade.</span></span>

    ![Okno Základy](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  <span data-ttu-id="8520f-179">V **nastavení** okně, vyberte **Brána virtuální sítě** a zkontrolujte **uplatnit autorizace** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="8520f-179">In the **Settings** blade, Select the **Virtual network gateway** and check the **Redeem authorization** check box.</span></span>

6.  <span data-ttu-id="8520f-180">Zadejte **autorizační klíč** a **Peer okruh URI** a pojmenujte připojení.</span><span class="sxs-lookup"><span data-stu-id="8520f-180">Enter the **Authorization key** and the **Peer circuit URI** and give the connection a name.</span></span> <span data-ttu-id="8520f-181">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8520f-181">Click **OK**.</span></span>

    ![Okno Nastavení](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. <span data-ttu-id="8520f-183">Informace v **Souhrn** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8520f-183">Review the information in the **Summary** blade and click **OK**.</span></span>


<span data-ttu-id="8520f-184">**Chcete-li uvolnit autorizace připojení**</span><span class="sxs-lookup"><span data-stu-id="8520f-184">**To release a connection authorization**</span></span>

<span data-ttu-id="8520f-185">Povolení můžete vydat odstraněním připojení, který odkazuje okruhu ExpressRoute do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="8520f-185">You can release an authorization by deleting the connection that links the ExpressRoute circuit to the virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8520f-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8520f-186">Next steps</span></span>
<span data-ttu-id="8520f-187">Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="8520f-187">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
