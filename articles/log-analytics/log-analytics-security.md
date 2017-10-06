---
title: "aaaLog zabezpečení dat Analytics | Microsoft Docs"
description: "Další informace o tom, jak analýzy protokolů chrání vaše osobní údaje a zabezpečuje data."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: magoedte
ms.openlocfilehash: 130b59f22fc3dd249f32717367cc62ea25c55a21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-data-security"></a>Protokolu zabezpečení analýzy dat
Společnost Microsoft je potvrzená tooprotecting vašich osobních údajů a zabezpečení vašich dat při dodávání softwaru a služeb, které vám pomohou při správě hello infrastruktury IT v organizaci. Uvědomujeme si, že pokud jste svěřit tooothers vaše data, tohoto vztahu důvěryhodnosti vyžaduje přísných zabezpečení. Microsoft dodržuje pravidla dodržování předpisů a zabezpečení toostrict – z kódování toooperating služby.

Zabezpečení a ochrana dat je nejvyšší prioritou ve společnosti Microsoft. Kontaktujte nás s dotazy, návrhy nebo problémy k libovolnému hello následující informace, včetně naše zásady zabezpečení v [možnosti Azure podpory](http://azure.microsoft.com/support/options/).

Tento článek vysvětluje, jak data jsou shromažďována, zpracování a zabezpečeny analýzy protokolů v hello Operations Management Suite (OMS). Můžete použít agenty tooconnect toohello webové služby, pomocí nástroje System Center Operations Manager toocollect provozních dat nebo načtení dat z Azure diagnostics pro použití analýzy protokolů. Hello shromážděná data se odesílají prostřednictvím hello Internetu pomocí ověřování pomocí certifikátů a protokol SSL 3 toohello analýzy protokolů služby, který je hostován v Microsoft Azure. Data se komprimují agentem hello před odesláním.

Hello analýzy protokolů služby bezpečně spravuje vaše data založená na cloudu pomocí hello následující metody:

* Oddělení dat
* uchovávání dat
* Fyzické zabezpečení
* Správa incidentů
* Dodržování předpisů
* certifikace standardy zabezpečení

## <a name="data-segregation"></a>Oddělení dat
Zákaznická data se ukládají na jednotlivých součástí v rámci hello OMS služby logicky samostatné. Všechna data jsou označená podle organizace. Toto značení přetrvává v průběhu cyklu hello dat a je požadováno v jednotlivých vrstvách služby hello. Každý zákazník má vyhrazený Azure blob, kde hello dlouhodobá data

## <a name="data-retention"></a>Uchovávání dat
Indexované data vyhledávání protokolu se ukládají a uchovávají podle tooyour ceny plán. Další informace najdete v tématu [Log Analytics ceny](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft odstraní data zákazníků 30 dní, po zavření pracovním prostorem OMS hello. Microsoft také odstraní hello účet úložiště Azure, kde jsou uložena hello data. Při odebrání data zákazníků žádné fyzické disky zničena.

Hello následující tabulka uvádí některé z hello k dispozici řešení v OMS a příklady hello typy shromážděná data.

| **Řešení** | **Datové typy** |
| --- | --- |
| Posouzení konfigurace |Konfigurační data, metadata a data o stavu |
| Plánování kapacit |Údaje o výkonu a metadata |
| Antimalware |Konfigurační data a metadata |
| Posouzení aktualizace systému |Stav metadata a data |
| Správa protokolů |Uživatelem definované protokoly událostí, protokoly událostí systému Windows nebo protokoly služby IIS |
| Sledování změn |Inventář softwaru a metadata služby systému Windows |
| SQL a hodnocení služby Active Directory |Data rozhraní WMI, registru dat, údaje o výkonu a dynamické správy SQL Server zobrazení výsledků |

Hello následující tabulka uvádí příklady typů dat:

| **Datový typ** | **Pole** |
| --- | --- |
| Výstrahy |Výstrahy název, popis výstrahy, BaseManagedEntityId, ID problému, IsMonitorAlert, RuleId, ResolutionState, Priority, závažnosti, kategorie, vlastníka, ResolvedBy, TimeRaised, TimeAdded, změněno, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved RepeatCount TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, |
| Konfigurace |CustomerID, ID agenta, EntityID, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Událost |ID události, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, číslo, kategorie, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Poznámka:** při zápisu událostí s vlastními poli v protokolu událostí systému Windows toohello OMS shromažďuje je. |
| Metadata |BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IP adresa, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP Adresa, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Výkon |ObjectName, název_čítače, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Stav |StateChangeEventId, StateId, NewHealthState, OldHealthState, kontext, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, identifikátor MonitorId, stav HealthState, změněno, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fyzické zabezpečení
Hello analýzy protokolů v OMS služby byla obsazena zaměstnanců společnosti Microsoft a všechny aktivity se protokolují a je možné je auditovat. Služba Hello úplně spuštění v Azure a v souladu s hello Azure běžné engineering kritéria. Podrobnosti o hello fyzického zabezpečení prostředků Azure lze zobrazit na stránce 18 hello [Přehled zabezpečení Microsoft Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Fyzický přístup práva toosecure oblastí se mění v rámci jednoho pracovního dne pro každého, kdo již nemá odpovědnost za hello OMS služby, včetně přenosu a ukončení. Další informace o globální fyzické infrastruktuře hello používáme v [Microsoft Datacenters](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Správa incidentů
OMS má proces správy incidentů, které splňovat všechny služby společnosti Microsoft. toosummarize, jsme:

* Použijte sdílenou odpovědnost modelu, kde část zabezpečení odpovědnost patří tooMicrosoft a část patří toohello zákazníka
* Spravovat incidenty zabezpečení Azure
  * Spustit šetření při zjištění incidentu
  * Vyhodnocení hello dopad a závažnost incident podle reakcí na incidenty na volání člen týmu. Na základě na důkaz, hello assessment může nebo nemusí mít za následek další eskalace toohello odpovědi tým pro zabezpečení.
  * Incident Diagnostika zabezpečení odpovědi odborníky tooconduct hello technické nebo forenzní šetření, identifikovat strategie omezení, omezení rizik a alternativní řešení. Pokud tým zabezpečení hello dochází k závěru, že data zákazníků se mohla stát zveřejněné tooan nezákonné nebo neoprávněným jednotlivých, paralelní provádění hello proces oznámení Incident zákazník zahájí paralelně.  
  * Stabilizovat a obnovit z hello incidentu. Hello reakcí na incidenty týmu vytvoří problémem hello toomitigate plánu obnovení. Krizové členství ve skupině kroky jako je například umístění do karantény ovlivněné systémy může dojít, okamžitě a souběžně s diagnostiku. Způsoby zmírnění rizik delší období může plánované kterých se provádějí až po uplynutí okamžitou riziko hello.  
  * Zavřete hello incident a provedení po porážce. Hello reakcí na incidenty týmu vytvoří postmortální, který popisuje podrobnosti hello hello incidentu, s hello záměr toorevise zásady, postupy a procesy tooprevent opakování hello události.
* Upozorněte na bezpečnostní incidenty v oblasti
  * Určení oboru hello dopad odběratelů a tooprovide kdokoliv, kdo je ovlivněno jako podrobné oznámení o nejdříve
  * Vytvořte oznámení tooprovide zákazníků s dostatečně podrobné informace, které můžete provést šetření na jejich end a splňovat žádné závazky, která byla tootheir koncoví uživatelé při zpozdit přílišný proces oznámení hello.
  * Potvrďte a deklarovat hello incident, podle potřeby.
  * Upozorněte zákazníkům incidentu oznámení bez prodlení nepřiměřený a v souladu s žádné právní či smluvními závazků. Oznámení o bezpečnostní incidenty v oblasti se dodávají tooone nebo více správců zákazníka a to jakýmkoli způsobem vybere Microsoft, včetně e-mailem.
* Chování team připravenosti a školení
  * Microsoft pracovníky jsou požadované toocomplete zabezpečení a školení sledování, které pomáhá je tooidentify a sestavy, které by mohly vzbuzovat podezření problémy se zabezpečením.  
  * Operátory pracující na hello služby Microsoft Azure mají povinnosti školení přidání kolem jejich přístup toosensitive systémy hostování zákaznická data.
  * Microsoft security odpovědi pracovníky přijímat specializované školení pro své role

Pokud dojde ke ztrátě všech datech zákazníků, upozorníme každého zákazníka a to během jednoho dne. Ale zákazníka došlo ke ztrátě dat. nikdy s OMS. Kromě toho jsme udržovat kopie dat, který byl vytvořen a je geograficky distribuovanou.

Další informace o odpovědí toosecurity incidenty Microsoft najdete v tématu [odpověď zabezpečení společnosti Microsoft Azure v cloudu hello](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in hello cloud.pdf).

## <a name="compliance"></a>Dodržování předpisů
Hello OMS softwaru vývoj a služby týmu informace o zabezpečení a zásad správného řízení programu podporuje požadavky na jeho firmy a dodržuje toolaws a zákonům o, jak je popsáno v [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) a [Dodržování předpisů Centrum zabezpečení Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Jak vytváří požadavky na zabezpečení OMS, identifikuje ovládací prvky zabezpečení, spravovat a sledovat rizika jsou také popsány existuje. Ročně, jsme zkontrolujte zásady, standardy, postupy a pokyny.

Každý člen týmu vývoj OMS obdrží formální aplikace bezpečnostního školení. Interně používáme systém správy verzí pro vývoj softwaru. Každý projekt softwaru je chráněn hello systém správy verzí.

Společnost Microsoft nemá tým zabezpečení a dodržování předpisů, který dohlíží a vyhodnocuje všechny služby ve službě Microsoft. Informace o zabezpečení osob tvoří tým hello a nejsou přidruženi s hello technici oddělení, které vyvíjet OMS. Hello zabezpečení osoby mít vlastní řetězec správy a provedení nezávislé posuzování produktů a služeb tooensure zabezpečení a dodržování předpisů.

Společnosti Microsoft správní rady je oznámení roční zprávu o všechny programy pro zabezpečení informací společnosti Microsoft.

Hello OMS softwaru vývoj a služby team aktivně pracuje s týmy hello Microsoft Legal a dodržování předpisů a jiné oborových partnerů tooacquire různé certifikáty.

## <a name="certifications-and-attestations"></a>Certifikace a atestace podle
Analýzy protokolů OMS splňuje hello následující požadavky:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/en-us/blog/iso22301/)
* [Platebních karet (PCI kompatibilní) Standard dat v odvětví zabezpečení (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) podle hello Council standardy PCI zabezpečení.
* [Typ služby organizace ovládacích prvků (SOC) 1 1 a 1 typ SOC 2](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) kompatibilní
* [HIPAA a HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) pro společnosti, které mají HIPAA obchodní přidružit smlouvu
* Společná kritéria technikům Windows
* Microsoft Trustworthy Computing
* Jako služby Azure hello součásti, které používá OMS splňovat požadavky na dodržování předpisů tooAzure. Další informace v [dodržování předpisů Center důvěřovat Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).

> [!NOTE]
> V některých osvědčení služby nebo atestace podle, analýzy protokolů je uveden v části jeho starší název *Statistika provozu*.
>
>


## <a name="cloud-computing-security-data-flow"></a>Cloud computing tok dat zabezpečení
Hello následující diagram znázorňuje Architektura zabezpečení cloud jako hello toku informací z vaší společnosti a jak jsou zabezpečená, jako je přesune toohello analýzy protokolů služby, nakonec kontaktu s vámi na portálu OMS hello. Další informace o jednotlivých kroků následuje hello diagram.

![Obrázek OMS shromažďování dat a zabezpečení](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Zaregistrujte si analýzy protokolů a shromažďování dat
Vaše organizace toosend data tooLog analýzy nakonfigurujte agenty se systémem Windows, agentů spuštěných na virtuálních počítačích Azure nebo OMS agentů pro Linux. Pokud používáte agenty nástroje Operations Manager a potom pomocí Průvodce konfigurací v tooconfigure konzoly Operations hello je. Uživatelé (které mohou být můžete, ostatní jednotlivé uživatele nebo skupinu uživatelů) vytvořte jeden nebo více účtů OMS (OMS pracovní prostory) a zaregistrujte agenty pomocí jedné z hello následující účty:

* [ID organizace](../active-directory/sign-up-organization.md)
* [Účet Microsoft - Outlook, Office Live, MSN](http://www.microsoft.com/account/default.aspx)

Pracovní prostor služby OMS je, kde se data shromažďují, agregován, analyzovat a zobrazovat. Pracovní prostor slouží především jako toopartition dat znamená a každém pracovním prostoru je jedinečný. Například můžete toohave provozními daty spravované s jedním pracovním prostorem OMS a testovací data spravovat pomocí jiného pracovního prostoru. Pracovní prostory také pomůže data toohello uživatel přístup správce ovládacího prvku. Každém pracovním prostoru může mít víc uživatelských účtů s ním spojená, a všechny uživatelské účty, můžete přístup k více OMS pracovních prostorů. Můžete vytvořit na základě datacenter oblasti pracovních prostorů. Každý pracovní prostor je replikované tooother datových centrech v oblasti hello, především pro dostupnost služeb OMS.

Pro nástroj Operations Manager po dokončení Průvodce konfigurací hello každou skupinu pro správu nástroje Operations Manager naváže připojení se hello analýzy protokolů služby. Pak použijete toochoose hello Průvodce přidáním počítače, které počítače ve skupině pro správu hello jsou povoleny služby toohello toosend data. Pro ostatní typy agenta připojí každý bezpečně toohello OMS služby.

Veškerá komunikace mezi systémy připojené a hello analýzy protokolů služby zašifrována.  Hello TLS protokolu (HTTPS) se používá pro šifrování.  je následovaný Hello procesu Microsoft SDL tooensure analýzy protokolů je aktuální pomocí nejnovější vylepšení hello kryptografické protokoly.

Každý typ agent shromažďuje data pro analýzu protokolu. Typ Hello data shromážděná je závisí na typech hello řešení použít. Zobrazí souhrn shromažďování dat v [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md). Kromě toho je k dispozici pro většinu řešení podrobnější informace o kolekci. Řešení je sady předdefinovaných zobrazení, protokolu vyhledávací dotazy, pravidla shromažďování dat a zpracování logiky. Analýzy protokolů tooimport řešení mohou používat pouze správci. Po importu řešení hello je servery pro správu nástroje Operations Manager přesunutý toohello (Pokud se používá) a potom tooany agentů, které jste zvolili. Potom agenty hello shromažďovat data o hello.

## <a name="2-send-data-from-agents"></a>2. Odesílání dat z agentů
Zaregistrujte všechny typy agenta s klíčem registrace a je navázat zabezpečené připojení mezi hello agenta a hello analýzy protokolů služby pomocí ověřování pomocí certifikátů a protokol SSL s portem 443. OMS používá toogenerate tajný úložiště a udržovat klíče. Privátní klíče otáčejí každých 90 dní a jsou uložené v Azure a spravuje hello Azure operace, kteří podle striktní regulačních a dodržování předpisů.

S nástrojem Operations Manager zaregistrujte pracovního prostoru analýzy protokolů službou hello a je navázat zabezpečené připojení HTTPS mezi serverem pro správu nástroje Operations Manager hello.

Klíč k úložišti jen pro čtení u agentů Windows spuštěných na virtuálních počítačích Azure, je použít tooread diagnostických událostí do tabulek Azure.

Pokud každého agenta je služba toohello nelze toocommunicate z jakéhokoli důvodu, hello shromážděná data ukládají se místně do mezipaměti pro dočasné a server pro správu hello pokusí tooresend hello dat každých 8 minut po dobu dvou hodin. data uložená v mezipaměti Hello agenta je chráněn úložiště přihlašovacích údajů hello operačního systému. Pokud služba hello nemůže zpracovat hello data za dvě hodiny, bude hello agenty fronty hello data. Pokud hello fronty plný, OMS spustí vyřazení datové typy, počínaje údaje o výkonu. limit fronty agenta Hello je klíč registru, takže ho můžete upravit, pokud je to nezbytné. Shromážděná data je zkomprimovat a odeslat toohello služby, bez místní databáze, tak nepřidá žádné toothem zatížení. Po shromáždění hello data se odesílají, odebere se z mezipaměti hello.

Jak je popsáno výše, data z agenty se odešlou přes SSL tooMicrosoft datových centrech Azure. Volitelně můžete zvýšit zabezpečení tooprovide ExpressRoute pro hello data. ExpressRoute je toodirectly způsob, jak připojit tooAzure z existující sítě WAN, například více protokol popisku přepínání sítě VPN (MPLS) poskytované poskytovatelem síťové služby. Další informace najdete v tématu [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-hello-log-analytics-service-receives-and-processes-data"></a>3. hello analýzy protokolů služby obdrží a zpracuje data
Hello analýzy protokolů služby zajišťuje, že příchozích dat z důvěryhodného zdroje pomocí ověřování certifikátů a hello integrity dat pomocí ověřování Azure. Hello nezpracované nezpracovaná data jsou pak uloženy jako objekt blob v [Microsoft Azure Storage](../storage/common/storage-introduction.md) a nejsou šifrována. Každý objekt blob úložiště Azure má však sadu jedinečnou sadu klíčů, které je přístupné pouze toothat uživatele. Typ Hello data, která je uložená závisí na typy hello řešení, které byly importovány a použít toocollect data. Potom hello analýzy protokolů služby zpracovává hello nezpracovaných dat pro objekt blob úložiště Azure hello.

## <a name="4-use-log-analytics-tooaccess-hello-data"></a>4. Použít data hello tooaccess analýzy protokolů
TooLog Analytics na portálu OMS hello můžete přihlásit pomocí účtu organizace hello nebo účtu Microsoft, kterou jste vytvořili dříve. Všechny přenosy mezi portálu OMS hello a analýzy protokolů v OMS se odesílají přes zabezpečený kanál protokolu HTTPS. Když používáte portál OMS hello, ID relace se generuje na hello uživatele klienta (webový prohlížeč) a data jsou uložena v místní mezipaměti, dokud hello relace je ukončena. Pokud byla ukončena, hello mezipaměť je Odstraněná. Soubory cookie na straně klienta, které neobsahují identifikovatelné osobní údaje, se automaticky neodeberou. Soubory cookie relací jsou označeny HTTPOnly, která jsou zabezpečená. Po předem určené době nečinnosti hello OMS portálu relace je ukončena.

Pomocí portálu OMS hello, můžete exportovat soubor CSV tooa dat a dostanete dat pomocí rozhraní API pro vyhledávání. Exportovat ve formátu CSV je omezená too50, 000 řádků na jeden exportu a rozhraní API dat je omezená too5 000 řádků na jeden vyhledávání.

## <a name="next-steps"></a>Další kroky
* [Začínáme s analýzy protokolů](log-analytics-get-started.md) toolearn více informací o analýzy protokolů a a její spuštění v minutách.
