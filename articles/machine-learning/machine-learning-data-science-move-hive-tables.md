---
title: "Vytváření tabulek Hive a načtení dat z Azure Blob Storage | Microsoft Docs"
description: "Vytváření tabulek Hive a načtení dat do objektu blob do tabulek hive"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: cff9280d-18ce-4b66-a54f-19f358d1ad90
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: eca4ecd8f639bb9816903f4b1d1f999755da819c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a><span data-ttu-id="2ee77-103">Vytváření tabulek Hive a načtení dat z Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="2ee77-103">Create Hive tables and load data from Azure Blob Storage</span></span>
<span data-ttu-id="2ee77-104">Toto téma představuje obecné dotazů Hive, které vytváření tabulek Hive a načtení dat z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="2ee77-104">This topic presents generic Hive queries that create Hive tables and load data from Azure blob storage.</span></span> <span data-ttu-id="2ee77-105">Některé pokyny jsou tu taky o dělení tabulek Hive a o používání optimalizované řádek sloupcovém (ORC) formátování pro zlepšení výkonu dotazů.</span><span class="sxs-lookup"><span data-stu-id="2ee77-105">Some guidance is also provided on partitioning Hive tables and on using the Optimized Row Columnar (ORC) formatting to improve query performance.</span></span>

<span data-ttu-id="2ee77-106">To **nabídky** odkazy na témata, které popisují, jak ingestovat data do cílové prostředí, kde můžete data ukládat a zpracovávat během tým datové vědy procesu (TDSP).</span><span class="sxs-lookup"><span data-stu-id="2ee77-106">This **menu** links to topics that describe how to ingest data into target environments where the data can be stored and processed during the Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="2ee77-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2ee77-107">Prerequisites</span></span>
<span data-ttu-id="2ee77-108">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="2ee77-108">This article assumes that you have:</span></span>

