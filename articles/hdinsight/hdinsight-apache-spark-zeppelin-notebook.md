---
title: "clusteru aaaUse poznámkových bloků Zeppelin s Apache Spark v Azure HDInsight | Microsoft Docs"
description: "Podrobný návod, jak clusterů toouse poznámkových bloků Zeppelin s Apache Spark v Azure HDInsight."
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
ms.openlocfilehash: 3ab479cfccc7fd38a9bf6a9fb4f5928beec8ff7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a><span data-ttu-id="9fd1c-103">Použití poznámkových bloků Zeppelin s clusterem Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fd1c-103">Use Zeppelin notebooks with Apache Spark cluster on Azure HDInsight</span></span>

<span data-ttu-id="9fd1c-104">Clustery HDInsight Spark zahrnují poznámkových bloků Zeppelin, které můžete použít toorun Spark úlohy.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-104">HDInsight Spark clusters include Zeppelin notebooks that you can use toorun Spark jobs.</span></span> <span data-ttu-id="9fd1c-105">V tomto článku se dozvíte, jak toouse hello Zeppelin poznámkového bloku v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-105">In this article, you learn how toouse hello Zeppelin notebook on an HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="9fd1c-106">Poznámkových bloků Zeppelin jsou dostupné pouze pro 1.6.3 Spark v HDInsight 3.5 a 2.1.0 Spark v HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-106">Zeppelin notebooks are available only for Spark 1.6.3 on HDInsight 3.5 and Spark 2.1.0 on HDInsight 3.6.</span></span>
>

<span data-ttu-id="9fd1c-107">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="9fd1c-107">**Prerequisites:**</span></span>

* <span data-ttu-id="9fd1c-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-108">An Azure subscription.</span></span> <span data-ttu-id="9fd1c-109">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="9fd1c-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="9fd1c-110">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-110">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="9fd1c-111">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="9fd1c-111">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="launch-a-zeppelin-notebook"></a><span data-ttu-id="9fd1c-112">Spusťte Zeppelin Poznámkový blok</span><span class="sxs-lookup"><span data-stu-id="9fd1c-112">Launch a Zeppelin notebook</span></span>
1. <span data-ttu-id="9fd1c-113">Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Zeppelin Poznámkový blok**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-113">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Zeppelin Notebook**.</span></span> <span data-ttu-id="9fd1c-114">Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-114">If prompted, enter hello admin credentials for hello cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9fd1c-115">Může také nedostanete hello Zeppelin Poznámkový blok pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-115">You may also reach hello Zeppelin Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="9fd1c-116">Nahraďte **CLUSTERNAME** s hello název clusteru:</span><span class="sxs-lookup"><span data-stu-id="9fd1c-116">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. <span data-ttu-id="9fd1c-117">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-117">Create a new notebook.</span></span> <span data-ttu-id="9fd1c-118">Z hello podokno záhlaví, klikněte na tlačítko **poznámkového bloku**a potom klikněte na **vytvořit novou poznámku**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-118">From hello header pane, click **Notebook**, and then click **Create New Note**.</span></span>
   
    <span data-ttu-id="9fd1c-119">![Vytvořte nový poznámkový blok Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "vytvořte nový poznámkový blok Zeppelin")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-119">![Create a new Zeppelin notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Create a new Zeppelin notebook")</span></span>
   
    <span data-ttu-id="9fd1c-120">Zadejte název pro hello Poznámkový blok a pak klikněte na tlačítko **vytvořit Poznámka**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-120">Enter a name for hello notebook, and then click **Create Note**.</span></span>
