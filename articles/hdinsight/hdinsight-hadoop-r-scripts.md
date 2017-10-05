---
title: "Využití R v HDInsight k přizpůsobení clusterů - Azure | Microsoft Docs"
description: "Naučte se instalovat pomocí akce skriptu R a použijte prostředí R v clusterech prostředí HDInsight."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: be851270-afa5-4af0-a69e-2d343a4deeb7
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 5b9b793d49217acd9f0c6c518596a7afb5600d69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="c10e2-103">Instalace a použití R na clusterech HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="c10e2-103">Install and use R on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="c10e2-104">Zjistěte, jak pro přizpůsobení systému Windows na základě clusteru HDInsight pomocí pomocí akce skriptu R a clusterů v tom, jak používat R v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c10e2-104">Learn how to customize Windows based HDInsight cluster with R using Script Action, and how to use R on HDInsight clusters.</span></span> <span data-ttu-id="c10e2-105">[HDInsight nabídky](https://azure.microsoft.com/pricing/details/hdinsight/) zahrnuje R Server v rámci clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c10e2-105">The [HDInsight offering](https://azure.microsoft.com/pricing/details/hdinsight/) includes R Server as part of your HDInsight cluster.</span></span> <span data-ttu-id="c10e2-106">To umožňuje skripty R se použije ke spuštění distribuovaných výpočty MapReduce a Spark.</span><span class="sxs-lookup"><span data-stu-id="c10e2-106">This allows R scripts to use MapReduce and Spark to run distributed computations.</span></span> <span data-ttu-id="c10e2-107">Další informace naleznete v tématu [Začínáme používat R Server v HDInsight](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c10e2-107">For more information, see [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md).</span></span> <span data-ttu-id="c10e2-108">Informace o používání R s clusteru se systémem Linux najdete v tématu [instalací a použitím R na clusterů systému HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c10e2-108">For information on using R with a Linux-based cluster, see [Install and use R on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span></span>

<span data-ttu-id="c10e2-109">R můžete nainstalovat na libovolný typ clusteru (Hadoop, Spark, HBase, Storm) v Azure HDInsight pomocí *akce skriptu*.</span><span class="sxs-lookup"><span data-stu-id="c10e2-109">You can install R on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="c10e2-110">Ukázkový skript instalace R na clusteru služby HDInsight je k dispozici z objektu blob úložiště Azure jen pro čtení v [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="c10e2-110">A sample script to install R on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

<span data-ttu-id="c10e2-111">**Související články**</span><span class="sxs-lookup"><span data-stu-id="c10e2-111">**Related articles**</span></span>

* [<span data-ttu-id="c10e2-112">Na nainstalovat a používat R clusterů systému HDinsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="c10e2-112">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="c10e2-113">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Obecné informace o vytváření clusterů HDInsight</span><span class="sxs-lookup"><span data-stu-id="c10e2-113">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="c10e2-114">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="c10e2-114">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="c10e2-115">Vývoj skriptů akce skriptu pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="c10e2-115">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a><span data-ttu-id="c10e2-116">Co je R?</span><span class="sxs-lookup"><span data-stu-id="c10e2-116">What is R?</span></span>
<span data-ttu-id="c10e2-117"><a href="http://www.r-project.org/" target="_blank">R projekt pro statistické výpočty</a> je open source jazyk a prostředí pro statistické výpočty.</span><span class="sxs-lookup"><span data-stu-id="c10e2-117">The <a href="http://www.r-project.org/" target="_blank">R Project for Statistical Computing</a> is an open source language and environment for statistical computing.</span></span> <span data-ttu-id="c10e2-118">R poskytuje stovky sestavení v statistických funkcí a vlastní programovací jazyk, který kombinuje aspektů funkčnosti a objektově orientované programování.</span><span class="sxs-lookup"><span data-stu-id="c10e2-118">R provides hundreds of build-in statistical functions and its own programming language that combines aspects of functional and object-oriented programming.</span></span> <span data-ttu-id="c10e2-119">Také poskytuje rozsáhlé možnosti grafického rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c10e2-119">It also provides extensive graphical capabilities.</span></span> <span data-ttu-id="c10e2-120">R je upřednostňovaný programovací prostředí pro nejvíce professional statistikami a vědců v celé řadě polí.</span><span class="sxs-lookup"><span data-stu-id="c10e2-120">R is the preferred programming environment for most professional statisticians and scientists in a wide variety of fields.</span></span>

<span data-ttu-id="c10e2-121">R je kompatibilní s Azure Blob Storage (WASB) tak, aby data, která je uložená existuje lze zpracovat využití R v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c10e2-121">R is compatible with Azure Blob Storage (WASB) so that data that is stored there can be processed using R on HDInsight.</span></span>  

## <a name="install-r"></a><span data-ttu-id="c10e2-122">Nainstalujte jazyk R</span><span class="sxs-lookup"><span data-stu-id="c10e2-122">Install R</span></span>
<span data-ttu-id="c10e2-123">A [ukázkový skript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) nainstalovat na HDInsight clusteru má k dispozici jen pro čtení objektů blob v Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c10e2-123">A [sample script](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) to install R on an HDInsight cluster is available from a read-only blob in Azure Storage.</span></span> <span data-ttu-id="c10e2-124">Tato část obsahuje informace o tom, jak pomocí vzorového skriptu při vytváření clusteru pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c10e2-124">This section provides instructions about how to use the sample script while creating the cluster using the Azure Portal.</span></span>

> [!NOTE]
> <span data-ttu-id="c10e2-125">Ukázkový skript byla zavedena v systému verze clusteru HDInsight verze 3.1.</span><span class="sxs-lookup"><span data-stu-id="c10e2-125">The sample script was introduced with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="c10e2-126">Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="c10e2-126">For more information about  HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

1. <span data-ttu-id="c10e2-127">Při vytváření clusteru služby HDInsight z portálu, klikněte na tlačítko **volitelné konfiguraci**a potom klikněte na **akcí skriptů**.</span><span class="sxs-lookup"><span data-stu-id="c10e2-127">When you create an HDInsight cluster from the Portal, click **Optional Configuration**, and then click **Script Actions**.</span></span>
2. <span data-ttu-id="c10e2-128">Na **akcí skriptů** stránky, zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="c10e2-128">On the **Script Actions** page, enter the following values:</span></span>

    <span data-ttu-id="c10e2-129">![Použití akce skriptu k přizpůsobení cluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "použití akce skriptu k přizpůsobení clusteru")</span><span class="sxs-lookup"><span data-stu-id="c10e2-129">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Use Script Action to customize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="c10e2-130">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c10e2-130">Property</span></span></th><th><span data-ttu-id="c10e2-131">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c10e2-131">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="c10e2-132">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c10e2-132">Name</span></span></td>
            <td><span data-ttu-id="c10e2-133">Zadejte název pro skript akce, například <b>nainstalovat R</b>.</span><span class="sxs-lookup"><span data-stu-id="c10e2-133">Specify a name for the script action, for example, <b>Install R</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="c10e2-134">Identifikátor URI skriptu</span><span class="sxs-lookup"><span data-stu-id="c10e2-134">Script URI</span></span></td>
            <td><span data-ttu-id="c10e2-135">Zadejte identifikátor URI skriptu, která je volána, chcete-li přizpůsobit clusteru, například <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span><span class="sxs-lookup"><span data-stu-id="c10e2-135">Specify the URI to the script that is invoked to customize the cluster, for example, <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="c10e2-136">Typ uzlu</span><span class="sxs-lookup"><span data-stu-id="c10e2-136">Node Type</span></span></td>
            <td><span data-ttu-id="c10e2-137">Zadejte uzly, na kterých běží přizpůsobení skriptu.</span><span class="sxs-lookup"><span data-stu-id="c10e2-137">Specify the nodes on which the customization script is run.</span></span> <span data-ttu-id="c10e2-138">Můžete zvolit <b>všechny uzly</b>, <b>hlavní pouze uzly</b>, nebo <b>uzlů pracovního procesu</b> pouze.</span><span class="sxs-lookup"><span data-stu-id="c10e2-138">You can choose <b>All Nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes</b> only.</span></span>
        <tr><td><span data-ttu-id="c10e2-139">Parametry</span><span class="sxs-lookup"><span data-stu-id="c10e2-139">Parameters</span></span></td>
            <td><span data-ttu-id="c10e2-140">Zadejte parametry, pokud se vyžadují skriptem.</span><span class="sxs-lookup"><span data-stu-id="c10e2-140">Specify the parameters, if required by the script.</span></span> <span data-ttu-id="c10e2-141">Skript, který chcete nainstalovat, ale nevyžaduje žádné parametry, tak to můžete nechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="c10e2-141">However, the script to install R does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="c10e2-142">Můžete přidat více než jednu akci skriptu pro instalaci více součástí v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c10e2-142">You can add more than one script action to install multiple components on the cluster.</span></span> <span data-ttu-id="c10e2-143">Po přidání skripty, kliknutím na značku zaškrtnutí zahájíte crating clusteru.</span><span class="sxs-lookup"><span data-stu-id="c10e2-143">After you have added the scripts, click the check mark to start crating the cluster.</span></span>

<span data-ttu-id="c10e2-144">Tento skript můžete použít také k instalaci R v HDInsight pomocí prostředí Azure PowerShell nebo sady SDK rozhraní .NET HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c10e2-144">You can also use the script to install R on HDInsight by using Azure PowerShell or the HDInsight .NET SDK.</span></span> <span data-ttu-id="c10e2-145">Pokyny pro tyto postupy jsou uvedeny dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c10e2-145">Instructions for these procedures are provided later in this article.</span></span>

## <a name="run-r-scripts"></a><span data-ttu-id="c10e2-146">Spusťte skripty R</span><span class="sxs-lookup"><span data-stu-id="c10e2-146">Run R scripts</span></span>
<span data-ttu-id="c10e2-147">Tato část popisuje, jak spustit skript v jazyce R na clusteru Hadoop v prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c10e2-147">This section describes how to run an R script on the Hadoop cluster with HDInsight.</span></span>

1. <span data-ttu-id="c10e2-148">**Navázat připojení ke vzdálené ploše do clusteru**: povolení služby Vzdálená plocha pro cluster, který jste vytvořili pomocí R nainstalované z aplikace Portal a připojte se ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="c10e2-148">**Establish a Remote Desktop connection to the cluster**: From the Portal, enable Remote Desktop for the cluster you created with R installed, and then connect to the cluster.</span></span> <span data-ttu-id="c10e2-149">Pokyny najdete v tématu [připojení ke clusterům HDInsight pomocí protokolu RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="c10e2-149">For instructions, see [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="c10e2-150">**Otevřete konzolu R**: instalace R vloží odkaz ke konzole R na ploše hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="c10e2-150">**Open the R console**: The R installation puts a link to the R console on the desktop of the head node.</span></span> <span data-ttu-id="c10e2-151">Klikněte na něj pro otevření konzoly R.</span><span class="sxs-lookup"><span data-stu-id="c10e2-151">Click on it to open the R console.</span></span>
3. <span data-ttu-id="c10e2-152">**Spustit skript jazyka R**: R skript můžete spustit přímo z konzoly R vložením, ho vyberete a stisknutím klávesy ENTER.</span><span class="sxs-lookup"><span data-stu-id="c10e2-152">**Run the R script**: The R script can be run directly from the R console by pasting it, selecting it, and pressing ENTER.</span></span> <span data-ttu-id="c10e2-153">Zde je jednoduchý příklad skript, který generuje čísla 1 až 100 a potom je vynásobí 2.</span><span class="sxs-lookup"><span data-stu-id="c10e2-153">Here is a simple example script that generates the numbers 1 to 100 and then multiplies them by 2.</span></span>

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

<span data-ttu-id="c10e2-154">První dva řádky volání RHadoop knihovny, které jsou nainstalované s R. Poslední řádek vytiskne výsledků do konzoly.</span><span class="sxs-lookup"><span data-stu-id="c10e2-154">The first two lines call the RHadoop libraries that are installed with R. The final line prints the results to the console.</span></span> <span data-ttu-id="c10e2-155">Výstup by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c10e2-155">The output should look like this:</span></span>

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a><span data-ttu-id="c10e2-156">Nainstalujte jazyk R pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c10e2-156">Install R using Aure PowerShell</span></span>
<span data-ttu-id="c10e2-157">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="c10e2-157">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="c10e2-158">Ukázka ukazuje, jak nainstalovat Spark pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c10e2-158">The sample demonstrates how to install Spark using Azure PowerShell.</span></span> <span data-ttu-id="c10e2-159">Je nutné přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="c10e2-159">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

## <a name="install-r-using-net-sdk"></a><span data-ttu-id="c10e2-160">Nainstalujte jazyk R pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c10e2-160">Install R using .NET SDK</span></span>
<span data-ttu-id="c10e2-161">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="c10e2-161">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="c10e2-162">Ukázka ukazuje, jak nainstalovat Spark pomocí sady .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="c10e2-162">The sample demonstrates how to install Spark using the .NET SDK.</span></span> <span data-ttu-id="c10e2-163">Je nutné přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span><span class="sxs-lookup"><span data-stu-id="c10e2-163">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span></span>

## <a name="see-also"></a><span data-ttu-id="c10e2-164">Viz také</span><span class="sxs-lookup"><span data-stu-id="c10e2-164">See also</span></span>
* [<span data-ttu-id="c10e2-165">Na nainstalovat a používat R clusterů systému HDinsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="c10e2-165">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="c10e2-166">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Obecné informace o vytváření clusterů HDInsight</span><span class="sxs-lookup"><span data-stu-id="c10e2-166">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="c10e2-167">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="c10e2-167">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="c10e2-168">Vývoj skriptů akce skriptu pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="c10e2-168">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)
* <span data-ttu-id="c10e2-169">[Nainstalovat a používat Spark v clusterech HDInsight][hdinsight-install-spark]: akce skriptu vzorku o instalaci Spark</span><span class="sxs-lookup"><span data-stu-id="c10e2-169">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark</span></span>
* <span data-ttu-id="c10e2-170">[Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install.md): akce skriptu vzorku o instalaci Giraph</span><span class="sxs-lookup"><span data-stu-id="c10e2-170">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph</span></span>
* <span data-ttu-id="c10e2-171">[Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install-linux.md): akce skriptu vzorku o instalaci Solr.</span><span class="sxs-lookup"><span data-stu-id="c10e2-171">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md): Script Action sample about installing Solr.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
