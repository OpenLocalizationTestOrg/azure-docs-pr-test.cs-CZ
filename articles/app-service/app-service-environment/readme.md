---
title: soubor readme aaaAzure App Service Environment
description: "Seznamy hello dokumentace, která popisuje Azure App Service Environment"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 77452413-5193-4762-8b3d-5fa8e4edf1ca
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 6edc74804ded7497e70c31c9e08252257add4415
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-documentation"></a><span data-ttu-id="3adcd-103">Dokumentace prostředí služby App Service</span><span class="sxs-lookup"><span data-stu-id="3adcd-103">App Service environment documentation</span></span>
 <span data-ttu-id="3adcd-104">Azure App Service Environment je funkce služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací App Service ve velkém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="3adcd-104">Azure App Service Environment is an Azure App Service feature that provides a fully isolated and dedicated environment for securely running App Service apps at high scale.</span></span> <span data-ttu-id="3adcd-105">Tato funkce může hostovat vaše [webové aplikace][webapps], [mobilní aplikace][mobileapps], [aplikace API][APIApps], a [funkce][Functions].</span><span class="sxs-lookup"><span data-stu-id="3adcd-105">This capability can host your [web apps][webapps], [mobile apps][mobileapps], [API apps][APIApps], and [functions][Functions].</span></span>

<span data-ttu-id="3adcd-106">Služby App Service Environment (ASEs) jsou ideální pro aplikační procesy, které vyžadují:</span><span class="sxs-lookup"><span data-stu-id="3adcd-106">App Service environments (ASEs) are ideal for application workloads that require:</span></span>

* <span data-ttu-id="3adcd-107">Velmi velký rozsah.</span><span class="sxs-lookup"><span data-stu-id="3adcd-107">Very high scale.</span></span>
* <span data-ttu-id="3adcd-108">Izolace a bezpečný přístup.</span><span class="sxs-lookup"><span data-stu-id="3adcd-108">Isolation and secure network access.</span></span>

<span data-ttu-id="3adcd-109">Zákazníci vytvářet více ASEs v rámci jedné oblasti Azure a nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="3adcd-109">Customers can create multiple ASEs within a single Azure region and across multiple Azure regions.</span></span> <span data-ttu-id="3adcd-110">Díky této univerzálnost je ASEs ideální pro bezstavové aplikačními vrstvami podporu vysoké zatížení RPS vodorovně škálování.</span><span class="sxs-lookup"><span data-stu-id="3adcd-110">This versatility makes ASEs ideal for horizontally scaling stateless application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="3adcd-111">ASEs jsou izolované toorunning jenom jednoho zákazníka aplikace a jsou vždy nasazené do virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="3adcd-111">ASEs are isolated toorunning only a single customer's applications and are always deployed into an Azure virtual network.</span></span> <span data-ttu-id="3adcd-112">Zákazníci mají jemně odstupňovanou kontrolu nad aplikace příchozí a odchozí síťový provoz s použitím [skupin zabezpečení sítě][NSGs].</span><span class="sxs-lookup"><span data-stu-id="3adcd-112">Customers have fine-grained control over inbound and outbound application network traffic by using [Network Security Groups][NSGs].</span></span> <span data-ttu-id="3adcd-113">Aplikace můžete také vytvořit vysokorychlostní zabezpečené připojení přes virtuální sítě tooon místním firemním prostředkům.</span><span class="sxs-lookup"><span data-stu-id="3adcd-113">Applications can also establish high-speed secure connections over virtual networks tooon-premises corporate resources.</span></span>

<span data-ttu-id="3adcd-114">Aplikace často potřebují tooaccess podnikovým prostředkům, jako je například interní databází a webové služby.</span><span class="sxs-lookup"><span data-stu-id="3adcd-114">Apps frequently need tooaccess corporate resources, such as internal databases and web services.</span></span> <span data-ttu-id="3adcd-115">Aplikace, které běží na ASEs mají přístup k prostředkům prostřednictvím [site-to-site] [ SiteToSite] VPN a [Azure ExpressRoute] [ ExpressRoute] připojení.</span><span class="sxs-lookup"><span data-stu-id="3adcd-115">Apps that run on ASEs can access resources via [site-to-site][SiteToSite] VPN and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

* <span data-ttu-id="3adcd-116">[Co je služba App Service environment?][Intro]</span><span class="sxs-lookup"><span data-stu-id="3adcd-116">[What is an App Service environment?][Intro]</span></span>
* <span data-ttu-id="3adcd-117">[Vytvoření služby App Service environment][MakeExternalASE]</span><span class="sxs-lookup"><span data-stu-id="3adcd-117">[Create an App Service environment][MakeExternalASE]</span></span>
* <span data-ttu-id="3adcd-118">[Vytvoření interní zátěže služby App Service environment][MakeILBASE]</span><span class="sxs-lookup"><span data-stu-id="3adcd-118">[Create an internal load balancer App Service environment][MakeILBASE]</span></span>
* <span data-ttu-id="3adcd-119">[Použití služby App Service environment][UsingASE]</span><span class="sxs-lookup"><span data-stu-id="3adcd-119">[Use an App Service environment][UsingASE]</span></span>
* <span data-ttu-id="3adcd-120">[Aspekty sítě a hello služby App Service environment][ASENetwork]</span><span class="sxs-lookup"><span data-stu-id="3adcd-120">[Networking considerations and hello App Service environment][ASENetwork]</span></span>
* <span data-ttu-id="3adcd-121">[Vytvoření služby App Service environment ze šablony][MakeASEfromTemplate]</span><span class="sxs-lookup"><span data-stu-id="3adcd-121">[Create an App Service environment from a template][MakeASEfromTemplate]</span></span>


## <a name="videos"></a><span data-ttu-id="3adcd-122">Videa</span><span class="sxs-lookup"><span data-stu-id="3adcd-122">Videos</span></span>
<span data-ttu-id="3adcd-123">Hlavní moderních PaaS pro hello Enterprise službou Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3adcd-123">Master Modern PaaS for hello Enterprise with Azure App Service</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

<span data-ttu-id="3adcd-124">Nasazení vysoce škálovatelných a zabezpečených aplikací</span><span class="sxs-lookup"><span data-stu-id="3adcd-124">Deploying Highly Scalable and Secure Apps</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

<span data-ttu-id="3adcd-125">Spuštění podnikových webových a mobilních aplikací v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3adcd-125">Running Enterprise Web and Mobile Apps on Azure App Service</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]

## <a name="app-service-environment-v1"></a><span data-ttu-id="3adcd-126">App Service Environment v1</span><span class="sxs-lookup"><span data-stu-id="3adcd-126">App Service Environment v1</span></span> ##
<span data-ttu-id="3adcd-127">Existují dvě verze App Service Environment: ASEv1 a ASEv2.</span><span class="sxs-lookup"><span data-stu-id="3adcd-127">There are two versions of App Service Environment: ASEv1 and ASEv2.</span></span> <span data-ttu-id="3adcd-128">Informace o ASEv1 najdete v tématu [App Service Environment v1 dokumentaci][ASEv1README].</span><span class="sxs-lookup"><span data-stu-id="3adcd-128">For information on ASEv1, see [App Service Environment v1 documentation][ASEv1README].</span></span>


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[ASEv1README]: ../app-service-app-service-environments-readme.md
[SiteToSite]: ../../vpn-gateway/vpn-gateway-site-to-site-create.md
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
