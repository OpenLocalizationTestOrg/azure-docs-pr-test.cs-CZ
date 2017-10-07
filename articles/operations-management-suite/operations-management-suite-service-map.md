---
title: "aaaUse hello řešení mapy služeb v Operations Management Suite | Microsoft Docs"
description: "Mapa služeb je do řešení služby Operations Management Suite, který automaticky zjišťuje součásti aplikace v systému Windows a systémy Linux a mapy hello komunikace mezi službami. Tento článek obsahuje podrobné informace pro nasazení mapy služeb ve vašem prostředí a jejich použití v různých scénářů."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: 3ceb84cc-32d7-4a7a-a916-8858ef70c0bd
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/22/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: f7c209182c9171cc520192ac13ca4d85174081b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-service-map-solution-in-operations-management-suite"></a>Použít hello mapy služeb řešení v Operations Management Suite
Mapa služeb automaticky vyhledá součásti aplikace v systémech Windows a Linux a mapy hello komunikace mezi službami. Pomocí mapy služeb, můžete zobrazit vaše servery hello takovým způsobem, který vás napadnou: jako vzájemně propojena systémy, které doručují důležité služby. Mapy služeb zobrazí připojení mezi servery, procesy a porty mezi všechny architektura připojení TCP, s výjimkou nutná žádná konfigurace hello instalace agenta.

Tento článek popisuje podrobnosti hello pomocí mapy služeb. Informace o konfiguraci mapy služeb a agentů registrace najdete v tématu [mapy služeb konfigurace řešení v Operations Management Suite](operations-management-suite-service-map-configure.md).


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Případy použití: Ujistěte se, IT procesy závislostí clustery

### <a name="discovery"></a>Zjišťování
Mapy služeb automaticky vytvoří běžné odkaz mapu závislostí mezi servery, procesy a služby třetích stran. Ji zjistí a mapuje všechny závislosti TCP, identifikace neočekávaném připojení, vzdálené systémy jiných výrobců, které závisí na a závislosti tootraditional tmavý oblasti sítě, jako je Active Directory. Mapy služeb zjistí selhání síťová připojení, spravovaných systémech zkoušíte toomake, pomáhá identifikovat potenciální chybné konfigurace serveru, výpadkem služby a problémů se sítí.

### <a name="incident-management"></a>Správa incidentů
Mapa služeb pomáhá eliminovat hello již nebudete muset odhadovat problém izolace ukazuje, jak jsou připojené systémy a které mají vliv na sebe navzájem. Kromě toho tooidentifying se nezdařilo připojení, pomáhá identifikovat nástroje pro vyrovnávání zatížení nesprávně nakonfigurované, překvapivé nebo nadměrnému zatížení důležité služby a podvodné klienty, například rozhovoru tooproduction systémy počítačích vývojářů. Pomocí integrovaného pracovních Operations Management Suite změnit sledování, uvidíte také vysvětluje, zda událost změny na back-end počítač či službu hello hlavní příčinu incidentu.

### <a name="migration-assurance"></a>Zajištění migrace
Pomocí mapy služeb můžete efektivně plánování, urychlit a ověření Azure migrace, který pomáhá zajistit že nic je ponecháno a nedojde k výpadku neočekávaném. Můžete zjistit všechny konkrétní systémy této toomigrate nutné společně, vyhodnocení konfiguraci systému a kapacity a zjistit, jestli je spuštěný systém stále obsluhuje uživatelé nebo dojít k vyřazení z provozu místo migrace. Po dokončení přesunu hello můžete zkontrolovat na tooverify zatížení a identity klienta, který testovací systémy a zákazníků se připojují. Pokud podsíť plánování a brány firewall definic problémy, se nezdařilo připojení v rámci služby maps mapy služeb bodu toohello systémy, které je třeba připojení.

