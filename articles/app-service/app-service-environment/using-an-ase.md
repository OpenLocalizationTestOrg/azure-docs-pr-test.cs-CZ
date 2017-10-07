---
title: "aaaUse služby Azure App Service environment"
description: "Jak toocreate, publikování a škálování aplikací v Azure App Service environment"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 30c89e384efc07c560254856c0ca7d4eb4b1f010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-an-app-service-environment"></a>Použití služby App Service environment #

## <a name="overview"></a>Přehled ##

Azure App Service Environment je nasazení služby Azure App Service na podsíť ve virtuální síti Azure zákazníka. Obsahuje:

- **Front-končí**: hello front-end jsou, kde protokolu HTTP nebo HTTPS ukončí ve službě App Service environment (App Service Environment).
- **Pracovníci**: hello zaměstnanci jsou hello prostředky, které jsou hostiteli aplikace.
- **Databáze**: hello databáze obsahuje informace, které definují hello prostředí.
- **Úložiště**: hello úložiště je použité toohost hello zákazníka publikované aplikace.

> [!NOTE]
> Existují dvě verze App Service Environment: ASEv1 a ASEv2. V ASEv1 musí spravovat hello prostředky, abyste mohli používat. toolearn jak tooconfigure a spravovat ASEv1, najdete v části [konfigurace v1 prostředí App Service][ConfigureASEv1]. Hello zbývající části tohoto článku se zaměřuje na ASEv2.
>
>

App Service Environment (ASEv1 a ASEv2) můžete nasadit vnější nebo vnitřní VIP pro přístup k aplikaci. Hello nasazení s externí VIP se obvykle nazývá externí App Service Environment. Interní verze Hello se nazývá hello ILB App Service Environment, protože používá interní nástroj (ILB). toolearn Další informace o hello ILB App Service Environment, najdete v části [vytvoření a použití App Service Environment ILB][MakeILBASE].

## <a name="create-a-web-app-in-an-ase"></a>Vytvoření webové aplikace v App Service Environment ##

toocreate webové aplikace v App Service Environment, použijte hello stejný proces, jako když vytvoříte ho normálně, ale s několika malé rozdíly. Když vytvoříte nový plán služby App Service:

- Místo výběru geografické umístění které toodeploy vaší aplikace, vyberte App Service Environment jako vaše umístění.
- Všechny plány služby App Service, které jsou vytvořené v App Service Environment musí být v izolovaná cenová úroveň.

Pokud nemáte App Service Environment, můžete vytvořit pomocí následujících pokynů hello v [vytvoření služby App Service environment][MakeExternalASE].

toocreate webové aplikace v App Service Environment:

1. Vyberte **nové** > **Web + mobilní** > **webová aplikace**.

2. Zadejte název pro hello webovou aplikaci. Pokud jste již vybrali plán aplikační služby App Service Environment, název domény hello hello aplikace zobrazuje název domény hello hello App Service Environment.

    ![Výběr názvu webové aplikace][1]

3. Vyberte předplatné.

4. Zadejte název pro novou skupinu prostředků, nebo vyberte **použít existující** a vyberte z rozevíracího seznamu hello.

5. Vybrat existující plán aplikační služby ve vaší App Service Environment, nebo vytvořte novou pomocí následujících kroků:

    a. Vyberte **vytvořit nový**.

    b. Zadejte název hello plán služby App Service.

    c. Vyberte vaše App Service Environment v hello **umístění** rozevíracího seznamu.

    d. Vyberte **izolovaná** cenová úroveň. Vyberte **vyberte**.

    e. Vyberte **OK**.
    
    ![Izolované cenové úrovně][2]

6. Vyberte **Vytvořit**.

## <a name="how-scale-works"></a>Jak škálování funguje ##

Všechny aplikace služby App Service běží v plán služby App Service. model kontejneru Hello je prostředí podržte plány služby App Service a plány služby App Service uchování aplikace. Při změně měřítka aplikace, můžete škálovat hello plán služby App Service a proto škálování všechny aplikace hello v hello stejný plán.

V ASEv2 když škálovat plán služby App Service hello potřeby infrastruktury se automaticky přidá. Je prodlevu tooscale operace při přidání hello infrastruktury. Hello potřeby infrastruktury v ASEv1, je nutné přidat před vytvořením nebo škálovat plán služby App Service. 

