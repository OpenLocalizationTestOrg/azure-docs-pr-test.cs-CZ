---
title: "aaaInstall a používat Giraph v HDInsight (Hadoop) - Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall Giraph na HDInsight se systémem Linux clustery pomocí akcí skriptů. Akce skriptu umožní toocustomize hello clusteru během vytváření, změně konfigurace clusteru nebo instalací služby a nástroje."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 0f195b65cebf5e24d1808ef33b95b4d362555521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-tooprocess-large-scale-graphs"></a><span data-ttu-id="6c55f-104">Nainstalujte Giraph clusterů systému HDInsight Hadoop a používat Giraph tooprocess rozsáhlé grafy</span><span class="sxs-lookup"><span data-stu-id="6c55f-104">Install Giraph on HDInsight Hadoop clusters, and use Giraph tooprocess large-scale graphs</span></span>

<span data-ttu-id="6c55f-105">Zjistěte, jak tooinstall Apache Giraph v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6c55f-105">Learn how tooinstall Apache Giraph on an HDInsight cluster.</span></span> <span data-ttu-id="6c55f-106">Hello funkce akce skriptu HDInsight vám umožní toocustomize clusteru spuštěním skriptu prostředí bash.</span><span class="sxs-lookup"><span data-stu-id="6c55f-106">hello script action feature of HDInsight allows you toocustomize your cluster by running a bash script.</span></span> <span data-ttu-id="6c55f-107">Skripty lze použít toocustomize clustery během a po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="6c55f-107">Scripts can be used toocustomize clusters during and after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6c55f-108">Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="6c55f-108">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="6c55f-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6c55f-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="6c55f-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="6c55f-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="6c55f-111"><a name="whatis"></a>Co je Giraph</span><span class="sxs-lookup"><span data-stu-id="6c55f-111"><a name="whatis"></a>What is Giraph</span></span>

