---
title: "aaaIdentify scénáře a plánování vašeho procesu analýzy - Azure | Microsoft Docs"
description: "Plán pro pokročilou analýzu v úvahu řadu klíčové otázky."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 421520dd-7728-4d29-889c-ebe6a0a6fb07
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: e445973be0d020a4f9949e5c9d8554fbbd4b515f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooidentify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Jak tooidentify scénáře a plán pro pokročilé analýzy zpracování dat
Jaké prostředky měli můžete naplánovat tooinclude při nastavení prostředí toodo pokročilou analýzu zpracování na datovou sadu? Tento článek navrhuje řadu tooask otázky, které pomohou identifikovat hello úlohy a prostředky, které jsou relevantní váš scénář. Hello pořadí kroků pro prediktivní analýzy popsané v [co je hello tým datové vědy procesu (TDSP)?](data-science-process-overview.md). Každá z těchto kroků bude vyžadovat specifické prostředky pro konkrétní scénář relevantní tooyour hello úlohy. Hello klíčové otázky tooidentify váš scénář problém data logistiky, charakteristiky, kvality hello hello datové sady a nástrojů hello a jazyky dáváte přednost toodo hello analýzy.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistic dotazy: dat – umístění a přesun
Hello logistic dotazy se týkají hello umístění hello **zdroj dat**, hello **cílového místa** v Azure a požadavky pro přesunutí hello data, včetně hello plán, velikost a prostředky zahrnuta. Hello dat může být nutné toobe přesunout několikrát během analýzy hello. Obvyklým scénářem je toomove místní data na určitou formu úložiště v Azure a pak na Machine Learning Studio.

1. **Co je zdroj dat?** Je místní nebo v cloudu hello? Například:
   
   * Hello data jsou veřejně dostupné v adresa protokolu HTTP.
   * Hello data nachází v místní nebo síťové umístění souboru.
   * Hello data jsou v databázi systému SQL Server.
   * Hello data se ukládají do kontejneru úložiště Azure
2. **Co je Azure cílové hello?** Kde nemusí toobe pro zpracování nebo modelování? Například:
   
   * Azure Blob Storage
   * Databáze SQL Azure
   * SQL Server na virtuálním počítači Azure
   * HDInsight (Hadoop v Azure) nebo tabulek Hive
   * Azure Machine Learning
   * Připojit Azure virtuální pevné disky.
3. **Jak budete toomove hello dat?**  hello postupy a prostředky k dispozici tooingest ani načíst data do různých jiného úložiště a zpracování prostředí jsou uvedeny v následujících tématech hello.
   
   * [Načtení dat do úložiště prostředí pro analýzu](machine-learning-data-science-ingest-data.md)
   * [Importu trénovacích dat do Azure Machine Learning Studio z různých zdrojů dat](machine-learning-data-science-import-data.md).
