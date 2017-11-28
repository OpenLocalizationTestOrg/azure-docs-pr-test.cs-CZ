---
title: "Používání Hadoop Hive v konzole dotazu v HDInsight - Azure | Microsoft Docs"
description: "Naučte se používat webovou konzolu dotazu ke spouštění dotazů Hive v clusteru HDInsight Hadoop z prohlížeče."
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
ms.openlocfilehash: 9ccac43ae365d79bfd6ac1edf4d9a799c11356a1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-hive-queries-using-the-query-console"></a><span data-ttu-id="ed811-103">Spouštění dotazů Hive pomocí konzole dotazu</span><span class="sxs-lookup"><span data-stu-id="ed811-103">Run Hive queries using the Query Console</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="ed811-104">V tomto článku se dozvíte, jak používat konzolu dotazu HDInsight ke spouštění dotazů Hive v clusteru HDInsight Hadoop z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ed811-104">In this article, you will learn how to use the HDInsight Query Console to run Hive queries on an HDInsight Hadoop cluster from your browser.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ed811-105">Konzole dotazu HDInsight je dostupná pouze na clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="ed811-105">The HDInsight Query Console is only available on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="ed811-106">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="ed811-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ed811-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ed811-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="ed811-108">HDInsight 3.4 nebo větší, projděte si téma [spouštění dotazů Hive v zobrazení Ambari Hive](hdinsight-hadoop-use-hive-ambari-view.md) informace o spouštění dotazů Hive z webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ed811-108">For HDInsight 3.4 or greater, see [Run Hive queries in Ambari Hive View](hdinsight-hadoop-use-hive-ambari-view.md) for information on running Hive queries from a web browser.</span></span>

## <span data-ttu-id="ed811-109"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="ed811-109"><a id="prereq"></a>Prerequisites</span></span>
<span data-ttu-id="ed811-110">Pokud chcete provést kroky v tomto článku, budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="ed811-110">To complete the steps in this article, you will need the following.</span></span>

* <span data-ttu-id="ed811-111">Cluster HDInsight Hadoop založené na systému Windows</span><span class="sxs-lookup"><span data-stu-id="ed811-111">A Windows-based HDInsight Hadoop cluster</span></span>
* <span data-ttu-id="ed811-112">Moderní webový prohlížeč</span><span class="sxs-lookup"><span data-stu-id="ed811-112">A modern web browser</span></span>

