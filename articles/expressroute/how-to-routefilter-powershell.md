---
title: "Nastavit filtry směrování pro partnerský vztah Azure ExpressRoute Microsoftu: prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak filtry tooconfigure trasy pro Microsoft Peering pomocí prostředí PowerShell"
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
ms.date: 08/16/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 395bcf7d6ad43c643ff041b154f8e4b797083e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="c6532-103">Konfigurace filtrů směrování pro partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="c6532-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="c6532-104">Filtry tras jsou způsob tooconsume podmnožinu podporované služby prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="c6532-104">Route filters are a way tooconsume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="c6532-105">Hello kroky v této nápovědě článku můžete nakonfigurovat a spravovat filtry tras pro okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="c6532-105">hello steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="c6532-106">Dynamics 365 služeb a služeb Office 365 například Exchange Online, SharePoint Online a Skype pro firmy, jsou přístupné prostřednictvím partnerského vztahu Microsoftu hello.</span><span class="sxs-lookup"><span data-stu-id="c6532-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through hello Microsoft peering.</span></span> <span data-ttu-id="c6532-107">Pokud partnerský vztah Microsoftu je nakonfigurován v okruhu ExpressRoute, jsou všechny služby související toothese předpony inzerované prostřednictvím hello relací protokolu BGP, které jsou vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="c6532-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related toothese services are advertised through hello BGP sessions that are established.</span></span> <span data-ttu-id="c6532-108">Hodnota komunity protokolu BGP je připojený tooevery předponu tooidentify hello služba, která je nabízeným přes hello předponu.</span><span class="sxs-lookup"><span data-stu-id="c6532-108">A BGP community value is attached tooevery prefix tooidentify hello service that is offered through hello prefix.</span></span> <span data-ttu-id="c6532-109">Seznam hodnot komunity protokolu BGP hello a hello služeb jsou mapovány najdete v tématu [komunit protokolu BGP](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="c6532-109">For a list of hello BGP community values and hello services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="c6532-110">Pokud budete potřebovat připojení tooall služeb, jsou velký počet předpon inzerovaných prostřednictvím protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="c6532-110">If you require connectivity tooall services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="c6532-111">Tím se výrazně zvyšuje velikost hello hello směrovací tabulky udržuje pomocí směrovačů v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="c6532-111">This significantly increases hello size of hello route tables maintained by routers within your network.</span></span> <span data-ttu-id="c6532-112">Pokud máte v plánu tooconsume pouze podmnožinu službám nabízeným přes partnerské vztahy Microsoft, můžete snížit velikost hello vaší směrovací tabulky dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="c6532-112">If you plan tooconsume only a subset of services offered through Microsoft peering, you can reduce hello size of your route tables in two ways.</span></span> <span data-ttu-id="c6532-113">Můžete:</span><span class="sxs-lookup"><span data-stu-id="c6532-113">You can:</span></span>

- <span data-ttu-id="c6532-114">Filtrovat nežádoucí předpony použitím filtry tras na komunit protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="c6532-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="c6532-115">Tento postup standardní sítě a běžně používá v rámci mnoho sítí.</span><span class="sxs-lookup"><span data-stu-id="c6532-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="c6532-116">Definovat filtry tras a použít tooyour okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="c6532-116">Define route filters and apply them tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="c6532-117">Filtr trasy je nový prostředek, který vám umožní vybrat hello seznam služeb můžete naplánovat tooconsume prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="c6532-117">A route filter is a new resource that lets you select hello list of services you plan tooconsume through Microsoft peering.</span></span> <span data-ttu-id="c6532-118">ExpressRoute směrovače odeslat pouze hello seznam předpon, které patří služby toohello stanovených ve filtru hello trasy.</span><span class="sxs-lookup"><span data-stu-id="c6532-118">ExpressRoute routers only send hello list of prefixes that belong toohello services identified in hello route filter.</span></span>

