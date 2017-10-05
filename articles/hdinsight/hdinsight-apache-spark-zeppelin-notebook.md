---
title: "Použití poznámkových bloků Zeppelin s clusterem Apache Spark v Azure HDInsight | Microsoft Docs"
description: "Podrobné pokyny o tom, jak používat poznámkových bloků Zeppelin s clustery Apache Spark v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 7fe5e3ec68e82945b972d2dd44f2cc3b8cf395d1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a><span data-ttu-id="1a0a6-103">Použití poznámkových bloků Zeppelin s clusterem Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="1a0a6-103">Use Zeppelin notebooks with Apache Spark cluster on Azure HDInsight</span></span>

<span data-ttu-id="1a0a6-104">Clustery HDInsight Spark zahrnují poznámkových bloků Zeppelin, které můžete použít ke spuštění úloh Spark.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-104">HDInsight Spark clusters include Zeppelin notebooks that you can use to run Spark jobs.</span></span> <span data-ttu-id="1a0a6-105">V tomto článku zjistěte, jak používat Zeppelin poznámkového bloku na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-105">In this article, you learn how to use the Zeppelin notebook on an HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="1a0a6-106">Poznámkových bloků Zeppelin jsou dostupné pouze pro 1.6.3 Spark v HDInsight 3.5 a 2.1.0 Spark v HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-106">Zeppelin notebooks are available only for Spark 1.6.3 on HDInsight 3.5 and Spark 2.1.0 on HDInsight 3.6.</span></span>
>

<span data-ttu-id="1a0a6-107">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="1a0a6-107">**Prerequisites:**</span></span>

* <span data-ttu-id="1a0a6-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-108">An Azure subscription.</span></span> <span data-ttu-id="1a0a6-109">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="1a0a6-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="1a0a6-110">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-110">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="1a0a6-111">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="1a0a6-111">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="launch-a-zeppelin-notebook"></a><span data-ttu-id="1a0a6-112">Spusťte Zeppelin Poznámkový blok</span><span class="sxs-lookup"><span data-stu-id="1a0a6-112">Launch a Zeppelin notebook</span></span>
1. <span data-ttu-id="1a0a6-113">Z okna clusteru Spark klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Zeppelin Poznámkový blok**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-113">From the Spark cluster blade, click **Cluster Dashboard**, and then click **Zeppelin Notebook**.</span></span> <span data-ttu-id="1a0a6-114">Po vyzvání zadejte přihlašovací údaje správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-114">If prompted, enter the admin credentials for the cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1a0a6-115">Může také dosáhnout Zeppelin Poznámkový blok pro váš cluster tak, že otevřete následující adresu URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-115">You may also reach the Zeppelin Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="1a0a6-116">Nahraďte **CLUSTERNAME** názvem clusteru:</span><span class="sxs-lookup"><span data-stu-id="1a0a6-116">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. <span data-ttu-id="1a0a6-117">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-117">Create a new notebook.</span></span> <span data-ttu-id="1a0a6-118">V podokně záhlaví, klikněte na tlačítko **poznámkového bloku**a potom klikněte na **vytvořit novou poznámku**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-118">From the header pane, click **Notebook**, and then click **Create New Note**.</span></span>
   
    <span data-ttu-id="1a0a6-119">![Vytvořte nový poznámkový blok Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "vytvořte nový poznámkový blok Zeppelin")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-119">![Create a new Zeppelin notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Create a new Zeppelin notebook")</span></span>
   
    <span data-ttu-id="1a0a6-120">Zadejte název pro poznámkový blok a pak klikněte na tlačítko **vytvořit Poznámka**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-120">Enter a name for the notebook, and then click **Create Note**.</span></span>
