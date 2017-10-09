---
title: aaaUse Hadoop Hive na hello konzoly dotazu v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak toouse hello webové konzole dotazu toorun dotazů Hive v HDInsight Hadoop clusteru z prohlížeče."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 621882082c9a07655d34b8dc980b8e47dd04b745
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-query-console"></a><span data-ttu-id="b5c0a-103">Spouštění dotazů Hive pomocí hello dotazu konzoly</span><span class="sxs-lookup"><span data-stu-id="b5c0a-103">Run Hive queries using hello Query Console</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="b5c0a-104">V tomto článku se dozvíte, jak dotazů Hive toouse hello HDInsight dotazu konzoly toorun na HDInsight Hadoop clusteru z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-104">In this article, you will learn how toouse hello HDInsight Query Console toorun Hive queries on an HDInsight Hadoop cluster from your browser.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b5c0a-105">Hello konzoly dotazu HDInsight je dostupná pouze na clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-105">hello HDInsight Query Console is only available on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="b5c0a-106">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b5c0a-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b5c0a-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="b5c0a-108">HDInsight 3.4 nebo větší, projděte si téma [spouštění dotazů Hive v zobrazení Ambari Hive](hdinsight-hadoop-use-hive-ambari-view.md) informace o spouštění dotazů Hive z webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-108">For HDInsight 3.4 or greater, see [Run Hive queries in Ambari Hive View](hdinsight-hadoop-use-hive-ambari-view.md) for information on running Hive queries from a web browser.</span></span>

## <span data-ttu-id="b5c0a-109"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5c0a-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="b5c0a-110">toocomplete hello kroky v tomto článku, budete potřebovat následující hello.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-110">toocomplete hello steps in this article, you will need hello following.</span></span>

* <span data-ttu-id="b5c0a-111">Cluster HDInsight Hadoop založené na systému Windows</span><span class="sxs-lookup"><span data-stu-id="b5c0a-111">A Windows-based HDInsight Hadoop cluster</span></span>
* <span data-ttu-id="b5c0a-112">Moderní webový prohlížeč</span><span class="sxs-lookup"><span data-stu-id="b5c0a-112">A modern web browser</span></span>