<span data-ttu-id="6c55f-112">[Apache Giraph](http://giraph.apache.org/) vám umožní tooperform grafu zpracování pomocí Hadoop a lze použít s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6c55f-112">[Apache Giraph](http://giraph.apache.org/) allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span> <span data-ttu-id="6c55f-113">Grafy model relace mezi objekty.</span><span class="sxs-lookup"><span data-stu-id="6c55f-113">Graphs model relationships between objects.</span></span> <span data-ttu-id="6c55f-114">Například hello připojení mezi směrovači v velkým síťovým typu hello Internet nebo vztahy mezi uživateli v sociálních sítích.</span><span class="sxs-lookup"><span data-stu-id="6c55f-114">For example, hello connections between routers on a large network like hello Internet, or relationships between people on social networks.</span></span> <span data-ttu-id="6c55f-115">Graf zpracování vám umožní tooreason o hello vztahy mezi objekty v grafu, jako například:</span><span class="sxs-lookup"><span data-stu-id="6c55f-115">Graph processing allows you tooreason about hello relationships between objects in a graph, such as:</span></span>

* <span data-ttu-id="6c55f-116">Identifikace potenciální přátel na základě vaší aktuální relací.</span><span class="sxs-lookup"><span data-stu-id="6c55f-116">Identifying potential friends based on your current relationships.</span></span>

* <span data-ttu-id="6c55f-117">Identifikace hello nejkratší trasy mezi dvěma počítači v síti.</span><span class="sxs-lookup"><span data-stu-id="6c55f-117">Identifying hello shortest route between two computers in a network.</span></span>

* <span data-ttu-id="6c55f-118">Výpočet pořadí stránku hello webové stránky.</span><span class="sxs-lookup"><span data-stu-id="6c55f-118">Calculating hello page rank of webpages.</span></span>

> [!WARNING]
> <span data-ttu-id="6c55f-119">Součásti, které jsou součástí clusteru HDInsight hello plně podporované – Microsoft Support pomáhá tooisolate a vyřešit problémy související toothese součásti.</span><span class="sxs-lookup"><span data-stu-id="6c55f-119">Components provided with hello HDInsight cluster are fully supported - Microsoft Support helps tooisolate and resolve issues related toothese components.</span></span>
>
> <span data-ttu-id="6c55f-120">Vlastní komponenty, například Giraph, přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello.</span><span class="sxs-lookup"><span data-stu-id="6c55f-120">Custom components, such as Giraph, receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="6c55f-121">Microsoft Support může být schopný tooresolving hello problém.</span><span class="sxs-lookup"><span data-stu-id="6c55f-121">Microsoft Support may be able tooresolving hello issue.</span></span> <span data-ttu-id="6c55f-122">Pokud ne, musíte obrátit komunit s otevřeným zdrojem, kterých se nachází hluboké znalosti pro tuto technologii.</span><span class="sxs-lookup"><span data-stu-id="6c55f-122">If not, you must consult open source communities where deep expertise for that technology is found.</span></span> <span data-ttu-id="6c55f-123">Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="6c55f-123">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>


## <a name="what-hello-script-does"></a><span data-ttu-id="6c55f-124">Jaké skript hello</span><span class="sxs-lookup"><span data-stu-id="6c55f-124">What hello script does</span></span>

<span data-ttu-id="6c55f-125">Tento skript provede hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="6c55f-125">This script performs hello following actions:</span></span>

* <span data-ttu-id="6c55f-126">Nainstaluje Giraph příliš`/usr/hdp/current/giraph`</span><span class="sxs-lookup"><span data-stu-id="6c55f-126">Installs Giraph too`/usr/hdp/current/giraph`</span></span>

* <span data-ttu-id="6c55f-127">Kopie hello `giraph-examples.jar` toodefault úložiště (WASB) pro váš cluster souborů:`/example/jars/giraph-examples.jar`</span><span class="sxs-lookup"><span data-stu-id="6c55f-127">Copies hello `giraph-examples.jar` file toodefault storage (WASB) for your cluster: `/example/jars/giraph-examples.jar`</span></span>

## <span data-ttu-id="6c55f-128"><a name="install"></a>Nainstalujte Giraph pomocí akcí skriptů</span><span class="sxs-lookup"><span data-stu-id="6c55f-128"><a name="install"></a>Install Giraph using Script Actions</span></span>

<span data-ttu-id="6c55f-129">Tooinstall skriptu ukázka Giraph v clusteru HDInsight je k dispozici na hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="6c55f-129">A sample script tooinstall Giraph on an HDInsight cluster is available at hello following location:</span></span>

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

<span data-ttu-id="6c55f-130">Tato část obsahuje pokyny jak toouse hello ukázkový skript při vytváření clusteru hello pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6c55f-130">This section provides instructions on how toouse hello sample script while creating hello cluster by using hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="6c55f-131">Akce skriptu můžete použít některé z následujících metod hello používá:</span><span class="sxs-lookup"><span data-stu-id="6c55f-131">A script action can be applied using any of hello following methods:</span></span>
> * <span data-ttu-id="6c55f-132">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c55f-132">Azure PowerShell</span></span>
> * <span data-ttu-id="6c55f-133">Hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="6c55f-133">hello Azure CLI</span></span>
> * <span data-ttu-id="6c55f-134">Hello HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="6c55f-134">hello HDInsight .NET SDK</span></span>
> * <span data-ttu-id="6c55f-135">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="6c55f-135">Azure Resource Manager templates</span></span>
> 
> <span data-ttu-id="6c55f-136">Můžete také použít skript akce tooalready spuštěné clustery.</span><span class="sxs-lookup"><span data-stu-id="6c55f-136">You can also apply script actions tooalready running clusters.</span></span> <span data-ttu-id="6c55f-137">Další informace najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6c55f-137">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

1. <span data-ttu-id="6c55f-138">Zahájení vytváření clusteru pomocí kroků hello v [clustery HDInsight se systémem Linux vytvořit](hdinsight-hadoop-create-linux-clusters-portal.md), ale nedokončila vytvoření.</span><span class="sxs-lookup"><span data-stu-id="6c55f-138">Start creating a cluster by using hello steps in [Create Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md), but do not complete creation.</span></span>

2. <span data-ttu-id="6c55f-139">Na hello **volitelné konfiguraci** vyberte **akcí skriptů**a zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="6c55f-139">On hello **Optional Configuration** blade, select **Script Actions**, and provide hello following information:</span></span>

   * <span data-ttu-id="6c55f-140">**NÁZEV**: Zadejte popisný název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="6c55f-140">**NAME**: Enter a friendly name for hello script action.</span></span>

   * <span data-ttu-id="6c55f-141">**Identifikátor URI skriptu**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh</span><span class="sxs-lookup"><span data-stu-id="6c55f-141">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh</span></span>

   * <span data-ttu-id="6c55f-142">**HEAD**: Zkontrolujte tuto položku</span><span class="sxs-lookup"><span data-stu-id="6c55f-142">**HEAD**: Check this entry</span></span>

   * <span data-ttu-id="6c55f-143">**PRACOVNÍ**: nechte zaškrtnuté políčko tuto položku</span><span class="sxs-lookup"><span data-stu-id="6c55f-143">**WORKER**: Leave this entry unchecked</span></span>

   * <span data-ttu-id="6c55f-144">**ZOOKEEPER**: nechte zaškrtnuté políčko tuto položku</span><span class="sxs-lookup"><span data-stu-id="6c55f-144">**ZOOKEEPER**: Leave this entry unchecked</span></span>

   * <span data-ttu-id="6c55f-145">**Parametry**: to pole ponechat prázdné</span><span class="sxs-lookup"><span data-stu-id="6c55f-145">**PARAMETERS**: Leave this field blank</span></span>

3. <span data-ttu-id="6c55f-146">Na konci hello hello **akcí skriptů**, použijte hello **vyberte** tlačítko toosave hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6c55f-146">At hello bottom of hello **Script Actions**, use hello **Select** button toosave hello configuration.</span></span> <span data-ttu-id="6c55f-147">Nakonec použijte hello **vyberte** tlačítko dole hello hello **volitelné konfiguraci** okno toosave hello volitelné konfiguraci informace.</span><span class="sxs-lookup"><span data-stu-id="6c55f-147">Finally, use hello **Select** button at hello bottom of hello **Optional Configuration** blade toosave hello optional configuration information.</span></span>

4. <span data-ttu-id="6c55f-148">Pokračovat ve vytváření clusteru hello, jak je popsáno v [clustery HDInsight se systémem Linux vytvořit](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6c55f-148">Continue creating hello cluster as described in [Create Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

## <span data-ttu-id="6c55f-149"><a name="usegiraph"></a>Jak používat Giraph v prostředí HDInsight?</span><span class="sxs-lookup"><span data-stu-id="6c55f-149"><a name="usegiraph"></a>How do I use Giraph in HDInsight?</span></span>

<span data-ttu-id="6c55f-150">Po vytvoření clusteru hello, použijte následující postup toorun hello SimpleShortestPathsComputation ukázka součástí Giraph hello.</span><span class="sxs-lookup"><span data-stu-id="6c55f-150">Once hello cluster has been created, use hello following steps toorun hello SimpleShortestPathsComputation example included with Giraph.</span></span> <span data-ttu-id="6c55f-151">Tento příklad používá hello basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) implementace pro hledání hello nejkratší cesta mezi objekty v grafu.</span><span class="sxs-lookup"><span data-stu-id="6c55f-151">This example uses hello basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) implementation for finding hello shortest path between objects in a graph.</span></span>

