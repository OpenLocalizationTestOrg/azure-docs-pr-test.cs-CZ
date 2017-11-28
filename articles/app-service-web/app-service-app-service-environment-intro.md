---
title: aaaIntroduction tooApp v1 Service Environment
description: "Další informace o funkci hello v1 App Service Environment, která poskytuje jednotek škálování zabezpečené, připojený k virtuální síti, vyhrazené pro spuštění všech aplikací."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 78e6d4f5-da46-4eb5-a632-b5fdc17d2394
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 6e3cd1909b241887b5ec19412b9f7884d870cc3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapp-service-environment-v1"></a><span data-ttu-id="97bfb-103">Úvod tooApp v1 Service Environment</span><span class="sxs-lookup"><span data-stu-id="97bfb-103">Introduction tooApp Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="97bfb-104">Tento článek je o hello App Service Environment v1.</span><span class="sxs-lookup"><span data-stu-id="97bfb-104">This article is about hello App Service Environment v1.</span></span>  <span data-ttu-id="97bfb-105">Není k dispozici novější verze hello služby App Service Environment, je snazší toouse a běží na výkonnější infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="97bfb-105">There is a newer version of hello App Service Environment that is easier  toouse and runs on more powerful infrastructure.</span></span> <span data-ttu-id="97bfb-106">Další informace o nové verzi hello začínat hello toolearn [toohello Úvod App Service Environment](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="97bfb-106">toolearn more about hello new version start with hello [Introduction toohello App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

## <a name="overview"></a><span data-ttu-id="97bfb-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="97bfb-107">Overview</span></span>
<span data-ttu-id="97bfb-108">Služby App Service Environment je [Premium] [ PremiumTier] služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service ve velkém rozsahu, včetně [webové aplikace][WebApps], [Mobile Apps][MobileApps], a [aplikace API][APIApps].</span><span class="sxs-lookup"><span data-stu-id="97bfb-108">An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].</span></span>  

<span data-ttu-id="97bfb-109">Služby App Service Environment jsou ideální pro aplikační procesy vyžadující:</span><span class="sxs-lookup"><span data-stu-id="97bfb-109">App Service Environments are ideal for application workloads requiring:</span></span>

* <span data-ttu-id="97bfb-110">Velmi velký rozsah</span><span class="sxs-lookup"><span data-stu-id="97bfb-110">Very high scale</span></span>
* <span data-ttu-id="97bfb-111">Izolace a bezpečný přístup</span><span class="sxs-lookup"><span data-stu-id="97bfb-111">Isolation and secure network access</span></span>

<span data-ttu-id="97bfb-112">Zákazníci vytvářet více prostředí App Service v rámci jedné oblasti Azure, a také nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="97bfb-112">Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.</span></span>  <span data-ttu-id="97bfb-113">Tím je ideální pro vodorovně škálování vrstvy aplikace bez stavu na podporu vysoké zatížení RPS prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="97bfb-113">This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="97bfb-114">Služby App Service Environment jsou izolované toorunning jenom jednoho zákazníka aplikace a jsou vždy nasazené do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="97bfb-114">App Service Environments are isolated toorunning only a single customer's applications, and are always deployed into a virtual network.</span></span>  <span data-ttu-id="97bfb-115">Zákazníci mají jemně odstupňovanou kontrolu nad obě aplikace příchozí a odchozí síťový provoz a aplikace může vytvořit vysokorychlostní zabezpečené připojení přes virtuální sítě tooon místním firemním prostředkům.</span><span class="sxs-lookup"><span data-stu-id="97bfb-115">Customers have fine-grained control over both inbound and outbound application network traffic, and applications can establish high-speed secure connections over virtual networks tooon-premises corporate resources.</span></span>

