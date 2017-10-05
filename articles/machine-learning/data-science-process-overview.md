---
title: "Přehled služby Azure proces vědecké účely dat Team | Microsoft Docs"
description: "Poskytuje data metodika vědecké účely k poskytování řešení prediktivní analýzy a inteligentní aplikací."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev;
ms.openlocfilehash: 006a1465a7cdced1878111beee709c8da4bc310f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="team-data-science-process-overview"></a>Přehled procesu vědecké účely Data Team

Proces pro vědecké účely Data Team (TDSP) je metodika vědecké účely agilní, iterativní data k poskytování efektivní řešení prediktivní analýzy a inteligentní aplikací. TDSP pomáhá zlepšit týmovou spolupráci a získávání informací. Obsahuje destilace osvědčené postupy a struktury od společnosti Microsoft a ostatní v odvětví které usnadňují úspěšné dokončení implementace iniciativy vědecké účely data. Cílem je pomoct společnosti plně pochopit výhody jejich analýzy programu.

Tento článek obsahuje přehled TDSP a jeho hlavní součásti. Poskytujeme obecný popis zde procesu, který může být implementováno s celou řadu nástrojů. Podrobnější popis projektu úlohy a role zahrnutých v životním cyklu procesu najdete v další související témata. K dispozici jsou také pokyny o tom, jak implementovat TDSP pomocí konkrétní sadu nástrojů Microsoft a infrastrukturu, která jsme použít k implementaci TDSP v našimi týmy.

## <a name="key-components-of-the-tdsp"></a>Klíčové komponenty TDSP

TDSP se skládá z následujících součástí klíče:

- A **datové vědy cyklu** definice
- A **standardizované struktura projektu**
- **Infrastruktura a prostředky** pro projekty data vědecké účely
- **Nástrojů a pomůcek** pro spuštění projektu


## <a name="data-science-lifecycle"></a>Životní cyklus dat vědecké účely

Proces pro vědecké účely Data Team (TDSP) poskytuje životního cyklu do struktury vývoj projekty vědecké účely data. Životní cyklus popisuje kroky, od začátku do konce, projekty obvykle postupujte při jejich spuštění.

Pokud použijete jiný vědecké účely životního cyklu data, jako [OSTRÉ DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) nebo vaše organizace vlastní proces založený na úlohách TDSP můžete dál používat v rámci těchto vývojové životní cykly. Na vysoké úrovni tyto různé metody mají hodně společné. 

Pro projekty vědecké účely dat, které se dodávají jako součást inteligentní aplikace byl navržen tak tohoto životního cyklu. Tyto aplikace nasadit machine learning nebo umělé intelligence modely pro prediktivní analýzy. Projekty vědecké účely nahodilého dat nebo ad hoc analytics projekty mohou také těžit z pomocí tohoto procesu. Ale v takovém případě některé z kroků popsaných nemusí být potřeba.    

TDSP životní cyklus se skládá z pěti hlavních fází, které jsou spouštěny interaktivně:

* **Pochopení obchodních**
* **Získávání dat a principy**
* **Modelování**
* **Nasazení**
* **Přijetí zákazníka**

Tady je vizuální reprezentace **procesu vědecké účely Team datového cyklu**. 

![TDSP cyklu](./media/data-science-process-overview/tdsp-lifecycle.png) 

Cíle, úlohy a artefakty dokumentace pro každé fáze životního cyklu v TDSP jsou popsané v [procesu vědecké účely Team datového cyklu](data-science-process-lifecycle.md) tématu. Tyto úlohy a artefaktů jsou přidruženy role projektu:

- Architekt řešení
- Správce projektu
- Vědecký pracovník dat
- Vedoucí projektu 

Následující diagram představuje zobrazení mřížky úloh (modře) a artefakty (zeleně) přidružené každé fáze životního cyklu (na vodorovné ose) pro tyto role (na svislé ose). 

![TDSP-role a úlohy](./media/data-science-process-overview/tdsp-tasks-by-roles.png)

## <a name="standardized-project-structure"></a>Struktura standardizované projektu