## <span data-ttu-id="ed811-113"><a id="run"></a>Spouštění dotazů Hive pomocí konzole dotazu</span><span class="sxs-lookup"><span data-stu-id="ed811-113"><a id="run"></a> Run Hive queries using the Query Console</span></span>
1. <span data-ttu-id="ed811-114">Otevřete webový prohlížeč a přejděte do **https://CLUSTERNAME.azurehdinsight.net**, kde **CLUSTERNAME** je název clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ed811-114">Open a web browser and navigate to **https://CLUSTERNAME.azurehdinsight.net**, where **CLUSTERNAME** is the name of your HDInsight cluster.</span></span> <span data-ttu-id="ed811-115">Pokud se zobrazí výzva, zadejte uživatelské jméno a heslo, které jste použili při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed811-115">If prompted, enter the user name and password that you used when you created the cluster.</span></span>
2. <span data-ttu-id="ed811-116">Odkazy v horní části stránky, vyberte **Hive Editor**.</span><span class="sxs-lookup"><span data-stu-id="ed811-116">From the links at the top of the page, select **Hive Editor**.</span></span> <span data-ttu-id="ed811-117">Zobrazí formulář, který slouží k zadání HiveQL příkazy, které chcete spustit v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ed811-117">This displays a form that can be used to enter the HiveQL statements that you want to run in the HDInsight cluster.</span></span>

    ![hive editor](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    <span data-ttu-id="ed811-119">Nahraďte text `Select * from hivesampletable` s následující příkazy HiveQL:</span><span class="sxs-lookup"><span data-stu-id="ed811-119">Replace the text `Select * from hivesampletable` with the following HiveQL statements:</span></span>

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="ed811-120">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="ed811-120">These statements perform the following actions:</span></span>

   * <span data-ttu-id="ed811-121">**VYŘAĎTE tabulku**: Odstraní tabulku a datový soubor, pokud tabulka již existuje.</span><span class="sxs-lookup"><span data-stu-id="ed811-121">**DROP TABLE**: Deletes the table and the data file if the table already exists.</span></span>
   * <span data-ttu-id="ed811-122">**Vytvoření externí tabulky**: vytvoří novou tabulku "externí" v Hive.</span><span class="sxs-lookup"><span data-stu-id="ed811-122">**CREATE EXTERNAL TABLE**: Creates a new 'external' table in Hive.</span></span> <span data-ttu-id="ed811-123">Externí tabulky uložit pouze definici tabulky Hive; data je ponechán v původním umístění.</span><span class="sxs-lookup"><span data-stu-id="ed811-123">External tables store only the table definition in Hive; the data is left in the original location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="ed811-124">Externí tabulky by měl být použit při očekáváte, že v základních datech aktualizovat externího zdroje (například procesu nahrávání automatizované dat), nebo jiná operace MapReduce, ale chcete, aby dotazy Hive používat nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="ed811-124">External tables should be used when you expect the underlying data to be updated by an external source (such as an automated data upload process) or by another MapReduce operation, but you always want Hive queries to use the latest data.</span></span>
     >
     > <span data-ttu-id="ed811-125">Vyřazení externí tabulku nemá **není** odstranit data, jenom definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="ed811-125">Dropping an external table does **not** delete the data, only the table definition.</span></span>
     >
     >
   * <span data-ttu-id="ed811-126">**Řádek formátu**: informuje Hive formátování data.</span><span class="sxs-lookup"><span data-stu-id="ed811-126">**ROW FORMAT**: Tells Hive how the data is formatted.</span></span> <span data-ttu-id="ed811-127">V takovém případě polí v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="ed811-127">In this case, the fields in each log are separated by a space.</span></span>
   * <span data-ttu-id="ed811-128">**ULOŽENÉ umístění textový soubor AS**: informuje Hive, kde je data uložená (adresář, příklad nebo data) a která je uložena jako text</span><span class="sxs-lookup"><span data-stu-id="ed811-128">**STORED AS TEXTFILE LOCATION**: Tells Hive where the data is stored (the example/data directory) and that it is stored as text</span></span>
   * <span data-ttu-id="ed811-129">**Vyberte**: Vyberte počet všech řádků kde sloupec **t4** obsahovat hodnotu **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="ed811-129">**SELECT**: Select a count of all rows where column **t4** contain the value **[ERROR]**.</span></span> <span data-ttu-id="ed811-130">To by měla vrátit hodnotu **3** vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ed811-130">This should return a value of **3** because there are three rows that contain this value.</span></span>
   * <span data-ttu-id="ed811-131">**INPUT__FILE__NAME jako '%.log'** -informuje Hive, který jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="ed811-131">**INPUT__FILE__NAME LIKE '%.log'** - Tells Hive that we should only return data from files ending in .log.</span></span> <span data-ttu-id="ed811-132">To brání hledání k sample.log souboru, který obsahuje data a udržuje ho z vrací data z jiných příkladu datové soubory, které neodpovídají schématu, které jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="ed811-132">This restricts the search to the sample.log file that contains the data, and keeps it from returning data from other example data files that do not match the schema we defined.</span></span>
3. <span data-ttu-id="ed811-133">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="ed811-133">Click **Submit**.</span></span> <span data-ttu-id="ed811-134">**Úlohy relace** v dolní části stránky by měl zobrazit podrobnosti pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="ed811-134">The **Job Session** at the bottom of the page should display details for the job.</span></span>
4. <span data-ttu-id="ed811-135">Když **stav** pole změny **dokončeno**, vyberte **zobrazit podrobnosti** pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="ed811-135">When the **Status** field changes to **Completed**, select **View Details** for the job.</span></span> <span data-ttu-id="ed811-136">Na stránce podrobností **výstup úlohy** obsahuje `[ERROR]    3`.</span><span class="sxs-lookup"><span data-stu-id="ed811-136">On the details page, the **Job Output** contains `[ERROR]    3`.</span></span> <span data-ttu-id="ed811-137">Můžete použít **Stáhnout** tlačítko v tomto poli ke stažení souboru, který obsahuje výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="ed811-137">You can use the **Download** button under this field to download a file that contains the output of the job.</span></span>

## <span data-ttu-id="ed811-138"><a id="summary"></a>Shrnutí</span><span class="sxs-lookup"><span data-stu-id="ed811-138"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="ed811-139">Jak vidíte, konzola dotazu poskytuje snadný způsob, jak spouštět dotazy Hive v clusteru služby HDInsight, monitorovat stav úlohy a načíst výstup.</span><span class="sxs-lookup"><span data-stu-id="ed811-139">As you can see, the Query Console provides an easy way to run Hive queries in an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

<span data-ttu-id="ed811-140">Další informace o použití konzoly dotaz Hive ke spuštění úloh Hive, vyberte **Začínáme** v horní části konzoly dotazu, pak použít vzorků, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ed811-140">To learn more about using Hive Query Console to run Hive jobs, select **Getting Started** at the top of the Query Console, then use the samples that are provided.</span></span> <span data-ttu-id="ed811-141">Každá ukázka vás provede procesem použití Hive k analýze dat, včetně vysvětlení o příkazy HiveQL použít ve vzorku.</span><span class="sxs-lookup"><span data-stu-id="ed811-141">Each sample walks through the process of using Hive to analyze data, including explanations about the HiveQL statements used in the sample.</span></span>

## <span data-ttu-id="ed811-142"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed811-142"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="ed811-143">Obecné informace o Hive v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ed811-143">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="ed811-144">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ed811-144">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="ed811-145">Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ed811-145">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="ed811-146">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ed811-146">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ed811-147">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ed811-147">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="ed811-148">Pokud používáte s Hive Tez, najdete v následujících dokumentech pro ladění informace:</span><span class="sxs-lookup"><span data-stu-id="ed811-148">If you are using Tez with Hive, see the following documents for debugging information:</span></span>

* [<span data-ttu-id="ed811-149">Pomocí uživatelského rozhraní Tez na HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="ed811-149">Use the Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="ed811-150">Použití zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="ed811-150">Use the Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

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
