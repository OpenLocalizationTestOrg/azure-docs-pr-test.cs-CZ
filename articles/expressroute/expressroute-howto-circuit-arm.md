---
title: "Vytvoření a úprava okruhu ExpressRoute: prostředí PowerShell: Azure Resource Manager | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate, zřizovat, ověřte, aktualizovat, odstranit a zrušit jejich zřízení okruhu ExpressRoute."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f997182e-9b25-4a7a-b079-b004221dadcc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 8d76c577a9cffdd393abac1b76cccc27d92e9e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="96b40-103">Vytvoření a úprava okruhu ExpressRoute pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="96b40-103">Create and modify an ExpressRoute circuit using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="96b40-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="96b40-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="96b40-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="96b40-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="96b40-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="96b40-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="96b40-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="96b40-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="96b40-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="96b40-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="96b40-109">Tento článek popisuje, jak okruhu Azure ExpressRoute toocreate pomocí modelu nasazení Azure Resource Manager prostředí PowerShell pro rutiny a hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-109">This article describes how toocreate an Azure ExpressRoute circuit by using PowerShell cmdlets and hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="96b40-110">Tento článek také ukazuje, jak stav hello toocheck hello okruh, aktualizovat, nebo odstranit a zrušit jejich zřízení se.</span><span class="sxs-lookup"><span data-stu-id="96b40-110">This article also shows you how toocheck hello status of hello circuit, update it, or delete and deprovision it.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="96b40-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="96b40-111">Before you begin</span></span>
* <span data-ttu-id="96b40-112">Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-112">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="96b40-113">Další informace najdete v tématu [Přehled prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="96b40-113">For more information, see [Overview of Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="96b40-114">Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="96b40-114">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>


## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="96b40-115">Vytvořit a zřídit okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="96b40-115">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a><span data-ttu-id="96b40-116">1. Přihlaste se tooyour účet Azure a vybrat své předplatné</span><span class="sxs-lookup"><span data-stu-id="96b40-116">1. Sign in tooyour Azure account and select your subscription</span></span>
<span data-ttu-id="96b40-117">toobegin konfiguraci přihlášení tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="96b40-117">toobegin your configuration, sign in tooyour Azure account.</span></span> <span data-ttu-id="96b40-118">Následující příklady toohelp, ke kterým se připojujete pomocí hello:</span><span class="sxs-lookup"><span data-stu-id="96b40-118">Use hello following examples toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="96b40-119">Zkontrolujte předplatná hello pro účet hello:</span><span class="sxs-lookup"><span data-stu-id="96b40-119">Check hello subscriptions for hello account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="96b40-120">Vyberte hello předplatné, které chcete toocreate pro okruh ExpressRoute:</span><span class="sxs-lookup"><span data-stu-id="96b40-120">Select hello subscription that you want toocreate an ExpressRoute circuit for:</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="96b40-121">2. Získat hello seznam podporovaných zprostředkovatelů, umístění a šířek pásma</span><span class="sxs-lookup"><span data-stu-id="96b40-121">2. Get hello list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="96b40-122">Než vytvoříte okruh ExpressRoute, musíte hello seznam poskytovatelů podporovaných připojení, umístění a možnosti šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="96b40-122">Before you create an ExpressRoute circuit, you need hello list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="96b40-123">rutiny prostředí PowerShell text Hello **Get-AzureRmExpressRouteServiceProvider** vrátí tyto informace, které budete používat v dalších krocích:</span><span class="sxs-lookup"><span data-stu-id="96b40-123">hello PowerShell cmdlet **Get-AzureRmExpressRouteServiceProvider** returns this information, which you’ll use in later steps:</span></span>

```powershell
Get-AzureRmExpressRouteServiceProvider
```

