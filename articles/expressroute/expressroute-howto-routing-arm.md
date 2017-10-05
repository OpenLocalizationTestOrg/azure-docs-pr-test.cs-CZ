---
title: "Postup konfigurace směrování (partnerský vztah) pro ExpressRoute okruh: Resource Manager: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek vás provede kroky pro vytváření a zřizování soukromého a veřejného partnerského vztahu a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak kontrolovat stav partnerských vztahů pro váš okruh, aktualizovat je nebo je odstranit."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0a036d51-77ae-4fee-9ddb-35f040fbdcdf
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: af68955b78239832e413e1b59e033d7d3da8d599
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="fb256-104">Vytvářet a upravovat partnerský vztah pro okruhu ExpressRoute pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb256-104">Create and modify peering for an ExpressRoute circuit using PowerShell</span></span>

<span data-ttu-id="fb256-105">Tento článek pomůže při vytváření a správě konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb256-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in the Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="fb256-106">Můžete také zkontrolovat stav, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-106">You can also check the status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="fb256-107">Pokud chcete použít jinou metodu pro práci se váš okruh, vyberte článek z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="fb256-107">If you want to use a different method to work with your circuit, select an article from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="fb256-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fb256-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="fb256-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb256-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="fb256-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fb256-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="fb256-111">Video - soukromého partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="fb256-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="fb256-112">Video - veřejného partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="fb256-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="fb256-113">Video - partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="fb256-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="fb256-114">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="fb256-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="fb256-115">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="fb256-115">Configuration prerequisites</span></span>

