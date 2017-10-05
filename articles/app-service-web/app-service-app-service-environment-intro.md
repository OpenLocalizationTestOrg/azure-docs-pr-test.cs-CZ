---
title: "Úvod do aplikace služby prostředí v1"
description: "Další informace o funkci v1 App Service Environment, která poskytuje jednotek škálování zabezpečené, připojený k virtuální síti, vyhrazené pro spuštění všech aplikací."
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
ms.openlocfilehash: 38cb79eb32bd61cdbfb6da91d50e6713d71a2b0d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="introduction-to-app-service-environment-v1"></a><span data-ttu-id="28073-103">Úvod do aplikace služby prostředí v1</span><span class="sxs-lookup"><span data-stu-id="28073-103">Introduction to App Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="28073-104">Tento článek je o v1 App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="28073-104">This article is about the App Service Environment v1.</span></span>  <span data-ttu-id="28073-105">Existuje novější verze App Service Environment, který je jednodušší použít a běží na výkonnější infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="28073-105">There is a newer version of the App Service Environment that is easier  to use and runs on more powerful infrastructure.</span></span> <span data-ttu-id="28073-106">Další informace o nové verzi spuštění s [Úvod do služby App Service Environment](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="28073-106">To learn more about the new version start with the [Introduction to the App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

## <a name="overview"></a><span data-ttu-id="28073-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="28073-107">Overview</span></span>
<span data-ttu-id="28073-108">Služby App Service Environment je [Premium] [ PremiumTier] služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service ve velkém rozsahu, včetně [webové aplikace][WebApps], [Mobile Apps][MobileApps], a [aplikace API][APIApps].</span><span class="sxs-lookup"><span data-stu-id="28073-108">An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].</span></span>  

<span data-ttu-id="28073-109">Služby App Service Environment jsou ideální pro aplikační procesy vyžadující:</span><span class="sxs-lookup"><span data-stu-id="28073-109">App Service Environments are ideal for application workloads requiring:</span></span>

* <span data-ttu-id="28073-110">Velmi velký rozsah</span><span class="sxs-lookup"><span data-stu-id="28073-110">Very high scale</span></span>
* <span data-ttu-id="28073-111">Izolace a bezpečný přístup</span><span class="sxs-lookup"><span data-stu-id="28073-111">Isolation and secure network access</span></span>

<span data-ttu-id="28073-112">Zákazníci vytvářet více prostředí App Service v rámci jedné oblasti Azure, a také nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="28073-112">Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.</span></span>  <span data-ttu-id="28073-113">Tím je ideální pro vodorovně škálování vrstvy aplikace bez stavu na podporu vysoké zatížení RPS prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="28073-113">This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="28073-114">Služby App Service Environment jsou izolované spouštění jenom jednoho zákazníka aplikací a vždy nasazených do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="28073-114">App Service Environments are isolated to running only a single customer's applications, and are always deployed into a virtual network.</span></span>  <span data-ttu-id="28073-115">Zákazníci mají jemně odstupňovanou kontrolu nad obě aplikace příchozí a odchozí síťový provoz a aplikace může vytvořit vysokorychlostní zabezpečené připojení přes virtuální sítě k firemním prostředkům místně.</span><span class="sxs-lookup"><span data-stu-id="28073-115">Customers have fine-grained control over both inbound and outbound application network traffic, and applications can establish high-speed secure connections over virtual networks to on-premises corporate resources.</span></span>

<span data-ttu-id="28073-116">Všechny články a jak – do této o prostředí App Service jsou k dispozici v [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="28073-116">All articles and How-To's about App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="28073-117">Přehled o tom, jak prostředí App Service povolit velkém rozsahu a zabezpečit přístup k síti, najdete v článku [podrobné informace AzureCon] [ AzureConDeepDive] na prostředí App Service!</span><span class="sxs-lookup"><span data-stu-id="28073-117">For an overview of how App Service Environments enable high scale and secure network access, see the [AzureCon Deep Dive][AzureConDeepDive] on App Service Environments!</span></span>

<span data-ttu-id="28073-118">Přímý informace na vodorovně škálování pomocí několika prostředí App Service najdete v článku o tom, jak instalace [geograficky distribuovaná aplikace nároků][GeodistributedAppFootprint].</span><span class="sxs-lookup"><span data-stu-id="28073-118">For a deep-dive on horizontally scaling using multiple App Service Environments see the article on how to setup a [geo-distributed app footprint][GeodistributedAppFootprint].</span></span>

<span data-ttu-id="28073-119">Informace o tom, jak byla nakonfigurována Architektura zabezpečení uvedené v AzureCon podrobné informace, najdete v článku na implementaci [Architektura zabezpečení na základě](app-service-app-service-environment-layered-security.md) s prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="28073-119">To see how the security architecture shown in the AzureCon Deep Dive was configured, see the article on implementing a [layered security architecture](app-service-app-service-environment-layered-security.md) with App Service Environments.</span></span>

<span data-ttu-id="28073-120">Aplikace běžící v prostředí App Service může mít svůj přístup závislé na nadřazený zařízení, jako jsou brány firewall systému webové aplikace (firewall webových aplikací).</span><span class="sxs-lookup"><span data-stu-id="28073-120">Apps running on App Service Environments can have their access gated by upstream devices such as web application firewalls (WAF).</span></span>  <span data-ttu-id="28073-121">V článku na [konfigurace firewall webových aplikací pro prostředí App Service](app-service-app-service-environment-web-application-firewall.md) popisuje tento scénář.</span><span class="sxs-lookup"><span data-stu-id="28073-121">The article on [configuring a WAF for App Service Environments](app-service-app-service-environment-web-application-firewall.md) covers this scenario.</span></span> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a><span data-ttu-id="28073-122">Vyhrazený výpočetní prostředky</span><span class="sxs-lookup"><span data-stu-id="28073-122">Dedicated Compute Resources</span></span>
<span data-ttu-id="28073-123">Všechny výpočetní prostředky ve službě App Service Environment jsou vyhrazené výhradně pro v rámci jednoho předplatného a služby App Service Environment se dá nakonfigurovat s až padesát (50) výpočetní prostředky pro výhradní použití jedné aplikace.</span><span class="sxs-lookup"><span data-stu-id="28073-123">All of the compute resources in an App Service Environment are dedicated exclusively to a single subscription, and an App Service Environment can be configured with up to fifty (50) compute resources for exclusive use by a single application.</span></span>

<span data-ttu-id="28073-124">Služby App Service Environment se skládá z front-endu výpočetní fondu zdrojů, jakož i fondy jednu až tři pracovní výpočetních prostředků.</span><span class="sxs-lookup"><span data-stu-id="28073-124">An App Service Environment is composed of a front-end compute resource pool, as well as one to three worker compute resource pools.</span></span> 

<span data-ttu-id="28073-125">Front-endu fond obsahuje výpočetní prostředky zodpovědná za ukončení protokolu SSL jako dobře automatické vyvažování zátěže požadavků aplikace v rámci služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="28073-125">The front-end pool contains compute resources responsible for SSL termination as well automatic load balancing of app requests within an App Service Environment.</span></span> 

<span data-ttu-id="28073-126">Každý pracovní fond obsahuje výpočetní prostředky přidělené [plány služby App Service][AppServicePlan], který naopak obsahovat jeden nebo více aplikacemi Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="28073-126">Each worker pool contains compute resources allocated to [App Service Plans][AppServicePlan], which in turn contain one or more Azure App Service apps.</span></span>  <span data-ttu-id="28073-127">Vzhledem k tomu, že ve službě App Service Environment může být až tři fondy různých pracovních procesů, máte možnost vybrat různé výpočetní prostředky pro každý fond pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="28073-127">Since there can be up to three different worker pools in an App Service Environment, you have the flexibility to choose different compute resources for each worker pool.</span></span>  

<span data-ttu-id="28073-128">Například tímto způsobem lze vytvořit jeden fond pracovních procesů s méně výkonná výpočetní prostředky pro plány služby App Service určený pro vývoj nebo testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="28073-128">For example, this allows you to create one worker pool with less powerful compute resources for App Service Plans intended for development or test apps.</span></span>  <span data-ttu-id="28073-129">Fond pracovních procesů druhý (nebo i třetí) může použít výkonnější výpočetní prostředky, které jsou určené pro plány služby App Service spouští aplikace produkční.</span><span class="sxs-lookup"><span data-stu-id="28073-129">A second (or even third) worker pool could use more powerful compute resources intended for App Service Plans running production apps.</span></span>

<span data-ttu-id="28073-130">Další informace o objemu výpočetní prostředky, které jsou k dispozici pro fondy front-endu a pracovního procesu, najdete v části [postup konfigurace služby App Service Environment][HowToConfigureanAppServiceEnvironment].</span><span class="sxs-lookup"><span data-stu-id="28073-130">For more details on the quantity of compute resources available to the front-end and worker pools, see [How To Configure an App Service Environment][HowToConfigureanAppServiceEnvironment].</span></span>  

<span data-ttu-id="28073-131">Podrobné informace o dostupných výpočetních prostředků velikosti podporované ve službě App Service Environment [App Service – ceny] [ AppServicePricing] stránky a projděte si dostupné možnosti pro prostředí App Service v Cenová úroveň Premium.</span><span class="sxs-lookup"><span data-stu-id="28073-131">For details on the available compute resource sizes supported in an App Service Environment, consult the [App Service Pricing][AppServicePricing] page and review the available options for App Service Environments in the Premium pricing tier.</span></span>

## <a name="virtual-network-support"></a><span data-ttu-id="28073-132">Podpora virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="28073-132">Virtual Network Support</span></span>
<span data-ttu-id="28073-133">Služby App Service Environment se dají vytvářet v **buď** virtuální síť Azure Resource Manager **nebo** virtuální síť modelu nasazení classic ([Další informace o virtuálních sítí] [MoreInfoOnVirtualNetworks]).</span><span class="sxs-lookup"><span data-stu-id="28073-133">An App Service Environment can be created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model virtual network ([more info on virtual networks][MoreInfoOnVirtualNetworks]).</span></span>  <span data-ttu-id="28073-134">Vzhledem k tomu, že služby App Service Environment vždy existuje ve virtuální síti a přesněji v podsíti virtuální sítě, můžete využít funkce zabezpečení virtuálních sítí k řízení i příchozí a odchozí síťovou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="28073-134">Since an App Service Environment always exists in a virtual network, and more precisely within a subnet of a virtual network, you can leverage the security features of virtual networks to control both inbound and outbound network communications.</span></span>  

<span data-ttu-id="28073-135">Služby App Service Environment může být buď internetové s veřejnou IP adresu, nebo interní, kterým čelí jenom adresu Azure vyrovnávání interní zatížení (ILB).</span><span class="sxs-lookup"><span data-stu-id="28073-135">An App Service Environment can be either Internet facing with a public IP address, or internal facing with only an Azure Internal Load Balancer (ILB) address.</span></span>

<span data-ttu-id="28073-136">Můžete použít [skupin zabezpečení sítě] [ NetworkSecurityGroups] omezit příchozí síťovou komunikaci na podsíť, které se nachází služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="28073-136">You can use [network security groups][NetworkSecurityGroups] to restrict inbound network communications to the subnet where an App Service Environment resides.</span></span>  <span data-ttu-id="28073-137">To umožňuje spouštět aplikace za nadřazeného zařízení a služeb, jako jsou brány firewall webových aplikací a poskytovatelů SaaS sítě.</span><span class="sxs-lookup"><span data-stu-id="28073-137">This allows you to run apps behind upstream devices and services such as web application firewalls, and network SaaS providers.</span></span>

<span data-ttu-id="28073-138">Aplikace také často potřebují přístup k podnikovým prostředkům, jako jsou třeba interní databáze a webové služby.</span><span class="sxs-lookup"><span data-stu-id="28073-138">Apps also frequently need to access corporate resources such as internal databases and web services.</span></span>  <span data-ttu-id="28073-139">Běžně se provést tyto koncové body k dispozici pouze pro interní síťový provoz v rámci virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="28073-139">A common approach is to make these endpoints available only to internal network traffic flowing within an Azure virtual network.</span></span>  <span data-ttu-id="28073-140">Jakmile služby App Service Environment se připojí ke stejné virtuální síti jako interní služby, aplikace běžící v prostředí k nim přístup, včetně koncových bodů, které jsou dostupné prostřednictvím [Site-to-Site] [ SiteToSite] a [Azure ExpressRoute] [ ExpressRoute] připojení.</span><span class="sxs-lookup"><span data-stu-id="28073-140">Once an App Service Environment is joined to the same virtual network as the internal services, apps running in the environment can access them, including endpoints reachable via [Site-to-Site][SiteToSite] and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

<span data-ttu-id="28073-141">Pro další informace o tom, jak prostředí App Service práci s virtuálními sítěmi a místními sítěmi najdete v následujících článcích na [síťovou architekturu][NetworkArchitectureOverview], [řízení příchozí Provoz][ControllingInboundTraffic], a [bezpečně připojování k back-EndY][SecurelyConnectingToBackends].</span><span class="sxs-lookup"><span data-stu-id="28073-141">For more details on how App Service Environments work with virtual networks and on-premises networks consult the following articles on [Network Architecture][NetworkArchitectureOverview], [Controlling Inbound Traffic][ControllingInboundTraffic], and [Securely Connecting to Backends][SecurelyConnectingToBackends].</span></span> 

## <a name="getting-started"></a><span data-ttu-id="28073-142">Začínáme</span><span class="sxs-lookup"><span data-stu-id="28073-142">Getting started</span></span>
<span data-ttu-id="28073-143">Chcete-li začít pracovat s prostředí App Service, přečtěte si téma [jak k vytvoření služby App Service Environment][HowToCreateAnAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="28073-143">To get started with App Service Environments, see [How To Create An App Service Environment][HowToCreateAnAppServiceEnvironment]</span></span>

<span data-ttu-id="28073-144">Všechny články a jak – do této pro App Service Environment jsou dostupné v [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="28073-144">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="28073-145">Další informace o platformě Azure App Service najdete v tématu [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="28073-145">For more information about the Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

<span data-ttu-id="28073-146">Přehled architektury sítě služby App Service Environment, najdete [přehled architektury sítě] [ NetworkArchitectureOverview] článku.</span><span class="sxs-lookup"><span data-stu-id="28073-146">For an overview of the App Service Environment network architecture, see the [Network Architecture Overview][NetworkArchitectureOverview] article.</span></span>

<span data-ttu-id="28073-147">Podrobné informace o používání služby App Service Environment se službou ExpressRoute, najdete v následujícím článku na [Express Route a prostředí App Service][NetworkConfigDetailsForExpressRoute].</span><span class="sxs-lookup"><span data-stu-id="28073-147">For details on using an App Service Environment with ExpressRoute, see the following article on [Express Route and App Service Environments][NetworkConfigDetailsForExpressRoute].</span></span>

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


