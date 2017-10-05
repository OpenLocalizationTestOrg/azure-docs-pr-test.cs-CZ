---
title: "Postup konfigurace směrování (partnerský vztah) pro ExpressRoute okruh: Resource Manager: Azure | Microsoft Docs"
description: "Tento článek vás provede kroky pro vytváření a zřizování soukromého a veřejného partnerského vztahu a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak kontrolovat stav partnerských vztahů pro váš okruh, aktualizovat je nebo je odstranit."
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
ms.openlocfilehash: 55ccadfea55b8098ee58dcaef942f6ba54093665
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a><span data-ttu-id="6d8a2-104">Vytvářet a upravovat partnerský vztah pro okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="6d8a2-104">Create and modify peering for an ExpressRoute circuit</span></span>

<span data-ttu-id="6d8a2-105">Tento článek pomůže při vytváření a správě konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in the Resource Manager deployment model using the Azure portal.</span></span> <span data-ttu-id="6d8a2-106">Můžete také zkontrolovat stav, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-106">You can also check the status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="6d8a2-107">Pokud chcete použít jinou metodu pro práci se váš okruh, vyberte článek z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-107">If you want to use a different method to work with your circuit, select an article from the following list:</span></span>


## <a name="configuration-prerequisites"></a><span data-ttu-id="6d8a2-108">Předpoklady konfigurace</span><span class="sxs-lookup"><span data-stu-id="6d8a2-108">Configuration prerequisites</span></span>

