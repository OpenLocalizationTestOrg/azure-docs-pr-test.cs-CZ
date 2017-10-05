---
title: "Řešení problémů s cluster Apache Spark v Azure HDInsight | Microsoft Docs"
description: "Další informace o problémech souvisejících s clustery Apache Spark v Azure HDInsight a postup ty obejít."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 610c4103-ffc8-4ec0-ad06-fdaf3c4d7c10
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3a493a2c35a6cdd31bb1e4ff66113a8f8d97d4f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="cb3c9-103">Známé problémy pro cluster Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb3c9-103">Known issues for Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="cb3c9-104">Tento dokument uchovává informace o všechny známé problémy pro verzi public preview HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-104">This document keeps track of all the known issues for the HDInsight Spark public preview.</span></span>  

## <a name="livy-leaks-interactive-session"></a><span data-ttu-id="cb3c9-105">Livy nevracení interaktivní relace.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-105">Livy leaks interactive session</span></span>
<span data-ttu-id="cb3c9-106">Při Livy restartování (z Ambari nebo z důvodu restartování virtuálního počítače headnode 0) k interaktivní relaci stále aktivní, bude úniku relaci interaktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-106">When Livy is restarted (from Ambari or due to headnode 0 virtual machine reboot) with an interactive session still alive, an interactive job session will be leaked.</span></span> <span data-ttu-id="cb3c9-107">Z toho důvodu nové úlohy můžete zablokované ve stavu platných a nejde ho spustit.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-107">Because of this, new jobs can stuck in the Accepted state, and cannot be started.</span></span>

<span data-ttu-id="cb3c9-108">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="cb3c9-108">**Mitigation:**</span></span>

<span data-ttu-id="cb3c9-109">Použijte následující postup, chcete-li vyřešit potíže:</span><span class="sxs-lookup"><span data-stu-id="cb3c9-109">Use the following procedure to workaround the issue:</span></span>

1. <span data-ttu-id="cb3c9-110">SSH do headnode.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-110">Ssh into headnode.</span></span> <span data-ttu-id="cb3c9-111">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="cb3c9-111">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="cb3c9-112">Spusťte následující příkaz k vyhledání ID aplikací interaktivní úlohy spuštěné prostřednictvím Livy.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-112">Run the following command to find the application IDs of the interactive jobs started through Livy.</span></span> 
   
        yarn application –list
   
    <span data-ttu-id="cb3c9-113">Názvy bude, že Livy Pokud úloh bylo zahájeno pomocí Livy interaktivní relaci se žádné explicitní názvy zadané, zahájení relace pro Livy pomocí poznámkového bloku Jupyter úlohu výchozí název úlohy se spustí s remotesparkmagics_ *.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-113">The default job names will be Livy if the jobs were started with a Livy interactive session with no explicit names specified, For the Livy session started by Jupyter notebook, the job name will start with remotesparkmagics_*.</span></span> 
3. <span data-ttu-id="cb3c9-114">Spusťte následující příkaz k ukončení těchto úloh.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-114">Run the following command to kill those jobs.</span></span> 
   
        yarn application –kill <Application ID>

<span data-ttu-id="cb3c9-115">Nové úlohy se spustí systémem.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-115">New jobs will start running.</span></span> 

## <a name="spark-history-server-not-started"></a><span data-ttu-id="cb3c9-116">Spark historie Server není spuštěn</span><span class="sxs-lookup"><span data-stu-id="cb3c9-116">Spark History Server not started</span></span>
<span data-ttu-id="cb3c9-117">Spark historie Server není spuštěn automaticky po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-117">Spark History Server is not started automatically after a cluster is created.</span></span>  

<span data-ttu-id="cb3c9-118">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="cb3c9-118">**Mitigation:**</span></span> 

<span data-ttu-id="cb3c9-119">Historie serveru spusťte ručně z Ambari.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-119">Manually start the history server from Ambari.</span></span>

