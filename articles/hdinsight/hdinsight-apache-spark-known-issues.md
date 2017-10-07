---
title: "cluster aaaTroubleshoot problémy s Apache Spark v Azure HDInsight | Microsoft Docs"
description: "Další informace o problémech souvisejících tooApache clustery Spark v Azure HDInsight a jak toowork kolem ty."
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
ms.openlocfilehash: 7373b90524ae5dbb10ab8ded593aa38d12c14b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="8169d-103">Známé problémy pro cluster Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8169d-103">Known issues for Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="8169d-104">Tento dokument uchovává informace o všech hello známé problémy pro hello verzi public preview HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="8169d-104">This document keeps track of all hello known issues for hello HDInsight Spark public preview.</span></span>  

## <a name="livy-leaks-interactive-session"></a><span data-ttu-id="8169d-105">Livy nevracení interaktivní relace.</span><span class="sxs-lookup"><span data-stu-id="8169d-105">Livy leaks interactive session</span></span>
<span data-ttu-id="8169d-106">Při Livy restartování (z Ambari nebo z důvodu restartování virtuálního počítače tooheadnode 0) k interaktivní relaci stále aktivní, bude úniku relaci interaktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="8169d-106">When Livy is restarted (from Ambari or due tooheadnode 0 virtual machine reboot) with an interactive session still alive, an interactive job session will be leaked.</span></span> <span data-ttu-id="8169d-107">Z toho důvodu může nové úlohy zasekla v automatickém hello platných stavu a nejde ho spustit.</span><span class="sxs-lookup"><span data-stu-id="8169d-107">Because of this, new jobs can stuck in hello Accepted state, and cannot be started.</span></span>

<span data-ttu-id="8169d-108">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="8169d-108">**Mitigation:**</span></span>

<span data-ttu-id="8169d-109">Použijte následující postup tooworkaround hello problém hello:</span><span class="sxs-lookup"><span data-stu-id="8169d-109">Use hello following procedure tooworkaround hello issue:</span></span>

1. <span data-ttu-id="8169d-110">SSH do headnode.</span><span class="sxs-lookup"><span data-stu-id="8169d-110">Ssh into headnode.</span></span> <span data-ttu-id="8169d-111">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8169d-111">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="8169d-112">Spuštění hello následující příkaz toofind hello aplikace ID úlohy interaktivních hello spustit prostřednictvím Livy.</span><span class="sxs-lookup"><span data-stu-id="8169d-112">Run hello following command toofind hello application IDs of hello interactive jobs started through Livy.</span></span> 
   
        yarn application –list
   
    <span data-ttu-id="8169d-113">Hello výchozí název úlohy bude Livy Pokud hello úloh bylo zahájeno pomocí Livy interaktivní relace se žádné explicitní názvy zadané pro hello Livy relaci spuštění pomocí poznámkového bloku Jupyter hello název úlohy se spustí s remotesparkmagics_ *.</span><span class="sxs-lookup"><span data-stu-id="8169d-113">hello default job names will be Livy if hello jobs were started with a Livy interactive session with no explicit names specified, For hello Livy session started by Jupyter notebook, hello job name will start with remotesparkmagics_*.</span></span> 
3. <span data-ttu-id="8169d-114">Spusťte následující příkaz tookill hello těchto úloh.</span><span class="sxs-lookup"><span data-stu-id="8169d-114">Run hello following command tookill those jobs.</span></span> 
   
        yarn application –kill <Application ID>

<span data-ttu-id="8169d-115">Nové úlohy se spustí systémem.</span><span class="sxs-lookup"><span data-stu-id="8169d-115">New jobs will start running.</span></span> 

## <a name="spark-history-server-not-started"></a><span data-ttu-id="8169d-116">Spark historie Server není spuštěn</span><span class="sxs-lookup"><span data-stu-id="8169d-116">Spark History Server not started</span></span>
<span data-ttu-id="8169d-117">Spark historie Server není spuštěn automaticky po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="8169d-117">Spark History Server is not started automatically after a cluster is created.</span></span>  

<span data-ttu-id="8169d-118">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="8169d-118">**Mitigation:**</span></span> 

<span data-ttu-id="8169d-119">Hello historie serveru spusťte ručně z Ambari.</span><span class="sxs-lookup"><span data-stu-id="8169d-119">Manually start hello history server from Ambari.</span></span>

