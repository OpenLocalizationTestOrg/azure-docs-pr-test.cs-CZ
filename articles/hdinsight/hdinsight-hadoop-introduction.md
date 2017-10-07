---
title: "aaaWhat jsou HDInsight, zásobníku technologie hello Hadoop a clusterů? – Azure | Dokumentace Microsoftu"
description: "Zásobníku Úvod tooHDInsight a hello Hadoop technologie a součástí, včetně Spark, Kafka, Hive, HBase pro analýzu velkých objemů dat."
keywords: "Azure hadoop, hadoop azure, úvod hadoop, úvod hadoop, technologie zásobníku hadoop, toohadoop úvod, úvod toohadoop, co je cluster hadoop, co je hadoop cluster, co je hadoop používá pro"
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
ms.date: 07/20/2017
ms.author: cgronlun
ms.openlocfilehash: 0aed3d1a6cf3c0cdc3b036cfa4865a2e0b58e2c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-hdinsight-hello-hadoop-technology-stack-and-hadoop-clusters"></a>Úvod tooAzure HDInsight, zásobníku technologie hello Hadoop a clustery systému Hadoop
 Tento článek obsahuje úvod tooAzure HDInsight, cloudové distribuční hello Hadoop technologie zásobníku. Také se v něm dozvíte, co je cluster Hadoop a kdy ho můžete použít. 

