---
title: "aspekty aaaSecurity proxy aplikace služby Azure AD | Microsoft Docs"
description: "Popisuje aspekty zabezpečení pomocí proxy aplikace služby Azure AD"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ebd14b9d1fc8f4629c5916e5a910595727d935d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a>Důležité informace o zabezpečení pro přístup k aplikacím vzdáleně pomocí proxy aplikace služby Azure AD

Tento článek vysvětluje, jak Proxy aplikace služby Active Directory Azure poskytuje zabezpečené službu pro publikování a vzdálený přístup k vaší aplikace.

Následující diagram ukazuje, jak Azure AD Hello umožňuje aplikacím místní tooyour zabezpečený vzdálený přístup.

 ![Diagram zabezpečený vzdálený přístup prostřednictvím proxy aplikace služby Azure AD](./media/application-proxy-security-considerations/secure-remote-access.png)

## <a name="security-benefits"></a>Výhody zabezpečení

Azure AD Application Proxy nabízí následující výhody zabezpečení hello:

### <a name="authenticated-access"></a>Ověřený přístup 

Pokud si zvolíte toouse předběžného ověřování Azure Active Directory, můžete přístup pouze ověřené připojení k síti.

Azure AD Application Proxy spoléhá na hello Azure AD služby tokenů zabezpečení (STS) pro všechny ověřování.  Předběžné ověření, ze své podstaty blokuje velký počet anonymní útoky, protože pouze ověřených identit můžete získat přístup k back-end aplikace hello.

Pokud si zvolíte průchozí jako způsob předběžného ověření, neobdržíte této výhody. 

### <a name="conditional-access"></a>Podmíněný přístup

Použijete širší zásad řízení před tooyour sítě jsou vytvořeno připojení.

S [podmíněného přístupu](active-directory-conditional-access-azuread-connected-apps.md), můžete definovat omezení na to, jaký provoz je povoleno tooaccess aplikace back-end. Můžete vytvořit zásady, které omezují přihlášení v závislosti na umístění, sílu ověřování a riziko profilu uživatele.

Můžete také použít zásady podmíněného přístupu tooconfigure služby Multi-Factor Authentication přidáním další úrovně ověřování uživatelů tooyour zabezpečení. 

### <a name="traffic-termination"></a>Ukončení provozu

Veškerý provoz, je ukončen v cloudu hello.

Proxy aplikace služby Azure AD je reverznímu proxy serveru, a proto všechny přenosy tooback-end aplikace se ukončuje na hello služby. Hello relace můžete získat obnovila pouze s hello back-end serverů, což znamená, že jsou vaše servery back-end není vystavený provoz toodirect HTTP. Tato konfigurace znamená, že je líp chráněný před cílenými útoky.

### <a name="all-access-is-outbound"></a>Je odchozí veškerý přístup. 

Nepotřebujete tooopen příchozí připojení toohello podnikové síti.

Konektory Proxy aplikace použít pouze odchozí připojení toohello Azure AD Proxy aplikace služby, což znamená, že není nutné tooopen porty brány firewall pro příchozí spojení. Tradiční proxy vyžaduje hraniční síti (označované také jako *DMZ*, *demilitarizovaná zóna*, nebo *monitorována podsíť*) a povolené toounauthenticated přístup připojení v hraniční síti hello. Tento scénář vyžaduje mnoho dodatečné investice do webové aplikace brány firewall provoz tooanalyze produkty a nabízet přidání ochrany toohello prostředí. Pomocí Proxy aplikace nepotřebujete hraniční síti, protože všechna připojení jsou odchozí a provést přes zabezpečený kanál.

Další informace o konektory najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md).

### <a name="cloud-scale-analytics-and-machine-learning"></a>Analýza cloudové škálování a machine learningu 

Získáte ochranu nejmodernější zabezpečení.

Protože je součástí služby Azure Active Directory, můžete využít Proxy aplikace [Azure AD Identity Protection](active-directory-identityprotection.md), s machine learning řízené intelligence a dat z hello Microsoft Security Response Center a jejichž šíření tým Dcu. Společně jsme aktivně identifikovat ohroženými účty a nabízejí ochranu v reálném čase z s vysokým rizikem přihlášení. Jsme vzít v úvahu mnoha faktorech, například přístup z nakažených zařízení prostřednictvím zavedení anonymity sítě a netypických a pravděpodobně umístění přístup.

Mnoho tyto sestavy a události je již k dispozici prostřednictvím rozhraní API pro integraci s informace o zabezpečení a událostí systémy správy (SIEM).

### <a name="remote-access-as-a-service"></a>Vzdálený přístup jako služby

Nemáte tooworry o údržbu a opravy na místní servery.

