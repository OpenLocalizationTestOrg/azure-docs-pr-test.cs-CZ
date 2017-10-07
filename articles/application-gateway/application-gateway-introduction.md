---
title: "aaaIntroduction tooAzure aplikační brány | Microsoft Docs"
description: "Tato stránka obsahuje přehled služby Application Gateway hello Vyrovnávání zatížení vrstvy 7, včetně velikosti brány, HTTP Vyrovnávání zatížení, na základě souboru cookie relace spřažení a přesměrování zpracování SSL."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: b37a2473-4f0e-496b-95e7-c0594e96f83e
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: c40c9dba64ab03d9f6f81b3cb8f26c6562ac26c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-application-gateway"></a>Přehled služby Application Gateway

Microsoft Azure Application Gateway je vyhrazené virtuální zařízení poskytující kontroler doručování aplikací (ADC) jako službu. Vaší aplikaci nabízí různé možnosti vyrovnávání zatížení vrstvy 7. To umožňuje zákazníkům toooptimize webové farmy produktivitu přesměrováním zátěže procesoru náročné SSL ukončení toohello aplikační brány. Poskytuje také další možnosti směrování vrstvy 7 včetně kruhové dotazování distribuce příchozí provoz, spřažení na základě souboru cookie relace, na základě cestu směrování adres URL a hello možnost toohost více webů za jeden Application Gateway. Brány firewall webových aplikací (firewall webových aplikací) je k dispozici jako součást hello aplikační brány firewall webových aplikací SKU. Poskytuje ochranu tooweb aplikací z běžných chyb zabezpečení webové a zneužití. Application Gateway je možné nakonfigurovat jako internetovou bránu nebo jen jako interní bránu, případně jako kombinaci obojího. 

![scénář](./media/application-gateway-introduction/scenario.png)

## <a name="features"></a>Funkce

Aplikační brána aktuálně poskytuje hello následující možnosti:


* **[Brány firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)**  -hello brány firewall webových aplikací (firewall webových aplikací) v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace.
* **Vyrovnávání zatížení HTTP** – Služba Application Gateway poskytuje vyrovnávání zatížení kruhovým dotazováním. Vyrovnávání zatížení probíhá na vrstvě 7 a slouží pouze pro přenosy pomocí protokolu HTTP nebo HTTPS.
* **Spřažení na základě souboru cookie relace** – funkce spřažení na základě souboru cookie relace hello je užitečné, když chcete tookeep a uživatelskou relací na hello stejnou back-end. Pomocí brány spravovat soubory cookie hello Application Gateway je možné toodirect následné provoz z toohello relace uživatel stejnou back-end pro zpracování. Tato funkce je důležité v případech, kde stav relace je uloženy lokálně na hello back-end serverů pro uživatelské relace.
* **[Zabezpečené snižování zátěže Sockets Layer (SSL)](application-gateway-ssl-arm.md)**  – tato funkce používá hello nákladná úlohy dešifrování provoz HTTPS vypnout webových serverů. Podle ukončující hello připojení SSL na hello aplikační brány a předávání hello požadavek toohello server bez šifrování je pomocí dešifrování unburdened hello webový server.  Aplikační brána znovu je zašifruje hello odpověď před odesláním zpět toohello klienta. Tato funkce je užitečný ve scénářích, kde se nachází hello back-end v hello stejné zabezpečené virtuální sítě jako hello Aplikační brána v Azure.
* **[Ukončení tooEnd SSL](application-gateway-backend-ssl.md)**  -Application Gateway podporuje ukončení tooend šifrování přenosů. Aplikační brána dosahuje tím, že se ukončuje připojení SSL hello na hello aplikační brány. Brána Hello poté použije pravidla směrování hello toohello provoz, znovu je zašifruje hello paketů a předá hello paketu toohello odpovídající back-end na základě pravidel směrování hello definované. Odpověď od hello webový server, na které se prochází hello stejný proces back toohello koncového uživatele.
* **[Na základě adresy URL obsahu směrování](application-gateway-url-route-overview.md)**  – tato funkce poskytuje schopnost hello toouse různých back-end serverů pro jiný přenos. Provoz pro složku na hello webového serveru nebo název CDN může být směrované tooa různých back-end. Tato schopnost snižuje nepotřebné zatížení na back-endech, které neposkytují konkrétní obsah.
* **[Více lokalit směrování](application-gateway-multi-site-overview.md)**  -Aplikační brána umožňuje tooconsolidate až too20 weby na bránu jednu aplikaci.
* **[Podpora protokolu Websocket](application-gateway-websocket.md)**  -jiný skvělé funkce Application Gateway je hello nativní podpora protokolu Websocket.
* **[Sledování stavu](application-gateway-probe-overview.md)**  -Application gateway poskytuje výchozí stav monitorování prostředků back-end a vlastní testy toomonitor pro více konkrétních scénářů.
* **[Zásady protokolu SSL a šifry](application-gateway-ssl-policy-overview.md)**  – tato funkce poskytuje možnost hello verzí protokolu SSL hello toolimit a hello šifer sady, které jsou podporovány a hello pořadí, ve kterém jsou zpracovány.
* **[Vyžádat přesměrování](application-gateway-redirect-overview.md)**  – tato funkce poskytuje hello schopností tooredirect HTTP požadavků tooan naslouchací proces HTTPS.
* **[Podpora back-endu s více tenanty](application-gateway-web-app-overview.md)** – Služba Application Gateway podporuje konfiguraci služeb back-end s více tenanty, jako je Azure Web Apps a brána rozhraní API, jako členy fondu back-end. 
* **[Rozšířená diagnostika](application-gateway-diagnostics.md)** – Služba Application Gateway poskytuje úplnou diagnostiku a protokoly přístupů. Protokoly brány firewall jsou dostupné pro prostředky služby Application Gateway, které mají povolený Firewall webových aplikací.