1. <span data-ttu-id="6c55f-152">Připojte toohello clusteru HDInsight pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="6c55f-152">Connect toohello HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="6c55f-153">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="6c55f-153">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="6c55f-154">Použití hello následující příkaz toocreate soubor s názvem **tiny_graph.txt**:</span><span class="sxs-lookup"><span data-stu-id="6c55f-154">Use hello following command toocreate a file named **tiny_graph.txt**:</span></span>

    ```bash
    nano tiny_graph.txt
    ```

    <span data-ttu-id="6c55f-155">Použijte hello následující text jako hello obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="6c55f-155">Use hello following text as hello contents of this file:</span></span>

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    <span data-ttu-id="6c55f-156">Tato data popisuje vztah mezi objekty v řízené grafu, ve formátu hello `[source_id, source_value,[[dest_id], [edge_value],...]]`.</span><span class="sxs-lookup"><span data-stu-id="6c55f-156">This data describes a relationship between objects in a directed graph, by using hello format `[source_id, source_value,[[dest_id], [edge_value],...]]`.</span></span> <span data-ttu-id="6c55f-157">Každý řádek představuje vztah mezi `source_id` objektů a jeden nebo více `dest_id` objekty.</span><span class="sxs-lookup"><span data-stu-id="6c55f-157">Each line represents a relationship between a `source_id` object and one or more `dest_id` objects.</span></span> <span data-ttu-id="6c55f-158">Hello `edge_value` si lze představit jako hello sílu nebo vzdálenost hello připojení mezi `source_id` a `dest\_id`.</span><span class="sxs-lookup"><span data-stu-id="6c55f-158">hello `edge_value` can be thought of as hello strength or distance of hello connection between `source_id` and `dest\_id`.</span></span>

    <span data-ttu-id="6c55f-159">Vykreslovat limitu, a pomocí hodnoty hello (nebo váhy) jako hello vzdálenost mezi objekty, hello dat může vypadat například hello následující diagram:</span><span class="sxs-lookup"><span data-stu-id="6c55f-159">Drawn out, and using hello value (or weight) as hello distance between objects, hello data might look like hello following diagram:</span></span>

    ![tiny_graph.txt vykreslovat jako kroužky řádků různých vzdálenost mezi](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. <span data-ttu-id="6c55f-161">toosave hello soubor, použijte **Ctrl + X**, pak **Y**a v neposlední řadě **Enter** název souboru tooaccept hello.</span><span class="sxs-lookup"><span data-stu-id="6c55f-161">toosave hello file, use **Ctrl+X**, then **Y**, and finally **Enter** tooaccept hello file name.</span></span>

4. <span data-ttu-id="6c55f-162">Použijte následující toostore hello data do primárního úložiště pro váš cluster HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="6c55f-162">Use hello following toostore hello data into primary storage for your HDInsight cluster:</span></span>

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. <span data-ttu-id="6c55f-163">Spusťte hello SimpleShortestPathsComputation příklad použití hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6c55f-163">Run hello SimpleShortestPathsComputation example using hello following command:</span></span>

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    <span data-ttu-id="6c55f-164">Hello parametry tohoto příkazu jsou popsané v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="6c55f-164">hello parameters used with this command are described in hello following table:</span></span>

   | <span data-ttu-id="6c55f-165">Parametr</span><span class="sxs-lookup"><span data-stu-id="6c55f-165">Parameter</span></span> | <span data-ttu-id="6c55f-166">Výsledek</span><span class="sxs-lookup"><span data-stu-id="6c55f-166">What it does</span></span> |
   | --- | --- |
   | `jar` |<span data-ttu-id="6c55f-167">Hello soubor jar obsahující příklady hello.</span><span class="sxs-lookup"><span data-stu-id="6c55f-167">hello jar file containing hello examples.</span></span> |
   | `org.apache.giraph.GiraphRunner` |<span data-ttu-id="6c55f-168">Třída Hello používá toostart hello příklady.</span><span class="sxs-lookup"><span data-stu-id="6c55f-168">hello class used toostart hello examples.</span></span> |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |<span data-ttu-id="6c55f-169">Hello příklad, který se používá.</span><span class="sxs-lookup"><span data-stu-id="6c55f-169">hello example that is used.</span></span> <span data-ttu-id="6c55f-170">V tomto příkladu vypočítá hello nejkratší cestě mezi ID 1 a všechna ID v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="6c55f-170">In this example, it computes hello shortest path between ID 1 and all other IDs in hello graph.</span></span> |
   | `-ca mapred.job.tracker` |<span data-ttu-id="6c55f-171">Hello headnode pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="6c55f-171">hello headnode for hello cluster.</span></span> |
   | `-vif` |<span data-ttu-id="6c55f-172">Hello toouse vstupní formát pro vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="6c55f-172">hello input format toouse for hello input data.</span></span> |
   | `-vip` |<span data-ttu-id="6c55f-173">Hello vstupní datový soubor.</span><span class="sxs-lookup"><span data-stu-id="6c55f-173">hello input data file.</span></span> |
   | `-vof` |<span data-ttu-id="6c55f-174">výstupní formát Hello.</span><span class="sxs-lookup"><span data-stu-id="6c55f-174">hello output format.</span></span> <span data-ttu-id="6c55f-175">V tomto příkladu, ID a hodnotu jako prostý text.</span><span class="sxs-lookup"><span data-stu-id="6c55f-175">In this example, ID and value as plain text.</span></span> |
   | `-op` |<span data-ttu-id="6c55f-176">umístění výstupu Hello.</span><span class="sxs-lookup"><span data-stu-id="6c55f-176">hello output location.</span></span> |
   | `-w 2` |<span data-ttu-id="6c55f-177">Hello počet toouse pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="6c55f-177">hello number of workers toouse.</span></span> <span data-ttu-id="6c55f-178">V tomto příkladu 2.</span><span class="sxs-lookup"><span data-stu-id="6c55f-178">In this example, 2.</span></span> |

    <span data-ttu-id="6c55f-179">Další informace o těchto a dalších parametrů použít s Giraph ukázky najdete v tématu hello [rychlý start Giraph](http://giraph.apache.org/quick_start.html).</span><span class="sxs-lookup"><span data-stu-id="6c55f-179">For more information on these, and other parameters used with Giraph samples, see hello [Giraph quickstart](http://giraph.apache.org/quick_start.html).</span></span>

6. <span data-ttu-id="6c55f-180">Po dokončení úlohy hello hello výsledky ukládat do hello **/example/out/shotestpaths** adresáře.</span><span class="sxs-lookup"><span data-stu-id="6c55f-180">Once hello job has finished, hello results are stored in hello **/example/out/shotestpaths** directory.</span></span> <span data-ttu-id="6c55f-181">Hello názvy výstupního souboru začínat **část-m -** a končit číslo určující hello první, druhý, soubor atd.</span><span class="sxs-lookup"><span data-stu-id="6c55f-181">hello output file names begin with **part-m-** and end with a number indicating hello first, second, etc. file.</span></span> <span data-ttu-id="6c55f-182">Použijte následující příkaz tooview hello výstup hello:</span><span class="sxs-lookup"><span data-stu-id="6c55f-182">Use hello following command tooview hello output:</span></span>

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    <span data-ttu-id="6c55f-183">výstup Hello by měla vypadat podobně jako toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="6c55f-183">hello output should appear similar toohello following text:</span></span>

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    <span data-ttu-id="6c55f-184">Příklad SimpleShortestPathComputation Hello je naprogramováno toostart s objektem ID 1 a najít hello nejkratší cesta tooother objekty.</span><span class="sxs-lookup"><span data-stu-id="6c55f-184">hello SimpleShortestPathComputation example is hard coded toostart with object ID 1 and find hello shortest path tooother objects.</span></span> <span data-ttu-id="6c55f-185">výstup Hello je ve formátu hello `destination_id` a `distance`.</span><span class="sxs-lookup"><span data-stu-id="6c55f-185">hello output is in hello format of `destination_id` and `distance`.</span></span> <span data-ttu-id="6c55f-186">Hello `distance` je hello hodnotu (nebo váhy) hello okrajů kterou urazit mezi objektem ID 1 a ID hello cíl.</span><span class="sxs-lookup"><span data-stu-id="6c55f-186">hello `distance` is hello value (or weight) of hello edges traveled between object ID 1 and hello target ID.</span></span>

    <span data-ttu-id="6c55f-187">Vizualizace tato data, můžete ověřit výsledky hello cestě hello nejkratší cest mezi ID 1 a všechny ostatní objekty.</span><span class="sxs-lookup"><span data-stu-id="6c55f-187">Visualizing this data, you can verify hello results by traveling hello shortest paths between ID 1 and all other objects.</span></span> <span data-ttu-id="6c55f-188">Cesta co nejkratší Hello ID 1 až ID 4 je 5.</span><span class="sxs-lookup"><span data-stu-id="6c55f-188">hello shortest path between ID 1 and ID 4 is 5.</span></span> <span data-ttu-id="6c55f-189">Tato hodnota je hello celkový vzdálenost mezi <span style="color:orange">ID 1 a 3</span>a potom <span style="color:red">ID 3 a 4</span>.</span><span class="sxs-lookup"><span data-stu-id="6c55f-189">This value is hello total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.</span></span>

    ![Kreslení objektů jako kroužky s nejkratší cest mezi](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a><span data-ttu-id="6c55f-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c55f-191">Next steps</span></span>

* <span data-ttu-id="6c55f-192">[Nainstalovat a používat Hue v clusterech HDInsight](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6c55f-192">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span>

* <span data-ttu-id="6c55f-193">[Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6c55f-193">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span>
