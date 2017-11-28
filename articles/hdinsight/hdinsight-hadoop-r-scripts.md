---
title: aaaUse R v clusterech HDInsight toocustomize - Azure | Microsoft Docs
description: "Zjistěte, jak pomocí tooinstall R skript akce a použití R na clustery HDInsight."
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
ms.openlocfilehash: bf5adf2e18dc43a743b29fd1567fad731b9c3ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="bd91d-103">Instalace a použití R na clusterech HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="bd91d-103">Install and use R on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="bd91d-104">Zjistěte, jak toocustomize Windows na základě clusteru HDInsight pomocí pomocí akce skriptu R a jak toouse R v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="bd91d-104">Learn how toocustomize Windows based HDInsight cluster with R using Script Action, and how toouse R on HDInsight clusters.</span></span> <span data-ttu-id="bd91d-105">Hello [HDInsight nabídky](https://azure.microsoft.com/pricing/details/hdinsight/) zahrnuje R Server v rámci clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bd91d-105">hello [HDInsight offering](https://azure.microsoft.com/pricing/details/hdinsight/) includes R Server as part of your HDInsight cluster.</span></span> <span data-ttu-id="bd91d-106">To umožňuje skripty R výpočtů toorun distribuované toouse MapReduce a Spark.</span><span class="sxs-lookup"><span data-stu-id="bd91d-106">This allows R scripts toouse MapReduce and Spark toorun distributed computations.</span></span> <span data-ttu-id="bd91d-107">Další informace naleznete v tématu [Začínáme používat R Server v HDInsight](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bd91d-107">For more information, see [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md).</span></span> <span data-ttu-id="bd91d-108">Informace o používání R s clusteru se systémem Linux najdete v tématu [instalací a použitím R na clusterů systému HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span><span class="sxs-lookup"><span data-stu-id="bd91d-108">For information on using R with a Linux-based cluster, see [Install and use R on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span></span>

<span data-ttu-id="bd91d-109">R můžete nainstalovat na libovolný typ clusteru (Hadoop, Spark, HBase, Storm) v Azure HDInsight pomocí *akce skriptu*.</span><span class="sxs-lookup"><span data-stu-id="bd91d-109">You can install R on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="bd91d-110">Tooinstall skriptu ukázka R na clusteru služby HDInsight je k dispozici z objektu blob úložiště Azure jen pro čtení v [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="bd91d-110">A sample script tooinstall R on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

<span data-ttu-id="bd91d-111">**Související články**</span><span class="sxs-lookup"><span data-stu-id="bd91d-111">**Related articles**</span></span>

* [<span data-ttu-id="bd91d-112">Na nainstalovat a používat R clusterů systému HDinsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="bd91d-112">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="bd91d-113">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Obecné informace o vytváření clusterů HDInsight</span><span class="sxs-lookup"><span data-stu-id="bd91d-113">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="bd91d-114">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="bd91d-114">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="bd91d-115">Vývoj skriptů akce skriptu pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="bd91d-115">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a><span data-ttu-id="bd91d-116">Co je R?</span><span class="sxs-lookup"><span data-stu-id="bd91d-116">What is R?</span></span>
<span data-ttu-id="bd91d-117">Hello <a href="http://www.r-project.org/" target="_blank">R projekt pro statistické výpočty</a> je open source jazyk a prostředí pro statistické výpočty.</span><span class="sxs-lookup"><span data-stu-id="bd91d-117">hello <a href="http://www.r-project.org/" target="_blank">R Project for Statistical Computing</a> is an open source language and environment for statistical computing.</span></span> <span data-ttu-id="bd91d-118">R poskytuje stovky sestavení v statistických funkcí a vlastní programovací jazyk, který kombinuje aspektů funkčnosti a objektově orientované programování.</span><span class="sxs-lookup"><span data-stu-id="bd91d-118">R provides hundreds of build-in statistical functions and its own programming language that combines aspects of functional and object-oriented programming.</span></span> <span data-ttu-id="bd91d-119">Také poskytuje rozsáhlé možnosti grafického rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bd91d-119">It also provides extensive graphical capabilities.</span></span> <span data-ttu-id="bd91d-120">R je hello upřednostňované programovací prostředí pro nejvíce professional statistikami a vědců v celé řadě polí.</span><span class="sxs-lookup"><span data-stu-id="bd91d-120">R is hello preferred programming environment for most professional statisticians and scientists in a wide variety of fields.</span></span>

<span data-ttu-id="bd91d-121">R je kompatibilní s Azure Blob Storage (WASB) tak, aby data, která je uložená existuje lze zpracovat využití R v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bd91d-121">R is compatible with Azure Blob Storage (WASB) so that data that is stored there can be processed using R on HDInsight.</span></span>  

## <a name="install-r"></a><span data-ttu-id="bd91d-122">Nainstalujte jazyk R</span><span class="sxs-lookup"><span data-stu-id="bd91d-122">Install R</span></span>
<span data-ttu-id="bd91d-123">A [ukázkový skript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) tooinstall R na clusteru HDInsight má k dispozici jen pro čtení objektů blob v Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd91d-123">A [sample script](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) tooinstall R on an HDInsight cluster is available from a read-only blob in Azure Storage.</span></span> <span data-ttu-id="bd91d-124">Tato část obsahuje pokyny, jak toouse hello ukázkový skript při vytváření clusteru hello pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bd91d-124">This section provides instructions about how toouse hello sample script while creating hello cluster using hello Azure Portal.</span></span>

> [!NOTE]
> <span data-ttu-id="bd91d-125">Ukázkový skript Hello byla zavedena v systému verze clusteru HDInsight verze 3.1.</span><span class="sxs-lookup"><span data-stu-id="bd91d-125">hello sample script was introduced with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="bd91d-126">Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="bd91d-126">For more information about  HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

1. <span data-ttu-id="bd91d-127">Při vytváření clusteru služby HDInsight z hello portálu, klikněte na tlačítko **volitelné konfiguraci**a potom klikněte na **akcí skriptů**.</span><span class="sxs-lookup"><span data-stu-id="bd91d-127">When you create an HDInsight cluster from hello Portal, click **Optional Configuration**, and then click **Script Actions**.</span></span>
2. <span data-ttu-id="bd91d-128">Na hello **akcí skriptů** zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="bd91d-128">On hello **Script Actions** page, enter hello following values:</span></span>

    <span data-ttu-id="bd91d-129">![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "použití akce skriptu toocustomize clusteru")</span><span class="sxs-lookup"><span data-stu-id="bd91d-129">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="bd91d-130">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bd91d-130">Property</span></span></th><th><span data-ttu-id="bd91d-131">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bd91d-131">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="bd91d-132">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="bd91d-132">Name</span></span></td>
            <td><span data-ttu-id="bd91d-133">Zadejte název akce skriptu hello, například <b>nainstalovat R</b>.</span><span class="sxs-lookup"><span data-stu-id="bd91d-133">Specify a name for hello script action, for example, <b>Install R</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="bd91d-134">Identifikátor URI skriptu</span><span class="sxs-lookup"><span data-stu-id="bd91d-134">Script URI</span></span></td>
            <td><span data-ttu-id="bd91d-135">Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello cluster, například <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span><span class="sxs-lookup"><span data-stu-id="bd91d-135">Specify hello URI toohello script that is invoked toocustomize hello cluster, for example, <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="bd91d-136">Typ uzlu</span><span class="sxs-lookup"><span data-stu-id="bd91d-136">Node Type</span></span></td>
            <td><span data-ttu-id="bd91d-137">Zadejte hello uzly, na kterých běží hello přizpůsobení skriptu.</span><span class="sxs-lookup"><span data-stu-id="bd91d-137">Specify hello nodes on which hello customization script is run.</span></span> <span data-ttu-id="bd91d-138">Můžete zvolit <b>všechny uzly</b>, <b>hlavní pouze uzly</b>, nebo <b>uzlů pracovního procesu</b> pouze.</span><span class="sxs-lookup"><span data-stu-id="bd91d-138">You can choose <b>All Nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes</b> only.</span></span>
        <tr><td><span data-ttu-id="bd91d-139">Parametry</span><span class="sxs-lookup"><span data-stu-id="bd91d-139">Parameters</span></span></td>
            <td><span data-ttu-id="bd91d-140">Zadejte parametry hello, pokud to vyžaduje hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="bd91d-140">Specify hello parameters, if required by hello script.</span></span> <span data-ttu-id="bd91d-141">Hello tooinstall skriptu R však nevyžaduje žádné parametry, takže to můžete nechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="bd91d-141">However, hello script tooinstall R does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="bd91d-142">Více součástí, které můžete přidat více než jeden tooinstall akce skriptu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="bd91d-142">You can add more than one script action tooinstall multiple components on hello cluster.</span></span> <span data-ttu-id="bd91d-143">Po přidání hello skripty, klikněte na tlačítko toostart zaškrtnutí hello crating hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="bd91d-143">After you have added hello scripts, click hello check mark toostart crating hello cluster.</span></span>

<span data-ttu-id="bd91d-144">Můžete taky tooinstall hello skriptu R v HDInsight pomocí prostředí Azure PowerShell nebo hello HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="bd91d-144">You can also use hello script tooinstall R on HDInsight by using Azure PowerShell or hello HDInsight .NET SDK.</span></span> <span data-ttu-id="bd91d-145">Pokyny pro tyto postupy jsou uvedeny dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="bd91d-145">Instructions for these procedures are provided later in this article.</span></span>

## <a name="run-r-scripts"></a><span data-ttu-id="bd91d-146">Spusťte skripty R</span><span class="sxs-lookup"><span data-stu-id="bd91d-146">Run R scripts</span></span>
<span data-ttu-id="bd91d-147">Tato část popisuje, jak clusteru toorun R skript na hello Hadoop v prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bd91d-147">This section describes how toorun an R script on hello Hadoop cluster with HDInsight.</span></span>

1. <span data-ttu-id="bd91d-148">**Vytvoření clusteru toohello připojení vzdálené plochy**: Z hello portál, povolení vzdálené plochy pro hello clusteru, které jste vytvořili pomocí R nainstalované a potom se připojte toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="bd91d-148">**Establish a Remote Desktop connection toohello cluster**: From hello Portal, enable Remote Desktop for hello cluster you created with R installed, and then connect toohello cluster.</span></span> <span data-ttu-id="bd91d-149">Pokyny najdete v tématu [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="bd91d-149">For instructions, see [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="bd91d-150">**Otevřete hello R konzoly**: instalace hello R vloží konzoly toohello R odkaz na ploše hello hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="bd91d-150">**Open hello R console**: hello R installation puts a link toohello R console on hello desktop of hello head node.</span></span> <span data-ttu-id="bd91d-151">Klikněte na jeho tooopen hello R konzoly.</span><span class="sxs-lookup"><span data-stu-id="bd91d-151">Click on it tooopen hello R console.</span></span>
3. <span data-ttu-id="bd91d-152">**Spusťte skript hello R**: hello R skript můžete spustit přímo z konzoly hello R vložením, ho vyberete a stisknutím klávesy ENTER.</span><span class="sxs-lookup"><span data-stu-id="bd91d-152">**Run hello R script**: hello R script can be run directly from hello R console by pasting it, selecting it, and pressing ENTER.</span></span> <span data-ttu-id="bd91d-153">Zde je jednoduchý příklad skript, který generuje hello čísla 1 too100 a potom je vynásobí 2.</span><span class="sxs-lookup"><span data-stu-id="bd91d-153">Here is a simple example script that generates hello numbers 1 too100 and then multiplies them by 2.</span></span>

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

<span data-ttu-id="bd91d-154">Hello první dva řádky volání hello RHadoop knihovny, které jsou nainstalované s R. hello poslední řádek výtisků hello výsledky toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="bd91d-154">hello first two lines call hello RHadoop libraries that are installed with R. hello final line prints hello results toohello console.</span></span> <span data-ttu-id="bd91d-155">výstup Hello by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="bd91d-155">hello output should look like this:</span></span>

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a><span data-ttu-id="bd91d-156">Nainstalujte jazyk R pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd91d-156">Install R using Aure PowerShell</span></span>
<span data-ttu-id="bd91d-157">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="bd91d-157">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="bd91d-158">Ukázka Hello ukazuje, jak tooinstall Spark pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd91d-158">hello sample demonstrates how tooinstall Spark using Azure PowerShell.</span></span> <span data-ttu-id="bd91d-159">Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="bd91d-159">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

## <a name="install-r-using-net-sdk"></a><span data-ttu-id="bd91d-160">Nainstalujte jazyk R pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="bd91d-160">Install R using .NET SDK</span></span>
<span data-ttu-id="bd91d-161">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="bd91d-161">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="bd91d-162">Ukázka Hello ukazuje, jak tooinstall Spark pomocí hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="bd91d-162">hello sample demonstrates how tooinstall Spark using hello .NET SDK.</span></span> <span data-ttu-id="bd91d-163">Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span><span class="sxs-lookup"><span data-stu-id="bd91d-163">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span></span>

## <a name="see-also"></a><span data-ttu-id="bd91d-164">Viz také</span><span class="sxs-lookup"><span data-stu-id="bd91d-164">See also</span></span>
* [<span data-ttu-id="bd91d-165">Na nainstalovat a používat R clusterů systému HDinsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="bd91d-165">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="bd91d-166">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Obecné informace o vytváření clusterů HDInsight</span><span class="sxs-lookup"><span data-stu-id="bd91d-166">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="bd91d-167">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="bd91d-167">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="bd91d-168">Vývoj skriptů akce skriptu pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="bd91d-168">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)
* <span data-ttu-id="bd91d-169">[Nainstalovat a používat Spark v clusterech HDInsight][hdinsight-install-spark]: akce skriptu vzorku o instalaci Spark</span><span class="sxs-lookup"><span data-stu-id="bd91d-169">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark</span></span>
* <span data-ttu-id="bd91d-170">[Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install.md): akce skriptu vzorku o instalaci Giraph</span><span class="sxs-lookup"><span data-stu-id="bd91d-170">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph</span></span>
* <span data-ttu-id="bd91d-171">[Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install-linux.md): akce skriptu vzorku o instalaci Solr.</span><span class="sxs-lookup"><span data-stu-id="bd91d-171">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md): Script Action sample about installing Solr.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
