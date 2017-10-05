---
title: "Nastavit filtry směrování pro partnerský vztah Azure ExpressRoute Microsoft: portál | Microsoft Docs"
description: "Tento článek popisuje, jak nastavit filtry tras pro aplikaci Microsoft Peering pomocí portálu Azure"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: f17bf3e475a33cfc617e8a026e9606b3792101f3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="d233d-103">Konfigurace filtrů směrování pro partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="d233d-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="d233d-104">Filtry tras představují způsob, jak využívat podmnožinu podporované služby prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="d233d-104">Route filters are a way to consume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="d233d-105">Kroky v tomto článku vám pomůžou nakonfigurovat a spravovat filtry tras pro okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="d233d-105">The steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="d233d-106">Dynamics 365 služeb a služeb Office 365 například Exchange Online, SharePoint Online a Skype pro firmy, jsou přístupné prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="d233d-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through the Microsoft peering.</span></span> <span data-ttu-id="d233d-107">Pokud partnerský vztah Microsoftu je nakonfigurován v okruhu ExpressRoute, jsou všechny předpony související s těmito službami inzerované prostřednictvím relace protokolu BGP, které jsou vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="d233d-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related to these services are advertised through the BGP sessions that are established.</span></span> <span data-ttu-id="d233d-108">Hodnota komunity protokolu BGP je připojený k každých předpona k identifikaci služby, které nabízí prostřednictvím předponu.</span><span class="sxs-lookup"><span data-stu-id="d233d-108">A BGP community value is attached to every prefix to identify the service that is offered through the prefix.</span></span> <span data-ttu-id="d233d-109">Seznam hodnot komunity protokolu BGP a služeb, jsou mapovány najdete v tématu [komunit protokolu BGP](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="d233d-109">For a list of the BGP community values and the services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="d233d-110">Pokud budete potřebovat připojení ke všem službám, jsou velký počet předpon inzerovaných prostřednictvím protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="d233d-110">If you require connectivity to all services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="d233d-111">Tím se výrazně zvyšuje velikost směrovací tabulky udržuje pomocí směrovačů v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="d233d-111">This significantly increases the size of the route tables maintained by routers within your network.</span></span> <span data-ttu-id="d233d-112">Pokud budete chtít využívat jenom podmnožinu službám nabízeným přes partnerský vztah Microsoftu, můžete snížit velikost vašeho směrovací tabulky dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="d233d-112">If you plan to consume only a subset of services offered through Microsoft peering, you can reduce the size of your route tables in two ways.</span></span> <span data-ttu-id="d233d-113">Můžete:</span><span class="sxs-lookup"><span data-stu-id="d233d-113">You can:</span></span>

- <span data-ttu-id="d233d-114">Filtrovat nežádoucí předpony použitím filtry tras na komunit protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="d233d-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="d233d-115">Tento postup standardní sítě a běžně používá v rámci mnoho sítí.</span><span class="sxs-lookup"><span data-stu-id="d233d-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="d233d-116">Zadejte filtry tras a použít je pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="d233d-116">Define route filters and apply them to your ExpressRoute circuit.</span></span> <span data-ttu-id="d233d-117">Filtr trasy je nový prostředek, který umožňuje vybrat seznam služeb, které budete chtít využívat prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="d233d-117">A route filter is a new resource that lets you select the list of services you plan to consume through Microsoft peering.</span></span> <span data-ttu-id="d233d-118">ExpressRoute směrovače odeslat pouze seznam předpon, které patří na služby určené ve filtru trasy.</span><span class="sxs-lookup"><span data-stu-id="d233d-118">ExpressRoute routers only send the list of prefixes that belong to the services identified in the route filter.</span></span>

### <span data-ttu-id="d233d-119"><a name="about"></a>O filtrech směrování</span><span class="sxs-lookup"><span data-stu-id="d233d-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="d233d-120">Pokud partnerský vztah Microsoftu je nakonfigurován na váš okruh ExpressRoute, Microsoft hraniční směrovače vytvořte dvojici relací protokolu BGP s hraniční směrovače (váš nebo váš poskytovatel připojení).</span><span class="sxs-lookup"><span data-stu-id="d233d-120">When Microsoft peering is configured on your ExpressRoute circuit, the Microsoft edge routers establish a pair of BGP sessions with the edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="d233d-121">Žádné trasy inzerovány vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="d233d-121">No routes are advertised to your network.</span></span> <span data-ttu-id="d233d-122">Pokud chcete povolit inzerování trasy k síti, je nutné přidružit filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="d233d-122">To enable route advertisements to your network, you must associate a route filter.</span></span>

