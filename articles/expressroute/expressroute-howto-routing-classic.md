---
title: "Postup konfigurace směrování (partnerský vztah) pro ExpressRoute okruh: Azure: classic | Microsoft Docs"
description: "Tento článek vás provede kroky pro vytváření a zřizování soukromého a veřejného partnerského vztahu a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak kontrolovat stav partnerských vztahů pro váš okruh, aktualizovat je nebo je odstranit."
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
ms.openlocfilehash: 37713db70f3ae837edafc997b78b16b121d0a885
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a><span data-ttu-id="3c574-104">Vytvářet a upravovat partnerský vztah pro okruh ExpressRoute (klasické)</span><span class="sxs-lookup"><span data-stu-id="3c574-104">Create and modify peering for an ExpressRoute circuit (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3c574-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3c574-105">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="3c574-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c574-106">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="3c574-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3c574-107">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="3c574-108">Video - soukromého partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="3c574-108">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="3c574-109">Video - veřejného partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="3c574-109">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="3c574-110">Video - partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="3c574-110">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="3c574-111">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="3c574-111">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

<span data-ttu-id="3c574-112">Tento článek vás provede kroky k vytvoření a správě konfigurace směrování pro okruhu ExpressRoute pomocí Powershellu a modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="3c574-112">This article walks you through the steps to create and manage routing configuration for an ExpressRoute circuit using PowerShell and the classic deployment model.</span></span> <span data-ttu-id="3c574-113">Dál uvedené kroky také ukazují, jak kontrolovat stav partnerských vztahů pro okruh ExpressRoute, aktualizovat je nebo je odstranit a zrušit jejich zřízení.</span><span class="sxs-lookup"><span data-stu-id="3c574-113">The steps below will also show you how to check the status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="3c574-114">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="3c574-114">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a><span data-ttu-id="3c574-115">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="3c574-115">Configuration prerequisites</span></span>
* <span data-ttu-id="3c574-116">Budete potřebovat nejnovější verzi rutin prostředí PowerShell Azure Service Management (SM).</span><span class="sxs-lookup"><span data-stu-id="3c574-116">You will need the latest version of the Azure Service Management (SM) PowerShell cmdlets.</span></span> <span data-ttu-id="3c574-117">Další informace najdete v tématu [Začínáme s rutinami prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3c574-117">For more information, see [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>  
* <span data-ttu-id="3c574-118">Před zahájením konfigurace se ujistěte, že jste si přečetli stránku s [předpoklady](expressroute-prerequisites.md), stránku s [požadavky směrování](expressroute-routing.md) a stránku s [pracovními postupy](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="3c574-118">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) page, the [routing requirements](expressroute-routing.md) page, and the [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="3c574-119">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="3c574-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="3c574-120">Postupujte podle pokynů a [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="3c574-120">Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="3c574-121">Abyste mohli spouštět rutiny popsané dál, musí být okruh ExpressRoute zřízený a povolený.</span><span class="sxs-lookup"><span data-stu-id="3c574-121">The ExpressRoute circuit must be in a provisioned and enabled state for you to be able to run the cmdlets described below.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c574-122">Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="3c574-122">These instructions only apply to circuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="3c574-123">Pokud používáte poskytovatele služeb nabízejícího spravované služby vrstvy 3 (obvykle IPVPN, např. MPLS), poskytovatel připojení provede konfiguraci a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="3c574-123">If you are using a service provider offering managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

<span data-ttu-id="3c574-124">Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="3c574-124">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="3c574-125">Partnerské vztahy můžete konfigurovat v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="3c574-125">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="3c574-126">Musíte se ale přesvědčit, že jste vždy konfiguraci každého partnerského vztahu dokončili.</span><span class="sxs-lookup"><span data-stu-id="3c574-126">However, you must make sure that you complete the configuration of each peering one at a time.</span></span>


### <a name="log-in-to-your-azure-account-and-select-a-subscription"></a><span data-ttu-id="3c574-127">Přihlaste se k účtu Azure a vybrat odběr</span><span class="sxs-lookup"><span data-stu-id="3c574-127">Log in to your Azure account and select a subscription</span></span>
1. <span data-ttu-id="3c574-128">Otevřete konzolu PowerShellu se zvýšenými oprávněními a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="3c574-128">Open your PowerShell console with elevated rights and connect to your account.</span></span> <span data-ttu-id="3c574-129">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="3c574-129">Use the following example to help you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="3c574-130">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="3c574-130">Check the subscriptions for the account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="3c574-131">Máte-li více předplatných, vyberte předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="3c574-131">If you have more than one subscription, select the subscription that you want to use.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="3c574-132">Potom použijte následující rutinu k předplatnému Azure přidat do prostředí PowerShell pro model nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="3c574-132">Next, use the following cmdlet to add your Azure subscription to PowerShell for the classic deployment model.</span></span>

        Add-AzureAccount


## <a name="azure-private-peering"></a><span data-ttu-id="3c574-133">Soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-133">Azure private peering</span></span>
<span data-ttu-id="3c574-134">Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci soukromého partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="3c574-134">This section provides instructions on how to create, get, update, and delete the Azure private peering configuration for an ExpressRoute circuit.</span></span> 

### <a name="to-create-azure-private-peering"></a><span data-ttu-id="3c574-135">Vytvoření soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-135">To create Azure private peering</span></span>
1. <span data-ttu-id="3c574-136">**Naimportujte modul Powershellu pro ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="3c574-136">**Import the PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="3c574-137">Pokud chcete začít používat rutiny pro ExpressRoute, je nutné naimportovat moduly Azure a ExpressRoute do relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c574-137">You must import the Azure and ExpressRoute modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="3c574-138">Spusťte následující příkazy a naimportovat moduly Azure a ExpressRoute do relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c574-138">Run the following commands to import the Azure and ExpressRoute modules into the PowerShell session.</span></span>  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="3c574-139">**Vytvoření okruhu ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="3c574-139">**Create an ExpressRoute circuit.**</span></span>
   
    <span data-ttu-id="3c574-140">Podle pokynů vytvořte [okruh ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho zřízený poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="3c574-140">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by the connectivity provider.</span></span> <span data-ttu-id="3c574-141">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil soukromý partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="3c574-141">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="3c574-142">V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="3c574-142">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="3c574-143">Pokud ale poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů dál.</span><span class="sxs-lookup"><span data-stu-id="3c574-143">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow the instructions below.</span></span> 
3. <span data-ttu-id="3c574-144">**Zkontrolujte okruh ExpressRoute a ověřte, že je zřízený.**</span><span class="sxs-lookup"><span data-stu-id="3c574-144">**Check the ExpressRoute circuit to ensure it is provisioned.**</span></span>
   
    <span data-ttu-id="3c574-145">Nejdřív musíte zkontrolovat, že stav okruhu ExpressRoute je Zřízený a také Povolený.</span><span class="sxs-lookup"><span data-stu-id="3c574-145">You must first check to see if the ExpressRoute circuit is Provisioned and also Enabled.</span></span> <span data-ttu-id="3c574-146">Viz následující příklad.</span><span class="sxs-lookup"><span data-stu-id="3c574-146">See the example below.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="3c574-147">Ujistěte se, že je okruh zobrazuje jako zajištěno a povoleno.</span><span class="sxs-lookup"><span data-stu-id="3c574-147">Make sure that the circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="3c574-148">Pokud tomu tak není, fungovat u svého poskytovatele připojení získat váš okruh. požadovaný stav a stav.</span><span class="sxs-lookup"><span data-stu-id="3c574-148">If it doesn't, work with your connectivity provider to get your circuit to the required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="3c574-149">**Nakonfigurujte soukromý partnerský vztah Azure pro okruh.**</span><span class="sxs-lookup"><span data-stu-id="3c574-149">**Configure Azure private peering for the circuit.**</span></span>
   
    <span data-ttu-id="3c574-150">Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:</span><span class="sxs-lookup"><span data-stu-id="3c574-150">Make sure that you have the following items before you proceed with the next steps:</span></span>
   
   * <span data-ttu-id="3c574-151">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="3c574-151">A /30 subnet for the primary link.</span></span> <span data-ttu-id="3c574-152">Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3c574-152">This must not be part of any address space reserved for virtual networks.</span></span>
   * <span data-ttu-id="3c574-153">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="3c574-153">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="3c574-154">Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3c574-154">This must not be part of any address space reserved for virtual networks.</span></span>
   * <span data-ttu-id="3c574-155">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="3c574-155">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="3c574-156">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="3c574-156">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
   * <span data-ttu-id="3c574-157">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="3c574-157">AS number for peering.</span></span> <span data-ttu-id="3c574-158">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="3c574-158">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="3c574-159">Pro tento partnerský vztah můžete použít soukromé číslo AS.</span><span class="sxs-lookup"><span data-stu-id="3c574-159">You can use a private AS number for this peering.</span></span> <span data-ttu-id="3c574-160">Zkontrolujte, že nepoužíváte 65515.</span><span class="sxs-lookup"><span data-stu-id="3c574-160">Ensure that you are not using 65515.</span></span>
   * <span data-ttu-id="3c574-161">Hodnota hash MD5, pokud se ji rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="3c574-161">An MD5 hash if you choose to use one.</span></span> <span data-ttu-id="3c574-162">**Tato položka je nepovinná.**</span><span class="sxs-lookup"><span data-stu-id="3c574-162">**This is optional**.</span></span>
     
    <span data-ttu-id="3c574-163">Spuštěním následující rutiny můžete nakonfigurovat soukromý partnerský vztah Azure pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="3c574-163">You can run the following cmdlet to configure Azure private peering for your circuit.</span></span>
     
        <span data-ttu-id="3c574-164">Nové AzureBGPPeering - AccessType privátní - klíč ServiceKey "***" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100</span><span class="sxs-lookup"><span data-stu-id="3c574-164">New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100</span></span>
     
    <span data-ttu-id="3c574-165">Pokud se rozhodnete použít hodnotu hash MD5, můžete použít následující rutinu.</span><span class="sxs-lookup"><span data-stu-id="3c574-165">You can use the cmdlet below if you choose to use an MD5 hash.</span></span>
     
        <span data-ttu-id="3c574-166">Nové AzureBGPPeering - AccessType privátní - klíč ServiceKey "***" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100 - SharedKey "A1B2C3D4"</span><span class="sxs-lookup"><span data-stu-id="3c574-166">New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="3c574-167">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="3c574-167">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
     > 
     > 

### <a name="to-view-azure-private-peering-details"></a><span data-ttu-id="3c574-168">Zobrazení podrobností soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-168">To view Azure private peering details</span></span>
<span data-ttu-id="3c574-169">Můžete získat podrobnosti o konfiguraci pomocí následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c574-169">You can get configuration details using the following cmdlet</span></span>

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


### <a name="to-update-azure-private-peering-configuration"></a><span data-ttu-id="3c574-170">Aktualizace konfigurace soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-170">To update Azure private peering configuration</span></span>
<span data-ttu-id="3c574-171">Libovolnou část konfigurace můžete aktualizovat pomocí následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c574-171">You can update any part of the configuration using the following cmdlet.</span></span> <span data-ttu-id="3c574-172">V následujícím příkladu je ID sítě VLAN okruhu aktualizováno z hodnoty 100 na hodnotu 500.</span><span class="sxs-lookup"><span data-stu-id="3c574-172">In the example below, the VLAN ID of the circuit is being updated from 100 to 500.</span></span>

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a><span data-ttu-id="3c574-173">Odstranění soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-173">To delete Azure private peering</span></span>
<span data-ttu-id="3c574-174">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c574-174">You can remove your peering configuration by running the following cmdlet.</span></span>

> [!WARNING]
> <span data-ttu-id="3c574-175">Před spuštěním této rutiny je nutné zajistit, že všechny virtuální sítě jsou od okruhu ExpressRoute odpojené.</span><span class="sxs-lookup"><span data-stu-id="3c574-175">You must ensure that all virtual networks are unlinked from the ExpressRoute circuit before running this cmdlet.</span></span> 
> 
> 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a><span data-ttu-id="3c574-176">Veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-176">Azure public peering</span></span>
<span data-ttu-id="3c574-177">Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci veřejného partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="3c574-177">This section provides instructions on how to create, get, update and delete the Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-public-peering"></a><span data-ttu-id="3c574-178">Vytvoření veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-178">To create Azure public peering</span></span>
1. <span data-ttu-id="3c574-179">**Naimportujte modul Powershellu pro ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="3c574-179">**Import the PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="3c574-180">Pokud chcete začít používat rutiny pro ExpressRoute, je nutné naimportovat moduly Azure a ExpressRoute do relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c574-180">You must import the Azure and ExpressRoute modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="3c574-181">Spusťte následující příkazy a naimportovat moduly Azure a ExpressRoute do relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c574-181">Run the following commands to import the Azure and ExpressRoute modules into the PowerShell session.</span></span> 
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="3c574-182">**Vytvoření okruhu ExpressRoute**</span><span class="sxs-lookup"><span data-stu-id="3c574-182">**Create an ExpressRoute circuit**</span></span>
   
    <span data-ttu-id="3c574-183">Podle pokynů vytvořte [okruh ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho zřízený poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="3c574-183">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by the connectivity provider.</span></span> <span data-ttu-id="3c574-184">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil veřejný partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="3c574-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure public peering for you.</span></span> <span data-ttu-id="3c574-185">V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="3c574-185">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="3c574-186">Pokud ale poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů dál.</span><span class="sxs-lookup"><span data-stu-id="3c574-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow the instructions below.</span></span>
3. <span data-ttu-id="3c574-187">**Zkontrolujte okruh ExpressRoute a ověřte, že je zřízený**</span><span class="sxs-lookup"><span data-stu-id="3c574-187">**Check ExpressRoute circuit to ensure it is provisioned**</span></span>
   
    <span data-ttu-id="3c574-188">Nejdřív musíte zkontrolovat, že stav okruhu ExpressRoute je Zřízený a také Povolený.</span><span class="sxs-lookup"><span data-stu-id="3c574-188">You must first check to see if the ExpressRoute circuit is Provisioned and also Enabled.</span></span> <span data-ttu-id="3c574-189">Viz následující příklad.</span><span class="sxs-lookup"><span data-stu-id="3c574-189">See the example below.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="3c574-190">Ujistěte se, že je okruh zobrazuje jako zajištěno a povoleno.</span><span class="sxs-lookup"><span data-stu-id="3c574-190">Make sure that the circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="3c574-191">Pokud tomu tak není, fungovat u svého poskytovatele připojení získat váš okruh. požadovaný stav a stav.</span><span class="sxs-lookup"><span data-stu-id="3c574-191">If it doesn't, work with your connectivity provider to get your circuit to the required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="3c574-192">**Nakonfigurujte veřejný partnerský vztah Azure pro okruh**</span><span class="sxs-lookup"><span data-stu-id="3c574-192">**Configure Azure public peering for the circuit**</span></span>
   
    <span data-ttu-id="3c574-193">Před pokračováním se ujistěte, že máte k dispozici následující informace.</span><span class="sxs-lookup"><span data-stu-id="3c574-193">Ensure that you have the following information before you proceed further.</span></span>
   
   * <span data-ttu-id="3c574-194">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="3c574-194">A /30 subnet for the primary link.</span></span> <span data-ttu-id="3c574-195">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="3c574-195">This must be a valid public IPv4 prefix.</span></span>
   * <span data-ttu-id="3c574-196">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="3c574-196">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="3c574-197">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="3c574-197">This must be a valid public IPv4 prefix.</span></span>
   * <span data-ttu-id="3c574-198">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="3c574-198">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="3c574-199">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="3c574-199">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
   * <span data-ttu-id="3c574-200">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="3c574-200">AS number for peering.</span></span> <span data-ttu-id="3c574-201">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="3c574-201">You can use both 2-byte and 4-byte AS numbers.</span></span>
   * <span data-ttu-id="3c574-202">Hodnota hash MD5, pokud se ji rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="3c574-202">An MD5 hash if you choose to use one.</span></span> <span data-ttu-id="3c574-203">**Tato položka je nepovinná.**</span><span class="sxs-lookup"><span data-stu-id="3c574-203">**This is optional**.</span></span>
     
    <span data-ttu-id="3c574-204">Spuštěním následující rutiny můžete nakonfigurovat veřejný partnerský vztah Azure pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="3c574-204">You can run the following cmdlet to configure Azure public peering for your circuit</span></span>
     
        <span data-ttu-id="3c574-205">Nové AzureBGPPeering - AccessType veřejný - klíč ServiceKey "***" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200</span><span class="sxs-lookup"><span data-stu-id="3c574-205">New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200</span></span>
     
    <span data-ttu-id="3c574-206">Pokud se rozhodnete použít hodnotu hash MD5, můžete použít následující rutinu.</span><span class="sxs-lookup"><span data-stu-id="3c574-206">You can use the cmdlet below if you choose to use an MD5 hash</span></span>
     
        <span data-ttu-id="3c574-207">Nové AzureBGPPeering - AccessType veřejný - klíč ServiceKey "***" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200 - SharedKey "A1B2C3D4"</span><span class="sxs-lookup"><span data-stu-id="3c574-207">New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="3c574-208">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, a ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="3c574-208">Ensure that you specify your AS number as peering ASN and not customer ASN.</span></span>
     > 
     > 

### <a name="to-view-azure-public-peering-details"></a><span data-ttu-id="3c574-209">Zobrazení podrobností veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-209">To view Azure public peering details</span></span>
<span data-ttu-id="3c574-210">Můžete získat podrobnosti o konfiguraci pomocí následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c574-210">You can get configuration details using the following cmdlet</span></span>

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


### <a name="to-update-azure-public-peering-configuration"></a><span data-ttu-id="3c574-211">Aktualizace konfigurace veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-211">To update Azure public peering configuration</span></span>
<span data-ttu-id="3c574-212">Libovolnou část konfigurace můžete aktualizovat pomocí následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c574-212">You can update any part of the configuration using the following cmdlet</span></span>

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

<span data-ttu-id="3c574-213">V předchozím příkladu je ID sítě VLAN okruhu aktualizováno z hodnoty 200 na hodnotu 600.</span><span class="sxs-lookup"><span data-stu-id="3c574-213">The VLAN ID of the circuit is being updated from 200 to 600 in the above example.</span></span>

### <a name="to-delete-azure-public-peering"></a><span data-ttu-id="3c574-214">Odstranění veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="3c574-214">To delete Azure public peering</span></span>
<span data-ttu-id="3c574-215">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c574-215">You can remove your peering configuration by running the following cmdlet</span></span>

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a><span data-ttu-id="3c574-216">Partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="3c574-216">Microsoft peering</span></span>
<span data-ttu-id="3c574-217">Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu Microsoftu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="3c574-217">This section provides instructions on how to create, get, update and delete the Microsoft peering configuration for an ExpressRoute circuit.</span></span> 

### <a name="to-create-microsoft-peering"></a><span data-ttu-id="3c574-218">Vytvoření partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="3c574-218">To create Microsoft peering</span></span>
1. <span data-ttu-id="3c574-219">**Naimportujte modul Powershellu pro ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="3c574-219">**Import the PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="3c574-220">Pokud chcete začít používat rutiny pro ExpressRoute, je nutné naimportovat moduly Azure a ExpressRoute do relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c574-220">You must import the Azure and ExpressRoute modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="3c574-221">Spusťte následující příkazy a naimportovat moduly Azure a ExpressRoute do relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c574-221">Run the following commands to import the Azure and ExpressRoute modules into the PowerShell session.</span></span>  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="3c574-222">**Vytvoření okruhu ExpressRoute**</span><span class="sxs-lookup"><span data-stu-id="3c574-222">**Create an ExpressRoute circuit**</span></span>
   
    <span data-ttu-id="3c574-223">Podle pokynů vytvořte [okruh ExpressRoute](expressroute-howto-circuit-classic.md) a mějte ho zřízený poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="3c574-223">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by the connectivity provider.</span></span> <span data-ttu-id="3c574-224">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil soukromý partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="3c574-224">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="3c574-225">V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="3c574-225">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="3c574-226">Pokud ale poskytovatel připojení nespravuje směrování, po vytvoření okruhu postupujte podle pokynů dál.</span><span class="sxs-lookup"><span data-stu-id="3c574-226">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow the instructions below.</span></span>
3. <span data-ttu-id="3c574-227">**Zkontrolujte okruh ExpressRoute a ověřte, že je zřízený**</span><span class="sxs-lookup"><span data-stu-id="3c574-227">**Check ExpressRoute circuit to ensure it is provisioned**</span></span>
   
    <span data-ttu-id="3c574-228">Nejdřív musíte zkontrolovat, jestli okruh ExpressRoute je ve stavu zajištěno a povoleno.</span><span class="sxs-lookup"><span data-stu-id="3c574-228">You must first check to see if the ExpressRoute circuit is in Provisioned and Enabled state.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="3c574-229">Ujistěte se, že je okruh zobrazuje jako zajištěno a povoleno.</span><span class="sxs-lookup"><span data-stu-id="3c574-229">Make sure that the circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="3c574-230">Pokud tomu tak není, fungovat u svého poskytovatele připojení získat váš okruh. požadovaný stav a stav.</span><span class="sxs-lookup"><span data-stu-id="3c574-230">If it doesn't, work with your connectivity provider to get your circuit to the required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="3c574-231">**Nakonfigurujte partnerský vztah Microsoftu pro okruh**</span><span class="sxs-lookup"><span data-stu-id="3c574-231">**Configure Microsoft peering for the circuit**</span></span>
   
    <span data-ttu-id="3c574-232">Před pokračováním se ujistěte, že máte k dispozici následující informace.</span><span class="sxs-lookup"><span data-stu-id="3c574-232">Make sure that you have the following information before you proceed.</span></span>
   
   * <span data-ttu-id="3c574-233">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="3c574-233">A /30 subnet for the primary link.</span></span> <span data-ttu-id="3c574-234">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="3c574-234">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
   * <span data-ttu-id="3c574-235">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="3c574-235">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="3c574-236">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="3c574-236">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
   * <span data-ttu-id="3c574-237">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="3c574-237">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="3c574-238">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="3c574-238">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
   * <span data-ttu-id="3c574-239">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="3c574-239">AS number for peering.</span></span> <span data-ttu-id="3c574-240">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="3c574-240">You can use both 2-byte and 4-byte AS numbers.</span></span>
   * <span data-ttu-id="3c574-241">Inzerované předpony: Musíte poskytnout seznam všech předpon, které plánujete inzerovat přes relaci protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="3c574-241">Advertised prefixes: You must provide a list of all prefixes you plan to advertise over the BGP session.</span></span> <span data-ttu-id="3c574-242">Přijímají se jenom předpony veřejných IP adres.</span><span class="sxs-lookup"><span data-stu-id="3c574-242">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="3c574-243">Pokud chcete odeslat sadu předpon, můžete odeslat seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="3c574-243">You can send a comma separated list if you plan to send a set of prefixes.</span></span> <span data-ttu-id="3c574-244">Tyto předpony musí být v RIR/IRR zaregistrované na vás.</span><span class="sxs-lookup"><span data-stu-id="3c574-244">These prefixes must be registered to you in an RIR / IRR.</span></span>
   * <span data-ttu-id="3c574-245">Zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované na číslo AS partnerského vztahu, můžete zadat číslo AS, na které jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="3c574-245">Customer ASN: If you are advertising prefixes that are not registered to the peering AS number, you can specify the AS number to which they are registered.</span></span> <span data-ttu-id="3c574-246">**Tato položka je nepovinná.**</span><span class="sxs-lookup"><span data-stu-id="3c574-246">**This is optional**.</span></span>
   * <span data-ttu-id="3c574-247">Název registru směrování: Můžete zadat RIR/IRR, kde jsou předpony a číslo AS registrované.</span><span class="sxs-lookup"><span data-stu-id="3c574-247">Routing Registry Name: You can specify the RIR / IRR against which the AS number and prefixes are registered.</span></span>
   * <span data-ttu-id="3c574-248">Hodnota hash MD5, pokud se ji rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="3c574-248">An MD5 hash, if you choose to use one.</span></span> <span data-ttu-id="3c574-249">**Tato položka je nepovinná.**</span><span class="sxs-lookup"><span data-stu-id="3c574-249">**This is optional.**</span></span>
     
    <span data-ttu-id="3c574-250">Spuštěním následující rutiny můžete nakonfigurovat Microsoft pering pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="3c574-250">You can run the following cmdlet to configure Microsoft pering for your circuit</span></span>
     
        <span data-ttu-id="3c574-251">Nové AzureBGPPeering - AccessType Microsoft - klíč ServiceKey "***" "131.107.0.0/30" - PrimaryPeerSubnet - SecondaryPeerSubnet "131.107.0.4/30" - VlanId 300 - PeerAsn 1234 - CustomerAsn. 2245 - AdvertisedPublicPrefixes " 123.0.0.0/30 "- RoutingRegistryName"ARIN"- SharedKey"A1B2C3D4"</span><span class="sxs-lookup"><span data-stu-id="3c574-251">New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"</span></span>

### <a name="to-view-microsoft-peering-details"></a><span data-ttu-id="3c574-252">Zobrazení podrobností partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="3c574-252">To view Microsoft peering details</span></span>
<span data-ttu-id="3c574-253">Můžete získat podrobnosti o konfiguraci pomocí následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c574-253">You can get configuration details using the following cmdlet.</span></span>

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


### <a name="to-update-microsoft-peering-configuration"></a><span data-ttu-id="3c574-254">Aktualizace konfigurace partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="3c574-254">To update Microsoft peering configuration</span></span>
<span data-ttu-id="3c574-255">Libovolnou část konfigurace můžete aktualizovat pomocí následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c574-255">You can update any part of the configuration using the following cmdlet.</span></span>

    Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a><span data-ttu-id="3c574-256">Odstranění partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="3c574-256">To delete Microsoft peering</span></span>
<span data-ttu-id="3c574-257">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c574-257">You can remove your peering configuration by running the following cmdlet.</span></span>

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a><span data-ttu-id="3c574-258">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c574-258">Next steps</span></span>
<span data-ttu-id="3c574-259">Dále [propojení virtuální sítě k okruhu ExpressRoute](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="3c574-259">Next, [Link a VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).</span></span>

* <span data-ttu-id="3c574-260">Další informace o pracovních postupech najdete v tématu [pracovních postupech](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="3c574-260">For more information about workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="3c574-261">Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="3c574-261">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>

