---
title: "Vytvoření a úprava okruhu ExpressRoute: prostředí PowerShell: portál Azure classic | Microsoft Docs"
description: "Tento článek vás provede kroky hello pro vytváření a zřizování okruhu ExpressRoute. Tento článek také ukazuje, jak toocheck hello stav, aktualizovat, nebo odstranit a zrušit jejich zřízení váš okruh."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 9897c88776a2153ba22aa9ff328becb9f12b660b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="31a32-104">Vytvoření a úprava okruhu ExpressRoute pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="31a32-104">Create and modify an ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="31a32-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31a32-105">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="31a32-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="31a32-106">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="31a32-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="31a32-107">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="31a32-108">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="31a32-108">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="31a32-109">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="31a32-109">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="31a32-110">Tento článek vás provede kroky toocreate hello okruh Azure ExpressRoute pomocí modelu nasazení classic rutiny a hello prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="31a32-110">This article walks you through hello steps toocreate an Azure ExpressRoute circuit by using PowerShell cmdlets and hello classic deployment model.</span></span> <span data-ttu-id="31a32-111">Tento článek také ukazuje, jak toocheck hello stav, aktualizovat, nebo odstranit a zrušit jejich zřízení okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="31a32-111">This article also shows you how toocheck hello status, update, or delete and deprovision an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


<span data-ttu-id="31a32-112">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="31a32-112">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a><span data-ttu-id="31a32-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="31a32-113">Before you begin</span></span>
### <a name="step-1-review-hello-prerequisites-and-workflow-articles"></a><span data-ttu-id="31a32-114">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="31a32-114">Step 1.</span></span> <span data-ttu-id="31a32-115">Hello požadavky a pracovní postup články</span><span class="sxs-lookup"><span data-stu-id="31a32-115">Review hello prerequisites and workflow articles</span></span>
<span data-ttu-id="31a32-116">Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="31a32-116">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>  

### <a name="step-2-install-hello-latest-versions-of-hello-azure-service-management-sm-powershell-modules"></a><span data-ttu-id="31a32-117">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="31a32-117">Step 2.</span></span> <span data-ttu-id="31a32-118">Nainstalujte nejnovější verzi modulů prostředí PowerShell Azure Service Management (SM) hello hello</span><span class="sxs-lookup"><span data-stu-id="31a32-118">Install hello latest versions of hello Azure Service Management (SM) PowerShell modules</span></span>
<span data-ttu-id="31a32-119">Postupujte podle pokynů hello v [Začínáme s rutinami prostředí Azure PowerShell](/powershell/azure/overview) podrobný návod jak tooconfigure modulů prostředí Azure PowerShell hello toouse vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="31a32-119">Follow hello instructions in [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview) for step-by-step guidance on how tooconfigure your computer toouse hello Azure PowerShell modules.</span></span>

