---
title: "aaaDNS řešení analýzy v Azure Log Analytics | Microsoft Docs"
description: "Nastavit a použít řešení hello DNS analýzy v analýzy protokolů toogather přehledy infrastruktura služby DNS na zabezpečení, výkonu a operací."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f44a40c4-820a-406e-8c40-70bd8dc67ae7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.openlocfilehash: be7982c54b65ba0c4b1c15ae7516d02eced313f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="gather-insights-about-your-dns-infrastructure-with-hello-dns-analytics-preview-solution"></a>Shromažďovat statistiky o infrastruktury služby DNS s hello DNS Analytics Preview řešení

![Symbol DNS Analytics](./media/log-analytics-dns/dns-analytics-symbol.png)

Tento článek popisuje, jak tooset nahoru a používání hello řešení Azure DNS analýzy v Azure Log Analytics toogather přehledy infrastruktura služby DNS na zabezpečení, výkonu a operací.

Analýza DNS vám pomůže:

- Identifikaci klientů, které se pokusí tooresolve škodlivý doménách.
- Identifikujte zastaralé záznamy o prostředcích.
- Identifikujte často dotazovaných doménách a talkative klienty DNS.
- Zobrazení žádosti o zatížení serverů DNS.
- Zobrazení dynamické chyby registrace DNS.

řešení Hello shromažďuje, analyzuje a korelaci DNS Windows analýzy a protokoly auditu a další související data z vašich serverů DNS.

## <a name="connected-sources"></a>Připojené zdroje

Hello následující tabulka popisuje hello připojené zdroje, které podporuje toto řešení:

| **Připojené zdroje** | **Podpora** | **Popis** |
| --- | --- | --- |
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ano | řešení Hello shromažďuje informace DNS z agentů v systému Windows. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne | řešení Hello neshromažďuje informace DNS z přímé agenty Linux. |
| [Skupina pro správu System Center Operations Manager](log-analytics-om-agents.md) | Ano | řešení Hello shromažďuje informace DNS z agentů v připojené skupině pro správu nástroje Operations Manager. Přímé připojení z toohello agenta nástroje Operations Manager hello Operations Management Suite se nevyžaduje. Data se předají z hello správy skupiny toohello Operations Management Suite úložiště. |
| [Účet služby Azure Storage](log-analytics-azure-storage.md) | Ne | Úložiště Azure není používán hello řešení. |

### <a name="data-collection-details"></a>Podrobnosti kolekce dat

