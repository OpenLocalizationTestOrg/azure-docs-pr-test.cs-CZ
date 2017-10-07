---
title: "aaaCreating a interní pro vyrovnávání zatížení pomocí služby App Service Environment | Microsoft Docs"
description: "Vytvoření a použití s ILB App Service Environment"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 20799f260993d6e81499408e5b547a2e21430174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Interní nástroj pomocí služby App Service Environment

> [!NOTE] 
> Tento článek je o hello App Service Environment v1. Není k dispozici novější verze hello služby App Service Environment, je snazší toouse a běží na výkonnější infrastruktury. Další informace o nové verzi hello začínat hello toolearn [toohello Úvod App Service Environment](../app-service/app-service-environment/intro.md).
>

Funkce prostředí App Service (App Service Environment) Hello je možnost Premium služby Azure App Service, který doručí funkci rozšířené konfigurace, která není k dispozici v hello víceklientské razítka. Funkce App Service Environment Hello v podstatě nasadí hello Azure App Service ve vaší virtuální Network(VNet) Azure. toogain lépe porozumět hello možnosti nabízí prostředí App Service číst hello [novinky služby App Service Environment] [ WhatisASE] dokumentaci. Pokud si nejste jisti, výhod hello provoz ve virtuální síti číst hello [Azure virtuální sítě, – nejčastější dotazy][virtualnetwork]. 

## <a name="overview"></a>Přehled
App Service Environment se dá nasadit pomocí Internetu přístupném koncovém bodu nebo IP adresu ve vaší virtuální síti. Pořadí tooset hello IP adresu tooa adres virtuální sítě je nutné toodeploy vaše App Service Environment se vnitřní Balancer(ILB) zatížení. Pokud vaše App Service Environment je konfigurován s ILB zadáte:

* vaše vlastní doména nebo subdoména. toomake ho snadno, že tento dokument předpokládá subdomény ale můžete jej nakonfigurovat v obou případech. 
* Hello certifikát používaný pro protokol HTTPS
* Správa služby DNS pro vaše subdomény. 

Naopak může provádět akce, jako:

* aplikace v síti intranet hostitele, jako je obchodních aplikací, bezpečně v hello cloudu které přístup prostřednictvím tooSite lokality nebo ExpressRoute VPN
* aplikace pro hostitele v cloudu hello, které nejsou uvedené v veřejné servery DNS
* Vytvoření internet izolované back-end aplikace, které vaše aplikace front-endu může bezpečně integrovat

#### <a name="disabled-functionality"></a>Zakázané funkce
Existují některé věci, které nejde dělat, když pomocí ILB App Service Environment. Zahrnout všechny tyto věci:

* pomocí IPSSL
* přiřazení IP adres toospecific aplikace
* nakupovat a certifikát pomocí aplikace prostřednictvím portálu hello. Samozřejmě stále můžete získat certifikáty přímo s certifikační autority a jeho použití s aplikací, ne jenom prostřednictvím hello portálu Azure.

## <a name="creating-an-ilb-ase"></a>Vytváření ILB App Service Environment
Vytváření ILB App Service Environment není výrazně liší od vytvoření App Service Environment normálně. Podrobnější informace o vytvoření App Service Environment číst [jak tooCreate služby App Service Environment][HowtoCreateASE]. proces toocreate Hello, App Service Environment ILB je hello stejné mezi vytvoření virtuální sítě během vytváření App Service Environment nebo vyberte existující virtuální síť. toocreate ILB App Service Environment: 

1. V portálu Azure vyberte hello **nový -> Web + mobilní zařízení -> Služba App Service Environment**
2. Vyberte předplatné
3. Vyberte nebo vytvořte skupinu prostředků.
4. Vyberte nebo vytvořte virtuální síť
5. Pokud vyberete virtuální síť vytvořit podsíť
6. Vyberte **virtuální sítě nebo umístění-> Konfigurace virtuální sítě** a sadu hello tooInternal VIP typu
7. Zadejte název subdomény (to se stane subdomény hello používá pro aplikace vytvořené v této App Service Environment)
8. Kliknutím na tlačítko Ok a poté vytvořit

![][1]

V okně hello virtuální sítě je možnost konfigurace virtuální sítě. Díky tomu se můžete vybrat, jestli chcete externí virtuální IP adresy nebo interní virtuální IP adresy. Výchozí hodnota Hello je externí. Pokud máte nastavené tooExternal vaše App Service Environment bude používat Internetu dostupné virtuální IP adresy. Pokud vyberete interní, vaše App Service Environment bude nakonfigurován s ILB na IP adresu ve virtuální síti. 