### <a name="step-3-log-in-tooyour-azure-account-and-select-a-subscription"></a><span data-ttu-id="31a32-120">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="31a32-120">Step 3.</span></span> <span data-ttu-id="31a32-121">Přihlaste se tooyour účet Azure a vybrat odběr</span><span class="sxs-lookup"><span data-stu-id="31a32-121">Log in tooyour Azure account and select a subscription</span></span>
1. <span data-ttu-id="31a32-122">Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="31a32-122">Open your PowerShell console with elevated rights and connect tooyour account.</span></span> <span data-ttu-id="31a32-123">Použijte následující příklad toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="31a32-123">Use hello following example toohelp you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="31a32-124">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-124">Check hello subscriptions for hello account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="31a32-125">Pokud máte více než jedno předplatné, vyberte hello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="31a32-125">If you have more than one subscription, select hello subscription that you want toouse.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="31a32-126">Pak pomocí následující rutiny tooadd hello tooPowerShell vaše předplatné Azure pro model nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-126">Next, use hello following cmdlet tooadd your Azure subscription tooPowerShell for hello classic deployment model.</span></span>

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="31a32-127">Vytvořit a zřídit okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="31a32-127">Create and provision an ExpressRoute circuit</span></span>
### <a name="step-1-import-hello-powershell-modules-for-expressroute"></a><span data-ttu-id="31a32-128">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="31a32-128">Step 1.</span></span> <span data-ttu-id="31a32-129">Naimportujte hello moduly Powershellu pro ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="31a32-129">Import hello PowerShell modules for ExpressRoute</span></span>
 <span data-ttu-id="31a32-130">Pokud jste tak již neučinili, je nutné naimportovat moduly Azure a ExpressRoute hello do relace prostředí PowerShell hello v pořadí toostart pomocí rutiny pro ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-130">If you have not already done so, you must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="31a32-131">Importovat hello moduly z hello umístění, které byly nainstalované tooon místního počítače.</span><span class="sxs-lookup"><span data-stu-id="31a32-131">You import hello modules from hello location that they were installed tooon your local computer.</span></span> <span data-ttu-id="31a32-132">V závislosti na metodě hello používá tooinstall hello moduly, může být jiný než hello následující příklad ukazuje umístění hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-132">Depending on hello method you used tooinstall hello modules, hello location may be different than hello following example shows.</span></span> <span data-ttu-id="31a32-133">Příklad hello podle potřeby upravte.</span><span class="sxs-lookup"><span data-stu-id="31a32-133">Modify hello example if necessary.</span></span>  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="31a32-134">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="31a32-134">Step 2.</span></span> <span data-ttu-id="31a32-135">Získat hello seznam podporovaných zprostředkovatelů, umístění a šířek pásma</span><span class="sxs-lookup"><span data-stu-id="31a32-135">Get hello list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="31a32-136">Než vytvoříte okruh ExpressRoute, musíte hello seznam poskytovatelů podporovaných připojení, umístění a možnosti šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="31a32-136">Before you create an ExpressRoute circuit, you need hello list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="31a32-137">rutiny prostředí PowerShell text Hello `Get-AzureDedicatedCircuitServiceProvider` vrátí tyto informace, které budete používat v dalších krocích:</span><span class="sxs-lookup"><span data-stu-id="31a32-137">hello PowerShell cmdlet `Get-AzureDedicatedCircuitServiceProvider` returns this information, which you’ll use in later steps:</span></span>

    Get-AzureDedicatedCircuitServiceProvider

<span data-ttu-id="31a32-138">Pokud poskytovatel připojení se nezobrazí, zkontrolujte toosee.</span><span class="sxs-lookup"><span data-stu-id="31a32-138">Check toosee if your connectivity provider is listed there.</span></span> <span data-ttu-id="31a32-139">Poznamenejte si následující informace, protože budete je potřebovat později při vytvoření okruhu hello:</span><span class="sxs-lookup"><span data-stu-id="31a32-139">Make a note of hello following information because you'll need it later when you create a circuit:</span></span>

* <span data-ttu-id="31a32-140">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="31a32-140">Name</span></span>
* <span data-ttu-id="31a32-141">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="31a32-141">PeeringLocations</span></span>
* <span data-ttu-id="31a32-142">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="31a32-142">BandwidthsOffered</span></span>

<span data-ttu-id="31a32-143">Nyní jste připravené toocreate okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="31a32-143">You're now ready toocreate an ExpressRoute circuit.</span></span>         

### <a name="step-3-create-an-expressroute-circuit"></a><span data-ttu-id="31a32-144">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="31a32-144">Step 3.</span></span> <span data-ttu-id="31a32-145">Vytvoření okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="31a32-145">Create an ExpressRoute circuit</span></span>
<span data-ttu-id="31a32-146">Hello následující příklad ukazuje, jak toocreate 200 MB/s ExpressRoute okruhu prostřednictvím Equinix ze Silicon Valley.</span><span class="sxs-lookup"><span data-stu-id="31a32-146">hello following example shows how toocreate a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="31a32-147">Pokud používáte k jinému zprostředkovateli a různá nastavení, dosaďte tyto informace při zkontrolujte vaši žádost.</span><span class="sxs-lookup"><span data-stu-id="31a32-147">If you're using a different provider and different settings, substitute that information when you make your request.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31a32-148">Váš okruh ExpressRoute bude účtován z hello okamžiku, kdy se objeví klíč služby.</span><span class="sxs-lookup"><span data-stu-id="31a32-148">Your ExpressRoute circuit will be billed from hello moment a service key is issued.</span></span> <span data-ttu-id="31a32-149">Ujistěte se, když hello poskytovatele připojení je připraven tooprovision hello okruh provedení této operace.</span><span class="sxs-lookup"><span data-stu-id="31a32-149">Ensure that you perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