## <span data-ttu-id="b5c0a-113"><a id="run"></a>Spouštění dotazů Hive pomocí hello dotazu konzoly</span><span class="sxs-lookup"><span data-stu-id="b5c0a-113"><a id="run"></a> Run Hive queries using hello Query Console</span></span>
1. <span data-ttu-id="b5c0a-114">Otevřete webový prohlížeč a přejděte příliš**https://CLUSTERNAME.azurehdinsight.net**, kde **CLUSTERNAME** je hello název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-114">Open a web browser and navigate too**https://CLUSTERNAME.azurehdinsight.net**, where **CLUSTERNAME** is hello name of your HDInsight cluster.</span></span> <span data-ttu-id="b5c0a-115">Pokud se zobrazí výzva, zadejte hello uživatelské jméno a heslo, které jste použili při vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-115">If prompted, enter hello user name and password that you used when you created hello cluster.</span></span>
2. <span data-ttu-id="b5c0a-116">Hello odkazy v horní části hello hello stránky, vyberte **Hive Editor**.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-116">From hello links at hello top of hello page, select **Hive Editor**.</span></span> <span data-ttu-id="b5c0a-117">Zobrazí formulář, který lze použít tooenter hello HiveQL příkazy, které chcete toorun v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-117">This displays a form that can be used tooenter hello HiveQL statements that you want toorun in hello HDInsight cluster.</span></span>

    ![Hello hive editor](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    <span data-ttu-id="b5c0a-119">Nahraďte hello text `Select * from hivesampletable` s hello následující příkazy HiveQL:</span><span class="sxs-lookup"><span data-stu-id="b5c0a-119">Replace hello text `Select * from hivesampletable` with hello following HiveQL statements:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="b5c0a-120">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="b5c0a-120">These statements perform hello following actions:</span></span>

   * <span data-ttu-id="b5c0a-121">**VYŘAĎTE tabulku**: Odstraní tabulku hello a hello datový soubor, pokud tabulka hello již existuje.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-121">**DROP TABLE**: Deletes hello table and hello data file if hello table already exists.</span></span>
   * <span data-ttu-id="b5c0a-122">**Vytvoření externí tabulky**: vytvoří novou tabulku "externí" v Hive.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-122">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="b5c0a-123">Externí tabulky uložit pouze definici tabulky hello Hive; Hello data je ponechán v hello původního umístění.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-123">External tables store only hello table definition in Hive; hello data is left in hello original location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="b5c0a-124">Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje (například procesu nahrávání automatizované dat), nebo jiná operace MapReduce, ale chcete, aby, že Hive dotazuje toouse hello nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-124">External tables should be used when you expect hello underlying data toobe updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries toouse hello latest data.</span></span>
     >
     > <span data-ttu-id="b5c0a-125">Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-125">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>
     >
     >
   * <span data-ttu-id="b5c0a-126">**Řádek formátu**: informuje Hive formátování dat hello.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-126">**ROW FORMAT**: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="b5c0a-127">V takovém případě hello pole v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-127">In this case, hello fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="b5c0a-128">**ULOŽENÉ umístění textový soubor AS**: informuje Hive, kde je hello data uložená (adresář hello příklad nebo data) a která je uložena jako text</span><span class="sxs-lookup"><span data-stu-id="b5c0a-128">**STORED AS TEXTFILE LOCATION**: Tells Hive where hello data is stored (hello example/data directory) and that it is stored as text</span></span>
   * <span data-ttu-id="b5c0a-129">**Vyberte**: Vyberte počet všech řádků kde sloupec **t4** obsahovat hodnotu hello **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-129">**SELECT**: Select a count of all rows where column **t4** contain hello value **[ERROR]**.</span></span> <span data-ttu-id="b5c0a-130">To by měla vrátit hodnotu **3** vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-130">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="b5c0a-131">**INPUT__FILE__NAME jako '%.log'** -informuje Hive, který jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-131">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="b5c0a-132">To omezuje hello vyhledávání toohello sample.log souboru, který obsahuje hello data a udržuje ho z vrací data z jiných příkladu datové soubory, které neodpovídají hello schéma, které jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-132">This restricts hello search toohello sample.log file that contains hello data, and keeps it from returning data from other example data files that do not match hello schema we defined.</span></span>
3. <span data-ttu-id="b5c0a-133">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-133">Click **Submit**.</span></span> <span data-ttu-id="b5c0a-134">Hello **úlohy relace** v hello dolní části stránky hello by měl zobrazit podrobnosti úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-134">hello **Job Session** at hello bottom of hello page should display details for hello job.</span></span>
4. <span data-ttu-id="b5c0a-135">Když hello **stav** pole změny příliš**dokončeno**, vyberte **zobrazit podrobnosti** pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-135">When hello **Status** field changes too**Completed**, select **View Details** for hello job.</span></span> <span data-ttu-id="b5c0a-136">Na stránce s podrobnostmi o hello, hello **výstup úlohy** obsahuje `[ERROR]    3`.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-136">On hello details page, hello **Job Output** contains `[ERROR]    3`.</span></span> <span data-ttu-id="b5c0a-137">Můžete použít hello **Stáhnout** tlačítko pod toto pole toodownload soubor, který obsahuje hello výstup hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-137">You can use hello **Download** button under this field toodownload a file that contains hello output of hello job.</span></span>

## <span data-ttu-id="b5c0a-138"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="b5c0a-138"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="b5c0a-139">Jak vidíte, dotazy Hive v clusteru služby HDInsight hello dotazu Konzola poskytuje toorun snadno monitorovat stav úlohy hello a načíst výstup hello.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-139">As you can see, hello Query Console provides an easy way toorun Hive queries in an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

<span data-ttu-id="b5c0a-140">Vyberte toolearn Další informace o použití úlohy Hive toorun konzoly dotaz Hive, **Začínáme** hello horní části hello konzoly dotazu, pak použít hello vzorků, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-140">toolearn more about using Hive Query Console toorun Hive jobs, select **Getting Started** at hello top of hello Query Console, then use hello samples that are provided.</span></span> <span data-ttu-id="b5c0a-141">Každá ukázka vás provede procesem hello pomocí Hive tooanalyze data, včetně vysvětlení o příkazy HiveQL hello používá v ukázce hello.</span><span class="sxs-lookup"><span data-stu-id="b5c0a-141">Each sample walks through hello process of using Hive tooanalyze data, including explanations about hello HiveQL statements used in hello sample.</span></span>

## <span data-ttu-id="b5c0a-142"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5c0a-142"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="b5c0a-143">Obecné informace o Hive v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b5c0a-143">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="b5c0a-144">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b5c0a-144">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="b5c0a-145">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b5c0a-145">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="b5c0a-146">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b5c0a-146">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="b5c0a-147">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="b5c0a-147">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="b5c0a-148">Pokud používáte Tez s Hive, zobrazit hello následující dokumenty pro ladění informace:</span><span class="sxs-lookup"><span data-stu-id="b5c0a-148">If you are using Tez with Hive, see hello following documents for debugging information:</span></span>

* [<span data-ttu-id="b5c0a-149">Použití hello Tez uživatelského rozhraní na HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="b5c0a-149">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="b5c0a-150">Použití hello zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="b5c0a-150">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
