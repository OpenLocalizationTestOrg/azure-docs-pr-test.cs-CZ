---
title: "aaaMonitor stav replikace služby Active Directory s Azure Log Analytics | Microsoft Docs"
description: "Hello sady řešení stav replikace Active Directory pravidelně monitoruje prostředí služby Active Directory k jeho selhání replikace a hlásí výsledky hello na řídicím panelu OMS."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 1b988972-8e01-4f83-a7f4-87f62778f91d
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 235e4f7a066ea50b79f681398182b22c91fb6d31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-active-directory-replication-status-with-log-analytics"></a>Monitorování stavu replikace služby Active Directory s analýzy protokolů

![Stav replikace AD – symbol](./media/log-analytics-ad-replication-status/ad-replication-status-symbol.png)

Služba Active Directory je klíčovou součástí organizace IT prostředí. tooensure vysokou dostupnost a vysoký výkon, každý řadič domény má svou vlastní kopii databáze služby Active Directory hello. Řadiče domény replikovat mezi sebou v pořadí toopropagate změny pomocí hello enterprise. Selhání v tomto procesu replikace může způsobit různé problémy napříč hello enterprise.

Hello stav replikace AD řešení pack pravidelně monitoruje prostředí služby Active Directory k jeho selhání replikace a hlásí výsledky hello na řídicím panelu OMS.

## <a name="installing-and-configuring-hello-solution"></a>Instalace a konfigurace řešení hello
Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.

