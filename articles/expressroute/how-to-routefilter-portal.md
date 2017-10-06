---
title: "Nastavit filtry směrování pro partnerský vztah Azure ExpressRoute Microsoft: portál | Microsoft Docs"
description: "Tento článek popisuje, jak hello tooconfigure filtry tras pro Microsoft Peering pomocí portálu Azure"
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
ms.openlocfilehash: 2a47d465ec5f175d9510cef94606f70f036f0862
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="66578-103">Konfigurace filtrů směrování pro partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="66578-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="66578-104">Filtry tras jsou způsob tooconsume podmnožinu podporované služby prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="66578-104">Route filters are a way tooconsume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="66578-105">Hello kroky v této nápovědě článku můžete nakonfigurovat a spravovat filtry tras pro okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="66578-105">hello steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="66578-106">Dynamics 365 služeb a služeb Office 365 například Exchange Online, SharePoint Online a Skype pro firmy, jsou přístupné prostřednictvím partnerského vztahu Microsoftu hello.</span><span class="sxs-lookup"><span data-stu-id="66578-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through hello Microsoft peering.</span></span> <span data-ttu-id="66578-107">Pokud partnerský vztah Microsoftu je nakonfigurován v okruhu ExpressRoute, jsou všechny služby související toothese předpony inzerované prostřednictvím hello relací protokolu BGP, které jsou vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="66578-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related toothese services are advertised through hello BGP sessions that are established.</span></span> <span data-ttu-id="66578-108">Hodnota komunity protokolu BGP je připojený tooevery předponu tooidentify hello služba, která je nabízeným přes hello předponu.</span><span class="sxs-lookup"><span data-stu-id="66578-108">A BGP community value is attached tooevery prefix tooidentify hello service that is offered through hello prefix.</span></span> <span data-ttu-id="66578-109">Seznam hodnot komunity protokolu BGP hello a hello služeb jsou mapovány najdete v tématu [komunit protokolu BGP](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="66578-109">For a list of hello BGP community values and hello services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="66578-110">Pokud budete potřebovat připojení tooall služeb, jsou velký počet předpon inzerovaných prostřednictvím protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="66578-110">If you require connectivity tooall services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="66578-111">Tím se výrazně zvyšuje velikost hello hello směrovací tabulky udržuje pomocí směrovačů v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="66578-111">This significantly increases hello size of hello route tables maintained by routers within your network.</span></span> <span data-ttu-id="66578-112">Pokud máte v plánu tooconsume pouze podmnožinu službám nabízeným přes partnerské vztahy Microsoft, můžete snížit velikost hello vaší směrovací tabulky dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="66578-112">If you plan tooconsume only a subset of services offered through Microsoft peering, you can reduce hello size of your route tables in two ways.</span></span> <span data-ttu-id="66578-113">Můžete:</span><span class="sxs-lookup"><span data-stu-id="66578-113">You can:</span></span>

- <span data-ttu-id="66578-114">Filtrovat nežádoucí předpony použitím filtry tras na komunit protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="66578-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="66578-115">Tento postup standardní sítě a běžně používá v rámci mnoho sítí.</span><span class="sxs-lookup"><span data-stu-id="66578-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="66578-116">Definovat filtry tras a použít tooyour okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="66578-116">Define route filters and apply them tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="66578-117">Filtr trasy je nový prostředek, který vám umožní vybrat hello seznam služeb můžete naplánovat tooconsume prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="66578-117">A route filter is a new resource that lets you select hello list of services you plan tooconsume through Microsoft peering.</span></span> <span data-ttu-id="66578-118">ExpressRoute směrovače odeslat pouze hello seznam předpon, které patří služby toohello stanovených ve filtru hello trasy.</span><span class="sxs-lookup"><span data-stu-id="66578-118">ExpressRoute routers only send hello list of prefixes that belong toohello services identified in hello route filter.</span></span>