## <a name="permission-issue-in-spark-log-directory"></a><span data-ttu-id="8169d-120">Problém s oprávněním v adresáři protokolu Spark</span><span class="sxs-lookup"><span data-stu-id="8169d-120">Permission issue in Spark log directory</span></span>
<span data-ttu-id="8169d-121">Pokud hdiuser odešle úlohu s spark-submit, dojde k chybě java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (bylo odepřeno oprávnění) a hello nebylo napsáno ovladač protokolu.</span><span class="sxs-lookup"><span data-stu-id="8169d-121">When hdiuser submits a job with spark-submit, there is an error java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied) and hello driver log is not written.</span></span> 

<span data-ttu-id="8169d-122">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="8169d-122">**Mitigation:**</span></span>

1. <span data-ttu-id="8169d-123">Přidejte skupinu Hadoop toohello hdiuser.</span><span class="sxs-lookup"><span data-stu-id="8169d-123">Add hdiuser toohello Hadoop group.</span></span> 
2. <span data-ttu-id="8169d-124">Zadejte 777 oprávnění na /var/log/spark až po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="8169d-124">Provide 777 permissions on /var/log/spark after cluster creation.</span></span> 
3. <span data-ttu-id="8169d-125">Aktualizujte umístění protokolu hello spark pomocí Ambari toobe adresář 777 oprávnění.</span><span class="sxs-lookup"><span data-stu-id="8169d-125">Update hello spark log location using Ambari toobe a directory with 777 permissions.</span></span>  
4. <span data-ttu-id="8169d-126">Spustit spark odeslat jako sudo.</span><span class="sxs-lookup"><span data-stu-id="8169d-126">Run spark-submit as sudo.</span></span>  

## <a name="spark-phoenix-connector-is-not-supported"></a><span data-ttu-id="8169d-127">Spark Phoenix konektor není podporovaný.</span><span class="sxs-lookup"><span data-stu-id="8169d-127">Spark-Phoenix connector is not supported</span></span>

<span data-ttu-id="8169d-128">V současné době hello Spark Phoenix konektor není podporovaný s clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="8169d-128">Currently, hello Spark-Phoenix connector is not supported with an HDInsight Spark cluster.</span></span>

<span data-ttu-id="8169d-129">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="8169d-129">**Mitigation:**</span></span>

