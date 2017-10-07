---
title: "Hello proces vědecké účely Team dat v akce – pomocí clusteru Azure HDInsight Hadoop na datovou sadu 1 TB | Microsoft Docs"
description: "Pomocí hello proces vědecké účely dat Team začátku do konce scénář nasazení HDInsight Hadoop clusteru toobuild a nasadit model pomocí velké veřejně dostupné datové sady (1 TB)"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 72d958c4-3205-49b9-ad82-47998d400d2b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 59b2af02e7840cb60a4b5b2f2c8ab0611df198ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a><span data-ttu-id="01974-103">Hello proces vědecké účely Team dat v akce – pomocí clusteru Azure HDInsight Hadoop na 1 TB datové sady</span><span class="sxs-lookup"><span data-stu-id="01974-103">hello Team Data Science Process in action - Using an Azure HDInsight Hadoop Cluster on a 1 TB dataset</span></span>

<span data-ttu-id="01974-104">V tomto návodu budeme demonstruje použití hello proces vědecké účely Team dat ve scénáři s začátku do konce s [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) toostore, prozkoumejte, funkci pracovníka a veřejně dolů ukázková data z jednoho z hello k dispozici [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datové sady.</span><span class="sxs-lookup"><span data-stu-id="01974-104">In this walkthrough, we demonstrate using hello Team Data Science Process in an end-to-end scenario with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) toostore, explore, feature engineer, and down sample data from one of hello publicly available [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datasets.</span></span> <span data-ttu-id="01974-105">Tato data použijeme Azure Machine Learning toobuild model binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="01974-105">We use Azure Machine Learning toobuild a binary classification model on this data.</span></span> <span data-ttu-id="01974-106">Také ukazují, jak toopublish jednu z těchto modelů jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="01974-106">We also show how toopublish one of these models as a Web service.</span></span>

<span data-ttu-id="01974-107">Je také možné toouse IPython poznámkového bloku tooaccomplish hello úlohy uvedené v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="01974-107">It is also possible toouse an IPython notebook tooaccomplish hello tasks presented in this walkthrough.</span></span> <span data-ttu-id="01974-108">Uživatelé, kteří by jako tento postup by se měli obrátit tootry hello [Criteo návod pomocí připojení Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) tématu.</span><span class="sxs-lookup"><span data-stu-id="01974-108">Users who would like tootry this approach should consult hello [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) topic.</span></span>

## <span data-ttu-id="01974-109"><a name="dataset"></a>Popis Criteo datové sady</span><span class="sxs-lookup"><span data-stu-id="01974-109"><a name="dataset"></a>Criteo Dataset Description</span></span>
<span data-ttu-id="01974-110">Hello Criteo data jsou předpovědi klikněte na datovou sadu, která je přibližně 370 gzip komprimované TSV souborů (~1.3TB nekomprimovaným), která obsahuje více než miliardu 4.3 záznamů.</span><span class="sxs-lookup"><span data-stu-id="01974-110">hello Criteo data is a click prediction dataset that is approximately 370GB of gzip compressed TSV files (~1.3TB uncompressed), comprising more than 4.3 billion records.</span></span> <span data-ttu-id="01974-111">Jsou převzaty z 24 dní klikněte na tlačítko dat, které poskytují [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/).</span><span class="sxs-lookup"><span data-stu-id="01974-111">It is taken from 24 days of click data made available by [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/).</span></span> <span data-ttu-id="01974-112">Pro usnadnění práce hello z datových vědců jsme mít rozbalené data k dispozici toous tooexperiment s.</span><span class="sxs-lookup"><span data-stu-id="01974-112">For hello convenience of data scientists, we have unzipped data available toous tooexperiment with.</span></span>

<span data-ttu-id="01974-113">Každý záznam v této datové sadě obsahuje 40 sloupce:</span><span class="sxs-lookup"><span data-stu-id="01974-113">Each record in this dataset contains 40 columns:</span></span>

* <span data-ttu-id="01974-114">Hello první sloupec je sloupec popisek, který označuje, zda uživatel klikne **přidat** (hodnota 1) nebo není klikněte na jednu (hodnota 0)</span><span class="sxs-lookup"><span data-stu-id="01974-114">hello first column is a label column that indicates whether a user clicks an **add** (value 1) or does not click one (value 0)</span></span>
* <span data-ttu-id="01974-115">Další 13 sloupce jsou číselné, a</span><span class="sxs-lookup"><span data-stu-id="01974-115">next 13 columns are numeric, and</span></span>
* <span data-ttu-id="01974-116">poslední 26 jsou kategorií sloupců</span><span class="sxs-lookup"><span data-stu-id="01974-116">last 26 are categorical columns</span></span>

<span data-ttu-id="01974-117">Hello sloupce jsou anonymizovány a použijte řadu výčtové názvy: "Sloupec1" (pro sloupec popisek hello) příliš ' Col40 "(pro poslední sloupec kategorií hello).</span><span class="sxs-lookup"><span data-stu-id="01974-117">hello columns are anonymized and use a series of enumerated names: "Col1" (for hello label column) too'Col40" (for hello last categorical column).</span></span>            

<span data-ttu-id="01974-118">Tady je výňatek hello nejprve 20 sloupce dvě hodnoty (řádky) s tuto datovou sadu:</span><span class="sxs-lookup"><span data-stu-id="01974-118">Here is an excerpt of hello first 20 columns of two observations (rows) from this dataset:</span></span>

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

<span data-ttu-id="01974-119">V této datové sady je chybějící hodnoty ve sloupcích i hello číselné a kategorií.</span><span class="sxs-lookup"><span data-stu-id="01974-119">There are missing values in both hello numeric and categorical columns in this dataset.</span></span> <span data-ttu-id="01974-120">Jsme popisují jednoduchý způsob zpracování hello chybějící hodnoty.</span><span class="sxs-lookup"><span data-stu-id="01974-120">We describe a simple method for handling hello missing values.</span></span> <span data-ttu-id="01974-121">Další podrobné informace o datech hello jsou prozkoumali, když jsme ukládat do tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="01974-121">Additional details of hello data are explored when we store them into Hive tables.</span></span>

<span data-ttu-id="01974-122">**Definice:** *Interaktivní kurz (PEV.cenu):* Toto je procento hello kliknutí v hello data.</span><span class="sxs-lookup"><span data-stu-id="01974-122">**Definition:** *Clickthrough rate (CTR):* This is hello percentage of clicks in hello data.</span></span> <span data-ttu-id="01974-123">V této datové sadě Criteo hello PEV.cenu je přibližně % 3.3 nebo 0.033.</span><span class="sxs-lookup"><span data-stu-id="01974-123">In this Criteo dataset, hello CTR is about 3.3% or 0.033.</span></span>

## <span data-ttu-id="01974-124"><a name="mltasks"></a>Příklady předpovědi úlohy</span><span class="sxs-lookup"><span data-stu-id="01974-124"><a name="mltasks"></a>Examples of prediction tasks</span></span>
<span data-ttu-id="01974-125">Dva ukázkové předpovědi problémy řešeny v tomto průvodci:</span><span class="sxs-lookup"><span data-stu-id="01974-125">Two sample prediction problems are addressed in this walkthrough:</span></span>

1. <span data-ttu-id="01974-126">**Binární klasifikace**: předpovídá, zda uživatel klikli add:</span><span class="sxs-lookup"><span data-stu-id="01974-126">**Binary classification**: Predicts whether a user clicked an add:</span></span>
   
   * <span data-ttu-id="01974-127">Třída 0: Žádné klikněte na</span><span class="sxs-lookup"><span data-stu-id="01974-127">Class 0: No Click</span></span>
   * <span data-ttu-id="01974-128">Třídy 1: klikněte na</span><span class="sxs-lookup"><span data-stu-id="01974-128">Class 1: Click</span></span>
2. <span data-ttu-id="01974-129">**Regrese**: předpovídá hello pravděpodobnost ad klikněte na z funkcí uživatelů.</span><span class="sxs-lookup"><span data-stu-id="01974-129">**Regression**: Predicts hello probability of an ad click from user features.</span></span>

## <span data-ttu-id="01974-130"><a name="setup"></a>Nastavit až HDInsight Hadoop clusteru pro vědecké zpracování dat</span><span class="sxs-lookup"><span data-stu-id="01974-130"><a name="setup"></a>Set Up an HDInsight Hadoop cluster for data science</span></span>
<span data-ttu-id="01974-131">**Poznámka:** se obvykle jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="01974-131">**Note:** This is typically an **Admin** task.</span></span>

<span data-ttu-id="01974-132">Nastavení prostředí vědecké zpracování dat Azure pro vytváření řešení prediktivní analýzy s clustery HDInsight v tři kroky:</span><span class="sxs-lookup"><span data-stu-id="01974-132">Set up your Azure Data Science environment for building predictive analytics solutions with HDInsight clusters in three steps:</span></span>

1. <span data-ttu-id="01974-133">[Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md): Tento účet úložiště se použité toostore data v Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="01974-133">[Create a storage account](../storage/common/storage-create-storage-account.md): This storage account is used toostore data in Azure Blob Storage.</span></span> <span data-ttu-id="01974-134">Zde jsou uložena data Hello používá v clusterech HDInsight.</span><span class="sxs-lookup"><span data-stu-id="01974-134">hello data used in HDInsight clusters is stored here.</span></span>
2. <span data-ttu-id="01974-135">[Přizpůsobení clusterů systému Hadoop HDInsight Azure pro vědecké zpracování dat](machine-learning-data-science-customize-hadoop-cluster.md): Tento krok vytvoří s 64-bit Anaconda Python 2.7 nainstalované na všech uzlech clusteru Azure HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="01974-135">[Customize Azure HDInsight Hadoop Clusters for Data Science](machine-learning-data-science-customize-hadoop-cluster.md): This step creates an Azure HDInsight Hadoop cluster with 64-bit Anaconda Python 2.7 installed on all nodes.</span></span> <span data-ttu-id="01974-136">Existují dva důležité kroky (popsaný v tomto tématu) toocomplete při přizpůsobení hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="01974-136">There are two important steps (described in this topic) toocomplete when customizing hello HDInsight cluster.</span></span>
   
   * <span data-ttu-id="01974-137">Je nutné propojit vytvořili v kroku 1 k vašemu clusteru HDInsight při vytvoření účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="01974-137">You must link hello storage account created in step 1 with your HDInsight cluster when it is created.</span></span> <span data-ttu-id="01974-138">Tento účet úložiště se používá pro přístup k datům, která může být zpracována v rámci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="01974-138">This storage account is used for accessing data that can be processed within hello cluster.</span></span>
   * <span data-ttu-id="01974-139">Po vytvoření je potřeba povolit vzdálený přístup toohello hlavního uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="01974-139">You must enable Remote Access toohello head node of hello cluster after it is created.</span></span> <span data-ttu-id="01974-140">Pamatovat přihlašovací údaje vzdáleného přístupu hello zde určíte (liší od nastavení zadané pro cluster hello při jeho vytváření): je potřebujete toocomplete hello následující postupy.</span><span class="sxs-lookup"><span data-stu-id="01974-140">Remember hello remote access credentials you specify here (different from those specified for hello cluster at its creation): you need them toocomplete hello following procedures.</span></span>
3. <span data-ttu-id="01974-141">[Vytvořit pracovní prostor služby Azure ML](machine-learning-create-workspace.md): Tento Azure Machine Learning prostoru slouží k vytváření modelů machine learning po zkoumání počáteční data a dolů vzorkování na clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="01974-141">[Create an Azure ML workspace](machine-learning-create-workspace.md): This Azure Machine Learning workspace is used for building machine learning models after an initial data exploration and down sampling on hello HDInsight cluster.</span></span>

## <span data-ttu-id="01974-142"><a name="getdata"></a>Získání a využívat data z veřejné zdroje</span><span class="sxs-lookup"><span data-stu-id="01974-142"><a name="getdata"></a>Get and consume data from a public source</span></span>
<span data-ttu-id="01974-143">Hello [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datové sady je přístupná kliknutím na odkaz hello, přijetí hello podmínky použití a poskytnutím název.</span><span class="sxs-lookup"><span data-stu-id="01974-143">hello [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset can be accessed by clicking on hello link, accepting hello terms of use, and providing a name.</span></span> <span data-ttu-id="01974-144">Zobrazí se zde snímek co vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="01974-144">A snapshot of what this looks like is shown here:</span></span>

![Přijměte podmínky Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

<span data-ttu-id="01974-146">Klikněte na tlačítko **pokračovat tooDownload** tooread více informací o hello datovou sadu a jeho dostupnost.</span><span class="sxs-lookup"><span data-stu-id="01974-146">Click **Continue tooDownload** tooread more about hello dataset and its availability.</span></span>

<span data-ttu-id="01974-147">Hello data se nachází v veřejné [úložiště objektů blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) umístění: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/.</span><span class="sxs-lookup"><span data-stu-id="01974-147">hello data resides in a public [Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) location: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/.</span></span> <span data-ttu-id="01974-148">Hello "wasb" odkazuje tooAzure umístění úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="01974-148">hello "wasb" refers tooAzure Blob Storage location.</span></span> 

1. <span data-ttu-id="01974-149">Hello dat v této veřejného objektu blob úložiště se skládá z tři podsložky rozbalené data.</span><span class="sxs-lookup"><span data-stu-id="01974-149">hello data in this public blob storage consists of three subfolders of unzipped data.</span></span>
   
   1. <span data-ttu-id="01974-150">Hello podsložky *nezpracovanou nebo počet/* obsahuje hello první 21 dnů dat – dni\_00 tooday\_20</span><span class="sxs-lookup"><span data-stu-id="01974-150">hello subfolder *raw/count/* contains hello first 21 days of data - from day\_00 tooday\_20</span></span>
   2. <span data-ttu-id="01974-151">Hello podsložky *nezpracovanou nebo train/* se skládá z jednoho dne dat, den\_21</span><span class="sxs-lookup"><span data-stu-id="01974-151">hello subfolder *raw/train/* consists of a single day of data, day\_21</span></span>
   3. <span data-ttu-id="01974-152">Hello podsložky *nezpracovaná/testovací/* se skládá ze dvou dnů dat, den\_22 a den\_23</span><span class="sxs-lookup"><span data-stu-id="01974-152">hello subfolder *raw/test/* consists of two days of data, day\_22 and day\_23</span></span>
2. <span data-ttu-id="01974-153">Pro ty, kteří chtějí toostart s hello gzip nezpracovaná data, jsou taky dostupné dostupné ve složce hlavní hello *nezpracovanou nebo* jako day_NN.gz, kde NN přejde z 00 too23.</span><span class="sxs-lookup"><span data-stu-id="01974-153">For those who want toostart with hello raw gzip data, these are also available in hello main folder *raw/* as day_NN.gz, where NN goes from 00 too23.</span></span>

<span data-ttu-id="01974-154">Tooaccess alternativní způsob prozkoumat a model dat, která nevyžaduje žádné místní stažení je popsáno dále v tomto návodu při vytváření tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="01974-154">An alternative approach tooaccess, explore, and model this data that does not require any local downloads is explained later in this walkthrough when we create Hive tables.</span></span>

## <span data-ttu-id="01974-155"><a name="login"></a>Přihlaste se headnode toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="01974-155"><a name="login"></a>Log in toohello cluster headnode</span></span>
<span data-ttu-id="01974-156">toolog v toohello headnode hello clusteru, použijte hello [portál Azure](https://ms.portal.azure.com) toolocate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="01974-156">toolog in toohello headnode of hello cluster, use hello [Azure portal](https://ms.portal.azure.com) toolocate hello cluster.</span></span> <span data-ttu-id="01974-157">Klikněte na ikonu Slon hello HDInsight na hello left a potom dvakrát klikněte na hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="01974-157">Click hello HDInsight elephant icon on hello left and then double-click hello name of your cluster.</span></span> <span data-ttu-id="01974-158">Přejděte toohello **konfigurace** kartě, dvakrát klikněte na ikonu připojit hello na hello dolní části stránky hello a zadejte své přihlašovací údaje vzdáleného přístupu při zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="01974-158">Navigate toohello **Configuration** tab, double-click hello CONNECT icon on hello bottom of hello page, and enter your remote access credentials when prompted.</span></span> <span data-ttu-id="01974-159">Tím přejdete toohello headnode hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="01974-159">This takes you toohello headnode of hello cluster.</span></span>

<span data-ttu-id="01974-160">Zde je typické prvním přihlášení headnode toohello clusteru, která bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="01974-160">Here is what a typical first log in toohello cluster headnode looks like:</span></span>

![Přihlaste se toocluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)

<span data-ttu-id="01974-162">Na levé straně hello vidíte hello "Hadoop příkazový řádek", což je naše Centrem pro zkoumání dat hello.</span><span class="sxs-lookup"><span data-stu-id="01974-162">On hello left, we see hello "Hadoop Command Line", which is our workhorse for hello data exploration.</span></span> <span data-ttu-id="01974-163">Vidíme také dva užitečné adresy URL - "Hadoop Yarn stav" a "Hadoop název uzlu".</span><span class="sxs-lookup"><span data-stu-id="01974-163">We also see two useful URLs - "Hadoop Yarn Status" and "Hadoop Name Node".</span></span> <span data-ttu-id="01974-164">Adresa URL Hello yarn stavu zobrazuje průběh úlohy a hello název uzlu adresa URL obsahuje údaje o konfiguraci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="01974-164">hello yarn status URL shows job progress and hello name node URL gives details on hello cluster configuration.</span></span>

<span data-ttu-id="01974-165">Nyní jsme jsou nastavené a připravené toobegin první část hello návod: zkoumání dat pomocí Hive a získání dat připravené pro Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="01974-165">Now we are set up and ready toobegin first part of hello walkthrough: data exploration using Hive and getting data ready for Azure Machine Learning.</span></span>

## <span data-ttu-id="01974-166"><a name="hive-db-tables"></a>Vytvoření databáze Hive a tabulky</span><span class="sxs-lookup"><span data-stu-id="01974-166"><a name="hive-db-tables"></a> Create Hive database and tables</span></span>
<span data-ttu-id="01974-167">toocreate Hive tabulky pro naší datové sadě Criteo otevřete hello ***Hadoop příkazového řádku*** hello plochy hello hlavního uzlu a zadejte adresář Hive hello zadáním příkazu hello</span><span class="sxs-lookup"><span data-stu-id="01974-167">toocreate Hive tables for our Criteo dataset, open hello ***Hadoop Command Line*** on hello desktop of hello head node, and enter hello Hive directory by entering hello command</span></span>

    cd %hive_home%\bin

> [!NOTE]
> <span data-ttu-id="01974-168">Spusťte všechny příkazy Hive v tomto návodu z hello Hive bin / directory řádku.</span><span class="sxs-lookup"><span data-stu-id="01974-168">Run all Hive commands in this walkthrough from hello Hive bin/ directory prompt.</span></span> <span data-ttu-id="01974-169">To postará o všech problémech, cesta automaticky.</span><span class="sxs-lookup"><span data-stu-id="01974-169">This takes care of any path issues automatically.</span></span> <span data-ttu-id="01974-170">Používáme podmínky hello "Hive directory výzva", "Hive bin / directory řádku", "Hadoop příkazový řádek" a zcela zaměnitelným významem.</span><span class="sxs-lookup"><span data-stu-id="01974-170">We use hello terms "Hive directory prompt", "Hive bin/ directory prompt", and "Hadoop Command Line" interchangeably.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="01974-171">tooexecute jakýkoli dotaz Hive jeden vždy použít hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="01974-171">tooexecute any Hive query, one can always use hello following commands:</span></span>
> 
> 

        cd %hive_home%\bin
        hive

<span data-ttu-id="01974-172">Po hello Hive REPL se zobrazí s "hive >" přihlásit, stačí vyjmout a vložit hello dotazu tooexecute ho.</span><span class="sxs-lookup"><span data-stu-id="01974-172">After hello Hive REPL appears with a "hive >"sign, simply cut and paste hello query tooexecute it.</span></span>

<span data-ttu-id="01974-173">Hello následující kód vytvoří databázi "criteo" a poté generuje 4 tabulky:</span><span class="sxs-lookup"><span data-stu-id="01974-173">hello following code creates a database "criteo" and then generates 4 tables:</span></span>

* <span data-ttu-id="01974-174">*tabulku pro generování počty* založený na den dnů\_00 tooday\_20,</span><span class="sxs-lookup"><span data-stu-id="01974-174">a *table for generating counts* built on days day\_00 tooday\_20,</span></span>
* <span data-ttu-id="01974-175">*tabulky pro použití jako hello datovou sadu train* založený na den\_21, a</span><span class="sxs-lookup"><span data-stu-id="01974-175">a *table for use as hello train dataset* built on day\_21, and</span></span>
* <span data-ttu-id="01974-176">dva *tabulky pro použití jako hello testovací datové sady* založený na den\_22 a den\_23 v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="01974-176">two *tables for use as hello test datasets* built on day\_22 and day\_23 respectively.</span></span>

<span data-ttu-id="01974-177">Protože jeden z hello dnů svátkem a chceme toodetermine, pokud hello model může zjistit rozdíly mezi svátek a bez svátek hello Interaktivní kurz jsme naše testovací datové sady rozdělit do dvou různých tabulek.</span><span class="sxs-lookup"><span data-stu-id="01974-177">We split our test dataset into two different tables because one of hello days is a holiday, and we want toodetermine if hello model can detect differences between a holiday and non-holiday from hello clickthrough rate.</span></span>

<span data-ttu-id="01974-178">Hello skriptu [ukázka &#95; hive &#95; vytvoření &#95; criteo &#95; databázi &#95; a &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) se zde zobrazí ke zvýšení pohodlí:</span><span class="sxs-lookup"><span data-stu-id="01974-178">hello script [sample&#95;hive&#95;create&#95;criteo&#95;database&#95;and&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) is displayed here for convenience:</span></span>

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

<span data-ttu-id="01974-179">Jsme Upozorňujeme, že jsou všechny tyto tabulky externí jako můžeme jednoduše tooAzure bod umístění úložiště objektů Blob (wasb).</span><span class="sxs-lookup"><span data-stu-id="01974-179">We note that all these tables are external as we simply point tooAzure Blob Storage (wasb) locations.</span></span>

<span data-ttu-id="01974-180">**Existují dva způsoby tooexecute ANY Hive dotaz, který jsme zmínili teď.**</span><span class="sxs-lookup"><span data-stu-id="01974-180">**There are two ways tooexecute ANY Hive query that we now mention.**</span></span>

1. <span data-ttu-id="01974-181">**Pomocí příkazového řádku Hive REPL hello**: hello nejprve je tooissue příkaz "hive" a zkopírujte a vložte dotaz na hello Hive REPL příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="01974-181">**Using hello Hive REPL command-line**: hello first is tooissue a "hive" command and copy and paste a query at hello Hive REPL command-line.</span></span> <span data-ttu-id="01974-182">toodo se:</span><span class="sxs-lookup"><span data-stu-id="01974-182">toodo this, do:</span></span>
   
        cd %hive_home%\bin
        hive
   
     <span data-ttu-id="01974-183">Nyní v hello REPL příkazového řádku, vyjímání a vkládání hello dotazu se provede.</span><span class="sxs-lookup"><span data-stu-id="01974-183">Now at hello REPL command-line, cutting and pasting hello query executes it.</span></span>
2. <span data-ttu-id="01974-184">**Ukládání dotazuje tooa souborů a provádění příkazu hello**: hello druhou je toosave hello dotazy tooa .hql souboru ([ukázka &#95; hive &#95; vytvoření &#95; criteo &#95; databázi &#95; a &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) a pak problém hello následující příkaz tooexecute hello dotazu:</span><span class="sxs-lookup"><span data-stu-id="01974-184">**Saving queries tooa file and executing hello command**: hello second is toosave hello queries tooa .hql file ([sample&#95;hive&#95;create&#95;criteo&#95;database&#95;and&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) and then issue hello following command tooexecute hello query:</span></span>
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a><span data-ttu-id="01974-185">Potvrďte vytváření databáze a tabulky</span><span class="sxs-lookup"><span data-stu-id="01974-185">Confirm database and table creation</span></span>
<span data-ttu-id="01974-186">V dalším kroku potvrdíme hello vytvoření databáze hello s hello následující příkaz z hello Hive bin / directory řádku:</span><span class="sxs-lookup"><span data-stu-id="01974-186">Next, we confirm hello creation of hello database with hello following command from hello Hive bin/ directory prompt:</span></span>

        hive -e "show databases;"

<span data-ttu-id="01974-187">Díky tomu jsou:</span><span class="sxs-lookup"><span data-stu-id="01974-187">This gives:</span></span>

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

<span data-ttu-id="01974-188">Potvrdíte hello vytvoření nové databáze hello "criteo".</span><span class="sxs-lookup"><span data-stu-id="01974-188">This confirms hello creation of hello new database, "criteo".</span></span>

<span data-ttu-id="01974-189">toosee tabulky, které jsme vytvořili, můžeme jednoduše příkaz hello zde z hello Hive bin / directory řádku:</span><span class="sxs-lookup"><span data-stu-id="01974-189">toosee what tables we created, we simply issue hello command here from hello Hive bin/ directory prompt:</span></span>

        hive -e "show tables in criteo;"

<span data-ttu-id="01974-190">Potom vidíme hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="01974-190">We then see hello following output:</span></span>

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <span data-ttu-id="01974-191"><a name="exploration"></a>Zkoumání dat v Hive</span><span class="sxs-lookup"><span data-stu-id="01974-191"><a name="exploration"></a> Data exploration in Hive</span></span>
<span data-ttu-id="01974-192">Teď nemůžeme připraveni toodo některé základní data zkoumání v Hive.</span><span class="sxs-lookup"><span data-stu-id="01974-192">Now we are ready toodo some basic data exploration in Hive.</span></span> <span data-ttu-id="01974-193">Začněte tím, že počítání hello počet příklady v hello train jsme test datových tabulek.</span><span class="sxs-lookup"><span data-stu-id="01974-193">We begin by counting hello number of examples in hello train and test data tables.</span></span>

### <a name="number-of-train-examples"></a><span data-ttu-id="01974-194">Počet train příklady</span><span class="sxs-lookup"><span data-stu-id="01974-194">Number of train examples</span></span>
<span data-ttu-id="01974-195">Hello obsah [ukázka &#95; hive &#95; počet &#95; train &#95; tabulky &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) zde se zobrazují:</span><span class="sxs-lookup"><span data-stu-id="01974-195">hello contents of [sample&#95;hive&#95;count&#95;train&#95;table&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) are shown here:</span></span>

        SELECT COUNT(*) FROM criteo.criteo_train;

<span data-ttu-id="01974-196">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="01974-196">This yields:</span></span>

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

<span data-ttu-id="01974-197">Alternativně jeden mohou také vystavovat hello následující příkaz z hello Hive bin / directory řádku:</span><span class="sxs-lookup"><span data-stu-id="01974-197">Alternatively, one may also issue hello following command from hello Hive bin/ directory prompt:</span></span>

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-hello-two-test-datasets"></a><span data-ttu-id="01974-198">Počet příklady testů v testovací hello dvě datové sady</span><span class="sxs-lookup"><span data-stu-id="01974-198">Number of test examples in hello two test datasets</span></span>
<span data-ttu-id="01974-199">Jsme teď počet hello příklady v hello dvě testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="01974-199">We now count hello number of examples in hello two test datasets.</span></span> <span data-ttu-id="01974-200">Hello obsah [ukázka &#95; hive &#95; počet &#95; criteo &#95; testovací &#95; den &#95; 22 &#95; tabulky &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) jsou tady:</span><span class="sxs-lookup"><span data-stu-id="01974-200">hello contents of [sample&#95;hive&#95;count&#95;criteo&#95;test&#95;day&#95;22&#95;table&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) are here:</span></span>

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

<span data-ttu-id="01974-201">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="01974-201">This yields:</span></span>

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

<span data-ttu-id="01974-202">Obvyklým způsobem, může také říkáme hello skriptu z hello Hive bin / directory výzva po vydání příkazu hello:</span><span class="sxs-lookup"><span data-stu-id="01974-202">As usual, we may also call hello script from hello Hive bin/ directory prompt by issuing hello command:</span></span>

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

<span data-ttu-id="01974-203">Nakonec jsme zkontrolujte hello počet testovací příklady v hello testovací datové sady na základě dne\_23.</span><span class="sxs-lookup"><span data-stu-id="01974-203">Finally, we examine hello number of test examples in hello test dataset based on day\_23.</span></span>

<span data-ttu-id="01974-204">Hello příkaz toodo Toto je podobné toohello jeden jenom (odkazovat příliš[ukázka &#95; hive &#95; počet &#95; criteo &#95; testovací &#95; den &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):</span><span class="sxs-lookup"><span data-stu-id="01974-204">hello command toodo this is similar toohello one just shown (refer too[sample&#95;hive&#95;count&#95;criteo&#95;test&#95;day&#95;23&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):</span></span>

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

<span data-ttu-id="01974-205">Díky tomu jsou:</span><span class="sxs-lookup"><span data-stu-id="01974-205">This gives:</span></span>

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-hello-train-dataset"></a><span data-ttu-id="01974-206">Popisek distribuce v datové sadě train hello</span><span class="sxs-lookup"><span data-stu-id="01974-206">Label distribution in hello train dataset</span></span>
<span data-ttu-id="01974-207">Popisek distribuční Hello v datové sadě train hello je určen.</span><span class="sxs-lookup"><span data-stu-id="01974-207">hello label distribution in hello train dataset is of interest.</span></span> <span data-ttu-id="01974-208">toosee toho jsme zobrazit obsah [ukázka &#95; hive &#95; criteo &#95; popisek &#95; distribuční &#95; train &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):</span><span class="sxs-lookup"><span data-stu-id="01974-208">toosee this, we show contents of [sample&#95;hive&#95;criteo&#95;label&#95;distribution&#95;train&#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):</span></span>

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

<span data-ttu-id="01974-209">Dostaneme distribuční popisek hello:</span><span class="sxs-lookup"><span data-stu-id="01974-209">This yields hello label distribution:</span></span>

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

<span data-ttu-id="01974-210">Všimněte si, že procento kladné popisky hello je přibližně 3.3 % (konzistentní s původní datové sady hello).</span><span class="sxs-lookup"><span data-stu-id="01974-210">Note that hello percentage of positive labels is about 3.3% (consistent with hello original dataset).</span></span>

### <a name="histogram-distributions-of-some-numeric-variables-in-hello-train-dataset"></a><span data-ttu-id="01974-211">Histogram distribuce některých číselné proměnných ve hello cvičení datové sady</span><span class="sxs-lookup"><span data-stu-id="01974-211">Histogram distributions of some numeric variables in hello train dataset</span></span>
<span data-ttu-id="01974-212">Můžeme použít nativní Hive na "histogram\_číselné" toofind na jaké hello distribuční číselné proměnných hello vypadá jako funkce.</span><span class="sxs-lookup"><span data-stu-id="01974-212">We can use Hive's native "histogram\_numeric" function toofind out what hello distribution of hello numeric variables looks like.</span></span> <span data-ttu-id="01974-213">Tady jsou hello obsah [ukázka &#95; hive &#95; criteo &#95; histogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):</span><span class="sxs-lookup"><span data-stu-id="01974-213">Here are hello contents of [sample&#95;hive&#95;criteo&#95;histogram&#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):</span></span>

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

<span data-ttu-id="01974-214">Dostaneme hello následující:</span><span class="sxs-lookup"><span data-stu-id="01974-214">This yields hello following:</span></span>

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

<span data-ttu-id="01974-215">Hello LATERÁLNÍ zobrazení – rozbalit kombinace v podregistru slouží tooproduce SQL jako výstup místo obvyklé seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="01974-215">hello LATERAL VIEW - explode combination in Hive serves tooproduce a SQL-like output instead of hello usual list.</span></span> <span data-ttu-id="01974-216">Všimněte si, že v hello tuto tabulku, první sloupec hello odpovídá toohello bin center a hello druhý toohello bin četnost.</span><span class="sxs-lookup"><span data-stu-id="01974-216">Note that in hello this table, hello first column corresponds toohello bin center and hello second toohello bin frequency.</span></span>

### <a name="approximate-percentiles-of-some-numeric-variables-in-hello-train-dataset"></a><span data-ttu-id="01974-217">Přibližná percentily některé číselné proměnné v hello cvičení datové sady</span><span class="sxs-lookup"><span data-stu-id="01974-217">Approximate percentiles of some numeric variables in hello train dataset</span></span>
<span data-ttu-id="01974-218">Také týkající se s číselnou proměnné je výpočet hello přibližnou percentily.</span><span class="sxs-lookup"><span data-stu-id="01974-218">Also of interest with numeric variables is hello computation of approximate percentiles.</span></span> <span data-ttu-id="01974-219">Hive je nativní "percentilu\_přibl" tomu pro nás.</span><span class="sxs-lookup"><span data-stu-id="01974-219">Hive's native "percentile\_approx" does this for us.</span></span> <span data-ttu-id="01974-220">Hello obsah [ukázka &#95; hive &#95; criteo &#95; přibližnou &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) jsou:</span><span class="sxs-lookup"><span data-stu-id="01974-220">hello contents of [sample&#95;hive&#95;criteo&#95;approximate&#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) are:</span></span>

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

<span data-ttu-id="01974-221">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="01974-221">This yields:</span></span>

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

<span data-ttu-id="01974-222">Jsme remark hello distribuce percentily obvykle je úzce související toohello histogram distribuční jakékoli číselné proměnné.</span><span class="sxs-lookup"><span data-stu-id="01974-222">We remark that hello distribution of percentiles is closely related toohello histogram distribution of any numeric variable usually.</span></span>         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-hello-train-dataset"></a><span data-ttu-id="01974-223">Najít počet jedinečných hodnot pro některé kategorií sloupců v datové sadě train hello</span><span class="sxs-lookup"><span data-stu-id="01974-223">Find number of unique values for some categorical columns in hello train dataset</span></span>
<span data-ttu-id="01974-224">Pokračováním hello zkoumání dat, nám teď najít, pro některé kategorií sloupce hello počet jedinečných hodnot, které jejich trvat.</span><span class="sxs-lookup"><span data-stu-id="01974-224">Continuing hello data exploration, we now find, for some categorical columns, hello number of unique values they take.</span></span> <span data-ttu-id="01974-225">toodo toho jsme zobrazit obsah [ukázka &#95; hive &#95; criteo &#95; jedinečný &#95; hodnoty &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):</span><span class="sxs-lookup"><span data-stu-id="01974-225">toodo this, we show contents of [sample&#95;hive&#95;criteo&#95;unique&#95;values&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):</span></span>

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

<span data-ttu-id="01974-226">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="01974-226">This yields:</span></span>

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

<span data-ttu-id="01974-227">Jsme Všimněte si, že Col15 má jedinečné hodnoty 19M!</span><span class="sxs-lookup"><span data-stu-id="01974-227">We note that Col15 has 19M unique values!</span></span> <span data-ttu-id="01974-228">Pomocí technik naïve jako "horkou jeden kódování" tooencode je nemožné takovéto vysokou dimenzí kategorií proměnné.</span><span class="sxs-lookup"><span data-stu-id="01974-228">Using naive techniques like "one-hot encoding" tooencode such high-dimensional categorical variables is infeasible.</span></span> <span data-ttu-id="01974-229">Konkrétně jsme vysvětlují a ukázka efektivní a výkonné techniku zvanou [Learning s počty](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) k boji efektivně se tento problém.</span><span class="sxs-lookup"><span data-stu-id="01974-229">In particular, we explain and demonstrate a powerful, robust technique called [Learning With Counts](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) for tackling this problem efficiently.</span></span>

<span data-ttu-id="01974-230">Zde jsme ukončení prohlížením hello počet jedinečných hodnot pro některé kategorií sloupce také.</span><span class="sxs-lookup"><span data-stu-id="01974-230">We end this sub-section by looking at hello number of unique values for some other categorical columns as well.</span></span> <span data-ttu-id="01974-231">Hello obsah [ukázka &#95; hive &#95; criteo &#95; jedinečný &#95; hodnoty &#95; několika &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) jsou:</span><span class="sxs-lookup"><span data-stu-id="01974-231">hello contents of [sample&#95;hive&#95;criteo&#95;unique&#95;values&#95;multiple&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) are:</span></span>

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

<span data-ttu-id="01974-232">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="01974-232">This yields:</span></span>

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

<span data-ttu-id="01974-233">Znovu vidíme, že s výjimkou Col20, všechny hello dalších sloupce mít mnoho jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="01974-233">Again we see that except for Col20, all hello other columns have many unique values.</span></span>

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-hello-train-dataset"></a><span data-ttu-id="01974-234">Počty společné výskyt párů kategorií proměnných v datové sadě train hello</span><span class="sxs-lookup"><span data-stu-id="01974-234">Co-occurrence counts of pairs of categorical variables in hello train dataset</span></span>

<span data-ttu-id="01974-235">počty společné výskyt Hello párů kategorií proměnných je také určen.</span><span class="sxs-lookup"><span data-stu-id="01974-235">hello co-occurrence counts of pairs of categorical variables is also of interest.</span></span> <span data-ttu-id="01974-236">To se dá určit pomocí hello kódu v [ukázka &#95; hive &#95; criteo &#95; spárované &#95; kategorií &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):</span><span class="sxs-lookup"><span data-stu-id="01974-236">This can be determined using hello code in [sample&#95;hive&#95;criteo&#95;paired&#95;categorical&#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):</span></span>

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

<span data-ttu-id="01974-237">Jsme reverse počty hello pořadí podle jejich výskytu a podívejte se na horní hello 15 v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="01974-237">We reverse order hello counts by their occurrence and look at hello top 15 in this case.</span></span> <span data-ttu-id="01974-238">To nám poskytuje:</span><span class="sxs-lookup"><span data-stu-id="01974-238">This gives us:</span></span>

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <span data-ttu-id="01974-239"><a name="downsample"></a>Dolů ukázkových datových sad hello pro Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="01974-239"><a name="downsample"></a> Down sample hello datasets for Azure Machine Learning</span></span>
<span data-ttu-id="01974-240">S prozkoumané hello datové sady a ukázán, jak jsme může tak, aby jsme mohou vytvářet modely v Azure Machine Learning se tento typ zkoumání pro všechny proměnné (včetně kombinací), jsme teď dolů ukázkových hello datových sad.</span><span class="sxs-lookup"><span data-stu-id="01974-240">Having explored hello datasets and demonstrated how we may do this type of exploration for any variables (including combinations), we now down sample hello data sets so that we can build models in Azure Machine Learning.</span></span> <span data-ttu-id="01974-241">Odvolat je tohoto problému hello zaměříme na to: danou sadu atributů příklad (funkce hodnoty z Col2 - Col40), jsme předpovědi, pokud je Sloupec1 0 (žádné kliknutím) nebo 1 (kliknutím).</span><span class="sxs-lookup"><span data-stu-id="01974-241">Recall that hello problem we focus on is: given a set of example attributes (feature values from Col2 - Col40), we predict if Col1 is a 0 (no click) or a 1 (click).</span></span>

<span data-ttu-id="01974-242">toodown ukázkové naše train a testovací datové sady too1 % hello původní velikost, používáme nativní funkce RAND() na Hive.</span><span class="sxs-lookup"><span data-stu-id="01974-242">toodown sample our train and test datasets too1% of hello original size, we use Hive's native RAND() function.</span></span> <span data-ttu-id="01974-243">Hello další skript, [ukázka &#95; hive &#95; criteo &#95; převzorkovat &#95; train &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) tomu pro datovou sadu train hello:</span><span class="sxs-lookup"><span data-stu-id="01974-243">hello next script, [sample&#95;hive&#95;criteo&#95;downsample&#95;train&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) does this for hello train dataset:</span></span>

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

<span data-ttu-id="01974-244">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="01974-244">This yields:</span></span>

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

<span data-ttu-id="01974-245">Hello skriptu [ukázka &#95; hive &#95; criteo &#95; převzorkovat &#95; testovací &#95; den &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) nemá pro testovací data den\_22:</span><span class="sxs-lookup"><span data-stu-id="01974-245">hello script [sample&#95;hive&#95;criteo&#95;downsample&#95;test&#95;day&#95;22&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) does it for test data, day\_22:</span></span>

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

<span data-ttu-id="01974-246">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="01974-246">This yields:</span></span>

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


<span data-ttu-id="01974-247">Nakonec hello skriptu [ukázka &#95; hive &#95; criteo &#95; převzorkovat &#95; testovací &#95; den &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) nemá pro testovací data den\_23:</span><span class="sxs-lookup"><span data-stu-id="01974-247">Finally, hello script [sample&#95;hive&#95;criteo&#95;downsample&#95;test&#95;day&#95;23&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) does it for test data, day\_23:</span></span>

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

<span data-ttu-id="01974-248">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="01974-248">This yields:</span></span>

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

<span data-ttu-id="01974-249">S tím, jsme jsou připravené toouse naše nižší vzorkovat natrénuje a otestuje datové sady pro vytváření modelů v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="01974-249">With this, we are ready toouse our down sampled train and test datasets for building models in Azure Machine Learning.</span></span>

<span data-ttu-id="01974-250">Před jsme přesunout na tooAzure Machine Learning, což je obavy hello počet tabulky je poslední důležité součásti.</span><span class="sxs-lookup"><span data-stu-id="01974-250">There is a final important component before we move on tooAzure Machine Learning, which is concerns hello count table.</span></span> <span data-ttu-id="01974-251">V další části dílčí hello probereme, to v některých podrobností.</span><span class="sxs-lookup"><span data-stu-id="01974-251">In hello next sub-section, we discuss this in some detail.</span></span>

## <span data-ttu-id="01974-252"><a name="count"></a>Stručný diskuse v tabulce počet hello</span><span class="sxs-lookup"><span data-stu-id="01974-252"><a name="count"></a> A brief discussion on hello count table</span></span>
<span data-ttu-id="01974-253">Jak jsme viděli, několik kategorií proměnné mají velmi vysoká dimenzionalitu.</span><span class="sxs-lookup"><span data-stu-id="01974-253">As we saw, several categorical variables have a very high dimensionality.</span></span> <span data-ttu-id="01974-254">V našem návodu Představujeme výkonné techniku zvanou [Learning s počty](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tooencode tyto proměnné efektivní a výkonné způsobem.</span><span class="sxs-lookup"><span data-stu-id="01974-254">In our walkthrough, we present a powerful technique called [Learning With Counts](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tooencode these variables in an efficient, robust manner.</span></span> <span data-ttu-id="01974-255">Další informace o této technice je v hello uvedeného odkazu.</span><span class="sxs-lookup"><span data-stu-id="01974-255">More information on this technique is in hello link provided.</span></span>

[!NOTE]
><span data-ttu-id="01974-256">V tomto návodu zaměříme na pomocí compact reprezentace tooproduce počet tabulek dimenzí vysokou kategorií funkcí.</span><span class="sxs-lookup"><span data-stu-id="01974-256">In this walkthrough, we focus on using count tables tooproduce compact representations of high-dimensional categorical features.</span></span> <span data-ttu-id="01974-257">Toto není hello pouze způsob tooencode kategorií funkcí. Další informace o jinými technikami, můžete také zajímat, uživatelé rezervovat [jeden hot kódování](http://en.wikipedia.org/wiki/One-hot) a [hashování](http://en.wikipedia.org/wiki/Feature_hashing).</span><span class="sxs-lookup"><span data-stu-id="01974-257">This is not hello only way tooencode categorical features; for more information on other techniques, interested users can check out [one-hot-encoding](http://en.wikipedia.org/wiki/One-hot) and [feature hashing](http://en.wikipedia.org/wiki/Feature_hashing).</span></span>
>

<span data-ttu-id="01974-258">počet tabulek toobuild na údaje o počtu hello, použijeme hello data hello složky nezpracovanou nebo počet.</span><span class="sxs-lookup"><span data-stu-id="01974-258">toobuild count tables on hello count data, we use hello data in hello folder raw/count.</span></span> <span data-ttu-id="01974-259">V hello modelování části ukážeme uživatelé jak se toobuild tyto počet tabulek pro kategorií funkce od začátku, případně toouse předdefinovaných počet tabulku pro jejich explorations.</span><span class="sxs-lookup"><span data-stu-id="01974-259">In hello modeling section, we show users how toobuild these count tables for categorical features from scratch, or alternatively toouse a pre-built count table for their explorations.</span></span> <span data-ttu-id="01974-260">V co způsobem, když označujeme příliš "předem vytvořené tabulky počet", jsme znamená použití hello počet tabulek, které poskytujeme.</span><span class="sxs-lookup"><span data-stu-id="01974-260">In what follows, when we refer too"pre-built count tables", we mean using hello count tables that we provide.</span></span> <span data-ttu-id="01974-261">Podrobné pokyny, jak tooaccess těchto tabulkách jsou uvedeny v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="01974-261">Detailed instructions on how tooaccess these tables are provided in hello next section.</span></span>

## <span data-ttu-id="01974-262"><a name="aml"></a>Vytvoření modelu pomocí Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="01974-262"><a name="aml"></a> Build a model with Azure Machine Learning</span></span>
<span data-ttu-id="01974-263">Naše model procesu v Azure Machine Learning vytváření zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01974-263">Our model building process in Azure Machine Learning follows these steps:</span></span>

1. [<span data-ttu-id="01974-264">Získat hello data z tabulek Hive do Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="01974-264">Get hello data from Hive tables into Azure Machine Learning</span></span>](#step1)
2. [<span data-ttu-id="01974-265">Vytvoření experimentu hello: Vyčištění dat hello a featurize s počet tabulek</span><span class="sxs-lookup"><span data-stu-id="01974-265">Create hello experiment: clean hello data and featurize with count tables</span></span>](#step2)
3. [<span data-ttu-id="01974-266">Sestavení, train a score hello model</span><span class="sxs-lookup"><span data-stu-id="01974-266">Build, train, and score hello model</span></span>](#step3)
4. [<span data-ttu-id="01974-267">Vyhodnocení modelu hello</span><span class="sxs-lookup"><span data-stu-id="01974-267">Evaluate hello model</span></span>](#step4)
5. [<span data-ttu-id="01974-268">Publikování modelu hello jako webové služby</span><span class="sxs-lookup"><span data-stu-id="01974-268">Publish hello model as a web-service</span></span>](#step5)

<span data-ttu-id="01974-269">Nyní jsme připravené toobuild modely v Azure Machine Learning studio.</span><span class="sxs-lookup"><span data-stu-id="01974-269">Now we are ready toobuild models in Azure Machine Learning studio.</span></span> <span data-ttu-id="01974-270">Naše dolů jen Vzorkovaná data se uloží jako tabulek Hive v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="01974-270">Our down sampled data is saved as Hive tables in hello cluster.</span></span> <span data-ttu-id="01974-271">Používáme hello Azure Machine Learning **importovat Data** modulu tooread tato data.</span><span class="sxs-lookup"><span data-stu-id="01974-271">We use hello Azure Machine Learning **Import Data** module tooread this data.</span></span> <span data-ttu-id="01974-272">účet úložiště Hello pověření tooaccess hello tohoto clusteru jsou uvedené v jaké způsobem.</span><span class="sxs-lookup"><span data-stu-id="01974-272">hello credentials tooaccess hello storage account of this cluster are provided in what follows.</span></span>

### <span data-ttu-id="01974-273"><a name="step1"></a>Krok 1: Načíst data do Azure Machine Learning pomocí modulu hello importovat Data z tabulek Hive a vyberte pro experimentu strojového učení</span><span class="sxs-lookup"><span data-stu-id="01974-273"><a name="step1"></a> Step 1: Get data from Hive tables into Azure Machine Learning using hello Import Data module and select it for a machine learning experiment</span></span>
<span data-ttu-id="01974-274">Začněte výběrem **+ nový** -> **EXPERIMENTU** -> **prázdný Experiment**.</span><span class="sxs-lookup"><span data-stu-id="01974-274">Start by selecting a **+NEW** -> **EXPERIMENT** -> **Blank Experiment**.</span></span> <span data-ttu-id="01974-275">Potom z hello **vyhledávání** pole hello nahoře vlevo, vyhledejte "Importovat Data".</span><span class="sxs-lookup"><span data-stu-id="01974-275">Then, from hello **Search** box on hello top left, search for "Import Data".</span></span> <span data-ttu-id="01974-276">Přetažení hello **importovat Data** modulu v toohello experimentu plátno (hello střední část úvodní obrazovka) toouse hello modulu pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="01974-276">Drag and drop hello **Import Data** module on toohello experiment canvas (hello middle portion of hello screen) toouse hello module for data access.</span></span>

<span data-ttu-id="01974-277">Toto je co hello **importovat Data** vypadá podobně jako při získávání dat z tabulky Hive hello:</span><span class="sxs-lookup"><span data-stu-id="01974-277">This is what hello **Import Data** looks like while getting data from hello Hive table:</span></span>

![Umožňuje importovat Data získá data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

<span data-ttu-id="01974-279">Pro hello **importovat Data** modulu, hello hodnoty hello parametry, které jsou k dispozici v hello obrázku jsou jenom příklady hello seřadit hodnoty, je nutné tooprovide.</span><span class="sxs-lookup"><span data-stu-id="01974-279">For hello **Import Data** module, hello values of hello parameters that are provided in hello graphic are just examples of hello sort of values you need tooprovide.</span></span> <span data-ttu-id="01974-280">Tady je některé obecné pokyny na tom, jak nastavit toofill vnější hello parametr pro hello **importovat Data** modulu.</span><span class="sxs-lookup"><span data-stu-id="01974-280">Here is some general guidance on how toofill out hello parameter set for hello **Import Data** module.</span></span>

1. <span data-ttu-id="01974-281">Volba "Dotaz Hive" pro **zdroje dat**</span><span class="sxs-lookup"><span data-stu-id="01974-281">Choose "Hive query" for **Data Source**</span></span>
2. <span data-ttu-id="01974-282">V hello **databázový dotaz Hive** pole jednoduchý příkaz SELECT * FROM < vaší\_databáze\_name.your\_tabulky\_název >-stačí.</span><span class="sxs-lookup"><span data-stu-id="01974-282">In hello **Hive database query** box, a simple SELECT * FROM <your\_database\_name.your\_table\_name> - is enough.</span></span>
3. <span data-ttu-id="01974-283">**Identifikátor URI serveru Hcatalog**: Pokud je váš cluster "abc", pak toto je jednoduše: https://abc.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="01974-283">**Hcatalog server URI**: If your cluster is "abc", then this is simply: https://abc.azurehdinsight.net</span></span>
4. <span data-ttu-id="01974-284">**Název uživatelského účtu Hadoop**: uživatelské jméno hello zvolena hello při uvedení do provozu hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="01974-284">**Hadoop user account name**: hello user name chosen at hello time of commissioning hello cluster.</span></span> <span data-ttu-id="01974-285">(Není hello RAS uživatelské jméno!)</span><span class="sxs-lookup"><span data-stu-id="01974-285">(NOT hello Remote Access user name!)</span></span>
5. <span data-ttu-id="01974-286">**Heslo uživatelského účtu Hadoop**: hello heslo pro uživatelské jméno hello zvolena hello při uvedení do provozu hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="01974-286">**Hadoop user account password**: hello password for hello user name chosen at hello time of commissioning hello cluster.</span></span> <span data-ttu-id="01974-287">(Není hello vzdáleného přístupu heslo!)</span><span class="sxs-lookup"><span data-stu-id="01974-287">(NOT hello Remote Access password!)</span></span>
6. <span data-ttu-id="01974-288">**Umístění výstupu dat**: Zvolte "Azure"</span><span class="sxs-lookup"><span data-stu-id="01974-288">**Location of output data**: Choose "Azure"</span></span>
7. <span data-ttu-id="01974-289">**Název účtu úložiště Azure**: hello účtu úložiště přidruženého k hello clusteru</span><span class="sxs-lookup"><span data-stu-id="01974-289">**Azure storage account name**: hello storage account associated with hello cluster</span></span>
8. <span data-ttu-id="01974-290">**Klíč účtu úložiště Azure**: hello klíč účtu úložiště hello přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="01974-290">**Azure storage account key**: hello key of hello storage account associated with hello cluster.</span></span>
9. <span data-ttu-id="01974-291">**Název kontejneru Azure**: Pokud je název clusteru hello "abc", pak toto je jednoduše "abc", obvykle.</span><span class="sxs-lookup"><span data-stu-id="01974-291">**Azure container name**: If hello cluster name is "abc", then this is simply "abc", usually.</span></span>

<span data-ttu-id="01974-292">Jednou hello **importovat Data** dokončení získání dat (uvidíte hello zelená značka na hello modulu), uložte tato data jako datové sady (s názvem podle svého výběru).</span><span class="sxs-lookup"><span data-stu-id="01974-292">Once hello **Import Data** finishes getting data (you see hello green tick on hello Module), save this data as a Dataset (with a name of your choice).</span></span> <span data-ttu-id="01974-293">Co to vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="01974-293">What this looks like:</span></span>

![Umožňuje importovat Data uložit data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

<span data-ttu-id="01974-295">Hello pravým tlačítkem klikněte na výstupní port hello **importovat Data** modulu.</span><span class="sxs-lookup"><span data-stu-id="01974-295">Right-click hello output port of hello **Import Data** module.</span></span> <span data-ttu-id="01974-296">Zobrazí se **uložit jako datovou sadu** možnost a **vizualizovat** možnost.</span><span class="sxs-lookup"><span data-stu-id="01974-296">This reveals a **Save as dataset** option and a **Visualize** option.</span></span> <span data-ttu-id="01974-297">Hello **vizualizovat** možnost, pokud kliknutí zobrazí 100 řádků hello dat, včetně pravém panelu, které jsou užitečné pro některé souhrnné statistiky.</span><span class="sxs-lookup"><span data-stu-id="01974-297">hello **Visualize** option, if clicked, displays 100 rows of hello data, along with a right panel that is useful for some summary statistics.</span></span> <span data-ttu-id="01974-298">toosave dat, jednoduše vyberte **uložit jako datovou sadu** a postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="01974-298">toosave data, simply select **Save as dataset** and follow instructions.</span></span>

<span data-ttu-id="01974-299">Datová sada uložena hello tooselect pro použití v experimentu machine learning, vyhledejte hello datové sady používající hello **vyhledávání** pole ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="01974-299">tooselect hello saved dataset for use in a machine learning experiment, locate hello datasets using hello **Search** box shown in hello following figure.</span></span> <span data-ttu-id="01974-300">Potom jednoduše typu out dáte název hello hello datovou sadu částečně hello tooaccess a přetáhněte hello datové sady na hlavní panel.</span><span class="sxs-lookup"><span data-stu-id="01974-300">Then simply type out hello name you gave hello dataset partially tooaccess it and drag hello dataset onto hello main panel.</span></span> <span data-ttu-id="01974-301">Umístíte jej na hlavní panel hello vybere pro použití v machine learning modelování.</span><span class="sxs-lookup"><span data-stu-id="01974-301">Dropping it onto hello main panel selects it for use in machine learning modeling.</span></span>

![Datová sada Drage na hlavní panel hello](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> <span data-ttu-id="01974-303">To lze proveďte pro hello train i hello testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="01974-303">Do this for both hello train and hello test datasets.</span></span> <span data-ttu-id="01974-304">Nezapomeňte taky, název databáze hello toouse a názvy tabulek, které jste zadali pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="01974-304">Also, remember toouse hello database name and table names that you gave for this purpose.</span></span> <span data-ttu-id="01974-305">hodnoty Hello hello obrázku jsou výhradně pro obrázek purposes.* *</span><span class="sxs-lookup"><span data-stu-id="01974-305">hello values used in hello figure are solely for illustration purposes.**</span></span>
> 
> 

### <span data-ttu-id="01974-306"><a name="step2"></a>Krok 2: Vytvoření jednoduchého experimentu v nástroji Azure Machine Learning toopredict klikne na tlačítko nebo bez kliknutí</span><span class="sxs-lookup"><span data-stu-id="01974-306"><a name="step2"></a> Step 2: Create a simple experiment in Azure Machine Learning toopredict clicks / no clicks</span></span>
<span data-ttu-id="01974-307">Naše Azure ML experimentu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="01974-307">Our Azure ML experiment looks like this:</span></span>

![Machine Learning experimentu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

<span data-ttu-id="01974-309">Nyní jsme zkontrolujte hello klíče součástí tohoto experimentu.</span><span class="sxs-lookup"><span data-stu-id="01974-309">We now examine hello key components of this experiment.</span></span> <span data-ttu-id="01974-310">Připomínáme potřebujeme toodrag naše uložené natrénuje a otestuje datové sady na plátno experimentu tooour nejdřív.</span><span class="sxs-lookup"><span data-stu-id="01974-310">As a reminder, we need toodrag our saved train and test datasets on tooour experiment canvas first.</span></span>

#### <a name="clean-missing-data"></a><span data-ttu-id="01974-311">Vyčištění chybějících dat</span><span class="sxs-lookup"><span data-stu-id="01974-311">Clean Missing Data</span></span>
<span data-ttu-id="01974-312">Hello **vyčištění chybějících dat** modul nemá naznačuje název: ho vyčistí chybějící data způsobem, který může být zadán uživatel.</span><span class="sxs-lookup"><span data-stu-id="01974-312">hello **Clean Missing Data** module does what its name suggests:  it cleans missing data in ways that can be user-specified.</span></span> <span data-ttu-id="01974-313">Vyhledávání na tento modul, vidíme toto:</span><span class="sxs-lookup"><span data-stu-id="01974-313">Looking into this module, we see this:</span></span>

![Vyčištění chybějících dat](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

<span data-ttu-id="01974-315">Zde jsme zvolili tooreplace všechny chybějící hodnoty s 0.</span><span class="sxs-lookup"><span data-stu-id="01974-315">Here, we chose tooreplace all missing values with a 0.</span></span> <span data-ttu-id="01974-316">Existují další možnosti, které se můžete podívat prohlížením hello rozevíracích seznamů v modulu hello.</span><span class="sxs-lookup"><span data-stu-id="01974-316">There are other options as well, which can be seen by looking at hello dropdowns in hello module.</span></span>

#### <a name="feature-engineering-on-hello-data"></a><span data-ttu-id="01974-317">Konstruování hello dat</span><span class="sxs-lookup"><span data-stu-id="01974-317">Feature engineering on hello data</span></span>
<span data-ttu-id="01974-318">Může existovat miliony jedinečné hodnoty pro některé kategorií funkce rozsáhlých datových sad.</span><span class="sxs-lookup"><span data-stu-id="01974-318">There can be millions of unique values for some categorical features of large datasets.</span></span> <span data-ttu-id="01974-319">Pomocí metod naïve třeba jeden horkou kódování představující vysokou dimenzí kategorií funkce, je zcela nebude proveditelné.</span><span class="sxs-lookup"><span data-stu-id="01974-319">Using naive methods such as one-hot encoding for representing such high-dimensional categorical features is entirely unfeasible.</span></span> <span data-ttu-id="01974-320">V tomto návodu ukážeme, jak toouse počet funkcí s použitím toogenerate moduly vestavěné Azure Machine Learning compact reprezentace těchto vysoce dimenzí kategorií proměnných.</span><span class="sxs-lookup"><span data-stu-id="01974-320">In this walkthrough, we demonstrate how toouse count features using built-in Azure Machine Learning modules toogenerate compact representations of these high-dimensional categorical variables.</span></span> <span data-ttu-id="01974-321">je menší velikost modelu, školení rychlejší a metriky výkonu, které jsou poměrně porovnatelný z hlediska toousing zprostředkovatele Hello konečný výsledek další techniky.</span><span class="sxs-lookup"><span data-stu-id="01974-321">hello end-result is a smaller model size, faster training times, and performance metrics that are quite comparable toousing other techniques.</span></span>

##### <a name="building-counting-transforms"></a><span data-ttu-id="01974-322">Vytváření počítání transformací</span><span class="sxs-lookup"><span data-stu-id="01974-322">Building counting transforms</span></span>
<span data-ttu-id="01974-323">Funkce count toobuild, používáme hello **sestavení počítání transformace** modul, který je k dispozici v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="01974-323">toobuild count features, we use hello **Build Counting Transform** module that is available in Azure Machine Learning.</span></span> <span data-ttu-id="01974-324">modul Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="01974-324">hello module looks like this:</span></span>

<span data-ttu-id="01974-325">![Sestavení modulu počítání transformace](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![sestavení počítání transformace modulu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)</span><span class="sxs-lookup"><span data-stu-id="01974-325">![Build Counting Transform module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![Build Counting Transform module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="01974-326">V hello **počet sloupců** pole jsme zadejte tyto sloupce, které jsme si přejí tooperform počty.</span><span class="sxs-lookup"><span data-stu-id="01974-326">In hello **Count columns** box, we enter those columns that we wish tooperform counts on.</span></span> <span data-ttu-id="01974-327">Obvykle jde o (jak je uvedeno) vysokou dimenzí kategorií sloupců.</span><span class="sxs-lookup"><span data-stu-id="01974-327">Typically, these are (as mentioned) high-dimensional categorical columns.</span></span> <span data-ttu-id="01974-328">Při spuštění hello jsme uvedených tuto datovou sadu Criteo hello má 26 kategorií sloupců: z Col15 tooCol40.</span><span class="sxs-lookup"><span data-stu-id="01974-328">At hello start, we mentioned that hello Criteo dataset has 26 categorical columns: from Col15 tooCol40.</span></span> <span data-ttu-id="01974-329">Zde jsme počet všem z nich a umožňují jejich indexy (z 15 too40 oddělených čárkami, jak je znázorněno).</span><span class="sxs-lookup"><span data-stu-id="01974-329">Here, we count on all of them and give their indices (from 15 too40 separated by commas as shown).</span></span>
> 

<span data-ttu-id="01974-330">modul hello toouse v hello MapReduce režimu (vhodné pro velké datové sady), jsme potřebovat přístup ke clusteru HDInsight Hadoop tooan (pro zkoumání funkce lze znovu použít pro tento účel také hello) a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="01974-330">toouse hello module in hello MapReduce mode (appropriate for large datasets), we need access tooan HDInsight Hadoop cluster (hello one used for feature exploration can be reused for this purpose as well) and its credentials.</span></span> <span data-ttu-id="01974-331">Hello předchozí následující obrázky znázorňují, které hello vyplněné hodnoty vypadá jako (Nahraďte hodnoty hello ilustrativní s těmi, které jsou relevantní pro vlastní případ použití).</span><span class="sxs-lookup"><span data-stu-id="01974-331">hello  previous figures illustrate what hello filled-in values look like (replace hello values provided for illustration with those relevant for your own use-case).</span></span>

![Parametry modulu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

<span data-ttu-id="01974-333">V předchozí obrázek hello, ukážeme, jak tooenter hello vstupní blob umístění.</span><span class="sxs-lookup"><span data-stu-id="01974-333">In hello figure above, we show how tooenter hello input blob location.</span></span> <span data-ttu-id="01974-334">Toto umístění obsahuje data hello vyhrazena pro vytváření počet tabulek.</span><span class="sxs-lookup"><span data-stu-id="01974-334">This location has hello data reserved for building count tables on.</span></span>

<span data-ttu-id="01974-335">Po dokončení tohoto modulu můžeme uložit hello transformace pro později hello modulu kliknete pravým tlačítkem a výběrem hello **uložit jako transformace** možnost:</span><span class="sxs-lookup"><span data-stu-id="01974-335">After this module finishes running, we can save hello transform for later by right-clicking hello module and selecting hello **Save as Transform** option:</span></span>

![Možnost "Uložit jako transformace"](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

<span data-ttu-id="01974-337">V našem experimentu architektuře výše uvedeném hello datovou sadu "ytransform2" odpovídá přesněji tooa uložit počet transformace.</span><span class="sxs-lookup"><span data-stu-id="01974-337">In our experiment architecture shown above, hello dataset "ytransform2" corresponds precisely tooa saved count transform.</span></span> <span data-ttu-id="01974-338">Pro hello zbývající část tohoto experimentu, předpokládáme, že čtečka hello používá **sestavení počítání transformace** modulu v některé počty toogenerate data a pak můžete použít tyto počty toogenerate počet funkce na hello train a testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="01974-338">For hello remainder of this experiment, we assume that hello reader used a **Build Counting Transform** module on some data toogenerate counts, and can then use those counts toogenerate count features on hello train and test datasets.</span></span>

##### <a name="choosing-what-count-features-tooinclude-as-part-of-hello-train-and-test-datasets"></a><span data-ttu-id="01974-339">Výběr co funkce tooinclude se počítají jako součást hello train a testovací datové sady</span><span class="sxs-lookup"><span data-stu-id="01974-339">Choosing what count features tooinclude as part of hello train and test datasets</span></span>
<span data-ttu-id="01974-340">Jakmile jsme počet transformace připraveno, může uživatel hello zvolte jaké funkce tooinclude v jejich train a testovací datové sady používající hello **upravit parametry tabulky počet** modulu.</span><span class="sxs-lookup"><span data-stu-id="01974-340">Once we have a count transform ready, hello user can choose what features tooinclude in their train and test datasets using hello **Modify Count Table Parameters** module.</span></span> <span data-ttu-id="01974-341">Tento modul ukážeme právě tady pro úplnost, ale v zájmu jednoduchosti nepoužívejte ve skutečnosti ho v našem experimentu.</span><span class="sxs-lookup"><span data-stu-id="01974-341">We just show this module here for completeness, but in interests of simplicity do not actually use it in our experiment.</span></span>

![Upravit parametry počet tabulky](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

<span data-ttu-id="01974-343">V takovém případě jak je vidět, jsme vybrali toouse jenom hello protokolu pravděpodobnost a tooignore hello regrese sloupce.</span><span class="sxs-lookup"><span data-stu-id="01974-343">In this case, as can be seen, we have chosen toouse just hello log-odds and tooignore hello back off column.</span></span> <span data-ttu-id="01974-344">Můžeme také nastavit parametry, třeba prahová hodnota bin paměti hello, kolik tooadd částečně předchozí příklady vyhlazení, a jestli toouse žádné Laplacian noise nebo ne.</span><span class="sxs-lookup"><span data-stu-id="01974-344">We can also set parameters such as hello garbage bin threshold, how many pseudo-prior examples tooadd for smoothing, and whether toouse any Laplacian noise or not.</span></span> <span data-ttu-id="01974-345">Všechny tyto pokročilé funkce a je, že toobe poznamenat, že hello výchozí hodnoty jsou to dobrý výchozí bod pro uživatele, kteří jsou nový typ toothis funkce generování.</span><span class="sxs-lookup"><span data-stu-id="01974-345">All these are advanced features and it is toobe noted that hello default values are a good starting point for users who are new toothis type of feature generation.</span></span>

##### <a name="data-transformation-before-generating-hello-count-features"></a><span data-ttu-id="01974-346">Transformace dat před vygenerováním hello počet funkcí</span><span class="sxs-lookup"><span data-stu-id="01974-346">Data transformation before generating hello count features</span></span>
<span data-ttu-id="01974-347">Teď soustředit na důležité bod o transformace naše train a testovací data předchozí tooactually generování funkce count.</span><span class="sxs-lookup"><span data-stu-id="01974-347">Now we focus on an important point about transforming our train and test data prior tooactually generating count features.</span></span> <span data-ttu-id="01974-348">Všimněte si, že existují dvě **spustit skript jazyka R** moduly používané před použijeme hello počet transformace tooour data.</span><span class="sxs-lookup"><span data-stu-id="01974-348">Note that there are two **Execute R Script** modules used before we apply hello count transform tooour data.</span></span>

![Spustit skript jazyka R moduly](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

<span data-ttu-id="01974-350">Tady je skript jazyka R první hello:</span><span class="sxs-lookup"><span data-stu-id="01974-350">Here is hello first R script:</span></span>

![První skript jazyka R](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)

<span data-ttu-id="01974-352">V tomto skriptu R jsme příliš přejmenovat naše toonames sloupce "Sloupec1" "Col40".</span><span class="sxs-lookup"><span data-stu-id="01974-352">In this R script, we rename our columns toonames "Col1" too"Col40".</span></span> <span data-ttu-id="01974-353">Je to proto, že počet transformace hello očekává názvy tohoto formátu.</span><span class="sxs-lookup"><span data-stu-id="01974-353">This is because hello count transform expects names of this format.</span></span>

<span data-ttu-id="01974-354">Ve skriptu R druhý hello, jsme vyvážit hello distribuční mezi kladné a záporné třídy (třídy 1 a 0) třídou převzorkování hello záporné.</span><span class="sxs-lookup"><span data-stu-id="01974-354">In hello second R script, we balance hello distribution between positive and negative classes (classes 1 and 0 respectively) by downsampling hello negative class.</span></span> <span data-ttu-id="01974-355">Hello R skript je zde ukázkou, jak toodo toto:</span><span class="sxs-lookup"><span data-stu-id="01974-355">hello R script here shows how toodo this:</span></span>

![Druhý skript jazyka R](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

<span data-ttu-id="01974-357">V této jednoduché skript jazyka R používáme "pos\_záporné\_poměr" tooset hello množství rovnováhu mezi hello kladné a záporné třídy hello.</span><span class="sxs-lookup"><span data-stu-id="01974-357">In this simple R script, we use "pos\_neg\_ratio" tooset hello amount of balance between hello positive and hello negative classes.</span></span> <span data-ttu-id="01974-358">To je důležité, že toodo vzhledem k tomu obvykle zlepšení nevyváženosti třída má výhody výkonu pro klasifikaci problémy kde hello třída distribuce je nesouměrně rozdělí (odvolání, že v našem případě obsahuje kladné a záporné třídu 96.7 % 3.3 %).</span><span class="sxs-lookup"><span data-stu-id="01974-358">This is important toodo since improving class imbalance usually has performance benefits for classification problems where hello class distribution is skewed (recall that in our case, we have 3.3% positive class and 96.7% negative class).</span></span>

##### <a name="applying-hello-count-transformation-on-our-data"></a><span data-ttu-id="01974-359">Použití transformace počet hello na našich dat</span><span class="sxs-lookup"><span data-stu-id="01974-359">Applying hello count transformation on our data</span></span>
<span data-ttu-id="01974-360">Nakonec můžeme použít hello **použít transformaci** modulu tooapply hello počet transformace na našem train a testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="01974-360">Finally, we can use hello **Apply Transformation** module tooapply hello count transforms on our train and test datasets.</span></span> <span data-ttu-id="01974-361">Tento modul trvá transformace počet hello uložit jako jeden vstup a hello cvičení nebo testovací datové sady jako hello jiného vstupu a vrátí data pomocí funkce count.</span><span class="sxs-lookup"><span data-stu-id="01974-361">This module takes hello saved count transform as one input and hello train or test datasets as hello other input, and returns data with count features.</span></span> <span data-ttu-id="01974-362">V tomto poli se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="01974-362">It is shown here:</span></span>

![Použít modul transformace](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-hello-count-features-look-like"></a><span data-ttu-id="01974-364">Výňatek jak vypadat hello počet funkcí</span><span class="sxs-lookup"><span data-stu-id="01974-364">An excerpt of what hello count features look like</span></span>
<span data-ttu-id="01974-365">Je významné toosee, jaký počet funkcí hello vypadat v našem případě.</span><span class="sxs-lookup"><span data-stu-id="01974-365">It is instructive toosee what hello count features look like in our case.</span></span> <span data-ttu-id="01974-366">Zde ukážeme výňatek tohoto:</span><span class="sxs-lookup"><span data-stu-id="01974-366">Here we show an excerpt of this:</span></span>

![Počet funkcí](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

<span data-ttu-id="01974-368">V této výňatek ze ukážeme, že pro hello sloupce, které jsme počítá na, jsme získat hello a přihlaste se pravděpodobnost relevantní backoffs tooany přidání.</span><span class="sxs-lookup"><span data-stu-id="01974-368">In this excerpt, we show that for hello columns that we counted on, we get hello counts and log odds in addition tooany relevant backoffs.</span></span>

<span data-ttu-id="01974-369">Snažíme se teď připravena toobuild model Azure Machine Learning použití těchto transformovaných datových sad.</span><span class="sxs-lookup"><span data-stu-id="01974-369">We are now ready toobuild an Azure Machine Learning model using these transformed datasets.</span></span> <span data-ttu-id="01974-370">V další části hello ukážeme, jak to lze provést.</span><span class="sxs-lookup"><span data-stu-id="01974-370">In hello next section, we show how this can be done.</span></span>

### <span data-ttu-id="01974-371"><a name="step3"></a>Krok 3: Vytvoření, školení a určení skóre modelu hello</span><span class="sxs-lookup"><span data-stu-id="01974-371"><a name="step3"></a> Step 3: Build, train, and score hello model</span></span>

#### <a name="choice-of-learner"></a><span data-ttu-id="01974-372">Volba student</span><span class="sxs-lookup"><span data-stu-id="01974-372">Choice of learner</span></span>
<span data-ttu-id="01974-373">Nejdřív potřebujeme toochoose student.</span><span class="sxs-lookup"><span data-stu-id="01974-373">First, we need toochoose a learner.</span></span> <span data-ttu-id="01974-374">Snažíme se probíhající toouse rozhodovací strom dvě třídy boosted jako naše student.</span><span class="sxs-lookup"><span data-stu-id="01974-374">We are going toouse a two class boosted decision tree as our learner.</span></span> <span data-ttu-id="01974-375">Zde jsou hello výchozí možnosti pro tento student:</span><span class="sxs-lookup"><span data-stu-id="01974-375">Here are hello default options for this learner:</span></span>

![Two-Class Boosted Decision Tree parametry](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

<span data-ttu-id="01974-377">Pro naše experimentu jsme probíhající toochoose hello výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="01974-377">For our experiment, we are going toochoose hello default values.</span></span> <span data-ttu-id="01974-378">Jsme Všimněte si, že tento hello výchozí hodnoty jsou obvykle smysluplný a rychlé účaří dobře tooget na výkon.</span><span class="sxs-lookup"><span data-stu-id="01974-378">We note that hello defaults are usually meaningful and a good way tooget quick baselines on performance.</span></span> <span data-ttu-id="01974-379">Můžete zlepšit výkon komínů parametry, pokud se rozhodnete tooonce, měli byste směrný plán.</span><span class="sxs-lookup"><span data-stu-id="01974-379">You can improve on performance by sweeping parameters if you choose tooonce you have a baseline.</span></span>

#### <a name="train-hello-model"></a><span data-ttu-id="01974-380">Train hello model</span><span class="sxs-lookup"><span data-stu-id="01974-380">Train hello model</span></span>
<span data-ttu-id="01974-381">Pro školení, můžeme jednoduše vyvolání **Train Model** modulu.</span><span class="sxs-lookup"><span data-stu-id="01974-381">For training, we simply invoke a **Train Model** module.</span></span> <span data-ttu-id="01974-382">Hello dva vstupy tooit jsou hello Two-Class Boosted Decision Tree Student a naší datové sadě vlaku.</span><span class="sxs-lookup"><span data-stu-id="01974-382">hello two inputs tooit are hello Two-Class Boosted Decision Tree learner and our train dataset.</span></span> <span data-ttu-id="01974-383">To je znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="01974-383">This is shown here:</span></span>

![Modulu Train Model](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-hello-model"></a><span data-ttu-id="01974-385">Určení skóre modelu hello</span><span class="sxs-lookup"><span data-stu-id="01974-385">Score hello model</span></span>
<span data-ttu-id="01974-386">Jakmile jsme modulu trained model, jsme připraveni tooscore na hello testovací datové sady a tooevaluate jeho výkon.</span><span class="sxs-lookup"><span data-stu-id="01974-386">Once we have a trained model, we are ready tooscore on hello test dataset and tooevaluate its performance.</span></span> <span data-ttu-id="01974-387">Provedeme to pomocí hello **Score Model** modulu znázorňuje následující obrázek, spolu s hello **Evaluate Model** modul:</span><span class="sxs-lookup"><span data-stu-id="01974-387">We do this by using hello **Score Model** module shown in hello following figure, along with an **Evaluate Model** module:</span></span>

![Modul Určení skóre modelu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)

### <span data-ttu-id="01974-389"><a name="step4"></a>Krok 4: Vyhodnocení modelu hello</span><span class="sxs-lookup"><span data-stu-id="01974-389"><a name="step4"></a> Step 4: Evaluate hello model</span></span>
<span data-ttu-id="01974-390">Nakonec rádi bychom znali tooanalyze modelu výkonu.</span><span class="sxs-lookup"><span data-stu-id="01974-390">Finally, we would like tooanalyze model performance.</span></span> <span data-ttu-id="01974-391">Pro dva – třída (binární) klasifikaci problémy, je dobré míru obvykle hello AUC.</span><span class="sxs-lookup"><span data-stu-id="01974-391">Usually, for two class (binary) classification problems, a good measure is hello AUC.</span></span> <span data-ttu-id="01974-392">toovisualize toho jsme spojit hello **Score Model** modulu tooan **Evaluate Model** modulu pro tento.</span><span class="sxs-lookup"><span data-stu-id="01974-392">toovisualize this, we hook up hello **Score Model** module tooan **Evaluate Model** module for this.</span></span> <span data-ttu-id="01974-393">Kliknutím na tlačítko **vizualizovat** na hello **Evaluate Model** modulu vypočítá obrázek jako hello jeden následující:</span><span class="sxs-lookup"><span data-stu-id="01974-393">Clicking **Visualize** on hello **Evaluate Model** module yields a graphic like hello following one:</span></span>

![Vyhodnocení modelu BDT modulu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

<span data-ttu-id="01974-395">Binární soubor (nebo dvě třídy) je klasifikace problémy, dobrý míru přesnost předpovědi hello oblasti pod křivky (AUC).</span><span class="sxs-lookup"><span data-stu-id="01974-395">In binary (or two class) classification problems, a good measure of prediction accuracy is hello Area Under Curve (AUC).</span></span> <span data-ttu-id="01974-396">V následující ukážeme naše výsledků pomocí tohoto modelu na naše testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="01974-396">In what follows, we show our results using this model on our test dataset.</span></span> <span data-ttu-id="01974-397">tooget tento, klikněte pravým tlačítkem na hello výstupní port modulu hello **Evaluate Model** modul a potom **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="01974-397">tooget this, right-click hello output port of hello **Evaluate Model** module and then **Visualize**.</span></span>

![Vizualizace modulu pro vyhodnocení modelu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <span data-ttu-id="01974-399"><a name="step5"></a>Krok 5: Publikování hello model jako webovou službu</span><span class="sxs-lookup"><span data-stu-id="01974-399"><a name="step5"></a> Step 5: Publish hello model as a Web service</span></span>
<span data-ttu-id="01974-400">Hello možnost toopublish model Azure Machine Learning jako webové služby s minimální fuss je cenné funkce pro vytváření široce dostupné.</span><span class="sxs-lookup"><span data-stu-id="01974-400">hello ability toopublish an Azure Machine Learning model as web services with a minimum of fuss is a valuable feature for making it widely available.</span></span> <span data-ttu-id="01974-401">Po dokončení, každý, kdo mohou být volání toohello webové služby s vstupní data, která potřebují předpovědi a webová služba hello používá hello model tooreturn těchto předpovědi.</span><span class="sxs-lookup"><span data-stu-id="01974-401">Once that is done, anyone can make calls toohello web service with input data that they need predictions for, and hello web service uses hello model tooreturn those predictions.</span></span>

<span data-ttu-id="01974-402">toodo toho jsme nejprve uložit naše trained model jako objekt Trained Model.</span><span class="sxs-lookup"><span data-stu-id="01974-402">toodo this, we first save our trained model as a Trained Model object.</span></span> <span data-ttu-id="01974-403">K tomu je potřeba pravým tlačítkem myši na hello **Train Model** modul a pomocí hello **uložit jako Trained Model** možnost.</span><span class="sxs-lookup"><span data-stu-id="01974-403">This is done by right-clicking hello **Train Model** module and using hello **Save as Trained Model** option.</span></span>

<span data-ttu-id="01974-404">Dále je třeba toocreate vstupní a výstupní porty pro naše webové služby:</span><span class="sxs-lookup"><span data-stu-id="01974-404">Next, we need toocreate input and output ports for our web service:</span></span>

* <span data-ttu-id="01974-405">vstupní port vezme všechna data v hello stejné jako hello data, která potřebujeme předpovědi formuláři</span><span class="sxs-lookup"><span data-stu-id="01974-405">an input port takes data in hello same form as hello data that we need predictions for</span></span>
* <span data-ttu-id="01974-406">na výstupní port vrátí popisky vyhodnocení hello a pravděpodobnostech hello přidružené.</span><span class="sxs-lookup"><span data-stu-id="01974-406">an output port returns hello Scored Labels and hello associated probabilities.</span></span>

#### <a name="select-a-few-rows-of-data-for-hello-input-port"></a><span data-ttu-id="01974-407">Vyberte pár řádků dat pro vstupní port hello</span><span class="sxs-lookup"><span data-stu-id="01974-407">Select a few rows of data for hello input port</span></span>
<span data-ttu-id="01974-408">Je vhodné toouse **použít transformaci SQL** modulu tooselect jenom 10 řádků tooserve jako hello vstupní port data.</span><span class="sxs-lookup"><span data-stu-id="01974-408">It is convenient toouse an **Apply SQL Transformation** module tooselect just 10 rows tooserve as hello input port data.</span></span> <span data-ttu-id="01974-409">Vyberte pouze na tyto řádky dat pro naše vstupní port pomocí dotazu SQL hello znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="01974-409">Select just these rows of data for our input port using hello SQL query shown here:</span></span>

![Vstupní port dat](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a><span data-ttu-id="01974-411">Webová služba</span><span class="sxs-lookup"><span data-stu-id="01974-411">Web service</span></span>
<span data-ttu-id="01974-412">Teď nemůžeme připraveni toorun malý experiment, kterou lze použít toopublish naše webové služby.</span><span class="sxs-lookup"><span data-stu-id="01974-412">Now we are ready toorun a small experiment that can be used toopublish our web service.</span></span>

#### <a name="generate-input-data-for-webservice"></a><span data-ttu-id="01974-413">Generovat vstupní data pro webovou službu</span><span class="sxs-lookup"><span data-stu-id="01974-413">Generate input data for webservice</span></span>
<span data-ttu-id="01974-414">Jako zeroth krok vzhledem k tomu, že tabulka počet hello je velký, jsme trvat pár řádků testovací data a vygenerovat výstupní data z něj pomocí funkce count.</span><span class="sxs-lookup"><span data-stu-id="01974-414">As a zeroth step, since hello count table is large, we take a few lines of test data and generate output data from it with count features.</span></span> <span data-ttu-id="01974-415">To může sloužit jako formát hello vstupní data pro naše webovou službu.</span><span class="sxs-lookup"><span data-stu-id="01974-415">This can serve as hello input data format for our webservice.</span></span> <span data-ttu-id="01974-416">To je znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="01974-416">This is shown here:</span></span>

![Vytvoření vstupní data BDT](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> <span data-ttu-id="01974-418">Pro formát hello vstupních dat, je nyní používána hello výstup hello **počet Featurizer** modulu.</span><span class="sxs-lookup"><span data-stu-id="01974-418">For hello input data format, we now use hello OUTPUT of hello **Count Featurizer** module.</span></span> <span data-ttu-id="01974-419">Jakmile to experimentovat dokončení spuštění, uložit výstup hello z hello **počet Featurizer** modulu jako datové sady.</span><span class="sxs-lookup"><span data-stu-id="01974-419">Once this experiment finishes running, save hello output from hello **Count Featurizer** module as a Dataset.</span></span> <span data-ttu-id="01974-420">Tato datová sada se používá pro hello vstupní data v hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="01974-420">This Dataset is used for hello input data in hello webservice.</span></span>
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a><span data-ttu-id="01974-421">Vyhodnocování experimentu pro publikování webovou službu</span><span class="sxs-lookup"><span data-stu-id="01974-421">Scoring experiment for publishing webservice</span></span>
<span data-ttu-id="01974-422">Nejprve ukážeme, jak to vypadá.</span><span class="sxs-lookup"><span data-stu-id="01974-422">First, we show what this looks like.</span></span> <span data-ttu-id="01974-423">Základní struktura Hello je **Score Model** modul, který přijímá objekt naše trained model a pár řádků vstupních dat, generovaný v předchozích krocích hello pomocí hello **počet Featurizer** modulu.</span><span class="sxs-lookup"><span data-stu-id="01974-423">hello essential structure is a **Score Model** module that accepts our trained model object and a few lines of input data that we generated in hello previous steps using hello **Count Featurizer** module.</span></span> <span data-ttu-id="01974-424">Používáme tooproject "Vyberte sloupců v datové sadě" hello Scored popisky a pravděpodobnostech skóre hello.</span><span class="sxs-lookup"><span data-stu-id="01974-424">We use "Select Columns in Dataset" tooproject out hello Scored labels and hello Score probabilities.</span></span>

![Výběr sloupců v datové sadě](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

<span data-ttu-id="01974-426">Všimněte si, jak hello **výběr sloupců v datové sadě** modul lze použít pro 'filtrování, data z datové sady.</span><span class="sxs-lookup"><span data-stu-id="01974-426">Notice how hello **Select Columns in Dataset** module can be used for 'filtering' data out from a dataset.</span></span> <span data-ttu-id="01974-427">Ukážeme hello obsah zde:</span><span class="sxs-lookup"><span data-stu-id="01974-427">We show hello contents here:</span></span>

![Filtrování s hello výběr sloupců v datové sadě modulu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

<span data-ttu-id="01974-429">porty tooget hello blue vstup a výstup, klepněte na tlačítko **Příprava webservice** v pravé dolní části hello.</span><span class="sxs-lookup"><span data-stu-id="01974-429">tooget hello blue input and output ports, you simply click **prepare webservice** at hello bottom right.</span></span> <span data-ttu-id="01974-430">Spuštění tohoto experimentu také umožňuje nám toopublish hello webové služby: klikněte na tlačítko hello **publikování webové služby** ikonu v hello dolní správné, uvedené tady:</span><span class="sxs-lookup"><span data-stu-id="01974-430">Running this experiment also allows us toopublish hello web service: click hello **PUBLISH WEB SERVICE** icon at hello bottom right, shown here:</span></span>

![Publikování webové služby](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)

<span data-ttu-id="01974-432">Po publikování hello webservice se nám získat přesměrovaného tooa stránky, která vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="01974-432">Once hello webservice is published, we get redirected tooa page that looks thus:</span></span>

![Řídicí panel webové služby](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

<span data-ttu-id="01974-434">Dva odkazy pro webovým službám vidíme na levé straně hello:</span><span class="sxs-lookup"><span data-stu-id="01974-434">We see two links for webservices on hello left side:</span></span>

* <span data-ttu-id="01974-435">Hello **požadavků a odpovědí** služby (nebo RRS) je určená pro jeden předpovědi a je co můžeme využívat tento Workshop.</span><span class="sxs-lookup"><span data-stu-id="01974-435">hello **REQUEST/RESPONSE** Service (or RRS) is meant for single predictions and is what we utilize in this workshop.</span></span>
* <span data-ttu-id="01974-436">Hello **BATCH EXECUTION** služby (BES) se používá pro předpovědi batch a vyžaduje hello vstupních dat používá toomake předpovědi, které se nacházejí v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="01974-436">hello **BATCH EXECUTION** Service (BES) is used for batch predictions and requires that hello input data used toomake predictions reside in Azure Blob Storage.</span></span>

<span data-ttu-id="01974-437">Kliknutím na odkaz hello **požadavků a odpovědí** trvá tooa stránky, která nám dává předem konzervované kódu v C#, python a R. Tento kód můžete pohodlně použít při výběru toohello volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="01974-437">Clicking on hello link **REQUEST/RESPONSE** takes us tooa page that gives us pre-canned code in C#, python, and R. This code can be conveniently used for making calls toohello webservice.</span></span> <span data-ttu-id="01974-438">Všimněte si, že hello klíč rozhraní API na této stránce musí toobe používá k ověřování.</span><span class="sxs-lookup"><span data-stu-id="01974-438">Note that hello API key on this page needs toobe used for authentication.</span></span>

<span data-ttu-id="01974-439">Je vhodné toocopy tento python code přes tooa nové buňky v poznámkovém bloku IPython hello.</span><span class="sxs-lookup"><span data-stu-id="01974-439">It is convenient toocopy this python code over tooa new cell in hello IPython notebook.</span></span>

<span data-ttu-id="01974-440">Zde ukážeme segment kódu python s hello správný klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="01974-440">Here we show a segment of python code with hello correct API key.</span></span>

![Kód jazyka Python](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)

<span data-ttu-id="01974-442">Všimněte si, že klíč rozhraní API výchozí hello byl nahrazen s klíčem rozhraní API naše webovým službám.</span><span class="sxs-lookup"><span data-stu-id="01974-442">Note that we replaced hello default API key with our webservices's API key.</span></span> <span data-ttu-id="01974-443">Kliknutím na tlačítko **spustit** v této buňce v IPython poznámkového bloku vypočítá hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="01974-443">Clicking **Run** on this cell in an IPython notebook yields hello following response:</span></span>

![IPython odpovědi](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

<span data-ttu-id="01974-445">Vidíme, že pro hello dvou testovacích příklady, které jsme požádáni o (v hello JSON architektura skript v jazyce python hello), se nám získat zpět odpovědi v podobě hello "Scored označení Scored Pravděpodobnostech".</span><span class="sxs-lookup"><span data-stu-id="01974-445">We see that for hello two test examples we asked about (in hello JSON framework of hello python script), we get back answers in hello form "Scored Labels, Scored Probabilities".</span></span> <span data-ttu-id="01974-446">Všimněte si, že v tomto případě jsme zvolili hello výchozí hodnoty tento kód předdefinovaným hello poskytuje (0 je pro všechny číselné sloupce a hello řetězec "value" pro všechny sloupce kategorií).</span><span class="sxs-lookup"><span data-stu-id="01974-446">Note that in this case, we chose hello default values that hello pre-canned code provides (0's for all numeric columns and hello string "value" for all categorical columns).</span></span>

<span data-ttu-id="01974-447">Tím končí naše zobrazující začátku do konce návod jak toohandle rozsáhlé datové sady pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="01974-447">This concludes our end-to-end walkthrough showing how toohandle large-scale dataset using Azure Machine Learning.</span></span> <span data-ttu-id="01974-448">Můžeme začít s terabajt dat, sestavený předpovědi modelu a nasadit jako webovou službu v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="01974-448">We started with a terabyte of data, constructed a prediction model and deployed it as a web service in hello cloud.</span></span>