## <a name="benefits"></a>Výhody

Služba Application Gateway je užitečná pro:

* Aplikace, které vyžadují požadavky z hello stejné tooreach relace uživatele/client hello stejnou back-end virtuálního počítače. Příklady těchto aplikací by mohly být aplikace nákupních košíků a servery webové pošty.
* Odstranění režie při ukončování protokolu SSL pro farmy webových serverů.
* Aplikace, jako je například síti pro doručování obsahu, který vyžaduje více požadavky HTTP na hello stejné toobe připojení TCP dlouho běžící v směrují nebo načíst vyrovnáváním toodifferent back-end serverů.
* Aplikace, které podporují přenos pomocí protokolu WebSocket.
* Ochranu webových aplikací před běžnými webovými útoky, jako jsou například útoky prostřednictvím injektáže SQL, skriptování mezi weby a napadení relace.
* Logickou distribuci přenosu na základě různých kritérií směrování, jako je například cesta URL nebo hlavičky domény.

Službu Application Gateway je možné plně spravovat v Azure, je škálovatelná a vysoce dostupná. Nabízí celou řadu možností diagnostiky a protokolování, které zlepšují správu. Když vytvoříte aplikační bránu, bude příchozímu síťovému přenosu přidružen koncový bod (veřejná virtuální IP adresa nebo interní IP adresa interního nástroje na vyrovnávání zatížení), který se pro něj použije. Tento virtuální IP adresy nebo ILB IP zajišťuje Vyrovnávání zatížení Azure práce na úrovni přenosu hello (TCP/UDP) a s všechny příchozí síťový provoz se Aplikační brána toohello vyrovnáváním zatížení instancí pracovního procesu. Hello Aplikační brána pak trasy hello přenosy HTTP/HTTPS, jestli je virtuální počítač, v závislosti na jeho konfiguraci Cloudová služba, interní nebo externí IP adresu.

Aplikační brána Vyrovnávání zatížení jako služby spravovat Azure umožňuje hello zřizování nástroje pro vyrovnávání zatížení vrstvy 7 za hello Azure softwarovému Vyrovnávání zatížení. Správce provozu může být použité toocomplete hello scénář, jak je vidět v hello následující bitové kopie, kde Traffic Manager poskytuje přesměrování a dostupnosti provozu prostředky brány toomultiple aplikace v různých oblastech, zatímco poskytuje aplikační brány Křížová Vyrovnávání zatížení vrstvy 7 oblast. Příklad tohoto scénáře naleznete na adrese: [pomocí zátěže služeb v cloudu Azure hello](../traffic-manager/traffic-manager-load-balancing-azure.md)

![scénář Traffic Manageru a služby Application Gateway](./media/application-gateway-introduction/tm-lb-ag-scenario.png)

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Velikosti a instance brány

