---
title: aaaFrequently dotazy pro Azure Application Gateway | Microsoft Docs
description: "Tato stránka obsahuje odpovědi toofrequently kladené dotazy týkající se Azure Application Gateway"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d54ee7ec-4d6b-4db7-8a17-6513fda7e392
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: b2df3a82a71a3264d3d34d317d08e4b4f72c6e3e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Nejčastější dotazy pro službu Application Gateway

## <a name="general"></a>Obecné

**OTÁZKY. Co je aplikační brána?**

Služba Azure Application Gateway je řadiče doručení aplikace (ADC) jako služba nabízí různé vrstvy 7 možnosti vyrovnávání zatížení pro vaše aplikace. Nabízí vysoce dostupné a škálovatelné služby, která je plně spravovaná službou Azure.

**OTÁZKY. Jaké funkce podporuje Application Gateway?**

Aplikační brána podporuje protokol SSL snižování zátěže a end tooend SSL, brány Firewall webových aplikací, spřažení na základě souboru cookie relace, adresa url na základě cesty směrování, hostování více lokality a ostatní. Úplný seznam podporovaných funkcích najdete na adrese [Úvod tooApplication brány](application-gateway-introduction.md)

**OTÁZKY. Co je hello rozdíl mezi aplikační bránu a nástroj pro vyrovnávání zatížení Azure?**

Application Gateway je Vyrovnávání zatížení vrstvy 7, což znamená, že pracuje pouze webový provoz (HTTP nebo HTTPS/WebSocket). Podporuje možnosti, například ukončení protokolu SSL, spřažení relace na základě souborů cookie a kruhové dotazování pro provoz služby Vyrovnávání zatížení. Nástroj pro vyrovnávání zatížení a načtěte provozu zůstatky na vrstvě 4 (TCP/UDP).

**OTÁZKY. Jaké protokoly podporuje Application Gateway?**

Aplikační brána podporuje protokoly HTTP, HTTPS a protokolu WebSocket.

**OTÁZKY. Jaké prostředky jsou dnes podporovány v rámci fondu back-end?**

Back-endové fondy může skládat ze síťových adaptérů, sady škálování virtuálního počítače, veřejné IP adresy, názvy interní IP adres, plně kvalifikované domény (FQDN) a víceklientské back EndY jako Azure Web Apps. Aplikační brány, které nejsou členy fondu back-end svázané tooan sady dostupnosti. Členy fondu back-end může být napříč clustery, datových center, nebo mimo Azure, dokud mají připojení pomocí protokolu IP.

**OTÁZKY. Jaké oblasti je dostupná v hello služby?**

