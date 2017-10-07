---
title: "aaaIntroduction tooAzure služby App Service Environment"
description: "Stručný přehled služby Azure App Service Environment"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 9261041333cf59374974a039edf89c4983c45cdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapp-service-environments"></a>Úvod tooApp služby prostředí #
 
## <a name="overview"></a>Přehled ##

Azure App Service Environment je funkce služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací App Service ve velkém rozsahu. Tato funkce může hostovat vaše [webové aplikace][webapps], [mobilní aplikace][mobileapps], [aplikace API][APIapps], a [funkce][Functions].

Služby App Service Environment (ASEs) jsou vhodné pro aplikační procesy, které vyžadují:

- Velmi velký rozsah.
- Izolace a bezpečný přístup.
- Využití velkého množství paměti.

Zákazníci vytvářet více ASEs v rámci jedné oblasti Azure nebo nad několika oblastmi Azure. Díky této flexibilitě ASEs ideální pro bezstavové aplikačními vrstvami podporu vysoké zatížení RPS vodorovně škálování.

ASEs jsou izolované toorunning jenom jednoho zákazníka aplikace a jsou vždy nasazené do virtuální sítě. Zákazníci mají jemně odstupňovanou kontrolu nad aplikace příchozí a odchozí síťový provoz. Aplikace může vytvořit vysokorychlostní zabezpečené připojení prostřednictvím sítě VPN tooon místním firemním prostředkům.

Všechny články a jak tooinstructions o ASEs jsou k dispozici v hello [soubor README pro služby App Service Environment][ASEReadme]:

* ASEs povolit špičkové aplikace hostuje s zabezpečený přístup. Další informace najdete v tématu hello [podrobné informace AzureCon](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) na ASEs.
* Více ASEs lze použít tooscale vodorovně. Další informace najdete v tématu [jak tooset až nároků geograficky distribuovaná aplikace](https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/).
* ASEs lze použít tooconfigure Architektura zabezpečení, jak je znázorněno v hello AzureCon podrobné informace. toosee, jak byla nakonfigurována Architektura zabezpečení hello ukazuje hello AzureCon podrobné informace, najdete v části hello [článek věnovaný tomu, jak tooimplement architektura vrstveného zabezpečení](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-layered-security) s služby App Service Environment.
* Aplikace běžící na ASEs může mít svůj přístup ověřované vrácení nadřazeného zařízeními, jako jsou brány firewall systému webové aplikace (WAFs). Další informace najdete v tématu [konfigurace firewall webových aplikací pro služby App Service Environment](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-web-application-firewall).

## <a name="dedicated-environment"></a>Vyhrazeném prostředí ##

App Service Environment je vyhrazené výhradně tooa jednoho předplatného a může být hostitelem 100 instancí. rozsah Hello může mít rozsah 100 instancí v jeden plány služby App Service plán too100 jednou instancí služby App Service a všechno mezi nimi.

App Service Environment se skládá z front je ukončená a pracovních procesů. Front-end jsou zodpovědní za ukončení protokolu HTTP nebo HTTPS a automatické vyvažování zátěže požadavků aplikace v App Service Environment. Front-end se automaticky přidají jako hello plány služby App Service v App Service Environment hello jsou škálovat na více systémů.

Zaměstnanci jsou role, které jsou hostiteli aplikace zákazníka. Pracovníci jsou k dispozici tři pevné velikosti:

* Jeden základní/3.5 GB paměti RAM
* Dvě jádra/7 GB paměti RAM
* Čtyři základní/14 GB paměti RAM

Zákazníci nemusí toomanage front-end a pracovních procesů. Všechny infrastruktury je automaticky přidán jako zákazníci škálování jejich plány služby App Service. Jako škálovat App Service Environment nebo vytvořit plány služby App Service vyžaduje hello infrastruktury je přidat nebo odebrat podle potřeby.

Není nestrukturované měsíční míra, pro který platí za hello infrastruktury a nezmění hello velikost hello App Service Environment App Service Environment. Kromě toho je s náklady za jádra plán služby App Service. Všechny aplikace, které jsou hostované v App Service Environment jsou v hello izolované ceny SKU. Informace o cenách pro App Service Environment, najdete v části hello [služby App Service – ceny] [ Pricing] stránky a projděte si dostupné možnosti ASEs hello.

## <a name="virtual-network-support"></a>Podpora virtuální sítě ##

App Service Environment může vytvořit pouze v virtuální síť Azure Resource Manager. toolearn Další informace o virtuálních sítí Azure, najdete v části hello [Azure virtuální sítě – nejčastější dotazy](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). App Service Environment vždy existuje ve virtuální síti a přesněji řečeno, v rámci jedné podsítě virtuální sítě. Můžete použít funkce zabezpečení hello virtuální sítě toocontrol příchozí a odchozí síťové komunikace pro vaše aplikace.

App Service Environment může být internetové s veřejnou IP adresu nebo interní přístupem jenom adresu (ILB) nástroj pro vyrovnávání zatížení Azure interní.

[Skupin zabezpečení sítě] [ NSGs] omezit příchozích síťových komunikace toohello podsítě, které se nachází App Service Environment. Můžete použít skupiny Nsg toorun aplikace za nadřazeného zařízení a služby, například WAFs a poskytovatelů SaaS sítě.

Aplikace také často potřebují tooaccess podnikovým prostředkům, jako je například interní databází a webové služby. Pokud nasadíte hello App Service Environment ve virtuální síti, který má místní síť VPN připojení toohello, hello aplikace v App Service Environment hello přístup hello místních prostředků. Tato možnost platí bez ohledu na to, jestli je hello VPN [site-to-site](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/) nebo [Azure ExpressRoute](http://azure.microsoft.com/services/expressroute/) sítě VPN.

Další informace o tom, jak ASEs práci s virtuálními sítěmi a místními sítěmi najdete v tématu [aspekty sítě služby App Service Environment][ASENetwork].

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##

Služba App Service Environment má dvě verze: ASEv1 a ASEv2. Hello předcházející informace bylo založeno na ASEv2. Tato část obsahuje hello rozdíly mezi ASEv1 a ASEv2. 

ASEv1, je nutné toomanage všechny prostředky hello ručně. Která obsahuje hello front-end, pracovní procesy a adres IP použitých pro založená na protokolu IP. Než můžete škálovat plán služby App Service, je třeba toofirst horizontální navýšení kapacity fondu pracovních procesů hello místo toohost ho.

ASEv1 používá jiný model tvorby cen z ASEv2. V ASEv1 platí pro každý jádra přidělené. Používá pro front-end nebo pracovních procesů, které nejsou hostování jakékoliv zátěže jader, který zahrnuje. V ASEv1 hello výchozí škálování maximální velikost App Service Environment je 55 celkový počet hostitelů. Který zahrnuje pracovníků a front-end. Jeden tooASEv1 výhod je, aby se dala nasadit v klasické virtuální sítě a virtuální sítě Resource Manager. toolearn Další informace o ASEv1, najdete v části [App Service Environment v1 ÚVOD][ASEv1Intro].

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