Služba Application Gateway je v současné době nabízena ve třech velikostech: **Small** (krátkodobé používání), **Medium** (střednědobé používání) a **Large** (dlouhodobé používání). Instance krátkodobého používání jsou určené pro scénáře vývoje a testování.

Můžete vytvořit až too50 application Gateway na jedno předplatné, a až too10 instance může mít každý aplikační brány. Každá služba Application Gateway se může skládat z 20 naslouchacích procesů HTTP. Úplný seznam omezení služby Application Gateway najdete na stránce [Omezení služby Application Gateway](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

Hello následující tabulka uvádí s propustností průměrná výkonu pro každou instanci brány aplikace s povolené přesměrování zpracování SSL:

| Odezva back-endové stránky | Krátkodobé používání | Střednědobé používání | Dlouhodobé používání |
| --- | --- | --- | --- |
| 6K |7,5 Mb/s |13 Mb/s |50 Mb/s |
| 100K |35 Mb/s |100 Mb/s |200 Mb/s |

> [!NOTE]
> Tyto hodnoty jsou přibližné hodnoty propustnosti služby Application Gateway. Skutečná propustnost Hello závisí na různé podrobnosti prostředí, jako je například velikost průměrná stránky umístění instancí back-end a tooserve čas zpracování stránky. Přesné údaje o výkonu získáte, když spustíte vlastní testy. Tyto hodnoty slouží jenom jako vodítko při plánování kapacity.

## <a name="health-monitoring"></a>Monitorování stavu

Služba Azure Application Gateway automaticky monitoruje stav hello hello back-end instancí prostřednictvím sondy základním nebo vlastním stavu. Pomocí sondy stavu služby zajišťuje, že pouze v pořádku hostitelích reagovat tootraffic. Další informace najdete v tématu [Přehled monitorování stavu ve službě Application Gateway](application-gateway-probe-overview.md).

## <a name="configuring-and-managing"></a>Konfigurace a správa

Služba Application Gateway může pro svůj koncový bod při konfiguraci mít veřejnou IP adresu, privátní IP adresu nebo obojí. Služba Application Gateway je nakonfigurována ve virtuální síti ve vlastní podsíti. Hello podsítě vytvořit nebo použít pro službu application gateway nemůže obsahovat u jiných typů prostředků, hello jenom prostředky, které jsou povoleny v podsíti hello jsou ostatní application Gateway. toosecure back-endové prostředky, hello back-end serverů může být obsažený v jiné podsíti v hello stejné virtuální síti jako hello aplikační brány. Tato podsíť, které není potřeba pro back-end aplikace hello. Tak dlouho, dokud hello aplikační bránu můžete dostat hello ip adresu, application gateway je možné tooprovide ADC možnosti pro hello back-end serverů. 

Službu Application Gateway můžete vytvořit a spravovat pomocí rozhraní REST API, rutin prostředí PowerShell, rozhraní příkazového řádku Azure nebo webu [Azure Portal](https://portal.azure.com/). Pro další dotazy na aplikační brány naleznete [Application Gateway – nejčastější dotazy](application-gateway-faq.md) tooview seznam běžné nejčastější dotazy.

## <a name="pricing"></a>Ceny

Ceny jsou založeny na hodinové sazbě za instanci brány a na poplatcích za zpracování dat. Za hodinu hello firewall webových aplikací SKU brány ceny se liší od standardní SKU poplatky. Informace o cenách najdete v tématu [Podrobnosti o cenách Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/). Zpracování dat, které zůstanou poplatky hello stejné.

## <a name="faq"></a>Nejčastější dotazy

Nejčastější dotazy k službě Application Gateway najdete v tématu [Nejčastější dotazy k Application Gateway](application-gateway-faq.md).

## <a name="next-steps"></a>Další kroky

Po získání informací o aplikační bránu, můžete [vytvoření služby application gateway](application-gateway-create-gateway-portal.md) nebo můžete [vytvoření služby application gateway přesměrování zpracování SSL](application-gateway-ssl-arm.md) tooload vyrovnávání připojení prostřednictvím protokolu HTTPS.

toolearn jak toocreate služby application gateway pomocí adresy URL na základě obsahu směrování přejděte příliš[vytvoření služby application gateway pomocí směrování na základě adresy URL](application-gateway-create-url-route-arm-ps.md) Další informace.

toolearn o některých hello Další klíč sítě možnosti Azure najdete v tématu [sítě Azure](../networking/networking-overview.md).