## <a name="permission-issue-in-spark-log-directory"></a><span data-ttu-id="cb3c9-120">Problém s oprávněním v adresáři protokolu Spark</span><span class="sxs-lookup"><span data-stu-id="cb3c9-120">Permission issue in Spark log directory</span></span>
<span data-ttu-id="cb3c9-121">Pokud hdiuser odešle úlohu s spark-submit, dojde k chybě java.io.FileNotFoundException: nebylo napsáno /var/log/spark/sparkdriver_hdiuser.log (bylo odepřeno oprávnění) a protokol ovladačů.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-121">When hdiuser submits a job with spark-submit, there is an error java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied) and the driver log is not written.</span></span> 

<span data-ttu-id="cb3c9-122">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="cb3c9-122">**Mitigation:**</span></span>

1. <span data-ttu-id="cb3c9-123">Přidejte hdiuser ke skupině Hadoop.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-123">Add hdiuser to the Hadoop group.</span></span> 
2. <span data-ttu-id="cb3c9-124">Zadejte 777 oprávnění na /var/log/spark až po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-124">Provide 777 permissions on /var/log/spark after cluster creation.</span></span> 
3. <span data-ttu-id="cb3c9-125">Aktualizujte umístění protokolu spark pomocí nástroje Ambari být adresář s 777 oprávnění.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-125">Update the spark log location using Ambari to be a directory with 777 permissions.</span></span>  
4. <span data-ttu-id="cb3c9-126">Spustit spark odeslat jako sudo.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-126">Run spark-submit as sudo.</span></span>  

## <a name="spark-phoenix-connector-is-not-supported"></a><span data-ttu-id="cb3c9-127">Spark Phoenix konektor není podporovaný.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-127">Spark-Phoenix connector is not supported</span></span>

<span data-ttu-id="cb3c9-128">V současné době Spark Phoenix konektor není podporovaný s clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-128">Currently, the Spark-Phoenix connector is not supported with an HDInsight Spark cluster.</span></span>

<span data-ttu-id="cb3c9-129">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="cb3c9-129">**Mitigation:**</span></span>

<span data-ttu-id="cb3c9-130">Místo toho musíte použít konektor Spark HBase.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-130">You must use the Spark-HBase connector instead.</span></span> <span data-ttu-id="cb3c9-131">Pokyny naleznete v části [postupy k používání konektoru Spark HBase](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span><span class="sxs-lookup"><span data-stu-id="cb3c9-131">For instructions see [How to use Spark-HBase connector](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span></span>

## <a name="issues-related-to-jupyter-notebooks"></a><span data-ttu-id="cb3c9-132">Problémy související s poznámkové bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="cb3c9-132">Issues related to Jupyter notebooks</span></span>
<span data-ttu-id="cb3c9-133">Toto jsou některé známé problémy související s poznámkové bloky Jupyter.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-133">Following are some known issues related to Jupyter notebooks.</span></span>

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a><span data-ttu-id="cb3c9-134">Poznámkové bloky s jiné znaky než ASCII v názvech souborů</span><span class="sxs-lookup"><span data-stu-id="cb3c9-134">Notebooks with non-ASCII characters in filenames</span></span>
<span data-ttu-id="cb3c9-135">Jupyter notebooks, které mohou být používány clustery Spark HDInsight by neměl mít jiné znaky než ASCII v názvech souborů.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-135">Jupyter notebooks that can be used in Spark HDInsight clusters should not have non-ASCII characters in filenames.</span></span> <span data-ttu-id="cb3c9-136">Pokud se pokusíte odeslat soubor prostřednictvím uživatelského rozhraní Jupyter, který má název souboru jiné sady než ASCII, nebude bezobslužná (tedy Jupyter nebude umožňují nahrát soubor, ale nezpůsobí výjimku viditelné chyba buď).</span><span class="sxs-lookup"><span data-stu-id="cb3c9-136">If you try to upload a file through the Jupyter UI which has a non-ASCII filename, it will fail silently (that is, Jupyter won’t let you upload the file, but it won’t throw a visible error either).</span></span> 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a><span data-ttu-id="cb3c9-137">Chyba při načítání poznámkové bloky o větší velikosti</span><span class="sxs-lookup"><span data-stu-id="cb3c9-137">Error while loading notebooks of larger sizes</span></span>
<span data-ttu-id="cb3c9-138">Může dojít k chybě  **`Error loading notebook`**  při načtení poznámkových bloků, které jsou větší velikost.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-138">You might see an error **`Error loading notebook`** when you load notebooks that are larger in size.</span></span>  

