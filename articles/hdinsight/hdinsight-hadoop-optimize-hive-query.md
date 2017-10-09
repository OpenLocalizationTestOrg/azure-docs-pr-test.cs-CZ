---
title: aaaOptimize Hive dotazuje v Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toooptimize vaše Hive dotazuje na Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d6174c08-06aa-42ac-8e9b-8b8718d9978e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2016
ms.author: jgao
ms.openlocfilehash: d27f8100e1e9f4823040ff9f693e7b78d6192c6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a><span data-ttu-id="e7279-103">Optimalizace dotazů Hive v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="e7279-103">Optimize Hive queries in Azure HDInsight</span></span>

<span data-ttu-id="e7279-104">Ve výchozím nastavení nejsou clusterů systému Hadoop optimalizované pro výkon.</span><span class="sxs-lookup"><span data-stu-id="e7279-104">By default, Hadoop clusters are not optimized for performance.</span></span> <span data-ttu-id="e7279-105">Tento článek se zabývá některé nejběžnější Hive optimalizace metody údajů o výkonu, můžete použít tooyour dotazy.</span><span class="sxs-lookup"><span data-stu-id="e7279-105">This article covers some most common Hive performance optimization methods that you can apply tooyour queries.</span></span>

## <a name="scale-out-worker-nodes"></a><span data-ttu-id="e7279-106">Horizontální navýšení kapacity uzlů pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="e7279-106">Scale out worker nodes</span></span>

<span data-ttu-id="e7279-107">Zvýšení hello počet uzlů pracovního procesu v clusteru, můžete využít další mappers a přechodky toobe souběžně.</span><span class="sxs-lookup"><span data-stu-id="e7279-107">Increasing hello number of worker nodes in a cluster can leverage more mappers and reducers toobe run in parallel.</span></span> <span data-ttu-id="e7279-108">Existují dva způsoby, jak můžete zvýšit možností horizontálního rozšíření kapacity v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="e7279-108">There are two ways you can increase scale out in HDInsight:</span></span>