<span data-ttu-id="97bfb-116">Všechny články a jak – do této o prostředí App Service jsou k dispozici v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="97bfb-116">All articles and How-To's about App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="97bfb-117">Přehled o tom, jak prostředí App Service povolit velkém rozsahu a zabezpečit přístup k síti najdete v tématu hello [podrobné informace AzureCon] [ AzureConDeepDive] na prostředí App Service!</span><span class="sxs-lookup"><span data-stu-id="97bfb-117">For an overview of how App Service Environments enable high scale and secure network access, see hello [AzureCon Deep Dive][AzureConDeepDive] on App Service Environments!</span></span>

<span data-ttu-id="97bfb-118">Přímý informace na vodorovně škálování pomocí několika prostředí App Service naleznete hello článku na postupy toosetup [geograficky distribuovaná aplikace nároků][GeodistributedAppFootprint].</span><span class="sxs-lookup"><span data-stu-id="97bfb-118">For a deep-dive on horizontally scaling using multiple App Service Environments see hello article on how toosetup a [geo-distributed app footprint][GeodistributedAppFootprint].</span></span>

<span data-ttu-id="97bfb-119">toosee, jak byla nakonfigurována Architektura zabezpečení hello ukazuje hello AzureCon podrobné informace, najdete v článku hello na implementaci [Architektura zabezpečení na základě](app-service-app-service-environment-layered-security.md) s prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="97bfb-119">toosee how hello security architecture shown in hello AzureCon Deep Dive was configured, see hello article on implementing a [layered security architecture](app-service-app-service-environment-layered-security.md) with App Service Environments.</span></span>

<span data-ttu-id="97bfb-120">Aplikace běžící v prostředí App Service může mít svůj přístup závislé na nadřazený zařízení, jako jsou brány firewall systému webové aplikace (firewall webových aplikací).</span><span class="sxs-lookup"><span data-stu-id="97bfb-120">Apps running on App Service Environments can have their access gated by upstream devices such as web application firewalls (WAF).</span></span>  <span data-ttu-id="97bfb-121">článek Hello na [konfigurace firewall webových aplikací pro prostředí App Service](app-service-app-service-environment-web-application-firewall.md) popisuje tento scénář.</span><span class="sxs-lookup"><span data-stu-id="97bfb-121">hello article on [configuring a WAF for App Service Environments](app-service-app-service-environment-web-application-firewall.md) covers this scenario.</span></span> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a><span data-ttu-id="97bfb-122">Vyhrazený výpočetní prostředky</span><span class="sxs-lookup"><span data-stu-id="97bfb-122">Dedicated Compute Resources</span></span>
<span data-ttu-id="97bfb-123">Všechny hello výpočetních prostředků ve službě App Service Environment jsou vyhrazené výhradně tooa jednoho předplatného a služby App Service Environment se dá nakonfigurovat s až toofifty (50) výpočetní prostředky pro výhradní použití jedné aplikace.</span><span class="sxs-lookup"><span data-stu-id="97bfb-123">All of hello compute resources in an App Service Environment are dedicated exclusively tooa single subscription, and an App Service Environment can be configured with up toofifty (50) compute resources for exclusive use by a single application.</span></span>

<span data-ttu-id="97bfb-124">Služby App Service Environment se skládá z front-endu výpočetní fondu zdrojů, jakož i fondy prostředků výpočetního jeden toothree pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="97bfb-124">An App Service Environment is composed of a front-end compute resource pool, as well as one toothree worker compute resource pools.</span></span> 

<span data-ttu-id="97bfb-125">Hello front-end fond obsahuje výpočetní prostředky zodpovědná za ukončení protokolu SSL jako dobře automatické vyvažování zátěže požadavků aplikace v rámci služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="97bfb-125">hello front-end pool contains compute resources responsible for SSL termination as well automatic load balancing of app requests within an App Service Environment.</span></span> 

