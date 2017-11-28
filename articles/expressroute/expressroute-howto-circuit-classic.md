---
title: "Vytvoření a úprava okruhu ExpressRoute: prostředí PowerShell: portál Azure classic | Microsoft Docs"
description: "Tento článek vás provede kroky pro vytváření a zřizování okruhu ExpressRoute. Tento článek také ukazuje, jak zkontrolovat stav, aktualizace nebo odstranění a zrušení zřízení váš okruh."
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
ms.openlocfilehash: 3b12bbb21ebf6a0160227c4a281c420cf192d6f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="018f3-104">Vytvoření a úprava okruhu ExpressRoute pomocí prostředí PowerShell (klasické)</span><span class="sxs-lookup"><span data-stu-id="018f3-104">Create and modify an ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="018f3-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="018f3-105">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="018f3-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="018f3-106">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="018f3-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="018f3-107">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="018f3-108">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="018f3-108">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="018f3-109">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="018f3-109">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="018f3-110">Tento článek vás provede kroky k vytvoření okruhu Azure ExpressRoute pomocí rutin prostředí PowerShell a modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="018f3-110">This article walks you through the steps to create an Azure ExpressRoute circuit by using PowerShell cmdlets and the classic deployment model.</span></span> <span data-ttu-id="018f3-111">Tento článek také ukazuje, jak zkontrolovat stav, aktualizace nebo odstranění a zrušení zřízení okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="018f3-111">This article also shows you how to check the status, update, or delete and deprovision an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


