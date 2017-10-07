---
title: "aaaAzure Traffic Manager – nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje odpovědi toofrequently kladené dotazy o Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: kumud
ms.openlocfilehash: 5530d03b55bbf49a52002841e281e2cf5819c2b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Traffic Manager nejčastější dotazy (FAQ)

## <a name="traffic-manager-basics"></a>Základy Traffic Manageru

### <a name="what-ip-address-does-traffic-manager-use"></a>Jaké IP adresu Traffic Manager používat?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje v hello úrovně DNS. Odešle odpovědí DNS toodirect klienti toohello příslušnou službu koncový bod. Klienti se potom připojují koncový bod služby toohello přímo, nejsou prostřednictvím Správce provozu.

Proto Traffic Manager neposkytuje na koncový bod nebo IP adresu pro klienty tooconnect k. Proto pokud chcete statickou IP adresu pro vaši službu, která je nutné nakonfigurovat v hello služby, není v Traffic Manageru.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Podporuje Traffic Manager "rychlé" relace?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje v hello úrovně DNS. Používá DNS odpovědí toodirect klienti toohello koncový bod příslušné služby. Klienti připojovat koncový bod služby toohello přímo, nejsou prostřednictvím Správce provozu. Traffic Manager proto nejsou vidět hello HTTP provoz mezi hello klient a hello server.

Kromě toho hello zdrojovou IP adresu dotaz DNS hello přijatých Traffic Managerem patří služby DNS toohello rekurzivní, není hello klienta. Proto Traffic Manager nemá žádné způsob tootrack jednotlivé klienty a nelze implementovat, trvalé' relací. Toto omezení je provoz na základě DNS běžné tooall systémy správy a není konkrétní tooTraffic správce.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Proč se při použití Traffic Manager zobrazuje chybu HTTP?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje v hello úrovně DNS. Používá DNS odpovědí toodirect klienti toohello koncový bod příslušné služby. Klienti se potom připojují koncový bod služby toohello přímo, nejsou prostřednictvím Správce provozu. Správce provozu nepodporuje najdete přenos HTTP není mezi klientem a serverem. Proto musí být chyby protokolu HTTP, které vidíte pocházejících z vaší aplikace. Pro aplikaci toohello tooconnect hello klienta jsou všechny kroky rozlišení DNS dokončeny. Interakce s Traffic Manager na toku přenosu hello aplikace, který zahrnuje.

Další šetření by proto soustředit na aplikace hello.

