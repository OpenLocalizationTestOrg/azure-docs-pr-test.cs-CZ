---
title: "Akce skriptu tooinstall aaaUse Solr na HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall Solr na systémem Linux HDInsight Hadoop clustery pomocí akcí skriptů."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cc93ed5c-a358-456a-91a4-f179185c0e98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: 4c179032b95ae187f1830d8927f8796372fa8ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="91a74-103">Na nainstalovat a používat Solr clusterů systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="91a74-103">Install and use Solr on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="91a74-104">Zjistěte, jak tooinstall Solr v Azure HDInsight pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="91a74-104">Learn how tooinstall Solr on Azure HDInsight by using Script Action.</span></span> <span data-ttu-id="91a74-105">Solr je platforma pro efektivní vyhledávání a poskytuje možnosti vyhledávání na úrovni podniku na data spravovaná přes Hadoop.</span><span class="sxs-lookup"><span data-stu-id="91a74-105">Solr is a powerful search platform and provides enterprise-level search capabilities on data managed by Hadoop.</span></span>

> [!IMPORTANT]
    > <span data-ttu-id="91a74-106">Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="91a74-106">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="91a74-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="91a74-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="91a74-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="91a74-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91a74-109">v tomto dokumentu Hello ukázkový skript nainstaluje Solr 4.9 s konkrétní konfigurací.</span><span class="sxs-lookup"><span data-stu-id="91a74-109">hello sample script used in this document installs Solr 4.9 with a specific configuration.</span></span> <span data-ttu-id="91a74-110">Pokud chcete tooconfigure hello Solr clusteru pomocí různých kolekcí, horizontálních oddílů, schémata, repliky, atd., je třeba upravit hello skriptu a Solr binární soubory.</span><span class="sxs-lookup"><span data-stu-id="91a74-110">If you want tooconfigure hello Solr cluster with different collections, shards, schemas, replicas, etc., you must modify hello script and Solr binaries.</span></span>

## <span data-ttu-id="91a74-111"><a name="whatis"></a>Co je Solr</span><span class="sxs-lookup"><span data-stu-id="91a74-111"><a name="whatis"></a>What is Solr</span></span>

