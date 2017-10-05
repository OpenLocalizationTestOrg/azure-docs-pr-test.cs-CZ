---
title: "Služba App Service Environment | Microsoft Docs"
description: "Co je Azure App Service Environment? Úvod do služby App Service Environment."
keywords: "Služba Azure app service environment, virtuální sítě, zabezpečení sítí"
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 1db5c057-3c56-4537-b580-cdd21fe3f3a7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: stefsch
ms.openlocfilehash: 1de3f2c513f4f5ce6fd2ab2b57514122955a9a98
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-environment-documentation"></a><span data-ttu-id="29135-105">Dokumentace prostředí služby App Service</span><span class="sxs-lookup"><span data-stu-id="29135-105">App Service Environment Documentation</span></span>
<span data-ttu-id="29135-106">Služby App Service Environment je [Premium] [ PremiumTier] služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service ve velkém rozsahu, včetně [webové aplikace][WebApps], [Mobile Apps][MobileApps], a [aplikace API][APIApps].</span><span class="sxs-lookup"><span data-stu-id="29135-106">An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].</span></span>  

<span data-ttu-id="29135-107">Služby App Service Environment jsou ideální pro aplikační procesy vyžadující:</span><span class="sxs-lookup"><span data-stu-id="29135-107">App Service Environments are ideal for application workloads requiring:</span></span>

* <span data-ttu-id="29135-108">Velmi velký rozsah</span><span class="sxs-lookup"><span data-stu-id="29135-108">Very high scale</span></span>
* <span data-ttu-id="29135-109">Izolace a bezpečný přístup</span><span class="sxs-lookup"><span data-stu-id="29135-109">Isolation and secure network access</span></span>

<span data-ttu-id="29135-110">Zákazníci vytvářet více prostředí App Service v rámci jedné oblasti Azure, a také nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="29135-110">Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.</span></span>  <span data-ttu-id="29135-111">Tím je ideální pro vodorovně škálování vrstvy aplikace bez stavu na podporu vysoké zatížení RPS prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="29135-111">This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="29135-112">Služby App Service Environment jsou izolované spouštění jenom jednoho zákazníka aplikací a vždy nasazených do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="29135-112">App Service Environments are isolated to running only a single customer's applications, and are always deployed into a virtual network.</span></span>  <span data-ttu-id="29135-113">Zákazníci mají jemně odstupňovanou kontrolu nad obě aplikace příchozí a odchozí síťový provoz pomocí [skupin zabezpečení sítě][NetworkSecurityGroups].</span><span class="sxs-lookup"><span data-stu-id="29135-113">Customers have fine-grained control over both inbound and outbound application network traffic using [network security groups][NetworkSecurityGroups].</span></span>  <span data-ttu-id="29135-114">Aplikace můžete také vytvořit vysokorychlostní zabezpečené připojení přes virtuální sítě k firemním prostředkům místně.</span><span class="sxs-lookup"><span data-stu-id="29135-114">Applications can also establish high-speed secure connections over virtual networks to on-premises corporate resources.</span></span>

<span data-ttu-id="29135-115">Aplikace často potřebují přístup k podnikovým prostředkům, jako jsou třeba interní databáze a webové služby.</span><span class="sxs-lookup"><span data-stu-id="29135-115">Apps frequently need to access corporate resources such as internal databases and web services.</span></span>  <span data-ttu-id="29135-116">Aplikace běžící v prostředí App Service mohou přistupovat k prostředkům, které jsou dostupné prostřednictvím [Site-to-Site] [ SiteToSite] VPN a [Azure ExpressRoute] [ ExpressRoute] připojení.</span><span class="sxs-lookup"><span data-stu-id="29135-116">Apps running on App Service Environments can access resources reachable via [Site-to-Site][SiteToSite] VPN and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

* [<span data-ttu-id="29135-117">Co je služba App Service Environment?</span><span class="sxs-lookup"><span data-stu-id="29135-117">What is an App Service Environment?</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
* [<span data-ttu-id="29135-118">Vytvoření služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="29135-118">Creating an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="29135-119">Vytvoření aplikací ve službě App Service Environment</span><span class="sxs-lookup"><span data-stu-id="29135-119">Creating Apps in an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [<span data-ttu-id="29135-120">Vytváření a používání vyrovnávání zatížení interní s služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="29135-120">Creating and Using an Internal Load Balancer with App Service Environments</span></span>](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [<span data-ttu-id="29135-121">Konfigurace služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="29135-121">Configuring an App Service Environment</span></span>](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [<span data-ttu-id="29135-122">Škálování aplikací ve službě App Service Environment</span><span class="sxs-lookup"><span data-stu-id="29135-122">Scaling Apps in an App Service Environment</span></span>](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [<span data-ttu-id="29135-123">Architektura a zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="29135-123">Network Security and Architecture</span></span>](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a><span data-ttu-id="29135-124">Postup pro</span><span class="sxs-lookup"><span data-stu-id="29135-124">How To's</span></span>
[!INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]

## <a name="videos"></a><span data-ttu-id="29135-125">Videa</span><span class="sxs-lookup"><span data-stu-id="29135-125">Videos</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]



<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