Hello řešení shromažďuje DNS inventář a data související s DNS ze serverů DNS hello, kde je nainstalován agent analýzy protokolů. Tato data potom odeslán tooLog analýzy a zobrazují na řídicím panelu řešení hello. Shromažďuje data související s inventářem, jako je například hello počet serverů DNS, zóny a záznamy o prostředcích, spuštěním rutin prostředí PowerShell DNS hello. Hello data se aktualizují každých dvou dnů. Hello událost související data se shromažďují téměř v reálném čase ze hello [analýzy a protokoly auditu](https://technet.microsoft.com/library/dn800669.aspx#enhanc) poskytované rozšířené protokolování DNS a Diagnostika ve Windows serveru 2012 R2.

## <a name="configuration"></a>Konfigurace

Použijte následující informace tooconfigure hello řešení hello:

- Musíte mít [Windows](log-analytics-windows-agents.md) nebo [nástroje Operations Manager](log-analytics-om-agents.md) agent na každém serveru DNS, které chcete toomonitor.
- Můžete přidat hello pracovní prostor služby Operations Management Suite tooyour DNS analýzy řešení z hello [Azure Marketplace](https://aka.ms/dnsanalyticsazuremarketplace). Můžete taky hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).

řešení Hello spustí shromažďování dat bez nutnosti hello další konfigurace. Můžete však použít následující konfigurace shromažďování dat toocustomize hello.

### <a name="configure-hello-solution"></a>Konfigurace řešení hello

Na řídicí panel řešení hello, klikněte na tlačítko **konfigurace** tooopen hello konfigurace DNS analýzy stránky. Existují dva typy změny konfigurace, které můžete použít:

- **Názvů domén seznam povolených adres**. řešení Hello nezpracovává všechny dotazy vyhledávání hello. Udržuje povolených přípon názvů domén. Hello vyhledávací dotazy, které překládat názvy domén toohello, které odpovídají přípony názvů domén v této povolených nejsou zpracovávány hello řešení. Nezpracuje názvy domén seznam povolených adres pomáhá toooptimize hello data odeslaná tooLog Analytics. Hello výchozí seznam povolených adres obsahuje názvy oblíbených veřejné domény, například www.google.com a www.facebook.com. Posouvání, můžete zobrazit seznam dokončení výchozí hello.

 Můžete upravit tooadd hello seznam přípon názvů domén, který chcete tooview vyhledávání insights pro. Můžete také odebrat přípon názvů domén, které nechcete tooview vyhledávání insights pro.

- **Prahová hodnota talkative klienta**. Klienty DNS, které překračují hello prahová hodnota pro počet hello vyhledávání požadavků, které jsou vyznačené na hello **klienty DNS** okno. Hello výchozí prahová hodnota je 1 000. Můžete upravit prahové hodnoty hello.

    ![Seznam povolených adres názvů domén](./media/log-analytics-dns/dns-config.png)

## <a name="management-packs"></a>Sady Management Pack

Pokud používáte hello agenta Microsoft Monitoring Agent tooconnect tooyour pracovní prostor služby Operations Management Suite, hello následující sady management pack je nainstalována:

- Microsoft DNS dat kolekce Intelligence Pack (Microsft.IntelligencePacks.Dns)

Pokud pracovní prostor služby Operations Management Suite tooyour připojené skupině pro správu nástroje Operations Manager se hello následující sady management Pack jsou nainstalovány v nástroji Operations Manager při přidání tohoto řešení. Neexistuje žádné požadovanou konfiguraci nebo údržby těchto sad management Pack:

- Microsoft DNS dat kolekce Intelligence Pack (Microsft.IntelligencePacks.Dns)
- Konfigurace DNS analýzy Microsoft System Center Advisor (Microsoft.IntelligencePack.Dns.Configuration)

Další informace o tom, jak jsou aktualizovány sady management Pack řešení najdete v tématu [tooLog připojení nástroje Operations Manager Analytics](log-analytics-om-agents.md).

## <a name="use-hello-dns-analytics-solution"></a>Použít řešení DNS Analytics hello

Tato část vysvětluje všechny funkce hello řídicího panelu a jak toouse je.

Po přidání prostoru tooyour hello řešení, dlaždice hello řešení na stránce Přehled služby Operations Management Suite hello poskytuje rychlý přehled infrastruktury služby DNS. Obsahuje hello počet serverů DNS, kde jsou shromažďována hello data. Zahrnuje také hello počet žádostí odeslaných klienty tooresolve škodlivý domény ve hello posledních 24 hodin. Když kliknete na dlaždici hello, otevře se řídicí panel řešení hello.

![Dlaždice DNS Analytics](./media/log-analytics-dns/dns-tile.png)

### <a name="solution-dashboard"></a>Řídicí panel řešení

řídicí panel řešení Hello obsahuje souhrnné informace o hello různých funkcí hello řešení. Obsahuje taky odkazy toohello podrobné zobrazení pro forenzní analýzy a diagnostiku. Ve výchozím nastavení zobrazí hello data pro hello posledních sedmi dnů. Datum a čas rozsah hello můžete změnit pomocí hello **ovládacího prvku pro výběr data a času**, jak ukazuje následující obrázek hello:

![Ovládacího prvku pro výběr čas](./media/log-analytics-dns/dns-time.png)

řídicí panel řešení Hello ukazuje hello následující okna:

**Zabezpečení DNS**. Sestavy hello klienty DNS, které se pokoušíte toocommunicate se zlými úmysly doménami. Pomocí Microsoft threat intelligence kanály můžete DNS Analytics detekuje klienta IP adres, který se pokoušíte tooaccess škodlivý domén. V mnoha případech hello nakažená malwarem zařízení "provádí volání" toohello "příkazy a ovládání" center hello škodlivý domény vyřešte malwaru název domény.

![Okno zabezpečení DNS](./media/log-analytics-dns/dns-security-blade.png)

Když kliknete na IP adresu klienta v seznamu hello, hledání protokolů otevře a zobrazí podrobnosti o vyhledávání hello hello příslušných dotazu. V následující ukázka hello, DNS Analytics zjištěno, že komunikace hello bylo provedeno s [IRCbot](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=Win32/IRCbot):

![Výsledky hledání protokolu zobrazující ircbot](./media/log-analytics-dns/ircbot.png)

Hello informace vám pomůžou tooidentify:

- Klient IP, která iniciovala hello komunikace.
- Název domény, která přeloží toohello škodlivé IP.
- IP adresy, které hello překládá název domény.
- Škodlivé IP adresa.
- Závažnost problému hello.
- Důvod seznamů zakázaných hello škodlivé IP.
- Čas detekce.

**Domény dotaz**. Poskytuje nejčastěji se vyskytující názvy domén hello která je dotazována hello klienty DNS ve vašem prostředí. Můžete zobrazit hello seznam všech názvů domény hello dotaz. Můžete také přejít dolů do hello vyhledávání žádost o podrobnosti o konkrétní doménu v hledání protokolů.

![Okno dotazovaný domén](./media/log-analytics-dns/domains-queried-blade.png)

**Klienti DNS**. Sestavy hello klientům *porušení prahové hodnoty hello* pro počet dotazů v hello zvolené časové období. Můžete zobrazit seznam hello všech klientů DNS hello a podrobnosti hello hello dotazů je provedené v protokolu vyhledávání.

![Okno klienty DNS](./media/log-analytics-dns/dns-clients-blade.png)

**Registrace serveru DNS dynamické**. Sestavy název chyby registrace. Všechny chyby při registraci pro adresu [záznamy o prostředcích](https://en.wikipedia.org/wiki/List_of_DNS_record_types) (typ A a AAAA) jsou vyznačené společně s hello klienta IP adres, který vytvořil hello požadavků na registraci. Tato informace toofind hello hlavní příčinu selhání registrace hello potom můžete pomocí následujících kroků:

1. Najít hello zóny, která je autoritativní hello název, který hello klient se pokouší tooupdate.

2. Pomocí informací o inventáři hello toocheck řešení hello této zóny.

3. Ověřte, že hello dynamické aktualizace zóny hello je povoleno.

4. Zkontrolujte, zda hello zóny je nakonfigurován pro zabezpečené dynamické aktualizace, nebo ne.

    ![Dynamické okno registrace serveru DNS](./media/log-analytics-dns/dynamic-dns-reg-blade.png)

**Název žádosti o registraci**. horní dlaždice Hello ukazuje spojnici trendu úspěšné i neúspěšné požadavky na dynamické aktualizace DNS. dlaždice nižší Hello uvádí hello prvních 10 klientů, kteří odesílají žádosti o aktualizaci se nezdařilo DNS toohello servery DNS, seřazené podle hello počet selhání.

![Okno požadavky registrace názvu ](./media/log-analytics-dns/name-reg-req-blade.png)

**Ukázkové dotazy Analytics DDI**. Obsahuje seznam hello nejběžnější vyhledávací dotazy, které přímo načíst nezpracovaná analytická data.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Ukázkové dotazy](./media/log-analytics-dns/queries.png)

Tyto dotazy můžete použít jako východisko pro vytvoření vlastní dotazy pro vytváření přizpůsobených sestav. dotazy Hello odkazu toohello hledání DNS analýzy protokolů stránky, kde se nezobrazí výsledky:

- **Seznam serverů DNS**. Zobrazuje seznam všech serverů DNS s jejich přidružené plně kvalifikovaný název domény, název domény, název doménové struktury a server IP adresy.
- **Seznam zón DNS**. Zobrazuje seznam všech zón DNS s hello název přidružené zóny, dynamická aktualizace stavu, názvové servery a stav podepsání DNSSEC.
- **Záznamy o prostředcích nepoužívané**. Zobrazuje seznam všech záznamů prostředků nepoužívané nebo zastaralé hello. Tento seznam obsahuje název záznamu prostředku hello, typ záznamu prostředku, server DNS hello související, doba generování záznamů a název zóny. Můžete použít tento seznam tooidentify hello záznamy prostředků DNS, které jsou již používán. Na základě těchto informací, můžete pak odebrat tyto záznamy ze serverů DNS hello.
- **Servery DNS dotaz zatížení**. Obsahuje informace, abyste měli hlediska hello načíst DNS na serverech DNS. Tyto informace mohou pomoci při plánování kapacity hello hello serverů. Můžete přejít toohello **metriky** kartě toochange hello zobrazení tooa grafické vizualizace. Toto zobrazení pomáhá pochopit, jak načíst DNS hello je distribuován do vaše servery DNS. Dotaz DNS zobrazuje trendy rychlost pro každý server.

    ![Výsledky hledání protokolu dotazu servery DNS](./media/log-analytics-dns/dns-servers-query-load.png)

- **Zóny DNS dotaz zatížení**. Zobrazuje hello DNS zóny dotaz za sekundu statistiky všechny zóny hello na serverech DNS hello je spravováno nástrojem hello řešení. Klikněte na tlačítko hello **metriky** kartě toochange hello zobrazení z podrobné záznamy tooa grafické vizualizace výsledků hello.
- **Události konfigurace**. Zobrazí všechny události změny konfigurace hello DNS a související zprávy. Potom můžete filtrovat tyto události podle času serveru DNS hello událostí, ID události, nebo kategorii úloh. Hello dat můžete auditovat servery DNS toospecific změny provedené v určitých časech.
- **Analytické protokol DNS**. Obsahuje všechny hello analytické události ve všech serverech DNS hello spravované řešením hello. Potom můžete filtrovat tyto události na základě času hello událostí, ID události DNS serveru, IP adresa klienta, který vytvořil hello vyhledávací dotaz a dotaz kategorii úloh typu. Analytické události DNS serveru povolit aktivity sledování na serveru DNS hello. Analytické událost je protokolována pokaždé, když hello server odeslání nebo přijetí informací DNS.

### <a name="search-by-using-dns-analytics-log-search"></a>Hledání s použitím hledání DNS analýzy protokolů

Na stránce hello hledání protokolů můžete vytvořit dotaz. Můžete filtrovat výsledky hledání pomocí ovládacích prvků omezující vlastnosti. Můžete také vytvořit tootransform pokročilými dotazy, filtr a sestavy na výsledky. Spustit pomocí hello následující dotazy:

1. V hello **vyhledávacího dotazu pole**, typ `Type=DnsEvents` tooview všechny hello DNS události vygenerované pomocí serverů DNS hello spravované řešením hello. výsledky Hello seznamu hello data protokolu pro všechny události související toolookup dotazů, dynamických registrací a změny konfigurace.

    ![Hledání DnsEvents protokolů](./media/log-analytics-dns/log-search-dnsevents.png)  

    a. Vyberte tooview hello data protokolu pro vyhledávací dotazy **LookUpQuery** jako hello **dílčí** filtru z ovládacího prvku omezující vlastnost hello na levé straně hello. Tabulka, která se zobrazí všechny události hello vyhledávací dotaz pro hello vybrané časové období se zobrazí.

    b. Vyberte tooview hello data protokolu pro dynamické registrace **DynamicRegistration** jako hello **dílčí** filtru z ovládacího prvku omezující vlastnost hello na levé straně hello. Tabulka, která zobrazuje všechny události hello dynamickou registraci pro hello vybrané časové období se zobrazí.

    c. data protokolu hello tooview pro změny konfigurace, vyberte **ConfigurationChange** jako hello **dílčí** filtru z ovládacího prvku omezující vlastnost hello na levé straně hello. Tabulka, která obsahuje seznam všech událostí změny konfigurace hello pro hello vybrané časové období se zobrazí.

2. V hello **vyhledávacího dotazu pole**, typ `Type=DnsInventory` tooview všechna data související s inventářem DNS pro servery DNS hello spravované řešením hello hello. výsledky Hello seznamu hello data protokolu pro servery DNS, zóny DNS a záznamy o prostředcích.

    ![Hledání DnsInventory protokolů](./media/log-analytics-dns/log-search-dnsinventory.png)

## <a name="feedback"></a>Váš názor

Poskytnete zpětnou vazbu dvěma způsoby:

- **UserVoice**. Návrhy pro toowork funkce DNS Analytics můžete zveřejnit na. Navštivte hello [Operations Management Suite UserVoice stránky](https://aka.ms/dnsanalyticsuservoice).
- **Připojení k naší kohorty**. Vždy zajímá s nové zákazníky připojení k naší kohorty tooget časná toonew funkce přístupu a Pomozte nám vylepšit DNS Analytics. Pokud vás zajímá připojení naše kohorty, vyplňte [tento rychlý průzkum](https://aka.ms/dnsanalyticssurvey).

## <a name="next-steps"></a>Další kroky

[V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné záznamy protokolu DNS.
