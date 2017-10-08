---
title: "aaaUsing prostředí PowerShell toomanage Traffic Manageru v Azure | Microsoft Docs"
description: "Pomocí prostředí PowerShell pro správce provozu pomocí Azure Resource Manageru"
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: bc247448-1d2e-4104-ac03-42b59ebde065
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 018c37db63beb82fdad54cfd7e13ab3cb645723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-powershell-toomanage-traffic-manager"></a><span data-ttu-id="0befb-103">Pomocí prostředí PowerShell toomanage Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="0befb-103">Using PowerShell toomanage Traffic Manager</span></span>

<span data-ttu-id="0befb-104">Azure Resource Manager je hello upřednostňované správu rozhraní pro služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="0befb-104">Azure Resource Manager is hello preferred management interface for services in Azure.</span></span> <span data-ttu-id="0befb-105">Profilů Azure Traffic Manager můžete spravovat pomocí rozhraní API založené na Azure Resource Manager a nástroje.</span><span class="sxs-lookup"><span data-stu-id="0befb-105">Azure Traffic Manager profiles can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="resource-model"></a><span data-ttu-id="0befb-106">Model prostředků</span><span class="sxs-lookup"><span data-stu-id="0befb-106">Resource model</span></span>

<span data-ttu-id="0befb-107">Azure Traffic Manager je konfigurovat pomocí kolekce nastavení nazývá profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="0befb-107">Azure Traffic Manager is configured using a collection of settings called a Traffic Manager profile.</span></span> <span data-ttu-id="0befb-108">Tento profil obsahuje nastavení DNS, nastavení směrování provozu, nastavení monitorování koncového bodu a prochází seznam přenosů toowhich koncové body služby.</span><span class="sxs-lookup"><span data-stu-id="0befb-108">This profile contains DNS settings, traffic routing settings, endpoint monitoring settings, and a list of service endpoints toowhich traffic is routed.</span></span>

<span data-ttu-id="0befb-109">Každý profil služby Traffic Manager je reprezentována prostředek typu 'TrafficManagerProfiles'.</span><span class="sxs-lookup"><span data-stu-id="0befb-109">Each Traffic Manager profile is represented by a resource of type 'TrafficManagerProfiles'.</span></span> <span data-ttu-id="0befb-110">V hello úroveň rozhraní API REST hello identifikátor URI pro každý profil je následující:</span><span class="sxs-lookup"><span data-stu-id="0befb-110">At hello REST API level, hello URI for each profile is as follows:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a><span data-ttu-id="0befb-111">Nastavení prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="0befb-111">Setting up Azure PowerShell</span></span>

<span data-ttu-id="0befb-112">Tyto pokyny použijte Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0befb-112">These instructions use Microsoft Azure PowerShell.</span></span> <span data-ttu-id="0befb-113">Hello následující článek vysvětluje, jak tooinstall a konfigurace prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0befb-113">hello following article explains how tooinstall and configure Azure PowerShell.</span></span>

* [<span data-ttu-id="0befb-114">Jak tooinstall a konfigurace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="0befb-114">How tooinstall and configure Azure PowerShell</span></span>](/powershell/azure/overview)

<span data-ttu-id="0befb-115">Hello příklady v tomto článku předpokládá, že máte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="0befb-115">hello examples in this article assume that you have an existing resource group.</span></span> <span data-ttu-id="0befb-116">Můžete vytvořit skupinu prostředků pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0befb-116">You can create a resource group using hello following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> <span data-ttu-id="0befb-117">Azure Resource Manager vyžaduje, zda mají všechny skupiny prostředků do umístění.</span><span class="sxs-lookup"><span data-stu-id="0befb-117">Azure Resource Manager requires that all resource groups have a location.</span></span> <span data-ttu-id="0befb-118">Toto umístění se používá jako výchozí hello pro prostředky vytvořené v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="0befb-118">This location is used as hello default for resources created in that resource group.</span></span> <span data-ttu-id="0befb-119">Ale vzhledem k tomu, že globální, a ne místní prostředky profil správce provozu se hello Volba umístění skupiny prostředků nemá žádný vliv na Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="0befb-119">However, since Traffic Manager profile resources are global, not regional, hello choice of resource group location has no impact on Azure Traffic Manager.</span></span>

## <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="0befb-120">Vytvoření profilu Správce provozu</span><span class="sxs-lookup"><span data-stu-id="0befb-120">Create a Traffic Manager Profile</span></span>

<span data-ttu-id="0befb-121">toocreate profil Traffic Manageru použijte hello `New-AzureRmTrafficManagerProfile` rutiny:</span><span class="sxs-lookup"><span data-stu-id="0befb-121">toocreate a Traffic Manager profile, use hello `New-AzureRmTrafficManagerProfile` cmdlet:</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

<span data-ttu-id="0befb-122">Hello následující tabulka popisuje parametry hello:</span><span class="sxs-lookup"><span data-stu-id="0befb-122">hello following table describes hello parameters:</span></span>