* <span data-ttu-id="6d8a2-109">Před zahájením konfigurace se ujistěte, že jste si přečetli stránku s [předpoklady](expressroute-prerequisites.md), stránku s [požadavky směrování](expressroute-routing.md) a stránku s [pracovními postupy](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="6d8a2-109">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) page, the [routing requirements](expressroute-routing.md) page, and the [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="6d8a2-110">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-110">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="6d8a2-111">Než budete pokračovat, podle pokynů [vytvořte okruh ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mějte ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-111">Follow the instructions to [Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="6d8a2-112">Okruh ExpressRoute musí být v zřízený a povolený stavu, abyste mohli spustit rutiny v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-112">The ExpressRoute circuit must be in a provisioned and enabled state for you to be able to run the cmdlets in the next sections.</span></span>
* <span data-ttu-id="6d8a2-113">Pokud budete chtít použít sdílené hodnotu hash klíče s algoritmem MD5, je nutné použít na obou stranách tunelového propojení a omezit počet znaků, který má být delší než 25.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-113">If you plan to use a shared key/MD5 hash, be sure to use this on both sides of the tunnel and limit the number of characters to a maximum of 25.</span></span>

<span data-ttu-id="6d8a2-114">Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-114">These instructions only apply to circuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="6d8a2-115">Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení konfiguruje a spravuje směrování.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-115">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider configures and manages routing for you.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6d8a2-116">Aktuálně prostřednictvím portálu pro správu služeb neinzerujeme partnerské vztahy nakonfigurované poskytovateli služeb.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-116">We currently do not advertise peerings configured by service providers through the service management portal.</span></span> <span data-ttu-id="6d8a2-117">Pracujeme na tom, aby tato možnost brzy fungovala.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-117">We are working on enabling this capability soon.</span></span> <span data-ttu-id="6d8a2-118">Před konfigurací partnerských vztahů protokolu BGP, zkontrolujte u vašeho poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-118">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="6d8a2-119">Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-119">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="6d8a2-120">Partnerské vztahy můžete konfigurovat v libovolném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-120">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="6d8a2-121">Musíte se ale přesvědčit, že jste vždy konfiguraci každého partnerského vztahu dokončili.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-121">However, you must make sure that you complete the configuration of each peering one at a time.</span></span>

## <a name="azure-private-peering"></a><span data-ttu-id="6d8a2-122">Soukromý partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-122">Azure private peering</span></span>

<span data-ttu-id="6d8a2-123">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit Azure konfigurace soukromého partnerského vztahu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-123">This section helps you create, get, update, and delete the Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-private-peering"></a><span data-ttu-id="6d8a2-124">Vytvoření soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-124">To create Azure private peering</span></span>

1. <span data-ttu-id="6d8a2-125">Nakonfigurujte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-125">Configure the ExpressRoute circuit.</span></span> <span data-ttu-id="6d8a2-126">Než budete pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-126">Ensure that the circuit is fully provisioned by the connectivity provider before continuing.</span></span>

  ![seznam](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="6d8a2-128">Nakonfigurujte soukromý partnerský vztah Azure pro okruh.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-128">Configure Azure private peering for the circuit.</span></span> <span data-ttu-id="6d8a2-129">Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-129">Make sure that you have the following items before you proceed with the next steps:</span></span>

  * <span data-ttu-id="6d8a2-130">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-130">A /30 subnet for the primary link.</span></span> <span data-ttu-id="6d8a2-131">Podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-131">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="6d8a2-132">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-132">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="6d8a2-133">Podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-133">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="6d8a2-134">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-134">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="6d8a2-135">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-135">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="6d8a2-136">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-136">AS number for peering.</span></span> <span data-ttu-id="6d8a2-137">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-137">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="6d8a2-138">Pro tento partnerský vztah můžete použít soukromé číslo AS.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-138">You can use a private AS number for this peering.</span></span> <span data-ttu-id="6d8a2-139">Zkontrolujte, že nepoužíváte 65515.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-139">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="6d8a2-140">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-140">**Optional -** An MD5 hash if you choose to use one.</span></span>
3. <span data-ttu-id="6d8a2-141">Vyberte řádek Azure privátní partnerský vztah, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-141">Select the Azure Private peering row, as shown in the following example:</span></span>

  ![Privátní](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. <span data-ttu-id="6d8a2-143">Nakonfigurujte soukromý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-143">Configure private peering.</span></span> <span data-ttu-id="6d8a2-144">Následující obrázek ukazuje příklad konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-144">The following image shows a configuration example:</span></span>

  ![Nakonfigurujte soukromý partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. <span data-ttu-id="6d8a2-146">Po zadání všech parametrů uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-146">Save the configuration once you have specified all parameters.</span></span> <span data-ttu-id="6d8a2-147">Po konfiguraci úspěšně přijatá, zobrazí se o něco podobného jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-147">After the configuration has been accepted successfully, you see something similar to the following example:</span></span>

  ![Uložit soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-view-azure-private-peering-details"></a><span data-ttu-id="6d8a2-149">Zobrazení podrobností soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-149">To view Azure private peering details</span></span>

<span data-ttu-id="6d8a2-150">Vlastnosti soukromého partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-150">You can view the properties of Azure private peering by selecting the peering.</span></span>

![Zobrazit soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-update-azure-private-peering-configuration"></a><span data-ttu-id="6d8a2-152">Aktualizace konfigurace soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-152">To update Azure private peering configuration</span></span>

<span data-ttu-id="6d8a2-153">Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-153">You can select the row for peering and modify the peering properties.</span></span>

![Aktualizovat soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="to-delete-azure-private-peering"></a><span data-ttu-id="6d8a2-155">Odstranění soukromého partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-155">To delete Azure private peering</span></span>

<span data-ttu-id="6d8a2-156">Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-156">You can remove your peering configuration by selecting the delete icon, as shown in the following image:</span></span>

![Odstranění soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a><span data-ttu-id="6d8a2-158">Veřejný partnerský vztah Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-158">Azure public peering</span></span>

<span data-ttu-id="6d8a2-159">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit veřejného partnerského vztahu konfiguraci Azure pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-159">This section helps you create, get, update, and delete the Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-public-peering"></a><span data-ttu-id="6d8a2-160">Vytvoření veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-160">To create Azure public peering</span></span>

1. <span data-ttu-id="6d8a2-161">Nakonfigurujte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-161">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="6d8a2-162">Než budete dál pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-162">Ensure that the circuit is fully provisioned by the connectivity provider before continuing further.</span></span>

  ![seznam veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="6d8a2-164">Nakonfigurujte veřejný partnerský vztah Azure pro okruh.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-164">Configure Azure public peering for the circuit.</span></span> <span data-ttu-id="6d8a2-165">Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-165">Make sure that you have the following items before you proceed with the next steps:</span></span>

  * <span data-ttu-id="6d8a2-166">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-166">A /30 subnet for the primary link.</span></span> <span data-ttu-id="6d8a2-167">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-167">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="6d8a2-168">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-168">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="6d8a2-169">Musí se jednat o platnou předponu veřejné IPv4 adresy.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-169">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="6d8a2-170">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-170">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="6d8a2-171">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-171">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="6d8a2-172">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-172">AS number for peering.</span></span> <span data-ttu-id="6d8a2-173">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-173">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="6d8a2-174">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-174">**Optional -** An MD5 hash if you choose to use one.</span></span>
3. <span data-ttu-id="6d8a2-175">Azure veřejného partnerského vztahu řádek, vyberte, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-175">Select the Azure public peering row, as shown in the following image:</span></span>

  ![Vyberte řádek pro veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. <span data-ttu-id="6d8a2-177">Nakonfigurujte veřejný partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-177">Configure public peering.</span></span> <span data-ttu-id="6d8a2-178">Následující obrázek ukazuje příklad konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-178">The following image shows a configuration example:</span></span>

  ![Nakonfigurujte veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. <span data-ttu-id="6d8a2-180">Po zadání všech parametrů uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-180">Save the configuration once you have specified all parameters.</span></span> <span data-ttu-id="6d8a2-181">Po konfiguraci úspěšně přijatá, zobrazí se o něco podobného jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-181">After the configuration has been accepted successfully, you see something similar to the following example:</span></span>

  ![Uložte konfiguraci veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-view-azure-public-peering-details"></a><span data-ttu-id="6d8a2-183">Zobrazení podrobností veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-183">To view Azure public peering details</span></span>

<span data-ttu-id="6d8a2-184">Vlastnosti veřejného partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-184">You can view the properties of Azure public peering by selecting the peering.</span></span>

![Zobrazit vlastnosti veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-update-azure-public-peering-configuration"></a><span data-ttu-id="6d8a2-186">Aktualizace konfigurace veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-186">To update Azure public peering configuration</span></span>

<span data-ttu-id="6d8a2-187">Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-187">You can select the row for peering and modify the peering properties.</span></span>

![Vyberte řádek pro veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="to-delete-azure-public-peering"></a><span data-ttu-id="6d8a2-189">Odstranění veřejného partnerského vztahu Azure</span><span class="sxs-lookup"><span data-stu-id="6d8a2-189">To delete Azure public peering</span></span>

<span data-ttu-id="6d8a2-190">Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-190">You can remove your peering configuration by selecting the delete icon, as shown in the following example:</span></span>

![odstranění veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a><span data-ttu-id="6d8a2-192">Partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="6d8a2-192">Microsoft peering</span></span>

<span data-ttu-id="6d8a2-193">Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu Microsoftu pro okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-193">This section helps you create, get, update, and delete the Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6d8a2-194">Okruhy ExpressRoute, které byly nakonfigurovány před 1. srpna 2017 partnerského vztahu Microsoftu bude mít všechny služby předpony inzerované prostřednictvím Microsoft partnerský vztah, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-194">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through the Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="6d8a2-195">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované dokud trasy filtr je připojen k okruhu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-195">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span> <span data-ttu-id="6d8a2-196">Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6d8a2-196">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="to-create-microsoft-peering"></a><span data-ttu-id="6d8a2-197">Vytvoření partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="6d8a2-197">To create Microsoft peering</span></span>

1. <span data-ttu-id="6d8a2-198">Nakonfigurujte okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-198">Configure ExpressRoute circuit.</span></span> <span data-ttu-id="6d8a2-199">Než budete dál pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-199">Ensure that the circuit is fully provisioned by the connectivity provider before continuing further.</span></span>

  ![seznam partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. <span data-ttu-id="6d8a2-201">Nakonfigurujte partnerský vztah Microsoftu pro okruh.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-201">Configure Microsoft peering for the circuit.</span></span> <span data-ttu-id="6d8a2-202">Před pokračováním se ujistěte, že máte k dispozici následující informace.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-202">Make sure that you have the following information before you proceed.</span></span>

  * <span data-ttu-id="6d8a2-203">Podsíť /30 pro primární propojení.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-203">A /30 subnet for the primary link.</span></span> <span data-ttu-id="6d8a2-204">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-204">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="6d8a2-205">Podsíť /30 pro sekundární propojení.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-205">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="6d8a2-206">Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-206">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="6d8a2-207">Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-207">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="6d8a2-208">Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-208">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="6d8a2-209">Číslo AS pro partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-209">AS number for peering.</span></span> <span data-ttu-id="6d8a2-210">Můžete použít 2bajtová i 4bajtová čísla AS.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-210">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="6d8a2-211">Inzerované předpony: Musíte poskytnout seznam všech předpon, které plánujete inzerovat přes relaci protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-211">Advertised prefixes: You must provide a list of all prefixes you plan to advertise over the BGP session.</span></span> <span data-ttu-id="6d8a2-212">Přijímají se jenom předpony veřejných IP adres.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-212">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="6d8a2-213">Pokud chcete odeslat sadu předpon, můžete odeslat seznam oddělený čárkami.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-213">If you plan to send a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="6d8a2-214">Tyto předpony musí být v RIR/IRR zaregistrované na vás.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-214">These prefixes must be registered to you in an RIR / IRR.</span></span>
  * <span data-ttu-id="6d8a2-215">**Volitelné -** zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované na číslo AS partnerského vztahu, můžete zadat číslo AS, do kterého jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-215">**Optional -** Customer ASN: If you are advertising prefixes that are not registered to the peering AS number, you can specify the AS number to which they are registered.</span></span>
  * <span data-ttu-id="6d8a2-216">Název registru směrování: Můžete zadat RIR/IRR, kde jsou předpony a číslo AS registrované.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-216">Routing Registry Name: You can specify the RIR / IRR against which the AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="6d8a2-217">**Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-217">**Optional -** An MD5 hash if you choose to use one.</span></span>
3. <span data-ttu-id="6d8a2-218">Můžete vybrat partnerský vztah, který chcete nakonfigurovat, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-218">You can select the peering you wish to configure, as shown in the following example.</span></span> <span data-ttu-id="6d8a2-219">Vyberte řádek partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-219">Select the Microsoft peering row.</span></span>

  ![Vyberte řádek partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. <span data-ttu-id="6d8a2-221">Nakonfigurujte partnerský vztah Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-221">Configure Microsoft peering.</span></span> <span data-ttu-id="6d8a2-222">Následující obrázek ukazuje příklad konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-222">The following image shows a configuration example:</span></span>

  ![Nakonfigurujte partnerský vztah Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. <span data-ttu-id="6d8a2-224">Po zadání všech parametrů uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-224">Save the configuration once you have specified all parameters.</span></span>

  <span data-ttu-id="6d8a2-225">Pokud váš okruh dostane do 'Ověřování potřeby' stavu (jak je znázorněno na obrázku), musíte otevřít lístek podpory, abyste ukázali důkaz vlastnictví předpon našemu týmu podpory.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-225">If your circuit gets to a 'Validation needed' state (as shown in the image), you must open a support ticket to show proof of ownership of the prefixes to our support team.</span></span>

  ![Uložte konfiguraci partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  <span data-ttu-id="6d8a2-227">Lístek podpory můžete otevřít přímo z portálu, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-227">You can open a support ticket directly from the portal, as shown in the following example:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. <span data-ttu-id="6d8a2-228">Po konfiguraci úspěšně přijatá, zobrazí se o něco podobného jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-228">After the configuration has been accepted successfully, you see something similar to the following image:</span></span>

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-view-microsoft-peering-details"></a><span data-ttu-id="6d8a2-229">Zobrazení podrobností partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="6d8a2-229">To view Microsoft peering details</span></span>

<span data-ttu-id="6d8a2-230">Vlastnosti veřejného partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-230">You can view the properties of Azure public peering by selecting the peering.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="to-update-microsoft-peering-configuration"></a><span data-ttu-id="6d8a2-231">Aktualizace konfigurace partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="6d8a2-231">To update Microsoft peering configuration</span></span>

<span data-ttu-id="6d8a2-232">Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="6d8a2-232">You can select the row for peering and modify the peering properties.</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-delete-microsoft-peering"></a><span data-ttu-id="6d8a2-233">Odstranění partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="6d8a2-233">To delete Microsoft peering</span></span>

<span data-ttu-id="6d8a2-234">Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="6d8a2-234">You can remove your peering configuration by selecting the delete icon, as shown in the following image:</span></span>

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a><span data-ttu-id="6d8a2-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d8a2-235">Next steps</span></span>

<span data-ttu-id="6d8a2-236">Dalším krokem je [propojení virtuální sítě k okruhu ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="6d8a2-236">Next step, [Link a VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-portal-resource-manager.md)</span></span>
* <span data-ttu-id="6d8a2-237">Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="6d8a2-237">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="6d8a2-238">Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="6d8a2-238">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="6d8a2-239">Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6d8a2-239">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
