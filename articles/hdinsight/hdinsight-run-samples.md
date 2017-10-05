---
title: "Spuštění ukázek sadu Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Začínáme používat službu Azure HDInsight s uvedené. Použití skriptů prostředí PowerShell, které spouštět programy MapReduce na clustery s daty."
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
ms.openlocfilehash: 741cce6f2c81efed1e4bd0547fcb46a231815263
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a><span data-ttu-id="8e70f-104">Spuštění ukázky MapReduce s Hadoop v HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="8e70f-104">Run Hadoop MapReduce samples in Windows-based HDInsight</span></span>
[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

<span data-ttu-id="8e70f-105">Sadu vzorků, které jsou k dispozici vám pomůže nastavit Začínáme spuštěné úlohy MapReduce na clusterů systému Hadoop pomocí Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8e70f-105">A set of samples are provided to help you get started running MapReduce jobs on Hadoop clusters using Azure HDInsight.</span></span> <span data-ttu-id="8e70f-106">Tyto ukázky jsou k dispozici na každém z clusterů HDInsight spravované, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="8e70f-106">These samples are made available on each of the HDInsight managed clusters that you create.</span></span> <span data-ttu-id="8e70f-107">Spuštění tyto ukázky vám seznámit se s pomocí rutin prostředí Azure PowerShell ke spuštění úloh na clusterů systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="8e70f-107">Running these samples familiarize you with using Azure PowerShell cmdlets to run jobs on Hadoop clusters.</span></span>

* <span data-ttu-id="8e70f-108">[**Aplikace Word počet**][hdinsight-sample-wordcount]: počty výskytů slova v textovém souboru.</span><span class="sxs-lookup"><span data-stu-id="8e70f-108">[**Word count**][hdinsight-sample-wordcount]: Counts word occurrences in a text file.</span></span>
* <span data-ttu-id="8e70f-109">[**C# streamování počet slov**][hdinsight-sample-csharp-streaming]: počty výskytů slova v textovém souboru pomocí rozhraní streamování Hadoop.</span><span class="sxs-lookup"><span data-stu-id="8e70f-109">[**C# streaming word count**][hdinsight-sample-csharp-streaming]: Counts word occurrences in a text file using the Hadoop streaming interface.</span></span>
* <span data-ttu-id="8e70f-110">[**Pi odhadu**][hdinsight-sample-pi-estimator]: používá statistické (jako Monte Carlo) metoda odhadnout hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="8e70f-110">[**Pi estimator**][hdinsight-sample-pi-estimator]: Uses a statistical (quasi-Monte Carlo) method to estimate the value of pi.</span></span>
* <span data-ttu-id="8e70f-111">[**10 GB Graysort**][hdinsight-sample-10gb-graysort]: spuštění pro obecné účely GraySort na 10 GB souboru pomocí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8e70f-111">[**10-GB Graysort**][hdinsight-sample-10gb-graysort]: Run a general-purpose GraySort on a 10 GB file by using HDInsight.</span></span> <span data-ttu-id="8e70f-112">Existují tři úlohy ke spuštění: Teragen pro generování dat, Terasort řadit data a Teravalidate potvrďte správně seřazená data.</span><span class="sxs-lookup"><span data-stu-id="8e70f-112">There are three jobs to run: Teragen to generate the data, Terasort to sort the data, and Teravalidate to confirm that the data has been properly sorted.</span></span>

> [!NOTE]
> <span data-ttu-id="8e70f-113">Zdrojový kód najdete v příloze.</span><span class="sxs-lookup"><span data-stu-id="8e70f-113">The source code can be found in the Appendix.</span></span>

<span data-ttu-id="8e70f-114">Existuje mnohem další dokumentaci na webu pro technologie související s Hadoop, jako je například programování založené na jazyce Java MapReduce, streamování a dokumentaci o rutinách, které se používají v prostředí Windows PowerShell skriptování.</span><span class="sxs-lookup"><span data-stu-id="8e70f-114">Much additional documentation exists on the web for Hadoop-related technologies, such as Java-based MapReduce programming and streaming, and documentation about the cmdlets that are used in Windows PowerShell scripting.</span></span> <span data-ttu-id="8e70f-115">Další informace o těchto prostředků najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="8e70f-115">For more information about these resources, see:</span></span>

* [<span data-ttu-id="8e70f-116">Vývoj aplikací Java MapReduce pro Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8e70f-116">Develop Java MapReduce programs for Hadoop in HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)
* [<span data-ttu-id="8e70f-117">Odesílání úloh Hadoop do služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="8e70f-117">Submit Hadoop jobs in HDInsight</span></span>](hdinsight-submit-hadoop-jobs-programmatically.md)
* <span data-ttu-id="8e70f-118">[Úvod do Azure HDInsight][hdinsight-introduction]</span><span class="sxs-lookup"><span data-stu-id="8e70f-118">[Introduction to Azure HDInsight][hdinsight-introduction]</span></span>

<span data-ttu-id="8e70f-119">Spousta lidí v současné době zvolte Hive a Pig přes MapReduce.</span><span class="sxs-lookup"><span data-stu-id="8e70f-119">Nowadays, many people choose Hive and Pig over MapReduce.</span></span>  <span data-ttu-id="8e70f-120">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="8e70f-120">For more information, see:</span></span>

* [<span data-ttu-id="8e70f-121">Používání Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8e70f-121">Use Hive in HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="8e70f-122">Použijte Pig v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8e70f-122">Use Pig in HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="8e70f-123">**Požadavky**:</span><span class="sxs-lookup"><span data-stu-id="8e70f-123">**Prerequisites**:</span></span>

* <span data-ttu-id="8e70f-124">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="8e70f-124">**An Azure subscription**.</span></span> <span data-ttu-id="8e70f-125">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="8e70f-125">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="8e70f-126">**cluster služby HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="8e70f-126">**an HDInsight cluster**.</span></span> <span data-ttu-id="8e70f-127">Pokyny o různých způsobech, ve kterých lze vytvořit tyto clustery najdete v tématu [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="8e70f-127">For instructions on the various ways in which such clusters can be created, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="8e70f-128">**Pracovní stanice s prostředím Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="8e70f-128">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="8e70f-129">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a 1. ledna 2017 dojde k jejímu odebrání.</span><span class="sxs-lookup"><span data-stu-id="8e70f-129">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and will be removed by January 1, 2017.</span></span> <span data-ttu-id="8e70f-130">Kroky v tomto dokumentu používají nové rutiny služby HDInsight, které pracují s Azure Resource Managerem.</span><span class="sxs-lookup"><span data-stu-id="8e70f-130">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="8e70f-131">Postupujte podle kroků v [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nainstalovat nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8e70f-131">Follow the steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="8e70f-132">Pokud máte skripty, které je potřeba upravit tak, aby používaly nové rutiny, které pracují s Azure Resource Manager, najdete v [migraci na Azure Resource Manager vývojové nástroje založené pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="8e70f-132">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md).</span></span>

## <span data-ttu-id="8e70f-133"><a name="hdinsight-sample-wordcount"></a>Počet - Java aplikace Word</span><span class="sxs-lookup"><span data-stu-id="8e70f-133"><a name="hdinsight-sample-wordcount"></a>Word count - Java</span></span>
<span data-ttu-id="8e70f-134">K odeslání projektu MapReduce, je nejprve vytvořit definici úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="8e70f-134">To submit a MapReduce project, you first create a MapReduce job definition.</span></span> <span data-ttu-id="8e70f-135">V definici úlohy zadáte na soubor jar program MapReduce a umístění na soubor jar, což je **wasb:///example/jars/hadoop-mapreduce-examples.jar**, název třídy a argumenty.</span><span class="sxs-lookup"><span data-stu-id="8e70f-135">In the job definition, you specify the MapReduce program jar file and the location of the jar file, which is **wasb:///example/jars/hadoop-mapreduce-examples.jar**, the class name, and the arguments.</span></span>  <span data-ttu-id="8e70f-136">Wordcount MapReduce program má dva argumenty: zdrojový soubor, který se používá k určení počtu slov a umístění pro výstup.</span><span class="sxs-lookup"><span data-stu-id="8e70f-136">The wordcount MapReduce program takes two arguments: the source file that is used to count words, and the location for output.</span></span>

<span data-ttu-id="8e70f-137">Zdrojový kód najdete v [příloha A](#apendix-a---the-word-count-MapReduce-program-in-java).</span><span class="sxs-lookup"><span data-stu-id="8e70f-137">The source code can be found in the [Appendix A](#apendix-a---the-word-count-MapReduce-program-in-java).</span></span>

<span data-ttu-id="8e70f-138">Postup tvorby Java MapReduce programu, najdete v části - [vyvíjet MapReduce Java programy pro Hadoop v HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)</span><span class="sxs-lookup"><span data-stu-id="8e70f-138">For the procedure of developing a Java MapReduce program, see - [Develop Java MapReduce programs for Hadoop in HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)</span></span>

<span data-ttu-id="8e70f-139">**Odeslat úlohu MapReduce počet aplikace word**</span><span class="sxs-lookup"><span data-stu-id="8e70f-139">**To submit a word count MapReduce job**</span></span>

1. <span data-ttu-id="8e70f-140">Otevřete **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="8e70f-140">Open **Windows PowerShell ISE**.</span></span> <span data-ttu-id="8e70f-141">Pokyny najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell][powershell-install-configure].</span><span class="sxs-lookup"><span data-stu-id="8e70f-141">For instructions, see [Install and configure Azure PowerShell][powershell-install-configure].</span></span>
2. <span data-ttu-id="8e70f-142">Vložte následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8e70f-142">Paste the following PowerShell script:</span></span>

    ```powershell
    $subscriptionName = "<Azure Subscription Name>"
    $resourceGroupName = "<Resource Group Name>"
    $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name

    Select-AzureRmSubscription -SubscriptionName $subscriptionName

    # Define the MapReduce job
    $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "wordcount" `
                                -Arguments "wasb:///example/data/gutenberg/davinci.txt", "wasb:///example/data/WordCountOutput"

    # Submit the job and wait for job completion
    $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:"
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

    # Get the job output
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

    # Download the job output to the workstation
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force

    # Display the output file
    cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"
    ```

    <span data-ttu-id="8e70f-143">Úlohu MapReduce vytvoří soubor s názvem *část r-00000*, který obsahuje slova a počty.</span><span class="sxs-lookup"><span data-stu-id="8e70f-143">The MapReduce job produces a file named *part-r-00000*, which contains words and the counts.</span></span> <span data-ttu-id="8e70f-144">Tento skript využívá **findstr** příkazu zobrazíte všechna slova, která obsahuje *"existuje"*.</span><span class="sxs-lookup"><span data-stu-id="8e70f-144">The script uses the **findstr** command to list all the words that contains *"there"*.</span></span>
3. <span data-ttu-id="8e70f-145">Nastavení prvních tří proměnných a spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="8e70f-145">Set the first three variables, and run the script.</span></span>

## <span data-ttu-id="8e70f-146"><a name="hdinsight-sample-csharp-streaming"></a>Aplikace Word počet - streamování C#</span><span class="sxs-lookup"><span data-stu-id="8e70f-146"><a name="hdinsight-sample-csharp-streaming"></a>Word count - C# streaming</span></span>
<span data-ttu-id="8e70f-147">Hadoop poskytuje streamování rozhraní API pro MapReduce, který umožňuje zapisovat mapy a omezit funkce v jiných jazyků než Java.</span><span class="sxs-lookup"><span data-stu-id="8e70f-147">Hadoop provides a streaming API to MapReduce, which enables you to write map and reduce functions in languages other than Java.</span></span>

> [!NOTE]
> <span data-ttu-id="8e70f-148">Kroky v tomto kurzu platí pouze pro clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="8e70f-148">The steps in this tutorial apply only to Windows-based HDInsight clusters.</span></span> <span data-ttu-id="8e70f-149">Příklad streamování pro clustery HDInsight se systémem Linux naleznete v části [vyvíjet Python streamování programy pro HDInsight](hdinsight-hadoop-streaming-python.md).</span><span class="sxs-lookup"><span data-stu-id="8e70f-149">For an example of streaming for Linux-based HDInsight clusters, see [Develop Python streaming programs for HDInsight](hdinsight-hadoop-streaming-python.md).</span></span>

<span data-ttu-id="8e70f-150">V příkladu mapper a reduktorem jsou spustitelné soubory, které číst vstupu z [stdin –] [ stdin-stdout-stderr] (řádek po řádku) a výstup do [stdout] [ stdin-stdout-stderr].</span><span class="sxs-lookup"><span data-stu-id="8e70f-150">In the example, the mapper and the reducer are executables that read the input from [stdin][stdin-stdout-stderr] (line-by-line) and emit the output to [stdout][stdin-stdout-stderr].</span></span> <span data-ttu-id="8e70f-151">Program spočítá všechna slova v textu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-151">The program counts all the words in the text.</span></span>

<span data-ttu-id="8e70f-152">Pokud je zadán parametr spustitelný soubor pro **mappers**, každý úkol mapper spustitelný soubor spouští jako samostatný proces při inicializaci mapper.</span><span class="sxs-lookup"><span data-stu-id="8e70f-152">When an executable is specified for **mappers**, each mapper task launches the executable as a separate process when the mapper is initialized.</span></span> <span data-ttu-id="8e70f-153">Jako spuštěna úloha mapper, převede vstupní řádky a kanály řádky, které se [stdin –] [ stdin-stdout-stderr] procesu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-153">As the mapper task runs, it converts its input into lines, and feeds the lines to the [stdin][stdin-stdout-stderr] of the process.</span></span>

<span data-ttu-id="8e70f-154">Do té doby mapper shromažďuje řádkový výstup z výstupu stdout procesu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-154">In the meantime, the mapper collects the line-oriented output from the stdout of the process.</span></span> <span data-ttu-id="8e70f-155">Každý řádek je převede na dvojice klíč/hodnota, která se shromažďují jako výstup mapper.</span><span class="sxs-lookup"><span data-stu-id="8e70f-155">It converts each line into a key/value pair, which is collected as the output of the mapper.</span></span> <span data-ttu-id="8e70f-156">Ve výchozím nastavení Předpona řádku až do prvního znaku, karta se klíč a zbytek na řádku (s výjimkou znak tabulátoru) je hodnota.</span><span class="sxs-lookup"><span data-stu-id="8e70f-156">By default, the prefix of a line up to the first Tab character is the key, and the remainder of the line (excluding the Tab character) is the value.</span></span> <span data-ttu-id="8e70f-157">Pokud není žádná karta znak v řádku, považuje za celý řádek jako klíč a hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="8e70f-157">If there is no Tab character in the line, entire line is considered as the key, and the value is null.</span></span>

<span data-ttu-id="8e70f-158">Pokud je zadán parametr spustitelný soubor pro **přechodky**, každý úkol reduktorem spouští spustitelný soubor jako samostatný proces, když reduktorem je inicializován.</span><span class="sxs-lookup"><span data-stu-id="8e70f-158">When an executable is specified for **reducers**, each reducer task launches the executable as a separate process when the reducer is initialized.</span></span> <span data-ttu-id="8e70f-159">Spuštění úlohy reduktorem, převede ji jeho páry vstupní klíč/hodnota do řádky a ho kanály řádky, které se [stdin –] [ stdin-stdout-stderr] procesu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-159">As the reducer task runs, it converts its input key/values pairs into lines, and it feeds the lines to the [stdin][stdin-stdout-stderr] of the process.</span></span>

<span data-ttu-id="8e70f-160">Do té doby reduktorem shromažďuje výstup řádkový [stdout] [ stdin-stdout-stderr] procesu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-160">In the meantime, the reducer collects the line-oriented output from the [stdout][stdin-stdout-stderr] of the process.</span></span> <span data-ttu-id="8e70f-161">Dvojice klíč/hodnota, která se shromažďují jako výstup reduktorem převede každý řádek.</span><span class="sxs-lookup"><span data-stu-id="8e70f-161">It converts each line to a key/value pair, which is collected as the output of the reducer.</span></span> <span data-ttu-id="8e70f-162">Ve výchozím nastavení Předpona řádku až do prvního znaku, karta se klíč a zbytek na řádku (s výjimkou znak tabulátoru) je hodnota.</span><span class="sxs-lookup"><span data-stu-id="8e70f-162">By default, the prefix of a line up to the first Tab character is the key, and the remainder of the line (excluding the Tab character) is the value.</span></span>

<span data-ttu-id="8e70f-163">**K odeslání C# streamování úlohy počet aplikace word**</span><span class="sxs-lookup"><span data-stu-id="8e70f-163">**To submit a C# streaming word count job**</span></span>

* <span data-ttu-id="8e70f-164">Postupujte podle pokynů v [aplikace Word počet - Java](#word-count-java)a nahraďte definici úlohy následující řádek:</span><span class="sxs-lookup"><span data-stu-id="8e70f-164">Follow the procedure in [Word count - Java](#word-count-java), and replace the job definition with the following line:</span></span>

    ```powershell
    $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                            -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                            -Mapper "cat.exe" `
                            -Reducer "wc.exe" `
                            -InputPath "/example/data/gutenberg/davinci.txt" `
                            -OutputPath "/example/data/StreamingOutput/wc.txt"
    ```

    <span data-ttu-id="8e70f-165">Výstupní soubor musí být:</span><span class="sxs-lookup"><span data-stu-id="8e70f-165">The output file shall be:</span></span>

        example/data/StreamingOutput/wc.txt/part-00000

## <span data-ttu-id="8e70f-166"><a name="hdinsight-sample-pi-estimator"></a>PI odhadu</span><span class="sxs-lookup"><span data-stu-id="8e70f-166"><a name="hdinsight-sample-pi-estimator"></a>PI estimator</span></span>
<span data-ttu-id="8e70f-167">Pi odhadu používá statistické (jako Monte Carlo) metoda odhadnout hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="8e70f-167">The pi estimator uses a statistical (quasi-Monte Carlo) method to estimate the value of pi.</span></span> <span data-ttu-id="8e70f-168">Body, které jsou umístěny v náhodných uvnitř jednotku čtvercovou také spadal do kruh označeny pravděpodobnost rovná oblasti kruhu, v rámci této hranaté pí/4.</span><span class="sxs-lookup"><span data-stu-id="8e70f-168">Points placed at random inside of a unit square also fall within a circle inscribed within that square with a probability equal to the area of the circle, pi/4.</span></span> <span data-ttu-id="8e70f-169">Hodnotu čísla pí se dá odhadnout od hodnoty 4R, kde R je poměr počtu bodů, které jsou uvnitř kruhu celkového počtu bodů, které jsou v rámci druhou mocninu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-169">The value of pi can be estimated from the value of 4R, where R is the ratio of the number of points that are inside the circle to the total number of points that are within the square.</span></span> <span data-ttu-id="8e70f-170">Větší ukázkové bodů použít, tím lepší odhad je.</span><span class="sxs-lookup"><span data-stu-id="8e70f-170">The larger the sample of points used, the better the estimate is.</span></span>

<span data-ttu-id="8e70f-171">Zadaná pro tento ukázkový skript odešle úlohu jar Hadoop a nastavena až na spuštění s hodnotou 16 maps, z nichž každý je potřeba výpočetní 10 milionů ukázka body podle hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="8e70f-171">The script provided for this sample submits a Hadoop jar job and is set up to run with a value 16 maps, each of which is required to compute 10 million sample points by the parameter values.</span></span> <span data-ttu-id="8e70f-172">Tyto hodnoty parametrů můžete změnit na zlepšení odhadované hodnotu čísla pí.</span><span class="sxs-lookup"><span data-stu-id="8e70f-172">These parameter values can be changed to improve the estimated value of pi.</span></span> <span data-ttu-id="8e70f-173">Pro odkaz jsou prvních 10 desetinných míst pí 3.1415926535.</span><span class="sxs-lookup"><span data-stu-id="8e70f-173">For reference, the first 10 decimal places of pi are 3.1415926535.</span></span>

<span data-ttu-id="8e70f-174">**Odeslat úlohu odhadu platformy**</span><span class="sxs-lookup"><span data-stu-id="8e70f-174">**To submit a pi estimator job**</span></span>

* <span data-ttu-id="8e70f-175">Postupujte podle pokynů v [aplikace Word počet - Java](#word-count-java)a nahraďte definici úlohy následující řádek:</span><span class="sxs-lookup"><span data-stu-id="8e70f-175">Follow the procedure in [Word count - Java](#word-count-java), and replace the job definition with the following line:</span></span>

    ```powershell
    $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "pi" `
                                -Arguments "16", "10000000"
    ```

## <span data-ttu-id="8e70f-176"><a name="hdinsight-sample-10gb-graysort"></a>Graysort 10 GB</span><span class="sxs-lookup"><span data-stu-id="8e70f-176"><a name="hdinsight-sample-10gb-graysort"></a>10-GB Graysort</span></span>
<span data-ttu-id="8e70f-177">Tato ukázka používá mírné 10GB dat tak, aby mohl být program spuštěn poměrně rychle.</span><span class="sxs-lookup"><span data-stu-id="8e70f-177">This sample uses a modest 10GB of data so that it can be run relatively quickly.</span></span> <span data-ttu-id="8e70f-178">Používá MapReduce aplikace vyvinuté tak, že Owen O'Malley a Arun Murthy získané roční srovnávacího testu řazení terabajt pro obecné účely ("daytona") v 2009 míru 0.578 TB za minutu (100 TB 173 minut).</span><span class="sxs-lookup"><span data-stu-id="8e70f-178">It uses the MapReduce applications developed by Owen O'Malley and Arun Murthy that won the annual general-purpose ("daytona") terabyte sort benchmark in 2009 with a rate of 0.578TB/min (100TB in 173 minutes).</span></span> <span data-ttu-id="8e70f-179">Další informace o tomto a dalších řazení srovnávacích testů najdete v tématu [Sortbenchmark](http://sortbenchmark.org/) lokality.</span><span class="sxs-lookup"><span data-stu-id="8e70f-179">For more information on this and other sorting benchmarks, see the [Sortbenchmark](http://sortbenchmark.org/) site.</span></span>

<span data-ttu-id="8e70f-180">Tato ukázka používá tři sady MapReduce programů:</span><span class="sxs-lookup"><span data-stu-id="8e70f-180">This sample uses three sets of MapReduce programs:</span></span>

1. <span data-ttu-id="8e70f-181">**TeraGen** je program MapReduce, který můžete použít ke generování řádky dat do řazení.</span><span class="sxs-lookup"><span data-stu-id="8e70f-181">**TeraGen** is a MapReduce program that you can use to generate the rows of data to sort.</span></span>
2. <span data-ttu-id="8e70f-182">**TeraSort** ukázky vstupní data a používá MapReduce k řazení dat. do celkové pořadí.</span><span class="sxs-lookup"><span data-stu-id="8e70f-182">**TeraSort** samples the input data and uses MapReduce to sort the data into a total order.</span></span> <span data-ttu-id="8e70f-183">TeraSort je standardní řazení funkcí MapReduce, s výjimkou vlastní dělicí metody, která používá seřazený seznam klíčů N-1 vzorků, které definují rozsah klíče pro každý snižte.</span><span class="sxs-lookup"><span data-stu-id="8e70f-183">TeraSort is a standard sort of MapReduce functions, except for a custom partitioner that uses a sorted list of N-1 sampled keys that define the key range for each reduce.</span></span> <span data-ttu-id="8e70f-184">Konkrétně, všechny klíče takové které ukázkové [i-1] < = klíč < ukázka [i] odešlou ke snížení i.</span><span class="sxs-lookup"><span data-stu-id="8e70f-184">In particular, all keys such that sample[i-1] <= key < sample[i] are sent to reduce i.</span></span> <span data-ttu-id="8e70f-185">To zaručuje, že výstupy snížit i jsou všechny menší než výstup snížit i + 1.</span><span class="sxs-lookup"><span data-stu-id="8e70f-185">This guarantees that the outputs of reduce i are all less than the output of reduce i+1.</span></span>
3. <span data-ttu-id="8e70f-186">**TeraValidate** je program MapReduce, která ověřuje, že výstup seřazen globálně.</span><span class="sxs-lookup"><span data-stu-id="8e70f-186">**TeraValidate** is a MapReduce program that validates that the output is globally sorted.</span></span> <span data-ttu-id="8e70f-187">Vytvoří jedna mapa každý soubor v adresáři výstup a každé mapování zajišťuje, že každý klíč je menší než nebo rovna předchozí.</span><span class="sxs-lookup"><span data-stu-id="8e70f-187">It creates one map per file in the output directory, and each map ensures that each key is less than or equal to the previous one.</span></span> <span data-ttu-id="8e70f-188">Funkce mapy vytvoří také záznamy první a poslední klíčů každého souboru a snižte funkce zajišťuje, že první klíč souboru i vyšší než poslední klíč i-1 souboru.</span><span class="sxs-lookup"><span data-stu-id="8e70f-188">The map function also generates records of the first and last keys of each file, and the reduce function ensures that the first key of file i is greater than the last key of file i-1.</span></span> <span data-ttu-id="8e70f-189">Jako výstup snižte hlásit problémy s klíči, které jsou mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="8e70f-189">Any problems are reported as an output of the reduce with the keys that are out of order.</span></span>

<span data-ttu-id="8e70f-190">Vstupní a výstupní formát, použít všechny tři aplikací, čte a zapisuje textových souborů ve správném formátu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-190">The input and output format, used by all three applications, reads and writes the text files in the right format.</span></span> <span data-ttu-id="8e70f-191">Výstup snižte má replikace nastavit hodnotu 1, místo výchozího 3, protože kontextu srovnávacího testu nevyžaduje, aby výstupní data replikovat na víc uzlů.</span><span class="sxs-lookup"><span data-stu-id="8e70f-191">The output of the reduce has replication set to 1, instead of the default 3, because the benchmark contest does not require that the output data be replicated on to multiple nodes.</span></span>

<span data-ttu-id="8e70f-192">Ukázka, každý odpovídající jednomu z MapReduce programy popsané v úvodu vyžadují tři úlohy:</span><span class="sxs-lookup"><span data-stu-id="8e70f-192">Three tasks are required by the sample, each corresponding to one of the MapReduce programs described in the introduction:</span></span>

1. <span data-ttu-id="8e70f-193">Generování dat pro řazení spuštěním **TeraGen** úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="8e70f-193">Generate the data for sorting by running the **TeraGen** MapReduce job.</span></span>
2. <span data-ttu-id="8e70f-194">Řazení dat spuštěním **TeraSort** úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="8e70f-194">Sort the data by running the **TeraSort** MapReduce job.</span></span>
3. <span data-ttu-id="8e70f-195">Potvrďte, že data správně seřazená spuštěním **TeraValidate** úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="8e70f-195">Confirm that the data has been correctly sorted by running the **TeraValidate** MapReduce job.</span></span>

<span data-ttu-id="8e70f-196">**K odeslání úlohy**</span><span class="sxs-lookup"><span data-stu-id="8e70f-196">**To submit the jobs**</span></span>

* <span data-ttu-id="8e70f-197">Postupujte podle pokynů v [aplikace Word počet - Java](#word-count-java)a použijte tyto definice úlohy:</span><span class="sxs-lookup"><span data-stu-id="8e70f-197">Follow the procedure in [Word count - Java](#word-count-java), and use the following job definitions:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8e70f-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e70f-198">Next steps</span></span>
<span data-ttu-id="8e70f-199">Z tohoto článku a články v každé z ukázky jste zjistili, jak ke spuštění ukázky, které jsou součástí clusterů HDInsight pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8e70f-199">From this article and the articles in each of the samples, you learned how to run the samples included with the HDInsight clusters by using Azure PowerShell.</span></span> <span data-ttu-id="8e70f-200">Kurzech k používání Pig, Hive a MapReduce s HDInsight naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="8e70f-200">For tutorials about using Pig, Hive, and MapReduce with HDInsight, see the following topics:</span></span>

* <span data-ttu-id="8e70f-201">[Začínáme používat Hadoop s Hive v HDInsight k analýze používání mobilního telefonu][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="8e70f-201">[Get started using Hadoop with Hive in HDInsight to analyze mobile handset use][hdinsight-get-started]</span></span>
* <span data-ttu-id="8e70f-202">[Použijte Pig s Hadoop v HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="8e70f-202">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="8e70f-203">[Použijte Hive s Hadoop v HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="8e70f-203">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="8e70f-204">[Odesílání úloh Hadoop v HDInsight][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="8e70f-204">[Submit Hadoop Jobs in HDInsight][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="8e70f-205">[Dokumentace k Azure HDInsight SDK][hdinsight-sdk-documentation]</span><span class="sxs-lookup"><span data-stu-id="8e70f-205">[Azure HDInsight SDK documentation][hdinsight-sdk-documentation]</span></span>

## <a name="appendix-a---the-word-count-source-code"></a><span data-ttu-id="8e70f-206">Příloha A - Word počet zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="8e70f-206">Appendix A - The Word count source code</span></span>

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

## <a name="appendix-b---the-word-count-streaming-source-code"></a><span data-ttu-id="8e70f-207">Dodatek B – počet slov streamování zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="8e70f-207">Appendix B - The word count streaming source code</span></span>
<span data-ttu-id="8e70f-208">MapReduce program cat.exe aplikace používá jako rozhraní mapování k vysílání datového proudu text do konzoly a wc.exe aplikace jako rozhraní snižte počet slova, která jsou datového proudu z dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-208">The MapReduce program uses the cat.exe application as a mapping interface to stream the text into the console and the wc.exe application as the reduce interface to count the number of words that are streamed from a document.</span></span> <span data-ttu-id="8e70f-209">Mapovač i reduktorem čtení znaků, řádek po řádku, od standardní vstupní proud (stdin) a zápisu do datového proudu standardního výstupního (stdout).</span><span class="sxs-lookup"><span data-stu-id="8e70f-209">Both the mapper and reducer read characters, line-by-line, from the standard input stream (stdin) and write to the standard output stream (stdout).</span></span>

```csharp
// The source code for the cat.exe (Mapper).

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

<span data-ttu-id="8e70f-210">Mapovač kód v souboru cat.cs používá [StreamReader] [ streamreader] objektu určeného ke čtení příchozího datového proudu ke konzole, který poté provede zápis do datového proudu do standardního výstupního datového proudu se statickými znaky[ Console.Writeline] [ console-writeline] metoda.</span><span class="sxs-lookup"><span data-stu-id="8e70f-210">The mapper code in the cat.cs file uses a [StreamReader][streamreader] object to read the characters of the incoming stream to the console, which then writes the stream to the standard output stream with the static [Console.Writeline][console-writeline] method.</span></span>

```csharp
// The source code for wc.exe (Reducer) is:

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

<span data-ttu-id="8e70f-211">Reduktorem kód v souboru wc.cs používá [StreamReader] [ streamreader] objektu určeného ke čtení znaků z standardní vstupní proud, který byl výstup mapovačem cat.exe.</span><span class="sxs-lookup"><span data-stu-id="8e70f-211">The reducer code in the wc.cs file uses a [StreamReader][streamreader]   object to read characters from the standard input stream that have been output by the cat.exe mapper.</span></span> <span data-ttu-id="8e70f-212">Jako přečte znaky s [Console.Writeline] [ console-writeline] metoda, počítá slova roven mezery a konec řádku znaky všech slov na konci.</span><span class="sxs-lookup"><span data-stu-id="8e70f-212">As it reads the characters with the [Console.Writeline][console-writeline] method, it counts the words by counting spaces and end-of-line characters at the end of each word.</span></span> <span data-ttu-id="8e70f-213">Celkový počet pak zapíše do standardního výstupního datového proudu s [Console.Writeline] [ console-writeline] metoda.</span><span class="sxs-lookup"><span data-stu-id="8e70f-213">It then writes the total to the standard output stream with the [Console.Writeline][console-writeline] method.</span></span>

## <a name="appendix-c---the-pi-estimator-source-code"></a><span data-ttu-id="8e70f-214">Příloha C – kód platformy odhadu zdroje</span><span class="sxs-lookup"><span data-stu-id="8e70f-214">Appendix C - The Pi estimator source code</span></span>
<span data-ttu-id="8e70f-215">Pi odhadu Java kód, který obsahuje funkce mapper a reduktorem k dispozici pro kontrolu níže.</span><span class="sxs-lookup"><span data-stu-id="8e70f-215">The pi estimator Java code that contains the mapper and reducer functions is available for inspection below.</span></span> <span data-ttu-id="8e70f-216">Mapper program generuje zadaný počet bodů, které jsou umístěny v náhodných uvnitř čtverce jednotky a pak udává počet z těchto bodů, které jsou uvnitř kruhu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-216">The mapper program generates a specified number of points placed at random inside of a unit square and then counts the number of those points that are inside the circle.</span></span> <span data-ttu-id="8e70f-217">Program reduktorem shromažďuje body zjištěné službou mappers a potom odhadne hodnotu čísla pí z vzorce 4R, kde R je poměr počtu bodů počítá uvnitř kruhu celkového počtu bodů, které jsou v rámci druhou mocninu.</span><span class="sxs-lookup"><span data-stu-id="8e70f-217">The reducer program accumulates points counted by the mappers and then estimates the value of pi from the formula 4R, where R is the ratio of the number of points counted inside the circle to the total number of points that are within the square.</span></span>

```java
/**
* Licensed to the Apache Software Foundation (ASF) under one
* or more contributor license agreements. See the NOTICE file
* distributed with this work for additional information
* regarding copyright ownership. The ASF licenses this file
* to you under the Apache License, Version 2.0 (the
* "License"); you may not use this file except in compliance
* with the License. You may obtain a copy of the License at
*
* http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed to in writing, software
* distributed under the License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or     implied.
* See the License for the specific language governing permissions and
* limitations under the License.
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

//A Map-reduce program to estimate the value of Pi
//using quasi-Monte Carlo method.
//
//Mapper:
//Generate points in a unit square
//and then count points inside/outside of the inscribed circle of the square.
//
//Reducer:
//Accumulate points inside/outside results from the mappers.
//Let numTotal = numInside + numOutside.
//The fraction numInside/numTotal is a rational approximation of
//the value (Area of the circle)/(Area of the square),
//where the area of the inscribed circle is Pi/4
//and the area of unit square is 1.
//Then, Pi is estimated value to be 4(numInside/numTotal).
//

public class PiEstimator extends Configured implements Tool {
//tmp directory for input/output
static private final Path TMP_DIR = new Path(
PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

//2-dimensional Halton sequence {H(i)},
//where H(i) is a 2-dimensional point and i >= 1 is the index.
//Halton sequence is used to generate sample points for Pi estimation.
private static class HaltonSequence {
// Bases
static final int[] P = {2, 3};
//Maximum number of digits allowed
static final int[] K = {63, 40};

private long index;
private double[] x;
private double[][] q;
private int[][] d;

//Initialize to H(startindex),
//so the sequence begins with H(startindex+1).
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
//Assume the current point is H(index).
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
//count points inside/outside of the inscribed circle of the square.
public static class PiMapper extends MapReduceBase
implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

//Map method.
//@param offset samples starting from the (offset+1)th sample.
//@param size the number of samples for this map
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

//count points inside/outside of the inscribed circle of the square
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
//Accumulate points inside/outside results from the mappers.
public static class PiReducer extends MapReduceBase
implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

private long numInside = 0;
private long numOutside = 0;
private JobConf conf; //configuration for accessing the file system

//Store job configuration.
@Override
public void configure(JobConf job) {
conf = job;
}

// Accumulate number of points inside/outside results from the mappers.
// @param isInside Is the points inside?
// @param values An iterator to a list of point counts
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

//Reduce task done, write output to a file.
@Override
public void close() throws IOException {
//write output to a file
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
//@return the estimated value of Pi.
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
// multiple writers to the same file.
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

## <a name="appendix-d---the-10gb-graysort-source-code"></a><span data-ttu-id="8e70f-218">Dodatek D – 10gb graysort zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="8e70f-218">Appendix D - The 10gb graysort source code</span></span>
<span data-ttu-id="8e70f-219">Pro kontroly v této části se zobrazí kód programu TeraSort MapReduce.</span><span class="sxs-lookup"><span data-stu-id="8e70f-219">The code for the TeraSort MapReduce program is presented for inspection in this section.</span></span>

```java
/**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements.  See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership.  The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License.  You may obtain a copy of the License at
    *
    *     http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
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
    * Generates the sampled split points, launches the job,
    * and waits for it to finish.
    * <p>
    * To run the program:
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
        * An inner trie node that contains 256 children based on the next
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
        * A leaf trie node that does string compares to figure out where the given
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
        * Read the cut points from the given sequence file.
        * @param fs the file system
        * @param p the path to read
        * @param job the job config
        * @return the strings to split the partitions on
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
        * Given a sorted set of cut points, build a trie that will find the correct
        * partition quickly.
        * @param splits the list of cut points
        * @param lower the lower bound of partitions 0..numPartitions-1
        * @param upper the upper bound of partitions 0..numPartitions-1
        * @param prefix the prefix that we have already checked against
        * @param maxDepth the maximum depth we will build a trie for
        * @return the trie node that will divide the splits correctly
        */
    private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                        Text prefix, int maxDepth) {
        int depth = prefix.getLength();
        if (depth >= maxDepth || lower == upper) {
        return new LeafTrieNode(depth, splits, lower, upper);
        }
        InnerTrieNode result = new InnerTrieNode(depth);
        Text trial = new Text(prefix);
        // append an extra byte on to the prefix
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
        // pick up the rest
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
