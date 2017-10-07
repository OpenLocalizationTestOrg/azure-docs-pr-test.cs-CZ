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
# <a name="introduction-tooapp-service-environment-v1"></a>Úvod tooApp v1 Service Environment

> [!NOTE]
> Tento článek je o hello App Service Environment v1.  Není k dispozici novější verze hello služby App Service Environment, je snazší toouse a běží na výkonnější infrastruktury. Další informace o nové verzi hello začínat hello toolearn [toohello Úvod App Service Environment](../app-service/app-service-environment/intro.md).
> 

## <a name="overview"></a>Přehled
Služby App Service Environment je [Premium] [ PremiumTier] služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service ve velkém rozsahu, včetně [webové aplikace][WebApps], [Mobile Apps][MobileApps], a [aplikace API][APIApps].  

Služby App Service Environment jsou ideální pro aplikační procesy vyžadující:

* Velmi velký rozsah
* Izolace a bezpečný přístup

Zákazníci vytvářet více prostředí App Service v rámci jedné oblasti Azure, a také nad několika oblastmi Azure.  Tím je ideální pro vodorovně škálování vrstvy aplikace bez stavu na podporu vysoké zatížení RPS prostředí App Service.

Služby App Service Environment jsou izolované toorunning jenom jednoho zákazníka aplikace a jsou vždy nasazené do virtuální sítě.  Zákazníci mají jemně odstupňovanou kontrolu nad obě aplikace příchozí a odchozí síťový provoz a aplikace může vytvořit vysokorychlostní zabezpečené připojení přes virtuální sítě tooon místním firemním prostředkům.

Všechny články a jak – do této o prostředí App Service jsou k dispozici v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

Přehled o tom, jak prostředí App Service povolit velkém rozsahu a zabezpečit přístup k síti najdete v tématu hello [podrobné informace AzureCon] [ AzureConDeepDive] na prostředí App Service!

Přímý informace na vodorovně škálování pomocí několika prostředí App Service naleznete hello článku na postupy toosetup [geograficky distribuovaná aplikace nároků][GeodistributedAppFootprint].

toosee, jak byla nakonfigurována Architektura zabezpečení hello ukazuje hello AzureCon podrobné informace, najdete v článku hello na implementaci [Architektura zabezpečení na základě](app-service-app-service-environment-layered-security.md) s prostředí App Service.

Aplikace běžící v prostředí App Service může mít svůj přístup závislé na nadřazený zařízení, jako jsou brány firewall systému webové aplikace (firewall webových aplikací).  článek Hello na [konfigurace firewall webových aplikací pro prostředí App Service](app-service-app-service-environment-web-application-firewall.md) popisuje tento scénář. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a>Vyhrazený výpočetní prostředky
Všechny hello výpočetních prostředků ve službě App Service Environment jsou vyhrazené výhradně tooa jednoho předplatného a služby App Service Environment se dá nakonfigurovat s až toofifty (50) výpočetní prostředky pro výhradní použití jedné aplikace.

Služby App Service Environment se skládá z front-endu výpočetní fondu zdrojů, jakož i fondy prostředků výpočetního jeden toothree pracovních procesů. 

Hello front-end fond obsahuje výpočetní prostředky zodpovědná za ukončení protokolu SSL jako dobře automatické vyvažování zátěže požadavků aplikace v rámci služby App Service Environment. 

Každý pracovní fond obsahuje výpočetní prostředky přidělené příliš[plány služby App Service][AppServicePlan], který naopak obsahovat jeden nebo více aplikacemi Azure App Service.  Vzhledem k tomu může být až toothree jiný pracovní fondy ve službě App Service Environment, máte hello flexibilitu toochoose různé výpočetní prostředky pro každý fond pracovních procesů.  

Například můžete fond toocreate jeden pracovní proces s méně výkonná výpočetní prostředky pro plány služby App Service určený pro vývoj nebo testování aplikací.  Fond pracovních procesů druhý (nebo i třetí) může použít výkonnější výpočetní prostředky, které jsou určené pro plány služby App Service spouští aplikace produkční.

Další informace o objemu hello výpočetní prostředky k dispozici toohello front-endu a fondy pracovních procesů v [jak tooConfigure služby App Service Environment][HowToConfigureanAppServiceEnvironment].  

Pro informace o hello dostupné výpočetní prostředek velikosti podporované ve službě App Service Environment, poraďte se hello [App Service – ceny] [ AppServicePricing] stránky a zkontrolujte hello k dispozici možnosti pro prostředí App Service v hello cenová úroveň Premium.

## <a name="virtual-network-support"></a>Podpora virtuální sítě
Služby App Service Environment se dají vytvářet v **buď** virtuální síť Azure Resource Manager **nebo** virtuální síť modelu nasazení classic ([Další informace o virtuálních sítí] [MoreInfoOnVirtualNetworks]).  Vzhledem k tomu, že služby App Service Environment vždy existuje ve virtuální síti a přesněji v podsíti virtuální sítě, můžete využívat funkce zabezpečení hello toocontrol virtuální sítě i příchozí a odchozí síťovou komunikaci.  

Služby App Service Environment může být buď internetové s veřejnou IP adresu, nebo interní, kterým čelí jenom adresu Azure vyrovnávání interní zatížení (ILB).

Můžete použít [skupin zabezpečení sítě] [ NetworkSecurityGroups] toorestrict příchozí podsíť toohello komunikace sítě, které se nachází služby App Service Environment.  To vám umožní toorun aplikace za nadřazeného zařízení a služeb, jako jsou brány firewall webových aplikací a poskytovatelů SaaS sítě.

Aplikace také často potřebují tooaccess podnikovým prostředkům, jako je například interní databází a webové služby.  Běžný postup je toomake tyto koncové body k dispozici pouze toointernal příchozí síťový provoz v rámci virtuální sítě Azure.  Jakmile služby App Service Environment je připojený k toohello stejné virtuální síti jako hello interních služeb, aplikace běžící v prostředí hello k nim přístup, včetně koncových bodů, které jsou dostupné prostřednictvím [Site-to-Site] [ SiteToSite]a [Azure ExpressRoute] [ ExpressRoute] připojení.

Pro další informace o tom, jak prostředí App Service práci s virtuálními sítěmi a místními sítěmi naleznete následující články hello [síťovou architekturu][NetworkArchitectureOverview], [řízení příchozí Provoz][ControllingInboundTraffic], a [bezpečně připojení tooBackends][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Začínáme
tooget začít s prostředí App Service najdete v části [jak tooCreate App Service Environment][HowToCreateAnAppServiceEnvironment]

Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service][AzureAppService].

Přehled hello App Service Environment síťovou architekturu, najdete v části hello [přehled architektury sítě] [ NetworkArchitectureOverview] článku.

Podrobnosti o používání služby App Service Environment se službou ExpressRoute, naleznete v následujícím článku hello [Express Route a prostředí App Service][NetworkConfigDetailsForExpressRoute].

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