3. <span data-ttu-id="1a0a6-121">Taky se ujistěte, že hlavičku poznámkového bloku zobrazí stav připojených.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-121">Also, make sure the notebook header shows a connected status.</span></span> <span data-ttu-id="1a0a6-122">Označuje zelená tečky v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-122">It is denoted by a green dot in the top-right corner.</span></span>
   
    <span data-ttu-id="1a0a6-123">![Stav poznámkového bloku Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin poznámkového bloku stav")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-123">![Zeppelin notebook status](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin notebook status")</span></span>
4. <span data-ttu-id="1a0a6-124">Načtěte vzorová data do dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-124">Load sample data into a temporary table.</span></span> <span data-ttu-id="1a0a6-125">Když vytvoříte Spark cluster v HDInsight, ukázkový datový soubor, **hvac.csv**, se zkopíruje do přidruženého účtu úložiště v rámci **\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-125">When you create a Spark cluster in HDInsight, the sample data file, **hvac.csv**, is copied to the associated storage account under **\HdiSamples\SensorSampleData\hvac**.</span></span>
   
    <span data-ttu-id="1a0a6-126">V prázdné odstavce, který se vytvoří ve výchozím nastavení v nový poznámkový blok vložte následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-126">In the empty paragraph that is created by default in the new notebook, paste the following snippet.</span></span>
   
        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter
   
        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    <span data-ttu-id="1a0a6-127">Stiskněte klávesu **SHIFT + ENTER** nebo klikněte na tlačítko **přehrání** tlačítko odstavce ke spuštění fragmentu.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-127">Press **SHIFT + ENTER** or click the **Play** button for the paragraph to run the snippet.</span></span> <span data-ttu-id="1a0a6-128">Stav na pravém horním rohu odstavce by měl průběhu z připravené, čeká na vyřízení, SPUŠTĚNÁ na DOKONČENO.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-128">The status on the right-corner of the paragraph should progress from READY, PENDING, RUNNING to FINISHED.</span></span> <span data-ttu-id="1a0a6-129">Výstup se zobrazí v dolní části stejné odstavce.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-129">The output shows up at the bottom of the same paragraph.</span></span> <span data-ttu-id="1a0a6-130">Na snímku obrazovky vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1a0a6-130">The screenshot looks like the following:</span></span>
   
    <span data-ttu-id="1a0a6-131">![Vytvořte dočasnou tabulku z nezpracovaná data](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "vytvořit dočasnou tabulku od nezpracovaných dat")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-131">![Create a temporary table from raw data](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Create a temporary table from raw data")</span></span>
   
    <span data-ttu-id="1a0a6-132">Můžete zadat také název jednotlivých odstavců.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-132">You can also provide a title to each paragraph.</span></span> <span data-ttu-id="1a0a6-133">V pravém rohu, klikněte na **nastavení** ikonu a pak klikněte na tlačítko **zobrazit nadpis**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-133">From the right-hand corner, click the **Settings** icon, and then click **Show title**.</span></span>
5. <span data-ttu-id="1a0a6-134">Teď můžete spustit příkazy Spark SQL na **TVK** tabulky.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-134">You can now run Spark SQL statements on the **hvac** table.</span></span> <span data-ttu-id="1a0a6-135">Vložte následující dotaz nový odstavec.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-135">Paste the following query in a new paragraph.</span></span> <span data-ttu-id="1a0a6-136">Dotaz načte ID budovy a rozdíl mezi cíl a skutečný teploty pro každé sestavení v určitém dni.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-136">The query retrieves the building ID and the difference between the target and actual temperatures for each building on a given date.</span></span> <span data-ttu-id="1a0a6-137">Stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-137">Press **SHIFT + ENTER**.</span></span>
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    <span data-ttu-id="1a0a6-138">**% Sql** na začátku informuje používat překladač Livy Scala poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-138">The **%sql** statement at the beginning tells the notebook to use the Livy Scala interpreter.</span></span>
   
    <span data-ttu-id="1a0a6-139">Následující snímek obrazovky ukazuje výstup.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-139">The following screenshot shows the output.</span></span>
   
    <span data-ttu-id="1a0a6-140">![Spustit příkaz Spark SQL pomocí poznámkového bloku](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "spustit příkaz Spark SQL pomocí poznámkového bloku")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-140">![Run a Spark SQL statement using the notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Run a Spark SQL statement using the notebook")</span></span>
   
     <span data-ttu-id="1a0a6-141">Klikněte na tlačítko Možnosti zobrazení (zvýrazněných v obdélníku) přepínat mezi různé reprezentace pro stejný výstup.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-141">Click the display options (highlighted in rectangle) to switch between different representations for the same output.</span></span> <span data-ttu-id="1a0a6-142">Klikněte na tlačítko **nastavení** zvolte co consitutes klíče a hodnoty ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-142">Click **Settings** to choose what consitutes the key and values in the output.</span></span> <span data-ttu-id="1a0a6-143">Výše uvedený snímek obrazovky používá **buildingID** jako klíč a průměr **temp_diff** jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-143">The screen capture above uses **buildingID** as the key and the average of **temp_diff** as the value.</span></span>