<span data-ttu-id="31a32-150">Následující Hello je požadavek příklad pro nový klíč služby:</span><span class="sxs-lookup"><span data-stu-id="31a32-150">hello following is an example request for a new service key:</span></span>

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

<span data-ttu-id="31a32-151">Nebo, pokud chcete toocreate okruh ExpressRoute s doplňkem hello premium, hello použijte následující příklad.</span><span class="sxs-lookup"><span data-stu-id="31a32-151">Or, if you want toocreate an ExpressRoute circuit with hello premium add-on, use hello following example.</span></span> <span data-ttu-id="31a32-152">Odkazovat toohello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-152">Refer toohello [ExpressRoute FAQ](expressroute-faqs.md) for more details about hello premium add-on.</span></span>

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


<span data-ttu-id="31a32-153">Hello odpověď bude obsahovat klíč služby hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-153">hello response will contain hello service key.</span></span> <span data-ttu-id="31a32-154">Podrobný popis všech parametrů hello můžete získat spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="31a32-154">You can get detailed descriptions of all hello parameters by running hello following:</span></span>

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-hello-expressroute-circuits"></a><span data-ttu-id="31a32-155">Krok 4.</span><span class="sxs-lookup"><span data-stu-id="31a32-155">Step 4.</span></span> <span data-ttu-id="31a32-156">Zobrazí seznam všech okruhy ExpressRoute hello</span><span class="sxs-lookup"><span data-stu-id="31a32-156">List all hello ExpressRoute circuits</span></span>
<span data-ttu-id="31a32-157">Můžete spustit hello `Get-AzureDedicatedCircuit` příkaz tooget seznam všech hello okruhy ExpressRoute, které jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="31a32-157">You can run hello `Get-AzureDedicatedCircuit` command tooget a list of all hello ExpressRoute circuits that you created:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="31a32-158">odpověď Hello bude něco podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="31a32-158">hello response will be something similar toohello following example:</span></span>

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="31a32-159">Tyto informace kdykoli můžete načíst pomocí hello `Get-AzureDedicatedCircuit` rutiny.</span><span class="sxs-lookup"><span data-stu-id="31a32-159">You can retrieve this information at any time by using hello `Get-AzureDedicatedCircuit` cmdlet.</span></span> <span data-ttu-id="31a32-160">Provedení hello volání bez parametrů jsou uvedeny všechny okruhy hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-160">Making hello call without any parameters lists all hello circuits.</span></span> <span data-ttu-id="31a32-161">Zobrazí se klíč služby v hello *klíč ServiceKey* pole.</span><span class="sxs-lookup"><span data-stu-id="31a32-161">Your service key will be listed in hello *ServiceKey* field.</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="31a32-162">Podrobný popis všech parametrů hello můžete získat spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="31a32-162">You can get detailed descriptions of all hello parameters by running hello following:</span></span>

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="31a32-163">Krok 5.</span><span class="sxs-lookup"><span data-stu-id="31a32-163">Step 5.</span></span> <span data-ttu-id="31a32-164">Odeslání poskytovatele připojení klíče tooyour hello služby pro zřizování</span><span class="sxs-lookup"><span data-stu-id="31a32-164">Send hello service key tooyour connectivity provider for provisioning</span></span>
<span data-ttu-id="31a32-165">*ServiceProviderProvisioningState* poskytuje informace o hello aktuální stav zřizování na straně hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="31a32-165">*ServiceProviderProvisioningState* provides information on hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="31a32-166">*Stav* poskytuje hello stavu na straně Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-166">*Status* provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="31a32-167">Další informace o zřizování stavy okruhu najdete v tématu hello [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.</span><span class="sxs-lookup"><span data-stu-id="31a32-167">For more information about circuit provisioning states, see hello [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="31a32-168">Když vytvoříte nový okruh ExpressRoute, bude mít okruh hello hello následující stav:</span><span class="sxs-lookup"><span data-stu-id="31a32-168">When you create a new ExpressRoute circuit, hello circuit will be in hello following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="31a32-169">okruh Hello přejde toohello následující stavu, pokud je zprostředkovatel připojení k hello v procesu hello povolení pro vás:</span><span class="sxs-lookup"><span data-stu-id="31a32-169">hello circuit will go toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="31a32-170">Okruh ExpressRoute musí být v následujícím stavu můžete toobe možné toouse hello ho:</span><span class="sxs-lookup"><span data-stu-id="31a32-170">An ExpressRoute circuit must be in hello following state for you toobe able toouse it:</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="31a32-171">Krok 6.</span><span class="sxs-lookup"><span data-stu-id="31a32-171">Step 6.</span></span> <span data-ttu-id="31a32-172">Pravidelně kontrolovat hello stav a stav hello hello okruh klíče</span><span class="sxs-lookup"><span data-stu-id="31a32-172">Periodically check hello status and hello state of hello circuit key</span></span>
<span data-ttu-id="31a32-173">Tímto způsobem zjistíte, když má poskytovatel povoleno váš okruh.</span><span class="sxs-lookup"><span data-stu-id="31a32-173">This lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="31a32-174">Po nakonfiguroval hello okruh *ServiceProviderProvisioningState* se zobrazí jako *zajištěno* jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="31a32-174">After hello circuit has been configured, *ServiceProviderProvisioningState* will display as *Provisioned* as shown in hello following example:</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a><span data-ttu-id="31a32-175">Krok 7.</span><span class="sxs-lookup"><span data-stu-id="31a32-175">Step 7.</span></span> <span data-ttu-id="31a32-176">Vytvořte vlastní konfiguraci směrování</span><span class="sxs-lookup"><span data-stu-id="31a32-176">Create your routing configuration</span></span>
<span data-ttu-id="31a32-177">Odkazovat toohello [konfigurace směrování pro okruh ExpressRoute (vytvoření a úprava okruhu partnerských vztahů)](expressroute-howto-routing-classic.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="31a32-177">Refer toohello [ExpressRoute circuit routing configuration (create and modify circuit peerings)](expressroute-howto-routing-classic.md) article for step-by-step instructions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31a32-178">Tyto pokyny platí pouze toocircuits, které jsou vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby.</span><span class="sxs-lookup"><span data-stu-id="31a32-178">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="31a32-179">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="31a32-179">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="step-8-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="31a32-180">Krok 8.</span><span class="sxs-lookup"><span data-stu-id="31a32-180">Step 8.</span></span> <span data-ttu-id="31a32-181">Propojení virtuální sítě tooan okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="31a32-181">Link a virtual network tooan ExpressRoute circuit</span></span>
<span data-ttu-id="31a32-182">V dalším kroku propojte virtuální sítě tooyour okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="31a32-182">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="31a32-183">Odkazovat příliš[okruhy ExpressRoute propojení sítí toovirtual](expressroute-howto-linkvnet-classic.md) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="31a32-183">Refer too[Linking ExpressRoute circuits toovirtual networks](expressroute-howto-linkvnet-classic.md) for step-by-step instructions.</span></span> <span data-ttu-id="31a32-184">Pokud potřebujete toocreate virtuální sítě pomocí modelu nasazení classic hello pro ExpressRoute, přečtěte si [vytvoření virtuální sítě pro ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="31a32-184">If you need toocreate a virtual network using hello classic deployment model for ExpressRoute, see [Create a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

## <a name="getting-hello-status-of-an-expressroute-circuit"></a><span data-ttu-id="31a32-185">Získávání hello stav okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="31a32-185">Getting hello status of an ExpressRoute circuit</span></span>
<span data-ttu-id="31a32-186">Tyto informace kdykoli můžete načíst pomocí hello `Get-AzureCircuit` rutiny.</span><span class="sxs-lookup"><span data-stu-id="31a32-186">You can retrieve this information at any time by using hello `Get-AzureCircuit` cmdlet.</span></span> <span data-ttu-id="31a32-187">Provedení hello volání bez parametrů jsou uvedeny všechny okruhy hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-187">Making hello call without any parameters lists all hello circuits.</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="31a32-188">Informace o konkrétní okruh ExpressRoute můžete získat pomocí předání klíče služby hello jako parametr toohello volání.</span><span class="sxs-lookup"><span data-stu-id="31a32-188">You can get information on a specific ExpressRoute circuit by passing hello service key as a parameter toohello call.</span></span>

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


<span data-ttu-id="31a32-189">Podrobný popis všech parametrů hello můžete získat spuštěním hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="31a32-189">You can get detailed descriptions of all hello parameters by running hello following example:</span></span>

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="31a32-190">Úprava okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="31a32-190">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="31a32-191">Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="31a32-191">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="31a32-192">Můžete provést následující bez výpadků hello:</span><span class="sxs-lookup"><span data-stu-id="31a32-192">You can do hello following with no downtime:</span></span>

* <span data-ttu-id="31a32-193">Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="31a32-193">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="31a32-194">Zvýšení hello šířka pásma okruhu ExpressRoute, pokud na portu hello je dostupné kapacity.</span><span class="sxs-lookup"><span data-stu-id="31a32-194">Increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="31a32-195">Všimněte si, že hello šířka pásma okruhu přechod na starší verzi není podporován.</span><span class="sxs-lookup"><span data-stu-id="31a32-195">Note that downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="31a32-196">Změnit plán z tooUnlimited dat – měření podle objemu dat měření hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-196">Change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="31a32-197">Poznámka: Změna hello měření plán z tooMetered neomezená Data na úrovni, dat není podporováno.</span><span class="sxs-lookup"><span data-stu-id="31a32-197">Note that changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="31a32-198">Můžete povolit nebo zakázat *povolit klasické operace*.</span><span class="sxs-lookup"><span data-stu-id="31a32-198">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="31a32-199">Odkazovat toohello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) Další informace o omezení.</span><span class="sxs-lookup"><span data-stu-id="31a32-199">Refer toohello [ExpressRoute FAQ](expressroute-faqs.md) for more information on limits and limitations.</span></span>

### <a name="tooenable-hello-expressroute-premium-add-on"></a><span data-ttu-id="31a32-200">doplněk ExpressRoute premium tooenable hello</span><span class="sxs-lookup"><span data-stu-id="31a32-200">tooenable hello ExpressRoute premium add-on</span></span>
<span data-ttu-id="31a32-201">Doplněk ExpressRoute premium hello pro váš okruh. existující můžete povolit pomocí následující rutiny prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="31a32-201">You can enable hello ExpressRoute premium add-on for your existing circuit by using hello following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

<span data-ttu-id="31a32-202">Váš okruh bude mít teď hello ExpressRoute premium rozšíření funkce povolené.</span><span class="sxs-lookup"><span data-stu-id="31a32-202">Your circuit will now have hello ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="31a32-203">Všimněte si, že jsme spustí hned, jak byl úspěšně spuštěn příkaz hello fakturace pro funkci rozšíření hello premium.</span><span class="sxs-lookup"><span data-stu-id="31a32-203">Note that we will start billing you for hello premium add-on capability as soon as hello command has successfully run.</span></span>

### <a name="toodisable-hello-expressroute-premium-add-on"></a><span data-ttu-id="31a32-204">doplněk ExpressRoute premium toodisable hello</span><span class="sxs-lookup"><span data-stu-id="31a32-204">toodisable hello ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="31a32-205">Tato operace může selhat, pokud používáte prostředky, které jsou větší než co je povolené standardní okruh hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-205">This operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

#### <a name="considerations"></a><span data-ttu-id="31a32-206">Požadavky</span><span class="sxs-lookup"><span data-stu-id="31a32-206">Considerations</span></span>

* <span data-ttu-id="31a32-207">Je nutné zajistit, že hello počet virtuální sítě propojené toohello okruh je menší než 10 před downgradovat z úrovně premium toostandard.</span><span class="sxs-lookup"><span data-stu-id="31a32-207">You must ensure that hello number of virtual networks linked toohello circuit is less than 10 before you downgrade from premium toostandard.</span></span> <span data-ttu-id="31a32-208">Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a budete fakturovaná hello zvýhodněné sazby.</span><span class="sxs-lookup"><span data-stu-id="31a32-208">If you don't do this, your update request will fail, and you'll be billed hello premium rates.</span></span>
* <span data-ttu-id="31a32-209">Je nutné zrušit všechny virtuální sítě v jiných geopolitické oblasti.</span><span class="sxs-lookup"><span data-stu-id="31a32-209">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="31a32-210">Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a budete fakturovaná hello zvýhodněné sazby.</span><span class="sxs-lookup"><span data-stu-id="31a32-210">If you don't do this, your update request will fail, and you'll be billed hello premium rates.</span></span>
* <span data-ttu-id="31a32-211">Směrovací tabulka musí být menší než 4 000 tras pro soukromý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="31a32-211">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="31a32-212">Pokud vaše velikost tabulky trasy je větší než 4 000 tras, relace BGP hello bude vyřaďte a nebude možné opětovně povolena dokud hello počet předpon inzerovaných přejde nižší než 4 000.</span><span class="sxs-lookup"><span data-stu-id="31a32-212">If your route table size is greater than 4,000 routes, hello BGP session will drop and won't be reenabled until hello number of advertised prefixes goes below 4,000.</span></span>

#### <a name="disable-hello-premium-add-on"></a><span data-ttu-id="31a32-213">Zakázat doplněk premium hello</span><span class="sxs-lookup"><span data-stu-id="31a32-213">Disable hello premium add-on</span></span>
<span data-ttu-id="31a32-214">Pomocí následující rutiny prostředí PowerShell hello můžete zakázat doplněk ExpressRoute premium hello pro váš okruh. stávající:</span><span class="sxs-lookup"><span data-stu-id="31a32-214">You can disable hello ExpressRoute premium add-on for your existing circuit by using hello following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a><span data-ttu-id="31a32-215">šířku pásma okruhu ExpressRoute tooupdate hello</span><span class="sxs-lookup"><span data-stu-id="31a32-215">tooupdate hello ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="31a32-216">Zkontrolujte hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) pro podporované možnosti šířky pásma pro poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="31a32-216">Check hello [ExpressRoute FAQ](expressroute-faqs.md) for supported bandwidth options for your provider.</span></span> <span data-ttu-id="31a32-217">Můžete vybrat libovolnou velikost, která je větší než velikost hello existujícího okruhu tak dlouho, dokud umožňuje hello fyzického portu (na kterém je vytvořený váš okruh).</span><span class="sxs-lookup"><span data-stu-id="31a32-217">You can pick any size that is greater than hello size of your existing circuit as long as hello physical port (on which your circuit is created) allows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31a32-218">Pokud je na existující port hello nedostatečné kapacity, mohou mít okruh ExpressRoute toorecreate hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-218">You may have toorecreate hello ExpressRoute circuit if there is inadequate capacity on hello existing port.</span></span> <span data-ttu-id="31a32-219">Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat hello okruh.</span><span class="sxs-lookup"><span data-stu-id="31a32-219">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="31a32-220">Nelze zmenšit hello šířku pásma okruhu ExpressRoute bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="31a32-220">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="31a32-221">Přechod na starší verzi šířky pásma vyžaduje, abyste okruh ExpressRoute hello toodeprovision a pak znova nezajistíte nové okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="31a32-221">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> 

#### <a name="resize-a-circuit"></a><span data-ttu-id="31a32-222">Změna velikosti okruhu</span><span class="sxs-lookup"><span data-stu-id="31a32-222">Resize a circuit</span></span>

<span data-ttu-id="31a32-223">Až se rozhodnete, jakou velikost, je nutné, můžete použít následující příkaz tooresize hello váš okruh:</span><span class="sxs-lookup"><span data-stu-id="31a32-223">After you decide what size you need, you can use hello following command tooresize your circuit:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="31a32-224">Váš okruh bude mít byla velikosti na straně Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-224">Your circuit will have been sized up on hello Microsoft side.</span></span> <span data-ttu-id="31a32-225">Obraťte se na vaše připojení poskytovatele tooupdate konfigurace na jejich straně toomatch tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="31a32-225">You must contact your connectivity provider tooupdate configurations on their side toomatch this change.</span></span> <span data-ttu-id="31a32-226">Všimněte si, že jsme spustí fakturace můžete pro hello aktualizovat na možnost šířky pásma z tohoto bodu.</span><span class="sxs-lookup"><span data-stu-id="31a32-226">Note that we will start billing you for hello updated bandwidth option from this point on.</span></span>

<span data-ttu-id="31a32-227">Pokud se zobrazí následující chyba, když zvýšení šířku pásma okruhu hello hello, znamená to, existuje ponecháno žádné dostatečnou šířku pásma na hello fyzického portu, kde se má vytvořit existujícím okruhem.</span><span class="sxs-lookup"><span data-stu-id="31a32-227">If you see hello following error when increasing hello circuit bandwidth, it means there is no sufficient bandwidth left on hello physical port where your existing circuit is created.</span></span> <span data-ttu-id="31a32-228">Máte toodelete tento okruh a vytvořit nové okruh hello velikosti, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="31a32-228">You have toodelete this circuit and create a new circuit of hello size you need.</span></span> 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available tooperform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="31a32-229">Zrušení zřízení a odstraňování okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="31a32-229">Deprovisioning and deleting an ExpressRoute circuit</span></span>

### <a name="considerations"></a><span data-ttu-id="31a32-230">Požadavky</span><span class="sxs-lookup"><span data-stu-id="31a32-230">Considerations</span></span>

* <span data-ttu-id="31a32-231">Je nutné zrušit všechny virtuální sítě od hello okruh ExpressRoute pro tuto operaci toosucceed.</span><span class="sxs-lookup"><span data-stu-id="31a32-231">You must unlink all virtual networks from hello ExpressRoute circuit for this operation toosucceed.</span></span> <span data-ttu-id="31a32-232">Zkontrolujte toosee, pokud máte žádné virtuální sítě, které jsou propojené toohello okruh, pokud se tato operace nezdaří.</span><span class="sxs-lookup"><span data-stu-id="31a32-232">Check toosee if you have any virtual networks that are linked toohello circuit if this operation fails.</span></span>
* <span data-ttu-id="31a32-233">Pokud je hello ExpressRoute okruhu poskytovatele služeb Stav zřizování **zřizování** nebo **zajištěno** na jejich straně, musíte pracovat se váš okruh hello toodeprovision zprostředkovatele služby.</span><span class="sxs-lookup"><span data-stu-id="31a32-233">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="31a32-234">Jsme bude i nadále tooreserve prostředky a dokud poskytovatele služeb hello dokončení zrušení zřízení hello okruhu a upozorní nám vám účtovat.</span><span class="sxs-lookup"><span data-stu-id="31a32-234">We will continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="31a32-235">Pokud má poskytovatel služeb hello zrušit okruh hello (Stav zřizování poskytovatele služeb hello je nastaven příliš**není zajišťováno**) pak můžete odstranit okruh hello.</span><span class="sxs-lookup"><span data-stu-id="31a32-235">If hello service provider has deprovisioned hello circuit (hello service provider provisioning state is set too**Not provisioned**) you can then delete hello circuit.</span></span> <span data-ttu-id="31a32-236">To se zastaví fakturace hello okruh.</span><span class="sxs-lookup"><span data-stu-id="31a32-236">This will stop billing for hello circuit.</span></span>

#### <a name="delete-a-circuit"></a><span data-ttu-id="31a32-237">Odstranit okruh</span><span class="sxs-lookup"><span data-stu-id="31a32-237">Delete a circuit</span></span>

<span data-ttu-id="31a32-238">Váš okruh ExpressRoute můžete odstranit spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="31a32-238">You can delete your ExpressRoute circuit by running hello following command:</span></span>

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a><span data-ttu-id="31a32-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31a32-239">Next steps</span></span>
<span data-ttu-id="31a32-240">Po vytvoření okruhu, ujistěte se, že hello následující:</span><span class="sxs-lookup"><span data-stu-id="31a32-240">After you create your circuit, make sure that you do hello following:</span></span>

* [<span data-ttu-id="31a32-241">Vytvoření a úprava směrování pro okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="31a32-241">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-classic.md)
* [<span data-ttu-id="31a32-242">Propojení vaší virtuální sítě tooyour okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="31a32-242">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)

