---
title: "Nastavit filtry směrování pro partnerský vztah Azure ExpressRoute Microsoftu: prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak nastavit filtry tras pro Microsoft Peering pomocí prostředí PowerShell"
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
ms.openlocfilehash: de3550c20439fa809869d98b8a57ea3be9c03e7c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a><span data-ttu-id="5d0d1-103">Konfigurace filtrů směrování pro partnerský vztah Microsoftu</span><span class="sxs-lookup"><span data-stu-id="5d0d1-103">Configure route filters for Microsoft peering</span></span>

<span data-ttu-id="5d0d1-104">Filtry tras představují způsob, jak využívat podmnožinu podporované služby prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-104">Route filters are a way to consume a subset of supported services through Microsoft peering.</span></span> <span data-ttu-id="5d0d1-105">Kroky v tomto článku vám pomůžou nakonfigurovat a spravovat filtry tras pro okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-105">The steps in this article help you configure and manage route filters for ExpressRoute circuits.</span></span>

<span data-ttu-id="5d0d1-106">Dynamics 365 služeb a služeb Office 365 například Exchange Online, SharePoint Online a Skype pro firmy, jsou přístupné prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-106">Dynamics 365 services, and Office 365 services such as Exchange Online, SharePoint Online, and Skype for Business, are accessible through the Microsoft peering.</span></span> <span data-ttu-id="5d0d1-107">Pokud partnerský vztah Microsoftu je nakonfigurován v okruhu ExpressRoute, jsou všechny předpony související s těmito službami inzerované prostřednictvím relace protokolu BGP, které jsou vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-107">When Microsoft peering is configured in an ExpressRoute circuit, all prefixes related to these services are advertised through the BGP sessions that are established.</span></span> <span data-ttu-id="5d0d1-108">Hodnota komunity protokolu BGP je připojený k každých předpona k identifikaci služby, které nabízí prostřednictvím předponu.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-108">A BGP community value is attached to every prefix to identify the service that is offered through the prefix.</span></span> <span data-ttu-id="5d0d1-109">Seznam hodnot komunity protokolu BGP a služeb, jsou mapovány najdete v tématu [komunit protokolu BGP](expressroute-routing.md#bgp).</span><span class="sxs-lookup"><span data-stu-id="5d0d1-109">For a list of the BGP community values and the services they  map to, see [BGP communities](expressroute-routing.md#bgp).</span></span>

<span data-ttu-id="5d0d1-110">Pokud budete potřebovat připojení ke všem službám, jsou velký počet předpon inzerovaných prostřednictvím protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-110">If you require connectivity to all services, a large number of prefixes are advertised through BGP.</span></span> <span data-ttu-id="5d0d1-111">Tím se výrazně zvyšuje velikost směrovací tabulky udržuje pomocí směrovačů v rámci vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-111">This significantly increases the size of the route tables maintained by routers within your network.</span></span> <span data-ttu-id="5d0d1-112">Pokud budete chtít využívat jenom podmnožinu službám nabízeným přes partnerský vztah Microsoftu, můžete snížit velikost vašeho směrovací tabulky dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-112">If you plan to consume only a subset of services offered through Microsoft peering, you can reduce the size of your route tables in two ways.</span></span> <span data-ttu-id="5d0d1-113">Můžete:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-113">You can:</span></span>

- <span data-ttu-id="5d0d1-114">Filtrovat nežádoucí předpony použitím filtry tras na komunit protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-114">Filter out unwanted prefixes by applying route filters on BGP communities.</span></span> <span data-ttu-id="5d0d1-115">Tento postup standardní sítě a běžně používá v rámci mnoho sítí.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-115">This is a standard networking practice and is used commonly within many networks.</span></span>

- <span data-ttu-id="5d0d1-116">Zadejte filtry tras a použít je pro váš okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-116">Define route filters and apply them to your ExpressRoute circuit.</span></span> <span data-ttu-id="5d0d1-117">Filtr trasy je nový prostředek, který umožňuje vybrat seznam služeb, které budete chtít využívat prostřednictvím partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-117">A route filter is a new resource that lets you select the list of services you plan to consume through Microsoft peering.</span></span> <span data-ttu-id="5d0d1-118">ExpressRoute směrovače odeslat pouze seznam předpon, které patří na služby určené ve filtru trasy.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-118">ExpressRoute routers only send the list of prefixes that belong to the services identified in the route filter.</span></span>