* Je nutné nainstalovat agenty na řadičích domény, které jsou členy toobe domény hello vyhodnotit. Nebo, je nutné nainstalovat agenty na členských serverech a nakonfigurovat hello agenty toosend AD replikace dat tooOMS. jak tooconnect tooOMS počítače systému Windows, najdete v části toounderstand [tooLog počítače připojit Windows Analytics](log-analytics-windows-agents.md). Pokud řadič domény je již součástí stávajícího prostředí System Center Operations Manager, které chcete tooconnect tooOMS, přečtěte si téma [tooLog připojení nástroje Operations Manager Analytics](log-analytics-om-agents.md).
* Přidat hello stav replikace Active Directory řešení tooyour pracovním prostorem OMS pomocí procesu hello popsané v [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.

## <a name="ad-replication-status-data-collection-details"></a>Podrobnosti služby AD stav replikace dat kolekce
Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se údaje pro stav replikace AD.

| Platforma | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |každých pět dní |

## <a name="optionally-enable-a-non-domain-controller-toosend-ad-data-toooms"></a>Volitelně můžete povolte řadiči domény toosend AD dat tooOMS
Pokud nechcete, aby tooconnect některé z řadičů domény přímo tooOMS, budete moct použít další OMS připojený počítač ve vaší doméně toocollect data pro hello stav replikace AD řešení pack a mějte ho odesílat hello data.

### <a name="tooenable-a-non-domain-controller-toosend-ad-data-toooms"></a>tooenable tooOMS data toosend AD řadiči domény
1. Ověřte, že tento počítač hello je členem domény hello chcete toomonitor pomocí řešení hello AD stav replikace.
2. [Připojit tooOMS počítače Windows hello](log-analytics-windows-agents.md) nebo [připojit pomocí vaše stávající prostředí tooOMS nástroje Operations Manager](log-analytics-om-agents.md), pokud již není připojen.
3. Na tomto počítači nastavte hello následující klíč registru:

   * Klíč: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management skupiny\<ManagementGroupName > \Solutions\ADReplication**
   * Hodnota: **IsTarget**
   * Údaj hodnoty: **true**

   > [!NOTE]
   > Tyto změny se projeví až vaše hello restartování služby Microsoft Monitoring Agent (HealthService.exe).
   >
   >

## <a name="understanding-replication-errors"></a>Principy chyby replikace
Až budete mít data stavu replikace AD posílaná tooOMS, uvidíte dlaždici podobné toohello následující bitové kopie na hello OMS řídicí panel, která určuje, kolik chyby replikace, které máte aktuálně.  
![Stav replikace AD dlaždici](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritické chyby replikace** chyby, které jsou nebo vyšší 75 % hello [životnosti objektů označených jako neplatné](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) pro doménovou strukturu Active Directory.

Když kliknete na dlaždici hello, zobrazí se další informace o chybách hello.
![Řídicí panel stavu replikace AD](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>Stav cílového serveru a stav zdrojového serveru
Těchto sloupcích se zobrazují stav hello cílové servery a zdrojové servery, ve kterých dochází k chybám při replikaci. číslo Hello po každé název řadiče domény označuje hello počet chyb replikace u tohoto řadiče domény.

Hello chyby pro cílové servery a zdrojové servery se zobrazí, protože některé problémy jsou jednodušší tootroubleshoot z perspektivy serveru hello zdroje a ostatní uživatele z perspektivy serveru cílové hello.

V tomto příkladu vidíte, že mnoho cílové servery mít zhruba hello stejný počet chyb, avšak neexistuje jeden zdrojový server (ADDC35), který má mnoho chyb více než hello všechny ostatní. Je pravděpodobné, že je na ADDC35 způsobuje ho toofail toosend data tooits replikaci nějaký problém. Řešení problémů hello na ADDC35 může vyřešit řadu hello chyby, které se zobrazují v hello cílový server oblasti.

### <a name="replication-error-types"></a>Typy chyb replikace
Tato oblast získáte informace o typech hello chyb zjištěných ve vašem podniku. Jednotlivé chyby má jedinečný číselný kód a zprávu, která vám pomohou určit hlavní příčinu hello hello chyby.

Hello prstenec v horní části hello získáte představu, které se zobrazí chyby další a méně často ve vašem prostředí.

Zobrazuje při několika řadičů domény prostředí hello stejné Chyba replikace. V takovém případě může být schopný toodiscover nebo identifikovat řešení na jeden řadič domény pak tento krok opakujte ji na jiných řadičích domény vliv hello stejná chyba.

### <a name="tombstone-lifetime"></a>Životnosti objektů označených jako neplatné
životnosti objektů označených jako neplatné Hello Určuje, jak dlouho odstraněný objekt, označuje tooas nepotřebná data, se uchovávají v databázi služby Active Directory hello. Když odstraněného objektu předává hello objektů označených jako neplatné životnost, proces kolekce paměti ji automaticky odstraní z databáze služby Active Directory hello.

životnosti objektů označených jako neplatné výchozí Hello je 180 dnů pro nejnovější verze systému Windows, ale bylo 60 dnů na starší verze a může se změnit explicitně správcem služby Active Directory.

Je důležité tooknow, pokud se vyskytnou chyby replikace, které se blíží nebo již skončila hello životnosti objektů označených jako neplatné. Pokud dva řadiče domény docházet k chybě replikace, která je uchována po hello životnosti objektů označených jako neplatné, replikace je zakázaná mezi tyto dva řadiče domény, i když hello základní Chyba replikace vyřešen.

Hello životnosti objektů označených jako neplatné oblasti pomáhá identifikovat místech, je-li zakázáno replikace nebezpečí situaci. Jednotlivé chyby v hello **více než 100 % TSL** kategorie představuje oddíl, který nebyla replikována mezi svůj zdrojový a cílový server pro na minimálně životnosti objektů označených jako neplatné hello hello doménové struktury.

V takovém případě jednoduše opravě chyba hello replikace nebude stačit. Minimálně je třeba toomanually prozkoumat tooidentify a vyčištění přetrvávání odstraněných objektů, než je možné restartovat replikace. Může být nutné i toodecommission řadič domény.

Přidání tooidentifying případné chyby replikace, které mají trvalé po hello životnosti objektů označených jako neplatné, také chcete toopay pozornost tooany chyby spadajících do hello **50 75 % TSL** nebo **TSL 75 100 %** kategorie.

Toto jsou chyby, které jsou jasně přetrvávajících odstraněných, není přechodný, a proto potřebují pravděpodobně tooresolve váš zásah. Dobrá zpráva Hello je, že se ještě nedosáhly hello životnosti objektů označených jako neplatné. Pokud jste tyto problémy opravit rychle a *před* nedostanou hello životnosti objektů označených jako neplatné, replikace můžete restartovat s minimálním ruční zásah.

Jak již bylo uvedeno dříve, hello dlaždici řídicího panelu pro stav replikace řešení hello AD zobrazuje počet hello *kritické* chyby replikace ve vašem prostředí, která je definována jako chyby, které se víc než 75 % životnosti objektů označených jako neplatné (včetně chyby, které jsou více než 100 % TSL). Zajistit tookeep tento počet na 0.

> [!NOTE]
> Všechny výpočty procento životnosti hello objektů označených jako neplatné jsou založenou na životnosti objektů označených jako neplatné skutečné hello pro doménovou strukturu Active Directory, tak můžete důvěřovat, že jsou tyto procenta správné, i když máte nastaví hodnota životnosti objektů označených jako neplatné vlastní.
>
>

### <a name="ad-replication-status-details"></a>Podrobnosti o stavu replikace AD
Pokud kliknete na libovolnou položku v jednom ze seznamů hello, uvidíte další podrobnosti o pomocí protokolu vyhledávání. výsledky Hello jsou filtrované tooshow pouze hello chyby toothat související položku. Například pokud kliknete na hello prvního řadiče domény, které se v části **stav cílového serveru (ADDC02)**, zobrazí výsledky hledání filtrovat tooshow chyby s tohoto řadiče domény, který je uveden jako cílový server hello:

![Chybám stav replikace služby Active Directory ve výsledcích hledání](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Tady můžete filtrovat další, upravit hello vyhledávací dotaz a tak dále. Další informace o používání hello hledání protokolů najdete v tématu [protokolu hledání](log-analytics-log-searches.md).

Hello **HelpLink** poli se zobrazí hello adresy URL stránky TechNet o další podrobnosti o konkrétní chybu. Můžete zkopírovat a vložit do prohlížeče okno toosee informací o řešení potíží a opravě chyby hello tento odkaz.

Můžete také kliknout na **exportovat** tooexport hello výsledků tooExcel. Export dat hello můžete vizualizovat data chyby replikace žádným způsobem, které si přejete.

![exportovaný chybám stav replikace služby Active Directory v aplikaci Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>Stav replikace AD – nejčastější dotazy
**Otázka: jak často se data stavu replikace AD aktualizovat?**
Odpověď: hello informace jsou aktualizovány každých pět dní.

**Otázka: je způsob, jak tooconfigure, jak často tato data se aktualizují?**
Odpověď: není v tuto chvíli.

**Otázka: Potřebuji tooadd všechny pracovní domény řadičů toomy OMS prostor v pořadí stav replikace toosee?**
Odpověď: Ne, je nutné přidat jenom jeden řadič domény. Pokud máte víc řadičů domény v pracovním prostoru OMS, data ze všech z nich se odešlou tooOMS.

**Otázka: nechci tooadd všechny domény řadičů toomy OMS prostoru. Použít hello stav replikace AD řešení**
Odpověď: Ano. Můžete nastavit hodnotu hello tooenable klíče registru ho. V tématu [tooenable tooOMS data řadiči domény toosend AD](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**Otázka: co je název hello hello proces, který hello shromažďování dat?**
Odpověď: AdvisorAssessment.exe

**Otázka: jak dlouho trvá toobe dat shromážděných?**
A: doba shromažďování dat závisí na velikosti hello hello prostředí služby Active Directory, ale obvykle trvá méně než 15 minut.

**Otázka: Jaký typ dat shromažďovaných?**
Odpověď: replikace informace jsou shromažďovány prostřednictvím protokolu LDAP.

**Otázka: existuje způsob, jak tooconfigure, když jsou shromažďována data?**
Odpověď: není v tuto chvíli.

**Otázka: co dělat, oprávnění potřebuji toocollect dat?**
Odpověď: normální uživatelské oprávnění tooActive adresář je dostatečné.

## <a name="troubleshoot-data-collection-problems"></a>Poradce při potížích kolekce dat
V datech toocollect pořadí hello AD stav replikace řešení pack vyžaduje alespoň jeden domény Řadič toobe připojené tooyour pracovním prostorem OMS. Až se připojíte řadič domény, zobrazí se zpráva označující, že **stále nejsou shromažďována data**.

Pokud potřebujete pomoc připojení jedním z řadičů domény, můžete zobrazit dokumentaci v [tooLog počítače připojit Windows Analytics](log-analytics-windows-agents.md). Případně, pokud je řadič domény už připojené tooan stávajícího prostředí System Center Operations Manager, můžete zobrazit dokumentaci v [tooLog připojit System Center Operations Manager Analytics](log-analytics-om-agents.md).

Pokud nechcete, aby tooconnect některé z řadičů domény přímo tooOMS nebo tooSCOM, najdete v části [tooenable tooOMS data řadiči domény toosend AD](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

## <a name="next-steps"></a>Další kroky
* Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobné údaje o stavu replikace služby Active Directory.