### <span data-ttu-id="c6532-119"><a name="about"></a>O filtrech směrování</span><span class="sxs-lookup"><span data-stu-id="c6532-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="c6532-120">Pokud partnerský vztah Microsoftu je nakonfigurován na váš okruh ExpressRoute, hello Microsoft hraniční směrovače vytvořte dvojici relací protokolu BGP s hello hraniční směrovače (váš nebo váš poskytovatel připojení).</span><span class="sxs-lookup"><span data-stu-id="c6532-120">When Microsoft peering is configured on your ExpressRoute circuit, hello Microsoft edge routers establish a pair of BGP sessions with hello edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="c6532-121">Trasy se inzerovaný tooyour sítě.</span><span class="sxs-lookup"><span data-stu-id="c6532-121">No routes are advertised tooyour network.</span></span> <span data-ttu-id="c6532-122">tooenable trasy oznámení o inzerovaném programu tooyour sítě, je nutné přidružit filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="c6532-122">tooenable route advertisements tooyour network, you must associate a route filter.</span></span>

<span data-ttu-id="c6532-123">Filtr trasy umožňuje identifikovat služby, které mají tooconsume prostřednictvím partnerského vztahu Microsoftu okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="c6532-123">A route filter lets you identify services you want tooconsume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="c6532-124">Je to v podstatě prázdný seznam všech hodnot komunity protokolu BGP hello.</span><span class="sxs-lookup"><span data-stu-id="c6532-124">It is essentially a white list of all hello BGP community values.</span></span> <span data-ttu-id="c6532-125">Prostředek trasy filtru je definovaný a připojené tooan okruh ExpressRoute, budete všech předpon, které mapují hodnoty komunity protokolu BGP toohello inzerovaný tooyour sítě.</span><span class="sxs-lookup"><span data-stu-id="c6532-125">Once a route filter resource is defined and attached tooan ExpressRoute circuit, all prefixes that map toohello BGP community values are advertised tooyour network.</span></span>