Application Gateway je k dispozici ve všech oblastech globální Azure. Je také dostupná v [Azure China](https://www.azure.cn/) a [Azure Government.](https://azure.microsoft.com/en-us/overview/clouds/government/)

**OTÁZKY. To je vyhrazený pro Moje předplatné nasazení nebo je sdílen na zákazníky?**

Application Gateway je vyhrazené nasazení ve virtuální síti.

**OTÁZKY. Je HTTP -> HTTPS přesměrování podporovány?**

Přesměrování je podporována. Navštivte [přehled přesměrování Application Gateway](application-gateway-redirect-overview.md) toolearn Další.

**OTÁZKY. Pořadí, v jakém jsou naslouchací procesy zpracování?**

Moduly pro naslouchání jsou zpracovány v hello pořadí, ve kterém jsou uvedené. Z tohoto důvodu Pokud základní naslouchací proces odpovídá příchozí požadavek zpracuje jej nejprve.  Moduly pro naslouchání více lokalit by měl být nakonfigurovaný před přenosem tooensure základní naslouchací proces směrované toohello správné back-end.

**OTÁZKY. Kde najít IP a DNS Application Gateway?**

Pokud používáte veřejnou IP adresu jako koncový bod, tyto informace naleznete na prostředek hello veřejné IP adresy nebo na stránku přehled hello pro hello Application Gateway hello portálu. Pro interní IP adresy najdete na stránce Přehled hello.

**OTÁZKY. Hello IP adresu nebo DNS mění přes hello životnost hello Application Gateway?**

Pokud po zastavení a spuštění zákazníkem hello hello brány, můžete změnit Hello VIP. Hello DNS přidružené Application Gateway přes životního cyklu hello hello brány nezmění. Z tohoto důvodu je doporučeno toouse CNAME alias a nasměrujete ho adresa DNS toohello hello Application Gateway.

**OTÁZKY. Podporuje Application Gateway statickou IP adresu?**

Ne, aplikační brána nepodporuje statické veřejné IP adresy, ale podporuje statické interní IP adresy.

**OTÁZKY. Podporuje Application Gateway víc veřejných IP adres v bráně hello?**

Na aplikační brány je podporovaná pouze jednu veřejnou IP adresu.

**OTÁZKY. Podporuje Application Gateway hlavičky x předávaných pro?**

Ano, Application Gateway vloží x předávaných pro, x předávaných proto a hlavičky x předávaných port do žádosti o hello předávaných toohello back-end. Hello formát hlavičky x předávaných pro je čárkami oddělený seznam IP: port. Hello platné hodnoty x předávaných proto jsou protokolu http nebo https. X předávaných port Určuje hello port, na které hello žádost dosáhla hello Application Gateway.

**OTÁZKY. Jak dlouho trvá toodeploy služby Application Gateway? Moje aplikace brány stále funguje při aktualizaci?**

Nová nasazení aplikací brány může trvat až tooprovision too20 minut. Tooinstance změny velikosti nebo počet nejsou rušivý a hello brány zůstává aktivní během této doby.

## <a name="configuration"></a>Konfigurace

**OTÁZKY. Je aplikační brána vždy nasazený ve virtuální síti?**

Ano, aplikační brány je vždy nasazena v podsíti virtuální sítě. Tato podsíť může obsahovat pouze Application Gateway.

**OTÁZKY. Může kontaktovat Application Gateway tooinstances mimo jeho virtuální síť?**

Aplikační brána může kontaktovat tooinstances mimo hello virtuální síť, která je v, dokud není IP připojení. Pokud máte v plánu toouse vyžaduje interní IP adresy jako členy fondu back-end, pak [VNET Peering](../virtual-network/virtual-network-peering-overview.md) nebo [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).

**OTÁZKY. Lze nasadit nic jiného v podsíť brány aplikace hello?**

Ne, ale můžete nasadit další aplikace v hello podsíť brány.

**OTÁZKY. Podporuje na podsíť brány aplikace hello skupin zabezpečení sítě?**

Skupiny zabezpečení sítě jsou podporovány v podsíti hello Application Gateway s hello následující omezení:

* Výjimky musí být umístěno v pro příchozí komunikaci na portech 65503 65534 pro back-end stavu toowork správně.

* Odchozí připojení k Internetu, nejde blokovat.

* Provoz z hello značka AzureLoadBalancer musí být povoleno.

**OTÁZKY. Jaká jsou omezení hello ve Application Gateway? Může tyto limity zvýšit?**

Navštivte [omezení brány aplikací](../azure-subscription-service-limits.md#application-gateway-limits) tooview hello omezení.

**OTÁZKY. Lze použít aplikační brány pro externí i interní provoz současně?**

Ano, podporuje aplikační brány s jeden interní IP adresy a jeden externí IP adresu na aplikační brány.

**OTÁZKY. VNet peering je podporovaný?**

Ano, partnerský vztah virtuální sítě je podporovaný a je výhodné pro provoz v jiných virtuálních sítí Vyrovnávání zatížení.

**OTÁZKY. Může kontaktovat tooon místní servery, když jsou připojeni pomocí ExpressRoute nebo VPN tunely?**

Ano, tak dlouho, dokud provoz je povolený.

**OTÁZKY. Může mít jeden fond back-end obsluhující mnoho aplikací na jiné porty?**

Architektura malých služby je podporována. Potřebovali byste více tooprobe nastavení protokolu http na jiné porty.

**OTÁZKY. Podporují vlastní testy paměti na data odpovědi zástupné znaky/regex?**

Vlastní testy paměti nepodporují zástupných znaků nebo regex na data odpovědi. 

**OTÁZKY. Jak se zpracovávají pravidla?**

Pravidla se zpracovávají v pořadí hello, které jsou nakonfigurované. Doporučuje se, zda pravidla více lokalit jsou nakonfigurovány před základních pravidel tooreduce hello šance, že se provoz směrovat nevhodných back-end toohello jako základní pravidlo hello odpovídá provoz založený na pravidlu více lokalit předchozí toohello port vyhodnocovaný.

**OTÁZKY. Jak se zpracovávají pravidla?**

Pravidla se zpracovávají v pořadí hello, které byly vytvořeny. Doporučuje se, že před základních pravidel jsou nakonfigurovaná pravidla více lokalit. Nakonfigurováním nejprve naslouchací procesy více lokalit, tato konfigurace snižuje riziko hello, provoz se směruje toohello nevhodných back-end. Tento problém směrování může docházet k základní pravidlo hello odpovídá provoz založený na pravidlu více lokalit předchozí toohello port vyhodnocovaný.

**OTÁZKY. Co označují hello pole hostitel pro vlastní testy paměti?**

Pole hostitel Určuje hello název toosend hello testu do. Platí jenom v případě více lokalit je nakonfigurovaná na aplikační bránu, v opačném případě použijte "127.0.0.1". Tato hodnota se liší od názvu hostitele virtuálních počítačů a je ve formátu \<protokol\>://\<hostitele\>:\<port\>\<cestu\>.

**OTÁZKY. Seznam povolených adres aplikační brány přístup tooa se dá několika zdrojové IP adresy?**

Tento scénář lze provést pomocí skupin Nsg na podsítě brány aplikace. následující omezení Hello měly být umístěny v podsíti hello v hello uvedené pořadí podle priority:

* Povolí příchozí provoz ze zdrojových rozsah IP/IP.

* Povolit příchozí požadavky od všech zdrojů tooports 65503 65534 pro [komunikace stavu back-end](application-gateway-diagnostics.md).

* Povolit příchozí nástroj pro vyrovnávání zatížení Azure sondy (značka AzureLoadBalancer) a příchozí přenosy virtuální sítě (virtuální síť značky) na hello [NSG](../virtual-network/virtual-networks-nsg.md).

* Blokovat všechna ostatní příchozí přenosy s odepření všechna pravidla.

* Povolit odchozí přenosy toohello internet pro všechna místa určení.

## <a name="performance"></a>Výkon

**OTÁZKY. Jak Application Gateway podporuje vysokou dostupnost a škálovatelnost?**

Aplikační brána podporuje scénáře s vysokou dostupností, až budete mít dva nebo více instancí nasazení. Azure distribuuje tyto instance v aktualizaci a odolnost tooensure domény, který v hello nedošlo k selhání všech instancí současně. Aplikační brána podporuje škálovatelnost přidáním více instancí hello stejné zatížení hello tooshare brány.

**OTÁZKY. Jak dosáhnout scénář zotavení po Havárii napříč datovými centry s Application Gateway?**

Zákazníci mohou používat Traffic Manager toodistribute provoz napříč více bran aplikace v různých datových centrech.

**OTÁZKY. Automatické škálování je podporovaný?**

Ne, ale Application Gateway poskytuje metriky propustnosti, kterou lze použít tooalert můžete po dosažení prahové hodnoty. Přidávání instancí nebo změna velikosti ručně nerestartuje hello brány a nemá negativní vliv na stávající přenosy dat.

**OTÁZKY. Podporuje ruční škálování nahoru/dolů příčina výpadku?**

Neexistuje žádné výpadky, instancí jsou rozmístěny v upgradu domén a domén selhání.

**OTÁZKY. Můžete změnit velikost instance ze střední toolarge bez přerušení?**

Ano, Azure rozděluje instance na aktualizace a selhání tooensure domény, který v hello nedošlo k selhání všech instancí současně. Aplikace podporuje brány škálování přidáním více instancí hello stejné zatížení hello tooshare brány.

## <a name="ssl-configuration"></a>Konfigurace protokolu SSL

**OTÁZKY. Jaké certifikáty jsou podporovány ve Application Gateway?**

Vlastní podepsané certifikáty, certifikátů certifikační Autority a certifikátů zástupnému znaku. jsou podporovány. Rozšířené ověření certifikátů nejsou podporovány.

**OTÁZKY. Jaké jsou hello aktuální šifrovací sada podporovaná Application Gateway?**

Hello následují hello aktuální šifrovací sada podporovaná aplikační brány. Navštivte: [konfigurace SSL verze zásad a šifrovací sady ve Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn jak toocustomize možností protokolu SSL.

- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

**OTÁZKY. Application Gateway také podporuje znova šifrovat provoz toohello back-end?**

Ano, přesměrování zpracování SSL Aplikační brána podporuje a end tooend SSL, který znovu zašifruje hello provoz toohello back-end.

**OTÁZKY. Můžete nakonfigurovat verzí protokolu SSL toocontrol zásady protokolu SSL?**

Ano, můžete nakonfigurovat Application Gateway toodeny TLS1.0, TLS1.1 a TLS1.2. Protokol SSL 2.0 a 3.0 jsou už ve výchozím nastavení zakázána a se nedají konfigurovat.

**OTÁZKY. Můžete nakonfigurovat šifrovací sady a pořadí zásad?**

Ano, [konfigurace šifrovací sady](application-gateway-ssl-policy-overview.md) je podporována. Při definování vlastní zásady, musí být povolena alespoň jeden z následujících šifrovací sady hello. Aplikační brána používá SHA256 toofor back-end správy.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

**OTÁZKY. Jak velký počet certifikátů SSL jsou podporovány?**

Až too20 SSL certifikáty jsou podporovány.

**OTÁZKY. Kolik ověřovací certifikáty pro back-end znova šifrovat jsou podporovány?**

Výchozí hodnota je 5 je podporováno až too10 certifikáty pro ověřování.

**OTÁZKY. Umožňuje Application Gateway integraci s Azure Key Vault nativně?**

Ne, není integrované s Azure Key Vault.

## <a name="web-application-firewall-waf-configuration"></a>Konfigurace brány Firewall (firewall webových aplikací) webové aplikace

**OTÁZKY. Nabízí hello firewall webových aplikací SKU všechny hello funkcí dostupných v hello standardní SKU?**

Ano, firewall webových aplikací podporuje všechny funkce hello v hello standardní SKU.

**OTÁZKY. Co je aplikační brána podporuje verze řádku hello?**

Aplikační brána podporuje řádku [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) a řádku [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

**OTÁZKY. Jak se monitorování firewall webových aplikací?**

Firewall webových aplikací je monitorována prostřednictvím protokolování diagnostiky, další informace o protokolování diagnostiky naleznete na [protokolování diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md)

**OTÁZKY. Detekce režimu blokování provozu?**

Ne, detekce režimu jenom protokoly přenosy, která spustí pravidlo firewall webových aplikací.

**OTÁZKY. Jak lze přizpůsobit pravidla firewall webových aplikací?**

Ano, pravidla firewall webových aplikací jsou přizpůsobitelné, další informace o tom, jak se toocustomize je navštívit [skupiny pravidel přizpůsobit firewall webových aplikací a pravidla](application-gateway-customize-waf-rules-portal.md)

**OTÁZKY. Jaká pravidla jsou nyní k dispozici?**

Firewall webových aplikací v současné době podporuje řádku [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) a [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), které poskytují základní zabezpečení proti většinu hello ohrožení zabezpečení prvních 10 identifikovaný hello otevřete webovou aplikaci zabezpečení projektu (OWASP) je zde uveden [OWASP top 10 ohrožení zabezpečení](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* Ochrana před útoky prostřednictvím injektáže SQL.

* Ochrana před skriptováním mezi weby.

* Ochrana před běžnými webovými útoky, jako je například injektáž příkazů, pronášení požadavků HTTP, rozdělování odpovědí protokolu HTTP a útok pomocí vložení vzdáleného souboru.

* Ochrana před narušením protokolu HTTP.

* Ochrana před anomáliemi protokolu HTTP, jako například chybějící údaj user-agent hostitele nebo hlavičky Accept.

* Ochrana před roboty, prohledávacími moduly a skenery.

* Detekce časté nesprávné konfigurace aplikace (tedy Apache, IIS, atd.)

**OTÁZKY. Firewall webových aplikací taky podporuje DDoS prevence?**

Ne, firewall webových aplikací neposkytuje DDoS prevence.

## <a name="diagnostics-and-logging"></a>Protokolování a diagnostiky

**OTÁZKY. Jaké typy protokolů jsou k dispozici s Application Gateway?**

Nejsou k dispozici pro službu Application Gateway tři protokoly. Další informace o těchto protokolů a jiné diagnostické funkce, najdete v článku [back-end stavu, protokolů diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog** – protokol přístupu hello obsahuje každý odeslaný požadavek toohello Application Gateway front-endu. Hello data zahrnují hello volajícího IP adresy, požadovaná, adresa URL odpovědi latence, návratový kód, bajtů a odhlášení. Protokol přístupu se shromažďují každých 300 sekund. Tento protokol obsahuje jeden záznam za instance aplikační brány.
- **ApplicationGatewayPerformanceLog** -hello výkonu protokolu zaznamená informace o výkonu na základě za instance včetně celkový požadavek zpracovat, propustnost v bajtech, celkový počet požadavků zpracovaných, počet chybných požadavků, v pořádku a není v pořádku počet instancí back-end.
- **ApplicationGatewayFirewallLog** -hello brány firewall protokol obsahuje požadavky, které se protokolují prostřednictvím zjišťování nebo zabránění režim služby application gateway, která je konfigurovaná pomocí brány firewall webových aplikací.

**OTÁZKY. Jak poznám, pokud jsou moje členy fondu back-end v pořádku?**

Můžete použít rutiny prostředí PowerShell hello `Get-AzureRmApplicationGatewayBackendHealth` nebo ověřit stav prostřednictvím portálu hello tak, že navštívíte [diagnostiku brány aplikace](application-gateway-diagnostics.md)

**OTÁZKY. Co je hello zásady uchovávání informací na hello diagnostické protokoly?**

Diagnostické protokoly účet úložiště zákazníci toohello toku a zákazníků můžete nastavit zásady uchovávání informací hello podle jejich předvoleb. Diagnostické protokoly můžete odeslat také tooan centra událostí nebo analýzy protokolů. Navštivte [Application Diagnostics brány](application-gateway-diagnostics.md) další podrobnosti.

**OTÁZKY. Získání protokolů auditu pro službu Application Gateway**

Protokoly auditu jsou k dispozici pro službu Application Gateway. Hello portálu, klikněte na tlačítko **protokol aktivit** v okně nabídky hello protokolu auditu hello tooaccess Application Gateway. 

**OTÁZKY. Můžete nastavit výstrahy s Application Gateway?**

Ano, aplikační brána podporuje výstrahy, se konfigurují vypnout metriky.  Application Gateway aktuálně poskytuje metriky propustnosti"", což může být nakonfigurované tooalert. toolearn více o výstrahách, navštivte [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**OTÁZKY. Back-end stavu vrátí Neznámý stav, co by mohlo být příčinou tento stav?**

Nejčastější příčinou Hello je skupina NSG nebo vlastní DNS je blokován přístup toohello back-end. Navštivte [back-end stavu, protokolování diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md) toolearn Další.

## <a name="next-steps"></a>Další kroky

toolearn najdete informace o bráně aplikace [Úvod tooApplication brány](application-gateway-introduction.md).
