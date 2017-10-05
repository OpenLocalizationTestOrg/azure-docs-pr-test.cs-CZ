---
title: "Pracovní postupy pro konfiguraci okruh ExpressRoute | Microsoft Docs"
description: "Tato stránka vás provede pracovní postupy pro konfiguraci okruhu ExpressRoute a partnerských vztahů"
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 55e0418c-e0bf-44a7-9aa1-720076df9297
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: cba1b2cfee379e7d2b079bcb3089981ef1044d66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a><span data-ttu-id="9db9a-103">Pracovní postupy ExpressRoute pro zřizování a stavy okruhů</span><span class="sxs-lookup"><span data-stu-id="9db9a-103">ExpressRoute workflows for circuit provisioning and circuit states</span></span>
<span data-ttu-id="9db9a-104">Tato stránka vás provede procesem služby zřizování a pracovní postupy konfigurace směrování na vysoké úrovni.</span><span class="sxs-lookup"><span data-stu-id="9db9a-104">This page walks you through the service provisioning and routing configuration workflows at a high level.</span></span>

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

<span data-ttu-id="9db9a-105">Následující obrázek a příslušné postupy ukazují úlohy, které je třeba provést, aby bylo možné používat okruhu ExpressRoute zřízeného začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="9db9a-105">The following figure and corresponding steps show the tasks you must follow in order to have an ExpressRoute circuit provisioned end-to-end.</span></span> 

