---
title: "aaaCreate tabulek Hive a načtení dat z Azure Blob Storage | Microsoft Docs"
description: "Vytváření tabulek Hive a načtení dat do tabulky toohive objektu blob"
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
ms.openlocfilehash: 09622972bcac31c2971858393a8340f24e4b7390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a><span data-ttu-id="280a4-103">Vytváření tabulek Hive a načtení dat z Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="280a4-103">Create Hive tables and load data from Azure Blob Storage</span></span>
<span data-ttu-id="280a4-104">Toto téma představuje obecné dotazů Hive, které vytváření tabulek Hive a načtení dat z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="280a4-104">This topic presents generic Hive queries that create Hive tables and load data from Azure blob storage.</span></span> <span data-ttu-id="280a4-105">Některé pokyny jsou tu taky o dělení tabulek Hive a o používání výkon formátování tooimprove dotazů hello optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="280a4-105">Some guidance is also provided on partitioning Hive tables and on using hello Optimized Row Columnar (ORC) formatting tooimprove query performance.</span></span>

<span data-ttu-id="280a4-106">To **nabídky** odkazy tootopics, které popisují, jak tooingest data do cílové prostředí, kde můžete ukládat a zpracovávat během hello data hello tým datové vědy procesu (TDSP).</span><span class="sxs-lookup"><span data-stu-id="280a4-106">This **menu** links tootopics that describe how tooingest data into target environments where hello data can be stored and processed during hello Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="280a4-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="280a4-107">Prerequisites</span></span>
<span data-ttu-id="280a4-108">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="280a4-108">This article assumes that you have:</span></span>