3. <span data-ttu-id="9fd1c-121">Také zkontrolujte, zda záhlaví poznámkového bloku hello zobrazí připojené stav.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-121">Also, make sure hello notebook header shows a connected status.</span></span> <span data-ttu-id="9fd1c-122">Označuje zelená tečky v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-122">It is denoted by a green dot in hello top-right corner.</span></span>
   
    <span data-ttu-id="9fd1c-123">![Stav poznámkového bloku Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin poznámkového bloku stav")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-123">![Zeppelin notebook status](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin notebook status")</span></span>
4. <span data-ttu-id="9fd1c-124">Načtěte vzorová data do dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-124">Load sample data into a temporary table.</span></span> <span data-ttu-id="9fd1c-125">Když vytvoříte Spark cluster v HDInsight, ukázkový datový soubor hello, **hvac.csv**, je zkopírovaný toohello přidruženého účtu úložiště **\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-125">When you create a Spark cluster in HDInsight, hello sample data file, **hvac.csv**, is copied toohello associated storage account under **\HdiSamples\SensorSampleData\hvac**.</span></span>
   
    <span data-ttu-id="9fd1c-126">V prázdné odstavci hello, který se vytvoří ve výchozím nastavení v hello nový poznámkový blok vložte následující fragment kódu hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-126">In hello empty paragraph that is created by default in hello new notebook, paste hello following snippet.</span></span>
   
        %livy.spark
        //hello above magic instructs Zeppelin toouse hello Livy Scala interpreter
   
        // Create an RDD using hello default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map hello values in hello .csv file toohello schema
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
   
    <span data-ttu-id="9fd1c-127">Stiskněte klávesu **SHIFT + ENTER** nebo klikněte na tlačítko hello **přehrání** tlačítko hello odstavce toorun hello fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-127">Press **SHIFT + ENTER** or click hello **Play** button for hello paragraph toorun hello snippet.</span></span> <span data-ttu-id="9fd1c-128">Stav Hello na hello pravém rohu hello odstavce by měl průběhu z PŘIPRAVENÝ, čeká na vyřízení, tooFINISHED SPUŠTĚNÁ.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-128">hello status on hello right-corner of hello paragraph should progress from READY, PENDING, RUNNING tooFINISHED.</span></span> <span data-ttu-id="9fd1c-129">výstup Hello objeví dole hello hello stejné odstavce.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-129">hello output shows up at hello bottom of hello same paragraph.</span></span> <span data-ttu-id="9fd1c-130">snímek obrazovky Hello vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="9fd1c-130">hello screenshot looks like hello following:</span></span>
   
    <span data-ttu-id="9fd1c-131">![Vytvořte dočasnou tabulku z nezpracovaná data](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "vytvořit dočasnou tabulku od nezpracovaných dat")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-131">![Create a temporary table from raw data](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Create a temporary table from raw data")</span></span>
   
    <span data-ttu-id="9fd1c-132">Můžete zadat také název tooeach odstavec.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-132">You can also provide a title tooeach paragraph.</span></span> <span data-ttu-id="9fd1c-133">V pravém rohu hello, klikněte na hello **nastavení** ikonu a pak klikněte na tlačítko **zobrazit nadpis**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-133">From hello right-hand corner, click hello **Settings** icon, and then click **Show title**.</span></span>
5. <span data-ttu-id="9fd1c-134">Teď můžete spustit příkazy Spark SQL na hello **TVK** tabulky.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-134">You can now run Spark SQL statements on hello **hvac** table.</span></span> <span data-ttu-id="9fd1c-135">Vložte hello následující dotaz nový odstavec.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-135">Paste hello following query in a new paragraph.</span></span> <span data-ttu-id="9fd1c-136">Hello dotaz načte ID budovy hello a hello rozdíl mezi hello cíl a skutečný teploty pro každé sestavení v určitém dni.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-136">hello query retrieves hello building ID and hello difference between hello target and actual temperatures for each building on a given date.</span></span> <span data-ttu-id="9fd1c-137">Stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-137">Press **SHIFT + ENTER**.</span></span>
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    <span data-ttu-id="9fd1c-138">Hello **% sql** příkaz od začátku hello sděluje hello poznámkového bloku toouse hello Livy Scala překladač.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-138">hello **%sql** statement at hello beginning tells hello notebook toouse hello Livy Scala interpreter.</span></span>
   
    <span data-ttu-id="9fd1c-139">Hello následující snímek obrazovky ukazuje výstup hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-139">hello following screenshot shows hello output.</span></span>
   
    <span data-ttu-id="9fd1c-140">![Spustit příkaz Spark SQL pomocí poznámkového bloku hello](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "spustit příkaz Spark SQL pomocí poznámkového bloku hello")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-140">![Run a Spark SQL statement using hello notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Run a Spark SQL statement using hello notebook")</span></span>
   
     <span data-ttu-id="9fd1c-141">Klikněte na tlačítko hello zobrazení možnosti (zvýraznit v obdélníku) tooswitch mezi různé reprezentace pro hello stejný výstup.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-141">Click hello display options (highlighted in rectangle) tooswitch between different representations for hello same output.</span></span> <span data-ttu-id="9fd1c-142">Klikněte na tlačítko **nastavení** toochoose co consitutes hello klíče a hodnoty ve výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-142">Click **Settings** toochoose what consitutes hello key and values in hello output.</span></span> <span data-ttu-id="9fd1c-143">snímek obrazovky výše používá Hello **buildingID** jako klíč hello a průměr hello **temp_diff** jako hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-143">hello screen capture above uses **buildingID** as hello key and hello average of **temp_diff** as hello value.</span></span>