| <span data-ttu-id="0befb-123">Parametr</span><span class="sxs-lookup"><span data-stu-id="0befb-123">Parameter</span></span> | <span data-ttu-id="0befb-124">Popis</span><span class="sxs-lookup"><span data-stu-id="0befb-124">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0befb-125">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="0befb-125">Name</span></span> |<span data-ttu-id="0befb-126">název prostředku Hello hello prostředků profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="0befb-126">hello resource name for hello Traffic Manager profile resource.</span></span> <span data-ttu-id="0befb-127">Profily v nástroji hello stejné skupiny prostředků musí mít jedinečné názvy.</span><span class="sxs-lookup"><span data-stu-id="0befb-127">Profiles in hello same resource group must have unique names.</span></span> <span data-ttu-id="0befb-128">Tento název je jiný než název DNS hello používá pro dotazy DNS.</span><span class="sxs-lookup"><span data-stu-id="0befb-128">This name is separate from hello DNS name used for DNS queries.</span></span> |
| <span data-ttu-id="0befb-129">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="0befb-129">ResourceGroupName</span></span> |<span data-ttu-id="0befb-130">Název Hello hello prostředků skupiny obsahující hello profil prostředku.</span><span class="sxs-lookup"><span data-stu-id="0befb-130">hello name of hello resource group containing hello profile resource.</span></span> |
| <span data-ttu-id="0befb-131">TrafficRoutingMethod</span><span class="sxs-lookup"><span data-stu-id="0befb-131">TrafficRoutingMethod</span></span> |<span data-ttu-id="0befb-132">Určuje hello směrování provozu metodu použitou toodetermine kterému koncovému bodu je vrácený v odpovědi dotaz DNS.</span><span class="sxs-lookup"><span data-stu-id="0befb-132">Specifies hello traffic-routing method used toodetermine which endpoint is returned in response a DNS query.</span></span> <span data-ttu-id="0befb-133">Možné hodnoty jsou 'Výkonu', "Vážená" nebo "Priority".</span><span class="sxs-lookup"><span data-stu-id="0befb-133">Possible values are 'Performance', 'Weighted' or 'Priority'.</span></span> |
| <span data-ttu-id="0befb-134">RelativeDnsName</span><span class="sxs-lookup"><span data-stu-id="0befb-134">RelativeDnsName</span></span> |<span data-ttu-id="0befb-135">Určuje část názvu hostitele hello název DNS hello poskytované tento profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="0befb-135">Specifies hello hostname portion of hello DNS name provided by this Traffic Manager profile.</span></span> <span data-ttu-id="0befb-136">Tato hodnota je kombinovat s názvem domény DNS hello používá Azure Traffic Manager tooform hello plně kvalifikovaný název domény (FQDN) hello profilu.</span><span class="sxs-lookup"><span data-stu-id="0befb-136">This value is combined with hello DNS domain name used by Azure Traffic Manager tooform hello fully qualified domain name (FQDN) of hello profile.</span></span> <span data-ttu-id="0befb-137">Například nastavení hodnoty hello "contoso" bude "contoso.trafficmanager.net."</span><span class="sxs-lookup"><span data-stu-id="0befb-137">For example, setting hello value of 'contoso' becomes 'contoso.trafficmanager.net.'</span></span> |
| <span data-ttu-id="0befb-138">HODNOTA TTL</span><span class="sxs-lookup"><span data-stu-id="0befb-138">TTL</span></span> |<span data-ttu-id="0befb-139">Určuje hello DNS Time-to-Live (TTL), v sekundách.</span><span class="sxs-lookup"><span data-stu-id="0befb-139">Specifies hello DNS Time-to-Live (TTL), in seconds.</span></span> <span data-ttu-id="0befb-140">Tato hodnota TTL informuje překladače hello místní DNS a klienty DNS, jak dlouho toocache odpovědí DNS pro tento profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="0befb-140">This TTL informs hello Local DNS resolvers and DNS clients how long toocache DNS responses for this Traffic Manager profile.</span></span> |
| <span data-ttu-id="0befb-141">MonitorProtocol</span><span class="sxs-lookup"><span data-stu-id="0befb-141">MonitorProtocol</span></span> |<span data-ttu-id="0befb-142">Určuje hello protokol toouse toomonitor koncový bod stavu.</span><span class="sxs-lookup"><span data-stu-id="0befb-142">Specifies hello protocol toouse toomonitor endpoint health.</span></span> <span data-ttu-id="0befb-143">Možné hodnoty jsou "HTTP" a "HTTPS".</span><span class="sxs-lookup"><span data-stu-id="0befb-143">Possible values are 'HTTP' and 'HTTPS'.</span></span> |
| <span data-ttu-id="0befb-144">MonitorPort</span><span class="sxs-lookup"><span data-stu-id="0befb-144">MonitorPort</span></span> |<span data-ttu-id="0befb-145">Určuje, že hello TCP port používá toomonitor koncový bod stavu.</span><span class="sxs-lookup"><span data-stu-id="0befb-145">Specifies hello TCP port used toomonitor endpoint health.</span></span> |
| <span data-ttu-id="0befb-146">MonitorPath</span><span class="sxs-lookup"><span data-stu-id="0befb-146">MonitorPath</span></span> |<span data-ttu-id="0befb-147">Určuje, že název domény pro koncový bod hello cesta relativní toohello používá tooprobe pro koncový bod stavu.</span><span class="sxs-lookup"><span data-stu-id="0befb-147">Specifies hello path relative toohello endpoint domain name used tooprobe for endpoint health.</span></span> |

