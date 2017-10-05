---
title: "Scénáře identifikovat a naplánovat váš proces analytics - Azure | Microsoft Docs"
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
ms.openlocfilehash: bfd6b9f104e1184bca46490c0fdcab43dfcd1447
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Jak určit scénáře a plánovat zpracování dat pokročilé analýzy
Jaké prostředky měli naplánovat při nastavení prostředí pro provádět pokročilé analýzy na datovou sadu? Tento článek navrhuje řadu otázek a požádejte, které vám pomůžou identifikovat úlohy a prostředky, které jsou relevantní váš scénář. Pořadí kroků pro prediktivní analýzy popsané v [co je proces pro vědecké účely Data Team (TDSP)?](data-science-process-overview.md). Každá z těchto kroků bude vyžadovat specifické prostředky pro úlohy, které jsou relevantní pro konkrétní scénář. Klíčové otázky k identifikaci váš scénář se týkají dat logistiky charakteristiky, kvalitu nástroje a jazyky, které chcete provést analýzu a datové sady.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistic dotazy: dat – umístění a přesun
Logistic dotazy se týkají umístění **zdroj dat**, **cílového místa** ve službě Azure a požadavky pro přesun dat, včetně plánu, velikost a prostředky zahrnuta. Data možná potřebujete přesunout několikrát během analýzy. Obvyklým scénářem je přesunout místní data do určitou formu úložiště v Azure a pak do nástroje Machine Learning Studio.

1. **Co je zdroj dat?** Je místní nebo v cloudu? Například:
   
   * Data jsou veřejně dostupné v adresa protokolu HTTP.
   * Data se nachází v místní nebo síťové umístění souboru.
   * Data jsou v databázi systému SQL Server.
   * Data jsou uložena v kontejneru úložiště Azure
2. **Co je Azure cílové?** Kde nemusí být pro zpracování nebo modelování? Například:
   
   * Azure Blob Storage
   * Databáze SQL Azure
   * SQL Server na virtuálním počítači Azure
   * HDInsight (Hadoop v Azure) nebo tabulek Hive
   * Azure Machine Learning
   * Připojit Azure virtuální pevné disky.
3. **Jak budete přesouvat data?** Postupy a prostředky, které jsou k dispozici ingestování nebo načtení dat do různých jiného úložiště a zpracování prostředí jsou uvedeny v následujících tématech.
   
   * [Načtení dat do úložiště prostředí pro analýzu](machine-learning-data-science-ingest-data.md)
   * [Importu trénovacích dat do Azure Machine Learning Studio z různých zdrojů dat](machine-learning-data-science-import-data.md).
4. **Potřebuje přesunout v pravidelných intervalech nebo změnit během migrace dat?** Zvážit použití Azure Data Factory (ADF), když data je třeba migrovat průběžně, zvlášť pokud hybridní scénář, který přistupuje k místní a cloudové prostředky je zahrnuta nebo když dat je zpracován, nebo musí být upraven nebo mít obchodní logiky přidána průběhu migrované. Další informace najdete v tématu [přesun dat z místního serveru SQL do SQL Azure s Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md)
5. **Kolik dat je přesunuta do Azure?** Datové sady, které jsou velké může překročit kapacitu úložiště určité prostředí. Příklad najdete v tématu diskuzi o omezení velikosti pro Machine Learning Studio v další části. V takových případech ukázkových dat lze během analýzy. Podrobnosti o tom, jak nižší sample datovou sadu v různých prostředích Azure najdete v tématu [vzorová data v procesu Team datové vědy](machine-learning-data-science-sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Otázky ohledně dat charakteristiky: typ, formát a velikosti
Tyto otázky jsou klíčem k plánování vašeho úložiště a zpracování prostředí, jež jsou vhodné pro různé typy dat a každý z nich mají určitá omezení.

1. **Jaké jsou typy dat?** Například:
   
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

Třeba proveďte v prostředí Azure Machine Learning Studio:

* Seznam formáty dat a typy podporované systémem Azure Machine Learning Studio najdete v tématu [formáty dat a datové typy podporované](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) části.
* Informace o omezení dat pro Azure Machine Learning Studio najdete v tématu **jak velká může být datová sada pro mé moduly?** části [import a export dat pro Machine Learning](machine-learning-faq.md#machine-learning-studio-questions)

Informace o omezeních jinými službami Azure používá v procesu analytics najdete v tématu [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Otázky ohledně kvality dat: zkoumání a předběžného zpracování
1. **Co víte o svá data?** Prozkoumejte data, když potřebujete získat pochopení jeho základními charakteristikami. Co vzory nebo trendů ho předměty, co je odlehlé hodnoty, případně chybí počet hodnot. Tento krok je důležitý pro stanovení rozsahu předběžné zpracování pro formulování hypotézy, které by mohla naznačovat nejvhodnější funkce nebo zadejte analýzy a formulování plány pro shromažďování dalších dat potřeby. Výpočet popisný statistiky a vykreslení vizualizace jsou užitečné techniky pro data kontroly. Podrobnosti o tom, jak prozkoumat datovou sadu v různých prostředích Azure najdete v tématu [zkoumat data v procesu Team datové vědy](machine-learning-data-science-explore-data.md).
2. **Vyžaduje data předem zpracování nebo čištění?**
   Předběžné zpracování a vyčištění dat jsou důležité úkoly, které obvykle musí být provedeny před datové sady je možné efektivně pro machine learning. Nezpracovaná data, je často aktivní nebo nespolehlivé a může být chybějící hodnoty. Pomocí těchto údajů pro modelování může vytvářet zavádějící výsledky. Popis najdete v tématu [úkoly přípravy dat pro vylepšený strojového učení](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Otázky týkající se nástroje a jazyky
Existuje mnoho možností v závislosti na tom, jaké jazyky a vývojových prostředích nebo nástroje potřebujete nebo jste nejvíce conformable pomocí.

1. **Jaké jazyky chcete použít pro analýzu?**  
   
   * R
   * Python
   * SQL
2. **Jaké nástroje můžete použít pro analýzu dat?**
   
   * [Microsoft Azure Powershell](/powershell/azure/overview) -skriptovací jazyk používají ke správě prostředků Azure v skriptovací jazyk.
   * [Azure Machine Learning Studio](machine-learning-what-is-ml-studio.md)
   * [Revolution Analytics](http://www.revolutionanalytics.com/revolution-r-open)
   * [Rstudia](http://www.rstudio.com)
   * [Python Tools pro Visual Studio](http://microsoft.github.io/PTVS/)
   * [Anaconda](https://www.continuum.io/why-anaconda)
   * [Poznámkové bloky Jupyter](http://jupyter.org/)
   * [Microsoft Power BI](http://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Identifikovat váš scénář pokročilou analýzu
Jakmile jste odpověděli na otázky v předchozí části, jste připraveni k určení scénář, který nejlepší vyhovuje váš případ. Vzorové scénáře jsou uvedeny v [scénáře pro pokročilou analýzu v Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).

