---
title: "aaaAgent stavu řešení v OMS | Microsoft Docs"
description: "Tento článek je určený toohelp víte, jak toouse toto řešení toomonitor hello stav agenty reporting přímo tooOMS nebo System Center Operations Manager."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: magoedte
ms.openlocfilehash: 071b14b4ab7af6680ae458eaa331246755c5bb56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="agent-health-solution-in-oms"></a>Řešení Agent Health v OMS
řešení Hello stav agenta v OMS vám pomůže pochopit, pro všechny hello agentů, které jsou reagovat a odesílá provozních dat reporting přímo pracovním prostorem OMS toohello nebo tooOMS připojené skupiny správy System Center Operations Manager.  Můžete také sledovat určité jak mnoho agentů, kteří jsou nasazeny, kde jsou distribuovány geograficky a provádět další dotazy toomaintain povědomí o hello distribuce agentů nasazených v Azure, jiné prostředí cloudu nebo místně.    

## <a name="prerequisites"></a>Požadavky
Před nasazením tohoto řešení, potvrďte je aktuálně podporované [agenty se systémem Windows](../log-analytics/log-analytics-windows-agents.md) toohello OMS pracovní prostor generování sestav nebo generování sestav tooan [skupiny pro správu nástroje Operations Manager](../log-analytics/log-analytics-om-agents.md) integrované s vaší Pracovní prostor OMS.    

## <a name="solution-components"></a>Součásti řešení
Toto řešení se skládá z hello následující prostředky, které jsou přidány tooyour prostoru a přímo připojených agentů nebo Operations Manager připojené skupiny pro správu.

### <a name="management-packs"></a>Sady Management Pack
Pokud pracovní prostor OMS tooan připojené skupině pro správu System Center Operations Manager se hello následující sady management Pack jsou nainstalovány v nástroji Operations Manager.  Tyto sady Management Pack se po přidání tohoto řešení nainstalují také na přímo připojené počítače s Windows. Není co tooconfigure nebo spravovat pomocí těchto sad management Pack.

* Microsoft System Center Advisor HealthAssessment Direct Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentDirect)
* Microsoft System Center Advisor HealthAssessment Server Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentViaServer).  

Další informace o tom, jak jsou aktualizovány sady management Pack řešení najdete v tématu [tooLog připojení nástroje Operations Manager Analytics](../log-analytics/log-analytics-om-agents.md).

## <a name="configuration"></a>Konfigurace
Přidat hello stav agenta řešení tooyour pracovním prostorem OMS pomocí procesu hello popsané v [přidat řešení](../log-analytics/log-analytics-add-solutions.md). Není nutná žádná další konfigurace.


## <a name="data-collection"></a>Shromažďování dat
### <a name="supported-agents"></a>Podporovaní agenti
Hello následující tabulka popisuje hello připojené zdroje, které podporuje toto řešení.

| Připojený zdroj | Podporuje se | Popis |
| --- | --- | --- |
| Agenti systému Windows | Ano | Události prezenčního signálu se shromažďují z přímých agentů systému Windows.|
| Skupina pro správu nástroje System Center Operations Manager | Ano | Prezenční signál události jsou shromažďovány z agentů reporting skupiny pro správu toohello každých 60 sekund a tooLog analýzy, předá. Přímé připojení z nástroje Operations Manager agenty tooLog Analytics se nevyžaduje. Data události prezenčního signálu se předají z hello správy skupiny toohello analýzy protokolů úložiště.|

## <a name="using-hello-solution"></a>Pomocí řešení hello
Když přidáte hello pracovním prostorem OMS tooyour řešení, hello **stav agenta** dlaždice se přidá tooyour OMS řídicího panelu. Tuto dlaždici zobrazuje celkový počet agentů hello a hello počtu agentů, reagovat v hello posledních 24 hodin.<br><br> ![Dlaždice řešení Agent Health na řídicím panelu](./media/oms-solution-agenthealth/agenthealth-solution-tile-homepage.png)

