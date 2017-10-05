---
title: "Co je proces vědecké účely Team dat?  | Dokumentace Microsoftu"
description: "Proces vědecké účely Team dat je systematické metodu pro vytvoření inteligentního aplikace, které využívají pokročilou analýzu."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a098aa2e-fd79-4543-8e15-9aae9d8b3ee6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2017
ms.author: bradsev
ROBOTS: NOINDEX
redirect_url: data-science-process-overview
redirect_document_id: TRUE
ms.openlocfilehash: d1ec602b2a69b0bd01bf7b43ef5fed9b8c2781c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-the-team-data-science-process-tdsp"></a>Co je vědecké zpracování týmových dat (TDSP)?
[Tým datové vědy procesu (TDSP)](data-science-process-overview.md) poskytuje systematicky k sestavení inteligentní aplikace umožňující týmy datových vědců efektivně spolupracovat přes celý životní potřeby Chcete-li tyto aktivity aplikace do produktů. Popisuje pořadí kroků, které poskytují TDSP **pokyny** o tom, jak definovat problém, nastavte nástroje a prostředí potřeby analýzy relevantní data, vytvářet a vyhodnotit prediktivní modely a pak nasadit tyto modely v podniku aplikace. 

Tady jsou kroky v **proces vědecké účely dat Team**:  

