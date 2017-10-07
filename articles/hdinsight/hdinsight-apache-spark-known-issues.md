---
title: "cluster aaaTroubleshoot problémy s Apache Spark v Azure HDInsight | Microsoft Docs"
description: "Další informace o problémech souvisejících tooApache clustery Spark v Azure HDInsight a jak toowork kolem ty."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 610c4103-ffc8-4ec0-ad06-fdaf3c4d7c10
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 7373b90524ae5dbb10ab8ded593aa38d12c14b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>Známé problémy pro cluster Apache Spark v HDInsight

Tento dokument uchovává informace o všech hello známé problémy pro hello verzi public preview HDInsight Spark.  

## <a name="livy-leaks-interactive-session"></a>Livy nevracení interaktivní relace.
Při Livy restartování (z Ambari nebo z důvodu restartování virtuálního počítače tooheadnode 0) k interaktivní relaci stále aktivní, bude úniku relaci interaktivní úlohy. Z toho důvodu může nové úlohy zasekla v automatickém hello platných stavu a nejde ho spustit.

**Omezení rizik:**

Použijte následující postup tooworkaround hello problém hello:

1. SSH do headnode. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Spuštění hello následující příkaz toofind hello aplikace ID úlohy interaktivních hello spustit prostřednictvím Livy. 
   
        yarn application –list
   
    Hello výchozí název úlohy bude Livy Pokud hello úloh bylo zahájeno pomocí Livy interaktivní relace se žádné explicitní názvy zadané pro hello Livy relaci spuštění pomocí poznámkového bloku Jupyter hello název úlohy se spustí s remotesparkmagics_ *. 
3. Spusťte následující příkaz tookill hello těchto úloh. 
   
        yarn application –kill <Application ID>

Nové úlohy se spustí systémem. 

## <a name="spark-history-server-not-started"></a>Spark historie Server není spuštěn
Spark historie Server není spuštěn automaticky po vytvoření clusteru.  

**Omezení rizik:** 

Hello historie serveru spusťte ručně z Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Problém s oprávněním v adresáři protokolu Spark
Pokud hdiuser odešle úlohu s spark-submit, dojde k chybě java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (bylo odepřeno oprávnění) a hello nebylo napsáno ovladač protokolu. 

**Omezení rizik:**

1. Přidejte skupinu Hadoop toohello hdiuser. 
2. Zadejte 777 oprávnění na /var/log/spark až po vytvoření clusteru. 
3. Aktualizujte umístění protokolu hello spark pomocí Ambari toobe adresář 777 oprávnění.  
4. Spustit spark odeslat jako sudo.  

## <a name="spark-phoenix-connector-is-not-supported"></a>Spark Phoenix konektor není podporovaný.

V současné době hello Spark Phoenix konektor není podporovaný s clusteru HDInsight Spark.

**Omezení rizik:**

Místo toho musíte použít konektor Spark HBase hello. Pokyny naleznete v části [jak konektor toouse Spark HBase](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).

## <a name="issues-related-toojupyter-notebooks"></a>Problémy související s tooJupyter poznámkových bloků
Toto jsou některé známé problémy související tooJupyter notebooks.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Poznámkové bloky s jiné znaky než ASCII v názvech souborů
Jupyter notebooks, které mohou být používány clustery Spark HDInsight by neměl mít jiné znaky než ASCII v názvech souborů. Pokud se pokusíte tooupload soubor prostřednictvím hello Jupyter uživatelského rozhraní, která má název souboru jiné sady než ASCII, se nezdaří bez upozornění (tedy Jupyter nebude umožňují nahrát soubor hello, ale nezpůsobí výjimku viditelné chyba buď). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Chyba při načítání poznámkové bloky o větší velikosti
Může dojít k chybě  **`Error loading notebook`**  při načtení poznámkových bloků, které jsou větší velikost.  

**Omezení rizik:**

K této chybě dojde, neznamená, že vaše data jsou poškozené nebo ztraceny.  Poznámkové bloky jsou stále na disku v `/var/lib/jupyter`, a můžete SSH do clusteru tooaccess hello je. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Jakmile se připojíte toohello clusteru pomocí protokolu SSH, můžete zkopírovat poznámkové bloky z clusteru tooyour místního počítače (pomocí spojovací bod služby nebo WinSCP) jako ztrátu hello zálohování tooprevent žádné důležitých dat v poznámkovém bloku hello. Pak můžete tunelového propojení SSH do vaší headnode na portu 8001 tooaccess Jupyter bez průchodu přes bránu hello.  Odtud můžete vymazat výstup hello v poznámkovém bloku a uložte ji znovu notebooky hello toominimize velikost.

tooprevent nedocházelo v hello budoucí, je třeba postupovat podle doporučených postupů, které tato chyba:

* Je důležité tookeep hello poznámkového bloku velikost malé. Všechny výstupy z úloh Spark, je odeslána zpět tooJupyter je uchován v hello Poznámkový blok.  Je osvědčeným postupem s Jupyter v obecné tooavoid systémem `.collect()` na velké RDD společnosti nebo dataframes; místo toho, pokud chcete toopeek na obsah RDD, zvažte spuštění `.take()` nebo `.sample()` tak, aby výstupu není získat příliš velký.
* Navíc při ukládání Poznámkový blok, zrušte všechny výstupní velikost hello tooreduce buněk.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Poznámkový blok počáteční spuštění trvá déle, než se očekávalo
První příkaz kódu do poznámkového bloku Jupyter pomocí Spark magic může trvat déle než minutu.  

**Vysvětlení:**

K tomu dochází, protože při spuštění první buňky kódu hello. V pozadí hello inicializováno konfiguraci relace a Spark, SQL a jsou nastavené kontexty Hive. Po nastavení jsou tyto kontexty, první příkaz hello běží a díky tomu hello dojem, že příkaz hello trvalo dlouhou dobu toocomplete.

### <a name="jupyter-notebook-timeout-in-creating-hello-session"></a>Časový limit poznámkového bloku Jupyter při vytváření hello relace
Pokud Spark cluster nemá dostatek prostředků, hello Spark a jádra Pyspark v hello Poznámkový blok Jupyter bude časový limit pokusu o toocreate hello relace. 

**Způsoby zmírnění rizik:** 

1. Uvolněte některé prostředky v clusteru Spark pomocí:
   
   * Zastavování jiných poznámkových bloků Spark budete toohello zavřete a zastavení nabídky nebo v hello poznámkového bloku Exploreru kliknete na vypnutí.
   * Zastavování dalších aplikací Spark z YARN.
2. Restartujte hello Poznámkový blok, které jste se pokoušeli toostart nahoru. Dostatek prostředků, musí být k dispozici pro toocreate můžete nyní relaci.

## <a name="see-also"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

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

