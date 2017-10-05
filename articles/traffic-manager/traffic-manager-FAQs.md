---
title: "Azure Traffic Manager – nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje odpovědi na nejčastější dotazy o Traffic Manager"
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
ms.openlocfilehash: 44762864e0a5adf568fcd4928b48661196f05b9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Traffic Manager nejčastější dotazy (FAQ)

## <a name="traffic-manager-basics"></a>Základy Traffic Manageru

### <a name="what-ip-address-does-traffic-manager-use"></a>Jaké IP adresu Traffic Manager používat?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje na úrovni DNS. Odešle odpovědí DNS k nasměrování klientů na koncový bod příslušné služby. Klienti se potom připojují ke koncovému bodu služby přímo, nejsou prostřednictvím Správce provozu.

Proto Traffic Manager neposkytuje na koncový bod nebo IP adresu pro klienty pro připojení k. Proto pokud chcete statickou IP adresu služby, který musí být nakonfigurované na službu, není v Traffic Manageru.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Podporuje Traffic Manager "rychlé" relace?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje na úrovni DNS. Odpovědí DNS používá k nasměrování klientů na koncový bod příslušné služby. Klienti připojovat k koncový bod služby přímo, nejsou prostřednictvím Správce provozu. Traffic Manager proto nejsou vidět provoz protokolu HTTP mezi klientem a serverem.

Kromě toho IP adresu zdrojového dotazu DNS přijatých Traffic Managerem patří do rekurzivní služba DNS, nikoli u klienta. Traffic Manager proto nemá žádný způsob, jak sledovat jednotlivých klientů a nelze implementovat, trvalé' relací. Toto omezení je společná pro všechny systémy správy provozu na základě DNS a není specifická pro Traffic Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Proč se při použití Traffic Manager zobrazuje chybu HTTP?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje na úrovni DNS. Odpovědí DNS používá k nasměrování klientů na koncový bod příslušné služby. Klienti se potom připojují ke koncovému bodu služby přímo, nejsou prostřednictvím Správce provozu. Správce provozu nepodporuje najdete přenos HTTP není mezi klientem a serverem. Proto musí být chyby protokolu HTTP, které vidíte pocházejících z vaší aplikace. Pro klienta se můžete připojit k aplikaci jsou všechny kroky rozlišení DNS dokončeny. Interakce s Traffic Manager na toku přenosu aplikace, který zahrnuje.

Další šetření by proto soustředit na aplikaci.

