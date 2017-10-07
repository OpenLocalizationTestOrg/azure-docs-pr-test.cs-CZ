---
title: "Přehled procesu vědecké účely Data Team aaaAzure | Microsoft Docs"
description: "Poskytuje data vědecké účely metodika toodeliver prediktivní analýzy řešení a inteligentní aplikace."
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
ms.openlocfilehash: 2ba03585a6f6f855faaa3b5c0c75149cad0a88bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="team-data-science-process-overview"></a>Přehled procesu vědecké účely Data Team

Hello tým datové vědy procesu (TDSP) je agilní, řešení prediktivní analýzy toodeliver metodika vědecké účely iterativní dat a inteligentní aplikace efektivně. TDSP pomáhá zlepšit týmovou spolupráci a získávání informací. Obsahuje destilace hello osvědčené postupy a struktury od společnosti Microsoft a ostatní v odvětví hello které usnadňují hello úspěšné dokončení implementace iniciativy vědecké účely data. Hello cílem je, že toohelp společnosti plně mějte na paměti hello výhod programu jejich analýzy.

Tento článek obsahuje přehled TDSP a jeho hlavní součásti. Poskytujeme obecný popis zde hello procesu, který může být implementováno s celou řadu nástrojů. Podrobnější popis hello projektu úlohy a role zahrnutých v průběhu životního cyklu hello hello procesu najdete v další související témata. Pokyny jak také zajišťuje tooimplement hello TDSP pomocí konkrétní sadu nástrojů Microsoft a infrastruktury, že používáme tooimplement hello TDSP našimi týmy.

## <a name="key-components-of-hello-tdsp"></a>Klíčové komponenty hello TDSP

TDSP se skládá ze hello následující klíčové komponenty:

- A **datové vědy cyklu** definice
- A **standardizované struktura projektu**
- **Infrastruktura a prostředky** pro projekty data vědecké účely
- **Nástrojů a pomůcek** pro spuštění projektu


## <a name="data-science-lifecycle"></a>Životní cyklus dat vědecké účely

Hello tým datové vědy procesu (TDSP) poskytuje hello vývoji toostructure životního cyklu vědecké účely projektů data. životní cyklus Hello popisuje hello kroky, z toofinish spuštění, který projekty obvykle postupujte při jejich spuštění.

Pokud použijete jiný vědecké účely životního cyklu data, jako [OSTRÉ DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) nebo vaše organizace vlastní proces, můžete dál používat hello založený na úlohách TDSP v kontextu hello těchto vývoje životní cykly. Na vysoké úrovni tyto různé metody mají hodně společné. 

Pro projekty vědecké účely dat, které se dodávají jako součást inteligentní aplikace byl navržen tak tohoto životního cyklu. Tyto aplikace nasadit machine learning nebo umělé intelligence modely pro prediktivní analýzy. Projekty vědecké účely nahodilého dat nebo ad hoc analytics projekty mohou také těžit z pomocí tohoto procesu. Ale v takových případech některé hello postupu nemusí být potřeba.    

Hello TDSP životní cyklus se skládá z pěti hlavních fází, které jsou spouštěny interaktivně:

* **Pochopení obchodních**
* **Získávání dat a principy**
* **Modelování**
* **Nasazení**
* **Přijetí zákazníka**

Tady je vizuální reprezentace hello **procesu vědecké účely Team datového cyklu**. 

![TDSP cyklu](./media/data-science-process-overview/tdsp-lifecycle.png) 

Hello cíle, úlohy a artefakty dokumentace pro každé fáze životního cyklu hello v TDSP jsou popsané v hello [procesu vědecké účely Team datového cyklu](data-science-process-lifecycle.md) tématu. Tyto úlohy a artefaktů jsou přidruženy role projektu:

- Architekt řešení
- Správce projektu
- Odborník přes data
- Vedoucí projektu 

Hello následující diagram představuje zobrazení mřížky hello úloh (modře) a artefakty (zeleně) přidružené každé fáze životního cyklu hello (na vodorovné ose hello) pro tyto role (na svislé ose hello). 

![TDSP-role a úlohy](./media/data-science-process-overview/tdsp-tasks-by-roles.png)

## <a name="standardized-project-structure"></a>Struktura standardizované projektu