<span data-ttu-id="018f3-112">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="018f3-112">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a><span data-ttu-id="018f3-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="018f3-113">Before you begin</span></span>
### <a name="step-1-review-the-prerequisites-and-workflow-articles"></a><span data-ttu-id="018f3-114">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="018f3-114">Step 1.</span></span> <span data-ttu-id="018f3-115">Přečtěte si požadavky a pracovní postup články</span><span class="sxs-lookup"><span data-stu-id="018f3-115">Review the prerequisites and workflow articles</span></span>
<span data-ttu-id="018f3-116">Ujistěte se, že jste si přečetli [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="018f3-116">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>  

### <a name="step-2-install-the-latest-versions-of-the-azure-service-management-sm-powershell-modules"></a><span data-ttu-id="018f3-117">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="018f3-117">Step 2.</span></span> <span data-ttu-id="018f3-118">Nainstalujte nejnovější verzi modulů prostředí PowerShell Azure Service Management (SM)</span><span class="sxs-lookup"><span data-stu-id="018f3-118">Install the latest versions of the Azure Service Management (SM) PowerShell modules</span></span>
<span data-ttu-id="018f3-119">Postupujte podle pokynů v [Začínáme s rutinami prostředí Azure PowerShell](/powershell/azure/overview) podrobné pokyny ke konfiguraci počítače pro používání modulů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="018f3-119">Follow the instructions in [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview) for step-by-step guidance on how to configure your computer to use the Azure PowerShell modules.</span></span>

### <a name="step-3-log-in-to-your-azure-account-and-select-a-subscription"></a><span data-ttu-id="018f3-120">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="018f3-120">Step 3.</span></span> <span data-ttu-id="018f3-121">Přihlaste se k účtu Azure a vybrat odběr</span><span class="sxs-lookup"><span data-stu-id="018f3-121">Log in to your Azure account and select a subscription</span></span>
1. <span data-ttu-id="018f3-122">Otevřete konzolu PowerShellu se zvýšenými oprávněními a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="018f3-122">Open your PowerShell console with elevated rights and connect to your account.</span></span> <span data-ttu-id="018f3-123">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="018f3-123">Use the following example to help you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="018f3-124">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="018f3-124">Check the subscriptions for the account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="018f3-125">Máte-li více předplatných, vyberte předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="018f3-125">If you have more than one subscription, select the subscription that you want to use.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="018f3-126">Potom použijte následující rutinu k předplatnému Azure přidat do prostředí PowerShell pro model nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="018f3-126">Next, use the following cmdlet to add your Azure subscription to PowerShell for the classic deployment model.</span></span>

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="018f3-127">Vytvořit a zřídit okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-127">Create and provision an ExpressRoute circuit</span></span>
### <a name="step-1-import-the-powershell-modules-for-expressroute"></a><span data-ttu-id="018f3-128">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="018f3-128">Step 1.</span></span> <span data-ttu-id="018f3-129">Importovat další moduly Powershellu pro ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-129">Import the PowerShell modules for ExpressRoute</span></span>
 <span data-ttu-id="018f3-130">Pokud jste tak již neučinili, je nutné naimportovat moduly Azure a ExpressRoute do relace prostředí PowerShell mohli začít používat rutiny pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="018f3-130">If you have not already done so, you must import the Azure and ExpressRoute modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="018f3-131">Naimportovat moduly z umístění, které byly instalovány k v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="018f3-131">You import the modules from the location that they were installed to on your local computer.</span></span> <span data-ttu-id="018f3-132">V závislosti na metodě, které jste použili pro instalaci modulů může být jiný než následující příklad ukazuje umístění.</span><span class="sxs-lookup"><span data-stu-id="018f3-132">Depending on the method you used to install the modules, the location may be different than the following example shows.</span></span> <span data-ttu-id="018f3-133">V příkladu podle potřeby upravte.</span><span class="sxs-lookup"><span data-stu-id="018f3-133">Modify the example if necessary.</span></span>  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-the-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="018f3-134">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="018f3-134">Step 2.</span></span> <span data-ttu-id="018f3-135">Získejte seznam podporovaných zprostředkovatelů, umístění a šířek pásma</span><span class="sxs-lookup"><span data-stu-id="018f3-135">Get the list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="018f3-136">Než vytvoříte okruh ExpressRoute, musíte seznam poskytovatelů podporovaných připojení, umístění a možnosti šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="018f3-136">Before you create an ExpressRoute circuit, you need the list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="018f3-137">Rutiny prostředí PowerShell `Get-AzureDedicatedCircuitServiceProvider` vrátí tyto informace, které budete používat v dalších krocích:</span><span class="sxs-lookup"><span data-stu-id="018f3-137">The PowerShell cmdlet `Get-AzureDedicatedCircuitServiceProvider` returns this information, which you’ll use in later steps:</span></span>

    Get-AzureDedicatedCircuitServiceProvider

<span data-ttu-id="018f3-138">Zkontrolujte, pokud poskytovatel připojení se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="018f3-138">Check to see if your connectivity provider is listed there.</span></span> <span data-ttu-id="018f3-139">Poznamenejte si následující informace vzhledem k tomu, že budete je potřebovat později při vytvoření okruhu:</span><span class="sxs-lookup"><span data-stu-id="018f3-139">Make a note of the following information because you'll need it later when you create a circuit:</span></span>

* <span data-ttu-id="018f3-140">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="018f3-140">Name</span></span>
* <span data-ttu-id="018f3-141">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="018f3-141">PeeringLocations</span></span>
* <span data-ttu-id="018f3-142">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="018f3-142">BandwidthsOffered</span></span>

<span data-ttu-id="018f3-143">Nyní jste připraveni vytvořit okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="018f3-143">You're now ready to create an ExpressRoute circuit.</span></span>         

### <a name="step-3-create-an-expressroute-circuit"></a><span data-ttu-id="018f3-144">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="018f3-144">Step 3.</span></span> <span data-ttu-id="018f3-145">Vytvoření okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-145">Create an ExpressRoute circuit</span></span>
<span data-ttu-id="018f3-146">Následující příklad ukazuje, jak vytvořit 200 MB/s okruh ExpressRoute prostřednictvím Equinix ze Silicon Valley.</span><span class="sxs-lookup"><span data-stu-id="018f3-146">The following example shows how to create a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="018f3-147">Pokud používáte k jinému zprostředkovateli a různá nastavení, dosaďte tyto informace při zkontrolujte vaši žádost.</span><span class="sxs-lookup"><span data-stu-id="018f3-147">If you're using a different provider and different settings, substitute that information when you make your request.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="018f3-148">Váš okruh ExpressRoute bude účtován od okamžiku, kdy se objeví klíč služby.</span><span class="sxs-lookup"><span data-stu-id="018f3-148">Your ExpressRoute circuit will be billed from the moment a service key is issued.</span></span> <span data-ttu-id="018f3-149">Ujistěte se, při provádění této operace, pokud poskytovatel připojení je připraven ke zřízení okruhu.</span><span class="sxs-lookup"><span data-stu-id="018f3-149">Ensure that you perform this operation when the connectivity provider is ready to provision the circuit.</span></span>
> 
> 

<span data-ttu-id="018f3-150">Toto je požadavek příklad pro nový klíč služby:</span><span class="sxs-lookup"><span data-stu-id="018f3-150">The following is an example request for a new service key:</span></span>

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

<span data-ttu-id="018f3-151">Nebo, pokud chcete vytvořit okruh ExpressRoute s doplňkem premium, použijte následující příklad.</span><span class="sxs-lookup"><span data-stu-id="018f3-151">Or, if you want to create an ExpressRoute circuit with the premium add-on, use the following example.</span></span> <span data-ttu-id="018f3-152">Odkazovat [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium.</span><span class="sxs-lookup"><span data-stu-id="018f3-152">Refer to the [ExpressRoute FAQ](expressroute-faqs.md) for more details about the premium add-on.</span></span>

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


<span data-ttu-id="018f3-153">Odpověď bude obsahovat klíč služby.</span><span class="sxs-lookup"><span data-stu-id="018f3-153">The response will contain the service key.</span></span> <span data-ttu-id="018f3-154">Podrobný popis všech parametrů získáte spuštěním následující:</span><span class="sxs-lookup"><span data-stu-id="018f3-154">You can get detailed descriptions of all the parameters by running the following:</span></span>

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-the-expressroute-circuits"></a><span data-ttu-id="018f3-155">Krok 4.</span><span class="sxs-lookup"><span data-stu-id="018f3-155">Step 4.</span></span> <span data-ttu-id="018f3-156">Zobrazí seznam všech okruhy ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-156">List all the ExpressRoute circuits</span></span>
<span data-ttu-id="018f3-157">Můžete spustit `Get-AzureDedicatedCircuit` získat seznam všech okruhy ExpressRoute, které jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="018f3-157">You can run the `Get-AzureDedicatedCircuit` command to get a list of all the ExpressRoute circuits that you created:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="018f3-158">Odpověď bude vypadat přibližně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="018f3-158">The response will be something similar to the following example:</span></span>

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="018f3-159">Tyto informace kdykoli můžete načíst pomocí `Get-AzureDedicatedCircuit` rutiny.</span><span class="sxs-lookup"><span data-stu-id="018f3-159">You can retrieve this information at any time by using the `Get-AzureDedicatedCircuit` cmdlet.</span></span> <span data-ttu-id="018f3-160">Provádění volání bez parametrů jsou uvedeny všechny okruhů.</span><span class="sxs-lookup"><span data-stu-id="018f3-160">Making the call without any parameters lists all the circuits.</span></span> <span data-ttu-id="018f3-161">Zobrazí se klíč služby v *klíč ServiceKey* pole.</span><span class="sxs-lookup"><span data-stu-id="018f3-161">Your service key will be listed in the *ServiceKey* field.</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="018f3-162">Podrobný popis všech parametrů získáte spuštěním následující:</span><span class="sxs-lookup"><span data-stu-id="018f3-162">You can get detailed descriptions of all the parameters by running the following:</span></span>

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a><span data-ttu-id="018f3-163">Krok 5.</span><span class="sxs-lookup"><span data-stu-id="018f3-163">Step 5.</span></span> <span data-ttu-id="018f3-164">Klíč služby poslat svého poskytovatele připojení pro zřizování</span><span class="sxs-lookup"><span data-stu-id="018f3-164">Send the service key to your connectivity provider for provisioning</span></span>
<span data-ttu-id="018f3-165">*ServiceProviderProvisioningState* poskytuje informace o aktuálním stavu zřizování na straně poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="018f3-165">*ServiceProviderProvisioningState* provides information on the current state of provisioning on the service-provider side.</span></span> <span data-ttu-id="018f3-166">*Stav* poskytuje stav na straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="018f3-166">*Status* provides the state on the Microsoft side.</span></span> <span data-ttu-id="018f3-167">Další informace o zřizování stavy okruhu najdete v tématu [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.</span><span class="sxs-lookup"><span data-stu-id="018f3-167">For more information about circuit provisioning states, see the [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="018f3-168">Když vytvoříte nový okruh ExpressRoute, okruhu bude v následujícím stavu:</span><span class="sxs-lookup"><span data-stu-id="018f3-168">When you create a new ExpressRoute circuit, the circuit will be in the following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="018f3-169">Okruh přejde na následující stav, pokud poskytovatel připojení probíhá povolení pro vás:</span><span class="sxs-lookup"><span data-stu-id="018f3-169">The circuit will go to the following state when the connectivity provider is in the process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="018f3-170">Okruh ExpressRoute musí být v následujícím stavu, abyste mohli používat ji:</span><span class="sxs-lookup"><span data-stu-id="018f3-170">An ExpressRoute circuit must be in the following state for you to be able to use it:</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a><span data-ttu-id="018f3-171">Krok 6.</span><span class="sxs-lookup"><span data-stu-id="018f3-171">Step 6.</span></span> <span data-ttu-id="018f3-172">Pravidelně kontrolovat stav a stav okruhu klíče</span><span class="sxs-lookup"><span data-stu-id="018f3-172">Periodically check the status and the state of the circuit key</span></span>
<span data-ttu-id="018f3-173">Tímto způsobem zjistíte, když má poskytovatel povoleno váš okruh.</span><span class="sxs-lookup"><span data-stu-id="018f3-173">This lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="018f3-174">Po nakonfiguroval okruhu *ServiceProviderProvisioningState* se zobrazí jako *zajištěno* jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="018f3-174">After the circuit has been configured, *ServiceProviderProvisioningState* will display as *Provisioned* as shown in the following example:</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a><span data-ttu-id="018f3-175">Krok 7.</span><span class="sxs-lookup"><span data-stu-id="018f3-175">Step 7.</span></span> <span data-ttu-id="018f3-176">Vytvořte vlastní konfiguraci směrování</span><span class="sxs-lookup"><span data-stu-id="018f3-176">Create your routing configuration</span></span>
<span data-ttu-id="018f3-177">Odkazovat [konfigurace směrování pro okruh ExpressRoute (vytvoření a úprava okruhu partnerských vztahů)](expressroute-howto-routing-classic.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="018f3-177">Refer to the [ExpressRoute circuit routing configuration (create and modify circuit peerings)](expressroute-howto-routing-classic.md) article for step-by-step instructions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="018f3-178">Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby.</span><span class="sxs-lookup"><span data-stu-id="018f3-178">These instructions only apply to circuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="018f3-179">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="018f3-179">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="step-8-link-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="018f3-180">Krok 8.</span><span class="sxs-lookup"><span data-stu-id="018f3-180">Step 8.</span></span> <span data-ttu-id="018f3-181">Propojení virtuální sítě k okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-181">Link a virtual network to an ExpressRoute circuit</span></span>
<span data-ttu-id="018f3-182">V dalším kroku propojení virtuální sítě k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="018f3-182">Next, link a virtual network to your ExpressRoute circuit.</span></span> <span data-ttu-id="018f3-183">Odkazovat na [okruhy ExpressRoute propojování virtuálních sítí](expressroute-howto-linkvnet-classic.md) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="018f3-183">Refer to [Linking ExpressRoute circuits to virtual networks](expressroute-howto-linkvnet-classic.md) for step-by-step instructions.</span></span> <span data-ttu-id="018f3-184">Pokud potřebujete k vytvoření virtuální sítě pomocí modelu nasazení classic pro ExpressRoute najdete v tématu [vytvoření virtuální sítě pro ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="018f3-184">If you need to create a virtual network using the classic deployment model for ExpressRoute, see [Create a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

## <a name="getting-the-status-of-an-expressroute-circuit"></a><span data-ttu-id="018f3-185">Získávání stav okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-185">Getting the status of an ExpressRoute circuit</span></span>
<span data-ttu-id="018f3-186">Tyto informace kdykoli můžete načíst pomocí `Get-AzureCircuit` rutiny.</span><span class="sxs-lookup"><span data-stu-id="018f3-186">You can retrieve this information at any time by using the `Get-AzureCircuit` cmdlet.</span></span> <span data-ttu-id="018f3-187">Provádění volání bez parametrů jsou uvedeny všechny okruhů.</span><span class="sxs-lookup"><span data-stu-id="018f3-187">Making the call without any parameters lists all the circuits.</span></span>

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

<span data-ttu-id="018f3-188">Informace o konkrétní okruh ExpressRoute můžete získat pomocí předání klíče služby jako parametr pro volání.</span><span class="sxs-lookup"><span data-stu-id="018f3-188">You can get information on a specific ExpressRoute circuit by passing the service key as a parameter to the call.</span></span>

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


<span data-ttu-id="018f3-189">Podrobný popis všech parametrů můžete získat spuštěním v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="018f3-189">You can get detailed descriptions of all the parameters by running the following example:</span></span>

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="018f3-190">Úprava okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-190">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="018f3-191">Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="018f3-191">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="018f3-192">Můžete provést následující bez výpadků:</span><span class="sxs-lookup"><span data-stu-id="018f3-192">You can do the following with no downtime:</span></span>

* <span data-ttu-id="018f3-193">Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="018f3-193">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="018f3-194">Zadaný na tomto portu je dostupná kapacita, zvětšete šířku pásma okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="018f3-194">Increase the bandwidth of your ExpressRoute circuit provided there is capacity available on the port.</span></span> <span data-ttu-id="018f3-195">Všimněte si, že šířku pásma okruhu přechod na starší verzi není podporován.</span><span class="sxs-lookup"><span data-stu-id="018f3-195">Note that downgrading the bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="018f3-196">Změňte plán měření z – měření podle objemu dat na neomezená Data na úrovni.</span><span class="sxs-lookup"><span data-stu-id="018f3-196">Change the metering plan from Metered Data to Unlimited Data.</span></span> <span data-ttu-id="018f3-197">Všimněte si, že změna měření plánu z neomezená Data – měření podle objemu dat není podporován.</span><span class="sxs-lookup"><span data-stu-id="018f3-197">Note that changing the metering plan from Unlimited Data to Metered Data is not supported.</span></span>
* <span data-ttu-id="018f3-198">Můžete povolit nebo zakázat *povolit klasické operace*.</span><span class="sxs-lookup"><span data-stu-id="018f3-198">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="018f3-199">Odkazovat [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) Další informace o omezení.</span><span class="sxs-lookup"><span data-stu-id="018f3-199">Refer to the [ExpressRoute FAQ](expressroute-faqs.md) for more information on limits and limitations.</span></span>

### <a name="to-enable-the-expressroute-premium-add-on"></a><span data-ttu-id="018f3-200">Chcete-li povolit doplněk ExpressRoute premium</span><span class="sxs-lookup"><span data-stu-id="018f3-200">To enable the ExpressRoute premium add-on</span></span>
<span data-ttu-id="018f3-201">Doplněk ExpressRoute premium pro váš okruh. existující můžete povolit pomocí následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="018f3-201">You can enable the ExpressRoute premium add-on for your existing circuit by using the following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

<span data-ttu-id="018f3-202">Funkce doplněk ExpressRoute premium povoleno bude mít teď váš okruh.</span><span class="sxs-lookup"><span data-stu-id="018f3-202">Your circuit will now have the ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="018f3-203">Všimněte si, že jsme spustí hned, jak byl úspěšně spuštěn příkaz fakturace pro funkci rozšíření premium.</span><span class="sxs-lookup"><span data-stu-id="018f3-203">Note that we will start billing you for the premium add-on capability as soon as the command has successfully run.</span></span>

### <a name="to-disable-the-expressroute-premium-add-on"></a><span data-ttu-id="018f3-204">Chcete-li zakázat doplněk ExpressRoute premium</span><span class="sxs-lookup"><span data-stu-id="018f3-204">To disable the ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="018f3-205">Tato operace může selhat, pokud používáte prostředky, které jsou větší než co je povoleno pro standardní okruh.</span><span class="sxs-lookup"><span data-stu-id="018f3-205">This operation can fail if you're using resources that are greater than what is permitted for the standard circuit.</span></span>
> 
> 

#### <a name="considerations"></a><span data-ttu-id="018f3-206">Požadavky</span><span class="sxs-lookup"><span data-stu-id="018f3-206">Considerations</span></span>

* <span data-ttu-id="018f3-207">Je nutné zajistit, že počet virtuální sítě propojené ke okruhu je menší než 10 před downgradovat z úrovně premium na standard.</span><span class="sxs-lookup"><span data-stu-id="018f3-207">You must ensure that the number of virtual networks linked to the circuit is less than 10 before you downgrade from premium to standard.</span></span> <span data-ttu-id="018f3-208">Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a budete mít účtují zvýhodněné sazby.</span><span class="sxs-lookup"><span data-stu-id="018f3-208">If you don't do this, your update request will fail, and you'll be billed the premium rates.</span></span>
* <span data-ttu-id="018f3-209">Je nutné zrušit všechny virtuální sítě v jiných geopolitické oblasti.</span><span class="sxs-lookup"><span data-stu-id="018f3-209">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="018f3-210">Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a budete mít účtují zvýhodněné sazby.</span><span class="sxs-lookup"><span data-stu-id="018f3-210">If you don't do this, your update request will fail, and you'll be billed the premium rates.</span></span>
* <span data-ttu-id="018f3-211">Směrovací tabulka musí být menší než 4 000 tras pro soukromý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="018f3-211">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="018f3-212">Pokud má větší než 4 000 tras velikost trasy tabulce, relaci protokolu BGP bude vyřaďte a nebude možné opětovně povolena dokud počet předpon inzerovaných přejde nižší než 4 000.</span><span class="sxs-lookup"><span data-stu-id="018f3-212">If your route table size is greater than 4,000 routes, the BGP session will drop and won't be reenabled until the number of advertised prefixes goes below 4,000.</span></span>

#### <a name="disable-the-premium-add-on"></a><span data-ttu-id="018f3-213">Zakázat doplněk premium</span><span class="sxs-lookup"><span data-stu-id="018f3-213">Disable the premium add-on</span></span>
<span data-ttu-id="018f3-214">Doplněk ExpressRoute premium pro váš okruh. existující můžete zakázat pomocí následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="018f3-214">You can disable the ExpressRoute premium add-on for your existing circuit by using the following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a><span data-ttu-id="018f3-215">Chcete-li aktualizovat šířku pásma okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-215">To update the ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="018f3-216">Zkontrolujte [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) pro podporované možnosti šířky pásma pro poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="018f3-216">Check the [ExpressRoute FAQ](expressroute-faqs.md) for supported bandwidth options for your provider.</span></span> <span data-ttu-id="018f3-217">Můžete vybrat libovolnou velikost, která je větší než velikost existujícího okruhu tak dlouho, dokud umožňuje fyzického portu (na kterém je vytvořený váš okruh).</span><span class="sxs-lookup"><span data-stu-id="018f3-217">You can pick any size that is greater than the size of your existing circuit as long as the physical port (on which your circuit is created) allows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="018f3-218">Možná budete muset znovu vytvořit okruh ExpressRoute, pokud je nedostatečné kapacity na existující port.</span><span class="sxs-lookup"><span data-stu-id="018f3-218">You may have to recreate the ExpressRoute circuit if there is inadequate capacity on the existing port.</span></span> <span data-ttu-id="018f3-219">Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat okruh.</span><span class="sxs-lookup"><span data-stu-id="018f3-219">You cannot upgrade the circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="018f3-220">Nejde snížit šířku pásma okruhu ExpressRoute bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="018f3-220">You cannot reduce the bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="018f3-221">Přechod na starší verzi šířky pásma vyžaduje zrušení zřízení okruh ExpressRoute a pak znova nezajistíte nové okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="018f3-221">Downgrading bandwidth requires you to deprovision the ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> 

#### <a name="resize-a-circuit"></a><span data-ttu-id="018f3-222">Změna velikosti okruhu</span><span class="sxs-lookup"><span data-stu-id="018f3-222">Resize a circuit</span></span>

<span data-ttu-id="018f3-223">Jakmile se rozhodnete, jakou velikost potřebujete, můžete ke změně velikosti okruhu následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="018f3-223">After you decide what size you need, you can use the following command to resize your circuit:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="018f3-224">Váš okruh bude mít byla velikosti na straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="018f3-224">Your circuit will have been sized up on the Microsoft side.</span></span> <span data-ttu-id="018f3-225">Obraťte se svého poskytovatele připojení k aktualizaci konfigurace na jejich straně tak, aby odpovídaly tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="018f3-225">You must contact your connectivity provider to update configurations on their side to match this change.</span></span> <span data-ttu-id="018f3-226">Všimněte si, že jsme spustí fakturace můžete pro možnost aktualizované šířky pásma z tohoto bodu na.</span><span class="sxs-lookup"><span data-stu-id="018f3-226">Note that we will start billing you for the updated bandwidth option from this point on.</span></span>

<span data-ttu-id="018f3-227">Pokud se zobrazí chybová zpráva při zvýšení šířku pásma okruhu, znamená to, existuje ponecháno žádné dostatečnou šířku pásma na fyzického portu, kde se má vytvořit existujícím okruhem.</span><span class="sxs-lookup"><span data-stu-id="018f3-227">If you see the following error when increasing the circuit bandwidth, it means there is no sufficient bandwidth left on the physical port where your existing circuit is created.</span></span> <span data-ttu-id="018f3-228">Budete muset odstranit tento okruh a vytvořit nové okruh velikosti, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="018f3-228">You have to delete this circuit and create a new circuit of the size you need.</span></span> 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="018f3-229">Zrušení zřízení a odstraňování okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-229">Deprovisioning and deleting an ExpressRoute circuit</span></span>

### <a name="considerations"></a><span data-ttu-id="018f3-230">Požadavky</span><span class="sxs-lookup"><span data-stu-id="018f3-230">Considerations</span></span>

* <span data-ttu-id="018f3-231">Je nutné zrušit všechny virtuální sítě od okruhu ExpressRoute pro tato operace proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="018f3-231">You must unlink all virtual networks from the ExpressRoute circuit for this operation to succeed.</span></span> <span data-ttu-id="018f3-232">Zkontrolujte, zda jsou všechny virtuální sítě, které jsou propojeny s okruh, pokud se tato operace nezdaří.</span><span class="sxs-lookup"><span data-stu-id="018f3-232">Check to see if you have any virtual networks that are linked to the circuit if this operation fails.</span></span>
* <span data-ttu-id="018f3-233">Pokud je poskytovatel služby okruh ExpressRoute Stav zřizování **zřizování** nebo **zajištěno** , musíte pracovat se svým poskytovatelem služeb pro zrušení zřízení okruhu na jejich straně.</span><span class="sxs-lookup"><span data-stu-id="018f3-233">If the ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider to deprovision the circuit on their side.</span></span> <span data-ttu-id="018f3-234">Budeme nadále rezervovat prostředky a dokud poskytovatele služeb dokončení zrušení okruhu a upozorní nám vám účtovat.</span><span class="sxs-lookup"><span data-stu-id="018f3-234">We will continue to reserve resources and bill you until the service provider completes deprovisioning the circuit and notifies us.</span></span>
* <span data-ttu-id="018f3-235">Pokud má poskytovatel služeb zrušit okruhu (poskytovatele služeb Stav zřizování je nastavena na **není zajišťováno**) pak můžete odstranit okruh.</span><span class="sxs-lookup"><span data-stu-id="018f3-235">If the service provider has deprovisioned the circuit (the service provider provisioning state is set to **Not provisioned**) you can then delete the circuit.</span></span> <span data-ttu-id="018f3-236">To se zastaví fakturace pro okruh.</span><span class="sxs-lookup"><span data-stu-id="018f3-236">This will stop billing for the circuit.</span></span>

#### <a name="delete-a-circuit"></a><span data-ttu-id="018f3-237">Odstranit okruh</span><span class="sxs-lookup"><span data-stu-id="018f3-237">Delete a circuit</span></span>

<span data-ttu-id="018f3-238">Váš okruh ExpressRoute můžete odstranit tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="018f3-238">You can delete your ExpressRoute circuit by running the following command:</span></span>

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a><span data-ttu-id="018f3-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="018f3-239">Next steps</span></span>
<span data-ttu-id="018f3-240">Po vytvoření okruhu, ujistěte se, že provedete následující kroky:</span><span class="sxs-lookup"><span data-stu-id="018f3-240">After you create your circuit, make sure that you do the following:</span></span>

* [<span data-ttu-id="018f3-241">Vytvoření a úprava směrování pro okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-241">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-classic.md)
* [<span data-ttu-id="018f3-242">Propojení virtuální sítě k okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="018f3-242">Link your virtual network to your ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)

