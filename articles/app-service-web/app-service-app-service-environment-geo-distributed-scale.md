---
title: "aaaGeo distribuované škálování s prostředí App Service"
description: "Zjistěte, jak toohorizontally škálování aplikace pomocí Traffic Manageru a prostředí App Service geo rozdělení."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: c1b05ca8-3703-4d87-a9ae-819d741787fb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: stefsch
ms.openlocfilehash: 9b441f637d8b7f679b3d83240baf99b8ee57e8f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="geo-distributed-scale-with-app-service-environments"></a>Geograficky distribuované škálování v prostředí App Service Environments
## <a name="overview"></a>Přehled
Scénáře aplikací, které vyžadují velmi vysoký škálování může být vyšší než hello výpočetních prostředků kapacity k dispozici tooa jedno nasazení aplikace.  Hlasování aplikace, sportovní události a události televizní Zábava jsou všechny příklady scénářů, které vyžadují velmi velkém rozsahu. Měřítko požadavky lze splnit vodorovně škálování aplikace s více nasazení aplikací, které jsou určené v jedné oblasti, a také v oblastech, toohandle extrémně zatížení požadavky.

Služby App Service Environment jsou ideální platformu pro horizontální škálování.  Po konfiguraci služby App Service Environment nebyla vybrána podporující rychlost známé požadavků, vývojáři můžete nasadit další prostředí App Service v "soubor cookie ořezávání" způsobem tooattain zatížení ve špičce požadovanou kapacitou.

Předpokládejme například, že aplikace spuštěné v konfiguraci služby App Service Environment byl otestovaný toohandle 20 K požadavků za sekundu (RPS).  Pokud zatížení ve špičce požadované hello kapacita je 100 tisíc RPS, pět (5) prostředí App Service je možné vytvořit a nakonfigurované tooensure hello aplikace dokáže zvládat hello maximální předpokládané zatížení.

Vzhledem k tomu, že zákazníci obvykle přístup aplikací pomocí vlastní (nebo jednoduché) domény, třeba vývojáři aplikace na způsob toodistribute požadavků ve všech instancí služby App Service Environment hello.  Skvělý způsob tooaccomplish, to je tooresolve hello vlastní doménu pomocí [profilu Azure Traffic Manageru][AzureTrafficManagerProfile].  Hello Traffic Manager profil může být nakonfigurované toopoint všechny hello jednotlivých prostředí App Service.  Traffic Manager automaticky zpracuje, distribuovat zákazníkům napříč všemi hello prostředí App Service v závislosti na nastavení v profilu služby Traffic Manager hello služby Vyrovnávání zatížení hello.  Tento postup funguje bez ohledu na to, jestli všechny hello prostředí App Service jsou nasazené v jedné oblasti Azure, nebo nasazeným po celém světě nad několika oblastmi Azure.

Navíc vzhledem k tomu, že zákazníkům přístup k aplikacím prostřednictvím hello jednoduché domény, zákazníci neberou hello počet prostředí App Service spuštěna aplikace.  V důsledku vývojáři můžete rychle a snadno přidat a odebrat, podle zjištěnou přenosové zatížení prostředí App Service.

Hello koncepční diagram znázorňuje aplikace horizontálně škálovat na více systémů mezi tři prostředí App Service v jedné oblasti.

![Konceptuální architektura][ConceptualArchitecture] 

Hello zbývající část tohoto tématu vás provede kroky hello s nastavením Distribuovaná topologie pro ukázkovou aplikaci hello pomocí několika prostředí App Service.

## <a name="planning-hello-topology"></a>Plánování hello topologie
Před vytvořením na nároky distribuované aplikace, pomůže toohave několik částí informace předem.

