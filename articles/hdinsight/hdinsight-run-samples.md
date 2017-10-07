---
title: "Ukázky aaaRun hello Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Začněte pomocí služby Azure HDInsight hello hello ukázky zadat. Použití skriptů prostředí PowerShell, které spouštět programy MapReduce na clustery s daty."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bf76d452-abb4-4210-87bd-a2067778c6ed
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 544856a2cdfe5154cbd9bf1fb05db081af86cd46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a><span data-ttu-id="f7617-104">Spuštění ukázky MapReduce s Hadoop v HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="f7617-104">Run Hadoop MapReduce samples in Windows-based HDInsight</span></span>
[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

<span data-ttu-id="f7617-105">Sadu vzorků, které jsou k dispozici toohelp, které můžete začít a spustit úlohy MapReduce na clusterů systému Hadoop pomocí Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f7617-105">A set of samples are provided toohelp you get started running MapReduce jobs on Hadoop clusters using Azure HDInsight.</span></span> <span data-ttu-id="f7617-106">Tyto ukázky jsou k dispozici na každém hello HDInsight spravované clustery, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="f7617-106">These samples are made available on each of hello HDInsight managed clusters that you create.</span></span> <span data-ttu-id="f7617-107">Spuštění tyto ukázky vám seznámit se s použitím úlohy toorun rutin prostředí Azure PowerShell na clusterů systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="f7617-107">Running these samples familiarize you with using Azure PowerShell cmdlets toorun jobs on Hadoop clusters.</span></span>

* <span data-ttu-id="f7617-108">[**Aplikace Word počet**][hdinsight-sample-wordcount]: počty výskytů slova v textovém souboru.</span><span class="sxs-lookup"><span data-stu-id="f7617-108">[**Word count**][hdinsight-sample-wordcount]: Counts word occurrences in a text file.</span></span>
* <span data-ttu-id="f7617-109">[**C# streamování počet slov**][hdinsight-sample-csharp-streaming]: počty výskytů slova v textového souboru pomocí hello streamování rozhraní Hadoop.</span><span class="sxs-lookup"><span data-stu-id="f7617-109">[**C# streaming word count**][hdinsight-sample-csharp-streaming]: Counts word occurrences in a text file using hello Hadoop streaming interface.</span></span>
* <span data-ttu-id="f7617-110">[**Pi odhadu**][hdinsight-sample-pi-estimator]: používá statistické (jako Monte Carlo) metoda tooestimate hello hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="f7617-110">[**Pi estimator**][hdinsight-sample-pi-estimator]: Uses a statistical (quasi-Monte Carlo) method tooestimate hello value of pi.</span></span>
* <span data-ttu-id="f7617-111">[**10 GB Graysort**][hdinsight-sample-10gb-graysort]: spuštění pro obecné účely GraySort na 10 GB souboru pomocí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f7617-111">[**10-GB Graysort**][hdinsight-sample-10gb-graysort]: Run a general-purpose GraySort on a 10 GB file by using HDInsight.</span></span> <span data-ttu-id="f7617-112">Existují tři úlohy toorun: Teragen toogenerate hello data, Terasort toosort hello data a Teravalidate tooconfirm správně seřadit hello data.</span><span class="sxs-lookup"><span data-stu-id="f7617-112">There are three jobs toorun: Teragen toogenerate hello data, Terasort toosort hello data, and Teravalidate tooconfirm that hello data has been properly sorted.</span></span>

> [!NOTE]
> <span data-ttu-id="f7617-113">Hello zdrojový kód najdete v příloze hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-113">hello source code can be found in hello Appendix.</span></span>

<span data-ttu-id="f7617-114">Existuje mnohem další dokumentaci na webu hello pro technologie související s Hadoop, jako je například programování založené na jazyce Java MapReduce, streamování a dokumentaci o rutinách hello, které se používají v prostředí Windows PowerShell skriptování.</span><span class="sxs-lookup"><span data-stu-id="f7617-114">Much additional documentation exists on hello web for Hadoop-related technologies, such as Java-based MapReduce programming and streaming, and documentation about hello cmdlets that are used in Windows PowerShell scripting.</span></span> <span data-ttu-id="f7617-115">Další informace o těchto prostředků najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="f7617-115">For more information about these resources, see:</span></span>

* [<span data-ttu-id="f7617-116">Vývoj aplikací Java MapReduce pro Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f7617-116">Develop Java MapReduce programs for Hadoop in HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)
* [<span data-ttu-id="f7617-117">Odesílání úloh Hadoop do služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="f7617-117">Submit Hadoop jobs in HDInsight</span></span>](hdinsight-submit-hadoop-jobs-programmatically.md)
* <span data-ttu-id="f7617-118">[Úvod tooAzure HDInsight][hdinsight-introduction]</span><span class="sxs-lookup"><span data-stu-id="f7617-118">[Introduction tooAzure HDInsight][hdinsight-introduction]</span></span>

<span data-ttu-id="f7617-119">Spousta lidí v současné době zvolte Hive a Pig přes MapReduce.</span><span class="sxs-lookup"><span data-stu-id="f7617-119">Nowadays, many people choose Hive and Pig over MapReduce.</span></span>  <span data-ttu-id="f7617-120">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="f7617-120">For more information, see:</span></span>

* [<span data-ttu-id="f7617-121">Používání Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f7617-121">Use Hive in HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="f7617-122">Použijte Pig v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f7617-122">Use Pig in HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="f7617-123">**Požadavky**:</span><span class="sxs-lookup"><span data-stu-id="f7617-123">**Prerequisites**:</span></span>

* <span data-ttu-id="f7617-124">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="f7617-124">**An Azure subscription**.</span></span> <span data-ttu-id="f7617-125">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="f7617-125">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="f7617-126">**cluster služby HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="f7617-126">**an HDInsight cluster**.</span></span> <span data-ttu-id="f7617-127">Pokyny hello různé způsoby, ve kterých lze vytvořit tyto clustery najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="f7617-127">For instructions on hello various ways in which such clusters can be created, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="f7617-128">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="f7617-128">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f7617-129">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a 1. ledna 2017 dojde k jejímu odebrání.</span><span class="sxs-lookup"><span data-stu-id="f7617-129">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and will be removed by January 1, 2017.</span></span> <span data-ttu-id="f7617-130">kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f7617-130">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="f7617-131">Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7617-131">Follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="f7617-132">Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="f7617-132">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md).</span></span>

## <span data-ttu-id="f7617-133"><a name="hdinsight-sample-wordcount"></a>Počet - Java aplikace Word</span><span class="sxs-lookup"><span data-stu-id="f7617-133"><a name="hdinsight-sample-wordcount"></a>Word count - Java</span></span>
<span data-ttu-id="f7617-134">toosubmit projektu MapReduce, nejdřív vytvořit definici úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="f7617-134">toosubmit a MapReduce project, you first create a MapReduce job definition.</span></span> <span data-ttu-id="f7617-135">V definici úlohy hello, zadáte soubor jar programu hello MapReduce a hello umístění soubor jar hello, což je **wasb:///example/jars/hadoop-mapreduce-examples.jar**hello název třídy a hello argumenty.</span><span class="sxs-lookup"><span data-stu-id="f7617-135">In hello job definition, you specify hello MapReduce program jar file and hello location of hello jar file, which is **wasb:///example/jars/hadoop-mapreduce-examples.jar**, hello class name, and hello arguments.</span></span>  <span data-ttu-id="f7617-136">Hello wordcount MapReduce program má dva argumenty: hello zdrojový soubor, který je použité toocount slova a hello umístění pro výstup.</span><span class="sxs-lookup"><span data-stu-id="f7617-136">hello wordcount MapReduce program takes two arguments: hello source file that is used toocount words, and hello location for output.</span></span>

<span data-ttu-id="f7617-137">Hello zdrojový kód najdete v hello [příloha A](#apendix-a---the-word-count-MapReduce-program-in-java).</span><span class="sxs-lookup"><span data-stu-id="f7617-137">hello source code can be found in hello [Appendix A](#apendix-a---the-word-count-MapReduce-program-in-java).</span></span>

<span data-ttu-id="f7617-138">Pro proceduru hello vývoje Java MapReduce programu, najdete v části - [vyvíjet MapReduce Java programy pro Hadoop v HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)</span><span class="sxs-lookup"><span data-stu-id="f7617-138">For hello procedure of developing a Java MapReduce program, see - [Develop Java MapReduce programs for Hadoop in HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)</span></span>

<span data-ttu-id="f7617-139">**toosubmit úlohu MapReduce počet aplikace word**</span><span class="sxs-lookup"><span data-stu-id="f7617-139">**toosubmit a word count MapReduce job**</span></span>

1. <span data-ttu-id="f7617-140">Otevřete **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="f7617-140">Open **Windows PowerShell ISE**.</span></span> <span data-ttu-id="f7617-141">Pokyny najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell][powershell-install-configure].</span><span class="sxs-lookup"><span data-stu-id="f7617-141">For instructions, see [Install and configure Azure PowerShell][powershell-install-configure].</span></span>
2. <span data-ttu-id="f7617-142">Vložte následující skript prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="f7617-142">Paste hello following PowerShell script:</span></span>

    ```powershell
    $subscriptionName = "<Azure Subscription Name>"
    $resourceGroupName = "<Resource Group Name>"
    $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name

    Select-AzureRmSubscription -SubscriptionName $subscriptionName

    # Define hello MapReduce job
    $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "wordcount" `
                                -Arguments "wasb:///example/data/gutenberg/davinci.txt", "wasb:///example/data/WordCountOutput"

    # Submit hello job and wait for job completion
    $cred = Get-Credential -Message "Enter hello HDInsight cluster HTTP user credential:"
    $mrJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $clusterName `
                        -HttpCredential $cred `
                        -JobDefinition $mrJobDefinition

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -JobId $mrJob.JobId

    # Get hello job output
    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -DefaultStorageAccountName $defaultStorageAccount `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultStorageContainer  `
        -JobId $mrJob.JobId `
        -DisplayOutputType StandardError

    # Download hello job output toohello workstation
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force

    # Display hello output file
    cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"
    ```

    <span data-ttu-id="f7617-143">Hello úlohu MapReduce vytvoří soubor s názvem *část r-00000*, který obsahuje slova, a počty hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-143">hello MapReduce job produces a file named *part-r-00000*, which contains words and hello counts.</span></span> <span data-ttu-id="f7617-144">skript Hello používá hello **findstr** příkaz toolist všechny hello slova, která obsahuje *"existuje"*.</span><span class="sxs-lookup"><span data-stu-id="f7617-144">hello script uses hello **findstr** command toolist all hello words that contains *"there"*.</span></span>
3. <span data-ttu-id="f7617-145">Nastavení hello prvních tří proměnných a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-145">Set hello first three variables, and run hello script.</span></span>

## <span data-ttu-id="f7617-146"><a name="hdinsight-sample-csharp-streaming"></a>Aplikace Word počet - streamování C#</span><span class="sxs-lookup"><span data-stu-id="f7617-146"><a name="hdinsight-sample-csharp-streaming"></a>Word count - C# streaming</span></span>
<span data-ttu-id="f7617-147">Hadoop poskytuje streamování tooMapReduce rozhraní API, která umožňuje toowrite mapy a omezit funkce v jiných jazyků než Java.</span><span class="sxs-lookup"><span data-stu-id="f7617-147">Hadoop provides a streaming API tooMapReduce, which enables you toowrite map and reduce functions in languages other than Java.</span></span>

> [!NOTE]
> <span data-ttu-id="f7617-148">Hello kroky v tomto kurzu použít pouze na základě tooWindows clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f7617-148">hello steps in this tutorial apply only tooWindows-based HDInsight clusters.</span></span> <span data-ttu-id="f7617-149">Příklad streamování pro clustery HDInsight se systémem Linux naleznete v části [vyvíjet Python streamování programy pro HDInsight](hdinsight-hadoop-streaming-python.md).</span><span class="sxs-lookup"><span data-stu-id="f7617-149">For an example of streaming for Linux-based HDInsight clusters, see [Develop Python streaming programs for HDInsight](hdinsight-hadoop-streaming-python.md).</span></span>

<span data-ttu-id="f7617-150">V příkladu hello, hello mapper a hello reduktorem jsou spustitelné soubory, které číst hello vstup z [stdin –] [ stdin-stdout-stderr] (řádek po řádku) a výstup hello příliš[stdout] [stdin-stdout-stderr].</span><span class="sxs-lookup"><span data-stu-id="f7617-150">In hello example, hello mapper and hello reducer are executables that read hello input from [stdin][stdin-stdout-stderr] (line-by-line) and emit hello output too[stdout][stdin-stdout-stderr].</span></span> <span data-ttu-id="f7617-151">Hello program počty všechna hello slova v textu hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-151">hello program counts all hello words in hello text.</span></span>

<span data-ttu-id="f7617-152">Pokud je zadán parametr spustitelný soubor pro **mappers**, při inicializaci hello mapper spustí každý úkol mapper hello spustitelného souboru jako samostatný proces.</span><span class="sxs-lookup"><span data-stu-id="f7617-152">When an executable is specified for **mappers**, each mapper task launches hello executable as a separate process when hello mapper is initialized.</span></span> <span data-ttu-id="f7617-153">Spuštění úlohy mapper hello, se převede vstupní řádky a kanály hello toohello řádky [stdin –] [ stdin-stdout-stderr] hello procesu.</span><span class="sxs-lookup"><span data-stu-id="f7617-153">As hello mapper task runs, it converts its input into lines, and feeds hello lines toohello [stdin][stdin-stdout-stderr] of hello process.</span></span>

<span data-ttu-id="f7617-154">V hello té doby hello mapper shromažďuje hello řádkový výstup z výstupu stdout hello hello procesu.</span><span class="sxs-lookup"><span data-stu-id="f7617-154">In hello meantime, hello mapper collects hello line-oriented output from hello stdout of hello process.</span></span> <span data-ttu-id="f7617-155">Každý řádek je převede na dvojice klíč/hodnota, která se shromažďují jako výstup hello hello mapper.</span><span class="sxs-lookup"><span data-stu-id="f7617-155">It converts each line into a key/value pair, which is collected as hello output of hello mapper.</span></span> <span data-ttu-id="f7617-156">Ve výchozím nastavení předpona hello řádku až toohello první kartě znak je hello klíč a hello zbytek hello řádku (s výjimkou hello znak tabulátoru) je hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-156">By default, hello prefix of a line up toohello first Tab character is hello key, and hello remainder of hello line (excluding hello Tab character) is hello value.</span></span> <span data-ttu-id="f7617-157">Pokud neexistuje žádný karta znak v řádku hello, považuje za celý řádek jako hello klíč a hodnotu hello má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="f7617-157">If there is no Tab character in hello line, entire line is considered as hello key, and hello value is null.</span></span>

<span data-ttu-id="f7617-158">Pokud je zadán parametr spustitelný soubor pro **přechodky**, při inicializaci hello reduktorem spustí každý úkol reduktorem hello spustitelného souboru jako samostatný proces.</span><span class="sxs-lookup"><span data-stu-id="f7617-158">When an executable is specified for **reducers**, each reducer task launches hello executable as a separate process when hello reducer is initialized.</span></span> <span data-ttu-id="f7617-159">Spuštění úlohy reduktorem hello, převede ji jeho páry vstupní klíč/hodnota do řádky a ho kanály hello řádky toohello [stdin –] [ stdin-stdout-stderr] hello procesu.</span><span class="sxs-lookup"><span data-stu-id="f7617-159">As hello reducer task runs, it converts its input key/values pairs into lines, and it feeds hello lines toohello [stdin][stdin-stdout-stderr] of hello process.</span></span>

<span data-ttu-id="f7617-160">V hello té doby hello reduktorem shromažďuje hello řádkový výstup z hello [stdout] [ stdin-stdout-stderr] hello procesu.</span><span class="sxs-lookup"><span data-stu-id="f7617-160">In hello meantime, hello reducer collects hello line-oriented output from hello [stdout][stdin-stdout-stderr] of hello process.</span></span> <span data-ttu-id="f7617-161">Převede každého řádku tooa dvojice klíč/hodnota, která se shromažďují jako výstup hello reduktorem hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-161">It converts each line tooa key/value pair, which is collected as hello output of hello reducer.</span></span> <span data-ttu-id="f7617-162">Ve výchozím nastavení předpona hello řádku až toohello první kartě znak je hello klíč a hello zbytek hello řádku (s výjimkou hello znak tabulátoru) je hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-162">By default, hello prefix of a line up toohello first Tab character is hello key, and hello remainder of hello line (excluding hello Tab character) is hello value.</span></span>

<span data-ttu-id="f7617-163">**počet úlohu streamování word toosubmit C#**</span><span class="sxs-lookup"><span data-stu-id="f7617-163">**toosubmit a C# streaming word count job**</span></span>

* <span data-ttu-id="f7617-164">Postupujte podle postupu hello v [aplikace Word počet - Java](#word-count-java)a nahraďte definici úlohy hello hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="f7617-164">Follow hello procedure in [Word count - Java](#word-count-java), and replace hello job definition with hello following line:</span></span>

    ```powershell
    $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                            -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                            -Mapper "cat.exe" `
                            -Reducer "wc.exe" `
                            -InputPath "/example/data/gutenberg/davinci.txt" `
                            -OutputPath "/example/data/StreamingOutput/wc.txt"
    ```

    <span data-ttu-id="f7617-165">Hello výstupní soubor musí být:</span><span class="sxs-lookup"><span data-stu-id="f7617-165">hello output file shall be:</span></span>

        example/data/StreamingOutput/wc.txt/part-00000

## <span data-ttu-id="f7617-166"><a name="hdinsight-sample-pi-estimator"></a>PI odhadu</span><span class="sxs-lookup"><span data-stu-id="f7617-166"><a name="hdinsight-sample-pi-estimator"></a>PI estimator</span></span>
<span data-ttu-id="f7617-167">pi odhadu Hello používá statistické (jako Monte Carlo) metoda tooestimate hello hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="f7617-167">hello pi estimator uses a statistical (quasi-Monte Carlo) method tooestimate hello value of pi.</span></span> <span data-ttu-id="f7617-168">Body, které jsou umístěny v náhodných uvnitř jednotku čtvercovou také spadal do kruh zapsány v rámci této hranaté s prostorem rovna toohello pravděpodobnosti hello kruh pí/4.</span><span class="sxs-lookup"><span data-stu-id="f7617-168">Points placed at random inside of a unit square also fall within a circle inscribed within that square with a probability equal toohello area of hello circle, pi/4.</span></span> <span data-ttu-id="f7617-169">z hodnoty hello 4R, kde R je poměr hello hello počet bodů, které jsou uvnitř hello kruh toohello celkový počet bodů, které jsou v rámci hranaté hello se dá odhadnout Hello hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="f7617-169">hello value of pi can be estimated from hello value of 4R, where R is hello ratio of hello number of points that are inside hello circle toohello total number of points that are within hello square.</span></span> <span data-ttu-id="f7617-170">Hello větší hello ukázka bodů použít, je lepší odhad hello hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-170">hello larger hello sample of points used, hello better hello estimate is.</span></span>

<span data-ttu-id="f7617-171">zadaná pro tento ukázkový skript Hello odešle úlohu jar Hadoop a nastavení toorun s hodnotou 16 maps, z nichž každý je požadovaná toocompute 10 milionů ukázka bodů pomocí hodnoty parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-171">hello script provided for this sample submits a Hadoop jar job and is set up toorun with a value 16 maps, each of which is required toocompute 10 million sample points by hello parameter values.</span></span> <span data-ttu-id="f7617-172">Tyto parametr hodnoty lze změnit tooimprove hello odhadované hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="f7617-172">These parameter values can be changed tooimprove hello estimated value of pi.</span></span> <span data-ttu-id="f7617-173">Pro referenci hello prvních 10 desetinných míst pí jsou 3.1415926535.</span><span class="sxs-lookup"><span data-stu-id="f7617-173">For reference, hello first 10 decimal places of pi are 3.1415926535.</span></span>

<span data-ttu-id="f7617-174">**toosubmit úlohu odhadu platformy**</span><span class="sxs-lookup"><span data-stu-id="f7617-174">**toosubmit a pi estimator job**</span></span>

* <span data-ttu-id="f7617-175">Postupujte podle postupu hello v [aplikace Word počet - Java](#word-count-java)a nahraďte definici úlohy hello hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="f7617-175">Follow hello procedure in [Word count - Java](#word-count-java), and replace hello job definition with hello following line:</span></span>

    ```powershell
    $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "pi" `
                                -Arguments "16", "10000000"
    ```

## <span data-ttu-id="f7617-176"><a name="hdinsight-sample-10gb-graysort"></a>Graysort 10 GB</span><span class="sxs-lookup"><span data-stu-id="f7617-176"><a name="hdinsight-sample-10gb-graysort"></a>10-GB Graysort</span></span>
<span data-ttu-id="f7617-177">Tato ukázka používá mírné 10GB dat tak, aby mohl být program spuštěn poměrně rychle.</span><span class="sxs-lookup"><span data-stu-id="f7617-177">This sample uses a modest 10GB of data so that it can be run relatively quickly.</span></span> <span data-ttu-id="f7617-178">Používá hello MapReduce aplikace vyvinuté tak, že Owen O'Malley a Arun Murthy získané hello roční pro obecné účely ("daytona") terabajt řazení srovnávací test v 2009 míru 0.578 TB za minutu (100 TB 173 minut).</span><span class="sxs-lookup"><span data-stu-id="f7617-178">It uses hello MapReduce applications developed by Owen O'Malley and Arun Murthy that won hello annual general-purpose ("daytona") terabyte sort benchmark in 2009 with a rate of 0.578TB/min (100TB in 173 minutes).</span></span> <span data-ttu-id="f7617-179">Další informace o tomto a dalších řazení srovnávacích testů najdete v tématu hello [Sortbenchmark](http://sortbenchmark.org/) lokality.</span><span class="sxs-lookup"><span data-stu-id="f7617-179">For more information on this and other sorting benchmarks, see hello [Sortbenchmark](http://sortbenchmark.org/) site.</span></span>

<span data-ttu-id="f7617-180">Tato ukázka používá tři sady MapReduce programů:</span><span class="sxs-lookup"><span data-stu-id="f7617-180">This sample uses three sets of MapReduce programs:</span></span>

1. <span data-ttu-id="f7617-181">**TeraGen** je program MapReduce, které můžete použít toogenerate hello řádky data toosort.</span><span class="sxs-lookup"><span data-stu-id="f7617-181">**TeraGen** is a MapReduce program that you can use toogenerate hello rows of data toosort.</span></span>
2. <span data-ttu-id="f7617-182">**TeraSort** ukázky hello vstupní data a používá MapReduce toosort hello data do celkové pořadí.</span><span class="sxs-lookup"><span data-stu-id="f7617-182">**TeraSort** samples hello input data and uses MapReduce toosort hello data into a total order.</span></span> <span data-ttu-id="f7617-183">TeraSort je standardní řazení funkcí MapReduce, s výjimkou vlastní dělicí metody, která používá seřazený seznam klíčů N-1 vzorků, které definují hello klíče rozsah pro každý snižte.</span><span class="sxs-lookup"><span data-stu-id="f7617-183">TeraSort is a standard sort of MapReduce functions, except for a custom partitioner that uses a sorted list of N-1 sampled keys that define hello key range for each reduce.</span></span> <span data-ttu-id="f7617-184">Konkrétně, všechny klíče takové které ukázkové [i-1] < = klíč < ukázka [i] jsou odesílány tooreduce i.</span><span class="sxs-lookup"><span data-stu-id="f7617-184">In particular, all keys such that sample[i-1] <= key < sample[i] are sent tooreduce i.</span></span> <span data-ttu-id="f7617-185">To zaručuje, že hello výstupy snížit i jsou všechny menší než výstup hello snížit i + 1.</span><span class="sxs-lookup"><span data-stu-id="f7617-185">This guarantees that hello outputs of reduce i are all less than hello output of reduce i+1.</span></span>
3. <span data-ttu-id="f7617-186">**TeraValidate** je globálně MapReduce program, který ověří tento výstup hello řazena.</span><span class="sxs-lookup"><span data-stu-id="f7617-186">**TeraValidate** is a MapReduce program that validates that hello output is globally sorted.</span></span> <span data-ttu-id="f7617-187">Vytvoří jedna mapa každý soubor v adresáři výstup hello a každé mapování zajišťuje, že každý klíč je menší než nebo rovna toohello předchozí.</span><span class="sxs-lookup"><span data-stu-id="f7617-187">It creates one map per file in hello output directory, and each map ensures that each key is less than or equal toohello previous one.</span></span> <span data-ttu-id="f7617-188">Hello mapy funkce také generuje záznamy hello první a poslední klíče každého souboru a hello reduce – funkce zajišťuje, že hello první klíč souboru i je větší než hello poslední klíč i-1 souboru.</span><span class="sxs-lookup"><span data-stu-id="f7617-188">hello map function also generates records of hello first and last keys of each file, and hello reduce function ensures that hello first key of file i is greater than hello last key of file i-1.</span></span> <span data-ttu-id="f7617-189">Všechny problémy, které jsou hlášeny jako výstup hello snížit hello klíče, které jsou mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="f7617-189">Any problems are reported as an output of hello reduce with hello keys that are out of order.</span></span>

<span data-ttu-id="f7617-190">Hello vstupní a výstupní formát, použít všechny tři aplikací, čte a zapisuje hello textových souborů ve správném formátu hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-190">hello input and output format, used by all three applications, reads and writes hello text files in hello right format.</span></span> <span data-ttu-id="f7617-191">Snižte Hello výstup hello replikace nastavil too1 místo výchozí hello 3, protože hello srovnávacího testu kontextu nevyžaduje, aby na uzlech toomultiple replikovat hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="f7617-191">hello output of hello reduce has replication set too1, instead of hello default 3, because hello benchmark contest does not require that hello output data be replicated on toomultiple nodes.</span></span>

<span data-ttu-id="f7617-192">Ukázka hello, každý odpovídající tooone programů MapReduce hello popsané v hello Úvod vyžadují tři úkoly:</span><span class="sxs-lookup"><span data-stu-id="f7617-192">Three tasks are required by hello sample, each corresponding tooone of hello MapReduce programs described in hello introduction:</span></span>

1. <span data-ttu-id="f7617-193">Generovat hello data pro řazení spuštěním hello **TeraGen** úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="f7617-193">Generate hello data for sorting by running hello **TeraGen** MapReduce job.</span></span>
2. <span data-ttu-id="f7617-194">Řazení dat hello spuštěním hello **TeraSort** úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="f7617-194">Sort hello data by running hello **TeraSort** MapReduce job.</span></span>
3. <span data-ttu-id="f7617-195">Potvrďte, že byla hello data správně seřazená spuštěním hello **TeraValidate** úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="f7617-195">Confirm that hello data has been correctly sorted by running hello **TeraValidate** MapReduce job.</span></span>

<span data-ttu-id="f7617-196">**toosubmit hello úlohy**</span><span class="sxs-lookup"><span data-stu-id="f7617-196">**toosubmit hello jobs**</span></span>

* <span data-ttu-id="f7617-197">Postupujte podle postupu hello v [aplikace Word počet - Java](#word-count-java), a použijte hello následující definice úlohy:</span><span class="sxs-lookup"><span data-stu-id="f7617-197">Follow hello procedure in [Word count - Java](#word-count-java), and use hello following job definitions:</span></span>

    ```powershell
    $teragen = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teragen" `
                                -Arguments "-Dmapred.map.tasks=50", "100000000", "/example/data/10GB-sort-input"

    $terasort = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "terasort" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-input", "/example/data/10GB-sort-output"

    $teravalidate = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teravalidate" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-output", "/example/data/10GB-sort-validate"
    ```

## <a name="next-steps"></a><span data-ttu-id="f7617-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7617-198">Next steps</span></span>
<span data-ttu-id="f7617-199">Z tohoto článku a hello články v každé hello vzorků, které jste se dozvěděli, jak toorun hello ukázky součástí hello clusterů HDInsight pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7617-199">From this article and hello articles in each of hello samples, you learned how toorun hello samples included with hello HDInsight clusters by using Azure PowerShell.</span></span> <span data-ttu-id="f7617-200">Kurzech k používání Pig, Hive a MapReduce s HDInsight najdete v části hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="f7617-200">For tutorials about using Pig, Hive, and MapReduce with HDInsight, see hello following topics:</span></span>

* <span data-ttu-id="f7617-201">[Začínáme používat Hadoop s Hive v HDInsight tooanalyze mobilního telefonu použití][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="f7617-201">[Get started using Hadoop with Hive in HDInsight tooanalyze mobile handset use][hdinsight-get-started]</span></span>
* <span data-ttu-id="f7617-202">[Použijte Pig s Hadoop v HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="f7617-202">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="f7617-203">[Použijte Hive s Hadoop v HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="f7617-203">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="f7617-204">[Odesílání úloh Hadoop v HDInsight][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="f7617-204">[Submit Hadoop Jobs in HDInsight][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="f7617-205">[Dokumentace k Azure HDInsight SDK][hdinsight-sdk-documentation]</span><span class="sxs-lookup"><span data-stu-id="f7617-205">[Azure HDInsight SDK documentation][hdinsight-sdk-documentation]</span></span>

## <a name="appendix-a---hello-word-count-source-code"></a><span data-ttu-id="f7617-206">Příloha A - hello Word počet zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="f7617-206">Appendix A - hello Word count source code</span></span>

```java
package org.apache.hadoop.examples;
import java.io.IOException;
import java.util.StringTokenizer;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
    extends Mapper<Object, Text, Text, IntWritable>{

private final static IntWritable one = new IntWritable(1);
private Text word = new Text();

public void map(Object key, Text value, Context context
                ) throws IOException, InterruptedException {
    StringTokenizer itr = new StringTokenizer(value.toString());
    while (itr.hasMoreTokens()) {
    word.set(itr.nextToken());
    context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
    extends Reducer<Text,IntWritable,Text,IntWritable> {
private IntWritable result = new IntWritable();

public void reduce(Text key, Iterable<IntWritable> values,
                    Context context
                    ) throws IOException, InterruptedException {
    int sum = 0;
    for (IntWritable val : values) {
    sum += val.get();
    }
    result.set(sum);
    context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
Configuration conf = new Configuration();
String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
if (otherArgs.length != 2) {
    System.err.println("Usage: wordcount <in> <out>");
    System.exit(2);
    }
Job job = new Job(conf, "word count");
job.setJarByClass(WordCount.class);
job.setMapperClass(TokenizerMapper.class);
job.setCombinerClass(IntSumReducer.class);
job.setReducerClass(IntSumReducer.class);
job.setOutputKeyClass(Text.class);
job.setOutputValueClass(IntWritable.class);
FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
    }
```

## <a name="appendix-b---hello-word-count-streaming-source-code"></a><span data-ttu-id="f7617-207">Dodatek B – počet slov hello streamování zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="f7617-207">Appendix B - hello word count streaming source code</span></span>
<span data-ttu-id="f7617-208">Hello MapReduce program používá hello cat.exe aplikace jako mapování rozhraní toostream hello text do konzoly hello a hello wc.exe aplikace jako hello snižte počet hello toocount rozhraní slov, která jsou datového proudu z dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f7617-208">hello MapReduce program uses hello cat.exe application as a mapping interface toostream hello text into hello console and hello wc.exe application as hello reduce interface toocount hello number of words that are streamed from a document.</span></span> <span data-ttu-id="f7617-209">Mapovač hello i reduktorem čtení řádek po řádku, znaky z hello standardní vstupní proud (stdin) a zápisu toohello standardního výstupního datového proudu (stdout).</span><span class="sxs-lookup"><span data-stu-id="f7617-209">Both hello mapper and reducer read characters, line-by-line, from hello standard input stream (stdin) and write toohello standard output stream (stdout).</span></span>

```csharp
// hello source code for hello cat.exe (Mapper).

using System;
using System.IO;

namespace cat
{
    class cat
    {
        static void Main(string[] args)
        {
            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            string line;
            char[] separators = { ' ', '\n'};
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split(separators);
                foreach (var word in words)
                {
                    Console.WriteLine("{0}\t1", word);
                }
            }
        }
    }
}
```

<span data-ttu-id="f7617-210">Hello mapper kód používá soubor cat.cs hello [StreamReader] [ streamreader] objektu tooread hello znaků hello příchozí datový proud toohello konzoly, který potom provede zápis hello datového proudu toohello standardní výstupní proud s hello statické [Console.Writeline] [ console-writeline] metoda.</span><span class="sxs-lookup"><span data-stu-id="f7617-210">hello mapper code in hello cat.cs file uses a [StreamReader][streamreader] object tooread hello characters of hello incoming stream toohello console, which then writes hello stream toohello standard output stream with hello static [Console.Writeline][console-writeline] method.</span></span>

```csharp
// hello source code for wc.exe (Reducer) is:

using System;
using System.IO;
using System.Linq;
using System.Collections;

namespace wc
{
    class wc
    {
        static void Main(string[] args)
        {
            string line;

            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            Hashtable wordCount = new Hashtable();
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split('\t');

                string key = words[0];

                if (wordCount.ContainsKey(key) == true)
                {
                    int n = Convert.ToInt32(wordCount[key]);
                    wordCount[key] = Convert.ToString(n + 1);
                }
                else
                {
                    wordCount[key] = words[1];
                }
            }
            foreach (var key in wordCount.Keys)
            {
                Console.WriteLine("{0} {1}", key, wordCount[key]);
            }
        }
    }
}
```

<span data-ttu-id="f7617-211">Hello reduktorem kód používá soubor wc.cs hello [StreamReader] [ streamreader] objektu tooread znaky z hello standardní vstupní datový proud, který byl výstup mapovačem cat.exe hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-211">hello reducer code in hello wc.cs file uses a [StreamReader][streamreader]   object tooread characters from hello standard input stream that have been output by hello cat.exe mapper.</span></span> <span data-ttu-id="f7617-212">Jako přečte hello znaky s hello [Console.Writeline] [ console-writeline] metoda, počítá hello slova roven mezer a konec řádku znaků na konci hello jednotlivých slov.</span><span class="sxs-lookup"><span data-stu-id="f7617-212">As it reads hello characters with hello [Console.Writeline][console-writeline] method, it counts hello words by counting spaces and end-of-line characters at hello end of each word.</span></span> <span data-ttu-id="f7617-213">Pak zapíše hello celkový toohello standardní výstupní datový proud s hello [Console.Writeline] [ console-writeline] metoda.</span><span class="sxs-lookup"><span data-stu-id="f7617-213">It then writes hello total toohello standard output stream with hello [Console.Writeline][console-writeline] method.</span></span>

## <a name="appendix-c---hello-pi-estimator-source-code"></a><span data-ttu-id="f7617-214">Příloha C – hello pí odhadu zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="f7617-214">Appendix C - hello Pi estimator source code</span></span>
<span data-ttu-id="f7617-215">Hello pí odhadu Java kód, který obsahuje funkce mapper a reduktorem hello k dispozici pro kontrolu níže.</span><span class="sxs-lookup"><span data-stu-id="f7617-215">hello pi estimator Java code that contains hello mapper and reducer functions is available for inspection below.</span></span> <span data-ttu-id="f7617-216">Hello mapper program generuje zadaný počet bodů, které jsou umístěny v náhodných uvnitř čtverce jednotky a pak počty hello číslo z těchto bodů, které jsou uvnitř hello kruhu.</span><span class="sxs-lookup"><span data-stu-id="f7617-216">hello mapper program generates a specified number of points placed at random inside of a unit square and then counts hello number of those points that are inside hello circle.</span></span> <span data-ttu-id="f7617-217">program reduktorem Hello shromažďuje body zjištěné službou hello mappers a potom odhadne hello hodnotu čísla pí z vzorce 4R hello, kde R je poměr hello hello počet bodů počítá uvnitř hello kruh toohello celkový počet bodů, které jsou v rámci hranaté hello.</span><span class="sxs-lookup"><span data-stu-id="f7617-217">hello reducer program accumulates points counted by hello mappers and then estimates hello value of pi from hello formula 4R, where R is hello ratio of hello number of points counted inside hello circle toohello total number of points that are within hello square.</span></span>

```java
/**
* Licensed toohello Apache Software Foundation (ASF) under one
* or more contributor license agreements. See hello NOTICE file
* distributed with this work for additional information
* regarding copyright ownership. hello ASF licenses this file
* tooyou under hello Apache License, Version 2.0 (the
* "License"); you may not use this file except in compliance
* with hello License. You may obtain a copy of hello License at
*
* http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed tooin writing, software
* distributed under hello License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or     implied.
* See hello License for hello specific language governing permissions and
* limitations under hello License.
*/

package org.apache.hadoop.examples;

import java.io.IOException;
import java.math.BigDecimal;
import java.util.Iterator;

import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.BooleanWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Writable;
import org.apache.hadoop.io.WritableComparable;
import org.apache.hadoop.io.SequenceFile.CompressionType;
import org.apache.hadoop.mapred.FileInputFormat;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.MapReduceBase;
import org.apache.hadoop.mapred.Mapper;
import org.apache.hadoop.mapred.OutputCollector;
import org.apache.hadoop.mapred.Reducer;
import org.apache.hadoop.mapred.Reporter;
import org.apache.hadoop.mapred.SequenceFileInputFormat;
import org.apache.hadoop.mapred.SequenceFileOutputFormat;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

//A Map-reduce program tooestimate hello value of Pi
//using quasi-Monte Carlo method.
//
//Mapper:
//Generate points in a unit square
//and then count points inside/outside of hello inscribed circle of hello square.
//
//Reducer:
//Accumulate points inside/outside results from hello mappers.
//Let numTotal = numInside + numOutside.
//hello fraction numInside/numTotal is a rational approximation of
//hello value (Area of hello circle)/(Area of hello square),
//where hello area of hello inscribed circle is Pi/4
//and hello area of unit square is 1.
//Then, Pi is estimated value toobe 4(numInside/numTotal).
//

public class PiEstimator extends Configured implements Tool {
//tmp directory for input/output
static private final Path TMP_DIR = new Path(
PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

//2-dimensional Halton sequence {H(i)},
//where H(i) is a 2-dimensional point and i >= 1 is hello index.
//Halton sequence is used toogenerate sample points for Pi estimation.
private static class HaltonSequence {
// Bases
static final int[] P = {2, 3};
//Maximum number of digits allowed
static final int[] K = {63, 40};

private long index;
private double[] x;
private double[][] q;
private int[][] d;

//Initialize tooH(startindex),
//so hello sequence begins with H(startindex+1).
HaltonSequence(long startindex) {
index = startindex;
x = new double[K.length];
q = new double[K.length][];
d = new int[K.length][];
for(int i = 0; i < K.length; i++) {
q[i] = new double[K[i]];
d[i] = new int[K[i]];
}

for(int i = 0; i < K.length; i++) {
long k = index;
x[i] = 0;

for(int j = 0; j < K[i]; j++) {
q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
d[i][j] = (int)(k % P[i]);
k = (k - d[i][j])/P[i];
x[i] += d[i][j] * q[i][j];
}
}
}

//Compute next point.
//Assume hello current point is H(index).
//Compute H(index+1).
//@return a 2-dimensional point with coordinates in [0,1)^2
double[] nextPoint() {
index++;
for(int i = 0; i < K.length; i++) {
for(int j = 0; j < K[i]; j++) {
d[i][j]++;
x[i] += q[i][j];
if (d[i][j] < P[i]) {
break;
}
d[i][j] = 0;
x[i] -= (j == 0? 1.0: q[i][j-1]);
}
}
return x;
}
}

//Mapper class for Pi estimation.
//Generate points in a unit square and then
//count points inside/outside of hello inscribed circle of hello square.
public static class PiMapper extends MapReduceBase
implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

//Map method.
//@param offset samples starting from hello (offset+1)th sample.
//@param size hello number of samples for this map
//@param out output {ture->numInside, false->numOutside}
//@param reporter
public void map(LongWritable offset,
LongWritable size,
OutputCollector<BooleanWritable, LongWritable> out,
Reporter reporter) throws IOException {

final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
long numInside = 0L;
long numOutside = 0L;

for(long i = 0; i < size.get(); ) {
//generate points in a unit square
final double[] point = haltonsequence.nextPoint();

//count points inside/outside of hello inscribed circle of hello square
final double x = point[0] - 0.5;
final double y = point[1] - 0.5;
if (x*x + y*y > 0.25) {
numOutside++;
} else {
numInside++;
}

//report status
i++;
if (i % 1000 == 0) {
reporter.setStatus("Generated " + i + " samples.");
}
}

//output map results
out.collect(new BooleanWritable(true), new LongWritable(numInside));
out.collect(new BooleanWritable(false), new LongWritable(numOutside));
}
}

//Reducer class for Pi estimation.
//Accumulate points inside/outside results from hello mappers.
public static class PiReducer extends MapReduceBase
implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

private long numInside = 0;
private long numOutside = 0;
private JobConf conf; //configuration for accessing hello file system

//Store job configuration.
@Override
public void configure(JobConf job) {
conf = job;
}

// Accumulate number of points inside/outside results from hello mappers.
// @param isInside Is hello points inside?
// @param values An iterator tooa list of point counts
// @param output dummy, not used here.
// @param reporter

public void reduce(BooleanWritable isInside,
Iterator<LongWritable> values,
OutputCollector<WritableComparable<?>, Writable> output,
Reporter reporter) throws IOException {
if (isInside.get()) {
for(; values.hasNext(); numInside += values.next().get());
} else {
for(; values.hasNext(); numOutside += values.next().get());
}
}

//Reduce task done, write output tooa file.
@Override
public void close() throws IOException {
//write output tooa file
Path outDir = new Path(TMP_DIR, "out");
Path outFile = new Path(outDir, "reduce-out");
FileSystem fileSys = FileSystem.get(conf);
SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
outFile, LongWritable.class, LongWritable.class,
CompressionType.NONE);
writer.append(new LongWritable(numInside), new LongWritable(numOutside));
writer.close();
}
}

//Run a map/reduce job for estimating Pi.
//@return hello estimated value of Pi.
public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
)
throws IOException {
//setup job conf
jobConf.setJobName(PiEstimator.class.getSimpleName());

jobConf.setInputFormat(SequenceFileInputFormat.class);

jobConf.setOutputKeyClass(BooleanWritable.class);
jobConf.setOutputValueClass(LongWritable.class);
jobConf.setOutputFormat(SequenceFileOutputFormat.class);

jobConf.setMapperClass(PiMapper.class);
jobConf.setNumMapTasks(numMaps);

jobConf.setReducerClass(PiReducer.class);
jobConf.setNumReduceTasks(1);

// turn off speculative execution, because DFS doesn't handle
// multiple writers toohello same file.
jobConf.setSpeculativeExecution(false);

//setup input/output directories
final Path inDir = new Path(TMP_DIR, "in");
final Path outDir = new Path(TMP_DIR, "out");
FileInputFormat.setInputPaths(jobConf, inDir);
FileOutputFormat.setOutputPath(jobConf, outDir);

final FileSystem fs = FileSystem.get(jobConf);
if (fs.exists(TMP_DIR)) {
throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
+ " already exists. Remove it first.");
}
if (!fs.mkdirs(inDir)) {
throw new IOException("Cannot create input directory " + inDir);
}

//generate an input file for each map task
try {
for(int i=0; i < numMaps; ++i) {
final Path file = new Path(inDir, "part"+i);
final LongWritable offset = new LongWritable(i * numPoints);
final LongWritable size = new LongWritable(numPoints);
final SequenceFile.Writer writer = SequenceFile.createWriter(
fs, jobConf, file,
LongWritable.class, LongWritable.class, CompressionType.NONE);
try {
writer.append(offset, size);
} finally {
writer.close();
}
System.out.println("Wrote input for Map #"+i);
}

//start a map/reduce job
System.out.println("Starting Job");
final long startTime = System.currentTimeMillis();
JobClient.runJob(jobConf);
final double duration = (System.currentTimeMillis() - startTime)/1000.0;
System.out.println("Job Finished in " + duration + " seconds");

//read outputs
Path inFile = new Path(outDir, "reduce-out");
LongWritable numInside = new LongWritable();
LongWritable numOutside = new LongWritable();
SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
try {
reader.next(numInside, numOutside);
} finally {
reader.close();
}

//compute estimated value
return BigDecimal.valueOf(4).setScale(20)
.multiply(BigDecimal.valueOf(numInside.get()))
.divide(BigDecimal.valueOf(numMaps))
.divide(BigDecimal.valueOf(numPoints));
} finally {
fs.delete(TMP_DIR, true);
}
}

//Parse arguments and then runs a map/reduce job.
//Print output in standard out.
//@return a non-zero if there is an error. Otherwise, return 0.
public int run(String[] args) throws Exception {
if (args.length != 2) {
System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
ToolRunner.printGenericCommandUsage(System.err);
return -1;
}

final int nMaps = Integer.parseInt(args[0]);
final long nSamples = Long.parseLong(args[1]);

System.out.println("Number of Maps = " + nMaps);
System.out.println("Samples per Map = " + nSamples);

final JobConf jobConf = new JobConf(getConf(), getClass());
System.out.println("Estimated value of Pi is "
+ estimate(nMaps, nSamples, jobConf));
return 0;
}

//main method for running it as a stand alone command.
public static void main(String[] argv) throws Exception {
System.exit(ToolRunner.run(null, new PiEstimator(), argv));
}
}
```

## <a name="appendix-d---hello-10gb-graysort-source-code"></a><span data-ttu-id="f7617-218">Dodatek D – hello 10gb graysort zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="f7617-218">Appendix D - hello 10gb graysort source code</span></span>
<span data-ttu-id="f7617-219">pro kontroly v této části se zobrazí kód Hello hello TeraSort MapReduce programu.</span><span class="sxs-lookup"><span data-stu-id="f7617-219">hello code for hello TeraSort MapReduce program is presented for inspection in this section.</span></span>

```java
/**
    * Licensed toohello Apache Software Foundation (ASF) under one
    * or more contributor license agreements.  See hello NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership.  hello ASF licenses this file
    * tooyou under hello Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with hello License.  You may obtain a copy of hello License at
    *
    *     http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed tooin writing, software
    * distributed under hello License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    * See hello License for hello specific language governing permissions and
    * limitations under hello License.
    */

package org.apache.hadoop.examples.terasort;

import java.io.IOException;
import java.io.PrintStream;
import java.net.URI;
import java.util.ArrayList;
import java.util.List;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.filecache.DistributedCache;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.Partitioner;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

/**
    * Generates hello sampled split points, launches hello job,
    * and waits for it toofinish.
    * <p>
    * toorun hello program:
    * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
    */

public class TeraSort extends Configured implements Tool {
    private static final Log LOG = LogFactory.getLog(TeraSort.class);

    /**
    * A partitioner that splits text keys into roughly equal
    * partitions in a global sorted order.
    */

    static class TotalOrderPartitioner implements Partitioner<Text,Text>{
    private TrieNode trie;
    private Text[] splitPoints;

    /**
        * A generic trie node
        */
    static abstract class TrieNode {
        private int level;
        TrieNode(int level) {
        this.level = level;
        }
        abstract int findPartition(Text key);
        abstract void print(PrintStream strm) throws IOException;
        int getLevel() {
        return level;
        }
    }

    /**
        * An inner trie node that contains 256 children based on hello next
        * character.
        */
    static class InnerTrieNode extends TrieNode {
        private TrieNode[] child = new TrieNode[256];

        InnerTrieNode(int level) {
        super(level);
        }
        int findPartition(Text key) {
        int level = getLevel();
        if (key.getLength() <= level) {
            return child[0].findPartition(key);
        }
        return child[key.getBytes()[level]].findPartition(key);
        }
        void setChild(int idx, TrieNode child) {
        this.child[idx] = child;
        }
        void print(PrintStream strm) throws IOException {
        for(int ch=0; ch < 255; ++ch) {
            for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
            }
            strm.print(ch);
            strm.println(" ->");
            if (child[ch] != null) {
            child[ch].print(strm);
            }
        }
        }
    }

    /**
        * A leaf trie node that does string compares toofigure out where hello given
        * key belongs between lower..upper.
        */
    static class LeafTrieNode extends TrieNode {
        int lower;
        int upper;
        Text[] splitPoints;
        LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
        super(level);
        this.splitPoints = splitPoints;
        this.lower = lower;
        this.upper = upper;
        }
        int findPartition(Text key) {
        for(int i=lower; i<upper; ++i) {
            if (splitPoints[i].compareTo(key) >= 0) {
            return i;
            }
        }
        return upper;
        }
        void print(PrintStream strm) throws IOException {
        for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
        }
        strm.print(lower);
        strm.print(", ");
        strm.println(upper);
        }
    }

    /**
        * Read hello cut points from hello given sequence file.
        * @param fs hello file system
        * @param p hello path tooread
        * @param job hello job config
        * @return hello strings toosplit hello partitions on
        * @throws IOException
        */
    private static Text[] readPartitions(FileSystem fs, Path p,
                                            JobConf job) throws IOException {
        SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
        List<Text> parts = new ArrayList<Text>();
        Text key = new Text();
        NullWritable value = NullWritable.get();
        while (reader.next(key, value)) {
        parts.add(key);
        key = new Text();
        }
        reader.close();
        return parts.toArray(new Text[parts.size()]);
    }

    /**
        * Given a sorted set of cut points, build a trie that will find hello correct
        * partition quickly.
        * @param splits hello list of cut points
        * @param lower hello lower bound of partitions 0..numPartitions-1
        * @param upper hello upper bound of partitions 0..numPartitions-1
        * @param prefix hello prefix that we have already checked against
        * @param maxDepth hello maximum depth we will build a trie for
        * @return hello trie node that will divide hello splits correctly
        */
    private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                        Text prefix, int maxDepth) {
        int depth = prefix.getLength();
        if (depth >= maxDepth || lower == upper) {
        return new LeafTrieNode(depth, splits, lower, upper);
        }
        InnerTrieNode result = new InnerTrieNode(depth);
        Text trial = new Text(prefix);
        // append an extra byte on toohello prefix
        trial.append(new byte[1], 0, 1);
        int currentBound = lower;
        for(int ch = 0; ch < 255; ++ch) {
        trial.getBytes()[depth] = (byte) (ch + 1);
        lower = currentBound;
        while (currentBound < upper) {
            if (splits[currentBound].compareTo(trial) >= 0) {
            break;
            }
            currentBound += 1;
        }
        trial.getBytes()[depth] = (byte) ch;
        result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                        maxDepth);
        }
        // pick up hello rest
        trial.getBytes()[depth] = 127;
        result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                    maxDepth);
        return result;
    }

    public void configure(JobConf job) {
        try {
        FileSystem fs = FileSystem.getLocal(job);
        Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
        splitPoints = readPartitions(fs, partFile, job);
        trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
        } catch (IOException ie) {
        throw new IllegalArgumentException("can't read paritions file", ie);
        }
    }

    public TotalOrderPartitioner() {
    }

    public int getPartition(Text key, Text value, int numPartitions) {
        return trie.findPartition(key);
    }

    }

    public int run(String[] args) throws Exception {
    LOG.info("starting");
    JobConf job = (JobConf) getConf();
    Path inputDir = new Path(args[0]);
    inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
    Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
    URI partitionUri = new URI(partitionFile.toString() +
                                "#" + TeraInputFormat.PARTITION_FILENAME);
    TeraInputFormat.setInputPaths(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    job.setJobName("TeraSort");
    job.setJarByClass(TeraSort.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(Text.class);
    job.setInputFormat(TeraInputFormat.class);
    job.setOutputFormat(TeraOutputFormat.class);
    job.setPartitionerClass(TotalOrderPartitioner.class);
    TeraInputFormat.writePartitionFile(job, partitionFile);
    DistributedCache.addCacheFile(partitionUri, job);
    DistributedCache.createSymlink(job);
    job.setInt("dfs.replication", 1);
    TeraOutputFormat.setFinalSync(job, true);
    JobClient.runJob(job);
    LOG.info("done");
    return 0;
    }

    /**
    * @param args
    */

    public static void main(String[] args) throws Exception {
    int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
    System.exit(res);
    }
}
```

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