<span data-ttu-id="96b40-124">Pokud poskytovatel připojení se nezobrazí, zkontrolujte toosee.</span><span class="sxs-lookup"><span data-stu-id="96b40-124">Check toosee if your connectivity provider is listed there.</span></span> <span data-ttu-id="96b40-125">Poznamenejte si hello následující informace.</span><span class="sxs-lookup"><span data-stu-id="96b40-125">Make a note of hello following information.</span></span> <span data-ttu-id="96b40-126">Budete je potřebovat později při vytvoření okruhu.</span><span class="sxs-lookup"><span data-stu-id="96b40-126">You'll need it later when you create a circuit.</span></span>

* <span data-ttu-id="96b40-127">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="96b40-127">Name</span></span>
* <span data-ttu-id="96b40-128">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="96b40-128">PeeringLocations</span></span>
* <span data-ttu-id="96b40-129">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="96b40-129">BandwidthsOffered</span></span>

<span data-ttu-id="96b40-130">Nyní jste připravené toocreate okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="96b40-130">You're now ready toocreate an ExpressRoute circuit.</span></span>   

### <a name="3-create-an-expressroute-circuit"></a><span data-ttu-id="96b40-131">3. Vytvoření okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="96b40-131">3. Create an ExpressRoute circuit</span></span>
<span data-ttu-id="96b40-132">Pokud ještě nemáte skupinu prostředků, je musíte vytvořit před vytvořením váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="96b40-132">If you don't already have a resource group, you must create one before you create your ExpressRoute circuit.</span></span> <span data-ttu-id="96b40-133">Můžete tak učinit spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="96b40-133">You can do so by running hello following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```


<span data-ttu-id="96b40-134">Hello následující příklad ukazuje, jak toocreate 200 MB/s ExpressRoute okruhu prostřednictvím Equinix ze Silicon Valley.</span><span class="sxs-lookup"><span data-stu-id="96b40-134">hello following example shows how toocreate a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="96b40-135">Pokud používáte k jinému zprostředkovateli a různá nastavení, dosaďte tyto informace při zkontrolujte vaši žádost.</span><span class="sxs-lookup"><span data-stu-id="96b40-135">If you're using a different provider and different settings, substitute that information when you make your request.</span></span> <span data-ttu-id="96b40-136">Následující Hello je požadavek příklad pro nový klíč služby:</span><span class="sxs-lookup"><span data-stu-id="96b40-136">hello following is an example request for a new service key:</span></span>

```powershell
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

<span data-ttu-id="96b40-137">Ujistěte se, že jste zadali správné úrovně SKU hello a řada SKU:</span><span class="sxs-lookup"><span data-stu-id="96b40-137">Make sure that you specify hello correct SKU tier and SKU family:</span></span>

* <span data-ttu-id="96b40-138">Úroveň SKU Určuje, zda je povolen ExpressRoute standard nebo doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="96b40-138">SKU tier determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="96b40-139">Můžete zadat *standardní* tooget hello standardní SKU nebo *Premium* pro doplněk premium hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-139">You can specify *Standard* tooget hello standard SKU or *Premium* for hello premium add-on.</span></span>
* <span data-ttu-id="96b40-140">Řada SKU Určuje typ fakturace hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-140">SKU family determines hello billing type.</span></span> <span data-ttu-id="96b40-141">Můžete zadat *Metereddata* pro plán měření podle objemu dat a *Unlimiteddata* pro plán neomezená data na úrovni.</span><span class="sxs-lookup"><span data-stu-id="96b40-141">You can specify *Metereddata* for a metered data plan and *Unlimiteddata* for an unlimited data plan.</span></span> <span data-ttu-id="96b40-142">Můžete změnit hello fakturace typu z *Metereddata* příliš*Unlimiteddata*, ale nemůžete změnit typ hello z *Unlimiteddata* příliš*Metereddata* .</span><span class="sxs-lookup"><span data-stu-id="96b40-142">You can change hello billing type from *Metereddata* too*Unlimiteddata*, but you can't change hello type from *Unlimiteddata* too*Metereddata*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96b40-143">Váš okruh ExpressRoute bude účtován z hello okamžiku, kdy se objeví klíč služby.</span><span class="sxs-lookup"><span data-stu-id="96b40-143">Your ExpressRoute circuit will be billed from hello moment a service key is issued.</span></span> <span data-ttu-id="96b40-144">Ujistěte se, když hello poskytovatele připojení je připraven tooprovision hello okruh provedení této operace.</span><span class="sxs-lookup"><span data-stu-id="96b40-144">Ensure that you perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

<span data-ttu-id="96b40-145">Hello odpovědi obsahuje klíč služby hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-145">hello response contains hello service key.</span></span> <span data-ttu-id="96b40-146">Podrobný popis všech parametrů hello můžete získat spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="96b40-146">You can get detailed descriptions of all hello parameters by running hello following command:</span></span>

```powershell
get-help New-AzureRmExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a><span data-ttu-id="96b40-147">4. Zobrazí seznam všech okruhy ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="96b40-147">4. List all ExpressRoute circuits</span></span>
<span data-ttu-id="96b40-148">seznam všech hello okruhy ExpressRoute, které jste vytvořili, tooget spustit hello **Get-AzureRmExpressRouteCircuit** příkaz:</span><span class="sxs-lookup"><span data-stu-id="96b40-148">tooget a list of all hello ExpressRoute circuits that you created, run hello **Get-AzureRmExpressRouteCircuit** command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