Klikněte na hello **stav agenta** dlaždice tooopen hello **stav agenta** řídicího panelu.  řídicí panel Hello obsahuje sloupce hello hello následující tabulka. Každý sloupec uvádí hello top deset události podle počtu, který se shodovat, že sloupce kritéria pro hello zadaný časový rozsah. Můžete spustit hledání protokolu, které poskytuje celý seznam hello výběrem **zobrazit všechny** v pravém dolním hello jednotlivých sloupců, nebo klikněte na záhlaví sloupce hello.

| Sloupec | Popis |
|--------|-------------|
| Počet agentů v průběhu času | Trend vývoje počtu linuxových agentů a agentů systému Windows za posledních sedm dnů.|
| Počet nereagujících agentů | Seznam agentů, které nebyly odeslány prezenční signál ve hello posledních 24 hodin.|
| Distribuce podle typu operačního systému | Rozdělení agentů systému Windows a linuxových agentů ve vašem prostředí.|
| Distribuce podle verze agenta | Oddíl hello různých agenta verze nainstalované ve vašem prostředí a počet každé z nich.|
| Distribuce podle kategorie agenta | Oddílu hello různých kategorií agentů, kteří jsou odesílání prezenčního signálu událostí: přímé agentů, agenti OpsMgr nebo hello OpsMgr Management Server.|
| Distribuce podle skupiny pro správu | Oddíl hello různých skupin správy SCOM ve vašem prostředí.|
| Geografické umístění agentů | Oddíl hello různých zemí, kde máte agenty a celkový počet hello počet agentů, které byly nainstalovány v každé země.|
| Počet nainstalovaných bran | Hello počet serverů, které mají hello OMS brána nainstalovaná a seznam těchto serverů.|

![Ukázka řídicího panelu řešení Agent Health](./media/oms-solution-agenthealth/agenthealth-solution-dashboard.png)  

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
řešení Hello vytvoří jeden typ záznamu v úložišti OMS hello.  

### <a name="heartbeat-records"></a>Záznamy prezenčního signálu
Vytvoří se záznam typu **Prezenční signál**.  Tyto záznamy mají vlastnosti hello v hello následující tabulka.  

| Vlastnost | Popis |
| --- | --- |
| Typ | *Heartbeat* (Prezenční signál)|
| Kategorie | Hodnota je *Direct Agent* (Přímý agent), *SCOM Agent* (Agent nástroje SCOM) nebo *SCOM Management Server* (Server pro správu nástroje SCOM).|
| Počítač | Název počítače.|
| OSType | Operační systém Windows nebo Linux.|
| OSMajorVersion | Hlavní verze operačního systému.|
| OSMinorVersion | Podverze operačního systému.|
| Verze | Verze agenta OMS nebo agenta nástroje Operations Manager.|
| SCAgentChannel | Hodnota je *Direct* (Přímý) nebo *SCManagementServer* (Server pro správu nástroje SCOM).|
| IsGatewayInstalled | Pokud je nainstalovaná brána OMS, hodnota je *true*, jinak je hodnota *false*.|
| ComputerIP | IP adresa počítače hello.|
| RemoteIPCountry | Zeměpisné umístění, kde je počítač nasazený.|
| ManagementGroupName | Název skupiny pro správu nástroje Operations Manager.|
| SourceComputerId | Jedinečné ID počítače.|
| RemoteIPLongitude | Zeměpisná délka zeměpisného umístění počítače.|
| RemoteIPLatitude | Zeměpisná šířka zeměpisného umístění počítače.|