6. <span data-ttu-id="1a0a6-144">Můžete také spustit příkazy Spark SQL pomocí proměnných v dotazu.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-144">You can also run Spark SQL statements using variables in the query.</span></span> <span data-ttu-id="1a0a6-145">Následující fragment kódu ukazuje, jak k definování proměnné, **Temp**, v dotazu s možné hodnoty mají být zobrazeny s.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-145">The next snippet shows how to define a variable, **Temp**, in the query with the possible values you want to query with.</span></span> <span data-ttu-id="1a0a6-146">Při prvním spuštění dotazu, rozevírací seznam se automaticky zadá hodnoty, které jste zadali pro proměnnou.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-146">When you first run the query, a drop-down is automatically populated with the values you specified for the variable.</span></span>
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    <span data-ttu-id="1a0a6-147">Vložte tento fragment kódu nový odstavec a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-147">Paste this snippet in a new paragraph and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="1a0a6-148">Následující snímek obrazovky ukazuje výstup.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-148">The following screenshot shows the output.</span></span>
   
    <span data-ttu-id="1a0a6-149">![Spustit příkaz Spark SQL pomocí poznámkového bloku](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "spustit příkaz Spark SQL pomocí poznámkového bloku")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-149">![Run a Spark SQL statement using the notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Run a Spark SQL statement using the notebook")</span></span>
   
    <span data-ttu-id="1a0a6-150">Pro následné dotazy můžete vybrat novou hodnotu z rozevíracího seznamu a spusťte dotaz znovu.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-150">For subsequent queries, you can select a new value from the drop-down and run the query again.</span></span> <span data-ttu-id="1a0a6-151">Klikněte na tlačítko **nastavení** zvolte co consitutes klíče a hodnoty ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-151">Click **Settings** to choose what consitutes the key and values in the output.</span></span> <span data-ttu-id="1a0a6-152">Výše uvedený snímek obrazovky používá **buildingID** jako klíč průměr **temp_diff** jako hodnotu, a **targettemp** jako skupinu.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-152">The screen capture above uses **buildingID** as the key, the average of **temp_diff** as the value, and **targettemp** as the group.</span></span>
