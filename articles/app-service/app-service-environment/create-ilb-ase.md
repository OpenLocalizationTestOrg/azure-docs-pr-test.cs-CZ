---
title: "aaaCreate a použití vyrovnávání zatížení interní s Azure App Service environment"
description: "Informace o tom, toocreate a používání služby Azure App Service environment izolované Internetu"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: d019ca6f231c3acfdab4cd380db375a076802f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Vytvořit a použít vyrovnávání zatížení interní s služby App Service environment #

 Azure App Service Environment je nasazení služby Azure App Service do používanou podsíť virtuální sítě Azure (VNet). Existují dva způsoby toodeploy služby App Service environment (App Service Environment): 

- V VIP na externí IP adresu často označuje jako externí App Service Environment.
- S virtuální IP adresu na interní IP adresu často říká App Service Environment ILB protože hello vnitřní koncový bod je vyrovnávání interní zatížení (ILB). 

Tento článek ukazuje, jak toocreate ILB App Service Environment. Přehled na hello App Service Environment, najdete v tématu [prostředí Service tooApp ÚVOD][Intro]. jak zjistit, toocreate externí App Service Environment, toolearn [vytvořit externí App Service Environment][MakeExternalASE].

## <a name="overview"></a>Přehled ##

App Service Environment se koncový bod přístupné z Internetu nebo IP adresu můžete nasadit ve vaší virtuální síti. tooset hello IP adres tooa adres virtuální sítě, hello App Service Environment musí být nasazený s ILB. Při nasazení vaší App Service Environment s ILB, je nutné zadat:

-   Vlastní domény, který použijete při vytváření aplikace.
-   Hello certifikát používaný pro protokol HTTPS.
-   Správa služby DNS pro vaši doménu.

Naopak může provádět akce, jako:

-   Hostování aplikací intranetu bezpečně v hello cloudu, což přistupujete přes síť site-to-site nebo Azure ExpressRoute VPN.
-   Aplikace hostitele, v cloudu hello, které nejsou uvedené v veřejné servery DNS.
-   Vytvořte samostatný internet back-end aplikace, které aplikace front-endu může bezpečně integrovat.

### <a name="disabled-functionality"></a>Zakázané funkce ###

Existují některé věci, které nejde dělat, když používáte ILB App Service Environment:

-   Použijte SSL založenou na protokolu IP.
-   Přiřaďte IP adresy toospecific aplikace.
-   Zakoupení a použití certifikátu s aplikací prostřednictvím hello portálu Azure. Můžete získat certifikáty přímo od certifikační autority a jejich používání s vaší aplikací. Nelze získat je prostřednictvím hello portálu Azure.

## <a name="create-an-ilb-ase"></a>Vytvořit App Service Environment ILB ##

toocreate ILB App Service Environment:

1. V hello portálu Azure, vyberte **nový** > **Web + mobilní** > **App Service Environment**.

2. Vyberte své předplatné.

3. Vyberte nebo vytvořte skupinu prostředků.

4. Vyberte nebo vytvořte virtuální síť.

5. Pokud vyberete stávající virtuální síť, musíte toocreate podsíť toohold hello App Service Environment. Zajistěte, aby tooset podsíť velikost dostatečně velké na to tooaccommodate případnému budoucímu růstu vašeho App Service Environment. Doporučujeme velikost `/25`, která je 128 adresy a dokáže zpracovat App Service Environment maximální velikost. můžete vybrat minimální velikost Hello je `/28`. Po potřebuje infrastruktury, může být tato velikost škálovat tooa nesmí být delší než 11 instancí.

    * Překročila maximální výchozí hello 100 instancí v plánu služby App Service.

    * Škálování téměř 100, ale s více Rychlé škálování front-endu.

6. Vyberte **virtuální sítě nebo umístění** > **konfigurace virtuální sítě**. Sada hello **VIP typ** příliš**interní**.

7. Zadejte název domény. Tato doména je hello, jedna pro aplikace vytvořené v této App Service Environment. Platí určitá omezení. Nemůže být:

    * NET   

    * azurewebsites.NET

    * p.azurewebsites.NET

    * &lt;asename&gt;. p.azurewebsites.net

   Hello vlastního názvu domény použít pro aplikace a název domény hello používá vaše App Service Environment se nesmí překrývat. Pro App Service Environment ILB s názvem domény hello _contoso.com_, nemůžete použít vlastní názvy domén pro vaše aplikace, jako je:

    * www.contoso.com

    * Abcd.def.contoso.com

    * Abcd.contoso.com

   Pokud znáte hello vlastní názvy domén pro aplikace, vyberte doménu pro hello ILB App Service Environment, který nebude došlo ke konfliktu s těmito názvy vlastních domén. V tomto příkladu můžete použít něco jako *contoso-internal.com* pro doménu vaší App Service Environment hello vzhledem k tomu, který nebude v konfliktu s názvy vlastních domén, které končí *. contoso.com*.