1. <span data-ttu-id="9db9a-106">Nakonfigurujte okruh ExpressRoute pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9db9a-106">Use PowerShell to configure an ExpressRoute circuit.</span></span> <span data-ttu-id="9db9a-107">Postupujte podle pokynů [okruhy ExpressRoute vytvořit](expressroute-howto-circuit-classic.md) další podrobnosti najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="9db9a-107">Follow the instructions in the [Create ExpressRoute circuits](expressroute-howto-circuit-classic.md) article for more details.</span></span>
2. <span data-ttu-id="9db9a-108">Pořadí připojení od poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="9db9a-108">Order connectivity from the service provider.</span></span> <span data-ttu-id="9db9a-109">Tento proces se liší.</span><span class="sxs-lookup"><span data-stu-id="9db9a-109">This process varies.</span></span> <span data-ttu-id="9db9a-110">Další informace o tom, jak pořadí připojení, obraťte se na svého poskytovatele připojení.</span><span class="sxs-lookup"><span data-stu-id="9db9a-110">Contact your connectivity provider for more details about how to order connectivity.</span></span>
3. <span data-ttu-id="9db9a-111">Ujistěte se, že je okruh má úspěšně zřízený kontrolou stavu pomocí prostředí PowerShell zřizování okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9db9a-111">Ensure that the circuit has been provisioned successfully by verifying the ExpressRoute circuit provisioning state through PowerShell.</span></span> 
4. <span data-ttu-id="9db9a-112">Konfigurace domény směrování.</span><span class="sxs-lookup"><span data-stu-id="9db9a-112">Configure routing domains.</span></span> <span data-ttu-id="9db9a-113">Pokud poskytovatel připojení vrstvy 3 spravuje za vás, nakonfiguruje směrování pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="9db9a-113">If your connectivity provider manages Layer 3 for you, they will configure routing for your circuit.</span></span> <span data-ttu-id="9db9a-114">Pokud poskytovatel připojení nabízí pouze služby vrstvy 2, je nutné nakonfigurovat směrování podle pokynů popsaných v [požadavky na směrování](expressroute-routing.md) a [konfigurace směrování](expressroute-howto-routing-classic.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="9db9a-114">If your connectivity provider only offers Layer 2 services, you must configure routing per guidelines described in the [routing requirements](expressroute-routing.md) and [routing configuration](expressroute-howto-routing-classic.md) pages.</span></span>
   
   * <span data-ttu-id="9db9a-115">Povolení soukromého partnerského vztahu Azure – je nutné povolit tohoto partnerského vztahu se připojit k virtuálním počítačům nebo cloudových služeb nasadit v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="9db9a-115">Enable Azure private peering - You must enable this peering to connect to VMs / cloud services deployed within virtual networks.</span></span>
   * <span data-ttu-id="9db9a-116">Povolit veřejný partnerský vztah Azure – je nutné povolit veřejný partnerský vztah Azure Pokud se chcete připojit ke službám Azure, které jsou hostované na veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9db9a-116">Enable Azure public peering - You must enable Azure public peering if you wish to connect to Azure services hosted on public IP addresses.</span></span> <span data-ttu-id="9db9a-117">Toto je požadavek pro přístup k prostředkům Azure, pokud jste vybrali k povolení výchozí směrování pro soukromý partnerský vztah Azure.</span><span class="sxs-lookup"><span data-stu-id="9db9a-117">This is a requirement to access Azure resources if you have chosen to enable default routing for Azure private peering.</span></span>
   * <span data-ttu-id="9db9a-118">Povolit partnerského vztahu Microsoftu – to je nutné povolit přístup k Office 365 a Dynamics 365.</span><span class="sxs-lookup"><span data-stu-id="9db9a-118">Enable Microsoft peering - You must enable this to access Office 365 and Dynamics 365.</span></span> 
     
     > [!IMPORTANT]
     > <span data-ttu-id="9db9a-119">Ujistěte se, že používáte samostatné proxy server / edge připojovali k serverům Microsoftu než ten, který můžete použít pro Internet.</span><span class="sxs-lookup"><span data-stu-id="9db9a-119">You must ensure that you use a separate proxy / edge to connect to Microsoft than the one you use for the Internet.</span></span> <span data-ttu-id="9db9a-120">Pomocí stejné hraniční pro ExpressRoute a Internetem se způsobit asymetrické směrování a připojení k výpadkům ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="9db9a-120">Using the same edge for both ExpressRoute and the Internet will cause asymmetric routing and cause connectivity outages for your network.</span></span>
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. <span data-ttu-id="9db9a-121">Propojení virtuální sítě pro okruhy ExpressRoute - můžete propojit virtuální sítě k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9db9a-121">Linking virtual networks to ExpressRoute circuits - You can link virtual networks to your ExpressRoute circuit.</span></span> <span data-ttu-id="9db9a-122">Postupujte podle pokynů [propojení virtuálních sítí](expressroute-howto-linkvnet-arm.md) pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="9db9a-122">Follow instructions [to link VNets](expressroute-howto-linkvnet-arm.md) to your circuit.</span></span> <span data-ttu-id="9db9a-123">Tyto virtuální sítě může být v rámci stejného předplatného Azure jako okruh ExpressRoute, nebo může být v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="9db9a-123">These VNets can either be in the same Azure subscription as the ExpressRoute circuit, or can be in a different subscription.</span></span>

## <a name="expressroute-circuit-provisioning-states"></a><span data-ttu-id="9db9a-124">Stavy zřizování okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="9db9a-124">ExpressRoute circuit provisioning states</span></span>
<span data-ttu-id="9db9a-125">Každý okruh ExpressRoute má dva stavy:</span><span class="sxs-lookup"><span data-stu-id="9db9a-125">Each ExpressRoute circuit has two states:</span></span>

* <span data-ttu-id="9db9a-126">Stav zřizování poskytovatele služby</span><span class="sxs-lookup"><span data-stu-id="9db9a-126">Service provider provisioning state</span></span>
* <span data-ttu-id="9db9a-127">Status</span><span class="sxs-lookup"><span data-stu-id="9db9a-127">Status</span></span>

<span data-ttu-id="9db9a-128">Stav představuje stav zřizování společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9db9a-128">Status represents Microsoft's provisioning state.</span></span> <span data-ttu-id="9db9a-129">Tato vlastnost nastavena na hodnotu povoleno vytvoření okruhu Expressroute</span><span class="sxs-lookup"><span data-stu-id="9db9a-129">This property is set to Enabled when you create an Expressroute circuit</span></span>

<span data-ttu-id="9db9a-130">Stav zřizování poskytovatele připojení představuje stav na straně poskytovatele připojení.</span><span class="sxs-lookup"><span data-stu-id="9db9a-130">The connectivity provider provisioning state represents the state on the connectivity provider's side.</span></span> <span data-ttu-id="9db9a-131">Může to být *NotProvisioned*, *zřizování*, nebo *zajištěno*.</span><span class="sxs-lookup"><span data-stu-id="9db9a-131">It can either be *NotProvisioned*, *Provisioning*, or *Provisioned*.</span></span> <span data-ttu-id="9db9a-132">Okruh ExpressRoute musí být ve stavu zajištěno, abyste mohli používat ho.</span><span class="sxs-lookup"><span data-stu-id="9db9a-132">The ExpressRoute circuit must be in Provisioned state for you to be able to use it.</span></span>

### <a name="possible-states-of-an-expressroute-circuit"></a><span data-ttu-id="9db9a-133">Možné stavy okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="9db9a-133">Possible states of an ExpressRoute circuit</span></span>
<span data-ttu-id="9db9a-134">Tato část uvádí se možné stavy pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9db9a-134">This section lists out the possible states for an ExpressRoute circuit.</span></span>

<span data-ttu-id="9db9a-135">**V okamžiku vytvoření**</span><span class="sxs-lookup"><span data-stu-id="9db9a-135">**At creation time**</span></span>

<span data-ttu-id="9db9a-136">Okruh ExpressRoute v následujícím stavu se zobrazí při spuštění rutiny prostředí PowerShell k vytvoření okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9db9a-136">You will see the ExpressRoute circuit in the following state as soon as you run the PowerShell cmdlet to create the ExpressRoute circuit.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="9db9a-137">**Pokud probíhá zřizování okruhu poskytovatele připojení**</span><span class="sxs-lookup"><span data-stu-id="9db9a-137">**When connectivity provider is in the process of provisioning the circuit**</span></span>

<span data-ttu-id="9db9a-138">Okruh ExpressRoute v následujícím stavu se zobrazí, jakmile je předat klíč služby do poskytovatele připojení a zahájil proces zřizování.</span><span class="sxs-lookup"><span data-stu-id="9db9a-138">You will see the ExpressRoute circuit in the following state as soon as you pass the service key to the connectivity provider and they have started the provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


<span data-ttu-id="9db9a-139">**Po dokončení procesu zřizování poskytovatele připojení**</span><span class="sxs-lookup"><span data-stu-id="9db9a-139">**When connectivity provider has completed the provisioning process**</span></span>

<span data-ttu-id="9db9a-140">Okruh ExpressRoute v následujícím stavu se zobrazí při dokončení procesu zřizování poskytovatele připojení.</span><span class="sxs-lookup"><span data-stu-id="9db9a-140">You will see the ExpressRoute circuit in the following state as soon as the connectivity provider has completed the provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

<span data-ttu-id="9db9a-141">Zřízený a je povoleno pouze stav okruhu může být v, abyste mohli používat ho.</span><span class="sxs-lookup"><span data-stu-id="9db9a-141">Provisioned and Enabled is the only state the circuit can be in for you to be able to use it.</span></span> <span data-ttu-id="9db9a-142">Pokud používáte poskytovatele vrstvy 2, můžete nakonfigurovat směrování pro váš okruh, pouze pokud je v tomto stavu.</span><span class="sxs-lookup"><span data-stu-id="9db9a-142">If you are using a Layer 2 provider, you can configure routing for your circuit only when it is in this state.</span></span>

<span data-ttu-id="9db9a-143">**Když je poskytovatel připojení zrušení zřízení okruhu**</span><span class="sxs-lookup"><span data-stu-id="9db9a-143">**When connectivity provider is deprovisioning the circuit**</span></span>

<span data-ttu-id="9db9a-144">Pokud jste požádali o poskytovateli služby zrušení zřízení okruh ExpressRoute, zobrazí se okruh nastavit následující stav po dokončení proces zrušení zřízení poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="9db9a-144">If you requested the service provider to deprovision the ExpressRoute circuit, you will see the circuit set to the following state after the service provider has completed the deprovisioning process.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="9db9a-145">Můžete znovu ji povolit, pokud je potřeba, nebo spusťte rutiny prostředí PowerShell odstranit okruh.</span><span class="sxs-lookup"><span data-stu-id="9db9a-145">You can choose to re-enable it if needed, or run PowerShell cmdlets to delete the circuit.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="9db9a-146">Pokud spustíte rutinu prostředí PowerShell odstranit okruh, když je zřizování ServiceProviderProvisioningState nebo zajištěno se operace nezdaří.</span><span class="sxs-lookup"><span data-stu-id="9db9a-146">If you run the PowerShell cmdlet to delete the circuit when the ServiceProviderProvisioningState is Provisioning or Provisioned the operation will fail.</span></span> <span data-ttu-id="9db9a-147">Spojte se s poskytovatelem připojení nejprve zrušit jejich zřízení okruh ExpressRoute a pak odstraňte okruh.</span><span class="sxs-lookup"><span data-stu-id="9db9a-147">Please work with your connectivity provider to deprovision the ExpressRoute circuit first and then delete the circuit.</span></span> <span data-ttu-id="9db9a-148">Microsoft bude účtovat pošle okruh, dokud nespustíte rutiny prostředí PowerShell odstranit okruh.</span><span class="sxs-lookup"><span data-stu-id="9db9a-148">Microsoft will continue to bill the circuit until you run the PowerShell cmdlet to delete the circuit.</span></span>
> 
> 

## <a name="routing-session-configuration-state"></a><span data-ttu-id="9db9a-149">Stav konfigurace směrování relace</span><span class="sxs-lookup"><span data-stu-id="9db9a-149">Routing session configuration state</span></span>
<span data-ttu-id="9db9a-150">Protokol BGP Stav zřizování umožňuje zjistit, zda byl povolen relaci protokolu BGP v Microsoft edge.</span><span class="sxs-lookup"><span data-stu-id="9db9a-150">The BGP provisioning state lets you know if the BGP session has been enabled on the Microsoft edge.</span></span> <span data-ttu-id="9db9a-151">Abyste mohli používat partnerského vztahu musí být povolen stav.</span><span class="sxs-lookup"><span data-stu-id="9db9a-151">The state must be enabled for you to be able to use the peering.</span></span>

<span data-ttu-id="9db9a-152">Je důležité zkontrolovat stav relace protokolu BGP hlavně pro partnerský vztah Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="9db9a-152">It is important to check the BGP session state especially for Microsoft peering.</span></span> <span data-ttu-id="9db9a-153">Kromě BGP stavu zřizování, je jiný stav názvem *inzerované předpony veřejných stavu*.</span><span class="sxs-lookup"><span data-stu-id="9db9a-153">In addition to the BGP provisioning state, there is another state called *advertised public prefixes state*.</span></span> <span data-ttu-id="9db9a-154">Inzerované předpony veřejných stavu musí být v *nakonfigurované* stavu, jak pro relaci protokolu BGP jako nahoru a směrování fungovat začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="9db9a-154">The advertised public prefixes state must be in *configured* state, both for the BGP session to be up and for your routing to work end-to-end.</span></span> 

<span data-ttu-id="9db9a-155">Pokud je nastavena do stavu inzerovaný předponu veřejné *ověření potřeby* stavu relaci protokolu BGP není povolen, protože inzerované předpony neodpovídá číslo AS v některém z registrech směrování.</span><span class="sxs-lookup"><span data-stu-id="9db9a-155">If the advertised public prefix state is set to a *validation needed* state, the BGP session is not enabled, as the advertised prefixes did not match the AS number in any of the routing registries.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="9db9a-156">Pokud je stav ohlášeného předpony veřejných v *ruční ověřování* stavu, musíte otevřít lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a prokázání, že vlastníte inzerované IP adresy spolu s přidružená čísla autonomního systému.</span><span class="sxs-lookup"><span data-stu-id="9db9a-156">If the advertised public prefixes state is in *manual validation* state, you must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and provide evidence that you own the IP addresses advertised along with the associated Autonomous System number.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="9db9a-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9db9a-157">Next steps</span></span>
* <span data-ttu-id="9db9a-158">Nakonfigurujte připojení ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="9db9a-158">Configure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="9db9a-159">Vytvoření okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="9db9a-159">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-arm.md)
  * [<span data-ttu-id="9db9a-160">Konfigurace směrování</span><span class="sxs-lookup"><span data-stu-id="9db9a-160">Configure routing</span></span>](expressroute-howto-routing-arm.md)
  * [<span data-ttu-id="9db9a-161">Propojení virtuální sítě s okruhem ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="9db9a-161">Link a VNet to an ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