Všechny projekty sdílet adresářovou strukturu a použití šablon projektu dokumentů s usnadňuje členové týmu najít informace o jejich projektů. Všechny kódu a dokumenty jsou uloženy v systému správy verzí (VC), jako je Git, sady TFS nebo Subversion umožňují týmovou spolupráci. Sledování úloh a funkce v projektu agilní sledování systému jako Jira, technologie Rally, Visual Studio Team Services umožňuje blíže sledování kód pro jednotlivé funkce. Takové sledování taky umožňuje týmy a získat lepší odhadované náklady. TDSP doporučuje vytvoření samostatné úložiště pro každý projekt na VC Správa verzí, informace o zabezpečení a spolupráci. Standardizovaná strukturu pro všechny projekty pomáhá vytvářet institucionální znalost celé organizace.

Poskytujeme šablony pro strukturu složek a požadované dokumenty standardní umístění. Tato struktura složek umožňuje uspořádat soubory obsahující kód pro zkoumání dat a funkce extrakce, a který záznam modelu iterací. Tyto šablony usnadňují členové týmu, abyste pochopili práci jinými uživateli a přidat nové členy do týmů. Je snadné k zobrazování a aktualizace šablony dokumentů ve formátu markdown. Použití šablon poskytnout klíčové otázky pro každý projekt zajistit, že problém je dobře definovaný a že výsledek splňují kvality očekávaný kontrolní seznamy. Příklady obsahují:

- Projekt titulů do dokumentů obchodního problému a rozsah projektu
- sestavy dat dokumentu strukturu a statistiky nezpracovaná data
- model sestavy do odvozené funkce dokumentů
- metriky výkonu modelu například křivek ROC nebo MSE


![TDSP adresáře](./media/data-science-process-overview/tdsp-dir-structure.png)

Strukturu adresáře můžete klonovat z [Githubu](https://github.com/Azure/Azure-TDSP-ProjectTemplate).

## <a name="infrastructure-and-resources-for-data-science-projects"></a>Infrastruktury a prostředky pro projekty data vědecké účely

TDSP poskytuje doporučení pro správu sdílené analýzy a infrastruktury úložiště, jako:

- cloudové systémy souborů pro ukládání datových sad 
- databáze
- velké objemy dat (Hadoop nebo Spark) clustery 
- služby Machine learning. 

Infrastruktura analýzy a úložiště může být v cloudu nebo místně. Toto je, kde jsou uložené datové sady nezpracovaná a zpracovány. Tato infrastruktura umožňuje reprodukovatelnou analýzu. Ho také předejdete duplikace, což může vést k nekonzistenci a náklady na infrastrukturu zbytečné. Nástroje jsou k dispozici a zřizovat sdílené prostředky, je sledovat a povolit každý člen týmu pro zabezpečené připojení k prostředkům. Je také vhodné mít členy projektu vytvořit konzistentní výpočetním prostředí. Členové týmu různých můžete replikovat a ověřit experimenty.

Tady je příklad týmu pracující na více projektů a sdílení různých součástí infrastruktury cloudu analytics.

![TDSP infrastruktury](./media/data-science-process-overview/tdsp-analytics-infra.png)


## <a name="tools-and-utilities-for-project-execution"></a>Nástroje pro spuštění projektu

Představení procesy v většina organizací je náročná. Nástroje poskytované implementace nápovědy proces a životního cyklu vědecké účely data nižší se překážek a zvýšit konzistence jejich přijetí. TDSP poskytuje počáteční sadu nástrojů a skripty, které vám pomohou rychle začít přijetí TDSP v rámci týmu. Pomáhá také automatizovat některé běžné úlohy v životním cyklu vědecké účely data například zkoumání dat a modelování směrného plánu. Není dobře nastavené struktury zadaný pro jednotlivce přispívání sdílené nástrojů a pomůcek do úložiště sdíleného kódu jejich tým. Tyto prostředky lze poté využít další projekty v rámci tým nebo organizace. TDSP také plány povolit příspěvky nástrojů a pomůcek celé komunitě. Nástroje TDSP dají klonovat z [Githubu](https://github.com/Azure/Azure-TDSP-Utilities).


## <a name="next-steps"></a>Další kroky

[Proces vědecké účely dat Team: Rolí a úloh](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/roles-tasks.md) popisuje role klíče pracovníky a jejich přidružených úloh pro data tým vědecké účely, standardizující tohoto postupu. 
