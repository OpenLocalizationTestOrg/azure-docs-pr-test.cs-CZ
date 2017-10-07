---
title: "monitorování porovnání produktů aaaMicrosoft | Microsoft Docs"
description: "Microsoft Operations Management Suite (OMS) je řešení pro správu IT, která pomáhá spravovat a chránit místní a cloudové infrastruktury založené na službě cloud společnosti Microsoft.  Tento článek identifikuje hello různé služby, zahrnuté v OMS a poskytuje odkazy tootheir podrobné obsah."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: a63ca0ad-61f8-425d-a48c-d87ba518c104
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren
ms.openlocfilehash: 61144a298fe73c35181070d552c41b96fc445097
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-monitoring-product-comparison"></a>Porovnání monitorování produktů společnosti Microsoft
Tento článek obsahuje porovnání mezi službou System Center Operations Manager (SCOM) a analýzy protokolů v Operations Management Suite (OMS) z hlediska jejich architekturu, jak je i sledování prostředků, a jak fungují analýzy dat hello hello logiku jejich Shromažďujte.  Toto je toogive je základní povědomí o jejich rozdíly a relativní síly.  

## <a name="basic-architecture"></a>Základní architektura
### <a name="system-center-operations-manager"></a>System Center Operations Manager
Všechny součásti SCOM jsou nainstalovány ve vašem datovém centru.  [Jsou nainstalováni agenti](http://technet.microsoft.com/library/hh551142.aspx) na počítačích systému Windows a Linux, které jsou spravovány nástrojem SCOM.  Agenti připojit příliš[servery pro správu](https://technet.microsoft.com/library/hh301922.aspx) který komunikovat s hello SCOM databáze a datový sklad.  Agenti spoléhají na servery toomanagement tooconnect ověření domény.  Ty mimo důvěryhodné domény mohou provádět ověřování pomocí certifikátu nebo připojení tooa [Server brány](https://technet.microsoft.com/library/hh212823.aspx).

SCOM vyžaduje dvě databáze SQL, jednu pro provozních dat a jiné datového skladu toosupport vytváření sestav a analýza dat.  A [serveru sestav](https://technet.microsoft.com/library/hh298611.aspx) tooreport služby SQL Reporting Services běží na data z datového skladu hello. 

SCOM můžete monitorovat cloudových prostředků pomocí sady management Pack pro produkty, jako třeba [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708), a [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html).  Tyto sady management Pack použít jeden nebo více agentů místní jako proxy pro zjišťování cloudových prostředků a spouštění pracovních postupů toomeasure jejich výkonu a dostupnosti.  Agenti proxy se také používají příliš[monitorování síťových zařízení](https://technet.microsoft.com/library/hh212935.aspx) a dalších externích zdrojů.

Hello konzole Operations Console je aplikace systému Windows, který připojí tooone hello serverů pro správu a umožňuje hello správce tooview a analyzovat shromážděná data a konfigurace prostředí SCOM hello.  Webová konzola může být hostovaný na libovolném serveru služby IIS a poskytuje analýzu dat prostřednictvím prohlížeče.

![Architektura SCOM](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Log Analytics
Většina OMS součásti jsou v hello cloudu Azure, abyste mohli nasadit a spravovat s minimálním nákladů a úsilí pro správu.  Všechna data shromažďovaná společností analýzy protokolů je uložen v úložišti OMS hello.

Analýzy protokolů můžete shromažďovat data z jednoho ze tří zdrojů:

* Fyzické a virtuální počítače se systémem Windows hello [Microsoft Monitoring Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) nebo Linux a hello [Operations Management Suite agenta pro Linux](https://technet.microsoft.com/library/mt622052.aspx).  Tyto počítače může být místní nebo virtuálních počítačů v Azure nebo v jiném cloudu.
* Účet úložiště Azure s [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) data shromážděná pomocí role pracovního procesu Azure, webovou roli nebo virtuálního počítače.
* [Připojení skupiny správy nástroje SCOM tooa](https://technet.microsoft.com/library/mt484104.aspx).  V této konfiguraci hello agenty komunikovat se servery pro správu SCOM, které poskytovat hello databáze systému SCOM toohello dat, kde ji pak doručí úložiště dat OMS toohello.
  Správci analyzovat shromážděná data a nakonfigurovat portál OMS hello, který je hostován v Azure a je přístupný z libovolného prohlížeče analýzy protokolů.  Mobilní aplikace tooaccess tato data jsou k dispozici pro standardní platformy hello.

![Architektura protokolu analýzy](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integrace SCOM a analýzy protokolů
Když SCOM slouží jako zdroj dat pro analýzy protokolů můžete využít funkce hello obou produktů v hybridním prostředí monitorování.  Můžete nakonfigurovat existující SCOM agentů prostřednictvím toobe konzole Operations Console hello spravuje OMS, kromě toocontinuing toorun management Pack z SCOM.  
Data z připojené skupiny správy SCOM doručuje tooLog analýzy pomocí jedné ze čtyř metod:

* Události a data výkonu jsou shromážděné agentem hello a doručit tooSCOM.  Servery pro správu v aplikaci SCOM doručí hello data tooLog Analytics.
* Některé události, jako jsou protokoly služby IIS a události zabezpečení pokračovat, toobe doručit přímo tooLog Analytics od agenta hello.
* Některá řešení bude poskytovat další software toohello agenta nebo vyžadují, aby software nainstalovaný toocollect další data.  Tato data obvykle odešle přímo tooLog Analytics.
* Některá řešení bude shromažďovat data přímo ze serverů pro správu SCOM, které nepochází z hello agenta.  Například hello [řešení pro správu výstrah](https://technet.microsoft.com/library/mt484092.aspx) shromažďuje výstrahy z SCOM. po jejich vytvoření.

## <a name="monitoring-logic"></a>Logika monitorování
SCOM a analýzy protokolů pracovat s podobnou data shromážděná z agentů, ale mají základní rozdíly v tom, jak definovat a implementovat jejich logiku pro shromažďování dat a jak jejich analýze shromážděných údajů hello.

### <a name="operations-manager"></a>Operations Manager
Monitorování logiku pro SCOM je implementována ve [sady management Pack](https://technet.microsoft.com/library/hh457558.aspx) který obsahují logiku pro zjišťování toomonitor součásti, měření stavu hello tyto součásti a shromažďování dat tooanalyze.  Data monitorování může být stejně jednoduché jako shromažďování čítačů událostí nebo výkonu, nebo by mohl použít komplexní logiku implementována ve skriptu.  Sady Management Pack, které zahrnují kompletní monitorování jsou k dispozici pro celou řadu [aplikací společnosti Microsoft a třetích stran](http://go.microsoft.com/fwlink/?LinkId=82105) v přidání toohardware a síťových zařízení.  Můžete [vytvářet vlastní sady management Pack](http://aka.ms/mpauthor) pro vlastní aplikace.

Sady Management Pack obsahují více pracovních postupů, které každý provádí některé odlišné monitorování funkce jako je například vzorkování čítače výkonu, kontroluje se stav hello služby nebo spouštění skriptu.  Každý pracovní postup probíhá nezávisle a definuje vlastní výsledky, například databáze, která bude zapisovat tooand zda bude vygenerována výstraha. 

Podrobnosti o pracovní postup, například hello frekvence, na které poběží, hello prahové hodnoty, které považují za chybu a závažnost hello hello výstrahy, které generují můžete přepsat.  Přidáním vlastních pracovních postupů můžete zadat také další funkce.

![Přepsání](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Sady Management Pack jsou nainstalovány v databázi nástroje Operations Manager hello a automaticky distribuovat tooagents servery pro správu.  Každý agent bude automaticky stáhnout sady management Pack a spouštění aplikací relevantní toohello pracovních postupů, které mají být nainstalovány.  Údaje shromážděné agentem hello doručuje back toohello serveru pro správu pro vložení do hello SCOM databáze a datový sklad.  Hello konzole Operations Console můžete tooview a analyzovat tato data prostřednictvím vlastních zobrazení, řídicí panely a sestavy zahrnuté v sadě management pack hello.

Hello distribuci sad management Pack je znázorněno v následujícím diagramu hello.

![Tok Management pack](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Log Analytics
#### <a name="event-and-performance-collection"></a>Události a shromažďování výkonu
Analýzy protokolů shromažďuje události a čítače výkonu z agenta systémů použití zdrojů, jako je například protokol událostí systému Windows, protokoly služby IIS a Syslog.  Můžete definovat kritéria, pro které jsou shromažďovány prostřednictvím portálu Analýza protokolů hello dat a potom vytvářet dotazy protokolu tooanalyze hello shromažďovat data.  Sadu standardních kritérií je definován při vytváření pracovního prostoru OMS a můžete definovat další data pro konkrétní aplikace. 

![Definování protokoly událostí v analýzy protokolů](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Zatímco SCOM má mnoho podrobné pracovní postupy, které obvykle definují určitá kritéria pro data a hello akce, která má být provedena v odpovědi, analýzy protokolů má obecnější kritéria pro shromažďování dat.  Protokol dotazy a řešení poskytují více cílových kritéria pro analýzu a funguje na konkrétní data v cloudu hello po se shromažďují.

#### <a name="solutions"></a>Řešení
Řešení poskytuje další logiku pro shromažďování dat a analýzu.  Řešení tooadd tooyour OMS předplatné můžete vybrat z hello Galerie řešení.

![Galerie řešení](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Řešení především spustit v rámci analýzy události a počítadla výkonu shromažďovaných v úložišti OMS hello cloudu hello.  Může definovat také toobe další data shromažďují, který lze analyzovat pomocí dotazů protokolu nebo další uživatelské rozhraní služby hello řešení hello OMS řídicím panelu. 

Například hello [řešení pro sledování změn](https://technet.microsoft.com/library/mt484099.aspx) zjistí konfigurace změny v systémech agenta a zapisuje události toohello OMS úložiště, který lze analyzovat s několika grafické zobrazení, které shrnují zjistila změny.  Podrobnostem ze zobrazení souhrnu hello do protokolu dotazy tohoto zobrazení hello podrobná data shromažďovaná společností hello řešení.

Když vyberete jaká řešení přidat předplatné tooyour, nemáte aktuálně hello možnost toocreate vlastní řešení.  Můžete vybrat hello události a toocollect čítače výkonu a vytvořit vlastní zobrazení založené na dotazech vlastního protokolu.

Hello logika monitorování pro analýzy protokolů je shrnuto v hello následující diagram.

![Tok řešení analýzy protokolů](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Monitorování stavu
### <a name="operations-manager"></a>Operations Manager
SCOM můžete model hello různé součásti aplikace a poskytují stavu v reálném čase pro všechny.  Díky tomu můžete toonot pouze zobrazení zjištěno chyb a výkonu v čase, ale také toovalidate hello skutečný stav aplikace nebo systému a všechny jeho komponenty v daném okamžiku.  Vzhledem k tomu, že rozumí hello časová období, které aplikace jsou k dispozici, modul stavu hello v aplikaci SCOM také podporuje o úrovni služeb smlouvy (SLA) který analýza a tvorba sestav o hello dostupnost aplikace v průběhu času.

Například hello zobrazení níže ukazuje stav v reálném čase hello modulů databáze SQL sledovaných nástrojem SCOM.  Hello stav jednotlivých hello databází pro jednu databázi modulů hello je zobrazený na hello dolní polovinu hello zobrazení.

![Zobrazení stavu](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Hello Průzkumníka stavů pro jednu databázi modulů hello jsou uvedené dole s hello monitorování, které jsou používané toodetermine jeho celkový stav.  Tato monitorování jsou definovány v hello SQL management pack a spouštění všechny databázové stroje SQL zjištěny nástrojem SCOM.

![Průzkumník stavů](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Součásti na více systémů může být kombinované toomeasure hello stavu distribuované aplikace.  To může být obzvláště užitečné pro obchodních aplikací, které obsahují několik součástí distribuované.  Model, který se měří hello stavu každé součásti, můžete vytvořit tento kumulativní do dostupnost pro aplikace hello.

Služba Active Directory je příkladem jeden management pack, která poskytuje model tooanalyze jeho distribuované komponenty.  diagram ukázkové Hello níže ukazuje stav hello hello celkové prostředí a hello vztah mezi doménovými strukturami, doménami a řadiče domény.  Více monitorů podobné SQL toohello příkladu výše a každou z těchto součástí zahrnuje tyto dílčí součásti.

![Zobrazení diagramu SCOM](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Log Analytics
OMS nezahrnuje běžné aplikace toomodel modul a měření jejich stav v reálném čase.  Jednotlivá řešení může vyhodnocení hello celkový stav konkrétního služeb na základě shromážděných dat a jejich může instalaci vlastní logiky v hello agenta tooperform analýzy v reálném čase.  Vzhledem k řešení běží v cloudu hello s úložištěm OMS toohello přístup, budou často poskytují podrobnější analýzy, než se obvykle provádí pomocí sady management Pack. 

Například hello [AD posouzení a vyhodnocení SQL řešení](https://technet.microsoft.com/library/mt484102.aspx) analýza shromážděná data a zadání hodnocení různých aspektů hello prostředí.  Obsahuje doporučení pro vylepšení, které můžete provedeny tooimprove hello dostupnost a výkon hello prostředí.

![Řešení pro vyhodnocení AD](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Analýza dat
SCOM a analýzy protokolů poskytují různé funkce tooanalyze shromažďovat data.  SCOM má zobrazení a řídicí panely v hello konzole Operations Console pro analýzu posledních dat v různých formátech a sestav pro prezentování data z datového skladu hello ve formě tabulky.  Analýzy protokolů poskytuje rozhraní a dokončení protokolu dotazovací jazyk pro analýzu dat v úložišti OMS hello.  Při SCOM slouží jako zdroj dat pro analýzy protokolů, hello úložiště zahrnuje údaje shromažďované nástrojem SCOM, aby bylo možné nástroje hello analýzy protokolů pro data použité tooanalyze z obou systémů.

### <a name="operations-manager"></a>Operations Manager
#### <a name="views"></a>Zobrazení
Zobrazení v konzole Operations Console hello povolit, můžete tooview různé datové typy shromažďují SCOM v různých formátech, obvykle tabulkové pro události, výstrahy a dat o stavu a řádku grafy pro data výkonu.  Zobrazení provést minimální analýza nebo konsolidace dat hello ale povolit toofilter podle tooparticular kritéria. 

![Zobrazení](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Sady Management Pack se obvykle poskytují více zobrazení podpora hello aplikace nebo systému, kterou monitoruje.  To může zahrnovat zobrazení stavu pro hello různých objektů, které hello management pack zjišťuje, výstrahy zobrazení zjištěných problémů a zobrazení výkonu pro čítače.

Zobrazení jsou obzvláště vhodný pro analýzu hello aktuální stav hello prostředí včetně otevřené výstrahy a hello stav monitorovaných systémů a objekty.  Podrobnostem dat události nebo výkonu toodetailed podpora konkrétní výstrahu v pořadí toodiagnose jeho hlavní příčinu. Podobně můžete zobrazit hello výkonu a stavu různých komponent aplikaci tooassess jeho aktuální stav.

#### <a name="dashboards"></a>Řídicí panely
Řídicí panely v hello konzole Operations Console především pracovat hello stejná data jako zobrazení ale jsou větší možnosti úprav a může obsahovat bohatší vizualizace.  Sadu standardních řídicí panely jsou k dispozici, můžete snadno přizpůsobit pro vlastní účely.  Můžete také použít widget prostředí PowerShell, které můžete zobrazit data vrácená z dotazu prostředí PowerShell.

![Řídicí panel](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Vývojáři mají hello možnost tooadd vlastní součásti toodashboards, které do svých sad management Pack.  To může být vysoce specializované tooa konkrétní aplikaci, například hello řídicího panelu hello SQL management pack vidíte níže.  Tento řídicí panel můžete také použít jako šablonu pro vlastní verze.

![Řídicí panel SQL](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Reports
Sestavy v aplikaci SCOM analyzovat data z datového skladu hello ve formě tabulky.  Může být vytisknout a naplánován pro odeslání automatizované v různých formátech, včetně souborů PDF, CSV a Word.  Sestavy pracovat s daty z datového skladu hello tak, aby byly obzvláště vhodný pro analýzu trendů dlouhou dobu.

Sady Management Pack obvykle poskytne vlastní sestavy pro konkrétní aplikace.  Můžete také vybrat z knihovny Obecné sestavy, které můžete přizpůsobit pro vlastní aplikace nebo pro provádění analýz ad hoc.

Následuje ukázka výkonu sestavy zobrazující data shromažďovaná společností hello sady Management Pack se službami Active Directory.

![Sestava](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Log Analytics
Má analýzy protokolů [dotazu jazyka](https://technet.microsoft.com/library/mt484120.aspx) používané tooperform analysis napříč data z více aplikací bez nutnosti toocreate hello vlastní zobrazení nebo sestavy.  Protože OMS je implementované v cloudu hello, výkon dotazů a analýza dat nejsou omezení hardwaru tooany subjektu a může rychle analyzovat dotazů včetně miliony záznamů. 

Dotazy v analýzy protokolů jsou také hello základ pro další funkce.  Můžete uložit dotazu, exportovat jeho výsledky tooExcel nebo ji automaticky spustit v pravidelných intervalech a generování výstrah v případě své výsledky odpovídají konkrétním kritériím.  

![Protokol dotazu toku](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Dole je příklad dotazu analýzy protokolů.  V tomto příkladu jsou všechny události se "spustit" v názvu hello vrátí a seskupené podle událost ID.  Hello uživatel jednoduše zadá dotaz hello a analýzy protokolů dynamicky vygeneruje hello uživatelské rozhraní tooperform hello analýzy.  Vyberte libovolnou položku v seznamu hello vrátí hello podrobná data události.

![Protokol dotazu](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Kromě toho tooproviding ad hoc analýzu, dotazy v analýzy protokolů můžete uložit pro budoucí použití a také přidané tooyour [řídicí panel OMS](http://technet.microsoft.com/library/mt484090.aspx) jak ukazuje následující příklad hello.

![Řídicí panel OMS](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Další kroky
* Nasazení [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
* Zaregistrujte si [analýzy protokolů](https://azure.microsoft.com/documentation/services/log-analytics).  

