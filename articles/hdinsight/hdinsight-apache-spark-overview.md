---
title: aaaIntroduction tooSpark v Azure HDInsight | Microsoft Docs
description: "Tento článek obsahuje úvod tooSpark na HDInsight a hello různé scénáře, ve kterých se může použít Spark cluster v HDInsight."
keywords: "Co je apache spark, spark cluster, úvod toospark spark v hdinsight"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 82334b9e-4629-4005-8147-19f875c8774e
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/12/2017
ms.author: nitinme
ms.openlocfilehash: 41996e733618b8534469fa239b980ac50161a535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toospark-on-hdinsight"></a>Úvod tooSpark v HDInsight

Tento článek vám poskytne úvod tooSpark v HDInsight. <a href="http://spark.apache.org/" target="_blank">Apache Spark</a> představuje rozhraní open-source paralelní zpracování, které podporuje v paměti zpracovává tooboost hello výkonu velkých objemů dat analytických aplikací. Cluster Spark v prostředí HDInsight je kompatibilní s úložištěm Azure Storage (WASB) a také se službou Azure Data Lake Store, takže svá stávající data uložená v Azure můžete v tomto clusteru snadno zpracovat.

Když vytvoříte cluster Spark v HDInsight, vytvoříte výpočetní prostředky Azure s nainstalovaným a nakonfigurovaným Sparkem. Trvá pouze asi deset minut clusteru toocreate Spark v HDInsight. Hello data toobe zpracování jsou uloženy v Azure Storage nebo Azure Data Lake Store. Další informace najdete v tématu [Použití Azure Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).

