---
title: "clusteru aaaManage prostředky pro Apache Spark v Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse spravovat prostředky pro clustery Spark v Azure HDInsight pro dosažení vyššího výkonu."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9da7d4e3-458e-4296-a628-77b14643f7e4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: e18682a24f77494db884105f9db03c0a350ddad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Správa prostředků v clusteru Apache Spark v Azure HDInsight 

V tomto článku se dozvíte, jak rozhraní hello tooaccess jako uživatelské rozhraní Ambari, uživatelském rozhraní YARN a hello Spark historie serveru přidruženého k vašemu clusteru Spark. Naučíte se také o tom, jak tootune hello konfigurace clusteru pro optimální výkon.

**Požadavky:**

Musíte mít následující hello:

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-launch-hello-ambari-web-ui"></a>Jak mohu spustit hello webové uživatelské rozhraní Ambari?
1. Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel). Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.
2. Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel**. Po zobrazení výzvy zadejte přihlašovací údaje správce hello clusteru Spark hello.

    ![Spusťte Ambari](./media/hdinsight-apache-spark-resource-manager/hdinsight-launch-cluster-dashboard.png "spuštění Správce prostředků")
3. Mělo by se zobrazit hello webové uživatelské rozhraní Ambari, jak je uvedeno níže.

    ![Webovému uživatelskému rozhraní Ambari](./media/hdinsight-apache-spark-resource-manager/ambari-web-ui.png "webovému uživatelskému rozhraní Ambari")   

## <a name="how-do-i-launch-hello-spark-history-server"></a>Jak spuštění hello Spark historie serveru?
1. Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel).
2. Z hello clusteru okno, v části **rychlé odkazy**, klikněte na tlačítko **řídicí panel clusteru**. V hello **řídicí panel clusteru** okně klikněte na tlačítko **Spark historie serveru**.

    ![Spark historie serveru](./media/hdinsight-apache-spark-resource-manager/launch-history-server.png "Spark historie serveru")

    Po zobrazení výzvy zadejte přihlašovací údaje správce hello clusteru Spark hello.

## <a name="how-do-i-launch-hello-yarn-ui"></a>Jak mohu spustit hello uživatelském rozhraní Yarn?
Můžete použít hello uživatelském rozhraní YARN toomonitor aplikace, které jsou aktuálně spuštěné na clusteru Spark hello.

