---
title: "Jak tooconfigure směrování pro okruh ExpressRoute (partnerský vztah): Azure: classic | Microsoft Docs"
description: "Tento článek vás provede kroky hello pro vytváření a zřizování hello soukromého a veřejného a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak stav hello toocheck, aktualizace nebo odstranění partnerských vztahů pro váš okruh."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: a4bd39d2-373a-467a-8b06-36cfcc1027d2
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: dc5bcc4b86c3bc965a88281b6c2a660f3bc58eb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a><span data-ttu-id="e97b1-104">Vytvářet a upravovat partnerský vztah pro okruh ExpressRoute (klasické)</span><span class="sxs-lookup"><span data-stu-id="e97b1-104">Create and modify peering for an ExpressRoute circuit (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e97b1-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e97b1-105">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="e97b1-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e97b1-106">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="e97b1-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e97b1-107">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="e97b1-108">Video - soukromého partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="e97b1-108">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="e97b1-109">Video - veřejného partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="e97b1-109">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="e97b1-110">Video - partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="e97b1-110">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="e97b1-111">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="e97b1-111">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

<span data-ttu-id="e97b1-112">Tento článek vás provede kroky toocreate hello a správě konfigurace směrování pro okruhu ExpressRoute pomocí prostředí PowerShell a hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="e97b1-112">This article walks you through hello steps toocreate and manage routing configuration for an ExpressRoute circuit using PowerShell and hello classic deployment model.</span></span> <span data-ttu-id="e97b1-113">Hello kroky se také zobrazí jak toocheck hello stav, aktualizovat, nebo odstranit a zrušit jejich zřízení partnerských vztahů pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="e97b1-113">hello steps below will also show you how toocheck hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="e97b1-114">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="e97b1-114">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a><span data-ttu-id="e97b1-115">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="e97b1-115">Configuration prerequisites</span></span>
* <span data-ttu-id="e97b1-116">Budete potřebovat nejnovější verzi rutin prostředí PowerShell Azure Service Management (SM) hello hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-116">You will need hello latest version of hello Azure Service Management (SM) PowerShell cmdlets.</span></span> <span data-ttu-id="e97b1-117">Další informace najdete v tématu [Začínáme s rutinami prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e97b1-117">For more information, see [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>  
* <span data-ttu-id="e97b1-118">Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md) stránce hello [požadavky na směrování](expressroute-routing.md) stránku a hello [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e97b1-118">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) page, hello [routing requirements](expressroute-routing.md) page, and hello [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="e97b1-119">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="e97b1-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="e97b1-120">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e97b1-120">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="e97b1-121">Hello okruh ExpressRoute musí být ve stavu zřízený a povolený pro vás toobe možné toorun hello rutiny popsané dál.</span><span class="sxs-lookup"><span data-stu-id="e97b1-121">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello cmdlets described below.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e97b1-122">Tyto pokyny platí pouze toocircuits vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="e97b1-122">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="e97b1-123">Pokud používáte poskytovatele služeb nabízejícího spravované služby vrstvy 3 (obvykle IPVPN, např. MPLS), poskytovatel připojení provede konfiguraci a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="e97b1-123">If you are using a service provider offering managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

<span data-ttu-id="e97b1-124">Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="e97b1-124">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="e97b1-125">Partnerské vztahy můžete konfigurovat v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="e97b1-125">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="e97b1-126">Přesvědčte se, že dokončení hello konfiguraci každého partnerského vztahu jeden po druhém.</span><span class="sxs-lookup"><span data-stu-id="e97b1-126">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span>


### <a name="log-in-tooyour-azure-account-and-select-a-subscription"></a><span data-ttu-id="e97b1-127">Přihlaste se tooyour účet Azure a vybrat odběr</span><span class="sxs-lookup"><span data-stu-id="e97b1-127">Log in tooyour Azure account and select a subscription</span></span>
1. <span data-ttu-id="e97b1-128">Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="e97b1-128">Open your PowerShell console with elevated rights and connect tooyour account.</span></span> <span data-ttu-id="e97b1-129">Použijte následující příklad toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="e97b1-129">Use hello following example toohelp you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="e97b1-130">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-130">Check hello subscriptions for hello account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="e97b1-131">Pokud máte více než jedno předplatné, vyberte hello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="e97b1-131">If you have more than one subscription, select hello subscription that you want toouse.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="e97b1-132">Pak pomocí následující rutiny tooadd hello tooPowerShell vaše předplatné Azure pro model nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-132">Next, use hello following cmdlet tooadd your Azure subscription tooPowerShell for hello classic deployment model.</span></span>

        Add-AzureAccount


## <a name="azure-private-peering"></a><span data-ttu-id="e97b1-133">Soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="e97b1-133">Azure private peering</span></span>
<span data-ttu-id="e97b1-134">Tato část obsahuje pokyny jak toocreate, získat, aktualizovat a odstranit hello privátní konfigurace partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="e97b1-134">This section provides instructions on how toocreate, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span> 

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="e97b1-135">toocreate soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="e97b1-135">toocreate Azure private peering</span></span>
1. <span data-ttu-id="e97b1-136">**Importujte modul PowerShell hello pro ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-136">**Import hello PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="e97b1-137">Je nutné naimportovat moduly Azure a ExpressRoute hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-137">You must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="e97b1-138">Hello spusťte následující příkazy tooimport hello Azure a ExpressRoute modulů do relace prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-138">Run hello following commands tooimport hello Azure and ExpressRoute modules into hello PowerShell session.</span></span>  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="e97b1-139">**Vytvoření okruhu ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-139">**Create an ExpressRoute circuit.**</span></span>
   
    <span data-ttu-id="e97b1-140">Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho zřízený poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-140">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by hello connectivity provider.</span></span> <span data-ttu-id="e97b1-141">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás.</span><span class="sxs-lookup"><span data-stu-id="e97b1-141">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="e97b1-142">V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-142">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="e97b1-143">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-143">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow hello instructions below.</span></span> 
3. <span data-ttu-id="e97b1-144">**Zkontrolujte tooensure okruh ExpressRoute hello, které je zřízený.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-144">**Check hello ExpressRoute circuit tooensure it is provisioned.**</span></span>
   
    <span data-ttu-id="e97b1-145">Nejdřív musíte zkontrolovat toosee Pokud hello okruh ExpressRoute je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="e97b1-145">You must first check toosee if hello ExpressRoute circuit is Provisioned and also Enabled.</span></span> <span data-ttu-id="e97b1-146">Viz následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-146">See hello example below.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="e97b1-147">Ujistěte se, že hello okruhu zobrazuje jako zajištěno a povoleno.</span><span class="sxs-lookup"><span data-stu-id="e97b1-147">Make sure that hello circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="e97b1-148">Pokud tomu tak není, pracujete s vaší tooget zprostředkovatele připojení k okruhu toohello vyžaduje stav a stav.</span><span class="sxs-lookup"><span data-stu-id="e97b1-148">If it doesn't, work with your connectivity provider tooget your circuit toohello required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="e97b1-149">**Nakonfigurujte soukromý partnerský vztah Azure pro okruh hello.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-149">**Configure Azure private peering for hello circuit.**</span></span>
   
    <span data-ttu-id="e97b1-150">Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e97b1-150">Make sure that you have hello following items before you proceed with hello next steps:</span></span>
   
   * <span data-ttu-id="e97b1-151">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="e97b1-151">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="e97b1-152">Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e97b1-152">This must not be part of any address space reserved for virtual networks.</span></span>
   * <span data-ttu-id="e97b1-153">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="e97b1-153">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="e97b1-154">Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e97b1-154">This must not be part of any address space reserved for virtual networks.</span></span>
   * <span data-ttu-id="e97b1-155">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="e97b1-155">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="e97b1-156">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="e97b1-156">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
   * <span data-ttu-id="e97b1-157">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="e97b1-157">AS number for peering.</span></span> <span data-ttu-id="e97b1-158">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="e97b1-158">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="e97b1-159">Pro tento partnerský vztah můžete použít soukromé číslo AS.</span><span class="sxs-lookup"><span data-stu-id="e97b1-159">You can use a private AS number for this peering.</span></span> <span data-ttu-id="e97b1-160">Zkontrolujte, že nepoužíváte 65515.</span><span class="sxs-lookup"><span data-stu-id="e97b1-160">Ensure that you are not using 65515.</span></span>
   * <span data-ttu-id="e97b1-161">Hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="e97b1-161">An MD5 hash if you choose toouse one.</span></span> <span data-ttu-id="e97b1-162">**Tato položka je nepovinná.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-162">**This is optional**.</span></span>
     
    <span data-ttu-id="e97b1-163">Spuštěním následující rutiny tooconfigure soukromý partnerský vztah Azure pro váš okruh hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-163">You can run hello following cmdlet tooconfigure Azure private peering for your circuit.</span></span>
     
        <span data-ttu-id="e97b1-164">Nové AzureBGPPeering - AccessType privátní - klíč ServiceKey "***" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100</span><span class="sxs-lookup"><span data-stu-id="e97b1-164">New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100</span></span>
     
    <span data-ttu-id="e97b1-165">Pokud se rozhodnete toouse hodnotu hash MD5, můžete použít následující rutinu hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-165">You can use hello cmdlet below if you choose toouse an MD5 hash.</span></span>
     
        <span data-ttu-id="e97b1-166">Nové AzureBGPPeering - AccessType privátní - klíč ServiceKey "***" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100 - SharedKey "A1B2C3D4"</span><span class="sxs-lookup"><span data-stu-id="e97b1-166">New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="e97b1-167">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="e97b1-167">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
     > 
     > 

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="e97b1-168">tooview Azure privátní partnerský vztah podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e97b1-168">tooview Azure private peering details</span></span>
<span data-ttu-id="e97b1-169">Můžete získat podrobnosti o konfiguraci pomocí následující rutiny hello</span><span class="sxs-lookup"><span data-stu-id="e97b1-169">You can get configuration details using hello following cmdlet</span></span>

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="e97b1-170">tooupdate konfigurace soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="e97b1-170">tooupdate Azure private peering configuration</span></span>
<span data-ttu-id="e97b1-171">Libovolnou část konfigurace hello pomocí hello následující rutiny můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="e97b1-171">You can update any part of hello configuration using hello following cmdlet.</span></span> <span data-ttu-id="e97b1-172">V příkladu hello níže je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 100 too500.</span><span class="sxs-lookup"><span data-stu-id="e97b1-172">In hello example below, hello VLAN ID of hello circuit is being updated from 100 too500.</span></span>

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="e97b1-173">toodelete soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="e97b1-173">toodelete Azure private peering</span></span>
<span data-ttu-id="e97b1-174">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-174">You can remove your peering configuration by running hello following cmdlet.</span></span>

> [!WARNING]
> <span data-ttu-id="e97b1-175">Je nutné zajistit, že všechny virtuální sítě se odpojit od hello okruh ExpressRoute před spuštěním této rutiny.</span><span class="sxs-lookup"><span data-stu-id="e97b1-175">You must ensure that all virtual networks are unlinked from hello ExpressRoute circuit before running this cmdlet.</span></span> 
> 
> 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a><span data-ttu-id="e97b1-176">Veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="e97b1-176">Azure public peering</span></span>
<span data-ttu-id="e97b1-177">Tato část obsahuje pokyny jak toocreate, získat, aktualizovat a odstranit hello veřejné konfigurace partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="e97b1-177">This section provides instructions on how toocreate, get, update and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="e97b1-178">toocreate veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="e97b1-178">toocreate Azure public peering</span></span>
1. <span data-ttu-id="e97b1-179">**Importujte modul PowerShell hello pro ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-179">**Import hello PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="e97b1-180">Je nutné naimportovat moduly Azure a ExpressRoute hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-180">You must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="e97b1-181">Hello spusťte následující příkazy tooimport hello Azure a ExpressRoute modulů do relace prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-181">Run hello following commands tooimport hello Azure and ExpressRoute modules into hello PowerShell session.</span></span> 
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="e97b1-182">**Vytvoření okruhu ExpressRoute**</span><span class="sxs-lookup"><span data-stu-id="e97b1-182">**Create an ExpressRoute circuit**</span></span>
   
    <span data-ttu-id="e97b1-183">Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho zřízený poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-183">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by hello connectivity provider.</span></span> <span data-ttu-id="e97b1-184">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure veřejný partnerský vztah za vás.</span><span class="sxs-lookup"><span data-stu-id="e97b1-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure public peering for you.</span></span> <span data-ttu-id="e97b1-185">V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-185">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="e97b1-186">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow hello instructions below.</span></span>
3. <span data-ttu-id="e97b1-187">**Zkontrolujte tooensure okruh ExpressRoute, které je zřízený**</span><span class="sxs-lookup"><span data-stu-id="e97b1-187">**Check ExpressRoute circuit tooensure it is provisioned**</span></span>
   
    <span data-ttu-id="e97b1-188">Nejdřív musíte zkontrolovat toosee Pokud hello okruh ExpressRoute je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="e97b1-188">You must first check toosee if hello ExpressRoute circuit is Provisioned and also Enabled.</span></span> <span data-ttu-id="e97b1-189">Viz následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-189">See hello example below.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="e97b1-190">Ujistěte se, že hello okruhu zobrazuje jako zajištěno a povoleno.</span><span class="sxs-lookup"><span data-stu-id="e97b1-190">Make sure that hello circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="e97b1-191">Pokud tomu tak není, pracujete s vaší tooget zprostředkovatele připojení k okruhu toohello vyžaduje stav a stav.</span><span class="sxs-lookup"><span data-stu-id="e97b1-191">If it doesn't, work with your connectivity provider tooget your circuit toohello required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="e97b1-192">**Nakonfigurujte veřejný partnerský vztah Azure pro okruh hello**</span><span class="sxs-lookup"><span data-stu-id="e97b1-192">**Configure Azure public peering for hello circuit**</span></span>
   
    <span data-ttu-id="e97b1-193">Ujistěte se, že máte následující informace před pokračováním hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-193">Ensure that you have hello following information before you proceed further.</span></span>
   
   * <span data-ttu-id="e97b1-194">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="e97b1-194">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="e97b1-195">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="e97b1-195">This must be a valid public IPv4 prefix.</span></span>
   * <span data-ttu-id="e97b1-196">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="e97b1-196">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="e97b1-197">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="e97b1-197">This must be a valid public IPv4 prefix.</span></span>
   * <span data-ttu-id="e97b1-198">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="e97b1-198">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="e97b1-199">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="e97b1-199">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
   * <span data-ttu-id="e97b1-200">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="e97b1-200">AS number for peering.</span></span> <span data-ttu-id="e97b1-201">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="e97b1-201">You can use both 2-byte and 4-byte AS numbers.</span></span>
   * <span data-ttu-id="e97b1-202">Hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="e97b1-202">An MD5 hash if you choose toouse one.</span></span> <span data-ttu-id="e97b1-203">**Tato položka je nepovinná.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-203">**This is optional**.</span></span>
     
    <span data-ttu-id="e97b1-204">Spuštěním následující rutiny tooconfigure veřejný partnerský vztah Azure pro váš okruh hello</span><span class="sxs-lookup"><span data-stu-id="e97b1-204">You can run hello following cmdlet tooconfigure Azure public peering for your circuit</span></span>
     
        <span data-ttu-id="e97b1-205">Nové AzureBGPPeering - AccessType veřejný - klíč ServiceKey "***" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200</span><span class="sxs-lookup"><span data-stu-id="e97b1-205">New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200</span></span>
     
    <span data-ttu-id="e97b1-206">Pokud zvolíte toouse hodnotu hash MD5, můžete použít následující rutinu hello</span><span class="sxs-lookup"><span data-stu-id="e97b1-206">You can use hello cmdlet below if you choose toouse an MD5 hash</span></span>
     
        <span data-ttu-id="e97b1-207">Nové AzureBGPPeering - AccessType veřejný - klíč ServiceKey "***" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200 - SharedKey "A1B2C3D4"</span><span class="sxs-lookup"><span data-stu-id="e97b1-207">New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="e97b1-208">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, a ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="e97b1-208">Ensure that you specify your AS number as peering ASN and not customer ASN.</span></span>
     > 
     > 

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="e97b1-209">tooview Azure veřejného partnerského vztahu podrobnosti</span><span class="sxs-lookup"><span data-stu-id="e97b1-209">tooview Azure public peering details</span></span>
<span data-ttu-id="e97b1-210">Můžete získat podrobnosti o konfiguraci pomocí následující rutiny hello</span><span class="sxs-lookup"><span data-stu-id="e97b1-210">You can get configuration details using hello following cmdlet</span></span>

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="e97b1-211">tooupdate konfigurace veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="e97b1-211">tooupdate Azure public peering configuration</span></span>
<span data-ttu-id="e97b1-212">Libovolnou část konfigurace hello pomocí hello následující rutiny můžete aktualizovat</span><span class="sxs-lookup"><span data-stu-id="e97b1-212">You can update any part of hello configuration using hello following cmdlet</span></span>

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

<span data-ttu-id="e97b1-213">Hello ID sítě VLAN okruhu hello je aktualizováno z hodnoty 200 too600 v hello výše příklad.</span><span class="sxs-lookup"><span data-stu-id="e97b1-213">hello VLAN ID of hello circuit is being updated from 200 too600 in hello above example.</span></span>

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="e97b1-214">toodelete veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="e97b1-214">toodelete Azure public peering</span></span>
<span data-ttu-id="e97b1-215">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny hello</span><span class="sxs-lookup"><span data-stu-id="e97b1-215">You can remove your peering configuration by running hello following cmdlet</span></span>

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a><span data-ttu-id="e97b1-216">Partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="e97b1-216">Microsoft peering</span></span>
<span data-ttu-id="e97b1-217">Tato část obsahuje pokyny jak toocreate, získat, aktualizovat a odstranit konfiguraci partnerského vztahu hello Microsoftu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="e97b1-217">This section provides instructions on how toocreate, get, update and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="e97b1-218">partnerský vztah Microsoftu toocreate</span><span class="sxs-lookup"><span data-stu-id="e97b1-218">toocreate Microsoft peering</span></span>
1. <span data-ttu-id="e97b1-219">**Importujte modul PowerShell hello pro ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-219">**Import hello PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="e97b1-220">Je nutné naimportovat moduly Azure a ExpressRoute hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-220">You must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="e97b1-221">Hello spusťte následující příkazy tooimport hello Azure a ExpressRoute modulů do relace prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-221">Run hello following commands tooimport hello Azure and ExpressRoute modules into hello PowerShell session.</span></span>  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="e97b1-222">**Vytvoření okruhu ExpressRoute**</span><span class="sxs-lookup"><span data-stu-id="e97b1-222">**Create an ExpressRoute circuit**</span></span>
   
    <span data-ttu-id="e97b1-223">Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho zřízený poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-223">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by hello connectivity provider.</span></span> <span data-ttu-id="e97b1-224">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás.</span><span class="sxs-lookup"><span data-stu-id="e97b1-224">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="e97b1-225">V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-225">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="e97b1-226">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-226">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow hello instructions below.</span></span>
3. <span data-ttu-id="e97b1-227">**Zkontrolujte tooensure okruh ExpressRoute, které je zřízený**</span><span class="sxs-lookup"><span data-stu-id="e97b1-227">**Check ExpressRoute circuit tooensure it is provisioned**</span></span>
   
    <span data-ttu-id="e97b1-228">Nejdřív musíte zkontrolovat toosee Pokud hello okruh ExpressRoute je ve stavu zajištěno a povoleno.</span><span class="sxs-lookup"><span data-stu-id="e97b1-228">You must first check toosee if hello ExpressRoute circuit is in Provisioned and Enabled state.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="e97b1-229">Ujistěte se, že hello okruhu zobrazuje jako zajištěno a povoleno.</span><span class="sxs-lookup"><span data-stu-id="e97b1-229">Make sure that hello circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="e97b1-230">Pokud tomu tak není, pracujete s vaší tooget zprostředkovatele připojení k okruhu toohello vyžaduje stav a stav.</span><span class="sxs-lookup"><span data-stu-id="e97b1-230">If it doesn't, work with your connectivity provider tooget your circuit toohello required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="e97b1-231">**Nakonfigurujte partnerský vztah Microsoftu pro okruh hello**</span><span class="sxs-lookup"><span data-stu-id="e97b1-231">**Configure Microsoft peering for hello circuit**</span></span>
   
    <span data-ttu-id="e97b1-232">Ujistěte se, že máte hello následující informace, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e97b1-232">Make sure that you have hello following information before you proceed.</span></span>
   
   * <span data-ttu-id="e97b1-233">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="e97b1-233">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="e97b1-234">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="e97b1-234">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
   * <span data-ttu-id="e97b1-235">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="e97b1-235">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="e97b1-236">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="e97b1-236">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
   * <span data-ttu-id="e97b1-237">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="e97b1-237">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="e97b1-238">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="e97b1-238">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
   * <span data-ttu-id="e97b1-239">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="e97b1-239">AS number for peering.</span></span> <span data-ttu-id="e97b1-240">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="e97b1-240">You can use both 2-byte and 4-byte AS numbers.</span></span>
   * <span data-ttu-id="e97b1-241">Inzerované předpony: musíte poskytnout seznam všech předpon plánování tooadvertise přes relaci protokolu BGP hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-241">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="e97b1-242">Přijímají se jenom předpony veřejných IP adres.</span><span class="sxs-lookup"><span data-stu-id="e97b1-242">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="e97b1-243">Pokud máte v plánu toosend sadu předpon, můžete odeslat seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="e97b1-243">You can send a comma separated list if you plan toosend a set of prefixes.</span></span> <span data-ttu-id="e97b1-244">Tyto předpony musí být registrovaný tooyou u rir / IRR.</span><span class="sxs-lookup"><span data-stu-id="e97b1-244">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
   * <span data-ttu-id="e97b1-245">Zákaznické číslo ASN: Pokud jsou inzerování předpon, které nejsou registrované toohello číslo AS partnerského vztahu, můžete hello zadat jako číslo toowhich, které jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="e97b1-245">Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span> <span data-ttu-id="e97b1-246">**Tato položka je nepovinná.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-246">**This is optional**.</span></span>
   * <span data-ttu-id="e97b1-247">Název registru směrování: Můžete zadat hello RIR / IRR, u které hello jako číslo a předpony jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="e97b1-247">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
   * <span data-ttu-id="e97b1-248">Hodnota hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="e97b1-248">An MD5 hash, if you choose toouse one.</span></span> <span data-ttu-id="e97b1-249">**Tato položka je nepovinná.**</span><span class="sxs-lookup"><span data-stu-id="e97b1-249">**This is optional.**</span></span>
     
    <span data-ttu-id="e97b1-250">Spuštěním následující rutiny tooconfigure Microsoft pering pro váš okruh hello</span><span class="sxs-lookup"><span data-stu-id="e97b1-250">You can run hello following cmdlet tooconfigure Microsoft pering for your circuit</span></span>
     
        <span data-ttu-id="e97b1-251">Nové AzureBGPPeering - AccessType Microsoft - klíč ServiceKey "***" "131.107.0.0/30" - PrimaryPeerSubnet - SecondaryPeerSubnet "131.107.0.4/30" - VlanId 300 - PeerAsn 1234 - CustomerAsn. 2245 - AdvertisedPublicPrefixes " 123.0.0.0/30 "- RoutingRegistryName"ARIN"- SharedKey"A1B2C3D4"</span><span class="sxs-lookup"><span data-stu-id="e97b1-251">New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"</span></span>

### <a name="tooview-microsoft-peering-details"></a><span data-ttu-id="e97b1-252">tooview podrobností partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="e97b1-252">tooview Microsoft peering details</span></span>
<span data-ttu-id="e97b1-253">Můžete získat podrobnosti o konfiguraci pomocí následující rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-253">You can get configuration details using hello following cmdlet.</span></span>

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="e97b1-254">konfiguraci partnerského vztahu Microsoftu tooupdate</span><span class="sxs-lookup"><span data-stu-id="e97b1-254">tooupdate Microsoft peering configuration</span></span>
<span data-ttu-id="e97b1-255">Libovolnou část konfigurace hello pomocí hello následující rutiny můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="e97b1-255">You can update any part of hello configuration using hello following cmdlet.</span></span>

    Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="e97b1-256">partnerský vztah Microsoftu toodelete</span><span class="sxs-lookup"><span data-stu-id="e97b1-256">toodelete Microsoft peering</span></span>
<span data-ttu-id="e97b1-257">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="e97b1-257">You can remove your peering configuration by running hello following cmdlet.</span></span>

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a><span data-ttu-id="e97b1-258">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e97b1-258">Next steps</span></span>
<span data-ttu-id="e97b1-259">Dále [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="e97b1-259">Next, [Link a VNet tooan ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).</span></span>

* <span data-ttu-id="e97b1-260">Další informace o pracovních postupech najdete v tématu [pracovních postupech](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="e97b1-260">For more information about workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="e97b1-261">Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="e97b1-261">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>

