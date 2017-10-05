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
# <a name="app-service-environment-documentation"></a>Dokumentace prostředí služby App Service
Služby App Service Environment je [Premium] [ PremiumTier] služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service ve velkém rozsahu, včetně [webové aplikace][WebApps], [Mobile Apps][MobileApps], a [aplikace API][APIApps].  

Služby App Service Environment jsou ideální pro aplikační procesy vyžadující:

* Velmi velký rozsah
* Izolace a bezpečný přístup

Zákazníci vytvářet více prostředí App Service v rámci jedné oblasti Azure, a také nad několika oblastmi Azure.  Tím je ideální pro vodorovně škálování vrstvy aplikace bez stavu na podporu vysoké zatížení RPS prostředí App Service.

Služby App Service Environment jsou izolované spouštění jenom jednoho zákazníka aplikací a vždy nasazených do virtuální sítě.  Zákazníci mají jemně odstupňovanou kontrolu nad obě aplikace příchozí a odchozí síťový provoz pomocí [skupin zabezpečení sítě][NetworkSecurityGroups].  Aplikace můžete také vytvořit vysokorychlostní zabezpečené připojení přes virtuální sítě k firemním prostředkům místně.

Aplikace často potřebují přístup k podnikovým prostředkům, jako jsou třeba interní databáze a webové služby.  Aplikace běžící v prostředí App Service mohou přistupovat k prostředkům, které jsou dostupné prostřednictvím [Site-to-Site] [ SiteToSite] VPN a [Azure ExpressRoute] [ ExpressRoute] připojení.

* [Co je služba App Service Environment?](../app-service-web/app-service-app-service-environment-intro.md)
* [Vytvoření služby App Service Environment](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Vytvoření aplikací ve službě App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Vytváření a používání vyrovnávání zatížení interní s služby App Service Environment](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Konfigurace služby App Service Environment](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Škálování aplikací ve službě App Service Environment](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Architektura a zabezpečení sítě](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Postup pro
[!INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]

## <a name="videos"></a>Videa
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