Každý agent generování sestav serveru pro správu nástroje Operations Manager tooan odešle dvě prezenčních signálů a bude se hodnota vlastnosti SCAgentChannel současně obsahovat **přímé** a **SCManagementServer** v závislosti na tom, co Zdroje dat protokolu analýzy a řešení, které jste povolili ve vašem předplatném OMS. Pokud si Vzpomínáte, jsou data z řešení odeslaných přímo z nástroje Operations Manager management server toohello OMS webové služby, nebo z důvodu hello objem dat, které jsou shromažďovány na hello agenta, jsou odesílány přímo z hello agenta tooOMS webové služby. Prezenční signál události, které mají hodnotu hello **SCManagementServer**, hello ComputerIP hodnota je hello IP adresa serveru pro správu hello vzhledem k tomu, že hello data je ve skutečnosti odeslaný ho.  Kde SCAgentChannel je nastaven příliš prezenčních signálů**přímé**, je hello veřejnou IP adresu hello agenta.  

## <a name="sample-log-searches"></a>Ukázky hledání v protokolech
Hello následující tabulka obsahuje ukázkové protokolu hledá záznamy shromažďují toto řešení.

| Dotaz | Popis |
| --- | --- |
| Type=Heartbeat &#124; distinct Computer |Celkový počet agentů |
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-24HOURS |Počet agentů reagovat v hello posledních 24 hodin |
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-15MINUTES |Počet agentů reagovat v hello posledních 15 minut |
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer IN {Type=Heartbeat TimeGenerated>NOW-24HOURS &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Počítače online (v hello posledních 24 hodin) |
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer NOT IN {Type=Heartbeat TimeGenerated>NOW-30MINUTES &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Celkový počet agentů v režimu Offline v posledních 30 minut (hello posledních 24 hodin) |
| Type=Heartbeat &#124; measure countdistinct(Computer) by OSType |Získání trendu vývoje počtu agentů v průběhu času podle typu operačního systému|
| Type=Heartbeat&#124;measure countdistinct(Computer) by OSType |Distribuce podle typu operačního systému |
| Type=Heartbeat&#124;measure countdistinct(Computer) by Version |Distribuce podle verze agenta |
| Type=Heartbeat&#124;measure count() by Category |Distribuce podle kategorie agenta |
| Type=Heartbeat&#124;measure countdistinct(Computer) by ManagementGroupName | Distribuce podle skupiny pro správu |
| Type=Heartbeat&#124;measure countdistinct(Computer) by RemoteIPCountry |Geografické umístění agentů |
| Type=Heartbeat IsGatewayInstalled=true&#124;Distinct Computer |Počet nainstalovaných bran OMS |


>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](../log-analytics/log-analytics-log-search-upgrade.md), pak změní následující toohello hello výše dotazy.
>
>| Dotaz | Popis |
|:---|:---|
| Heartbeat &#124; distinct Computer |Celkový počet agentů |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(24h) |Počet agentů reagovat v hello posledních 24 hodin |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(15m) |Počet agentů reagovat v hello posledních 15 minut |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer in ((Heartbeat &#124; where TimeGenerated > ago(24h) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Počítače online (v hello posledních 24 hodin) |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer !in ((Heartbeat &#124; where TimeGenerated > ago(30m) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Celkový počet agentů v režimu Offline v posledních 30 minut (hello posledních 24 hodin) |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Získání trendu vývoje počtu agentů v průběhu času podle typu operačního systému|
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Distribuce podle typu operačního systému |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by Version |Distribuce podle verze agenta |
| Heartbeat &#124; summarize AggregatedValue = count() by Category |Distribuce podle kategorie agenta |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by ManagementGroupName | Distribuce podle skupiny pro správu |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by RemoteIPCountry |Geografické umístění agentů |
| Heartbeat &#124; where iff(isnotnull(toint(IsGatewayInstalled)), IsGatewayInstalled == true, IsGatewayInstalled == "true") == true &#124; distinct Computer |Počet nainstalovaných bran OMS |

## <a name="next-steps"></a>Další kroky

* Podrobnosti o generování upozornění ze služby Log Analytics najdete v tématu [Upozornění v Log Analytics](../log-analytics/log-analytics-alerts.md).
