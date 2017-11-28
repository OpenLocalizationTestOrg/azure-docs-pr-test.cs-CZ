---
title: "aaaAnalyze webových protokolů pomocí knihovny Python ve Sparku - Azure | Microsoft Docs"
description: "Tento poznámkový blok ukazuje, jak tooanalyze protokolu dat pomocí Spark v Azure HDInsight vlastní knihovny."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c61c70f-fe7f-4f0f-a4ab-0cccee5668c9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 29e4308b2a359aee6d69494a98307d4da07f7909
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-spark-cluster-on-hdinsight"></a><span data-ttu-id="4dfdd-103">Analýza webových protokolů pomocí vlastní knihovna Python s clusterem Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="4dfdd-103">Analyze website logs using a custom Python library with Spark cluster on HDInsight</span></span>

<span data-ttu-id="4dfdd-104">Tento poznámkový blok ukazuje, jak tooanalyze protokolu dat pomocí Spark v HDInsight vlastní knihovny.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-104">This notebook demonstrates how tooanalyze log data using a custom library with Spark on HDInsight.</span></span> <span data-ttu-id="4dfdd-105">Vlastní knihovna Hello používáme je knihovna Python názvem **iislogparser.py**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-105">hello custom library we use is a Python library called **iislogparser.py**.</span></span>

> [!TIP]
> <span data-ttu-id="4dfdd-106">V tomto kurzu je také k dispozici jako poznámkový blok Jupyter v clusteru Spark (Linux), který vytvoříte v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-106">This tutorial is also available as a Jupyter notebook on a Spark (Linux) cluster that you create in HDInsight.</span></span> <span data-ttu-id="4dfdd-107">Hello poznámkového bloku prostředí vám umožní spustit hello Python fragmenty z Poznámkový blok hello sám sebe.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-107">hello notebook experience lets you run hello Python snippets from hello notebook itself.</span></span> <span data-ttu-id="4dfdd-108">kurz hello tooperform z v rámci Poznámkový blok, vytvořte Spark cluster, spusťte poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a poté spusťte Poznámkový blok hello **Analýza protokolů s Spark pomocí vlastní library.ipynb** pod hello  **PySpark** složky.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-108">tooperform hello tutorial from within a notebook, create a Spark cluster, launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), and then run hello notebook **Analyze logs with Spark using a custom library.ipynb** under hello **PySpark** folder.</span></span>
>
>

<span data-ttu-id="4dfdd-109">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="4dfdd-109">**Prerequisites:**</span></span>

<span data-ttu-id="4dfdd-110">Musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-110">You must have hello following:</span></span>