### <a name="business-continuity"></a>Kontinuita podnikových procesů
Pokud používáte Azure Site Recovery a potřebovat pomoc definování hello obnovení pořadí pro vaše prostředí aplikace mapy služeb můžete automaticky zjistit, jak systémy spoléhají na sobě navzájem tooensure, že je váš plán obnovení spolehlivé. Zvolit důležitého serveru nebo skupiny a zobrazením jeho klienty, můžete identifikovat které front-endu systémy toorecover po hello serveru obnovena a k dispozici. Naopak prohlížením důležité servery back-end závislosti, můžete určit, které systémy toorecover předtím, než se obnoví vaše systémy fokus.

### <a name="patch-management"></a>Opravy správy
Mapy služeb vylepšuje používání hello vyhodnocení aktualizací Operations Management Suite systému ukazuje, který ostatními týmy a servery závisí na službě, tak můžete upozornit předem před vypnout vaše systémy pro opravy. Mapy služeb taky zlepšuje správu oprava v Operations Management Suite ukazuje, zda jsou k dispozici a správně připojené po vaší služby jsou opravit a restartovat.


## <a name="mapping-overview"></a>Přehled mapování
Mapy služeb agenty shromažďovat informace o všech procesů připojení protokolu TCP na hello serveru, kde se instalují a podrobnosti o hello příchozí a odchozí připojení pro jednotlivé procesy. V seznamu hello v levém podokně hello můžete vybrat počítače nebo skupiny, které mají mapy služeb agenty toovisualize závislé v zadaném časovém období. Počítač závislostí mapuje zaměřit na konkrétní počítač a zobrazují všechny hello počítače, které jsou přímé TCP klientů nebo serverů tohoto počítače.  Mapování skupin počítačů zobrazit sady serverů a jejich závislosti.

![Přehled mapy služeb](media/oms-service-map/service-map-overview.png)

Počítače můžete rozšířit hello mapy tooshow hello spuštěným procesům pomocí aktivní síťové připojení během hello vybrané časové rozmezí. Podrobnosti o procesu rozšířené tooshow při vzdáleném počítači s agentem mapy služeb se zobrazují pouze procesy, které komunikují s počítači fokus hello. počet Hello bez agentů front-end počítačů, které připojit do počítače fokus hello je uveden na levé straně hello hello procesů, ke kterým se připojují k. Pokud hello fokus počítač je počítač back-end tooa připojení, který nemá žádný agent, hello back-end serverů je zahrnutý do skupiny Port serveru, spolu s další připojení toohello stejné číslo portu.

Ve výchozím nastavení mapy služeb mapuje hello zobrazit informace o závislostech za posledních 30 minut. Pomocí hello ovládací prvky v levé horní části hello se můžete dotazovat mapy pro historické časové rozsahy až tooshow hodinu tooone jak závislosti hledá v hello minulosti (například během incident nebo před došlo ke změně). Mapa služeb data jsou uložena po dobu 30 dnů v placené pracovních prostorů a 7 dní v bezplatné pracovní prostory.

## <a name="status-badges-and-border-coloring"></a>Stav odznaky a barvy ohraničení
Dolní každého serveru v mapě hello může být v hello seznam stav odznaky zdůraznění stavové informace o serveru hello. odznaky Hello znamenat, že některé důležité informace pro server hello z jednoho z integrace řešení hello Operations Management Suite. Kliknutím oznámení "BADGE" přejdete přímo toohello podrobnosti o stavu hello v pravém podokně hello. Hello aktuálně k dispozici stav odznaky zahrnují výstrahy, technickou podporu, změny, zabezpečení a aktualizace.

V závislosti na závažnosti hello odznaky stav hello můžete počítač uzel ohraničení být barevnou red (kritická), žlutý (varování) nebo modrá (informativní). Barva Hello představuje hello nejzávažnějšího stav každého odznaky stav hello. Šedé ohraničení označuje uzel, který nemá žádné indikátory stavu.

![Stav oznámení](media/oms-service-map/status-badges.png)

## <a name="machine-groups"></a>Skupiny počítačů
Skupiny počítačů umožňují vám mapy toosee zaměřená na sadu serverů, nikoli pouze jeden, abyste viděli všichni členové clusteru vícevrstvé aplikace nebo serveru v jedna mapa hello.