* <span data-ttu-id="280a4-109">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="280a4-109">Created an Azure storage account.</span></span> <span data-ttu-id="280a4-110">Pokud budete potřebovat pokyny, najdete v části [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="280a4-110">If you need instructions, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
* <span data-ttu-id="280a4-111">Zřizuje přizpůsobené clusteru Hadoop s hello služba HDInsight.</span><span class="sxs-lookup"><span data-stu-id="280a4-111">Provisioned a customized Hadoop cluster with hello HDInsight service.</span></span>  <span data-ttu-id="280a4-112">Pokud budete potřebovat pokyny, najdete v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilou analýzu](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="280a4-112">If you need instructions, see [Customize Azure HDInsight Hadoop clusters for advanced analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="280a4-113">Clusteru toohello povoleno vzdáleného přístupu, přihlášení a otevřít konzolu příkazového řádku hello Hadoop.</span><span class="sxs-lookup"><span data-stu-id="280a4-113">Enabled remote access toohello cluster, logged in, and opened hello Hadoop Command-Line console.</span></span> <span data-ttu-id="280a4-114">Pokud budete potřebovat pokyny, najdete v části [přístup hello uzlu clusteru Hadoop Head](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="280a4-114">If you need instructions, see [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <a name="upload-data-tooazure-blob-storage"></a><span data-ttu-id="280a4-115">Nahrání dat tooAzure objektu blob úložiště</span><span class="sxs-lookup"><span data-stu-id="280a4-115">Upload data tooAzure blob storage</span></span>
<span data-ttu-id="280a4-116">Pokud jste vytvořili virtuální počítač Azure podle pokynů hello součástí [nastavení virtuálního počítače Azure pro pokročilou analýzu](machine-learning-data-science-setup-virtual-machine.md), tento soubor skriptu by musela být stažené toohello *C:\\ Uživatelé\\\<uživatelské jméno\>\\dokumenty\\datové vědy skripty* v hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="280a4-116">If you created an Azure virtual machine by following hello instructions provided in [Set up an Azure virtual machine for advanced analytics](machine-learning-data-science-setup-virtual-machine.md), this script file should have been downloaded toohello *C:\\Users\\\<user name\>\\Documents\\Data Science Scripts* directory on hello virtual machine.</span></span> <span data-ttu-id="280a4-117">Tyto dotazy Hive vyžadují jenom zařaďte vlastní schéma dat a konfigurace úložiště objektů blob v Azure v toobe odpovídající pole hello připravené k odeslání.</span><span class="sxs-lookup"><span data-stu-id="280a4-117">These Hive queries only require that you plug in your own data schema and Azure blob storage configuration in hello appropriate fields toobe ready for submission.</span></span>

<span data-ttu-id="280a4-118">Předpokládáme, že data hello tabulek Hive jsou v **nekomprimované** tabulkovém formátu, a aby byla hello data nahrané toohello výchozí (nebo další tooan) kontejneru účtu úložiště hello používá hello Hadoop cluster.</span><span class="sxs-lookup"><span data-stu-id="280a4-118">We assume that hello data for Hive tables is in an **uncompressed** tabular format, and that hello data has been uploaded toohello default (or tooan additional) container of hello storage account used by hello Hadoop cluster.</span></span>

<span data-ttu-id="280a4-119">Pokud chcete, aby toopractice na hello **NYC taxíkem cestě Data**, budete muset:</span><span class="sxs-lookup"><span data-stu-id="280a4-119">If you want toopractice on hello **NYC Taxi Trip Data**, you need to:</span></span>

* <span data-ttu-id="280a4-120">**Stáhněte si** hello 24 [NYC taxíkem cestě Data](http://www.andresmh.com/nyctaxitrips) soubory (12 cestě soubory a soubory 12 tarif),</span><span class="sxs-lookup"><span data-stu-id="280a4-120">**download** hello 24 [NYC Taxi Trip Data](http://www.andresmh.com/nyctaxitrips) files (12 Trip files and 12 Fare files),</span></span>
* <span data-ttu-id="280a4-121">**Rozbalte** všechny soubory do souborů .csv a potom</span><span class="sxs-lookup"><span data-stu-id="280a4-121">**unzip** all files into .csv files, and then</span></span>
* <span data-ttu-id="280a4-122">**Nahrát** je toohello výchozí (nebo odpovídajícího kontejneru) hello účtu úložiště Azure, který byl vytvořen postup hello uvedených v hello [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilé analýzy proces a technologie ](machine-learning-data-science-customize-hadoop-cluster.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="280a4-122">**upload** them toohello default (or appropriate container) of hello Azure storage account that was created by hello procedure outlined in hello [Customize Azure HDInsight Hadoop clusters for Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md) topic.</span></span> <span data-ttu-id="280a4-123">Hello proces tooupload hello .csv soubory toohello výchozí kontejner hello účtu úložiště najdete v tomto [stránky](machine-learning-data-science-process-hive-walkthrough.md#upload).</span><span class="sxs-lookup"><span data-stu-id="280a4-123">hello process tooupload hello .csv files toohello default container on hello storage account can be found on this [page](machine-learning-data-science-process-hive-walkthrough.md#upload).</span></span>

## <span data-ttu-id="280a4-124"><a name="submit"></a>Jak toosubmit dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="280a4-124"><a name="submit"></a>How toosubmit Hive queries</span></span>
<span data-ttu-id="280a4-125">Dotazy Hive můžete odeslat pomocí:</span><span class="sxs-lookup"><span data-stu-id="280a4-125">Hive queries can be submitted by using:</span></span>

1. [<span data-ttu-id="280a4-126">Odesílání dotazů Hive prostřednictvím Hadoop příkazového řádku v headnode clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="280a4-126">Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>](#headnode)
2. [<span data-ttu-id="280a4-127">Odesílání dotazů Hive s hello Hive Editor</span><span class="sxs-lookup"><span data-stu-id="280a4-127">Submit Hive queries with hello Hive Editor</span></span>](#hive-editor)
3. [<span data-ttu-id="280a4-128">Odesílání dotazů Hive pomocí příkazů prostředí PowerShell Azure</span><span class="sxs-lookup"><span data-stu-id="280a4-128">Submit Hive queries with Azure PowerShell Commands</span></span>](#ps)

<span data-ttu-id="280a4-129">Dotazy Hive jsou podobné jazyku SQL.</span><span class="sxs-lookup"><span data-stu-id="280a4-129">Hive queries are SQL-like.</span></span> <span data-ttu-id="280a4-130">Pokud jste obeznámeni s SQL, může se stát hello [Hive pro SQL uživatelé cheaty list](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) užitečné.</span><span class="sxs-lookup"><span data-stu-id="280a4-130">If you are familiar with SQL, you may find hello [Hive for SQL Users Cheat Sheet](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) useful.</span></span>

<span data-ttu-id="280a4-131">Při odesílání dotazů Hive, můžete také ovládat hello cílové hello výstupu z dotazů Hive, zda byl na hello obrazovky nebo tooa místního souboru na hello hlavního uzlu nebo tooan objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="280a4-131">When submitting a Hive query, you can also control hello destination of hello output from Hive queries, whether it be on hello screen or tooa local file on hello head node or tooan Azure blob.</span></span>

### <span data-ttu-id="280a4-132"><a name="headnode"></a> 1. Odesílání dotazů Hive prostřednictvím Hadoop příkazového řádku v headnode clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="280a4-132"><a name="headnode"></a> 1. Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>
<span data-ttu-id="280a4-133">Pokud hello Hive dotaz je složité, odesílá se, že jej přímo do hello hlavního uzlu v clusteru Hadoop hello obvykle vede zapnout toofaster kolem než odesílání se v editoru Hive nebo Azure PowerShell skripty.</span><span class="sxs-lookup"><span data-stu-id="280a4-133">If hello Hive query is complex, submitting it directly in hello head node of hello Hadoop cluster typically leads toofaster turn around than submitting it with a Hive Editor or Azure PowerShell scripts.</span></span>

<span data-ttu-id="280a4-134">Přihlášení toohello hlavního uzlu v clusteru Hadoop hello, otevřete na ploše hello hlavního uzlu hello hello Hadoop příkazového řádku a zadejte příkaz `cd %hive_home%\bin`.</span><span class="sxs-lookup"><span data-stu-id="280a4-134">Log in toohello head node of hello Hadoop cluster, open hello Hadoop Command Line on hello desktop of hello head node, and enter command `cd %hive_home%\bin`.</span></span>

<span data-ttu-id="280a4-135">Máte tři způsoby toosubmit Hive dotazy v hello Hadoop příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="280a4-135">You have three ways toosubmit Hive queries in hello Hadoop Command Line:</span></span>

* <span data-ttu-id="280a4-136">přímo</span><span class="sxs-lookup"><span data-stu-id="280a4-136">directly</span></span>
* <span data-ttu-id="280a4-137">pomocí souborů .hql</span><span class="sxs-lookup"><span data-stu-id="280a4-137">using .hql files</span></span>
* <span data-ttu-id="280a4-138">s hello Hive konzoly příkazu</span><span class="sxs-lookup"><span data-stu-id="280a4-138">with hello Hive command console</span></span>

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a><span data-ttu-id="280a4-139">Odesílání dotazů Hive přímo v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="280a4-139">Submit Hive queries directly in Hadoop Command Line.</span></span>
<span data-ttu-id="280a4-140">Můžete spustit příkaz jako `hive -e "<your hive query>;` toosubmit jednoduchých dotazů Hive přímo v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="280a4-140">You can run command like `hive -e "<your hive query>;` toosubmit simple Hive queries directly in Hadoop Command Line.</span></span> <span data-ttu-id="280a4-141">Tady je příklad, kde hello red pole jsou podrobněji popsány dále hello příkaz, který odešle dotaz Hive hello a hello zelené pole jsou podrobněji popsány dále hello výstup hello dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-141">Here is an example, where hello red box outlines hello command that submits hello Hive query, and hello green box outlines hello output from hello Hive query.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a><span data-ttu-id="280a4-143">Odesílání dotazů Hive v souborech .hql</span><span class="sxs-lookup"><span data-stu-id="280a4-143">Submit Hive queries in .hql files</span></span>
<span data-ttu-id="280a4-144">Pokud dotaz Hive hello je složitější a má více řádků, není praktické úpravy dotazů v příkazovém řádku nebo konzoly příkazu Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-144">When hello Hive query is more complicated and has multiple lines, editing queries in command line or Hive command console is not practical.</span></span> <span data-ttu-id="280a4-145">Alternativou je toouse textového editoru v hello hlavního uzlu v clusteru hello Hadoop toosave hello dotazů na Hive v souboru .hql do místního adresáře hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="280a4-145">An alternative is toouse a text editor in hello head node of hello Hadoop cluster toosave hello Hive queries in a .hql file in a local directory of hello head node.</span></span> <span data-ttu-id="280a4-146">Potom pomocí hello jde odeslat dotaz Hive hello v souboru .hql hello `-f` argument následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="280a4-146">Then hello Hive query in hello .hql file can be submitted by using hello `-f` argument as follows:</span></span>

    hive -f "<path toohello .hql file>"

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)

<span data-ttu-id="280a4-148">**Potlačit tisk obrazovky stav průběhu dotazů Hive**</span><span class="sxs-lookup"><span data-stu-id="280a4-148">**Suppress progress status screen print of Hive queries**</span></span>

<span data-ttu-id="280a4-149">Ve výchozím nastavení po odeslání dotazu Hive v systému Hadoop příkazového řádku hello průběh úlohy mapy nebo snižte hello se vytiskne na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="280a4-149">By default, after Hive query is submitted in Hadoop Command Line, hello progress of hello Map/Reduce job is printed out on screen.</span></span> <span data-ttu-id="280a4-150">toosuppress hello obrazovky tiskových průběh úlohy mapy nebo snižte hello, můžete použít argument `-S` ("S velkými písmeny) v hello příkazový řádek následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="280a4-150">toosuppress hello screen print of hello Map/Reduce job progress, you can use an argument `-S` ("S" in upper case) in hello command line as follows:</span></span>

    hive -S -f "<path toohello .hql file>"
<span data-ttu-id="280a4-151">.</span><span class="sxs-lookup"><span data-stu-id="280a4-151">.</span></span>    <span data-ttu-id="280a4-152">Hive -S -e "<Hive queries>"</span><span class="sxs-lookup"><span data-stu-id="280a4-152">hive -S -e "<Hive queries>"</span></span>

#### <a name="submit-hive-queries-in-hive-command-console"></a><span data-ttu-id="280a4-153">Odesílání dotazů Hive v konzole příkaz Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-153">Submit Hive queries in Hive command console.</span></span>
<span data-ttu-id="280a4-154">Hello Hive příkaz konzoly můžete zadat také nejprve spuštěním příkazu `hive` v Hadoop příkazového řádku a pak odesílání dotazů Hive v konzole příkaz Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-154">You can also first enter hello Hive command console by running command `hive` in Hadoop Command Line, and then submit Hive queries in Hive command console.</span></span> <span data-ttu-id="280a4-155">Tady je příklad.</span><span class="sxs-lookup"><span data-stu-id="280a4-155">Here is an example.</span></span> <span data-ttu-id="280a4-156">V tomto příkladu hello dvě červená pole zvýraznění hello příkazy používané tooenter hello Hive konzoly příkazu a hello dotaz Hive odeslání v konzole příkaz Hive v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="280a4-156">In this example, hello two red boxes highlight hello commands used tooenter hello Hive command console, and hello Hive query submitted in Hive command console, respectively.</span></span> <span data-ttu-id="280a4-157">pole Hello zelená označuje hello výstup hello dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-157">hello green box highlights hello output from hello Hive query.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

<span data-ttu-id="280a4-159">předchozí příklady Hello přímo výstup výsledků dotazu Hive hello na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="280a4-159">hello previous examples directly output hello Hive query results on screen.</span></span> <span data-ttu-id="280a4-160">Je také možné zapsat místního souboru tooa hello výstup hello hlavního uzlu nebo tooan objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="280a4-160">You can also write hello output tooa local file on hello head node, or tooan Azure blob.</span></span> <span data-ttu-id="280a4-161">Pak můžete použít jiné nástroje toofurther analyzovat výstup hello dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-161">Then, you can use other tools toofurther analyze hello output of Hive queries.</span></span>

<span data-ttu-id="280a4-162">**Výstupu podregistru dotazu výsledky tooa místního souboru.**</span><span class="sxs-lookup"><span data-stu-id="280a4-162">**Output Hive query results tooa local file.**</span></span>
<span data-ttu-id="280a4-163">toooutput Hive dotaz výsledky tooa místního adresáře v hello hlavního uzlu, máte dotaz Hive hello toosubmit v hello Hadoop příkazový řádek následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="280a4-163">toooutput Hive query results tooa local directory on hello head node, you have toosubmit hello Hive query in hello Hadoop Command Line as follows:</span></span>

    hive -e "<hive query>" > <local path in hello head node>

<span data-ttu-id="280a4-164">V následujícím příkladu hello, výstup hello dotazu Hive je zapsán do souboru `hivequeryoutput.txt` v adresáři `C:\apps\temp`.</span><span class="sxs-lookup"><span data-stu-id="280a4-164">In hello following example, hello output of Hive query is written into a file `hivequeryoutput.txt` in directory `C:\apps\temp`.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

<span data-ttu-id="280a4-166">**Výstupu podregistru dotazu výsledky tooan objektů blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="280a4-166">**Output Hive query results tooan Azure blob**</span></span>

<span data-ttu-id="280a4-167">Můžete také výstup hello Hive dotaz výsledky tooan objektů blob Azure v rámci clusteru Hadoop hello hello výchozí kontejner.</span><span class="sxs-lookup"><span data-stu-id="280a4-167">You can also output hello Hive query results tooan Azure blob, within hello default container of hello Hadoop cluster.</span></span> <span data-ttu-id="280a4-168">dotaz Hive Hello to vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="280a4-168">hello Hive query for this is as follows:</span></span>

    insert overwrite directory wasb:///<directory within hello default container> <select clause from ...>

<span data-ttu-id="280a4-169">V následujícím příkladu hello, je zapsán výstup hello dotaz Hive tooa blob directory `queryoutputdir` v rámci clusteru Hadoop hello hello výchozí kontejner.</span><span class="sxs-lookup"><span data-stu-id="280a4-169">In hello following example, hello output of Hive query is written tooa blob directory `queryoutputdir` within hello default container of hello Hadoop cluster.</span></span> <span data-ttu-id="280a4-170">Zde potřebujete jenom název adresáře hello tooprovide, bez hello název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="280a4-170">Here, you only need tooprovide hello directory name, without hello blob name.</span></span> <span data-ttu-id="280a4-171">Pokud například zadáte názvy adresáře a objektů blob, je vyvolána chyba `wasb:///queryoutputdir/queryoutput.txt`.</span><span class="sxs-lookup"><span data-stu-id="280a4-171">An error is thrown if you provide both directory and blob names, such as `wasb:///queryoutputdir/queryoutput.txt`.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

<span data-ttu-id="280a4-173">Pokud otevřete hello výchozí kontejner hello clusteru Hadoop pomocí Průzkumníka úložiště Azure, uvidíte výstup hello dotaz Hive hello, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="280a4-173">If you open hello default container of hello Hadoop cluster using Azure Storage Explorer, you can see hello output of hello Hive query as shown in hello following figure.</span></span> <span data-ttu-id="280a4-174">Můžete použít objekt blob hello filtru (zvýrazněné podle červeným rámečkem) tooonly načtení hello s zadaná písmena v názvech.</span><span class="sxs-lookup"><span data-stu-id="280a4-174">You can apply hello filter (highlighted by red box) tooonly retrieve hello blob with specified letters in names.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

### <span data-ttu-id="280a4-176"><a name="hive-editor"></a> 2. Odesílání dotazů Hive s hello Hive Editor</span><span class="sxs-lookup"><span data-stu-id="280a4-176"><a name="hive-editor"></a> 2. Submit Hive queries with hello Hive Editor</span></span>
<span data-ttu-id="280a4-177">Můžete použít také hello dotazu konzoly (Hive Editor) tak, že zadáte adresu URL ve formátu hello *https://&#60; Název clusteru Hadoop >.azurehdinsight.net/Home/HiveEditor* do webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="280a4-177">You can also use hello Query Console (Hive Editor) by entering a URL of hello form *https://&#60;Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor* into a web browser.</span></span> <span data-ttu-id="280a4-178">Musí být zaznamenána najdete v tématu hello této konzoly a proto je třeba zde pověření clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="280a4-178">You must be logged in hello see this console and so you need your Hadoop cluster credentials here.</span></span>

### <span data-ttu-id="280a4-179"><a name="ps"></a> 3. Odesílání dotazů Hive pomocí příkazů prostředí PowerShell Azure</span><span class="sxs-lookup"><span data-stu-id="280a4-179"><a name="ps"></a> 3. Submit Hive queries with Azure PowerShell Commands</span></span>
<span data-ttu-id="280a4-180">Můžete také použít dotazy Hive toosubmit prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="280a4-180">You can also use PowerShell toosubmit Hive queries.</span></span> <span data-ttu-id="280a4-181">Pokyny najdete v tématu [úlohy odeslání Hive pomocí prostředí PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="280a4-181">For instructions, see [Submit Hive jobs using PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span></span>

## <span data-ttu-id="280a4-182"><a name="create-tables"></a>Vytvoření databáze Hive a tabulky</span><span class="sxs-lookup"><span data-stu-id="280a4-182"><a name="create-tables"></a>Create Hive database and tables</span></span>
<span data-ttu-id="280a4-183">Hello dotazů Hive jsou sdílené v hello [úložiště GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) a z ní si můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="280a4-183">hello Hive queries are shared in hello [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) and can be downloaded from there.</span></span>

<span data-ttu-id="280a4-184">Zde je hello dotaz Hive, který vytvoří tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-184">Here is hello Hive query that creates a Hive table.</span></span>

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

<span data-ttu-id="280a4-185">Tady je popis hello hello pole, které je třeba tooplug v a další konfigurace:</span><span class="sxs-lookup"><span data-stu-id="280a4-185">Here are hello descriptions of hello fields that you need tooplug in and other configurations:</span></span>

* <span data-ttu-id="280a4-186">**& č. 60; název databáze >**: název hello hello databáze, které chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="280a4-186">**&#60;database name>**: hello name of hello database that you want toocreate.</span></span> <span data-ttu-id="280a4-187">Pokud chcete toouse hello výchozí databázi, hello dotazu *vytvoření databáze...*  lze vynechat.</span><span class="sxs-lookup"><span data-stu-id="280a4-187">If you just want toouse hello default database, hello query *create database...* can be omitted.</span></span>
* <span data-ttu-id="280a4-188">**& č. 60; název tabulky >**: hello název hello tabulky, které chcete toocreate v rámci hello zadaná databáze.</span><span class="sxs-lookup"><span data-stu-id="280a4-188">**&#60;table name>**: hello name of hello table that you want toocreate within hello specified database.</span></span> <span data-ttu-id="280a4-189">Pokud chcete toouse hello výchozí databázi, se hello tabulky můžete přímo odkazuje *& č. 60; název tabulky >* bez & č. 60; název databáze >.</span><span class="sxs-lookup"><span data-stu-id="280a4-189">If you want toouse hello default database, hello table can be directly referred by *&#60;table name>* without &#60;database name>.</span></span>
* <span data-ttu-id="280a4-190">**& č. 60; oddělovač polí >**: hello oddělovač, který vymezuje pole v hello dat souboru toobe nahrán toohello tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-190">**&#60;field separator>**: hello separator that delimits fields in hello data file toobe uploaded toohello Hive table.</span></span>
* <span data-ttu-id="280a4-191">**& č. 60; oddělovací čáry >**: hello oddělovač, který vymezuje řádků v datovém souboru hello.</span><span class="sxs-lookup"><span data-stu-id="280a4-191">**&#60;line separator>**: hello separator that delimits lines in hello data file.</span></span>
* <span data-ttu-id="280a4-192">**& č. 60; umístění úložiště >**: hello data o umístění toosave hello úložiště Azure tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-192">**&#60;storage location>**: hello Azure storage location toosave hello data of Hive tables.</span></span> <span data-ttu-id="280a4-193">Pokud nezadáte *umístění & č. 60; umístění úložiště >*, hello databáze a hello tabulek jsou uložené v *hive neboskladu/* adresář v kontejneru výchozí hello hello Hive clusteru ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="280a4-193">If you do not specify *LOCATION &#60;storage location>*, hello database and hello tables are stored in *hive/warehouse/* directory in hello default container of hello Hive cluster by default.</span></span> <span data-ttu-id="280a4-194">Pokud chcete umístění úložiště hello toospecify, má umístění úložiště hello toobe v rámci hello výchozí kontejner hello databáze a tabulky.</span><span class="sxs-lookup"><span data-stu-id="280a4-194">If you want toospecify hello storage location, hello storage location has toobe within hello default container for hello database and tables.</span></span> <span data-ttu-id="280a4-195">Toto umístění se označuje jako umístění relativní toohello výchozí kontejner hello clusteru ve formátu hello toobe *' wasb: / / / & č. 60; adresář 1 > / "* nebo *' wasb: / / / & č. 60; adresář 1 > / & č. 60; adresář 2 > / "*atd. Po provedení dotazu hello jsou vytvořeny hello relativní adresáře v rámci hello výchozí kontejner.</span><span class="sxs-lookup"><span data-stu-id="280a4-195">This location has toobe referred as location relative toohello default container of hello cluster in hello format of *'wasb:///&#60;directory 1>/'* or *'wasb:///&#60;directory 1>/&#60;directory 2>/'*, etc. After hello query is executed, hello relative directories are created within hello default container.</span></span>
* <span data-ttu-id="280a4-196">**TBLPROPERTIES("Skip.Header.line.Count"="1")**: Pokud hello datový soubor obsahuje řádek záhlaví, máte tooadd tuto vlastnost **na konci hello** z hello *vytvořit tabulku* dotazu.</span><span class="sxs-lookup"><span data-stu-id="280a4-196">**TBLPROPERTIES("skip.header.line.count"="1")**: If hello data file has a header line, you have tooadd this property **at hello end** of hello *create table* query.</span></span> <span data-ttu-id="280a4-197">Řádek záhlaví hello, jinak je načten jako tabulku záznamů toohello.</span><span class="sxs-lookup"><span data-stu-id="280a4-197">Otherwise, hello header line is loaded as a record toohello table.</span></span> <span data-ttu-id="280a4-198">Pokud hello datový soubor nemá řádek záhlaví, můžete tuto konfiguraci vynechat v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="280a4-198">If hello data file does not have a header line, this configuration can be omitted in hello query.</span></span>

## <span data-ttu-id="280a4-199"><a name="load-data"></a>Načíst data tabulky tooHive</span><span class="sxs-lookup"><span data-stu-id="280a4-199"><a name="load-data"></a>Load data tooHive tables</span></span>
<span data-ttu-id="280a4-200">Zde je hello dotaz Hive, který načte data do tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="280a4-200">Here is hello Hive query that loads data into a Hive table.</span></span>

    LOAD DATA INPATH '<path tooblob data>' INTO TABLE <database name>.<table name>;

* <span data-ttu-id="280a4-201">**& č. 60; data tooblob cesty >**: Pokud hello objektu blob souboru toobe nahrán toohello tabulku Hive v hello výchozí kontejner hello clusteru HDInsight Hadoop, hello *& č. 60; data tooblob cesty >* musí být ve formátu hello *' wasb: / / / & č. 60; adresář v tomto kontejneru > / & č. 60; název souboru objektů blob >'*.</span><span class="sxs-lookup"><span data-stu-id="280a4-201">**&#60;path tooblob data>**: If hello blob file toobe uploaded toohello Hive table is in hello default container of hello HDInsight Hadoop cluster, hello *&#60;path tooblob data>* should be in hello format *'wasb:///&#60;directory in this container>/&#60;blob file name>'*.</span></span> <span data-ttu-id="280a4-202">soubor Hello blob může být také v další kontejner hello clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="280a4-202">hello blob file can also be in an additional container of hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="280a4-203">V takovém případě *& č. 60; data tooblob cesty >* musí být ve formátu hello *' wasb: / / & č. 60; název kontejneru > @& č. 60; název účtu úložiště >.blob.core.windows.net/ & č. 60; název souboru objektů blob >'*.</span><span class="sxs-lookup"><span data-stu-id="280a4-203">In this case, *&#60;path tooblob data>* should be in hello format *'wasb://&#60;container name>@&#60;storage account name>.blob.core.windows.net/&#60;blob file name>'*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="280a4-204">Hello tabulka tooHive toobe nahrát data objektů blob má toobe v hello výchozí nebo další kontejner hello účtu úložiště pro hello Hadoop cluster.</span><span class="sxs-lookup"><span data-stu-id="280a4-204">hello blob data toobe uploaded tooHive table has toobe in hello default or additional container of hello storage account for hello Hadoop cluster.</span></span> <span data-ttu-id="280a4-205">V opačném hello *načítání dat* nesouhlasících, že nelze přístup k datům hello se dotaz nezdaří.</span><span class="sxs-lookup"><span data-stu-id="280a4-205">Otherwise, hello *LOAD DATA* query fails complaining that it cannot access hello data.</span></span>
  >
  >

## <span data-ttu-id="280a4-206"><a name="partition-orc"></a>Rozšířené témata: tabulka a úložiště Hive data ve formátu ORC na oddíly</span><span class="sxs-lookup"><span data-stu-id="280a4-206"><a name="partition-orc"></a>Advanced topics: partitioned table and store Hive data in ORC format</span></span>
<span data-ttu-id="280a4-207">Pokud hello data velká, vytváření oddílů tabulky hello je výhodné pro dotazy, které stačí tooscan několik oddílů tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="280a4-207">If hello data is large, partitioning hello table is beneficial for queries that only need tooscan a few partitions of hello table.</span></span> <span data-ttu-id="280a4-208">Například je přiměřené toopartition data protokolu hello webové stránky podle data.</span><span class="sxs-lookup"><span data-stu-id="280a4-208">For instance, it is reasonable toopartition hello log data of a web site by dates.</span></span>

<span data-ttu-id="280a4-209">V přidání toopartitioning Hive tabulky, je také užitečné toostore hello Hive data ve formátu hello optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="280a4-209">In addition toopartitioning Hive tables, it is also beneficial toostore hello Hive data in hello Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="280a4-210">Další informace o ORC formátování naleznete v tématu <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">a souborů ORC pomocí zvyšuje výkon při čtení, zápisu a zpracování dat Hive</a>.</span><span class="sxs-lookup"><span data-stu-id="280a4-210">For more information on ORC formatting, see <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">Using ORC files improves performance when Hive is reading, writing, and processing data</a>.</span></span>

### <a name="partitioned-table"></a><span data-ttu-id="280a4-211">Dělenou tabulku</span><span class="sxs-lookup"><span data-stu-id="280a4-211">Partitioned table</span></span>
<span data-ttu-id="280a4-212">Zde je hello dotaz Hive, který vytvoří dělenou tabulku a načte data do ní.</span><span class="sxs-lookup"><span data-stu-id="280a4-212">Here is hello Hive query that creates a partitioned table and loads data into it.</span></span>

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

<span data-ttu-id="280a4-213">Při dotazování dělené tabulky, se doporučuje tooadd hello oddílu podmínky v hello **začátku** z hello `where` klauzule jako to zvyšuje účinnost hello výrazně vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="280a4-213">When querying partitioned tables, it is recommended tooadd hello partition condition in hello **beginning** of hello `where` clause as this improves hello efficacy of searching significantly.</span></span>

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <span data-ttu-id="280a4-214"><a name="orc"></a>Ukládání dat Hive ve formátu ORC</span><span class="sxs-lookup"><span data-stu-id="280a4-214"><a name="orc"></a>Store Hive data in ORC format</span></span>
<span data-ttu-id="280a4-215">Nelze načíst přímo dat z úložiště objektů blob do tabulek Hive, které je uložený ve formátu ORC hello.</span><span class="sxs-lookup"><span data-stu-id="280a4-215">You cannot directly load data from blob storage into Hive tables that is stored in hello ORC format.</span></span> <span data-ttu-id="280a4-216">Tady jsou kroky hello tooHive tabulek uložených ve formátu ORC objekty tohoto hello potřebujete tootake tooload dat z Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="280a4-216">Here are hello steps that hello you need tootake tooload data from Azure blobs tooHive tables stored in ORC format.</span></span>

<span data-ttu-id="280a4-217">Vytvoření externí tabulky **ULOŽENÝ textový soubor AS** a načtení dat z tabulky toohello úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="280a4-217">Create an external table **STORED AS TEXTFILE** and load data from blob storage toohello table.</span></span>

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

        LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<table name>;

<span data-ttu-id="280a4-218">Vytvoření interní tabulky s hello stejného schématu jako hello externí tabulky v kroku 1, s hello stejné oddělovač polí a uložte hello Hive data ve formátu ORC hello.</span><span class="sxs-lookup"><span data-stu-id="280a4-218">Create an internal table with hello same schema as hello external table in step 1, with hello same field delimiter, and store hello Hive data in hello ORC format.</span></span>

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

<span data-ttu-id="280a4-219">Vyberte data z externí tabulky hello v kroku 1 a vložit do tabulky ORC hello</span><span class="sxs-lookup"><span data-stu-id="280a4-219">Select data from hello external table in step 1 and insert into hello ORC table</span></span>

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> <span data-ttu-id="280a4-220">Pokud tabulka textový soubor hello *& č. 60; název databáze >. & č. 60; název tabulky externí textový soubor >* má oddíly, v KROKU 3 hello `SELECT * FROM <database name>.<external textfile table name>` příkaz vybere hello oddílu proměnné pole v hello vrácený datové sady.</span><span class="sxs-lookup"><span data-stu-id="280a4-220">If hello TEXTFILE table *&#60;database name>.&#60;external textfile table name>* has partitions, in STEP 3, hello `SELECT * FROM <database name>.<external textfile table name>` command selects hello partition variable as a field in hello returned data set.</span></span> <span data-ttu-id="280a4-221">Vkládání do hello *& č. 60; název databáze >. & č. 60; název tabulky ORC >* selže od *& č. 60; název databáze >. & č. 60; název tabulky ORC >* nemá hello jako proměnnou oddílu pole ve schématu tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="280a4-221">Inserting it into hello *&#60;database name>.&#60;ORC table name>* fails since *&#60;database name>.&#60;ORC table name>* does not have hello partition variable as a field in hello table schema.</span></span> <span data-ttu-id="280a4-222">V takovém případě musíte toospecifically vyberte hello pole toobe vložit příliš*& č. 60; název databáze >. & č. 60; název tabulky ORC >* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="280a4-222">In this case, you need toospecifically select hello fields toobe inserted too*&#60;database name>.&#60;ORC table name>* as follows:</span></span>
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

<span data-ttu-id="280a4-223">Je bezpečné toodrop hello *& č. 60; název tabulky externí textový soubor >* při použití hello následující dotaz po všechna data byla vložena do *& č. 60; název databáze >. & č. 60; název tabulky ORC >*:</span><span class="sxs-lookup"><span data-stu-id="280a4-223">It is safe toodrop hello *&#60;external textfile table name>* when using hello following query after all data has been inserted into *&#60;database name>.&#60;ORC table name>*:</span></span>

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

<span data-ttu-id="280a4-224">Až projdete tímto postupem, měli byste mít tabulku s daty v připravené toouse formátu hello ORC.</span><span class="sxs-lookup"><span data-stu-id="280a4-224">After following this procedure, you should have a table with data in hello ORC format ready toouse.</span></span>  