1. V okně hello clusteru, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **YARN**.

    ![Spustit uživatelské rozhraní YARN](./media/hdinsight-apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > Alternativně můžete také spustit hello uživatelském rozhraní YARN z hello uživatelského rozhraní Ambari. Klikněte na tlačítko toolaunch hello uživatelského rozhraní Ambari, v okně clusteru hello, **řídicí panel clusteru**a potom klikněte na **řídicí panel clusteru HDInsight**. Z hello uživatelského rozhraní Ambari, klikněte na tlačítko **YARN**, klikněte na tlačítko **rychlé odkazy**, klikněte na tlačítko hello active resource Manageru a pak klikněte na **uživatelského rozhraní ResourceManager**.
   >
   >

## <a name="what-is-hello-optimum-cluster-configuration-toorun-spark-applications"></a>Co je hello optimální clusteru konfigurace toorun Spark aplikace?
Hello tři klíčové parametry, které lze použít pro konfigurace Spark v závislosti na požadavcích aplikace jsou `spark.executor.instances`, `spark.executor.cores`, a `spark.executor.memory`. Vykonavatele je proces spuštění pro aplikaci Spark. Tato běží na hello pracovního uzlu a je zodpovědná toocarry hello úloh aplikace hello. Hello výchozí počet velikost hello vykonavatele pro každý cluster a vykonavatelů je vypočítáváno na hello počet uzlů pracovního procesu a velikost uzlu hello pracovního procesu. Tyto jsou uložené v `spark-defaults.conf` na hlavních uzlech clusteru hello.

tři parametry konfigurace Hello lze nastavit na úrovni clusteru hello (pro všechny aplikace, které běží na clusteru hello) nebo lze zadat pro každou jednotlivých aplikací.

### <a name="change-hello-parameters-using-ambari-ui"></a>Změňte parametry hello pomocí uživatelského rozhraní Ambari
1. Z uživatelského rozhraní Ambari hello klikněte na tlačítko **Spark**, klikněte na tlačítko **konfigurací**a potom rozbalte **vlastní spark – výchozí**.

    ![Nastavení parametrů pomocí nástroje Ambari](./media/hdinsight-apache-spark-resource-manager/set-parameters-using-ambari.png)
2. Hello výchozí hodnoty jsou dobrou toohave 4 aplikací Spark v hello clusteru současně spustit. Můžete změny tyto hodnoty z hello uživatelské rozhraní, jak je uvedeno níže.

    ![Nastavení parametrů pomocí nástroje Ambari](./media/hdinsight-apache-spark-resource-manager/set-executor-parameters.png)
3. Klikněte na tlačítko **Uložit** změny konfigurace toosave hello. V horní části hello hello stránky, zobrazí se výzva toorestart hello všechny ovlivněné služby. Klikněte na tlačítko **restartujte**.

    ![Restartujte služby](./media/hdinsight-apache-spark-resource-manager/restart-services.png)

### <a name="change-hello-parameters-for-an-application-running-in-jupyter-notebook"></a>Změňte parametry hello pro aplikace běžící v poznámkového bloku Jupyter
Pro aplikace běžící v hello Poznámkový blok Jupyter, můžete použít hello `%%configure` kouzelná změny konfigurace toomake hello. V ideálním případě je nutné provést tyto změny od začátku hello hello aplikace před spuštěním vaše první buňky kódu. To zajistí, že konfigurace hello je použité toohello Livy relace, když se vytvoří. Pokud chcete konfiguraci hello toochange v pozdější fázi v hello aplikaci, musíte použít hello `-f` parametr. Ale pomocí tohoto postupu, takže všechny průběhu v hello aplikace se ztratí.

Následující fragment Hello ukazuje, jak toochange hello konfigurace pro aplikace běžící v Jupyter.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Parametry konfigurace musí být předán jako řetězec formátu JSON a musí být na další řádek hello po hello magic, jak je znázorněno v příkladu sloupec hello.

### <a name="change-hello-parameters-for-an-application-submitted-using-spark-submit"></a>Změna hello parametry pro aplikaci odesílat pomocí spark odeslání
Následující příkaz je příkladem jak toochange hello parametry konfigurace pro aplikaci batch, které je odeslána pomocí `spark-submit`.

    spark-submit --class <hello application class tooexecute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-hello-parameters-for-an-application-submitted-using-curl"></a>Změňte parametry hello pro aplikaci odesílat pomocí cURL
Následující příkaz je příkladem jak toochange hello parametry konfigurace pro aplikaci batch, které je odeslána pomocí pomocí cURL.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<hello application class tooexecute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="how-do-i-change-these-parameters-on-a-spark-thrift-server"></a>Změna těchto parametrů na serveru Spark Thrift
Spark Thrift Server poskytuje clusteru Spark tooa přístupu JDBC nebo ODBC a je použité tooservice dotazů Spark SQL. Nástroje, jako Power BI, Tableau atd. použijte protokol toocommunicate ODBC s dotazů Spark SQL serveru Spark Thrift tooexecute jako aplikace Spark. Při vytvoření clusteru Spark, dvě instance hello spuštění serveru Spark Thrift, jednu na každý hlavního uzlu. Každý Server Spark Thrift se zobrazí jako aplikace Spark v uživatelském rozhraní YARN hello.

Použití serveru Spark Thrift Spark přidělení dynamické vykonavatele a proto hello `spark.executor.instances` nepoužívá. Místo toho používá serveru Spark Thrift `spark.dynamicAllocation.minExecutors` a `spark.dynamicAllocation.maxExecutors` toospecify hello vykonavatele count. Hello parametry konfigurace `spark.executor.cores` a `spark.executor.memory` je použít toomodify hello vykonavatele velikost. Tyto parametry můžete změnit, jak je uvedeno níže.

* Rozbalte hello **Advanced spark thrift-sparkconf** kategorie tooupdate hello parametry `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, a `spark.executor.memory`.

    ![Konfigurace serveru Spark thrift](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-1.png)    
* Rozbalte hello **vlastní spark-thrift-sparkconf** kategorie tooupdate hello parametr `spark.executor.cores`.

    ![Konfigurace serveru Spark thrift](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="how-do-i-change-hello-driver-memory-of-hello-spark-thrift-server"></a>Změna hello ovladač paměti hello serveru Spark Thrift
Serveru Spark Thrift ovladač paměť je % nakonfigurované too25 velikosti paměti RAM hlavního uzlu hello, pokud celková velikost paměti RAM hello hlavního uzlu hello vyšší než 14GB. Můžete hello konfigurace uživatelského rozhraní Ambari toochange hello ovladač paměti, jak je uvedeno níže.

* Z hello uživatelského rozhraní Ambari klikněte na tlačítko **Spark**, klikněte na tlačítko **konfigurací**, rozbalte položku **Advanced spark env**a pak zadejte hodnotu hello **spark_thrift_cmd_opts**.

    ![Konfigurace serveru Spark thrift paměti RAM](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="i-do-not-use-bi-with-spark-cluster-how-do-i-take-hello-resources-back"></a>S clusterem Spark nepoužívám BI. Jak můžu zpět trvat hello prostředky?
Vzhledem k tomu, že používáme dynamické přidělování Spark, hello jenom prostředky, které se spotřebovávají thrift serveru jsou hello prostředky pro dva vzory aplikace hello. tooreclaim tyto prostředky, je nutné zastavit hello Thrift Server běžící v clusteru hello.

1. Z hello uživatelského rozhraní Ambari, hello levém podokně klikněte na tlačítko **Spark**.
2. Na další stránku hello, klikněte na **Spark Thrift servery**.

    ![Restartujte thrift server](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-1.png)
3. Měli byste vidět dvě headnodes hello, na které hello Spark Thrift serveru běží. Klikněte na jednu z hello headnodes.

    ![Restartujte thrift server](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-2.png)
4. Další stránku Hello uvádí všechny hello služeb spuštěných na tomto headnode. Hello seznamu klikněte na tlačítko Další tooSpark rozevírací tlačítko hello Thrift serveru a pak klikněte na tlačítko **Zastavit**.

    ![Restartujte thrift server](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-3.png)
5. Opakujte tyto kroky na hello také jiné headnode.

## <a name="my-jupyter-notebooks-are-not-running-as-expected-how-can-i-restart-hello-service"></a>Moje poznámkové bloky Jupyter neběží podle očekávání. Jak můžete hello službu restartovat?
Spusťte hello webové uživatelské rozhraní Ambari, jak je uvedeno výše. V levém navigačním podokně hello, klikněte na **Jupyter**, klikněte na tlačítko **služby akce**a potom klikněte na **restartujte všechny**. Tím se spustí služba Jupyter hello na všechny headnodes hello.

    ![Restart Jupyter](./media/hdinsight-apache-spark-resource-manager/restart-jupyter.png "Restart Jupyter")

## <a name="how-do-i-know-if-i-am-running-out-of-resources"></a>Jak poznám, že pokud používám mimo prostředky?
Spusťte hello uživatelském rozhraní Yarn, jak je uvedeno výše. V tabulce clusteru metriky nad úvodní obrazovka, zkontrolujte hodnoty **paměti používá** a **celkové paměti** sloupce. Pokud jsou hodnoty hello 2 velmi zavřít, nemusí být další aplikace dostatek prostředků toostart hello. Hello totéž platí i toohello **VCores používá** a **VCores celkem** sloupce. Také v hlavní zobrazení hello, pokud je aplikace zůstanou ve **platných** stavu a není přechod do **systémem** ani **se nezdařilo** stavu, může se také jednat jako ukazatel toho to není získávání toostart dostatek prostředků.

    ![Resource Limit](./media/hdinsight-apache-spark-resource-manager/resource-limit.png "Resource Limit")

## <a name="how-do-i-kill-a-running-application-toofree-up-resource"></a>Jak ukončení spuštění aplikace toofree až prostředků?
1. V uživatelském rozhraní Yarn hello z hello levém panelu, klikněte na **systémem**. Hello seznamu spuštěných aplikací, určit toobe aplikace hello ukončeny a klikněte na hello **ID**.

    ![Příkaz kill App1](./media/hdinsight-apache-spark-resource-manager/kill-app1.png "Kill App1")

2. Klikněte na tlačítko **ukončení aplikace** na hello pravém horním rohu klikněte **OK**.

    ![Příkaz kill počítači App2](./media/hdinsight-apache-spark-resource-manager/kill-app2.png "Kill počítači App2")

## <a name="see-also"></a>Viz také
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>Pro analytiky dat

* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Analýza dat telemetrie Application Insights pomocí Sparku v HDInsight](hdinsight-spark-analyze-application-insight-logs.md)
* [Použití Caffe v Azure HDInsight Spark pro distribuované hloubkové learning](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a>Pro vývojáře, Spark

* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)