Neopravené softwaru stále účty pro velký počet útoků. Proxy aplikace služby Azure AD je internetových služba, která Microsoft vlastní, takže vždy získáte hello nejnovější opravy zabezpečení a upgrady.

tooimprove hello zabezpečení aplikací publikovaných serverem proxy aplikace služby Azure AD, jsme blokovat webové prohledávacího modulu robotů ze indexování a archivaci vaší aplikace. Pokaždé, když robot webový prohledávací modul, pokusí se načíst nastavení robot pro publikované aplikace Proxy aplikace odpoví odpovědí s robots.txt soubor, který zahrnuje `User-agent: * Disallow: /`.

## <a name="under-hello-hood"></a>Pod pokličkou hello

Proxy aplikace služby Azure AD se skládá ze dvou částí:

* cloudové služby Hello: Tato služba běží v Azure a je, kde jsou vytvářeny připojení hello externí klient/uživatel.
* [Hello místní konektor](application-proxy-understand-connectors.md): komponentu místní konektor hello čeká na požadavky od služby Proxy aplikace hello Azure AD a zpracovává připojení toohello interní aplikace. 

Tok mezi hello konektoru a hello Proxy aplikace služby se zjistí při:

* konektor Hello je nejdřív nastavit.
* konektor Hello vrátí informace o konfiguraci z hello Proxy aplikace služby.
* Přístupu uživatele k publikované aplikaci.

>[!NOTE]
>Veškerá komunikace probíhá přes protokol SSL a vždy vycházet v toohello hello konektoru Proxy aplikace služby. Služba Hello je pouze odchozí.

konektor Hello používá toohello tooauthenticate certifikátu klienta služby Proxy aplikace pro téměř všechna volání. Hello pouze výjimka toothis proces je počáteční nastavení krok text hello, kde je vytvořeno hello klientský certifikát.

### <a name="installing-hello-connector"></a>Instalace konektoru hello

Pokud konektor hello je nejdřív nastavený, hello následující události toku proběhnout:

1. jako součást instalace hello hello konektoru se stane služby toohello registrace konektoru Hello. Uživatelé jsou výzvami tooenter přihlašovacích údajů správce Azure AD. Token získali z tohoto ověřování se předloží Azure AD Application Proxy služby toohello.
2. Hello Proxy aplikace služby vyhodnotí hello token. Zajišťuje, že tento uživatel hello je, že správce společnosti v rámci hello klienta, který hello token vydán pro. Pokud hello uživatel není správcem, ukončení procesu hello.
3. konektor Hello generuje žádost o certifikát klienta a předává je, společně s hello tokenu, toohello Proxy aplikace služby. Služba Hello zase ověří hello token a podepíše žádost o certifikát klienta hello.
4. konektor Hello používá hello klientský certifikát pro budoucí komunikaci s hello Proxy aplikace služby.
5. konektor Hello provádí počáteční vyžádání hello systému konfiguračních dat ze služby hello pomocí jeho klientský certifikát a je nyní připraven tootake požadavky.

### <a name="updating-hello-configuration-settings"></a>Aktualizuje se nastavení konfigurace hello

Vždy, když hello konfigurace nastavení aktualizací hello Proxy aplikace služby, hello následující události toku proběhnout:

1. konektor Hello připojuje toohello konfigurace koncového bodu v rámci hello Proxy aplikace služby pomocí jeho klientský certifikát.
2. Po hello klientský certifikát byl ověřen, vrátí hello Proxy aplikace služby konektoru toohello konfigurační data (například skupinu hello konektoru, která hello konektor musí být součástí).
3. Pokud je aktuální certifikát hello víc než 180 dní, konektor hello generuje novou žádost o certifikát, který efektivně aktualizace hello certifikát klienta za 180 dní.

### <a name="accessing-published-applications"></a>Přístup k publikovaným aplikacím

Při přístupu uživatelů k publikované aplikaci, hello následující události proběhnout mezi hello Proxy aplikace služby a konektor Proxy aplikace hello:

