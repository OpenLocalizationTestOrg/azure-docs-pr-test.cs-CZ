---
title: "Použití Powershellu ke správě Traffic Manageru v Azure | Microsoft Docs"
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
ms.openlocfilehash: 1cd7bd7e32c96398d72c7cd3b51e2b456d60f01d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="using-powershell-to-manage-traffic-manager"></a><span data-ttu-id="cd744-103">Použití Powershellu ke správě Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="cd744-103">Using PowerShell to manage Traffic Manager</span></span>

<span data-ttu-id="cd744-104">Azure Resource Manager je rozhraní pro správu upřednostňované pro služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="cd744-104">Azure Resource Manager is the preferred management interface for services in Azure.</span></span> <span data-ttu-id="cd744-105">Profilů Azure Traffic Manager můžete spravovat pomocí rozhraní API založené na Azure Resource Manager a nástroje.</span><span class="sxs-lookup"><span data-stu-id="cd744-105">Azure Traffic Manager profiles can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="resource-model"></a><span data-ttu-id="cd744-106">Model prostředků</span><span class="sxs-lookup"><span data-stu-id="cd744-106">Resource model</span></span>

<span data-ttu-id="cd744-107">Azure Traffic Manager je konfigurovat pomocí kolekce nastavení nazývá profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="cd744-107">Azure Traffic Manager is configured using a collection of settings called a Traffic Manager profile.</span></span> <span data-ttu-id="cd744-108">Tento profil obsahuje nastavení DNS, nastavení směrování provozu, nastavení monitorování koncového bodu a seznam koncových bodů služby, na které se směruje provoz.</span><span class="sxs-lookup"><span data-stu-id="cd744-108">This profile contains DNS settings, traffic routing settings, endpoint monitoring settings, and a list of service endpoints to which traffic is routed.</span></span>

<span data-ttu-id="cd744-109">Každý profil služby Traffic Manager je reprezentována prostředek typu 'TrafficManagerProfiles'.</span><span class="sxs-lookup"><span data-stu-id="cd744-109">Each Traffic Manager profile is represented by a resource of type 'TrafficManagerProfiles'.</span></span> <span data-ttu-id="cd744-110">Na úrovni rozhraní API REST identifikátor URI pro každý profil je následující:</span><span class="sxs-lookup"><span data-stu-id="cd744-110">At the REST API level, the URI for each profile is as follows:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a><span data-ttu-id="cd744-111">Nastavení prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd744-111">Setting up Azure PowerShell</span></span>

<span data-ttu-id="cd744-112">Tyto pokyny použijte Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cd744-112">These instructions use Microsoft Azure PowerShell.</span></span> <span data-ttu-id="cd744-113">V následujícím článku vysvětluje, jak nainstalovat a nakonfigurovat Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cd744-113">The following article explains how to install and configure Azure PowerShell.</span></span>

