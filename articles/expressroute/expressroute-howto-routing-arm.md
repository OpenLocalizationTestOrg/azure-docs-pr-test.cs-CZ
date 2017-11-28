---
title: "Jak tooconfigure směrování pro okruh ExpressRoute (partnerský vztah): Resource Manager: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tento článek vás provede kroky hello pro vytváření a zřizování hello soukromého a veřejného a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak stav hello toocheck, aktualizace nebo odstranění partnerských vztahů pro váš okruh."
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
ms.openlocfilehash: eb3ddf5c05a086ac3e22c64417e51381ef465921
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="16143-104">Vytvářet a upravovat partnerský vztah pro okruhu ExpressRoute pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="16143-104">Create and modify peering for an ExpressRoute circuit using PowerShell</span></span>

<span data-ttu-id="16143-105">Tento článek pomůže při vytváření a správě konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager hello pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="16143-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in hello Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="16143-106">Můžete také zkontrolovat stav hello, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-106">You can also check hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="16143-107">Pokud chcete toouse toowork jinou metodu s váš okruh, vyberte článek z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="16143-107">If you want toouse a different method toowork with your circuit, select an article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="16143-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="16143-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="16143-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="16143-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="16143-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="16143-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="16143-111">Video - soukromého partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="16143-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="16143-112">Video - veřejného partnerského vztahu</span><span class="sxs-lookup"><span data-stu-id="16143-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="16143-113">Video - partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="16143-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="16143-114">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="16143-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="16143-115">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="16143-115">Configuration prerequisites</span></span>