V hello víceklientské služby App Service, škálování je obvykle okamžitou protože fond zdrojů je snadno dostupné toosupport ho. App Service Environment neexistuje žádný takový vyrovnávací paměti a prostředky se přidělují při nutné.

V App Service Environment můžete škálovat instance too100. Ty 100 instance může být v jedné jednoho plánu služby App Service nebo rozdělené mezi více plány služby App Service.

## <a name="ip-addresses"></a>IP adresy ##

Služby App Service má možnost tooallocate hello tooan aplikace na vyhrazené IP adresy. Tato funkce je dostupná, po dokončení konfigurace SSL založenou na protokolu IP, jak je popsáno v [vytvořit vazbu existující vlastní SSL certifikát tooAzure webové aplikace][ConfigureSSL]. V App Service Environment, existuje však významné výjimky. Nelze přidat další IP adresy toobe používá pro protokolem SSL založenou na protokolu IP v App Service Environment ILB.

ASEv1 je nutné tooallocate hello IP adresy jako prostředky než budete moci použít. V ASEv2 je použít z aplikace stejným způsobem jako v hello víceklientské služby App Service. Je vždy jedna adresa k výměně za chodu v ASEv2 too30 IP adres. Pokaždé, když používáte jednu, jiné se přidá tak, aby vždy snadno dostupné pro použití adresu. Čas zpoždění je potřeba tooallocate jinou IP adresu, která brání přidání IP adres rychle po sobě.

## <a name="front-end-scaling"></a>Škálování front-endu ##

V ASEv2, když škálovat plánu služby App Service, pracovníci se automaticky přidají toosupport je. Každý App Service Environment se vytvoří s dva elementy front end. Kromě toho hello front-end automaticky škálovat s rychlostí jeden front-endu pro každých 15 instance v plánu služby App Service. Například pokud máte instancí 15, pak máte tři front-end. Pokud změníte velikost instance too30, pak máte čtyři front-elementy end a tak dále.

Tento počet elementů front end musí být víc než dost pro většinu scénářů. Však můžete škálovat s vyšší rychlostí. Můžete změnit hello poměr, které tooas nedostatek jako jeden front-endu pro každých pět instancí. Není k dispozici zdarma pro změnu hello poměr. Další informace najdete v tématu [Azure App Service – ceny][Pricing].

Front-endové prostředky jsou hello koncový bod HTTP nebo HTTPS pro hello App Service Environment. S konfigurací front-end hello výchozí je využití paměti na front-endu konzistentně přibližně 60 procent. Na front-end nejsou spouštěny úloh zákazníka. Hello klíčovým faktorem pro front-end s ohledem tooscale je hello procesoru, který doprovází především komunikaci přes protokol HTTPS.

## <a name="app-access"></a>Přístup k aplikaci ##

Externí App Service Environment se liší od hello víceklientské služby App Service hello domény, který se používá při vytváření aplikace. Obsahuje název hello hello App Service Environment. Další informace o tom, najdete v části toocreate externí App Service Environment, [vytvoření služby App Service environment][MakeExternalASE]. název domény Hello externí App Service Environment vypadá jako *.&lt; asename&gt;. p.azurewebsites.net*. Například pokud je název vaší App Service Environment _externí App Service Environment_ a hostování aplikace volá _contoso_ v této App Service Environment, dostanete se na následující adresy URL hello:

- contoso.external ase.p.azurewebsites.net
- contoso.SCM.external ase.p.azurewebsites.net

Adresa URL Hello contoso.scm.external-ase.p.azurewebsites.net je použité tooaccess hello Kudu konzole nebo pro publikování aplikace pomocí webového nasazení. Informace v konzole Kudu hello najdete v tématu [konzoly Kudu pro službu Azure App Service][Kudu]. Hello Kudu konzole vám webového uživatelského rozhraní pro ladění, nahrávání souborů, úpravy souborů a mnoho dalšího.

V App Service Environment ILB určíte hello domény v době nasazení. Další informace o tom, jak toocreate ILB App Service Environment, najdete v části [vytvoření a použití App Service Environment ILB][MakeILBASE]. Pokud zadáte název domény hello _ilb ase.info_, hello aplikace v tomto App Service Environment používala tuto doménu během vytváření aplikace. Pro hello aplikaci s názvem _contoso_, se hello adresy URL:

- contoso.ilb ase.info
- contoso.SCM.ilb ase.info

## <a name="publishing"></a>Publikování ##

Stejně jako u hello víceklientské služby App Service, v App Service Environment můžete publikovat pomocí:

- Nasazení webu.
- FTP.
- Nepřetržité integrace.
- Přetáhnout myší v konzole Kudu hello.
- IDE, jako je například Visual Studio, Eclipse nebo IntelliJ IDEA.

S externí App Service Environment tyto možnosti publikování, které všechny chovat hello stejné. Další informace najdete v tématu [nasazení v Azure App Service][AppDeploy]. 

Hlavní rozdíl Hello s publikování je s ohledem tooan ILB App Service Environment. S ILB App Service Environment jsou všechny dostupné pouze prostřednictvím hello ILB hello publikování koncové body. Hello ILB je v privátní IP v podsíti hello App Service Environment ve virtuální síti hello. Pokud nemáte síťový přístup toohello ILB, nelze publikovat všechny aplikace na tomto App Service Environment. Jak jsme uvedli v [vytvoření a použití App Service Environment ILB][MakeILBASE], potřebujete tooconfigure DNS pro aplikace hello systému hello. Hello SCM koncový bod, který zahrnuje. Pokud nejsou definovány správně, nelze publikovat. Vaše integrovaného vývojového prostředí také potřebovat toohave síťový přístup toohello ILB v pořadí toopublish tooit přímo.

Internetové systémy položek konfigurace, například Githubu a Visual Studio Team Services, nefungují s ILB App Service Environment, protože hello publikování koncový bod není dostupný na Internetu. Místo toho musíte toouse CI systému, který používá model pull, jako je Dropbox.

Hello publikování koncové body pro aplikace v App Service Environment ILB použít doménu hello této hello, který byl vytvořen ILB App Service Environment. Zobrazí se v profilu publikování hello aplikace a v okně portálu aplikace hello (v **přehled** > **Essentials** a také v **vlastnosti**). 

## <a name="pricing"></a>Ceny ##

Hello ceny SKU názvem **izolovaná** vytvořený pro použití pouze s ASEv2. Všechny plány služby App Service, které jsou hostované v ASEv2 jsou v hello Isolated ceny SKU. Izolované sazby plán služby App Service se může lišit podle oblastí. 

Také plány toohello ceny pro aplikační služby, je míra dvojrozměrném pro App Service Environment, sám sebe. Hello paušálních nemění hello velikost vašeho App Service Environment a platí za hello App Service Environment infrastruktury na výchozí počet 1 Další škálování front-endu pro každých 15 instancích plánu služby App Service.  

Pokud není dostatečná hello výchozí škálování míra 1 front-endu pro každých 15 instancích plánu služby App Service, můžete upravit hello poměr, kdy front-end se přidají nebo hello velikost hello front-end.  Když upravíte poměr hello nebo velikost, platíte za hello front-end jader, které by ve výchozím nastavení přidat.  

Například pokud nastavíte hello škálování poměr too10, front-end přidá se pro každých 10 instancí v plánu služby App Service. ploché poplatek Hello zahrnuje škálování výši jeden front-endu pro každých 15 instance. S poměrem škálování 10 platí poplatek za hello třetí front-end, který byl přidán pro hello 10 instancích plánu služby App Service. Nepotřebujete toopay pro něj, když se dostanete 15 instance, protože byl přidán automaticky.

Pokud velikost Nastaví hello hello front-end too2 jader, ale pak platíte za neměnit hello poměr hello navíc jader.  App Service Environment se vytvoří s 2 front-end, tak i pod hello automatické škálování prahové hodnoty, které by platíte za 2 jádra navíc Pokud vyšší hello velikost too2 základní front-end.

Další informace najdete v tématu [Azure App Service – ceny][Pricing].

## <a name="delete-an-ase"></a>Odstranit App Service Environment ##

toodelete App Service Environment: 

1. Použití **odstranit** hello horní části hello **App Service Environment** okno. 

2. Zadejte název vaší tooconfirm App Service Environment, které chcete toodelete hello ho. Pokud odstraníte App Service Environment, odstraní se všechny hello obsah v něm také. 

    ![Odstranění App Service Environment][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png


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