### <span data-ttu-id="66578-119"><a name="about"></a>O filtrech směrování</span><span class="sxs-lookup"><span data-stu-id="66578-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="66578-120">Pokud partnerský vztah Microsoftu je nakonfigurován na váš okruh ExpressRoute, hello Microsoft hraniční směrovače vytvořte dvojici relací protokolu BGP s hello hraniční směrovače (váš nebo váš poskytovatel připojení).</span><span class="sxs-lookup"><span data-stu-id="66578-120">When Microsoft peering is configured on your ExpressRoute circuit, hello Microsoft edge routers establish a pair of BGP sessions with hello edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="66578-121">Trasy se inzerovaný tooyour sítě.</span><span class="sxs-lookup"><span data-stu-id="66578-121">No routes are advertised tooyour network.</span></span> <span data-ttu-id="66578-122">tooenable trasy oznámení o inzerovaném programu tooyour sítě, je nutné přidružit filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="66578-122">tooenable route advertisements tooyour network, you must associate a route filter.</span></span>

<span data-ttu-id="66578-123">Filtr trasy umožňuje identifikovat služby, které mají tooconsume prostřednictvím partnerského vztahu Microsoftu okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="66578-123">A route filter lets you identify services you want tooconsume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="66578-124">Je to v podstatě prázdný seznam všech hodnot komunity protokolu BGP hello.</span><span class="sxs-lookup"><span data-stu-id="66578-124">It is essentially a white list of all hello BGP community values.</span></span> <span data-ttu-id="66578-125">Prostředek trasy filtru je definovaný a připojené tooan okruh ExpressRoute, budete všech předpon, které mapují hodnoty komunity protokolu BGP toohello inzerovaný tooyour sítě.</span><span class="sxs-lookup"><span data-stu-id="66578-125">Once a route filter resource is defined and attached tooan ExpressRoute circuit, all prefixes that map toohello BGP community values are advertised tooyour network.</span></span>