<span data-ttu-id="91a74-112">[Apache Solr](http://lucene.apache.org/solr/features.html) je platforma enterprise vyhledávání, která umožňuje efektivní fulltextové vyhledávání na data.</span><span class="sxs-lookup"><span data-stu-id="91a74-112">[Apache Solr](http://lucene.apache.org/solr/features.html) is an enterprise search platform that enables powerful full-text search on data.</span></span> <span data-ttu-id="91a74-113">Při Hadoop umožňuje ukládání a správě obrovské množství dat, Apache Solr poskytuje možnosti vyhledávání hello tooquickly načtení hello data.</span><span class="sxs-lookup"><span data-stu-id="91a74-113">While Hadoop enables storing and managing vast amounts of data, Apache Solr provides hello search capabilities tooquickly retrieve hello data.</span></span>

> [!WARNING]
> <span data-ttu-id="91a74-114">Microsoft podporuje součásti, které jsou součástí clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="91a74-114">Components provided with hello HDInsight cluster are fully supported by Microsoft.</span></span>
>
> <span data-ttu-id="91a74-115">Vlastní komponenty, například Solr, přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello.</span><span class="sxs-lookup"><span data-stu-id="91a74-115">Custom components, such as Solr, receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="91a74-116">Podporu společnosti Microsoft nemusí být možné tooresolve problémy s vlastními komponentami.</span><span class="sxs-lookup"><span data-stu-id="91a74-116">Microsoft support may not be able tooresolve problems with custom components.</span></span> <span data-ttu-id="91a74-117">O pomoc může být nutné tooengage hello s otevřeným zdrojem komunit.</span><span class="sxs-lookup"><span data-stu-id="91a74-117">You may need tooengage hello open source communities for assistance.</span></span> <span data-ttu-id="91a74-118">Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="91a74-118">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>

## <a name="what-hello-script-does"></a><span data-ttu-id="91a74-119">Jaké skript hello</span><span class="sxs-lookup"><span data-stu-id="91a74-119">What hello script does</span></span>

<span data-ttu-id="91a74-120">Tento skript vytvoří hello následující clusteru HDInsight toohello změny:</span><span class="sxs-lookup"><span data-stu-id="91a74-120">This script makes hello following changes toohello HDInsight cluster:</span></span>

* <span data-ttu-id="91a74-121">Nainstaluje Solr 4.9 do`/usr/hdp/current/solr`</span><span class="sxs-lookup"><span data-stu-id="91a74-121">Installs Solr 4.9 into `/usr/hdp/current/solr`</span></span>
* <span data-ttu-id="91a74-122">Vytvoří uživatele, **solrusr**, což je použité toorun hello Solr služby</span><span class="sxs-lookup"><span data-stu-id="91a74-122">Creates a user, **solrusr**, which is used toorun hello Solr service</span></span>
* <span data-ttu-id="91a74-123">Nastaví **solruser** jako vlastník hello`/usr/hdp/current/solr`</span><span class="sxs-lookup"><span data-stu-id="91a74-123">Sets **solruser** as hello owner of `/usr/hdp/current/solr`</span></span>
* <span data-ttu-id="91a74-124">Přidá [Upstart](http://upstart.ubuntu.com/) konfigurace, který se automaticky spouští Solr.</span><span class="sxs-lookup"><span data-stu-id="91a74-124">Adds an [Upstart](http://upstart.ubuntu.com/) configuration that starts Solr automatically.</span></span>

## <span data-ttu-id="91a74-125"><a name="install"></a>Nainstalujte Solr pomocí akcí skriptů</span><span class="sxs-lookup"><span data-stu-id="91a74-125"><a name="install"></a>Install Solr using Script Actions</span></span>

<span data-ttu-id="91a74-126">Tooinstall skriptu ukázka Solr v clusteru HDInsight je k dispozici na hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="91a74-126">A sample script tooinstall Solr on an HDInsight cluster is available at hello following location:</span></span>

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

<span data-ttu-id="91a74-127">toocreate cluster, který má nainstalovaný Solr hello použijte kroky v hello [Tvorba clusterů HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="91a74-127">toocreate a cluster that has Solr installed, use hello steps in hello [Create HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md) document.</span></span> <span data-ttu-id="91a74-128">Během procesu vytváření hello použijte následující postup tooinstall Solr hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-128">During hello creation process, use hello following steps tooinstall Solr:</span></span>

1. <span data-ttu-id="91a74-129">Z hello __clusteru Souhrn__ okno, select__Advanced settings__, pak __skript akce__.</span><span class="sxs-lookup"><span data-stu-id="91a74-129">From hello __Cluster summary__ blade, select__Advanced settings__, then __Script actions__.</span></span> <span data-ttu-id="91a74-130">Použijte následující informace toopopulate hello formuláře hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-130">Use hello following information toopopulate hello form:</span></span>

   * <span data-ttu-id="91a74-131">**NÁZEV**: Zadejte popisný název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="91a74-131">**NAME**: Enter a friendly name for hello script action.</span></span>
   * <span data-ttu-id="91a74-132">**Identifikátor URI skriptu**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh</span><span class="sxs-lookup"><span data-stu-id="91a74-132">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh</span></span>
   * <span data-ttu-id="91a74-133">**HEAD**: zaškrtnete tuto možnost,</span><span class="sxs-lookup"><span data-stu-id="91a74-133">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="91a74-134">**PRACOVNÍ**: zaškrtnete tuto možnost,</span><span class="sxs-lookup"><span data-stu-id="91a74-134">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="91a74-135">**ZOOKEEPER**: Zkontrolujte tuto možnost tooinstall na uzlu Zookeeper hello</span><span class="sxs-lookup"><span data-stu-id="91a74-135">**ZOOKEEPER**: Check this option tooinstall on hello Zookeeper node</span></span>
   * <span data-ttu-id="91a74-136">**Parametry**: to pole ponechat prázdné</span><span class="sxs-lookup"><span data-stu-id="91a74-136">**PARAMETERS**: Leave this field blank</span></span>

2. <span data-ttu-id="91a74-137">Na konci hello hello **skript akce** okno, použijte hello **vyberte** tlačítko toosave hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="91a74-137">At hello bottom of hello **Script actions** blade, use hello **Select** button toosave hello configuration.</span></span> <span data-ttu-id="91a74-138">Nakonec použijte hello **Další** tlačítko tooreturn toohello __souhrn clusteru__</span><span class="sxs-lookup"><span data-stu-id="91a74-138">Finally, use hello **Next** button tooreturn toohello __Cluster summary__</span></span>

3. <span data-ttu-id="91a74-139">Z hello __clusteru Souhrn__ vyberte __vytvořit__ toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="91a74-139">From hello __Cluster summary__ page, select __Create__ toocreate hello cluster.</span></span>

## <span data-ttu-id="91a74-140"><a name="usesolr"></a>Jak používat Solr v prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="91a74-140"><a name="usesolr"></a>How do I use Solr in HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91a74-141">Hello kroky v této části ukazují základní funkce Solr.</span><span class="sxs-lookup"><span data-stu-id="91a74-141">hello steps in this section demonstrate basic Solr functionality.</span></span> <span data-ttu-id="91a74-142">Další informace o použití Solr, najdete v části hello [Apache Solr lokality](http://lucene.apache.org/solr/).</span><span class="sxs-lookup"><span data-stu-id="91a74-142">For more information on using Solr, see hello [Apache Solr site](http://lucene.apache.org/solr/).</span></span>

### <a name="index-data"></a><span data-ttu-id="91a74-143">Data indexu</span><span class="sxs-lookup"><span data-stu-id="91a74-143">Index data</span></span>

<span data-ttu-id="91a74-144">Použijte následující kroky tooadd příklad dat tooSolr hello a pak zadat dotaz:</span><span class="sxs-lookup"><span data-stu-id="91a74-144">Use hello following steps tooadd example data tooSolr, and then query it:</span></span>

1. <span data-ttu-id="91a74-145">Připojte toohello clusteru HDInsight pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="91a74-145">Connect toohello HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="91a74-146">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="91a74-146">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="91a74-147">Kroky později v tomto dokumentu používají SSL tunel tooconnect toohello Solr webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="91a74-147">Steps later in this document use an SSL tunnel tooconnect toohello Solr web UI.</span></span> <span data-ttu-id="91a74-148">toouse tyto kroky, je potřeba vytvořit protokolem SSL tunelování a pak proveďte konfiguraci vašeho prohlížeče toouse ho.</span><span class="sxs-lookup"><span data-stu-id="91a74-148">toouse these steps, you must establish an SSL tunnel and then configure your browser toouse it.</span></span>
     >
     > <span data-ttu-id="91a74-149">Další informace najdete v tématu hello [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="91a74-149">For more information, see hello [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

2. <span data-ttu-id="91a74-150">Použijte následující příkazy toohave Solr index ukázková data hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-150">Use hello following commands toohave Solr index sample data:</span></span>

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    <span data-ttu-id="91a74-151">Hello následující výstup se vrátí toohello konzoly:</span><span class="sxs-lookup"><span data-stu-id="91a74-151">hello following output is returned toohello console:</span></span>

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    <span data-ttu-id="91a74-152">Hello `post.jar` nástroj přidá hello **solr.xml** a **monitor.xml** dokumenty toohello index.</span><span class="sxs-lookup"><span data-stu-id="91a74-152">hello `post.jar` utility adds hello **solr.xml** and **monitor.xml** documents toohello index.</span></span>
  
3. <span data-ttu-id="91a74-153">Použijte následující příkaz tooquery hello Solr REST API hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-153">Use hello following command tooquery hello Solr REST API:</span></span>

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    <span data-ttu-id="91a74-154">Tento příkaz vyhledá **collection1** pro všechny dokumenty odpovídající  **\*:\***  (kódovaná jako \*% 3A\* v řetězci dotazu hello).</span><span class="sxs-lookup"><span data-stu-id="91a74-154">This command searches **collection1** for any documents matching **\*:\*** (encoded as \*%3A\* in hello query string).</span></span> <span data-ttu-id="91a74-155">Hello následující dokumentu JSON je příklad hello odpovědi:</span><span class="sxs-lookup"><span data-stu-id="91a74-155">hello following JSON document is an example of hello response:</span></span>

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

### <a name="using-hello-solr-dashboard"></a><span data-ttu-id="91a74-156">Pomocí řídicího panelu Solr hello</span><span class="sxs-lookup"><span data-stu-id="91a74-156">Using hello Solr dashboard</span></span>

<span data-ttu-id="91a74-157">řídicí panel Solr Hello je webové uživatelské rozhraní, které vám umožní toowork s Solr prostřednictvím webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="91a74-157">hello Solr dashboard is a web UI that allows you toowork with Solr through your web browser.</span></span> <span data-ttu-id="91a74-158">řídicí panel Solr Hello není zveřejněné přímo na hello Internetu z clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91a74-158">hello Solr dashboard is not exposed directly on hello Internet from your HDInsight cluster.</span></span> <span data-ttu-id="91a74-159">Můžete použít tooaccess tunelového propojení SSH ho.</span><span class="sxs-lookup"><span data-stu-id="91a74-159">You can use an SSH tunnel tooaccess it.</span></span> <span data-ttu-id="91a74-160">Další informace o používání tunelového propojení SSH naleznete v části hello [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="91a74-160">For more information on using an SSH tunnel, see hello [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

<span data-ttu-id="91a74-161">Po vytvoření tunelu SSH použijte následující postup toouse hello Solr řídicí panel hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-161">Once you have established an SSH tunnel, use hello following steps toouse hello Solr dashboard:</span></span>

1. <span data-ttu-id="91a74-162">Určete hello název hostitele pro primární headnode hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-162">Determine hello host name for hello primary headnode:</span></span>

   1. <span data-ttu-id="91a74-163">Použití SSH tooconnect toohello hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="91a74-163">Use SSH tooconnect toohello cluster head node.</span></span> <span data-ttu-id="91a74-164">Například, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="91a74-164">For example, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span></span>

       <span data-ttu-id="91a74-165">Další informace o používání SSH najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="91a74-165">For more information on using SSH, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

   2. <span data-ttu-id="91a74-166">Použijte následující příkaz tooget hello plně kvalifikovaný název hostitele hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-166">Use hello following command tooget hello fully qualified hostname:</span></span>

        ```bash
        hostname -f
        ```

        <span data-ttu-id="91a74-167">Tento příkaz vrátí hodnotu podobné toohello následující název hostitele:</span><span class="sxs-lookup"><span data-stu-id="91a74-167">This command returns a value similar toohello following host name:</span></span>

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        <span data-ttu-id="91a74-168">Hello hodnota vrácená, uložte, protože se později používá.</span><span class="sxs-lookup"><span data-stu-id="91a74-168">Save hello value returned, as it is used later.</span></span>

2. <span data-ttu-id="91a74-169">V prohlížeči připojit příliš**http://HOSTNAME:8983/solr / #/**, kde **HOSTNAME** je název hello určit v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="91a74-169">In your browser, connect too**http://HOSTNAME:8983/solr/#/**, where **HOSTNAME** is hello name you determined in hello previous steps.</span></span>

    <span data-ttu-id="91a74-170">žádost o Hello směrován přes hello SSH tunel toohello Solr webového uživatelského rozhraní v clusteru.</span><span class="sxs-lookup"><span data-stu-id="91a74-170">hello request is routed through hello SSH tunnel toohello Solr web UI on your cluster.</span></span> <span data-ttu-id="91a74-171">Podobně jako toohello následující bitové kopie se zobrazí stránka Hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-171">hello page appears similar toohello following image:</span></span>

    ![Obrázek Solr řídicí panel](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. <span data-ttu-id="91a74-173">Použijte v levém podokně hello hello **základní selektor** rozevíracího seznamu tooselect **collection1**.</span><span class="sxs-lookup"><span data-stu-id="91a74-173">From hello left pane, use hello **Core Selector** drop-down tooselect **collection1**.</span></span> <span data-ttu-id="91a74-174">Několik položek by je se níže zobrazí **collection1**.</span><span class="sxs-lookup"><span data-stu-id="91a74-174">Several entries should them appear below **collection1**.</span></span>

4. <span data-ttu-id="91a74-175">Z níže uvedené položky hello **collection1**, vyberte **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="91a74-175">From hello entries below **collection1**, select **Query**.</span></span> <span data-ttu-id="91a74-176">Použijte následující hodnoty toopopulate hello vyhledávání stránku hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-176">Use hello following values toopopulate hello search page:</span></span>

   * <span data-ttu-id="91a74-177">V hello **q** textové pole, zadejte  **\*:**\*.</span><span class="sxs-lookup"><span data-stu-id="91a74-177">In hello **q** text box, enter **\*:**\*.</span></span> <span data-ttu-id="91a74-178">Tento dotaz vrací všechny hello dokumenty, které jsou uloženy v Solr.</span><span class="sxs-lookup"><span data-stu-id="91a74-178">This query returns all hello documents that are indexed in Solr.</span></span> <span data-ttu-id="91a74-179">Pokud chcete toosearch konkrétní řetězec v hello dokumenty, můžete zadat sem tento řetězec.</span><span class="sxs-lookup"><span data-stu-id="91a74-179">If you want toosearch for a specific string within hello documents, you can enter that string here.</span></span>
   * <span data-ttu-id="91a74-180">V hello **wt** textového pole, vyberte hello výstupní formát.</span><span class="sxs-lookup"><span data-stu-id="91a74-180">In hello **wt** text box, select hello output format.</span></span> <span data-ttu-id="91a74-181">Výchozí hodnota je **json**.</span><span class="sxs-lookup"><span data-stu-id="91a74-181">Default is **json**.</span></span>

     <span data-ttu-id="91a74-182">Nakonec vyberte hello **spuštění dotazu** tlačítko dole hello pate vyhledávání hello.</span><span class="sxs-lookup"><span data-stu-id="91a74-182">Finally, select hello **Execute Query** button at hello bottom of hello search pate.</span></span>

     ![Pomocí akce skriptu toocustomize clusteru](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     <span data-ttu-id="91a74-184">výstup Hello vrátí hello dva dokumenty, zda jste přidali toohello indexu dříve.</span><span class="sxs-lookup"><span data-stu-id="91a74-184">hello output returns hello two documents that you added toohello index earlier.</span></span> <span data-ttu-id="91a74-185">Hello výstup je podobné toohello následující dokumentu JSON:</span><span class="sxs-lookup"><span data-stu-id="91a74-185">hello output is similar toohello following JSON document:</span></span>

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

### <a name="starting-and-stopping-solr"></a><span data-ttu-id="91a74-186">Spuštění a zastavení Solr</span><span class="sxs-lookup"><span data-stu-id="91a74-186">Starting and stopping Solr</span></span>

<span data-ttu-id="91a74-187">Použijte následující příkazy toomanually zastavit a spustit Solr hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-187">Use hello following commands toomanually stop and start Solr:</span></span>

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a><span data-ttu-id="91a74-188">Zálohování indexovaná data</span><span class="sxs-lookup"><span data-stu-id="91a74-188">Backup indexed data</span></span>

<span data-ttu-id="91a74-189">Použijte následující postup tooback Solr data toohello výchozí úložiště pro váš cluster hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-189">Use hello following steps tooback up Solr data toohello default storage for your cluster:</span></span>

1. <span data-ttu-id="91a74-190">Připojte toohello clusteru pomocí protokolu SSH, potom použijte následující příkaz tooget hello název hostitele hlavního uzlu hello hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-190">Connect toohello cluster using SSH, then use hello following command tooget hello host name for hello head node:</span></span>

    ```bash
    hostname -f
    ```

2. <span data-ttu-id="91a74-191">Použijte následující příkaz toocreate snímek dat hello indexované hello.</span><span class="sxs-lookup"><span data-stu-id="91a74-191">Use hello following command toocreate a snapshot of hello indexed data.</span></span> <span data-ttu-id="91a74-192">Nahraďte **HOSTNAME** s názvem hello vrácená z předchozí příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="91a74-192">Replace **HOSTNAME** with hello name returned from hello previous command:</span></span>

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    <span data-ttu-id="91a74-193">odpověď Hello je podobné toohello následující XML:</span><span class="sxs-lookup"><span data-stu-id="91a74-193">hello response is similar toohello following XML:</span></span>

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. <span data-ttu-id="91a74-194">Změňte adresáře příliš`/usr/hdp/current/solr/example/solr`.</span><span class="sxs-lookup"><span data-stu-id="91a74-194">Change directories too`/usr/hdp/current/solr/example/solr`.</span></span> <span data-ttu-id="91a74-195">Není podadresáři sem pro každou kolekci.</span><span class="sxs-lookup"><span data-stu-id="91a74-195">There is a subdirectory here for each collection.</span></span> <span data-ttu-id="91a74-196">Každý adresář kolekce obsahuje `data` adresář, který obsahuje snímek hello hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="91a74-196">Each collection directory contains a `data` directory that contains hello snapshot for hello collection.</span></span>

4. <span data-ttu-id="91a74-197">toocreate komprimovaný archiv hello snímku složky, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="91a74-197">toocreate a compressed archive of hello snapshot folder, use hello following command:</span></span>

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    <span data-ttu-id="91a74-198">Nahraďte hello `snapshot.20150806185338855` hodnoty s názvem hello hello snímku pro vaše kolekce.</span><span class="sxs-lookup"><span data-stu-id="91a74-198">Replace hello `snapshot.20150806185338855` values with hello name of hello snapshot for your collection.</span></span>

    <span data-ttu-id="91a74-199">Tento příkaz vytvoří archiv s názvem **snapshot.20150806185338855.tgz**, který obsahuje obsah hello hello **snapshot.20150806185338855** adresáře.</span><span class="sxs-lookup"><span data-stu-id="91a74-199">This command creates an archive named **snapshot.20150806185338855.tgz**, which contains hello contents of hello **snapshot.20150806185338855** directory.</span></span>

5. <span data-ttu-id="91a74-200">Pak můžete uložit hello archivu toohello clusteru primárního úložiště pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="91a74-200">You can then store hello archive toohello cluster's primary storage using hello following command:</span></span>

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

<span data-ttu-id="91a74-201">Další informace o práci s Solr zálohování a obnovení najdete v tématu [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).</span><span class="sxs-lookup"><span data-stu-id="91a74-201">For more information on working with Solr backup and restores, see [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).</span></span>

## <a name="next-steps"></a><span data-ttu-id="91a74-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="91a74-202">Next steps</span></span>

* <span data-ttu-id="91a74-203">[Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="91a74-203">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="91a74-204">Použijte vlastní nastavení tooinstall clusteru, které Giraph na HDInsight Hadoop clusterů.</span><span class="sxs-lookup"><span data-stu-id="91a74-204">Use cluster customization tooinstall Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="91a74-205">Giraph můžete graf tooperform zpracování pomocí Hadoop a lze použít s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91a74-205">Giraph allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="91a74-206">[Instalace aplikace Hue v clusterech HDInsight](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="91a74-206">[Install Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="91a74-207">Použití clusteru přizpůsobení tooinstall Hue v clusterech HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="91a74-207">Use cluster customization tooinstall Hue on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="91a74-208">HUE je že sada webových aplikací používá toointeract s clusterem Hadoop.</span><span class="sxs-lookup"><span data-stu-id="91a74-208">Hue is a set of Web applications used toointeract with a Hadoop cluster.</span></span>

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
