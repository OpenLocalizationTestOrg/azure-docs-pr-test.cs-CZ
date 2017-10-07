---
title: "aaaCloud aplikace zjišťování zabezpečení a důležité informace o ochraně osobních údajů | Microsoft Docs"
description: "Toto téma popisuje hello zabezpečení a ochrany osobních údajů aspekty související tooCloud App Discovery."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 2fce5c82-d3de-4097-808f-40214768df9e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 33659e85bd2cf4294e443512e69a85401f7c53f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App Discovery zabezpečení a důležité informace o ochraně osobních údajů
Společnost Microsoft je potvrzená tooprotecting vašich osobních údajů a zabezpečení vašich dat při dodávání softwaru a služeb, které vám pomohou při správě hello zabezpečení vaší organizace.  
Uvědomujeme si, že pokud jste svěřit tooothers vaše data, tohoto vztahu důvěryhodnosti vyžaduje přísných zabezpečení investic a znalosti tooback ho.
Microsoft dodržuje toostrict dodržování předpisů a pokyny pro zabezpečení ze zabezpečení softwaru vývoj životního cyklu postupy toooperating služby.  
Zabezpečení a ochrana dat je nejvyšší prioritou ve společnosti Microsoft.

Toto téma vysvětluje, jak data jsou shromažďována, zpracování a zabezpečené v rámci Azure Active Directory Cloud App Discovery

## <a name="overview"></a>Přehled
Cloud App Discovery je funkce služby Azure AD a je hostován v Microsoft Azure.  
Hello Cloud App Discovery endpoint agent je použité toocollect aplikace data zjišťování od IT spravované počítače.  
Hello shromážděná data se odešlou bezpečně přes toohello šifrovaný kanál služby Azure AD Cloud App Discovery.  
Hello Cloud App Discovery data v organizaci se pak zobrazí na hello portálu Azure. 

![Jak funguje Cloud App Discovery.](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png) 

Hello následující části použijte hello toku informací a popisují, jak jsou zabezpečená při jejich přesunu ze služby Cloud App Discovery toohello organizace a nakonec toohello portál Cloud App Discovery.

## <a name="collecting-data-from-your-organization"></a>Shromažďování dat z vaší organizace
Pořadí toouse Azure Active Directory pro cloudové aplikace zjišťování funkce tooget přehledy hello aplikace používané zaměstnanci ve vaší organizaci, je nutné toofirst nasazení hello Azure AD Cloud App Discovery endpoint agent toomachines ve vaší organizaci.

Správci klienta Azure Active Directory hello (nebo jejich delegáta) mohou stáhnout instalační balíček agenta hello z hello portál Azure. Hello agenta můžete buď ručně nainstalovány nebo nainstalován napříč více počítačů v organizaci hello pomocí SCCM nebo zásad skupiny.

Další pokyny o možnostech nasazení najdete v tématu [cloudové aplikace zjišťování skupiny zásad Deployment Guide](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).