![Zakončení workflow](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

Proces je **iterativní**: pochopení nové a stávající nebo obecnější v modelu zpracovaní a vyžaduje přebudování dříve dokončit v pořadí kroků. Stávající organizační vývoj a plánování procesy projektů jsou **snadno přizpůsobena** pro práci s definované TDSP pořadí kroků. 

Kroky v procesu jsou na tomto serveru provedeny a propojené v [TDSP studijní](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a popsané níže.  

## <a name="preparation-steps"></a>Přípravné kroky
## <a name="p1-plan-the-analytics-project"></a>P1. Plánování projektu analytics
Spusťte projekt analytics definováním obchodních cílů a problémů. Jsou uvedené z hlediska **obchodní požadavky**. Ústředním cílem tohoto kroku je identifikace klíče obchodní proměnné (prognózy prodeje nebo pravděpodobnost pořadí podvodné, např.), které analýza potřebuje k předvídání splňovat tyto požadavky. Další informace o plánování je potom obvykle nezbytně nutné, aby pochopili jejich **zdroje dat** potřebné pro adresu cíli projektu z analytické perspektivy. Například není neobvyklé, zjistíte, je potřeba stávajících systémů shromažďovat a protokolovat další typy dat potíže vyřešit a dosáhnout cíle projektu. Pokyny najdete v tématu [plánování prostředí pro proces tým datové vědy](machine-learning-data-science-plan-your-environment.md) a [scénáře pro pokročilou analýzu v Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).  

## <a name="p2-setup-analytics-environment"></a>P2. Nastavení analytického prostředí
Analýza prostředí pro proces tým datové vědy zahrnuje několik komponent: 

* **pracovní prostory data** tam, kde je vynášené data pro analýzu a modelování, 
* **infrastrukturu zpracování** pro předběžné zpracování, zkoumat a modelování dat
* **infrastrukturu modulu runtime** zprovoznit analytical modely a spuštění aplikace, které využívají modely inteligentního klienta.  

Analýza infrastrukturu, která je potřeba nastavit je často součástí prostředí, která je oddělená od základní operační systémy. Ale obvykle využívá data z více systémů v podniku i ze zdroje mimo společnost. Infrastruktura analytics může být čistě cloudové, nebo instalaci na místní nebo hybridní dvou. Možnosti najdete v tématu [nastavení prostředí vědecké účely dat pro použití v procesu Team datové vědy](machine-learning-data-science-environment-setup.md).

## <a name="analytics-steps"></a>Analýza kroky:
## <a name="1-ingest-data-into-the-analytical-environment"></a>1. Ingestovat Data do analytical prostředí
Prvním krokem je zajištění příslušných dat z různých zdrojů, buď z nebo z mimo organizaci, analytické prostředích, kde lze zpracovat data. **Formát** u zdroje dat se můžou lišit od formátu vyžadovanou cíl. Proto některé transformaci dat může mít také provést pomocí nástrojů přijímání. Možnosti najdete v tématu [načtení dat do úložiště prostředí pro analýzu](machine-learning-data-science-ingest-data.md)

Kromě počáteční přijímání dat je potřeba pravidelně aktualizuje data v rámci procesu probíhající learning mnoho inteligentní aplikace. To můžete provést nastavením **datovém kanálu** nebo pracovního postupu. To je součástí část iterativní proces, který zahrnuje znovu sestavit a znovu vyhodnocení analytical modely používá inteligentního aplikace nasazení řešení. Zobrazit, například [přesun dat z místního serveru SQL do SQL Azure s Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md).

## <a name="2-explore-and-pre-process-data"></a>2. Prozkoumání a předzpracování dat
Dalším krokem je získat podrobnější vysvětlení dat zjišťováním jeho **souhrnné statistiky** , vztahy a pomocí techniky takové **vizualizace**. Toto je také kde problémy z **kvality dat.** a integritu, například chybějící hodnoty, neshody typu dat a relace nekonzistentní data, jsou zpracovávány. Předběžné zpracování transformace se používá k vyčištění nezpracovaná data před další analýza a modelování může proběhnout. Popis najdete v tématu [úkoly přípravy dat pro vylepšený strojového učení](machine-learning-data-science-prepare-data.md).

## <a name="3-develop-features"></a>3. Vývoj funkce
Datových vědců ve spolupráci s odborníky domény, musíte určit funkce, která zachycení nejdůležitějšími vlastnosti datové sady a který dají nejlíp využít k předvídání klíče obchodní proměnné identifikovaný při plánování. Tyto nové funkce mohou být odvozen z existujících dat nebo můžou vyžadovat další data, které se mají shromažďovat. Tento proces se označuje jako **konstruování** a patří mezi klíčové kroky při vytváření systému efektivní prediktivní analýzy. Tento krok vyžaduje tvůrčí kombinací domény znalosti a statistiky získat z kroku zkoumání dat. Pokyny najdete v tématu [konstruování v procesu Team datové vědy](machine-learning-data-science-create-features.md).

## <a name="4-create-predictive-models"></a>4. Vytváření prediktivních modelů
Datových vědců sestavení analytical modely k předvídání klíče proměnné identifikovaný obchodní požadavky definované v kroku data, která byla vyčištěna a featurized plánování. Machine learning systémy podporovat více **modelování algoritmy** které se vztahují k široké škále případů. Pokyny najdete v tématu [jak zvolit algoritmy pro tým Azure Machine Learning](machine-learning-algorithm-choice.md).

Datových vědců musíte zvolit nejvhodnější model pro svoji úlohu předpovědi a není, že výsledky z více modelů potřeba kombinovat k dosažení nejlepších výsledků. Vstupní data pro modelování je obvykle náhodně rozdělené do tří částí:

* trénovací datové sady 
* ověření datové sady 
* testování datové sady 

Modely jsou vytvořené pomocí **trénovací datové sady**. Spuštěním modely a měření v chybách předpovědi je vybrána optimální kombinace modelů (s parametry přizpůsobená) **ověření datové sady**. Nakonec **testovací datové sady** slouží k vyhodnocení výkonu příslušného zvolené modelu na nezávislé dat, který nebyl použit k trénování případně k ověření modelu.  Postupy najdete v tématu [postup vyhodnocení modelu výkon v Azure Machine Learning](machine-learning-evaluate-model-performance.md).

## <a name="5-deploy-and-consume-models"></a>5. Nasazení a využívat modely
Jakmile jsme sadu modely, které provádějí dobře, může být **operationalized** u ostatních aplikací používat. V závislosti na obchodní požadavky, jsou vytvářeny předpovědi v **reálném čase** nebo na **batch** intervalech. Chcete-li být operationalized, třeba zpřístupnit s modely **otevřete rozhraní API** snadno použít z různých aplikací takové online webu, tabulek, řídicí panely nebo obchodní a back-end aplikace. V tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Souhrn a další kroky
[Proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) je modelovaná jako pořadí kroků, vstupní, **obsahují pokyny,** na úlohách, které jsou potřeba k použití pokročilou analýzu vytvářet inteligentního aplikace. Každý krok také obsahuje podrobné informace o tom, jak splnit úkoly popsané pomocí různých technologií Microsoft. 

Při TDSP neuloží konkrétní typy **dokumentace** artefaktů, je osvědčeným postupem dokumentu výsledky zkoumání dat, modelování a vyhodnocení a uložte příslušné kód tak, aby možné analýza vstupní vyžádání. To také umožňuje opakované použití analytics práce při práci na jiné aplikace zahrnující podobná data a úlohy předpovědi.

Úplné návody začátku do konce, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. V tématu, například:

* [Proces Team dat. vědecké účely v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md)
* [Proces Team dat. vědecké účely v akci: pomocí clusterů systému HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md).
* [Vědecké zpracování dat pomocí Spark v Azure HD.mdnsight](machine-learning-data-science-spark-overview.md)
* [Škálovatelné vědecké zpracování dat v Azure Data Lake: návod začátku do konce](machine-learning-data-science-process-data-lake-walkthrough.md)