Uživatelé vybrat, které servery patří ve skupině společně a zvolte název pro skupinu hello.  Potom můžete vybrat skupinu hello tooview se všemi jeho připojení a procesů nebo zobrazit pouze s hello procesy a připojení, která se přímo týkají toohello ostatní členy skupiny hello.

![Skupinu počítačů](media/oms-service-map/machine-group.png)

### <a name="creating-a-machine-group"></a>Vytvoření skupiny počítačů
toocreate skupinu, vyberte hello počítače nebo počítače chcete, aby na počítačích hello seznamu a klikněte na tlačítko **přidat toogroup**.

![Vytvoření skupiny](media/oms-service-map/machine-groups-create.png)

Zde můžete **vytvořit nový** a pojmenujte hello skupiny.

![Název skupiny](media/oms-service-map/machine-groups-name.png)

>[!NOTE]
>Skupiny počítače jsou aktuálně omezená too10 servery, ale plánujeme tooincrease tento limit brzy k dispozici.

### <a name="viewing-a-group"></a>Zobrazení skupiny
Po vytvoření některé skupiny, můžete je zobrazit výběrem karty skupiny hello.

![Karta skupiny](media/oms-service-map/machine-groups-tab.png)

Pak vyberte hello skupiny název tooview hello mapy pro tuto skupinu pro počítač.
![Skupinu počítače](media/oms-service-map/machine-group.png) hello počítače, které patří toohello skupiny jsou uvedeny v bílé v mapě hello.

Se zvětšující hello skupiny se zobrazí seznam hello počítače, které tvoří hello skupinu počítačů.

![Počítač skupiny počítačů](media/oms-service-map/machine-groups-machines.png)

### <a name="filter-by-processes"></a>Filtrovat podle procesy
Můžete přepnout zobrazení mapy hello mezi zobrazující všechny procesy a připojení v hello skupiny a pouze ta, která se přímo týkají toohello skupinu počítače hello.  Hello výchozí zobrazení je tooshow všechny procesy.  Hello zobrazení můžete změnit kliknutím na ikonu filtru hello výše hello mapy.

![Filtr skupiny](media/oms-service-map/machine-groups-filter.png)

Když **všechny procesy** je vybrána, mapy hello bude obsahovat všechny procesy a připojení na všech počítačích hello v hello skupiny.

![Zpracuje všechny skupinu počítačů](media/oms-service-map/machine-groups-all.png)

Pokud změníte hello zobrazení pouze tooshow **připojené skupiny procesy**, budou co nejlépe určen hello mapy DOLŮ tooonly tyto procesy a připojení, které jsou přímo připojené tooother počítače do skupiny hello, vytváření zjednodušené zobrazení.

![Skupina počítačů filtrovaná procesy](media/oms-service-map/machine-groups-filtered.png)
 
### <a name="adding-machines-tooa-group"></a>Přidání skupiny tooa počítače
tooadd počítačů tooan existující skupiny, zkontrolujte hello oknech a potom klikněte na další počítače toohello **přidat toogroup**.  Zvolte hello skupinu, kterou chcete tooadd hello počítače.
 
### <a name="removing-machines-from-a-group"></a>Odebrání počítače ze skupiny
V seznamu skupin hello rozbalte položku hello skupiny název toolist hello počítačů v hello skupinu počítačů.  Potom klikněte na hello třemi tečkami nabídky Další toohello počítače chcete tooremove a zvolte **odebrat**.

![Odeberte počítače ze skupiny](media/oms-service-map/machine-groups-remove.png)

### <a name="removing-or-renaming-a-group"></a>Odebrání nebo přejmenování skupiny
Klikněte na hello třemi tečkami nabídky Další toohello název skupiny v hello seznam skupiny.

![Počítač skupiny nabídky](media/oms-service-map/machine-groups-menu.png)


