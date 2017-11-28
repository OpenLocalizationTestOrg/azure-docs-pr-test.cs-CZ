---
title: "Propojení virtuální sítě tooan okruh ExpressRoute: prostředí PowerShell: classic: Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak toolink virtuální sítě (virtuální sítě) tooExpressRoute okruhy pomocí modelu nasazení classic hello a prostředí PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: ganesr
ms.openlocfilehash: 6b8a01dcd4bbb9618ec3dd438cf0107538fb2a7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="39d3a-103">Připojit okruh ExpressRoute tooan virtuální sítě pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="39d3a-103">Connect a virtual network tooan ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="39d3a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="39d3a-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="39d3a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="39d3a-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="39d3a-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="39d3a-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="39d3a-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="39d3a-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="39d3a-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="39d3a-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="39d3a-109">Tento článek vám pomůže propojení virtuální sítě (virtuální sítě) okruhy ExpressRoute tooAzure pomocí modelu nasazení classic hello a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="39d3a-109">This article will help you link virtual networks (VNets) tooAzure ExpressRoute circuits by using hello classic deployment model and PowerShell.</span></span> <span data-ttu-id="39d3a-110">Virtuální sítě může být v hello stejného předplatného nebo můžou být součástí jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="39d3a-110">Virtual networks can either be in hello same subscription or can be part of another subscription.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="39d3a-111">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="39d3a-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a><span data-ttu-id="39d3a-112">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="39d3a-112">Configuration prerequisites</span></span>
1. <span data-ttu-id="39d3a-113">Je nutné hello nejnovější verzi modulů prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="39d3a-113">You need hello latest version of hello Azure PowerShell modules.</span></span> <span data-ttu-id="39d3a-114">Nejnovější moduly Powershellu hello si můžete stáhnout z hello prostředí PowerShell části hello [Azure stáhne stránky](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="39d3a-114">You can download hello latest PowerShell modules from hello PowerShell section of hello [Azure Downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="39d3a-115">Postupujte podle pokynů hello v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) podrobný návod jak tooconfigure modulů prostředí Azure PowerShell hello toouse vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="39d3a-115">Follow hello instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for step-by-step guidance on how tooconfigure your computer toouse hello Azure PowerShell modules.</span></span>
2. <span data-ttu-id="39d3a-116">Je třeba tooreview hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="39d3a-116">You need tooreview hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
3. <span data-ttu-id="39d3a-117">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="39d3a-117">You must have an active ExpressRoute circuit.</span></span>
   * <span data-ttu-id="39d3a-118">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md) a váš poskytovatel připojení povolit hello okruh.</span><span class="sxs-lookup"><span data-stu-id="39d3a-118">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have your connectivity provider enable hello circuit.</span></span>
   * <span data-ttu-id="39d3a-119">Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="39d3a-119">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="39d3a-120">V tématu hello [konfigurace směrování](expressroute-howto-routing-classic.md) směrování pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="39d3a-120">See hello [Configure routing](expressroute-howto-routing-classic.md) article for routing instructions.</span></span>
   * <span data-ttu-id="39d3a-121">Zajistěte, aby soukromý partnerský vztah Azure je nakonfigurován a hello partnerského vztahu protokolu BGP mezi vaší sítí a Microsoftem zapnutý tak, že povolíte připojení klient server.</span><span class="sxs-lookup"><span data-stu-id="39d3a-121">Ensure that Azure private peering is configured and hello BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
   * <span data-ttu-id="39d3a-122">Musí mít virtuální sítě a brány virtuální sítě vytvoří a plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="39d3a-122">You must have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="39d3a-123">Postupujte podle pokynů hello příliš[konfigurace virtuální sítě pro ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="39d3a-123">Follow hello instructions too[configure a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

<span data-ttu-id="39d3a-124">Můžete propojit až too10 virtuální sítě tooan okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="39d3a-124">You can link up too10 virtual networks tooan ExpressRoute circuit.</span></span> <span data-ttu-id="39d3a-125">Všechny virtuální sítě musí být v hello stejné geopolitické oblasti.</span><span class="sxs-lookup"><span data-stu-id="39d3a-125">All virtual networks must be in hello same geopolitical region.</span></span> <span data-ttu-id="39d3a-126">Můžete propojit větší počet virtuálních sítí tooyour okruh ExpressRoute nebo propojení virtuálních sítí, které jsou v dalších geopolitických oblastí, pokud jste povolili hello doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="39d3a-126">You can link a larger number of virtual networks tooyour ExpressRoute circuit, or link virtual networks that are in other geopolitical regions if you enabled hello ExpressRoute premium add-on.</span></span> <span data-ttu-id="39d3a-127">Zkontrolujte hello [– nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium hello.</span><span class="sxs-lookup"><span data-stu-id="39d3a-127">Check hello [FAQ](expressroute-faqs.md) for more details on hello premium add-on.</span></span>

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="39d3a-128">Připojit virtuální síť v hello stejnému okruhu tooa předplatného</span><span class="sxs-lookup"><span data-stu-id="39d3a-128">Connect a virtual network in hello same subscription tooa circuit</span></span>
<span data-ttu-id="39d3a-129">Pomocí následující rutiny hello můžete propojit virtuální sítě tooan okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="39d3a-129">You can link a virtual network tooan ExpressRoute circuit by using hello following cmdlet.</span></span> <span data-ttu-id="39d3a-130">Ujistěte se, že hello brány virtuální sítě se vytvoří a je připravený k propojení před spuštěním rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="39d3a-130">Make sure that hello virtual network gateway is created and is ready for linking before you run hello cmdlet.</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="39d3a-131">Připojit virtuální síť v okruhu tooa jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="39d3a-131">Connect a virtual network in a different subscription tooa circuit</span></span>
<span data-ttu-id="39d3a-132">Okruh ExpressRoute můžete sdílet mezi více předplatných.</span><span class="sxs-lookup"><span data-stu-id="39d3a-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="39d3a-133">Hello následující obrázek znázorňuje jednoduchý schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.</span><span class="sxs-lookup"><span data-stu-id="39d3a-133">hello following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="39d3a-134">Každý hello menší cloudy v rámci cloudu velké hello je použité toorepresent odběry, které patří toodifferent oddělení v organizaci.</span><span class="sxs-lookup"><span data-stu-id="39d3a-134">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span> <span data-ttu-id="39d3a-135">Každé oddělení hello v rámci organizace hello můžete použít vlastní předplatné pro nasazení jejich služeb – ale hello oddělení může sdílet jeden back tooyour místní síť tooconnect okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="39d3a-135">Each of hello departments within hello organization can use their own subscription for deploying their services--but hello departments can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span> <span data-ttu-id="39d3a-136">Jednoho oddělení (v tomto příkladu: IT) můžete vlastní hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="39d3a-136">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="39d3a-137">Hello okruh ExpressRoute můžete použít jiné odběry v rámci organizace hello.</span><span class="sxs-lookup"><span data-stu-id="39d3a-137">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="39d3a-138">Připojení a šířku pásma poplatky za hello vyhrazené okruhu bude použité toohello vlastníka okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="39d3a-138">Connectivity and bandwidth charges for hello dedicated circuit will be applied toohello ExpressRoute circuit owner.</span></span> <span data-ttu-id="39d3a-139">Všechny virtuální sítě sdílejí hello stejné šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="39d3a-139">All virtual networks share hello same bandwidth.</span></span>
> 
> 

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a><span data-ttu-id="39d3a-141">Správa</span><span class="sxs-lookup"><span data-stu-id="39d3a-141">Administration</span></span>
<span data-ttu-id="39d3a-142">Hello *vlastníka okruhu* je hello správce nebo spolusprávce předplatného hello, ve které hello ExpressRoute je vytvořen okruh.</span><span class="sxs-lookup"><span data-stu-id="39d3a-142">hello *circuit owner* is hello administrator/coadministrator of hello subscription in which hello ExpressRoute circuit is created.</span></span> <span data-ttu-id="39d3a-143">Hello vlastníka okruhu může autorizovat správci nebo coadministrators další odběry, označují tooas *okruhů uživatelů*, toouse hello vyhrazené okruh, kterou vlastní.</span><span class="sxs-lookup"><span data-stu-id="39d3a-143">hello circuit owner can authorize administrators/coadministrators of other subscriptions, referred tooas *circuit users*, toouse hello dedicated circuit that they own.</span></span> <span data-ttu-id="39d3a-144">Okruh uživatelé, kteří jsou může okruh ExpressRoute organizace hello autorizovaný toouse propojit hello virtuální síť ve své předplatné toohello okruh ExpressRoute po jejich oprávnění.</span><span class="sxs-lookup"><span data-stu-id="39d3a-144">Circuit users who are authorized toouse hello organization's ExpressRoute circuit can link hello virtual network in their subscription toohello ExpressRoute circuit after they are authorized.</span></span>

<span data-ttu-id="39d3a-145">vlastníka okruhu Hello má hello power toomodify a odvolat oprávnění kdykoli.</span><span class="sxs-lookup"><span data-stu-id="39d3a-145">hello circuit owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="39d3a-146">Odvolání povolení bude výsledkem všechny odkazy odstraňuje z předplatného hello jehož přístup byl odvolán.</span><span class="sxs-lookup"><span data-stu-id="39d3a-146">Revoking an authorization will result in all links being deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="39d3a-147">Operace vlastníka okruhu</span><span class="sxs-lookup"><span data-stu-id="39d3a-147">Circuit owner operations</span></span>

<span data-ttu-id="39d3a-148">**Vytvoření ověření**</span><span class="sxs-lookup"><span data-stu-id="39d3a-148">**Creating an authorization**</span></span>

<span data-ttu-id="39d3a-149">Hello vlastníka okruhu autorizuje hello správci odběr toouse hello zadaný okruh.</span><span class="sxs-lookup"><span data-stu-id="39d3a-149">hello circuit owner authorizes hello administrators of other subscriptions toouse hello specified circuit.</span></span> <span data-ttu-id="39d3a-150">V následujícím příkladu hello správce hello okruhu hello (Contoso IT) umožňuje hello správce jiné předplatné (Dev-Test) toolink si okruh toohello tootwo virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="39d3a-150">In hello following example, hello administrator of hello circuit (Contoso IT) enables hello administrator of another subscription (Dev-Test) toolink up tootwo virtual networks toohello circuit.</span></span> <span data-ttu-id="39d3a-151">Správce Contoso IT Hello umožňuje to tak, že zadáte ID hello Microsoft Dev-Test.</span><span class="sxs-lookup"><span data-stu-id="39d3a-151">hello Contoso IT administrator enables this by specifying hello Dev-Test Microsoft ID.</span></span> <span data-ttu-id="39d3a-152">rutiny Hello neodešle toohello e-mailu zadat ID společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="39d3a-152">hello cmdlet doesn't send email toohello specified Microsoft ID.</span></span> <span data-ttu-id="39d3a-153">vlastníka okruhu Hello musí tooexplicitly oznámit hello jiných vlastník předplatného, které hello autorizace je dokončena.</span><span class="sxs-lookup"><span data-stu-id="39d3a-153">hello circuit owner needs tooexplicitly notify hello other subscription owner that hello authorization is complete.</span></span>

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

<span data-ttu-id="39d3a-154">**Kontrola povolení**</span><span class="sxs-lookup"><span data-stu-id="39d3a-154">**Reviewing authorizations**</span></span>

<span data-ttu-id="39d3a-155">vlastníka okruhu Hello můžete zkontrolovat všechny autorizací, které jsou vydány na konkrétní okruhu spuštěním následující rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="39d3a-155">hello circuit owner can review all authorizations that are issued on a particular circuit by running hello following cmdlet:</span></span>

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


<span data-ttu-id="39d3a-156">**Aktualizace oprávnění**</span><span class="sxs-lookup"><span data-stu-id="39d3a-156">**Updating authorizations**</span></span>

<span data-ttu-id="39d3a-157">vlastníka okruhu Hello můžete upravit autorizací pomocí hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="39d3a-157">hello circuit owner can modify authorizations by using hello following cmdlet:</span></span>

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


<span data-ttu-id="39d3a-158">**Odstranění autorizací**</span><span class="sxs-lookup"><span data-stu-id="39d3a-158">**Deleting authorizations**</span></span>

<span data-ttu-id="39d3a-159">vlastníka okruhu Hello můžete odvolání nebo odstranění autorizací toohello uživatele spuštěním následující rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="39d3a-159">hello circuit owner can revoke/delete authorizations toohello user by running hello following cmdlet:</span></span>

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a><span data-ttu-id="39d3a-160">Operace okruh uživatele</span><span class="sxs-lookup"><span data-stu-id="39d3a-160">Circuit user operations</span></span>

<span data-ttu-id="39d3a-161">**Kontrola povolení**</span><span class="sxs-lookup"><span data-stu-id="39d3a-161">**Reviewing authorizations**</span></span>

<span data-ttu-id="39d3a-162">uživatele okruhu Hello můžete zkontrolovat autorizací pomocí hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="39d3a-162">hello circuit user can review authorizations by using hello following cmdlet:</span></span>

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

<span data-ttu-id="39d3a-163">**Uplatňuje autorizací propojení**</span><span class="sxs-lookup"><span data-stu-id="39d3a-163">**Redeeming link authorizations**</span></span>

<span data-ttu-id="39d3a-164">uživatele okruhu Hello můžete spustit následující rutiny tooredeem hello odkaz autorizace:</span><span class="sxs-lookup"><span data-stu-id="39d3a-164">hello circuit user can run hello following cmdlet tooredeem a link authorization:</span></span>

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

<span data-ttu-id="39d3a-165">Spusťte tento příkaz v předplatném hello nově propojené hello virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="39d3a-165">Run this command in hello newly linked subscription for hello virtual network:</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a><span data-ttu-id="39d3a-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39d3a-166">Next steps</span></span>
<span data-ttu-id="39d3a-167">Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="39d3a-167">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