<span data-ttu-id="d233d-123">Filtr trasy umožňuje identifikoval služby, které chcete využívat prostřednictvím partnerského vztahu Microsoftu okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="d233d-123">A route filter lets you identify services you want to consume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="d233d-124">Je v podstatě prázdný seznam všech hodnot komunity protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="d233d-124">It is essentially a white list of all the BGP community values.</span></span> <span data-ttu-id="d233d-125">Po filtru prostředek trasy definované a připojené k okruhu ExpressRoute, jsou všechny předpony, které jsou mapovány na hodnoty komunity protokolu BGP inzerovaný do vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="d233d-125">Once a route filter resource is defined and attached to an ExpressRoute circuit, all prefixes that map to the BGP community values are advertised to your network.</span></span>

<span data-ttu-id="d233d-126">Abyste mohli připojit filtry tras se službami Office 365 na ně, musí mít oprávnění pro využívání služeb Office 365 prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="d233d-126">To be able to attach route filters with Office 365 services on them, you must have authorization to consume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="d233d-127">Pokud nemáte oprávnění k využívání služeb Office 365 prostřednictvím ExpressRoute, operace připojit filtry tras se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="d233d-127">If you are not authorized to consume Office 365 services through ExpressRoute, the operation to attach route filters fails.</span></span> <span data-ttu-id="d233d-128">Další informace o procesu autorizačního najdete v tématu [Azure ExpressRoute pro Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="d233d-128">For more information about the authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="d233d-129">Připojení ke službám Dynamics 365 nevyžaduje žádné předchozí autorizace.</span><span class="sxs-lookup"><span data-stu-id="d233d-129">Connectivity to Dynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d233d-130">Okruhy ExpressRoute, které byly nakonfigurovány před 1. srpna 2017 partnerského vztahu Microsoftu bude mít všechny služby předpony inzerované prostřednictvím Microsoft partnerský vztah, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="d233d-130">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="d233d-131">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované dokud trasy filtr je připojen k okruhu.</span><span class="sxs-lookup"><span data-stu-id="d233d-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span>
> 
> 

### <span data-ttu-id="d233d-132"><a name="workflow"></a>Pracovní postup</span><span class="sxs-lookup"><span data-stu-id="d233d-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="d233d-133">Abyste mohli úspěšně připojit ke službám prostřednictvím partnerského vztahu Microsoftu, musíte provést následující kroky konfigurace:</span><span class="sxs-lookup"><span data-stu-id="d233d-133">To be able to successfully connect to services through Microsoft peering, you must complete the following configuration steps:</span></span>

- <span data-ttu-id="d233d-134">Musí mít aktivní okruh ExpressRoute, který má Microsoft partnerský vztah zřízené.</span><span class="sxs-lookup"><span data-stu-id="d233d-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="d233d-135">K těmto úkolům můžete použít následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="d233d-135">You can use the following instructions to accomplish these tasks:</span></span>
  - <span data-ttu-id="d233d-136">[Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mějte ho povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="d233d-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="d233d-137">Okruh ExpressRoute musí být ve stavu zřízený a povolený.</span><span class="sxs-lookup"><span data-stu-id="d233d-137">The ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="d233d-138">[Vytvoření partnerského vztahu Microsoftu](expressroute-howto-routing-portal-resource-manager.md) Pokud budete spravovat přímo relaci protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="d233d-138">[Create Microsoft peering](expressroute-howto-routing-portal-resource-manager.md) if you manage the BGP session directly.</span></span> <span data-ttu-id="d233d-139">Nebo váš poskytovatel připojení zřídit partnerský vztah Microsoftu pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="d233d-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="d233d-140">Musíte vytvořit a konfigurovat filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="d233d-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="d233d-141">Identifikovat služby vám využívat prostřednictvím partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="d233d-141">Identify the services you with to consume through Microsoft peering</span></span>
    - <span data-ttu-id="d233d-142">Určení seznamu hodnot komunity protokolu BGP přidruženého ke službám</span><span class="sxs-lookup"><span data-stu-id="d233d-142">Identify the list of BGP community values associated with the services</span></span>
    - <span data-ttu-id="d233d-143">Vytvořit pravidlo, které umožňují seznamu předponu odpovídající hodnoty komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="d233d-143">Create a rule to allow the prefix list matching the BGP community values</span></span>

-  <span data-ttu-id="d233d-144">Filtr trasy musí připojit k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="d233d-144">You must attach the route filter to the ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d233d-145">Než začnete</span><span class="sxs-lookup"><span data-stu-id="d233d-145">Before you begin</span></span>

<span data-ttu-id="d233d-146">Před zahájením konfigurace, ujistěte se, že splňují následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="d233d-146">Before you begin configuration, make sure you meet the following criteria:</span></span>

 - <span data-ttu-id="d233d-147">Zkontrolujte [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d233d-147">Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="d233d-148">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="d233d-148">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="d233d-149">Než budete pokračovat, podle pokynů [vytvořte okruh ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mějte ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="d233d-149">Follow the instructions to [Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="d233d-150">Okruh ExpressRoute musí být ve stavu zřízený a povolený.</span><span class="sxs-lookup"><span data-stu-id="d233d-150">The ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="d233d-151">Musíte mít active partnerský vztah Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="d233d-151">You must have an active Microsoft peering.</span></span> <span data-ttu-id="d233d-152">Postupujte podle pokynů v [vytvoření a úprava konfigurace partnerského vztahu](expressroute-howto-routing-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="d233d-152">Follow instructions at [Create and modifying peering configuration](expressroute-howto-routing-portal-resource-manager.md)</span></span>


## <span data-ttu-id="d233d-153"><a name="prefixes"></a>Krok 1.</span><span class="sxs-lookup"><span data-stu-id="d233d-153"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="d233d-154">Získat seznam předpon a hodnotami komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="d233d-154">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="d233d-155">1. Získat seznam hodnot komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="d233d-155">1. Get a list of BGP community values</span></span>

<span data-ttu-id="d233d-156">Je k dispozici v souvisejících se službou přístupné prostřednictvím partnerského vztahu Microsoftu hodnotami komunity protokolu BGP [požadavky na směrování služby ExpressRoute](expressroute-routing.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="d233d-156">BGP community values associated with services accessible through Microsoft peering is available in the [ExpressRoute routing requirements](expressroute-routing.md) page.</span></span>

### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a><span data-ttu-id="d233d-157">2. Zkontrolujte seznam hodnot, které chcete použít</span><span class="sxs-lookup"><span data-stu-id="d233d-157">2. Make a list of the values that you want to use</span></span>

<span data-ttu-id="d233d-158">Zkontrolujte seznam hodnotami komunity protokolu BGP, které chcete použít ve filtru trasy.</span><span class="sxs-lookup"><span data-stu-id="d233d-158">Make a list of BGP community values you want to use in the route filter.</span></span> <span data-ttu-id="d233d-159">Jako příklad je hodnota komunity protokolu BGP pro služby Dynamics 365 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="d233d-159">As an example, the BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="d233d-160"><a name="filter"></a>Krok 2.</span><span class="sxs-lookup"><span data-stu-id="d233d-160"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="d233d-161">Vytvořte filtr trasy a pravidlo filtru</span><span class="sxs-lookup"><span data-stu-id="d233d-161">Create a route filter and a filter rule</span></span>

<span data-ttu-id="d233d-162">Filtr trasy může mít pouze jedno pravidlo a pravidlo musí být typu 'Povolit'.</span><span class="sxs-lookup"><span data-stu-id="d233d-162">A route filter can have only one rule, and the rule must be of type 'Allow'.</span></span> <span data-ttu-id="d233d-163">Toto pravidlo může mít seznam hodnot komunity protokolu BGP s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="d233d-163">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="d233d-164">1. Vytvoření filtru trasy</span><span class="sxs-lookup"><span data-stu-id="d233d-164">1. Create a route filter</span></span>
<span data-ttu-id="d233d-165">Pokud vyberete možnost vytvořit nový prostředek, můžete vytvořit filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="d233d-165">You can create a route filter by selecting the option to create a new resource.</span></span> <span data-ttu-id="d233d-166">Klikněte na tlačítko **nový** > **sítě** > **RouteFilter**, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="d233d-166">Click **New** > **Networking** > **RouteFilter**, as shown in the following image:</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\CreateRouteFilter1.png)

<span data-ttu-id="d233d-168">Je nutné umístit filtru tras ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="d233d-168">You must place the route filter in a resource group.</span></span> 

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="d233d-170">2. Vytvořit pravidlo filtru</span><span class="sxs-lookup"><span data-stu-id="d233d-170">2. Create a filter rule</span></span>

<span data-ttu-id="d233d-171">Můžete přidat a aktualizovat pravidla vyberte na kartě Správa pravidlo filtru trasy.</span><span class="sxs-lookup"><span data-stu-id="d233d-171">You can add and update rules by selecting the manage rule tab for your route filter.</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\ManageRouteFilter.png)


<span data-ttu-id="d233d-173">Můžete vybrat služby, které chcete připojit se z rozevíracího seznamu a uložíte pravidlo po dokončení.</span><span class="sxs-lookup"><span data-stu-id="d233d-173">You can select the services you want to connect to from the drop down list and save the rule when done.</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\AddRouteFilterRule.png)


## <span data-ttu-id="d233d-175"><a name="attach"></a>Krok 3.</span><span class="sxs-lookup"><span data-stu-id="d233d-175"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="d233d-176">Připojte filtr trasy k okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="d233d-176">Attach the route filter to an ExpressRoute circuit</span></span>

<span data-ttu-id="d233d-177">Filtr trasy můžete připojit k okruhu vyberte tlačítko "Přidat okruh" a tlačítko okruh ExpressRoute z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="d233d-177">You can attach the route filter to a circuit by selecting the "add Circuit" button and selecting the ExpressRoute circuit from the drop down list.</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\AddCktToRouteFilter.png)