* [<span data-ttu-id="cd744-114">Jak nainstalovat a nakonfigurovat Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd744-114">How to install and configure Azure PowerShell</span></span>](/powershell/azure/overview)

<span data-ttu-id="cd744-115">V příkladech v tomto článku předpokládá, že máte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd744-115">The examples in this article assume that you have an existing resource group.</span></span> <span data-ttu-id="cd744-116">Můžete vytvořit skupinu prostředků. pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="cd744-116">You can create a resource group using the following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> <span data-ttu-id="cd744-117">Azure Resource Manager vyžaduje, zda mají všechny skupiny prostředků do umístění.</span><span class="sxs-lookup"><span data-stu-id="cd744-117">Azure Resource Manager requires that all resource groups have a location.</span></span> <span data-ttu-id="cd744-118">Toto umístění se používá jako výchozí pro prostředky vytvořené v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd744-118">This location is used as the default for resources created in that resource group.</span></span> <span data-ttu-id="cd744-119">Ale vzhledem k tomu, že globální, a ne místní prostředky profil správce provozu se volba umístění skupiny prostředků nemá žádný vliv na Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="cd744-119">However, since Traffic Manager profile resources are global, not regional, the choice of resource group location has no impact on Azure Traffic Manager.</span></span>

## <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="cd744-120">Vytvoření profilu Správce provozu</span><span class="sxs-lookup"><span data-stu-id="cd744-120">Create a Traffic Manager Profile</span></span>

<span data-ttu-id="cd744-121">Chcete-li vytvořit profil Traffic Manageru, použijte `New-AzureRmTrafficManagerProfile` rutiny:</span><span class="sxs-lookup"><span data-stu-id="cd744-121">To create a Traffic Manager profile, use the `New-AzureRmTrafficManagerProfile` cmdlet:</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

<span data-ttu-id="cd744-122">Následující tabulka popisuje parametry:</span><span class="sxs-lookup"><span data-stu-id="cd744-122">The following table describes the parameters:</span></span>

| <span data-ttu-id="cd744-123">Parametr</span><span class="sxs-lookup"><span data-stu-id="cd744-123">Parameter</span></span> | <span data-ttu-id="cd744-124">Popis</span><span class="sxs-lookup"><span data-stu-id="cd744-124">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cd744-125">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="cd744-125">Name</span></span> |<span data-ttu-id="cd744-126">Název prostředků pro prostředek profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="cd744-126">The resource name for the Traffic Manager profile resource.</span></span> <span data-ttu-id="cd744-127">Profily ve stejné skupině prostředků musí mít jedinečné názvy.</span><span class="sxs-lookup"><span data-stu-id="cd744-127">Profiles in the same resource group must have unique names.</span></span> <span data-ttu-id="cd744-128">Tento název se liší od názvu DNS pro dotazy DNS.</span><span class="sxs-lookup"><span data-stu-id="cd744-128">This name is separate from the DNS name used for DNS queries.</span></span> |
| <span data-ttu-id="cd744-129">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="cd744-129">ResourceGroupName</span></span> |<span data-ttu-id="cd744-130">Název skupiny prostředků obsahující profil prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd744-130">The name of the resource group containing the profile resource.</span></span> |
| <span data-ttu-id="cd744-131">TrafficRoutingMethod</span><span class="sxs-lookup"><span data-stu-id="cd744-131">TrafficRoutingMethod</span></span> |<span data-ttu-id="cd744-132">Určuje metodu směrování provozu umožňuje určit, kterému koncovému bodu je vrácený v odpovědi dotaz DNS.</span><span class="sxs-lookup"><span data-stu-id="cd744-132">Specifies the traffic-routing method used to determine which endpoint is returned in response a DNS query.</span></span> <span data-ttu-id="cd744-133">Možné hodnoty jsou 'Výkonu', "Vážená" nebo "Priority".</span><span class="sxs-lookup"><span data-stu-id="cd744-133">Possible values are 'Performance', 'Weighted' or 'Priority'.</span></span> |
| <span data-ttu-id="cd744-134">RelativeDnsName</span><span class="sxs-lookup"><span data-stu-id="cd744-134">RelativeDnsName</span></span> |<span data-ttu-id="cd744-135">Určuje název hostitele část názvu DNS, který poskytuje tento profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="cd744-135">Specifies the hostname portion of the DNS name provided by this Traffic Manager profile.</span></span> <span data-ttu-id="cd744-136">Tato hodnota je v kombinaci s názvem domény DNS, který se používá k vytvoření plně kvalifikovaný název domény (FQDN) profilu Azure Traffic Managerem.</span><span class="sxs-lookup"><span data-stu-id="cd744-136">This value is combined with the DNS domain name used by Azure Traffic Manager to form the fully qualified domain name (FQDN) of the profile.</span></span> <span data-ttu-id="cd744-137">Například nastavení hodnoty "contoso" bude "contoso.trafficmanager.net."</span><span class="sxs-lookup"><span data-stu-id="cd744-137">For example, setting the value of 'contoso' becomes 'contoso.trafficmanager.net.'</span></span> |
| <span data-ttu-id="cd744-138">HODNOTA TTL</span><span class="sxs-lookup"><span data-stu-id="cd744-138">TTL</span></span> |<span data-ttu-id="cd744-139">DNS Time-to-Live (TTL), určuje v sekundách.</span><span class="sxs-lookup"><span data-stu-id="cd744-139">Specifies the DNS Time-to-Live (TTL), in seconds.</span></span> <span data-ttu-id="cd744-140">Tato hodnota TTL informuje překladače místní DNS a klienty DNS, jak dlouho chcete mezipaměť odpovědí DNS pro tento profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="cd744-140">This TTL informs the Local DNS resolvers and DNS clients how long to cache DNS responses for this Traffic Manager profile.</span></span> |
| <span data-ttu-id="cd744-141">MonitorProtocol</span><span class="sxs-lookup"><span data-stu-id="cd744-141">MonitorProtocol</span></span> |<span data-ttu-id="cd744-142">Určuje protokol, který se použít k monitorování stavu koncový bod.</span><span class="sxs-lookup"><span data-stu-id="cd744-142">Specifies the protocol to use to monitor endpoint health.</span></span> <span data-ttu-id="cd744-143">Možné hodnoty jsou "HTTP" a "HTTPS".</span><span class="sxs-lookup"><span data-stu-id="cd744-143">Possible values are 'HTTP' and 'HTTPS'.</span></span> |
| <span data-ttu-id="cd744-144">MonitorPort</span><span class="sxs-lookup"><span data-stu-id="cd744-144">MonitorPort</span></span> |<span data-ttu-id="cd744-145">Určuje port TCP používá k monitorování stavu koncový bod.</span><span class="sxs-lookup"><span data-stu-id="cd744-145">Specifies the TCP port used to monitor endpoint health.</span></span> |
| <span data-ttu-id="cd744-146">MonitorPath</span><span class="sxs-lookup"><span data-stu-id="cd744-146">MonitorPath</span></span> |<span data-ttu-id="cd744-147">Určuje cestu k názvu domény koncový bod, který je použít k testu pro koncový bod stavu.</span><span class="sxs-lookup"><span data-stu-id="cd744-147">Specifies the path relative to the endpoint domain name used to probe for endpoint health.</span></span> |

<span data-ttu-id="cd744-148">Rutina vytvoří profil Traffic Manageru v Azure a vrátí objekt odpovídající profil prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cd744-148">The cmdlet creates a Traffic Manager profile in Azure and returns a corresponding profile object to PowerShell.</span></span> <span data-ttu-id="cd744-149">V tomto okamžiku profil neobsahuje žádné koncové body.</span><span class="sxs-lookup"><span data-stu-id="cd744-149">At this point, the profile does not contain any endpoints.</span></span> <span data-ttu-id="cd744-150">Další informace o přidávání koncové body profilu Správce provozu najdete v tématu [přidání koncové body Traffic Manager](#adding-traffic-manager-endpoints).</span><span class="sxs-lookup"><span data-stu-id="cd744-150">For more information about adding endpoints to a Traffic Manager profile, see [Adding Traffic Manager Endpoints](#adding-traffic-manager-endpoints).</span></span>

## <a name="get-a-traffic-manager-profile"></a><span data-ttu-id="cd744-151">Získat profil správce provozu</span><span class="sxs-lookup"><span data-stu-id="cd744-151">Get a Traffic Manager Profile</span></span>

<span data-ttu-id="cd744-152">Chcete-li načíst objekt existující profil Traffic Manageru, použijte `Get-AzureRmTrafficManagerProfle` rutiny:</span><span class="sxs-lookup"><span data-stu-id="cd744-152">To retrieve an existing Traffic Manager profile object, use the `Get-AzureRmTrafficManagerProfle` cmdlet:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="cd744-153">Tato rutina vrátí objekt profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="cd744-153">This cmdlet returns a Traffic Manager profile object.</span></span>

## <a name="update-a-traffic-manager-profile"></a><span data-ttu-id="cd744-154">Aktualizovat profil správce provozu</span><span class="sxs-lookup"><span data-stu-id="cd744-154">Update a Traffic Manager Profile</span></span>

<span data-ttu-id="cd744-155">Úprava profily Traffic Manager zahrnuje proces krok 3:</span><span class="sxs-lookup"><span data-stu-id="cd744-155">Modifying Traffic Manager profiles follows a 3-step process:</span></span>

1. <span data-ttu-id="cd744-156">Načíst profil pomocí `Get-AzureRmTrafficManagerProfile` nebo použijte profil vrácený `New-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="cd744-156">Retrieve the profile using `Get-AzureRmTrafficManagerProfile` or use the profile returned by `New-AzureRmTrafficManagerProfile`.</span></span>
2. <span data-ttu-id="cd744-157">Upravte profil.</span><span class="sxs-lookup"><span data-stu-id="cd744-157">Modify the profile.</span></span> <span data-ttu-id="cd744-158">Můžete přidat a odebrat koncových bodů nebo změňte parametry koncového bodu nebo profilu.</span><span class="sxs-lookup"><span data-stu-id="cd744-158">You can add and remove endpoints or change endpoint or profile parameters.</span></span> <span data-ttu-id="cd744-159">Tyto změny jsou offline operace.</span><span class="sxs-lookup"><span data-stu-id="cd744-159">These changes are off-line operations.</span></span> <span data-ttu-id="cd744-160">Měníte jenom místní objekt v paměti, která představuje profil.</span><span class="sxs-lookup"><span data-stu-id="cd744-160">You are only changing the local object in memory that represents the profile.</span></span>
3. <span data-ttu-id="cd744-161">Potvrdit změny pomocí `Set-AzureRmTrafficManagerProfile` rutiny.</span><span class="sxs-lookup"><span data-stu-id="cd744-161">Commit your changes using the `Set-AzureRmTrafficManagerProfile` cmdlet.</span></span>

<span data-ttu-id="cd744-162">S výjimkou RelativeDnsName profilu lze změnit všechny vlastnosti profilu.</span><span class="sxs-lookup"><span data-stu-id="cd744-162">All profile properties can be changed except the profile's RelativeDnsName.</span></span> <span data-ttu-id="cd744-163">Chcete-li změnit RelativeDnsName, je nutné odstranit profil a nový profil s novým názvem.</span><span class="sxs-lookup"><span data-stu-id="cd744-163">To change the RelativeDnsName, you must delete profile and a new profile with a new name.</span></span>

<span data-ttu-id="cd744-164">Následující příklad ukazuje, jak změnit profilu TTL:</span><span class="sxs-lookup"><span data-stu-id="cd744-164">The following example demonstrates how to change the profile's TTL:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="cd744-165">Existují tři typy koncových bodů Traffic Manager:</span><span class="sxs-lookup"><span data-stu-id="cd744-165">There are three types of Traffic Manager endpoints:</span></span>

1. <span data-ttu-id="cd744-166">**Koncové body Azure** jsou služby hostované v Azure</span><span class="sxs-lookup"><span data-stu-id="cd744-166">**Azure endpoints** are services hosted in Azure</span></span>
2. <span data-ttu-id="cd744-167">**Externí koncové body** jsou služby hostované mimo Azure</span><span class="sxs-lookup"><span data-stu-id="cd744-167">**External endpoints** are services hosted outside of Azure</span></span>
3. <span data-ttu-id="cd744-168">**Vnořené koncové body** se používají k vytvoření vnořené hierarchií profily Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="cd744-168">**Nested endpoints** are used to construct nested hierarchies of Traffic Manager profiles.</span></span> <span data-ttu-id="cd744-169">Vnořené koncové body povolit rozšířené konfigurace směrování provozu pro komplexní aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd744-169">Nested endpoints enable advanced traffic-routing configurations for complex applications.</span></span>

<span data-ttu-id="cd744-170">Ve všech případech tři koncové body můžete přidat dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="cd744-170">In all three cases, endpoints can be added in two ways:</span></span>

1. <span data-ttu-id="cd744-171">Pomocí kroku 3 postup popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="cd744-171">Using a 3-step process described previously.</span></span> <span data-ttu-id="cd744-172">Výhodou této metody je, že lze provést několik změn koncového bodu v jednu aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="cd744-172">The advantage of this method is that several endpoint changes can be made in a single update.</span></span>
2. <span data-ttu-id="cd744-173">Pomocí rutiny New-AzureRmTrafficManagerEndpoint.</span><span class="sxs-lookup"><span data-stu-id="cd744-173">Using the New-AzureRmTrafficManagerEndpoint cmdlet.</span></span> <span data-ttu-id="cd744-174">Tato rutina přidá koncový bod do existujícího profilu Správce provozu v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="cd744-174">This cmdlet adds an endpoint to an existing Traffic Manager profile in a single operation.</span></span>

## <a name="adding-azure-endpoints"></a><span data-ttu-id="cd744-175">Přidání koncové body Azure</span><span class="sxs-lookup"><span data-stu-id="cd744-175">Adding Azure Endpoints</span></span>

<span data-ttu-id="cd744-176">Koncové body Azure odkazovat služby hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="cd744-176">Azure endpoints reference services hosted in Azure.</span></span> <span data-ttu-id="cd744-177">Podporuje dva typy koncové body Azure:</span><span class="sxs-lookup"><span data-stu-id="cd744-177">Two types of Azure endpoints are supported:</span></span>

1. <span data-ttu-id="cd744-178">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="cd744-178">Azure Web Apps</span></span>
2. <span data-ttu-id="cd744-179">Azure prostředky PublicIpAddress (které lze připojit k vyrovnávání zatížení nebo virtuální počítač síťový adaptér).</span><span class="sxs-lookup"><span data-stu-id="cd744-179">Azure PublicIpAddress resources (which can be attached to a load-balancer or a virtual machine NIC).</span></span> <span data-ttu-id="cd744-180">PublicIpAddress musí mít název DNS přiřazené k použití v Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="cd744-180">The PublicIpAddress must have a DNS name assigned to be used in Traffic Manager.</span></span>

<span data-ttu-id="cd744-181">V každém případě:</span><span class="sxs-lookup"><span data-stu-id="cd744-181">In each case:</span></span>

* <span data-ttu-id="cd744-182">Služba je zadán pomocí parametru 'targetResourceId' `Add-AzureRmTrafficManagerEndpointConfig` nebo `New-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="cd744-182">The service is specified using the 'targetResourceId' parameter of `Add-AzureRmTrafficManagerEndpointConfig` or `New-AzureRmTrafficManagerEndpoint`.</span></span>
* <span data-ttu-id="cd744-183">"Target" a 'EndpointLocation' jsou implicitní parametrem TargetResourceId.</span><span class="sxs-lookup"><span data-stu-id="cd744-183">The 'Target' and 'EndpointLocation' are implied by the TargetResourceId.</span></span>
* <span data-ttu-id="cd744-184">Zadání 'Weight' je volitelný.</span><span class="sxs-lookup"><span data-stu-id="cd744-184">Specifying the 'Weight' is optional.</span></span> <span data-ttu-id="cd744-185">Váhu používají jenom Pokud je profil je nakonfigurovaný na použití metodu směrování provozu 'Vážená'.</span><span class="sxs-lookup"><span data-stu-id="cd744-185">Weights are only used if the profile is configured to use the 'Weighted' traffic-routing method.</span></span> <span data-ttu-id="cd744-186">Jinak jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="cd744-186">Otherwise, they are ignored.</span></span> <span data-ttu-id="cd744-187">-Li zadána, hodnota musí být číslo v rozsahu od 1 do 1000.</span><span class="sxs-lookup"><span data-stu-id="cd744-187">If specified, the value must be a number between 1 and 1000.</span></span> <span data-ttu-id="cd744-188">Výchozí hodnota je '1'.</span><span class="sxs-lookup"><span data-stu-id="cd744-188">The default value is '1'.</span></span>
* <span data-ttu-id="cd744-189">Zadání "Priority" je volitelný.</span><span class="sxs-lookup"><span data-stu-id="cd744-189">Specifying the 'Priority' is optional.</span></span> <span data-ttu-id="cd744-190">Priority používají jenom Pokud je profil je nakonfigurovaný na použití metodu směrování provozu "Priority".</span><span class="sxs-lookup"><span data-stu-id="cd744-190">Priorities are only used if the profile is configured to use the 'Priority' traffic-routing method.</span></span> <span data-ttu-id="cd744-191">Jinak jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="cd744-191">Otherwise, they are ignored.</span></span> <span data-ttu-id="cd744-192">Platné hodnoty jsou od 1 do 1000 s nižšími hodnotami, která znamená vyšší prioritu.</span><span class="sxs-lookup"><span data-stu-id="cd744-192">Valid values are from 1 to 1000 with lower values indicating a higher priority.</span></span> <span data-ttu-id="cd744-193">Pokud je zadán pro jeden koncový bod, musí být zadán pro všechny koncové body.</span><span class="sxs-lookup"><span data-stu-id="cd744-193">If specified for one endpoint, they must be specified for all endpoints.</span></span> <span data-ttu-id="cd744-194">Pokud tento parametr vynechán, se použijí výchozí hodnoty od '1' v pořadí, jsou uvedeny koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="cd744-194">If omitted, default values starting from '1' are applied in the order that the endpoints are listed.</span></span>

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a><span data-ttu-id="cd744-195">Příklad 1: Přidání koncových bodů webové aplikace pomocí`Add-AzureRmTrafficManagerEndpointConfig`</span><span class="sxs-lookup"><span data-stu-id="cd744-195">Example 1: Adding Web App endpoints using `Add-AzureRmTrafficManagerEndpointConfig`</span></span>

<span data-ttu-id="cd744-196">V tomto příkladu jsme vytvořte profil Traffic Manageru a přidejte dva koncové body webové aplikace pomocí `Add-AzureRmTrafficManagerEndpointConfig` rutiny.</span><span class="sxs-lookup"><span data-stu-id="cd744-196">In this example, we create a Traffic Manager profile and add two Web App endpoints using the `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="cd744-197">Příklad 2: Přidání koncového bodu publicIpAddress pomocí`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="cd744-197">Example 2: Adding a publicIpAddress endpoint using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="cd744-198">V tomto příkladu je prostředek veřejné IP adresy do profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="cd744-198">In this example, a public IP address resource is added to the Traffic Manager profile.</span></span> <span data-ttu-id="cd744-199">Veřejná IP adresa musí mít název DNS nakonfigurovaný a mohou být vázány na síťový adaptér virtuálního počítače nebo ke službě Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="cd744-199">The public IP address must have a DNS name configured, and can be bound either to the NIC of a VM or to a load balancer.</span></span>

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a><span data-ttu-id="cd744-200">Přidání externí koncové body</span><span class="sxs-lookup"><span data-stu-id="cd744-200">Adding External Endpoints</span></span>

<span data-ttu-id="cd744-201">Traffic Manager používá externí koncové body pro přesměrování provozu ke službám hostovaným mimo Azure.</span><span class="sxs-lookup"><span data-stu-id="cd744-201">Traffic Manager uses external endpoints to direct traffic to services hosted outside of Azure.</span></span> <span data-ttu-id="cd744-202">Jak se koncové body Azure externí koncové body přidáním buď pomocí `Add-AzureRmTrafficManagerEndpointConfig` následuje `Set-AzureRmTrafficManagerProfile`, nebo `New-AzureRMTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="cd744-202">As with Azure endpoints, external endpoints can be added either using `Add-AzureRmTrafficManagerEndpointConfig` followed by `Set-AzureRmTrafficManagerProfile`, or `New-AzureRMTrafficManagerEndpoint`.</span></span>

<span data-ttu-id="cd744-203">Při zadání externí koncové body:</span><span class="sxs-lookup"><span data-stu-id="cd744-203">When specifying external endpoints:</span></span>

* <span data-ttu-id="cd744-204">Název domény koncového bodu musí být určen pomocí parametru 'Target'</span><span class="sxs-lookup"><span data-stu-id="cd744-204">The endpoint domain name must be specified using the 'Target' parameter</span></span>
* <span data-ttu-id="cd744-205">Pokud se používá metodu směrování provozu 'Výkonu', 'EndpointLocation' je povinný.</span><span class="sxs-lookup"><span data-stu-id="cd744-205">If the 'Performance' traffic-routing method is used, the 'EndpointLocation' is required.</span></span> <span data-ttu-id="cd744-206">V opačném případě je volitelný.</span><span class="sxs-lookup"><span data-stu-id="cd744-206">Otherwise it is optional.</span></span> <span data-ttu-id="cd744-207">Hodnota musí být [platný oblast Azure název](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="cd744-207">The value must be a [valid Azure region name](https://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="cd744-208">'Weight' a 'prioritu, jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="cd744-208">The 'Weight' and 'Priority' are optional.</span></span>

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="cd744-209">Příklad 1: Přidání externí koncové body pomocí `Add-AzureRmTrafficManagerEndpointConfig` a`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="cd744-209">Example 1: Adding external endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="cd744-210">V tomto příkladu jsme vytvořit profil Traffic Manageru, přidejte dva externí koncové body a potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="cd744-210">In this example, we create a Traffic Manager profile, add two external endpoints, and commit the changes.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="cd744-211">Příklad 2: Přidání externí koncové body pomocí`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="cd744-211">Example 2: Adding external endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="cd744-212">V tomto příkladu přidáme externí koncový bod do existujícího profilu.</span><span class="sxs-lookup"><span data-stu-id="cd744-212">In this example, we add an external endpoint to an existing profile.</span></span> <span data-ttu-id="cd744-213">Profil je zadán pomocí názvy skupin profilu a prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd744-213">The profile is specified using the profile and resource group names.</span></span>

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a><span data-ttu-id="cd744-214">Přidání koncových bodů 'vnořené.</span><span class="sxs-lookup"><span data-stu-id="cd744-214">Adding 'Nested' endpoints</span></span>

<span data-ttu-id="cd744-215">Každý profil služby Traffic Manager určuje jednu metodu směrování provozu.</span><span class="sxs-lookup"><span data-stu-id="cd744-215">Each Traffic Manager profile specifies a single traffic-routing method.</span></span> <span data-ttu-id="cd744-216">Existují však scénáře, které vyžadují složitější směrování provozu, než směrování poskytuje jeden profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="cd744-216">However, there are scenarios that require more sophisticated traffic routing than the routing provided by a single Traffic Manager profile.</span></span> <span data-ttu-id="cd744-217">Lze vnořit profily Traffic Manageru se zkombinovat výhod více než jednu metodu směrování provozu.</span><span class="sxs-lookup"><span data-stu-id="cd744-217">You can nest Traffic Manager profiles to combine the benefits of more than one traffic-routing method.</span></span> <span data-ttu-id="cd744-218">Vnořené profily umožňují přepsat výchozí chování Traffic Manager na podporu větší a složitější nasazení aplikací.</span><span class="sxs-lookup"><span data-stu-id="cd744-218">Nested profiles allow you to override the default Traffic Manager behavior to support larger and more complex application deployments.</span></span> <span data-ttu-id="cd744-219">Podrobné příklady, najdete v části [profily Traffic Manageru vnořené](traffic-manager-nested-profiles.md).</span><span class="sxs-lookup"><span data-stu-id="cd744-219">For more detailed examples, see [Nested Traffic Manager profiles](traffic-manager-nested-profiles.md).</span></span>

<span data-ttu-id="cd744-220">Vnořené koncových bodů jsou nakonfigurované v nadřazené profil, pomocí konkrétní koncový bod typu, 'NestedEndpoints'.</span><span class="sxs-lookup"><span data-stu-id="cd744-220">Nested endpoints are configured at the parent profile, using a specific endpoint type, 'NestedEndpoints'.</span></span> <span data-ttu-id="cd744-221">Při zadávání vnořené koncové body:</span><span class="sxs-lookup"><span data-stu-id="cd744-221">When specifying nested endpoints:</span></span>

* <span data-ttu-id="cd744-222">Koncový bod musí být určen pomocí parametru 'targetResourceId.</span><span class="sxs-lookup"><span data-stu-id="cd744-222">The endpoint must be specified using the 'targetResourceId' parameter</span></span>
* <span data-ttu-id="cd744-223">Pokud se používá metodu směrování provozu 'Výkonu', 'EndpointLocation' je povinný.</span><span class="sxs-lookup"><span data-stu-id="cd744-223">If the 'Performance' traffic-routing method is used, the 'EndpointLocation' is required.</span></span> <span data-ttu-id="cd744-224">V opačném případě je volitelný.</span><span class="sxs-lookup"><span data-stu-id="cd744-224">Otherwise it is optional.</span></span> <span data-ttu-id="cd744-225">Hodnota musí být [platný oblast Azure název](http://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="cd744-225">The value must be a [valid Azure region name](http://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="cd744-226">'Weight' a 'prioritu, jsou volitelné, jako koncové body Azure.</span><span class="sxs-lookup"><span data-stu-id="cd744-226">The 'Weight' and 'Priority' are optional, as for Azure endpoints.</span></span>
* <span data-ttu-id="cd744-227">Parametr 'MinChildEndpoints' je volitelné.</span><span class="sxs-lookup"><span data-stu-id="cd744-227">The 'MinChildEndpoints' parameter is optional.</span></span> <span data-ttu-id="cd744-228">Výchozí hodnota je '1'.</span><span class="sxs-lookup"><span data-stu-id="cd744-228">The default value is '1'.</span></span> <span data-ttu-id="cd744-229">Pokud počet dostupných koncových bodů klesne pod touto prahovou hodnotou, profil nadřazené zvažuje profilu podřízené 'snížený výkon a přesměruje přenosy na dalších koncových bodů v nadřazené profilu.</span><span class="sxs-lookup"><span data-stu-id="cd744-229">If the number of available endpoints falls below this threshold, the parent profile considers the child profile 'degraded' and diverts traffic to the other endpoints in the parent profile.</span></span>

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="cd744-230">Příklad 1: Přidání vnořené koncové body pomocí `Add-AzureRmTrafficManagerEndpointConfig` a`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="cd744-230">Example 1: Adding nested endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="cd744-231">V tomto příkladu jsme vytvoření nové Traffic Manager podřízenými a nadřazenými profilů, přidat podřízené jako koncový bod vnořené do nadřazené a potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="cd744-231">In this example, we create new Traffic Manager child and parent profiles, add the child as a nested endpoint to the parent, and commit the changes.</span></span>

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="cd744-232">Jako stručný výtah v tomto příkladu jsme nepřidali žádné další koncové body nadřazená nebo podřízená profily.</span><span class="sxs-lookup"><span data-stu-id="cd744-232">For brevity in this example, we did not add any other endpoints to the child or parent profiles.</span></span>

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="cd744-233">Příklad 2: Přidání vnořené koncové body pomocí`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="cd744-233">Example 2: Adding nested endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="cd744-234">V tomto příkladu přidáme stávající profil podřízené jako vnořené koncový bod do existujícího profilu nadřazené.</span><span class="sxs-lookup"><span data-stu-id="cd744-234">In this example, we add an existing child profile as a nested endpoint to an existing parent profile.</span></span> <span data-ttu-id="cd744-235">Profil je zadán pomocí názvy skupin profilu a prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd744-235">The profile is specified using the profile and resource group names.</span></span>

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a><span data-ttu-id="cd744-236">Aktualizovat koncový bod Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="cd744-236">Update a Traffic Manager Endpoint</span></span>

<span data-ttu-id="cd744-237">Existují dva způsoby, jak aktualizovat existující koncový bod Traffic Manager:</span><span class="sxs-lookup"><span data-stu-id="cd744-237">There are two ways to update an existing Traffic Manager endpoint:</span></span>

1. <span data-ttu-id="cd744-238">Získat pomocí profilu Traffic Manageru `Get-AzureRmTrafficManagerProfile`, aktualizovat vlastnosti koncového bodu v rámci profilu a provést změny v nástroji `Set-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="cd744-238">Get the Traffic Manager profile using `Get-AzureRmTrafficManagerProfile`, update the endpoint properties within the profile, and commit the changes using `Set-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="cd744-239">Tato metoda má výhodu v podobě bude možné aktualizovat více než jeden koncový bod v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="cd744-239">This method has the advantage of being able to update more than one endpoint in a single operation.</span></span>
2. <span data-ttu-id="cd744-240">Získat pomocí koncového bodu Traffic Manager `Get-AzureRmTrafficManagerEndpoint`, aktualizovat vlastnosti koncový bod a provést změny v nástroji `Set-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="cd744-240">Get the Traffic Manager endpoint using `Get-AzureRmTrafficManagerEndpoint`, update the endpoint properties, and commit the changes using `Set-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="cd744-241">Tato metoda je jednodušší, protože nevyžaduje indexování do pole koncových bodů v profilu.</span><span class="sxs-lookup"><span data-stu-id="cd744-241">This method is simpler, since it does not require indexing into the Endpoints array in the profile.</span></span>

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="cd744-242">Příklad 1: Aktualizace koncové body pomocí `Get-AzureRmTrafficManagerProfile` a`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="cd744-242">Example 1: Updating endpoints using `Get-AzureRmTrafficManagerProfile` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="cd744-243">V tomto příkladu jsme změnit prioritu na dva koncové body v rámci stávající profil.</span><span class="sxs-lookup"><span data-stu-id="cd744-243">In this example, we modify the priority on two endpoints within an existing profile.</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a><span data-ttu-id="cd744-244">Příklad 2: Aktualizace k koncový bod pomocí `Get-AzureRmTrafficManagerEndpoint` a`Set-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="cd744-244">Example 2: Updating an endpoint using `Get-AzureRmTrafficManagerEndpoint` and `Set-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="cd744-245">V tomto příkladu jsme změnit váhu jeden koncový bod v existující profil.</span><span class="sxs-lookup"><span data-stu-id="cd744-245">In this example, we modify the weight of a single endpoint in an existing profile.</span></span>

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a><span data-ttu-id="cd744-246">Povolování a zakazování koncových bodů a profily</span><span class="sxs-lookup"><span data-stu-id="cd744-246">Enabling and Disabling Endpoints and Profiles</span></span>

<span data-ttu-id="cd744-247">Traffic Manager umožňuje jednotlivé koncové body pro povolené a zakázané, a také umožňuje zapnutí a vypnutí celý profilů.</span><span class="sxs-lookup"><span data-stu-id="cd744-247">Traffic Manager allows individual endpoints to be enabled and disabled, as well as allowing enabling and disabling of entire profiles.</span></span>
<span data-ttu-id="cd744-248">Tyto změny provést získávání/aktualizace nebo nastavení koncového bodu nebo profil prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd744-248">These changes can be made by getting/updating/setting the endpoint or profile resources.</span></span> <span data-ttu-id="cd744-249">Zefektivnění těchto běžných operací, podporovány jsou i přes vyhrazené rutiny.</span><span class="sxs-lookup"><span data-stu-id="cd744-249">To streamline these common operations, they are also supported via dedicated cmdlets.</span></span>

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a><span data-ttu-id="cd744-250">Příklad 1: Povolení a zakázání profilu Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="cd744-250">Example 1: Enabling and disabling a Traffic Manager profile</span></span>

<span data-ttu-id="cd744-251">Pokud chcete povolit profil Traffic Manageru, použijte `Enable-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="cd744-251">To enable a Traffic Manager profile, use `Enable-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="cd744-252">Profil lze zadat pomocí objektu profilu.</span><span class="sxs-lookup"><span data-stu-id="cd744-252">The profile can be specified using a profile object.</span></span> <span data-ttu-id="cd744-253">Objekt profilu se dá předat prostřednictvím kanálu nebo pomocí '-TrafficManagerProfile' parametr.</span><span class="sxs-lookup"><span data-stu-id="cd744-253">The profile object can be passed via the pipeline or by using the '-TrafficManagerProfile' parameter.</span></span> <span data-ttu-id="cd744-254">V tomto příkladu určíme profil podle názvu skupiny profilu a prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd744-254">In this example, we specify the profile by the profile and resource group name.</span></span>

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="cd744-255">Chcete-li zakázat profil správce provozu:</span><span class="sxs-lookup"><span data-stu-id="cd744-255">To disable a Traffic Manager profile:</span></span>

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="cd744-256">Rutina Disable-AzureRmTrafficManagerProfile vyzve k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="cd744-256">The Disable-AzureRmTrafficManagerProfile cmdlet prompts for confirmation.</span></span> <span data-ttu-id="cd744-257">Tuto výzvu lze potlačit pomocí '-Force' parametr.</span><span class="sxs-lookup"><span data-stu-id="cd744-257">This prompt can be suppressed using the '-Force' parameter.</span></span>

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a><span data-ttu-id="cd744-258">Příklad 2: Povolení a zakázání koncový bod Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="cd744-258">Example 2: Enabling and disabling a Traffic Manager endpoint</span></span>

<span data-ttu-id="cd744-259">Chcete-li povolit koncový bod Traffic Manager, použijte `Enable-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="cd744-259">To enable a Traffic Manager endpoint, use `Enable-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="cd744-260">Existují dva způsoby, jak zadat koncový bod</span><span class="sxs-lookup"><span data-stu-id="cd744-260">There are two ways to specify the endpoint</span></span>

1. <span data-ttu-id="cd744-261">Pomocí objekt TrafficManagerEndpoint předány prostřednictvím kanálu nebo pomocí '-TrafficManagerEndpoint' parametr</span><span class="sxs-lookup"><span data-stu-id="cd744-261">Using a TrafficManagerEndpoint object passed via the pipeline or using the '-TrafficManagerEndpoint' parameter</span></span>
2. <span data-ttu-id="cd744-262">Pomocí název koncového bodu, typ koncového bodu, název profilu a název skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="cd744-262">Using the endpoint name, endpoint type, profile name, and resource group name:</span></span>

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="cd744-263">Podobně chcete-li zakázat koncový bod Traffic Manager:</span><span class="sxs-lookup"><span data-stu-id="cd744-263">Similarly, to disable a Traffic Manager endpoint:</span></span>

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

<span data-ttu-id="cd744-264">Stejně jako u `Disable-AzureRmTrafficManagerProfile`, `Disable-AzureRmTrafficManagerEndpoint` rutiny vyzve k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="cd744-264">As with `Disable-AzureRmTrafficManagerProfile`, the `Disable-AzureRmTrafficManagerEndpoint` cmdlet prompts for confirmation.</span></span> <span data-ttu-id="cd744-265">Tuto výzvu lze potlačit pomocí '-Force' parametr.</span><span class="sxs-lookup"><span data-stu-id="cd744-265">This prompt can be suppressed using the '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-endpoint"></a><span data-ttu-id="cd744-266">Odstranit koncový bod Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="cd744-266">Delete a Traffic Manager Endpoint</span></span>

<span data-ttu-id="cd744-267">Chcete-li odebrat jednotlivé koncové body, použijte `Remove-AzureRmTrafficManagerEndpoint` rutiny:</span><span class="sxs-lookup"><span data-stu-id="cd744-267">To remove individual endpoints, use the `Remove-AzureRmTrafficManagerEndpoint` cmdlet:</span></span>

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="cd744-268">Tato rutina vyzve k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="cd744-268">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="cd744-269">Tuto výzvu lze potlačit pomocí '-Force' parametr.</span><span class="sxs-lookup"><span data-stu-id="cd744-269">This prompt can be suppressed using the '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-profile"></a><span data-ttu-id="cd744-270">Odstranit profil správce provozu</span><span class="sxs-lookup"><span data-stu-id="cd744-270">Delete a Traffic Manager Profile</span></span>

<span data-ttu-id="cd744-271">Pokud chcete odstranit profil správce provozu, použijte `Remove-AzureRmTrafficManagerProfile` rutiny zadání názvu skupiny profilu a prostředků:</span><span class="sxs-lookup"><span data-stu-id="cd744-271">To delete a Traffic Manager profile, use the `Remove-AzureRmTrafficManagerProfile` cmdlet, specifying the profile and resource group names:</span></span>

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

<span data-ttu-id="cd744-272">Tato rutina vyzve k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="cd744-272">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="cd744-273">Tuto výzvu lze potlačit pomocí '-Force' parametr.</span><span class="sxs-lookup"><span data-stu-id="cd744-273">This prompt can be suppressed using the '-Force' parameter.</span></span>

<span data-ttu-id="cd744-274">Profil pro odstranění je možné zadat také pomocí objektu profilu:</span><span class="sxs-lookup"><span data-stu-id="cd744-274">The profile to be deleted can also be specified using a profile object:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

<span data-ttu-id="cd744-275">Toto pořadí lze také přesměrovat:</span><span class="sxs-lookup"><span data-stu-id="cd744-275">This sequence can also be piped:</span></span>

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a><span data-ttu-id="cd744-276">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd744-276">Next steps</span></span>

[<span data-ttu-id="cd744-277">Monitorování správce provozu</span><span class="sxs-lookup"><span data-stu-id="cd744-277">Traffic Manager monitoring</span></span>](traffic-manager-monitoring.md)

[<span data-ttu-id="cd744-278">Důležité informace o výkonu nástroje Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="cd744-278">Traffic Manager performance considerations</span></span>](traffic-manager-performance-considerations.md)