4. **Potřebuje hello data toobe přesunout v pravidelných intervalech nebo změnit během migrace?** Zvážit použití Azure Data Factory (ADF), když data potřebám toobe průběžně migrovat, zvlášť pokud hybridní scénář, který přistupuje k místní a cloudové prostředky je zahrnuta, nebo když hello dat je zpracován musí toobe změnit nebo na obchodní logiku Přidat tooit v postupu hello se migruje. Další informace najdete v tématu [přesun dat z tooSQL serveru SQL místní Azure s Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md)
5. **Kolik dat hello je tooAzure toobe přesunout?** Datové sady, které jsou velké může překročit kapacitu úložiště hello určité prostředí. Příklad najdete v tématu hello diskuzi o omezení velikosti pro Machine Learning Studio v další části hello. V takových případech ukázku hello dat lze během analýzy hello. Podrobné informace o tom, jak toodown – ukázka datovou sadu v různých prostředích s Azure, najdete v části [ukázková data v hello proces vědecké účely dat Team](machine-learning-data-science-sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Otázky ohledně dat charakteristiky: typ, formát a velikosti
Tyto otázky jsou klíče tooplanning úložiště a zpracování prostředí, jež jsou příslušné toovarious typy dat a každý z nich mají určitá omezení.

1. **Jaké jsou typy dat hello?** Například:
   
   * Číselné
   * Kategorické
   * Řetězce
   * Binární
2. **Vaše data formátování?** Například:
   
   * Textový soubor s oddělovači (CSV) nebo plochých souborů oddělené tabulátory (TSV)
   * Komprimované nebo nekomprimovaným
   * Azure BLOB
   * Tabulek Hadoop Hive
   * Tabulky serveru SQL Server
3. **Jak velké jsou vaše data?**
   
   * Malá: menší než 2GB
   * Střední: Větší než 2GB a menší než 10GB
   * Velké: Větší než 10GB

Provede hello Azure Machine Learning Studio prostředí, například:

* Seznam formáty dat hello a typy podporované systémem Azure Machine Learning Studio najdete v tématu [formáty dat a datové typy podporované](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) části.
* Informace o omezení dat pro Azure Machine Learning Studio najdete v tématu hello **jak velká může hello být datová sada pro mé moduly?** části [import a export dat pro Machine Learning](machine-learning-faq.md#machine-learning-studio-questions)

Informace o omezení hello používá v procesu analýzy hello jinými službami Azure, najdete v části [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Otázky ohledně kvality dat: zkoumání a předběžného zpracování
1. **Co víte o svá data?** Prozkoumejte data Pokud budete potřebovat pochopení toogain jeho základními charakteristikami. Co vzory nebo trendů ho předměty, co je odlehlé hodnoty, případně chybí počet hodnot. Tento krok je důležitý pro určení rozsahu hello předzpracováním potřeby pro formulování hypotézy, které by mohla naznačovat hello nejvhodnější funkce nebo zadejte analýzy a formulování plány pro shromažďování dalších dat. Výpočet popisný statistiky a vykreslení vizualizace jsou užitečné techniky pro data kontroly. Podrobnosti o tom, jak tooexplore datovou sadu v různých prostředích s Azure, najdete v části [zkoumat data v hello proces vědecké účely dat Team](machine-learning-data-science-explore-data.md).
2. **Vyžaduje hello data předzpracováním nebo čištění?**
   Předběžné zpracování a vyčištění dat jsou důležité úkoly, které obvykle musí být provedeny před datové sady je možné efektivně pro machine learning. Nezpracovaná data, je často aktivní nebo nespolehlivé a může být chybějící hodnoty. Pomocí těchto údajů pro modelování může vytvářet zavádějící výsledky. Popis najdete v tématu [tooprepare dat pro rozšířené machine learning úkolů](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Otázky týkající se nástroje a jazyky
Existuje mnoho možností v závislosti na tom, jaké jazyky a vývojových prostředích nebo nástroje potřebujete nebo jste nejvíce conformable pomocí.

1. **Čemu jazyky se raději toouse pro analýzu?**  
   
   * R
   * Python
   * SQL
2. **Jaké nástroje můžete použít pro analýzu dat?**
   
   * [Microsoft Azure Powershell](/powershell/azure/overview) -skriptovací jazyk použít tooadminister vašich prostředků Azure v skriptovací jazyk.
   * [Azure Machine Learning Studio](machine-learning-what-is-ml-studio.md)
   * [Revolution Analytics](http://www.revolutionanalytics.com/revolution-r-open)
   * [Rstudia](http://www.rstudio.com)
   * [Python Tools for Visual Studio](http://microsoft.github.io/PTVS/)
   * [Anaconda](https://www.continuum.io/why-anaconda)
   * [Poznámkové bloky Jupyter](http://jupyter.org/)
   * [Microsoft Power BI](http://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Identifikovat váš scénář pokročilou analýzu
Jakmile jste odpověděli hello otázky v předchozí části hello, jste připravené toodetermine scénář, který nejlepší vyhovuje váš případ. Hello vzorové scénáře jsou uvedeny v [scénáře pro pokročilou analýzu v Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).