6. <span data-ttu-id="9fd1c-144">Můžete také spustit příkazy Spark SQL pomocí proměnných v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-144">You can also run Spark SQL statements using variables in hello query.</span></span> <span data-ttu-id="9fd1c-145">Hello další fragment kódu ukazuje jak toodefine proměnnou, **Temp**, hello dotaz s hello možné hodnoty chcete tooquery s.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-145">hello next snippet shows how toodefine a variable, **Temp**, in hello query with hello possible values you want tooquery with.</span></span> <span data-ttu-id="9fd1c-146">Při prvním spuštění dotazu hello, rozevírací seznam se automaticky zadá hello hodnoty, které jste zadali pro proměnnou hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-146">When you first run hello query, a drop-down is automatically populated with hello values you specified for hello variable.</span></span>
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    <span data-ttu-id="9fd1c-147">Vložte tento fragment kódu nový odstavec a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-147">Paste this snippet in a new paragraph and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="9fd1c-148">Hello následující snímek obrazovky ukazuje výstup hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-148">hello following screenshot shows hello output.</span></span>
   
    <span data-ttu-id="9fd1c-149">![Spustit příkaz Spark SQL pomocí poznámkového bloku hello](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "spustit příkaz Spark SQL pomocí poznámkového bloku hello")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-149">![Run a Spark SQL statement using hello notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Run a Spark SQL statement using hello notebook")</span></span>
   
    <span data-ttu-id="9fd1c-150">Pro následné dotazy můžete vybrat novou hodnotu z rozevíracího seznamu hello a znovu spusťte dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-150">For subsequent queries, you can select a new value from hello drop-down and run hello query again.</span></span> <span data-ttu-id="9fd1c-151">Klikněte na tlačítko **nastavení** toochoose co consitutes hello klíče a hodnoty ve výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-151">Click **Settings** toochoose what consitutes hello key and values in hello output.</span></span> <span data-ttu-id="9fd1c-152">snímek obrazovky výše používá Hello **buildingID** jako klíč hello hello průměrný počet **temp_diff** jako hello hodnotu, a **targettemp** jako skupina hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-152">hello screen capture above uses **buildingID** as hello key, hello average of **temp_diff** as hello value, and **targettemp** as hello group.</span></span>
7. <span data-ttu-id="9fd1c-153">Restartujte hello Livy překladač tooexit hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-153">Restart hello Livy interpreter tooexit hello application.</span></span> <span data-ttu-id="9fd1c-154">toodo tak, že otevřete nastavení překladač kliknutím hello přihlášení uživatelské jméno ze hello pravém horním rohu a pak klikněte na **překladač**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-154">toodo so, open interpreter settings by clicking hello logged in user name from hello top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="9fd1c-155">![Spustí překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "výstupu podregistru")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-155">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
8. <span data-ttu-id="9fd1c-156">Posuňte se tooLivy překladač nastavení a pak klikněte na tlačítko **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-156">Scroll tooLivy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="9fd1c-157">![Restartujte hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "restartujte hello Zeppelin intepreter")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-157">![Restart hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart hello Zeppelin intepreter")</span></span>

## <a name="how-do-i-use-external-packages-with-hello-notebook"></a><span data-ttu-id="9fd1c-158">Jak používat externí balíčky s hello Poznámkový blok?</span><span class="sxs-lookup"><span data-stu-id="9fd1c-158">How do I use external packages with hello notebook?</span></span>
<span data-ttu-id="9fd1c-159">V clusteru Apache Spark v HDInsight (Linux) toouse externí, komunity podílí balíčky, které nejsou zahrnuté out-of-the-box v hello clusteru můžete nakonfigurovat hello Zeppelin poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-159">You can configure hello Zeppelin notebook in Apache Spark cluster on HDInsight (Linux) toouse external, community-contributed packages that are not included out-of-the-box in hello cluster.</span></span> <span data-ttu-id="9fd1c-160">Můžete hledat hello [Maven úložiště](http://search.maven.org/) hello úplný seznam balíčků, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-160">You can search hello [Maven repository](http://search.maven.org/) for hello complete list of packages that are available.</span></span> <span data-ttu-id="9fd1c-161">Seznam dostupných balíčků můžete také získat z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-161">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="9fd1c-162">Například je k dispozici úplný seznam balíčků podílí komunity [Spark balíčky](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="9fd1c-162">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="9fd1c-163">V tomto článku se zobrazí jak toouse hello [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) balíček s hello Poznámkový blok Jupyter.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-163">In this article, you will see how toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with hello Jupyter notebook.</span></span>

1. <span data-ttu-id="9fd1c-164">Otevřete nastavení překladač.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-164">Open interpreter settings.</span></span> <span data-ttu-id="9fd1c-165">V pravém horním rohu hello, klikněte hello přihlášení uživatelské jméno a potom klikněte na **překladač**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-165">From hello top-right corner, click hello logged in user name, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="9fd1c-166">![Spustí překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "výstupu podregistru")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-166">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="9fd1c-167">Posuňte se tooLivy překladač nastavení a pak klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-167">Scroll tooLivy interpreter settings and then click **Edit**.</span></span>
   
    <span data-ttu-id="9fd1c-168">![Změna nastavení překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "změnit nastavení překladač")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-168">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Change interpreter settings")</span></span>
3. <span data-ttu-id="9fd1c-169">Přidejte nový klíč, nazývá **livy.spark.jars.packages** a nastavení jeho hodnoty ve formátu hello `group:id:version`.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-169">Add a new key, called **livy.spark.jars.packages** and set its value in hello format `group:id:version`.</span></span> <span data-ttu-id="9fd1c-170">Pokud chcete, aby toouse hello tedy [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) balíčku, je nutné nastavit hello hodnotu klíče hello příliš`com.databricks:spark-csv_2.10:1.4.0`.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-170">So, if you want toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package, you must set hello value of hello key too`com.databricks:spark-csv_2.10:1.4.0`.</span></span>
   
    <span data-ttu-id="9fd1c-171">![Změna nastavení překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "změnit nastavení překladač")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-171">![Change interpreter settings](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Change interpreter settings")</span></span>
   
    <span data-ttu-id="9fd1c-172">Klikněte na tlačítko **Uložit** a pak restartujte hello Livy překladač.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-172">Click **Save** and then restart hello Livy interpreter.</span></span>