Po výběru interní, hello možnost tooadd více IP adres tooyour App Service Environment je odebrána a místo toho je třeba tooprovide hello subdoména hello App Service Environment. V App Service Environment se hello externí virtuální IP adresy se používá název App Service Environment hello v hello subdomény pro aplikace vytvořené v této App Service Environment. Pokud vaše App Service Environment byla volána ***contosotest*** a aplikace v rámci této App Service Environment byla volána ***test*** hello subdomény by být ve formátu hello ***contosotest.p.azurewebsites.net*** a Dobrý den pro tuto aplikaci by měla adresa URL ***mytest.contosotest.p.azurewebsites.net***. Pokud jste nastavili hello tooInternal typ VIP, vaše jméno App Service Environment není používá v hello subdomény pro hello App Service Environment. Zadejte hello subdomény explicitně. Pokud byl váš subdomény ***contoso.corp.net*** a k aplikaci v tom, že s názvem App Service Environment ***timereporting*** pak hello adresu URL pro tuto aplikaci by ***timereporting.contoso.corp.net***.

## <a name="apps-in-an-ilb-ase"></a>Aplikace v App Service Environment ILB
Vytvoření aplikace v App Service Environment ILB je hello stejné jako za normálních okolností vytváření aplikace v App Service Environment. 

1. V portálu Azure vyberte hello **nový -> Web + mobilní zařízení -> Web** nebo **Mobile** nebo **aplikace API**
2. Zadejte název aplikace
3. Výběr předplatného
4. Vyberte nebo vytvořte skupinu prostředků
5. Vyberte nebo vytvořte Plan(ASP) aplikace služby. Vytvoření nové ASP pak vyberte vaše App Service Environment jako hello umístění a fondu pracovních procesů vyberte hello chcete vaší ASP toobe vytvořené v. Když vytvoříte hello ASP vyberete jako hello umístění a fondu pracovních procesů hello vaší App Service Environment. Když zadáte název hello hello aplikace, které uvidíte, že hello subdomény v části název vaší aplikace je nahrazena hello subdomény pro vaše App Service Environment. 
6. Vyberte možnost vytvořit. Měli byste vybrat hello **Pin toodashboard** zaškrtávací políčko, pokud chcete tooshow aplikace hello až na řídicím panelu. 

![][2]

V části hello aplikace získá název subdomény hello aktualizované tooreflect hello subdoména vaší App Service Environment. 

## <a name="post-ilb-ase-creation-validation"></a>Ověření vytvoření POST ILB App Service Environment
App Service Environment ILB je trochu jiná než hello jiný - ILB App Service Environment. Jak již poznamenat, budete muset toomanage vlastní DNS a můžete si taky tooprovide svůj vlastní certifikát pro připojení prostřednictvím protokolu HTTPS. 

Po vytvoření vaší App Service Environment si všimnete, že hello subdomény ukazuje hello subdoménu jste zadali a v hello se nová položka **nastavení** nabídky s názvem **ILB certifikát**. Hello App Service Environment se vytvoří s certifikát podepsaný svým držitelem, takže je jednodušší tootest HTTPS. Hello portál vám oznámí, že budete tooprovide potřebovat vlastní certifikát pro protokol HTTPS, ale toto je toodrive toohave certifikát, který prochází s vaší subdomény. 

![][3]

Pokud jsou jednoduše vyzkoušení věcí a neznáte, jak toocreate certifikát, můžete použít hello konzoly MMC služby IIS toocreate aplikace konzoly a vlastní certifikát podepsaný. Po vytvoření můžete ho exportovat jako soubor .pfx a nahrajte ho v hello uživatelské rozhraní konektoru Certificate ILB. Při přístupu lokality zabezpečen certifikát podepsaný svým držitelem, prohlížeč získáte upozornění, které hello lokality, ke které přistupujete není zabezpečený kvůli toohello nemohou toovalidate hello certifikát. Pokud chcete tooavoid, že upozornění musíte správně podepsaný certifikát, který odpovídá vaší subdomény a má řetěz certifikátů, který váš prohlížeč rozpozná.

![][6]

Pokud chcete tootry hello toku s vlastní certifikáty a otestovat HTTP a HTTPS přístup tooyour App Service Environment:

1. Po vytvoření App Service Environment přejděte tooASE uživatelského rozhraní **App Service Environment -> Nastavení -> ILB certifikáty**
2. Nastavte ILB certifikát tak, že vyberete soubor pfx certifikátu a zadejte heslo. Tento krok vezme trochu při tooprocess a zobrazí uvítací zprávu, která škálování operace probíhá.
3. Získat hello ILB adresu pro vaše App Service Environment (**App Service Environment -> Vlastnosti -> virtuální IP adresy**)
4. Vytvoření webové aplikace v App Service Environment po vytvoření 
5. Vytvoření virtuálního počítače, pokud nemáte v ní (hello není ve stejné podsíti jako hello App Service Environment nebo přerušení věcí)
6. Nastavení DNS pro vaše subdomény. Můžete použít zástupný znak s vaší subdomény v DNS nebo pokud chcete toodo několik jednoduchých testů, upravte soubor hostitelů hello na vaše tooset webové aplikace název tooVIP IP adresy virtuálních počítačů. Pokud vaše App Service Environment měli název subdomény hello. ilbase.com a můžete provedeny hello mytestapp webové aplikace, aby by se řešit na mytestapp.ilbase.com a nastavte ho v souboru hostitelů. (V systému Windows hello soubor hostitele je v C:\Windows\System32\drivers\etc\)
7. Použít prohlížeč na tento virtuální počítač a přejděte toohttp://mytestapp.ilbase.com (nebo ať je název vaší webové aplikace s vaší subdomény)
8. Použít prohlížeč na tento virtuální počítač a přejděte toohttps://mytestapp.ilbase.com bude mít nedostatek hello tooaccept zabezpečení, pokud používáte certifikát podepsaný svým držitelem. 

Hello IP adresu pro vaše ILB je uveden ve vlastnostech jako hello virtuální IP adresy

![][4]

## <a name="using-an-ilb-ase"></a>Pomocí ILB App Service Environment
#### <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě)
Izolace sítě umožňuje pro vaše aplikace app Service Environment ILB na jako aplikace hello nejsou přístupné nebo i známých podle hello Internetu. Toto je vynikající pro hostování intranetových serverů, například obchodních aplikací. Když potřebujete přístup toorestrict i další stále můžete Groups(NSGs) zabezpečení sítě toocontrol přístupu na úrovni sítě hello. 

Pokud chcete toouse skupiny Nsg toofurther omezit přístup pak je nutné se, že nedojde k narušení komunikace hello toomake této hello App Service Environment musí v toooperate pořadí. I když hello přístup protokolu HTTP nebo HTTPS pouze prostřednictvím hello ILB používá hello hello App Service Environment, App Service Environment se stále závisí na prostředku mimo hello virtuální sítě. Podívejte se na hello informace v dokumentu hello toosee, jaký přístup k síti je pořád potřebuje na [tooan řízení příchozí provoz služby App Service Environment] [ ControlInbound] a hello dokumentu [sítě Podrobnosti konfigurace pro služby App Service Environment s ExpressRoute][ExpressRoute]. 

tooconfigure, které skupin Nsg je nutné tooknow hello IP adres, který je používán Azure toomanage vaše App Service Environment. Tuto IP adresu je také hello odchozí IP adresu z vaší App Service Environment, pokud se zadá žádosti internetových. Hello odchozí IP adresu pro vaše App Service Environment bude stále statickou hello dobu životnosti vaší App Service Environment. Pokud odstraníte a znovu vytvořte vaše App Service Environment, obdržíte novou IP adresu. Tato IP adresa přejděte příliš toofind**Nastavení -> vlastnosti** a najde hello **odchozí IP adresu**. 

![][5]

#### <a name="general-ilb-ase-management"></a>Správa obecné ILB App Service Environment
Správa App Service Environment ILB je hello z velké části stejný jako správa App Service Environment normálně. Potřebujete tooscale si vaše pracovní fondy toohost více instancí ASP a škálovat vaše Front-endu servery toohandle vyšší objemy přenosy HTTP/HTTPS. Obecné informace o správě hello konfiguraci App Service Environment, přečtěte si dokument hello [konfigurace služby App Service Environment][ASEConfig]. 

Hello další správu položky jsou správy certifikátů a správu DNS. Třeba tooobtain a nahrát hello certifikát používaný pro protokol HTTPS po vytvoření ILB App Service Environment a nahraďte ji, než vyprší její platnost. Protože Azure vlastní hello základní doménu můžete poskytujeme certifikáty ASEs externí VIP. Vzhledem k tomu, že hello subdomény používané ILB App Service Environment může být jakýkoli, budete potřebovat tooprovide vlastní certifikát pro protokol HTTPS. 

#### <a name="dns-configuration"></a>Konfigurace služby DNS
Při použití hello externí virtuální IP adresy, které DNS spravuje Azure. Všechny aplikace vytvořená v vaší App Service Environment se automaticky přidá tooAzure DNS, který je veřejném DNS. V App Service Environment ILB toomanage máte vlastní DNS. Pro danou subdoména například contoso.corp.net potřebujete toocreate, DNS A zaznamenává tuto adresu ILB tooyour bodu pro:

    * 
    publikování *.SCM ftp 


## <a name="getting-started"></a>Začínáme
Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).

tooget začít s prostředí App Service najdete v části [Úvod tooApp prostředí Service][WhatisASE]

Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
