---
title: aaaUse akce skriptu tooinstall Solr na clusteru Hadoop - Azure | Microsoft Docs
description: "Zjistěte, jak toocustomize HDInsight, cluster s Solr pomocí akce skriptu."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b1e6f338-8ac1-4b38-bbb5-2f7388b9de3b
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 022ba56b7499390a91bfe869e5069893e56b6503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="b8a08-103">Nainstalovat a používat Solr v clusterech HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="b8a08-103">Install and use Solr on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="b8a08-104">Zjistěte, jak toocustomize HDInsight se systémem Windows cluster s Solr pomocí akce skriptu a jak toouse Solr toosearch data.</span><span class="sxs-lookup"><span data-stu-id="b8a08-104">Learn how toocustomize Windows-based HDInsight cluster with Solr using Script Action, and how toouse Solr toosearch data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b8a08-105">Hello kroky v této dokumentu funguje jenom s clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="b8a08-105">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="b8a08-106">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="b8a08-106">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="b8a08-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b8a08-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b8a08-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b8a08-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="b8a08-109">Informace o používání Solr s clusteru se systémem Linux najdete v tématu [instalací a použitím Solr na clusterů systému HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="b8a08-109">For information on using Solr with a Linux-based cluster, see [Install and use Solr on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-solr-install-linux.md).</span></span>


<span data-ttu-id="b8a08-110">Solr můžete nainstalovat na libovolný typ clusteru (Hadoop, Spark, HBase, Storm) v Azure HDInsight pomocí *akce skriptu*.</span><span class="sxs-lookup"><span data-stu-id="b8a08-110">You can install Solr on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="b8a08-111">Tooinstall skriptu ukázka Solr v clusteru HDInsight je k dispozici z objektu blob úložiště Azure jen pro čtení v [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="b8a08-111">A sample script tooinstall Solr on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

<span data-ttu-id="b8a08-112">Ukázkový skript Hello pracuje pouze s verze clusteru HDInsight verze 3.1.</span><span class="sxs-lookup"><span data-stu-id="b8a08-112">hello sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="b8a08-113">Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="b8a08-113">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="b8a08-114">Ukázkový skript Hello použitým v tomto tématu vytvoří cluster se systémem Windows Solr s konkrétní konfigurací.</span><span class="sxs-lookup"><span data-stu-id="b8a08-114">hello sample script used in this topic creates a Windows-based Solr cluster with a specific configuration.</span></span> <span data-ttu-id="b8a08-115">Pokud chcete tooconfigure hello Solr clusteru pomocí různých kolekcí, horizontálních oddílů, schémata, repliky, atd., musíte upravit hello skript a binární soubory Solr odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="b8a08-115">If you want tooconfigure hello Solr cluster with different collections, shards, schemas, replicas, etc., you must modify hello script and Solr binaries accordingly.</span></span>

<span data-ttu-id="b8a08-116">**Související články**</span><span class="sxs-lookup"><span data-stu-id="b8a08-116">**Related articles**</span></span>

* [<span data-ttu-id="b8a08-117">Na nainstalovat a používat Solr clusterů systému HDinsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="b8a08-117">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="b8a08-118">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b8a08-118">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="b8a08-119">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="b8a08-119">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="b8a08-120">[Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="b8a08-120">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-solr"></a><span data-ttu-id="b8a08-121">Co je Solr?</span><span class="sxs-lookup"><span data-stu-id="b8a08-121">What is Solr?</span></span>
<span data-ttu-id="b8a08-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> je platforma enterprise vyhledávání, která umožňuje efektivní fulltextové vyhledávání na data.</span><span class="sxs-lookup"><span data-stu-id="b8a08-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> is an enterprise search platform that enables powerful full-text search on data.</span></span> <span data-ttu-id="b8a08-123">Při Hadoop umožňuje ukládání a správě obrovské množství dat, Apache Solr poskytuje možnosti vyhledávání hello tooquickly načtení hello data.</span><span class="sxs-lookup"><span data-stu-id="b8a08-123">While Hadoop enables storing and managing vast amounts of data, Apache Solr provides hello search capabilities tooquickly retrieve hello data.</span></span>

## <a name="install-solr-using-portal"></a><span data-ttu-id="b8a08-124">Nainstalujte Solr pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="b8a08-124">Install Solr using portal</span></span>
1. <span data-ttu-id="b8a08-125">Zahájení vytváření clusteru pomocí hello **vytvořit vlastní** možnost, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="b8a08-125">Start creating a cluster by using hello **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="b8a08-126">Na hello **akcí skriptů** stránku hello průvodce, klikněte na tlačítko **přidat akce skriptu** tooprovide podrobností o hello akce skriptu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="b8a08-126">On hello **Script Actions** page of hello wizard, click **add script action** tooprovide details about hello script action, as shown below:</span></span>

    <span data-ttu-id="b8a08-127">![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "použití akce skriptu toocustomize clusteru")</span><span class="sxs-lookup"><span data-stu-id="b8a08-127">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="b8a08-128">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b8a08-128">Property</span></span></th><th><span data-ttu-id="b8a08-129">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b8a08-129">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="b8a08-130">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b8a08-130">Name</span></span></td>
            <td><span data-ttu-id="b8a08-131">Zadejte název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="b8a08-131">Specify a name for hello script action.</span></span> <span data-ttu-id="b8a08-132">Například <b>nainstalovat Solr</b>.</span><span class="sxs-lookup"><span data-stu-id="b8a08-132">For example, <b>Install Solr</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="b8a08-133">Identifikátor URI skriptu</span><span class="sxs-lookup"><span data-stu-id="b8a08-133">Script URI</span></span></td>
            <td><span data-ttu-id="b8a08-134">Zadejte hello identifikátor URI (Uniform Resource) toohello skript, který je vyvolaná toocustomize hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="b8a08-134">Specify hello Uniform Resource Identifier (URI) toohello script that is invoked toocustomize hello cluster.</span></span> <span data-ttu-id="b8a08-135">Například <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="b8a08-135">For example, <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="b8a08-136">Typ uzlu</span><span class="sxs-lookup"><span data-stu-id="b8a08-136">Node Type</span></span></td>
            <td><span data-ttu-id="b8a08-137">Zadejte hello uzly, na kterých běží hello přizpůsobení skriptu.</span><span class="sxs-lookup"><span data-stu-id="b8a08-137">Specify hello nodes on which hello customization script is run.</span></span> <span data-ttu-id="b8a08-138">Můžete zvolit <b>všechny uzly</b>, <b>hlavní pouze uzly</b>, nebo <b>pouze uzly pracovního procesu</b>.</span><span class="sxs-lookup"><span data-stu-id="b8a08-138">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="b8a08-139">Parametry</span><span class="sxs-lookup"><span data-stu-id="b8a08-139">Parameters</span></span></td>
            <td><span data-ttu-id="b8a08-140">Zadejte parametry hello, pokud to vyžaduje hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="b8a08-140">Specify hello parameters, if required by hello script.</span></span> <span data-ttu-id="b8a08-141">Hello skriptu tooinstall Solr nevyžaduje žádné parametry, takže to můžete nechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="b8a08-141">hello script tooinstall Solr does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="b8a08-142">Více součástí, které můžete přidat více než jeden tooinstall akce skriptu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b8a08-142">You can add more than one script action tooinstall multiple components on hello cluster.</span></span> <span data-ttu-id="b8a08-143">Po přidání hello skripty, klikněte na tlačítko toostart zaškrtnutí hello vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b8a08-143">After you have added hello scripts, click hello checkmark toostart creating hello cluster.</span></span>

## <a name="use-solr"></a><span data-ttu-id="b8a08-144">Použití Solr</span><span class="sxs-lookup"><span data-stu-id="b8a08-144">Use Solr</span></span>
<span data-ttu-id="b8a08-145">Musí začínat indexování Solr s některých datových souborů.</span><span class="sxs-lookup"><span data-stu-id="b8a08-145">You must start with indexing Solr with some data files.</span></span> <span data-ttu-id="b8a08-146">Potom můžete Solr toorun vyhledávací dotazy na data hello indexované.</span><span class="sxs-lookup"><span data-stu-id="b8a08-146">You can then use Solr toorun search queries on hello indexed data.</span></span> <span data-ttu-id="b8a08-147">Proveďte následující kroky toouse Solr v clusteru služby HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="b8a08-147">Perform hello following steps toouse Solr in an HDInsight cluster:</span></span>

1. <span data-ttu-id="b8a08-148">**Použití tooremote protokol RDP (Remote Desktop) do clusteru HDInsight hello s Solr nainstalován**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-148">**Use Remote Desktop Protocol (RDP) tooremote into hello HDInsight cluster with Solr installed**.</span></span> <span data-ttu-id="b8a08-149">Z hello portálu Azure povolení vzdálené plochy pro hello clusteru, který jste vytvořili pomocí Solr hello nainstalovaná a pak vzdáleného do clusteru.</span><span class="sxs-lookup"><span data-stu-id="b8a08-149">From hello Azure portal, enable Remote Desktop for hello cluster you created with Solr installed, and then remote into hello cluster.</span></span> <span data-ttu-id="b8a08-150">Pokyny najdete v tématu [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="b8a08-150">For instructions, see [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="b8a08-151">**Index Solr tím, že nahrajete datové soubory**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-151">**Index Solr by uploading data files**.</span></span> <span data-ttu-id="b8a08-152">Pokud jste indexu Solr, vložíte dokumenty v něm, které můžete potřebovat toosearch na.</span><span class="sxs-lookup"><span data-stu-id="b8a08-152">When you index Solr, you put documents in it that you may need toosearch on.</span></span> <span data-ttu-id="b8a08-153">tooindex Solr, pomocí protokolu RDP tooremote do clusteru hello přejděte toohello plochy, otevřete hello Hadoop příkazového řádku a přejděte příliš**C:\apps\dist\solr-4.7.2\example\exampledocs**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-153">tooindex Solr, use RDP tooremote into hello cluster, navigate toohello desktop, open hello Hadoop command line, and navigate too**C:\apps\dist\solr-4.7.2\example\exampledocs**.</span></span> <span data-ttu-id="b8a08-154">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b8a08-154">Run hello following command:</span></span>

        java -jar post.jar solr.xml monitor.xml

    <span data-ttu-id="b8a08-155">Zobrazí se následující výstup na konzolu hello hello:</span><span class="sxs-lookup"><span data-stu-id="b8a08-155">You'll see hello following output on hello console:</span></span>

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    <span data-ttu-id="b8a08-156">Nástroj post.jar Hello indexuje Solr s dva ukázkové dokumenty **solr.xml** a **monitor.xml**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-156">hello post.jar utility indexes Solr with two sample documents, **solr.xml** and **monitor.xml**.</span></span> <span data-ttu-id="b8a08-157">Nástroj post.jar Hello a hello ukázkové dokumenty jsou dostupné s Solr instalace.</span><span class="sxs-lookup"><span data-stu-id="b8a08-157">hello post.jar utility and hello sample documents are available with Solr installation.</span></span>
3. <span data-ttu-id="b8a08-158">**Použití hello Solr řídicí panel toosearch v rámci hello indexované dokumenty**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-158">**Use hello Solr dashboard toosearch within hello indexed documents**.</span></span> <span data-ttu-id="b8a08-159">V hello RDP relace toohello HDInsight clusteru, otevřete Internet Explorer a spusťte hello Solr řídicí panel v **http://headnodehost:8983/solr / #/**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-159">In hello RDP session toohello HDInsight cluster, open Internet Explorer, and launch hello Solr dashboard at **http://headnodehost:8983/solr/#/**.</span></span> <span data-ttu-id="b8a08-160">Z levého podokna hello z hello **základní selektor** rozevíracího seznamu, vyberte **collection1**a v rámci, klikněte na tlačítko **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-160">From hello left pane, from hello **Core Selector** drop-down, select **collection1**, and within that, click **Query**.</span></span> <span data-ttu-id="b8a08-161">Jako příklad, tooselect a vrátí všechny dokumenty hello v Solr, zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b8a08-161">As an example, tooselect and return all hello docs in Solr, provide hello following values:</span></span>

   * <span data-ttu-id="b8a08-162">V hello **q** textové pole, zadejte  **\*:**\*.</span><span class="sxs-lookup"><span data-stu-id="b8a08-162">In hello **q** text box, enter **\*:**\*.</span></span> <span data-ttu-id="b8a08-163">Tato možnost vrátí všechny hello dokumenty, které jsou uloženy ve Solr.</span><span class="sxs-lookup"><span data-stu-id="b8a08-163">This will return all hello documents that are indexed in Solr.</span></span> <span data-ttu-id="b8a08-164">Pokud chcete toosearch konkrétní řetězec v hello dokumenty, můžete zadat sem tento řetězec.</span><span class="sxs-lookup"><span data-stu-id="b8a08-164">If you want toosearch for a specific string within hello documents, you can enter that string here.</span></span>
   * <span data-ttu-id="b8a08-165">V hello **wt** textového pole, vyberte hello výstupní formát.</span><span class="sxs-lookup"><span data-stu-id="b8a08-165">In hello **wt** text box, select hello output format.</span></span> <span data-ttu-id="b8a08-166">Výchozí hodnota je **json**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-166">Default is **json**.</span></span> <span data-ttu-id="b8a08-167">Klikněte na tlačítko **spuštění dotazu**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-167">Click **Execute Query**.</span></span>

     <span data-ttu-id="b8a08-168">![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "spuštění dotazu na řídicím panelu Solr")</span><span class="sxs-lookup"><span data-stu-id="b8a08-168">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Run a query on Solr dashboard")</span></span>

     <span data-ttu-id="b8a08-169">výstup Hello vrátí hello dva dokumenty, které jsme použili pro indexování Solr.</span><span class="sxs-lookup"><span data-stu-id="b8a08-169">hello output returns hello two docs that we used for indexing Solr.</span></span> <span data-ttu-id="b8a08-170">výstup Hello se podobá následující hello:</span><span class="sxs-lookup"><span data-stu-id="b8a08-170">hello output resembles hello following:</span></span>

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, hello Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication tooother Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over hello e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }
4. <span data-ttu-id="b8a08-171">**Doporučená: Zálohování hello indexované data z Solr tooAzure úložiště objektů Blob, které jsou spojené s clusterem HDInsight hello**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-171">**Recommended: Back up hello indexed data from Solr tooAzure Blob storage associated with hello HDInsight cluster**.</span></span> <span data-ttu-id="b8a08-172">Jako osvědčených postupů měli zálohovat data hello indexované z uzlů clusteru Solr hello do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="b8a08-172">As a good practice, you should back up hello indexed data from hello Solr cluster nodes onto Azure Blob storage.</span></span> <span data-ttu-id="b8a08-173">Proveďte následující kroky toodo tak hello:</span><span class="sxs-lookup"><span data-stu-id="b8a08-173">Perform hello following steps toodo so:</span></span>

   1. <span data-ttu-id="b8a08-174">Z relace RDP hello otevřete Internet Explorer a bodu toohello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="b8a08-174">From hello RDP session, open Internet Explorer, and point toohello following URL:</span></span>

           http://localhost:8983/solr/replication?command=backup

       <span data-ttu-id="b8a08-175">Zobrazí takováto odpověď:</span><span class="sxs-lookup"><span data-stu-id="b8a08-175">You should see a response like this:</span></span>

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. <span data-ttu-id="b8a08-176">V relaci vzdálené hello přejděte příliš {SOLR_HOME}\{kolekce} \data.</span><span class="sxs-lookup"><span data-stu-id="b8a08-176">In hello remote session, navigate too{SOLR_HOME}\{Collection}\data.</span></span> <span data-ttu-id="b8a08-177">Pro cluster hello vytvořené prostřednictvím hello ukázkový skript, měl by být **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span><span class="sxs-lookup"><span data-stu-id="b8a08-177">For hello cluster created via hello sample script, this should be **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span></span> <span data-ttu-id="b8a08-178">V tomto umístění, měli byste vidět do složky snímku vytvořen s názvem podobné příliš**snímku.* časové razítko***.</span><span class="sxs-lookup"><span data-stu-id="b8a08-178">At this location, you should see a snapshot folder created with a name similar too**snapshot.*timestamp***.</span></span>
   3. <span data-ttu-id="b8a08-179">Složka snímku hello ZIP a nahrajte ho tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="b8a08-179">Zip hello snapshot folder and upload it tooAzure Blob storage.</span></span> <span data-ttu-id="b8a08-180">Z příkazového řádku hello Hadoop přejděte toohello umístění složky snímků hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b8a08-180">From hello Hadoop command line, navigate toohello location of hello snapshot folder by using hello following command:</span></span>

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       <span data-ttu-id="b8a08-181">Tento příkaz kopie hello příliš/příklad nebo data snímku/v kontejneru hello v rámci hello výchozí úložiště účtu přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="b8a08-181">This command copies hello snapshot too/example/data/ under hello container within hello default Storage account associated with hello cluster.</span></span>

## <a name="install-solr-using-aure-powershell"></a><span data-ttu-id="b8a08-182">Nainstalujte Solr pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8a08-182">Install Solr using Aure PowerShell</span></span>
<span data-ttu-id="b8a08-183">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="b8a08-183">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="b8a08-184">Ukázka Hello ukazuje, jak tooinstall Spark pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b8a08-184">hello sample demonstrates how tooinstall Spark using Azure PowerShell.</span></span> <span data-ttu-id="b8a08-185">Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="b8a08-185">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="install-solr-using-net-sdk"></a><span data-ttu-id="b8a08-186">Nainstalujte Solr pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b8a08-186">Install Solr using .NET SDK</span></span>
<span data-ttu-id="b8a08-187">V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="b8a08-187">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="b8a08-188">Ukázka Hello ukazuje, jak tooinstall Spark pomocí hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="b8a08-188">hello sample demonstrates how tooinstall Spark using hello .NET SDK.</span></span> <span data-ttu-id="b8a08-189">Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="b8a08-189">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="b8a08-190">Viz také</span><span class="sxs-lookup"><span data-stu-id="b8a08-190">See also</span></span>
* [<span data-ttu-id="b8a08-191">Na nainstalovat a používat Solr clusterů systému HDinsight Hadoop (Linux)</span><span class="sxs-lookup"><span data-stu-id="b8a08-191">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="b8a08-192">[Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b8a08-192">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="b8a08-193">[Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="b8a08-193">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="b8a08-194">[Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="b8a08-194">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="b8a08-195">[Nainstalovat a používat Spark v clusterech HDInsight][hdinsight-install-spark]: akce skriptu vzorku o instalaci Spark.</span><span class="sxs-lookup"><span data-stu-id="b8a08-195">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="b8a08-196">[Nainstalujte jazyk R v clusterech HDInsight][hdinsight-install-r]: akce skriptu vzorku o instalaci R.</span><span class="sxs-lookup"><span data-stu-id="b8a08-196">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="b8a08-197">[Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install.md): akce skriptu vzorku o instalaci Giraph.</span><span class="sxs-lookup"><span data-stu-id="b8a08-197">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