### <span data-ttu-id="5d0d1-119"><a name="about"></a>O filtrech směrování</span><span class="sxs-lookup"><span data-stu-id="5d0d1-119"><a name="about"></a>About route filters</span></span>

<span data-ttu-id="5d0d1-120">Pokud partnerský vztah Microsoftu je nakonfigurován na váš okruh ExpressRoute, Microsoft hraniční směrovače vytvořte dvojici relací protokolu BGP s hraniční směrovače (váš nebo váš poskytovatel připojení).</span><span class="sxs-lookup"><span data-stu-id="5d0d1-120">When Microsoft peering is configured on your ExpressRoute circuit, the Microsoft edge routers establish a pair of BGP sessions with the edge routers (yours or your connectivity provider's).</span></span> <span data-ttu-id="5d0d1-121">Žádné trasy inzerovány vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-121">No routes are advertised to your network.</span></span> <span data-ttu-id="5d0d1-122">Pokud chcete povolit inzerování trasy k síti, je nutné přidružit filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-122">To enable route advertisements to your network, you must associate a route filter.</span></span>

<span data-ttu-id="5d0d1-123">Filtr trasy umožňuje identifikoval služby, které chcete využívat prostřednictvím partnerského vztahu Microsoftu okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-123">A route filter lets you identify services you want to consume through your ExpressRoute circuit's Microsoft peering.</span></span> <span data-ttu-id="5d0d1-124">Je v podstatě prázdný seznam všech hodnot komunity protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-124">It is essentially a white list of all the BGP community values.</span></span> <span data-ttu-id="5d0d1-125">Po filtru prostředek trasy definované a připojené k okruhu ExpressRoute, jsou všechny předpony, které jsou mapovány na hodnoty komunity protokolu BGP inzerovaný do vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-125">Once a route filter resource is defined and attached to an ExpressRoute circuit, all prefixes that map to the BGP community values are advertised to your network.</span></span>

<span data-ttu-id="5d0d1-126">Abyste mohli připojit filtry tras se službami Office 365 na ně, musí mít oprávnění pro využívání služeb Office 365 prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-126">To be able to attach route filters with Office 365 services on them, you must have authorization to consume Office 365 services through ExpressRoute.</span></span> <span data-ttu-id="5d0d1-127">Pokud nemáte oprávnění k využívání služeb Office 365 prostřednictvím ExpressRoute, operace připojit filtry tras se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-127">If you are not authorized to consume Office 365 services through ExpressRoute, the operation to attach route filters fails.</span></span> <span data-ttu-id="5d0d1-128">Další informace o procesu autorizačního najdete v tématu [Azure ExpressRoute pro Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span><span class="sxs-lookup"><span data-stu-id="5d0d1-128">For more information about the authorization process, see [Azure ExpressRoute for Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd).</span></span> <span data-ttu-id="5d0d1-129">Připojení ke službám Dynamics 365 nevyžaduje žádné předchozí autorizace.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-129">Connectivity to Dynamics 365 services does NOT require any prior authorization.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d0d1-130">Okruhy ExpressRoute, které byly nakonfigurovány před 1. srpna 2017 partnerského vztahu Microsoftu bude mít všechny služby předpony inzerované prostřednictvím Microsoft partnerský vztah, i když nejsou definovány filtry tras.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-130">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="5d0d1-131">Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované dokud trasy filtr je připojen k okruhu.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-131">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span>
> 
> 

### <span data-ttu-id="5d0d1-132"><a name="workflow"></a>Pracovní postup</span><span class="sxs-lookup"><span data-stu-id="5d0d1-132"><a name="workflow"></a>Workflow</span></span>

<span data-ttu-id="5d0d1-133">Abyste mohli úspěšně připojit ke službám prostřednictvím partnerského vztahu Microsoftu, musíte provést následující kroky konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-133">To be able to successfully connect to services through Microsoft peering, you must complete the following configuration steps:</span></span>

- <span data-ttu-id="5d0d1-134">Musí mít aktivní okruh ExpressRoute, který má Microsoft partnerský vztah zřízené.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-134">You must have an active ExpressRoute circuit that has Microsoft peering provisioned.</span></span> <span data-ttu-id="5d0d1-135">K těmto úkolům můžete použít následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-135">You can use the following instructions to accomplish these tasks:</span></span>
  - <span data-ttu-id="5d0d1-136">[Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho povolený vaším poskytovatelem připojení, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-136">[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="5d0d1-137">Okruh ExpressRoute musí být ve stavu zřízený a povolený.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-137">The ExpressRoute circuit must be in a provisioned and enabled state.</span></span>
  - <span data-ttu-id="5d0d1-138">[Vytvoření partnerského vztahu Microsoftu](expressroute-circuit-peerings.md) Pokud budete spravovat přímo relaci protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-138">[Create Microsoft peering](expressroute-circuit-peerings.md) if you manage the BGP session directly.</span></span> <span data-ttu-id="5d0d1-139">Nebo váš poskytovatel připojení zřídit partnerský vztah Microsoftu pro váš okruh.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-139">Or, have your connectivity provider provision Microsoft peering for your circuit.</span></span>

-  <span data-ttu-id="5d0d1-140">Musíte vytvořit a konfigurovat filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-140">You must create and configure a route filter.</span></span>
    - <span data-ttu-id="5d0d1-141">Identifikovat služby vám využívat prostřednictvím partnerského vztahu Microsoftu</span><span class="sxs-lookup"><span data-stu-id="5d0d1-141">Identify the services you with to consume through Microsoft peering</span></span>
    - <span data-ttu-id="5d0d1-142">Určení seznamu hodnot komunity protokolu BGP přidruženého ke službám</span><span class="sxs-lookup"><span data-stu-id="5d0d1-142">Identify the list of BGP community values associated with the services</span></span>
    - <span data-ttu-id="5d0d1-143">Vytvořit pravidlo, které umožňují seznamu předponu odpovídající hodnoty komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="5d0d1-143">Create a rule to allow the prefix list matching the BGP community values</span></span>

-  <span data-ttu-id="5d0d1-144">Filtr trasy musí připojit k okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-144">You must attach the route filter to the ExpressRoute circuit.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5d0d1-145">Než začnete</span><span class="sxs-lookup"><span data-stu-id="5d0d1-145">Before you begin</span></span>

<span data-ttu-id="5d0d1-146">Před zahájením konfigurace, ujistěte se, že splňují následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-146">Before you begin configuration, make sure you meet the following criteria:</span></span>

 - <span data-ttu-id="5d0d1-147">Nainstalujte nejnovější verzi rutin PowerShellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-147">Install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="5d0d1-148">Další informace najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="5d0d1-148">For more information, see [Install and configure Azure PowerShelll](/powershell/azure/install-azurerm-ps).</span></span>

  > [!NOTE]
  > <span data-ttu-id="5d0d1-149">Stáhněte nejnovější verzi z Galerie prostředí PowerShell, nikoli pomocí Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-149">Download the latest version from the PowerShell Gallery, rather than using the Installer.</span></span> <span data-ttu-id="5d0d1-150">Instalační program aktuálně nepodporuje požadovaných rutin.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-150">The Installer currently does not support the required cmdlets.</span></span>
  > 

 - <span data-ttu-id="5d0d1-151">Zkontrolujte [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-151">Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>

 - <span data-ttu-id="5d0d1-152">Musí mít aktivní okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-152">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="5d0d1-153">Než budete pokračovat, podle pokynů [vytvořte okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho povolený vaším poskytovatelem připojení.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-153">Follow the instructions to [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="5d0d1-154">Okruh ExpressRoute musí být ve stavu zřízený a povolený.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-154">The ExpressRoute circuit must be in a provisioned and enabled state.</span></span>

 - <span data-ttu-id="5d0d1-155">Musíte mít active partnerský vztah Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-155">You must have an active Microsoft peering.</span></span> <span data-ttu-id="5d0d1-156">Postupujte podle pokynů v [vytvoření a úprava konfigurace partnerského vztahu](expressroute-circuit-peerings.md)</span><span class="sxs-lookup"><span data-stu-id="5d0d1-156">Follow instructions at [Create and modifying peering configuration](expressroute-circuit-peerings.md)</span></span>

### <a name="log-in-to-your-azure-account"></a><span data-ttu-id="5d0d1-157">Přihlaste se k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="5d0d1-157">Log in to your Azure account</span></span>

<span data-ttu-id="5d0d1-158">Před zahájením této konfigurace se musíte přihlásit ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-158">Before beginning this configuration, you must log in to your Azure account.</span></span> <span data-ttu-id="5d0d1-159">Tato rutina vás vyzve k zadání přihlašovacích údajů k vašemu účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-159">The cmdlet prompts you for the login credentials for your Azure account.</span></span> <span data-ttu-id="5d0d1-160">Po přihlášení se stáhne nastavení účtu, aby bylo dostupné v Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-160">After logging in, it downloads your account settings so they are available to Azure PowerShell.</span></span>

<span data-ttu-id="5d0d1-161">Otevřete konzolu PowerShellu se zvýšenými oprávněními a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-161">Open your PowerShell console with elevated privileges, and connect to your account.</span></span> <span data-ttu-id="5d0d1-162">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-162">Use the following example to help you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="5d0d1-163">Pokud máte více předplatných Azure, zkontrolujte předplatná pro daný účet.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-163">If you have multiple Azure subscriptions, check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="5d0d1-164">Určete předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-164">Specify the subscription that you want to use.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <span data-ttu-id="5d0d1-165"><a name="prefixes"></a>Krok 1.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-165"><a name="prefixes"></a>Step 1.</span></span> <span data-ttu-id="5d0d1-166">Získat seznam předpon a hodnotami komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="5d0d1-166">Get a list of prefixes and BGP community values</span></span>

### <a name="1-get-a-list-of-bgp-community-values"></a><span data-ttu-id="5d0d1-167">1. Získat seznam hodnot komunity protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="5d0d1-167">1. Get a list of BGP community values</span></span>

<span data-ttu-id="5d0d1-168">Chcete-li získat seznam hodnot komunity protokolu BGP souvisejících se službou přístupné prostřednictvím partnerského vztahu Microsoftu a seznam předpon, které jsou s nimi spojených použijte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-168">Use the following cmdlet to get the list of BGP community values associated with services accessible through Microsoft peering, and the list of prefixes associated with them:</span></span>

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a><span data-ttu-id="5d0d1-169">2. Zkontrolujte seznam hodnot, které chcete použít</span><span class="sxs-lookup"><span data-stu-id="5d0d1-169">2. Make a list of the values that you want to use</span></span>

<span data-ttu-id="5d0d1-170">Zkontrolujte seznam hodnotami komunity protokolu BGP, které chcete použít ve filtru trasy.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-170">Make a list of BGP community values you want to use in the route filter.</span></span> <span data-ttu-id="5d0d1-171">Jako příklad je hodnota komunity protokolu BGP pro služby Dynamics 365 12076:5040.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-171">As an example, the BGP community value for Dynamics 365 services is 12076:5040.</span></span>

## <span data-ttu-id="5d0d1-172"><a name="filter"></a>Krok 2.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-172"><a name="filter"></a>Step 2.</span></span> <span data-ttu-id="5d0d1-173">Vytvořte filtr trasy a pravidlo filtru</span><span class="sxs-lookup"><span data-stu-id="5d0d1-173">Create a route filter and a filter rule</span></span>

<span data-ttu-id="5d0d1-174">Filtr trasy může mít pouze jedno pravidlo a pravidlo musí být typu 'Povolit'.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-174">A route filter can have only one rule, and the rule must be of type 'Allow'.</span></span> <span data-ttu-id="5d0d1-175">Toto pravidlo může mít seznam hodnot komunity protokolu BGP s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-175">This rule can have a list of BGP community values associated with it.</span></span>

### <a name="1-create-a-route-filter"></a><span data-ttu-id="5d0d1-176">1. Vytvoření filtru trasy</span><span class="sxs-lookup"><span data-stu-id="5d0d1-176">1. Create a route filter</span></span>

<span data-ttu-id="5d0d1-177">Nejprve vytvořte filtr trasy.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-177">First, create the route filter.</span></span> <span data-ttu-id="5d0d1-178">Příkaz 'New-AzureRmRouteFilter, pouze vytvoří prostředek filtru trasy.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-178">The command 'New-AzureRmRouteFilter' only creates a route filter resource.</span></span> <span data-ttu-id="5d0d1-179">Po vytvoření prostředku, musí pak vytvořit pravidlo a připojte ji k objektu filtru trasy.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-179">After you create the resource, you must then create a rule and attach it to the route filter object.</span></span> <span data-ttu-id="5d0d1-180">Spusťte následující příkaz pro vytvoření filtru prostředku trasy:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-180">Run the following command to create a route filter resource:</span></span>

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a><span data-ttu-id="5d0d1-181">2. Vytvořit pravidlo filtru</span><span class="sxs-lookup"><span data-stu-id="5d0d1-181">2. Create a filter rule</span></span>

<span data-ttu-id="5d0d1-182">Sadu komunit protokolu BGP jako seznam oddělený čárkami, můžete zadat, jak je znázorněno v příkladu.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-182">You can specify a set of BGP communities as a comma-separated list, as shown in the example.</span></span> <span data-ttu-id="5d0d1-183">Spusťte následující příkaz pro vytvoření nového pravidla:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-183">Run the following command to create a new rule:</span></span>
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-the-rule-to-the-route-filter"></a><span data-ttu-id="5d0d1-184">3. Přidat pravidlo filtru trasy</span><span class="sxs-lookup"><span data-stu-id="5d0d1-184">3. Add the rule to the route filter</span></span>

<span data-ttu-id="5d0d1-185">Spusťte následující příkaz pro přidání tomuto pravidlu filtru do filtru tras:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-185">Run the following command to add the filter rule to the route filter:</span></span>
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="5d0d1-186"><a name="attach"></a>Krok 3.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-186"><a name="attach"></a>Step 3.</span></span> <span data-ttu-id="5d0d1-187">Připojte filtr trasy k okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5d0d1-187">Attach the route filter to an ExpressRoute circuit</span></span>

<span data-ttu-id="5d0d1-188">Spusťte následující příkaz pro připojení filtru trasy k okruhu ExpressRoute, za předpokladu, že máte jenom partnerského vztahu Microsoftu:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-188">Run the following command to attach the route filter to the ExpressRoute circuit, assuming you have only Microsoft peering:</span></span>

```powershell
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="5d0d1-189"><a name="getproperties"></a>Chcete-li získat vlastnosti filtru trasy</span><span class="sxs-lookup"><span data-stu-id="5d0d1-189"><a name="getproperties"></a>To get the properties of a route filter</span></span>

<span data-ttu-id="5d0d1-190">Vlastnosti filtru trasy, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-190">To get the properties of a route filter, use the following steps:</span></span>

1. <span data-ttu-id="5d0d1-191">Spusťte následující příkaz, který vám daný prostředek trasy filtru:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-191">Run the following command to get the route filter resource:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. <span data-ttu-id="5d0d1-192">Získáte trasy pravidla filtru pro prostředek trasy filtru, tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-192">Get the route filter rules for the route-filter resource by running the following command:</span></span>

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

## <span data-ttu-id="5d0d1-193"><a name="updateproperties"></a>Aktualizovat vlastnosti filtr trasy</span><span class="sxs-lookup"><span data-stu-id="5d0d1-193"><a name="updateproperties"></a>To update the properties of a route filter</span></span>

<span data-ttu-id="5d0d1-194">Pokud trasa filtru je již připojen k okruhu, aktualizace do seznamu komunity protokolu BGP automaticky rozšíří změny příslušné předpony oznámení o inzerovaném programu prostřednictvím zavedených relací protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-194">If the route filter is already attached to a circuit, updates to the BGP community list automatically propagates appropriate prefix advertisement changes through the established BGP sessions.</span></span> <span data-ttu-id="5d0d1-195">Můžete aktualizovat seznam komunity protokolu BGP filtru trasu pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-195">You can update the BGP community list of your route filter using the following command:</span></span>

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <span data-ttu-id="5d0d1-196"><a name="detach"></a>Odpojit filtr trasy z okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5d0d1-196"><a name="detach"></a>To detach a route filter from an ExpressRoute circuit</span></span>

<span data-ttu-id="5d0d1-197">Jakmile trasy filtr je odpojená od okruhu ExpressRoute, jsou bez předpony inzerované prostřednictvím relace protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-197">Once a route filter is detached from the ExpressRoute circuit, no prefixes are advertised through the BGP session.</span></span> <span data-ttu-id="5d0d1-198">Můžete odpojit filtr trasy z okruhu ExpressRoute pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-198">You can detach a route filter from an ExpressRoute circuit using the following command:</span></span>
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <span data-ttu-id="5d0d1-199"><a name="delete"></a>Odstranění filtru trasy</span><span class="sxs-lookup"><span data-stu-id="5d0d1-199"><a name="delete"></a>To delete a route filter</span></span>

<span data-ttu-id="5d0d1-200">Filtr trasy může odstranit pouze, pokud není připojený k žádné okruh.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-200">You can only delete a route filter if it is not attached to any circuit.</span></span> <span data-ttu-id="5d0d1-201">Zkontrolujte, že filtr trasy není připojen k žádné okruh před pokusem o jeho odstranění.</span><span class="sxs-lookup"><span data-stu-id="5d0d1-201">Ensure that the route filter is not attached to any circuit before attempting to delete it.</span></span> <span data-ttu-id="5d0d1-202">Odstraněním trasy filtr, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5d0d1-202">You can delete a route filter using the following command:</span></span>

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a><span data-ttu-id="5d0d1-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d0d1-203">Next steps</span></span>

<span data-ttu-id="5d0d1-204">Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="5d0d1-204">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>