<span data-ttu-id="66578-126">filtry tras toobe možné tooattach se službami Office 365 na ně, musí mít služby Office 365 tooconsume autorizace prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="66578-126">toobe able tooattach route filters with Office 365 services on them, you must have authorization tooconsume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="66578-127">Pokud si nejste oprávnění tooconsume Office 365 služby prostřednictvím ExpressRoute, filtry tras tooattach hello operace selže.</span><span class="sxs-lookup"><span data-stu-id="66578-127">If you are not authorized tooconsume Office 365 services through ExpressRoute, hello operation tooattach route filters fails.</span></span> <span data-ttu-id="66578-128">Další informace o procesu autorizačního hello najdete v tématu [Azure ExpressRoute pro Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="66578-128">For more information about hello authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="66578-129">365 služeb připojení k tooDynamics nevyžaduje žádné předchozí autorizace.</span><span class="sxs-lookup"><span data-stu-id="66578-129">Connectivity tooDynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66578-130">Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím partnerského vztahu Microsoftu, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="66578-130">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="66578-131">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh.</span><span class="sxs-lookup"><span data-stu-id="66578-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span>
> 
> 

### <span data-ttu-id="66578-132"><a name="workflow"></a>Pracovní postup</span><span class="sxs-lookup"><span data-stu-id="66578-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="66578-133">možnost toosuccessfully toobe připojit tooservices prostřednictvím partnerského vztahu Microsoftu, je třeba provést následující kroky konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="66578-133">toobe able toosuccessfully connect tooservices through Microsoft peering, you must complete hello following configuration steps:</span></span>

- <span data-ttu-id="66578-134">Musí mít aktivní okruh ExpressRoute, který má Microsoft partnerský vztah zřízené.</span><span class="sxs-lookup"><span data-stu-id="66578-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="66578-135">Následující pokyny tooaccomplish hello můžete použít tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="66578-135">You can use hello following instructions tooaccomplish these tasks:</span></span>
  - <span data-ttu-id="66578-136">[Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="66578-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="66578-137">Hello okruh ExpressRoute musí být ve stavu zřízený a povolený.</span><span class="sxs-lookup"><span data-stu-id="66578-137">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="66578-138">[Vytvoření partnerského vztahu Microsoftu](expressroute-howto-routing-portal-resource-manager.md) Pokud spravujete hello přímo v relaci protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="66578-138">[Create Microsoft peering](expressroute-howto-routing-portal-resource-manager.md) if you manage hello BGP session directly.</span></span> <span data-ttu-id="66578-139">Nebo váš poskytovatel připojení zřídit partnerský vztah Microsoftu pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="66578-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="66578-140">Musíte vytvořit a konfigurovat filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="66578-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="66578-141">Identifikovat hello služby services s tooconsume prostřednictvím partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="66578-141">Identify hello services you with tooconsume through Microsoft peering</span></span>
    - <span data-ttu-id="66578-142">Identifikovat hello seznam hodnot komunity protokolu BGP souvisejících se službou hello</span><span class="sxs-lookup"><span data-stu-id="66578-142">Identify hello list of BGP community values associated with hello services</span></span>
    - <span data-ttu-id="66578-143">Vytvoření pravidlo tooallow hello předponu seznamu odpovídající hello hodnotami komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="66578-143">Create a rule tooallow hello prefix list matching hello BGP community values</span></span>

-  <span data-ttu-id="66578-144">Je nutné připojit okruh ExpressRoute toohello filtr trasy hello.</span><span class="sxs-lookup"><span data-stu-id="66578-144">You must attach hello route filter toohello ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="66578-145">Než začnete</span><span class="sxs-lookup"><span data-stu-id="66578-145">Before you begin</span></span>

<span data-ttu-id="66578-146">Před zahájením konfigurace, ujistěte se, že splňujete hello následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="66578-146">Before you begin configuration, make sure you meet hello following criteria:</span></span>

 - <span data-ttu-id="66578-147">Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="66578-147">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="66578-148">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="66578-148">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="66578-149">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="66578-149">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="66578-150">Hello okruh ExpressRoute musí být ve stavu zřízený a povolený.</span><span class="sxs-lookup"><span data-stu-id="66578-150">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="66578-151">Musíte mít active partnerský vztah Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="66578-151">You must have an active Microsoft peering.</span></span> <span data-ttu-id="66578-152">Postupujte podle pokynů v [vytvoření a úprava konfigurace partnerského vztahu](expressroute-howto-routing-portal-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="66578-152">Follow instructions at [Create and modifying peering configuration](expressroute-howto-routing-portal-resource-manager.md)</span></span>


## <span data-ttu-id="66578-153"><a name="prefixes"></a>Krok 1.</span><span class="sxs-lookup"><span data-stu-id="66578-153"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="66578-154">Získat seznam předpon a hodnotami komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="66578-154">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="66578-155">1. Získat seznam hodnot komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="66578-155">1. Get a list of BGP community values</span></span>

<span data-ttu-id="66578-156">Hodnoty komunity protokolu BGP souvisejících se službou přístupné prostřednictvím partnerského vztahu Microsoftu je k dispozici v hello [požadavky na směrování služby ExpressRoute](expressroute-routing.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="66578-156">BGP community values associated with services accessible through Microsoft peering is available in hello [ExpressRoute routing requirements](expressroute-routing.md) page.</span></span>

### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a><span data-ttu-id="66578-157">2. Zkontrolujte seznam hello hodnot, které chcete toouse</span><span class="sxs-lookup"><span data-stu-id="66578-157">2. Make a list of hello values that you want toouse</span></span>

<span data-ttu-id="66578-158">Zkontrolujte seznam hodnotami komunity protokolu BGP, které chcete toouse ve filtru hello trasy.</span><span class="sxs-lookup"><span data-stu-id="66578-158">Make a list of BGP community values you want toouse in hello route filter.</span></span> <span data-ttu-id="66578-159">Jako příklad je hello hodnota komunity protokolu BGP pro služby Dynamics 365 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="66578-159">As an example, hello BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="66578-160"><a name="filter"></a>Krok 2.</span><span class="sxs-lookup"><span data-stu-id="66578-160"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="66578-161">Vytvořte filtr trasy a pravidlo filtru</span><span class="sxs-lookup"><span data-stu-id="66578-161">Create a route filter and a filter rule</span></span>

<span data-ttu-id="66578-162">Filtr trasy může mít pouze jedno pravidlo a hello pravidlo musí být typu 'Povolit'.</span><span class="sxs-lookup"><span data-stu-id="66578-162">A route filter can have only one rule, and hello rule must be of type 'Allow'.</span></span> <span data-ttu-id="66578-163">Toto pravidlo může mít seznam hodnot komunity protokolu BGP s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="66578-163">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="66578-164">1. Vytvoření filtru trasy</span><span class="sxs-lookup"><span data-stu-id="66578-164">1. Create a route filter</span></span>
<span data-ttu-id="66578-165">Filtr trasy můžete vytvořit tak, že vyberete možnost toocreate hello nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="66578-165">You can create a route filter by selecting hello option toocreate a new resource.</span></span> <span data-ttu-id="66578-166">Klikněte na tlačítko **nový** > **sítě** > **RouteFilter**, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="66578-166">Click **New** > **Networking** > **RouteFilter**, as shown in hello following image:</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\CreateRouteFilter1.png)

<span data-ttu-id="66578-168">Je nutné umístit hello trasy filtru ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="66578-168">You must place hello route filter in a resource group.</span></span> 

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="66578-170">2. Vytvořit pravidlo filtru</span><span class="sxs-lookup"><span data-stu-id="66578-170">2. Create a filter rule</span></span>

<span data-ttu-id="66578-171">Můžete přidat a spravovat pravidla aktualizací výběrem hello karta pravidlo filtru trasy.</span><span class="sxs-lookup"><span data-stu-id="66578-171">You can add and update rules by selecting hello manage rule tab for your route filter.</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\ManageRouteFilter.png)


<span data-ttu-id="66578-173">Můžete vybrat hello služby, které mají tooconnect toofrom hello rozevíracím seznamu a uložíte pravidlo hello po dokončení.</span><span class="sxs-lookup"><span data-stu-id="66578-173">You can select hello services you want tooconnect toofrom hello drop down list and save hello rule when done.</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\AddRouteFilterRule.png)


## <span data-ttu-id="66578-175"><a name="attach"></a>Krok 3.</span><span class="sxs-lookup"><span data-stu-id="66578-175"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="66578-176">Připojte okruh ExpressRoute tooan filtr trasy hello</span><span class="sxs-lookup"><span data-stu-id="66578-176">Attach hello route filter tooan ExpressRoute circuit</span></span>

<span data-ttu-id="66578-177">Vyberte tlačítko "Přidat okruh" hello a tlačítko hello okruh ExpressRoute z hello rozevíracího seznamu můžete připojit hello trasy filtru tooa okruh.</span><span class="sxs-lookup"><span data-stu-id="66578-177">You can attach hello route filter tooa circuit by selecting hello "add Circuit" button and selecting hello ExpressRoute circuit from hello drop down list.</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\AddCktToRouteFilter.png)