1. [Služba Hello ověřuje hello uživatele pro aplikaci hello](#the-service-checks-the-configuration-settings-for-the-app)
2. [Služba Hello umístí požadavek ve frontě konektoru hello](#The-service-places-a-request-in-the-connector-queue)
3. [Konektor zpracuje požadavek hello z fronty hello](#the-connector-receives-the-request-from-the-queue)
4. [konektor Hello čeká na odpověď](#the-connector-waits-for-a-response)
5. [uživatel toohello Hello služby datové proudy dat](#the-service-streams-data-to-the-user)

zachovat čtení toolearn Další informace o co probíhá v každé z těchto kroků.


#### <a name="1-hello-service-authenticates-hello-user-for-hello-app"></a>1. hello služby ověřuje hello uživatele pro aplikaci hello

Pokud jste nakonfigurovali toouse aplikace hello průchozí jako jeho metoda předběžného ověření, hello kroky v této části se přeskočí.

Pokud jste nakonfigurovali toopreauthenticate aplikace hello s Azure AD, jsou uživatelé přesměrovaného toohello tooauthenticate služby tokenů zabezpečení Azure AD a hello tyto kroky provést:

1. Proxy aplikací kontroluje všechny požadavky zásad podmíněného přístupu pro konkrétní aplikaci hello. Tento krok zajistí, že tento hello uživatel má přiřazeno toohello aplikace. Pokud dvoustupňové ověření je potřeba, pořadí hello ověřování vyzve uživatele hello druhé metody ověřování.

2. Po všechny kontroly, hello tokenů zabezpečení Azure AD vydá token podepsané aplikace hello a hello přesměruje uživatele zpět toohello Proxy aplikace služby.

3. Proxy aplikací ověřuje, že tento hello token vydán toocorrect hello aplikace. Také provede další kontroly, například ujistit se, že hello byl podepsán token službou Azure AD a je to pořád se nachází v okně platný hello.

4. Proxy aplikací nastaví tooindicate souboru cookie šifrované ověřování, který ověřování toohello aplikace došlo k chybě. Hello souboru cookie zahrnuje vypršení platnosti časové razítko, který je založen na hello tokenu z Azure AD a další data, jako je na základě hello uživatelské jméno, které hello ověřování. Hello soubor cookie se šifruje pomocí privátní klíč známý jenom toohello Proxy aplikace služby.

5. Aplikace Proxy přesměrování hello uživatele zpět toohello původně požadované adresy URL.

Pokud se libovolná součást hello kroky předběžného ověření nezdaří, hello uživatele požadavek se odmítne a hello uživatele se zobrazí zprávu s upozorněním hello zdroj problému hello.


#### <a name="2-hello-service-places-a-request-in-hello-connector-queue"></a>2. hello služby umístí požadavek ve frontě konektoru hello

Konektory ponechat otevřené toohello Proxy aplikace služby odchozí připojení. Když přijde žádost, fronty služby hello hello požadavek na jednom z hello otevřená připojení pro konektor toopick hello nahoru.

Hello žádost obsahuje položky z hello aplikace, třeba hlavičky žádosti hello, data ze souboru cookie hello zašifrovaná, hello uživatele provedení hello požadavku a hello požadavků ID. I když se s žádostí hello pošle dat z šifrovaného souboru cookie hello ověřovacího souboru cookie hello samotné není.

#### <a name="3-hello-connector-processes-hello-request-from-hello-queue"></a>3. hello konektor zpracuje požadavek hello z fronty hello. 

Na základě žádosti hello provádí Proxy aplikace jednu hello následující akce:

* Pokud požadavek hello je jednoduchá operace (například nejsou žádná data v textu hello, jako je se dosáhl standardu RESTful *získat* požadavek), konektor hello usnadňuje připojení toohello cílový interní prostředek a potom čeká na odpověď.

* Pokud hello žádost obsahuje data přidružená v textu hello (například dosáhl standardu RESTful *POST* operaci), konektor hello vytvoří odchozí připojení pomocí hello klientský certifikát toohello Proxy aplikace instance. Vytvoří tato připojení toorequest hello data a otevřete prostředek interní toohello připojení. Po přijetí žádosti hello z konektoru hello, služba Proxy aplikace hello začne přijímat obsah z hello uživatele a předá konektor toohello dat. Hello konektor, pak předá hello data toohello interního prostředku.

#### <a name="4-hello-connector-waits-for-a-response"></a>4. hello konektor čeká na odpověď.

Po hello požadavku a přenosu veškerý obsah toohello back end je dokončena, konektor hello čeká na odpověď.

Po obdržení odpovědi, díky hello konektoru Proxy aplikace služby toohello odchozí připojení, tooreturn hello podrobnosti záhlaví a začít streamování hello vracená data.

#### <a name="5-hello-service-streams-data-toohello-user"></a>5. hello služby datové proudy dat toohello uživatele. 

Některé zpracování hello aplikace může dojít, zde. Pokud jste nakonfigurovali hlavičky tootranslate Proxy aplikace nebo adresy URL v aplikaci, se stane toto zpracování podle potřeby v tomto kroku.


## <a name="next-steps"></a>Další kroky

[Aspekty topologie sítě při použití proxy aplikace služby Azure AD](application-proxy-network-topology-considerations.md)

[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)