<span data-ttu-id="96b40-149">Hello odpověď bude vypadat podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="96b40-149">hello response will look similar toohello following example:</span></span>

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
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

<span data-ttu-id="96b40-150">Tyto informace kdykoli můžete načíst pomocí hello `Get-AzureRmExpressRouteCircuit` rutiny.</span><span class="sxs-lookup"><span data-stu-id="96b40-150">You can retrieve this information at any time by using hello `Get-AzureRmExpressRouteCircuit` cmdlet.</span></span> <span data-ttu-id="96b40-151">Provedení hello volání bez parametrů jsou uvedeny všechny okruhy hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-151">Making hello call with no parameters lists all hello circuits.</span></span> <span data-ttu-id="96b40-152">Zobrazí se klíč služby v hello *klíč ServiceKey* pole:</span><span class="sxs-lookup"><span data-stu-id="96b40-152">Your service key will be listed in hello *ServiceKey* field:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit
```


<span data-ttu-id="96b40-153">Hello odpověď bude vypadat podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="96b40-153">hello response will look similar toohello following example:</span></span>

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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


<span data-ttu-id="96b40-154">Podrobný popis všech parametrů hello můžete získat spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="96b40-154">You can get detailed descriptions of all hello parameters by running hello following command:</span></span>

```powershell
get-help Get-AzureRmExpressRouteCircuit -detailed
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="96b40-155">5. Odeslání poskytovatele připojení klíče tooyour hello služby pro zřizování</span><span class="sxs-lookup"><span data-stu-id="96b40-155">5. Send hello service key tooyour connectivity provider for provisioning</span></span>
<span data-ttu-id="96b40-156">*ServiceProviderProvisioningState* poskytuje informace o hello aktuální stav zřizování na straně hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="96b40-156">*ServiceProviderProvisioningState* provides information about hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="96b40-157">Stav poskytuje hello stavu na straně Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-157">Status provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="96b40-158">Další informace o zřizování stavy okruhu najdete v tématu hello [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.</span><span class="sxs-lookup"><span data-stu-id="96b40-158">For more information about circuit provisioning states, see hello [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="96b40-159">Když vytvoříte nový okruh ExpressRoute, bude mít okruh hello hello následující stav:</span><span class="sxs-lookup"><span data-stu-id="96b40-159">When you create a new ExpressRoute circuit, hello circuit will be in hello following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