7. <span data-ttu-id="1a0a6-153">Překladač Livy ukončete aplikaci restartujte.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-153">Restart the Livy interpreter to exit the application.</span></span> <span data-ttu-id="1a0a6-154">Uděláte to tak, otevřete překladač nastavení kliknutím na příkaz přihlášeného v uživatelské jméno v pravém horním rohu a pak klikněte na tlačítko **překladač**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-154">To do so, open interpreter settings by clicking the logged in user name from the top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="1a0a6-155">![Spustí překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "výstupu podregistru")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-155">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
8. <span data-ttu-id="1a0a6-156">Přejděte na nastavení překladač Livy a pak klikněte na **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-156">Scroll to Livy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="1a0a6-157">![Restartujte Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "restartujte Zeppelin intepreter")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-157">![Restart the Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart the Zeppelin intepreter")</span></span>

## <a name="how-do-i-use-external-packages-with-the-notebook"></a><span data-ttu-id="1a0a6-158">Jak používat externí balíčky s poznámkového bloku?</span><span class="sxs-lookup"><span data-stu-id="1a0a6-158">How do I use external packages with the notebook?</span></span>
<span data-ttu-id="1a0a6-159">Můžete nakonfigurovat Zeppelin poznámkového bloku v clusteru Apache Spark v HDInsight (Linux) používat externí, komunity podílí balíčky, které nejsou zahrnuté out-of-the-box v clusteru.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-159">You can configure the Zeppelin notebook in Apache Spark cluster on HDInsight (Linux) to use external, community-contributed packages that are not included out-of-the-box in the cluster.</span></span> <span data-ttu-id="1a0a6-160">Můžete hledat [Maven úložiště](http://search.maven.org/) úplný seznam balíčků, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-160">You can search the [Maven repository](http://search.maven.org/) for the complete list of packages that are available.</span></span> <span data-ttu-id="1a0a6-161">Seznam dostupných balíčků můžete také získat z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-161">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="1a0a6-162">Například je k dispozici úplný seznam balíčků podílí komunity [Spark balíčky](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="1a0a6-162">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="1a0a6-163">V tomto článku, zobrazí se postup použití [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) balíček s poznámkovým blokem Jupyter.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-163">In this article, you will see how to use the [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with the Jupyter notebook.</span></span>

1. <span data-ttu-id="1a0a6-164">Otevřete nastavení překladač.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-164">Open interpreter settings.</span></span> <span data-ttu-id="1a0a6-165">V pravém horním rohu klikněte přihlášeného v uživatelské jméno a potom klikněte na **překladač**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-165">From the top-right corner, click the logged in user name, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="1a0a6-166">![Spustí překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "výstupu podregistru")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-166">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="1a0a6-167">Přejděte na nastavení překladač Livy a pak klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-167">Scroll to Livy interpreter settings and then click **Edit**.</span></span>
   
    <span data-ttu-id="1a0a6-168">![Změna nastavení překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "změnit nastavení překladač")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-168">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Change interpreter settings")</span></span>
3. <span data-ttu-id="1a0a6-169">Přidejte nový klíč, nazývá **livy.spark.jars.packages** a nastavení jeho hodnoty ve formátu `group:id:version`.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-169">Add a new key, called **livy.spark.jars.packages** and set its value in the format `group:id:version`.</span></span> <span data-ttu-id="1a0a6-170">Takže pokud chcete použít [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) balíčku, je nutné nastavit hodnotu klíče, který se `com.databricks:spark-csv_2.10:1.4.0`.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-170">So, if you want to use the [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package, you must set the value of the key to `com.databricks:spark-csv_2.10:1.4.0`.</span></span>
   
    <span data-ttu-id="1a0a6-171">![Změna nastavení překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "změnit nastavení překladač")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-171">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Change interpreter settings")</span></span>
   
    <span data-ttu-id="1a0a6-172">Klikněte na tlačítko **Uložit** a pak restartujte Livy překladač.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-172">Click **Save** and then restart the Livy interpreter.</span></span>
4. <span data-ttu-id="1a0a6-173">**Tip**: Pokud chcete pochopit, jak přijaty ve výše uvedených hodnotu klíče, zde je způsob.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-173">**Tip**: If you want to understand how to arrive at the value of the key entered above, here's how.</span></span>
   
    <span data-ttu-id="1a0a6-174">a.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-174">a.</span></span> <span data-ttu-id="1a0a6-175">Najděte balíček v úložišti Maven.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-175">Locate the package in the Maven Repository.</span></span> <span data-ttu-id="1a0a6-176">V tomto kurzu jsme použili [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="1a0a6-176">For this tutorial, we used [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="1a0a6-177">b.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-177">b.</span></span> <span data-ttu-id="1a0a6-178">Z úložiště, shromážděte hodnoty **GroupId**, **ArtifactId**, a **verze**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-178">From the repository, gather the values for **GroupId**, **ArtifactId**, and **Version**.</span></span>
   
    <span data-ttu-id="1a0a6-179">![Použijte externí balíčky s Poznámkový blok Jupyter](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "použijte externí balíčky s poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-179">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="1a0a6-180">c.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-180">c.</span></span> <span data-ttu-id="1a0a6-181">Řetězení tři hodnoty oddělené dvojtečkou (**:**).</span><span class="sxs-lookup"><span data-stu-id="1a0a6-181">Concatenate the three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a><span data-ttu-id="1a0a6-182">Kam se ukládají poznámkových bloků Zeppelin?</span><span class="sxs-lookup"><span data-stu-id="1a0a6-182">Where are the Zeppelin notebooks saved?</span></span>
<span data-ttu-id="1a0a6-183">Poznámkových bloků Zeppelin se uloží do headnodes clusteru.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-183">The Zeppelin notebooks are saved to the cluster headnodes.</span></span> <span data-ttu-id="1a0a6-184">Takže pokud odstranění clusteru, se odstraní také poznámkových bloků.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-184">So, if you delete the cluster, the notebooks will be deleted as well.</span></span> <span data-ttu-id="1a0a6-185">Pokud chcete zachovat poznámkové bloky pro pozdější použití v jiných clusterech, je nutné je po dokončení spuštění úlohy exportovat.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-185">If you want to preserve your notebooks for later use on other clusters, you must export them after you have finished running the jobs.</span></span> <span data-ttu-id="1a0a6-186">Chcete-li exportovat Poznámkový blok, klikněte na tlačítko **exportovat** ikonu, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-186">To export a notebook, click the **Export** icon as shown in the image below.</span></span>

<span data-ttu-id="1a0a6-187">![Stáhnout poznámkového bloku](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "stáhnout poznámkového bloku")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-187">![Download notebook](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Download the notebook")</span></span>

<span data-ttu-id="1a0a6-188">To umožňuje ušetřit poznámkového bloku jako soubor JSON ve vašem umístění stahování.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-188">This saves the notebook as a JSON file in your download location.</span></span>

## <a name="livy-session-management"></a><span data-ttu-id="1a0a6-189">Správa relací Livy</span><span class="sxs-lookup"><span data-stu-id="1a0a6-189">Livy session management</span></span>
<span data-ttu-id="1a0a6-190">Když spustíte prvním odstavci kódu v poznámkovém Zeppelin, je vytvořit novou relaci Livy v clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-190">When you run the first code paragraph in your Zeppelin notebook, a new Livy session is created in your HDInsight Spark cluster.</span></span> <span data-ttu-id="1a0a6-191">Tuto relaci je sdílen na všech poznámkových bloků Zeppelin, které následně vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-191">This session is shared across all Zeppelin notebooks that you subsequently create.</span></span> <span data-ttu-id="1a0a6-192">Pokud z nějakého důvodu Livy relace je ukončená (restartování clusteru atd.), nebudete moci spouštět úlohy z Zeppelin poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-192">If for some reason the Livy session is killed (cluster reboot, etc.), you will not be able to run jobs from the Zeppelin notebook.</span></span>

<span data-ttu-id="1a0a6-193">V takovém případě musíte provést následující kroky předtím, než můžete začít spouštět úlohy z Zeppelin Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-193">In such a case, you must perform the following steps before you can start running jobs from a Zeppelin notebook.</span></span> 

1. <span data-ttu-id="1a0a6-194">Restartujte překladač Livy z Zeppelin poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-194">Restart the Livy interpreter from the Zeppelin notebook.</span></span> <span data-ttu-id="1a0a6-195">Uděláte to tak, otevřete překladač nastavení kliknutím na příkaz přihlášeného v uživatelské jméno v pravém horním rohu a pak klikněte na tlačítko **překladač**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-195">To do so, open interpreter settings by clicking the logged in user name from the top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="1a0a6-196">![Spustí překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "výstupu podregistru")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-196">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="1a0a6-197">Přejděte na nastavení překladač Livy a pak klikněte na **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-197">Scroll to Livy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="1a0a6-198">![Restartujte Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "restartujte Zeppelin intepreter")</span><span class="sxs-lookup"><span data-stu-id="1a0a6-198">![Restart the Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart the Zeppelin intepreter")</span></span>
3. <span data-ttu-id="1a0a6-199">Spusťte buňky kódu z existujícího Zeppelin poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-199">Run a code cell from an existing Zeppelin notebook.</span></span> <span data-ttu-id="1a0a6-200">Tím se vytvoří novou relaci Livy v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1a0a6-200">This creates a new Livy session in the HDInsight cluster.</span></span>

## <span data-ttu-id="1a0a6-201"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="1a0a6-201"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="1a0a6-202">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="1a0a6-202">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="1a0a6-203">Scénáře</span><span class="sxs-lookup"><span data-stu-id="1a0a6-203">Scenarios</span></span>
* [<span data-ttu-id="1a0a6-204">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="1a0a6-204">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="1a0a6-205">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="1a0a6-205">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="1a0a6-206">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="1a0a6-206">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="1a0a6-207">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="1a0a6-207">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="1a0a6-208">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1a0a6-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="1a0a6-209">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="1a0a6-209">Create and run applications</span></span>
* [<span data-ttu-id="1a0a6-210">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="1a0a6-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="1a0a6-211">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="1a0a6-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="1a0a6-212">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="1a0a6-212">Tools and extensions</span></span>
* [<span data-ttu-id="1a0a6-213">Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="1a0a6-213">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="1a0a6-214">Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark</span><span class="sxs-lookup"><span data-stu-id="1a0a6-214">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="1a0a6-215">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="1a0a6-215">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="1a0a6-216">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="1a0a6-216">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="1a0a6-217">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="1a0a6-217">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="1a0a6-218">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="1a0a6-218">Manage resources</span></span>
* [<span data-ttu-id="1a0a6-219">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="1a0a6-219">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="1a0a6-220">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1a0a6-220">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md 







