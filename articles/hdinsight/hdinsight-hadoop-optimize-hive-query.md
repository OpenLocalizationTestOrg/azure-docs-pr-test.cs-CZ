---
title: "Optimalizace dotazů Hive v Azure HDInsight | Microsoft Docs"
description: "Informace o optimalizaci své dotazy Hive pro Hadoop v HDInsight."
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
ms.openlocfilehash: edbf797e6277a65b5311e4939f5ab72776b11557
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a><span data-ttu-id="a9f0e-103">Optimalizace dotazů Hive v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="a9f0e-103">Optimize Hive queries in Azure HDInsight</span></span>

<span data-ttu-id="a9f0e-104">Ve výchozím nastavení nejsou clusterů systému Hadoop optimalizované pro výkon.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-104">By default, Hadoop clusters are not optimized for performance.</span></span> <span data-ttu-id="a9f0e-105">Tento článek se zabývá některé nejběžnější Hive výkonu optimalizace metody, které můžete použít pro své dotazy.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-105">This article covers some most common Hive performance optimization methods that you can apply to your queries.</span></span>

## <a name="scale-out-worker-nodes"></a><span data-ttu-id="a9f0e-106">Horizontální navýšení kapacity uzlů pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="a9f0e-106">Scale out worker nodes</span></span>

<span data-ttu-id="a9f0e-107">Zvýšit počet uzlů pracovního procesu v clusteru s podporou můžete využít další mappers a přechodky ke spuštění paralelně.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-107">Increasing the number of worker nodes in a cluster can leverage more mappers and reducers to be run in parallel.</span></span> <span data-ttu-id="a9f0e-108">Existují dva způsoby, jak můžete zvýšit možností horizontálního rozšíření kapacity v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-108">There are two ways you can increase scale out in HDInsight:</span></span>

* <span data-ttu-id="a9f0e-109">Během zřizování můžete zadat počet uzlů pracovního procesu pomocí portálu Azure, Azure PowerShell nebo rozhraní příkazového řádku pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-109">At the provision time, you can specify the number of worker nodes using the Azure portal, Azure PowerShell, or Cross-platform command-line interface.</span></span>  <span data-ttu-id="a9f0e-110">Další informace najdete v tématu [Vytvoření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="a9f0e-110">For more information, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="a9f0e-111">Následující snímek obrazovky ukazuje pracovního procesu konfigurace uzlu na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-111">The following screenshot shows the worker node configuration on the Azure portal:</span></span>
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* <span data-ttu-id="a9f0e-113">Za běhu můžete taky škálovat cluster bez nutnosti opětovného vytvoření jeden:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-113">At the run time, you can also scale out a cluster without recreating one:</span></span>

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