<span data-ttu-id="96b40-160">okruh Hello změní toohello následující stavu, pokud je zprostředkovatel připojení k hello v procesu hello povolení pro vás:</span><span class="sxs-lookup"><span data-stu-id="96b40-160">hello circuit will change toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="96b40-161">Pro jste toobe možné toouse okruh ExpressRoute musí být v hello následující stav:</span><span class="sxs-lookup"><span data-stu-id="96b40-161">For you toobe able toouse an ExpressRoute circuit, it must be in hello following state:</span></span>

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="96b40-162">6. Pravidelně kontrolovat hello stav a stav hello hello okruh klíče</span><span class="sxs-lookup"><span data-stu-id="96b40-162">6. Periodically check hello status and hello state of hello circuit key</span></span>
<span data-ttu-id="96b40-163">Kontrola stavu hello a stavu hello hello okruh klíče umožňuje vědět, pokud se váš poskytovatel povolil váš okruh.</span><span class="sxs-lookup"><span data-stu-id="96b40-163">Checking hello status and hello state of hello circuit key lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="96b40-164">Po nakonfiguroval hello okruh *ServiceProviderProvisioningState* se zobrazí jako *zajištěno*, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="96b40-164">After hello circuit has been configured, *ServiceProviderProvisioningState* appears as *Provisioned*, as shown in hello following example:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


<span data-ttu-id="96b40-165">Hello odpověď bude vypadat podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="96b40-165">hello response will look similar toohello following example:</span></span>

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

### <a name="7-create-your-routing-configuration"></a><span data-ttu-id="96b40-166">7. Vytvořte vlastní konfiguraci směrování</span><span class="sxs-lookup"><span data-stu-id="96b40-166">7. Create your routing configuration</span></span>
<span data-ttu-id="96b40-167">Podrobné pokyny najdete v tématu hello [konfigurace směrování pro okruh ExpressRoute](expressroute-howto-routing-arm.md) článek toocreate a upravit partnerských vztahů pro okruh.</span><span class="sxs-lookup"><span data-stu-id="96b40-167">For step-by-step instructions, see hello [ExpressRoute circuit routing configuration](expressroute-howto-routing-arm.md) article toocreate and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96b40-168">Tyto pokyny platí pouze toocircuits, které jsou vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby.</span><span class="sxs-lookup"><span data-stu-id="96b40-168">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="96b40-169">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="96b40-169">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="96b40-170">8. Propojení virtuální sítě tooan okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="96b40-170">8. Link a virtual network tooan ExpressRoute circuit</span></span>
<span data-ttu-id="96b40-171">V dalším kroku propojte virtuální sítě tooyour okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="96b40-171">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="96b40-172">Použití hello [propojení virtuální sítě okruhů tooExpressRoute](expressroute-howto-linkvnet-arm.md) článek při práci s modelem nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-172">Use hello [Linking virtual networks tooExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with hello Resource Manager deployment model.</span></span>

## <a name="getting-hello-status-of-an-expressroute-circuit"></a><span data-ttu-id="96b40-173">Získávání hello stav okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="96b40-173">Getting hello status of an ExpressRoute circuit</span></span>
<span data-ttu-id="96b40-174">Tyto informace kdykoli můžete načíst pomocí hello **Get-AzureRmExpressRouteCircuit** rutiny.</span><span class="sxs-lookup"><span data-stu-id="96b40-174">You can retrieve this information at any time by using hello **Get-AzureRmExpressRouteCircuit** cmdlet.</span></span> <span data-ttu-id="96b40-175">Provedení hello volání bez parametrů jsou uvedeny všechny okruhy hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-175">Making hello call with no parameters lists all hello circuits.</span></span>

```powershell
Get-AzureRmExpressRouteCircuit
```


<span data-ttu-id="96b40-176">odpověď Hello bude podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="96b40-176">hello response will be similar toohello following example:</span></span>

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