## <a name="role-icons"></a>Role ikony
Některé procesy sloužit konkrétní role na počítačích: webové servery, aplikační servery, databáze a tak dále. Mapy služeb označí proces a najděte počítač polí s toohelp ikony role na první pohled hello role procesu nebo server plní.

| Ikona role | Popis |
|:--|:--|
| ![Webový server](media/oms-service-map/role-web-server.png) | Webový server |
| ![Aplikace serveru](media/oms-service-map/role-application-server.png) | Aplikační server |
| ![Databázový server](media/oms-service-map/role-database.png) | Databázový server |
| ![LDAP server](media/oms-service-map/role-ldap.png) | LDAP server |
| ![Server s protokolem SMB](media/oms-service-map/role-smb.png) | Server s protokolem SMB |

![Role ikony](media/oms-service-map/role-icons.png)


## <a name="failed-connections"></a>Neúspěšné připojení
Neúspěšné připojení jsou uvedeny v mapy služeb mapy pro počítače a procesy s na přerušovanou red čáru indikující, že klientský systém selhává tooreach procesu nebo portu. Pokud daný systém hello jedno při pokusu hello se nezdařilo připojení se nezdařilo připojení hlásit z jakéhokoli systému s nasazené agentem mapy služeb. Mapa služeb měří tento proces sledování sockets TCP, které nesplní tooestablish připojení. Tato chyba může být výsledkem bránu firewall, chybné konfigurace v hello klienta nebo serveru, nebo vzdálené služby není k dispozici.

![Neúspěšné připojení](media/oms-service-map/failed-connections.png)

Seznámení se nezdařilo připojení může pomoci při řešení, ověření migrace, analýzu zabezpečení a porozumění celkového architektury. Neúspěšné připojení jsou někdy neškodné, ale jejich často bodu přímo tooa problém, například prostředí převzetí služeb při selhání najednou stane nedostupný, nebo dvě úrovně aplikace se nedá tootalk po migraci cloudu.

## <a name="client-groups"></a>Skupin klientů
Skupin klientů jsou polí na mapě hello, která představují klientské počítače, které nemají závislostí agenty. Jedné skupiny klientů představuje hello klientům pro jednotlivé procesu nebo počítače.

![Skupin klientů](media/oms-service-map/client-groups.png)

toosee hello IP adresy hello servery ve skupině klienta, vyberte hello skupiny. obsah Hello hello skupiny jsou uvedeny v hello **vlastnosti skupiny klienta** podokně.

![Vlastnosti skupiny klientů](media/oms-service-map/client-group-properties.png)

## <a name="server-port-groups"></a>Port serveru skupiny
Port serveru skupiny jsou polí, která představují porty serveru na serverech, které nemají závislostí agenty. pole Hello obsahuje port serveru hello a počet hello počet serverů s portem toothat připojení. Rozbalte hello pole toosee hello jednotlivé servery a připojení. Pokud je pole hello pouze jeden server, je uvedený hello název nebo IP adresu.

![Port serveru skupiny](media/oms-service-map/server-port-groups.png)

## <a name="context-menu"></a>Místní nabídka
Kliknutím na tlačítko hello tečkami (...) hello horní pravé žádného serveru zobrazí hello kontextové nabídky pro tento server.

![Neúspěšné připojení](media/oms-service-map/context-menu.png)

### <a name="load-server-map"></a>Zatížení serveru mapy
Kliknutím na tlačítko **zatížení serveru mapy** přejdete tooa nové mapování hello vybraný server jako nový počítač fokus hello.

### <a name="show-self-links"></a>Zobrazit odkazů na sebe sama
Kliknutím na tlačítko **zobrazit Self-Links** uzel serveru hello překreslí ho, včetně všech odkazů na sebe sama, které jsou připojení TCP, které začínají a končí na procesy v rámci serveru hello. Pokud odkazů na sebe sama se zobrazí, hello změny příkaz nabídky příliš**skrýt Self-Links**, takže je můžete vypnout.