<span data-ttu-id="97bfb-126">Každý pracovní fond obsahuje výpočetní prostředky přidělené příliš[plány služby App Service][AppServicePlan], který naopak obsahovat jeden nebo více aplikacemi Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="97bfb-126">Each worker pool contains compute resources allocated too[App Service Plans][AppServicePlan], which in turn contain one or more Azure App Service apps.</span></span>  <span data-ttu-id="97bfb-127">Vzhledem k tomu může být až toothree jiný pracovní fondy ve službě App Service Environment, máte hello flexibilitu toochoose různé výpočetní prostředky pro každý fond pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="97bfb-127">Since there can be up toothree different worker pools in an App Service Environment, you have hello flexibility toochoose different compute resources for each worker pool.</span></span>  

<span data-ttu-id="97bfb-128">Například můžete fond toocreate jeden pracovní proces s méně výkonná výpočetní prostředky pro plány služby App Service určený pro vývoj nebo testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="97bfb-128">For example, this allows you toocreate one worker pool with less powerful compute resources for App Service Plans intended for development or test apps.</span></span>  <span data-ttu-id="97bfb-129">Fond pracovních procesů druhý (nebo i třetí) může použít výkonnější výpočetní prostředky, které jsou určené pro plány služby App Service spouští aplikace produkční.</span><span class="sxs-lookup"><span data-stu-id="97bfb-129">A second (or even third) worker pool could use more powerful compute resources intended for App Service Plans running production apps.</span></span>

<span data-ttu-id="97bfb-130">Další informace o objemu hello výpočetní prostředky k dispozici toohello front-endu a fondy pracovních procesů v [jak tooConfigure služby App Service Environment][HowToConfigureanAppServiceEnvironment].</span><span class="sxs-lookup"><span data-stu-id="97bfb-130">For more details on hello quantity of compute resources available toohello front-end and worker pools, see [How tooConfigure an App Service Environment][HowToConfigureanAppServiceEnvironment].</span></span>  

<span data-ttu-id="97bfb-131">Pro informace o hello dostupné výpočetní prostředek velikosti podporované ve službě App Service Environment, poraďte se hello [App Service – ceny] [ AppServicePricing] stránky a zkontrolujte hello k dispozici možnosti pro prostředí App Service v hello cenová úroveň Premium.</span><span class="sxs-lookup"><span data-stu-id="97bfb-131">For details on hello available compute resource sizes supported in an App Service Environment, consult hello [App Service Pricing][AppServicePricing] page and review hello available options for App Service Environments in hello Premium pricing tier.</span></span>

## <a name="virtual-network-support"></a><span data-ttu-id="97bfb-132">Podpora virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="97bfb-132">Virtual Network Support</span></span>
<span data-ttu-id="97bfb-133">Služby App Service Environment se dají vytvářet v **buď** virtuální síť Azure Resource Manager **nebo** virtuální síť modelu nasazení classic ([Další informace o virtuálních sítí] [MoreInfoOnVirtualNetworks]).</span><span class="sxs-lookup"><span data-stu-id="97bfb-133">An App Service Environment can be created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model virtual network ([more info on virtual networks][MoreInfoOnVirtualNetworks]).</span></span>  <span data-ttu-id="97bfb-134">Vzhledem k tomu, že služby App Service Environment vždy existuje ve virtuální síti a přesněji v podsíti virtuální sítě, můžete využívat funkce zabezpečení hello toocontrol virtuální sítě i příchozí a odchozí síťovou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="97bfb-134">Since an App Service Environment always exists in a virtual network, and more precisely within a subnet of a virtual network, you can leverage hello security features of virtual networks toocontrol both inbound and outbound network communications.</span></span>  

<span data-ttu-id="97bfb-135">Služby App Service Environment může být buď internetové s veřejnou IP adresu, nebo interní, kterým čelí jenom adresu Azure vyrovnávání interní zatížení (ILB).</span><span class="sxs-lookup"><span data-stu-id="97bfb-135">An App Service Environment can be either Internet facing with a public IP address, or internal facing with only an Azure Internal Load Balancer (ILB) address.</span></span>