4. <span data-ttu-id="9fd1c-173">**Tip**: Pokud chcete, aby toounderstand jak tooarrive v hodnotě hello hello klíče zadaná výše, zde uvádíme jak.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-173">**Tip**: If you want toounderstand how tooarrive at hello value of hello key entered above, here's how.</span></span>
   
    <span data-ttu-id="9fd1c-174">a.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-174">a.</span></span> <span data-ttu-id="9fd1c-175">Najděte balíček hello v hello Maven úložiště.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-175">Locate hello package in hello Maven Repository.</span></span> <span data-ttu-id="9fd1c-176">V tomto kurzu jsme použili [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="9fd1c-176">For this tutorial, we used [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="9fd1c-177">b.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-177">b.</span></span> <span data-ttu-id="9fd1c-178">Z úložiště hello shromážděte hello hodnoty pro **GroupId**, **ArtifactId**, a **verze**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-178">From hello repository, gather hello values for **GroupId**, **ArtifactId**, and **Version**.</span></span>
   
    <span data-ttu-id="9fd1c-179">![Použijte externí balíčky s Poznámkový blok Jupyter](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "použijte externí balíčky s poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-179">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="9fd1c-180">c.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-180">c.</span></span> <span data-ttu-id="9fd1c-181">Řetězení hello tři hodnoty, oddělené dvojtečkou (**:**).</span><span class="sxs-lookup"><span data-stu-id="9fd1c-181">Concatenate hello three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-hello-zeppelin-notebooks-saved"></a><span data-ttu-id="9fd1c-182">Kde jsou hello poznámkových bloků Zeppelin uložit?</span><span class="sxs-lookup"><span data-stu-id="9fd1c-182">Where are hello Zeppelin notebooks saved?</span></span>
<span data-ttu-id="9fd1c-183">poznámkových bloků Zeppelin Hello ukládají headnodes toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-183">hello Zeppelin notebooks are saved toohello cluster headnodes.</span></span> <span data-ttu-id="9fd1c-184">Takže pokud odstraníte hello clusteru, se odstraní také hello poznámkových bloků.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-184">So, if you delete hello cluster, hello notebooks will be deleted as well.</span></span> <span data-ttu-id="9fd1c-185">Pokud chcete, toopreserve poznámkové bloky pro pozdější použití v jiných clusterech, je nutné je po dokončení probíhajících úloh hello exportovat.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-185">If you want toopreserve your notebooks for later use on other clusters, you must export them after you have finished running hello jobs.</span></span> <span data-ttu-id="9fd1c-186">tooexport Poznámkový blok, klikněte na tlačítko hello **exportovat** ikonu, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-186">tooexport a notebook, click hello **Export** icon as shown in hello image below.</span></span>

<span data-ttu-id="9fd1c-187">![Stáhnout poznámkového bloku](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "stažení hello Poznámkový blok")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-187">![Download notebook](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Download hello notebook")</span></span>

<span data-ttu-id="9fd1c-188">To umožňuje ušetřit hello Poznámkový blok jako soubor JSON ve vašem umístění stahování.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-188">This saves hello notebook as a JSON file in your download location.</span></span>

## <a name="livy-session-management"></a><span data-ttu-id="9fd1c-189">Správa relací Livy</span><span class="sxs-lookup"><span data-stu-id="9fd1c-189">Livy session management</span></span>
<span data-ttu-id="9fd1c-190">Když spustíte hello prvním odstavci kódu v poznámkovém Zeppelin, je vytvořit novou relaci Livy v clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-190">When you run hello first code paragraph in your Zeppelin notebook, a new Livy session is created in your HDInsight Spark cluster.</span></span> <span data-ttu-id="9fd1c-191">Tuto relaci je sdílen na všech poznámkových bloků Zeppelin, které následně vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-191">This session is shared across all Zeppelin notebooks that you subsequently create.</span></span> <span data-ttu-id="9fd1c-192">Pokud pro některé hello důvod Livy relace je ukončená (restartování clusteru atd.) a nebude ji již možné toorun úlohy z hello Zeppelin poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-192">If for some reason hello Livy session is killed (cluster reboot, etc.), you will not be able toorun jobs from hello Zeppelin notebook.</span></span>

<span data-ttu-id="9fd1c-193">V takovém případě je nutné provést následující kroky předtím, než můžete začít spouštět úlohy z Poznámkový blok Zeppelin hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-193">In such a case, you must perform hello following steps before you can start running jobs from a Zeppelin notebook.</span></span> 

1. <span data-ttu-id="9fd1c-194">Restartujte hello Livy překladač z hello Zeppelin poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-194">Restart hello Livy interpreter from hello Zeppelin notebook.</span></span> <span data-ttu-id="9fd1c-195">toodo tak, že otevřete nastavení překladač kliknutím hello přihlášení uživatelské jméno ze hello pravém horním rohu a pak klikněte na **překladač**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-195">toodo so, open interpreter settings by clicking hello logged in user name from hello top-right corner, and then click **Interpreter**.</span></span>
   
    <span data-ttu-id="9fd1c-196">![Spustí překladač](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "výstupu podregistru")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-196">![Launch interpreter](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive output")</span></span>
2. <span data-ttu-id="9fd1c-197">Posuňte se tooLivy překladač nastavení a pak klikněte na tlačítko **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-197">Scroll tooLivy interpreter settings and then click **Restart**.</span></span>
   
    <span data-ttu-id="9fd1c-198">![Restartujte hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "restartujte hello Zeppelin intepreter")</span><span class="sxs-lookup"><span data-stu-id="9fd1c-198">![Restart hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Restart hello Zeppelin intepreter")</span></span>
3. <span data-ttu-id="9fd1c-199">Spusťte buňky kódu z existujícího Zeppelin poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-199">Run a code cell from an existing Zeppelin notebook.</span></span> <span data-ttu-id="9fd1c-200">Tím se vytvoří novou relaci Livy v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="9fd1c-200">This creates a new Livy session in hello HDInsight cluster.</span></span>

## <span data-ttu-id="9fd1c-201"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="9fd1c-201"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="9fd1c-202">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fd1c-202">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="9fd1c-203">Scénáře</span><span class="sxs-lookup"><span data-stu-id="9fd1c-203">Scenarios</span></span>
* [<span data-ttu-id="9fd1c-204">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="9fd1c-204">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="9fd1c-205">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="9fd1c-205">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="9fd1c-206">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fd1c-206">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="9fd1c-207">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="9fd1c-207">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="9fd1c-208">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fd1c-208">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="9fd1c-209">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="9fd1c-209">Create and run applications</span></span>
* [<span data-ttu-id="9fd1c-210">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="9fd1c-210">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="9fd1c-211">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="9fd1c-211">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="9fd1c-212">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="9fd1c-212">Tools and extensions</span></span>
* [<span data-ttu-id="9fd1c-213">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="9fd1c-213">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="9fd1c-214">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="9fd1c-214">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="9fd1c-215">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fd1c-215">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="9fd1c-216">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="9fd1c-216">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="9fd1c-217">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="9fd1c-217">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="9fd1c-218">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="9fd1c-218">Manage resources</span></span>
* [<span data-ttu-id="9fd1c-219">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fd1c-219">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="9fd1c-220">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="9fd1c-220">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md 







