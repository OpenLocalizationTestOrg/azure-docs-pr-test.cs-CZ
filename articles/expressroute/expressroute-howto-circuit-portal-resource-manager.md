---
title: "Vytvoření a úprava okruhu ExpressRoute: portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak vytvořit, zřízení, ověřte, aktualizovat, odstranit a zrušit jejich zřízení okruhu ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/07/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: e3721cd3c031622788f553e71a6555b844f3f7dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a><span data-ttu-id="63846-103">Vytvoření a úprava okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63846-103">Create and modify an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="63846-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="63846-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="63846-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="63846-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="63846-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="63846-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="63846-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="63846-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="63846-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="63846-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="63846-109">Tento článek popisuje postup vytvoření okruhu Azure ExpressRoute pomocí webu Azure portal a modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="63846-109">This article describes how to create an Azure ExpressRoute circuit by using the Azure portal and the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="63846-110">Následující kroky také ukazují zkontrolujte stav okruhu, aktualizaci, nebo odstranit a zrušit jejich zřízení se.</span><span class="sxs-lookup"><span data-stu-id="63846-110">The following steps also show you how to check the status of the circuit, update it, or delete and deprovision it.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="63846-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="63846-111">Before you begin</span></span>
* <span data-ttu-id="63846-112">Zkontrolujte [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="63846-112">Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="63846-113">Zajistěte, abyste měli přístup k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="63846-113">Ensure that you have access to the [Azure portal](https://portal.azure.com).</span></span>
* <span data-ttu-id="63846-114">Ujistěte se, zda máte oprávnění k vytvoření nových síťových prostředků.</span><span class="sxs-lookup"><span data-stu-id="63846-114">Ensure that you have permissions to create new networking resources.</span></span> <span data-ttu-id="63846-115">Pokud nemáte správná oprávnění, obraťte se na správce účtu.</span><span class="sxs-lookup"><span data-stu-id="63846-115">Contact your account administrator if you do not have the right permissions.</span></span>
* <span data-ttu-id="63846-116">Můžete [zobrazit video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) před zahájením pro lepší pochopení kroků.</span><span class="sxs-lookup"><span data-stu-id="63846-116">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) before beginning in order to better understand the steps.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="63846-117">Vytvořit a zřídit okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63846-117">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-to-the-azure-portal"></a><span data-ttu-id="63846-118">1. Přihlášení k webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="63846-118">1. Sign in to the Azure portal</span></span>
<span data-ttu-id="63846-119">V prohlížeči přejděte na web [Azure Portal](http://portal.azure.com) a přihlaste se pomocí svého účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="63846-119">From a browser, navigate to the [Azure portal](http://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="2-create-a-new-expressroute-circuit"></a><span data-ttu-id="63846-120">2. Vytvořit nový okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="63846-120">2. Create a new ExpressRoute circuit</span></span>
> [!IMPORTANT]
> <span data-ttu-id="63846-121">Váš okruh ExpressRoute bude účtován od okamžiku, kdy se objeví klíč služby.</span><span class="sxs-lookup"><span data-stu-id="63846-121">Your ExpressRoute circuit will be billed from the moment a service key is issued.</span></span> <span data-ttu-id="63846-122">Ujistěte se, při provádění této operace, pokud poskytovatel připojení je připraven ke zřízení okruhu.</span><span class="sxs-lookup"><span data-stu-id="63846-122">Ensure that you perform this operation when the connectivity provider is ready to provision the circuit.</span></span>
> 
> 

1. <span data-ttu-id="63846-123">Okruh ExpressRoute můžete vytvořit tak, že vyberete možnost vytvořit nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="63846-123">You can create an ExpressRoute circuit by selecting the option to create a new resource.</span></span> <span data-ttu-id="63846-124">Klikněte na tlačítko **nový** > **sítě** > **ExpressRoute**, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="63846-124">Click **New** > **Networking** > **ExpressRoute**, as shown in the following image:</span></span>
   
    ![Vytvoření okruhu ExpressRoute](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. <span data-ttu-id="63846-126">Po kliknutí na tlačítko **ExpressRoute**, uvidíte **okruh ExpressRoute vytvořit** okno.</span><span class="sxs-lookup"><span data-stu-id="63846-126">After you click **ExpressRoute**, you'll see the **Create ExpressRoute circuit** blade.</span></span> <span data-ttu-id="63846-127">Když jste vyplnění hodnoty v tomto okně, ujistěte se, že jste zadali správné úrovně SKU a data měření.</span><span class="sxs-lookup"><span data-stu-id="63846-127">When you're filling in the values on this blade, make sure that you specify the correct SKU tier and data metering.</span></span>
   
   * <span data-ttu-id="63846-128">**Úroveň** Určuje, zda je povoleno ExpressRoute standard nebo doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="63846-128">**Tier** determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="63846-129">Můžete zadat **standardní** získat standardní SKU nebo **Premium** pro doplněk premium.</span><span class="sxs-lookup"><span data-stu-id="63846-129">You can specify **Standard** to get the standard SKU or **Premium** for the premium add-on.</span></span>
   * <span data-ttu-id="63846-130">**Měření dat** Určuje typ fakturace.</span><span class="sxs-lookup"><span data-stu-id="63846-130">**Data metering** determines the billing type.</span></span> <span data-ttu-id="63846-131">Můžete zadat **Metered** pro plán měření podle objemu dat a **neomezený** pro plán neomezená data na úrovni.</span><span class="sxs-lookup"><span data-stu-id="63846-131">You can specify **Metered** for a metered data plan and **Unlimited** for an unlimited data plan.</span></span> <span data-ttu-id="63846-132">Můžete změnit typ fakturace z **Metered** k **neomezený**, ale nemůžete změnit typ z **neomezený** k **Metered**.</span><span class="sxs-lookup"><span data-stu-id="63846-132">Note that you can change the billing type from **Metered** to **Unlimited**, but you can't change the type from **Unlimited** to **Metered**.</span></span>
     
     ![Konfigurace SKU vrstvy a měření dat.](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> <span data-ttu-id="63846-134">Upozorňujeme, že označuje umístění partnerského vztahu [fyzické umístění](expressroute-locations.md) kde jste partnerského vztahu se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63846-134">Please be aware that the Peering Location indicates the [physical location](expressroute-locations.md) where you are peering with Microsoft.</span></span> <span data-ttu-id="63846-135">Toto je **není** propojené s vlastností "Umístění", která odkazuje na geography, kde se nachází na poskytovatele síťové prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="63846-135">This is **not** linked to "Location" property, which refers to the geography where the Azure Network Resource Provider is located.</span></span> <span data-ttu-id="63846-136">Při nesouvisí, je vhodné vybrat poskytovatele síťových prostředků geograficky v blízkosti umístění partnerského vztahu okruhu.</span><span class="sxs-lookup"><span data-stu-id="63846-136">While they are not related, it is a good practice to choose a Network Resource Provider geographically close to the Peering Location of the circuit.</span></span> 
> 
> 

### <a name="3-view-the-circuits-and-properties"></a><span data-ttu-id="63846-137">3. Zobrazit okruhy a vlastnosti</span><span class="sxs-lookup"><span data-stu-id="63846-137">3. View the circuits and properties</span></span>
<span data-ttu-id="63846-138">**Zobrazit všechny okruhy**</span><span class="sxs-lookup"><span data-stu-id="63846-138">**View all the circuits**</span></span>

<span data-ttu-id="63846-139">Můžete zobrazit všechny okruhů, které jste vytvořili výběrem **všechny prostředky** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="63846-139">You can view all the circuits that you created by selecting **All resources** on the left-side menu.</span></span>

![Okruhy zobrazení](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

<span data-ttu-id="63846-141">**Zobrazit vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="63846-141">**View the properties**</span></span>

    You can view the properties of the circuit by selecting it. On this blade, note the service key for the circuit. You must copy the circuit key for your circuit and pass it down to the service provider to complete the provisioning process. The circuit key is specific to your circuit.

![Zobrazení vlastností](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a><span data-ttu-id="63846-143">4. Klíč služby poslat svého poskytovatele připojení pro zřizování</span><span class="sxs-lookup"><span data-stu-id="63846-143">4. Send the service key to your connectivity provider for provisioning</span></span>
<span data-ttu-id="63846-144">V tomto okně **stav zprostředkovatele** poskytuje informace o aktuálním stavu zřizování na straně poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="63846-144">On this blade, **Provider status** provides information on the current state of provisioning on the service-provider side.</span></span> <span data-ttu-id="63846-145">**Stav okruhu** poskytuje stav na straně společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63846-145">**Circuit status** provides the state on the Microsoft side.</span></span> <span data-ttu-id="63846-146">Další informace o zřizování stavy okruhu najdete v tématu [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.</span><span class="sxs-lookup"><span data-stu-id="63846-146">For more information about circuit provisioning states, see the [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="63846-147">Když vytvoříte nový okruh ExpressRoute, okruhu bude v následujícím stavu:</span><span class="sxs-lookup"><span data-stu-id="63846-147">When you create a new ExpressRoute circuit, the circuit will be in the following state:</span></span>

<span data-ttu-id="63846-148">Stav zprostředkovatele: není zajišťováno</span><span class="sxs-lookup"><span data-stu-id="63846-148">Provider status: Not provisioned</span></span><BR>
<span data-ttu-id="63846-149">Stav okruhu: povoleno</span><span class="sxs-lookup"><span data-stu-id="63846-149">Circuit status: Enabled</span></span>

![Zahájení procesu zřizování](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

<span data-ttu-id="63846-151">Okruh se změní na následující stav, když probíhá povolení můžete poskytovatele připojení:</span><span class="sxs-lookup"><span data-stu-id="63846-151">The circuit will change to the following state when the connectivity provider is in the process of enabling it for you:</span></span>

<span data-ttu-id="63846-152">Stav zprostředkovatele: zřizování</span><span class="sxs-lookup"><span data-stu-id="63846-152">Provider status: Provisioning</span></span><BR>
<span data-ttu-id="63846-153">Stav okruhu: povoleno</span><span class="sxs-lookup"><span data-stu-id="63846-153">Circuit status: Enabled</span></span>

<span data-ttu-id="63846-154">Abyste mohli používat okruh ExpressRoute musí být v následujícím stavu:</span><span class="sxs-lookup"><span data-stu-id="63846-154">For you to be able to use an ExpressRoute circuit, it must be in the following state:</span></span>

<span data-ttu-id="63846-155">Stav zprostředkovatele: zřídit</span><span class="sxs-lookup"><span data-stu-id="63846-155">Provider status: Provisioned</span></span><BR>
<span data-ttu-id="63846-156">Stav okruhu: povoleno</span><span class="sxs-lookup"><span data-stu-id="63846-156">Circuit status: Enabled</span></span>

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a><span data-ttu-id="63846-157">5. Pravidelně kontrolovat stav a stav okruhu klíče</span><span class="sxs-lookup"><span data-stu-id="63846-157">5. Periodically check the status and the state of the circuit key</span></span>
<span data-ttu-id="63846-158">Můžete zobrazit vlastnosti okruhu, který máte zájem výběrem.</span><span class="sxs-lookup"><span data-stu-id="63846-158">You can view the properties of the circuit that you're interested in by selecting it.</span></span> <span data-ttu-id="63846-159">Zkontrolujte **stav zprostředkovatele** a ujistěte se, že se přesunul na **zajištěno** než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="63846-159">Check the **Provider status** and ensure that it has moved to **Provisioned** before you continue.</span></span>

![Stav okruhu a zprostředkovatele](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a><span data-ttu-id="63846-161">6. Vytvořte vlastní konfiguraci směrování</span><span class="sxs-lookup"><span data-stu-id="63846-161">6. Create your routing configuration</span></span>
<span data-ttu-id="63846-162">Podrobné pokyny najdete v části [konfigurace směrování pro okruh ExpressRoute](expressroute-howto-routing-portal-resource-manager.md) článek k vytvoření a úpravám partnerských vztahů pro okruh.</span><span class="sxs-lookup"><span data-stu-id="63846-162">For step-by-step instructions, refer to the [ExpressRoute circuit routing configuration](expressroute-howto-routing-portal-resource-manager.md) article to create and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="63846-163">Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby.</span><span class="sxs-lookup"><span data-stu-id="63846-163">These instructions only apply to circuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="63846-164">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="63846-164">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="63846-165">7. Propojení virtuální sítě k okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63846-165">7. Link a virtual network to an ExpressRoute circuit</span></span>
<span data-ttu-id="63846-166">V dalším kroku propojení virtuální sítě k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="63846-166">Next, link a virtual network to your ExpressRoute circuit.</span></span> <span data-ttu-id="63846-167">Použití [propojování virtuálních sítí pro okruhy ExpressRoute](expressroute-howto-linkvnet-arm.md) článek při práci s modelem nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="63846-167">Use the [Linking virtual networks to ExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with the Resource Manager deployment model.</span></span>

## <a name="getting-the-status-of-an-expressroute-circuit"></a><span data-ttu-id="63846-168">Získávání stav okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63846-168">Getting the status of an ExpressRoute circuit</span></span>
<span data-ttu-id="63846-169">Výběrem můžete zobrazit stav okruhu.</span><span class="sxs-lookup"><span data-stu-id="63846-169">You can view the status of a circuit by selecting it.</span></span> 

![Stav okruhu ExpressRoute](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="63846-171">Úprava okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63846-171">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="63846-172">Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="63846-172">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="63846-173">Můžete provést následující bez výpadků:</span><span class="sxs-lookup"><span data-stu-id="63846-173">You can do the following with no downtime:</span></span>

* <span data-ttu-id="63846-174">Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="63846-174">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="63846-175">Zadaný na tomto portu je dostupná kapacita, zvětšete šířku pásma okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="63846-175">Increase the bandwidth of your ExpressRoute circuit provided there is capacity available on the port.</span></span> <span data-ttu-id="63846-176">Všimněte si, že šířku pásma okruhu přechod na starší verzi není podporován.</span><span class="sxs-lookup"><span data-stu-id="63846-176">Note that downgrading the bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="63846-177">Změňte plán měření z – měření podle objemu dat na neomezená Data na úrovni.</span><span class="sxs-lookup"><span data-stu-id="63846-177">Change the metering plan from Metered Data to Unlimited Data.</span></span> <span data-ttu-id="63846-178">Všimněte si, že změna měření plánu z neomezená Data – měření podle objemu dat není podporován.</span><span class="sxs-lookup"><span data-stu-id="63846-178">Note that changing the metering plan from Unlimited Data to Metered Data is not supported.</span></span>
* <span data-ttu-id="63846-179">Můžete povolit nebo zakázat *povolit klasické operace*.</span><span class="sxs-lookup"><span data-stu-id="63846-179">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="63846-180">Další informace o omezení a omezení, najdete v části [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="63846-180">For more information on limits and limitations, refer to the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

<span data-ttu-id="63846-181">Chcete-li upravit okruh ExpressRoute, klikněte na **konfigurace** jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="63846-181">To modify an ExpressRoute circuit, click on the **Configuration** as shown in the figure below.</span></span>

![Úprava okruhu](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

<span data-ttu-id="63846-183">Můžete upravit šířku pásma, SKU, model fakturace a povolit klasické operace v rámci konfigurace okna.</span><span class="sxs-lookup"><span data-stu-id="63846-183">You can modify the bandwidth, SKU, billing model and allow classic operations within the configuration blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="63846-184">Možná budete muset znovu vytvořit okruh ExpressRoute, pokud je nedostatečné kapacity na existující port.</span><span class="sxs-lookup"><span data-stu-id="63846-184">You may have to recreate the ExpressRoute circuit if there is inadequate capacity on the existing port.</span></span> <span data-ttu-id="63846-185">Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat okruh.</span><span class="sxs-lookup"><span data-stu-id="63846-185">You cannot upgrade the circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="63846-186">Nejde snížit šířku pásma okruhu ExpressRoute bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="63846-186">You cannot reduce the bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="63846-187">Přechod na starší verzi šířky pásma vyžaduje zrušení zřízení okruh ExpressRoute a pak znova nezajistíte nové okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="63846-187">Downgrading bandwidth requires you to deprovision the ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> <span data-ttu-id="63846-188">Zakázat doplněk premium operace může selhat, pokud používáte prostředky, které jsou větší než co je povoleno pro standardní okruh.</span><span class="sxs-lookup"><span data-stu-id="63846-188">Disable premium add-on operation can fail if you're using resources that are greater than what is permitted for the standard circuit.</span></span>
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="63846-189">Zrušení zřízení a odstraňování okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63846-189">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="63846-190">Váš okruh ExpressRoute můžete odstranit výběrem **odstranit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="63846-190">You can delete your ExpressRoute circuit by selecting the **delete** icon.</span></span> <span data-ttu-id="63846-191">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="63846-191">Note the following:</span></span>

* <span data-ttu-id="63846-192">Je nutné zrušit všechny virtuální sítě od okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="63846-192">You must unlink all virtual networks from the ExpressRoute circuit.</span></span> <span data-ttu-id="63846-193">Pokud se tato operace nezdaří, zkontrolujte, zda jsou všechny virtuální sítě propojené ke okruh.</span><span class="sxs-lookup"><span data-stu-id="63846-193">If this operation fails, check whether any virtual networks are linked to the circuit.</span></span>
* <span data-ttu-id="63846-194">Pokud je poskytovatel služby okruh ExpressRoute Stav zřizování **zřizování** nebo **zajištěno** , musíte pracovat se svým poskytovatelem služeb pro zrušení zřízení okruhu na jejich straně.</span><span class="sxs-lookup"><span data-stu-id="63846-194">If the ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider to deprovision the circuit on their side.</span></span> <span data-ttu-id="63846-195">Budeme nadále rezervovat prostředky a dokud poskytovatele služeb dokončení zrušení okruhu a upozorní nám vám účtovat.</span><span class="sxs-lookup"><span data-stu-id="63846-195">We will continue to reserve resources and bill you until the service provider completes deprovisioning the circuit and notifies us.</span></span>
* <span data-ttu-id="63846-196">Pokud má poskytovatel služeb zrušit okruhu (poskytovatele služeb Stav zřizování je nastavena na **není zajišťováno**) pak můžete odstranit okruh.</span><span class="sxs-lookup"><span data-stu-id="63846-196">If the service provider has deprovisioned the circuit (the service provider provisioning state is set to **Not provisioned**) you can then delete the circuit.</span></span> <span data-ttu-id="63846-197">Tato akce ukončí fakturace pro okruh</span><span class="sxs-lookup"><span data-stu-id="63846-197">This will stop billing for the circuit</span></span>

## <a name="next-steps"></a><span data-ttu-id="63846-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63846-198">Next steps</span></span>
<span data-ttu-id="63846-199">Po vytvoření okruhu, ujistěte se, že provedete následující kroky:</span><span class="sxs-lookup"><span data-stu-id="63846-199">After you create your circuit, make sure that you do the following:</span></span>

* [<span data-ttu-id="63846-200">Vytvoření a úprava směrování pro okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63846-200">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-portal-resource-manager.md)
* [<span data-ttu-id="63846-201">Propojení virtuální sítě k okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63846-201">Link your virtual network to your ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