<span data-ttu-id="8169d-130">Místo toho musíte použít konektor Spark HBase hello.</span><span class="sxs-lookup"><span data-stu-id="8169d-130">You must use hello Spark-HBase connector instead.</span></span> <span data-ttu-id="8169d-131">Pokyny naleznete v části [jak konektor toouse Spark HBase](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span><span class="sxs-lookup"><span data-stu-id="8169d-131">For instructions see [How toouse Spark-HBase connector](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).</span></span>

## <a name="issues-related-toojupyter-notebooks"></a><span data-ttu-id="8169d-132">Problémy související s tooJupyter poznámkových bloků</span><span class="sxs-lookup"><span data-stu-id="8169d-132">Issues related tooJupyter notebooks</span></span>
<span data-ttu-id="8169d-133">Toto jsou některé známé problémy související tooJupyter notebooks.</span><span class="sxs-lookup"><span data-stu-id="8169d-133">Following are some known issues related tooJupyter notebooks.</span></span>

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a><span data-ttu-id="8169d-134">Poznámkové bloky s jiné znaky než ASCII v názvech souborů</span><span class="sxs-lookup"><span data-stu-id="8169d-134">Notebooks with non-ASCII characters in filenames</span></span>
<span data-ttu-id="8169d-135">Jupyter notebooks, které mohou být používány clustery Spark HDInsight by neměl mít jiné znaky než ASCII v názvech souborů.</span><span class="sxs-lookup"><span data-stu-id="8169d-135">Jupyter notebooks that can be used in Spark HDInsight clusters should not have non-ASCII characters in filenames.</span></span> <span data-ttu-id="8169d-136">Pokud se pokusíte tooupload soubor prostřednictvím hello Jupyter uživatelského rozhraní, která má název souboru jiné sady než ASCII, se nezdaří bez upozornění (tedy Jupyter nebude umožňují nahrát soubor hello, ale nezpůsobí výjimku viditelné chyba buď).</span><span class="sxs-lookup"><span data-stu-id="8169d-136">If you try tooupload a file through hello Jupyter UI which has a non-ASCII filename, it will fail silently (that is, Jupyter won’t let you upload hello file, but it won’t throw a visible error either).</span></span> 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a><span data-ttu-id="8169d-137">Chyba při načítání poznámkové bloky o větší velikosti</span><span class="sxs-lookup"><span data-stu-id="8169d-137">Error while loading notebooks of larger sizes</span></span>
<span data-ttu-id="8169d-138">Může dojít k chybě  **`Error loading notebook`**  při načtení poznámkových bloků, které jsou větší velikost.</span><span class="sxs-lookup"><span data-stu-id="8169d-138">You might see an error **`Error loading notebook`** when you load notebooks that are larger in size.</span></span>  

<span data-ttu-id="8169d-139">**Omezení rizik:**</span><span class="sxs-lookup"><span data-stu-id="8169d-139">**Mitigation:**</span></span>

<span data-ttu-id="8169d-140">K této chybě dojde, neznamená, že vaše data jsou poškozené nebo ztraceny.</span><span class="sxs-lookup"><span data-stu-id="8169d-140">If you get this error, it does not mean your data is corrupt or lost.</span></span>  <span data-ttu-id="8169d-141">Poznámkové bloky jsou stále na disku v `/var/lib/jupyter`, a můžete SSH do clusteru tooaccess hello je.</span><span class="sxs-lookup"><span data-stu-id="8169d-141">Your notebooks are still on disk in `/var/lib/jupyter`, and you can SSH into hello cluster tooaccess them.</span></span> <span data-ttu-id="8169d-142">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8169d-142">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="8169d-143">Jakmile se připojíte toohello clusteru pomocí protokolu SSH, můžete zkopírovat poznámkové bloky z clusteru tooyour místního počítače (pomocí spojovací bod služby nebo WinSCP) jako ztrátu hello zálohování tooprevent žádné důležitých dat v poznámkovém bloku hello.</span><span class="sxs-lookup"><span data-stu-id="8169d-143">Once you have connected toohello cluster using SSH, you can copy your notebooks from your cluster tooyour local machine (using SCP or WinSCP) as a backup tooprevent hello loss of any important data in hello notebook.</span></span> <span data-ttu-id="8169d-144">Pak můžete tunelového propojení SSH do vaší headnode na portu 8001 tooaccess Jupyter bez průchodu přes bránu hello.</span><span class="sxs-lookup"><span data-stu-id="8169d-144">You can then SSH tunnel into your headnode at port 8001 tooaccess Jupyter without going through hello gateway.</span></span>  <span data-ttu-id="8169d-145">Odtud můžete vymazat výstup hello v poznámkovém bloku a uložte ji znovu notebooky hello toominimize velikost.</span><span class="sxs-lookup"><span data-stu-id="8169d-145">From there, you can clear hello output of your notebook and re-save it toominimize hello notebook’s size.</span></span>

<span data-ttu-id="8169d-146">tooprevent nedocházelo v hello budoucí, je třeba postupovat podle doporučených postupů, které tato chyba:</span><span class="sxs-lookup"><span data-stu-id="8169d-146">tooprevent this error from happening in hello future, you must follow some best practices:</span></span>

* <span data-ttu-id="8169d-147">Je důležité tookeep hello poznámkového bloku velikost malé.</span><span class="sxs-lookup"><span data-stu-id="8169d-147">It is important tookeep hello notebook size small.</span></span> <span data-ttu-id="8169d-148">Všechny výstupy z úloh Spark, je odeslána zpět tooJupyter je uchován v hello Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="8169d-148">Any output from your Spark jobs that is sent back tooJupyter is persisted in hello notebook.</span></span>  <span data-ttu-id="8169d-149">Je osvědčeným postupem s Jupyter v obecné tooavoid systémem `.collect()` na velké RDD společnosti nebo dataframes; místo toho, pokud chcete toopeek na obsah RDD, zvažte spuštění `.take()` nebo `.sample()` tak, aby výstupu není získat příliš velký.</span><span class="sxs-lookup"><span data-stu-id="8169d-149">It is a best practice with Jupyter in general tooavoid running `.collect()` on large RDD’s or dataframes; instead, if you want toopeek at an RDD’s contents, consider running `.take()` or `.sample()` so that your output doesn’t get too big.</span></span>
* <span data-ttu-id="8169d-150">Navíc při ukládání Poznámkový blok, zrušte všechny výstupní velikost hello tooreduce buněk.</span><span class="sxs-lookup"><span data-stu-id="8169d-150">Also, when you save a notebook, clear all output cells tooreduce hello size.</span></span>

### <a name="notebook-initial-startup-takes-longer-than-expected"></a><span data-ttu-id="8169d-151">Poznámkový blok počáteční spuštění trvá déle, než se očekávalo</span><span class="sxs-lookup"><span data-stu-id="8169d-151">Notebook initial startup takes longer than expected</span></span>
<span data-ttu-id="8169d-152">První příkaz kódu do poznámkového bloku Jupyter pomocí Spark magic může trvat déle než minutu.</span><span class="sxs-lookup"><span data-stu-id="8169d-152">First code statement in Jupyter notebook using Spark magic could take more than a minute.</span></span>  

<span data-ttu-id="8169d-153">**Vysvětlení:**</span><span class="sxs-lookup"><span data-stu-id="8169d-153">**Explanation:**</span></span>

<span data-ttu-id="8169d-154">K tomu dochází, protože při spuštění první buňky kódu hello.</span><span class="sxs-lookup"><span data-stu-id="8169d-154">This happens because when hello first code cell is run.</span></span> <span data-ttu-id="8169d-155">V pozadí hello inicializováno konfiguraci relace a Spark, SQL a jsou nastavené kontexty Hive.</span><span class="sxs-lookup"><span data-stu-id="8169d-155">In hello background this initiates session configuration and Spark, SQL, and Hive contexts are set.</span></span> <span data-ttu-id="8169d-156">Po nastavení jsou tyto kontexty, první příkaz hello běží a díky tomu hello dojem, že příkaz hello trvalo dlouhou dobu toocomplete.</span><span class="sxs-lookup"><span data-stu-id="8169d-156">After these contexts are set, hello first statement is run and this gives hello impression that hello statement took a long time toocomplete.</span></span>

### <a name="jupyter-notebook-timeout-in-creating-hello-session"></a><span data-ttu-id="8169d-157">Časový limit poznámkového bloku Jupyter při vytváření hello relace</span><span class="sxs-lookup"><span data-stu-id="8169d-157">Jupyter notebook timeout in creating hello session</span></span>
<span data-ttu-id="8169d-158">Pokud Spark cluster nemá dostatek prostředků, hello Spark a jádra Pyspark v hello Poznámkový blok Jupyter bude časový limit pokusu o toocreate hello relace.</span><span class="sxs-lookup"><span data-stu-id="8169d-158">When Spark cluster is out of resources, hello Spark and Pyspark kernels in hello Jupyter notebook will timeout trying toocreate hello session.</span></span> 

<span data-ttu-id="8169d-159">**Způsoby zmírnění rizik:**</span><span class="sxs-lookup"><span data-stu-id="8169d-159">**Mitigations:**</span></span> 

1. <span data-ttu-id="8169d-160">Uvolněte některé prostředky v clusteru Spark pomocí:</span><span class="sxs-lookup"><span data-stu-id="8169d-160">Free up some resources in your Spark cluster by:</span></span>
   
   * <span data-ttu-id="8169d-161">Zastavování jiných poznámkových bloků Spark budete toohello zavřete a zastavení nabídky nebo v hello poznámkového bloku Exploreru kliknete na vypnutí.</span><span class="sxs-lookup"><span data-stu-id="8169d-161">Stopping other Spark notebooks by going toohello Close and Halt menu or clicking Shutdown in hello notebook explorer.</span></span>
   * <span data-ttu-id="8169d-162">Zastavování dalších aplikací Spark z YARN.</span><span class="sxs-lookup"><span data-stu-id="8169d-162">Stopping other Spark applications from YARN.</span></span>
2. <span data-ttu-id="8169d-163">Restartujte hello Poznámkový blok, které jste se pokoušeli toostart nahoru.</span><span class="sxs-lookup"><span data-stu-id="8169d-163">Restart hello notebook you were trying toostart up.</span></span> <span data-ttu-id="8169d-164">Dostatek prostředků, musí být k dispozici pro toocreate můžete nyní relaci.</span><span class="sxs-lookup"><span data-stu-id="8169d-164">Enough resources should be available for you toocreate a session now.</span></span>

## <a name="see-also"></a><span data-ttu-id="8169d-165">Viz také</span><span class="sxs-lookup"><span data-stu-id="8169d-165">See also</span></span>
* [<span data-ttu-id="8169d-166">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8169d-166">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="8169d-167">Scénáře</span><span class="sxs-lookup"><span data-stu-id="8169d-167">Scenarios</span></span>
* [<span data-ttu-id="8169d-168">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="8169d-168">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="8169d-169">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="8169d-169">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="8169d-170">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="8169d-170">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="8169d-171">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="8169d-171">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="8169d-172">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8169d-172">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="8169d-173">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="8169d-173">Create and run applications</span></span>
* [<span data-ttu-id="8169d-174">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="8169d-174">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="8169d-175">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="8169d-175">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="8169d-176">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="8169d-176">Tools and extensions</span></span>
* [<span data-ttu-id="8169d-177">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="8169d-177">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="8169d-178">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="8169d-178">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="8169d-179">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8169d-179">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="8169d-180">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="8169d-180">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="8169d-181">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="8169d-181">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="8169d-182">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="8169d-182">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="8169d-183">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="8169d-183">Manage resources</span></span>
* [<span data-ttu-id="8169d-184">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8169d-184">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="8169d-185">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8169d-185">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