<span data-ttu-id="cb3c9-139">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="cb3c9-139">**Mitigation:**</span></span>

<span data-ttu-id="cb3c9-140">K této chybě dojde, neznamená, že vaše data jsou poškozené nebo ztraceny.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-140">If you get this error, it does not mean your data is corrupt or lost.</span></span>  <span data-ttu-id="cb3c9-141">Poznámkové bloky jsou stále na disku v `/var/lib/jupyter`, a můžete SSH do clusteru, aby k nim přístup.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-141">Your notebooks are still on disk in `/var/lib/jupyter`, and you can SSH into the cluster to access them.</span></span> <span data-ttu-id="cb3c9-142">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="cb3c9-142">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="cb3c9-143">Jakmile se připojíte ke clusteru pomocí SSH, můžete zkopírovat poznámkové bloky z clusteru do místního počítače (pomocí spojovací bod služby nebo WinSCP) jako zálohu nedošlo ke ztrátě všech důležitých dat v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-143">Once you have connected to the cluster using SSH, you can copy your notebooks from your cluster to your local machine (using SCP or WinSCP) as a backup to prevent the loss of any important data in the notebook.</span></span> <span data-ttu-id="cb3c9-144">Pak můžete tunelového propojení SSH do vaší headnode na portu 8001 pro přístup k Jupyter bez průchodu přes bránu.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-144">You can then SSH tunnel into your headnode at port 8001 to access Jupyter without going through the gateway.</span></span>  <span data-ttu-id="cb3c9-145">Odtud můžete vymazat výstup Poznámkový blok a znovu ho uložit pro minimální velikost poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-145">From there, you can clear the output of your notebook and re-save it to minimize the notebook’s size.</span></span>

<span data-ttu-id="cb3c9-146">Chcete-li zabránit v budoucnu nedošlo k této chybě, je třeba provést některé z osvědčených postupů:</span><span class="sxs-lookup"><span data-stu-id="cb3c9-146">To prevent this error from happening in the future, you must follow some best practices:</span></span>

* <span data-ttu-id="cb3c9-147">Je důležité udržet co nejmenší velikost poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-147">It is important to keep the notebook size small.</span></span> <span data-ttu-id="cb3c9-148">Všechny výstupy z úloh Spark, která budou odeslána zpět do Jupyter je jako trvalý v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-148">Any output from your Spark jobs that is sent back to Jupyter is persisted in the notebook.</span></span>  <span data-ttu-id="cb3c9-149">Obecně je osvědčeným postupem s Jupyter předejdete spuštěná `.collect()` na velké RDD společnosti nebo dataframes; místo toho, pokud chcete prohlížet obsah RDD, zvažte spuštění `.take()` nebo `.sample()` tak, aby výstupu není získat příliš velký.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-149">It is a best practice with Jupyter in general to avoid running `.collect()` on large RDD’s or dataframes; instead, if you want to peek at an RDD’s contents, consider running `.take()` or `.sample()` so that your output doesn’t get too big.</span></span>
* <span data-ttu-id="cb3c9-150">Navíc při ukládání Poznámkový blok, zrušte všechny výstupní buněk ke snížení velikosti.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-150">Also, when you save a notebook, clear all output cells to reduce the size.</span></span>

### <a name="notebook-initial-startup-takes-longer-than-expected"></a><span data-ttu-id="cb3c9-151">Poznámkový blok počáteční spuštění trvá déle, než se očekávalo</span><span class="sxs-lookup"><span data-stu-id="cb3c9-151">Notebook initial startup takes longer than expected</span></span>
<span data-ttu-id="cb3c9-152">První příkaz kódu do poznámkového bloku Jupyter pomocí Spark magic může trvat déle než minutu.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-152">First code statement in Jupyter notebook using Spark magic could take more than a minute.</span></span>  

<span data-ttu-id="cb3c9-153">**Vysvětlení:**</span><span class="sxs-lookup"><span data-stu-id="cb3c9-153">**Explanation:**</span></span>