<span data-ttu-id="0befb-148">Hello rutina vytvoří profil Traffic Manageru v Azure a vrátí odpovídající tooPowerShell objekt profilu.</span><span class="sxs-lookup"><span data-stu-id="0befb-148">hello cmdlet creates a Traffic Manager profile in Azure and returns a corresponding profile object tooPowerShell.</span></span> <span data-ttu-id="0befb-149">V tomto okamžiku hello profil neobsahuje žádné koncové body.</span><span class="sxs-lookup"><span data-stu-id="0befb-149">At this point, hello profile does not contain any endpoints.</span></span> <span data-ttu-id="0befb-150">Další informace o přidávání profil služby Traffic Manager tooa koncové body najdete v tématu [přidání koncové body Traffic Manager](#adding-traffic-manager-endpoints).</span><span class="sxs-lookup"><span data-stu-id="0befb-150">For more information about adding endpoints tooa Traffic Manager profile, see [Adding Traffic Manager Endpoints](#adding-traffic-manager-endpoints).</span></span>

## <a name="get-a-traffic-manager-profile"></a><span data-ttu-id="0befb-151">Získat profil správce provozu</span><span class="sxs-lookup"><span data-stu-id="0befb-151">Get a Traffic Manager Profile</span></span>

<span data-ttu-id="0befb-152">tooretrieve objekt existující profil Traffic Manageru použijte hello `Get-AzureRmTrafficManagerProfle` rutiny:</span><span class="sxs-lookup"><span data-stu-id="0befb-152">tooretrieve an existing Traffic Manager profile object, use hello `Get-AzureRmTrafficManagerProfle` cmdlet:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="0befb-153">Tato rutina vrátí objekt profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="0befb-153">This cmdlet returns a Traffic Manager profile object.</span></span>

## <a name="update-a-traffic-manager-profile"></a><span data-ttu-id="0befb-154">Aktualizovat profil správce provozu</span><span class="sxs-lookup"><span data-stu-id="0befb-154">Update a Traffic Manager Profile</span></span>

<span data-ttu-id="0befb-155">Úprava profily Traffic Manager zahrnuje proces krok 3:</span><span class="sxs-lookup"><span data-stu-id="0befb-155">Modifying Traffic Manager profiles follows a 3-step process:</span></span>

1. <span data-ttu-id="0befb-156">Načtení hello profil pomocí `Get-AzureRmTrafficManagerProfile` nebo použijte profil hello vrácený `New-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="0befb-156">Retrieve hello profile using `Get-AzureRmTrafficManagerProfile` or use hello profile returned by `New-AzureRmTrafficManagerProfile`.</span></span>
2. <span data-ttu-id="0befb-157">Upravte profil hello.</span><span class="sxs-lookup"><span data-stu-id="0befb-157">Modify hello profile.</span></span> <span data-ttu-id="0befb-158">Můžete přidat a odebrat koncových bodů nebo změňte parametry koncového bodu nebo profilu.</span><span class="sxs-lookup"><span data-stu-id="0befb-158">You can add and remove endpoints or change endpoint or profile parameters.</span></span> <span data-ttu-id="0befb-159">Tyto změny jsou offline operace.</span><span class="sxs-lookup"><span data-stu-id="0befb-159">These changes are off-line operations.</span></span> <span data-ttu-id="0befb-160">Měníte jenom místní objekt hello v paměti, která představuje hello profilu.</span><span class="sxs-lookup"><span data-stu-id="0befb-160">You are only changing hello local object in memory that represents hello profile.</span></span>
3. <span data-ttu-id="0befb-161">Potvrdit změny pomocí hello `Set-AzureRmTrafficManagerProfile` rutiny.</span><span class="sxs-lookup"><span data-stu-id="0befb-161">Commit your changes using hello `Set-AzureRmTrafficManagerProfile` cmdlet.</span></span>

<span data-ttu-id="0befb-162">S výjimkou hello profil RelativeDnsName lze změnit všechny vlastnosti profilu.</span><span class="sxs-lookup"><span data-stu-id="0befb-162">All profile properties can be changed except hello profile's RelativeDnsName.</span></span> <span data-ttu-id="0befb-163">toochange hello RelativeDnsName, je nutné odstranit profil a nový profil s novým názvem.</span><span class="sxs-lookup"><span data-stu-id="0befb-163">toochange hello RelativeDnsName, you must delete profile and a new profile with a new name.</span></span>

<span data-ttu-id="0befb-164">Hello následující příklad ukazuje, jak toochange hello profilu TTL:</span><span class="sxs-lookup"><span data-stu-id="0befb-164">hello following example demonstrates how toochange hello profile's TTL:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="0befb-165">Existují tři typy koncových bodů Traffic Manager:</span><span class="sxs-lookup"><span data-stu-id="0befb-165">There are three types of Traffic Manager endpoints:</span></span>

1. <span data-ttu-id="0befb-166">**Koncové body Azure** jsou služby hostované v Azure</span><span class="sxs-lookup"><span data-stu-id="0befb-166">**Azure endpoints** are services hosted in Azure</span></span>
2. <span data-ttu-id="0befb-167">**Externí koncové body** jsou služby hostované mimo Azure</span><span class="sxs-lookup"><span data-stu-id="0befb-167">**External endpoints** are services hosted outside of Azure</span></span>
3. <span data-ttu-id="0befb-168">**Vnořené koncové body** jsou použité tooconstruct vnořené hierarchie profily Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="0befb-168">**Nested endpoints** are used tooconstruct nested hierarchies of Traffic Manager profiles.</span></span> <span data-ttu-id="0befb-169">Vnořené koncové body povolit rozšířené konfigurace směrování provozu pro komplexní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0befb-169">Nested endpoints enable advanced traffic-routing configurations for complex applications.</span></span>

<span data-ttu-id="0befb-170">Ve všech případech tři koncové body můžete přidat dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="0befb-170">In all three cases, endpoints can be added in two ways:</span></span>

1. <span data-ttu-id="0befb-171">Pomocí kroku 3 postup popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="0befb-171">Using a 3-step process described previously.</span></span> <span data-ttu-id="0befb-172">Hello Výhodou této metody je, že lze provést několik změn koncového bodu v jednu aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="0befb-172">hello advantage of this method is that several endpoint changes can be made in a single update.</span></span>
2. <span data-ttu-id="0befb-173">Pomocí rutiny New-AzureRmTrafficManagerEndpoint hello.</span><span class="sxs-lookup"><span data-stu-id="0befb-173">Using hello New-AzureRmTrafficManagerEndpoint cmdlet.</span></span> <span data-ttu-id="0befb-174">Tato rutina přidá koncový bod tooan existující profil služby Traffic Manager v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="0befb-174">This cmdlet adds an endpoint tooan existing Traffic Manager profile in a single operation.</span></span>

## <a name="adding-azure-endpoints"></a><span data-ttu-id="0befb-175">Přidání koncové body Azure</span><span class="sxs-lookup"><span data-stu-id="0befb-175">Adding Azure Endpoints</span></span>

<span data-ttu-id="0befb-176">Koncové body Azure odkazovat služby hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="0befb-176">Azure endpoints reference services hosted in Azure.</span></span> <span data-ttu-id="0befb-177">Podporuje dva typy koncové body Azure:</span><span class="sxs-lookup"><span data-stu-id="0befb-177">Two types of Azure endpoints are supported:</span></span>

1. <span data-ttu-id="0befb-178">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="0befb-178">Azure Web Apps</span></span>
2. <span data-ttu-id="0befb-179">Azure prostředky PublicIpAddress (které můžou být připojené tooa služby Vyrovnávání zatížení nebo virtuální počítač síťový adaptér).</span><span class="sxs-lookup"><span data-stu-id="0befb-179">Azure PublicIpAddress resources (which can be attached tooa load-balancer or a virtual machine NIC).</span></span> <span data-ttu-id="0befb-180">Hello PublicIpAddress musí mít název DNS přiřazené toobe použít v Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="0befb-180">hello PublicIpAddress must have a DNS name assigned toobe used in Traffic Manager.</span></span>

<span data-ttu-id="0befb-181">V každém případě:</span><span class="sxs-lookup"><span data-stu-id="0befb-181">In each case:</span></span>

* <span data-ttu-id="0befb-182">Hello služby je zadán pomocí parametru 'targetResourceId' hello `Add-AzureRmTrafficManagerEndpointConfig` nebo `New-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="0befb-182">hello service is specified using hello 'targetResourceId' parameter of `Add-AzureRmTrafficManagerEndpointConfig` or `New-AzureRmTrafficManagerEndpoint`.</span></span>
* <span data-ttu-id="0befb-183">Hello "Target" a 'EndpointLocation' jsou implikované hello TargetResourceId.</span><span class="sxs-lookup"><span data-stu-id="0befb-183">hello 'Target' and 'EndpointLocation' are implied by hello TargetResourceId.</span></span>
* <span data-ttu-id="0befb-184">Určení hello 'Weight' je volitelné.</span><span class="sxs-lookup"><span data-stu-id="0befb-184">Specifying hello 'Weight' is optional.</span></span> <span data-ttu-id="0befb-185">Váhu používají jenom Pokud je profil hello nakonfigurované toouse hello 'Vážená' směrování provozu metoda.</span><span class="sxs-lookup"><span data-stu-id="0befb-185">Weights are only used if hello profile is configured toouse hello 'Weighted' traffic-routing method.</span></span> <span data-ttu-id="0befb-186">Jinak jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="0befb-186">Otherwise, they are ignored.</span></span> <span data-ttu-id="0befb-187">-Li zadána, hello hodnota musí být číslo v rozsahu od 1 do 1000.</span><span class="sxs-lookup"><span data-stu-id="0befb-187">If specified, hello value must be a number between 1 and 1000.</span></span> <span data-ttu-id="0befb-188">Hello výchozí hodnota je '1'.</span><span class="sxs-lookup"><span data-stu-id="0befb-188">hello default value is '1'.</span></span>
* <span data-ttu-id="0befb-189">Zadání hello "Priority" je volitelný.</span><span class="sxs-lookup"><span data-stu-id="0befb-189">Specifying hello 'Priority' is optional.</span></span> <span data-ttu-id="0befb-190">Priority používají jenom Pokud je profil hello nakonfigurované toouse hello "Priority" směrování provozu metoda.</span><span class="sxs-lookup"><span data-stu-id="0befb-190">Priorities are only used if hello profile is configured toouse hello 'Priority' traffic-routing method.</span></span> <span data-ttu-id="0befb-191">Jinak jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="0befb-191">Otherwise, they are ignored.</span></span> <span data-ttu-id="0befb-192">Platné hodnoty jsou 1 too1000 s nižšími hodnotami, která znamená vyšší prioritu.</span><span class="sxs-lookup"><span data-stu-id="0befb-192">Valid values are from 1 too1000 with lower values indicating a higher priority.</span></span> <span data-ttu-id="0befb-193">Pokud je zadán pro jeden koncový bod, musí být zadán pro všechny koncové body.</span><span class="sxs-lookup"><span data-stu-id="0befb-193">If specified for one endpoint, they must be specified for all endpoints.</span></span> <span data-ttu-id="0befb-194">Pokud tento parametr vynechán, se použijí výchozí hodnoty od '1' v hello pořadí, jsou uvedeny hello koncové body.</span><span class="sxs-lookup"><span data-stu-id="0befb-194">If omitted, default values starting from '1' are applied in hello order that hello endpoints are listed.</span></span>

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a><span data-ttu-id="0befb-195">Příklad 1: Přidání koncových bodů webové aplikace pomocí`Add-AzureRmTrafficManagerEndpointConfig`</span><span class="sxs-lookup"><span data-stu-id="0befb-195">Example 1: Adding Web App endpoints using `Add-AzureRmTrafficManagerEndpointConfig`</span></span>

<span data-ttu-id="0befb-196">V tomto příkladu jsme vytvořte profil Traffic Manageru a přidejte dva koncové body webové aplikace pomocí hello `Add-AzureRmTrafficManagerEndpointConfig` rutiny.</span><span class="sxs-lookup"><span data-stu-id="0befb-196">In this example, we create a Traffic Manager profile and add two Web App endpoints using hello `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="0befb-197">Příklad 2: Přidání koncového bodu publicIpAddress pomocí`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="0befb-197">Example 2: Adding a publicIpAddress endpoint using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="0befb-198">V tomto příkladu je přidat prostředek veřejné IP adresy toohello profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="0befb-198">In this example, a public IP address resource is added toohello Traffic Manager profile.</span></span> <span data-ttu-id="0befb-199">Hello veřejnou IP adresu musí mít název DNS nakonfigurovaný a může být vázána buď toohello síťová karta tohoto virtuálního počítače nebo tooa Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="0befb-199">hello public IP address must have a DNS name configured, and can be bound either toohello NIC of a VM or tooa load balancer.</span></span>

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a><span data-ttu-id="0befb-200">Přidání externí koncové body</span><span class="sxs-lookup"><span data-stu-id="0befb-200">Adding External Endpoints</span></span>

<span data-ttu-id="0befb-201">Provoz Manager používá externí koncové body toodirect provoz tooservices hostované mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="0befb-201">Traffic Manager uses external endpoints toodirect traffic tooservices hosted outside of Azure.</span></span> <span data-ttu-id="0befb-202">Jak se koncové body Azure externí koncové body přidáním buď pomocí `Add-AzureRmTrafficManagerEndpointConfig` následuje `Set-AzureRmTrafficManagerProfile`, nebo `New-AzureRMTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="0befb-202">As with Azure endpoints, external endpoints can be added either using `Add-AzureRmTrafficManagerEndpointConfig` followed by `Set-AzureRmTrafficManagerProfile`, or `New-AzureRMTrafficManagerEndpoint`.</span></span>

<span data-ttu-id="0befb-203">Při zadání externí koncové body:</span><span class="sxs-lookup"><span data-stu-id="0befb-203">When specifying external endpoints:</span></span>

* <span data-ttu-id="0befb-204">název domény Hello koncového bodu musí být určen pomocí parametru 'Target' hello</span><span class="sxs-lookup"><span data-stu-id="0befb-204">hello endpoint domain name must be specified using hello 'Target' parameter</span></span>
* <span data-ttu-id="0befb-205">Pokud se používá metoda směrování provozu "Výkonu" hello, je třeba hello 'EndpointLocation'.</span><span class="sxs-lookup"><span data-stu-id="0befb-205">If hello 'Performance' traffic-routing method is used, hello 'EndpointLocation' is required.</span></span> <span data-ttu-id="0befb-206">V opačném případě je volitelný.</span><span class="sxs-lookup"><span data-stu-id="0befb-206">Otherwise it is optional.</span></span> <span data-ttu-id="0befb-207">Hello hodnota musí být [platný oblast Azure název](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="0befb-207">hello value must be a [valid Azure region name](https://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="0befb-208">Hello 'Vážené' a 'prioritu, jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="0befb-208">hello 'Weight' and 'Priority' are optional.</span></span>

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="0befb-209">Příklad 1: Přidání externí koncové body pomocí `Add-AzureRmTrafficManagerEndpointConfig` a`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="0befb-209">Example 1: Adding external endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="0befb-210">V tomto příkladu jsme vytvořit profil Traffic Manageru, přidejte dva externí koncové body a provést změny hello.</span><span class="sxs-lookup"><span data-stu-id="0befb-210">In this example, we create a Traffic Manager profile, add two external endpoints, and commit hello changes.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="0befb-211">Příklad 2: Přidání externí koncové body pomocí`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="0befb-211">Example 2: Adding external endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="0befb-212">V tomto příkladu přidáme stávající profil tooan externí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="0befb-212">In this example, we add an external endpoint tooan existing profile.</span></span> <span data-ttu-id="0befb-213">profil Hello je zadán pomocí názvy hello profil a prostředků skupiny.</span><span class="sxs-lookup"><span data-stu-id="0befb-213">hello profile is specified using hello profile and resource group names.</span></span>

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a><span data-ttu-id="0befb-214">Přidání koncových bodů 'vnořené.</span><span class="sxs-lookup"><span data-stu-id="0befb-214">Adding 'Nested' endpoints</span></span>

<span data-ttu-id="0befb-215">Každý profil služby Traffic Manager určuje jednu metodu směrování provozu.</span><span class="sxs-lookup"><span data-stu-id="0befb-215">Each Traffic Manager profile specifies a single traffic-routing method.</span></span> <span data-ttu-id="0befb-216">Existují však scénáře, které vyžadují složitější směrování provozu, než hello směrování poskytuje jeden profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="0befb-216">However, there are scenarios that require more sophisticated traffic routing than hello routing provided by a single Traffic Manager profile.</span></span> <span data-ttu-id="0befb-217">Lze vnořit profily Traffic Manager toocombine hello výhody víc než jednu metodu směrování provozu.</span><span class="sxs-lookup"><span data-stu-id="0befb-217">You can nest Traffic Manager profiles toocombine hello benefits of more than one traffic-routing method.</span></span> <span data-ttu-id="0befb-218">Vnořené profily umožňují toooverride hello výchozí Traffic Manager chování toosupport větší a složitější nasazení aplikací.</span><span class="sxs-lookup"><span data-stu-id="0befb-218">Nested profiles allow you toooverride hello default Traffic Manager behavior toosupport larger and more complex application deployments.</span></span> <span data-ttu-id="0befb-219">Podrobné příklady, najdete v části [profily Traffic Manageru vnořené](traffic-manager-nested-profiles.md).</span><span class="sxs-lookup"><span data-stu-id="0befb-219">For more detailed examples, see [Nested Traffic Manager profiles](traffic-manager-nested-profiles.md).</span></span>

<span data-ttu-id="0befb-220">Vnořené koncových bodů jsou nakonfigurované v nadřazené profil hello, pomocí konkrétní koncový bod typu, 'NestedEndpoints'.</span><span class="sxs-lookup"><span data-stu-id="0befb-220">Nested endpoints are configured at hello parent profile, using a specific endpoint type, 'NestedEndpoints'.</span></span> <span data-ttu-id="0befb-221">Při zadávání vnořené koncové body:</span><span class="sxs-lookup"><span data-stu-id="0befb-221">When specifying nested endpoints:</span></span>

* <span data-ttu-id="0befb-222">koncový bod Hello je třeba zadat pomocí parametru 'targetResourceId' hello</span><span class="sxs-lookup"><span data-stu-id="0befb-222">hello endpoint must be specified using hello 'targetResourceId' parameter</span></span>
* <span data-ttu-id="0befb-223">Pokud se používá metoda směrování provozu "Výkonu" hello, je třeba hello 'EndpointLocation'.</span><span class="sxs-lookup"><span data-stu-id="0befb-223">If hello 'Performance' traffic-routing method is used, hello 'EndpointLocation' is required.</span></span> <span data-ttu-id="0befb-224">V opačném případě je volitelný.</span><span class="sxs-lookup"><span data-stu-id="0befb-224">Otherwise it is optional.</span></span> <span data-ttu-id="0befb-225">Hello hodnota musí být [platný oblast Azure název](http://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="0befb-225">hello value must be a [valid Azure region name](http://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="0befb-226">Dobrý den, vážené' a 'prioritu, jsou volitelné, jako koncové body Azure.</span><span class="sxs-lookup"><span data-stu-id="0befb-226">hello 'Weight' and 'Priority' are optional, as for Azure endpoints.</span></span>
* <span data-ttu-id="0befb-227">Parametr 'MinChildEndpoints' Hello je volitelné.</span><span class="sxs-lookup"><span data-stu-id="0befb-227">hello 'MinChildEndpoints' parameter is optional.</span></span> <span data-ttu-id="0befb-228">Hello výchozí hodnota je '1'.</span><span class="sxs-lookup"><span data-stu-id="0befb-228">hello default value is '1'.</span></span> <span data-ttu-id="0befb-229">Pokud hello počet dostupných koncových bodů klesne pod touto prahovou hodnotou, profil nadřazené hello zvažuje hello podřízené profilu 'snížený výkon a přesměruje přenosy toohello dalších koncových bodů v profilu nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="0befb-229">If hello number of available endpoints falls below this threshold, hello parent profile considers hello child profile 'degraded' and diverts traffic toohello other endpoints in hello parent profile.</span></span>

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="0befb-230">Příklad 1: Přidání vnořené koncové body pomocí `Add-AzureRmTrafficManagerEndpointConfig` a`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="0befb-230">Example 1: Adding nested endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="0befb-231">V tomto příkladu jsme vytvoření nové Traffic Manager podřízenými a nadřazenými profilů, přidat podřízené hello jako nadřazená toohello vnořené koncový bod a provést změny hello.</span><span class="sxs-lookup"><span data-stu-id="0befb-231">In this example, we create new Traffic Manager child and parent profiles, add hello child as a nested endpoint toohello parent, and commit hello changes.</span></span>

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="0befb-232">Jako stručný výtah v tomto příkladu jsme nepřidali žádné další koncové body toohello nadřazená nebo podřízená profily.</span><span class="sxs-lookup"><span data-stu-id="0befb-232">For brevity in this example, we did not add any other endpoints toohello child or parent profiles.</span></span>

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="0befb-233">Příklad 2: Přidání vnořené koncové body pomocí`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="0befb-233">Example 2: Adding nested endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="0befb-234">V tomto příkladu přidáme stávající profil podřízené jako existující nadřazené profil tooan vnořené koncový bod.</span><span class="sxs-lookup"><span data-stu-id="0befb-234">In this example, we add an existing child profile as a nested endpoint tooan existing parent profile.</span></span> <span data-ttu-id="0befb-235">profil Hello je zadán pomocí názvy hello profil a prostředků skupiny.</span><span class="sxs-lookup"><span data-stu-id="0befb-235">hello profile is specified using hello profile and resource group names.</span></span>

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a><span data-ttu-id="0befb-236">Aktualizovat koncový bod Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="0befb-236">Update a Traffic Manager Endpoint</span></span>

<span data-ttu-id="0befb-237">Existují dva způsoby tooupdate existující koncový bod Traffic Manager:</span><span class="sxs-lookup"><span data-stu-id="0befb-237">There are two ways tooupdate an existing Traffic Manager endpoint:</span></span>

1. <span data-ttu-id="0befb-238">Získat pomocí profilu Traffic Manageru hello `Get-AzureRmTrafficManagerProfile`, aktualizovat vlastnosti hello koncového bodu v rámci profilu hello a provést změny hello pomocí `Set-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="0befb-238">Get hello Traffic Manager profile using `Get-AzureRmTrafficManagerProfile`, update hello endpoint properties within hello profile, and commit hello changes using `Set-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="0befb-239">Tato metoda má výhod hello je možné tooupdate více než jeden koncový bod v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="0befb-239">This method has hello advantage of being able tooupdate more than one endpoint in a single operation.</span></span>
2. <span data-ttu-id="0befb-240">Získat hello Traffic Manager koncový bod pomocí `Get-AzureRmTrafficManagerEndpoint`, aktualizovat vlastnosti hello koncový bod a provést změny hello pomocí `Set-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="0befb-240">Get hello Traffic Manager endpoint using `Get-AzureRmTrafficManagerEndpoint`, update hello endpoint properties, and commit hello changes using `Set-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="0befb-241">Tato metoda je jednodušší, protože nevyžaduje indexování do pole hello koncové body v profilu hello.</span><span class="sxs-lookup"><span data-stu-id="0befb-241">This method is simpler, since it does not require indexing into hello Endpoints array in hello profile.</span></span>

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="0befb-242">Příklad 1: Aktualizace koncové body pomocí `Get-AzureRmTrafficManagerProfile` a`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="0befb-242">Example 1: Updating endpoints using `Get-AzureRmTrafficManagerProfile` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="0befb-243">V tomto příkladu jsme změnit prioritu hello na dva koncové body v rámci stávající profil.</span><span class="sxs-lookup"><span data-stu-id="0befb-243">In this example, we modify hello priority on two endpoints within an existing profile.</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a><span data-ttu-id="0befb-244">Příklad 2: Aktualizace k koncový bod pomocí `Get-AzureRmTrafficManagerEndpoint` a`Set-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="0befb-244">Example 2: Updating an endpoint using `Get-AzureRmTrafficManagerEndpoint` and `Set-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="0befb-245">V tomto příkladu jsme upravit hello váhu jeden koncový bod v existující profil.</span><span class="sxs-lookup"><span data-stu-id="0befb-245">In this example, we modify hello weight of a single endpoint in an existing profile.</span></span>

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a><span data-ttu-id="0befb-246">Povolování a zakazování koncových bodů a profily</span><span class="sxs-lookup"><span data-stu-id="0befb-246">Enabling and Disabling Endpoints and Profiles</span></span>

<span data-ttu-id="0befb-247">Traffic Manager umožňuje toobe jednotlivé koncové body povolené a zakázané, a také umožňuje zapnutí a vypnutí celý profilů.</span><span class="sxs-lookup"><span data-stu-id="0befb-247">Traffic Manager allows individual endpoints toobe enabled and disabled, as well as allowing enabling and disabling of entire profiles.</span></span>
<span data-ttu-id="0befb-248">Tyto změny provést hello získávání/aktualizace nebo nastavení koncového bodu nebo profil prostředky.</span><span class="sxs-lookup"><span data-stu-id="0befb-248">These changes can be made by getting/updating/setting hello endpoint or profile resources.</span></span> <span data-ttu-id="0befb-249">toostreamline tyto běžné operace jsou podporovány také prostřednictvím vyhrazené rutiny.</span><span class="sxs-lookup"><span data-stu-id="0befb-249">toostreamline these common operations, they are also supported via dedicated cmdlets.</span></span>

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a><span data-ttu-id="0befb-250">Příklad 1: Povolení a zakázání profilu Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="0befb-250">Example 1: Enabling and disabling a Traffic Manager profile</span></span>

<span data-ttu-id="0befb-251">použít tooenable profil Traffic Manageru `Enable-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="0befb-251">tooenable a Traffic Manager profile, use `Enable-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="0befb-252">profil Hello lze zadat pomocí objektu profilu.</span><span class="sxs-lookup"><span data-stu-id="0befb-252">hello profile can be specified using a profile object.</span></span> <span data-ttu-id="0befb-253">Hello objektu profilu se dá předat prostřednictvím kanálu hello nebo pomocí hello '-TrafficManagerProfile' parametr.</span><span class="sxs-lookup"><span data-stu-id="0befb-253">hello profile object can be passed via hello pipeline or by using hello '-TrafficManagerProfile' parameter.</span></span> <span data-ttu-id="0befb-254">V tomto příkladu určíme hello profil podle názvu skupiny hello profilu a prostředků.</span><span class="sxs-lookup"><span data-stu-id="0befb-254">In this example, we specify hello profile by hello profile and resource group name.</span></span>

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="0befb-255">toodisable profil Traffic Manageru:</span><span class="sxs-lookup"><span data-stu-id="0befb-255">toodisable a Traffic Manager profile:</span></span>

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="0befb-256">rutina Disable-AzureRmTrafficManagerProfile Hello vyzve k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="0befb-256">hello Disable-AzureRmTrafficManagerProfile cmdlet prompts for confirmation.</span></span> <span data-ttu-id="0befb-257">Tuto výzvu lze potlačit pomocí hello '-Force' parametr.</span><span class="sxs-lookup"><span data-stu-id="0befb-257">This prompt can be suppressed using hello '-Force' parameter.</span></span>

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a><span data-ttu-id="0befb-258">Příklad 2: Povolení a zakázání koncový bod Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="0befb-258">Example 2: Enabling and disabling a Traffic Manager endpoint</span></span>

<span data-ttu-id="0befb-259">použít tooenable koncový bod Traffic Manager `Enable-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="0befb-259">tooenable a Traffic Manager endpoint, use `Enable-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="0befb-260">Existují dva způsoby toospecify hello koncový bod</span><span class="sxs-lookup"><span data-stu-id="0befb-260">There are two ways toospecify hello endpoint</span></span>

1. <span data-ttu-id="0befb-261">Pomocí objekt TrafficManagerEndpoint předány prostřednictvím kanálu hello nebo pomocí hello '-TrafficManagerEndpoint' parametr</span><span class="sxs-lookup"><span data-stu-id="0befb-261">Using a TrafficManagerEndpoint object passed via hello pipeline or using hello '-TrafficManagerEndpoint' parameter</span></span>
2. <span data-ttu-id="0befb-262">Pomocí hello název koncového bodu, typ koncového bodu, název profilu a název skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="0befb-262">Using hello endpoint name, endpoint type, profile name, and resource group name:</span></span>

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="0befb-263">Podobně toodisable koncový bod Traffic Manager:</span><span class="sxs-lookup"><span data-stu-id="0befb-263">Similarly, toodisable a Traffic Manager endpoint:</span></span>

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

<span data-ttu-id="0befb-264">Stejně jako u `Disable-AzureRmTrafficManagerProfile`, hello `Disable-AzureRmTrafficManagerEndpoint` rutiny vyzve k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="0befb-264">As with `Disable-AzureRmTrafficManagerProfile`, hello `Disable-AzureRmTrafficManagerEndpoint` cmdlet prompts for confirmation.</span></span> <span data-ttu-id="0befb-265">Tuto výzvu lze potlačit pomocí hello '-Force' parametr.</span><span class="sxs-lookup"><span data-stu-id="0befb-265">This prompt can be suppressed using hello '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-endpoint"></a><span data-ttu-id="0befb-266">Odstranit koncový bod Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="0befb-266">Delete a Traffic Manager Endpoint</span></span>

<span data-ttu-id="0befb-267">tooremove jednotlivých koncových bodů, použijte hello `Remove-AzureRmTrafficManagerEndpoint` rutiny:</span><span class="sxs-lookup"><span data-stu-id="0befb-267">tooremove individual endpoints, use hello `Remove-AzureRmTrafficManagerEndpoint` cmdlet:</span></span>

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="0befb-268">Tato rutina vyzve k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="0befb-268">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="0befb-269">Tuto výzvu lze potlačit pomocí hello '-Force' parametr.</span><span class="sxs-lookup"><span data-stu-id="0befb-269">This prompt can be suppressed using hello '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-profile"></a><span data-ttu-id="0befb-270">Odstranit profil správce provozu</span><span class="sxs-lookup"><span data-stu-id="0befb-270">Delete a Traffic Manager Profile</span></span>

<span data-ttu-id="0befb-271">toodelete profil Traffic Manageru použijte hello `Remove-AzureRmTrafficManagerProfile` rutiny zadání názvy hello profil a prostředek skupiny:</span><span class="sxs-lookup"><span data-stu-id="0befb-271">toodelete a Traffic Manager profile, use hello `Remove-AzureRmTrafficManagerProfile` cmdlet, specifying hello profile and resource group names:</span></span>

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

<span data-ttu-id="0befb-272">Tato rutina vyzve k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="0befb-272">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="0befb-273">Tuto výzvu lze potlačit pomocí hello '-Force' parametr.</span><span class="sxs-lookup"><span data-stu-id="0befb-273">This prompt can be suppressed using hello '-Force' parameter.</span></span>

<span data-ttu-id="0befb-274">Hello profil toobe odstranit lze zadat také pomocí objektu profilu:</span><span class="sxs-lookup"><span data-stu-id="0befb-274">hello profile toobe deleted can also be specified using a profile object:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

<span data-ttu-id="0befb-275">Toto pořadí lze také přesměrovat:</span><span class="sxs-lookup"><span data-stu-id="0befb-275">This sequence can also be piped:</span></span>

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a><span data-ttu-id="0befb-276">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0befb-276">Next steps</span></span>

[<span data-ttu-id="0befb-277">Monitorování správce provozu</span><span class="sxs-lookup"><span data-stu-id="0befb-277">Traffic Manager monitoring</span></span>](traffic-manager-monitoring.md)

[<span data-ttu-id="0befb-278">Důležité informace o výkonu nástroje Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="0befb-278">Traffic Manager performance considerations</span></span>](traffic-manager-performance-considerations.md)
