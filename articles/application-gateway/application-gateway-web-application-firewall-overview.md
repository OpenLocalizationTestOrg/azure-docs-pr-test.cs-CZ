---
title: "aaaIntroduction tooweb application firewall (firewall webových aplikací) pro Azure Application Gateway | Microsoft Docs"
description: "Na této stránce najdete přehled firewallu webových aplikací (WAF) služby Application Gateway."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 04b362bc-6653-4765-86f6-55ee8ec2a0ff
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: amsriva
ms.openlocfilehash: 5a42ce0fb2bd12a391844099e2de8fa2571195e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="web-application-firewall-waf"></a>Firewall webových aplikací (WAF)

Firewall webových aplikací (WAF) je funkce služby Application Gateway poskytující centralizovanou ochranu webových aplikací před běžným zneužitím a ohrožením zabezpečení. 

Brány firewall webových aplikací je na základě pravidel z hello [sady pravidel základní OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 nebo 2.2.9. Webové aplikace se čím dál častěji stávají cílem škodlivých útoků, které zneužívají běžně známé chyby zabezpečení. Jsou běžné mezi tyto zneužitím prostřednictvím injektáže SQL, skriptování mezi weby útoků tooname pár. Zabránění takové útoky v kódu aplikace může být náročné což může vyžadovat přísných Údržba, opravy a monitorování v několika vrstev topologie aplikace hello. Brány firewall centralizované webových aplikací pomáhá zkontrolujte mnohem jednodušší správu zabezpečení a nabízí lepší záruku tooapplication správci proti hrozbám nebo vniknutí. Řešení firewall webových aplikací můžete rovněž reagovat ohrožení zabezpečení tooa rychlejší podle opravy známých ohrožení zabezpečení do centrálního umístění a zabezpečení těchto jednotlivých webových aplikací. Existující application Gateway může být snadno převedený tooa webové aplikace povolena brána firewall aplikační brány.

![imageURLroute](./media/application-gateway-web-application-firewall-overview/WAF1.png)

Aplikační brána funguje jako aplikaci doručení řadič a nabízí ukončení protokolu SSL, spřažení relace na základě souborů cookie, distribuce zatížení pomocí kruhového dotazování, na základě obsahu směrování, možnost toohost několik vylepšení weby a zabezpečení. Vylepšení zabezpečení, které nabízí Application Gateway patří Správa zásad protokolu SSL, tooend end podporu protokolu SSL. Podle firewall webových aplikací (brány firewall webových aplikací), je přímo integrovaná do nabídky hello ADC je nyní posílit zabezpečení aplikací. To zajišťuje toomanage centrální umístění snadno tooconfigure a chránit proti známých chyb zabezpečení webové webových aplikací.

## <a name="benefits"></a>Výhody

Hello následují hello základní výhody, které poskytují brány firewall aplikační brány a webových aplikací:

### <a name="protection"></a>Ochrana

* Chraňte před ohrožení zabezpečení webové a útoky bez úpravy kódu toobackend webové aplikace.

* Ochrana více webových aplikací na hello stejný čas za služby application gateway. Aplikační brána podporuje, hostování až too20 weby za jednu bránu, která by mohla všechny chráněné proti útokům na web s firewall webových aplikací.

### <a name="monitoring"></a>Monitorování

* Můžete monitorovat útoky na své webové aplikace pomocí protokolu WAF generovaného v reálném čase. Tento protokol je integrovaná s [Azure monitorování](../monitoring-and-diagnostics/monitoring-overview.md) tootrack firewall webových aplikací výstrah a protokoly a snadno monitorovat trendy.

* Brzy budeme integrovat WAF do služby Azure Security Center. Azure Security Center umožňuje centrální zobrazení stavu zabezpečení hello všech vašich prostředků Azure.

### <a name="customization"></a>Přizpůsobení

* Hello možnost toocustomize firewall webových aplikací pravidla a pravidla skupiny toosuit vaše požadavky aplikací a eliminovat falešně pozitivních zjištění.

## <a name="features"></a>Funkce

Vybavená předem nakonfigurovaným rozhraním řádku 3.0 ve výchozím nastavení brány firewall webových aplikací nebo můžete zvolit toouse 2.2.9. CRS 3.0 dosahuje menšího počtu falešně pozitivních nálezů oproti verzi 2.2.9. Hello možnost příliš[přizpůsobit pravidla toosuit potřeb](application-gateway-customize-waf-rules-portal.md) je k dispozici. Některé z běžných chyb webové hello které brány firewall webových aplikací chrání před zahrnuje:

* Ochrana před útoky prostřednictvím injektáže SQL.
* Ochrana před skriptováním mezi weby.
* Ochrana před běžnými webovými útoky, jako je například injektáž příkazů, pronášení požadavků HTTP, rozdělování odpovědí protokolu HTTP a útok pomocí vložení vzdáleného souboru.
* Ochrana před narušením protokolu HTTP.
* Ochrana před anomáliemi protokolu HTTP, jako například chybějící údaj user-agent hostitele nebo hlavičky Accept.
* Ochrana před roboty, prohledávacími moduly a skenery.
* Detekce běžných chyb v konfiguraci aplikací (tj. Apache, IIS atd.).

Podrobnější seznam pravidel a jejich ochrany naleznete v následujících hello [základní sady pravidel](#core-rule-sets).

### <a name="core-rule-sets"></a>Základní sady pravidel

Application Gateway podporuje dvě sady pravidel, CRS 3.0 a CRS 2.2.9. Tyto základní sady pravidel jsou kolekce pravidel, které chrání vaše webové aplikace před škodlivými aktivitami.

#### <a name="owasp30"></a>OWASP_3.0

Sada pravidel základní Hello 3.0 poskytuje má 13 skupiny pravidel, jak je znázorněno v následující tabulce hello. Každá z těchto skupin obsahuje několik pravidel, která mohou být zakázána.

|RuleGroup|Popis|
|---|---|
|**[REQUEST-910-IP-REPUTATION](application-gateway-crs-rulegroups-rules.md#crs910)**|Obsahuje pravidla tooprotect proti známé nevyžádanou poštu nebo škodlivé aktivity.|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Obsahuje pravidla toolock metod down (PUT, PATCH <...)|
|**[REQUEST-912-DOS-PROTECTION](application-gateway-crs-rulegroups-rules.md#crs912)**| Obsahuje pravidla tooprotect proti útokům na dostupnost služby (DoS).|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**| Obsahuje pravidla tooprotect proti skenery port a prostředí.|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Obsahuje pravidla tooprotect proti kódování problémy a protokolu.|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Obsahuje pravidla tooprotect proti vkládání záhlaví, podvržení požadavku a odpovědi rozdělení|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Obsahuje pravidla tooprotect proti útokům na soubor a cestu.|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Obsahuje pravidla tooprotect proti zahrnutí vzdáleného souboru (RFI)|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Obsahuje pravidla tooprotect znovu vzdálené spuštění kódu.|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|Obsahuje pravidla tooprotect před útoky vkládání PHP.|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|Obsahuje pravidla pro ochranu před křížovým skriptováním.|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|Obsahuje pravidla pro ochranu před injektáží SQL.|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Obsahuje pravidla tooprotect proti útokům záznam relace.|

#### <a name="owasp229"></a>OWASP_2.2.9

Sada pravidel základní Hello 2.2.9 poskytuje má 10 skupiny pravidel, jak je znázorněno v následující tabulce hello. Každá z těchto skupin obsahuje několik pravidel, která mohou být zakázána.

|RuleGroup|Popis|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|Obsahuje pravidla tooprotect proti porušení protocol (neplatné znaky, GET s textu žádosti atd.)|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Obsahuje pravidla tooprotect proti záhlaví nesprávné informace.|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Obsahuje pravidla tooprotect proti argumentů nebo soubory, které překračují omezení.|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Obsahuje pravidla tooprotect před s omezeným přístupem metody a hlavičky a typy souborů. |
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Obsahuje pravidla tooprotect proti webové prohledávací moduly a skenerů.|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|Obsahuje pravidla tooprotect před obecné útoky (záznam relace vzdáleného souboru zahrnutí, vkládání PHP, atd.)|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|Obsahuje pravidla tooprotect před útoky vkládání SQL|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Obsahuje pravidla tooprotect proti křížové skriptování.|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Obsahuje pravidla tooprotect před útoky traversal cesta|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Obsahuje pravidla tooprotect proti trojskými koni.|

### <a name="waf-modes"></a>Režimy WAF

Aplikace brány firewall webových aplikací může být nakonfigurované toorun v hello následujících dvou režimů:

* **Detekce režimu** – Pokud nakonfigurované toorun v režimu detekce aplikace brány firewall webových aplikací sleduje a zaznamená všechny výstrahy hrozba v souboru protokolu tooa. Diagnostiku protokolování pro službu Application Gateway by měl být zapnut pomocí hello **diagnostiky** části. Musíte taky tooensure, který hello firewall webových aplikací je vybraná a zapnuta protokolu. když je firewall webových aplikací spuštěný v režimu detekce, neblokuje příchozí požadavky.
* **Prevence režimu** – když nakonfigurované toorun v režimu prevence, Application Gateway aktivně blokuje napadením zjistil jeho pravidly. Hello útočník obdrží 403 výjimce neoprávněného přístupu a hello připojení bude ukončeno. Prevence režimu pokračuje toolog takové útoky v protokolech hello firewall webových aplikací.

### <a name="application-gateway-waf-reports"></a>Monitorování WAF

Monitorování stavu hello svoji službu application gateway je důležité. Monitorování stavu hello vaší webové aplikace brány firewall a hello aplikací, které chrání je zajišťována prostřednictvím protokolování a integraci s Azure monitorování, Azure Security Center (už brzy) a analýzy protokolů.

![Diagnostika](./media/application-gateway-web-application-firewall-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure Monitor

Každý protokol aplikační brány je integrovaný do služby [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).  To vám umožní tootrack diagnostické informace, včetně výstrahy firewall webových aplikací a protokoly.  Tato funkce je uvedené v hello prostředku aplikační brány hello portálu v části hello **diagnostiky** kartě nebo přímo prostřednictvím hello Azure monitorování služby. toolearn najdete informace o povolení diagnostických protokolů pro službu application gateway [diagnostics Application Gateway](application-gateway-diagnostics.md)

#### <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](../security-center/security-center-intro.md) pomáhá zabránit, zjišťovat a odpovědět toothreats s lepší přehled a kontrolu nad hello zabezpečení vašich prostředků Azure. Služba Application Gateway je nyní [integrována do služby Azure Security Center](application-gateway-integration-security-center.md). Azure Security Center vyhledá prostředí toodetect nechráněné webových aplikací. Ji můžete nyní doporučujeme aplikace brány firewall webových aplikací tooprotect tyto prostředky snadno napadnutelný. Aplikace brány firewall webových aplikací můžete vytvořit přímo z hello Azure Security Center.  Tyto instance firewall webových aplikací jsou integrované s Azure Security Center a bude posílat upozornění a informace o stavu zpět tooAzure Security Center pro vytváření sestav.

![Obrázek 1](./media/application-gateway-web-application-firewall-overview/figure1.png)

#### <a name="logging"></a>Protokolování

Firewall webových aplikací (WAF) služby Application Gateway poskytuje podrobné generování sestav o každém útoky, který detekuje. Protokolování je integrované v protokolech diagnostiky Azure a výstrahy se zaznamenávají ve formátu JSON. Tyto protokoly je možné integrovat se službou [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).

![imageURLroute](./media/application-gateway-web-application-firewall-overview/waf2.png)

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupId}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{appGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

## <a name="application-gateway-waf-sku-pricing"></a>Ceny SKU WAF služby Application Gateway

Firewall webových aplikací je k dispozici jako nová položka WAF SKU. Tato SKU je k dispozici pouze v modelu zřizování Azure Resource Manager a není v rámci modelu nasazení classic hello. WAF SKU je navíc dostupný jen pro střední a velké instance aplikační brány. Všechny hello limity pro službu application gateway platí také toohello SKU firewall webových aplikací. Ceny jsou založeny na hodinové sazbě za instanci brány a na poplatcích za zpracování dat. Hodinová sazba za bránu se pro položku WAF SKU liší od sazby pro standardní položky SKU. Sazby jsou uvedeny v článku [Podrobnosti o cenách Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/). Zpracování dat, které zůstanou poplatky hello stejné. Neexistují žádné poplatky za pravidla nebo skupiny pravidel. Můžete chránit několika webových aplikací za hello stejné webové aplikace brány firewall a neexistují žádné další poplatky za podporu více aplikací. 

Pro firewall webových aplikací, začne běžet fakturace efektivně 5/5/2017, dokud pak hello brány firewall webových aplikací SKU pokračuje toobe výši standardních sazeb.

## <a name="next-steps"></a>Další kroky

Po dozvědět další informace o možnostech hello firewall webových aplikací, navštivte [jak tooconfigure web application firewall na aplikační brána](application-gateway-web-application-firewall-portal.md).