* <span data-ttu-id="fb256-116">Budete potřebovat nejnovější verzi rutin Powershellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fb256-116">You will need the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="fb256-117">Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fb256-117">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> 
* <span data-ttu-id="fb256-118">Před zahájením konfigurace se ujistěte, že jste si přečetli stránku s [předpoklady](expressroute-prerequisites.md), stránku s [požadavky směrování](expressroute-routing.md) a stránku s [pracovními postupy](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="fb256-118">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) page, the [routing requirements](expressroute-routing.md) page, and the [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="fb256-119">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="fb256-120">Než budete pokračovat, podle pokynů [vytvořte okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-120">Follow the instructions to [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="fb256-121">Okruh ExpressRoute musí být v zřízený a povolený stavu, abyste mohli spustit rutiny v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="fb256-121">The ExpressRoute circuit must be in a provisioned and enabled state for you to be able to run the cmdlets in this article.</span></span>

<span data-ttu-id="fb256-122">Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="fb256-122">These instructions only apply to circuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="fb256-123">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="fb256-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb256-124">Aktuálně prostřednictvím portálu pro správu služeb neinzerujeme partnerské vztahy nakonfigurované poskytovateli služeb.</span><span class="sxs-lookup"><span data-stu-id="fb256-124">We currently do not advertise peerings configured by service providers through the service management portal.</span></span> <span data-ttu-id="fb256-125">Pracujeme na tom, aby tato možnost brzy fungovala.</span><span class="sxs-lookup"><span data-stu-id="fb256-125">We are working on enabling this capability soon.</span></span> <span data-ttu-id="fb256-126">Před konfigurací partnerských vztahů protokolu BGP, zkontrolujte u vašeho poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="fb256-126">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="fb256-127">Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-127">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="fb256-128">Partnerské vztahy můžete konfigurovat v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="fb256-128">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="fb256-129">Musíte se ale přesvědčit, že jste vždy konfiguraci každého partnerského vztahu dokončili.</span><span class="sxs-lookup"><span data-stu-id="fb256-129">However, you must make sure that you complete the configuration of each peering one at a time.</span></span> 

## <a name="azure-private-peering"></a><span data-ttu-id="fb256-130">Soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-130">Azure private peering</span></span>

<span data-ttu-id="fb256-131">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit Azure konfigurace soukromého partnerského vztahu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-131">This section helps you create, get, update, and delete the Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-private-peering"></a><span data-ttu-id="fb256-132">Vytvoření soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-132">To create Azure private peering</span></span>

1. <span data-ttu-id="fb256-133">Naimportujte modul PowerShellu pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-133">Import the PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="fb256-134">Abyste mohli začít používat rutiny pro ExpressRoute, musíte nainstalovat nejnovější verzi instalačního programu PowerShellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru do relace PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="fb256-134">You must install the latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import the Azure Resource Manager modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="fb256-135">Musíte spustit PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="fb256-135">You will need to run PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  <span data-ttu-id="fb256-136">Importujte všechny moduly AzureRM.* v rozsahu známé sémantické verze.</span><span class="sxs-lookup"><span data-stu-id="fb256-136">Import all of the AzureRM.* modules within the known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="fb256-137">Můžete můžete také naimportovat jenom modul select v rozsahu známé sémantické verze.</span><span class="sxs-lookup"><span data-stu-id="fb256-137">You can also just import a select module within the known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network 
  ```

  <span data-ttu-id="fb256-138">Přihlaste se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="fb256-138">Sign in to your account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="fb256-139">Vyberte předplatné, které chcete vytvořit okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-139">Select the subscription you want to create ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="fb256-140">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-140">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="fb256-141">Podle pokynů vytvořte [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-141">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="fb256-142">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil soukromý partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="fb256-142">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="fb256-143">V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="fb256-143">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="fb256-144">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí následující kroky.</span><span class="sxs-lookup"><span data-stu-id="fb256-144">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="fb256-145">Zkontrolujte okruh ExpressRoute a ujistěte se, že je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="fb256-145">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="fb256-146">Použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="fb256-146">Use the following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="fb256-147">Odpověď je stejný jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="fb256-147">The response is similar to the following example:</span></span>

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. <span data-ttu-id="fb256-148">Nakonfigurujte soukromý partnerský vztah Azure pro okruh.</span><span class="sxs-lookup"><span data-stu-id="fb256-148">Configure Azure private peering for the circuit.</span></span> <span data-ttu-id="fb256-149">Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:</span><span class="sxs-lookup"><span data-stu-id="fb256-149">Make sure that you have the following items before you proceed with the next steps:</span></span>

  * <span data-ttu-id="fb256-150">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-150">A /30 subnet for the primary link.</span></span> <span data-ttu-id="fb256-151">Podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fb256-151">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="fb256-152">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-152">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="fb256-153">Podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="fb256-153">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="fb256-154">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="fb256-154">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="fb256-155">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="fb256-155">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="fb256-156">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="fb256-156">AS number for peering.</span></span> <span data-ttu-id="fb256-157">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="fb256-157">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="fb256-158">Pro tento partnerský vztah můžete použít soukromé číslo AS.</span><span class="sxs-lookup"><span data-stu-id="fb256-158">You can use a private AS number for this peering.</span></span> <span data-ttu-id="fb256-159">Zkontrolujte, že nepoužíváte 65515.</span><span class="sxs-lookup"><span data-stu-id="fb256-159">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="fb256-160">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="fb256-160">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="fb256-161">Následující příklad použijte ke konfiguraci soukromého partnerského vztahu Azure pro váš okruh:</span><span class="sxs-lookup"><span data-stu-id="fb256-161">Use the following example to configure Azure private peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="fb256-162">Pokud se rozhodnete použít hodnotu hash MD5, použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="fb256-162">If you choose to use an MD5 hash, use the following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="fb256-163">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="fb256-163">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="to-view-azure-private-peering-details"></a><span data-ttu-id="fb256-164">Zobrazení podrobností soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-164">To view Azure private peering details</span></span>

<span data-ttu-id="fb256-165">Můžete získat podrobnosti o konfiguraci pomocí následující příklad:</span><span class="sxs-lookup"><span data-stu-id="fb256-165">You can get configuration details by using the following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
```

### <a name="to-update-azure-private-peering-configuration"></a><span data-ttu-id="fb256-166">Aktualizace konfigurace soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-166">To update Azure private peering configuration</span></span>

<span data-ttu-id="fb256-167">Libovolnou část konfigurace pomocí následujícího příkladu můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="fb256-167">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="fb256-168">V tomto příkladu je ID sítě VLAN okruhu aktualizováno z 100 na 500.</span><span class="sxs-lookup"><span data-stu-id="fb256-168">In this example, the VLAN ID of the circuit is being updated from 100 to 500.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-private-peering"></a><span data-ttu-id="fb256-169">Odstranění soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-169">To delete Azure private peering</span></span>

<span data-ttu-id="fb256-170">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující příklad:</span><span class="sxs-lookup"><span data-stu-id="fb256-170">You can remove your peering configuration by running the following example:</span></span>

> [!WARNING]
> <span data-ttu-id="fb256-171">Je nutné zajistit, že všechny virtuální sítě jsou před spuštěním tohoto příkladu odpojit od okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-171">You must ensure that all virtual networks are unlinked from the ExpressRoute circuit before running this example.</span></span> 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-public-peering"></a><span data-ttu-id="fb256-172">Veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-172">Azure public peering</span></span>

<span data-ttu-id="fb256-173">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit veřejného partnerského vztahu konfiguraci Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-173">This section helps you create, get, update, and delete the Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-public-peering"></a><span data-ttu-id="fb256-174">Vytvoření veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-174">To create Azure public peering</span></span>

1. <span data-ttu-id="fb256-175">Naimportujte modul PowerShellu pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-175">Import the PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="fb256-176">Abyste mohli začít používat rutiny pro ExpressRoute, musíte nainstalovat nejnovější verzi instalačního programu PowerShellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru do relace PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="fb256-176">You must install the latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import the Azure Resource Manager modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="fb256-177">Musíte spustit PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="fb256-177">You will need to run PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  <span data-ttu-id="fb256-178">Importujte všechny moduly AzureRM.* v rozsahu známé sémantické verze.</span><span class="sxs-lookup"><span data-stu-id="fb256-178">Import all of the AzureRM.* modules within the known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="fb256-179">Můžete můžete také naimportovat jenom modul select v rozsahu známé sémantické verze.</span><span class="sxs-lookup"><span data-stu-id="fb256-179">You can also just import a select module within the known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
```

  <span data-ttu-id="fb256-180">Přihlaste se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="fb256-180">Sign in to your account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="fb256-181">Vyberte předplatné, které chcete vytvořit okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-181">Select the subscription you want to create ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="fb256-182">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-182">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="fb256-183">Podle pokynů vytvořte [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-183">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="fb256-184">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil soukromý partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="fb256-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="fb256-185">V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="fb256-185">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="fb256-186">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí následující kroky.</span><span class="sxs-lookup"><span data-stu-id="fb256-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="fb256-187">Zkontrolujte okruh ExpressRoute a ověřte je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="fb256-187">Check the ExpressRoute circuit to ensure it is provisioned and also enabled.</span></span> <span data-ttu-id="fb256-188">Použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="fb256-188">Use the following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="fb256-189">Odpověď je stejný jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="fb256-189">The response is similar to the following example:</span></span>

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                      "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. <span data-ttu-id="fb256-190">Nakonfigurujte veřejný partnerský vztah Azure pro okruh.</span><span class="sxs-lookup"><span data-stu-id="fb256-190">Configure Azure public peering for the circuit.</span></span> <span data-ttu-id="fb256-191">Ujistěte se, že máte následující informace předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="fb256-191">Make sure that you have the following information before you proceed further.</span></span>

  * <span data-ttu-id="fb256-192">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-192">A /30 subnet for the primary link.</span></span> <span data-ttu-id="fb256-193">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="fb256-193">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="fb256-194">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-194">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="fb256-195">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="fb256-195">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="fb256-196">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="fb256-196">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="fb256-197">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="fb256-197">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="fb256-198">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="fb256-198">AS number for peering.</span></span> <span data-ttu-id="fb256-199">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="fb256-199">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="fb256-200">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="fb256-200">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="fb256-201">Spustit podle následujícího příkladu lze nakonfigurovat veřejný partnerský vztah Azure pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="fb256-201">Run the following example to configure Azure public peering for your circuit</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="fb256-202">Pokud se rozhodnete použít hodnotu hash MD5, použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="fb256-202">If you choose to use an MD5 hash, use the following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="fb256-203">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="fb256-203">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="to-view-azure-public-peering-details"></a><span data-ttu-id="fb256-204">Zobrazení podrobností veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-204">To view Azure public peering details</span></span>

<span data-ttu-id="fb256-205">Můžete získat podrobnosti o konfiguraci pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="fb256-205">You can get configuration details using the following cmdlet:</span></span>

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="to-update-azure-public-peering-configuration"></a><span data-ttu-id="fb256-206">Aktualizace konfigurace veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-206">To update Azure public peering configuration</span></span>

<span data-ttu-id="fb256-207">Libovolnou část konfigurace pomocí následujícího příkladu můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="fb256-207">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="fb256-208">V tomto příkladu je ID sítě VLAN okruhu aktualizováno z hodnoty 200 na hodnotu 600.</span><span class="sxs-lookup"><span data-stu-id="fb256-208">In this example, the VLAN ID of the circuit is being updated from 200 to 600.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-public-peering"></a><span data-ttu-id="fb256-209">Odstranění veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="fb256-209">To delete Azure public peering</span></span>

<span data-ttu-id="fb256-210">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující příklad:</span><span class="sxs-lookup"><span data-stu-id="fb256-210">You can remove your peering configuration by running the following example:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="microsoft-peering"></a><span data-ttu-id="fb256-211">Partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="fb256-211">Microsoft peering</span></span>

<span data-ttu-id="fb256-212">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu Microsoftu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-212">This section helps you create, get, update, and delete the Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb256-213">Okruhy ExpressRoute, které byly nakonfigurovány před 1. srpna 2017 partnerského vztahu Microsoftu bude mít všechny služby předpony inzerované prostřednictvím Microsoft partnerský vztah, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="fb256-213">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through the Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="fb256-214">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované dokud trasy filtr je připojen k okruhu.</span><span class="sxs-lookup"><span data-stu-id="fb256-214">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span> <span data-ttu-id="fb256-215">Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="fb256-215">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="to-create-microsoft-peering"></a><span data-ttu-id="fb256-216">Vytvoření partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="fb256-216">To create Microsoft peering</span></span>

1. <span data-ttu-id="fb256-217">Naimportujte modul PowerShellu pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-217">Import the PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="fb256-218">Abyste mohli začít používat rutiny pro ExpressRoute, musíte nainstalovat nejnovější verzi instalačního programu PowerShellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru do relace PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="fb256-218">You must install the latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import the Azure Resource Manager modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="fb256-219">Musíte spustit PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="fb256-219">You will need to run PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  <span data-ttu-id="fb256-220">Importujte všechny moduly AzureRM.* v rozsahu známé sémantické verze.</span><span class="sxs-lookup"><span data-stu-id="fb256-220">Import all of the AzureRM.* modules within the known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="fb256-221">Můžete můžete také naimportovat jenom modul select v rozsahu známé sémantické verze.</span><span class="sxs-lookup"><span data-stu-id="fb256-221">You can also just import a select module within the known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
  ```

  <span data-ttu-id="fb256-222">Přihlaste se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="fb256-222">Sign in to your account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="fb256-223">Vyberte předplatné, které chcete vytvořit okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-223">Select the subscription you want to create ExpressRoute circuit.</span></span>

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="fb256-224">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="fb256-224">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="fb256-225">Podle pokynů vytvořte [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-225">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="fb256-226">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete poskytovatele připojení požádat, aby povolil soukromý partnerský vztah Azure za vás.</span><span class="sxs-lookup"><span data-stu-id="fb256-226">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="fb256-227">V takovém případě nebudete muset postupovat podle pokynů uvedených v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="fb256-227">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="fb256-228">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí následující kroky.</span><span class="sxs-lookup"><span data-stu-id="fb256-228">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="fb256-229">Zkontrolujte okruh ExpressRoute a ujistěte se, že je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="fb256-229">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="fb256-230">Použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="fb256-230">Use the following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="fb256-231">Odpověď je stejný jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="fb256-231">The response is similar to the following example:</span></span>

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. <span data-ttu-id="fb256-232">Nakonfigurujte partnerský vztah Microsoftu pro okruh.</span><span class="sxs-lookup"><span data-stu-id="fb256-232">Configure Microsoft peering for the circuit.</span></span> <span data-ttu-id="fb256-233">Před pokračováním se ujistěte, že máte k dispozici následující informace.</span><span class="sxs-lookup"><span data-stu-id="fb256-233">Make sure that you have the following information before you proceed.</span></span>

  * <span data-ttu-id="fb256-234">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-234">A /30 subnet for the primary link.</span></span> <span data-ttu-id="fb256-235">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="fb256-235">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="fb256-236">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="fb256-236">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="fb256-237">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="fb256-237">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="fb256-238">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="fb256-238">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="fb256-239">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="fb256-239">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="fb256-240">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="fb256-240">AS number for peering.</span></span> <span data-ttu-id="fb256-241">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="fb256-241">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="fb256-242">Inzerované předpony: Musíte poskytnout seznam všech předpon, které plánujete inzerovat přes relaci protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="fb256-242">Advertised prefixes: You must provide a list of all prefixes you plan to advertise over the BGP session.</span></span> <span data-ttu-id="fb256-243">Přijímají se jenom předpony veřejných IP adres.</span><span class="sxs-lookup"><span data-stu-id="fb256-243">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="fb256-244">Pokud chcete odeslat sadu předpon, můžete odeslat seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="fb256-244">If you plan to send a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="fb256-245">Tyto předpony musí být v RIR/IRR zaregistrované na vás.</span><span class="sxs-lookup"><span data-stu-id="fb256-245">These prefixes must be registered to you in an RIR / IRR.</span></span>
  * <span data-ttu-id="fb256-246">**Volitelné -** zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované na číslo AS partnerského vztahu, můžete zadat číslo AS, do kterého jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="fb256-246">**Optional -** Customer ASN: If you are advertising prefixes that are not registered to the peering AS number, you can specify the AS number to which they are registered.</span></span>
  * <span data-ttu-id="fb256-247">Název registru směrování: Můžete zadat RIR/IRR, kde jsou předpony a číslo AS registrované.</span><span class="sxs-lookup"><span data-stu-id="fb256-247">Routing Registry Name: You can specify the RIR / IRR against which the AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="fb256-248">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="fb256-248">**Optional -** An MD5 hash if you choose to use one.</span></span>

   <span data-ttu-id="fb256-249">Umožňuje nakonfigurovat partnerský vztah Microsoftu pro váš okruh v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="fb256-249">Use the following example to configure Microsoft peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="to-get-microsoft-peering-details"></a><span data-ttu-id="fb256-250">Získání podrobností partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="fb256-250">To get Microsoft peering details</span></span>

<span data-ttu-id="fb256-251">Můžete získat podrobnosti o konfiguraci pomocí následující příklad:</span><span class="sxs-lookup"><span data-stu-id="fb256-251">You can get configuration details using the following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="to-update-microsoft-peering-configuration"></a><span data-ttu-id="fb256-252">Aktualizace konfigurace partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="fb256-252">To update Microsoft peering configuration</span></span>

<span data-ttu-id="fb256-253">Libovolnou část konfigurace pomocí následujícího příkladu můžete aktualizovat:</span><span class="sxs-lookup"><span data-stu-id="fb256-253">You can update any part of the configuration using the following example:</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-microsoft-peering"></a><span data-ttu-id="fb256-254">Odstranění partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="fb256-254">To delete Microsoft peering</span></span>

<span data-ttu-id="fb256-255">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="fb256-255">You can remove your peering configuration by running the following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a><span data-ttu-id="fb256-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb256-256">Next steps</span></span>

<span data-ttu-id="fb256-257">Dalším krokem je [Propojení virtuální sítě s okruhem ExpressRoute](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="fb256-257">Next step, [Link a VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

* <span data-ttu-id="fb256-258">Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="fb256-258">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="fb256-259">Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="fb256-259">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="fb256-260">Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fb256-260">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