<span data-ttu-id="c6532-126">filtry tras toobe možné tooattach se službami Office 365 na ně, musí mít služby Office 365 tooconsume autorizace prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="c6532-126">toobe able tooattach route filters with Office 365 services on them, you must have authorization tooconsume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="c6532-127">Pokud si nejste oprávnění tooconsume Office 365 služby prostřednictvím ExpressRoute, filtry tras tooattach hello operace selže.</span><span class="sxs-lookup"><span data-stu-id="c6532-127">If you are not authorized tooconsume Office 365 services through ExpressRoute, hello operation tooattach route filters fails.</span></span> <span data-ttu-id="c6532-128">Další informace o procesu autorizačního hello najdete v tématu [Azure ExpressRoute pro Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="c6532-128">For more information about hello authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="c6532-129">365 služeb připojení k tooDynamics nevyžaduje žádné předchozí autorizace.</span><span class="sxs-lookup"><span data-stu-id="c6532-129">Connectivity tooDynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c6532-130">Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím partnerského vztahu Microsoftu, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="c6532-130">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="c6532-131">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh.</span><span class="sxs-lookup"><span data-stu-id="c6532-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span>
> 
> 

### <span data-ttu-id="c6532-132"><a name="workflow"></a>Pracovní postup</span><span class="sxs-lookup"><span data-stu-id="c6532-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="c6532-133">možnost toosuccessfully toobe připojit tooservices prostřednictvím partnerského vztahu Microsoftu, je třeba provést následující kroky konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="c6532-133">toobe able toosuccessfully connect tooservices through Microsoft peering, you must complete hello following configuration steps:</span></span>

- <span data-ttu-id="c6532-134">Musí mít aktivní okruh ExpressRoute, který má Microsoft partnerský vztah zřízené.</span><span class="sxs-lookup"><span data-stu-id="c6532-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="c6532-135">Následující pokyny tooaccomplish hello můžete použít tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="c6532-135">You can use hello following instructions tooaccomplish these tasks:</span></span>
  - <span data-ttu-id="c6532-136">[Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c6532-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="c6532-137">Hello okruh ExpressRoute musí být ve stavu zřízený a povolený.</span><span class="sxs-lookup"><span data-stu-id="c6532-137">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="c6532-138">[Vytvoření partnerského vztahu Microsoftu](expressroute-circuit-peerings.md) Pokud spravujete hello přímo v relaci protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="c6532-138">[Create Microsoft peering](expressroute-circuit-peerings.md) if you manage hello BGP session directly.</span></span> <span data-ttu-id="c6532-139">Nebo váš poskytovatel připojení zřídit partnerský vztah Microsoftu pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="c6532-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="c6532-140">Musíte vytvořit a konfigurovat filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="c6532-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="c6532-141">Identifikovat hello služby services s tooconsume prostřednictvím partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="c6532-141">Identify hello services you with tooconsume through Microsoft peering</span></span>
    - <span data-ttu-id="c6532-142">Identifikovat hello seznam hodnot komunity protokolu BGP souvisejících se službou hello</span><span class="sxs-lookup"><span data-stu-id="c6532-142">Identify hello list of BGP community values associated with hello services</span></span>
    - <span data-ttu-id="c6532-143">Vytvoření pravidlo tooallow hello předponu seznamu odpovídající hello hodnotami komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="c6532-143">Create a rule tooallow hello prefix list matching hello BGP community values</span></span>

-  <span data-ttu-id="c6532-144">Je nutné připojit okruh ExpressRoute toohello filtr trasy hello.</span><span class="sxs-lookup"><span data-stu-id="c6532-144">You must attach hello route filter toohello ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c6532-145">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c6532-145">Before you begin</span></span>

<span data-ttu-id="c6532-146">Před zahájením konfigurace, ujistěte se, že splňujete hello následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="c6532-146">Before you begin configuration, make sure you meet hello following criteria:</span></span>

 - <span data-ttu-id="c6532-147">Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello.</span><span class="sxs-lookup"><span data-stu-id="c6532-147">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="c6532-148">Další informace najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="c6532-148">For more information, see [Install and configure Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span></span>

  > [!NOTE]
  > <span data-ttu-id="c6532-149">Stažení nejnovější verze hello z Galerie prostředí PowerShell text hello, nikoli pomocí hello Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="c6532-149">Download hello latest version from hello PowerShell Gallery, rather than using hello Installer.</span></span> <span data-ttu-id="c6532-150">Hello instalační program aktuálně nepodporuje hello požadované rutiny.</span><span class="sxs-lookup"><span data-stu-id="c6532-150">hello Installer currently does not support hello required cmdlets.</span></span>
  > 

 - <span data-ttu-id="c6532-151">Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c6532-151">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="c6532-152">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="c6532-152">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="c6532-153">Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c6532-153">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="c6532-154">Hello okruh ExpressRoute musí být ve stavu zřízený a povolený.</span><span class="sxs-lookup"><span data-stu-id="c6532-154">hello ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="c6532-155">Musíte mít active partnerský vztah Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="c6532-155">You must have an active Microsoft peering.</span></span> <span data-ttu-id="c6532-156">Postupujte podle pokynů v [vytvoření a úprava konfigurace partnerského vztahu](expressroute-circuit-peerings.md)</span><span class="sxs-lookup"><span data-stu-id="c6532-156">Follow instructions at [Create and modifying peering configuration](expressroute-circuit-peerings.md)</span></span>

### <a name="log-in-tooyour-azure-account"></a><span data-ttu-id="c6532-157">Přihlaste se tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="c6532-157">Log in tooyour Azure account</span></span>

<span data-ttu-id="c6532-158">Před zahájením této konfigurace, musíte být přihlášení tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c6532-158">Before beginning this configuration, you must log in tooyour Azure account.</span></span> <span data-ttu-id="c6532-159">Hello rutina vás vyzve k zadání hello přihlašovací údaje k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6532-159">hello cmdlet prompts you for hello login credentials for your Azure account.</span></span> <span data-ttu-id="c6532-160">Po přihlášení stahování nastavení svého účtu tak, aby byly k dispozici tooAzure prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c6532-160">After logging in, it downloads your account settings so they are available tooAzure PowerShell.</span></span>

<span data-ttu-id="c6532-161">Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="c6532-161">Open your PowerShell console with elevated privileges, and connect tooyour account.</span></span> <span data-ttu-id="c6532-162">Použijte následující příklad toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="c6532-162">Use hello following example toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="c6532-163">Pokud máte víc předplatných Azure, zkontrolujte hello předplatná pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="c6532-163">If you have multiple Azure subscriptions, check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="c6532-164">Zadejte hello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="c6532-164">Specify hello subscription that you want toouse.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <span data-ttu-id="c6532-165"><a name="prefixes"></a>Krok 1.</span><span class="sxs-lookup"><span data-stu-id="c6532-165"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="c6532-166">Získat seznam předpon a hodnotami komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="c6532-166">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="c6532-167">1. Získat seznam hodnot komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="c6532-167">1. Get a list of BGP community values</span></span>

<span data-ttu-id="c6532-168">Použijte následující rutinu tooget hello seznam hodnot komunity protokolu BGP souvisejících se službou přístupné prostřednictvím partnerského vztahu Microsoftu hello a hello seznam předpon, které jsou s nimi spojených:</span><span class="sxs-lookup"><span data-stu-id="c6532-168">Use hello following cmdlet tooget hello list of BGP community values associated with services accessible through Microsoft peering, and hello list of prefixes associated with them:</span></span>

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a><span data-ttu-id="c6532-169">2. Zkontrolujte seznam hello hodnot, které chcete toouse</span><span class="sxs-lookup"><span data-stu-id="c6532-169">2. Make a list of hello values that you want toouse</span></span>

<span data-ttu-id="c6532-170">Zkontrolujte seznam hodnotami komunity protokolu BGP, které chcete toouse ve filtru hello trasy.</span><span class="sxs-lookup"><span data-stu-id="c6532-170">Make a list of BGP community values you want toouse in hello route filter.</span></span> <span data-ttu-id="c6532-171">Jako příklad je hello hodnota komunity protokolu BGP pro služby Dynamics 365 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="c6532-171">As an example, hello BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="c6532-172"><a name="filter"></a>Krok 2.</span><span class="sxs-lookup"><span data-stu-id="c6532-172"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="c6532-173">Vytvořte filtr trasy a pravidlo filtru</span><span class="sxs-lookup"><span data-stu-id="c6532-173">Create a route filter and a filter rule</span></span>

<span data-ttu-id="c6532-174">Filtr trasy může mít pouze jedno pravidlo a hello pravidlo musí být typu 'Povolit'.</span><span class="sxs-lookup"><span data-stu-id="c6532-174">A route filter can have only one rule, and hello rule must be of type 'Allow'.</span></span> <span data-ttu-id="c6532-175">Toto pravidlo může mít seznam hodnot komunity protokolu BGP s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="c6532-175">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="c6532-176">1. Vytvoření filtru trasy</span><span class="sxs-lookup"><span data-stu-id="c6532-176">1. Create a route filter</span></span>

<span data-ttu-id="c6532-177">Nejprve vytvořte filtr hello trasy.</span><span class="sxs-lookup"><span data-stu-id="c6532-177">First, create hello route filter.</span></span> <span data-ttu-id="c6532-178">příkaz Hello 'New-AzureRmRouteFilter, pouze vytvoří prostředek trasy filtru.</span><span class="sxs-lookup"><span data-stu-id="c6532-178">hello command 'New-AzureRmRouteFilter' only creates a route filter resource.</span></span> <span data-ttu-id="c6532-179">Po vytvoření hello prostředků, musíte pak vytvořit pravidlo a připojte ji toohello trasy filtru objektu.</span><span class="sxs-lookup"><span data-stu-id="c6532-179">After you create hello resource, you must then create a rule and attach it toohello route filter object.</span></span> <span data-ttu-id="c6532-180">Spusťte následující příkaz toocreate hello prostředek trasy filtru:</span><span class="sxs-lookup"><span data-stu-id="c6532-180">Run hello following command toocreate a route filter resource:</span></span>

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="c6532-181">2. Vytvořit pravidlo filtru</span><span class="sxs-lookup"><span data-stu-id="c6532-181">2. Create a filter rule</span></span>

<span data-ttu-id="c6532-182">Sadu komunit protokolu BGP jako seznam oddělený čárkami, můžete zadat, jak je znázorněno v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="c6532-182">You can specify a set of BGP communities as a comma-separated list, as shown in hello example.</span></span> <span data-ttu-id="c6532-183">Spusťte následující příkaz toocreate hello nové pravidlo:</span><span class="sxs-lookup"><span data-stu-id="c6532-183">Run hello following command toocreate a new rule:</span></span>
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-hello-rule-toohello-route-filter"></a><span data-ttu-id="c6532-184">3. Přidat filtr trasy toohello pravidlo hello</span><span class="sxs-lookup"><span data-stu-id="c6532-184">3. Add hello rule toohello route filter</span></span>

<span data-ttu-id="c6532-185">Spusťte následující příkaz tooadd hello filtru pravidlo toohello trasy filtru hello:</span><span class="sxs-lookup"><span data-stu-id="c6532-185">Run hello following command tooadd hello filter rule toohello route filter:</span></span>
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="c6532-186"><a name="attach"></a>Krok 3.</span><span class="sxs-lookup"><span data-stu-id="c6532-186"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="c6532-187">Připojte okruh ExpressRoute tooan filtr trasy hello</span><span class="sxs-lookup"><span data-stu-id="c6532-187">Attach hello route filter tooan ExpressRoute circuit</span></span>

<span data-ttu-id="c6532-188">Spusťte následující příkaz tooattach hello trasy filtru toohello okruh ExpressRoute, za předpokladu, že máte jenom partnerského vztahu Microsoft hello:</span><span class="sxs-lookup"><span data-stu-id="c6532-188">Run hello following command tooattach hello route filter toohello ExpressRoute circuit, assuming you have only Microsoft peering:</span></span>

```powershell
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="c6532-189"><a name="getproperties"></a>Vlastnosti hello tooget filtru trasy</span><span class="sxs-lookup"><span data-stu-id="c6532-189"><a name="getproperties"></a>tooget hello properties of a route filter</span></span>

<span data-ttu-id="c6532-190">Vlastnosti hello tooget filtru trasy, hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c6532-190">tooget hello properties of a route filter, use hello following steps:</span></span>

1. <span data-ttu-id="c6532-191">Spusťte následující příkaz tooget hello trasy filtru prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="c6532-191">Run hello following command tooget hello route filter resource:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. <span data-ttu-id="c6532-192">Získáte pravidla filtru hello trasy pro prostředek trasy filtru hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c6532-192">Get hello route filter rules for hello route-filter resource by running hello following command:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

## <span data-ttu-id="c6532-193"><a name="updateproperties"></a>Vlastnosti hello tooupdate filtru trasy</span><span class="sxs-lookup"><span data-stu-id="c6532-193"><a name="updateproperties"></a>tooupdate hello properties of a route filter</span></span>

<span data-ttu-id="c6532-194">Pokud hello trasy filtru je již připojen tooa okruhu seznamu komunity protokolu BGP toohello aktualizace automaticky šíří oznámení o inzerovaném programu změny příslušné předpony prostřednictvím relace protokolu BGP hello navázat.</span><span class="sxs-lookup"><span data-stu-id="c6532-194">If hello route filter is already attached tooa circuit, updates toohello BGP community list automatically propagates appropriate prefix advertisement changes through hello established BGP sessions.</span></span> <span data-ttu-id="c6532-195">Můžete aktualizovat seznam komunity protokolu BGP hello filtru trasu pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c6532-195">You can update hello BGP community list of your route filter using hello following command:</span></span>

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="c6532-196"><a name="detach"></a>toodetach filtr trasy z okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="c6532-196"><a name="detach"></a>toodetach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="c6532-197">Jakmile filtr trasy odpojený od hello okruh ExpressRoute, jsou bez předpony inzerované prostřednictvím relace protokolu BGP hello.</span><span class="sxs-lookup"><span data-stu-id="c6532-197">Once a route filter is detached from hello ExpressRoute circuit, no prefixes are advertised through hello BGP session.</span></span> <span data-ttu-id="c6532-198">Můžete odpojit filtr trasy z okruhu ExpressRoute pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c6532-198">You can detach a route filter from an ExpressRoute circuit using hello following command:</span></span>
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="c6532-199"><a name="delete"></a>toodelete filtr trasy</span><span class="sxs-lookup"><span data-stu-id="c6532-199"><a name="delete"></a>toodelete a route filter</span></span>

<span data-ttu-id="c6532-200">Můžete pouze odstranění filtru trasy, pokud není připojen tooany okruh.</span><span class="sxs-lookup"><span data-stu-id="c6532-200">You can only delete a route filter if it is not attached tooany circuit.</span></span> <span data-ttu-id="c6532-201">Zkontrolujte tento filtr trasy hello není připojen tooany okruhu před pokusem o toodelete ho.</span><span class="sxs-lookup"><span data-stu-id="c6532-201">Ensure that hello route filter is not attached tooany circuit before attempting toodelete it.</span></span> <span data-ttu-id="c6532-202">Odstraněním trasy filtrovat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c6532-202">You can delete a route filter using hello following command:</span></span>

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="c6532-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6532-203">Next steps</span></span>

<span data-ttu-id="c6532-204">Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="c6532-204">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
