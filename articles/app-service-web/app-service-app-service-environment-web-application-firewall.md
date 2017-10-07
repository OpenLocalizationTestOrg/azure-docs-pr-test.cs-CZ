---
title: "aaaConfiguring webové aplikace brány Firewall (firewall webových aplikací) pro App Service Environment"
description: "Zjistěte, jak brány firewall tooconfigure webové aplikace před služby App Service Environment."
services: app-service\web
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: naziml
ms.openlocfilehash: 0fcf62aea871751c9d4f294d2d24df2186fc0e7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurace brány Firewall webových aplikací (firewall webových aplikací) pro služby App Service Environment
## <a name="overview"></a>Přehled
Webové aplikace brány firewall jako hello [Barracuda firewall webových aplikací pro Azure](https://www.barracuda.com/programs/azure) která je dostupná na hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) pomáhá zabezpečit vaše webové aplikace zkontrolováním příchozí webové přenosy tooblock SQL injekce, skriptování, malwaru nahrávání & aplikace DDoS a jiným útokům. Je také zkontroluje, zda obsahuje hello odpovědí z hello back endové webové servery pro prevenci ztráty dat (DLP). V kombinaci s hello izolace a další škálování poskytované prostředí App Service, toto poskytuje ideální prostředí toohost obchodní kritické webové aplikace, které potřebují toowithstand škodlivými požadavky a vyšší objemy přenosů.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Nastavení
K tomuto dokumentu, který bude nakonfigurujeme naše App Service Environment za několik zatížení vyvážit instancí Barracuda firewall webových aplikací tak, aby pouze provoz z firewall webových aplikací hello dosáhnout hello App Service Environment a nebude dostupný z hello hraniční sítě. Azure Traffic Manager jsme bude mít i před naše vyrovnávání tooload instance Barracuda firewall webových aplikací mezi datových center Azure a oblastech. Nejvyšší úrovni diagram instalačního programu hello bude vypadat co jsou uvedeny níže.

![Architektura][Architecture] 

> Poznámka: S hello zavedení [ILB podporu pro App Service Environment](app-service-environment-with-internal-load-balancer.md), můžete nakonfigurovat toobe hello App Service Environment nejsou přístupné z hello DMZ a pouze být k dispozici toohello privátní sítě. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Konfigurace prostředí služby App Service
tooconfigure služby App Service Environment odkazovat příliš[naší dokumentaci](app-service-web-how-to-create-an-app-service-environment.md) na hello subjektu. Po vytvoření služby App Service Environment máte, můžete vytvořit [webové aplikace](app-service-web-overview.md), [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md) a [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) v tomto prostředí, které budou všechny chráněné za firewall webových aplikací hello jsme Nakonfigurujte v další části hello.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfigurace Barracuda firewall webových aplikací cloudové služby
Barracuda má [podrobné článku](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) na nasazení jeho firewall webových aplikací na virtuálním počítači v Azure. Ale vzhledem k tomu, že nám chcete redundance a není způsobit jediný bod selhání, aby toodeploy minimálně 2 Firewall webových aplikací instance virtuálních počítačů do hello stejné cloudové služby při těchto pokynů.

### <a name="adding-endpoints-toocloud-service"></a>Přidání tooCloud koncové body služby
Jakmile máte 2 nebo více virtuálních počítačů firewall webových aplikací instancí v rámci cloudové služby můžete použít hello [portál Azure](https://portal.azure.com/) tooadd HTTP a HTTPS koncových bodů, které se používají v aplikaci, jak ukazuje následující obrázek hello.

![Konfigurace koncového bodu][ConfigureEndpoint]

Pokud vaše aplikace používat ostatní koncové body, ujistěte se, že tooadd také tyto toothis seznamu. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurace Barracuda firewall webových aplikací prostřednictvím portálu pro správu
Barracuda firewall webových aplikací používá TCP Port 8000 pro konfiguraci prostřednictvím portálu pro správu. Vzhledem k tomu, že máme několik instancí virtuálních počítačů hello firewall webových aplikací je nutné toorepeat hello zde uvedených kroků pro každou instanci virtuálního počítače. 

> Poznámka: Po dokončení konfigurace firewall webových aplikací odebrání koncový bod TCP/8000 hello všechny virtuální počítače firewall webových aplikací tookeep zabezpečené vaší firewall webových aplikací.
> 
> 

Přidáte koncový bod správy hello jak je znázorněno v následujícím tooconfigure obrázku hello vaší Barracuda firewall webových aplikací.

![Přidání koncového bodu správy][AddManagementEndpoint]

Pomocí prohlížeče toobrowse toohello správu koncového bodu v cloudové službě. Pokud cloudové služby je volána test.cloudapp.net, by procházením toohttp://test.cloudapp.net:8000 přístup k tomuto koncovému bodu. Měli byste vidět přihlašovací stránky, jako níže, můžete se přihlásit pomocí přihlašovacích údajů zadaných ve fázi instalace virtuálních počítačů hello firewall webových aplikací.

![Správa přihlašovací stránky][ManagementLoginPage]

Po přihlášení byste měli vidět řídicí panel jako hello jednu v bitové kopii hello nižší, než je nabídne základní statistické údaje o hello ochrany firewall webových aplikací.

![Řídicí panel správy][ManagementDashboard]

Kliknutím na kartu hello služby vám umožní nakonfigurovat vaše firewall webových aplikací pro služby, které chrání. Další informace o konfiguraci vašeho Barracuda firewall webových aplikací lze najít [jejich dokumentaci](https://techlib.barracuda.com/waf/getstarted1). V příkladu hello níže webové aplikace Azure byla nakonfigurována s provozem na protokolu HTTP a HTTPS.

![Přidání služeb správy][ManagementAddServices]

> Poznámka: V závislosti na tom, jak jsou nakonfigurované vašich aplikací a jaké funkce jsou používány ve službě App Service Environment, budete potřebovat tooforward provoz pro porty TCP než 80 a 443, například pokud máte instalaci IP SSL pro webovou aplikaci. Seznam síťové porty používané v prostředí App Service naleznete příliš[řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) části síťové porty.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurace Microsoft Azure Traffic Manageru (volitelné)
Pokud vaše aplikace je k dispozici v několika oblastech, pak by chcete vyrovnávat tooload je za [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). toodo, můžete přidat koncový bod v hello [portál Azure classic](https://manage.azure.com) pomocí hello název cloudové služby pro vaše firewall webových aplikací v hello profil služby Traffic Manager, jak ukazuje následující obrázek hello. 

![Koncový bod Traffic Manager][TrafficManagerEndpoint]

Pokud vaše aplikace vyžaduje ověřování, zajistěte, že abyste měli některé prostředků, která nevyžaduje žádné ověřování pro Traffic Manager tooping hello dostupnosti vaší aplikace. Hello adresy URL v části hello část konfigurace můžete nakonfigurovat na hello [portál Azure classic](https://manage.azure.com) jak je uvedeno níže.

![Konfigurace Traffic Manageru][ConfigureTrafficManager]

tooforward příkazy ping hello Traffic Manageru z vaší aplikace tooyour firewall webových aplikací, musíte překlady toosetup webu na firewall webových aplikací Barracuda tooforward provoz tooyour aplikace jak je znázorněno v následujícím příkladu hello.

![Překlady webu][WebsiteTranslations]

## <a name="securing-traffic-tooapp-service-environment-using-network-security-groups-nsg"></a>Zabezpečení provozu tooApp služby prostředí pomocí zabezpečení sítě skupiny (NSG)
Postupujte podle hello [řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) pro informace o omezení provozu tooyour App Service Environment z hello firewall webových aplikací pouze pomocí hello VIP adresy cloudové služby. Zde je ukázka příkazu prostředí Powershell pro provádění této úlohy pro TCP port 80.

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Nahraďte hello SourceAddressPrefix hello virtuální IP adresa (VIP) vaší firewall webových aplikací cloudové služby.

> Poznámka: hello VIP cloudové služby se změní, když je odstranit a znovu vytvořit hello cloudové služby. Ujistěte se, tooupdate hello IP adresu ve skupině prostředků sítě hello až to uděláte tak. 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