**clusteru toocreate Spark v HDInsight**, najdete v části [rychlý start: Vytvořte Spark cluster v HDInsight a spusťte interaktivní dotazu pomocí Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="what-is-apache-spark-on-azure-hdinsight"></a>Co je Apache Spark ve službě Azure HDInsight?
Clustery Spark v prostředí HDInsight nabízejí plně spravovanou službu Spark. Tady najdete výhody, které přináší vytvoření clusteru Spark v HDInsight.

| Funkce | Popis |
| --- | --- |
| Snadné vytváření clusterů Spark |Během pár minut pomocí hello portálu Azure, Azure PowerShell nebo hello SDK rozhraní .NET HDInsight můžete vytvořit nový cluster Spark v HDInsight. Viz [Začínáme s clusterem Spark v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Snadné používání |Součástí clusteru Spark v prostředí HDInsight jsou poznámkové bloky Jupyter a Zeppelin. Můžete je použít pro interaktivní zpracování dat a vizualizaci.|
| Rozhraní REST API |Clustery Spark v HDInsight zahrnují [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), založené na REST API Spark úlohy serveru tooremotely odeslání a monitorování úloh. |
| Podpora pro Azure Data Lake Store | Cluster Spark v HDInsight může být nakonfigurované toouse Azure Data Lake Store jako další úložiště a také jako primárního úložiště (pouze s clustery HDInsight 3.5). Další informace o Data Lake Store naleznete v tématu [Přehled o Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). |
| Integrace se službami Azure |Cluster Spark v HDInsight se dodává s tooAzure konektor služby Event Hubs. Zákazníci mohou vytvářet aplikace streamování pomocí hello Event Hubs, kromě příliš[Kafka](http://kafka.apache.org/), která je již k dispozici jako součást Spark. |
| Podpora pro R Server | Můžete nastavit serveru R na clusteru toorun distribuovaných R výpočtů s hello rychlostmi dohodnutými s clusterem Spark HDInsight Spark. Další informace naleznete v tématu [Začínáme používat R Server v HDInsight](hdinsight-hadoop-r-server-get-started.md). |
| Integrace v prostředí IDE třetích stran | HDInsight poskytuje modulů plug-in pro integrovaného vývojového prostředí jako IntelliJ IDEA a Eclipse, můžete použít toocreate a odeslání aplikací tooan clusteru HDInsight Spark. Další informace najdete v tématu [Použití sady Azure Toolkit pro IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin.md) a [Použití sady Azure Toolkit pro Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md).|
| Počet souběžných dotazů |Clustery Spark v HDInsight podporují souběžné dotazy. To umožňuje více dotazů od jednoho uživatele nebo více dotazů od různých uživatelů a aplikací tooshare hello stejných prostředků clusteru. |
| Ukládání do mezipaměti na SSD |Můžete zvolit toocache dat, buď v paměti nebo na SSD disky připojené toohello uzly clusteru. Ukládání do mezipaměti v paměti poskytuje nejlepší výkon dotazů hello ale může být nákladné; ukládání do mezipaměti na SSD poskytuje skvělou možnost pro zlepšení výkonu dotazů bez nutnosti toocreate hello cluster velikosti, která je požadovaná toofit hello celá datová sada v paměti. |
| Integrace s nástroji BI |Clustery Spark pro HDInsight nabízí konektory pro nástroje BI, například [Power BI](http://www.powerbi.com/) a [Tableau](http://www.tableau.com/products/desktop) pro analýzu dat. |
| Předem zavedené knihovny Anaconda |Clustery Spark na HDInsight přichází s předinstalovanými knihovnami Anaconda. [Anaconda](http://docs.continuum.io/anaconda/) poskytuje zavřít too200 knihoven pro machine learning, analýzy dat, vizualizace atd. |
| Škálovatelnost |I když můžete zadat číslo hello uzlů v clusteru během vytváření, můžete chtít toogrow nebo zmenšit zatížení toomatch hello clusteru. Všechny clustery HDInsight umožňují toochange hello počet uzlů v clusteru hello. Navíc clustery Spark můžete vyřadit bez ztráty dat, vzhledem k tomu, že všechny hello data jsou uložena v Azure Storage nebo Data Lake Store. |
| Nepřetržitá podpora |Clustery Spark v HDInsight přináší nepřetržitou podporu napříč celou podnikovou sítí a SLA, která zajišťuje 99,9% dostupnost. |

## <a name="what-are-hello-use-cases-for-spark-on-hdinsight"></a>Jaké jsou hello případy použití pro Spark v HDInsight?
Clustery Spark v HDInsight povolit hello následující klíčové scénáře.

### <a name="interactive-data-analysis-and-bi"></a>Interaktivní analýzu dat a BI
[Podívejte se na kurz](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark v HDInsight ukládá data do úložiště Azure Storage nebo služby Azure Data Lake Store. Obchodní specialisté a osoby provádějící klíčová rozhodnutí můžete analyzovat a vytváření sestav těchto dat a použít Microsoft Power BI toobuild interaktivních sestav z hello analyzovat data. Analytici můžete spustit z nestrukturovaných/částečně strukturovaných dat v úložišti clusteru, definovat schéma pro data hello využitím poznámkových bloků a následně vytvořit modely dat pomocí Microsoft Power BI. Clustery Spark v HDInsight podporují také různé nástroje třetích stran pro BI, například Tableau. Představují tak ideální platformu pro analytiky dat, obchodní specialisty a osoby provádějící klíčová rozhodnutí.

### <a name="spark-machine-learning"></a>Spark Machine Learning
[Podívejte se na kurz: předpovídání teplot sestavení pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Podívejte se na kurz: předpovídání výsledků kontroly potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Systém Apache Spark je vybavený knihovnou [MLlib](http://spark.apache.org/mllib/) pro strojové učení, jejímž základem je Spark a kterou můžete používat z clusteru Spark v HDInsight. Cluster Spark v HDInsight obsahuje Anacondu, distribuci jazyka Python s různými balíčky pro strojové učení. Spojte tyto možnosti s vestavěnou podporou pro poznámkové bloky Jupyter a Zeppelin a máte nejmodernější prostředí pro tvorbu machine learning aplikací.

### <a name="spark-streaming-and-real-time-data-analysis"></a>Vysílání datových proudů a analýza dat v reálném čase ve Sparku
[Podívejte se na kurz](hdinsight-apache-spark-eventhub-streaming.md)

Clustery Spark v HDInsight nabízí bohatou podporu pro vytváření řešení pro analýzu v reálném čase. Zatímco Spark již obsahuje konektory tooingest dat z mnoha zdrojů, například soketů Kafka, Flume, Twitter, ZeroMQ nebo TCP, Spark v HDInsight přidává prvotřídní podporu pro příjem dat z Azure Event Hubs. Centra událostí jsou hello nejčastěji používané službou řazení front v Azure. Díky připravené podpoře pro službu Event Hubs představují clustery Spark v HDInsight ideální platformu pro vytvoření kanálu k analýze dat v reálném čase.

## <a name="next-steps"></a>Jaké součásti jsou zahrnuté v clusteru Spark?
Clustery Spark v HDInsight zahrnují hello následující součásti, které jsou k dispozici v clusterech hello ve výchozím nastavení.

* [Spark Core](https://spark.apache.org/docs/1.5.1/). Obsahuje Spark Core, Spark SQL, rozhraní API pro vysílání datového proudu Spark, GraphX a MLlib.
* [Anaconda](http://docs.continuum.io/anaconda/)
* [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
* [Poznámkový blok Jupyter](https://jupyter.org)
* [Poznámkový blok Zeppelin](http://zeppelin-project.org/)

Clustery Spark v HDInsight také poskytují [ovladač ODBC](http://go.microsoft.com/fwlink/?LinkId=616229) pro připojení k tooSpark clusterů v HDInsight z nástrojů BI, například Microsoft Power BI a Tableau.

## <a name="where-do-i-start"></a>Kde mám začít?
Začněte tak, že vytvoříte cluster Spark v HDInsight. Přečtěte si téma [Rychlý začátek: Vytvoření clusteru Spark v HDInsight v Linuxu a spuštění interaktivního dotazu pomocí Jupyteru](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Další kroky
### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
* [Známé problémy systému Apache Spark v Azure HDInsight](hdinsight-apache-spark-known-issues.md)