### <a name="data-collected-by-hello-agent"></a>Údaje shromážděné agentem hello
Hello informace uvedené v následujícím seznamu hello shromažďuje hello agenta, když připojení tooa webové aplikace. Hello se shromáždí informace o pouze u aplikací, má tento správce hello nakonfigurován pro zjišťování.  
Můžete upravit hello seznam cloudové aplikace, které hello agenta monitorování prostřednictvím hello Cloud App Discovery okno v hello Microsoft [portál Azure](https://portal.azure.com/)v části **nastavení**->**dat Kolekce**->**seznam kolekcí aplikace**. Další podrobnosti najdete v tématu [získávání spuštěna s Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


**Informace o kategorii**: informace o uživateli  
**Popis**:  
uživatelské jméno systému Windows Hello hello procesu, který vytvořil požadavek toohello cíl webové aplikace (např: doména\uživatelské jméno) a také hello identifikátor zabezpečení systému Windows (SID) uživatele hello.

**Informace o kategorii**: zpracovat informace  
**Popis**:  
Název Hello hello procesu, který vytvořil hello požadavek toohello cílové webové aplikace (například: "iexplore.exe")

**Informace o kategorii**: počítač informace  
**Popis**:  
název NetBIOS počítače Hello ve které hello je nainstalován agent.

**Informace o kategorii**: informace o provozu aplikaci.  
**Popis**: 

Hello následující informace o připojení:

* Zdroj Hello (místní počítač) a cílové IP adresy a čísla portů
* Hello veřejnou IP adresu hello organizace, prostřednictvím které hello žádost prochází.
* čas Hello hello požadavku
* Hello objem provozu, odesílání a přijímání
* verze IP Hello (4 nebo 6)
* Pro připojení protokolu TLS pouze: název hostitele cílového hello z rozšíření indikace názvu serveru hello nebo hello certifikát serveru.

Následující informace HTTP Hello:

* Metoda (GET, POST atd.)
* Protokol (HTTP/1.1, atd.)
* Řetězec uživatelského agenta
* Název hostitele
* Cílový identifikátor URI (s výjimkou řetězce dotazu)
* Informace o typu obsahu
* Informace o adresách URL odkazující server (s výjimkou řetězce dotazu)

> [!NOTE]
> Hello HTTP výše uvedené informace se shromažďují pro všechna připojení bez šifrování.
> Pro připojení protokolu TLS se tyto informace zaznamená pouze když je zapnuté nastavení "Hloubkové kontrole" hello hello portálu. nastavení Hello je 'ON' ve výchozím nastavení.
> Další podrobnosti najdete v tématu níže, a [získávání spuštěna s Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 

Kromě toohello data tohoto agenta hello shromažďuje o hello síťové aktivity, je také shromažďovat anonymní informace o hello softwarové a hardwarové konfigurace, zprávy o chybách a informace o tom, jak je používán hello agent.


### <a name="how-hello-agent-works"></a>Jak funguje hello agenta
Instalace agenta Hello obsahuje dvě součásti:

* Součást uživatelského režimu
* Součást ovladače režimu jádra (ovladače Windows Filtering Platform)

Při první instalaci agenta hello ukládá specifické pro počítač důvěryhodný certifikát na počítači hello který pak použije tooestablish zabezpečeného připojení se službou Cloud App Discovery hello.  
Hello agent pravidelně načte konfiguraci zásad z hello služby Cloud App Discovery přes tento zabezpečené připojení.  
Hello zásady obsahují informace o toomonitor které cloudové aplikace a jestli automatické aktualizace by měla být povolená, mimo jiné.

Jako webový provoz se odesílají a přijímají na počítači hello z Internet Exploreru a Chrome, hello Cloud App Discovery agent analyzuje hello provoz a extrahuje hello relevantní metadata (viz hello **Data shromážděná pomocí agenta hello** části výše).  
Každou minutu hello agent odešle hello shromážděných metadat toohello služby Cloud App Discovery přes šifrovaný kanál.

Hello ovladač součást zachycuje hello šifrovaný provoz a sám vloží do šifrované datového proudu hello. Další podrobnosti v hello **zachycení dat ze šifrované připojení (při hloubkové kontrole)** části níže.

### <a name="respecting-user-privacy"></a>Bere ohledy na soukromí uživatelů
Naším cílem je tooprovide správci hello nástroje tooset hello rovnováhu mezi podrobné kabel do aplikace využití a uživatele o ochraně osobních údajů v závislosti na jejich organizace. toothat end poskytujeme hello následující knoflíky v hello nastavení na stránce hello portálu:

* **Shromažďování dat**: můžou správci rozhodnout toospecify, které aplikace nebo chtějí na data zjišťování tooget kategorií aplikací.
* **Při hloubkové kontrole**: Správci můžete zvolit toospecify, pokud hello agent shromažďuje přenos HTTP pro připojení protokolem SSL/TLS (neboli **'Hloubkové kontrole'**). Další informace o to v další části hello.
* **Souhlas možnosti**: Správci mohou pomocí portálu toochoose hello Cloud App Discovery. jestli uživatelé toonotify shromažďování dat hello hello Agent a zda toorequire uživatele souhlas před zahájením hello agenta shromažďování dat uživatele.

Hello Cloud App Discovery endpoint agent shromažďuje jenom hello informace popsané v hello **Data shromážděná pomocí agenta hello** část výše.

### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Zachycení dat ze šifrované připojení (při hloubkové kontrole)
Jak jsme už zmínili dřív, správci mohou nakonfigurovat hello agenta toomonitor data z šifrované připojení ('hloubkové kontrole'). Protokol TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) je jedním z hello dnes nejběžnější protokoly na hello Internetu používá. Šifrování komunikace s TLS, klient může vytvořit zabezpečený a privátní komunikaci s webového serveru; Protokol TLS poskytuje základní ochranu pro předávání pověření ověřování a zabránit hello úniku citlivých informací.

Při hello začátku do konce zabezpečené šifrovaný kanál poskytované TLS umožňuje důležité zabezpečení a ochrany osobních údajů, je pro účely škodlivý nebo nefarious často zneužít hello protokolu. Mnohem tedy ve skutečnosti TLS se často označuje tooas hello (universal obcházení brány firewall protocol). kořenové Hello hello problému je, že většinu bran firewall komunikace TLS nelze tooinspect protože hello aplikační vrstvě data se šifrují pomocí SSL. Zároveň budete vědět, útočníci často využívají TLS toodeliver škodlivý datových částí tooa uživatele přesvědčeni, že jsou úplně skrytá tooTLS i hello nejvíce inteligentního aplikační vrstvě brány firewall a musí jednoduše předávání TLS komunikace mezi hostiteli. Koncoví uživatelé často využívají TLS toobypass přístup k ovládacím prvkům vynucují podle podnikových bran firewall a proxy servery, používat proxy toopublic tooconnect a pro protokoly – protokol TLS přes bránu firewall hello, který může být jinak blokovaly zásadami pro tunelové propojení.

Při hloubkové kontrole může hello Cloud App Discovery agent tooact jako důvěryhodné man-in-the-middle. Při požadavku klientů tooaccess HTTPS chráněný prostředek, hello Endpoint Agent ovladač zachycuje hello připojení a vytváří tooretrieves cílový server sady nové připojení toohello svůj certifikát SSL jménem klienta hello. Hello agent pak ověří, že hello certifikát může být důvěryhodný (Kontrola, zda nebyl odvolaný a provádět další kontroly certifikátu), a pokud tyto podmínky, hello Endpoint Agent pak zkopíruje hello informace z hello certifikát serveru a vytvoří vlastní certifikát serveru – známý jako certifikát zachycení – pomocí těchto informací. Hello zachycení certifikát je podepsaný na průběžně hello endpoint Agent kořenový certifikát, který je nainstalován v úložišti důvěryhodných certifikátů Windows hello. Tento certifikát podepsaný svým držitelem kořenové neexportovatelného je označena a seznamu ACL měl tooadministrators. Je určený toonever ponechejte hello počítače, v němž byla vytvořena. Když hello koncového uživatele klientská aplikace obdrží hello zachycení certifikát, bude ho ho důvěřujete, protože můžete úspěšně ověřit řetěz certifikátů hello všechny hello způsob toohello kořenový certifikát. Tento proces je ve většině případů transparentní z Koncoví uživatele hlediska se několik aspektů, jak je popsáno níže.

Povolením hloubkové kontrole hello Cloud App Discovery Endpoint Agent můžou dešifrovat a kontrolovat TLS zašifrovaná komunikace, povolení hello služby tooreduce šumu a poskytnou přehled o využití hello hello šifrované cloudových aplikací.

#### <a name="a-word-of-caution"></a>Word varování
Před zapnutím hloubkové kontroly, důrazně doporučujeme komunikovat vaše záměry tooyour právní a Personálního oddělení a získat jejich souhlasu. Zkontrolujete privátní šifrování komunikace koncový uživatel může být citlivé předmět, zřejmé důvodů. Předtím, než produkční zavádění hloubkové kontroly, ujistěte se, že vaše podnikové zabezpečení a podmínky použití zásady byly aktualizovány bude prozkoumá tooindicate, který šifrované komunikace. Oznámení pro uživatele a výjimky webů považují za citlivé (například bankovnictví a lékařské webů) může být také nutné v případě, že nakonfigurujete toomonitor Cloud App Discovery je. Jak je uvedeno nahoře, mohou správci portálu toochoose hello Cloud App Discovery. jestli uživatelé toonotify shromažďování dat hello hello Agent a zda toorequire uživatele souhlas před zahájením hello agenta shromažďování dat uživatele.

### <a name="known-issues-and-drawbacks"></a>Známé problémy a nevýhody
Existuje několik případů, kdy zachycení TLS může mít vliv na prostředí koncového uživatele hello:

* Rozšířené certifikáty ověření (EV) vykreslit hello adresního řádku hello webové prohlížeče zelená tooact vizuálně indikují, že navštívíte důvěryhodného webu. TLS kontroly nelze duplicitní EV v hello certifikát, který vystavuje toohello klienta, aby webové servery, které používají certifikáty EV fungovat normálně, ale nezobrazí adresním řádku hello zelená.  
* Veřejné klíče Připnutí (také označované jako Připnutí certifikátu) jsou navržené tak toohelp chránit uživatele před útoky man-in-the-middle a podvodné certifikačních autorit. Když hello kořenový certifikát pro lokalitu definovaného neodpovídá jeden hello známé dobré Certifikační autority, prohlížeč hello odmítne hello připojení se stala chyba. Vzhledem k tomu, že protokol TLS zachycení je ve skutečnosti man-in-the-middle, tato připojení se nezdaří.
* Pokud uživatelé klikněte na ikonu zámku hello v hello prohlížeče adresu řádku prohlížeče tooinspect hello informace o lokalitě, neuvidí řetěz končící na hello certifikační autorita používá certifikát webu hello toosign, ale místo toho řetěz certifikátů končí hello Windows úložiště důvěryhodných certifikátů.

tooreduce hello výskyty tyto problémy, budeme sledovat cloudové služby a klientské aplikace, které jsou známé toouse rozšířené ověření nebo veřejného klíče Připnutí a vyzvat tooavoid Endpoint Agent hello brání ovlivněné připojení. I v těchto případech bude nadále získávat sestavy hello použití těchto cloudových aplikací a hello objem přenášených dat, ale vzhledem k tomu, že nejsou hloubkové prověřovány, bude k dispozici žádné podrobnosti o použití aplikace hello byly.

## <a name="sending-data-toocloud-app-discovery"></a>Odesílání dat tooCloud App Discovery
Jakmile metadata jsou shromážděná agentem hello, se uloží do mezipaměti v počítači hello až tooone minutu, nebo dokud hello data uložená v mezipaměti dosáhne velikosti 5 MB. Pak je komprimované a odešlou přes zabezpečené připojení toohello služby Cloud App Discovery.

Pokud je hello agent nelze toocommunicate s hello služby Cloud App Discovery z jakéhokoli důvodu, hello shromážděných metadata se ukládají do mezipaměti místního souboru, který lze přistupovat pouze mohou uživatelé s oprávněním na počítači hello (například skupiny Administrators hello).  
Hello agent automaticky pokusy o tooresend hello uloží do mezipaměti metadat dokud úspěšně byla přijata službou hello Cloud App Discovery.

## <a name="receiving-hello-data-at-hello-service-end"></a>Přijetí dat hello na konci služby hello
agenti Hello ověřit toohello Cloud App Discovery služby pomocí hello počítače konkrétní klientský ověřovací certifikát výše uvedené a předává data přes šifrovaný kanál.  
služby Cloud App Discovery Hello kanálu analýzy procesy metadata pro každého zákazníka a to samostatně pomocí logicky vytváření oddílů všechny fázemi kanálu analýzy hello.
Hello analyzovány metadata jednotky hello různé sestavy portálu hello.

Hello nezpracované metadata a hello analyzovali metadata se ukládají až too180 dnů. Kromě toho můžete zákazníkům toocapture hello analyzovali metadata v účtu úložiště objektů blob v Azure podle vlastního uvážení.
To je užitečné pro offline analýzu metadat, jakož i déle uchovávání dat hello.

## <a name="accessing-hello-data-using-hello-azure-portal"></a>Přístup k datům hello pomocí hello portálu Azure
V metadatech hello tookeep úsilí shromážděných zabezpečený a ve výchozím nastavení jenom globální Správci klienta hello mají funkce Cloud App Discovery toohello přístupu v hello portálu Azure.  
Však mohou správci toodelegate tento přístup tooother uživatele nebo skupiny.

> [!NOTE]
> Další podrobnosti najdete v tématu [získávání spuštěna s Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 


Žádná data uživatelů přístupem hello hello portálu, musí mít licenci s licencí Azure AD Premium.

## <a name="additional-resources"></a>Další zdroje
* [Jak může zjišťovat nedovolené cloudové aplikace, které se používají v rámci Moje organizace](active-directory-cloudappdiscovery-whatis.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