Hlavička hostitele Hello HTTP odeslané z prohlížeče klienta hello je hello zdroje většiny běžných problémů. Ujistěte se, že aplikace hello je nakonfigurované tooaccept hello hlavička správné hostitele pro hello název domény, který používáte. Koncové body pomocí hello Azure App Service, najdete v části [konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service pomocí Traffic Manager](../app-service-web/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-hello-performance-impact-of-using-traffic-manager"></a>Co je hello vlivu na výkon pomocí Traffic Manageru?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje v hello úrovně DNS. Vzhledem k tomu, že klienti připojují přímo koncové body služby tooyour, neexistuje žádný dopad na výkon vzniklé při použití Traffic Manager po navázání připojení hello.

Vzhledem k tomu, že Traffic Manager se integruje s aplikacemi v hello úroveň DNS, nevyžaduje další toobe vyhledávání DNS vloženy do hello řetězu rozlišení DNS. dopad Hello Traffic Manager na dobu překladu názvů DNS je minimální. Používá globální sítě názvové servery Traffic Manageru a [libovolného vysílání](https://en.wikipedia.org/wiki/Anycast) sítě tooensure dotazy DNS jsou vždy směrované toohello nejbližší dostupné název serveru. Kromě toho ukládání do mezipaměti odpovědí DNS znamená, že hello další DNS latence při pomocí Traffic Manager aplikuje pouze tooa podíl relací.

Hello výkonu metodu směrování provozu toohello nejbližší dostupný koncový bod. Hello net výsledkem je, že hello by měl být minimální dopad na celkový výkon přidružený k této metodě. Každé zvýšení latence DNS by měla být pokryta koncový bod toohello latence sítě nižší.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Jaké protokoly aplikace lze použít s nástrojem Traffic Manager?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje v hello úrovně DNS. Po dokončení vyhledávání DNS hello klienti připojovat koncový bod aplikace toohello přímo, nejsou prostřednictvím Správce provozu. Proto hello připojení můžete použít libovolný protokol pro aplikace. Pokud vyberete TCP jako hello monitorování protokolu, Traffic Manager monitorování stavu koncového bodu lze provést bez použití žádné protokoly aplikací. Pokud si zvolíte stavu hello toohave ověřil aplikační protokol, musí koncový bod hello toobe možné toorespond tooeither HTTP nebo HTTPS získat požadavky.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Můžete použít Traffic Manager s názvem "holé" domény?

Ne. standardech DNS Hello nepovoluje záznamů CNAME tooco-existovat s další záznamy DNS hello stejný název. Hello vrcholu (nebo kořenové) zóny DNS vždy obsahuje dva existující záznamy DNS; Hello SOA a hello záznamy autoritativních NS. To znamená, že na vrcholu zóny hello nelze vytvořit záznam CNAME bez narušení standardech DNS hello.

Správce provozu vyžaduje název DNS jednoduché hello toomap záznamů DNS CNAME. Například mapování www.contoso.com toohello Traffic Manager profil DNS název contoso.trafficmanager.net. Kromě toho hello profil služby Traffic Manager Vrátí druhou tooindicate DNS CNAME, kterému koncovému bodu hello klient připojit.

toowork chcete tento problém vyřešit, doporučujeme používat HTTP přesměrování toodirect provoz z hello holé domény název tooa jinou adresu URL, který lze potom použít Traffic Manager. Například hello holé doménu "contoso.com" může přesměrovat uživatele toohello CNAME "www.contoso.com" ukazující toohello název DNS Traffic Manager.

Plnou podporu pro holé domén v Traffic Manager je sledovány v našem funkce nevyřízených položek. Můžete zaregistrovat podporu pro tento požadavek funkce [volíte ho na náš web pro zasílání názorů komunity](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-hello-client-subnet-address-when-handling-dns-queries"></a>Traffic Manager zvažte adresa podsítě hello klienta při zpracování dotazů DNS 
Ne, aktuálně považovat jenom hello zdrojovou IP adresu hello DNS dotazu, které obdrží, což obvykle je IP adresa hello hello překladač služby DNS, při hledání pro výkon a Geographic metody směrování Traffic Manager.  
Konkrétně [RFC 7871 – klientské podsíti v dotazech DNS](https://tools.ietf.org/html/rfc7871) , který poskytuje [mechanismus rozšíření pro službu DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) které můžete předat na adresu podsítě hello klienta z překladače, které ji podporují tooDNS servery je aktuálně není podporována v Traffic Manageru. Můžete zaregistrovat podporu pro tento požadavek funkce prostřednictvím našich [web pro zasílání názorů komunity](https://feedback.azure.com/forums/217313-networking).


### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>Co je DNS TTL a jak ovlivní to svým uživatelům?

Pokud dotaz DNS pojmenováváme v Traffic Manageru, nastaví hodnotu v odpovědi hello nazývá time to live (TTL). Tato hodnota, jejíž jednotka je v sekundách, označuje tooDNS překladače po proudu na jak dlouho toocache této odpovědi. Zatímco překladače služby DNS se nezaručuje, že toocache tento výsledek mezipaměti umožňuje jejich toorespond tooany následné dotazy vypnout hello mezipaměti místo tooTraffic Správce DNS servery. To ovlivní hello odpovědi následujícím způsobem:
- vyšší hodnota TTL snižuje hello počet dotazů, které zobrazovat hello serverů DNS Traffic Manageru, které může snížit náklady na hello pro zákazníka, vzhledem k tomu, že počet dotazů na zpracování se fakturovatelné využití.
- vyšší hodnota TTL může potenciálně snížit čas hello trvá toodo vyhledávání DNS.
- vyšší hodnota TTL také znamená, že vaše data nemusí odpovídat hello nejnovější informace o stavu, Traffic Manager má získaných pomocí jeho testování agenti.

### <a name="how-high-or-low-can-i-set-hello-ttl-for-traffic-manager-responses"></a>Jak horní nebo dolní lze nastavit hello TTL pro odpovědi Traffic Manageru?

Můžete nastavit, v na úrovni profilu hello toobe DNS TTL jako nízkou jako 0 sekund a až 2 147 483 647 sekund (hello maximální rozsah kompatibilní s [definicí RFC 1035](https://www.ietf.org/rfc/rfc1035.txt )). Hodnota TTL 0 znamená, že podřízené překladače služby DNS Neukládat do mezipaměti odpovědi na dotazy a všechny dotazy jsou očekávané tooreach hello provoz Správce DNS servery pro překlad.

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Metodu směrování provozu Traffic Manageru Geographic

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Jaké jsou některé případy použití, kde geografické směrování je užitečné? 
Zeměpisná typ směrování lze použít v žádném scénáři, kde Azure zákazníků potřebuje toodistinguish uživatelů podle zeměpisné oblasti. Například používáte metodu směrování hello geografické provozu, můžete uživatelům z určitých oblastí v jiné uživatelské rozhraní než ty, které z jiných oblastí. Dalším příkladem je dodržování vyžaduje suverenity místní data, které vyžadují, aby uživatelé z určité oblasti zpracovat pouze pomocí koncových bodů v této oblasti.

### <a name="what-are-hello-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Jaké jsou hello oblastí, které jsou podporovány nástrojem Traffic Manager pro zeměpisnou směrování? 
hierarchie Hello země nebo oblast, která se používá Traffic Managerem najdete [zde](traffic-manager-geographic-regions.md). Když tato stránka je udržováno odpovídalo nejnovějším změnám, můžete také programově načíst hello stejné informace pomocí hello [REST API služby Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Jak traffic Manageru určit, kde je uživatel dotazování z? 
Traffic Manager porovná hello zdrojové IP adresy hello dotazu (je to s největší pravděpodobností místní překladač DNS provádění hello dotazování jménem uživatele hello) a používá interní IP tooregion mapy toodetermine hello umístění služby. Tato mapa se aktualizuje na průběžně tooaccount změny v hello Internetu. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-hello-exact-geographic-location-of-hello-user-in-every-case"></a>Ho záruku, že se Traffic Manageru můžete určit správně hello přesný zeměpisnou polohu uživatele hello v každém případě?
Ne, Traffic Manager nemůže zaručit, že hello geografické oblasti jsme odvození z hello zdrojové IP adresy dotazu DNS bude vždy odpovídají umístění toohello uživatele z důvodu následující toohello důvodů: 

- Nejprve jak je popsáno v předchozí – nejčastější dotazy hello, hello zdrojové IP adresy, které vidíte, že to hello vyhledávání jménem uživatele hello Překladač DNS. Při hello zeměpisnou polohu překladač služby DNS hello je dobré proxy pro zeměpisnou polohu hello hello uživatele, mohou být také různé v závislosti na nároky hello hello služba Překladač DNS a hello konkrétní překladač služby DNS, který má vybrali zákazníka toouse. Jako příklad zákazník umístěný v Malajsii může zadejte svoje zařízení používá nastavení překladač služby DNS může získat jejichž server DNS v Singapur zachyceny toohandle hello dotazu řešení pro tohoto uživatele či zařízení. V tomto případě Traffic Manager lze zobrazit pouze hello překladač IP adresy, které odpovídá toohello Singapur umístění. Viz také hello podporují starší – nejčastější dotazy týkající se adresa podsítě klienta na této stránce.

- Za druhé Traffic Manager používá k interní mapy toodo hello překladu IP adres toogeographic oblast. Tato mapa je ověřen a aktualizovat na průběžně tooincrease jeho přesnost a účet pro hello vyvíjející se povaze hello internet, stále existuje hello možnost, že naše informace není přesný reprezentace hello zeměpisné umístění všech Hello IP adresy.


###  <a name="does-an-endpoint-need-toobe-physically-located-in-hello-same-region-as-hello-one-it-is-configured-with-for-geographic-routing"></a>Koncový bod toobe potřeba fyzicky umístěné ve hello stejné oblasti jako hello jeden, který je nakonfigurován s geografické směrování? 
Ne, hello umístění koncového bodu hello ukládá bez omezení, na které oblasti mohou být namapované tooit. Koncový bod v oblasti Azure USA – střed, například může mít všichni uživatelé z tooit Indie směrované.

### <a name="can-i-assign-geographic-regions-tooendpoints-in-a-profile-that-is-not-configured-toodo-geographic-routing"></a>Můžete přiřadit zeměpisné oblasti tooendpoints v profilu, který není nakonfigurovaný toodo geografické směrování? 

Ano, pokud není geografické metody směrování hello profilu, můžete použít hello [REST API služby Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/) tooassign tooendpoints zeměpisné oblasti v tomto profilu. V případě hello-geografické směrování typ profilů toto nastavení je ignorováno. Pokud později změníte takové toogeographic směrování typ profilu, Traffic Manager můžete použít tyto mapování.


### <a name="why-am-i-getting-an-error-when-i-try-toochange-hello-routing-method-of-an-existing-profile-toogeographic"></a>Proč dochází k chybě při toochange hello směrování metodu existující profil tooGeographic?

Všechny koncové body hello pod profil s geografické směrování potřebovat toohave tooit namapovaná aspoň jedna oblast. tooconvert existující typ směrování toogeographic profil, musíte nejprve tooassociate zeměpisné oblasti tooall své koncové body pomocí hello [REST API služby Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/) před změnou hello směrování zadejte toogeographic. Pokud pomocí portálu, nejprve odstranit hello koncových bodů, změnit metodu směrování hello hello profil toogeographic a pak přidejte koncové body hello spolu s jejich mapování geografické oblasti. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Proč se důrazně doporučujeme, aby zákazníci vytvořit vnořených profilů místo koncové body v rámci profilu s povoleným směrováním geografické? 

V oblasti lze přiřadit tooonly jeden koncový bod v profilu pokud jeho pomocí zeměpisné typ směrování. Pokud tohoto koncového bodu není typu vnořené s profil připojen a podřízené tooit, pokud tohoto koncového bodu není v pořádku, budete Traffic Manager dál provoz tooit toosend od hello alternativní není odesílání, že přenosy dat se všechny lepší. Traffic Manager nemá převzetí služeb při selhání tooanother koncovému bodu, i v případě, že oblast hello přiřazené je "nadřazená" hello oblasti přiřazené toohello koncový bod, který se není v pořádku (například pokud koncový bod, který má oblast Španělsko přejde není v pořádku, že provedeme není tooanother převzetí služeb při selhání koncový bod, který má hello oblast Evropa přiřazen tooit). To se provádí tooensure, který Traffic Manager ohledech hello geografické hranice, které zákazník má instalační program v svůj profil. tooget hello výhodou selhání přes tooanother koncového bodu, kdy přestane koncový bod není v pořádku, je doporučeno, zeměpisné oblasti přiřadit toonested profily s víc koncových bodů v něm místo jednotlivých koncových bodů. Tímto způsobem Pokud koncový bod v hello vnořené podřízené profil selže, provoz může koncový bod tooanother převzetí služeb při selhání uvnitř hello stejné vnořené podřízené profilu.

### <a name="are-there-any-restrictions-on-hello-api-version-that-supports-this-routing-type"></a>Existují nějaká omezení na verzi hello rozhraní API, která podporuje tento typ směrování?

Ano, pouze verze rozhraní API 2017-03-01 a novější podporuje hello geografické typ směrování. Všechny starší verze rozhraní API nelze použít toocreated profily geografické typ směrování ani přiřadit tooendpoints zeměpisné oblasti. Pokud starší verze rozhraní API je použité tooretrieve profily z předplatného Azure, není vrácen žádný profil geografické typ směrování. Kromě toho, pokud používáte starší verze rozhraní API vrátil žádný profil, má koncové body s přiřazením zeměpisnou oblast, nemá jeho přiřazení geografické oblasti vidět.



## <a name="traffic-manager-endpoints"></a>Koncové body Traffic Manageru

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Můžete použít Správce provozu s koncovými body z více předplatných?

Pomocí koncových bodů z více předplatných není možné pomocí Azure Web Apps. Azure Web Apps vyžaduje, aby všechny vlastní název domény použít s webovými aplikacemi se používá pouze v rámci jednoho předplatného. Není možné toouse webové aplikace z více předplatných s hello stejným názvem domény.

Pro jiné typy koncových bodů je možné toouse Traffic Manager s koncovými body z více než jedno předplatné. Ve Správci prostředků koncových bodů z libovolné předplatné, lze přidat tooTraffic Manager, tak dlouho, dokud uživatel hello konfigurace profil služby Traffic Manager hello má koncový bod toohello přístup pro čtení. Tato oprávnění lze udělit pomocí [Azure Resource Manager řízení přístupu na základě role (RBAC)](../active-directory/role-based-access-control-configure.md).


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Můžete použít Správce provozu se sloty 'Staging, Cloudová služba?

Ano. Cloudové služby, přípravné sloty lze nakonfigurovat v Traffic Manageru jako externí koncové body. Kontroly stavu budou účtovat stále rychlostí hello koncové body Azure. Protože hello typ externí koncový bod se používá, změny toohello základní služby nejsou zachyceny automaticky. S externí koncové body Traffic Manageru nelze rozpoznat, kdy hello Cloudová služba je zastavena nebo odstranit. Proto hello Traffic Manager pokračuje fakturace kontroly stavu, dokud je koncový bod hello deaktivované nebo odstraněné.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Podporuje správce provoz koncovými body IPv6?

Traffic Manager aktuálně neposkytuje IPv6 addressible názvové servery. Ale Traffic Manager stále možné IPv6 klienti připojení tooIPv6 koncové body. Klient neprovede DNS požadavky přímo tooTraffic správce. Místo toho hello klient používá službu DNS rekurzivní. Pouze protokol IPv6 klient odešle požadavky služby DNS rekurzivní toohello prostřednictvím protokolu IPv6. Potom hello rekurzivní služby musí být schopný toocontact hello Traffic Manager názvové servery pomocí protokolu IPv4.

Správce provozu odpoví název DNS hello hello koncového bodu. koncový bod toosupport IPv6, záznam DNS AAAA polohovací hello koncový bod DNS název toohello IPv6 adres, musí existovat. Kontroly stavu Traffic Manageru se podporují jenom adresy IPv4. Hello služba musí koncový bod tooexpose IPv4 na hello stejný název DNS.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-hello-same-region"></a>Můžete použít Správce provozu s více než jednu webovou aplikaci v hello stejné oblasti?

Traffic Manager je obvykle používané toodirect provoz tooapplications nasadit v různých oblastech. Ale můžete taky použije, pokud aplikace má víc než jedno nasazení v hello stejné oblasti. Hello koncové body Azure Traffic Manager nepovoluje více než jeden koncový bod webové aplikace z hello stejné oblasti Azure toobe přidat toohello stejný profil služby Traffic Manager.

##  <a name="traffic-manager-endpoint-monitoring"></a>Monitorování koncového bodu Traffic Manageru

### <a name="is-traffic-manager-resilient-tooazure-region-failures"></a>Je odolný tooAzure Traffic Manager oblast selhání?

Správce provozu je klíčovou součástí hello doručení vysoce dostupných aplikací v Azure.
toodeliver vysokou dostupnost, Traffic Manager musí mít velmi vysokou dostupnost a být odolné tooregional selhání.

Standardně jsou součásti Traffic Manager odolné tooa kompletní selhání všechny oblasti Azure. Tato odolnost platí tooall Traffic Manager komponenty: názvových serverů DNS hello, hello API, hello vrstvy úložiště a monitorování služby endpoint hello.

V hello nepravděpodobnému k výpadku celé oblasti Azure Traffic Manager je očekávané toocontinue toofunction normálně. Aplikace nasazené v několika oblastmi Azure můžete spoléhat na Traffic Manager toodirect provoz tooan dostupné instance svých aplikacích.

### <a name="how-does-hello-choice-of-resource-group-location-affect-traffic-manager"></a>Jak hello Volba umístění skupiny prostředků ovlivní Traffic Manageru?

Správce provozu je jediná globální služba. Není místní. Volba umístění skupiny prostředků Hello díky žádný rozdíl tooTraffic Manager profilů nasazených v příslušné skupině prostředků.

Azure Resource Manager vyžaduje všechny toospecify skupin prostředků do umístění, která určuje hello výchozí umístění pro prostředky nasazené v příslušné skupině prostředků. Když vytvoříte profil Traffic Manageru, se vytvoří ve skupině prostředků. Všechny profily Traffic Manager používat **globální** jako jejich umístění přepsání výchozí skupiny prostředků hello.

### <a name="how-do-i-determine-hello-current-health-of-each-endpoint"></a>Jak je možné zjistit aktuální stav hello každý koncový bod?

Hello aktuální stav každého koncového bodu monitorování v přidání toohello celkové profil, se zobrazí v hello portálu Azure. Tyto informace jsou také k dispozici prostřednictvím hello sledování provozu [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/mt125941.aspx), a [a platformy Azure CLI](../cli-install-nodejs.md).

Azure neposkytuje historické informace o posledních koncový bod stavu nebo hello možnost tooraise výstrahy týkající se stavu tooendpoint změny.

### <a name="can-i-monitor-https-endpoints"></a>Můžete monitorovat koncových bodů HTTPS?

Ano. Traffic Manager podporuje zjišťování přes protokol HTTPS. Konfigurace **HTTPS** jako protokol hello v konfiguraci monitorování hello.

Správce provozu neposkytuje žádné ověření certifikátu, včetně:

* Serverové certifikáty nejsou ověřené.
* SNI serverové certifikáty nejsou podporovány.
* Klientské certifikáty nejsou podporovány.

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Můžete použít i v případě, že Moje aplikace nemá podpora protokolu HTTP nebo HTTPS Traffic Manageru?

Ano. TCP můžete určit, jak může hello monitorování protokol a Traffic Manager inicializuje připojení TCP a čekat na odpověď z koncového bodu hello. Pokud koncový bod hello reaguje toohello požadavek na připojení pomocí připojení hello tooestablish odpovědi, v časovém limitu hello období a pak tohoto koncového bodu je označen jako v pořádku.

### <a name="what-specific-responses-are-required-from-hello-endpoint-when-using-tcp-monitoring"></a>Jaké konkrétní odpovědi jsou potřeba z koncového bodu hello při použití protokolu TCP monitorování?

Pokud se používá TCP monitorování, spustí Traffic Manager TCP třícestné odesláním SYN požadavku tooendpoint v hello zadaný port. Ji potom počká, než pro určitou dobu (jako je zadaný v nastavení časového limitu hello) pro odpověď z koncového bodu hello. Pokud koncový bod hello odpoví toohello SYN žádosti s SYN ACK odpověď v rámci hello časový limit zadaný v nastavení sledování hello a potom tento koncový bod se považuje v pořádku. Pokud byl přijat hello SYN ACK odpovědi hello Traffic Manager obnoví připojení hello podle reagovat zpět se RVNÍ.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Jak rychle přesunout Traffic Manager Moji uživatelé mimo koncový bod není v pořádku?

Traffic Manager poskytuje několik nastavení, které vám mohou pomoci toocontrol hello převzetí služeb při selhání chování vašeho profilu Traffic Manageru následujícím způsobem:
- můžete určit, že hello Traffic Manager sondy koncové body hello častěji nastavením hello zjišťování Interval na 10 sekund. Tím se zajistí, že žádný koncový bod není v pořádku, budete se může zjistit co nejdříve. 
- můžete určit, jak dlouho toowait před žádost o kontrolu stavu časového limitu (časový limit minimální hodnota je 5 s).
- můžete určit, kolik selhání může dojít předtím, než koncový bod hello je označen jako chybný. Tato hodnota může být v rozsahu od 0, ve které případu hello koncový bod je označen jako chybný, při selhání hello zkontrolujte první stavu. Použití hello minimální hodnotu 0 pro počet hello tolerovat selhání však může vést tooendpoints se dostala mimo otočení kvůli tooany přechodné problémy, které může dojít v době hello zjišťování.
- můžete zadat hello time to live (TTL) pro toobe odpověď DNS hello v rozsahu od 0. V tom má znamená, že odpověď hello nelze mezipaměti překladače služby DNS a každém novém dotazu získá odpovědi, která zahrnuje hello nejaktuálnější informace o stavu této hello Traffic Manager.

Pomocí těchto nastavení můžete Traffic Manager zadejte převzetí služeb při selhání v části 10 sekund poté, co koncový bod přejde není v pořádku a dotaz DNS se provádí před hello odpovídající profil.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Jak můžete zadat různé nastavení monitorování pro různými koncovými body v profilu?

Nastavení monitorování jsou v Traffic Manageru úrovni profilu. Pokud potřebujete toouse jiné nastavení monitorování pro pouze jeden koncový bod, je možné ji provést tak, že tohoto koncového bodu jako [vnořené profil](traffic-manager-nested-profiles.md) jejichž nastavení monitorování se liší od nadřazené profil hello.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Jaké proveďte koncový bod stavu pro hostitele záhlaví zjišťuje použít?

Traffic Manager používá hlavičky hostitele v kontroly stavu HTTP a HTTPS. Hlavička hostitele Hello používaný správcem provoz je hello název cílového koncového bodu hello nakonfigurované v profilu hello. Hodnota Hello používaná v hlavičce hostitele hello nelze zadat samostatně z vlastnosti cílového hello.

### <a name="what-are-hello-ip-addresses-from-which-hello-health-checks-originate"></a>Jaké jsou hello IP adresy, ze kterých kontroluje stav hello pocházejí?

Hello následující seznam obsahuje hello IP adresy, ze kterých kontroluje stav Traffic Manager může pocházet. Můžete použít tento seznam tooensure, příchozí připojení z těchto IP adres jsou povoleny v hello koncové body toocheck jeho stav.

* 40.68.30.66
* 40.68.31.178
* 137.135.80.149
* 137.135.82.249
* 23.96.236.252
* 65.52.217.19
* 40.87.147.10
* 40.87.151.34
* 13.75.124.254
* 13.75.127.63
* 52.172.155.168
* 52.172.158.37
* 104.215.91.84
* 13.75.153.124
* 13.84.222.37
* 23.101.191.199
* 23.96.213.12
* 137.135.46.163
* 137.135.47.215
* 191.232.208.52
* 191.232.214.62
* 13.75.152.253
* 104.41.187.209
* 104.41.190.203

### <a name="how-many-health-checks-toomy-endpoint-can-i-expect-from-traffic-manager"></a>Kolik endpoint toomy kontroly stavu můžete očekávat od Traffic Manageru?

Hello počet stavu Traffic Manager zkontroluje, že dosažení váš koncový bod, závisí na následujících hello:
- Hello hodnotu, která jste nastavili pro interval monitorování hello (menší interval znamená další požadavky cílová stránka na váš koncový bod v jakékoli dané časové období).
- Hello počet umístění, ze kterých kontroly stavu hello pocházejí (hello IP adresy z kde může být těmito kontrolami je uvedena v hello předcházející – nejčastější dotazy).

## <a name="traffic-manager-nested-profiles"></a>Správce provozu vnořených profilů

### <a name="how-do-i-configure-nested-profiles"></a>Konfigurování vnořených profilů

Vnořené profily Traffic Manager lze nakonfigurovat pomocí obou hello Azure Resource Manager a hello classic Azure rozhraní REST API, rutin prostředí Azure PowerShell a rozhraní příkazového řádku Azure příkazy napříč platformami. Podporovány jsou i prostřednictvím nového portálu Azure hello. Nejsou podporovány na portálu classic hello.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Počet vrstev vnoření nemá provoz Manager podporuje?

Lze vnořit profily až too10 úrovněmi. 'Cyklu' nejsou povoleny.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-hello-same-traffic-manager-profile"></a>Můžete používat kombinaci jiné typy koncových bodů s profily vnořené podřízené, v hello stejný profil Traffic Manageru?

Ano. Neexistují žádná omezení na tom, jak kombinovat koncové body různých typů v rámci profilu.

### <a name="how-does-hello-billing-model-apply-for-nested-profiles"></a>Tom, jak model fakturace hello použít pro vnořené profily?

Neexistuje žádné negativní dopad pomocí vnořených profilů – ceny.

Správce provozu fakturace má dvě součásti: kontroly stavu koncový bod a miliony dotazy DNS

* Kontroly stavu koncový bod: je bezplatná profilu podřízené při nakonfigurovaný jako koncový bod v profilu nadřazené. Monitorování hello koncových bodů v profilu podřízené hello se fakturuje v hello obvyklým způsobem.
* Dotazy DNS: každý dotaz se počítá pouze jednou. Dotaz vůči nadřazené profil, který vrátí koncový bod z profilu podřízené se počítá proti hello nadřazené jenom profil.

Úplné podrobnosti najdete v tématu hello [Traffic Manager stránce s cenami](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Je k dispozici pro vnořených profilů dopad na výkon?

Ne. Neexistuje žádný dopad na výkon vzniklé při použití vnořených profilů.

Hello Traffic Manager názvové servery procházení hierarchie profil hello interně při zpracování každý dotaz DNS. Profil nadřazené tooa dotazu DNS může přijímat odpověď DNS se koncový bod z podřízené profilu. Jeden záznam CNAME se používá, zda používáte jediného profilu nebo vnořených profilů. Neexistuje žádné toocreate nutné záznam CNAME pro každý profil v hierarchii hello.

### <a name="how-does-traffic-manager-compute-hello-health-of-a-nested-endpoint-in-a-parent-profile"></a>Jak Traffic Manager výpočetní hello stavu vnořené koncového bodu v nadřazené profilu?

profil nadřazené Hello neprovede kontroly stavu na podřízené hello přímo. Místo toho hello stavu profilu podřízené hello koncových bodů jsou použité toocalculate hello celkový stav hello podřízené profilu. Tato informace jsou přeneseny souhrn stavu hello hello vnořené profil hierarchie toodetermine hello vnořené koncového bodu. profil nadřazené Hello používá tento toodetermine agregovaného stavu, zda hello provozu může být směrovanou toohello podřízené.

Hello následující tabulka popisuje hello chování služby Traffic Manager kontroluje stav pro vnořené koncový bod.

| Stav monitorování podřízených profilu | Stav nadřazeného monitorování koncového bodu | Poznámky |
| --- | --- | --- |
| Zakázané. Hello podřízené profilu bylo zakázáno. |Zastaveno |Stav koncového bodu nadřazené Hello je zastavena, není zakázáno. Hello stav Zakázáno, je vyhrazený pro indikující, že jste zakázali hello koncového bodu v profilu nadřazené hello. |
| Snížený výkon. Koncový bod profilu služby alespoň jednu podřízenou je ve stavu snížený. |Online: hello počet Online koncových bodů v profilu podřízené hello je alespoň hello hodnotu MinChildEndpoints.<BR>CheckingEndpoint: hello počet Online plus CheckingEndpoint koncových bodů v profilu podřízené hello je alespoň hello hodnotu MinChildEndpoints.<BR>Snížený výkon: jinak. |Provoz je koncový bod směrované tooan stavu CheckingEndpoint. Pokud MinChildEndpoints se nastaví příliš vysoko, koncový bod hello vždy snížený výkon. |
| Online. Koncový bod profilu služby alespoň jednu podřízenou je stavu Online. Žádný koncový bod je v hello snížený stavu. |Viz výše. | |
| CheckingEndpoints. Koncový bod profilu služby alespoň jednu podřízenou je 'CheckingEndpoint'. Nejsou žádné koncové body, Online' nebo 'snížený výkon. |Stejné jako výše. | |
| Neaktivní. Všechny podřízené profil koncové body jsou zakázán nebo zastaven, nebo tento profil nemá žádné koncové body. |Zastaveno | |

## <a name="next-steps"></a>Další kroky:
- Další informace o Traffic Manager [koncového bodu monitorování a automatické převzetí služeb při selhání](../traffic-manager/traffic-manager-monitoring.md).
- Další informace o Traffic Manager [metodách směrování provozu](../traffic-manager/traffic-manager-routing-methods.md).