## <a name="computer-summary"></a>Souhrn počítače
Hello **počítač Souhrn** podokně obsahuje přehled operačního systému serveru, počtu závislostí a data z jiných řešení služby Operations Management Suite. Taková data zahrnuje metrik výkonu, lístků podpory služby, sledování změn, zabezpečení a aktualizace.

![Podokno Souhrn počítače](media/oms-service-map/machine-summary.png)

## <a name="computer-and-process-properties"></a>Vlastnosti počítače a procesu
Když přejdete mapu mapy služeb, můžete vybrat počítače a procesy toogain další kontext o svých vlastnostech. Počítače poskytují informace o DNS název, IPv4 adresy, procesoru a paměti kapacitu, typ virtuálního počítače, operační systém a verze poslední restartovat čas a ID hello jejich agentů Operations Management Suite a mapy služeb.

![Podokno vlastností počítače](media/oms-service-map/machine-properties.png)

Podrobnosti o procesu můžete shromáždit z operačního systému metadata o spuštěných procesů, včetně název procesu, popis procesu, uživatelské jméno a doménu (v systému Windows), název společnosti, název produktu, verze produktu, pracovní adresář, příkazový řádek a proces Počáteční čas.

![Podokno vlastností procesu](media/oms-service-map/process-properties.png)

Hello **souhrn procesu** podokno poskytuje další informace o procesu hello připojení, včetně jeho vázané porty, příchozí a odchozí připojení a připojení se nezdařilo.

![Podokno Souhrn procesu](media/oms-service-map/process-summary.png)

## <a name="operations-management-suite-alerts-integration"></a>Integrace nástroje Operations Management Suite výstrahy
Mapa služeb se integruje s Operations Management Suite výstrahy tooshow aktivováno výstrahy pro vybraný server hello v hello vybrané časové rozmezí. Hello server zobrazuje ikonu, pokud existují výstrahy a hello **počítač výstrahy** podokně zobrazí výstrahy hello.

![Počítač podokně výstrahy](media/oms-service-map/machine-alerts.png)

tooenable mapy služeb toodisplay příslušné výstrahy, vytvořte pravidlo, které aktivuje se v určitém počítači. toocreate správné výstrahy:
- Zahrnout klauzule toogroup počítač (například **počítače interval 1 minuta**).
- Zvolte tooalert podle metriky měření.

![Konfigurace výstrah](media/oms-service-map/alert-configuration.png)


## <a name="operations-management-suite-log-events-integration"></a>Integrace protokolu událostí nástroje Operations Management Suite
Mapy služeb se integruje s hledání protokolů tooshow počet všechny dostupné protokolu události pro vybraný server hello během hello vybrané časové rozmezí. Můžete klikněte na libovolný řádek v seznamu hello událostí počty toojump tooLog vyhledávání a zobrazte hello jednotlivých protokolu události.

![V podokně protokolu události počítače](media/oms-service-map/log-events.png)