Všechny projekty sdílet adresářovou strukturu a použití šablon projektu dokumentů s usnadňuje hello team členy toofind informace o jejich projektů. Všechny kódu a dokumenty jsou uloženy v systému správy verzí (VC), jako je Git, sady TFS nebo Subversion tooenable týmovou spolupráci. Sledování úloh a funkce v projektu agilní sledování systému jako Jira, technologie Rally, Visual Studio Team Services umožňuje blíže sledování hello kódu pro jednotlivé funkce. Takové sledování taky umožňuje lépe náklady odhady tooobtain týmy. TDSP doporučuje vytvoření samostatné úložiště pro každý projekt na hello VC Správa verzí, informace o zabezpečení a spolupráci. Hello standardizované strukturu pro všechny projekty pomáhá sestavení institucionální znalostní báze napříč hello organizace.

Poskytujeme šablony pro strukturu složek hello a požadované dokumenty standardní umístění. Tato struktura složek organizuje hello soubory obsahující kód pro zkoumání dat a funkce extrakce, a který zaznamenejte modelu iterací. Tyto šablony usnadňují team členy toounderstand objem práce jinými uživateli a tooadd nové členy tooteams. Je snadno tooview a aktualizace šablony dokumentů ve formátu markdown. Použijte kontrolní seznamy tooprovide šablony s klíčové otázky pro každý projekt tooinsure, problém hello je dobře definovaný a že výsledek splňovat hello kvality očekává. Příklady obsahují:

- Projekt titulů toodocument hello obchodního problému a rozsah projektu hello
- Struktura hello toodocument dat sestavy a statistiky hello nezpracovaná data
- model sestavy toodocument hello odvozené funkce
- metriky výkonu modelu například křivek ROC nebo MSE


![TDSP adresáře](./media/data-science-process-overview/tdsp-dir-structure.png)

struktura adresářů Hello dají klonovat z [Githubu](https://github.com/Azure/Azure-TDSP-ProjectTemplate).

## <a name="infrastructure-and-resources-for-data-science-projects"></a>Infrastruktury a prostředky pro projekty data vědecké účely

TDSP poskytuje doporučení pro správu sdílené analýzy a infrastruktury úložiště, jako:

- cloudové systémy souborů pro ukládání datových sad 
- databáze
- velké objemy dat (Hadoop nebo Spark) clustery 
- služby Machine learning. 

Hello analytics infrastruktury a infrastruktury úložiště může být v hello cloudu nebo místně. Toto je, kde jsou uložené datové sady nezpracovaná a zpracovány. Tato infrastruktura umožňuje reprodukovatelnou analýzu. Ho také předejdete duplikace, což může vést tooinconsistencies a náklady na infrastrukturu zbytečné. Nástroje jsou k dispozici tooprovision hello sdílené prostředky, je sledovat a povolit každé tooconnect člen týmu toothose prostředky bezpečně. Je také vhodné mít členy projektu vytvořit konzistentní výpočetním prostředí. Členové týmu různých můžete replikovat a ověřit experimenty.

Tady je příklad týmu pracující na více projektů a sdílení různých součástí infrastruktury cloudu analytics.

![TDSP infrastruktury](./media/data-science-process-overview/tdsp-analytics-infra.png)


## <a name="tools-and-utilities-for-project-execution"></a>Nástroje pro spuštění projektu

Představení procesy v většina organizací je náročná. Nástroje, pokud proces tooimplement hello dat vědy a životního cyklu pomohou nižší tooand překážek hello zvýšit hello konzistence jejich přijetí. TDSP poskytuje počáteční sadu nástrojů a skriptů toojump počáteční přijetí TDSP v rámci týmu. Pomáhá také automatizovat některé běžné úlohy hello v hello datové vědy životního cyklu například zkoumání dat a modelování směrného plánu. Zadaný pro jednotlivce toocontribute sdílené nástrojů a pomůcek do úložiště sdíleného kódu jejich tým není dobře nastavené struktury. Tyto prostředky můžete využít pak další projekty v rámci hello tým nebo organizace hello. TDSP také plány tooenable hello příspěvky ze strany komunity celou toohello, nástrojů a pomůcek. Hello TDSP nástrojů můžete klonovat z [Githubu](https://github.com/Azure/Azure-TDSP-Utilities).


## <a name="next-steps"></a>Další kroky

[Proces vědecké účely dat Team: Rolí a úloh](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/roles-tasks.md) popisuje role hello klíče pracovníky a jejich přidružených úloh pro data tým vědecké účely, standardizující tohoto postupu. 
