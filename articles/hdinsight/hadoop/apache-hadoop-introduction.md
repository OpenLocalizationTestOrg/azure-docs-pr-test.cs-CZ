---
title: "Co jsou HDInsight a technologie Hadoop a Spark? – Azure | Dokumentace Microsoftu"
description: "Úvod do služby HDInsight a technologií Hadoop a Spark a jejich komponent, včetně systémů Kafka, Hive, Storm a HBase pro analýzy velkých objemů dat."
keywords: "azure hadoop, hadoop azure, hadoop úvod, úvod hadoop, technologie hadoop, úvod do technologie hadoop, úvod k technologii hadoop, co je cluster hadoop, co je hadoop cluster, k čemu slouží hadoop"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/13/2017
ms.author: cgronlun
ms.openlocfilehash: c53c4eba6d46c03bbfc6bb316ae4e505abb7b781
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/13/2018
---
# <a name="introduction-to-azure-hdinsight-and-the-hadoop-and-spark-technology-stack"></a>Úvod do služby Azure HDInsight a technologií Hadoop a Spark
Tento článek obsahuje úvod k Azure HDInsight. Azure HDInsight je plně spravovaná opensourcová analytická služba určená pro podniky. Můžete používat open source architektury, jako jsou Hadoop, Spark, Hive, LLAP, Kafka, Storm, R a další. 

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