* **Vlastní doména pro aplikace hello:** co je hello vlastní název domény, zákazníci použijete tooaccess hello aplikace?  Pro vlastní doménu, hello ukázkové aplikace hello název je *www.scalableasedemo.com*
* **Doménu Traffic Manageru:** název domény musí toobe vybrali při vytváření [profilu Azure Traffic Manageru][AzureTrafficManagerProfile].  Tento název bude sloučen s hello *trafficmanager.net* přípona tooregister položku domény, který je spravován nástrojem Traffic Manager.  Ukázkové aplikace hello je zvolený název hello *škálovatelné App Service Environment ukázku*.  Výsledkem je název hello úplné domény, který je spravován nástrojem Traffic Manager *škálovatelné App Service Environment demo.trafficmanager.net*.
* **Strategie pro škálování nároků aplikace hello:** budou nároky aplikace hello distribuována mezi více prostředí App Service v jedné oblasti?  Více oblastí?  Kombinaci a match obou přístupů?  Hello rozhodnutí by měla být založena na očekávání kde provozu zákazníka budou pocházet, a jak můžete škálovat zbytek dobře hello aplikace podpůrná infrastruktura back-end.  Například s 100 % bezstavové aplikace, aplikace je možné massively rozšířit pomocí kombinace více prostředí App Service na oblast Azure, násobenou prostředí App Service nasadit nad několika oblastmi Azure.  S 15 + veřejných oblastí Azure k dispozici toochoose z skutečně zákazníci mohou vytvářet nároky celém světě flexibilně škálovatelné aplikace.  Pro hello ukázková aplikace používá pro tento článek byly vytvořeny tři prostředí App Service v jedné oblasti Azure (Jižní střední USA).
* **Zásady vytváření názvů pro hello prostředí App Service:** každý App Service Environment vyžaduje jedinečný název.  Nad rámec jednoho nebo dvou prostředí App Service je užitečné toohave zásady vytváření názvů toohelp identifikaci jednotlivých App Service Environment.  Pro ukázkovou aplikaci hello byl použit jednoduché zásady vytváření názvů.  Hello názvy hello tři prostředí App Service jsou *fe1ase*, *fe2ase*, a *fe3ase*.
* **Zásady vytváření názvů pro aplikace hello:** vzhledem k tomu, že budou nasazeny více instancí aplikace hello, název se vyžaduje pro každou instanci hello nasazené aplikace.  Jeden málo známé, ale je užitečná funkce prostředí App Service je tento text hello, které lze použít stejný název aplikace v rámci více prostředí App Service.  Vzhledem k tomu, že každý App Service Environment má příponu domény jedinečný, mohou zvolit použití toore hello přesně stejný název aplikace v každém prostředí.  Vývojář může mít třeba aplikace s názvem následujícím způsobem: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*atd.  Pro hello ukázkovou aplikaci, když každá instance aplikace také musí mít jedinečný název.  Hello aplikace instance používá názvů *webfrontend1*, *webfrontend2*, a *webfrontend3*.

## <a name="setting-up-hello-traffic-manager-profile"></a>Nastavení hello profil služby Traffic Manager
Jakmile více instancí aplikace nasazené na několika prostředí App Service, instance jednotlivých aplikací hello lze registrovat pomocí nástroje Traffic Manager.  Pro ukázkovou aplikaci hello Traffic Manager profil je potřeba pro *škálovatelné demo.trafficmanager.net App Service Environment* , může směrovat instancí aplikace zákazníkům tooany hello následující nasazení:

* **webfrontend1.fe1ase.p.azurewebsites.NET:** instanci hello ukázkové aplikace nasazené na hello první App Service Environment.
* **webfrontend2.fe2ase.p.azurewebsites.NET:** instanci hello ukázkové aplikace nasazené na hello druhý App Service Environment.
* **webfrontend3.fe3ase.p.azurewebsites.NET:** instanci hello ukázkové aplikace nasazené na hello třetí App Service Environment.

Nejjednodušší způsob, jak tooregister Hello několik Azure App Service koncových bodů, všech spuštěných v hello **stejné** oblast Azure, je s hello prostředí Powershell [podpory Azure Resource Manager Traffic Manager] [ ARMTrafficManager].  

prvním krokem Hello je toocreate profilu Azure Traffic Manageru.  Následující kód Hello ukazuje, jak hello profil vytvořený pro hello ukázkovou aplikaci:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Všimněte si, jak hello *RelativeDnsName* parametr byl nastaven příliš*škálovatelné App Service Environment ukázku*.  Toto je název domény jak hello *škálovatelné App Service Environment demo.trafficmanager.net* je vytvořena a přidružena profil Traffic Manageru.

Hello *TrafficRoutingMethod* parametr definuje zásady Traffic Manager použije toodetermine jak zákazník toospread zatížení v rámci všech dostupných koncových bodů hello rozložení zátěže hello.  V tento příklad hello *vážená* jste vybrali metodu.  Tato akce způsobí žádostem zákazníků se šíří přes všechny koncové body hello registrované aplikace založené na relativní váhu hello přidružené každý koncový bod. 

Když hello profil vytvořen každá instance aplikace přidána toohello profil jako nativní koncového bodu Azure.  Následující kód Hello načte odkaz tooeach front-endu webové aplikace a pak přidá každou aplikaci jako koncový bod Traffic Manager prostřednictvím hello *TargetResourceId* parametr.

    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10

    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

