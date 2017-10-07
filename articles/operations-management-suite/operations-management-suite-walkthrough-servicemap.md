---
title: "aaaService mapu řešení vlastní pracovníky ukázku | Microsoft Docs"
description: "Mapa služeb je řešení v Operations Management Suite (OMS), který automaticky zjišťuje součásti aplikace v systému Windows a systémy Linux a mapy hello komunikace mezi službami.  Toto je vlastní pracovníky ukázku, která provede pomocí mapy služeb tooidentify a diagnostikovat problém simulované ve webové aplikaci."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: bwren
ms.openlocfilehash: 13f26241cd55a9b35c07d6ca52760a968abffc64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="operations-management-suite-oms-self-paced-demo---service-map"></a>Ukázka sady Operations Management Suite (OMS) vlastním tempem – Service Map
Toto je vlastní pracovníky ukázku, která provede pomocí hello [mapy služeb řešení](operations-management-suite-service-map.md) v tooidentify Operations Management Suite (OMS) a diagnostikovat problém simulované ve webové aplikaci.  Mapa služeb automaticky vyhledá součásti aplikace v systémech Windows a Linux a mapy hello komunikace mezi službami.  Slučuje i data shromažďovaná společností jiných služeb tooassist OMS v Analýza výkonu a identifikaci problémů.  Také budete používat [přihlásit analýzy protokolů hledání](../log-analytics/log-analytics-log-searches.md) toodrill dolů na shromážděná data v pořadí tooidentify hello příčiny problému.


## <a name="scenario-description"></a>Popis scénáře
Právě jste obdrželi oznámení, že aplikace zákaznický portál: MSN hello má problémy s výkonem.  Hello pouze informace, které jste je, že tyto problémy spuštěné přibližně 4:00 am PST ještě dnes.  Nejste zcela jisti všechny součásti hello tohoto portálu hello je závislá na mimo sadu webových serverů.  

## <a name="components-and-features-used"></a>Použité komponenty a funkce
- [Řešení Service Map](operations-management-suite-service-map.md)
- [Prohledávání protokolu služby Log Analytics](../log-analytics/log-analytics-log-searches.md)


## <a name="walk-through"></a>Názorný postup