## <a name="operations-management-suite-service-desk-integration"></a>Integrace nástroje Operations Management Suite služby podpory
Integrace mapy služeb s hello IT Service Management Connector je automatické, pokud obě řešení jsou povolené a nakonfigurované v pracovním prostoru služby Operations Management Suite. integrace Hello ve mapy služeb se s označením "Technickou podporu." Další informace najdete v tématu [centrálně spravovat ITSM pracovních položek pomocí konektoru služby správy IT](https://docs.microsoft.com/azure/log-analytics/log-analytics-itsmc-overview).

Hello **počítač technickou podporu** podokně zobrazí všechny události správy služeb IT pro vybraný server hello v hello vybrané časové rozmezí. Hello server zobrazuje ikonu, pokud je aktuální počet položek a hello počítač technickou podporu podokně zobrazí je.

![Služby podpory podokně počítače](media/oms-service-map/service-desk.png)

Klikněte na položku hello tooopen v řešení připojených ITSM **zobrazení pracovní položka**.

Klikněte na tlačítko Podrobnosti hello tooview hello položky v protokolu vyhledávání **zobrazit v protokolu vyhledávání**.


## <a name="operations-management-suite-change-tracking-integration"></a>Integrace nástroje Operations Management Suite změna sledování
Integrace mapy služeb s sledování změn je automaticky, pokud obě řešení jsou povolené a nakonfigurované v pracovním prostoru služby Operations Management Suite.

Hello **sledování změn počítače** podokně uvádí všechny změny, s hello nejnovější nejprve společně s toodrill odkaz dolů tooLog vyhledejte další podrobnosti.

![Sledování změn podokně počítače](media/oms-service-map/change-tracking.png)

Hello následující obrázek je podrobný přehled o ConfigurationChange událost, která může dojít po výběru **zobrazit v analýzy protokolů**.

![Změnakonfigurace událostí](media/oms-service-map/configuration-change-event.png)


## <a name="operations-management-suite-performance-integration"></a>Integrace nástroje Operations Management Suite výkonu
Hello **výkon počítače** podokně se zobrazí standardní výkonu metriky pro vybraný server hello. Hello metrikám patří využití procesoru, využití paměti, sítě Bajty odeslané a přijaté a seznam důležitých procesů hello pomocí sítě přijatých a odeslaných bajtů. data výkonu sítě tooget hello, musí taky povolíte hello řešení přenosu dat 2.0 v Operations Management Suite.

![Počítač podokně výkonu](media/oms-service-map/machine-performance.png)


## <a name="operations-management-suite-security-integration"></a>Integrace nástroje Operations Management Suite zabezpečení
Integrace mapy služeb se zabezpečení a Audit je automatické, pokud obě řešení jsou povolené a nakonfigurované v pracovním prostoru služby Operations Management Suite.

Hello **zabezpečení počítače** podokně se zobrazují data z hello řešení zabezpečení Operations Management Suite a auditu pro vybraný server hello. Hello podokně obsahuje souhrnný seznam všechny zbývající bezpečnostní problémy pro hello server během hello vybrané časové rozmezí. Kliknutím na některé z projde problémy zabezpečení hello dolů do hledání protokolů podrobnosti o nich.

![Podokno zabezpečení počítače](media/oms-service-map/machine-security.png)


## <a name="operations-management-suite-updates-integration"></a>Integrace nástroje Operations Management Suite aktualizace
Integrace mapy služeb se Správa aktualizací je automatické, pokud obě řešení jsou povolené a nakonfigurované v pracovním prostoru služby Operations Management Suite.

Hello **Machine aktualizace** podokně se zobrazí data z hello řešení správy Operations Management Suite aktualizací pro vybraný server hello. Hello podokně obsahuje souhrnný seznam všechny chybějící aktualizace pro hello server během hello vybrané časové rozmezí.

![Sledování změn podokně počítače](media/oms-service-map/machine-updates.png)

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Data inventáře počítače a procesu mapy služeb je k dispozici pro [vyhledávání](../log-analytics/log-analytics-log-searches.md) v analýzy protokolů. Můžete použít tento tooscenarios dat, které zahrnují plánování migrace, analýzy kapacity, zjišťování a řešení potíží s výkonem na vyžádání.

Generuje se jeden záznam za hodinu pro každý jedinečný počítač a proces, kromě toohello záznamy, které jsou generovány, pokud začíná nebo procesu nebo počítače je na zahrnuté tooService mapy. Tyto záznamy mají vlastnosti hello v hello následující tabulky. Hello pole a hodnoty v událostech ServiceMapComputer_CL hello mapovat toofields hello prostředek počítače v hello ServiceMap rozhraní API Správce prostředků Azure. Hello pole a hodnoty v událostech ServiceMapProcess_CL hello mapovat toohello pole hello proces prostředku v hello ServiceMap rozhraní API Správce prostředků Azure. pole ResourceName_s Hello odpovídá hello pole název odpovídající prostředku Resource Manager hello. 

>[!NOTE]
>Jelikož funkce mapy služeb, tato pole jsou toochange subjektu.

Nejsou k dispozici interně generované vlastnosti, které můžete použít tooidentify jedinečný procesy a počítače:

- Počítač: Použití ResourceId nebo ResourceName_s toouniquely identifikovat počítač v rámci pracovní prostor služby Operations Management Suite.
- Proces: Použití ResourceId toouniquely identifikovat procesu v rámci pracovní prostor služby Operations Management Suite. ResourceName_s je jedinečné v rámci kontextu hello hello počítače, na které hello proces spuštěný (MachineResourceName_s) 

Protože pro proces a počítač v zadaném časovém rozmezí může existovat více záznamů, dotazy mohou vracet víc než jeden záznam pro hello stejného počítače nebo proces. tooinclude hello pouze poslední záznam, přidejte "| Při odstraňování duplicitních dat ResourceId"toohello dotaz.

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL záznamů
Záznamů s typem *ServiceMapComputer_CL* mít data inventáře pro servery s agenty mapy služeb. Tyto záznamy mají vlastnosti hello v hello následující tabulka:

| Vlastnost | Popis |
|:--|:--|
| Typ | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ID prostředku | Hello jedinečný identifikátor pro počítač v rámci pracovního prostoru hello |
| ResourceName_s | Hello jedinečný identifikátor pro počítač v rámci pracovního prostoru hello |
| ComputerName_s | počítač Hello plně kvalifikovaný název domény |
| Ipv4Addresses_s | Seznam adres IPv4 hello server |
| Ipv6Addresses_s | Seznam adres IPv6 hello server |
| DnsNames_s | Pole názvy DNS |
| OperatingSystemFamily_s | Windows nebo Linux |
| OperatingSystemFullName_s | celý název operačního systému hello Hello  |
| Bitness_s | počet bitů Hello hello počítače (32bitová nebo 64bitová verze)  |
| PhysicalMemory_d | Hello fyzické paměti v MB |
| Cpus_d | Hello počet procesorů |
| CpuSpeed_d | Hello rychlost procesoru v MHz|
| VirtualizationState_s | *Neznámý*, *fyzické*, *virtuální*, *hypervisoru* |
| VirtualMachineType_s | *HyperV*, *vmware*a tak dále |
| VirtualMachineNativeMachineId_g | Hello ID virtuálního počítače přiřazené službou jeho hypervisoru |
| VirtualMachineName_s | Název Hello hello virtuálních počítačů |
| BootTime_t | spuštění Hello |



### <a name="servicemapprocesscl-type-records"></a>Typ ServiceMapProcess_CL záznamů
Záznamů s typem *ServiceMapProcess_CL* mít data inventáře pro připojení TCP procesy na serverech s agenty mapy služeb. Tyto záznamy mají vlastnosti hello v hello následující tabulka:

| Vlastnost | Popis |
|:--|:--|
| Typ | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ID prostředku | Jedinečný identifikátor procesu v rámci pracovního prostoru hello Hello |
| ResourceName_s | Hello jedinečný identifikátor pro zpracování v rámci hello počítače, na kterém je spuštěn|
| MachineResourceName_s | název prostředku Hello hello počítače |
| ExecutableName_s | Hello název spustitelného souboru procesu hello |
| StartTime_t | čas zahájení fondu procesů Hello |
| FirstPid_d | první PID Hello fond procesů hello |
| Description_s | Popis procesu Hello |
| CompanyName_s | Hello název společnosti hello |
| InternalName_s | interní název Hello |
| ProductName_s | Název Hello produktu hello |
| ProductVersion_s | verze produktu Hello |
| FileVersion_s | verze souboru Hello |
| CommandLine_s | Hello příkazového řádku |
| ExecutablePath _Malá | Hello cesta toohello spustitelný soubor |
| WorkingDirectory_s | Hello pracovní adresář |
| Uživatelské jméno | Hello účet, pod které hello proces prováděn |
| UserDomain | v rámci které hello proces spouští domény Hello |


## <a name="sample-log-searches"></a>Ukázky hledání v protokolech

### <a name="list-all-known-machines"></a>Seznam známých všechny počítače
Typ = ServiceMapComputer_CL | odstranění duplicitních dat ResourceId

### <a name="list-hello-physical-memory-capacity-of-all-managed-computers"></a>Kapacita fyzické paměti hello seznam všech spravovaných počítačů.
Typ = ServiceMapComputer_CL | Vyberte PhysicalMemory_d ComputerName_s | ResourceId odstraňování duplicitních dat

### <a name="list-computer-name-dns-ip-and-os"></a>Název počítače seznamu, DNS, IP a operačního systému.
Typ = ServiceMapComputer_CL | Vyberte ComputerName_s, OperatingSystemFullName_s, DnsNames_s, IPv4Addresses_s | odstranění duplicitních dat ResourceId

### <a name="find-all-processes-with-sql-in-hello-command-line"></a>Vyhledá všechny procesy s "sql" v hello příkazového řádku
Typ = ServiceMapProcess_CL CommandLine_s = \*sql\* | ResourceId odstraňování duplicitních dat

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Najít počítač (poslední záznam) podle názvu prostředku
Typ = ServiceMapComputer_CL "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | odstranění duplicitních dat ResourceId

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Najít počítač (poslední záznam) podle IP adresy
Typ = ServiceMapComputer_CL "10.229.243.232" | odstranění duplicitních dat ResourceId

### <a name="list-all-known-processes-on-a-specified-machine"></a>Seznam všech známých procesů v zadaném počítači
Typ = ServiceMapProcess_CL MachineResourceName_s="m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | odstranění duplicitních dat ResourceId

### <a name="list-all-computers-running-sql"></a>Zobrazí seznam všech počítačů se systémem SQL
Typ = ServiceMapComputer_CL ResourceName_s IN {typ = ServiceMapProcess_CL \*sql\* | Odlišné MachineResourceName_s} | odstranění duplicitních dat ResourceId | Odlišné ComputerName_s

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Seznam všech verzí produktu jedinečný curl Moje datového centra.
Typ = ServiceMapProcess_CL ExecutableName_s = curl | Odlišné ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Vytvořit skupinu počítačů všech počítačů se systémem CentOS
Typ = ServiceMapComputer_CL OperatingSystemFullName_s = \*CentOS\* | Odlišné ComputerName_s


## <a name="rest-api"></a>REST API
Všechny hello serveru, proces a závislostí data v mapy služeb nejsou k dispozici prostřednictvím hello [rozhraní API REST služby mapy](https://docs.microsoft.com/rest/api/servicemap/).


## <a name="diagnostic-and-usage-data"></a>data o využití a Diagnostika
Microsoft automaticky shromažďuje data o využití a výkonu prostřednictvím vašeho používání hello služby mapy služeb. Společnost Microsoft používá tato data tooprovide a zlepšit hello kvality, zabezpečení a integrity hello služby mapy služeb. tooprovide přesná a efektivní možnosti pro odstraňování potíží, hello data zahrnují informace o konfiguraci hello vašeho softwaru, jako je operační systém a verze, IP adresu, název DNS a název pracovní stanice. Společnost Microsoft neshromažďuje jména, adresy ani jiné kontaktní informace.

Další informace o shromažďování a používání dat najdete v tématu hello [prohlášení o ochraně osobních údajů služeb Microsoft Online](https://go.microsoft.com/fwlink/?LinkId=512132).


## <a name="next-steps"></a>Další kroky
Další informace o [protokolu hledání](../log-analytics/log-analytics-log-searches.md) v datech tooretrieve analýzy protokolů, které shromažďuje mapy služeb.


## <a name="troubleshooting"></a>Řešení potíží
V tématu hello [hello Konfigurace mapy služeb dokumentu v části řešení potíží s](operations-management-suite-service-map-configure.md#troubleshooting).


## <a name="feedback"></a>Váš názor
Máte k dispozici žádné zpětná vazba nám o mapy služeb nebo této dokumentace?  Navštivte naše [User Voice stránky](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), kde můžete navrhnout funkce nebo hlasovat až existující návrhy.
