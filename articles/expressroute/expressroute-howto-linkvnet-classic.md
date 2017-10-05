---
title: "Propojení virtuální sítě k okruhu ExpressRoute: prostředí PowerShell: classic: Azure | Microsoft Docs"
description: "Tento dokument obsahuje přehled o tom, jak propojit virtuální sítě (virtuální sítě) pro okruhy ExpressRoute pomocí modelu nasazení classic a prostředí PowerShell."
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
ms.openlocfilehash: 8df8a4c6ff0897c821e13248e0494b17e1a4992d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="21712-103">Připojit virtuální sítě k okruhu ExpressRoute pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="21712-103">Connect a virtual network to an ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="21712-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="21712-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="21712-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="21712-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="21712-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="21712-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="21712-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="21712-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="21712-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="21712-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="21712-109">Tento článek vám pomůže propojení virtuální sítě (virtuální sítě) pro okruhy Azure ExpressRoute pomocí modelu nasazení classic a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21712-109">This article will help you link virtual networks (VNets) to Azure ExpressRoute circuits by using the classic deployment model and PowerShell.</span></span> <span data-ttu-id="21712-110">Virtuální sítě může být ve stejném předplatném nebo můžou být součástí jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="21712-110">Virtual networks can either be in the same subscription or can be part of another subscription.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="21712-111">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="21712-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a><span data-ttu-id="21712-112">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="21712-112">Configuration prerequisites</span></span>
1. <span data-ttu-id="21712-113">Je třeba nejnovější verzi modulů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21712-113">You need the latest version of the Azure PowerShell modules.</span></span> <span data-ttu-id="21712-114">Nejnovější moduly Powershellu si můžete stáhnout z části prostředí PowerShell [Azure stáhne stránky](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="21712-114">You can download the latest PowerShell modules from the PowerShell section of the [Azure Downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="21712-115">Postupujte podle pokynů v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) podrobné pokyny ke konfiguraci počítače pro používání modulů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21712-115">Follow the instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) for step-by-step guidance on how to configure your computer to use the Azure PowerShell modules.</span></span>
2. <span data-ttu-id="21712-116">Je potřeba posoudit [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="21712-116">You need to review the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
3. <span data-ttu-id="21712-117">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="21712-117">You must have an active ExpressRoute circuit.</span></span>
   * <span data-ttu-id="21712-118">Postupujte podle pokynů a [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md) a váš poskytovatel připojení povolte okruh.</span><span class="sxs-lookup"><span data-stu-id="21712-118">Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have your connectivity provider enable the circuit.</span></span>
   * <span data-ttu-id="21712-119">Ujistěte se, abyste měli soukromého partnerského vztahu Azure nakonfigurovaný pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="21712-119">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="21712-120">Najdete v článku [konfigurace směrování](expressroute-howto-routing-classic.md) směrování pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="21712-120">See the [Configure routing](expressroute-howto-routing-classic.md) article for routing instructions.</span></span>
   * <span data-ttu-id="21712-121">Zajistěte, aby soukromý partnerský vztah Azure je nakonfigurován a partnerském vztahu protokolu BGP mezi vaší sítí a Microsoftem zapnutý tak, že povolíte připojení klient server.</span><span class="sxs-lookup"><span data-stu-id="21712-121">Ensure that Azure private peering is configured and the BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
   * <span data-ttu-id="21712-122">Musí mít virtuální sítě a brány virtuální sítě vytvoří a plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="21712-122">You must have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="21712-123">Postupujte podle pokynů a [konfigurace virtuální sítě pro ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="21712-123">Follow the instructions to [configure a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

<span data-ttu-id="21712-124">Můžete se propojit až 10 virtuální sítě k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="21712-124">You can link up to 10 virtual networks to an ExpressRoute circuit.</span></span> <span data-ttu-id="21712-125">Všechny virtuální sítě musí být ve stejné geopolitické oblasti.</span><span class="sxs-lookup"><span data-stu-id="21712-125">All virtual networks must be in the same geopolitical region.</span></span> <span data-ttu-id="21712-126">Můžete propojit větší počet virtuální sítě k okruhu ExpressRoute nebo propojení virtuálních sítí, které jsou v dalších geopolitických oblastí, pokud jste povolili doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="21712-126">You can link a larger number of virtual networks to your ExpressRoute circuit, or link virtual networks that are in other geopolitical regions if you enabled the ExpressRoute premium add-on.</span></span> <span data-ttu-id="21712-127">Zkontrolujte [– nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium.</span><span class="sxs-lookup"><span data-stu-id="21712-127">Check the [FAQ](expressroute-faqs.md) for more details on the premium add-on.</span></span>

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a><span data-ttu-id="21712-128">Připojit k okruhu virtuální sítě v rámci stejného předplatného</span><span class="sxs-lookup"><span data-stu-id="21712-128">Connect a virtual network in the same subscription to a circuit</span></span>
<span data-ttu-id="21712-129">Pomocí následující rutiny můžete propojit virtuální sítě k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="21712-129">You can link a virtual network to an ExpressRoute circuit by using the following cmdlet.</span></span> <span data-ttu-id="21712-130">Ujistěte se, že bránu virtuální sítě se vytvoří a je připravený pro propojování předtím, než spustíte rutinu.</span><span class="sxs-lookup"><span data-stu-id="21712-130">Make sure that the virtual network gateway is created and is ready for linking before you run the cmdlet.</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a><span data-ttu-id="21712-131">Připojení virtuální sítě jiného předplatného k okruhu</span><span class="sxs-lookup"><span data-stu-id="21712-131">Connect a virtual network in a different subscription to a circuit</span></span>
<span data-ttu-id="21712-132">Okruh ExpressRoute můžete sdílet mezi více předplatných.</span><span class="sxs-lookup"><span data-stu-id="21712-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="21712-133">Následující obrázek znázorňuje jednoduchý schéma o tom, jak sdílení funguje pro okruhy ExpressRoute napříč více předplatných.</span><span class="sxs-lookup"><span data-stu-id="21712-133">The following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="21712-134">Každý z menší cloudy v rámci velké cloudu se používá k reprezentování odběry, které patří k různým oblastem v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="21712-134">Each of the smaller clouds within the large cloud is used to represent subscriptions that belong to different departments within an organization.</span></span> <span data-ttu-id="21712-135">Každé oddělení v organizaci můžete použít vlastní předplatné pro nasazení služeb – ale jako vodítko použijte oddělení může sdílet jeden okruh ExpressRoute zpětné připojení k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="21712-135">Each of the departments within the organization can use their own subscription for deploying their services--but the departments can share a single ExpressRoute circuit to connect back to your on-premises network.</span></span> <span data-ttu-id="21712-136">Jednoho oddělení (v tomto příkladu: IT) můžete vlastní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="21712-136">A single department (in this example: IT) can own the ExpressRoute circuit.</span></span> <span data-ttu-id="21712-137">Okruh ExpressRoute můžete použít jiné odběry v rámci organizace.</span><span class="sxs-lookup"><span data-stu-id="21712-137">Other subscriptions within the organization can use the ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="21712-138">Připojení a šířku pásma poplatky za vyhrazené okruh uplatní na vlastníka okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="21712-138">Connectivity and bandwidth charges for the dedicated circuit will be applied to the ExpressRoute circuit owner.</span></span> <span data-ttu-id="21712-139">Všechny virtuální sítě sdílejí stejné šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="21712-139">All virtual networks share the same bandwidth.</span></span>
> 
> 

![Připojení mezi předplatnými](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a><span data-ttu-id="21712-141">Správa</span><span class="sxs-lookup"><span data-stu-id="21712-141">Administration</span></span>
<span data-ttu-id="21712-142">*Vlastníka okruhu* je správce nebo spolusprávce předplatného, ve kterém se vytvoří okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="21712-142">The *circuit owner* is the administrator/coadministrator of the subscription in which the ExpressRoute circuit is created.</span></span> <span data-ttu-id="21712-143">Vlastníka okruhu může autorizovat správci nebo coadministrators další odběry, označuje jako *okruhů uživatelů*, používat vyhrazené okruh, kterou vlastní.</span><span class="sxs-lookup"><span data-stu-id="21712-143">The circuit owner can authorize administrators/coadministrators of other subscriptions, referred to as *circuit users*, to use the dedicated circuit that they own.</span></span> <span data-ttu-id="21712-144">Okruh uživatelů, kteří mají oprávnění využívat organizace okruh ExpressRoute můžete propojit virtuální sítě v rámci svého předplatného okruh ExpressRoute po jejich oprávnění.</span><span class="sxs-lookup"><span data-stu-id="21712-144">Circuit users who are authorized to use the organization's ExpressRoute circuit can link the virtual network in their subscription to the ExpressRoute circuit after they are authorized.</span></span>

<span data-ttu-id="21712-145">Vlastníka okruhu má právo upravit a kdykoli odvolat oprávnění.</span><span class="sxs-lookup"><span data-stu-id="21712-145">The circuit owner has the power to modify and revoke authorizations at any time.</span></span> <span data-ttu-id="21712-146">Odvolání povolení bude výsledkem všechny odkazy odstraňuje z předplatného, jejichž přístup byl odvolán.</span><span class="sxs-lookup"><span data-stu-id="21712-146">Revoking an authorization will result in all links being deleted from the subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="21712-147">Operace vlastníka okruhu</span><span class="sxs-lookup"><span data-stu-id="21712-147">Circuit owner operations</span></span>

<span data-ttu-id="21712-148">**Vytvoření ověření**</span><span class="sxs-lookup"><span data-stu-id="21712-148">**Creating an authorization**</span></span>

<span data-ttu-id="21712-149">Autorizuje vlastníka okruhu správcům další odběry použít určený okruh.</span><span class="sxs-lookup"><span data-stu-id="21712-149">The circuit owner authorizes the administrators of other subscriptions to use the specified circuit.</span></span> <span data-ttu-id="21712-150">V následujícím příkladu správce okruhu (Contoso IT) umožňuje správci jiné předplatné (Dev-Test) propojit až dvě virtuální sítě k okruhu.</span><span class="sxs-lookup"><span data-stu-id="21712-150">In the following example, the administrator of the circuit (Contoso IT) enables the administrator of another subscription (Dev-Test) to link up to two virtual networks to the circuit.</span></span> <span data-ttu-id="21712-151">Správce Contoso IT umožňuje to tak, že zadáte ID Microsoft Dev-Test.</span><span class="sxs-lookup"><span data-stu-id="21712-151">The Contoso IT administrator enables this by specifying the Dev-Test Microsoft ID.</span></span> <span data-ttu-id="21712-152">Rutina neodešle e-mailu zadané ID společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="21712-152">The cmdlet doesn't send email to the specified Microsoft ID.</span></span> <span data-ttu-id="21712-153">Vlastníka okruhu je potřeba explicitně oznámit jiných vlastník předplatného, registrace je dokončena.</span><span class="sxs-lookup"><span data-stu-id="21712-153">The circuit owner needs to explicitly notify the other subscription owner that the authorization is complete.</span></span>

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

<span data-ttu-id="21712-154">**Kontrola povolení**</span><span class="sxs-lookup"><span data-stu-id="21712-154">**Reviewing authorizations**</span></span>

<span data-ttu-id="21712-155">Vlastníka okruhu můžete zkontrolovat všechny autorizací, které jsou vydány na konkrétní okruhu spuštěním následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="21712-155">The circuit owner can review all authorizations that are issued on a particular circuit by running the following cmdlet:</span></span>

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


<span data-ttu-id="21712-156">**Aktualizace oprávnění**</span><span class="sxs-lookup"><span data-stu-id="21712-156">**Updating authorizations**</span></span>

<span data-ttu-id="21712-157">Vlastníka okruhu můžete upravit autorizací pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="21712-157">The circuit owner can modify authorizations by using the following cmdlet:</span></span>

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


<span data-ttu-id="21712-158">**Odstranění autorizací**</span><span class="sxs-lookup"><span data-stu-id="21712-158">**Deleting authorizations**</span></span>

<span data-ttu-id="21712-159">Vlastníka okruhu můžete odvolání nebo odstranění autorizací pro uživatele tak, že spustíte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="21712-159">The circuit owner can revoke/delete authorizations to the user by running the following cmdlet:</span></span>

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a><span data-ttu-id="21712-160">Operace okruh uživatele</span><span class="sxs-lookup"><span data-stu-id="21712-160">Circuit user operations</span></span>

<span data-ttu-id="21712-161">**Kontrola povolení**</span><span class="sxs-lookup"><span data-stu-id="21712-161">**Reviewing authorizations**</span></span>

<span data-ttu-id="21712-162">Uživatele okruhu můžete zkontrolovat autorizací pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="21712-162">The circuit user can review authorizations by using the following cmdlet:</span></span>

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

<span data-ttu-id="21712-163">**Uplatňuje autorizací propojení**</span><span class="sxs-lookup"><span data-stu-id="21712-163">**Redeeming link authorizations**</span></span>

<span data-ttu-id="21712-164">Okruh může spouštět následující rutiny můžete uplatnit odkaz autorizace:</span><span class="sxs-lookup"><span data-stu-id="21712-164">The circuit user can run the following cmdlet to redeem a link authorization:</span></span>

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

<span data-ttu-id="21712-165">Spusťte tento příkaz v nově propojené předplatné virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="21712-165">Run this command in the newly linked subscription for the virtual network:</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a><span data-ttu-id="21712-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21712-166">Next steps</span></span>
<span data-ttu-id="21712-167">Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="21712-167">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

