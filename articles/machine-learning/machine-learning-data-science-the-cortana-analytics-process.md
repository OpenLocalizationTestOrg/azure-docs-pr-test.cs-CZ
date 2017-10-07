---
title: "aaaWhat je proces vědecké účely Team dat?  | Dokumentace Microsoftu"
description: "Hello proces vědecké účely Team dat je systematické metoda pro vytvoření inteligentního aplikace, které využívají pokročilou analýzu."
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
redirect_document_id: True
ms.openlocfilehash: 57187be9c884389c13c226eab74aff137f5514a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-team-data-science-process-tdsp"></a>Co je hello tým datové vědy procesu (TDSP)?
Hello [tým datové vědy procesu (TDSP)](data-science-process-overview.md) poskytuje systematicky toobuilding inteligentního aplikace, které umožňuje týmy data vědců toocollaborate efektivně přes hello úplný životní cyklus aktivity potřebné tooturn těchto aplikací do produktů. Popisuje pořadí kroků, které poskytují Hello TDSP **pokyny** na tom, jak toodefine hello problému, nastavení hello nástrojů a prostředí potřeby analyzovat relevantní data, vytvářet a vyhodnotit prediktivní modely a pak nasadit tyto modely v podnikové aplikace. 

Tady jsou kroky hello v **proces vědecké účely dat Team**:  