[Apache Hadoop](http://hadoop.apache.org/) byl původní open source architekturou pro distribuované zpracování a analýzy velkých datových sad v clusterech. Součástí technologie Hadoop je potřebný software a nástroje, jako je Apache Hive, HBase, Spark, Kafka a řada dalších. 

Komponenty technologie Hadoop dostupné ve službě HDInsight najdete v tématu [Dostupné komponenty a verze ve službě HDInsight][component-versioning]. Další informace o platformě Hadoop v prostředí HDInsight najdete v tématu [Funkce Azure pro HDInsight](https://azure.microsoft.com/services/hdinsight/).

[Apache Spark](http://spark.apache.org) je opensourcová architektura pro paralelní zpracování, která podporuje zpracování v paměti pro zvýšení výkonu analytických aplikací s velkým objemem dat. Další informace o Sparku ve službě HDInsight najdete v tématu [Představujeme Spark v Azure HDInsight](../spark/apache-spark-overview.md). 

<a href="https://ms.portal.azure.com/#create/Microsoft.HDInsightCluster" target="_blank"><img src="./media/apache-hadoop-introduction/deploy-to-azure.png" alt="Deploy an Azure HDInsight cluster"></a>

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>Co je HDInsight a technologie Hadoop? 
Azure HDInsight je cloudová distribuce komponent Hadoop z platformy [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/). Azure HDInsight umožňuje snadné, rychlé a nákladově efektivní zpracování obrovského množství dat. Můžete využít nejoblíbenější opensourcové architektury, jako jsou Hadoop, Spark, Hive, LLAP, Kafka, Storm, R a další. Tyto architektury podporují širokou škálu scénářů, jako jsou ETL (extrakce, transformace a načítání), datové sklady, strojové učení a IoT.

## <a name="what-is-big-data"></a>Co jsou velké objemy dat?

Velké objemy dat se shromažďují v narůstajícím množství, s vyšší rychlostí a stále větší pestrostí formátů. Mohou to být historická data (to znamená uložená) nebo data v reálném čase (to znamená streamovaná ze zdroje). Informace o nejběžnějších případech použití velkých objemů dat najdete v části [Scénáře použití služby HDInsight](#scenarios-for-using-hdinsight).

## <a name="why-should-i-use-hdinsight"></a>Proč bych měl/a používat službu HDInsight?

Tato část uvádí schopnosti služby Azure HDInsight.


|Schopnost  |Popis  |
|---------|---------|
|Nativní pro cloud     |     Azure HDInsight umožňuje vytvářet optimalizované clustery pro [Hadoop](apache-hadoop-linux-tutorial-get-started.md),  [Spark](../spark/apache-spark-jupyter-spark-sql.md),  [Interaktivní dotazy (LLAP)](../interactive-query/apache-interactive-query-get-started.md),  [Kafka](../kafka/apache-kafka-get-started.md),  [Storm](../storm/apache-storm-tutorial-get-started-linux.md),  [HBase](../hbase/apache-hbase-tutorial-get-started-linux.md) a  [R Server](../r-server/r-server-get-started.md) v Azure. HDInsight poskytuje také komplexní smlouvu SLA pro všechny vaše produkční úlohy.  |
|Škálovatelnost a nízké náklady     | HDInsight umožňuje [vertikálně navyšovat nebo snižovat kapacitu](../hdinsight-administer-use-portal-linux.md)  úloh.  [Vytváření clusterů na vyžádání](../hdinsight-hadoop-create-linux-clusters-adf.md)  vám umožní snížit náklady a platit jenom za to, co používáte. Můžete také sestavovat datové kanály pro zprovoznění úloh. Oddělený výpočetní výkon a úložiště poskytují lepší výkon a flexibilitu. |
|Bezpečnost a dodržování předpisů    | HDInsight umožňuje chránit datové prostředky vašeho podniku pomocí služby [Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md), [šifrování](../hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md) a integrace se službou [Azure Active Directory](../domain-joined/apache-domain-joined-introduction.md). HDInsight také splňuje nejoblíbenější oborové a vládní [standardy dodržování předpisů](https://azure.microsoft.com/overview/trusted-cloud).        |
|Monitorování    | Azure HDInsight se integruje s [Azure Log Analytics](../hdinsight-hadoop-oms-log-analytics-tutorial.md) a poskytuje tak jednotné rozhraní pro monitorování všech vašich clusterů.        |
|Globální dostupnost | Služba HDInsight je dostupná ve více  [oblastech](https://azure.microsoft.com/regions/services/)  než jakákoli jiná nabídka analýz velkých objemů dat. Služba Azure HDInsight je dostupná také pro Azure Government, Čínu a Německo a umožňuje tak splnit požadavky vašeho podniku v klíčových suverénních oblastech. |  
|Produktivita     |  Azure HDInsight umožňuje používat bohaté nástroje zvyšující produktivitu pro Hadoop a Spark s oblíbeným vývojovým prostředím. Mezi tato vývojová prostředí patří [Visual Studio](apache-hadoop-visual-studio-tools-get-started.md), [Eclipse](../spark/apache-spark-eclipse-tool-plugin.md) a [IntelliJ](../spark/apache-spark-intellij-tool-plugin.md) pro zajištění podpory jazyků Scala, Python, R, Java a .NET. Odborníci přes data můžou také spolupracovat s využitím oblíbených poznámkových bloků, jako jsou [Jupyter](../spark/apache-spark-jupyter-notebook-kernels.md) a [Zeppelin](../spark/apache-spark-zeppelin-notebook.md).    |
|Rozšiřitelnost     |  Clustery HDInsight můžete rozšířit instalací komponent (Hue, Presto atd.) pomocí [akcí skriptů](../hdinsight-hadoop-customize-cluster-linux.md), [přidáním hraničních uzlů](../hdinsight-apps-use-edge-node.md) nebo [integrací s dalšími aplikacemi certifikovanými pro práci s velkými objemy dat](../hdinsight-apps-install-applications.md). HDInsight umožňuje bezproblémovou integraci s nejoblíbenějšími řešeními pro velké objemy dat prostřednictvím nasazení [jedním kliknutím](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/).|

## <a name="scenarios-for-using-hdinsight"></a>Scénáře použití služby HDInsight

Azure HDInsight je možné použít v nejrůznějších scénářích velkých objemů dat. Může se jednat o historická data (data, která jsou už shromážděná a uložená) nebo o data v reálném čase (data proudící přímo ze zdroje). Scénáře zpracování těchto dat můžeme shrnout do následujících kategorií: 

### <a name="batch-processing-etl"></a>Dávkové zpracování (ETL)

Extrakce, transformace a načítání (ETL) je proces, při kterém se nestrukturovaná nebo strukturovaná data extrahují z heterogenních zdrojů dat. Potom se transformují do strukturovaného formátu a načítají do úložiště dat. Transformovaná data je možné použít pro datové vědy nebo datové sklady.

### <a name="internet-of-things-iot"></a>Internet věcí (IoT)

Pomocí služby HDInsight můžete zpracovávat streamovaná data přijímaná v reálném čase z různých zařízení. Pokud chcete získat další informace, [přečtěte si tento blogový příspěvek z Azure, který oznamuje verzi Public Preview pro Apache Kafka v HDInsightu se službou Azure Managed Disks](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/).

![Architektura HDInsight: Internet věcí](./media/apache-hadoop-introduction/hdinsight-architecture-iot.png) 

### <a name="data-science"></a>Datové vědy

Pomocí služby HDInsight můžete sestavovat aplikace, které z dat extrahují důležité informace. Navíc můžete pomocí služby Azure Machine Learning předvídat budoucí trendy pro vaši firmu. Další informace najdete v [tomto příběhu zákazníka](https://customers.microsoft.com/story/pros).

![Architektura HDInsight: Datové vědy](./media/apache-hadoop-introduction/hdinsight-architecture-data-science.png)

### <a name="data-warehousing"></a>Datové sklady

Pomocí služby HDInsight můžete provádět interaktivní dotazy v petabajtovém měřítku nad strukturovanými i nestrukturovanými daty v jakémkoli měřítku. Můžete také sestavovat modely, které je propojí s nástroji BI. Další informace najdete v [tomto příběhu zákazníka](https://customers.microsoft.com/story/milliman). 

![Architektura HDInsight: Datové sklady](./media/apache-hadoop-introduction/hdinsight-architecture-data-warehouse.png)

### <a name="hybrid"></a>Hybridní

Pomocí služby HDInsight můžete rozšířit svou stávající místní infrastrukturu velkých dat do Azure a využívat možnosti pokročilých analýz v cloudu.

![Architektura HDInsight: Hybridní](./media/apache-hadoop-introduction/hdinsight-architecture-hybrid.png)

## <a name="overview"></a>Typy clusterů ve službě HDInsight
HDInsight zahrnuje specifické typy clusterů a možnosti přizpůsobení clusterů, jako je například možnost přidávání komponent, nástrojů a jazyků.

### <a name="spark-kafka-interactive-query-hbase-customized-and-other-cluster-types"></a>Spark, Kafka, Interactive Query, HBase, přizpůsobené clustery a další typy clusterů
HDInsight nabízí následující typy clusteru:

* **[Apache Hadoop:](https://wiki.apache.org/hadoop)** Architektura, která používá [HDFS](#hdfs), správu prostředků [YARN](#yarn) a jednoduchý programovací model [MapReduce](#mapreduce) pro paralelní zpracování a analýzu dat dávek.

* **[Apache Spark:](http://spark.apache.org/)** Architektura pro paralelní zpracování, která podporuje zpracování v paměti pro zvýšení výkonu analytických aplikací s velkým objemem dat. Spark se dá využít pro SQL, streamování dat a strojové učení. Přečtěte si téma [Co je Apache Spark v prostředí HDInsight?](../spark/apache-spark-overview.md)

* **[Apache HBase:](http://hbase.apache.org/)** Databáze NoSQL postavená na systému Hadoop, která umožňuje náhodný přístup a zajišťuje velkou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat (potenciálně až miliardy řádků krát miliony sloupců). Přečtěte si téma [Co je HBase v HDInsight?](../hbase/apache-hbase-overview.md)

* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: Server pro hostování a správu paralelních, distribuovaných procesů R. Poskytuje datovým vědcům, statistikům a programátorům v R přístup ke škálovatelným, distribuovaným analytickým metodám služby HDInsight na vyžádání. Viz [Přehled R Serveru ve službě HDInsight](../r-server/r-server-overview.md).

* **[Apache Storm](https://storm.incubator.apache.org/)**: distribuovaný výpočetní systém v reálném čase pro rychlé zpracování velkých streamů dat. Storm je poskytován jako spravovaný cluster v prostředí HDInsight. Viz [Analýza dat snímačů v reálném čase pomocí nástrojů Storm a Hadoop](../storm/apache-storm-sensor-data-analysis.md).

* **[Apache Interactive Query ve verzi Preview (také označované LLAP (Live Long and Process)):](https://cwiki.apache.org/confluence/display/Hive/LLAP)** Ukládání do mezipaměti pro interaktivnější a rychlejší dotazy Hive. Viz [Použití Interactive Query ve službě HDInsight](../interactive-query/apache-interactive-query-get-started.md).

* **[Apache Kafka:](https://kafka.apache.org/)** Opensourcová platforma sloužící k vytváření aplikací a kanálů pro streamovaná data. Kafka také poskytuje funkce propojující fronty zpráv, pomocí kterých můžete publikovat datové streamy a přihlašovat se k jejich odběru. Viz [Úvod k Apache Kafka ve službě HDInsight](../kafka/apache-kafka-introduction.md).

## <a name="open-source-components-in-hdinsight"></a>Opensourcové komponenty ve službě HDInsight

Azure HDInsight umožňuje vytvářet clustery pomocí opensourcových architektur, jako jsou Hadoop, Spark, Hive, LLAP, Kafka, Storm, HBase a R. Tyto clustery ve výchozím nastavení zahrnují další opensourcové komponenty, jako jsou například [Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md), [Avro](http://avro.apache.org/docs/current/spec.html), [Hive](http://hive.apache.org), [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog/), [Mahout](https://mahout.apache.org/), [MapReduce](http://wiki.apache.org/hadoop/MapReduce), [YARN](http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Phoenix](http://phoenix.apache.org/), [Pig](http://pig.apache.org/), [Sqoop](http://sqoop.apache.org/), [Tez](http://tez.apache.org/), [Oozie](http://oozie.apache.org/) a [ZooKeeper](http://zookeeper.apache.org/).  


## <a name="programming-languages-in-hdinsight"></a>Programovací jazyky v prostředí HDInsight
Clustery HDInsight, včetně clusterů Spark, HBase, Kafka, Hadoop a dalších, podporují celou řadu programovacích jazyků. Některé z nich ale nejsou ve výchozím nastavení nainstalované. Pro knihovny, moduly nebo balíčky, které nejsou ve výchozím nastavení nainstalované, [použijte akci skriptu pro instalaci součásti](../hdinsight-hadoop-script-actions-linux.md). 

### <a name="default-programming-language-support"></a>Výchozí podpora programovacích jazyků
Ve výchozím nastavení podporují clustery prostředí HDInsight tyto jazyky:

* Java
* Python

Další jazyky je možné nainstalovat pomocí [příkazů skriptů](../hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Jazyky Java virtual machine (JVM)
V prostředí Java Virtual Machine (JVM) je možné spouštět celou řadu jiných jazyků, než je Java. Pokud ale některé z těchto jazyků spustíte, může být v clusteru potřeba nainstalovat další součásti.

Clustery prostředí HDInsight podporují následující jazyky založené na JVM:

* Clojure
* Jython (Python pro jazyk Java)
* Scala

### <a name="hadoop-specific-languages"></a>Jazyky pro Hadoop
Clustery HDInsight podporují následující jazyky, které jsou určené konkrétně pro technologii Hadoop:

* Pig Latin pro úlohy Pig
* HiveQL pro úlohy Hive a SparkSQL

## <a name="business-intelligence-on-hdinsight"></a>Business intelligence ve službě HDInsight
Známé nástroje business intelligence (BI) načítají, analyzují a vykazují data integrovaná v prostředí HDInsight buď pomocí doplňku Power Query, nebo ovladače Microsoft Hive ODBC Driver:

* [Apache Spark BI s využitím nástrojů pro vizualizaci dat ve službě Azure HDInsight](../spark/apache-spark-use-bi-tools.md)

* [Vizualizace dat Hive pomocí Microsoft Power BI ve službě Azure HDInsight](apache-hadoop-connect-hive-power-bi.md) 

* [Vizualizace dat Interactive Query Hive pomocí Power BI ve službě Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)

* [Připojení aplikace Excel k systému Hadoop pomocí Power Query:](apache-hadoop-connect-excel-power-query.md) Zjistěte, jak připojit Excel k účtu služby Azure Storage, kde se ukládají data z vašeho clusteru HDInsight, pomocí doplňku Microsoft Power Query pro Excel. Vyžadují se pracovní stanice Windows. 

* [Připojení aplikace Excel k systému Hadoop pomocí ovladače ODBC Microsoft Hive](apache-hadoop-connect-excel-hive-odbc-driver.md): Zjistěte, jak importovat data z prostředí HDInsight pomocí ovladače ODBC Microsoft Hive. Vyžadují se pracovní stanice Windows. 

* [Cloudová platforma Microsoft](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Získejte informace o službě Power BI pro Office 365, stáhněte si zkušební verzi serveru SQL Server a nastavte si SharePoint Server 2013 a SQL Server BI.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)

* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Další kroky

* [Začínáme s Hadoopem ve službě HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Začínáme se Sparkem ve službě HDInsight](../spark/apache-spark-jupyter-spark-sql.md)
* [Začínáme s Kafka ve službě HDInsight](../kafka/apache-kafka-get-started.md)
* [Začínáme se Stormem ve službě HDInsight](../storm/apache-storm-tutorial-get-started-linux.md)
* [Začínáme s HBase ve službě HDInsight](../hbase/apache-hbase-tutorial-get-started-linux.md)
* [Začínáme s interaktivními dotazy (LLAP) ve službě HDInsight](../interactive-query/apache-interactive-query-get-started.md)
* [Začínáme s R Serverem ve službě HDInsight](../r-server/r-server-get-started.md)
* [Správa clusterů HDInsight](../hdinsight-administer-use-portal-linux.md)
* [Zabezpečení clusterů HDInsight](../domain-joined/apache-domain-joined-introduction.md)
* [Monitorování clusterů HDInsight](../hdinsight-hadoop-oms-log-analytics-tutorial.md)


[component-versioning]: ../hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
