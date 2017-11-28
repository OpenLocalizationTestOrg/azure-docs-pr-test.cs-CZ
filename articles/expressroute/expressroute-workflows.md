---
title: aaaWorkflows pro konfiguraci okruh ExpressRoute | Microsoft Docs
description: "Tato stránka vás provede hello pracovní postupy pro konfiguraci okruhu ExpressRoute a partnerských vztahů"
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
ms.openlocfilehash: 8e1dfc137401e0d6d53608ae6c8de0085e182eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a><span data-ttu-id="b454b-103">Pracovní postupy ExpressRoute pro zřizování a stavy okruhů</span><span class="sxs-lookup"><span data-stu-id="b454b-103">ExpressRoute workflows for circuit provisioning and circuit states</span></span>
<span data-ttu-id="b454b-104">Tato stránka vás provede procesem hello zřizování služby a pracovní postupy směrování configuration na vysoké úrovni.</span><span class="sxs-lookup"><span data-stu-id="b454b-104">This page walks you through hello service provisioning and routing configuration workflows at a high level.</span></span>

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

<span data-ttu-id="b454b-105">Hello následující obrázek a příslušné postupy ukazují hello úlohy, že je třeba provést v pořadí toohave ExpressRoute okruh zřízený end až do konce.</span><span class="sxs-lookup"><span data-stu-id="b454b-105">hello following figure and corresponding steps show hello tasks you must follow in order toohave an ExpressRoute circuit provisioned end-to-end.</span></span> 