<span data-ttu-id="97bfb-136">Můžete použít [skupin zabezpečení sítě] [ NetworkSecurityGroups] toorestrict příchozí podsíť toohello komunikace sítě, které se nachází služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="97bfb-136">You can use [network security groups][NetworkSecurityGroups] toorestrict inbound network communications toohello subnet where an App Service Environment resides.</span></span>  <span data-ttu-id="97bfb-137">To vám umožní toorun aplikace za nadřazeného zařízení a služeb, jako jsou brány firewall webových aplikací a poskytovatelů SaaS sítě.</span><span class="sxs-lookup"><span data-stu-id="97bfb-137">This allows you toorun apps behind upstream devices and services such as web application firewalls, and network SaaS providers.</span></span>

<span data-ttu-id="97bfb-138">Aplikace také často potřebují tooaccess podnikovým prostředkům, jako je například interní databází a webové služby.</span><span class="sxs-lookup"><span data-stu-id="97bfb-138">Apps also frequently need tooaccess corporate resources such as internal databases and web services.</span></span>  <span data-ttu-id="97bfb-139">Běžný postup je toomake tyto koncové body k dispozici pouze toointernal příchozí síťový provoz v rámci virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="97bfb-139">A common approach is toomake these endpoints available only toointernal network traffic flowing within an Azure virtual network.</span></span>  <span data-ttu-id="97bfb-140">Jakmile služby App Service Environment je připojený k toohello stejné virtuální síti jako hello interních služeb, aplikace běžící v prostředí hello k nim přístup, včetně koncových bodů, které jsou dostupné prostřednictvím [Site-to-Site] [ SiteToSite]a [Azure ExpressRoute] [ ExpressRoute] připojení.</span><span class="sxs-lookup"><span data-stu-id="97bfb-140">Once an App Service Environment is joined toohello same virtual network as hello internal services, apps running in hello environment can access them, including endpoints reachable via [Site-to-Site][SiteToSite] and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

<span data-ttu-id="97bfb-141">Pro další informace o tom, jak prostředí App Service práci s virtuálními sítěmi a místními sítěmi naleznete následující články hello [síťovou architekturu][NetworkArchitectureOverview], [řízení příchozí Provoz][ControllingInboundTraffic], a [bezpečně připojení tooBackends][SecurelyConnectingToBackends].</span><span class="sxs-lookup"><span data-stu-id="97bfb-141">For more details on how App Service Environments work with virtual networks and on-premises networks consult hello following articles on [Network Architecture][NetworkArchitectureOverview], [Controlling Inbound Traffic][ControllingInboundTraffic], and [Securely Connecting tooBackends][SecurelyConnectingToBackends].</span></span> 

## <a name="getting-started"></a><span data-ttu-id="97bfb-142">Začínáme</span><span class="sxs-lookup"><span data-stu-id="97bfb-142">Getting started</span></span>
<span data-ttu-id="97bfb-143">tooget začít s prostředí App Service najdete v části [jak tooCreate App Service Environment][HowToCreateAnAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="97bfb-143">tooget started with App Service Environments, see [How tooCreate An App Service Environment][HowToCreateAnAppServiceEnvironment]</span></span>

<span data-ttu-id="97bfb-144">Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="97bfb-144">All articles and How-To's for App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="97bfb-145">Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="97bfb-145">For more information about hello Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

<span data-ttu-id="97bfb-146">Přehled hello App Service Environment síťovou architekturu, najdete v části hello [přehled architektury sítě] [ NetworkArchitectureOverview] článku.</span><span class="sxs-lookup"><span data-stu-id="97bfb-146">For an overview of hello App Service Environment network architecture, see hello [Network Architecture Overview][NetworkArchitectureOverview] article.</span></span>

<span data-ttu-id="97bfb-147">Podrobnosti o používání služby App Service Environment se službou ExpressRoute, naleznete v následujícím článku hello [Express Route a prostředí App Service][NetworkConfigDetailsForExpressRoute].</span><span class="sxs-lookup"><span data-stu-id="97bfb-147">For details on using an App Service Environment with ExpressRoute, see hello following article on [Express Route and App Service Environments][NetworkConfigDetailsForExpressRoute].</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->