* <span data-ttu-id="e7279-109">Během hello zřizování můžete zadat hello počet uzlů pracovního procesu pomocí hello portálu Azure, Azure PowerShell nebo rozhraní příkazového řádku pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="e7279-109">At hello provision time, you can specify hello number of worker nodes using hello Azure portal, Azure PowerShell, or Cross-platform command-line interface.</span></span>  <span data-ttu-id="e7279-110">Další informace najdete v tématu [Vytvoření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="e7279-110">For more information, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="e7279-111">Hello následující snímek obrazovky ukazuje hello pracovní konfigurace uzlu na hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e7279-111">hello following screenshot shows hello worker node configuration on hello Azure portal:</span></span>
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* <span data-ttu-id="e7279-113">V hello běh můžete taky škálovat cluster bez nutnosti opětovného vytvoření jeden:</span><span class="sxs-lookup"><span data-stu-id="e7279-113">At hello run time, you can also scale out a cluster without recreating one:</span></span>

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

<span data-ttu-id="e7279-115">Další informace o hello různé virtuální počítače podporované v prostředí HDInsight najdete v tématu [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="e7279-115">For more information about hello different virtual machines supported by HDInsight, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="enable-tez"></a><span data-ttu-id="e7279-116">Povolení Tez</span><span class="sxs-lookup"><span data-stu-id="e7279-116">Enable Tez</span></span>

<span data-ttu-id="e7279-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) je modul alternativní provádění modul toohello MapReduce:</span><span class="sxs-lookup"><span data-stu-id="e7279-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) is an alternative execution engine toohello MapReduce engine:</span></span>

![tez_1][image-hdi-optimize-hive-tez_1]

<span data-ttu-id="e7279-119">Tez je rychlejší, protože:</span><span class="sxs-lookup"><span data-stu-id="e7279-119">Tez is faster because:</span></span>

* <span data-ttu-id="e7279-120">**Spuštění směrované Acyklické grafu (DAG) jako jednu úlohu v modulu MapReduce hello**.</span><span class="sxs-lookup"><span data-stu-id="e7279-120">**Execute Directed Acyclic Graph (DAG) as a single job in hello MapReduce engine**.</span></span> <span data-ttu-id="e7279-121">Hello DAG vyžaduje každou sadu mappers toobe následuje jednu sadu přechodky.</span><span class="sxs-lookup"><span data-stu-id="e7279-121">hello DAG requires each set of mappers toobe followed by one set of reducers.</span></span> <span data-ttu-id="e7279-122">To způsobí, že více toobe úlohy MapReduce spuštěné pro každý dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="e7279-122">This causes multiple MapReduce jobs toobe spun off for each Hive query.</span></span> <span data-ttu-id="e7279-123">Tez nemá takové omezení a může zpracovat komplexní DAG jako jednu úlohu, čímž dojde k minimalizaci režie spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="e7279-123">Tez does not have such constraint and can process complex DAG as one job thus minimizing job startup overhead.</span></span>
* <span data-ttu-id="e7279-124">**Zabraňuje zbytečným zápisy**.</span><span class="sxs-lookup"><span data-stu-id="e7279-124">**Avoids unnecessary writes**.</span></span> <span data-ttu-id="e7279-125">Z důvodu toomultiple úlohy se spouštějí pro hello stejný dotaz Hive v hello MapReduce stroje, výstup hello Každá úloha se zapíše tooHDFS pro mezilehlá data.</span><span class="sxs-lookup"><span data-stu-id="e7279-125">Due toomultiple jobs being spun for hello same Hive query in hello MapReduce engine, hello output of each job is written tooHDFS for intermediate data.</span></span> <span data-ttu-id="e7279-126">Vzhledem k tomu, že minimalizuje počet úloh pro každý dotaz Hive Tez je možné tooavoid nepotřebné zápisu.</span><span class="sxs-lookup"><span data-stu-id="e7279-126">Since Tez minimizes number of jobs for each Hive query it is able tooavoid unnecessary write.</span></span>
* <span data-ttu-id="e7279-127">**Minimalizuje zpoždění spuštění**.</span><span class="sxs-lookup"><span data-stu-id="e7279-127">**Minimizes start-up delays**.</span></span> <span data-ttu-id="e7279-128">Tez je lepší zpoždění spuštění možné toominimize snížením hello počet mappers, které potřebuje toostart a také vylepšení optimalizace v průběhu.</span><span class="sxs-lookup"><span data-stu-id="e7279-128">Tez is better able toominimize start-up delay by reducing hello number of mappers it needs toostart and also improving optimization throughout.</span></span>
* <span data-ttu-id="e7279-129">**Opětovně používá kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="e7279-129">**Reuses containers**.</span></span> <span data-ttu-id="e7279-130">Vždy, když možné Tez je možné tooreuse kontejnery tooensure této latence kvůli toostarting až kontejnery se snižuje.</span><span class="sxs-lookup"><span data-stu-id="e7279-130">Whenever possible Tez is able tooreuse containers tooensure that latency due toostarting up containers is reduced.</span></span>
* <span data-ttu-id="e7279-131">**Techniky průběžné optimalizace**.</span><span class="sxs-lookup"><span data-stu-id="e7279-131">**Continuous optimization techniques**.</span></span> <span data-ttu-id="e7279-132">Optimalizace tradičně bylo provedeno během fáze kompilace.</span><span class="sxs-lookup"><span data-stu-id="e7279-132">Traditionally optimization was done during compilation phase.</span></span> <span data-ttu-id="e7279-133">Ale je k dispozici další informace o hello vstupy, které umožňují lepší optimalizace za běhu.</span><span class="sxs-lookup"><span data-stu-id="e7279-133">However more information about hello inputs is available that allow for better optimization during runtime.</span></span> <span data-ttu-id="e7279-134">Tez používá průběžné optimalizace technik, které umožňuje toooptimize hello Další plán do fáze runtime hello.</span><span class="sxs-lookup"><span data-stu-id="e7279-134">Tez uses continuous optimization techniques that allows it toooptimize hello plan further into hello runtime phase.</span></span>

<span data-ttu-id="e7279-135">Další podrobnosti o těchto pojmech najdete v tématu [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="e7279-135">For more details on these concepts, see [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="e7279-136">Můžete použít jakýkoli dotaz Hive Tez povoleno pomocí prefixu hello dotazu s nastavením hello níže:</span><span class="sxs-lookup"><span data-stu-id="e7279-136">You can make any Hive query Tez enabled by prefixing hello query with hello setting below:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="e7279-137">Clustery HDInsight se systémem Linux mít Tez ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="e7279-137">Linux-based HDInsight clusters have Tez enabled by default.</span></span>


## <a name="hive-partitioning"></a><span data-ttu-id="e7279-138">Hive, vytváření oddílů</span><span class="sxs-lookup"><span data-stu-id="e7279-138">Hive partitioning</span></span>

<span data-ttu-id="e7279-139">Vstupně-výstupní operace je hello potíže hlavní výkonu pro spouštění dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="e7279-139">I/O operation is hello major performance bottleneck for running Hive queries.</span></span> <span data-ttu-id="e7279-140">je možné zlepšit výkon Hello, pokud hello množství dat, který potřebuje toobe přečíst, může být nižší.</span><span class="sxs-lookup"><span data-stu-id="e7279-140">hello performance can be improved if hello amount of data that needs toobe read can be reduced.</span></span> <span data-ttu-id="e7279-141">Ve výchozím nastavení kontrolu dotazů Hive celých tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="e7279-141">By default, Hive queries scan entire Hive tables.</span></span> <span data-ttu-id="e7279-142">Toto je skvělá pro dotazy jako prohledávání tabulky.</span><span class="sxs-lookup"><span data-stu-id="e7279-142">This is great for queries like table scans.</span></span> <span data-ttu-id="e7279-143">Ale pro dotazy, které stačí tooscan malé množství dat (například dotazy s filtrování), toto chování vytvoří nepotřebné režie.</span><span class="sxs-lookup"><span data-stu-id="e7279-143">However for queries that only need tooscan a small amount of data (for example, queries with filtering), this behavior creates unnecessary overhead.</span></span> <span data-ttu-id="e7279-144">Vytváření oddílů Hive umožňuje Hive dotazy tooaccess pouze hello nezbytné množství dat v tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="e7279-144">Hive partitioning allows Hive queries tooaccess only hello necessary amount of data in Hive tables.</span></span>

<span data-ttu-id="e7279-145">Vytváření oddílů Hive je implementováno modulem reorganizace hello nezpracovaná data do nového adresáře s každý oddíl má svou vlastní directory – kde je definován hello oddílu uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="e7279-145">Hive partitioning is implemented by reorganizing hello raw data into new directories with each partition having its own directory - where hello partition is defined by hello user.</span></span> <span data-ttu-id="e7279-146">Hello následující diagram znázorňuje dělení tabulku Hive podle sloupce hello *roku*.</span><span class="sxs-lookup"><span data-stu-id="e7279-146">hello following diagram illustrates partitioning a Hive table by hello column *Year*.</span></span> <span data-ttu-id="e7279-147">Pro každý rok se vytvoří nový adresář.</span><span class="sxs-lookup"><span data-stu-id="e7279-147">A new directory is created for each year.</span></span>

![Dělení na oddíly][image-hdi-optimize-hive-partitioning_1]

<span data-ttu-id="e7279-149">Některé rozdělení aspekty:</span><span class="sxs-lookup"><span data-stu-id="e7279-149">Some partitioning considerations:</span></span>

* <span data-ttu-id="e7279-150">**Proveďte není v oddílu** – vytváření oddílů pro sloupce s pouze několik hodnot může způsobit několik oddílů.</span><span class="sxs-lookup"><span data-stu-id="e7279-150">**Do not under partition** - Partitioning on columns with only a few values can cause few partitions.</span></span> <span data-ttu-id="e7279-151">Například vytváření oddílů na pohlaví pouze vytvoří dva oddíly toobe vytvořili (mužského a ženského), tedy pouze snížit latenci hello nejvýše o polovinu.</span><span class="sxs-lookup"><span data-stu-id="e7279-151">For example, partitioning on gender only creates two partitions toobe created (male and female), thus only reduce hello latency by a maximum of half.</span></span>
* <span data-ttu-id="e7279-152">**Proveďte nikoli prostřednictvím oddílu** – na jiné extreme Dobrý den, vytváření oddílů u sloupce s jedinečnou hodnotu (například ID uživatele) způsobí, že více oddílů.</span><span class="sxs-lookup"><span data-stu-id="e7279-152">**Do not over partition** - On hello other extreme, creating a partition on a column with a unique value (for example, userid) causes multiple partitions.</span></span> <span data-ttu-id="e7279-153">V oddílu způsobí, že mnoho přízvuk na hello clusteru namenode jako má toohandle hello velký počet adresářů.</span><span class="sxs-lookup"><span data-stu-id="e7279-153">Over partition causes much stress on hello cluster namenode as it has toohandle hello large number of directories.</span></span>
* <span data-ttu-id="e7279-154">**Vyhněte se data zkosení** -vyberte klíč rozdělení dobře tak, aby všechny oddíly jsou i velikost.</span><span class="sxs-lookup"><span data-stu-id="e7279-154">**Avoid data skew** - Choose your partitioning key wisely so that all partitions are even size.</span></span> <span data-ttu-id="e7279-155">Příklad rozdělení do oddílů na *stavu* může způsobit hello počet záznamů v části kalifornské toobe téměř 30 x u Vermont kvůli toohello rozdíl v naplnění.</span><span class="sxs-lookup"><span data-stu-id="e7279-155">An example is partitioning on *State* may cause hello number of records under California toobe almost 30x that of Vermont due toohello difference in population.</span></span>

<span data-ttu-id="e7279-156">toocreate tabulku oddílů použijte hello *rozdělena na oddíly pomocí* klauzule:</span><span class="sxs-lookup"><span data-stu-id="e7279-156">toocreate a partition table, use hello *Partitioned By* clause:</span></span>

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

<span data-ttu-id="e7279-157">Po vytvoření hello dělenou tabulku můžete buď vytvořit statické dělení nebo dynamické rozdělení.</span><span class="sxs-lookup"><span data-stu-id="e7279-157">Once hello partitioned table is created, you can either create static partitioning or dynamic partitioning.</span></span>

* <span data-ttu-id="e7279-158">**Statické dělení** znamená, že máte již horizontálně dělená data v hello příslušným adresáře a můžete požádat Hive oddíly ručně podle umístění adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="e7279-158">**Static partitioning** means that you have already sharded data in hello appropriate directories and you can ask Hive partitions manually based on hello directory location.</span></span> <span data-ttu-id="e7279-159">Následující fragment kódu Hello je příklad.</span><span class="sxs-lookup"><span data-stu-id="e7279-159">hello following code snippet is an example.</span></span>
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* <span data-ttu-id="e7279-160">**Dynamické vytváření oddílů** znamená, že oddíly toocreate Hive automaticky za vás.</span><span class="sxs-lookup"><span data-stu-id="e7279-160">**Dynamic partitioning** means that you want Hive toocreate partitions automatically for you.</span></span> <span data-ttu-id="e7279-161">Vzhledem k tomu, že jsme vytvořili již hello vytváření oddílů tabulky z hello pracovní tabulky, všechny potřebujeme toodo je tabulka toohello rozdělena na oddíly vložení dat:</span><span class="sxs-lookup"><span data-stu-id="e7279-161">Since we have already created hello partitioning table from hello staging table, all we need toodo is insert data toohello partitioned table:</span></span>
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

<span data-ttu-id="e7279-162">Další podrobnosti najdete v tématu [rozdělena na oddíly tabulky](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span><span class="sxs-lookup"><span data-stu-id="e7279-162">For more details, see [Partitioned Tables](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span></span>

## <a name="use-hello-orcfile-format"></a><span data-ttu-id="e7279-163">Použijte formát ORCFile hello</span><span class="sxs-lookup"><span data-stu-id="e7279-163">Use hello ORCFile format</span></span>
<span data-ttu-id="e7279-164">Hive podporuje jiné formáty souborů.</span><span class="sxs-lookup"><span data-stu-id="e7279-164">Hive supports different file formats.</span></span> <span data-ttu-id="e7279-165">Například:</span><span class="sxs-lookup"><span data-stu-id="e7279-165">For example:</span></span>

* <span data-ttu-id="e7279-166">**Text**: Toto je výchozí formát souboru hello a funguje s většinu scénářů</span><span class="sxs-lookup"><span data-stu-id="e7279-166">**Text**: this is hello default file format and works with most scenarios</span></span>
* <span data-ttu-id="e7279-167">**Avro**: funguje dobře pro scénáře interoperability</span><span class="sxs-lookup"><span data-stu-id="e7279-167">**Avro**: works well for interoperability scenarios</span></span>
* <span data-ttu-id="e7279-168">**ORC/Parquet**: nejvhodnější pro výkon</span><span class="sxs-lookup"><span data-stu-id="e7279-168">**ORC/Parquet**: best suited for performance</span></span>

<span data-ttu-id="e7279-169">Formát ORC (optimalizované řádek sloupcovém) je vysoce efektivní způsob toostore Hive data.</span><span class="sxs-lookup"><span data-stu-id="e7279-169">ORC (Optimized Row Columnar) format is a highly efficient way toostore Hive data.</span></span> <span data-ttu-id="e7279-170">Porovnání tooother formátů, ORC má hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e7279-170">Compared tooother formats, ORC has hello following advantages:</span></span>

* <span data-ttu-id="e7279-171">Podpora pro komplexní typy, včetně data a času a částečně strukturovaných a komplexní typy</span><span class="sxs-lookup"><span data-stu-id="e7279-171">support for complex types including DateTime and complex and semi-structured types</span></span>
* <span data-ttu-id="e7279-172">až too70 % komprese</span><span class="sxs-lookup"><span data-stu-id="e7279-172">up too70% compression</span></span>
* <span data-ttu-id="e7279-173">indexy každých 10 000 řádky, které povolit přeskočení řádků</span><span class="sxs-lookup"><span data-stu-id="e7279-173">indexes every 10,000 rows, which allow skipping rows</span></span>
* <span data-ttu-id="e7279-174">významné pokles v běhu provádění</span><span class="sxs-lookup"><span data-stu-id="e7279-174">a significant drop in run-time execution</span></span>

<span data-ttu-id="e7279-175">Formát ORC tooenable, nejprve vytvoříte tabulku s klauzulí hello *uložené jako ORC*:</span><span class="sxs-lookup"><span data-stu-id="e7279-175">tooenable ORC format, you first create a table with hello clause *Stored as ORC*:</span></span>

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

<span data-ttu-id="e7279-176">V dalším kroku vložení dat toohello ORC tabulky z hello pracovní tabulky.</span><span class="sxs-lookup"><span data-stu-id="e7279-176">Next, you insert data toohello ORC table from hello staging table.</span></span> <span data-ttu-id="e7279-177">Například:</span><span class="sxs-lookup"><span data-stu-id="e7279-177">For example:</span></span>

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
            L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

<span data-ttu-id="e7279-178">Si můžete přečíst informace o formátu ORC hello [zde](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span><span class="sxs-lookup"><span data-stu-id="e7279-178">You can read more on hello ORC format [here](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span></span>

## <a name="vectorization"></a><span data-ttu-id="e7279-179">Vectorization</span><span class="sxs-lookup"><span data-stu-id="e7279-179">Vectorization</span></span>

<span data-ttu-id="e7279-180">Vectorization umožňuje Hive tooprocess dávky 1024 řádků společně, namísto zpracování jeden řádek v čase.</span><span class="sxs-lookup"><span data-stu-id="e7279-180">Vectorization allows Hive tooprocess a batch of 1024 rows together instead of processing one row at a time.</span></span> <span data-ttu-id="e7279-181">Znamená to, že jednoduché operace jsou rychlejší provést, protože menší interní kód potřebuje toorun.</span><span class="sxs-lookup"><span data-stu-id="e7279-181">It means that simple operations are done faster because less internal code needs toorun.</span></span>

<span data-ttu-id="e7279-182">tooenable vectorization předpony dotaz Hive s hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="e7279-182">tooenable vectorization prefix your Hive query with hello following setting:</span></span>

    set hive.vectorized.execution.enabled = true;

<span data-ttu-id="e7279-183">Další informace najdete v tématu [Vectorized provádění dotazu](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span><span class="sxs-lookup"><span data-stu-id="e7279-183">For more information, see [Vectorized query execution](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span></span>

## <a name="other-optimization-methods"></a><span data-ttu-id="e7279-184">Ostatní metody optimalizace</span><span class="sxs-lookup"><span data-stu-id="e7279-184">Other optimization methods</span></span>
<span data-ttu-id="e7279-185">Existují další metody optimalizace, které můžete zvážit, například:</span><span class="sxs-lookup"><span data-stu-id="e7279-185">There are more optimization methods that you can consider, for example:</span></span>

* <span data-ttu-id="e7279-186">**Hive bucketing:** technika, který umožňuje toocluster nebo segment velkého objemu dat výkonu dotazů toooptimize.</span><span class="sxs-lookup"><span data-stu-id="e7279-186">**Hive bucketing:** a technique that allows toocluster or segment large sets of data toooptimize query performance.</span></span>
* <span data-ttu-id="e7279-187">**Připojení k optimalizaci:** optimalizace spouštění dotazů Hive na plánování tooimprove hello efektivitu spojení a snížit hello potřebu pomocné parametry uživatele.</span><span class="sxs-lookup"><span data-stu-id="e7279-187">**Join optimization:** optimization of Hive's query execution planning tooimprove hello efficiency of joins and reduce hello need for user hints.</span></span> <span data-ttu-id="e7279-188">Další informace najdete v tématu [připojení optimalizace](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span><span class="sxs-lookup"><span data-stu-id="e7279-188">For more information, see [Join optimization](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span></span>
* <span data-ttu-id="e7279-189">**Zvýšit přechodky**.</span><span class="sxs-lookup"><span data-stu-id="e7279-189">**Increase Reducers**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7279-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7279-190">Next steps</span></span>
<span data-ttu-id="e7279-191">V tomto článku jste se naučili několik běžné metody optimalizace dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="e7279-191">In this article, you have learned several common Hive query optimization methods.</span></span> <span data-ttu-id="e7279-192">toolearn více, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="e7279-192">toolearn more, see hello following articles:</span></span>

* [<span data-ttu-id="e7279-193">Používání Apache Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e7279-193">Use Apache Hive in HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="e7279-194">Analýza dat zpoždění letu pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e7279-194">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="e7279-195">Analýza dat Twitteru pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e7279-195">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="e7279-196">Analýza dat snímačů pomocí hello Hive dotaz konzoly systému Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e7279-196">Analyze sensor data using hello Hive Query Console on Hadoop in HDInsight</span></span>](hdinsight-hive-analyze-sensor-data.md)
* [<span data-ttu-id="e7279-197">Použijte Hive s HDInsight tooanalyze protokoly z webů</span><span class="sxs-lookup"><span data-stu-id="e7279-197">Use Hive with HDInsight tooanalyze logs from websites</span></span>](hdinsight-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