![Zakončení workflow](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

proces Hello je **iterativní**: hello porozumět nové a stávající nebo obecnější v modelu hello zpracovaní a vyžaduje přebudování dříve dokončena hello pořadí kroků. Stávající organizační vývoj a plánování procesy projektů jsou **snadno přizpůsobena** toowork s hello definované TDSP pořadí kroků. 

Hello kroků v procesu hello jsou na tomto serveru provedeny a propojené v hello [TDSP studijní](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a popsané níže.  

## <a name="preparation-steps"></a>Přípravné kroky
## <a name="p1-plan-hello-analytics-project"></a>P1. Plánování projektu analytics hello
Spusťte projekt analytics definováním obchodních cílů a problémů. Jsou uvedené z hlediska **obchodní požadavky**. Ústředním cílem tohoto kroku je tooidentify hello klíče obchodní proměnné (prodej prognózy nebo hello pravděpodobnost pořadí podvodné, třeba) které hello toosatisfy toopredict analysis musí tyto požadavky. Další informace o plánování je pak toodevelop obvykle základní představu o hello **zdroje dat** potřeby tooaddress hello cíli projektu hello z analytické perspektivy. Není, například toofind, že existující systémy potřebovat toocollect a protokolu další typy dat tooaddress hello problém a dosáhnout cíle projektu hello. Pokyny najdete v tématu [plánování prostředí pro hello proces vědecké účely dat Team](machine-learning-data-science-plan-your-environment.md) a [scénáře pro pokročilou analýzu v Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).  

## <a name="p2-setup-analytics-environment"></a>P2. Nastavení analytického prostředí
Analýza prostředí pro hello proces vědecké účely dat Team zahrnuje několik komponent: 

* **pracovní prostory data** kde hello dat je připravený pro analýzu a modelování, 
* **infrastrukturu zpracování** pro předběžné zpracování, zkoumat a modelování hello dat
* **infrastrukturu modulu runtime** toooperationalize hello analytical modely a spusťte hello inteligentního klientských aplikací, které využívají hello modelů.  

Hello analytics infrastrukturu, která musí instalační program toobe je často součástí prostředí, která je oddělená od základní operační systémy. Ale obvykle využívá data z více systémů v rámci podniku hello i ze společnosti toohello externí zdroje. Hello analytics infrastruktury může být čistě cloudové, nebo instalaci na místní nebo hybridní hello dva. Možnosti najdete v tématu [nastavení prostředí vědecké účely dat pro použití v hello proces vědecké účely dat Team](machine-learning-data-science-environment-setup.md).

## <a name="analytics-steps"></a>Analýza kroky:
## <a name="1-ingest-data-into-hello-analytical-environment"></a>1. Ingestovat Data do prostředí analytical hello
prvním krokem Hello je toobring hello relevantní data z různých zdrojů, buď v rámci nebo z mimo hello enterprise, do analytické prostředí, kde lze zpracovat hello data. Hello **formát** z hello u zdroje dat se můžou lišit od formátu hello vyžaduje cílový hello. Některé transformaci dat tak může mít toobe provádí hello přijímání nástrojů. Možnosti najdete v tématu [načtení dat do úložiště prostředí pro analýzu](machine-learning-data-science-ingest-data.md)

V přijímání počáteční přidání toohello dat jsou mnoho inteligentní aplikace vyžaduje toorefresh hello data pravidelně jako součást procesu probíhající učení. To můžete provést nastavením **datovém kanálu** nebo pracovního postupu. To je součástí hello iterativní součástí hello proces, který zahrnuje znovu sestavit a znovu vyhodnocení analytical modely hello používá inteligentního aplikace hello nasazení řešení hello. Zobrazit, například [přesun dat z tooSQL serveru SQL místní Azure s Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md).

## <a name="2-explore-and-pre-process-data"></a>2. Prozkoumání a předzpracování dat
Hello dalším krokem je tooobtain podrobnější vysvětlení hello dat na odstranění příčin jeho **souhrnné statistiky** , vztahy a pomocí techniky takové **vizualizace**. Toto je také kde problémy z **kvality dat.** a integritu, například chybějící hodnoty, neshody typu dat a relace nekonzistentní data, jsou zpracovávány. Předběžné zpracování transformace jsou použité tooclean hello nezpracovaná data před další analýza a modelování může proběhnout. Popis najdete v tématu [tooprepare dat pro rozšířené machine learning úkolů](machine-learning-data-science-prepare-data.md).

## <a name="3-develop-features"></a>3. Vývoj funkce
Datových vědců ve spolupráci s odborníky domény, musí identifikovat hello funkce zachycení hello nejdůležitějšími vlastnosti hello datové sady a který může být nejvhodnější použít toopredict hello firmy klíče proměnné identifikovaný při plánování. Tyto nové funkce může být odvozen z existujících dat nebo můžou vyžadovat další data toobe shromažďují. Tento proces se označuje jako **konstruování** a je jedním z hello klíčové kroky při vytváření systému efektivní prediktivní analýzy. Tento krok vyžaduje tvůrčí kombinaci domény znalosti a hello statistiky získat z kroku zkoumání dat hello. Pokyny najdete v tématu [konstruování v hello proces vědecké účely dat Team](machine-learning-data-science-create-features.md).

## <a name="4-create-predictive-models"></a>4. Vytváření prediktivních modelů
Datových vědců sestavení analytical modely toopredict hello klíče proměnné identifikovaný hello obchodní požadavky definované v plánování krok pomocí dat, která byla vyčištěna a featurized hello. Machine learning systémy podporovat více **modelování algoritmy** , které jsou použitelné tooa širokou škálu případech. Pokyny najdete v tématu [jak toochoose algoritmy pro tým Azure Machine Learning](machine-learning-algorithm-choice.md).

Datových vědců musíte zvolit hello nejvhodnější model pro svoji úlohu předpovědi a není, že výsledky z více modelů nutné toobe kombinaci tooobtain hello nejlepších výsledků. Hello vstupní data pro modelování je obvykle rozdělené náhodně do tří částí:

* trénovací datové sady 
* ověření datové sady 
* testování datové sady 

Hello modely jsou vytvořeny pomocí hello **trénovací datové sady**. Hello optimální modelů (s parametry přizpůsobená) je vybrána kombinace spuštěním hello modely a měření hello předpovědi chyby pro hello **ověření datové sady**. Nakonec hello **testovací datové sady** je použité tooevaluate hello výkon hello vybrali modelu na nezávislé data, která nebyla použité tootrain nebo ověření modelu hello.  Postupy najdete v tématu [jak tooevaluate modelu výkon v Azure Machine Learning](machine-learning-evaluate-model-performance.md).

## <a name="5-deploy-and-consume-models"></a>5. Nasazení a využívat modely
Jakmile jsme sadu modely, které provádějí dobře, může být **operationalized** pro tooconsume jiné aplikace. V závislosti na požadavcích hello firmy, jsou vytvářeny předpovědi v **reálném čase** nebo na **batch** intervalech. toobe operationalized, hello modely mají toobe zveřejněné s **otevřete rozhraní API** snadno použít z různých aplikací takové online webu, tabulek, řídicí panely nebo obchodní a back-end aplikace. V tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Souhrn a další kroky
Hello [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) je modelovaná jako pořadí kroků, vstupní, **obsahují pokyny,** hello úloh potřebné toouse pokročilé analýzy toobuild inteligentní aplikace. Každý krok také podrobné informace o tom, jak toouse různé úlohy hello toocomplete Microsoft technologie popsané. 

Při TDSP neuloží konkrétní typy **dokumentace** artefaktů, je nejlepší výsledky hello toodocument postupem hello zkoumání dat, modelování a vyhodnocení a toosave hello příslušné kód, který hello analýzy můžete vstupní v případě potřeby. To také umožňuje opakované použití hello analytics funguje při práci na jiné aplikace zahrnující podobná data a úlohy předpovědi.

Úplné návody začátku do konce, které ukazují všechny kroky hello v hello proces **konkrétních scénářů** jsou také uvedeny. V tématu, například:

* [Hello proces vědecké účely Team dat v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md)
* [Hello proces vědecké účely Team dat v akci: pomocí clusterů systému HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md).
* [Vědecké zpracování dat pomocí Spark v Azure HD.mdnsight](machine-learning-data-science-spark-overview.md)
* [Škálovatelné vědecké zpracování dat v Azure Data Lake: návod začátku do konce](machine-learning-data-science-process-data-lake-walkthrough.md)