* <span data-ttu-id="2ee77-109">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="2ee77-109">Created an Azure storage account.</span></span> <span data-ttu-id="2ee77-110">Pokud budete potřebovat pokyny, najdete v části [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="2ee77-110">If you need instructions, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
* <span data-ttu-id="2ee77-111">Zřizuje přizpůsobené clusteru Hadoop se službou HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2ee77-111">Provisioned a customized Hadoop cluster with the HDInsight service.</span></span>  <span data-ttu-id="2ee77-112">Pokud budete potřebovat pokyny, najdete v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilou analýzu](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="2ee77-112">If you need instructions, see [Customize Azure HDInsight Hadoop clusters for advanced analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="2ee77-113">Povoleno pro vzdálený přístup ke clusteru, přihlášení a otevřít konzolu příkazového řádku Hadoop.</span><span class="sxs-lookup"><span data-stu-id="2ee77-113">Enabled remote access to the cluster, logged in, and opened the Hadoop Command-Line console.</span></span> <span data-ttu-id="2ee77-114">Pokud budete potřebovat pokyny, najdete v části [přístup hlavního uzlu Hadoop clusteru](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="2ee77-114">If you need instructions, see [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <a name="upload-data-to-azure-blob-storage"></a><span data-ttu-id="2ee77-115">Nahrání dat do Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="2ee77-115">Upload data to Azure blob storage</span></span>
<span data-ttu-id="2ee77-116">Pokud jste vytvořili virtuální počítač Azure podle pokynů uvedených v [nastavení virtuálního počítače Azure pro pokročilou analýzu](machine-learning-data-science-setup-virtual-machine.md), tento soubor skriptu by byly staženy *C:\\uživatelů \\ \<uživatelské jméno\>\\dokumenty\\datové vědy skripty* adresář na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="2ee77-116">If you created an Azure virtual machine by following the instructions provided in [Set up an Azure virtual machine for advanced analytics](machine-learning-data-science-setup-virtual-machine.md), this script file should have been downloaded to the *C:\\Users\\\<user name\>\\Documents\\Data Science Scripts* directory on the virtual machine.</span></span> <span data-ttu-id="2ee77-117">Tyto dotazy Hive vyžadují jenom zařaďte vlastní schéma dat a konfigurace úložiště objektů blob Azure do příslušných polí bude připravená k odeslání.</span><span class="sxs-lookup"><span data-stu-id="2ee77-117">These Hive queries only require that you plug in your own data schema and Azure blob storage configuration in the appropriate fields to be ready for submission.</span></span>

<span data-ttu-id="2ee77-118">Předpokládáme, že se data pro tabulek Hive v **nekomprimované** tabulkovém formátu, a že data byl odeslán na výchozí hodnoty (nebo dalších) kontejneru účtu úložiště používaný v clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="2ee77-118">We assume that the data for Hive tables is in an **uncompressed** tabular format, and that the data has been uploaded to the default (or to an additional) container of the storage account used by the Hadoop cluster.</span></span>

<span data-ttu-id="2ee77-119">Pokud chcete postupem na **NYC taxíkem cestě Data**, budete muset:</span><span class="sxs-lookup"><span data-stu-id="2ee77-119">If you want to practice on the **NYC Taxi Trip Data**, you need to:</span></span>

* <span data-ttu-id="2ee77-120">**Stáhněte si** 24 [NYC taxíkem cestě Data](http://www.andresmh.com/nyctaxitrips) soubory (12 cestě soubory a soubory 12 tarif),</span><span class="sxs-lookup"><span data-stu-id="2ee77-120">**download** the 24 [NYC Taxi Trip Data](http://www.andresmh.com/nyctaxitrips) files (12 Trip files and 12 Fare files),</span></span>
* <span data-ttu-id="2ee77-121">**Rozbalte** všechny soubory do souborů .csv a potom</span><span class="sxs-lookup"><span data-stu-id="2ee77-121">**unzip** all files into .csv files, and then</span></span>
* <span data-ttu-id="2ee77-122">**Nahrát** je výchozí (nebo odpovídajícího kontejneru) účet úložiště Azure, který byl vytvořen pomocí pokynů uvedených v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilé analýzy proces a technologie](machine-learning-data-science-customize-hadoop-cluster.md)tématu.</span><span class="sxs-lookup"><span data-stu-id="2ee77-122">**upload** them to the default (or appropriate container) of the Azure storage account that was created by the procedure outlined in the [Customize Azure HDInsight Hadoop clusters for Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md) topic.</span></span> <span data-ttu-id="2ee77-123">Postup nahrání souborů .csv do výchozího kontejneru v účtu úložiště najdete v tomto [stránky](machine-learning-data-science-process-hive-walkthrough.md#upload).</span><span class="sxs-lookup"><span data-stu-id="2ee77-123">The process to upload the .csv files to the default container on the storage account can be found on this [page](machine-learning-data-science-process-hive-walkthrough.md#upload).</span></span>

## <span data-ttu-id="2ee77-124"><a name="submit"></a>Postup odesílání dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="2ee77-124"><a name="submit"></a>How to submit Hive queries</span></span>
<span data-ttu-id="2ee77-125">Dotazy Hive můžete odeslat pomocí:</span><span class="sxs-lookup"><span data-stu-id="2ee77-125">Hive queries can be submitted by using:</span></span>

1. [<span data-ttu-id="2ee77-126">Odesílání dotazů Hive prostřednictvím Hadoop příkazového řádku v headnode clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="2ee77-126">Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>](#headnode)
2. [<span data-ttu-id="2ee77-127">Odesílání dotazů Hive pomocí editoru Hive</span><span class="sxs-lookup"><span data-stu-id="2ee77-127">Submit Hive queries with the Hive Editor</span></span>](#hive-editor)
3. [<span data-ttu-id="2ee77-128">Odesílání dotazů Hive pomocí příkazů prostředí PowerShell Azure</span><span class="sxs-lookup"><span data-stu-id="2ee77-128">Submit Hive queries with Azure PowerShell Commands</span></span>](#ps)

<span data-ttu-id="2ee77-129">Dotazy Hive jsou podobné jazyku SQL.</span><span class="sxs-lookup"><span data-stu-id="2ee77-129">Hive queries are SQL-like.</span></span> <span data-ttu-id="2ee77-130">Pokud jste obeznámeni s SQL, můžete zjistit [Hive pro SQL uživatelé cheaty list](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) užitečné.</span><span class="sxs-lookup"><span data-stu-id="2ee77-130">If you are familiar with SQL, you may find the [Hive for SQL Users Cheat Sheet](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) useful.</span></span>

<span data-ttu-id="2ee77-131">Při odesílání dotazů Hive, můžete také ovládat cíl výstupu z dotazů Hive, ať to na obrazovce nebo do místního souboru z hlavního uzlu nebo do objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="2ee77-131">When submitting a Hive query, you can also control the destination of the output from Hive queries, whether it be on the screen or to a local file on the head node or to an Azure blob.</span></span>

### <span data-ttu-id="2ee77-132"><a name="headnode"></a> 1. Odesílání dotazů Hive prostřednictvím Hadoop příkazového řádku v headnode clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="2ee77-132"><a name="headnode"></a> 1. Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>
<span data-ttu-id="2ee77-133">Pokud se dotaz Hive komplexní, odesílání přímo v hlavního uzlu hadoop clusteru obvykle vede k rychlejší otáčení, než odesílání pomocí editoru Hive nebo Azure PowerShell skriptů.</span><span class="sxs-lookup"><span data-stu-id="2ee77-133">If the Hive query is complex, submitting it directly in the head node of the Hadoop cluster typically leads to faster turn around than submitting it with a Hive Editor or Azure PowerShell scripts.</span></span>

<span data-ttu-id="2ee77-134">Přihlaste se k hlavnímu uzlu clusteru Hadoop, otevřete příkazový řádek Hadoop na ploše hlavního uzlu a zadejte příkaz `cd %hive_home%\bin`.</span><span class="sxs-lookup"><span data-stu-id="2ee77-134">Log in to the head node of the Hadoop cluster, open the Hadoop Command Line on the desktop of the head node, and enter command `cd %hive_home%\bin`.</span></span>

<span data-ttu-id="2ee77-135">Máte tři způsoby, jak odesílání dotazů Hive Hadoop příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="2ee77-135">You have three ways to submit Hive queries in the Hadoop Command Line:</span></span>

* <span data-ttu-id="2ee77-136">přímo</span><span class="sxs-lookup"><span data-stu-id="2ee77-136">directly</span></span>
* <span data-ttu-id="2ee77-137">pomocí souborů .hql</span><span class="sxs-lookup"><span data-stu-id="2ee77-137">using .hql files</span></span>
* <span data-ttu-id="2ee77-138">s konzolou příkaz Hive</span><span class="sxs-lookup"><span data-stu-id="2ee77-138">with the Hive command console</span></span>

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a><span data-ttu-id="2ee77-139">Odesílání dotazů Hive přímo v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2ee77-139">Submit Hive queries directly in Hadoop Command Line.</span></span>
<span data-ttu-id="2ee77-140">Můžete spustit příkaz jako `hive -e "<your hive query>;` k odesílání dotazů Hive jednoduché přímo v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2ee77-140">You can run command like `hive -e "<your hive query>;` to submit simple Hive queries directly in Hadoop Command Line.</span></span> <span data-ttu-id="2ee77-141">Tady je příklad, kde červeným rámečkem popisuje příkaz, který odešle dotaz Hive, a popisuje zelené pole výstup dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-141">Here is an example, where the red box outlines the command that submits the Hive query, and the green box outlines the output from the Hive query.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a><span data-ttu-id="2ee77-143">Odesílání dotazů Hive v souborech .hql</span><span class="sxs-lookup"><span data-stu-id="2ee77-143">Submit Hive queries in .hql files</span></span>
<span data-ttu-id="2ee77-144">Pokud dotaz Hive je složitější a má více řádků, není praktické úpravy dotazů v příkazovém řádku nebo konzoly příkazu Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-144">When the Hive query is more complicated and has multiple lines, editing queries in command line or Hive command console is not practical.</span></span> <span data-ttu-id="2ee77-145">Alternativou je uložit do souboru .hql do místního adresáře hlavního uzlu dotazů Hive pomocí textového editoru v hlavního uzlu clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="2ee77-145">An alternative is to use a text editor in the head node of the Hadoop cluster to save the Hive queries in a .hql file in a local directory of the head node.</span></span> <span data-ttu-id="2ee77-146">Potom pomocí jde odeslat dotaz Hive v souboru .hql `-f` argument následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2ee77-146">Then the Hive query in the .hql file can be submitted by using the `-f` argument as follows:</span></span>

    hive -f "<path to the .hql file>"

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)

<span data-ttu-id="2ee77-148">**Potlačit tisk obrazovky stav průběhu dotazů Hive**</span><span class="sxs-lookup"><span data-stu-id="2ee77-148">**Suppress progress status screen print of Hive queries**</span></span>

<span data-ttu-id="2ee77-149">Ve výchozím nastavení po odeslání dotazu Hive v Hadoop příkazového řádku, průběh úlohy mapy nebo snižte se vytiskne na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="2ee77-149">By default, after Hive query is submitted in Hadoop Command Line, the progress of the Map/Reduce job is printed out on screen.</span></span> <span data-ttu-id="2ee77-150">Chcete-li potlačit print obrazovky průběh úlohy mapy nebo snižte, můžete použít argument `-S` ("S velkými písmeny) v příkazu řádek následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2ee77-150">To suppress the screen print of the Map/Reduce job progress, you can use an argument `-S` ("S" in upper case) in the command line as follows:</span></span>

    hive -S -f "<path to the .hql file>"
<span data-ttu-id="2ee77-151">.</span><span class="sxs-lookup"><span data-stu-id="2ee77-151">.</span></span>    <span data-ttu-id="2ee77-152">Hive -S -e "<Hive queries>"</span><span class="sxs-lookup"><span data-stu-id="2ee77-152">hive -S -e "<Hive queries>"</span></span>

#### <a name="submit-hive-queries-in-hive-command-console"></a><span data-ttu-id="2ee77-153">Odesílání dotazů Hive v konzole příkaz Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-153">Submit Hive queries in Hive command console.</span></span>
<span data-ttu-id="2ee77-154">Příkaz konzole Hive můžete zadat také nejprve spuštěním příkazu `hive` v Hadoop příkazového řádku a pak odesílání dotazů Hive v konzole příkaz Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-154">You can also first enter the Hive command console by running command `hive` in Hadoop Command Line, and then submit Hive queries in Hive command console.</span></span> <span data-ttu-id="2ee77-155">Tady je příklad.</span><span class="sxs-lookup"><span data-stu-id="2ee77-155">Here is an example.</span></span> <span data-ttu-id="2ee77-156">V tomto příkladu zvýrazněte dva červená pole příkazy používané k zadání Hive konzoly příkazu a dotaz Hive odeslání v konzole příkaz Hive v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2ee77-156">In this example, the two red boxes highlight the commands used to enter the Hive command console, and the Hive query submitted in Hive command console, respectively.</span></span> <span data-ttu-id="2ee77-157">Pole zelená označuje výstup dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-157">The green box highlights the output from the Hive query.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

<span data-ttu-id="2ee77-159">V předchozích příkladech přímo výstupu podregistru výsledky dotazu na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="2ee77-159">The previous examples directly output the Hive query results on screen.</span></span> <span data-ttu-id="2ee77-160">Může také zapisovat výstup do místního souboru z hlavního uzlu, nebo do objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="2ee77-160">You can also write the output to a local file on the head node, or to an Azure blob.</span></span> <span data-ttu-id="2ee77-161">Potom můžete dalších nástrojů k další analýze výstup dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-161">Then, you can use other tools to further analyze the output of Hive queries.</span></span>

<span data-ttu-id="2ee77-162">**Výstup výsledků dotazu Hive do místního souboru.**</span><span class="sxs-lookup"><span data-stu-id="2ee77-162">**Output Hive query results to a local file.**</span></span>
<span data-ttu-id="2ee77-163">K vypsání výsledků dotazu Hive do místního adresáře z hlavního uzlu, budete muset odeslat dotaz Hive v příkazovém řádku Hadoop následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2ee77-163">To output Hive query results to a local directory on the head node, you have to submit the Hive query in the Hadoop Command Line as follows:</span></span>

    hive -e "<hive query>" > <local path in the head node>

<span data-ttu-id="2ee77-164">V následujícím příkladu výstupu podregistru dotazu zápisu do souboru `hivequeryoutput.txt` v adresáři `C:\apps\temp`.</span><span class="sxs-lookup"><span data-stu-id="2ee77-164">In the following example, the output of Hive query is written into a file `hivequeryoutput.txt` in directory `C:\apps\temp`.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

<span data-ttu-id="2ee77-166">**Výsledky dotazu výstupu podregistru do objektu blob Azure**</span><span class="sxs-lookup"><span data-stu-id="2ee77-166">**Output Hive query results to an Azure blob**</span></span>

<span data-ttu-id="2ee77-167">Můžete také výstup výsledků dotazu Hive do objektu blob Azure, v rámci výchozího kontejneru clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="2ee77-167">You can also output the Hive query results to an Azure blob, within the default container of the Hadoop cluster.</span></span> <span data-ttu-id="2ee77-168">Pro tento dotaz Hive vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2ee77-168">The Hive query for this is as follows:</span></span>

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

<span data-ttu-id="2ee77-169">V následujícím příkladu výstupu podregistru dotazu je zapsán do adresáře, objektů blob `queryoutputdir` v rámci výchozího kontejneru clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="2ee77-169">In the following example, the output of Hive query is written to a blob directory `queryoutputdir` within the default container of the Hadoop cluster.</span></span> <span data-ttu-id="2ee77-170">Zde stačí zadat název adresáře, bez názvu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="2ee77-170">Here, you only need to provide the directory name, without the blob name.</span></span> <span data-ttu-id="2ee77-171">Pokud například zadáte názvy adresáře a objektů blob, je vyvolána chyba `wasb:///queryoutputdir/queryoutput.txt`.</span><span class="sxs-lookup"><span data-stu-id="2ee77-171">An error is thrown if you provide both directory and blob names, such as `wasb:///queryoutputdir/queryoutput.txt`.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

<span data-ttu-id="2ee77-173">Pokud otevřete výchozí kontejner clusteru Hadoop pomocí Průzkumníka úložiště Azure, uvidíte výstup dotaz Hive, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="2ee77-173">If you open the default container of the Hadoop cluster using Azure Storage Explorer, you can see the output of the Hive query as shown in the following figure.</span></span> <span data-ttu-id="2ee77-174">Filtr (zvýrazněné podle červeným rámečkem) můžete použít pouze načíst objekt blob se zadaná písmena v názvech.</span><span class="sxs-lookup"><span data-stu-id="2ee77-174">You can apply the filter (highlighted by red box) to only retrieve the blob with specified letters in names.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

### <span data-ttu-id="2ee77-176"><a name="hive-editor"></a> 2. Odesílání dotazů Hive pomocí editoru Hive</span><span class="sxs-lookup"><span data-stu-id="2ee77-176"><a name="hive-editor"></a> 2. Submit Hive queries with the Hive Editor</span></span>
<span data-ttu-id="2ee77-177">Můžete také použít konzolu dotazu (Hive Editor) tak, že zadáte adresu URL ve formátu *https://&#60; Název clusteru Hadoop >.azurehdinsight.net/Home/HiveEditor* do webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2ee77-177">You can also use the Query Console (Hive Editor) by entering a URL of the form *https://&#60;Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor* into a web browser.</span></span> <span data-ttu-id="2ee77-178">Musí být zaznamenána v najdete v této konzoly a proto je třeba zde pověření clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="2ee77-178">You must be logged in the see this console and so you need your Hadoop cluster credentials here.</span></span>

### <span data-ttu-id="2ee77-179"><a name="ps"></a> 3. Odesílání dotazů Hive pomocí příkazů prostředí PowerShell Azure</span><span class="sxs-lookup"><span data-stu-id="2ee77-179"><a name="ps"></a> 3. Submit Hive queries with Azure PowerShell Commands</span></span>
<span data-ttu-id="2ee77-180">Můžete také použít PowerShell k odesílání dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-180">You can also use PowerShell to submit Hive queries.</span></span> <span data-ttu-id="2ee77-181">Pokyny najdete v tématu [úlohy odeslání Hive pomocí prostředí PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2ee77-181">For instructions, see [Submit Hive jobs using PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span></span>

## <span data-ttu-id="2ee77-182"><a name="create-tables"></a>Vytvoření databáze Hive a tabulky</span><span class="sxs-lookup"><span data-stu-id="2ee77-182"><a name="create-tables"></a>Create Hive database and tables</span></span>
<span data-ttu-id="2ee77-183">Sdílené dotazy Hive v [úložiště GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) a z ní si můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="2ee77-183">The Hive queries are shared in the [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) and can be downloaded from there.</span></span>

<span data-ttu-id="2ee77-184">Zde je dotaz Hive, který vytvoří tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-184">Here is the Hive query that creates a Hive table.</span></span>

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

<span data-ttu-id="2ee77-185">Zde je uveden popis pole, která je potřeba zařadit a další konfigurace:</span><span class="sxs-lookup"><span data-stu-id="2ee77-185">Here are the descriptions of the fields that you need to plug in and other configurations:</span></span>

* <span data-ttu-id="2ee77-186">**& č. 60; název databáze >**: název databáze, kterou chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="2ee77-186">**&#60;database name>**: the name of the database that you want to create.</span></span> <span data-ttu-id="2ee77-187">Pokud chcete použít výchozí databázi, dotaz *vytvoření databáze...*  lze vynechat.</span><span class="sxs-lookup"><span data-stu-id="2ee77-187">If you just want to use the default database, the query *create database...* can be omitted.</span></span>
* <span data-ttu-id="2ee77-188">**& č. 60; název tabulky >**: název tabulky, který chcete vytvořit v rámci zadaná databáze.</span><span class="sxs-lookup"><span data-stu-id="2ee77-188">**&#60;table name>**: the name of the table that you want to create within the specified database.</span></span> <span data-ttu-id="2ee77-189">Pokud chcete použít výchozí databáze, v tabulce může být přímo na které odkazuje *& č. 60; název tabulky >* bez & č. 60; název databáze >.</span><span class="sxs-lookup"><span data-stu-id="2ee77-189">If you want to use the default database, the table can be directly referred by *&#60;table name>* without &#60;database name>.</span></span>
* <span data-ttu-id="2ee77-190">**& č. 60; oddělovač polí >**: oddělovač, který vymezuje pole v datovém souboru k odeslání na tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-190">**&#60;field separator>**: the separator that delimits fields in the data file to be uploaded to the Hive table.</span></span>
* <span data-ttu-id="2ee77-191">**& č. 60; oddělovací čáry >**: oddělovač, který vymezuje řádků v datovém souboru.</span><span class="sxs-lookup"><span data-stu-id="2ee77-191">**&#60;line separator>**: the separator that delimits lines in the data file.</span></span>
* <span data-ttu-id="2ee77-192">**& č. 60; umístění úložiště >**: umístění úložiště Azure pro uložení dat tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-192">**&#60;storage location>**: the Azure storage location to save the data of Hive tables.</span></span> <span data-ttu-id="2ee77-193">Pokud nezadáte *umístění & č. 60; umístění úložiště >*, databáze a tabulky jsou uloženy v *hive neboskladu/* adresář ve výchozím kontejneru Hive clusteru ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2ee77-193">If you do not specify *LOCATION &#60;storage location>*, the database and the tables are stored in *hive/warehouse/* directory in the default container of the Hive cluster by default.</span></span> <span data-ttu-id="2ee77-194">Pokud chcete zadat umístění úložiště, musí být v rámci výchozího kontejneru databáze a tabulky umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ee77-194">If you want to specify the storage location, the storage location has to be within the default container for the database and tables.</span></span> <span data-ttu-id="2ee77-195">Toto umístění musí být označuje jako umístění relativně k výchozí kontejner clusteru ve formátu *' wasb: / / / & č. 60; adresář 1 > / "* nebo *' wasb: / / / & č. 60; adresář 1 > / & č. 60; adresář 2 > nebo "*atd. Po provedení dotazu relativní adresáře jsou vytvořeny v rámci výchozího kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2ee77-195">This location has to be referred as location relative to the default container of the cluster in the format of *'wasb:///&#60;directory 1>/'* or *'wasb:///&#60;directory 1>/&#60;directory 2>/'*, etc. After the query is executed, the relative directories are created within the default container.</span></span>
* <span data-ttu-id="2ee77-196">**TBLPROPERTIES("Skip.Header.line.Count"="1")**: Pokud datový soubor obsahuje řádek záhlaví, budete muset přidat tato vlastnost **na konci** z *vytvořit tabulku* dotazu.</span><span class="sxs-lookup"><span data-stu-id="2ee77-196">**TBLPROPERTIES("skip.header.line.count"="1")**: If the data file has a header line, you have to add this property **at the end** of the *create table* query.</span></span> <span data-ttu-id="2ee77-197">Řádek záhlaví, jinak je načten jako záznamu do tabulky.</span><span class="sxs-lookup"><span data-stu-id="2ee77-197">Otherwise, the header line is loaded as a record to the table.</span></span> <span data-ttu-id="2ee77-198">Pokud datový soubor nemá řádek záhlaví, můžete tuto konfiguraci vynechat v dotazu.</span><span class="sxs-lookup"><span data-stu-id="2ee77-198">If the data file does not have a header line, this configuration can be omitted in the query.</span></span>

## <span data-ttu-id="2ee77-199"><a name="load-data"></a>Načtení dat do tabulek Hive</span><span class="sxs-lookup"><span data-stu-id="2ee77-199"><a name="load-data"></a>Load data to Hive tables</span></span>
<span data-ttu-id="2ee77-200">Zde je dotaz Hive, který načítá data do tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="2ee77-200">Here is the Hive query that loads data into a Hive table.</span></span>

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

* <span data-ttu-id="2ee77-201">**& č. 60; cesta k datům objektu blob >**: Pokud je objekt blob soubor k odeslání do tabulky Hive ve výchozím kontejneru clusteru HDInsight Hadoop *& č. 60; cesta k datům objektu blob >* by měl být ve formátu *. wasb: / / / & č. 60; adresář v tomto kontejneru > / & č. 60; název souboru objektů blob >'*.</span><span class="sxs-lookup"><span data-stu-id="2ee77-201">**&#60;path to blob data>**: If the blob file to be uploaded to the Hive table is in the default container of the HDInsight Hadoop cluster, the *&#60;path to blob data>* should be in the format *'wasb:///&#60;directory in this container>/&#60;blob file name>'*.</span></span> <span data-ttu-id="2ee77-202">Soubor blob může být také v další kontejner clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="2ee77-202">The blob file can also be in an additional container of the HDInsight Hadoop cluster.</span></span> <span data-ttu-id="2ee77-203">V takovém případě *& č. 60; cesta k datům objektu blob >* by měl být ve formátu *' wasb: / / & č. 60; název kontejneru > @& č. 60; název účtu úložiště >.blob.core.windows.net/ & č. 60; název souboru objektů blob >'*.</span><span class="sxs-lookup"><span data-stu-id="2ee77-203">In this case, *&#60;path to blob data>* should be in the format *'wasb://&#60;container name>@&#60;storage account name>.blob.core.windows.net/&#60;blob file name>'*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="2ee77-204">Data objektů blob k odeslání na tabulku Hive musí být ve výchozím nastavení nebo další kontejneru účtu úložiště pro Hadoop cluster.</span><span class="sxs-lookup"><span data-stu-id="2ee77-204">The blob data to be uploaded to Hive table has to be in the default or additional container of the storage account for the Hadoop cluster.</span></span> <span data-ttu-id="2ee77-205">V opačném *načítání dat* nesouhlasících, že nelze přístup k datům se dotaz nezdaří.</span><span class="sxs-lookup"><span data-stu-id="2ee77-205">Otherwise, the *LOAD DATA* query fails complaining that it cannot access the data.</span></span>
  >
  >

## <span data-ttu-id="2ee77-206"><a name="partition-orc"></a>Rozšířené témata: tabulka a úložiště Hive data ve formátu ORC na oddíly</span><span class="sxs-lookup"><span data-stu-id="2ee77-206"><a name="partition-orc"></a>Advanced topics: partitioned table and store Hive data in ORC format</span></span>
<span data-ttu-id="2ee77-207">Pokud data velká, vytváření oddílů v tabulce je výhodné pro dotazy, které stačí ke kontrole několik oddílů tabulky.</span><span class="sxs-lookup"><span data-stu-id="2ee77-207">If the data is large, partitioning the table is beneficial for queries that only need to scan a few partitions of the table.</span></span> <span data-ttu-id="2ee77-208">Například je možné logicky oddílu data protokolu webu pomocí kalendářní data.</span><span class="sxs-lookup"><span data-stu-id="2ee77-208">For instance, it is reasonable to partition the log data of a web site by dates.</span></span>

<span data-ttu-id="2ee77-209">Kromě vytváření oddílů tabulek Hive, je také užitečné k ukládání dat Hive ve formátu optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="2ee77-209">In addition to partitioning Hive tables, it is also beneficial to store the Hive data in the Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="2ee77-210">Další informace o ORC formátování naleznete v tématu <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">a souborů ORC pomocí zvyšuje výkon při čtení, zápisu a zpracování dat Hive</a>.</span><span class="sxs-lookup"><span data-stu-id="2ee77-210">For more information on ORC formatting, see <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">Using ORC files improves performance when Hive is reading, writing, and processing data</a>.</span></span>

### <a name="partitioned-table"></a><span data-ttu-id="2ee77-211">Dělenou tabulku</span><span class="sxs-lookup"><span data-stu-id="2ee77-211">Partitioned table</span></span>
<span data-ttu-id="2ee77-212">Zde je dotaz Hive, který vytvoří dělenou tabulku a načte data do ní.</span><span class="sxs-lookup"><span data-stu-id="2ee77-212">Here is the Hive query that creates a partitioned table and loads data into it.</span></span>

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

<span data-ttu-id="2ee77-213">Při dotazování dělené tabulky, doporučuje se přidejte podmínku oddílu v **začátku** z `where` klauzule jako to zvyšuje účinnost vyhledávání výrazně.</span><span class="sxs-lookup"><span data-stu-id="2ee77-213">When querying partitioned tables, it is recommended to add the partition condition in the **beginning** of the `where` clause as this improves the efficacy of searching significantly.</span></span>

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <span data-ttu-id="2ee77-214"><a name="orc"></a>Ukládání dat Hive ve formátu ORC</span><span class="sxs-lookup"><span data-stu-id="2ee77-214"><a name="orc"></a>Store Hive data in ORC format</span></span>
<span data-ttu-id="2ee77-215">Nelze načíst přímo dat z úložiště objektů blob do tabulek Hive, které je uložený ve formátu ORC.</span><span class="sxs-lookup"><span data-stu-id="2ee77-215">You cannot directly load data from blob storage into Hive tables that is stored in the ORC format.</span></span> <span data-ttu-id="2ee77-216">Tady jsou kroky, které nutné provádět k načtení dat z Azure objektů BLOB do tabulek Hive uložený ve formátu ORC.</span><span class="sxs-lookup"><span data-stu-id="2ee77-216">Here are the steps that the you need to take to load data from Azure blobs to Hive tables stored in ORC format.</span></span>

<span data-ttu-id="2ee77-217">Vytvoření externí tabulky **ULOŽENÝ textový soubor AS** a načtení dat z úložiště objektů blob do tabulky.</span><span class="sxs-lookup"><span data-stu-id="2ee77-217">Create an external table **STORED AS TEXTFILE** and load data from blob storage to the table.</span></span>

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

<span data-ttu-id="2ee77-218">Vytvoření interní tabulky se stejným schématem jako externí tabulky v kroku 1, se stejnou oddělovačem polí a ukládat Hive data ve formátu ORC.</span><span class="sxs-lookup"><span data-stu-id="2ee77-218">Create an internal table with the same schema as the external table in step 1, with the same field delimiter, and store the Hive data in the ORC format.</span></span>

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

<span data-ttu-id="2ee77-219">Vyberte data z externí tabulky v kroku 1 a vložit do tabulky ORC</span><span class="sxs-lookup"><span data-stu-id="2ee77-219">Select data from the external table in step 1 and insert into the ORC table</span></span>

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> <span data-ttu-id="2ee77-220">Pokud v tabulce textový soubor *& č. 60; název databáze >. & č. 60; název tabulky externí textový soubor >* má oddíly, v KROKU 3 `SELECT * FROM <database name>.<external textfile table name>` příkaz vybere proměnnou oddílu jako pole v sadě vrácená data.</span><span class="sxs-lookup"><span data-stu-id="2ee77-220">If the TEXTFILE table *&#60;database name>.&#60;external textfile table name>* has partitions, in STEP 3, the `SELECT * FROM <database name>.<external textfile table name>` command selects the partition variable as a field in the returned data set.</span></span> <span data-ttu-id="2ee77-221">Vkládání do *& č. 60; název databáze >. & č. 60; název tabulky ORC >* selže od *& č. 60; název databáze >. & č. 60; název tabulky ORC >* nemá proměnnou oddílu jako pole ve schématu tabulky.</span><span class="sxs-lookup"><span data-stu-id="2ee77-221">Inserting it into the *&#60;database name>.&#60;ORC table name>* fails since *&#60;database name>.&#60;ORC table name>* does not have the partition variable as a field in the table schema.</span></span> <span data-ttu-id="2ee77-222">V takovém případě budete muset konkrétně vyberte pole, která má být vložen do *& č. 60; název databáze >. & č. 60; název tabulky ORC >* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2ee77-222">In this case, you need to specifically select the fields to be inserted to *&#60;database name>.&#60;ORC table name>* as follows:</span></span>
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

<span data-ttu-id="2ee77-223">Je bezpečné vyřadit *& č. 60; název tabulky externí textový soubor >* při pomocí následujícího dotazu po všechna data byla vložena do *& č. 60; název databáze >. & č. 60; název tabulky ORC >*:</span><span class="sxs-lookup"><span data-stu-id="2ee77-223">It is safe to drop the *&#60;external textfile table name>* when using the following query after all data has been inserted into *&#60;database name>.&#60;ORC table name>*:</span></span>

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

<span data-ttu-id="2ee77-224">Až projdete tímto postupem, měli byste mít tabulku s daty ve formátu ORC připravené k použití.</span><span class="sxs-lookup"><span data-stu-id="2ee77-224">After following this procedure, you should have a table with data in the ORC format ready to use.</span></span>  