## <span data-ttu-id="d233d-179"><a name="getproperties"></a>Chcete-li získat vlastnosti filtru trasy</span><span class="sxs-lookup"><span data-stu-id="d233d-179"><a name="getproperties"></a>To get the properties of a route filter</span></span>

<span data-ttu-id="d233d-180">Při otevření prostředku na portálu, můžete zobrazit vlastnosti filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="d233d-180">You can view properties of a route filter when you open the resource in the portal.</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\ViewRouteFilter.png)


## <span data-ttu-id="d233d-182"><a name="updateproperties"></a>Aktualizovat vlastnosti filtr trasy</span><span class="sxs-lookup"><span data-stu-id="d233d-182"><a name="updateproperties"></a>To update the properties of a route filter</span></span>

<span data-ttu-id="d233d-183">Můžete aktualizovat seznam hodnot komunity protokolu BGP připojit k okruhu výběrem tlačítko "Správa pravidlo".</span><span class="sxs-lookup"><span data-stu-id="d233d-183">You can update the list of BGP community values attached to a circuit by selecting the "Manage rule" button.</span></span>


![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\ManageRouteFilter.png)

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\AddRouteFilterRule.png) 


## <span data-ttu-id="d233d-186"><a name="detach"></a>Odpojit filtr trasy z okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="d233d-186"><a name="detach"></a>To detach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="d233d-187">Odpojit okruh z filtru tras, klikněte pravým tlačítkem na okruh a klikněte na "zrušit přidružení".</span><span class="sxs-lookup"><span data-stu-id="d233d-187">To detach a circuit from the route filter, right click on the circuit and click on "disassociate".</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\DetachRouteFilter.png) 


## <span data-ttu-id="d233d-189"><a name="delete"></a>Odstranění filtru trasy</span><span class="sxs-lookup"><span data-stu-id="d233d-189"><a name="delete"></a>To delete a route filter</span></span>

<span data-ttu-id="d233d-190">Výběrem tlačítko Odstranit odstraníte filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="d233d-190">You can delete a route filter by selecting the delete button.</span></span> 

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\DeleteRouteFilter.png) 

## <a name="next-steps"></a><span data-ttu-id="d233d-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d233d-192">Next steps</span></span>

<span data-ttu-id="d233d-193">Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="d233d-193">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>