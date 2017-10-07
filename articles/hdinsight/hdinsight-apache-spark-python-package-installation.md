---
title: "Akce aaaScript – balíčky nainstalovat Python s Jupyter v Azure HDInsight | Microsoft Docs"
description: "Podrobný návod, jak toouse skript akce tooconfigure poznámkové bloky Jupyter k dispozici s HDInsight Spark clusterů toouse python externí balíčky."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21978b71-eb53-480b-a3d1-c5d428a7eb5b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 54db79e67995dee7ca00abff979f7d74ae5ab9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-script-action-tooinstall-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Použijte externí balíčky Python akce skriptu tooinstall pro poznámkové bloky Jupyter v clusterech Apache Spark v HDInsight
> [!div class="op_single_selector"]
> * [Pomocí buňky magic](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [Pomocí akce skriptu](hdinsight-apache-spark-python-package-installation.md)
>
>

Zjistěte, jak toouse akcí skriptů tooconfigure cluster Apache Spark v HDInsight (Linux) toouse externí, komunity podílí **python** balíčky, které nejsou součástí clusteru hello se na pole.

> [!NOTE]
> Můžete také nakonfigurovat poznámkového bloku Jupyter pomocí `%%configure` kouzelná toouse externí balíčky. Pokyny najdete v tématu [použijte externí balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark v HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

Můžete hledat hello [indexu balíčků](https://pypi.python.org/pypi) hello úplný seznam balíčků, které jsou k dispozici. Seznam dostupných balíčků můžete také získat z jiných zdrojů. Například můžete nainstalovat balíčky, které jsou k dispozici prostřednictvím [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) nebo [conda vytvoření](https://conda-forge.github.io/feedstocks.html).

V tomto článku se dozvíte, jak tooinstall hello [TensorFlow](https://www.tensorflow.org/) balíček pomocí skriptu Actoin v clusteru a použít ho pomocí poznámkového bloku Jupyter hello.

## <a name="prerequisites"></a>Požadavky
Musíte mít následující hello:

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > Pokud už cluster Spark na HDInsight Linux nemáte, můžete spustit skript akce při vytváření clusteru. Navštivte hello dokumentace na [jak toouse vlastní skript akce](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a>Použijte externí balíčky s poznámkovými bloky Jupyter

1. Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel). Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.   

2. Z okna clusteru Spark hello, klikněte na tlačítko **akcí skriptů** pod **využití**. Spuštěním hello vlastní akce, který se instaluje TensorFlow hello head uzlů a uzlů pracovního procesu hello. skript bash Hello lze odkazovat z: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh návštěvu hello dokumentaci na [jak toouse vlastní skript akce](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   > [!NOTE]
   > Existují dvě python instalace v clusteru hello. Spark použije hello Anaconda python instalace nacházející se v `/usr/bin/anaconda/bin`. Referenční instalaci ve vaší vlastní akce prostřednictvím `/usr/bin/anaconda/bin/pip` a `/usr/bin/anaconda/bin/conda`.
   > 
   > 

3. Otevřete Poznámkový blok PySpark Jupyter

    ![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Vytvoření nového poznámkového bloku Jupyter")

4. Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb. Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.

    ![Zadejte název pro hello notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "zadejte název pro hello Poznámkový blok")

5. Nyní budete `import tensorflow` a spusťte hello world – ukázka. 

    Toocopy kódu:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    Hello výsledek bude vypadat takto:
    
    ![Provádění kódu TensorFlow](./media/hdinsight-apache-spark-python-package-installation/execution.png "TensorFlow spuštění kódu")



## <a name="seealso"></a>Viz také
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
* [Použijte externí balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark v HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
