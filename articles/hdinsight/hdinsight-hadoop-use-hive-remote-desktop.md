---
title: "aaaUse Hadoop Hive a Vzdálená plocha v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooHadoop clusteru v prostředí HDInsight pomocí vzdálené plochy a pak spouštění dotazů Hive pomocí hello Hive rozhraní příkazového řádku."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c228e35-d58a-4f22-917a-1d20c9da89b4
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: f86ffc1be33a8b0b2346d1a1388e5dfa6d0f8777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a><span data-ttu-id="cb244-103">Použijte Hive s Hadoop v HDInsight pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="cb244-103">Use Hive with Hadoop on HDInsight with Remote Desktop</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="cb244-104">V tomto článku se dozvíte, jak tooconnect tooan HDInsight clusteru pomocí vzdálené plochy a poté spusťte Hive dotazuje pomocí hello Hive rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="cb244-104">In this article, you will learn how tooconnect tooan HDInsight cluster by using Remote Desktop, and then run Hive queries by using hello Hive Command-Line Interface (CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cb244-105">Vzdálená plocha je dostupná pouze na clustery HDInsight, které používají Windows jako hello operační systém.</span><span class="sxs-lookup"><span data-stu-id="cb244-105">Remote Desktop is only available on HDInsight clusters that use Windows as hello operating system.</span></span> <span data-ttu-id="cb244-106">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="cb244-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="cb244-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="cb244-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="cb244-108">HDInsight 3.4 nebo větší, projděte si téma [používání Hive s HDInsight a Beeline](hdinsight-hadoop-use-hive-beeline.md) informace o spouštění dotazů Hive přímo na clusteru hello z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="cb244-108">For HDInsight 3.4 or greater, see [Use Hive with HDInsight and Beeline](hdinsight-hadoop-use-hive-beeline.md) for information on running Hive queries directly on hello cluster from a command-line.</span></span>

## <span data-ttu-id="cb244-109"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="cb244-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="cb244-110">toocomplete hello kroky v tomto článku, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="cb244-110">toocomplete hello steps in this article, you will need hello following:</span></span>

* <span data-ttu-id="cb244-111">Cluster HDInsight se systémem Windows (Hadoop v HDInsight)</span><span class="sxs-lookup"><span data-stu-id="cb244-111">A Windows-based HDInsight (Hadoop on HDInsight) cluster</span></span>
* <span data-ttu-id="cb244-112">Klientský počítač se systémem Windows 10, Windows 8 nebo Windows 7</span><span class="sxs-lookup"><span data-stu-id="cb244-112">A client computer running Windows 10, Window 8, or Windows 7</span></span>

## <span data-ttu-id="cb244-113"><a id="connect"></a>Připojit pomocí vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="cb244-113"><a id="connect"></a>Connect with Remote Desktop</span></span>
<span data-ttu-id="cb244-114">Povolení vzdálené plochy pro hello HDInsight cluster a pak připojit tooit podle následujících pokynů hello [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="cb244-114">Enable Remote Desktop for hello HDInsight cluster, then connect tooit by following hello instructions at [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

## <span data-ttu-id="cb244-115"><a id="hive"></a>Použijte příkaz Hive hello</span><span class="sxs-lookup"><span data-stu-id="cb244-115"><a id="hive"></a>Use hello Hive command</span></span>
<span data-ttu-id="cb244-116">Když připojíte toohello desktop pro hello HDInsight cluster, použijte následující kroky toowork s Hive hello:</span><span class="sxs-lookup"><span data-stu-id="cb244-116">When you have connected toohello desktop for hello HDInsight cluster, use hello following steps toowork with Hive:</span></span>

1. <span data-ttu-id="cb244-117">Z plochy hello HDInsight, spusťte hello **Hadoop příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="cb244-117">From hello HDInsight desktop, start hello **Hadoop Command Line**.</span></span>
2. <span data-ttu-id="cb244-118">Zadejte následující příkaz toostart hello Hive CLI hello:</span><span class="sxs-lookup"><span data-stu-id="cb244-118">Enter hello following command toostart hello Hive CLI:</span></span>

        %hive_home%\bin\hive

    <span data-ttu-id="cb244-119">Pokud bylo zahájeno hello rozhraní příkazového řádku, zobrazí se řádku hello Hive rozhraní příkazového řádku: `hive>`.</span><span class="sxs-lookup"><span data-stu-id="cb244-119">When hello CLI has started, you will see hello Hive CLI prompt: `hive>`.</span></span>
3. <span data-ttu-id="cb244-120">Pomocí hello rozhraní příkazového řádku, zadejte následující příkazy toocreate novou tabulku s názvem hello **log4jLogs** pomocí ukázkových dat:</span><span class="sxs-lookup"><span data-stu-id="cb244-120">Using hello CLI, enter hello following statements toocreate a new table named **log4jLogs** using sample data:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="cb244-121">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="cb244-121">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="cb244-122">**VYŘAĎTE tabulku**: Odstraní tabulku hello a hello datový soubor, pokud tabulka hello již existuje.</span><span class="sxs-lookup"><span data-stu-id="cb244-122">**DROP TABLE**: Deletes hello table and hello data file if hello table already exists.</span></span>
   * <span data-ttu-id="cb244-123">**Vytvoření externí tabulky**: vytvoří novou tabulku "externí" v Hive.</span><span class="sxs-lookup"><span data-stu-id="cb244-123">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="cb244-124">Externí tabulky uložit pouze definici tabulky hello Hive (hello dat je ponecháno v původním umístění hello).</span><span class="sxs-lookup"><span data-stu-id="cb244-124">External tables store only hello table definition in Hive (hello data is left in hello original location).</span></span>

     > [!NOTE]
     > <span data-ttu-id="cb244-125">Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje (například procesu nahrávání automatizované dat), nebo jiná operace MapReduce, ale chcete, aby, že Hive dotazuje toouse hello nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="cb244-125">External tables should be used when you expect hello underlying data toobe updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries toouse hello latest data.</span></span>
     >
     > <span data-ttu-id="cb244-126">Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb244-126">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>
     >
     >
   * <span data-ttu-id="cb244-127">**Řádek formátu**: informuje Hive formátování dat hello.</span><span class="sxs-lookup"><span data-stu-id="cb244-127">**ROW FORMAT**: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="cb244-128">V takovém případě hello pole v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="cb244-128">In this case, hello fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="cb244-129">**ULOŽENÉ umístění textový soubor AS**: informuje Hive, kde je hello data uložená (adresář hello příklad nebo data) a která je uložena jako text.</span><span class="sxs-lookup"><span data-stu-id="cb244-129">**STORED AS TEXTFILE LOCATION**: Tells Hive where hello data is stored (hello example/data directory) and that it is stored as text.</span></span>
   * <span data-ttu-id="cb244-130">**Vyberte**: počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu hello **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="cb244-130">**SELECT**: Selects a count of all rows where column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="cb244-131">To by měla vrátit hodnotu **3** vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cb244-131">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="cb244-132">**INPUT__FILE__NAME jako '%.log'** -informuje Hive, který jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="cb244-132">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="cb244-133">To omezuje hello vyhledávání toohello sample.log souboru, který obsahuje hello data a udržuje ho z vrací data z jiných příkladu datové soubory, které neodpovídají hello schéma, které jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="cb244-133">This restricts hello search toohello sample.log file that contains hello data, and keeps it from returning data from other example data files that do not match hello schema we defined.</span></span>
4. <span data-ttu-id="cb244-134">Hello použijte následující příkazy toocreate novou "interní" tabulku s názvem **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="cb244-134">Use hello following statements toocreate a new 'internal' table named **errorLogs**:</span></span>

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    <span data-ttu-id="cb244-135">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="cb244-135">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="cb244-136">**Vytvoření tabulka není v případě existuje**: vytvoří tabulku, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="cb244-136">**CREATE TABLE IF NOT EXISTS**: Creates a table if it does not already exist.</span></span> <span data-ttu-id="cb244-137">Protože hello **externí** – klíčové slovo nepoužívá, jde o interní tabulku, která je uložena v datovém skladu hello Hive a je zcela spravuje Hive.</span><span class="sxs-lookup"><span data-stu-id="cb244-137">Because hello **EXTERNAL** keyword is not used, this is an internal table, which is stored in hello Hive data warehouse and is managed completely by Hive.</span></span>

     > [!NOTE]
     > <span data-ttu-id="cb244-138">Na rozdíl od **externí** tabulky, vyřazení interní tabulku taky odstraní hello základní data.</span><span class="sxs-lookup"><span data-stu-id="cb244-138">Unlike **EXTERNAL** tables, dropping an internal table also deletes hello underlying data.</span></span>
     >
     >
   * <span data-ttu-id="cb244-139">**ULOŽENÉ ORC AS**: ukládá hello data ve sloupcovém formátu (ORC) optimalizované řádek.</span><span class="sxs-lookup"><span data-stu-id="cb244-139">**STORED AS ORC**: Stores hello data in optimized row columnar (ORC) format.</span></span> <span data-ttu-id="cb244-140">Toto je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="cb244-140">This is a highly optimized and efficient format for storing Hive data.</span></span>
   * <span data-ttu-id="cb244-141">**PŘEPSAT INSERT... Vyberte**: vybere řádky z hello **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vloží hello data do hello **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb244-141">**INSERT OVERWRITE ... SELECT**: Selects rows from hello **log4jLogs** table that contain **[ERROR]**, then inserts hello data into hello **errorLogs** table.</span></span>

     <span data-ttu-id="cb244-142">tooverify, který pouze řádků, které obsahují **[Chyba]** ve sloupci t4 byly uložené toohello **errorLogs** tabulky, použijte následující příkaz tooreturn všechny řádky z hello hello **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="cb244-142">tooverify that only rows that contain **[ERROR]** in column t4 were stored toohello **errorLogs** table, use hello following statement tooreturn all hello rows from **errorLogs**:</span></span>

       <span data-ttu-id="cb244-143">Vybrat * z errorLogs;</span><span class="sxs-lookup"><span data-stu-id="cb244-143">SELECT * from errorLogs;</span></span>

     <span data-ttu-id="cb244-144">Tří řádků dat má být vrácen, všechny obsahující **[Chyba]** v t4 sloupce.</span><span class="sxs-lookup"><span data-stu-id="cb244-144">Three rows of data should be returned, all containing **[ERROR]** in column t4.</span></span>

## <span data-ttu-id="cb244-145"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="cb244-145"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="cb244-146">Jak vidíte, hello hello Hive příkaz poskytuje toointeractively snadný způsob spouštění dotazů Hive v clusteru HDInsight, monitorování hello stav úlohy a načíst výstup hello.</span><span class="sxs-lookup"><span data-stu-id="cb244-146">As you can see, hello hello Hive command provides an easy way toointeractively run Hive queries on an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <span data-ttu-id="cb244-147"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb244-147"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="cb244-148">Obecné informace o Hive v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="cb244-148">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="cb244-149">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb244-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="cb244-150">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="cb244-150">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="cb244-151">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb244-151">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="cb244-152">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cb244-152">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="cb244-153">Pokud používáte Tez s Hive, zobrazit hello následující dokumenty pro ladění informace:</span><span class="sxs-lookup"><span data-stu-id="cb244-153">If you are using Tez with Hive, see hello following documents for debugging information:</span></span>

* [<span data-ttu-id="cb244-154">Použití hello Tez uživatelského rozhraní na HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="cb244-154">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="cb244-155">Použití hello zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="cb244-155">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