## <span data-ttu-id="66578-179"><a name="getproperties"></a>Vlastnosti hello tooget filtru trasy</span><span class="sxs-lookup"><span data-stu-id="66578-179"><a name="getproperties"></a>tooget hello properties of a route filter</span></span>

<span data-ttu-id="66578-180">Při otevření prostředku hello hello portálu, můžete zobrazit vlastnosti filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="66578-180">You can view properties of a route filter when you open hello resource in hello portal.</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\ViewRouteFilter.png)


## <span data-ttu-id="66578-182"><a name="updateproperties"></a>Vlastnosti hello tooupdate filtru trasy</span><span class="sxs-lookup"><span data-stu-id="66578-182"><a name="updateproperties"></a>tooupdate hello properties of a route filter</span></span>

<span data-ttu-id="66578-183">Výběrem tlačítka hello "Správa pravidlo" můžete aktualizovat seznam hello okruhu připojené tooa hodnoty komunity protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="66578-183">You can update hello list of BGP community values attached tooa circuit by selecting hello "Manage rule" button.</span></span>


![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\ManageRouteFilter.png)

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\AddRouteFilterRule.png) 


## <span data-ttu-id="66578-186"><a name="detach"></a>toodetach filtr trasy z okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="66578-186"><a name="detach"></a>toodetach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="66578-187">toodetach okruhu z hello trasy filtru, klikněte pravým tlačítkem na okruh hello a klikněte na "zrušit přidružení".</span><span class="sxs-lookup"><span data-stu-id="66578-187">toodetach a circuit from hello route filter, right click on hello circuit and click on "disassociate".</span></span>

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\DetachRouteFilter.png) 


## <span data-ttu-id="66578-189"><a name="delete"></a>toodelete filtr trasy</span><span class="sxs-lookup"><span data-stu-id="66578-189"><a name="delete"></a>toodelete a route filter</span></span>

<span data-ttu-id="66578-190">Výběrem hello tlačítko Odstranit odstraníte filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="66578-190">You can delete a route filter by selecting hello delete button.</span></span> 

![Vytvoření filtru trasy](.\media\how-to-routefilter-portal\DeleteRouteFilter.png) 

## <a name="next-steps"></a><span data-ttu-id="66578-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66578-192">Next steps</span></span>

<span data-ttu-id="66578-193">Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="66578-193">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