1. <span data-ttu-id="b454b-106">Pomocí prostředí PowerShell tooconfigure okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b454b-106">Use PowerShell tooconfigure an ExpressRoute circuit.</span></span> <span data-ttu-id="b454b-107">Postupujte podle pokynů hello v hello [okruhy ExpressRoute vytvořit](expressroute-howto-circuit-classic.md) další podrobnosti najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="b454b-107">Follow hello instructions in hello [Create ExpressRoute circuits](expressroute-howto-circuit-classic.md) article for more details.</span></span>
2. <span data-ttu-id="b454b-108">Pořadí připojení od poskytovatele služeb hello.</span><span class="sxs-lookup"><span data-stu-id="b454b-108">Order connectivity from hello service provider.</span></span> <span data-ttu-id="b454b-109">Tento proces se liší.</span><span class="sxs-lookup"><span data-stu-id="b454b-109">This process varies.</span></span> <span data-ttu-id="b454b-110">Další podrobnosti o tom, obraťte se na svého poskytovatele připojení tooorder připojení.</span><span class="sxs-lookup"><span data-stu-id="b454b-110">Contact your connectivity provider for more details about how tooorder connectivity.</span></span>
3. <span data-ttu-id="b454b-111">Ujistěte se, že hello okruh má úspěšně zřízený kontrolou okruh ExpressRoute hello zřizování stavu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b454b-111">Ensure that hello circuit has been provisioned successfully by verifying hello ExpressRoute circuit provisioning state through PowerShell.</span></span> 
4. <span data-ttu-id="b454b-112">Konfigurace domény směrování.</span><span class="sxs-lookup"><span data-stu-id="b454b-112">Configure routing domains.</span></span> <span data-ttu-id="b454b-113">Pokud poskytovatel připojení vrstvy 3 spravuje za vás, nakonfiguruje směrování pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="b454b-113">If your connectivity provider manages Layer 3 for you, they will configure routing for your circuit.</span></span> <span data-ttu-id="b454b-114">Pokud poskytovatel připojení nabízí pouze služby vrstvy 2, je nutné nakonfigurovat směrování podle pokynů popsaných v hello [požadavky na směrování](expressroute-routing.md) a [konfigurace směrování](expressroute-howto-routing-classic.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="b454b-114">If your connectivity provider only offers Layer 2 services, you must configure routing per guidelines described in hello [routing requirements](expressroute-routing.md) and [routing configuration](expressroute-howto-routing-classic.md) pages.</span></span>
   
   * <span data-ttu-id="b454b-115">Povolení soukromého partnerského vztahu Azure – musí povolit tohoto partnerského vztahu tooVMs tooconnect / cloudových služeb nasadit v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b454b-115">Enable Azure private peering - You must enable this peering tooconnect tooVMs / cloud services deployed within virtual networks.</span></span>
   * <span data-ttu-id="b454b-116">Povolit veřejný partnerský vztah Azure – je nutné povolit veřejný partnerský vztah Azure chcete tooconnect tooAzure služeb hostovaných na veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b454b-116">Enable Azure public peering - You must enable Azure public peering if you wish tooconnect tooAzure services hosted on public IP addresses.</span></span> <span data-ttu-id="b454b-117">Toto je požadavek tooaccess prostředky Azure, pokud jste vybrali tooenable výchozí směrování pro soukromý partnerský vztah Azure.</span><span class="sxs-lookup"><span data-stu-id="b454b-117">This is a requirement tooaccess Azure resources if you have chosen tooenable default routing for Azure private peering.</span></span>
   * <span data-ttu-id="b454b-118">Povolit partnerského vztahu Microsoftu - je nutné povolit tuto tooaccess Office 365 a Dynamics 365.</span><span class="sxs-lookup"><span data-stu-id="b454b-118">Enable Microsoft peering - You must enable this tooaccess Office 365 and Dynamics 365.</span></span> 
     
     > [!IMPORTANT]
     > <span data-ttu-id="b454b-119">Ujistěte se, že používáte samostatné proxy server / edge tooconnect tooMicrosoft než text hello, který používáte pro hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="b454b-119">You must ensure that you use a separate proxy / edge tooconnect tooMicrosoft than hello one you use for hello Internet.</span></span> <span data-ttu-id="b454b-120">Pomocí hello stejné hraniční pro ExpressRoute a hello Internetu bude způsobit asymetrické směrování a způsobit výpadky připojení pro vaši síť.</span><span class="sxs-lookup"><span data-stu-id="b454b-120">Using hello same edge for both ExpressRoute and hello Internet will cause asymmetric routing and cause connectivity outages for your network.</span></span>
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. <span data-ttu-id="b454b-121">Propojení virtuální sítě okruhů tooExpressRoute – můžete propojit okruh ExpressRoute tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b454b-121">Linking virtual networks tooExpressRoute circuits - You can link virtual networks tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="b454b-122">Postupujte podle pokynů [toolink virtuálních sítí](expressroute-howto-linkvnet-arm.md) tooyour okruh.</span><span class="sxs-lookup"><span data-stu-id="b454b-122">Follow instructions [toolink VNets](expressroute-howto-linkvnet-arm.md) tooyour circuit.</span></span> <span data-ttu-id="b454b-123">Tyto virtuální sítě může být buď v hello stejné předplatné jako hello okruh ExpressRoute, nebo může být v jiném předplatném.</span><span class="sxs-lookup"><span data-stu-id="b454b-123">These VNets can either be in hello same Azure subscription as hello ExpressRoute circuit, or can be in a different subscription.</span></span>

## <a name="expressroute-circuit-provisioning-states"></a><span data-ttu-id="b454b-124">Stavy zřizování okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="b454b-124">ExpressRoute circuit provisioning states</span></span>
<span data-ttu-id="b454b-125">Každý okruh ExpressRoute má dva stavy:</span><span class="sxs-lookup"><span data-stu-id="b454b-125">Each ExpressRoute circuit has two states:</span></span>

* <span data-ttu-id="b454b-126">Stav zřizování poskytovatele služby</span><span class="sxs-lookup"><span data-stu-id="b454b-126">Service provider provisioning state</span></span>
* <span data-ttu-id="b454b-127">Status</span><span class="sxs-lookup"><span data-stu-id="b454b-127">Status</span></span>

<span data-ttu-id="b454b-128">Stav představuje stav zřizování společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b454b-128">Status represents Microsoft's provisioning state.</span></span> <span data-ttu-id="b454b-129">Tato vlastnost nastavena tooEnabled při vytvoření okruhu Expressroute</span><span class="sxs-lookup"><span data-stu-id="b454b-129">This property is set tooEnabled when you create an Expressroute circuit</span></span>

<span data-ttu-id="b454b-130">Stav zřizování zprostředkovatele připojení k Hello představuje hello stav na straně poskytovatele připojení hello.</span><span class="sxs-lookup"><span data-stu-id="b454b-130">hello connectivity provider provisioning state represents hello state on hello connectivity provider's side.</span></span> <span data-ttu-id="b454b-131">Může to být *NotProvisioned*, *zřizování*, nebo *zajištěno*.</span><span class="sxs-lookup"><span data-stu-id="b454b-131">It can either be *NotProvisioned*, *Provisioning*, or *Provisioned*.</span></span> <span data-ttu-id="b454b-132">Hello okruh ExpressRoute musí být v zajištěno stavu můžete toobe možné toouse ho.</span><span class="sxs-lookup"><span data-stu-id="b454b-132">hello ExpressRoute circuit must be in Provisioned state for you toobe able toouse it.</span></span>

### <a name="possible-states-of-an-expressroute-circuit"></a><span data-ttu-id="b454b-133">Možné stavy okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="b454b-133">Possible states of an ExpressRoute circuit</span></span>
<span data-ttu-id="b454b-134">Tato část uvádí out hello možné stavy pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b454b-134">This section lists out hello possible states for an ExpressRoute circuit.</span></span>

<span data-ttu-id="b454b-135">**V okamžiku vytvoření**</span><span class="sxs-lookup"><span data-stu-id="b454b-135">**At creation time**</span></span>

<span data-ttu-id="b454b-136">Zobrazí se hello okruh ExpressRoute v hello následující stavu co nejdříve můžete spustit rutiny prostředí PowerShell text hello, toocreate okruh ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="b454b-136">You will see hello ExpressRoute circuit in hello following state as soon as you run hello PowerShell cmdlet toocreate hello ExpressRoute circuit.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="b454b-137">**Pokud poskytovatel připojení je v hello proces zřizování okruh hello**</span><span class="sxs-lookup"><span data-stu-id="b454b-137">**When connectivity provider is in hello process of provisioning hello circuit**</span></span>

<span data-ttu-id="b454b-138">Zobrazí se hello okruh ExpressRoute v hello následující stavu co nejdříve můžete předat poskytovatele připojení klíče toohello hello služby a zahájil hello procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="b454b-138">You will see hello ExpressRoute circuit in hello following state as soon as you pass hello service key toohello connectivity provider and they have started hello provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


<span data-ttu-id="b454b-139">**Po dokončení procesu zřizování hello poskytovatele připojení**</span><span class="sxs-lookup"><span data-stu-id="b454b-139">**When connectivity provider has completed hello provisioning process**</span></span>

<span data-ttu-id="b454b-140">Zobrazí se hello okruh ExpressRoute v hello následující stavu, jakmile poskytovatel připojení hello dokončil hello procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="b454b-140">You will see hello ExpressRoute circuit in hello following state as soon as hello connectivity provider has completed hello provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

<span data-ttu-id="b454b-141">Zřízený a povolený je hello stavu hello okruh může být pouze v pro jste toobe možné toouse ho.</span><span class="sxs-lookup"><span data-stu-id="b454b-141">Provisioned and Enabled is hello only state hello circuit can be in for you toobe able toouse it.</span></span> <span data-ttu-id="b454b-142">Pokud používáte poskytovatele vrstvy 2, můžete nakonfigurovat směrování pro váš okruh, pouze pokud je v tomto stavu.</span><span class="sxs-lookup"><span data-stu-id="b454b-142">If you are using a Layer 2 provider, you can configure routing for your circuit only when it is in this state.</span></span>

<span data-ttu-id="b454b-143">**Když je poskytovatel připojení zrušení zřízení okruhu hello**</span><span class="sxs-lookup"><span data-stu-id="b454b-143">**When connectivity provider is deprovisioning hello circuit**</span></span>

<span data-ttu-id="b454b-144">Pokud jste si vyžádali hello služby zprostředkovatele toodeprovision hello okruh ExpressRoute, zobrazí se hello okruh nastavit toohello následující stav po dokončení procesu zrušení zřízení hello hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="b454b-144">If you requested hello service provider toodeprovision hello ExpressRoute circuit, you will see hello circuit set toohello following state after hello service provider has completed hello deprovisioning process.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="b454b-145">Můžete povolit toore je-li potřeba nebo se spustit rutiny prostředí PowerShell toodelete hello okruh.</span><span class="sxs-lookup"><span data-stu-id="b454b-145">You can choose toore-enable it if needed, or run PowerShell cmdlets toodelete hello circuit.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="b454b-146">Pokud spustíte hello prostředí PowerShell rutinu toodelete hello okruh hello ServiceProviderProvisioningState při zřizování nebo zajištěno hello operace selže.</span><span class="sxs-lookup"><span data-stu-id="b454b-146">If you run hello PowerShell cmdlet toodelete hello circuit when hello ServiceProviderProvisioningState is Provisioning or Provisioned hello operation will fail.</span></span> <span data-ttu-id="b454b-147">Spojte se s okruh ExpressRoute toodeprovision hello vaše připojení poskytovatele nejprve a pak odstraňte hello okruh.</span><span class="sxs-lookup"><span data-stu-id="b454b-147">Please work with your connectivity provider toodeprovision hello ExpressRoute circuit first and then delete hello circuit.</span></span> <span data-ttu-id="b454b-148">Microsoft bude pokračovat toobill hello okruh, dokud nespustíte hello okruh hello toodelete rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b454b-148">Microsoft will continue toobill hello circuit until you run hello PowerShell cmdlet toodelete hello circuit.</span></span>
> 
> 

## <a name="routing-session-configuration-state"></a><span data-ttu-id="b454b-149">Stav konfigurace směrování relace</span><span class="sxs-lookup"><span data-stu-id="b454b-149">Routing session configuration state</span></span>
<span data-ttu-id="b454b-150">Hello Stav zřizování BGP umožňuje zjistit, zda byla relace protokolu BGP hello zapnuta hello Microsoft edge.</span><span class="sxs-lookup"><span data-stu-id="b454b-150">hello BGP provisioning state lets you know if hello BGP session has been enabled on hello Microsoft edge.</span></span> <span data-ttu-id="b454b-151">Hello stav musí být povolen pro vás hello toobe možné toouse partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="b454b-151">hello state must be enabled for you toobe able toouse hello peering.</span></span>

<span data-ttu-id="b454b-152">Je stav relace protokolu BGP důležité toocheck hello hlavně pro partnerský vztah Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="b454b-152">It is important toocheck hello BGP session state especially for Microsoft peering.</span></span> <span data-ttu-id="b454b-153">V přidání toohello BGP zřizování stavu, je jiný stav názvem *inzerované předpony veřejných stavu*.</span><span class="sxs-lookup"><span data-stu-id="b454b-153">In addition toohello BGP provisioning state, there is another state called *advertised public prefixes state*.</span></span> <span data-ttu-id="b454b-154">Hello inzerované předpony veřejných stavu musí být v *nakonfigurované* stavu, jak pro toobe relace protokolu BGP hello nahoru a vaše směrování toowork začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="b454b-154">hello advertised public prefixes state must be in *configured* state, both for hello BGP session toobe up and for your routing toowork end-to-end.</span></span> 

<span data-ttu-id="b454b-155">Pokud hello inzerované předponu veřejné stav je nastavený tooa *ověření potřeby* stavu relace protokolu BGP hello není povolena, protože hello inzerované předpony neodpovídá hello jako číslo v některém z registrech směrování hello.</span><span class="sxs-lookup"><span data-stu-id="b454b-155">If hello advertised public prefix state is set tooa *validation needed* state, hello BGP session is not enabled, as hello advertised prefixes did not match hello AS number in any of hello routing registries.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b454b-156">Pokud hello inzerované předpony veřejných stavu je v *ruční ověřování* stavu, musíte otevřít lístek podpory s [podporu společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a prokázání, že vlastníte hello IP adresy inzerované podél s hello související číslo autonomního systému.</span><span class="sxs-lookup"><span data-stu-id="b454b-156">If hello advertised public prefixes state is in *manual validation* state, you must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and provide evidence that you own hello IP addresses advertised along with hello associated Autonomous System number.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="b454b-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b454b-157">Next steps</span></span>
* <span data-ttu-id="b454b-158">Nakonfigurujte připojení ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b454b-158">Configure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="b454b-159">Vytvoření okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="b454b-159">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-arm.md)
  * [<span data-ttu-id="b454b-160">Konfigurace směrování</span><span class="sxs-lookup"><span data-stu-id="b454b-160">Configure routing</span></span>](expressroute-howto-routing-arm.md)
  * [<span data-ttu-id="b454b-161">Propojení virtuální sítě tooan okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="b454b-161">Link a VNet tooan ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