* <span data-ttu-id="4dfdd-111">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-111">An Azure subscription.</span></span> <span data-ttu-id="4dfdd-112">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="4dfdd-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="4dfdd-113">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="4dfdd-114">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="4dfdd-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="save-raw-data-as-an-rdd"></a><span data-ttu-id="4dfdd-115">Uložit jako RDD nezpracovaná data</span><span class="sxs-lookup"><span data-stu-id="4dfdd-115">Save raw data as an RDD</span></span>
<span data-ttu-id="4dfdd-116">V této části používáme hello [Jupyter](https://jupyter.org) poznámkového bloku přidružené cluster Apache Spark v HDInsight toorun úlohy, které zpracování nezpracovaných ukázkových dat a uložte ho jako tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-116">In this section, we use hello [Jupyter](https://jupyter.org) notebook associated with an Apache Spark cluster in HDInsight toorun jobs that process your raw sample data and save it as a Hive table.</span></span> <span data-ttu-id="4dfdd-117">Hello ukázkových dat je soubor .csv (hvac.csv) k dispozici na všech clusterech ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-117">hello sample data is a .csv file (hvac.csv) available on all clusters by default.</span></span>

<span data-ttu-id="4dfdd-118">Vaše data uložena jako tabulku Hive, v další části hello jsme se připojí toohello tabulku Hive pomocí nástrojů BI, například Power BI a Tableau.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-118">Once your data is saved as a Hive table, in hello next section we will connect toohello Hive table using BI tools such as Power BI and Tableau.</span></span>

1. <span data-ttu-id="4dfdd-119">Z hello [portál Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="4dfdd-119">From hello [Azure portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="4dfdd-120">Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-120">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="4dfdd-121">Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-121">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="4dfdd-122">Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-122">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4dfdd-123">Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-123">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="4dfdd-124">Nahraďte **CLUSTERNAME** s hello název clusteru:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-124">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="4dfdd-125">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-125">Create a new notebook.</span></span> <span data-ttu-id="4dfdd-126">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-126">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="4dfdd-127">![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Vytvoření nového poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="4dfdd-127">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>
4. <span data-ttu-id="4dfdd-128">Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-128">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="4dfdd-129">Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-129">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="4dfdd-130">![Zadejte název pro hello notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "zadejte název pro hello Poznámkový blok")</span><span class="sxs-lookup"><span data-stu-id="4dfdd-130">![Provide a name for hello notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "Provide a name for hello notebook")</span></span>
5. <span data-ttu-id="4dfdd-131">Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-131">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="4dfdd-132">Hello kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu hello.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-132">hello Spark and Hive contexts will be automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="4dfdd-133">Můžete začít importem hello typy, které jsou požadovány pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-133">You can start by importing hello types that are required for this scenario.</span></span> <span data-ttu-id="4dfdd-134">Vložte následující fragment kódu do prázdné buňky hello a potom stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-134">Paste hello following snippet in an empty cell, and then press **SHIFT + ENTER**.</span></span>

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. <span data-ttu-id="4dfdd-135">Vytvoření RDD pomocí hello ukázková protokolu data již k dispozici v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-135">Create an RDD using hello sample log data already available on hello cluster.</span></span> <span data-ttu-id="4dfdd-136">Můžou k datům hello ve hello výchozí účet úložiště přidruženého k hello clusteru na **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-136">You can access hello data in hello default storage account associated with hello cluster at **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.</span></span>

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. <span data-ttu-id="4dfdd-137">Načtěte ukázkové sady tooverify protokolu, který hello předchozího kroku byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-137">Retrieve a sample log set tooverify that hello previous step completed successfully.</span></span>

        logs.take(5)

    <span data-ttu-id="4dfdd-138">Měli byste vidět výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-138">You should see an output similar toohello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a><span data-ttu-id="4dfdd-139">Analyzovat data protokolu pomocí vlastní knihovna Python</span><span class="sxs-lookup"><span data-stu-id="4dfdd-139">Analyze log data using a custom Python library</span></span>
1. <span data-ttu-id="4dfdd-140">Ve výstupu hello výše hello prvních několika řádků obsahovat informace hlavičky hello a každý zbývající řádek odpovídá schématu hello popsané v této hlavičce.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-140">In hello output above, hello first couple lines include hello header information and each remaining line matches hello schema described in that header.</span></span> <span data-ttu-id="4dfdd-141">Analýza tyto protokoly může být složité.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-141">Parsing such logs could be complicated.</span></span> <span data-ttu-id="4dfdd-142">Ano, používáme vlastní knihovna Python (**iislogparser.py**), díky analýze tyto protokoly mnohem jednodušší.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-142">So, we use a custom Python library (**iislogparser.py**) that makes parsing such logs much easier.</span></span> <span data-ttu-id="4dfdd-143">Ve výchozím nastavení, je součástí clusteru Spark v HDInsight v této knihovně **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-143">By default, this library is included with your Spark cluster on HDInsight at **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.</span></span>

    <span data-ttu-id="4dfdd-144">Ale v této knihovně se nenachází v hello `PYTHONPATH` tak jsme nelze použít pomocí příkazu import jako `import iislogparser`.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-144">However, this library is not in hello `PYTHONPATH` so we cannot use it by using an import statement like `import iislogparser`.</span></span> <span data-ttu-id="4dfdd-145">toouse tuto knihovnu jsme ji musíte distribuovat tooall hello pracovním uzlům.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-145">toouse this library, we must distribute it tooall hello worker nodes.</span></span> <span data-ttu-id="4dfdd-146">Spusťte hello následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-146">Run hello following snippet.</span></span>

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. <span data-ttu-id="4dfdd-147">`iislogparser`poskytuje funkci `parse_log_line` , který vrací `None` Pokud protokolu řádek je řádek záhlaví a vrací instanci třídy hello `LogLine` třídy v případě nalezení řádek protokolu.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-147">`iislogparser` provides a function `parse_log_line` that returns `None` if a log line is a header row, and returns an instance of hello `LogLine` class if it encounters a log line.</span></span> <span data-ttu-id="4dfdd-148">Použití hello `LogLine` třída tooextract pouze hello protokolu řádky z hello RDD:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-148">Use hello `LogLine` class tooextract only hello log lines from hello RDD:</span></span>

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. <span data-ttu-id="4dfdd-149">Načtení pár tooverify extrahované protokolu řádky, které hello krok byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-149">Retrieve a couple of extracted log lines tooverify that hello step completed successfully.</span></span>

       logLines.take(2)

   <span data-ttu-id="4dfdd-150">výstup Hello by měl vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-150">hello output should be similar toohello following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. <span data-ttu-id="4dfdd-151">Hello `LogLine` třída, pak má některé užitečné metody, jako je třeba `is_error()`, který vrátí, zda položka protokolu má chybový kód.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-151">hello `LogLine` class, in turn, has some useful methods, like `is_error()`, which returns whether a log entry has an error code.</span></span> <span data-ttu-id="4dfdd-152">Pomocí této toocompute hello počet chyb v hello extrahovat protokolu řádky a potom všechny hello chyby tooa jiný soubor protokolu.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-152">Use this toocompute hello number of errors in hello extracted log lines, and then log all hello errors tooa different file.</span></span>

       errors = logLines.filter(lambda p: p.is_error())
       numLines = logLines.count()
       numErrors = errors.count()
       print 'There are', numErrors, 'errors and', numLines, 'log entries'
       errors.map(lambda p: str(p)).saveAsTextFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

   <span data-ttu-id="4dfdd-153">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-153">You should see an output like hello following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       There are 30 errors and 646 log entries
4. <span data-ttu-id="4dfdd-154">Můžete také použít **Matplotlib** tooconstruct vizualizace dat hello.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-154">You can also use **Matplotlib** tooconstruct a visualization of hello data.</span></span> <span data-ttu-id="4dfdd-155">Například pokud chcete tooisolate hello příčinu požadavků, které spustit po dlouhou dobu, můžete toofind hello soubory, které provést hello většinu času tooserve v průměru.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-155">For example, if you want tooisolate hello cause of requests that run for a long time, you might want toofind hello files that take hello most time tooserve on average.</span></span>
   <span data-ttu-id="4dfdd-156">Následující fragment Hello načte hello nejvyšší 25 zdroje, které trvalo tooserve většinu času požadavku.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-156">hello snippet below retrieves hello top 25 resources that took most time tooserve a request.</span></span>

       def avgTimeTakenByKey(rdd):
           return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                   lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                   lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                     .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

       avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

   <span data-ttu-id="4dfdd-157">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-157">You should see an output like hello following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [(u'/blogposts/mvc4/step13.png', 197.5),
        (u'/blogposts/mvc2/step10.jpg', 179.5),
        (u'/blogposts/extractusercontrol/step5.png', 170.0),
        (u'/blogposts/mvc4/step8.png', 159.0),
        (u'/blogposts/mvcrouting/step22.jpg', 155.0),
        (u'/blogposts/mvcrouting/step3.jpg', 152.0),
        (u'/blogposts/linqsproc1/step16.jpg', 138.75),
        (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
        (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
        (u'/blogposts/nested/step2.jpg', 126.0),
        (u'/blogposts/adminpack/step1.png', 124.0),
        (u'/BlogPosts/datalistpaging/step2.png', 118.0),
        (u'/blogposts/mvc4/step35.png', 117.0),
        (u'/blogposts/mvcrouting/step2.jpg', 116.5),
        (u'/blogposts/aboutme/basketball.jpg', 109.0),
        (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
        (u'/blogposts/mvc4/step12.png', 106.0),
        (u'/blogposts/linq8/step0.jpg', 105.5),
        (u'/blogposts/mvc2/step18.jpg', 104.0),
        (u'/blogposts/mvc2/step11.jpg', 104.0),
        (u'/blogposts/mvcrouting/step1.jpg', 104.0),
        (u'/blogposts/extractusercontrol/step1.png', 103.0),
        (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
        (u'/blogposts/mvcrouting/step21.jpg', 101.0),
        (u'/blogposts/mvc4/step1.png', 98.0)]
5. <span data-ttu-id="4dfdd-158">Můžete také prezentuje tyto informace v hello formátu vykreslení.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-158">You can also present this information in hello form of plot.</span></span> <span data-ttu-id="4dfdd-159">Jako první krok toocreate vykreslení. dejte nám nejdřív vytvořit dočasné tabulky **AverageTime**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-159">As a first step toocreate a plot, let us first create a temporary table **AverageTime**.</span></span> <span data-ttu-id="4dfdd-160">Hello tabulka skupiny hello zaznamená podle času toosee, pokud v kterémkoli okamžiku nebyly žádné neobvyklou latence špičky.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-160">hello table groups hello logs by time toosee if there were any unusual latency spikes at any particular time.</span></span>

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. <span data-ttu-id="4dfdd-161">Potom můžete spustit následující tooget dotazu SQL hello všechny záznamy hello v hello **AverageTime** tabulky.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-161">You can then run hello following SQL query tooget all hello records in hello **AverageTime** table.</span></span>

       %%sql -o averagetime
       SELECT * FROM AverageTime

   <span data-ttu-id="4dfdd-162">Hello `%%sql` magic následuje `-o averagetime` zajistí, že výstup hello hello dotazu je trvalé místně na serveru Jupyter hello (obvykle hello headnode hello clusteru).</span><span class="sxs-lookup"><span data-stu-id="4dfdd-162">hello `%%sql` magic followed by `-o averagetime` ensures that hello output of hello query is persisted locally on hello Jupyter server (typically hello headnode of hello cluster).</span></span> <span data-ttu-id="4dfdd-163">výstup Hello je uchován jako [Pandas](http://pandas.pydata.org/) dataframe s hello zadaný název **averagetime**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-163">hello output is persisted as a [Pandas](http://pandas.pydata.org/) dataframe with hello specified name **averagetime**.</span></span>

   <span data-ttu-id="4dfdd-164">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-164">You should see an output like hello following:</span></span>

   <span data-ttu-id="4dfdd-165">![Výstup dotazu SQL](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "výstupu dotazu SQL")</span><span class="sxs-lookup"><span data-stu-id="4dfdd-165">![SQL query output](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL query output")</span></span>

   <span data-ttu-id="4dfdd-166">Další informace o hello `%%sql` magic, viz [parametry podporovány s hello %% sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="4dfdd-166">For more information about hello `%%sql` magic, see [Parameters supported with hello %%sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>
7. <span data-ttu-id="4dfdd-167">Teď můžete použít Matplotlib, knihovny použita tooconstruct vizualizaci dat, toocreate vykreslení.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-167">You can now use Matplotlib, a library used tooconstruct visualization of data, toocreate a plot.</span></span> <span data-ttu-id="4dfdd-168">Protože hello výkresu musí být vytvořeny z místně hello trvalé **averagetime** dataframe, fragment kódu hello musí začínat řetězcem hello `%%local` magic.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-168">Because hello plot must be created from hello locally persisted **averagetime** dataframe, hello code snippet must begin with hello `%%local` magic.</span></span> <span data-ttu-id="4dfdd-169">To zajistí hello kód je místně na serveru Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-169">This ensures that hello code is run locally on hello Jupyter server.</span></span>

       %%local
       %matplotlib inline
       import matplotlib.pyplot as plt

       plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
       plt.xlabel('Time (min)')
       plt.ylabel('Average time taken for request (ms)')

   <span data-ttu-id="4dfdd-170">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-170">You should see an output like hello following:</span></span>

   <span data-ttu-id="4dfdd-171">![Výstup Matplotlib](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib výstup")</span><span class="sxs-lookup"><span data-stu-id="4dfdd-171">![Matplotlib output](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib output")</span></span>
8. <span data-ttu-id="4dfdd-172">Po dokončení spuštění aplikace hello, měli byste vypnout hello poznámkového bloku toorelease hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-172">After you have finished running hello application, you should shutdown hello notebook toorelease hello resources.</span></span> <span data-ttu-id="4dfdd-173">toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-173">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="4dfdd-174">Dojde k vypnutí a zavřít hello Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-174">This will shutdown and close hello notebook.</span></span>

## <span data-ttu-id="4dfdd-175"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="4dfdd-175"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="4dfdd-176">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="4dfdd-176">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="4dfdd-177">Scénáře</span><span class="sxs-lookup"><span data-stu-id="4dfdd-177">Scenarios</span></span>
* [<span data-ttu-id="4dfdd-178">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="4dfdd-178">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="4dfdd-179">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="4dfdd-179">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="4dfdd-180">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="4dfdd-180">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="4dfdd-181">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="4dfdd-181">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="4dfdd-182">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="4dfdd-182">Create and run applications</span></span>
* [<span data-ttu-id="4dfdd-183">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="4dfdd-183">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="4dfdd-184">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="4dfdd-184">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="4dfdd-185">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="4dfdd-185">Tools and extensions</span></span>
* [<span data-ttu-id="4dfdd-186">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="4dfdd-186">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="4dfdd-187">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="4dfdd-187">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="4dfdd-188">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="4dfdd-188">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="4dfdd-189">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="4dfdd-189">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="4dfdd-190">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="4dfdd-190">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="4dfdd-191">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="4dfdd-191">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="4dfdd-192">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="4dfdd-192">Manage resources</span></span>
* [<span data-ttu-id="4dfdd-193">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="4dfdd-193">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="4dfdd-194">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="4dfdd-194">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