* <span data-ttu-id="16143-116">Budete potřebovat nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello.</span><span class="sxs-lookup"><span data-stu-id="16143-116">You will need hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="16143-117">Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="16143-117">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> 
* <span data-ttu-id="16143-118">Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md) stránce hello [požadavky na směrování](expressroute-routing.md) stránku a hello [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="16143-118">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) page, hello [routing requirements](expressroute-routing.md) page, and hello [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="16143-119">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="16143-120">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="16143-120">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="16143-121">Hello okruh ExpressRoute musí být ve stavu zřízený a povolený pro vás toobe možné toorun hello rutin v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="16143-121">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello cmdlets in this article.</span></span>

<span data-ttu-id="16143-122">Tyto pokyny platí pouze toocircuits vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="16143-122">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="16143-123">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="16143-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="16143-124">Jsme aktuálně neprovádějí ohlášení partnerské vztahy nakonfigurované poskytovateli služeb prostřednictvím portálu pro správu služby hello.</span><span class="sxs-lookup"><span data-stu-id="16143-124">We currently do not advertise peerings configured by service providers through hello service management portal.</span></span> <span data-ttu-id="16143-125">Pracujeme na tom, aby tato možnost brzy fungovala.</span><span class="sxs-lookup"><span data-stu-id="16143-125">We are working on enabling this capability soon.</span></span> <span data-ttu-id="16143-126">Před konfigurací partnerských vztahů protokolu BGP, zkontrolujte u vašeho poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="16143-126">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="16143-127">Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-127">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="16143-128">Partnerské vztahy můžete konfigurovat v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="16143-128">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="16143-129">Přesvědčte se, že dokončení hello konfiguraci každého partnerského vztahu jeden po druhém.</span><span class="sxs-lookup"><span data-stu-id="16143-129">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span> 

## <a name="azure-private-peering"></a><span data-ttu-id="16143-130">Soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="16143-130">Azure private peering</span></span>

<span data-ttu-id="16143-131">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello privátní konfigurace partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-131">This section helps you create, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="16143-132">toocreate soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="16143-132">toocreate Azure private peering</span></span>

1. <span data-ttu-id="16143-133">Importujte modul PowerShell hello pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-133">Import hello PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="16143-134">Je nutné nainstalovat hello nejnovější verzi instalačního programu Powershellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="16143-134">You must install hello latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import hello Azure Resource Manager modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="16143-135">Budete potřebovat toorun prostředí PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="16143-135">You will need toorun PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  <span data-ttu-id="16143-136">Importujte všechny moduly AzureRM.* hello hello známé sémantické verze. rozsah.</span><span class="sxs-lookup"><span data-stu-id="16143-136">Import all of hello AzureRM.* modules within hello known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="16143-137">Můžete můžete také naimportovat jenom modul select v hello známé sémantické verze. rozsah.</span><span class="sxs-lookup"><span data-stu-id="16143-137">You can also just import a select module within hello known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network 
  ```

  <span data-ttu-id="16143-138">Přihlaste se tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="16143-138">Sign in tooyour account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="16143-139">Vyberte předplatné hello chcete toocreate okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-139">Select hello subscription you want toocreate ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="16143-140">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-140">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="16143-141">Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="16143-141">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="16143-142">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás.</span><span class="sxs-lookup"><span data-stu-id="16143-142">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="16143-143">V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="16143-143">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="16143-144">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.</span><span class="sxs-lookup"><span data-stu-id="16143-144">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="16143-145">Zkontrolujte, zda je zřízený a také povolený toomake okruh ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="16143-145">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="16143-146">Použijte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="16143-146">Use hello following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="16143-147">odpověď Hello je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="16143-147">hello response is similar toohello following example:</span></span>

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
4. <span data-ttu-id="16143-148">Nakonfigurujte soukromý partnerský vztah Azure pro okruh hello.</span><span class="sxs-lookup"><span data-stu-id="16143-148">Configure Azure private peering for hello circuit.</span></span> <span data-ttu-id="16143-149">Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="16143-149">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="16143-150">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="16143-150">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="16143-151">Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="16143-151">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="16143-152">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="16143-152">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="16143-153">Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="16143-153">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="16143-154">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="16143-154">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="16143-155">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="16143-155">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="16143-156">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="16143-156">AS number for peering.</span></span> <span data-ttu-id="16143-157">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="16143-157">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="16143-158">Pro tento partnerský vztah můžete použít soukromé číslo AS.</span><span class="sxs-lookup"><span data-stu-id="16143-158">You can use a private AS number for this peering.</span></span> <span data-ttu-id="16143-159">Zkontrolujte, že nepoužíváte 65515.</span><span class="sxs-lookup"><span data-stu-id="16143-159">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="16143-160">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="16143-160">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="16143-161">Použijte následující příklad tooconfigure privátní partnerský vztah pro váš okruh Azure hello:</span><span class="sxs-lookup"><span data-stu-id="16143-161">Use hello following example tooconfigure Azure private peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="16143-162">Pokud si zvolíte toouse hodnotu hash MD5, použijte následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="16143-162">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="16143-163">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="16143-163">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="16143-164">tooview Azure privátní partnerský vztah podrobnosti</span><span class="sxs-lookup"><span data-stu-id="16143-164">tooview Azure private peering details</span></span>

<span data-ttu-id="16143-165">Podrobnosti o konfiguraci můžete získat pomocí hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="16143-165">You can get configuration details by using hello following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
```

### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="16143-166">tooupdate konfigurace soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="16143-166">tooupdate Azure private peering configuration</span></span>

<span data-ttu-id="16143-167">Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="16143-167">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="16143-168">V tomto příkladu je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 100 too500.</span><span class="sxs-lookup"><span data-stu-id="16143-168">In this example, hello VLAN ID of hello circuit is being updated from 100 too500.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="16143-169">toodelete soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="16143-169">toodelete Azure private peering</span></span>

<span data-ttu-id="16143-170">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="16143-170">You can remove your peering configuration by running hello following example:</span></span>

> [!WARNING]
> <span data-ttu-id="16143-171">Je nutné zajistit, že všechny virtuální sítě jsou před spuštěním tohoto příkladu odpojit od hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-171">You must ensure that all virtual networks are unlinked from hello ExpressRoute circuit before running this example.</span></span> 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-public-peering"></a><span data-ttu-id="16143-172">Veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="16143-172">Azure public peering</span></span>

<span data-ttu-id="16143-173">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello veřejné konfigurace partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-173">This section helps you create, get, update, and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="16143-174">toocreate veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="16143-174">toocreate Azure public peering</span></span>

1. <span data-ttu-id="16143-175">Importujte modul PowerShell hello pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-175">Import hello PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="16143-176">Je nutné nainstalovat hello nejnovější verzi instalačního programu Powershellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="16143-176">You must install hello latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import hello Azure Resource Manager modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="16143-177">Budete potřebovat toorun prostředí PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="16143-177">You will need toorun PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  <span data-ttu-id="16143-178">Importujte všechny moduly AzureRM.* hello hello známé sémantické verze. rozsah.</span><span class="sxs-lookup"><span data-stu-id="16143-178">Import all of hello AzureRM.* modules within hello known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="16143-179">Můžete můžete také naimportovat jenom modul select v hello známé sémantické verze. rozsah.</span><span class="sxs-lookup"><span data-stu-id="16143-179">You can also just import a select module within hello known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
```

  <span data-ttu-id="16143-180">Přihlaste se tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="16143-180">Sign in tooyour account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="16143-181">Vyberte předplatné hello chcete toocreate okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-181">Select hello subscription you want toocreate ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="16143-182">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-182">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="16143-183">Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="16143-183">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="16143-184">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás.</span><span class="sxs-lookup"><span data-stu-id="16143-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="16143-185">V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="16143-185">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="16143-186">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.</span><span class="sxs-lookup"><span data-stu-id="16143-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="16143-187">Zkontrolujte tooensure okruh ExpressRoute hello je zřízený a také povolený.</span><span class="sxs-lookup"><span data-stu-id="16143-187">Check hello ExpressRoute circuit tooensure it is provisioned and also enabled.</span></span> <span data-ttu-id="16143-188">Použijte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="16143-188">Use hello following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="16143-189">odpověď Hello je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="16143-189">hello response is similar toohello following example:</span></span>

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
4. <span data-ttu-id="16143-190">Nakonfigurujte veřejný partnerský vztah Azure pro okruh hello.</span><span class="sxs-lookup"><span data-stu-id="16143-190">Configure Azure public peering for hello circuit.</span></span> <span data-ttu-id="16143-191">Ujistěte se, že máte hello následující informace, než budete pokračovat dál.</span><span class="sxs-lookup"><span data-stu-id="16143-191">Make sure that you have hello following information before you proceed further.</span></span>

  * <span data-ttu-id="16143-192">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="16143-192">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="16143-193">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="16143-193">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="16143-194">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="16143-194">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="16143-195">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="16143-195">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="16143-196">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="16143-196">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="16143-197">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="16143-197">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="16143-198">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="16143-198">AS number for peering.</span></span> <span data-ttu-id="16143-199">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="16143-199">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="16143-200">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="16143-200">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="16143-201">Spustit hello následující příklad tooconfigure Azure veřejný partnerský vztah pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="16143-201">Run hello following example tooconfigure Azure public peering for your circuit</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="16143-202">Pokud si zvolíte toouse hodnotu hash MD5, použijte následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="16143-202">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="16143-203">Ujistěte se, že své číslo AS zadáváte jako partnerské číslo ASN, ne zákaznické číslo ASN.</span><span class="sxs-lookup"><span data-stu-id="16143-203">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="16143-204">tooview Azure veřejného partnerského vztahu podrobnosti</span><span class="sxs-lookup"><span data-stu-id="16143-204">tooview Azure public peering details</span></span>

<span data-ttu-id="16143-205">Můžete získat podrobnosti o konfiguraci pomocí následující rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="16143-205">You can get configuration details using hello following cmdlet:</span></span>

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="16143-206">tooupdate konfigurace veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="16143-206">tooupdate Azure public peering configuration</span></span>

<span data-ttu-id="16143-207">Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="16143-207">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="16143-208">V tomto příkladu je hello ID sítě VLAN hello okruhu aktualizováno z hodnoty 200 too600.</span><span class="sxs-lookup"><span data-stu-id="16143-208">In this example, hello VLAN ID of hello circuit is being updated from 200 too600.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="16143-209">toodelete veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="16143-209">toodelete Azure public peering</span></span>

<span data-ttu-id="16143-210">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="16143-210">You can remove your peering configuration by running hello following example:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="microsoft-peering"></a><span data-ttu-id="16143-211">Partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="16143-211">Microsoft peering</span></span>

<span data-ttu-id="16143-212">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu hello Microsoftu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-212">This section helps you create, get, update, and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="16143-213">Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím hello partnerský vztah Microsoftu, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="16143-213">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through hello Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="16143-214">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh.</span><span class="sxs-lookup"><span data-stu-id="16143-214">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span> <span data-ttu-id="16143-215">Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="16143-215">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="16143-216">partnerský vztah Microsoftu toocreate</span><span class="sxs-lookup"><span data-stu-id="16143-216">toocreate Microsoft peering</span></span>

1. <span data-ttu-id="16143-217">Importujte modul PowerShell hello pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-217">Import hello PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="16143-218">Je nutné nainstalovat hello nejnovější verzi instalačního programu Powershellu z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportovat moduly Azure Resource Manageru hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="16143-218">You must install hello latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import hello Azure Resource Manager modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="16143-219">Budete potřebovat toorun prostředí PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="16143-219">You will need toorun PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  <span data-ttu-id="16143-220">Importujte všechny moduly AzureRM.* hello hello známé sémantické verze. rozsah.</span><span class="sxs-lookup"><span data-stu-id="16143-220">Import all of hello AzureRM.* modules within hello known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="16143-221">Můžete můžete také naimportovat jenom modul select v hello známé sémantické verze. rozsah.</span><span class="sxs-lookup"><span data-stu-id="16143-221">You can also just import a select module within hello known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
  ```

  <span data-ttu-id="16143-222">Přihlaste se tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="16143-222">Sign in tooyour account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="16143-223">Vyberte předplatné hello chcete toocreate okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-223">Select hello subscription you want toocreate ExpressRoute circuit.</span></span>

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="16143-224">Vytvořte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16143-224">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="16143-225">Postupujte podle pokynů toocreate hello [okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho zřízený poskytovatelem připojení hello.</span><span class="sxs-lookup"><span data-stu-id="16143-225">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="16143-226">Pokud poskytovatel připojení nabízí spravované služby vrstvy 3, můžete požádat vašeho tooenable zprostředkovatele připojení k Azure privátní partnerský vztah za vás.</span><span class="sxs-lookup"><span data-stu-id="16143-226">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="16143-227">V takovém případě nebudete potřebovat toofollow pokynů uvedených v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="16143-227">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="16143-228">Ale pokud poskytovatel připojení nespravuje směrování, po vytvoření okruhu, pokračujte v konfiguraci pomocí hello další kroky.</span><span class="sxs-lookup"><span data-stu-id="16143-228">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="16143-229">Zkontrolujte, zda je zřízený a také povolený toomake okruh ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="16143-229">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="16143-230">Použijte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="16143-230">Use hello following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="16143-231">odpověď Hello je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="16143-231">hello response is similar toohello following example:</span></span>

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
4. <span data-ttu-id="16143-232">Nakonfigurujte partnerský vztah Microsoftu pro okruh hello.</span><span class="sxs-lookup"><span data-stu-id="16143-232">Configure Microsoft peering for hello circuit.</span></span> <span data-ttu-id="16143-233">Ujistěte se, že máte hello následující informace, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="16143-233">Make sure that you have hello following information before you proceed.</span></span>

  * <span data-ttu-id="16143-234">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="16143-234">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="16143-235">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="16143-235">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="16143-236">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="16143-236">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="16143-237">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="16143-237">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="16143-238">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="16143-238">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="16143-239">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="16143-239">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="16143-240">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="16143-240">AS number for peering.</span></span> <span data-ttu-id="16143-241">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="16143-241">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="16143-242">Inzerované předpony: musíte poskytnout seznam všech předpon plánování tooadvertise přes relaci protokolu BGP hello.</span><span class="sxs-lookup"><span data-stu-id="16143-242">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="16143-243">Přijímají se jenom předpony veřejných IP adres.</span><span class="sxs-lookup"><span data-stu-id="16143-243">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="16143-244">Pokud máte v plánu toosend sadu předpon, můžete odeslat seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="16143-244">If you plan toosend a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="16143-245">Tyto předpony musí být registrovaný tooyou u rir / IRR.</span><span class="sxs-lookup"><span data-stu-id="16143-245">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
  * <span data-ttu-id="16143-246">**Volitelné -** zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované toohello číslo AS partnerského vztahu, můžete zadat hello jako číslo toowhich jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="16143-246">**Optional -** Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span>
  * <span data-ttu-id="16143-247">Název registru směrování: Můžete zadat hello RIR / IRR, u které hello jako číslo a předpony jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="16143-247">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="16143-248">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="16143-248">**Optional -** An MD5 hash if you choose toouse one.</span></span>

   <span data-ttu-id="16143-249">Použijte následující příklad tooconfigure partnerský vztah Microsoftu pro váš okruh hello:</span><span class="sxs-lookup"><span data-stu-id="16143-249">Use hello following example tooconfigure Microsoft peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="tooget-microsoft-peering-details"></a><span data-ttu-id="16143-250">tooget podrobností partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="16143-250">tooget Microsoft peering details</span></span>

<span data-ttu-id="16143-251">Můžete získat podrobnosti o konfiguraci pomocí hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="16143-251">You can get configuration details using hello following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="16143-252">konfiguraci partnerského vztahu Microsoftu tooupdate</span><span class="sxs-lookup"><span data-stu-id="16143-252">tooupdate Microsoft peering configuration</span></span>

<span data-ttu-id="16143-253">Libovolnou část konfigurace hello pomocí hello následující ukázka můžete aktualizovat:</span><span class="sxs-lookup"><span data-stu-id="16143-253">You can update any part of hello configuration using hello following example:</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="16143-254">partnerský vztah Microsoftu toodelete</span><span class="sxs-lookup"><span data-stu-id="16143-254">toodelete Microsoft peering</span></span>

<span data-ttu-id="16143-255">Konfiguraci partnerského vztahu můžete odebrat spuštěním následující rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="16143-255">You can remove your peering configuration by running hello following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a><span data-ttu-id="16143-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16143-256">Next steps</span></span>

<span data-ttu-id="16143-257">Dalším krokem je [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="16143-257">Next step, [Link a VNet tooan ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

* <span data-ttu-id="16143-258">Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="16143-258">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="16143-259">Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="16143-259">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="16143-260">Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="16143-260">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