<span data-ttu-id="a9f0e-115">Další informace o různé virtuální počítače podporované v prostředí HDInsight najdete v tématu [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a9f0e-115">For more information about the different virtual machines supported by HDInsight, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="enable-tez"></a><span data-ttu-id="a9f0e-116">Povolení Tez</span><span class="sxs-lookup"><span data-stu-id="a9f0e-116">Enable Tez</span></span>

<span data-ttu-id="a9f0e-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) je modul alternativní provádění k modulu MapReduce:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-117">[Apache Tez](http://hortonworks.com/hadoop/tez/) is an alternative execution engine to the MapReduce engine:</span></span>

![tez_1][image-hdi-optimize-hive-tez_1]

<span data-ttu-id="a9f0e-119">Tez je rychlejší, protože:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-119">Tez is faster because:</span></span>

* <span data-ttu-id="a9f0e-120">**Spuštění směrované Acyklické grafu (DAG) jako jednu úlohu v modulu MapReduce**.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-120">**Execute Directed Acyclic Graph (DAG) as a single job in the MapReduce engine**.</span></span> <span data-ttu-id="a9f0e-121">DAG vyžaduje každou sadu mappers pro jednu sadu přechodky následovat.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-121">The DAG requires each set of mappers to be followed by one set of reducers.</span></span> <span data-ttu-id="a9f0e-122">To způsobí, že se pro každý dotaz Hive spuštěné více úloh MapReduce.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-122">This causes multiple MapReduce jobs to be spun off for each Hive query.</span></span> <span data-ttu-id="a9f0e-123">Tez nemá takové omezení a může zpracovat komplexní DAG jako jednu úlohu, čímž dojde k minimalizaci režie spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-123">Tez does not have such constraint and can process complex DAG as one job thus minimizing job startup overhead.</span></span>
* <span data-ttu-id="a9f0e-124">**Zabraňuje zbytečným zápisy**.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-124">**Avoids unnecessary writes**.</span></span> <span data-ttu-id="a9f0e-125">Z důvodu několik úloh, které se spouštějí pro stejný dotaz Hive v rámci stroje MapReduce výstup každé úlohy zapsán do HDFS pro mezilehlá data.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-125">Due to multiple jobs being spun for the same Hive query in the MapReduce engine, the output of each job is written to HDFS for intermediate data.</span></span> <span data-ttu-id="a9f0e-126">Vzhledem k tomu, že minimalizuje počet úloh pro každý dotaz Hive Tez je k tomu nepotřebné zápisu.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-126">Since Tez minimizes number of jobs for each Hive query it is able to avoid unnecessary write.</span></span>
* <span data-ttu-id="a9f0e-127">**Minimalizuje zpoždění spuštění**.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-127">**Minimizes start-up delays**.</span></span> <span data-ttu-id="a9f0e-128">Tez je lépe minimalizovat snížení počtu mappers, musí se spustit a také vylepšení optimalizace v rámci zpoždění spuštění.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-128">Tez is better able to minimize start-up delay by reducing the number of mappers it needs to start and also improving optimization throughout.</span></span>
* <span data-ttu-id="a9f0e-129">**Opětovně používá kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-129">**Reuses containers**.</span></span> <span data-ttu-id="a9f0e-130">Vždy, když je možné Tez mohli znovu použít kontejnery zajistit, že se snižuje latence kvůli spuštění kontejnery.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-130">Whenever possible Tez is able to reuse containers to ensure that latency due to starting up containers is reduced.</span></span>
* <span data-ttu-id="a9f0e-131">**Techniky průběžné optimalizace**.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-131">**Continuous optimization techniques**.</span></span> <span data-ttu-id="a9f0e-132">Optimalizace tradičně bylo provedeno během fáze kompilace.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-132">Traditionally optimization was done during compilation phase.</span></span> <span data-ttu-id="a9f0e-133">Ale je k dispozici další informace o vstupních hodnot, které umožňují lepší optimalizace za běhu.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-133">However more information about the inputs is available that allow for better optimization during runtime.</span></span> <span data-ttu-id="a9f0e-134">Tez používá průběžné optimalizace technik, které umožní Optimalizace plánu další do fáze modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-134">Tez uses continuous optimization techniques that allows it to optimize the plan further into the runtime phase.</span></span>

<span data-ttu-id="a9f0e-135">Další podrobnosti o těchto pojmech najdete v tématu [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="a9f0e-135">For more details on these concepts, see [Apache TEZ](http://hortonworks.com/hadoop/tez/).</span></span>

<span data-ttu-id="a9f0e-136">Můžete použít jakýkoli dotaz Hive Tez povoleno pomocí prefixu dotaz pomocí tohoto nastavení:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-136">You can make any Hive query Tez enabled by prefixing the query with the setting below:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="a9f0e-137">Clustery HDInsight se systémem Linux mít Tez ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-137">Linux-based HDInsight clusters have Tez enabled by default.</span></span>


## <a name="hive-partitioning"></a><span data-ttu-id="a9f0e-138">Hive, vytváření oddílů</span><span class="sxs-lookup"><span data-stu-id="a9f0e-138">Hive partitioning</span></span>

<span data-ttu-id="a9f0e-139">Vstupně-výstupní operace je potíže hlavní výkonu pro spouštění dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-139">I/O operation is the major performance bottleneck for running Hive queries.</span></span> <span data-ttu-id="a9f0e-140">Pokud se může snížit množství dat, který vyžaduje čtení, je možné zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-140">The performance can be improved if the amount of data that needs to be read can be reduced.</span></span> <span data-ttu-id="a9f0e-141">Ve výchozím nastavení kontrolu dotazů Hive celých tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-141">By default, Hive queries scan entire Hive tables.</span></span> <span data-ttu-id="a9f0e-142">Toto je skvělá pro dotazy jako prohledávání tabulky.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-142">This is great for queries like table scans.</span></span> <span data-ttu-id="a9f0e-143">Ale pro dotazy, které stačí ke kontrole malé množství dat (například dotazy s filtrování), toto chování vytvoří nepotřebné režie.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-143">However for queries that only need to scan a small amount of data (for example, queries with filtering), this behavior creates unnecessary overhead.</span></span> <span data-ttu-id="a9f0e-144">Vytváření oddílů Hive umožňuje dotazů Hive přístup pouze nezbytné množství dat v tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-144">Hive partitioning allows Hive queries to access only the necessary amount of data in Hive tables.</span></span>

<span data-ttu-id="a9f0e-145">Vytváření oddílů Hive je implementováno modulem reorganizace nezpracovaná data do nového adresáře s každý oddíl má svou vlastní directory – kde je oddíl definovaných uživatelem.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-145">Hive partitioning is implemented by reorganizing the raw data into new directories with each partition having its own directory - where the partition is defined by the user.</span></span> <span data-ttu-id="a9f0e-146">Následující diagram znázorňuje dělení tabulku Hive podle sloupce *roku*.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-146">The following diagram illustrates partitioning a Hive table by the column *Year*.</span></span> <span data-ttu-id="a9f0e-147">Pro každý rok se vytvoří nový adresář.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-147">A new directory is created for each year.</span></span>

![Dělení na oddíly][image-hdi-optimize-hive-partitioning_1]

<span data-ttu-id="a9f0e-149">Některé rozdělení aspekty:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-149">Some partitioning considerations:</span></span>

* <span data-ttu-id="a9f0e-150">**Proveďte není v oddílu** – vytváření oddílů pro sloupce s pouze několik hodnot může způsobit několik oddílů.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-150">**Do not under partition** - Partitioning on columns with only a few values can cause few partitions.</span></span> <span data-ttu-id="a9f0e-151">Například vytváření oddílů na pohlaví vytvoří pouze dva oddíly, které mají být vytvořené (mužského a ženského), tedy pouze snížit latenci nejvýše o polovinu.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-151">For example, partitioning on gender only creates two partitions to be created (male and female), thus only reduce the latency by a maximum of half.</span></span>
* <span data-ttu-id="a9f0e-152">**Proveďte nikoli prostřednictvím oddílu** – na jiné extreme, vytváření oddílů u sloupce s jedinečnou hodnotu (například ID uživatele) způsobí, že více oddílů.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-152">**Do not over partition** - On the other extreme, creating a partition on a column with a unique value (for example, userid) causes multiple partitions.</span></span> <span data-ttu-id="a9f0e-153">V oddílu způsobí, že mnoho přízvuk na namenode clusteru jako má zpracovat velký počet adresářů.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-153">Over partition causes much stress on the cluster namenode as it has to handle the large number of directories.</span></span>
* <span data-ttu-id="a9f0e-154">**Vyhněte se data zkosení** -vyberte klíč rozdělení dobře tak, aby všechny oddíly jsou i velikost.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-154">**Avoid data skew** - Choose your partitioning key wisely so that all partitions are even size.</span></span> <span data-ttu-id="a9f0e-155">Příklad rozdělení do oddílů na *stavu* může způsobit, že počet záznamů v části kalifornské být téměř 30 x u Vermont kvůli rozdílu ve naplnění.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-155">An example is partitioning on *State* may cause the number of records under California to be almost 30x that of Vermont due to the difference in population.</span></span>

<span data-ttu-id="a9f0e-156">Chcete-li vytvořit tabulku oddílu, použijte *rozdělena na oddíly pomocí* klauzule:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-156">To create a partition table, use the *Partitioned By* clause:</span></span>

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

<span data-ttu-id="a9f0e-157">Po vytvoření oddílů tabulky, můžete buď vytvořit statické dělení nebo dynamické rozdělení.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-157">Once the partitioned table is created, you can either create static partitioning or dynamic partitioning.</span></span>

* <span data-ttu-id="a9f0e-158">**Statické dělení** znamená, že máte již horizontálně dělená data v adresáři odpovídající a můžete požádat Hive oddíly ručně v závislosti na umístění adresáře.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-158">**Static partitioning** means that you have already sharded data in the appropriate directories and you can ask Hive partitions manually based on the directory location.</span></span> <span data-ttu-id="a9f0e-159">Následující fragment kódu je příklad.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-159">The following code snippet is an example.</span></span>
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* <span data-ttu-id="a9f0e-160">**Dynamické vytváření oddílů** znamená, že Hive, aby pro vás automaticky vytvořil oddíly.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-160">**Dynamic partitioning** means that you want Hive to create partitions automatically for you.</span></span> <span data-ttu-id="a9f0e-161">Vzhledem k tomu, že jsme vytvořili již rozdělení tabulky z pracovní tabulky, je potřeba udělat je vložit data do oddílů tabulky:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-161">Since we have already created the partitioning table from the staging table, all we need to do is insert data to the partitioned table:</span></span>
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

<span data-ttu-id="a9f0e-162">Další podrobnosti najdete v tématu [rozdělena na oddíly tabulky](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span><span class="sxs-lookup"><span data-stu-id="a9f0e-162">For more details, see [Partitioned Tables](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).</span></span>

## <a name="use-the-orcfile-format"></a><span data-ttu-id="a9f0e-163">Použijte formát ORCFile</span><span class="sxs-lookup"><span data-stu-id="a9f0e-163">Use the ORCFile format</span></span>
<span data-ttu-id="a9f0e-164">Hive podporuje jiné formáty souborů.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-164">Hive supports different file formats.</span></span> <span data-ttu-id="a9f0e-165">Například:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-165">For example:</span></span>

* <span data-ttu-id="a9f0e-166">**Text**: Toto je výchozí formát souboru a spolupracuje s většinu scénářů</span><span class="sxs-lookup"><span data-stu-id="a9f0e-166">**Text**: this is the default file format and works with most scenarios</span></span>
* <span data-ttu-id="a9f0e-167">**Avro**: funguje dobře pro scénáře interoperability</span><span class="sxs-lookup"><span data-stu-id="a9f0e-167">**Avro**: works well for interoperability scenarios</span></span>
* <span data-ttu-id="a9f0e-168">**ORC/Parquet**: nejvhodnější pro výkon</span><span class="sxs-lookup"><span data-stu-id="a9f0e-168">**ORC/Parquet**: best suited for performance</span></span>

<span data-ttu-id="a9f0e-169">Formát ORC (optimalizované řádek sloupcovém) je vysoce efektivní způsob, jak ukládat Hive data.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-169">ORC (Optimized Row Columnar) format is a highly efficient way to store Hive data.</span></span> <span data-ttu-id="a9f0e-170">Porovnání s jinými formáty, ORC má následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-170">Compared to other formats, ORC has the following advantages:</span></span>

* <span data-ttu-id="a9f0e-171">Podpora pro komplexní typy, včetně data a času a částečně strukturovaných a komplexní typy</span><span class="sxs-lookup"><span data-stu-id="a9f0e-171">support for complex types including DateTime and complex and semi-structured types</span></span>
* <span data-ttu-id="a9f0e-172">až 70 % komprese</span><span class="sxs-lookup"><span data-stu-id="a9f0e-172">up to 70% compression</span></span>
* <span data-ttu-id="a9f0e-173">indexy každých 10 000 řádky, které povolit přeskočení řádků</span><span class="sxs-lookup"><span data-stu-id="a9f0e-173">indexes every 10,000 rows, which allow skipping rows</span></span>
* <span data-ttu-id="a9f0e-174">významné pokles v běhu provádění</span><span class="sxs-lookup"><span data-stu-id="a9f0e-174">a significant drop in run-time execution</span></span>

<span data-ttu-id="a9f0e-175">Pokud chcete povolit ORC formátu, nejprve vytvoříte tabulku s klauzulí *uložené jako ORC*:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-175">To enable ORC format, you first create a table with the clause *Stored as ORC*:</span></span>

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

<span data-ttu-id="a9f0e-176">V dalším kroku vkládání dat do tabulky ORC z pracovní tabulky.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-176">Next, you insert data to the ORC table from the staging table.</span></span> <span data-ttu-id="a9f0e-177">Například:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-177">For example:</span></span>

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

<span data-ttu-id="a9f0e-178">Další informace o formátu ORC [zde](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span><span class="sxs-lookup"><span data-stu-id="a9f0e-178">You can read more on the ORC format [here](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).</span></span>

## <a name="vectorization"></a><span data-ttu-id="a9f0e-179">Vectorization</span><span class="sxs-lookup"><span data-stu-id="a9f0e-179">Vectorization</span></span>

<span data-ttu-id="a9f0e-180">Vectorization umožňuje Hive ke zpracování dávku řádků 1024 společně místo zpracování jeden řádek v čase.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-180">Vectorization allows Hive to process a batch of 1024 rows together instead of processing one row at a time.</span></span> <span data-ttu-id="a9f0e-181">Znamená to, že jednoduché operace jsou rychlejší provést, protože menší interní kód je potřeba spustit.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-181">It means that simple operations are done faster because less internal code needs to run.</span></span>

<span data-ttu-id="a9f0e-182">Chcete-li povolit předpony vectorization dotaz Hive s následujícím nastavením:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-182">To enable vectorization prefix your Hive query with the following setting:</span></span>

    set hive.vectorized.execution.enabled = true;

<span data-ttu-id="a9f0e-183">Další informace najdete v tématu [Vectorized provádění dotazu](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span><span class="sxs-lookup"><span data-stu-id="a9f0e-183">For more information, see [Vectorized query execution](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).</span></span>

## <a name="other-optimization-methods"></a><span data-ttu-id="a9f0e-184">Ostatní metody optimalizace</span><span class="sxs-lookup"><span data-stu-id="a9f0e-184">Other optimization methods</span></span>
<span data-ttu-id="a9f0e-185">Existují další metody optimalizace, které můžete zvážit, například:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-185">There are more optimization methods that you can consider, for example:</span></span>

* <span data-ttu-id="a9f0e-186">**Hive bucketing:** technika, který umožňuje clusteru nebo segmentovat velké nastaví dat za účelem optimalizace výkonu dotazů.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-186">**Hive bucketing:** a technique that allows to cluster or segment large sets of data to optimize query performance.</span></span>
* <span data-ttu-id="a9f0e-187">**Připojení k optimalizaci:** optimalizace spouštění dotazů Hive na plánování ke zlepšení efektivity spojení a snížit nutnost pomocné parametry uživatele.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-187">**Join optimization:** optimization of Hive's query execution planning to improve the efficiency of joins and reduce the need for user hints.</span></span> <span data-ttu-id="a9f0e-188">Další informace najdete v tématu [připojení optimalizace](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span><span class="sxs-lookup"><span data-stu-id="a9f0e-188">For more information, see [Join optimization](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).</span></span>
* <span data-ttu-id="a9f0e-189">**Zvýšit přechodky**.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-189">**Increase Reducers**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9f0e-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a9f0e-190">Next steps</span></span>
<span data-ttu-id="a9f0e-191">V tomto článku jste se naučili několik běžné metody optimalizace dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="a9f0e-191">In this article, you have learned several common Hive query optimization methods.</span></span> <span data-ttu-id="a9f0e-192">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="a9f0e-192">To learn more, see the following articles:</span></span>

* [<span data-ttu-id="a9f0e-193">Používání Apache Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a9f0e-193">Use Apache Hive in HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="a9f0e-194">Analýza dat zpoždění letu pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a9f0e-194">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="a9f0e-195">Analýza dat Twitteru pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a9f0e-195">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="a9f0e-196">Analýza dat snímačů v konzole dotaz Hive na Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a9f0e-196">Analyze sensor data using the Hive Query Console on Hadoop in HDInsight</span></span>](hdinsight-hive-analyze-sensor-data.md)
* [<span data-ttu-id="a9f0e-197">Použijte Hive s HDInsight k analýze protokolů z webů</span><span class="sxs-lookup"><span data-stu-id="a9f0e-197">Use Hive with HDInsight to analyze logs from websites</span></span>](hdinsight-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