### <a name="1-connect-toohello-oms-experience-center"></a>1. Připojit toohello OMS prostředí Center
Této ukázce používá hello [Operations Management Suite prostředí Center](https://experience.mms.microsoft.com/) které poskytuje úplný prostředí OMS s ukázkovými daty. Začněte tím, že následující tento odkaz, poskytnout vaše údaje a pak vyberte hello **přehledy a analýzy** scénář.


### <a name="2-start-service-map"></a>2. Spuštění řešení Service Map
Začněte tím, že kliknete na hello hello mapy služeb řešení **mapy služeb** dlaždici.

![Dlaždice Service Map](media/operations-management-suite-walkthrough-servicemap/tile.png)

Otevře se konzola Hello mapy služeb.  V hello levém podokně je seznam počítačů ve vašem prostředí s nainstalovaným agentem hello mapy služeb.  Vyberete hello počítače, které chcete tooview z tohoto seznamu.

![Seznam počítačů](media/operations-management-suite-walkthrough-servicemap/computer-list.png)


### <a name="3-view-computer"></a>3. Zobrazení počítače
Víme, že hello webové servery se nazývají AcmeWFE001 a AcmeWFE002, takže to vypadá jako toostart přiměřené místní.  Klikněte na **AcmeWFE001**.  Zobrazí hello mapy pro AcmeWFE001 a všechny jeho závislé součásti.  Můžete zobrazit na vybraném počítači hello a které jsou spuštěny které procesy komunikují s externích služeb.

![Webový server](media/operations-management-suite-walkthrough-servicemap/web-server.png)

Nemohli jsme zajímá hello výkon naše webové aplikace, klikněte na hello **AcmeAppPool (fond aplikací služby IIS)** procesu.  Toto zobrazí hello podrobnosti pro tento proces a zvýrazňuje jeho závislé součásti.  

![Fond aplikací](media/operations-management-suite-walkthrough-servicemap/app-pool.png)


### <a name="4-change-time-window"></a>4. Změna časového intervalu

Slyšeli jsme tohoto hello problému hned spustit ve 4:00:00 Podíváme se na co se děje v daném čase. Klikněte na **časový rozsah** a změňte čas too4 hello: 00 AM PST (zachovat hello aktuální datum a upravit místním časovém pásmu) v délce 20 minut.

![Výběr času](./media/operations-management-suite-walkthrough-servicemap/time-picker.png)


### <a name="5-view-alert"></a>5. Zobrazení výstrahy

Teď vidíme, že hello **acmetomcat** závislostí se zobrazí, upozornění, tak, aby se naše potenciální problém.  Klikněte na ikonu výstrahy hello v **acmetomcat** tooshow hello podrobnosti výstrahy hello.  Vidíme, že máme kritické využití procesoru, a můžeme zobrazit další podrobnosti.  To je pravděpodobná příčina nízkého výkonu. 

![Výstrahy](./media/operations-management-suite-walkthrough-servicemap/alert.png)


### <a name="6-view-performance"></a>6. Zobrazení výkonu

Podívejme se na **acmetomcat** blíž.  Klikněte na tlačítko v hello horním rohu **acmetomcat** a vyberte **zatížení serveru mapy** tooshow hello podrobností a závislosti pro tento počítač. Můžete pak podíváme trochu více do těchto tooverify čítače výkonu naše podezření.  Vyberte hello **výkonu** kartě toodisplay hello [shromáždit čítače výkonu analýzy protokolů](../log-analytics/log-analytics-data-sources-performance-counters.md) přes hello časový rozsah.  Vidíme, že jsme zasílá pravidelné špičky v hello procesoru a paměti.

![Výkon](./media/operations-management-suite-walkthrough-servicemap/performance.png)


### <a name="7-view-change-tracking"></a>7. Zobrazení sledování změn
Podívejme se, jestli můžeme zjistit příčinu tohoto vysokého využití.  Klikněte na hello **Souhrn** kartě.  To poskytuje informace, které OMS shromáždil z hello počítače, jako se nezdařilo připojení, kritické výstrahy a změny softwaru.  Už by měl být rozšířen oddíly s zajímavé nejnovější informace a další části tooinspect informace, které obsahují můžete rozšířit.


Pokud ještě není otevřené **Sledování změn**, rozbalte ho.  Zobrazí informace získané nástrojem hello [řešení pro sledování změn](../log-analytics/log-analytics-change-tracking.md).  Vypadá to, že se během tohoto časového intervalu změnil software.  Klikněte na **softwaru** tooget podrobnosti.  Proces zálohování byl přidán toohello počítač bezprostředně za 4:00 AM, tak se zobrazuje toobe hello který hello spotřebovávanou nadměrné prostředků.

![Sledování změn](./media/operations-management-suite-walkthrough-servicemap/change-tracking.png)



### <a name="8-view-details-in-log-search"></a>8. Zobrazení podrobností v prohledávání protokolu
Jsme to ještě další ověření prohlížením hello podrobné informace o výkonu, které jsou shromážděny v úložiště analýzy protokolů hello.  Klikněte na hello **výstrahy** kartě znovu a pak na hello **vysoké využití procesoru** výstrahy.  Klikněte na **Zobrazení podrobností v prohledávání protokolu**.  Otevře se okno hledání protokolů hello, kde můžete provádět [protokolu hledání](../log-analytics/log-analytics-log-searches.md) proti všechna data uložená v úložišti hello.  Mapy služeb queriy pro nás vyplněno již výstraha hello tooretrieve zajímají nás.  

![Prohledávání protokolů](./media/operations-management-suite-walkthrough-servicemap/log-search.png)


### <a name="9-open-saved-search"></a>9. Otevření uloženého hledání
Podíváme se, pokud jsme získejte některé další informace o shromažďování výkonu hello, která vygenerovala tuto výstrahu a ověřit naše podezření, že hello problémy jsou způsobeny tento proces zálohování.  Změnit časové rozmezí hello příliš**6 hodin**.  Potom klikněte na **Oblíbené** a přejděte dolů toohello uložit hledá **mapy služeb**.  Tyto dotazy jsme vytvořili speciálně pro tuto analýzu.  Klikněte na **Top 5 Processes by CPU for acmetomcat** (Hlavních 5 procesů podle CPU pro acmetomcat).

![Uložené hledání](./media/operations-management-suite-walkthrough-servicemap/saved-search.png)


Tento dotaz vrátí seznam hodnot hello top 5 procesy nespotřebovávají procesoru na **acmetomcat**.  Si můžete prohlédnout hello dotazu tooget Úvod toohello dotazovací jazyk používá pro vyhledávání protokolu.  Pokud byste byli zájem o hello procesy v jiných počítačích, může změnit dotaz tooretrieve hello tyto informace.

V takovém případě můžete vidíte, že proces zálohování hello konzistentně využívá přibližně 60 % procesoru hello aplikačního serveru.  Je tedy zřejmé, že za naše problémy s výkonem zodpovídá právě tento nový proces.  Naše řešení bude samozřejmě tooremove tento nový zálohovacího softwaru vypněte hello aplikačního serveru.  Jsme ve skutečnosti může využít požadovaného stavu konfigurace (DSC) spravuje zásady toodefine Azure Automation, které zajistěte, aby byl že tento proces se nikdy běží na tyto důležité systémy.


## <a name="summary-points"></a>Souhrn v bodech
- [Mapa služeb](operations-management-suite-service-map.md) poskytuje přehled celé aplikace i v případě, že nevíte o všech serverech a závislostech.
- Mapa služeb zobrazí data shromažďují jiných toohelp OMS řešení můžete identifikovat problémy se vaše aplikace a jeho podpůrné infrastruktuře.
- [Přihlaste se hledání](../log-analytics/log-analytics-log-searches.md) umožňují toodrill dolů do konkrétní údaje shromažďované úložiště analýzy protokolů hello.    

## <a name="next-steps"></a>Další kroky
- Přečtěte si víc o řešení [Service Map](operations-management-suite-service-map.md).
- [Nasaďte a nakonfigurujte](operations-management-suite-service-map-configure.md) Service Map.
- Přečtěte si víc o službě [Log Analytics](../log-analytics/log-analytics-overview.md), která se pro Service Map vyžaduje a ve které jsou uložená provozní data ukládaná agenty.