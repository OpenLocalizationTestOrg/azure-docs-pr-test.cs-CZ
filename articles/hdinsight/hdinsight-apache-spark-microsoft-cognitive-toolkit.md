---
title: "aaaMicrosoft kognitivní Toolkit s Azure HDInsight Spark pro přímý learning | Microsoft Docs"
description: "Zjistěte, jak sada vyškolení Microsoft kognitivní nástrojů hloubkové learning modelu může být datové sady použité tooa pomocí hello Spark Python API v clusteru Azure HDInsight Spark."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: c296d4697f16d4ef6a958fdb55289807d745ea40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Použít Microsoft kognitivní Toolkit hloubkové učení modelu s clusteru Azure HDInsight Spark

V tomto článku hello následující kroky.

1. Spusťte vlastní skript tooinstall Microsoft kognitivní Toolkit v clusteru Azure HDInsight Spark.

2. Jak nahrát toosee clusteru Spark Jupyter poznámkového bloku toohello tooapply vyškolení Microsoft kognitivní Toolkit hloubkové učení modelu toofiles v účtu úložiště objektů Blob Azure pomocí hello [API Python Spark (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Než začnete tento kurz, musíte mít předplatné Azure. Přečtěte si téma [Bezplatné vytvoření účtu Microsoft Azure ještě dnes](https://azure.microsoft.com/free).

* **Cluster Azure HDInsight Spark**. V tomto článku vytvořte cluster Spark 2.0. Pokyny najdete v tématu [cluster vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Jak toto řešení tok?

Toto řešení je rozděleno mezi v tomto článku a který v rámci tohoto kurzu nahrajete poznámkového bloku Jupyter. V tomto článku proveďte následující kroky hello:

* Spusťte akci skriptu na clusteru HDInsight Spark balíčků tooinstall Microsoft kognitivní Toolkit a Python.
* Nahrajte poznámkového bloku Jupyter hello, který spouští hello řešení toohello clusteru HDInsight Spark.

Hello následující zbývající kroky jsou popsané v hello Poznámkový blok Jupyter.

- Načíst ukázková bitové kopie do Spark Resiliant distribuované datové sady nebo RDD
   - Načtení moduly a definování přednastavení
   - Stáhnout datovou sadu hello místně na clusteru Spark hello
   - Převést RDD hello datové sady
- Skóre hello obrázky pomocí modulu trained model kognitivní Toolkit
   - Stáhnout clusteru Spark toohello modelu kognitivní Toolkit hello cvičení
   - Definování funkcí toobe používané uzlů pracovního procesu
   - Skóre hello obrázků uzlů pracovního procesu
   - Vyhodnocení přesnosti modelu


## <a name="install-microsoft-cognitive-toolkit"></a>Nainstalovat sadu Microsoft kognitivní Toolkit

Sada nástrojů pro kognitivní můžete nainstalovat na clusteru Spark pomocí akce skriptu. Akce skriptu používá vlastní skripty tooinstall součásti v clusteru hello, které nejsou k dispozici ve výchozím nastavení. Pomocí HDInsight .NET SDK, nebo pomocí prostředí Azure PowerShell, můžete použít vlastní skript hello z hello portálu Azure. Můžete taky hello skriptu tooinstall hello toolkit buď jako součást vytváření clusteru, nebo po hello clusteru je spuštěná. 

V tomto článku používáme hello portálu tooinstall hello toolkit, po vytvoření clusteru hello. Jiné způsoby toorun hello vlastních skriptů, najdete v části [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-hello-azure-portal"></a>Pomocí hello portálu Azure

Pokyny jak toouse hello portálu Azure toorun skript akce, najdete v části [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Ujistěte se, že zadáte následující vstupy tooinstall kognitivní nástrojů Microsoft hello.

* Zadejte hodnotu pro název akce skriptu hello.

* Pro **Bash skriptu URI**, zadejte `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.

* Zajistěte, aby spuštění skriptu hello pouze na hello head a pracovní uzly a zrušte všechny ostatní zaškrtávací políčka hello.

* Klikněte na možnost **Vytvořit**.

## <a name="upload-hello-jupyter-notebook-tooazure-hdinsight-spark-cluster"></a>Nahrát tooAzure poznámkového bloku Jupyter hello clusteru HDInsight Spark

hello toouse kognitivní nástrojů Microsoft s hello clusteru Azure HDInsight Spark, je nutné načíst hello Poznámkový blok Jupyter **CNTK_model_scoring_on_Spark_walkthrough.ipynb** toohello clusteru Azure HDInsight Spark. Tento poznámkový blok je k dispozici na webu GitHub na [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. Klonování úložiště GitHub hello [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). Tooclone pokyny najdete v části [klonování úložiště](https://help.github.com/articles/cloning-a-repository/).

2. Hello portálu Azure, otevřete okno clusteru Spark hello, které jste už zřízené, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**.

    Poznámkový blok Jupyter hello můžete spustit také tak, že přejdete na adresu URL toohello `https://<clustername>.azurehdinsight.net/jupyter/`. Nahraďte \<clustername > s názvem hello clusteru HDInsight.

3. Z hello Poznámkový blok Jupyter, klikněte na tlačítko **nahrát** v hello pravém horním rohu a pak přejděte toohello umístění, které jste naklonovali úložiště GitHub hello.

    ![Nahrát tooAzure poznámkového bloku Jupyter clusteru HDInsight Spark](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "tooAzure poznámkového bloku Jupyter nahrát clusteru HDInsight Spark")

4. Klikněte na tlačítko **nahrát** znovu.

5. Po odeslání hello Poznámkový blok klikněte na název hello hello Poznámkový blok a pak postupujte podle pokynů hello v poznámkovém bloku hello sám sebe na tom, jak tooload hello datové sady a provádět hello kurzu.

## <a name="see-also"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Analýza dat telemetrie Application Insights pomocí Sparku v HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

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

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