<span data-ttu-id="96b40-177">Informace o konkrétní okruh ExpressRoute můžete získat pomocí předání hello název skupiny prostředků a název okruh jako parametr toohello volání:</span><span class="sxs-lookup"><span data-stu-id="96b40-177">You can get information on a specific ExpressRoute circuit by passing hello resource group name and circuit name as a parameter toohello call:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


<span data-ttu-id="96b40-178">Hello odpověď bude vypadat podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="96b40-178">hello response will look similar toohello following example:</span></span>

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


<span data-ttu-id="96b40-179">Podrobný popis všech parametrů hello můžete získat spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="96b40-179">You can get detailed descriptions of all hello parameters by running hello following command:</span></span>

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <span data-ttu-id="96b40-180"><a name="modify"></a>Úprava okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="96b40-180"><a name="modify"></a>Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="96b40-181">Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="96b40-181">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="96b40-182">Můžete provést následující bez výpadků hello:</span><span class="sxs-lookup"><span data-stu-id="96b40-182">You can do hello following with no downtime:</span></span>

* <span data-ttu-id="96b40-183">Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="96b40-183">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="96b40-184">Zvýšení hello šířka pásma okruhu ExpressRoute, pokud na portu hello je dostupné kapacity.</span><span class="sxs-lookup"><span data-stu-id="96b40-184">Increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="96b40-185">Přechod na starší verzi hello šířka pásma okruhu není podporována.</span><span class="sxs-lookup"><span data-stu-id="96b40-185">Downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="96b40-186">Změnit plán z tooUnlimited dat – měření podle objemu dat měření hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-186">Change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="96b40-187">Změna hello měření plán z tooMetered neomezená Data na úrovni, dat není podporováno.</span><span class="sxs-lookup"><span data-stu-id="96b40-187">Changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="96b40-188">Můžete povolit nebo zakázat *povolit klasické operace*.</span><span class="sxs-lookup"><span data-stu-id="96b40-188">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="96b40-189">Další informace o omezení a omezení, najdete v části toohello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="96b40-189">For more information on limits and limitations, refer toohello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

### <a name="tooenable-hello-expressroute-premium-add-on"></a><span data-ttu-id="96b40-190">doplněk ExpressRoute premium tooenable hello</span><span class="sxs-lookup"><span data-stu-id="96b40-190">tooenable hello ExpressRoute premium add-on</span></span>
<span data-ttu-id="96b40-191">Doplněk ExpressRoute premium hello u existujícího okruhu můžete povolit pomocí hello následující fragment kódu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="96b40-191">You can enable hello ExpressRoute premium add-on for your existing circuit by using hello following PowerShell snippet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

<span data-ttu-id="96b40-192">Hello okruhu bude mít teď hello ExpressRoute premium rozšíření funkce povolené.</span><span class="sxs-lookup"><span data-stu-id="96b40-192">hello circuit will now have hello ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="96b40-193">Začneme hned, jak byl úspěšně spuštěn příkaz hello fakturace pro funkci rozšíření hello premium.</span><span class="sxs-lookup"><span data-stu-id="96b40-193">We will begin billing you for hello premium add-on capability as soon as hello command has successfully run.</span></span>

### <a name="toodisable-hello-expressroute-premium-add-on"></a><span data-ttu-id="96b40-194">doplněk ExpressRoute premium toodisable hello</span><span class="sxs-lookup"><span data-stu-id="96b40-194">toodisable hello ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="96b40-195">Tato operace může selhat, pokud používáte prostředky, které jsou větší než co je povolené standardní okruh hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-195">This operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

<span data-ttu-id="96b40-196">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="96b40-196">Note hello following:</span></span>