Hlavička hostitele HTTP odeslané z prohlížeče klienta je zdroji většiny běžných problémů. Ujistěte se, že aplikace je nakonfigurovaná tak, aby přijímal hlavičku správné hostitele pro název domény, který používáte. Koncové body pomocí služby Azure App Service, najdete v části [konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service pomocí Traffic Manager](../app-service-web/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Co je dopad na výkon pomocí Traffic Manageru?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje na úrovni DNS. Vzhledem k tomu, že klienti připojují k vaší koncové body služby přímo, neexistuje žádný dopad na výkon vzniklé při použití Traffic Manager po navázání připojení.

Vzhledem k tomu, že Traffic Manager se integruje s aplikacemi na úrovni DNS, nevyžaduje další vyhledávání DNS má být vložen do řetězu rozlišení DNS. Dopad Traffic Manager na dobu překladu názvů DNS je minimální. Používá globální sítě názvové servery Traffic Manageru a [libovolného vysílání](https://en.wikipedia.org/wiki/Anycast) sítě zajistit DNS dotazy jsou vždy směrovány do nejbližší dostupné název serveru. Kromě toho ukládání do mezipaměti odpovědí DNS znamená další DNS latence při pomocí Traffic Manager, které se vztahuje pouze na zlomek relací.

Metoda výkonu směruje provoz na nejbližší dostupný koncový bod. Net výsledkem je, že by měla být minimální dopad na celkový výkon přidružený k této metodě. Každé zvýšení latence DNS by měla být pokryta nižší latenci sítě ke koncovému bodu.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Jaké protokoly aplikace lze použít s nástrojem Traffic Manager?

Jak je popsáno v [jak Traffic Manager funguje](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager funguje na úrovni DNS. Po dokončení vyhledávání DNS se klienti připojovat k koncový bod aplikace přímo, nejsou prostřednictvím Správce provozu. Proto připojení můžete použít libovolný protokol pro aplikace. Pokud vyberete TCP jako monitorování protokol Traffic Manager na koncový bod sledování stavu lze provést bez použití žádné protokoly aplikací. Pokud zvolíte možnost mít stav ověření pomocí aplikační protokol, musí být schopné reagovat na žádosti protokolu HTTP nebo HTTPS získat koncový bod.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Můžete použít Traffic Manager s názvem "holé" domény?

Ne. Standardech DNS nepovoluje záznamů CNAME, aby nevznikaly konflikty s další záznamy DNS se stejným názvem. Vrcholu (nebo kořenové) zóny DNS vždy obsahuje dva existující záznamy DNS; záznamy autoritativních NS a SOA. To znamená, že nelze bez narušení standardech DNS vytvořit záznam CNAME ve vrcholu zóny.

Správce provozu vyžaduje záznam DNS CNAME pro mapování názvu DNS jednoduché. Například namapujete www.contoso.com na název contoso.trafficmanager.net Traffic Manager profil DNS. Kromě toho vrátí profil služby Traffic Manager druhý DNS CNAME označíte, kterému koncovému bodu, klient se musí připojit k.

Chcete-li tento problém obejít, doporučujeme používat přesměrování protokolu HTTP pro přímé přenosy z názvu holé domény na jinou adresu URL, který lze potom použít Traffic Manager. Například holé doménu "contoso.com" může přesměrovat uživatele do CNAME 'www.contoso.com, který odkazuje na název DNS Traffic Manageru.

Plnou podporu pro holé domén v Traffic Manager je sledovány v našem funkce nevyřízených položek. Můžete zaregistrovat podporu pro tento požadavek funkce [volíte ho na náš web pro zasílání názorů komunity](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries"></a>Traffic Manager zvažte adresu podsítě klienta při zpracování dotazů DNS 
Ne, aktuálně považovat jenom pro zdrojové IP adresy DNS dotazu, který obdrží, což obvykle je IP adresa Překladač DNS při vyhledávání pro výkon a Geographic metody směrování Traffic Manager.  
Konkrétně [RFC 7871 – klientské podsíti v dotazech DNS](https://tools.ietf.org/html/rfc7871) , který poskytuje [mechanismus rozšíření pro službu DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) které můžete předat na adresu podsítě klienta z překladače, které ji podporují na servery DNS je aktuálně není podporována v Traffic Manageru. Můžete zaregistrovat podporu pro tento požadavek funkce prostřednictvím našich [web pro zasílání názorů komunity](https://feedback.azure.com/forums/217313-networking).


### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>Co je DNS TTL a jak ovlivní to svým uživatelům?

Pokud dotaz DNS pojmenováváme v Traffic Manageru, nastaví hodnotu v odpovědi nazývá time to live (TTL). Tato hodnota, jejíž jednotka je v sekundách, označuje do překladače služby DNS po proudu na jak dlouho pro ukládání do mezipaměti této odpovědi. Zatímco překladače služby DNS se nezaručuje, že pro ukládání do mezipaměti tento výsledek, mezipaměti umožňuje, aby odpovídal na všechny následné dotazy vypnout mezipaměti místo na servery DNS Traffic Manager. To ovlivní odpovědi následujícím způsobem:
- vyšší hodnota TTL snižuje počet dotazů, které nebude zobrazovat na serverech DNS Traffic Manager, které může snížit náklady pro zákazníka, vzhledem k tomu, že počet dotazů na zpracování se fakturovatelné využití.
- vyšší hodnota TTL potenciálně můžete zkrátit dobu potřebnou k provést vyhledávání DNS.
- vyšší hodnota TTL také znamená, že vaše data nemusí odpovídat nejnovější informace o stavu, který Traffic Manager má získaných pomocí jeho testování agenti.

### <a name="how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses"></a>Jak horní nebo dolní lze nastavit hodnotu TTL pro odpovědi Traffic Manageru?

Můžete nastavit, v na úrovni profilu, TTL DNS v rozsahu od 0 sekund a až 2 147 483 647 sekund (maximální rozsah, který je kompatibilní s [definicí RFC 1035](https://www.ietf.org/rfc/rfc1035.txt )). Hodnota TTL 0 znamená, že podřízené překladače služby DNS Neukládat do mezipaměti odpovědi na dotazy a všechny dotazy se očekává k dosažení servery pro překlad DNS Traffic Manageru.

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Metodu směrování provozu Traffic Manageru Geographic

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Jaké jsou některé případy použití, kde geografické směrování je užitečné? 
Zeměpisná typ směrování lze použít v žádném scénáři, kde Azure zákazníků potřebuje k rozlišení uživatelů podle zeměpisné oblasti. Například používáte metodu směrování provozu geografické, můžete uživatelům z určitých oblastí v jiné uživatelské rozhraní než ty, které z jiných oblastí. Dalším příkladem je dodržování vyžaduje suverenity místní data, které vyžadují, aby uživatelé z určité oblasti zpracovat pouze pomocí koncových bodů v této oblasti.

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Jaké jsou oblasti, které jsou podporovány nástrojem Traffic Manager pro zeměpisnou směrování? 
Hierarchie země nebo oblast, která se používá Traffic Managerem najdete [zde](traffic-manager-geographic-regions.md). Při této stránky je udržováno odpovídalo nejnovějším změnám, můžete také programově načíst stejné informace pomocí [REST API služby Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Jak traffic Manageru určit, kde je uživatel dotazování z? 
Správce provozu vypadá na zdrojové IP dotazu (je to s největší pravděpodobností místní překladač DNS provádění dotazování jménem uživatele) a používá k určení umístění na interní IP adresu do oblasti mapy. Tato mapa se aktualizuje průběžně pro případ změny v Internetu. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case"></a>Ho záruku, že Traffic Manageru můžete určit správně přesné geografické umístění uživatele v každém případě?
Ne, Traffic Manager nemůže zaručit, že zeměpisnou oblast, kterou jsme odvození z IP adresu zdrojového dotazu DNS vždycky odpovídají umístění uživatele z následujících důvodů: 

- Nejprve jak je popsáno v části Nejčastější dotazy pro předchozí, zdrojovou IP adresu, kterou vidíte je, že překladače DNS provádění vyhledávání jménem uživatele. Zeměpisnou polohu Překladač DNS je dobré proxy pro zeměpisnou polohu uživatele, může také být různé v závislosti na nároky na překladač služby DNS a konkrétní službu Překladač DNS, kterou zákazník se rozhodli použít. Jako příklad může zákazník umístěný v Malajsii zadejte svoje zařízení používá nastavení překladač služby DNS pro zpracování dotazu řešení pro tohoto uživatele či zařízení může získat zachyceny jejichž server DNS v Singapur. V takovém případě Traffic Manager uvidí jenom k překladači IP adresu, která odpovídá Singapur umístění. Viz také starší – nejčastější dotazy týkající se podpory adresu podsítě klienta na této stránce.

- Druhý Traffic Manager používá interní mapy pro IP adresu, kterou překlad geografické oblasti. Tato mapa je ověřen a aktualizovat průběžně zvýšit jeho přesnost a účet pro vyvíjející povaha Internetu, je stále možné naše informace není přesný reprezentace zeměpisnou polohu všechny IP adresy.


###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Potřebuje byly fyzicky umístěné ve stejné oblasti jako ten, který je nakonfigurován s geografické směrování koncový bod? 
Ne, ukládá umístění koncového bodu bez omezení, na které můžete k němu mapována oblasti. Koncový bod v oblasti Azure USA – střed, například může mít všichni uživatelé z Indie směrované na ni.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Můžete přiřadit zeměpisné oblasti s koncovými body v profilu, který není nakonfigurovaný geografické směrování? 

Ano, pokud není geografické metodu směrování profilu, můžete použít [REST API služby Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/) přiřadit zeměpisné oblasti s koncovými body v tomto profilu. V případě-geografické směrování profily typů toto nastavení je ignorováno. Pokud později změníte takový profil geografické typ směrování, Traffic Manager můžete použít tyto mapování.


### <a name="why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic"></a>Proč dochází k chybě při pokusu změnit metodu směrování existujícího profilu na Geographic?

Všechny koncové body v rámci profilu s geografické směrování musí být alespoň jedné oblasti, které jsou k němu mapována. Převést existující profil geografické typ směrování, je nutné nejprve přidružit zeměpisné oblasti všechny jeho pomocí koncových bodů [REST API služby Azure Traffic Manager](https://docs.microsoft.com/rest/api/trafficmanager/) před změnou geografické typ směrování. Pokud používáte portál, nejprve odstranit koncových bodů, změňte metodu směrování profilu geografické a poté přidejte koncové body spolu s jejich mapování geografické oblasti. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Proč se důrazně doporučujeme, aby zákazníci vytvořit vnořených profilů místo koncové body v rámci profilu s povoleným směrováním geografické? 

V oblasti lze přiřadit pouze jeden koncový bod v rámci profilu, pokud jeho pomocí zeměpisné typ směrování. Pokud tohoto koncového bodu není vnořené typy k profilu podřízené připojit, pokud provoz tohoto koncového bodu přejdete není v pořádku, správce nadále odeslat provoz od alternativní není odesílání přenosy dat se všechny lepší. Traffic Manager nemá převzetí služeb při selhání jiným koncovým bodem, i v případě, že oblast přiřazené je "nadřazená" oblast přiřazená koncového bodu, který se není v pořádku (například pokud koncový bod, který má oblast Španělsko přejde není v pořádku, že provedeme není převzetí služeb při selhání na jiný koncový bod která má oblast, kterou Evropa přiřazen). Tomu je potřeba zajistit, že Traffic Manager respektuje geografické hranice, že zákazník má instalační program v svůj profil. Chcete-li získat výhody převzetí služeb při selhání na jiný koncový bod, kdy přestane koncový bod není v pořádku, doporučujeme zeměpisné oblasti být přiřazovány vnořených profilů s víc koncových bodů v něm místo jednotlivých koncových bodů. Tímto způsobem Pokud koncový bod v profilu vnořené podřízené selže, můžete provoz převzetí služeb při selhání jiným koncovým bodem uvnitř stejný profil vnořené podřízené.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Existují nějaká omezení na verzi rozhraní API, který podporuje tento typ směrování?

Ano, geografické směrování zadejte pouze verze rozhraní API 2017-03-01 a novější podporuje. Všechny starší verze rozhraní API nelze použít k vytvoření profilů geografické typ směrování nebo zeměpisné oblasti přiřadit koncové body. Pokud starší verze rozhraní API slouží k načtení profilů z předplatného Azure, není vrácen žádný profil geografické typ směrování. Kromě toho, pokud používáte starší verze rozhraní API vrátil žádný profil, má koncové body s přiřazením zeměpisnou oblast, nemá jeho přiřazení geografické oblasti vidět.



## <a name="traffic-manager-endpoints"></a>Koncové body Traffic Manageru

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Můžete použít Správce provozu s koncovými body z více předplatných?

Pomocí koncových bodů z více předplatných není možné pomocí Azure Web Apps. Azure Web Apps vyžaduje, aby všechny vlastní název domény použít s webovými aplikacemi se používá pouze v rámci jednoho předplatného. Není možné používat webové aplikace z více předplatných se stejným názvem domény.

Pro jiné typy koncových bodů je možné pomocí služby Traffic Manager s koncovými body z více než jedno předplatné. Ve Správci prostředků koncových bodů z libovolné předplatné, mohou být přidány do Traffic Manageru, tak dlouho, dokud uživatel konfigurace profil služby Traffic Manager má přístup pro čtení ke koncovému bodu. Tato oprávnění lze udělit pomocí [Azure Resource Manager řízení přístupu na základě role (RBAC)](../active-directory/role-based-access-control-configure.md).


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Můžete použít Správce provozu se sloty 'Staging, Cloudová služba?

Ano. Cloudové služby, přípravné sloty lze nakonfigurovat v Traffic Manageru jako externí koncové body. Kontroly stavu budou účtovat stále rychlostí koncové body Azure. Vzhledem k tomu, že typ externí koncový bod se používá, změny základní služby nejsou zachyceny automaticky. S externí koncové body Traffic Manageru nelze rozpoznat, kdy se Cloudová služba je zastavena nebo odstranit. Traffic Manager proto pokračuje fakturace kontroly stavu, dokud koncový bod je deaktivované nebo odstraněné.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Podporuje správce provoz koncovými body IPv6?

Traffic Manager aktuálně neposkytuje IPv6 addressible názvové servery. Ale Traffic Manager stále možné IPv6 klienti připojení ke koncovým bodům protokol IPv6. Klient neprovede požadavky na DNS přímo do Traffic Manageru. Klient místo toho použije služba DNS rekurzivní. Klientem pouze protokol IPv6 zasílá požadavky na službu DNS rekurzivní prostřednictvím protokolu IPv6. Službu rekurzivní pak by měl být schopen kontaktovat Traffic Manager názvové servery pomocí protokolu IPv4.

Správce provozu odpoví DNS název koncového bodu. Pro podporu koncový bod IPv6, musí existovat záznam DNS AAAA DNS název koncového bodu přejdete na adresu IPv6. Kontroly stavu Traffic Manageru se podporují jenom adresy IPv4. Služba potřebuje ke zveřejnění na IPv4 koncový bod se stejným názvem DNS.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Můžete použít Správce provozu s více než jednu webovou aplikaci ve stejné oblasti?

Správce provozu se zpravidla používá, chcete-li směrovat provoz na aplikace nasazené v různých oblastech. Však může taky sloužit kde aplikace má víc než jedno nasazení ve stejné oblasti. Koncové body Azure Traffic Manager nepovoluje více než jeden koncový bod webové aplikace ze stejné oblasti Azure, který se má přidat na stejný profil Traffic Manageru.

##  <a name="traffic-manager-endpoint-monitoring"></a>Monitorování koncového bodu Traffic Manageru

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Je odolné vůči selhání oblast Azure Traffic Manageru?

Správce provozu je klíčovou součástí doručení vysoce dostupných aplikací v Azure.
Traffic Manager k poskytování vysoké dostupnosti, musí mít velmi vysokou dostupnost a být odolné vůči selhání místní.

Standardně jsou odolné vůči selhání dokončení v libovolné oblasti Azure Traffic Manager součásti. Tato odolnost platí pro všechny součásti Traffic Manager: název DNS, serverů, rozhraní API, vrstvy úložiště a koncový bod služby monitorování.

K nepravděpodobnému výpadku celé oblasti Azure Traffic Manager budou i nadále fungovat normálně. Aplikace nasazené v několika oblastmi Azure můžete spoléhat na Traffic Manager slouží k řízení provozu na dostupnou instanci své aplikace.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Jak Volba umístění skupiny prostředků ovlivní Traffic Manageru?

Správce provozu je jediná globální služba. Není místní. Volba umístění skupiny prostředků díky žádný rozdíl profily Traffic Manager nasazené v příslušné skupině prostředků.

Azure Resource Manager vyžaduje všechny skupiny prostředků a zadejte umístění, která určuje výchozí umístění pro prostředky nasazené v příslušné skupině prostředků. Když vytvoříte profil Traffic Manageru, se vytvoří ve skupině prostředků. Všechny profily Traffic Manager používat **globální** jako jejich umístění přepsání výchozího skupiny prostředků.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Jak je možné zjistit aktuální stav každý koncový bod?

Aktuální stav monitorování každý koncový bod, kromě celkové profil, se zobrazí na portálu Azure. Tyto informace jsou také k dispozici prostřednictvím sledování provozu [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/mt125941.aspx), a [a platformy Azure CLI](../cli-install-nodejs.md).

Azure neposkytuje historické informace o posledních stav koncového bodu nebo možnost vygeneroval výstrahy týkající se změny stavu koncový bod.

### <a name="can-i-monitor-https-endpoints"></a>Můžete monitorovat koncových bodů HTTPS?

Ano. Traffic Manager podporuje zjišťování přes protokol HTTPS. Konfigurace **HTTPS** jako protokol v konfiguraci monitorování.

Správce provozu neposkytuje žádné ověření certifikátu, včetně:

* Serverové certifikáty nejsou ověřené.
* SNI serverové certifikáty nejsou podporovány.
* Klientské certifikáty nejsou podporovány.

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Můžete použít i v případě, že Moje aplikace nemá podpora protokolu HTTP nebo HTTPS Traffic Manageru?

Ano. Můžete zadat TCP jako protokol pro monitorování a Traffic Manageru můžete iniciovat připojení TCP a čekat na odpověď z koncového bodu. Koncový bod reaguje na požadavek na připojení s odpověď k navázání připojení během časového limitu tohoto koncového bodu je označen jako v pořádku.

### <a name="what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring"></a>Jaké konkrétní odpovědi jsou potřeba z koncového bodu, při použití protokolu TCP monitorování?

Při monitorování TCP se používá, spustí Traffic Manager třícestné TCP odesláním SYN požadavku na koncový bod na zadaný port. Potom počká na určitou dobu (jako je zadaný v nastavení časového limitu) pro odpověď z koncového bodu. Koncový bod odpoví na žádost SYN s SYN ACK odpověď v rámci časový limit zadaný v nastavení monitorování, že koncový bod se považuje v pořádku. Pokud je odpověď SYN potvrzení, Traffic Manager obnoví připojení pomocí reagovat zpět se RVNÍ.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Jak rychle přesunout Traffic Manager Moji uživatelé mimo koncový bod není v pořádku?

Traffic Manager poskytuje několik nastavení, které umožňují řídit chování převzetí služeb při selhání vašeho profilu Traffic Manageru následujícím způsobem:
- můžete určit, že Traffic Manager sondy koncových bodů častěji nastavením zkušební fáze Interval na 10 sekund. Tím se zajistí, že žádný koncový bod není v pořádku, budete se může zjistit co nejdříve. 
- můžete určit, jak dlouho má počkat, než stavu žádosti časy rezervovat (hodnota minimální časový limit je 5 s).
- můžete určit, kolik selhání může dojít předtím, než koncový bod je označen jako chybný. Tato hodnota může být v rozsahu od 0, ve kterém případ koncový bod je označen jako chybný při selhání první kontrola stavu. Však pomocí minimální hodnotu 0. povolená počet selhání může vést ke koncovým bodům se dostala mimo otočení kvůli všechny přechodné problémy, které mohou nastat při zjišťování.
- můžete zadat time to live (TTL) pro odpověď DNS na být menší než 0. V tom znamená, že překladače služby DNS nelze odpověď do mezipaměti a každém novém dotazu získá odpověď, který obsahuje nejaktuálnější informace o stavu, který má Traffic Manager.

Pomocí těchto nastavení můžete Traffic Manager zadejte převzetí služeb při selhání v části 10 sekund poté, co koncový bod přejde není v pořádku a dotazu DNS je vytvořen pro odpovídající profil.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Jak můžete zadat různé nastavení monitorování pro různými koncovými body v profilu?

Nastavení monitorování jsou v Traffic Manageru úrovni profilu. Pokud budete muset použít jiné nastavení monitorování pro pouze jeden koncový bod, je možné ji provést tak, že tohoto koncového bodu jako [vnořené profil](traffic-manager-nested-profiles.md) jejichž nastavení monitorování se liší od nadřazené profilu.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Jaké proveďte koncový bod stavu pro hostitele záhlaví zjišťuje použít?

Traffic Manager používá hlavičky hostitele v kontroly stavu HTTP a HTTPS. Hlavička hostitele používaný správcem provoz je název cílového koncového bodu nakonfigurované v profilu. Hodnota použitá v hlavičce hostitele nelze zadat samostatně z vlastnost target.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>Jaké jsou IP adresy, ze kterých kontroluje stav pocházejí?

Následující seznam obsahuje IP adresy, ze kterých kontroluje stav Traffic Manager může pocházet. Chcete-li zajistit, že příchozí připojení z těchto IP adres jsou povolené v koncových bodů zkontrolovat její stav, může pomocí tohoto seznamu.

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

### <a name="how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager"></a>Kolik kontroly stavu na můj koncový bod je možné očekávat z Traffic Manageru?

Počet stavů Traffic Manager zkontroluje, že dosažení váš koncový bod, závisí na následujících:
- hodnota, která jste nastavili pro monitorování intervalu (interval menší znamená další požadavky cílová stránka na váš koncový bod v jakékoli dané časové období).
- počet umístění, ze kterých kontroly stavu pocházejí (IP adresy, kde může být těmito kontrolami je uveden v části Nejčastější dotazy pro předchozí).

## <a name="traffic-manager-nested-profiles"></a>Správce provozu vnořených profilů

### <a name="how-do-i-configure-nested-profiles"></a>Konfigurování vnořených profilů

Vnořené profily Traffic Manager lze nakonfigurovat pomocí Azure Resource Manager a classic Azure rozhraní API REST, rutin prostředí Azure PowerShell a rozhraní příkazového řádku Azure příkazy napříč platformami. Podporovány jsou i prostřednictvím nového portálu Azure. Nejsou podporovány na portálu classic.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Počet vrstev vnoření nemá provoz Manager podporuje?

Profily až 10 úrovní do hloubky vnoření. 'Cyklu' nejsou povoleny.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Můžete kombinovat jiné typy koncových bodů s profily vnořené podřízené, v jednom profilu Traffic Manageru?

Ano. Neexistují žádná omezení na tom, jak kombinovat koncové body různých typů v rámci profilu.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Jak platí fakturační model pro vnořené profily?

Neexistuje žádné negativní dopad pomocí vnořených profilů – ceny.

Správce provozu fakturace má dvě součásti: kontroly stavu koncový bod a miliony dotazy DNS

* Kontroly stavu koncový bod: je bezplatná profilu podřízené při nakonfigurovaný jako koncový bod v profilu nadřazené. Monitorování koncových bodů v podřízených profilu se fakturuje obvyklým způsobem.
* Dotazy DNS: každý dotaz se počítá pouze jednou. Dotaz vůči nadřazené profil, který vrátí koncový bod z profilu podřízené se počítá s profilem nadřazené.

Úplné podrobnosti najdete v tématu [Traffic Manager stránce s cenami](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Je k dispozici pro vnořených profilů dopad na výkon?

Ne. Neexistuje žádný dopad na výkon vzniklé při použití vnořených profilů.

Traffic Manager názvové servery procházení hierarchie profil interně při zpracování každý dotaz DNS. Dotaz DNS do nadřazené profilu může přijímat odpověď DNS se koncový bod z podřízené profilu. Jeden záznam CNAME se používá, zda používáte jediného profilu nebo vnořených profilů. Není nutné vytvořit záznam CNAME pro každý profil v hierarchii.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Jak Traffic Manager výpočetní stav vnořené koncového bodu v nadřazené profilu?

Profil nadřazené neprovede kontroly stavu na podřízených přímo. Místo toho stav koncových bodů podřízené profil slouží k výpočtu celkového stavu podřízené profilu. Tato informace jsou přeneseny hierarchie vnořených profilů k určení stavu vnořené koncového bodu. Profil nadřazené používá k určení, jestli provoz může přesměrovat k podřízené toto agregovaný stav.

Následující tabulka popisuje chování služby Traffic Manager kontroluje stav pro vnořené koncový bod.

| Stav monitorování podřízených profilu | Stav nadřazeného monitorování koncového bodu | Poznámky |
| --- | --- | --- |
| Zakázané. Podřízené profilu bylo zakázáno. |Zastaveno |Stav koncového bodu nadřazené je zastavena, není zakázáno. Stav Zakázáno, je vyhrazený pro indikující, že jste zakázali koncový bod v nadřazené profilu. |
| Snížený výkon. Koncový bod profilu služby alespoň jednu podřízenou je ve stavu snížený. |Online: počet Online koncových bodů v podřízených profilu je alespoň hodnota MinChildEndpoints.<BR>CheckingEndpoint: počet Online plus CheckingEndpoint koncových bodů v podřízených profilu je alespoň hodnota MinChildEndpoints.<BR>Snížený výkon: jinak. |Provoz se směruje na koncový bod stavu CheckingEndpoint. Pokud MinChildEndpoints se nastaví příliš vysoko, koncový bod vždy snížený výkon. |
| Online. Koncový bod profilu služby alespoň jednu podřízenou je stavu Online. Žádný koncový bod je ve stavu snížený. |Viz výše. | |
| CheckingEndpoints. Koncový bod profilu služby alespoň jednu podřízenou je 'CheckingEndpoint'. Nejsou žádné koncové body, Online' nebo 'snížený výkon. |Stejné jako výše. | |
| Neaktivní. Všechny podřízené profil koncové body jsou zakázán nebo zastaven, nebo tento profil nemá žádné koncové body. |Zastaveno | |

## <a name="next-steps"></a>Další kroky:
- Další informace o Traffic Manager [koncového bodu monitorování a automatické převzetí služeb při selhání](../traffic-manager/traffic-manager-monitoring.md).
- Další informace o Traffic Manager [metodách směrování provozu](../traffic-manager/traffic-manager-routing-methods.md).