---
title: "Jak tooconfigure směrování pro okruh ExpressRoute (partnerský vztah): Resource Manager: Azure | Microsoft Docs"
description: "Tento článek vás provede kroky hello pro vytváření a zřizování hello soukromého a veřejného a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak stav hello toocheck, aktualizace nebo odstranění partnerských vztahů pro váš okruh."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: a8ca25f488dde728cb3b06cd2c91873f3069feaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a><span data-ttu-id="6cd4c-104">Vytvářet a upravovat partnerský vztah pro okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6cd4c-104">Create and modify peering for an ExpressRoute circuit</span></span>

<span data-ttu-id="6cd4c-105">Tento článek pomůže při vytváření a správě konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager hello pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in hello Resource Manager deployment model using hello Azure portal.</span></span> <span data-ttu-id="6cd4c-106">Můžete také zkontrolovat stav hello, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-106">You can also check hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="6cd4c-107">Pokud chcete toouse toowork jinou metodu s váš okruh, vyberte článek z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-107">If you want toouse a different method toowork with your circuit, select an article from hello following list:</span></span>


## <a name="configuration-prerequisites"></a><span data-ttu-id="6cd4c-108">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="6cd4c-108">Configuration prerequisites</span></span>

* <span data-ttu-id="6cd4c-109">Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md) stránce hello [požadavky na směrování](expressroute-routing.md) stránku a hello [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-109">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) page, hello [routing requirements](expressroute-routing.md) page, and hello [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="6cd4c-110">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-110">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="6cd4c-111">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-111">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="6cd4c-112">Hello okruh ExpressRoute musí být ve stavu zřízený a povolený pro vás toobe možné toorun hello rutiny v dalších částech hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-112">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello cmdlets in hello next sections.</span></span>
* <span data-ttu-id="6cd4c-113">Pokud máte v plánu toouse sdílené hodnota hash klíče s algoritmem MD5, být jisti toouse to na obou stranách hello tunelové propojení a limit hello počet znaků tooa maximálně 25.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-113">If you plan toouse a shared key/MD5 hash, be sure toouse this on both sides of hello tunnel and limit hello number of characters tooa maximum of 25.</span></span>

<span data-ttu-id="6cd4c-114">Tyto pokyny platí pouze toocircuits vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-114">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="6cd4c-115">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení konfiguruje a spravuje směrování.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-115">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider configures and manages routing for you.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6cd4c-116">Jsme aktuálně neprovádějí ohlášení partnerské vztahy nakonfigurované poskytovateli služeb prostřednictvím portálu pro správu služby hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-116">We currently do not advertise peerings configured by service providers through hello service management portal.</span></span> <span data-ttu-id="6cd4c-117">Pracujeme na tom, aby tato možnost brzy fungovala.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-117">We are working on enabling this capability soon.</span></span> <span data-ttu-id="6cd4c-118">Před konfigurací partnerských vztahů protokolu BGP, zkontrolujte u vašeho poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-118">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="6cd4c-119">Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-119">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="6cd4c-120">Partnerské vztahy můžete konfigurovat v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-120">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="6cd4c-121">Přesvědčte se, že dokončení hello konfiguraci každého partnerského vztahu jeden po druhém.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-121">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="6cd4c-122">Soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="6cd4c-122">Azure private peering</span></span>

<span data-ttu-id="6cd4c-123">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello privátní konfigurace partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-123">This section helps you create, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="6cd4c-124">toocreate soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6cd4c-124">toocreate Azure private peering</span></span>

1. <span data-ttu-id="6cd4c-125">Nakonfigurujte okruh ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-125">Configure hello ExpressRoute circuit.</span></span> <span data-ttu-id="6cd4c-126">Ujistěte se, že hello okruh plně zřízený poskytovatelem připojení hello než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-126">Ensure that hello circuit is fully provisioned by hello connectivity provider before continuing.</span></span>

  ![seznam](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="6cd4c-128">Nakonfigurujte soukromý partnerský vztah Azure pro okruh hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-128">Configure Azure private peering for hello circuit.</span></span> <span data-ttu-id="6cd4c-129">Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-129">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="6cd4c-130">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-130">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="6cd4c-131">Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-131">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="6cd4c-132">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-132">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="6cd4c-133">Hello podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-133">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="6cd4c-134">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-134">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="6cd4c-135">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-135">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="6cd4c-136">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-136">AS number for peering.</span></span> <span data-ttu-id="6cd4c-137">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-137">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="6cd4c-138">Pro tento partnerský vztah můžete použít soukromé číslo AS.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-138">You can use a private AS number for this peering.</span></span> <span data-ttu-id="6cd4c-139">Zkontrolujte, že nepoužíváte 65515.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-139">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="6cd4c-140">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-140">**Optional -** An MD5 hash if you choose toouse one.</span></span>
3. <span data-ttu-id="6cd4c-141">Vyberte řádek Azure privátní partnerský vztah hello, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-141">Select hello Azure Private peering row, as shown in hello following example:</span></span>

  ![Privátní](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. <span data-ttu-id="6cd4c-143">Nakonfigurujte soukromý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-143">Configure private peering.</span></span> <span data-ttu-id="6cd4c-144">Hello následující obrázek ukazuje příklad konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-144">hello following image shows a configuration example:</span></span>

  ![Nakonfigurujte soukromý partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. <span data-ttu-id="6cd4c-146">Uložte konfiguraci hello po zadání všech parametrů.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-146">Save hello configuration once you have specified all parameters.</span></span> <span data-ttu-id="6cd4c-147">Po hello konfigurace úspěšně přijatá, zobrazí se něco podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-147">After hello configuration has been accepted successfully, you see something similar toohello following example:</span></span>

  ![Uložit soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="6cd4c-149">tooview Azure privátní partnerský vztah podrobnosti</span><span class="sxs-lookup"><span data-stu-id="6cd4c-149">tooview Azure private peering details</span></span>

<span data-ttu-id="6cd4c-150">Můžete zobrazit vlastnosti hello soukromého partnerského vztahu Azure výběrem partnerského vztahu hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-150">You can view hello properties of Azure private peering by selecting hello peering.</span></span>

![Zobrazit soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="6cd4c-152">tooupdate konfigurace soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6cd4c-152">tooupdate Azure private peering configuration</span></span>

<span data-ttu-id="6cd4c-153">Můžete vybrat hello řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-153">You can select hello row for peering and modify hello peering properties.</span></span>

![Aktualizovat soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="6cd4c-155">toodelete soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6cd4c-155">toodelete Azure private peering</span></span>

<span data-ttu-id="6cd4c-156">Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění hello, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-156">You can remove your peering configuration by selecting hello delete icon, as shown in hello following image:</span></span>

![Odstranění soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a><span data-ttu-id="6cd4c-158">Veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="6cd4c-158">Azure public peering</span></span>

<span data-ttu-id="6cd4c-159">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit hello veřejné konfigurace partnerského vztahu Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-159">This section helps you create, get, update, and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="6cd4c-160">toocreate veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6cd4c-160">toocreate Azure public peering</span></span>

1. <span data-ttu-id="6cd4c-161">Nakonfigurujte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-161">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="6cd4c-162">Zkontrolujte, zda text hello okruh plně zřízený poskytovatelem připojení hello než budete dál pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-162">Ensure that hello circuit is fully provisioned by hello connectivity provider before continuing further.</span></span>

  ![seznam veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="6cd4c-164">Nakonfigurujte veřejný partnerský vztah Azure pro okruh hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-164">Configure Azure public peering for hello circuit.</span></span> <span data-ttu-id="6cd4c-165">Ujistěte se, že máte hello předtím, než budete pokračovat v dalších krocích hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-165">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="6cd4c-166">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-166">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="6cd4c-167">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-167">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="6cd4c-168">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-168">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="6cd4c-169">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-169">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="6cd4c-170">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-170">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="6cd4c-171">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-171">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="6cd4c-172">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-172">AS number for peering.</span></span> <span data-ttu-id="6cd4c-173">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-173">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="6cd4c-174">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-174">**Optional -** An MD5 hash if you choose toouse one.</span></span>
3. <span data-ttu-id="6cd4c-175">Vyberte hello řádek Azure veřejný partnerský vztah, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-175">Select hello Azure public peering row, as shown in hello following image:</span></span>

  ![Vyberte řádek pro veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. <span data-ttu-id="6cd4c-177">Nakonfigurujte veřejný partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-177">Configure public peering.</span></span> <span data-ttu-id="6cd4c-178">Hello následující obrázek ukazuje příklad konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-178">hello following image shows a configuration example:</span></span>

  ![Nakonfigurujte veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. <span data-ttu-id="6cd4c-180">Uložte konfiguraci hello po zadání všech parametrů.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-180">Save hello configuration once you have specified all parameters.</span></span> <span data-ttu-id="6cd4c-181">Po hello konfigurace úspěšně přijatá, zobrazí se něco podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-181">After hello configuration has been accepted successfully, you see something similar toohello following example:</span></span>

  ![Uložte konfiguraci veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="6cd4c-183">tooview Azure veřejného partnerského vztahu podrobnosti</span><span class="sxs-lookup"><span data-stu-id="6cd4c-183">tooview Azure public peering details</span></span>

<span data-ttu-id="6cd4c-184">Můžete zobrazit vlastnosti hello veřejného partnerského vztahu Azure výběrem partnerského vztahu hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-184">You can view hello properties of Azure public peering by selecting hello peering.</span></span>

![Zobrazit vlastnosti veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="6cd4c-186">tooupdate konfigurace veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6cd4c-186">tooupdate Azure public peering configuration</span></span>

<span data-ttu-id="6cd4c-187">Můžete vybrat hello řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-187">You can select hello row for peering and modify hello peering properties.</span></span>

![Vyberte řádek pro veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="6cd4c-189">toodelete veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6cd4c-189">toodelete Azure public peering</span></span>

<span data-ttu-id="6cd4c-190">Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění hello, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-190">You can remove your peering configuration by selecting hello delete icon, as shown in hello following example:</span></span>

![odstranění veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a><span data-ttu-id="6cd4c-192">Partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="6cd4c-192">Microsoft peering</span></span>

<span data-ttu-id="6cd4c-193">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu hello Microsoftu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-193">This section helps you create, get, update, and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6cd4c-194">Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím hello partnerský vztah Microsoftu, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-194">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through hello Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="6cd4c-195">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-195">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span> <span data-ttu-id="6cd4c-196">Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6cd4c-196">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="6cd4c-197">partnerský vztah Microsoftu toocreate</span><span class="sxs-lookup"><span data-stu-id="6cd4c-197">toocreate Microsoft peering</span></span>

1. <span data-ttu-id="6cd4c-198">Nakonfigurujte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-198">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="6cd4c-199">Zkontrolujte, zda text hello okruh plně zřízený poskytovatelem připojení hello než budete dál pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-199">Ensure that hello circuit is fully provisioned by hello connectivity provider before continuing further.</span></span>

  ![seznam partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="6cd4c-201">Nakonfigurujte partnerský vztah Microsoftu pro okruh hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-201">Configure Microsoft peering for hello circuit.</span></span> <span data-ttu-id="6cd4c-202">Ujistěte se, že máte hello následující informace, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-202">Make sure that you have hello following information before you proceed.</span></span>

  * <span data-ttu-id="6cd4c-203">/ 30 pro primární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-203">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="6cd4c-204">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-204">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="6cd4c-205">/ 30 pro sekundární propojení hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-205">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="6cd4c-206">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-206">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="6cd4c-207">Platné ID sítě VLAN tooestablish tohoto partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-207">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="6cd4c-208">Zajistěte, aby žádný jiný partnerský vztah v okruhu hello hello používá stejný identifikátor VLAN ID.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-208">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="6cd4c-209">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-209">AS number for peering.</span></span> <span data-ttu-id="6cd4c-210">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-210">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="6cd4c-211">Inzerované předpony: musíte poskytnout seznam všech předpon plánování tooadvertise přes relaci protokolu BGP hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-211">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="6cd4c-212">Přijímají se jenom předpony veřejných IP adres.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-212">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="6cd4c-213">Pokud máte v plánu toosend sadu předpon, můžete odeslat seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-213">If you plan toosend a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="6cd4c-214">Tyto předpony musí být registrovaný tooyou u rir / IRR.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-214">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
  * <span data-ttu-id="6cd4c-215">**Volitelné -** zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované toohello číslo AS partnerského vztahu, můžete zadat hello jako číslo toowhich jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-215">**Optional -** Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span>
  * <span data-ttu-id="6cd4c-216">Název registru směrování: Můžete zadat hello RIR / IRR, u které hello jako číslo a předpony jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-216">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="6cd4c-217">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete toouse jeden.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-217">**Optional -** An MD5 hash if you choose toouse one.</span></span>
3. <span data-ttu-id="6cd4c-218">Můžete vybrat hello partnerský vztah chcete tooconfigure, jak je znázorněno v hello následující příklad.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-218">You can select hello peering you wish tooconfigure, as shown in hello following example.</span></span> <span data-ttu-id="6cd4c-219">Vyberte řádek partnerského vztahu Microsoftu hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-219">Select hello Microsoft peering row.</span></span>

  ![Vyberte řádek partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. <span data-ttu-id="6cd4c-221">Nakonfigurujte partnerský vztah Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-221">Configure Microsoft peering.</span></span> <span data-ttu-id="6cd4c-222">Hello následující obrázek ukazuje příklad konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-222">hello following image shows a configuration example:</span></span>

  ![Nakonfigurujte partnerský vztah Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. <span data-ttu-id="6cd4c-224">Uložte konfiguraci hello po zadání všech parametrů.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-224">Save hello configuration once you have specified all parameters.</span></span>

  <span data-ttu-id="6cd4c-225">Pokud váš okruh dostane tooa 'potřeby ověřování, stavu (jak je znázorněno v bitové kopii hello), musíte otevřít tooshow lístek podpory, která je doklad vlastnictví tým podpory tooour předpony hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-225">If your circuit gets tooa 'Validation needed' state (as shown in hello image), you must open a support ticket tooshow proof of ownership of hello prefixes tooour support team.</span></span>

  ![Uložte konfiguraci partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  <span data-ttu-id="6cd4c-227">Lístek podpory můžete otevřít přímo z portálu hello, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-227">You can open a support ticket directly from hello portal, as shown in hello following example:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. <span data-ttu-id="6cd4c-228">Po hello konfigurace úspěšně přijatá, zobrazí se něco podobné toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-228">After hello configuration has been accepted successfully, you see something similar toohello following image:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="tooview-microsoft-peering-details"></a><span data-ttu-id="6cd4c-229">tooview podrobností partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="6cd4c-229">tooview Microsoft peering details</span></span>

<span data-ttu-id="6cd4c-230">Můžete zobrazit vlastnosti hello veřejného partnerského vztahu Azure výběrem partnerského vztahu hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-230">You can view hello properties of Azure public peering by selecting hello peering.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="6cd4c-231">konfiguraci partnerského vztahu Microsoftu tooupdate</span><span class="sxs-lookup"><span data-stu-id="6cd4c-231">tooupdate Microsoft peering configuration</span></span>

<span data-ttu-id="6cd4c-232">Můžete vybrat hello řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu hello.</span><span class="sxs-lookup"><span data-stu-id="6cd4c-232">You can select hello row for peering and modify hello peering properties.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="6cd4c-233">partnerský vztah Microsoftu toodelete</span><span class="sxs-lookup"><span data-stu-id="6cd4c-233">toodelete Microsoft peering</span></span>

<span data-ttu-id="6cd4c-234">Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění hello, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="6cd4c-234">You can remove your peering configuration by selecting hello delete icon, as shown in hello following image:</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a><span data-ttu-id="6cd4c-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6cd4c-235">Next steps</span></span>

<span data-ttu-id="6cd4c-236">Dalším krokem je [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="6cd4c-236">Next step, [Link a VNet tooan ExpressRoute circuit](expressroute-howto-linkvnet-portal-resource-manager.md)</span></span>
* <span data-ttu-id="6cd4c-237">Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="6cd4c-237">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="6cd4c-238">Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="6cd4c-238">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="6cd4c-239">Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6cd4c-239">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