<span data-ttu-id="cb3c9-154">K tomu dochází, protože při spuštění první buňky kódu.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-154">This happens because when the first code cell is run.</span></span> <span data-ttu-id="cb3c9-155">Na pozadí to zahájí konfiguraci relace a Spark, SQL a jsou nastavené kontexty Hive.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-155">In the background this initiates session configuration and Spark, SQL, and Hive contexts are set.</span></span> <span data-ttu-id="cb3c9-156">Po tyto kontexty jsou nastaveny, se spustí první příkaz a díky dojem, který příkaz trvalo dlouhou dobu pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-156">After these contexts are set, the first statement is run and this gives the impression that the statement took a long time to complete.</span></span>

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a><span data-ttu-id="cb3c9-157">Časový limit Poznámkový blok Jupyter v vytvoření relace</span><span class="sxs-lookup"><span data-stu-id="cb3c9-157">Jupyter notebook timeout in creating the session</span></span>
<span data-ttu-id="cb3c9-158">Pokud Spark cluster nemá dostatek prostředků, jádrech Spark a Pyspark v poznámkovém bloku Jupyter bude časový limit pokusu o vytvoření relace.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-158">When Spark cluster is out of resources, the Spark and Pyspark kernels in the Jupyter notebook will timeout trying to create the session.</span></span> 

<span data-ttu-id="cb3c9-159">**Způsoby zmírnění rizik:**</span><span class="sxs-lookup"><span data-stu-id="cb3c9-159">**Mitigations:**</span></span> 

1. <span data-ttu-id="cb3c9-160">Uvolněte některé prostředky v clusteru Spark pomocí:</span><span class="sxs-lookup"><span data-stu-id="cb3c9-160">Free up some resources in your Spark cluster by:</span></span>
   
   * <span data-ttu-id="cb3c9-161">Zastavování jiných poznámkových bloků Spark přechodem do nabídky zavřete a zastavení nebo kliknutím na vypnutí počítače v Průzkumníku poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-161">Stopping other Spark notebooks by going to the Close and Halt menu or clicking Shutdown in the notebook explorer.</span></span>
   * <span data-ttu-id="cb3c9-162">Zastavování dalších aplikací Spark z YARN.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-162">Stopping other Spark applications from YARN.</span></span>
2. <span data-ttu-id="cb3c9-163">Restartujte poznámkového bloku, který chcete spustit.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-163">Restart the notebook you were trying to start up.</span></span> <span data-ttu-id="cb3c9-164">Musí být pro vytvoření relace nyní k dispozici dostatek prostředků.</span><span class="sxs-lookup"><span data-stu-id="cb3c9-164">Enough resources should be available for you to create a session now.</span></span>

## <a name="see-also"></a><span data-ttu-id="cb3c9-165">Viz také</span><span class="sxs-lookup"><span data-stu-id="cb3c9-165">See also</span></span>
* [<span data-ttu-id="cb3c9-166">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb3c9-166">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="cb3c9-167">Scénáře</span><span class="sxs-lookup"><span data-stu-id="cb3c9-167">Scenarios</span></span>
* [<span data-ttu-id="cb3c9-168">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="cb3c9-168">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="cb3c9-169">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="cb3c9-169">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="cb3c9-170">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="cb3c9-170">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="cb3c9-171">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="cb3c9-171">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="cb3c9-172">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb3c9-172">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="cb3c9-173">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="cb3c9-173">Create and run applications</span></span>
* [<span data-ttu-id="cb3c9-174">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="cb3c9-174">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="cb3c9-175">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="cb3c9-175">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="cb3c9-176">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="cb3c9-176">Tools and extensions</span></span>
* [<span data-ttu-id="cb3c9-177">Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="cb3c9-177">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="cb3c9-178">Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark</span><span class="sxs-lookup"><span data-stu-id="cb3c9-178">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="cb3c9-179">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb3c9-179">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="cb3c9-180">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb3c9-180">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="cb3c9-181">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="cb3c9-181">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="cb3c9-182">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="cb3c9-182">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="cb3c9-183">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="cb3c9-183">Manage resources</span></span>
* [<span data-ttu-id="cb3c9-184">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb3c9-184">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="cb3c9-185">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb3c9-185">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

