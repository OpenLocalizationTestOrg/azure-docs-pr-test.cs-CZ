---
title: "aspekty aaaNetworking pomocí služby Azure App Service environment"
description: "Vysvětluje hello App Service Environment síťový provoz a jak tooset skupiny Nsg a udr s vaší App Service Environment"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ccompy
ms.openlocfilehash: d4d3000f4d4d75814b1e6d47079d967334eb1a3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="networking-considerations-for-an-app-service-environment"></a>Aspekty sítě služby App Service environment #

## <a name="overview"></a>Přehled ##

 Azure [App Service Environment] [ Intro] nasazení služby Azure App Service na podsíť ve virtuální síti Azure (VNet). Existují dva typy nasazení pro služby App Service environment (App Service Environment):

- **Externí App Service Environment**: zpřístupňuje hello aplikace app Service Environment hostované na IP adresu přístupné z Internetu. Další informace najdete v tématu [vytvořit externí App Service Environment][MakeExternalASE].
- **App Service Environment ILB**: zpřístupňuje hello aplikace app Service Environment hostované na IP adresu ve virtuální síti. vnitřní koncový bod Hello je interní nástroj (ILB), proto volal ILB App Service Environment. Další informace najdete v tématu [vytvoření a použití App Service Environment ILB][MakeILBASE].

Existují dvě verze služby App Service Environment: ASEv1 a ASEv2. Informace o ASEv1 najdete v tématu [Úvod tooApp Service Environment v1][ASEv1Intro]. ASEv1 se dá nasadit v klasický nebo virtuální sítě Resource Manageru. ASEv2 lze nasadit pouze do virtuální sítě Resource Manageru.

Všechna volání ze App Service Environment, které přejděte toohello internet ponechte hello virtuální sítě pomocí virtuální IP adresu přiřadit pro hello App Service Environment. Hello veřejnou IP adresu z této VIP je pak hello zdrojové IP adresy pro všechna volání z hello App Service Environment, které přejděte toohello Internetu. Pokud aplikace hello ve vašem App Service Environment umožňují volání tooresources ve vaší virtuální síti nebo přes síť VPN, je jedním z hello IP adres v podsíti hello používá vaše App Service Environment hello zdrojové IP adresy. Protože hello App Service Environment není v rámci hello virtuální sítě, je také přístup k prostředkům v rámci hello virtuální sítě bez jakékoli dodatečné konfigurace. Pokud hello virtuální sítě je připojený tooyour do místní sítě, aplikací ve vaší App Service Environment mít také přístup tooresources existuje. Nepotřebujete tooconfigure hello App Service Environment nebo aplikace pro všechny další.

![Externí App Service Environment][1] 

Pokud máte externí App Service Environment, veřejné VIP hello je také hello koncový bod, že vaše aplikace app Service Environment přeložit toofor:

* HTTP/S. 
* FTP/S. 
* Nasazení webu.
* Vzdálené ladění.

![ILB APP SERVICE ENVIRONMENT][2]

Pokud máte App Service Environment ILB, adresa IP hello hello ILB je hello koncový bod HTTP/S, FTP/S, nasazení webu a vzdálené ladění.

Hello normální aplikace přístupové porty jsou:

| Použití | Z | příliš|
|----------|---------|-------------|
|  PROTOKOL HTTP NEBO HTTPS  | Konfigurovatelná uživatelem |  80, 443 |
|  FTP/FTPS    | Konfigurovatelná uživatelem |  21, 990, 10001-10020 |
|  Visual Studio vzdálené ladění  |  Konfigurovatelná uživatelem |  4016, 4018, 4020, 4022 |