8. Vyberte **OK**a potom vyberte **vytvořit**.

    ![Vytvoření služby ASE][1]

Na hello **virtuální sítě** okna, je **konfigurace virtuální sítě** možnost. Můžete ji tooselect na externí virtuální IP adresy nebo interní virtuální IP adresy. Výchozí hodnota Hello je **externí**. Pokud vyberete **externí**, vaše App Service Environment používá přístupné z Internetu VIP. Pokud vyberete **interní**, vaše App Service Environment je nakonfigurován s ILB na IP adresu ve virtuální síti.

Po výběru **interní**, tooadd možnost hello více IP adres tooyour App Service Environment se odebere. Místo toho musíte tooprovide hello domény hello App Service Environment. V App Service Environment externí VIP hello název hello App Service Environment se používá v hello domény pro aplikace vytvořené v této App Service Environment.

Pokud nastavíte **VIP typ** příliš**interní**, vaše jméno App Service Environment se nepoužívá v doméně hello pro hello App Service Environment. Zadejte domény hello explicitně. Pokud vaše doména je *contoso.corp.net* a vytvoříte aplikaci v tom, že s názvem App Service Environment *timereporting*, hello adresu URL pro tuto aplikaci je timereporting.contoso.corp.net.


## <a name="create-an-app-in-an-ilb-ase"></a>Vytvoření aplikace v App Service Environment ILB ##

Vytvoření aplikace v App Service Environment ILB v hello stejným způsobem jako za normálních okolností vytvoření aplikace v App Service Environment.

1. V hello portálu Azure, vyberte **nový** > **Web + mobilní** > **webové** nebo **Mobile** nebo  **Aplikace API**.

2. Zadejte název hello aplikace hello.

3. Vyberte předplatné hello.

4. Vyberte nebo vytvořte skupinu prostředků.

5. Vyberte nebo vytvořte plán služby App Service. Pokud chcete toocreate nový plán aplikační služby, vyberte jako hello umístění vaší App Service Environment. Vyberte místo, kam chcete vaší toobe plán služby App Service vytvořit fond pracovních procesů hello. Když vytvoříte hello plán služby App Service, vyberte jako hello umístění a fondu pracovních procesů hello vaší App Service Environment. Když zadáte název hello hello aplikace, domény hello pod názvem vaší aplikace je nahrazena hello domény pro váš App Service Environment.

6. Vyberte **Vytvořit**. Pokud chcete tooappear aplikace hello na řídicím panelu, vyberte **Pin toodashboard** zaškrtávací políčko.

    ![Vytvoření plánu služby App Service][2]

    V části **název aplikace**, název domény hello je doménou aktualizované tooreflect hello vaší App Service Environment.

## <a name="post-ilb-ase-creation-validation"></a>Ověření vytvoření POST-ILB App Service Environment ##

App Service Environment ILB je trochu jiná než hello jiný - ILB App Service Environment. Jak je již uvedeno je nutné toomanage vlastní DNS. Máte také tooprovide svůj vlastní certifikát pro připojení prostřednictvím protokolu HTTPS.

Po vytvoření vaší App Service Environment, zobrazuje název domény hello hello domény, které zadáte. Nová položka se zobrazí v **nastavení** nabídky s názvem **ILB certifikát**. Hello App Service Environment se vytvoří certifikát, který není zadejte doménu App Service Environment ILB hello. Pokud používáte hello App Service Environment pomocí certifikátu, prohlížeč informuje, že je neplatný. Tento certifikát umožňuje snazší tootest HTTPS, ale potřebujete tooupload vlastní certifikát, který je vázané tooyour App Service Environment ILB domény. Tento krok je nutný bez ohledu na to, jestli je vašeho certifikátu podepsaného svým držitelem nebo získali od certifikační autority.

![Název domény ILB App Service Environment][3]

Vaše ILB App Service Environment musí platný certifikát SSL. Použít interní certifikační úřady, koupit certifikát od externí vystavitele nebo použít certifikát podepsaný svým držitelem. Bez ohledu na to hello zdroje hello certifikátu protokolu SSL hello následující atributy certifikát potřebovat toobe správně nakonfigurována:

* **Předmět**: Tento atribut lze nastavit too*.your kořenové domény sem.
* **Alternativní název subjektu**: Tento atribut musí obsahovat i **.your kořenové domény sem* a **.scm.your-kořenové-domény-zde*. Toohello připojení SSL SCM/Kudu lokality spojené s každou aplikaci použít adresu ve tvaru hello *your-app-name.scm.your-root-domain-here*.

Převést nebo uložení certifikátu SSL hello jako soubor .pfx. soubor .pfx Hello musí obsahovat všechny zprostředkující a kořenové certifikáty. Zabezpečte ji pomocí hesla.

Pokud chcete toocreate certifikát podepsaný svým držitelem, můžete použít příkazy prostředí PowerShell hello sem. Zda toouse být název domény App Service Environment ILB místo *internal.contoso.com*: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

Hello certifikát, který tyto příkazy prostředí PowerShell generovat označen příznakem pomocí prohlížeče, protože hello certifikát certifikační autority, která je v prohlížeči řetězu certifikátů nebyl vytvořen. tooget certifikát, který váš prohlížeč vztahy důvěryhodnosti, ji zakoupit od komerční certifikační autority v prohlížeči řetěz certifikátů. 

![Nastavte ILB certifikát][4]

tooupload vlastní certifikáty a testovací přístup:

1. Po vytvoření App Service Environment hello přejděte toohello App Service Environment uživatelského rozhraní. Vyberte **App Service Environment** > **nastavení** > **ILB certifikát**.

2. tooset hello ILB certifikát, vyberte soubor certifikátu .pfx hello a zadejte heslo hello. Tento krok provede některé tooprocess čas. Zobrazí se zpráva, že operace nahrávání probíhá.

3. Získáte hello ILB adresu pro vaše App Service Environment. Vyberte **App Service Environment** > **vlastnosti** > **virtuální IP adresa**.

4. Vytvoření webové aplikace ve vašem App Service Environment, po vytvoření hello App Service Environment.

5. Vytvoření virtuálního počítače, pokud nemáte v této virtuální sítě.

    > [!NOTE] 
    > Nepokoušejte toocreate tento virtuální počítač v hello stejné podsíti jako hello App Service Environment, protože se nezdaří nebo způsobit problémy.
    >
    >

6. Nastavte hello DNS pro doménu služby App Service Environment. Můžete použít zástupný znak s doménou v DNS. testy toodo některé jednoduchý, upravit soubor hostitelů hello na toohello aplikace název vašeho virtuálního počítače tooset hello webové VIP IP adresu:

    a. Pokud vaše App Service Environment má název domény hello _. ilbase.com_ a vytvoříte hello webovou aplikaci s názvem _mytestapp_, je určena v _mytestapp.ilbase.com_. Pak můžete nastavit _mytestapp.ilbase.com_ tooresolve toohello ILB adresu. (V systému Windows, je hello souboru hostitelů na _C:\Windows\System32\drivers\etc\_.)

    b. tootest webové nasazení publikování nebo přístup toohello, pokročilé konzoly, vytvořte záznam pro _mytestapp.scm.ilbase.com_.

7. Použít prohlížeč na tento virtuální počítač a přejděte k http://mytestapp.ilbase.com. (Nebo přejděte toowhatever, je název vaší webové aplikace s vaší domény.)

8. Použít prohlížeč na tento virtuální počítač a přejděte k https://mytestapp.ilbase.com. Pokud používáte certifikát podepsaný svým držitelem, přijměte hello nedostatečná zabezpečení.

    Hello IP adresu pro vaše ILB je uveden v části **IP adresy**. Tento seznam má také používaných hello externí virtuální IP adresy a pro přenos pro správu příchozích hello IP adresy.

    ![ILB IP adresa][5]

### <a name="web-jobs-functions-and-hello-ilb-ase"></a>Webové úlohy, funkce a hello ILB App Service Environment

Funkce a webové úlohy jsou podporovány v App Service Environment ILB, ale pro hello portálu toowork s nimi, je nutné mít přístup toohello SCM síť společnosti.  To znamená, že váš prohlížeč musí být buď na hostiteli, který je buď v nebo toohello virtuální sítě.  

Pokud používáte Azure Functions v App Service Environment ILB, může získat chybovou zprávu, která říká "jsme nejsou možné tooretrieve funkce se teď. Zkuste to prosím znovu později." K této chybě dojde, protože hello uživatelského rozhraní funkce využívá hello SCM lokality pomocí protokolu HTTPS a není hello kořenový certifikát v řetězu prohlížeče hello vztahu důvěryhodnosti. Webové úlohy je podobný problém. tooavoid tohoto problému můžete provést jednu z následujících akcí hello:

- Přidejte hello certifikát tooyour důvěryhodného úložiště certifikátů. To odblokuje okraj a prohlížeče Internet Explorer.
- Použijte Chrome a nejprve přejděte toohello SCM lokality, přijměte hello nedůvěryhodný certifikát a potom přejděte toohello portálu.
- Použijte komerční certifikát, který je v řetězu certifikátů váš prohlížeč.  Toto je nejlepší možnost hello.  

## <a name="dns-configuration"></a>Konfigurace služby DNS ##

Pokud používáte externí VIP, hello DNS spravuje Azure. Všechny aplikace vytvořená v vaší App Service Environment se automaticky přidá tooAzure DNS, který je veřejném DNS. ILB App Service Environment musí spravovat vlastní DNS. Pro danou doménu, jako _contoso.net_, je nutné toocreate DNS A zaznamenává v DNS tuto adresu ILB tooyour bodu pro:

- *. contoso.net
- *. scm.contoso.net

Pokud vaše doména ILB App Service Environment se používá pro několik věcí mimo tento App Service Environment, bude pravděpodobně nutné toomanage DNS na základě názvu na aplikaci. Tato metoda je náročné, protože potřebujete tooadd každý nový název aplikace do DNS při jeho vytvoření. Z tohoto důvodu doporučujeme používat vyhrazené domény.

## <a name="publish-with-an-ilb-ase"></a>Publikování s ILB App Service Environment ##

Pro každou aplikaci, která je vytvořena jsou dva koncové body. V App Service Environment ILB, máte  *&lt;název aplikace >.&lt; App Service Environment domény ILB >* a  *&lt;název aplikace > .scm.&lt; App Service Environment domény ILB >*. 

Název lokality SCM Hello přejdete konzole Kudu toohello názvem hello **pokročilé portál**, uvnitř hello portálu Azure. Konzola Kudu Hello umožňuje zobrazení proměnné prostředí, prozkoumejte hello disku, použijte konzolu a mnoho dalšího. Další informace najdete v tématu [konzoly Kudu pro službu Azure App Service][Kudu]. 

V hello víceklientské služby App Service a externí App Service Environment není jednotné přihlašování mezi hello portál Azure a hello Kudu konzoly. Hello App Service Environment ILB ale musíte toouse vaše publikování toosign přihlašovací údaje do konzoly Kudu hello.

Internetové systémy položek konfigurace, například Githubu a Visual Studio Team Services, nefungují s ILB App Service Environment, protože hello publikování koncový bod není dostupný na Internetu. Místo toho musíte toouse CI systému, který používá model pull, jako je Dropbox.

Hello publikování koncové body pro aplikace v App Service Environment ILB použít doménu hello této hello, který byl vytvořen ILB App Service Environment. Tuto doménu se zobrazí v profilu publikování hello aplikace a v okně portálu aplikace hello (**přehled** > **Essentials** a také **vlastnosti**). Pokud máte App Service Environment ILB s hello subdomény *contoso.net* a aplikaci s názvem *test*, použijte *mytest.contoso.net* pro FTP a *mytest.scm.contoso.net*  pro nasazení webu.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Spojte ILB App Service Environment se zařízením firewall webových aplikací ##

Aplikační služba Azure poskytuje mnoho bezpečnostní opatření, které chrání hello systému. Umožňují taky toodetermine, zda se hacker aplikace. Hello nejlepší ochrany pro webovou aplikaci je toocouple hostující platforma, jako je například Azure App Service pomocí brány firewall webových aplikací (firewall webových aplikací). Protože hello App Service Environment ILB má koncový bod sítě izolované aplikace, je vhodné pro toto použití.

Další informace o tom, jak tooconfigure App Service Environment vaší ILB s zařízením firewall webových aplikací najdete v části toolearn [konfigurace brány firewall webových aplikací pomocí služby App Service environment][ASEWAF]. Tento článek ukazuje, jak toouse Barracuda virtuální zařízení s vaší App Service Environment. Další možností je toouse Azure Application Gateway. Aplikační brána používá hello OWASP základní pravidla toosecure všechny aplikace se umístí za ho. Další informace o Application Gateway najdete v tématu [brány firewall Úvod toohello Azure webových aplikací][AppGW].

## <a name="get-started"></a>Začínáme ##

Všechny články a jak tooinstructions pro ASEs jsou k dispozici v [soubor README pro služby App Service Environment][ASEReadme].

* tooget začít s ASEs, najdete v části [prostředí Service tooApp ÚVOD][Intro].
* Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

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
