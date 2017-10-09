---
title: "aaaDebug Apache Spark úlohy spuštěné v Azure HDInsight | Microsoft Docs"
description: "Pomocí uživatelského rozhraní YARN, Spark uživatelského rozhraní a Spark historie serveru tootrack a ladění úloh spuštěných v clusteru Spark v Azure HDInsight"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 33d352a5773920735aa4e5e8532b78122f381377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Ladění Apache Spark úlohy spuštěné v Azure HDInsight

V tomto článku se dozvíte, jak tootrack a ladění úloh spuštěných na clusterů HDInsight pomocí hello uživatelském rozhraní YARN, Spark uživatelského rozhraní, Spark a hello Spark historie serveru. V tomto článku jsme spustí úlohu Spark pomocí poznámkového bloku dostupné s clusterem Spark hello, **strojového učení: prediktivní analýzy dat kontroly potravin pomocí MLLib**. Můžete použít následující tootrack aplikace, která jste odeslali pomocí jakékoli jiné přístup také, například postup hello **odeslání spark**.

## <a name="prerequisites"></a>Požadavky
Musíte mít následující hello:

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Můžete by měl mít spuštění hello Poznámkový blok,  **[strojového učení: prediktivní analýzy dat kontroly potravin pomocí MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**. Návod, jak toorun tento poznámkový blok, postupujte podle hello odkaz.  

## <a name="track-an-application-in-hello-yarn-ui"></a>Sledování aplikace v uživatelském rozhraní YARN hello
1. Spusťte hello uživatelském rozhraní YARN. V okně hello clusteru, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **YARN**.
   
    ![Spustit uživatelské rozhraní YARN](./media/hdinsight-apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > Alternativně můžete také spustit hello uživatelském rozhraní YARN z hello uživatelského rozhraní Ambari. Klikněte na tlačítko toolaunch hello uživatelského rozhraní Ambari, v okně clusteru hello, **řídicí panel clusteru**a potom klikněte na **řídicí panel clusteru HDInsight**. Z hello uživatelského rozhraní Ambari, klikněte na tlačítko **YARN**, klikněte na tlačítko **rychlé odkazy**, klikněte na tlačítko hello active resource Manageru a pak klikněte na **uživatelského rozhraní ResourceManager**.    
   > 
   > 
2. Protože jste spustili úlohy Spark hello pomocí Jupyter notebooks, aplikace hello má název hello **remotesparkmagics** (to je hello název pro všechny aplikace, které jsou spuštěné z poznámkových bloků hello). Další informace o úloze hello klikněte na tlačítko ID aplikace hello proti tooget název aplikace hello. Spustí zobrazení aplikace hello.
   
    ![Vyhledání ID aplikací Spark](./media/hdinsight-apache-spark-job-debugging/find-application-id.png)
   
    Pro tyto aplikace, které jsou spouštěny z poznámkových bloků Jupyter hello hello stav je vždy **systémem** do ukončení hello Poznámkový blok.
3. Z hlediska aplikace hello podrobnostem další toofind out hello kontejnery přidružené aplikace hello a protokoly hello (stdout/stderr). Hello Spark uživatelského rozhraní můžete také spustit kliknutím hello propojení odpovídající toohello **sledování adresy URL**, jak je uvedeno níže. 
   
    ![Stažení protokolů o kontejneru](./media/hdinsight-apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-hello-spark-ui"></a>Sledování aplikace hello Spark uživatelského rozhraní
V hello Spark uživatelského rozhraní můžete se přejděte do hello Spark úlohy, které jsou vytvořený službou hello aplikaci, kterou jste spustili dříve.

1. toolaunch hello Spark uživatelského rozhraní, ze zobrazení aplikace hello, klikněte na odkaz hello proti hello **sledování adresy URL**, jak ukazuje snímek obrazovky hello výše. Zobrazí se všechny úlohy Spark hello, které jsou spouštěny podle hello aplikace běžící v hello Poznámkový blok Jupyter.
   
    ![Zobrazit úlohy Spark](./media/hdinsight-apache-spark-job-debugging/view-spark-jobs.png)
2. Klikněte na tlačítko hello **vykonavatelů** kartě toosee zpracování a ukládání informací pro každý prováděcího modulu. Můžete také načíst zásobník volání hello kliknutím na hello **vláken Dump** odkaz.
   
    ![Zobrazit vykonavatelů Spark](./media/hdinsight-apache-spark-job-debugging/view-spark-executors.png)
3. Klikněte na tlačítko hello **fázích** kartě toosee hello fázemi, přidruženou k aplikaci hello.
   
    ![Zobrazit Spark fáze](./media/hdinsight-apache-spark-job-debugging/view-spark-stages.png)
   
    Každá fáze může mít více úloh, pro které je možné zobrazit provádění statistiky, jako vidíte níže.
   
    ![Zobrazit Spark fáze](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-details.png) 
4. Na stránce Podrobnosti hello fáze můžete spustit DAG vizualizace. Rozbalte hello **DAG vizualizace** odkaz v horní části hello hello stránky, jak je uvedeno níže.
   
    ![Zobrazit vizualizace Spark fázích DAG](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    DAG nebo přímé Aclyic grafu představuje hello různých fázích v aplikaci hello. Každé pole blue v grafu hello představuje Spark operace, vyvolané z aplikace hello.
5. Na stránce podrobnosti fáze hello můžete také spustit zobrazení časové osy aplikace hello. Rozbalte hello **časová osa událostí** odkaz v horní části hello hello stránky, jak je uvedeno níže.
   
    ![Zpracuje událost časová osa zobrazení Spark](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Zobrazí se události hello Spark v podobě hello časové osy. Časová osa zobrazení Hello je k dispozici ve třech úrovních mezi úlohami v rámci úlohy a v rámci úsek. Hello obrázku výše zaznamená hello časová osa zobrazení pro danou fázi.
   
   > [!TIP]
   > Pokud vyberete hello **zapnout zvětšování** zaškrtávací políčko můžete posunete doleva a doprava napříč hello časová osa zobrazení.
   > 
   > 
6. Ostatní karty v hello uživatelského rozhraní Spark poskytují užitečné informace o hello Spark také instanci.
   
   * Karta úložiště – Pokud vaše aplikace vytvoří RDDs, může najít informace o těch, na kartě úložiště hello.
   * Karta prostředí – tato karta obsahuje mnoho užitečných informací o vaší instanci Spark, jako je například hello 
     * Verze Scala
     * Adresář protokolu událostí související s clusterem hello
     * Počet jader vykonavatele pro aplikaci hello
     * Atd.

## <a name="find-information-about-completed-jobs-using-hello-spark-history-server"></a>Najít informace o dokončené úlohy pomocí Spark historie Server hello
Po dokončení úlohy je v hello Spark historie serveru jako trvalý hello informace o úloze hello.

1. Klikněte na tlačítko toolaunch hello serveru Spark historie, v okně clusteru hello, **řídicí panel clusteru**a potom klikněte na **Spark historie serveru**.
   
    ![Spusťte Server historie Spark](./media/hdinsight-apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > Alternativně můžete také spustit hello uživatelské rozhraní serveru Spark historie z hello uživatelského rozhraní Ambari. Klikněte na tlačítko toolaunch hello uživatelského rozhraní Ambari, v okně clusteru hello, **řídicí panel clusteru**a potom klikněte na **řídicí panel clusteru HDInsight**. Z hello uživatelského rozhraní Ambari, klikněte na tlačítko **Spark**, klikněte na tlačítko **rychlé odkazy**a potom klikněte na **uživatelské rozhraní serveru Spark historie**.
   > 
   > 
2. Zobrazí se všechny aplikace hello dokončit uvedené. Klikněte na tlačítko toodrill ID aplikace dolů do aplikace pro další informace.
   
    ![Spusťte Server historie Spark](./media/hdinsight-apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Viz také
*  [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

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


