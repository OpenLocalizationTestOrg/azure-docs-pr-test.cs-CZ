---
title: cluster aaaCreate Apache Spark v Azure HDInsight | Microsoft Docs
description: "Rychlý start HDInsight Spark na tom, jak clusteru toocreate Apache Spark v HDInsight."
keywords: spark quickstart, interactive spark, interactive query, hdinsight spark, azure spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 002f71b3cd4fb315d4a556cebc9263026515ec4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Vytvoření clusteru Apache Spark ve službě Azure HDInsight

V tomto článku se dozvíte, jak clusteru toocreate Apache Spark v Azure HDInsight. Informace o Apache Spark ve službě HDInsight najdete v tématu [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md).

   ![Rychlý start diagramu popisující kroky toocreate cluster Apache Spark v Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "rychlý start Spark pomocí Apache Spark v HDInsight. Popsané postupy: vytvoření clusteru, spuštění interaktivního dotazu Spark")

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Než začnete tento kurz, musíte mít předplatné Azure. Přečtěte si téma [Bezplatné vytvoření účtu Microsoft Azure ještě dnes](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>Vytvoření clusteru HDInsight Spark

V této části vytvoříte cluster HDInsight Spark pomocí [šablony Azure Resource Manageru](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Ostatní metody tvorby clusteru najdete v části [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klikněte na tlačítko hello následující šablony hello tooopen bitové kopie v hello portálu Azure.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. Zadejte hello následující hodnoty:

    ![Vytvoření clusteru HDInsight Spark pomocí šablony Azure Resource Manageru](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Vytvoření clusteru Spark ve službě HDInsight pomocí šablony Azure Resource Manageru")

    * **Předplatné:** Vyberte předplatné Azure pro tento cluster.
    * **Skupina prostředků:** Vytvořte skupinu prostředků nebo vyberte stávající. Skupina prostředků je použité toomanage prostředků Azure pro projekty.
    * **Umístění**: Vyberte umístění pro skupinu prostředků hello. Šablona Hello používá toto umístění pro vytvoření clusteru hello stejně jako u hello výchozí clusteru úložiště.
    * **Název clusteru**: Zadejte název pro cluster HDInsight hello, které chcete toocreate.
    * **Spark verze**: vyberte **2.0** jako hello verze, kterou chcete tooinstall v clusteru hello.
    * **Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je admin.
    * **Uživatelské jméno a heslo SSH**.

   Zapište si tyto hodnoty.  Budete potřebovat později v kurzu hello.

3. Vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**, vyberte **Pin toodashboard**a potom klikněte na **nákupu**. Zobrazí se nová dlaždice s názvem Odeslání nasazení pro šablonu nasazení. Jak dlouho trvá přibližně 20 minut toocreate hello clusteru.

Pokud narazíte na problém s vytváření clusterů HDInsight, může to být hello správná oprávnění toodo proto nemají. Další informace najdete v tématu popisujícím [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

> [!NOTE]
> V tomto článku vytváří cluster Spark, který používá [clusteru úložiště objektů BLOB služby Azure Storage jako hello](hdinsight-hadoop-use-blob-storage.md). Můžete také vytvořit cluster Spark, která používá [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) jako hello výchozí úložiště. Pokyny naleznete v tématu [Vytvoření clusteru HDInsight pomocí Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

## <a name="run-a-hive-query-using-spark-sql"></a>Spuštění dotazu Hive pomocí Spark SQL

Když použijete poznámkového bloku Jupyter nakonfigurované pro váš cluster HDInsight Spark, zobrazí jedno z přednastavení `sqlContext` , které můžete použít dotazy Hive toorun pomocí Spark SQL. V této části se dozvíte, jak toostart poznámkového bloku Jupyter a poté spusťte základního dotazu Hive.

1. Otevřete hello [portál Azure](https://portal.azure.com/).

2. Když jste se rozhodli řídicí panel toohello toopin hello clusteru, klikněte na dlaždici hello clusteru z hello řídicí panel toolaunch hello clusteru okno.

    Pokud připnete není hello toohello řídicí panel clusteru, v levém podokně hello, klikněte na tlačítko **clustery HDInsight**a potom klikněte na cluster hello jste vytvořili.

3. V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.

   ![Otevřete Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "poznámkového bloku Jupyter otevřete toorun interaktivní Spark SQL dotazu")

   > [!NOTE]
   > Může také přístup k hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Vytvořte poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

   ![Vytvořit Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "vytvořit Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz")

   Nový poznámkový blok se vytvoří a otevřít s názvem hello Untitled(Untitled.pynb).

4. Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název, pokud chcete.

    ![Zadejte název pro hello Jupter poznámkového bloku toorun interaktivní Spark dotaz z](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "zadejte název pro hello Jupter poznámkového bloku toorun interaktivní Spark dotaz z")

5.  Následující hello vložení kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER** toorun hello kódu. V následující kód hello `%%sql` (magic sql volané hello) informuje přednastavení hello toouse poznámkového bloku Jupyter `sqlContext` dotaz Hive toorun hello. Hello dotaz načte hello horních 10 řádků z tabulky Hive (**hivesampletable**), je k dispozici ve výchozím nastavení na všech clusterech HDInsight.

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    ![Dotaz Hive v HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Dotaz Hive v HDInsight Spark")

    Další informace o hello `%%sql` magic a hello přednastavení kontexty najdete v tématu [Jupyter jádra dostupná pro cluster služby HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

    > [!NOTE]
    > Při každém spuštění dotazu v Jupyter se název okna webového prohlížeče ukazuje **(zaneprázdněn)** společně s názvem poznámkového bloku hello. Zobrazí také další toohello plný kroužek **PySpark** text v pravém horním rohu hello. Po dokončení úlohy hello změní tooa prázdný kruh.
    >
    >
    
6. úvodní obrazovka by měl aktualizovat tooshow hello dotazu výstup.

    ![Výstup dotazu Hive v HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Výstup dotazu Hive v HDInsight Spark")

7. Po dokončení spuštění aplikace hello vypněte prostředky clusteru hello poznámkového bloku toorelease hello. toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.

8. Pokud máte v plánu další kroky toocomplete hello později, nezapomeňte že odstranit hello clusteru HDInsight, kterou jste vytvořili v tomto článku. 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Další krok 

V tomto článku jste se dozvěděli, jak toocreate HDInsight Spark clusteru a spuštění základní Spark SQL dotazu. Posunutí toohello další článek toolearn jak toouse HDInsight Spark clusteru toorun interaktivních dotazů na ukázková data.

> [!div class="nextstepaction"]
>[Spouštění interaktivních dotazů v clusteru HDInsight Spark.](hdinsight-apache-spark-load-data-run-query.md)