To platí, pokud jste na externí App Service Environment nebo ILB App Service Environment. Pokud jste na externí App Service Environment, stiskněte tlačítko tyto porty na veřejné VIP hello. Pokud jste v App Service Environment ILB, stiskněte tlačítko tyto porty na hello ILB. Pokud se uzamknout port 443, může být dopad na některé funkce zveřejněné hello portálu. Další informace najdete v tématu [portál závislosti](#portaldep).

## <a name="ase-dependencies"></a>Závislosti App Service Environment ##

App Service Environment příchozí přístup závislostí je:

| Použití | Z | příliš|
|-----|------|----|
| Správa | Adresy pro správu služby aplikace | Podsíť App Service Environment: 454, 455 |
|  Interní komunikaci App Service Environment | Podsíť App Service Environment: všechny porty | Podsíť App Service Environment: všechny porty
|  Povolit pro vyrovnávání zatížení Azure příchozí | Nástroje pro vyrovnávání zatížení Azure | Podsíť App Service Environment: všechny porty
|  Aplikace přiřazené IP adresy | Aplikace, které jsou přiřazeny adresy | Podsíť App Service Environment: všechny porty

Hello příchozí provoz poskytuje příkazy a ovládání App Service Environment hello v přidání toosystem monitorování. Zdroj Hello IP adres pro tyto přenosy dat jsou uvedené v hello [adresy App Service Environment správu] [ ASEManagement] dokumentu. Konfigurace zabezpečení sítě Hello potřebuje přístup tooallow z všechny IP adresy na portech 454 a 455.

V rámci podsítě hello App Service Environment jsou mnoho porty se použijí pro komunikaci interní součásti a můžou změnit.  To vyžaduje všechny porty hello v toobe podsíť hello App Service Environment dostupný z podsítě hello App Service Environment. 

Pro hello komunikaci mezi pro vyrovnávání zatížení Azure hello a hello App Service Environment podsíť hello minimální porty jsou tohoto toobe potřeba otevřít 454 a 455 16001. Hello 16001 port je používán pro udržování připojení provoz mezi hello nástroj pro vyrovnávání zatížení a hello App Service Environment. Pokud používáte App Service Environment ILB můžete uzamknout provoz směrem dolů toojust hello 454, 455, 16001 porty.  Pokud používáte externí App Service Environment bude nutné tootake do účtu hello normální aplikace přístupové porty.  Pokud používáte aplikaci přiřazenou adresy je třeba tooopen ho tooall porty.  Pokud adresu přiřazen tooa konkrétní aplikaci, nástroj pro vyrovnávání zatížení hello použije porty, které nejsou známá předem toosend HTTP a HTTPS provoz toohello App Service Environment.

Pokud používáte aplikaci přiřazenou IP adresy musíte tooallow provoz z hello přiřazené tooyour aplikace toohello App Service Environment podsítě IP adres.

Pro odchozí přístup App Service Environment závisí na několika externími systémy. Tyto závislosti systému jsou definovány s názvy DNS a nemáte mapování tooa pevné sadu IP adres. Proto hello App Service Environment vyžaduje odchozí přístup z podsítě tooall hello App Service Environment externí IP adres napříč různými porty. App Service Environment má hello následující odchozí závislosti:

| Použití | Z | příliš|
|-----|------|----|
| Azure Storage | Podsíť App Service Environment | Table.Core.Windows.NET, blob.core.windows.net, queue.core.windows.net, file.core.windows.net: 80, 443, 445 (445 je potřeba jenom pro ASEv1.) |
| Azure SQL Database | Podsíť App Service Environment | Database.Windows.NET: 1433, 11000 11999, 14000 14999 (Další informace najdete v tématu [využití portu SQL Database verze 12](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).)|
| Správa Azure | Podsíť App Service Environment | Management.Core.Windows.NET, management.azure.com: 443 
| Ověření certifikátu SSL |  Podsíť App Service Environment            |  OCSP.msocsp.com, mscrl.microsoft.com, crl.microsoft.com: 443
| Azure Active Directory        | Podsíť App Service Environment            |  Internet: 443
| Správa služby App Service        | Podsíť App Service Environment            |  Internet: 443
| Azure DNS                     | Podsíť App Service Environment            |  Internet: 53
| Interní komunikaci App Service Environment    | Podsíť App Service Environment: všechny porty |  Podsíť App Service Environment: všechny porty

Pokud hello App Service Environment ztratí přístup toothese závislosti, přestane fungovat. Pokud k tomu dojde dlouho dostatek, hello App Service Environment je pozastaven.

### <a name="customer-dns"></a>Zákazník DNS ###

Pokud hello virtuální síť je nakonfigurované na serveru DNS definovanou zákazníkem, hello klientské úlohy ji použít. Hello App Service Environment stále potřebuje toocommunicate s Azure DNS pro účely správy. 

Pokud hello virtuální síť je nakonfigurované zákazník DNS na hello druhé straně sítě VPN, musí být dostupný z podsítě hello, který obsahuje App Service Environment hello hello DNS server.

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portál závislosti ##

V přidání toohello App Service Environment funkční závislosti existuje několik prostředí portálu související toohello další položky. Některé z možností hello v hello portálu Azure, závisí na site_ too_SCM přímý přístup. Pro každou aplikaci ve službě Azure App Service existují dvě adresy URL. První adresa URL Hello je tooaccess vaší aplikace. Hello druhý adresa URL je tooaccess hello SCM webu, což se označuje taky jako hello _Kudu konzoly_. Funkce, které používají hello SCM lokality:

-   Webové úlohy
-   Funkce
-   Protokol streamování
-   Kudu
-   Rozšíření
-   Průzkumník procesů
-   Konzola

Při použití App Service Environment ILB hello SCM lokalita není dostupný z vnějšku hello virtuální sítě internet. Pokud vaše aplikace je hostitelem App Service Environment ILB, některé funkce nebudou fungovat z portálu hello.  

Řadu tyto funkce, které závisí na hello SCM lokality jsou k dispozici přímo v konzoli Kudu hello. Tooit můžete připojit přímo spíše než pomocí portálu hello. Pokud je vaše aplikace hostovaná v App Service Environment ILB, pomocí vaší publikování toosign přihlašovací údaje v. Hello URL tooaccess hello SCM lokality aplikace hostované v App Service Environment ILB má hello následující formát: 

```
<appname>.scm.<domain name hello ILB ASE was created with> 
```

Pokud vaše App Service Environment ILB je název domény hello *contoso.net* a je název vaší aplikace *testapp*, hello aplikace je k dispozici na adrese *testapp.contoso.net*. Hello SCM webu, která jde s ním je k dispozici na adrese *testapp.scm.contoso.net*.

### <a name="functions-and-web-jobs"></a>Funkce a webové úlohy ###

Funkce a webové úlohy závisí na hello SCM lokality, ale jsou podporovány pro použití v portálu hello i v případě, že vaše aplikace nacházely ve ILB App Service Environment, dokud bude prohlížeč dosáhnout hello SCM lokality.  Pokud používáte certifikát podepsaný svým držitelem s vaší App Service Environment ILB, budete potřebovat tooenable tootrust váš prohlížeč, který certifikát.  Pro aplikaci Internet Explorer a okraj, který znamená hello certifikát má toobe v počítači důvěryhodnosti hello uložit.  Pokud používáte Chrome, pak to znamená, že jste dříve přijali hello certifikátu v prohlížeči hello zasažení pravděpodobně z důvodu hello scm lokality přímo.  Hello nejlepším řešením je toouse komerční certifikát, který je v řetězu prohlížeče hello vztahu důvěryhodnosti.  

## <a name="ase-ip-addresses"></a>App Service Environment IP adresy ##

App Service Environment má několik adres IP toobe vědět. Jsou:

- **Veřejná IP adresa příchozí**: použít pro provoz aplikace v externím App Service Environment a přenosy správy v App Service Environment ILB i externí App Service Environment.
- **Odchozí veřejnou IP adresu**: použít jako hello "z" IP adresy pro odchozí připojení z hello App Service Environment této ponechejte hello virtuální sítě, které nejsou směrovány dolů sítě VPN.
- **ILB IP adresu**: Pokud používáte ILB App Service Environment.
- **Přiřazené aplikace založená na IP adresy**: jedinou možnou s externí App Service Environment a je nakonfigurovaný na základě IP protokol SSL.

Tyto IP adresy lze snadno zobrazit v ASEv2 v hello portál Azure z hello App Service Environment uživatelského rozhraní. Pokud máte App Service Environment ILB, je uvedený hello IP pro hello ILB.

![IP adresy][3]

### <a name="app-assigned-ip-addresses"></a>Aplikace přiřazené IP adresy ###

Externí App Service Environment můžete přiřadit IP adresy tooindividual aplikace. Nejde udělat s ILB App Service Environment. Další informace o tom, tooconfigure vaší aplikace toohave vlastní IP adresu, najdete v části [vytvořit vazbu existující vlastní SSL certifikát tooAzure webové aplikace](../../app-service-web/app-service-web-tutorial-custom-ssl.md).

Pokud aplikace má vlastní založená na IP adresu, vyhrazuje hello App Service Environment dva porty toomap toothat IP adresu. Je jeden port pro přenos HTTP, a hello jiných port se pro protokol HTTPS. Tyto porty jsou uvedeny v hello App Service Environment uživatelského rozhraní v části adresy IP hello. Přenosy musí být schopný tooreach tyto porty z hello VIP nebo hello aplikace jsou nedostupné. Tento požadavek je důležité tooremember při konfiguraci skupin zabezpečení sítě (Nsg).

## <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě) ##

[Skupin zabezpečení sítě] [ NSGs] zadejte hello možnost toocontrol přístup k síti v rámci virtuální sítě. Pokud používáte hello portál, je implicitní zakázat pravidlo v hello nejnižší priorita toodeny vše. Můžete vytvořit jsou vaše povolí pravidla.

V App Service Environment nemáte přístup toohello virtuálních počítačů použít toohost hello App Service Environment sám sebe. Jsou v předplatném spravovaný společností Microsoft. Pokud chcete aplikace toohello toorestrict přístup na hello App Service Environment, nastavte skupiny Nsg na podsítě hello App Service Environment. Při tom věnujte pozornost pečlivě toohello App Service Environment závislosti. Pokud zablokujete všechny závislosti, hello App Service Environment přestane fungovat.

Skupiny Nsg se dá nakonfigurovat pomocí hello portál Azure nebo pomocí prostředí PowerShell. Zde Hello informace zobrazí hello portálu Azure. Vytvořit a spravovat skupiny Nsg hello portálu jako prostředek nejvyšší úrovně v rámci **sítě**.

Když hello příchozí a odchozí požadavky jsou vzít v úvahu, hello skupiny Nsg by měla vypadat podobně jako skupiny Nsg toohello uvedeno v tomto příkladu. Hello rozsah adres virtuální sítě je _192.168.250.0/16_, a je hello podsítě, která se App Service Environment hello _192.168.251.128/25_.

první dva příchozí požadavky pro App Service Environment toofunction hello Hello se zobrazí v horní části hello hello seznamu v tomto příkladu. Jejich umožňuje správu App Service Environment a hello App Service Environment toocommunicate sama se sebou. Hello další položky jsou všechny konfigurovatelné klienta a můžou řídit síťový přístup toohello App Service Environment webové aplikace. 

![Příchozí pravidla zabezpečení][4]

Výchozí pravidlo umožňuje hello IP adres v podsíti, hello virtuální síť tootalk toohello App Service Environment. Jiné výchozí pravidlo umožňuje hello Vyrovnávání zatížení, také známé jako veřejné VIP hello, toocommunicate s hello App Service Environment. toosee hello výchozí pravidla, vyberte **výchozí pravidla** další toohello **přidat** ikonu. Když vložíte odepření všechno ostatní pravidla po hello NSG pravidla uvedené, abyste zabránili provoz mezi hello VIP a hello App Service Environment. tooprevent provoz, který přichází z uvnitř hello sítě VNet, přidat vlastní pravidlo tooallow příchozí. Použijte stejné tooAzureLoadBalancer zdroj s cílovým serverem **žádné** a rozsah portů ** \* **. Pravidla NSG hello je použité toohello App Service Environment podsítě, a proto není nutné toobe konkrétní v cílovém umístění hello.

Pokud jste přiřadili aplikace tooyour IP adresu, zkontrolujte, zda že zachovat hello porty brány. Vyberte toosee hello porty, **App Service Environment** > **IP adresy**.  

Všechny položky hello ukazuje hello následující odchozích pravidel, je potřeba, s výjimkou hello poslední položky. Umožňují síťový přístup toohello App Service Environment závislosti, které jste si poznamenali dříve v tomto článku. Pokud některý z nich zablokujete, vaše App Service Environment přestane fungovat. Hello poslední položky v seznamu hello umožňuje vaší App Service Environment toocommunicate s další prostředky ve virtuální síti.

![Odchozí pravidla zabezpečení][5]

Po skupin Nsg jsou definovány, přiřaďte jim toohello podsítě, který vaše App Service Environment je na. Pokud si nepamatujete hello App Service Environment virtuální síť nebo podsíť, zobrazí se z portálu pro správu hello App Service Environment. tooassign hello NSG tooyour podsíť, přejděte toohello podsíť uživatelského rozhraní a vyberte hello NSG.

## <a name="routes"></a>Trasy ##

Trasy stát nejčastěji problematické, pokud konfigurujete virtuální síť s Azure ExpressRoute. Existují tři typy tras ve virtuální síti:

-   Systémové trasy
-   Trasy protokolu BGP
-   Trasy definované uživatelem (udr)

Trasy protokolu BGP přepsat systémové trasy. Udr přepsat trasy protokolu BGP. Další informace o trasách v Azure virtuální sítě najdete v tématu [trasy definované uživatelem přehled][UDRs].

databáze Azure SQL Hello že App Service Environment hello používá systém hello toomanage má bránu firewall. To vyžaduje toooriginate komunikace z hello veřejné VIP App Service Environment. Databáze SQL toohello připojení z hello App Service Environment bude odepřen, pokud jsou odesílány dolů hello připojení ExpressRoute a na jinou IP adresu.

Pokud požadavky na správu tooincoming odpovědi odesílá dolů hello ExpressRoute, se liší od původního cíle hello hello zpáteční adresu. Tato neshoda dělí komunikaci TCP hello.

Pro vaše App Service Environment toowork při vaši virtuální síť je nakonfigurované ExpressRoute, toodo co nejjednodušší hello je:

-   Konfigurace ExpressRoute tooadvertise _0.0.0.0/0_. Ve výchozím nastavení, je vynutit tunely všechny odchozí přenosy na místě.
-   Vytvořte UDR. Použít toohello podsítě, která obsahuje App Service Environment hello s předponu adresy z _0.0.0.0/0_ a dalšího směrování typ _Internet_.

Pokud provedete tyto dvě změny, určené internetové komunikaci z podsítě hello App Service Environment není vynutit hello ExpressRoute a hello App Service Environment funguje. 

> [!IMPORTANT]
> Hello trasy definované v UDR musí být dost konkrétní tootake přednost přes všechny trasy inzerované konfigurace ExpressRoute hello. Hello předchozí příklad používá rozsah adres široký 0.0.0.0/0 hello. Se dá potenciálně omylem přepsat inzerování tras, které používají podrobnější rozsahy adres.
>
> Konfigurace ExpressRoute, které mezi Inzerovat trasy z hello partnerský vztah veřejné cesta toohello partnerský vztah privátní cesty ASEs nepodporuje. Konfigurace ExpressRoute s veřejné partnerské vztahy nakonfigurované dostávat inzerování trasy od Microsoftu. oznámení o inzerovaném programu Hello obsahovat velké sady rozsahů adres Microsoft Azure IP. Pokud rozsahy adres hello ohlášené mezi v cestě partnerský vztah privátní hello, jsou všechny odchozí síťových paketů z podsítě hello App Service Environment pro zákazníka tunelových tooa vynutit místní síťové infrastruktury. ASEs aktuálně nepodporuje tento tok sítě. Jedním z problémů toothis řešení je toostop mezi – reklamu trasy z hello partnerský vztah veřejné cesta toohello partnerský vztah privátní cesty.

toocreate UDR, postupujte takto:

1. Přejděte toohello portálu Azure. Vyberte **sítě** > **směrovacích tabulek**.

2. Vytvořit novou tabulku směrování v hello stejné oblasti jako virtuální síť.

3. V rámci tabulky tras uživatelského rozhraní, vyberte **trasy** > **přidat**.

4. Sada hello **typ dalšího směrování** příliš**Internet** a hello **předpona adresy** příliš**0.0.0.0/0**. Vyberte **Uložit**.

    Zobrazí se něco podobného jako hello následující:

    ![Funkční trasy][6]

5. Po vytvoření hello nové směrovací tabulku, přejděte toohello podsítě, která obsahuje vaše App Service Environment. Vyberte směrovací tabulku ze seznamu hello hello portálu. Po uložení změn hello, měli byste vidět pak hello skupiny Nsg a trasy zaznamenán s podsíť.

    ![Skupiny Nsg a trasy][7]

### <a name="deploy-into-existing-azure-virtual-networks-that-are-integrated-with-expressroute"></a>Nasazení do virtuální sítě Azure, které jsou integrované s ExpressRoute ###

toodeploy vaše App Service Environment do virtuální sítě, která je integrovaná s ExpressRoute, předem nakonfigurujte hello podsíť, kam chcete nasadit App Service Environment hello. Potom pomocí toodeploy šablony Resource Manageru ho. toocreate App Service Environment ve virtuální síti která již má nakonfigurované ExpressRoute:

- Vytvořte App Service Environment text podsíť toohost hello.

    > [!NOTE]
    > V podsíti hello ale hello App Service Environment může být nic jiného. Být jisti toochoose adresní prostor, který umožňuje růstem do budoucna. Nelze změnit, tato nastavení později. Doporučujeme velikost `/25` adresy 128.

- Vytvoření udr (například směrovací tabulky), jak je popsáno výše a nastavte ho na podsíť hello.
- Vytvořit App Service Environment hello pomocí šablony Resource Manageru, jak je popsáno v [vytvořit App Service Environment pomocí šablony Resource Manageru][MakeASEfromTemplate].

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png

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
[ASEManagement]: ./management-addresses.md
