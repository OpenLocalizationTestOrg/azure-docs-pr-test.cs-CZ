---
title: "Vytvoření a úprava okruhu ExpressRoute: portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate, zřizovat, ověřte, aktualizovat, odstranit a zrušit jejich zřízení okruhu ExpressRoute."
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
ms.openlocfilehash: 200418aed6bdebace43a2f57cf532d1c8d13cb83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a><span data-ttu-id="6759e-103">Vytvoření a úprava okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6759e-103">Create and modify an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6759e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6759e-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="6759e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6759e-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="6759e-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6759e-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="6759e-107">Video – portál Azure</span><span class="sxs-lookup"><span data-stu-id="6759e-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="6759e-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="6759e-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="6759e-109">Tento článek popisuje, jak hello toocreate okruhu Azure ExpressRoute pomocí Azure portal a hello modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6759e-109">This article describes how toocreate an Azure ExpressRoute circuit by using hello Azure portal and hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="6759e-110">Hello také následující kroky ukazují, jak stav hello toocheck hello okruh, aktualizovat, nebo odstranit a zrušit jejich zřízení se.</span><span class="sxs-lookup"><span data-stu-id="6759e-110">hello following steps also show you how toocheck hello status of hello circuit, update it, or delete and deprovision it.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="6759e-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6759e-111">Before you begin</span></span>
* <span data-ttu-id="6759e-112">Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6759e-112">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="6759e-113">Zajistěte, abyste měli přístup toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6759e-113">Ensure that you have access toohello [Azure portal](https://portal.azure.com).</span></span>
* <span data-ttu-id="6759e-114">Ujistěte se, zda máte oprávnění toocreate nové síťové prostředky.</span><span class="sxs-lookup"><span data-stu-id="6759e-114">Ensure that you have permissions toocreate new networking resources.</span></span> <span data-ttu-id="6759e-115">Pokud nemáte hello správná oprávnění, obraťte se na správce účtu.</span><span class="sxs-lookup"><span data-stu-id="6759e-115">Contact your account administrator if you do not have hello right permissions.</span></span>
* <span data-ttu-id="6759e-116">Můžete [zobrazit video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) před zahájením v pořadí toobetter pochopit hello kroky.</span><span class="sxs-lookup"><span data-stu-id="6759e-116">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) before beginning in order toobetter understand hello steps.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="6759e-117">Vytvořit a zřídit okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6759e-117">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-toohello-azure-portal"></a><span data-ttu-id="6759e-118">1. Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6759e-118">1. Sign in toohello Azure portal</span></span>
<span data-ttu-id="6759e-119">V prohlížeči přejděte toohello [portál Azure](http://portal.azure.com) a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="6759e-119">From a browser, navigate toohello [Azure portal](http://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="2-create-a-new-expressroute-circuit"></a><span data-ttu-id="6759e-120">2. Vytvořit nový okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6759e-120">2. Create a new ExpressRoute circuit</span></span>
> [!IMPORTANT]
> <span data-ttu-id="6759e-121">Váš okruh ExpressRoute bude účtován z hello okamžiku, kdy se objeví klíč služby.</span><span class="sxs-lookup"><span data-stu-id="6759e-121">Your ExpressRoute circuit will be billed from hello moment a service key is issued.</span></span> <span data-ttu-id="6759e-122">Ujistěte se, když hello poskytovatele připojení je připraven tooprovision hello okruh provedení této operace.</span><span class="sxs-lookup"><span data-stu-id="6759e-122">Ensure that you perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

1. <span data-ttu-id="6759e-123">Okruh ExpressRoute můžete vytvořit tak, že vyberete možnost toocreate hello nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="6759e-123">You can create an ExpressRoute circuit by selecting hello option toocreate a new resource.</span></span> <span data-ttu-id="6759e-124">Klikněte na tlačítko **nový** > **sítě** > **ExpressRoute**, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="6759e-124">Click **New** > **Networking** > **ExpressRoute**, as shown in hello following image:</span></span>
   
    ![Vytvoření okruhu ExpressRoute](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. <span data-ttu-id="6759e-126">Po kliknutí na tlačítko **ExpressRoute**, uvidíte hello **okruh ExpressRoute vytvořit** okno.</span><span class="sxs-lookup"><span data-stu-id="6759e-126">After you click **ExpressRoute**, you'll see hello **Create ExpressRoute circuit** blade.</span></span> <span data-ttu-id="6759e-127">Když jste vyplnění hello hodnoty v tomto okně, ujistěte se, že zadáváte správnou úroveň SKU hello a data měření.</span><span class="sxs-lookup"><span data-stu-id="6759e-127">When you're filling in hello values on this blade, make sure that you specify hello correct SKU tier and data metering.</span></span>
   
   * <span data-ttu-id="6759e-128">**Úroveň** Určuje, zda je povoleno ExpressRoute standard nebo doplněk ExpressRoute premium.</span><span class="sxs-lookup"><span data-stu-id="6759e-128">**Tier** determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="6759e-129">Můžete zadat **standardní** tooget hello standardní SKU nebo **Premium** pro doplněk premium hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-129">You can specify **Standard** tooget hello standard SKU or **Premium** for hello premium add-on.</span></span>
   * <span data-ttu-id="6759e-130">**Měření dat** Určuje typ fakturace hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-130">**Data metering** determines hello billing type.</span></span> <span data-ttu-id="6759e-131">Můžete zadat **Metered** pro plán měření podle objemu dat a **neomezený** pro plán neomezená data na úrovni.</span><span class="sxs-lookup"><span data-stu-id="6759e-131">You can specify **Metered** for a metered data plan and **Unlimited** for an unlimited data plan.</span></span> <span data-ttu-id="6759e-132">Můžete změnit hello fakturace typu z **Metered** příliš**neomezený**, ale nemůžete změnit typ hello z **neomezený** příliš**Metered**.</span><span class="sxs-lookup"><span data-stu-id="6759e-132">Note that you can change hello billing type from **Metered** too**Unlimited**, but you can't change hello type from **Unlimited** too**Metered**.</span></span>
     
     ![Konfigurace hello SKU vrstvy a měření dat.](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> <span data-ttu-id="6759e-134">Upozorňujeme, že hello umístění v partnerském vztahu označuje hello [fyzické umístění](expressroute-locations.md) kde jste partnerského vztahu se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6759e-134">Please be aware that hello Peering Location indicates hello [physical location](expressroute-locations.md) where you are peering with Microsoft.</span></span> <span data-ttu-id="6759e-135">Toto je **není** příliš propojená vlastnost "Umístění", který odkazuje toohello geography, kde se nachází hello poskytovatele síťových prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6759e-135">This is **not** linked too"Location" property, which refers toohello geography where hello Azure Network Resource Provider is located.</span></span> <span data-ttu-id="6759e-136">I když se nesouvisí, je vhodné toochoose geograficky zavřít toohello poskytovatele síťových prostředků umístění v partnerském vztahu okruhu hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-136">While they are not related, it is a good practice toochoose a Network Resource Provider geographically close toohello Peering Location of hello circuit.</span></span> 
> 
> 

### <a name="3-view-hello-circuits-and-properties"></a><span data-ttu-id="6759e-137">3. Zobrazení hello okruhy a vlastnosti</span><span class="sxs-lookup"><span data-stu-id="6759e-137">3. View hello circuits and properties</span></span>
<span data-ttu-id="6759e-138">**Zobrazit všechny okruhy hello**</span><span class="sxs-lookup"><span data-stu-id="6759e-138">**View all hello circuits**</span></span>

<span data-ttu-id="6759e-139">Můžete zobrazit všechny hello okruhů, které jste vytvořili výběrem **všechny prostředky** v levé nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-139">You can view all hello circuits that you created by selecting **All resources** on hello left-side menu.</span></span>

![Okruhy zobrazení](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

<span data-ttu-id="6759e-141">**Zobrazit vlastnosti hello**</span><span class="sxs-lookup"><span data-stu-id="6759e-141">**View hello properties**</span></span>

    You can view hello properties of hello circuit by selecting it. On this blade, note hello service key for hello circuit. You must copy hello circuit key for your circuit and pass it down toohello service provider toocomplete hello provisioning process. hello circuit key is specific tooyour circuit.

![Zobrazení vlastností](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="6759e-143">4. Odeslání poskytovatele připojení klíče tooyour hello služby pro zřizování</span><span class="sxs-lookup"><span data-stu-id="6759e-143">4. Send hello service key tooyour connectivity provider for provisioning</span></span>
<span data-ttu-id="6759e-144">V tomto okně **stav zprostředkovatele** poskytuje informace o hello aktuální stav zřizování na straně hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="6759e-144">On this blade, **Provider status** provides information on hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="6759e-145">**Stav okruhu** poskytuje hello stavu na straně Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-145">**Circuit status** provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="6759e-146">Další informace o zřizování stavy okruhu najdete v tématu hello [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.</span><span class="sxs-lookup"><span data-stu-id="6759e-146">For more information about circuit provisioning states, see hello [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="6759e-147">Když vytvoříte nový okruh ExpressRoute, bude mít okruh hello hello následující stav:</span><span class="sxs-lookup"><span data-stu-id="6759e-147">When you create a new ExpressRoute circuit, hello circuit will be in hello following state:</span></span>

<span data-ttu-id="6759e-148">Stav zprostředkovatele: není zajišťováno</span><span class="sxs-lookup"><span data-stu-id="6759e-148">Provider status: Not provisioned</span></span><BR>
<span data-ttu-id="6759e-149">Stav okruhu: povoleno</span><span class="sxs-lookup"><span data-stu-id="6759e-149">Circuit status: Enabled</span></span>

![Zahájení procesu zřizování](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

<span data-ttu-id="6759e-151">okruh Hello změní toohello následující stavu, pokud je zprostředkovatel připojení k hello v procesu hello povolení pro vás:</span><span class="sxs-lookup"><span data-stu-id="6759e-151">hello circuit will change toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

<span data-ttu-id="6759e-152">Stav zprostředkovatele: zřizování</span><span class="sxs-lookup"><span data-stu-id="6759e-152">Provider status: Provisioning</span></span><BR>
<span data-ttu-id="6759e-153">Stav okruhu: povoleno</span><span class="sxs-lookup"><span data-stu-id="6759e-153">Circuit status: Enabled</span></span>

<span data-ttu-id="6759e-154">Pro jste toobe možné toouse okruh ExpressRoute musí být v hello následující stav:</span><span class="sxs-lookup"><span data-stu-id="6759e-154">For you toobe able toouse an ExpressRoute circuit, it must be in hello following state:</span></span>

<span data-ttu-id="6759e-155">Stav zprostředkovatele: zřídit</span><span class="sxs-lookup"><span data-stu-id="6759e-155">Provider status: Provisioned</span></span><BR>
<span data-ttu-id="6759e-156">Stav okruhu: povoleno</span><span class="sxs-lookup"><span data-stu-id="6759e-156">Circuit status: Enabled</span></span>

### <a name="5-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="6759e-157">5. Pravidelně kontrolovat hello stav a stav hello hello okruh klíče</span><span class="sxs-lookup"><span data-stu-id="6759e-157">5. Periodically check hello status and hello state of hello circuit key</span></span>
<span data-ttu-id="6759e-158">Můžete zobrazit vlastnosti hello hello okruhu, který máte zájem výběrem.</span><span class="sxs-lookup"><span data-stu-id="6759e-158">You can view hello properties of hello circuit that you're interested in by selecting it.</span></span> <span data-ttu-id="6759e-159">Zkontrolujte hello **stav zprostředkovatele** a ujistěte se, že se přesunul příliš**zajištěno** než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6759e-159">Check hello **Provider status** and ensure that it has moved too**Provisioned** before you continue.</span></span>

![Stav okruhu a zprostředkovatele](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a><span data-ttu-id="6759e-161">6. Vytvořte vlastní konfiguraci směrování</span><span class="sxs-lookup"><span data-stu-id="6759e-161">6. Create your routing configuration</span></span>
<span data-ttu-id="6759e-162">Podrobné pokyny najdete v části toohello [konfigurace směrování pro okruh ExpressRoute](expressroute-howto-routing-portal-resource-manager.md) článek toocreate a upravit partnerských vztahů pro okruh.</span><span class="sxs-lookup"><span data-stu-id="6759e-162">For step-by-step instructions, refer toohello [ExpressRoute circuit routing configuration](expressroute-howto-routing-portal-resource-manager.md) article toocreate and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6759e-163">Tyto pokyny platí pouze toocircuits, které jsou vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby.</span><span class="sxs-lookup"><span data-stu-id="6759e-163">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="6759e-164">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.</span><span class="sxs-lookup"><span data-stu-id="6759e-164">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="7-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="6759e-165">7. Propojení virtuální sítě tooan okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6759e-165">7. Link a virtual network tooan ExpressRoute circuit</span></span>
<span data-ttu-id="6759e-166">V dalším kroku propojte virtuální sítě tooyour okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6759e-166">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="6759e-167">Použití hello [propojení virtuální sítě okruhů tooExpressRoute](expressroute-howto-linkvnet-arm.md) článek při práci s modelem nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-167">Use hello [Linking virtual networks tooExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with hello Resource Manager deployment model.</span></span>

## <a name="getting-hello-status-of-an-expressroute-circuit"></a><span data-ttu-id="6759e-168">Získávání hello stav okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6759e-168">Getting hello status of an ExpressRoute circuit</span></span>
<span data-ttu-id="6759e-169">Hello stav okruhu můžete zobrazit výběrem.</span><span class="sxs-lookup"><span data-stu-id="6759e-169">You can view hello status of a circuit by selecting it.</span></span> 

![Stav okruhu ExpressRoute](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="6759e-171">Úprava okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6759e-171">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="6759e-172">Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6759e-172">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="6759e-173">Můžete provést následující bez výpadků hello:</span><span class="sxs-lookup"><span data-stu-id="6759e-173">You can do hello following with no downtime:</span></span>

* <span data-ttu-id="6759e-174">Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6759e-174">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="6759e-175">Zvýšení hello šířka pásma okruhu ExpressRoute, pokud na portu hello je dostupné kapacity.</span><span class="sxs-lookup"><span data-stu-id="6759e-175">Increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="6759e-176">Všimněte si, že hello šířka pásma okruhu přechod na starší verzi není podporován.</span><span class="sxs-lookup"><span data-stu-id="6759e-176">Note that downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="6759e-177">Změnit plán z tooUnlimited dat – měření podle objemu dat měření hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-177">Change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="6759e-178">Poznámka: Změna hello měření plán z tooMetered neomezená Data na úrovni, dat není podporováno.</span><span class="sxs-lookup"><span data-stu-id="6759e-178">Note that changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="6759e-179">Můžete povolit nebo zakázat *povolit klasické operace*.</span><span class="sxs-lookup"><span data-stu-id="6759e-179">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="6759e-180">Další informace o omezení a omezení, najdete v části toohello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="6759e-180">For more information on limits and limitations, refer toohello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

<span data-ttu-id="6759e-181">toomodify okruh ExpressRoute, klikněte na hello **konfigurace** jak je znázorněno na následujícím obrázku hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-181">toomodify an ExpressRoute circuit, click on hello **Configuration** as shown in hello figure below.</span></span>

![Úprava okruhu](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

<span data-ttu-id="6759e-183">Můžete upravit hello šířky pásma, SKU, model fakturace a povolit klasické operace v rámci okna konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-183">You can modify hello bandwidth, SKU, billing model and allow classic operations within hello configuration blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6759e-184">Pokud je na existující port hello nedostatečné kapacity, mohou mít okruh ExpressRoute toorecreate hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-184">You may have toorecreate hello ExpressRoute circuit if there is inadequate capacity on hello existing port.</span></span> <span data-ttu-id="6759e-185">Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat hello okruh.</span><span class="sxs-lookup"><span data-stu-id="6759e-185">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="6759e-186">Nelze zmenšit hello šířku pásma okruhu ExpressRoute bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="6759e-186">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="6759e-187">Přechod na starší verzi šířky pásma vyžaduje, abyste okruh ExpressRoute hello toodeprovision a pak znova nezajistíte nové okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6759e-187">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> <span data-ttu-id="6759e-188">Zakázat doplněk premium operace může selhat, pokud používáte prostředky, které jsou větší než co je povolené standardní okruh hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-188">Disable premium add-on operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="6759e-189">Zrušení zřízení a odstraňování okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6759e-189">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="6759e-190">Váš okruh ExpressRoute můžete odstranit výběrem hello **odstranit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6759e-190">You can delete your ExpressRoute circuit by selecting hello **delete** icon.</span></span> <span data-ttu-id="6759e-191">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="6759e-191">Note hello following:</span></span>

* <span data-ttu-id="6759e-192">Je nutné zrušit všechny virtuální sítě od hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6759e-192">You must unlink all virtual networks from hello ExpressRoute circuit.</span></span> <span data-ttu-id="6759e-193">Pokud se tato operace nezdaří, zkontrolujte, zda jsou všechny virtuální sítě propojené toohello okruh.</span><span class="sxs-lookup"><span data-stu-id="6759e-193">If this operation fails, check whether any virtual networks are linked toohello circuit.</span></span>
* <span data-ttu-id="6759e-194">Pokud je hello ExpressRoute okruhu poskytovatele služeb Stav zřizování **zřizování** nebo **zajištěno** na jejich straně, musíte pracovat se váš okruh hello toodeprovision zprostředkovatele služby.</span><span class="sxs-lookup"><span data-stu-id="6759e-194">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="6759e-195">Jsme bude i nadále tooreserve prostředky a dokud poskytovatele služeb hello dokončení zrušení zřízení hello okruhu a upozorní nám vám účtovat.</span><span class="sxs-lookup"><span data-stu-id="6759e-195">We will continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="6759e-196">Pokud má poskytovatel služeb hello zrušit okruh hello (Stav zřizování poskytovatele služeb hello je nastaven příliš**není zajišťováno**) pak můžete odstranit okruh hello.</span><span class="sxs-lookup"><span data-stu-id="6759e-196">If hello service provider has deprovisioned hello circuit (hello service provider provisioning state is set too**Not provisioned**) you can then delete hello circuit.</span></span> <span data-ttu-id="6759e-197">Tato akce ukončí fakturace pro okruh hello</span><span class="sxs-lookup"><span data-stu-id="6759e-197">This will stop billing for hello circuit</span></span>

## <a name="next-steps"></a><span data-ttu-id="6759e-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6759e-198">Next steps</span></span>
<span data-ttu-id="6759e-199">Po vytvoření okruhu, ujistěte se, že hello následující:</span><span class="sxs-lookup"><span data-stu-id="6759e-199">After you create your circuit, make sure that you do hello following:</span></span>

* [<span data-ttu-id="6759e-200">Vytvoření a úprava směrování pro okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6759e-200">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-portal-resource-manager.md)
* [<span data-ttu-id="6759e-201">Propojení vaší virtuální sítě tooyour okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6759e-201">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

