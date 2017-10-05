---
title: "Vytvoření clusteru Apache Spark ve službě Azure HDInsight | Dokumentace Microsoftu"
description: "Rychlý start k HDInsight Spark týkající se vytvoření clusteru Apache Spark ve službě HDInsight."
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
ms.openlocfilehash: ad4330a1fc7f8de154d9aaa8df3acc2ab59b9dc1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a><span data-ttu-id="74d46-104">Vytvoření clusteru Apache Spark ve službě Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="74d46-104">Create an Apache Spark cluster in Azure HDInsight</span></span>

<span data-ttu-id="74d46-105">V tomto článku se dozvíte, jak vytvořit cluster Apache Spark ve službě Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="74d46-105">In this article, you learn how to create an Apache Spark cluster in Azure HDInsight.</span></span> <span data-ttu-id="74d46-106">Informace o Apache Spark ve službě HDInsight najdete v tématu [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="74d46-106">For information on Spark on HDInsight, see [Overview: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md).</span></span>

   <span data-ttu-id="74d46-107">![Diagram rychlého startu popisující postup vytvoření clusteru Apache Spark ve službě Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Rychlý start k použití Apache Spark ve službě HDInsight. Popsané postupy: vytvoření clusteru, spuštění interaktivního dotazu Spark")</span><span class="sxs-lookup"><span data-stu-id="74d46-107">![Quickstart diagram describing steps to create an Apache Spark cluster on Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark quickstart using Apache Spark in HDInsight. Steps illustrated: create a cluster; run Spark interactive query")</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74d46-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="74d46-108">Prerequisites</span></span>

* <span data-ttu-id="74d46-109">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="74d46-109">**An Azure subscription**.</span></span> <span data-ttu-id="74d46-110">Než začnete tento kurz, musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="74d46-110">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="74d46-111">Přečtěte si téma [Bezplatné vytvoření účtu Microsoft Azure ještě dnes](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="74d46-111">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

## <a name="create-hdinsight-spark-cluster"></a><span data-ttu-id="74d46-112">Vytvoření clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="74d46-112">Create HDInsight Spark cluster</span></span>

<span data-ttu-id="74d46-113">V této části vytvoříte cluster HDInsight Spark pomocí [šablony Azure Resource Manageru](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span><span class="sxs-lookup"><span data-stu-id="74d46-113">In this section, you create an HDInsight Spark cluster using an [Azure Resource Manager template](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span></span> <span data-ttu-id="74d46-114">Ostatní metody tvorby clusteru najdete v části [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="74d46-114">For other cluster creation methods, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="74d46-115">Kliknutím na následující obrázek otevřete šablonu na portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="74d46-115">Click the following image to open the template in the Azure portal.</span></span>         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. <span data-ttu-id="74d46-116">Zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="74d46-116">Enter the following values:</span></span>

    <span data-ttu-id="74d46-117">![Vytvoření clusteru HDInsight Spark pomocí šablony Azure Resource Manageru](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Vytvoření clusteru Spark ve službě HDInsight pomocí šablony Azure Resource Manageru")</span><span class="sxs-lookup"><span data-stu-id="74d46-117">![Create HDInsight Spark cluster using an Azure Resource Manager template](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Create Spark cluster in HDInsight using an Azure Resource Manager template")</span></span>

    * <span data-ttu-id="74d46-118">**Předplatné:** Vyberte předplatné Azure pro tento cluster.</span><span class="sxs-lookup"><span data-stu-id="74d46-118">**Subscription**: Select your Azure subscription for this cluster.</span></span>
    * <span data-ttu-id="74d46-119">**Skupina prostředků:** Vytvořte skupinu prostředků nebo vyberte stávající.</span><span class="sxs-lookup"><span data-stu-id="74d46-119">**Resource group**: Create a resource group or select an existing one.</span></span> <span data-ttu-id="74d46-120">Skupina prostředků slouží ke správě prostředků Azure pro vaše projekty.</span><span class="sxs-lookup"><span data-stu-id="74d46-120">Resource group is used to manage Azure resources for your projects.</span></span>
    * <span data-ttu-id="74d46-121">**Umístění:** Vyberte umístění pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="74d46-121">**Location**: Select a location for the resource group.</span></span> <span data-ttu-id="74d46-122">Šablona toto umístění používá k vytvoření clusteru i jako výchozí úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="74d46-122">The template uses this location for creating the cluster as well as for the default cluster storage.</span></span>
    * <span data-ttu-id="74d46-123">**Název clusteru:** Zadejte název pro cluster HDInsight, který chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="74d46-123">**ClusterName**: Enter a name for the HDInsight cluster that you want to create.</span></span>
    * <span data-ttu-id="74d46-124">**Verze Sparku:** Jako verzi, kterou chcete nainstalovat v clusteru, vyberte **2.0**.</span><span class="sxs-lookup"><span data-stu-id="74d46-124">**Spark version**: Select **2.0** as the version that you want to install on the cluster.</span></span>
    * <span data-ttu-id="74d46-125">**Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je admin.</span><span class="sxs-lookup"><span data-stu-id="74d46-125">**Cluster login name and password**: The default login name is admin.</span></span>
    * <span data-ttu-id="74d46-126">**Uživatelské jméno a heslo SSH**.</span><span class="sxs-lookup"><span data-stu-id="74d46-126">**SSH user name and password**.</span></span>

   <span data-ttu-id="74d46-127">Zapište si tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="74d46-127">Write down these values.</span></span>  <span data-ttu-id="74d46-128">Budete je potřebovat později v kurzu.</span><span class="sxs-lookup"><span data-stu-id="74d46-128">You need them later in the tutorial.</span></span>

3. <span data-ttu-id="74d46-129">Vyberte **Souhlasím s podmínkami a ujednáními uvedenými nahoře**, vyberte **Připnout na řídicí panel** a potom klikněte na **Koupit**.</span><span class="sxs-lookup"><span data-stu-id="74d46-129">Select **I agree to the terms and conditions stated above**, select **Pin to dashboard**, and then click **Purchase**.</span></span> <span data-ttu-id="74d46-130">Zobrazí se nová dlaždice s názvem Odeslání nasazení pro šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="74d46-130">You can see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="74d46-131">Vytvoření clusteru trvá přibližně 20 minut.</span><span class="sxs-lookup"><span data-stu-id="74d46-131">It takes about 20 minutes to create the cluster.</span></span>

<span data-ttu-id="74d46-132">Pokud narazíte na problém s vytvářením clusterů HDInsight, může to být způsobeno tím, že k tomu nemáte správná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="74d46-132">If you run into an issue with creating HDInsight clusters, it could be that you do not have the right permissions to do so.</span></span> <span data-ttu-id="74d46-133">Další informace najdete v tématu popisujícím [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="74d46-133">See [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="74d46-134">Tento článek vytvoří cluster Spark, který využívá [Azure Storage Blob jako úložiště clusteru](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="74d46-134">This article creates a Spark cluster that uses [Azure Storage Blobs as the cluster storage](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="74d46-135">Můžete také vytvořit cluster Spark, který jako výchozí úložiště používá [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="74d46-135">You can also create a Spark cluster that uses [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) as the default storage.</span></span> <span data-ttu-id="74d46-136">Pokyny naleznete v tématu [Vytvoření clusteru HDInsight pomocí Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="74d46-136">For instructions, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>
>
>

## <a name="run-a-hive-query-using-spark-sql"></a><span data-ttu-id="74d46-137">Spuštění dotazu Hive pomocí Spark SQL</span><span class="sxs-lookup"><span data-stu-id="74d46-137">Run a Hive query using Spark SQL</span></span>

<span data-ttu-id="74d46-138">Pokud používáte poznámkový blok Jupyter nakonfigurovaný pro váš cluster HDInsight Spark, získáte přednastavený kontext `sqlContext`, který můžete použít ke spouštění dotazů Hive pomocí Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="74d46-138">When you use a Jupyter notebook configured for your HDInsight Spark cluster, you get a preset `sqlContext` that you can use to run Hive queries using Spark SQL.</span></span> <span data-ttu-id="74d46-139">V této části zjistíte, jak spustit poznámkový blok Jupyter a potom spustit základní dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="74d46-139">In this section, you learn how to start a Jupyter notebook and then run a basic Hive query.</span></span>

1. <span data-ttu-id="74d46-140">Otevřete web [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="74d46-140">Open the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="74d46-141">Pokud jste se rozhodli připnout cluster na řídicí panel, kliknutím na dlaždici clusteru na řídicím panelu spusťte okno clusteru.</span><span class="sxs-lookup"><span data-stu-id="74d46-141">If you opted to pin the cluster to the dashboard, click the cluster tile from the dashboard to launch the cluster blade.</span></span>

    <span data-ttu-id="74d46-142">Pokud jste cluster na řídicí panel nepřipnuli, v levém podokně klikněte na **Clustery HDInsight** a pak klikněte na cluster, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="74d46-142">If you did not pin the cluster to the dashboard, from the left pane, click **HDInsight clusters**, and then click the cluster you created.</span></span>

3. <span data-ttu-id="74d46-143">V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="74d46-143">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="74d46-144">Po vyzvání zadejte přihlašovací údaje správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="74d46-144">If prompted, enter the admin credentials for the cluster.</span></span>

   <span data-ttu-id="74d46-145">![Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")</span><span class="sxs-lookup"><span data-stu-id="74d46-145">![Open Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook to run interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="74d46-146">K poznámkovému bloku Jupyter pro váš cluster se dostanete také otevřením následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="74d46-146">You may also access the Jupyter notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="74d46-147">Nahraďte **CLUSTERNAME** názvem clusteru:</span><span class="sxs-lookup"><span data-stu-id="74d46-147">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="74d46-148">Vytvořte poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="74d46-148">Create a notebook.</span></span> <span data-ttu-id="74d46-149">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="74d46-149">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="74d46-150">![Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")</span><span class="sxs-lookup"><span data-stu-id="74d46-150">![Create a Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook to run interactive Spark SQL query")</span></span>

   <span data-ttu-id="74d46-151">Nový poznámkový blok se vytvoří a otevře s názvem Bez názvu (Bez názvu.pynb).</span><span class="sxs-lookup"><span data-stu-id="74d46-151">A new notebook is created and opened with the name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="74d46-152">Pokud chcete, klikněte na název poznámkového bloku v horní části a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="74d46-152">Click the notebook name at the top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="74d46-153">![Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark")</span><span class="sxs-lookup"><span data-stu-id="74d46-153">![Provide a name for the Jupter notebook to run interactive Spark query from](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Provide a name for the Jupter notebook to run interactive Spark query from")</span></span>

5.  <span data-ttu-id="74d46-154">Do prázdné buňky vložte následující kód a stisknutím **SHIFT + ENTER** kód spusťte.</span><span class="sxs-lookup"><span data-stu-id="74d46-154">Paste the following code in an empty cell, and then press **SHIFT + ENTER** to run the code.</span></span> <span data-ttu-id="74d46-155">V následujícím kódu příkaz `%%sql` (označovaný jako magický příkaz jazyka SQL) říká poznámkovému bloku Jupyter, aby ke spuštění dotazu Hive použil přednastavený kontext `sqlContext`.</span><span class="sxs-lookup"><span data-stu-id="74d46-155">In the code below `%%sql` (called the sql magic) tells Jupyter notebook to use the preset `sqlContext` to run the Hive query.</span></span> <span data-ttu-id="74d46-156">Dotaz načte prvních 10 řádků z tabulky Hive (**hivesampletable**), která je ve výchozím nastavení k dispozici na všech clusterech HDInsight.</span><span class="sxs-lookup"><span data-stu-id="74d46-156">The query retrieves the top 10 rows from a Hive table (**hivesampletable**) that is available by default on all HDInsight clusters.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    <span data-ttu-id="74d46-157">![Dotaz Hive v HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Dotaz Hive v HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="74d46-157">![Hive query in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive query in HDInsight Spark")</span></span>

    <span data-ttu-id="74d46-158">Další informace o magickém příkazu `%%sql` a přednastavených kontextech najdete v tématu [Dostupná jádra Jupyter pro cluster HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="74d46-158">For more information on the `%%sql` magic and the preset contexts, see [Jupyter kernels available for an HDInsight cluster](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="74d46-159">Při každém spuštění dotazu v Jupyter se v názvu okna webového prohlížeče zobrazí stav **(Busy)** (Zaneprázdněn) společně s názvem poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="74d46-159">Every time you run a query in Jupyter, your web browser window title shows a **(Busy)** status along with the notebook title.</span></span> <span data-ttu-id="74d46-160">Zobrazí se také plný kroužek vedle textu **PySpark** v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="74d46-160">You also see a solid circle next to the **PySpark** text in the top-right corner.</span></span> <span data-ttu-id="74d46-161">Po dokončení úlohy se změní na prázdný kruh.</span><span class="sxs-lookup"><span data-stu-id="74d46-161">After the job is completed, it changes to a hollow circle.</span></span>
    >
    >
    
6. <span data-ttu-id="74d46-162">Obrazovka by se měla aktualizovat a zobrazit výstup dotazu.</span><span class="sxs-lookup"><span data-stu-id="74d46-162">The screen should refresh to show the query output.</span></span>

    <span data-ttu-id="74d46-163">![Výstup dotazu Hive v HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Výstup dotazu Hive v HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="74d46-163">![Hive query output in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive query output in HDInsight Spark")</span></span>

7. <span data-ttu-id="74d46-164">Po dokončení spuštění aplikace můžete poznámkový blok vypnout a uvolnit tak prostředky clusteru.</span><span class="sxs-lookup"><span data-stu-id="74d46-164">Shut down the notebook to release the cluster resources after you have finished running the application.</span></span> <span data-ttu-id="74d46-165">To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="74d46-165">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span>

8. <span data-ttu-id="74d46-166">Pokud se chystáte další kroky dokončit později, nezapomeňte odstranit cluster HDInsight, který jste vytvořili v rámci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="74d46-166">If you plan to complete the next steps at a later time, make sure you delete the HDInsight cluster you created in this article.</span></span> 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a><span data-ttu-id="74d46-167">Další krok</span><span class="sxs-lookup"><span data-stu-id="74d46-167">Next step</span></span> 

<span data-ttu-id="74d46-168">V tomto článku jste zjistili, jak vytvořit cluster HDInsight Spark a spustit základní dotaz Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="74d46-168">In this article you learned how to create an HDInsight Spark cluster and run a basic Spark SQL query.</span></span> <span data-ttu-id="74d46-169">Přejděte k dalšímu článku a zjistěte, jak pomocí clusteru HDInsight Spark spouštět interaktivní dotazy na ukázková data.</span><span class="sxs-lookup"><span data-stu-id="74d46-169">Advance to the next article to learn how to use an HDInsight Spark cluster to run interactive queries on sample data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="74d46-170">Spouštění interaktivních dotazů v clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="74d46-170">Run interactive queries on an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-load-data-run-query.md)