Všimněte si, jak je jedno volání příliš*přidat AzureTrafficManagerEndpointConfig* pro každou instanci jednotlivých aplikací.  Hello *TargetResourceId* odkazuje na parametr v každém příkazu prostředí Powershell, jeden hello tři instancí nasazené aplikace.  Hello profil služby Traffic Manager se zatížení rozloženy všechny tři koncové body, které jsou zaregistrované v profilu hello.

Všechny tři koncových bodů použít hello hello stejné hodnoty (10) hello *váhy* parametr.  Výsledkem rozprostření žádostem zákazníků Traffic Manager napříč všemi instancemi tři aplikace relativně rovnoměrně. 

## <a name="pointing-hello-apps-custom-domain-at-hello-traffic-manager-domain"></a>Vlastní doména polohovací hello aplikace v hello doménu Traffic Manageru
posledním krokem Hello potřeby je toopoint hello vlastní domény aplikace hello na doménu Traffic Manageru hello.  Ukázková aplikace hello to znamená, odkazující *www.scalableasedemo.com* v *škálovatelné App Service Environment demo.trafficmanager.net*.  Tento krok musí toobe byla dokončena s hello doménového registrátora, která spravuje hello vlastní doménu.  

Pomocí nástrojů pro správu vašeho registrátora domény toobe potřebám záznamy CNAME vytvoření vlastní domény které body hello na doménu Traffic Manageru hello.  Hello následující obrázek ukazuje příklad tuto konfiguraci CNAME, které bude vypadat takto:

![CNAME pro vlastní doménu.][CNAMEforCustomDomain] 

I když nejsou zahrnuté v tomto tématu, mějte na paměti, že každá instance jednotlivých aplikací vyžaduje toohave hello vlastní domény se také zaregistrována.  Jinak pokud žádost o umožňuje tooan instanci aplikace a aplikace hello nemá hello zaregistrována aplikace hello vlastní doménu, hello požadavek selže.  

Tento příklad hello vlastní doména je *www.scalableasedemo.com*, a každá instance aplikace má vlastní domény hello s ním spojená.

![Vlastní doména][CustomDomain] 

Rekapitulace registrace vlastní domény s aplikacemi Azure App Service, najdete v části hello následujícího článku [registrace vlastní domény][RegisterCustomDomain].

## <a name="trying-out-hello-distributed-topology"></a>Vyzkoušení hello Distribuovaná topologie
Hello konečný výsledek hello Traffic Manager a DNS konfigurace se žádostí o *www.scalableasedemo.com* bude procházet hello následující pořadí:

1. Prohlížeč nebo zařízení bude vyhledávání DNS *www.scalableasedemo.com*
2. Hello záznam CNAME u registrátora domény hello způsobí, že hello DNS vyhledávání toobe přesměrováno tooAzure Traffic Manager.
3. Vyhledávání DNS se provádí *škálovatelné App Service Environment demo.trafficmanager.net* proti jeden ze serverů Azure DNS Traffic Manager hello.
4. Podle zásad rozložení zátěže hello (hello *TrafficRoutingMethod* parametr dříve použitý při vytváření profilu služby Traffic Manager hello), bude Traffic Manager vyberte jeden z hello nakonfigurované koncové body a vrátí hello plně kvalifikovaný název domény, koncový bod toohello prohlížeč nebo zařízení.
5. Vzhledem k tomu, že hello plně kvalifikovaný název domény hello koncového bodu je adresa Url hello instance aplikace spuštěná na služby App Service Environment, hello prohlížeče nebo zařízení požádá server Microsoft Azure DNS tooresolve hello plně kvalifikovaný název domény tooan IP adresu. 
6. Hello prohlížeče nebo zařízení, odešle hello HTTP/S požadavek toohello IP adresu.  
7. žádost o Hello dorazí na jednu z instancí aplikace hello spuštěn v jednom z hello prostředí App Service.

Hello konzoly následující obrázek ukazuje vyhledávání DNS pro hello ukázkovou aplikaci vlastní domény úspěšně řešení tooan instanci aplikace spuštěn v jednom z ukázkové hello tři prostředí App Service (v tomto případě hello sekundu hello tři prostředí App Service):

![Vyhledávání DNS][DNSLookup] 

## <a name="additional-links-and-information"></a>Další odkazy a informace
Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

Dokumentace na hello prostředí Powershell [podpory Azure Resource Manager Traffic Manager][ARMTrafficManager].  

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]: ../traffic-manager/traffic-manager-manage-profiles.md
[ARMTrafficManager]: ../traffic-manager/traffic-manager-powershell-arm.md
[RegisterCustomDomain]: app-service-web-tutorial-custom-domain.md


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
