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
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a><span data-ttu-id="5252a-104">Vytvoření clusteru Apache Spark ve službě Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="5252a-104">Create an Apache Spark cluster in Azure HDInsight</span></span>

<span data-ttu-id="5252a-105">V tomto článku se dozvíte, jak clusteru toocreate Apache Spark v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5252a-105">In this article, you learn how toocreate an Apache Spark cluster in Azure HDInsight.</span></span> <span data-ttu-id="5252a-106">Informace o Apache Spark ve službě HDInsight najdete v tématu [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5252a-106">For information on Spark on HDInsight, see [Overview: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md).</span></span>

   <span data-ttu-id="5252a-107">![Rychlý start diagramu popisující kroky toocreate cluster Apache Spark v Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "rychlý start Spark pomocí Apache Spark v HDInsight. Popsané postupy: vytvoření clusteru, spuštění interaktivního dotazu Spark")</span><span class="sxs-lookup"><span data-stu-id="5252a-107">![Quickstart diagram describing steps toocreate an Apache Spark cluster on Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark quickstart using Apache Spark in HDInsight. Steps illustrated: create a cluster; run Spark interactive query")</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5252a-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5252a-108">Prerequisites</span></span>

* <span data-ttu-id="5252a-109">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="5252a-109">**An Azure subscription**.</span></span> <span data-ttu-id="5252a-110">Než začnete tento kurz, musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5252a-110">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="5252a-111">Přečtěte si téma [Bezplatné vytvoření účtu Microsoft Azure ještě dnes](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="5252a-111">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

## <a name="create-hdinsight-spark-cluster"></a><span data-ttu-id="5252a-112">Vytvoření clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="5252a-112">Create HDInsight Spark cluster</span></span>

<span data-ttu-id="5252a-113">V této části vytvoříte cluster HDInsight Spark pomocí [šablony Azure Resource Manageru](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span><span class="sxs-lookup"><span data-stu-id="5252a-113">In this section, you create an HDInsight Spark cluster using an [Azure Resource Manager template](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span></span> <span data-ttu-id="5252a-114">Ostatní metody tvorby clusteru najdete v části [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="5252a-114">For other cluster creation methods, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="5252a-115">Klikněte na tlačítko hello následující šablony hello tooopen bitové kopie v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5252a-115">Click hello following image tooopen hello template in hello Azure portal.</span></span>         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. <span data-ttu-id="5252a-116">Zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5252a-116">Enter hello following values:</span></span>

    <span data-ttu-id="5252a-117">![Vytvoření clusteru HDInsight Spark pomocí šablony Azure Resource Manageru](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Vytvoření clusteru Spark ve službě HDInsight pomocí šablony Azure Resource Manageru")</span><span class="sxs-lookup"><span data-stu-id="5252a-117">![Create HDInsight Spark cluster using an Azure Resource Manager template](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Create Spark cluster in HDInsight using an Azure Resource Manager template")</span></span>

    * <span data-ttu-id="5252a-118">**Předplatné:** Vyberte předplatné Azure pro tento cluster.</span><span class="sxs-lookup"><span data-stu-id="5252a-118">**Subscription**: Select your Azure subscription for this cluster.</span></span>
    * <span data-ttu-id="5252a-119">**Skupina prostředků:** Vytvořte skupinu prostředků nebo vyberte stávající.</span><span class="sxs-lookup"><span data-stu-id="5252a-119">**Resource group**: Create a resource group or select an existing one.</span></span> <span data-ttu-id="5252a-120">Skupina prostředků je použité toomanage prostředků Azure pro projekty.</span><span class="sxs-lookup"><span data-stu-id="5252a-120">Resource group is used toomanage Azure resources for your projects.</span></span>
    * <span data-ttu-id="5252a-121">**Umístění**: Vyberte umístění pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="5252a-121">**Location**: Select a location for hello resource group.</span></span> <span data-ttu-id="5252a-122">Šablona Hello používá toto umístění pro vytvoření clusteru hello stejně jako u hello výchozí clusteru úložiště.</span><span class="sxs-lookup"><span data-stu-id="5252a-122">hello template uses this location for creating hello cluster as well as for hello default cluster storage.</span></span>
    * <span data-ttu-id="5252a-123">**Název clusteru**: Zadejte název pro cluster HDInsight hello, které chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="5252a-123">**ClusterName**: Enter a name for hello HDInsight cluster that you want toocreate.</span></span>
    * <span data-ttu-id="5252a-124">**Spark verze**: vyberte **2.0** jako hello verze, kterou chcete tooinstall v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5252a-124">**Spark version**: Select **2.0** as hello version that you want tooinstall on hello cluster.</span></span>
    * <span data-ttu-id="5252a-125">**Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je admin.</span><span class="sxs-lookup"><span data-stu-id="5252a-125">**Cluster login name and password**: hello default login name is admin.</span></span>
    * <span data-ttu-id="5252a-126">**Uživatelské jméno a heslo SSH**.</span><span class="sxs-lookup"><span data-stu-id="5252a-126">**SSH user name and password**.</span></span>

   <span data-ttu-id="5252a-127">Zapište si tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5252a-127">Write down these values.</span></span>  <span data-ttu-id="5252a-128">Budete potřebovat později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="5252a-128">You need them later in hello tutorial.</span></span>

3. <span data-ttu-id="5252a-129">Vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**, vyberte **Pin toodashboard**a potom klikněte na **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="5252a-129">Select **I agree toohello terms and conditions stated above**, select **Pin toodashboard**, and then click **Purchase**.</span></span> <span data-ttu-id="5252a-130">Zobrazí se nová dlaždice s názvem Odeslání nasazení pro šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="5252a-130">You can see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="5252a-131">Jak dlouho trvá přibližně 20 minut toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="5252a-131">It takes about 20 minutes toocreate hello cluster.</span></span>

<span data-ttu-id="5252a-132">Pokud narazíte na problém s vytváření clusterů HDInsight, může to být hello správná oprávnění toodo proto nemají.</span><span class="sxs-lookup"><span data-stu-id="5252a-132">If you run into an issue with creating HDInsight clusters, it could be that you do not have hello right permissions toodo so.</span></span> <span data-ttu-id="5252a-133">Další informace najdete v tématu popisujícím [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="5252a-133">See [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="5252a-134">V tomto článku vytváří cluster Spark, který používá [clusteru úložiště objektů BLOB služby Azure Storage jako hello](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="5252a-134">This article creates a Spark cluster that uses [Azure Storage Blobs as hello cluster storage](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="5252a-135">Můžete také vytvořit cluster Spark, která používá [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) jako hello výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="5252a-135">You can also create a Spark cluster that uses [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) as hello default storage.</span></span> <span data-ttu-id="5252a-136">Pokyny naleznete v tématu [Vytvoření clusteru HDInsight pomocí Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5252a-136">For instructions, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>
>
>

## <a name="run-a-hive-query-using-spark-sql"></a><span data-ttu-id="5252a-137">Spuštění dotazu Hive pomocí Spark SQL</span><span class="sxs-lookup"><span data-stu-id="5252a-137">Run a Hive query using Spark SQL</span></span>

<span data-ttu-id="5252a-138">Když použijete poznámkového bloku Jupyter nakonfigurované pro váš cluster HDInsight Spark, zobrazí jedno z přednastavení `sqlContext` , které můžete použít dotazy Hive toorun pomocí Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="5252a-138">When you use a Jupyter notebook configured for your HDInsight Spark cluster, you get a preset `sqlContext` that you can use toorun Hive queries using Spark SQL.</span></span> <span data-ttu-id="5252a-139">V této části se dozvíte, jak toostart poznámkového bloku Jupyter a poté spusťte základního dotazu Hive.</span><span class="sxs-lookup"><span data-stu-id="5252a-139">In this section, you learn how toostart a Jupyter notebook and then run a basic Hive query.</span></span>

1. <span data-ttu-id="5252a-140">Otevřete hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5252a-140">Open hello [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="5252a-141">Když jste se rozhodli řídicí panel toohello toopin hello clusteru, klikněte na dlaždici hello clusteru z hello řídicí panel toolaunch hello clusteru okno.</span><span class="sxs-lookup"><span data-stu-id="5252a-141">If you opted toopin hello cluster toohello dashboard, click hello cluster tile from hello dashboard toolaunch hello cluster blade.</span></span>

    <span data-ttu-id="5252a-142">Pokud připnete není hello toohello řídicí panel clusteru, v levém podokně hello, klikněte na tlačítko **clustery HDInsight**a potom klikněte na cluster hello jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="5252a-142">If you did not pin hello cluster toohello dashboard, from hello left pane, click **HDInsight clusters**, and then click hello cluster you created.</span></span>

3. <span data-ttu-id="5252a-143">V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="5252a-143">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="5252a-144">Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="5252a-144">If prompted, enter hello admin credentials for hello cluster.</span></span>

   <span data-ttu-id="5252a-145">![Otevřete Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "poznámkového bloku Jupyter otevřete toorun interaktivní Spark SQL dotazu")</span><span class="sxs-lookup"><span data-stu-id="5252a-145">![Open Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook toorun interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="5252a-146">Může také přístup k hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="5252a-146">You may also access hello Jupyter notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="5252a-147">Nahraďte **CLUSTERNAME** s hello název clusteru:</span><span class="sxs-lookup"><span data-stu-id="5252a-147">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="5252a-148">Vytvořte poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="5252a-148">Create a notebook.</span></span> <span data-ttu-id="5252a-149">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="5252a-149">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="5252a-150">![Vytvořit Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "vytvořit Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz")</span><span class="sxs-lookup"><span data-stu-id="5252a-150">![Create a Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook toorun interactive Spark SQL query")</span></span>

   <span data-ttu-id="5252a-151">Nový poznámkový blok se vytvoří a otevřít s názvem hello Untitled(Untitled.pynb).</span><span class="sxs-lookup"><span data-stu-id="5252a-151">A new notebook is created and opened with hello name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="5252a-152">Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="5252a-152">Click hello notebook name at hello top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="5252a-153">![Zadejte název pro hello Jupter poznámkového bloku toorun interaktivní Spark dotaz z](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "zadejte název pro hello Jupter poznámkového bloku toorun interaktivní Spark dotaz z")</span><span class="sxs-lookup"><span data-stu-id="5252a-153">![Provide a name for hello Jupter notebook toorun interactive Spark query from](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Provide a name for hello Jupter notebook toorun interactive Spark query from")</span></span>

5.  <span data-ttu-id="5252a-154">Následující hello vložení kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER** toorun hello kódu.</span><span class="sxs-lookup"><span data-stu-id="5252a-154">Paste hello following code in an empty cell, and then press **SHIFT + ENTER** toorun hello code.</span></span> <span data-ttu-id="5252a-155">V následující kód hello `%%sql` (magic sql volané hello) informuje přednastavení hello toouse poznámkového bloku Jupyter `sqlContext` dotaz Hive toorun hello.</span><span class="sxs-lookup"><span data-stu-id="5252a-155">In hello code below `%%sql` (called hello sql magic) tells Jupyter notebook toouse hello preset `sqlContext` toorun hello Hive query.</span></span> <span data-ttu-id="5252a-156">Hello dotaz načte hello horních 10 řádků z tabulky Hive (**hivesampletable**), je k dispozici ve výchozím nastavení na všech clusterech HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5252a-156">hello query retrieves hello top 10 rows from a Hive table (**hivesampletable**) that is available by default on all HDInsight clusters.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    <span data-ttu-id="5252a-157">![Dotaz Hive v HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Dotaz Hive v HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="5252a-157">![Hive query in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive query in HDInsight Spark")</span></span>

    <span data-ttu-id="5252a-158">Další informace o hello `%%sql` magic a hello přednastavení kontexty najdete v tématu [Jupyter jádra dostupná pro cluster služby HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="5252a-158">For more information on hello `%%sql` magic and hello preset contexts, see [Jupyter kernels available for an HDInsight cluster](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="5252a-159">Při každém spuštění dotazu v Jupyter se název okna webového prohlížeče ukazuje **(zaneprázdněn)** společně s názvem poznámkového bloku hello.</span><span class="sxs-lookup"><span data-stu-id="5252a-159">Every time you run a query in Jupyter, your web browser window title shows a **(Busy)** status along with hello notebook title.</span></span> <span data-ttu-id="5252a-160">Zobrazí také další toohello plný kroužek **PySpark** text v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="5252a-160">You also see a solid circle next toohello **PySpark** text in hello top-right corner.</span></span> <span data-ttu-id="5252a-161">Po dokončení úlohy hello změní tooa prázdný kruh.</span><span class="sxs-lookup"><span data-stu-id="5252a-161">After hello job is completed, it changes tooa hollow circle.</span></span>
    >
    >
    
6. <span data-ttu-id="5252a-162">úvodní obrazovka by měl aktualizovat tooshow hello dotazu výstup.</span><span class="sxs-lookup"><span data-stu-id="5252a-162">hello screen should refresh tooshow hello query output.</span></span>

    <span data-ttu-id="5252a-163">![Výstup dotazu Hive v HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Výstup dotazu Hive v HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="5252a-163">![Hive query output in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive query output in HDInsight Spark")</span></span>

7. <span data-ttu-id="5252a-164">Po dokončení spuštění aplikace hello vypněte prostředky clusteru hello poznámkového bloku toorelease hello.</span><span class="sxs-lookup"><span data-stu-id="5252a-164">Shut down hello notebook toorelease hello cluster resources after you have finished running hello application.</span></span> <span data-ttu-id="5252a-165">toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.</span><span class="sxs-lookup"><span data-stu-id="5252a-165">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span>

8. <span data-ttu-id="5252a-166">Pokud máte v plánu další kroky toocomplete hello později, nezapomeňte že odstranit hello clusteru HDInsight, kterou jste vytvořili v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5252a-166">If you plan toocomplete hello next steps at a later time, make sure you delete hello HDInsight cluster you created in this article.</span></span> 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a><span data-ttu-id="5252a-167">Další krok</span><span class="sxs-lookup"><span data-stu-id="5252a-167">Next step</span></span> 

<span data-ttu-id="5252a-168">V tomto článku jste se dozvěděli, jak toocreate HDInsight Spark clusteru a spuštění základní Spark SQL dotazu.</span><span class="sxs-lookup"><span data-stu-id="5252a-168">In this article you learned how toocreate an HDInsight Spark cluster and run a basic Spark SQL query.</span></span> <span data-ttu-id="5252a-169">Posunutí toohello další článek toolearn jak toouse HDInsight Spark clusteru toorun interaktivních dotazů na ukázková data.</span><span class="sxs-lookup"><span data-stu-id="5252a-169">Advance toohello next article toolearn how toouse an HDInsight Spark cluster toorun interactive queries on sample data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="5252a-170">Spouštění interaktivních dotazů v clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="5252a-170">Run interactive queries on an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-load-data-run-query.md)