## <a name="what-is-hdinsight-and-hello-hadoop-technology-stack"></a>Co je HDInsight a hello zásobníku technologie Hadoop? 
Azure HDInsight je distribuce cloudu hello Hadoop součásti ze hello [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/). [Apache Hadoop](http://hadoop.apache.org/) byla hello původní open-source rozhraní pro distribuované zpracování a analýzu velkých datových sad v clusterech počítačů. 


zásobníku technologie Hadoop Hello zahrnuje související softwaru a nástrojů, včetně Apache Hive HBase, Spark, Kafka a mnohé další. toosee dostupné technologie zásobníku komponent systému Hadoop v HDInsight, najdete v části [součásti a verze, které jsou k dispozici s HDInsight][component-versioning]. tooread Další informace o Hadoop v HDInsight, najdete v části hello [stránky funkce Azure pro HDInsight](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-a-hadoop-cluster-and-when-do-you-use-it"></a>Co je cluster Hadoop a kdy se používá?
*Hadoop* je také typ clusteru, který obsahuje:

* YARN pro plánování úloh a správu prostředků
* MapReduce pro paralelní zpracování
* systém souborů Hadoop distributed Hello (HDFS)
  
Clustery Hadoop se nejčastěji používají pro dávkové zpracování uložených dat. Jiné druhy clusterů ve službě HDInsight mají další funkce: Spark je stále oblíbenější díky rychlejšímu zpracování v paměti. Podrobnosti najdete v tématu [Typy clusterů ve službě HDInsight](#overview).

## <a name="what-is-big-data"></a>Co jsou velké objemy dat?
Velké objemy dat označují všechny rozsáhlé soubory s digitálními informacemi, jako je například:

* Data senzorů v průmyslových zařízeních
* Údaje o aktivitě zákazníka na webové stránce
* Informační kanál sítě Twitter

Velké objemy dat se shromažďují v narůstajícím množství, vyšší rychlostí a ve větší pestrosti formátů. Může být historických (tj. uložené) nebo v reálném čase (tj. streamovaných ze zdroje hello). 

## <a name="overview"></a>Typy clusterů ve službě HDInsight
HDInsight zahrnuje specifické typy clusterů a možnosti přizpůsobení clusterů, jako je například přidávání komponent, nástrojů a jazyků.

### <a name="spark-kafka-interactive-hive-hbase-customized-and-other-cluster-types"></a>Spark, Kafka, interaktivní Hive, HBase, přizpůsobené clustery a další typy clusterů
HDInsight nabízí hello následující typy clusteru:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: používá [HDFS](#hdfs), [YARN](#yarn) správy prostředků a jednoduchý [MapReduce](#mapreduce) programovací model tooprocess a Analýza dat dávky paralelně.
* **[Apache Spark](http://spark.apache.org/)**: rozhraní paralelní zpracování, které podporuje zpracování v paměti tooboost hello výkon aplikací pro analýzu velkých objemů dat, Spark pro SQL, streamování dat a strojové učení. Přečtěte si téma [Co je Apache Spark v prostředí HDInsight?](hdinsight-apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: Databáze NoSQL postavená na systému Hadoop, která umožňuje náhodný přístup a zajišťuje velkou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat – potenciálně až miliardy řádků krát miliony sloupců. Přečtěte si téma [Co je HBase v HDInsight?](hdinsight-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: Server pro hostování a správu paralelních, distribuovaných procesů R. Poskytne datových vědců statistikami a programátory v jazyce R tooscalable přístup na vyžádání, distribuované metody analýz v HDInsight. Viz [Přehled R Serveru ve službě HDInsight](hdinsight-hadoop-r-server-overview.md).
* **[Apache Storm](https://storm.incubator.apache.org/)**: distribuovaný výpočetní systém v reálném čase pro rychlé zpracování velkých streamů dat. Storm je poskytován jako spravovaný cluster v prostředí HDInsight. Viz [Analýza dat snímačů v reálném čase pomocí nástrojů Storm a Hadoop](hdinsight-storm-sensor-data-analysis.md).
* **[Apache Interactive Hive ve verzi Preview (také označované LLAP (Live Long and Process))](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: Ukládání do mezipaměti pro interaktivní a rychlejší dotazy Hivu. Viz [Použití Interactive Hivu ve službě HDInsight](hdinsight-hadoop-use-interactive-hive.md).
* **[Apache Kafka](https://kafka.apache.org/)**: Open source platforma sloužící k vytváření aplikací a kanálů pro streamovaná data. Kafka také poskytuje funkce fronta zpráv, které vám umožní toopublish a přihlášení k odběru toodata datových proudů. V tématu [Úvod tooApache Kafka v HDInsight](hdinsight-apache-kafka-introduction.md).

Můžete taky nakonfigurovat pomocí hello následující metody:
* **[Připojené k doméně clustery preview](hdinsight-domain-joined-introduction.md)**: cluster připojený k doméně služby Active Directory tooan tak, aby mohl řízení přístupu a poskytovat zásad správného řízení pro data.
* **[Vlastní clustery s akcemi skriptů](hdinsight-hadoop-customize-cluster-linux.md)**: Clustery se skripty, které se spouštějí během zřizování a instalují další komponenty.

### <a name="example-cluster-customization-scripts"></a>Příklady skriptů pro přizpůsobení clusterů
Akce skriptů jsou skripty Bash v systému Linux, na kterých běží při zřizování clusteru a který lze použít tooinstall další součásti v clusteru hello. 

Hello následující příklady skriptů poskytovaných týmem HDInsight hello:

* **[HUE](hdinsight-hadoop-hue-linux.md)**: Sada webových aplikací používaných toointeract s clusterem. Pouze clustery v Linuxu.
* **[Giraph](hdinsight-hadoop-giraph-install-linux.md)**: grafu zpracování toomodel vztahů mezi věcmi nebo osobami.
* **[Solr](hdinsight-hadoop-solr-install-linux.md)**: Vyhledávací podniková platforma, která umožňuje fulltextové vyhledávání dat.

Informace o vývoji vlastních akcí skriptů naleznete v tématu [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).

## <a name="components-and-utilities-on-hdinsight-clusters"></a>Komponenty a nástroje v clusterech HDInsight
Hello následující součásti a nástroje jsou zahrnuty v clusterech HDInsight:

* **[Ambari](#ambari)**: zřizování, správa, monitorování a nástroje clusterů.
* **[Avro](#avro)**  (knihovna Microsoft .NET pro Avro): serializaci dat pro prostředí Microsoft .NET hello. 
* **[Hive &amp; HCatalog](#hive)**: Dotazování typu strukturovaného dotazovacího jazyka (SQL) a vrstva správy tabulek a ukládání.
* **[Mahout](#mahout)**: pro škálovatelné aplikace strojového učení.
* **[MapReduce](#mapreduce)**: starší verze rozhraní pro distribuované zpracování a správu prostředků systému Hadoop. Viz [YARN](#yarn).
* **[Oozie](#oozie)**: řízení pracovních postupů.
* **[Phoenix](#phoenix)**: vrstva relační databáze nad HBase.
* **[Pig](#pig)**: jednodušší skriptování pro transformace prostředí MapReduce.
* **[Sqoop](#sqoop)**: import a export dat.
* **[Tez](#tez)**: efektivně umožňuje toorun datově náročných procesů ve velkém měřítku.
* **[YARN](#yarn)**: Správa prostředků, který je součástí základní knihovny hello Hadoop.
* **[ZooKeeper](#zookeeper)**: koordinace procesů v distribuovaných systémech.

> [!NOTE]
> Informace o konkrétní součásti hello a informace o verzi najdete v tématu [Hadoop součásti a verzí v HDInsight][component-versioning]
>
>

### <a name="ambari"></a>Ambari
Apache Ambari slouží k zřizování, správě a monitoringu clusterů systému Apache Hadoop. Obsahuje intuitivní nástrojů pro operátory a výkonnou sadu rozhraní API, které překonávají složitost hello systému Hadoop a zjednodušují operace hello s clustery. Clustery HDInsight v Linuxu poskytují hello webovému uživatelskému rozhraní Ambari i hello Ambari REST API. Zobrazení Ambari v clusterech prostředí HDInsight umožňují funkce zásuvných uživatelských rozhraní.
Viz [Správa clusterů HDInsight pomocí zobrazení Ambari](hdinsight-hadoop-manage-ambari.md) a <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Rozhraní API v zobrazení Apache Ambari – referenční informace</a>.

### <a name="avro"></a>Avro (knihovna rozhraní Microsoft .NET pro Avro)
Hello knihovně Microsoft .NET pro Avro implementuje hello Apache Avro kompaktní binární formát pro předávání dat pro serializaci prostředí Microsoft .NET hello. Definuje jazykově nezávislé schéma, aby data sériovaná v jednom jazyce bylo možné číst v jiném jazyce. Podrobné informace o formátu hello naleznete v hello < target = _ "blank" href = "http://avro.apache.org/docs/current/spec.html" > Apache Avro Specification</a>. Formát Avro soubory podporuje hello Hello distribuované programovací model MapReduce: soubory jsou "rozdělitelné", což znamená, můžete vyhledat libovolný bod v souboru a začít číst od určitého bloku. toofind jak, najdete v části [serializace dat pomocí hello knihovny Microsoft .NET pro Avro](hdinsight-dotnet-avro-serialization.md). Toocome podpora clusteru se systémem Linux.

### <a name="hdfs"></a>HDFS
Hadoop Distributed File System (HDFS) je systém souborů, který se YARN a MapReduce, je základní hello technologie Hadoop. To je hello standardní systém souborů pro clustery Hadoop v HDInsight. Přečtěte si téma [Dotazování dat z úložiště kompatibilního se systémem HDFS](hdinsight-hadoop-use-blob-storage.md).

### <a name="hive"></a>Hive &amp; HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> datového skladu postavená na Hadoop, který vám umožní tooquery softwaru a správu rozsáhlých datových sad v distribuovaném úložišti pomocí jazyka SQL jako nazývaného HiveQL. Hive je stejně jako Pig abstrakce vycházející z nástroje MapReduce a překládá dotazy do řady úloh MapReduce. Hive je blíže systému správy relačních databází tooa než Pig a používá s více strukturovanými daty. Pro Nestrukturovaná data Pig je lepší volbou hello. Viz [Použití nástroje Hive se systémem Hadoop v prostředí HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> je vrstva správy úložiště a tabulek pro systém Hadoop, která poskytuje relační zobrazení dat. V HCatalogu můžete číst a zapisovat soubory v libovolném formátu, který je použitelný pro Hive SerDe (serializátor-deserializátor).

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> představuje knihovnu algoritmů strojového učení, které se dají spouštět v systému Hadoop. S využitím principů statistiky učí aplikace strojového učení systémy toolearn z dat a toouse po výstupy toodetermine budoucího chování. Viz [Generování doporučení pomocí nástroje Mahout v systému Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce je hello starší verze softwarového rozhraní pro Hadoop pro psaní aplikací toobatch zpracování velkých sad paralelně. Úloha MapReduce rozdělí rozsáhlé datové sady a uspořádá hello data do páry klíč hodnota pro zpracování. Úlohy MapReduce je možné spustit na technologii [YARN](#yarn). V tématu <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> v hello Hadoop Wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> je systém koordinace pracovních postupů, který spravuje úlohy platformy Hadoop. Je integrován s hello zásobníku Hadoop a podporuje úlohy systému Hadoop pro MapReduce, Pig, Hive a Sqoop. Lze také použít tooschedule systém tooa konkrétní úlohy, jako jsou programy v jazyce Java nebo skripty prostředí. Viz [Použití Oozie se systémem Hadoop](hdinsight-use-oozie-linux-mac.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> je vrstva relační databáze nad HBase. Phoenix zahrnuje ovladač JDBC, který vám umožní tooquery a správu tabulek SQL přímo. Phoenix přeloží dotazy a další příkazy do nativních volání rozhraní API typu NoSQL, místo použití prostředí MapReduce, což umožní rychlejší aplikace nad uloženými daty NoSQL. Viz [Použití nástrojů Apache Phoenix a SQuirreL s clustery HBase](hdinsight-hbase-phoenix-squirrel.md).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> je vysokoúrovňová platforma, která vám umožní tooperform komplexní transformace MapReduce na rozsáhlých datových sad pomocí jednoduchého skriptovacího jazyka nazvaného Pig Latin. Pig překládá skripty hello Pig Latin, budete spustit v systému Hadoop. Můžete vytvořit uživatelem definované funkce (UDF) tooextend Pig Latin. Viz [Použití Pig se systémem Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> je nástroj pro co nejefektivnější hromadný přenos dat mezi platformou Hadoop a relačními databázemi jako SQL nebo dalšími strukturovanými datovými úložišti. Viz [Použití nástroje Sqoop se systémem Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> je aplikační rozhraní založené na technologii Hadoop YARN, které vytváří komplexní acyklické grafy obecného zpracování dat. Je pružnější a výkonnější nástupce toohello MapReduce rozhraní, které umožňuje datově náročných procesů, například Hive, toorun efektivněji ve velkém měřítku. Viz [„Použití rozhraní Apache Tez pro zlepšení výkonu“ v tématu Použití Hive a HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN je nová generace prostředí MapReduce (MapReduce 2.0 nebo MRv2) hello a podporuje scénáře zpracování dat nad rámec dávkového zpracování s větší škálovatelností a zpracováním v reálném čase prostředí MapReduce. YARN poskytuje správu prostředků a distribuované aplikační rozhraní. Úlohy MapReduce je možné spustit na technologii YARN. Přečtěte si téma <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordinuje procesy ve velkých distribuovaných systémech pomocí sdíleného hierarchického oboru názvů datových registrů (znodes). Znodes obsahují malé množství meta informace potřebné toocoordinate procesů: stav, umístění, konfigurace a tak dále. Podívejte se na příklad uzlu [ZooKeeper s clusterem HBase a vrstvou Apache Phoenix](hdinsight-hbase-phoenix-squirrel-linux.md). 

## <a name="programming-languages-on-hdinsight"></a>Programovací jazyky v prostředí HDInsight
Clustery – Spark, HBase, Kafka, Hadoop a další – podporují řadu programovacích jazyků, ale některé nejsou ve výchozím nastavení nainstalované. Pro knihovny, moduly nebo balíčky nejsou nainstalované ve výchozím nastavení [použít skript akce tooinstall hello součást](hdinsight-hadoop-script-actions-linux.md). 

### <a name="default-programming-language-support"></a>Výchozí podpora programovacích jazyků
Ve výchozím nastavení podporují clustery prostředí HDInsight tyto jazyky:

* Java
* Python

Další jazyky lze nainstalovat pomocí [příkazů skriptů](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Jazyky Java virtual machine (JVM)
Řadu jiných jazyků než Java lze spustit na nástroje Java virtual machine (JVM); spuštění některých z těchto jazyků však může vyžadovat další součásti nainstalované v clusteru hello.

Clustery prostředí HDInsight podporují následující jazyky založené na JVM:

* Clojure
* Jython (Python pro jazyk Java)
* Scala

### <a name="hadoop-specific-languages"></a>Jazyky pro Hadoop
Clustery prostředí HDInsight podporují následující jazyky, které jsou zásobníku technologie Hadoop konkrétní toohello hello:

* Pig Latin pro úlohy Pig
* HiveQL pro úlohy Hive a SparkSQL

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard a HDInsight Premium
HDInsight nabízí cloud pro velké objemy dat ve dvou kategoriích, Standard a Premium. HDInsight Standard poskytuje cluster v podnikovém měřítku, organizace můžete použít toorun jejich velkých objemů dat pracovního vytížení. HDInsight Premium vychází z funkcí úrovně Standard a poskytuje rozšířené analytické a bezpečnostní možnosti pro cluster prostředí HDInsight. Další informace viz [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a name="microsoft-business-intelligence-and-hdinsight"></a>Microsoft business intelligence a HDInsight
Nástroje známé business intelligence (BI) načíst, analyzovat a generování sestav dat integrovaných v prostředí HDInsight pomocí doplňku Power Query buď hello nebo hello ovladače ODBC Microsoft Hive:

* [Připojení aplikace Excel tooHadoop doplněk Power Query](hdinsight-connect-excel-power-query.md): Zjistěte, jak hello tooconnect Excel toohello účet úložiště Azure, která ukládá data z clusteru HDInsight pomocí Microsoft Power Query pro Excel. Jsou vyžadovány pracovní stanice Windows. 
* [Připojení aplikace Excel tooHadoop s hello ovladače ODBC Microsoft Hive](hdinsight-connect-excel-hive-odbc-driver.md): Zjistěte, jak hello tooimport data z prostředí HDInsight pomocí ovladače ODBC Microsoft Hive. Jsou vyžadovány pracovní stanice Windows. 
* [Cloudová platforma Microsoft](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Přečtěte si informace o službě Power BI pro Office 365, stáhněte si zkušební verzi systému SQL Server hello a nastavte si SharePoint Server 2013 a SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Další kroky

* [Začínáme se systémem Hadoop v prostředí HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): Rychlý kurz zřizování clusterů systému Hadoop HDInsight a spuštění ukázkových dotazů nástroje Hive.
* [Začínáme se Spark v prostředí HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): Rychlý kurz vytvoření clusteru Spark a spouštění interaktivních dotazů Spark SQL.
* [Použití R Serveru v prostředí HDInsight](hdinsight-hadoop-r-server-get-started.md): Zahájení práce s R Serverem v prostředí HDInsight Premium.
* [Zřizování clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Zjistěte, jak hello tooprovision clusteru HDInsight Hadoop prostřednictvím portálu Azure, rozhraní příkazového řádku Azure nebo Azure PowerShell.


[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/