* <span data-ttu-id="96b40-197">Můžete se downgradovat z úrovně premium toostandard, je nutné zajistit hello počet virtuálních sítí, které jsou propojeny toohello okruh je menší než 10.</span><span class="sxs-lookup"><span data-stu-id="96b40-197">Before you downgrade from premium toostandard, you must ensure that hello number of virtual networks that are linked toohello circuit is less than 10.</span></span> <span data-ttu-id="96b40-198">Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a jsme vám bude účtovat za zvýhodněné sazby.</span><span class="sxs-lookup"><span data-stu-id="96b40-198">If you don't do this, your update request fails, and we will bill you at premium rates.</span></span>
* <span data-ttu-id="96b40-199">Je nutné zrušit všechny virtuální sítě v jiných geopolitické oblasti.</span><span class="sxs-lookup"><span data-stu-id="96b40-199">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="96b40-200">Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a jsme vám bude účtovat za zvýhodněné sazby.</span><span class="sxs-lookup"><span data-stu-id="96b40-200">If you don't do this, your update request will fail, and we will bill you at premium rates.</span></span>
* <span data-ttu-id="96b40-201">Směrovací tabulka musí být menší než 4 000 tras pro soukromý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="96b40-201">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="96b40-202">Pokud vaše velikost tabulky trasy je větší než 4 000 tras, relace BGP hello zahodí a nebude možné opětovně povolena dokud hello počet předpon inzerovaných přejde nižší než 4 000.</span><span class="sxs-lookup"><span data-stu-id="96b40-202">If your route table size is greater than 4,000 routes, hello BGP session drops and won't be reenabled until hello number of advertised prefixes goes below 4,000.</span></span>

<span data-ttu-id="96b40-203">Doplněk ExpressRoute premium hello u existujícího okruhu hello můžete zakázat pomocí hello následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="96b40-203">You can disable hello ExpressRoute premium add-on for hello existing circuit by using hello following PowerShell cmdlet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a><span data-ttu-id="96b40-204">šířku pásma okruhu ExpressRoute tooupdate hello</span><span class="sxs-lookup"><span data-stu-id="96b40-204">tooupdate hello ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="96b40-205">Pro možnosti podporované šířky pásma pro zprostředkovatele, zkontrolujte hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="96b40-205">For supported bandwidth options for your provider, check hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span> <span data-ttu-id="96b40-206">Můžete vybrat libovolnou velikost, která je větší než velikost hello existujícím okruhem.</span><span class="sxs-lookup"><span data-stu-id="96b40-206">You can pick any size greater than hello size of your existing circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96b40-207">Pokud je na existující port hello nedostatečné kapacity, mohou mít okruh ExpressRoute toorecreate hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-207">You may have toorecreate hello ExpressRoute circuit if there is inadequate capacity on hello existing port.</span></span> <span data-ttu-id="96b40-208">Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat hello okruh.</span><span class="sxs-lookup"><span data-stu-id="96b40-208">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="96b40-209">Nelze zmenšit hello šířku pásma okruhu ExpressRoute bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="96b40-209">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="96b40-210">Přechod na starší verzi šířky pásma vyžaduje, abyste okruh ExpressRoute hello toodeprovision a pak znova nezajistíte nové okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="96b40-210">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 

