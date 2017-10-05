---
title: Soubor readme Azure App Service Environment
description: "Jsou uvedeny v dokumentaci, která popisuje Azure App Service Environment"
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
ms.openlocfilehash: 5b1362854dbc3b0098718bd2ea3cffb06366000c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="app-service-environment-documentation"></a>Dokumentace prostředí služby App Service
 Azure App Service Environment je funkce služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací App Service ve velkém rozsahu. Tato funkce může hostovat vaše [webové aplikace][webapps], [mobilní aplikace][mobileapps], [aplikace API][APIApps], a [funkce][Functions].

Služby App Service Environment (ASEs) jsou ideální pro aplikační procesy, které vyžadují:

* Velmi velký rozsah.
* Izolace a bezpečný přístup.

Zákazníci vytvářet více ASEs v rámci jedné oblasti Azure a nad několika oblastmi Azure. Díky této univerzálnost je ASEs ideální pro bezstavové aplikačními vrstvami podporu vysoké zatížení RPS vodorovně škálování.

ASEs izolují spouštění jenom jednoho zákazníka aplikací a vždy nasazených do virtuální sítě Azure. Zákazníci mají jemně odstupňovanou kontrolu nad aplikace příchozí a odchozí síťový provoz s použitím [skupin zabezpečení sítě][NSGs]. Aplikace můžete také vytvořit vysokorychlostní zabezpečené připojení přes virtuální sítě k firemním prostředkům místně.

Aplikace často potřebují přístup k podnikovým prostředkům, jako jsou třeba interní databáze a webové služby. Aplikace, které běží na ASEs mají přístup k prostředkům prostřednictvím [site-to-site] [ SiteToSite] VPN a [Azure ExpressRoute] [ ExpressRoute] připojení.

* [Co je služba App Service environment?][Intro]
* [Vytvoření služby App Service environment][MakeExternalASE]
* [Vytvoření interní zátěže služby App Service environment][MakeILBASE]
* [Použití služby App Service environment][UsingASE]
* [Aspekty sítě a služby App Service environment][ASENetwork]
* [Vytvoření služby App Service environment ze šablony][MakeASEfromTemplate]


## <a name="videos"></a>Videa
Zvládnutí moderních funkcí PaaS ve firmě s využitím Azure App Service
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

Nasazení vysoce škálovatelných a zabezpečených aplikací
>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

Spuštění podnikových webových a mobilních aplikací v Azure App Service
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##
Existují dvě verze App Service Environment: ASEv1 a ASEv2. Informace o ASEv1 najdete v tématu [App Service Environment v1 dokumentaci][ASEv1README].


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