<span data-ttu-id="96b40-211">Až se rozhodnete, jakou velikost, budete potřebovat, použijte následující příkaz tooresize hello váš okruh:</span><span class="sxs-lookup"><span data-stu-id="96b40-211">After you decide what size you need, use hello following command tooresize your circuit:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```


<span data-ttu-id="96b40-212">Váš okruh bude mít velikost na straně Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-212">Your circuit will be sized up on hello Microsoft side.</span></span> <span data-ttu-id="96b40-213">Pak bude nutné kontaktovat vaše připojení poskytovatele tooupdate konfigurace na jejich straně toomatch tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="96b40-213">Then you must contact your connectivity provider tooupdate configurations on their side toomatch this change.</span></span> <span data-ttu-id="96b40-214">Když provedete toto oznámení, začneme fakturace můžete pro možnost hello aktualizovat šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="96b40-214">After you make this notification, we will begin billing you for hello updated bandwidth option.</span></span>

### <a name="toomove-hello-sku-from-metered-toounlimited"></a><span data-ttu-id="96b40-215">toomove hello SKU z monitorovaných toounlimited</span><span class="sxs-lookup"><span data-stu-id="96b40-215">toomove hello SKU from metered toounlimited</span></span>
<span data-ttu-id="96b40-216">Hello SKU okruhu ExpressRoute můžete změnit pomocí hello následující fragment kódu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="96b40-216">You can change hello SKU of an ExpressRoute circuit by using hello following PowerShell snippet:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a><span data-ttu-id="96b40-217">toocontrol přístup toohello classic a Resource Manager prostředí</span><span class="sxs-lookup"><span data-stu-id="96b40-217">toocontrol access toohello classic and Resource Manager environments</span></span>
<span data-ttu-id="96b40-218">Přečtěte si pokyny hello v [okruhy ExpressRoute přesunout z modelu nasazení Resource Manager classic toohello hello](expressroute-howto-move-arm.md).</span><span class="sxs-lookup"><span data-stu-id="96b40-218">Review hello instructions in [Move ExpressRoute circuits from hello classic toohello Resource Manager deployment model](expressroute-howto-move-arm.md).</span></span>  

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="96b40-219">Zrušení zřízení a odstraňování okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="96b40-219">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="96b40-220">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="96b40-220">Note hello following:</span></span>

* <span data-ttu-id="96b40-221">Je nutné zrušit všechny virtuální sítě od hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="96b40-221">You must unlink all virtual networks from hello ExpressRoute circuit.</span></span> <span data-ttu-id="96b40-222">Pokud se tato operace nezdaří, zkontrolujte, že toosee, pokud žádné virtuální sítě propojené toohello okruh.</span><span class="sxs-lookup"><span data-stu-id="96b40-222">If this operation fails, check toosee if any virtual networks are linked toohello circuit.</span></span>
* <span data-ttu-id="96b40-223">Pokud je hello ExpressRoute okruhu poskytovatele služeb Stav zřizování **zřizování** nebo **zajištěno** na jejich straně, musíte pracovat se váš okruh hello toodeprovision zprostředkovatele služby.</span><span class="sxs-lookup"><span data-stu-id="96b40-223">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="96b40-224">Jsme bude i nadále tooreserve prostředky a dokud poskytovatele služeb hello dokončení zrušení zřízení hello okruhu a upozorní nám vám účtovat.</span><span class="sxs-lookup"><span data-stu-id="96b40-224">We will continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="96b40-225">Pokud má poskytovatel služeb hello zrušit okruh hello (Stav zřizování poskytovatele služeb hello je nastaven příliš**není zajišťováno**) pak můžete odstranit okruh hello.</span><span class="sxs-lookup"><span data-stu-id="96b40-225">If hello service provider has deprovisioned hello circuit (hello service provider provisioning state is set too**Not provisioned**) you can then delete hello circuit.</span></span> <span data-ttu-id="96b40-226">Tato akce ukončí fakturace pro okruh hello</span><span class="sxs-lookup"><span data-stu-id="96b40-226">This will stop billing for hello circuit</span></span>

<span data-ttu-id="96b40-227">Váš okruh ExpressRoute můžete odstranit spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="96b40-227">You can delete your ExpressRoute circuit by running hello following command:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a><span data-ttu-id="96b40-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96b40-228">Next steps</span></span>

<span data-ttu-id="96b40-229">Po vytvoření okruhu, ujistěte se, že hello následující:</span><span class="sxs-lookup"><span data-stu-id="96b40-229">After you create your circuit, make sure that you do hello following:</span></span>

* [<span data-ttu-id="96b40-230">Vytvoření a úprava směrování pro okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="96b40-230">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-arm.md)
* [<span data-ttu-id="96b40-231">Propojení vaší virtuální sítě tooyour okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="96b40-231">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)
